---
title: Caricamento di dati correlati - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: bfabe8fd5b0a64edd5d97baff3beab9d712f1c20
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654637"
---
# <a name="loading-related-data"></a><span data-ttu-id="63933-102">Caricamento di dati correlati</span><span class="sxs-lookup"><span data-stu-id="63933-102">Loading Related Data</span></span>

<span data-ttu-id="63933-103">Entity Framework Core consente di usare le proprietà di navigazione nel modello per caricare entità correlate.</span><span class="sxs-lookup"><span data-stu-id="63933-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="63933-104">Esistono tre modelli di O/RM (Object-Relational Mapping) comuni usati per caricare i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="63933-104">There are three common O/RM patterns used to load related data.</span></span>

* <span data-ttu-id="63933-105">**Caricamento eager** significa che i dati correlati vengono caricati dal database come parte della query iniziale.</span><span class="sxs-lookup"><span data-stu-id="63933-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="63933-106">**Caricamento esplicito** significa che i dati correlati vengono caricati in modo esplicito dal database in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="63933-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="63933-107">**Caricamento lazy** significa che i dati correlati vengono caricati in modo trasparente dal database quando si accede alla proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="63933-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="63933-108">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="63933-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="63933-109">Caricamento eager</span><span class="sxs-lookup"><span data-stu-id="63933-109">Eager loading</span></span>

<span data-ttu-id="63933-110">È possibile usare il metodo `Include` per specificare i dati correlati da includere nei risultati della query.</span><span class="sxs-lookup"><span data-stu-id="63933-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="63933-111">Nell'esempio seguente la proprietà `Posts` dei blog restituiti nei risultati verrà popolata con i post correlati.</span><span class="sxs-lookup"><span data-stu-id="63933-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="63933-112">Entity Framework Core correggerà automaticamente le proprietà di navigazione per qualsiasi altra entità caricata in precedenza nell'istanza di contesto.</span><span class="sxs-lookup"><span data-stu-id="63933-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="63933-113">Anche se i dati per una proprietà di navigazione non vengono inclusi in modo esplicito, la proprietà può comunque essere popolata se alcune o tutte le entità correlate sono state caricate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="63933-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

<span data-ttu-id="63933-114">È possibile includere dati correlati da più relazioni in una singola query.</span><span class="sxs-lookup"><span data-stu-id="63933-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="63933-115">Inclusione di più livelli</span><span class="sxs-lookup"><span data-stu-id="63933-115">Including multiple levels</span></span>

<span data-ttu-id="63933-116">È possibile eseguire il drill-down delle relazioni per includere più livelli di dati correlati tramite il metodo `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="63933-116">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="63933-117">L'esempio seguente carica tutti i blog, i post correlati e l'autore di ogni post.</span><span class="sxs-lookup"><span data-stu-id="63933-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#SingleThenInclude)]

<span data-ttu-id="63933-118">È possibile concatenare più chiamate a `ThenInclude` per continuare a includere ulteriori livelli di dati correlati.</span><span class="sxs-lookup"><span data-stu-id="63933-118">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="63933-119">È possibile combinare tutto ciò per includere dati correlati da più livelli e più nodi radice nella stessa query.</span><span class="sxs-lookup"><span data-stu-id="63933-119">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="63933-120">È possibile che si vogliano includere più entità correlate per una delle entità incluse.</span><span class="sxs-lookup"><span data-stu-id="63933-120">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="63933-121">Quando ad esempio si eseguono query per `Blogs`, è necessario includere `Posts` e poi si può anche decidere di includere `Author` e `Tags` per `Posts`.</span><span class="sxs-lookup"><span data-stu-id="63933-121">For example, when querying `Blogs`, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="63933-122">A tale scopo, occorre specificare ogni percorso di inclusione iniziando dalla radice.</span><span class="sxs-lookup"><span data-stu-id="63933-122">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="63933-123">Ad esempio, `Blog -> Posts -> Author` e `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="63933-123">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="63933-124">Questo non significa che si otterranno join ridondanti. Nella maggior parte dei casi EF consoliderà i join durante la generazione di SQL.</span><span class="sxs-lookup"><span data-stu-id="63933-124">This does not mean you will get redundant joins; in most cases, EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

> [!CAUTION]
> <span data-ttu-id="63933-125">Dalla versione 3.0.0, ogni `Include` provocherà l'aggiunta di un JOIN aggiuntivo alle query SQL prodotte dai provider relazionali, mentre le versioni precedenti generavano query SQL aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="63933-125">Since version 3.0.0, each `Include` will cause an additional JOIN to be added to SQL queries produced by relational providers, whereas previous versions generated additional SQL queries.</span></span> <span data-ttu-id="63933-126">Questo può modificare in modo significativo le prestazioni delle query, per un miglioramento o peggio.</span><span class="sxs-lookup"><span data-stu-id="63933-126">This can significantly change the performance of your queries, for better or worse.</span></span> <span data-ttu-id="63933-127">In particolare, è possibile che le query LINQ con un numero estremamente elevato di operatori di `Include` debbano essere suddivise in più query LINQ separate per evitare il problema di esplosione cartesiana.</span><span class="sxs-lookup"><span data-stu-id="63933-127">In particular, LINQ queries with an exceedingly high number of `Include` operators may need to be broken down into multiple separate LINQ queries in order to avoid the cartesian explosion problem.</span></span>

### <a name="include-on-derived-types"></a><span data-ttu-id="63933-128">Inclusione per i tipi derivati</span><span class="sxs-lookup"><span data-stu-id="63933-128">Include on derived types</span></span>

<span data-ttu-id="63933-129">È possibile includere dati correlati dalle navigazioni definite solo per un tipo derivato mediante `Include` e `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="63933-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span>

<span data-ttu-id="63933-130">Dato il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="63933-130">Given the following model:</span></span>

```csharp
public class SchoolContext : DbContext
{
    public DbSet<Person> People { get; set; }
    public DbSet<School> Schools { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<School>().HasMany(s => s.Students).WithOne(s => s.School);
    }
}

public class Person
{
    public int Id { get; set; }
    public string Name { get; set; }
}

public class Student : Person
{
    public School School { get; set; }
}

public class School
{
    public int Id { get; set; }
    public string Name { get; set; }

    public List<Student> Students { get; set; }
}
```

<span data-ttu-id="63933-131">Il contenuto della navigazione `School` di tutte le entità People che sono Student può essere caricato in modalità eager usando vari modelli:</span><span class="sxs-lookup"><span data-stu-id="63933-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

* <span data-ttu-id="63933-132">Cast</span><span class="sxs-lookup"><span data-stu-id="63933-132">using cast</span></span>

  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

* <span data-ttu-id="63933-133">Operatore `as`</span><span class="sxs-lookup"><span data-stu-id="63933-133">using `as` operator</span></span>

  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

* <span data-ttu-id="63933-134">Overload di `Include` che accetta parametri di tipo `string`</span><span class="sxs-lookup"><span data-stu-id="63933-134">using overload of `Include` that takes parameter of type `string`</span></span>

  ```csharp
  context.People.Include("School").ToList()
  ```

## <a name="explicit-loading"></a><span data-ttu-id="63933-135">Caricamento esplicito</span><span class="sxs-lookup"><span data-stu-id="63933-135">Explicit loading</span></span>

<span data-ttu-id="63933-136">È possibile caricare in modo esplicito una proprietà di navigazione tramite l'API `DbContext.Entry(...)`.</span><span class="sxs-lookup"><span data-stu-id="63933-136">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="63933-137">È anche possibile caricare in modo esplicito una proprietà di navigazione eseguendo una query separata che restituisce le entità correlate.</span><span class="sxs-lookup"><span data-stu-id="63933-137">You can also explicitly load a navigation property by executing a separate query that returns the related entities.</span></span> <span data-ttu-id="63933-138">Se è abilitato il rilevamento delle modifiche, durante il caricamento di un'entità EF Core imposterà automaticamente le proprietà di navigazione dell'entità appena caricata in modo da fare riferimento alle entità già caricate e imposterà le proprietà di navigazione delle entità già caricate in modo da fare riferimento all'entità appena caricata.</span><span class="sxs-lookup"><span data-stu-id="63933-138">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="63933-139">Esecuzione di query su entità correlate</span><span class="sxs-lookup"><span data-stu-id="63933-139">Querying related entities</span></span>

<span data-ttu-id="63933-140">È anche possibile ottenere una query LINQ che rappresenta il contenuto di una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="63933-140">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="63933-141">Ciò consente di eseguire operazioni quali l'esecuzione di un operatore di aggregazione sulle entità correlate senza caricarle in memoria.</span><span class="sxs-lookup"><span data-stu-id="63933-141">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="63933-142">È anche possibile filtrare le entità correlate che vengono caricate in memoria.</span><span class="sxs-lookup"><span data-stu-id="63933-142">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="63933-143">Caricamento lazy</span><span class="sxs-lookup"><span data-stu-id="63933-143">Lazy loading</span></span>

<span data-ttu-id="63933-144">Il modo più semplice per usare il caricamento lazy consiste nell'installare il pacchetto [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) e abilitarlo con una chiamata a `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="63933-144">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="63933-145">Esempio:</span><span class="sxs-lookup"><span data-stu-id="63933-145">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```

<span data-ttu-id="63933-146">O quando si usa AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="63933-146">Or when using AddDbContext:</span></span>

```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```

<span data-ttu-id="63933-147">EF Core abiliterà quindi il caricamento lazy per qualsiasi proprietà di navigazione che può essere sottoposta a override, ovvero deve essere `virtual` e in una classe ereditabile.</span><span class="sxs-lookup"><span data-stu-id="63933-147">EF Core will then enable lazy loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="63933-148">Ad esempio, nelle entità seguenti, le proprietà di navigazione `Post.Blog` e `Blog.Posts` vengono caricate in modalità lazy.</span><span class="sxs-lookup"><span data-stu-id="63933-148">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>

```csharp
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public virtual Blog Blog { get; set; }
}
```

### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="63933-149">Caricamento lazy senza proxy</span><span class="sxs-lookup"><span data-stu-id="63933-149">Lazy loading without proxies</span></span>

<span data-ttu-id="63933-150">I proxy di caricamento lazy operano inserendo il servizio `ILazyLoader` in un'entità, come descritto in [Costruttori di tipi di entità](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="63933-150">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="63933-151">Esempio:</span><span class="sxs-lookup"><span data-stu-id="63933-151">For example:</span></span>

```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```

<span data-ttu-id="63933-152">In questo caso non è richiesto che i tipi di entità vengano ereditati o che le proprietà di navigazione siano virtuali e le istanze di entità possono essere create con `new` per eseguire il caricamento lazy dopo il collegamento a un contesto.</span><span class="sxs-lookup"><span data-stu-id="63933-152">This doesn't require entity types to be inherited from or navigation properties to be virtual, and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="63933-153">Tuttavia, è necessario un riferimento al servizio `ILazyLoader`, che viene definito nel pacchetto [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/).</span><span class="sxs-lookup"><span data-stu-id="63933-153">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="63933-154">Questo pacchetto contiene un set minimo di tipi in modo che vi sia un impatto minimo per le dipendenze.</span><span class="sxs-lookup"><span data-stu-id="63933-154">This package contains a minimal set of types so that there is very little impact in depending on it.</span></span> <span data-ttu-id="63933-155">Tuttavia, per evitare completamente di dipendere da pacchetti di EF Core nei tipi di entità, è possibile inserire il metodo `ILazyLoader.Load` come delegato.</span><span class="sxs-lookup"><span data-stu-id="63933-155">However, to completely avoid depending on any EF Core packages in the entity types, it is possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="63933-156">Esempio:</span><span class="sxs-lookup"><span data-stu-id="63933-156">For example:</span></span>

```csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```

<span data-ttu-id="63933-157">Il codice precedente usa un metodo di estensione `Load` per chiarire l'uso del delegato:</span><span class="sxs-lookup"><span data-stu-id="63933-157">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>

```csharp
public static class PocoLoadingExtensions
{
    public static TRelated Load<TRelated>(
        this Action<object, string> loader,
        object entity,
        ref TRelated navigationField,
        [CallerMemberName] string navigationName = null)
        where TRelated : class
    {
        loader?.Invoke(entity, navigationName);

        return navigationField;
    }
}
```

> [!NOTE]  
> <span data-ttu-id="63933-158">Il parametro del costruttore per il delegato di caricamento lazy deve essere chiamato "lazyLoader".</span><span class="sxs-lookup"><span data-stu-id="63933-158">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="63933-159">La possibilità di configurare l'uso di un nome diverso è pianificata per una versione futura.</span><span class="sxs-lookup"><span data-stu-id="63933-159">Configuration to use a different name than this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="63933-160">Dati correlati e serializzazione</span><span class="sxs-lookup"><span data-stu-id="63933-160">Related data and serialization</span></span>

<span data-ttu-id="63933-161">Dato che EF Core correggerà automaticamente le proprietà di navigazione, è possibile che il grafo degli oggetti contenga cicli.</span><span class="sxs-lookup"><span data-stu-id="63933-161">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="63933-162">Ad esempio, il caricamento di un blog e dei post correlati risulterà in un oggetto blog che fa riferimento a una raccolta di post.</span><span class="sxs-lookup"><span data-stu-id="63933-162">For example, loading a blog and its related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="63933-163">Ognuno di tali post avrà un riferimento al blog.</span><span class="sxs-lookup"><span data-stu-id="63933-163">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="63933-164">Alcuni framework di serializzazione non consentono tali cicli.</span><span class="sxs-lookup"><span data-stu-id="63933-164">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="63933-165">Ad esempio, Json.NET genererà l'eccezione seguente se viene rilevato un ciclo.</span><span class="sxs-lookup"><span data-stu-id="63933-165">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="63933-166">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'. (Newtonsoft.Json.JsonSerializationException: ciclo autoreferenziale rilevato per la proprietà 'Blog' con tipo 'MyApplication.Models.Blog')</span><span class="sxs-lookup"><span data-stu-id="63933-166">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="63933-167">Se si usa ASP.NET Core, è possibile configurare Json.NET per ignorare i cicli trovati nel grafo degli oggetti.</span><span class="sxs-lookup"><span data-stu-id="63933-167">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="63933-168">Questa operazione viene eseguita nel metodo `ConfigureServices(...)` in `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="63933-168">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

```csharp
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

<span data-ttu-id="63933-169">In alternativa è possibile aggiungere l'attributo `[JsonIgnore]` a una proprietà di navigazione, per indicare a Json.NET di non attraversare la proprietà di navigazione durante la serializzazione.</span><span class="sxs-lookup"><span data-stu-id="63933-169">Another alternative is to decorate one of the navigation properties with the `[JsonIgnore]` attribute, which instructs Json.NET to not traverse that navigation property while serializing.</span></span>
