---
title: Visualizzazioni pregenerate mapping - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: cb374d007252710b42c31061bf15d7d32af0db27
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489245"
---
# <a name="pre-generated-mapping-views"></a>Visualizzazioni pregenerate mapping
Prima di Entity Framework può eseguire una query o salvare le modifiche all'origine dati, è necessario generare un set di visualizzazioni dei mapping di accesso al database. Queste visualizzazioni dei mapping di un siano dell'istruzione Entity SQL che rappresentano il database in modo astratto e fanno parte dei metadati memorizzati nella cache per ogni dominio dell'applicazione. Se si creano più istanze dello stesso contesto nel dominio dell'applicazione stessa, queste riutilizzeranno le visualizzazioni di mapping dai metadati memorizzati nella cache anziché rigenerarle. Poiché la generazione di visualizzazioni mapping è una parte significativa del costo complessivo dell'esecuzione della prima query, Entity Framework consente di pre-generare viste di mapping e includerli nel progetto compilato. Per altre informazioni, vedere [considerazioni sulle prestazioni (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).

## <a name="generating-mapping-views-with-the-ef-power-tools"></a>Generazione di visualizzazioni con EF Power Tools di Mapping

Il modo più semplice per pregenerare le visualizzazioni è usare il [EF Power Tools](http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d). Una volta installati gli strumenti Power si avrà un'opzione di menu per generare visualizzazioni, come indicato di seguito.

-   Per la **Code First** modelli fare doppio clic sul file di codice che contiene la classe DbContext.
-   Per la **Entity Framework Designer** modelli fare doppio clic sul file EDMX.

![generare viste](~/ef6/media/generateviews.png)

Una volta completato il processo si avrà una classe simile alla seguente generato

![Visualizzazioni generate](~/ef6/media/generatedviews.png)

A questo punto, quando si esegue l'applicazione Entity Framework utilizzerà questa classe per caricare le visualizzazioni in base alle esigenze. Se le modifiche apportate al modello e non generare di nuovo questa classe Entity Framework genererà un'eccezione.

## <a name="generating-mapping-views-from-code---ef6-onwards"></a>La generazione di viste di Mapping da codice - 6 e versioni successive

L'altro modo per generare visualizzazioni è usare le API che consente a Entity Framework. Quando si usa questo metodo che si ha la possibilità di serializzare le viste, tuttavia si desidera, ma è anche necessario caricare le visualizzazioni autonomamente.

> [!NOTE]
> **Entity Framework 6 e versioni successive solo** -le API illustrate in questa sezione sono state introdotte in Entity Framework 6. Se si usa una versione precedente queste informazioni non valide.

### <a name="generating-views"></a>La generazione di visualizzazioni

Le API per generare visualizzazioni sono nella classe System.Data.Entity.Core.Mapping.StorageMappingItemCollection. È possibile recuperare un StorageMappingCollection per un contesto con l'elemento MetadataWorkspace di ObjectContext. Se si usa l'API DbContext più recente, quindi è possibile accedere a questa usando il IObjectContextAdapter simile alla seguente, in questo codice è disponibile un'istanza di DbContext derivata denominata dbContext:

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

Dopo aver StorageMappingItemCollection sia creato è possibile ottenere l'accesso ai metodi GenerateViews e ComputeMappingHashValue.

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

Il primo metodo crea un dizionario con una voce per ogni visualizzazione nel mapping del contenitore. Il secondo metodo calcola un valore hash per il mapping singolo contenitore e viene utilizzato in fase di esecuzione per convalidare che il modello non è stato modificato poiché le visualizzazioni sono state generate in precedenza. Esegue l'override dei due metodi vengono fornito per scenari complessi che coinvolgono più mapping del contenitore.

Durante la generazione di visualizzazioni verrà chiamare il metodo GenerateViews e quindi scrivere il EntitySetBase e DbMappingView risultante. È necessario anche archiviare l'hash generato dal metodo ComputeMappingHashValue.

### <a name="loading-views"></a>Il caricamento di viste

Per caricare le visualizzazioni generate dal metodo GenerateViews, è possibile fornire Entity Framework con una classe che eredita dalla classe astratta DbMappingViewCache. DbMappingViewCache specifica due metodi che è necessario implementare:

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

La proprietà MappingHashValue deve restituire l'hash generato dal metodo ComputeMappingHashValue. Quando Entity Framework è corso porre la domanda per le visualizzazioni innanzitutto genererà e confrontare il valore hash del modello con il valore hash restituito da questa proprietà. Se non corrispondono a Entity Framework genererà un'eccezione EntityCommandCompilationException.

Il metodo GetView accetterà un EntitySetBase ed è necessario restituire un DbMappingVIew contenente EntitySql che è stato generato per cui è stata associata la EntitySetBase specificata nel dizionario generato dal metodo GenerateViews. Se vengono richiesti a Entity Framework per una vista che non è quindi GetView deve restituire null.

Di seguito è riportato un estratto DbMappingViewCache generata con strumenti avanzati di come descritto in precedenza, in esso che vediamo un modo per archiviare e recuperare il EntitySql necessari.

``` csharp
    public override string MappingHashValue
    {
        get { return "a0b843f03dd29abee99789e190a6fb70ce8e93dc97945d437d9a58fb8e2afd2e"; }
    }

    public override DbMappingView GetView(EntitySetBase extent)
    {
        if (extent == null)
        {
            throw new ArgumentNullException("extent");
        }

        var extentName = extent.EntityContainer.Name + "." + extent.Name;

        if (extentName == "BlogContext.Blogs")
        {
            return GetView2();
        }

        if (extentName == "BlogContext.Posts")
        {
            return GetView3();
        }

        return null;
    }

    private static DbMappingView GetView2()
    {
        return new DbMappingView(@"
            SELECT VALUE -- Constructing Blogs
            [BlogApp.Models.Blog](T1.Blog_BlogId, T1.Blog_Test, T1.Blog_title, T1.Blog_Active, T1.Blog_SomeDecimal)
            FROM (
            SELECT
                T.BlogId AS Blog_BlogId,
                T.Test AS Blog_Test,
                T.title AS Blog_title,
                T.Active AS Blog_Active,
                T.SomeDecimal AS Blog_SomeDecimal,
                True AS _from0
            FROM CodeFirstDatabase.Blog AS T
            ) AS T1");
    }
```

Perché usare Entity Framework il DbMappingViewCache è aggiungere utilizzare DbMappingViewCacheTypeAttribute, specificare il contesto di cui è stato creato per. Nel codice seguente viene associata la BlogContext con la classe MyMappingViewCache.

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

Per scenari più complessi, è possono specificare le istanze di cache di visualizzazione di mapping specificando una factory di cache di visualizzazione di mapping. Questa operazione può essere eseguita mediante l'implementazione della classe astratta System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory. L'istanza della factory della cache Visualizza mapping utilizzato può essere recuperato o impostato utilizzando il StorageMappingItemCollection.MappingViewCacheFactoryproperty.
