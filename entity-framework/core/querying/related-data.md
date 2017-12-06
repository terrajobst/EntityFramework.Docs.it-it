---
title: Dati - Core EF correlati di caricamento
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: cd26bd2e6f85083f73d97b1356d0ba38f53e0b8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="loading-related-data"></a><span data-ttu-id="c9041-102">Caricamento dei dati correlati</span><span class="sxs-lookup"><span data-stu-id="c9041-102">Loading Related Data</span></span>

<span data-ttu-id="c9041-103">Entity Framework Core consente di utilizzare le proprietà di navigazione del modello per caricare entità correlate.</span><span class="sxs-lookup"><span data-stu-id="c9041-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="c9041-104">Esistono tre modelli di O/RM comune utilizzati per caricare i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="c9041-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="c9041-105">**Caricamento eager** indica che i dati correlati vengono caricati dal database come parte della query iniziale.</span><span class="sxs-lookup"><span data-stu-id="c9041-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="c9041-106">**Caricamento esplicito** indica che i dati correlati vengono caricati in modo esplicito dal database in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="c9041-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="c9041-107">**Caricamento lazy** indica che i dati correlati vengono caricati in modo trasparente dal database quando si accede alla proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="c9041-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span> <span data-ttu-id="c9041-108">Il caricamento differito non è ancora possibile con Entity Framework di base.</span><span class="sxs-lookup"><span data-stu-id="c9041-108">Lazy loading is not yet possible with EF Core.</span></span>

> [!TIP]  
> <span data-ttu-id="c9041-109">È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) su GitHub.</span><span class="sxs-lookup"><span data-stu-id="c9041-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="c9041-110">Caricamento eager</span><span class="sxs-lookup"><span data-stu-id="c9041-110">Eager loading</span></span>

<span data-ttu-id="c9041-111">È possibile utilizzare il `Include` per specificare i dati correlati da includere nei risultati della query.</span><span class="sxs-lookup"><span data-stu-id="c9041-111">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="c9041-112">Nell'esempio seguente, sarà necessario il blog che vengono restituiti nei risultati della loro `Posts` proprietà popolata con i post correlati.</span><span class="sxs-lookup"><span data-stu-id="c9041-112">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="c9041-113">Entity Framework Core verrà automaticamente correzione proprietà di navigazione da qualsiasi altra entità che sono stata caricata in precedenza l'istanza del contesto.</span><span class="sxs-lookup"><span data-stu-id="c9041-113">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="c9041-114">Pertanto, anche se in modo esplicito non includono i dati per una proprietà di navigazione, la proprietà comunque possibile popolare se alcune o tutte le entità correlate sono state caricate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="c9041-114">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="c9041-115">È possibile includere dati correlati tratti da più relazioni in una singola query.</span><span class="sxs-lookup"><span data-stu-id="c9041-115">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="c9041-116">Inclusi più livelli</span><span class="sxs-lookup"><span data-stu-id="c9041-116">Including multiple levels</span></span>

<span data-ttu-id="c9041-117">È possibile il drill-down tramite le relazioni da includere più livelli di dati correlati mediante la `ThenInclude` metodo.</span><span class="sxs-lookup"><span data-stu-id="c9041-117">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="c9041-118">Nell'esempio seguente carica tutti i blog, i post correlati e l'autore di ogni post.</span><span class="sxs-lookup"><span data-stu-id="c9041-118">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

<span data-ttu-id="c9041-119">È possibile concatenare più chiamate a `ThenInclude` continuare inclusi ulteriori livelli di dati correlati.</span><span class="sxs-lookup"><span data-stu-id="c9041-119">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="c9041-120">È possibile combinare queste per includere dati correlati tratti da più livelli e più nodi radice nella stessa query.</span><span class="sxs-lookup"><span data-stu-id="c9041-120">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="c9041-121">È consigliabile includere più entità correlate per un'entità che è incluso.</span><span class="sxs-lookup"><span data-stu-id="c9041-121">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="c9041-122">Ad esempio, quando si eseguono query `Blog`s, includere `Posts` e si desidera includere sia il `Author` e `Tags` del `Posts`.</span><span class="sxs-lookup"><span data-stu-id="c9041-122">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="c9041-123">A tale scopo, è necessario specificare ogni percorso iniziando dalla radice di inclusione.</span><span class="sxs-lookup"><span data-stu-id="c9041-123">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="c9041-124">Ad esempio `Blog -> Posts -> Author` e `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="c9041-124">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="c9041-125">Ciò non significa che si otterranno join ridondanti, nella maggior parte dei casi che verranno consolidate EF i join durante la generazione di SQL.</span><span class="sxs-lookup"><span data-stu-id="c9041-125">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a><span data-ttu-id="c9041-126">Ignorato include</span><span class="sxs-lookup"><span data-stu-id="c9041-126">Ignored includes</span></span>

<span data-ttu-id="c9041-127">Se si modifica la query in modo che non restituisca non sono più istanze del tipo di entità che la query inizia con, vengono ignorati gli operatori di inclusione.</span><span class="sxs-lookup"><span data-stu-id="c9041-127">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="c9041-128">Nell'esempio seguente, si basano gli operatori di inclusione di `Blog`, tuttavia, il `Select` operatore viene usato per modificare la query per restituire un tipo anonimo.</span><span class="sxs-lookup"><span data-stu-id="c9041-128">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="c9041-129">In questo caso, gli operatori di inclusione non hanno effetto.</span><span class="sxs-lookup"><span data-stu-id="c9041-129">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="c9041-130">Per impostazione predefinita, Core EF registrerà un avviso quando includono operatori vengono ignorati.</span><span class="sxs-lookup"><span data-stu-id="c9041-130">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="c9041-131">Vedere [registrazione](../miscellaneous/logging.md) per ulteriori informazioni sulla visualizzazione dell'output di registrazione.</span><span class="sxs-lookup"><span data-stu-id="c9041-131">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="c9041-132">È possibile modificare il comportamento quando un operatore di inclusione viene ignorato per generare o non eseguono alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="c9041-132">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="c9041-133">Questa operazione viene eseguita quando si configura le opzioni per il contesto, in genere in `DbContext.OnConfiguring`, o in `Startup.cs` se si utilizza ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c9041-133">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="c9041-134">Caricamento esplicito</span><span class="sxs-lookup"><span data-stu-id="c9041-134">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="c9041-135">Questa funzionalità è stato introdotto in Entity Framework Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="c9041-135">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="c9041-136">È possibile caricare in modo esplicito una proprietà di navigazione tramite il `DbContext.Entry(...)` API.</span><span class="sxs-lookup"><span data-stu-id="c9041-136">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="c9041-137">Eseguendo una query separata che restituisce le entità correlate, è possibile caricare anche in modo esplicito una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="c9041-137">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="c9041-138">Se è abilitato il rilevamento delle modifiche, quindi durante il caricamento di un'entità, EF Core verrà automaticamente impostato le proprietà di navigazione del entitiy appena caricati per fare riferimento a qualsiasi entità già caricato e impostare le proprietà di navigazione delle entità già caricato per fare riferimento al entità appena caricati.</span><span class="sxs-lookup"><span data-stu-id="c9041-138">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="c9041-139">Esecuzione di query su entità correlate</span><span class="sxs-lookup"><span data-stu-id="c9041-139">Querying related entities</span></span>

<span data-ttu-id="c9041-140">È inoltre possibile ottenere una query LINQ che rappresenta il contenuto di una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="c9041-140">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="c9041-141">Ciò consente di eseguire operazioni quali l'esecuzione di un operatore di aggregazione senza il loro caricamento in memoria tramite le entità correlate.</span><span class="sxs-lookup"><span data-stu-id="c9041-141">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="c9041-142">È inoltre possibile filtrare le entità correlate vengono caricate in memoria.</span><span class="sxs-lookup"><span data-stu-id="c9041-142">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="c9041-143">Caricamento lazy</span><span class="sxs-lookup"><span data-stu-id="c9041-143">Lazy loading</span></span>

<span data-ttu-id="c9041-144">Il caricamento differito non è ancora supportato da Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c9041-144">Lazy loading is not yet supported by EF Core.</span></span> <span data-ttu-id="c9041-145">È possibile visualizzare il [elemento caricamento lazy il nostro backlog](https://github.com/aspnet/EntityFramework/issues/3797) per tenere traccia di questa funzionalità.</span><span class="sxs-lookup"><span data-stu-id="c9041-145">You can view the [lazy loading item on our backlog](https://github.com/aspnet/EntityFramework/issues/3797) to track this feature.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="c9041-146">Serializzazione e i dati correlati</span><span class="sxs-lookup"><span data-stu-id="c9041-146">Related data and serialization</span></span>

<span data-ttu-id="c9041-147">Poiché le proprietà di navigazione di correzione automaticamente verrà di Entity Framework Core, possono finire con cicli nell'oggetto grafico.</span><span class="sxs-lookup"><span data-stu-id="c9041-147">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="c9041-148">Ad esempio, il caricamento di un blog e è correlato post verrà creato un oggetto di blog che fa riferimento a un insieme di pubblicazioni.</span><span class="sxs-lookup"><span data-stu-id="c9041-148">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="c9041-149">Ognuno di tali pubblicazioni avrà un riferimento al blog.</span><span class="sxs-lookup"><span data-stu-id="c9041-149">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="c9041-150">Alcuni framework di serializzazione non consentono tali cicli.</span><span class="sxs-lookup"><span data-stu-id="c9041-150">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="c9041-151">Ad esempio, Json.NET genererà l'eccezione seguente se un ciclo è presente una.</span><span class="sxs-lookup"><span data-stu-id="c9041-151">For example, Json.NET will throw the following exception if a cycle is encoutered.</span></span>

> <span data-ttu-id="c9041-152">Newtonsoft.Json.JsonSerializationException: Self ciclo rilevato per la proprietà 'Blog' con 'MyApplication.Models.Blog' di tipo di riferimento.</span><span class="sxs-lookup"><span data-stu-id="c9041-152">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="c9041-153">Se si utilizza ASP.NET Core, è possibile configurare Json.NET per ignorare i cicli che trova nell'oggetto grafico.</span><span class="sxs-lookup"><span data-stu-id="c9041-153">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="c9041-154">Questa operazione viene eseguita `ConfigureServices(...)` metodo `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="c9041-154">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

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
