---
title: Resilienza della connessione - EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: d6e31cf2b9b783ea503703536d159b34bf2e18c0
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997190"
---
# <a name="connection-resiliency"></a>Resilienza della connessione

Resilienza della connessione ritenta automaticamente i comandi di database non riuscita. La funzionalità è utilizzabile con qualsiasi database specificando una "strategia di esecuzione", che incapsula la logica necessaria per rilevare gli errori e ripetere i comandi. Provider di EF Core può fornire le strategie di esecuzione personalizzate in base a condizioni di errore specifico del database e i criteri di ripetizione dei tentativi ottimale.

Ad esempio, il provider SQL Server include una strategia di esecuzione che viene personalizzata in modo specifico per SQL Server (incluso SQL Azure). Si è a conoscenza dei tipi di eccezioni che possono essere ripetuti e offre impostazioni predefinite ragionevoli per numero massimo di tentativi, intervallo tra tentativi e così via.

Una strategia di esecuzione viene specificata quando si configurano le opzioni per il contesto. Si tratta in genere nel `OnConfiguring` metodo del contesto derivato o in `Startup.cs` per un'applicazione ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a>Strategia di esecuzione personalizzata

C'è un meccanismo per registrare una strategia di esecuzione personalizzata personalizzato se si desidera modificare le impostazioni predefinite.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a>Le transazioni e strategie di esecuzione

Una strategia di esecuzione che esegue automaticamente nuovi tentativi in caso di errori deve essere in grado di riprodurre ogni operazione in un blocco di ripetizione dei tentativi che ha esito negativo. Quando sono abilitati i tentativi, ogni operazione che viene eseguita tramite EF Core diventa la propria operazione con possibilità di ritentare. Vale a dire, ogni query e ogni chiamata a `SaveChanges()` verrà ritentata come unità se si verifica un errore temporaneo.

Tuttavia, se il codice avvia una transazione tramite `BeginTransaction()` si sta definendo il proprio gruppo di operazioni che devono essere considerati come un'unità e tutti gli elementi all'interno della transazione dovrà essere riprodotti deve verificarsi un errore. Se si prova a eseguire questa operazione quando si usa una strategia di esecuzione, si riceverà un'eccezione simile al seguente:

> InvalidOperationException: La strategia di esecuzione configurata 'SqlServerRetryingExecutionStrategy' non supporta le transazioni avviate dall'utente. Usare la strategia di esecuzione restituita da 'DbContext.Database.CreateExecutionStrategy()' per eseguire tutte le operazioni nella transazione come un'unità con possibilità di ritentare.

La soluzione consiste nel richiamare manualmente la strategia di esecuzione con un delegato che rappresenta tutti gli elementi che deve essere eseguito. Se si verifica un errore temporaneo, la strategia di esecuzione richiamerà nuovamente il delegato.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>Errore commit della transazione e il problema di idempotenza

In generale, quando si verifica un errore di connessione corrente è rollback della transazione. Tuttavia, se la connessione viene eliminata mentre la transazione è in corso il commit risultante lo stato della transazione è sconosciuto. Vedere questo [post di blog](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) per altri dettagli.

Per impostazione predefinita, la strategia di esecuzione ritenterà l'operazione come se è stato eseguito il rollback della transazione, ma se non è il caso ciò comporterà un'eccezione se il nuovo stato del database non è compatibile o potrebbe causare **danneggiamento dei dati** se la operazione non si basa su un determinato stato, ad esempio quando si inserisce una nuova riga con valori di chiave generati automaticamente.

Esistono diversi modi per risolvere questo problema.

### <a name="option-1---do-almost-nothing"></a>Opzione 1: nothing (quasi)

La probabilità di un errore di connessione durante il commit della transazione è bassa, pertanto potrebbe essere accettabile per l'applicazione viene eseguita solo se questa condizione si verifica effettivamente.

Tuttavia, è necessario evitare di usare le chiavi generate dall'archivio per garantire che invece di aggiungere una riga duplicata viene generata un'eccezione. È consigliabile usare un valore GUID generato dal client o un generatore di valore lato client.

### <a name="option-2---rebuild-application-state"></a>Opzione 2: lo stato dell'applicazione di ricompilazione

1. Rimuovere l'oggetto corrente `DbContext`.
2. Creare un nuovo `DbContext` e ripristinare lo stato dell'applicazione dal database.
3. Informare l'utente che l'ultima operazione potrebbe non siano stata completata correttamente.

### <a name="option-3---add-state-verification"></a>Opzione 3: aggiungere la verifica dello stato

Per la maggior parte delle operazioni che modificano lo stato del database è possibile aggiungere il codice che verifica se ha avuto esito positivo. Entity Framework fornisce un metodo di estensione per semplificare questa operazione - `IExecutionStrategy.ExecuteInTransaction`.

Questo metodo avvia ed esegue il commit di una transazione e accetta anche una funzione nel `verifySucceeded` parametro che viene richiamato quando si verifica un errore temporaneo durante il commit della transazione.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> Qui `SaveChanges` viene richiamato con `acceptAllChangesOnSuccess` impostata su `false` per evitare la modifica dello stato del `Blog` entità `Unchanged` se `SaveChanges` ha esito positivo. Ciò consente di ripetere l'operazione stessa se il commit ha esito negativo e viene eseguito il rollback della transazione.

### <a name="option-4---manually-track-the-transaction"></a>Opzione 4: registrare manualmente la transazione

Se è necessario usare le chiavi generate dall'archivio o necessitano di un modo generico di gestione degli errori di commit che non variano a seconda dell'operazione eseguita ogni transazione è stato possibile assegnare un ID che viene controllato durante il commit ha esito negativo.

1. Aggiungere una tabella nel database usato per tenere traccia dello stato delle transazioni.
2. Inserire una riga nella tabella all'inizio di ogni transazione.
3. Se la connessione non riesce durante il commit, verificare la presenza della riga corrispondente nel database.
4. Se il commit ha esito positivo, eliminare la riga corrispondente per evitare la crescita della tabella.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Assicurarsi che il contesto utilizzato per la verifica abbia una strategia di esecuzione definita come la connessione è probabile che non viene eseguita nuovamente durante la verifica se non è riuscito durante il commit della transazione.
