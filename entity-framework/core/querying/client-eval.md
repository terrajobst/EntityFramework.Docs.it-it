---
title: Valutazione tra client e server - EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: e01bd146c4dfe7a8d36b641cb52ae366fddd8239
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417760"
---
# <a name="client-vs-server-evaluation"></a>Client e valutazione del server

Come regola generale, Entity Framework Core tenta di valutare una query sul server il più possibile. EF Core converte parti della query in parametri, che è possibile valutare sul lato client. Il resto della query (insieme ai parametri generati) viene assegnato al provider di database per determinare la query di database equivalente da valutare nel server. EF Core supporta la valutazione parziale del client nella proiezione `Select()`di primo livello (essenzialmente, l'ultima chiamata a ). Se la proiezione di primo livello nella query non può essere convertita nel server, EF Core recupererà i dati necessari dal server e valuterà le parti rimanenti della query sul client. Se EF Core rileva un'espressione, in qualsiasi posizione diversa dalla proiezione di primo livello, che non può essere convertita nel server, viene generata un'eccezione di runtime. Vedere [come query funziona](xref:core/querying/how-query-works) per comprendere come EF Core determina ciò che non può essere convertito in server.

> [!NOTE]
> Prima della versione 3.0, Entity Framework Core supportava la valutazione client in qualsiasi punto della query. Per ulteriori informazioni, vedere la [sezione delle versioni precedenti.](#previous-versions)

> [!TIP]
> È possibile visualizzare l'[esempio](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) di questo articolo in GitHub.

## <a name="client-evaluation-in-the-top-level-projection"></a>Valutazione del client nella proiezione di primo livello

Nell'esempio seguente viene usato un metodo helper per standardizzare gli URL per i blog, che vengono restituiti da un database di SQL Server.In the following example, a helper method is used to standardize URLs for blogs, which are returned from a SQL Server database. Poiché il provider SQL Server non dispone di informazioni dettagliate su come viene implementato questo metodo, non è possibile convertirlo in SQL. Tutti gli altri aspetti della query vengono valutati nel `URL` database, ma il passaggio dell'oggetto restituito tramite questo metodo viene eseguito sul client.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientMethod)]

## <a name="unsupported-client-evaluation"></a>Valutazione client non supportata

Sebbene la valutazione del client sia utile, talvolta le prestazioni possono risultare scarse. Si consideri la query seguente, in cui il metodo helper viene ora usato in un filtro where. Poiché il filtro non può essere applicato nel database, tutti i dati devono essere estratti in memoria per applicare il filtro sul client. In base al filtro e alla quantità di dati sul server, la valutazione del client potrebbe comportare una riduzione delle prestazioni. Così Entity Framework Core blocca tale valutazione del client e genera un'eccezione di runtime.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientWhere)]

## <a name="explicit-client-evaluation"></a>Valutazione esplicita del cliente

Potrebbe essere necessario forzare esplicitamente la valutazione del cliente in alcuni casi, ad esempio

- La quantità di dati è piccola in modo che la valutazione sul client non commichi un'enorme riduzione delle prestazioni.
- L'operatore LINQ utilizzato non dispone di alcuna conversione sul lato server.

In questi casi, è possibile rifiutare esplicitamente`AsAsyncEnumerable` `ToListAsync` la valutazione del client chiamando metodi come `AsEnumerable` o `ToList` (o per async). Utilizzando `AsEnumerable` si sarebbe lo streaming dei `ToList` risultati, ma utilizzando causerebbe buffering creando un elenco, che richiede anche memoria aggiuntiva. Anche se se si sta enumerando più volte, quindi l'archiviazione dei risultati in un elenco aiuta di più poiché c'è solo una query al database. A seconda dell'utilizzo specifico, è necessario valutare quale metodo è più utile per il caso.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ExplicitClientEval)]

## <a name="potential-memory-leak-in-client-evaluation"></a>Potenziale perdita di memoria nella valutazione del client

Poiché la conversione e la compilazione delle query sono costose, EF Core memorizza nella cache il piano di query compilato. Il delegato memorizzato nella cache può utilizzare il codice client durante la valutazione client della proiezione di primo livello. EF Core genera parametri per le parti valutate dal client dell'albero e riutilizza il piano di query sostituendo i valori dei parametri. Tuttavia, alcune costanti nell'albero delle espressioni non possono essere convertite in parametri. Se il delegato memorizzato nella cache contiene tali costanti, tali oggetti non possono essere sottoposti a garbage collection poiché ne fanno ancora riferimento. Se tale oggetto contiene un oggetto DbContext o altri servizi in esso contenuti, l'utilizzo della memoria dell'app potrebbe aumentare nel tempo. Questo comportamento è in genere un segno di una perdita di memoria. EF Core genera un'eccezione ogni volta che si imbatte in costanti di un tipo che non può essere mappato utilizzando il provider di database corrente. Le cause più comuni e le relative soluzioni sono le seguenti:

- **Utilizzo di un metodo**di istanza : quando si utilizzano metodi di istanza in una proiezione client, la struttura ad albero dell'espressione contiene una costante dell'istanza. Se il metodo non utilizza dati dall'istanza, è consigliabile rendere statico il metodo. Se sono necessari dati di istanza nel corpo del metodo, passare i dati specifici come argomento al metodo.
- **Passaggio di argomenti costanti al metodo** `this` : questo caso si verifica in genere utilizzando in un argomento al metodo client. Valutare la possibilità di suddividere l'argomento in più argomenti scalari, che possono essere mappati dal provider di database.
- **Altre costanti**: Se una costante viene inqualsiasi caso, è possibile valutare se la costante è necessaria nell'elaborazione. Se è necessario avere la costante o se non è possibile usare una soluzione dai casi precedenti, creare una variabile locale per archiviare il valore e usare la variabile locale nella query. EF Core convertirà la variabile locale in un parametro.

## <a name="previous-versions"></a>Versioni precedenti

La sezione seguente si applica alle versioni di EF Core precedenti alla 3.0.The following section applies to EF Core versions before 3.0.

Le versioni precedenti di EF Core supportavano la valutazione client in qualsiasi parte della query, non solo la proiezione di primo livello. Ecco perché le query simili a una pubblicata nella sezione [Valutazione client non supportata](#unsupported-client-evaluation) hanno funzionato correttamente. Poiché questo comportamento potrebbe causare problemi di prestazioni inosservati, EF Core registrato un avviso di valutazione del client. Per ulteriori informazioni sulla visualizzazione dell'output di registrazione, vedere [Registrazione](xref:core/miscellaneous/logging).

Facoltativamente, EF Core consente di modificare il comportamento predefinito per generare un'eccezione o non eseguire alcuna operazione quando si esegue la valutazione del client (ad eccezione della proiezione). Il comportamento di generazione di eccezioni lo renderebbe simile al comportamento in 3.0.The exception throwing behavior would make it similar to the behavior in 3.0. Per modificare il comportamento, è necessario configurare gli avvisi durante `DbContext.OnConfiguring`l'impostazione delle opzioni per il contesto, in genere in o in `Startup.cs` se si utilizza ASP.NET Core.

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
