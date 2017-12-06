---
title: Resilienza di connessione - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: aec69577cd4b19fdebedb33ed6fd8f2665b0a032
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2017
---
# <a name="connection-resiliency"></a>Resilienza della connessione

Resilienza di connessione ritenta automaticamente i comandi di database non riuscita. La funzionalità è utilizzabile con qualsiasi database specificando una "strategia di esecuzione", che incapsula la logica necessaria per rilevare errori e ripetere i comandi. Il provider di sistema di Entity Framework può fornire le strategie di esecuzione personalizzata in base a criteri di tentativi ottimali e le condizioni di errore di database specifico.

Ad esempio, il provider SQL Server include una strategia di esecuzione che è adatto a SQL Server (incluso SQL Azure). È compatibile con i tipi di eccezione che è possibile ritentare e si dispone di impostazioni predefinite ragionevoli per numero massimo di tentativi, intervallo tra tentativi e così via.

Una strategia di esecuzione viene specificata quando si configurano le opzioni per il contesto. Si tratta in genere nel `OnConfiguring` metodo del contesto derivato o in `Startup.cs` per un'applicazione ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a>Strategia di esecuzione personalizzato

È un meccanismo per registrare una strategia di esecuzione personalizzata personalizzati se si desidera modificare le impostazioni predefinite.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a>Le transazioni e le strategie di esecuzione

Una strategia di esecuzione che ritenta automaticamente in caso di errore deve essere in grado di riprodurre ogni operazione in un blocco di ripetizione che ha esito negativo. Quando sono abilitati i tentativi, ogni operazione effettuata tramite EF Core diventa il proprio riproducibile, vale a dire ogni query e ogni chiamata a `SaveChanges()` verrà ritentata come unità se si verifica un errore temporaneo.

Tuttavia, se il codice avvia una transazione tramite `BeginTransaction()` si sta definendo un proprio gruppo di operazioni che devono essere trattati come un'unità, ad esempio tutti gli elementi all'interno della transazione dovrà essere riprodotti deve verificarsi un errore. Se si tenta di eseguire questa operazione quando si utilizza una strategia di esecuzione, si riceverà un'eccezione simile al seguente.

> InvalidOperationException: La strategia di esecuzione configurata 'SqlServerRetryingExecutionStrategy' non supporta le transazioni avviate dall'utente. Utilizzare la strategia di esecuzione restituita da 'DbContext.Database.CreateExecutionStrategy()' per eseguire tutte le operazioni nella transazione come unità possibilità di ritentare.

La soluzione consiste nel richiamare manualmente la strategia di esecuzione con un delegato che rappresenta gli elementi che deve essere eseguito. Se si verifica un errore temporaneo, la strategia di esecuzione verrà nuovamente richiamare il delegato.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>Errore commit della transazione e il problema idempotenza

In generale, quando si verifica un errore di connessione è eseguire il rollback della transazione corrente. Tuttavia, se la connessione viene eliminata mentre la transazione è stata eseguito il commit risultante lo stato della transazione è sconosciuto. Vedere questo [post di blog](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) per altri dettagli.

Per impostazione predefinita, la strategia di esecuzione verrà ripetere l'operazione come se è stato eseguito il rollback della transazione, ma se non è il caso questa genererà un'eccezione se il nuovo stato del database non è compatibile o potrebbe causare **il danneggiamento dei dati** se la operazione non si basa su uno stato specifico, ad esempio quando si inserisce una nuova riga con valori di chiave generato automaticamente.

Esistono diversi modi per risolvere questo problema.

### <a name="option-1---do-almost-nothing"></a>Opzione 1: nothing (quasi)

La probabilità di un errore di connessione durante il commit delle transazioni è bassa, pertanto potrebbe essere accettabile per l'applicazione viene eseguita solo se questa condizione si verifica effettivamente.

Tuttavia, è necessario evitare le chiavi generate dall'archivio per garantire che viene generata un'eccezione anziché aggiungere una riga duplicata. È consigliabile utilizzare un valore GUID generato dal client o un generatore di valore sul lato client.

### <a name="option-2---rebuild-application-state"></a>Opzione 2: lo stato dell'applicazione di ricompilazione

1. Eliminare l'oggetto corrente `DbContext`.
2. Creare un nuovo `DbContext` e ripristinare lo stato dell'applicazione dal database.
3. Informare l'utente che l'ultima operazione potrebbe non sia stata completata correttamente.

### <a name="option-3---add-state-verification"></a>Opzione 3: aggiungere la verifica dello stato

Per la maggior parte delle operazioni che modificano lo stato del database è possibile aggiungere codice che verifica se ha avuto esito positivo. Entity Framework fornisce un metodo di estensione per semplificare questa operazione - `IExecutionStrategy.ExecuteInTransaction`.

Questo metodo inizia e viene eseguito il commit di una transazione e accetta inoltre una funzione di `verifySucceeded` parametro che viene richiamato quando si verifica un errore temporaneo durante il commit della transazione.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> Qui `SaveChanges` viene richiamato con `acceptAllChangesOnSuccess` impostato su `false` per evitare di modificare lo stato del `Blog` entità `Unchanged` se `SaveChanges` ha esito positivo. Questo consente di ripetere la stessa operazione, se il commit non riesce e viene eseguito il rollback della transazione.

### <a name="option-4---manually-track-the-transaction"></a>Opzione 4: registrare manualmente la transazione

Se è necessario utilizzare le chiavi generate dall'archivio o una modalità generica di gestione degli errori di commit che non dipende l'operazione eseguita ogni transazione è possibile assegnare un ID che viene controllato durante il commit ha esito negativo.

1. Aggiungere una tabella per il database utilizzato per tenere traccia dello stato delle transazioni.
2. Inserire una riga nella tabella all'inizio di ogni transazione.
3. Se la connessione non riesce durante il commit, verificare la presenza della riga corrispondente nel database.
4. Se il commit ha esito positivo, è possibile eliminare la riga corrispondente per evitare la crescita della tabella.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> Assicurarsi che il contesto utilizzato per la verifica ha una strategia di esecuzione definita come la connessione è probabile che non viene eseguita nuovamente durante la verifica se non è riuscito durante il commit della transazione.
