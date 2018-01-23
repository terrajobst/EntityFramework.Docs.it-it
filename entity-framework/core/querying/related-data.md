---
title: Dati - Core EF correlati di caricamento
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: ec69bb128890a1e0b72fe77014f37747585bb5a5
ms.sourcegitcommit: 3b21a7fdeddc7b3c70d9b7777b72bef61f59216c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2018
---
# <a name="loading-related-data"></a>Caricamento dei dati correlati

Entity Framework Core consente di utilizzare le proprietà di navigazione del modello per caricare entità correlate. Esistono tre modelli di O/RM comune utilizzati per caricare i dati correlati.
* **Caricamento eager** indica che i dati correlati vengono caricati dal database come parte della query iniziale.
* **Caricamento esplicito** indica che i dati correlati vengono caricati in modo esplicito dal database in un secondo momento.
* **Caricamento lazy** indica che i dati correlati vengono caricati in modo trasparente dal database quando si accede alla proprietà di navigazione. Il caricamento differito non è ancora possibile con Entity Framework di base.

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) di questo articolo in GitHub.

## <a name="eager-loading"></a>Caricamento eager

È possibile utilizzare il `Include` per specificare i dati correlati da includere nei risultati della query. Nell'esempio seguente, sarà necessario il blog che vengono restituiti nei risultati della loro `Posts` proprietà popolata con i post correlati.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core verrà automaticamente correzione proprietà di navigazione da qualsiasi altra entità che sono stata caricata in precedenza l'istanza del contesto. Pertanto, anche se in modo esplicito non includono i dati per una proprietà di navigazione, la proprietà comunque possibile popolare se alcune o tutte le entità correlate sono state caricate in precedenza.


È possibile includere dati correlati tratti da più relazioni in una singola query.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Inclusi più livelli

È possibile il drill-down tramite le relazioni da includere più livelli di dati correlati mediante la `ThenInclude` metodo. Nell'esempio seguente carica tutti i blog, i post correlati e l'autore di ogni post.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> Le versioni correnti di Visual Studio offrono le opzioni di completamento di codice non corretto e può causare espressioni corrette viene contrassegnata con errori di sintassi quando si utilizza il `ThenInclude` metodo dopo una proprietà di navigazione della raccolta. Si tratta di un sintomo di un bug IntelliSense rilevato al https://github.com/dotnet/roslyn/issues/8237. È possibile ignorare questi errori di sintassi di vario tipo, purché il codice sia corretto e può essere compilato correttamente. 

È possibile concatenare più chiamate a `ThenInclude` continuare inclusi ulteriori livelli di dati correlati.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

È possibile combinare queste per includere dati correlati tratti da più livelli e più nodi radice nella stessa query.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

È consigliabile includere più entità correlate per un'entità che è incluso. Ad esempio, quando si eseguono query `Blog`s, includere `Posts` e si desidera includere sia il `Author` e `Tags` del `Posts`. A tale scopo, è necessario specificare ogni percorso iniziando dalla radice di inclusione. Ad esempio, `Blog -> Posts -> Author` e `Blog -> Posts -> Tags`. Ciò non significa che si otterranno join ridondanti, nella maggior parte dei casi che verranno consolidate EF i join durante la generazione di SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a>Ignorato include

Se si modifica la query in modo che non restituisca non sono più istanze del tipo di entità che la query inizia con, vengono ignorati gli operatori di inclusione.

Nell'esempio seguente, si basano gli operatori di inclusione di `Blog`, tuttavia, il `Select` operatore viene usato per modificare la query per restituire un tipo anonimo. In questo caso, gli operatori di inclusione non hanno effetto.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

Per impostazione predefinita, Core EF registrerà un avviso quando includono operatori vengono ignorati. Vedere [registrazione](../miscellaneous/logging.md) per ulteriori informazioni sulla visualizzazione dell'output di registrazione. È possibile modificare il comportamento quando un operatore di inclusione viene ignorato per generare o non eseguono alcuna operazione. Questa operazione viene eseguita quando si configura le opzioni per il contesto, in genere in `DbContext.OnConfiguring`, o in `Startup.cs` se si utilizza ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>Caricamento esplicito

> [!NOTE]  
> Questa funzionalità è stato introdotto in Entity Framework Core 1.1.

È possibile caricare in modo esplicito una proprietà di navigazione tramite il `DbContext.Entry(...)` API.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Eseguendo una query separata che restituisce le entità correlate, è possibile caricare anche in modo esplicito una proprietà di navigazione. Se è abilitato il rilevamento delle modifiche, quindi durante il caricamento di un'entità, EF Core verrà automaticamente impostato le proprietà di navigazione del entitiy appena caricati per fare riferimento a qualsiasi entità già caricato e impostare le proprietà di navigazione delle entità già caricato per fare riferimento al entità appena caricati.

### <a name="querying-related-entities"></a>Esecuzione di query su entità correlate

È inoltre possibile ottenere una query LINQ che rappresenta il contenuto di una proprietà di navigazione.

Ciò consente di eseguire operazioni quali l'esecuzione di un operatore di aggregazione senza il loro caricamento in memoria tramite le entità correlate.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

È inoltre possibile filtrare le entità correlate vengono caricate in memoria.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Caricamento lazy

Il caricamento differito non è ancora supportato da Entity Framework Core. È possibile visualizzare il [elemento caricamento lazy il nostro backlog](https://github.com/aspnet/EntityFramework/issues/3797) per tenere traccia di questa funzionalità.

## <a name="related-data-and-serialization"></a>Serializzazione e i dati correlati

Poiché le proprietà di navigazione di correzione automaticamente verrà di Entity Framework Core, possono finire con cicli nell'oggetto grafico. Ad esempio, il caricamento di un blog e è correlato post verrà creato un oggetto di blog che fa riferimento a un insieme di pubblicazioni. Ognuno di tali pubblicazioni avrà un riferimento al blog.

Alcuni framework di serializzazione non consentono tali cicli. Ad esempio, Json.NET genererà l'eccezione seguente se un ciclo è presente una.

> Newtonsoft.Json.JsonSerializationException: Self ciclo rilevato per la proprietà 'Blog' con 'MyApplication.Models.Blog' di tipo di riferimento.

Se si utilizza ASP.NET Core, è possibile configurare Json.NET per ignorare i cicli che trova nell'oggetto grafico. Questa operazione viene eseguita `ConfigureServices(...)` metodo `Startup.cs`.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
