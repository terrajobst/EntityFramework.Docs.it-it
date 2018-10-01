---
title: Visualizzazioni pregenerate mapping - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: 1fda9fe9638adce9b24a6b81aa081effeb0def81
ms.sourcegitcommit: c568d33214fc25c76e02c8529a29da7a356b37b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2018
ms.locfileid: "47459526"
---
# <a name="pre-generated-mapping-views"></a><span data-ttu-id="87508-102">Visualizzazioni pregenerate mapping</span><span class="sxs-lookup"><span data-stu-id="87508-102">Pre-generated mapping views</span></span>
<span data-ttu-id="87508-103">Prima di Entity Framework può eseguire una query o salvare le modifiche all'origine dati, è necessario generare un set di visualizzazioni dei mapping di accesso al database.</span><span class="sxs-lookup"><span data-stu-id="87508-103">Before the Entity Framework can execute a query or save changes to the data source, it must generate a set of mapping views to access the database.</span></span> <span data-ttu-id="87508-104">Queste visualizzazioni dei mapping di un siano dell'istruzione Entity SQL che rappresentano il database in modo astratto e fanno parte dei metadati memorizzati nella cache per ogni dominio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="87508-104">These mapping views are a set of Entity SQL statement that represent the database in an abstract way, and are part of the metadata which is cached per application domain.</span></span> <span data-ttu-id="87508-105">Se si creano più istanze dello stesso contesto nel dominio dell'applicazione stessa, queste riutilizzeranno le visualizzazioni di mapping dai metadati memorizzati nella cache anziché rigenerarle.</span><span class="sxs-lookup"><span data-stu-id="87508-105">If you create multiple instances of the same context in the same application domain, they will reuse mapping views from the cached metadata rather than regenerating them.</span></span> <span data-ttu-id="87508-106">Poiché la generazione di visualizzazioni mapping è una parte significativa del costo complessivo dell'esecuzione della prima query, Entity Framework consente di pre-generare viste di mapping e includerli nel progetto compilato.</span><span class="sxs-lookup"><span data-stu-id="87508-106">Because mapping view generation is a significant part of the overall cost of executing the first query, the Entity Framework enables you to pre-generate mapping views and include them in the compiled project.</span></span> <span data-ttu-id="87508-107">Per altre informazioni, vedere [considerazioni sulle prestazioni (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span><span class="sxs-lookup"><span data-stu-id="87508-107">For more information, see  [Performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span></span>

## <a name="generating-mapping-views-with-the-ef-power-tools-community-edition"></a><span data-ttu-id="87508-108">Generazione di visualizzazioni con la Community Edition di EF Power Tools di Mapping</span><span class="sxs-lookup"><span data-stu-id="87508-108">Generating Mapping Views with the EF Power Tools Community Edition</span></span>

<span data-ttu-id="87508-109">Il modo più semplice per pregenerare le visualizzazioni è usare il [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition).</span><span class="sxs-lookup"><span data-stu-id="87508-109">The easiest way to pre-generate views is to use the [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition).</span></span> <span data-ttu-id="87508-110">Una volta installati gli strumenti Power si avrà un'opzione di menu per generare visualizzazioni, come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="87508-110">Once you have the Power Tools installed you will have a menu option to Generate Views, as below.</span></span>

-   <span data-ttu-id="87508-111">Per la **Code First** modelli fare doppio clic sul file di codice che contiene la classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="87508-111">For **Code First** models right-click on the code file that contains your DbContext class.</span></span>
-   <span data-ttu-id="87508-112">Per la **Entity Framework Designer** modelli fare doppio clic sul file EDMX.</span><span class="sxs-lookup"><span data-stu-id="87508-112">For **EF Designer** models right-click on your EDMX file.</span></span>

![generare viste](~/ef6/media/generateviews.png)

<span data-ttu-id="87508-114">Una volta completato il processo si avrà una classe simile alla seguente generato</span><span class="sxs-lookup"><span data-stu-id="87508-114">Once the process is finished you will have a class similar to the following generated</span></span>

![Visualizzazioni generate](~/ef6/media/generatedviews.png)

<span data-ttu-id="87508-116">A questo punto, quando si esegue l'applicazione Entity Framework utilizzerà questa classe per caricare le visualizzazioni in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="87508-116">Now when you run your application EF will use this class to load views as required.</span></span> <span data-ttu-id="87508-117">Se le modifiche apportate al modello e non generare di nuovo questa classe Entity Framework genererà un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="87508-117">If your model changes and you do not re-generate this class then EF will throw an exception.</span></span>

## <a name="generating-mapping-views-from-code---ef6-onwards"></a><span data-ttu-id="87508-118">La generazione di viste di Mapping da codice - 6 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="87508-118">Generating Mapping Views from Code - EF6 Onwards</span></span>

<span data-ttu-id="87508-119">L'altro modo per generare visualizzazioni è usare le API che consente a Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="87508-119">The other way to generate views is to use the APIs that EF provides.</span></span> <span data-ttu-id="87508-120">Quando si usa questo metodo che si ha la possibilità di serializzare le viste, tuttavia si desidera, ma è anche necessario caricare le visualizzazioni autonomamente.</span><span class="sxs-lookup"><span data-stu-id="87508-120">When using this method you have the freedom to serialize the views however you like, but you also need to load the views yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="87508-121">**Entity Framework 6 e versioni successive solo** -le API illustrate in questa sezione sono state introdotte in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="87508-121">**EF6 Onwards Only** - The APIs shown in this section were introduced in Entity Framework 6.</span></span> <span data-ttu-id="87508-122">Se si usa una versione precedente queste informazioni non valide.</span><span class="sxs-lookup"><span data-stu-id="87508-122">If you are using an earlier version this information does not apply.</span></span>

### <a name="generating-views"></a><span data-ttu-id="87508-123">La generazione di visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="87508-123">Generating Views</span></span>

<span data-ttu-id="87508-124">Le API per generare visualizzazioni sono nella classe System.Data.Entity.Core.Mapping.StorageMappingItemCollection.</span><span class="sxs-lookup"><span data-stu-id="87508-124">The APIs to generate views are on the System.Data.Entity.Core.Mapping.StorageMappingItemCollection class.</span></span> <span data-ttu-id="87508-125">È possibile recuperare un StorageMappingCollection per un contesto con l'elemento MetadataWorkspace di ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="87508-125">You can retrieve a StorageMappingCollection for a Context by using the MetadataWorkspace of an ObjectContext.</span></span> <span data-ttu-id="87508-126">Se si usa l'API DbContext più recente, quindi è possibile accedere a questa usando il IObjectContextAdapter simile alla seguente, in questo codice è disponibile un'istanza di DbContext derivata denominata dbContext:</span><span class="sxs-lookup"><span data-stu-id="87508-126">If you are using the newer DbContext API then you can access this by using the IObjectContextAdapter like below, in this code we have an instance of your derived DbContext called dbContext:</span></span>

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

<span data-ttu-id="87508-127">Dopo aver StorageMappingItemCollection sia creato è possibile ottenere l'accesso ai metodi GenerateViews e ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="87508-127">Once you have the StorageMappingItemCollection then you can get access to the GenerateViews and ComputeMappingHashValue methods.</span></span>

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

<span data-ttu-id="87508-128">Il primo metodo crea un dizionario con una voce per ogni visualizzazione nel mapping del contenitore.</span><span class="sxs-lookup"><span data-stu-id="87508-128">The first method creates a dictionary with an entry for each view in the container mapping.</span></span> <span data-ttu-id="87508-129">Il secondo metodo calcola un valore hash per il mapping singolo contenitore e viene utilizzato in fase di esecuzione per convalidare che il modello non è stato modificato poiché le visualizzazioni sono state generate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="87508-129">The second method computes a hash value for the single container mapping and is used at runtime to validate that the model has not changed since the views were pre-generated.</span></span> <span data-ttu-id="87508-130">Esegue l'override dei due metodi vengono fornito per scenari complessi che coinvolgono più mapping del contenitore.</span><span class="sxs-lookup"><span data-stu-id="87508-130">Overrides of the two methods are provided for complex scenarios involving multiple container mappings.</span></span>

<span data-ttu-id="87508-131">Durante la generazione di visualizzazioni verrà chiamare il metodo GenerateViews e quindi scrivere il EntitySetBase e DbMappingView risultante.</span><span class="sxs-lookup"><span data-stu-id="87508-131">When generating views you will call the GenerateViews method and then write out the resulting EntitySetBase and DbMappingView.</span></span> <span data-ttu-id="87508-132">È necessario anche archiviare l'hash generato dal metodo ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="87508-132">You will also need to store the hash generated by the ComputeMappingHashValue method.</span></span>

### <a name="loading-views"></a><span data-ttu-id="87508-133">Il caricamento di viste</span><span class="sxs-lookup"><span data-stu-id="87508-133">Loading Views</span></span>

<span data-ttu-id="87508-134">Per caricare le visualizzazioni generate dal metodo GenerateViews, è possibile fornire Entity Framework con una classe che eredita dalla classe astratta DbMappingViewCache.</span><span class="sxs-lookup"><span data-stu-id="87508-134">In order to load the views generated by the GenerateViews method, you can provide EF with a class that inherits from the DbMappingViewCache abstract class.</span></span> <span data-ttu-id="87508-135">DbMappingViewCache specifica due metodi che è necessario implementare:</span><span class="sxs-lookup"><span data-stu-id="87508-135">DbMappingViewCache specifies two methods that you must implement:</span></span>

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

<span data-ttu-id="87508-136">La proprietà MappingHashValue deve restituire l'hash generato dal metodo ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="87508-136">The MappingHashValue property must return the hash generated by the ComputeMappingHashValue method.</span></span> <span data-ttu-id="87508-137">Quando Entity Framework è corso porre la domanda per le visualizzazioni innanzitutto genererà e confrontare il valore hash del modello con il valore hash restituito da questa proprietà.</span><span class="sxs-lookup"><span data-stu-id="87508-137">When EF is going to ask for views it will first generate and compare the hash value of the model with the hash returned by this property.</span></span> <span data-ttu-id="87508-138">Se non corrispondono a Entity Framework genererà un'eccezione EntityCommandCompilationException.</span><span class="sxs-lookup"><span data-stu-id="87508-138">If they do not match then EF will throw an EntityCommandCompilationException exception.</span></span>

<span data-ttu-id="87508-139">Il metodo GetView accetterà un EntitySetBase ed è necessario restituire un DbMappingVIew contenente EntitySql che è stato generato per cui è stata associata la EntitySetBase specificata nel dizionario generato dal metodo GenerateViews.</span><span class="sxs-lookup"><span data-stu-id="87508-139">The GetView method will accept an EntitySetBase and you need to return a DbMappingVIew containing the EntitySql that was generated for that was associated with the given EntitySetBase in the dictionary generated by the GenerateViews method.</span></span> <span data-ttu-id="87508-140">Se vengono richiesti a Entity Framework per una vista che non è quindi GetView deve restituire null.</span><span class="sxs-lookup"><span data-stu-id="87508-140">If EF asks for a view that you do not have then GetView should return null.</span></span>

<span data-ttu-id="87508-141">Di seguito è riportato un estratto DbMappingViewCache generata con strumenti avanzati di come descritto in precedenza, in esso che vediamo un modo per archiviare e recuperare il EntitySql necessari.</span><span class="sxs-lookup"><span data-stu-id="87508-141">The following is an extract from the DbMappingViewCache that is generated with the Power Tools as described above, in it we see one way to store and retrieve the EntitySql required.</span></span>

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

<span data-ttu-id="87508-142">Perché usare Entity Framework il DbMappingViewCache è aggiungere utilizzare DbMappingViewCacheTypeAttribute, specificare il contesto di cui è stato creato per.</span><span class="sxs-lookup"><span data-stu-id="87508-142">To have EF use your DbMappingViewCache you add use the DbMappingViewCacheTypeAttribute, specifying the context that it was created for.</span></span> <span data-ttu-id="87508-143">Nel codice seguente viene associata la BlogContext con la classe MyMappingViewCache.</span><span class="sxs-lookup"><span data-stu-id="87508-143">In the code below we associate the BlogContext with the MyMappingViewCache class.</span></span>

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

<span data-ttu-id="87508-144">Per scenari più complessi, è possono specificare le istanze di cache di visualizzazione di mapping specificando una factory di cache di visualizzazione di mapping.</span><span class="sxs-lookup"><span data-stu-id="87508-144">For more complex scenarios, mapping view cache instances can be provided by specifying a mapping view cache factory.</span></span> <span data-ttu-id="87508-145">Questa operazione può essere eseguita mediante l'implementazione della classe astratta System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span><span class="sxs-lookup"><span data-stu-id="87508-145">This can be done by implementing the abstract class System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span></span> <span data-ttu-id="87508-146">L'istanza della factory della cache Visualizza mapping utilizzato può essere recuperato o impostato utilizzando il StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span><span class="sxs-lookup"><span data-stu-id="87508-146">The instance of the mapping view cache factory that is used can be retrieved or set using the StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span></span>
