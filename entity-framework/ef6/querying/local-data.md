---
title: Dati locali - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
caps.latest.revision: 3
ms.openlocfilehash: 79f0d2175199780d41b43088832bab808ab2fff0
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121136"
---
# <a name="local-data"></a><span data-ttu-id="2357a-102">Dati locali</span><span class="sxs-lookup"><span data-stu-id="2357a-102">Local Data</span></span>
<span data-ttu-id="2357a-103">Eseguendo una query LINQ direttamente un elemento DbSet inviano sempre una query al database, ma è possibile accedere ai dati che è attualmente in memoria usando la proprietà DbSet.Local.</span><span class="sxs-lookup"><span data-stu-id="2357a-103">Running a LINQ query directly against a DbSet will always send a query to the database, but you can access the data that is currently in-memory using the DbSet.Local property.</span></span> <span data-ttu-id="2357a-104">È anche possibile accedere EF tiene traccia delle informazioni aggiuntive sulle entità usando i metodi DbContext.Entry e DbContext.ChangeTracker.Entries.</span><span class="sxs-lookup"><span data-stu-id="2357a-104">You can also access the extra information EF is tracking about your entities using the DbContext.Entry and DbContext.ChangeTracker.Entries methods.</span></span> <span data-ttu-id="2357a-105">Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.</span><span class="sxs-lookup"><span data-stu-id="2357a-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="using-local-to-look-at-local-data"></a><span data-ttu-id="2357a-106">Utilizzo locale per esaminare i dati locali</span><span class="sxs-lookup"><span data-stu-id="2357a-106">Using Local to look at local data</span></span>  

<span data-ttu-id="2357a-107">La proprietà locale di DbSet consente un facile accesso alle entità del set che non viene rilevato dal contesto e non sono state contrassegnate come eliminate.</span><span class="sxs-lookup"><span data-stu-id="2357a-107">The Local property of DbSet provides simple access to the entities of the set that are currently being tracked by the context and have not been marked as Deleted.</span></span> <span data-ttu-id="2357a-108">L'accesso alla proprietà locale non causerà mai una query da inviare al database.</span><span class="sxs-lookup"><span data-stu-id="2357a-108">Accessing the Local property never causes a query to be sent to the database.</span></span> <span data-ttu-id="2357a-109">Ciò significa che in genere viene utilizzato dopo che è già stata eseguita una query.</span><span class="sxs-lookup"><span data-stu-id="2357a-109">This means that it is usually used after a query has already been performed.</span></span> <span data-ttu-id="2357a-110">Il metodo di estensione Load può essere utilizzato per eseguire una query in modo che il contesto tiene traccia dei risultati.</span><span class="sxs-lookup"><span data-stu-id="2357a-110">The Load extension method can be used to execute a query so that the context tracks the results.</span></span> <span data-ttu-id="2357a-111">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2357a-111">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs from the database into the context
    context.Blogs.Load();

    // Add a new blog to the context
    context.Blogs.Add(new Blog { Name = "My New Blog" });

    // Mark one of the existing blogs as Deleted
    context.Blogs.Remove(context.Blogs.Find(1));

    // Loop over the blogs in the context.
    Console.WriteLine("In Local: ");
    foreach (var blog in context.Blogs.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }

    // Perform a query against the database.
    Console.WriteLine("\nIn DbSet query: ");
    foreach (var blog in context.Blogs)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }
}
```  

<span data-ttu-id="2357a-112">Se avessimo due blog nel database: 'ADO.NET Blog' con un BlogId pari a 1 - e "Blog di Visual Studio' con un BlogId pari a 2 è stato possibile prevedere il seguente output:</span><span class="sxs-lookup"><span data-stu-id="2357a-112">If we had two blogs in the database - 'ADO.NET Blog' with a BlogId of 1 and 'The Visual Studio Blog' with a BlogId of 2 - we could expect the following output:</span></span>  

```  
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

<span data-ttu-id="2357a-113">Ciò illustra tre punti:</span><span class="sxs-lookup"><span data-stu-id="2357a-113">This illustrates three points:</span></span>  

- <span data-ttu-id="2357a-114">Il nuovo blog 'My nuovo post di Blog' è incluso nella raccolta locale anche se non è ancora stato salvato nel database.</span><span class="sxs-lookup"><span data-stu-id="2357a-114">The new blog 'My New Blog' is included in the Local collection even though it has not yet been saved to the database.</span></span> <span data-ttu-id="2357a-115">Questo blog contiene una chiave primaria pari a zero perché il database non ha ancora generato un tasto effettivo per l'entità.</span><span class="sxs-lookup"><span data-stu-id="2357a-115">This blog has a primary key of zero because the database has not yet generated a real key for the entity.</span></span>  
- <span data-ttu-id="2357a-116">Blog di ADO.NET' ' non è incluso nella raccolta locale anche se si è ancora rilevato dal contesto.</span><span class="sxs-lookup"><span data-stu-id="2357a-116">The 'ADO.NET Blog' is not included in the local collection even though it is still being tracked by the context.</span></span> <span data-ttu-id="2357a-117">Questo avviene perché è stata rimossa alla classe DbSet, contrassegnandolo come eliminato.</span><span class="sxs-lookup"><span data-stu-id="2357a-117">This is because we removed it from the DbSet thereby marking it as deleted.</span></span>  
- <span data-ttu-id="2357a-118">Quando DbSet viene usato per eseguire una query il blog contrassegnato per l'eliminazione (Blog di ADO.NET) è incluso nei risultati e il nuovo post di blog (Blog di nuovo personale) non è ancora stato salvato nel database non è incluso nei risultati.</span><span class="sxs-lookup"><span data-stu-id="2357a-118">When DbSet is used to perform a query the blog marked for deletion (ADO.NET Blog) is included in the results and the new blog (My New Blog) that has not yet been saved to the database is not included in the results.</span></span> <span data-ttu-id="2357a-119">Questo avviene perché DbSet sta eseguendo una query sul database e i risultati restituiti sempre riflettano gli elementi nel database.</span><span class="sxs-lookup"><span data-stu-id="2357a-119">This is because DbSet is performing a query against the database and the results returned always reflect what is in the database.</span></span>  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a><span data-ttu-id="2357a-120">Utilizzo locale per aggiungere e rimuovere entità dal contesto</span><span class="sxs-lookup"><span data-stu-id="2357a-120">Using Local to add and remove entities from the context</span></span>  

<span data-ttu-id="2357a-121">La proprietà locale in DbSet restituisce un [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) con eventi collegati in modo che rimanga sincronizzato con il contenuto del contesto.</span><span class="sxs-lookup"><span data-stu-id="2357a-121">The Local property on DbSet returns an [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) with events hooked up such that it stays in sync with the contents of the context.</span></span> <span data-ttu-id="2357a-122">Ciò significa che le entità possono essere aggiunto o rimosso dalla raccolta locale o alla classe DbSet.</span><span class="sxs-lookup"><span data-stu-id="2357a-122">This means that entities can be added or removed from either the Local collection or the DbSet.</span></span> <span data-ttu-id="2357a-123">Significa anche che le query che offrono nuove entità nel contesto di comporterà la raccolta locale venga aggiornata con tali entità.</span><span class="sxs-lookup"><span data-stu-id="2357a-123">It also means that queries that bring new entities into the context will result in the Local collection being updated with those entities.</span></span> <span data-ttu-id="2357a-124">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2357a-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load some posts from the database into the context
    context.Posts.Where(p => p.Tags.Contains("entity-framework").Load();  

    // Get the local collection and make some changes to it
    var localPosts = context.Posts.Local;
    localPosts.Add(new Post { Name = "What's New in EF" });
    localPosts.Remove(context.Posts.Find(1));  

    // Loop over the posts in the context.
    Console.WriteLine("In Local after entity-framework query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }

    var post1 = context.Posts.Find(1);
    Console.WriteLine(
        "State of post 1: {0} is {1}",
        post1.Name,  
        context.Entry(post1).State);  

    // Query some more posts from the database
    context.Posts.Where(p => p.Tags.Contains("asp.net").Load();  

    // Loop over the posts in the context again.
    Console.WriteLine("\nIn Local after asp.net query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }
}
```  

<span data-ttu-id="2357a-125">Assumendo che alcuni post con tag 'entity framework' e 'asp.net' l'output simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="2357a-125">Assuming we had a few posts tagged with 'entity-framework' and 'asp.net' the output may look something like this:</span></span>  

```  
In Local after entity-framework query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
State of post 1: EF Beginners Guide is Deleted

In Local after asp.net query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
Found 4: ASP.NET Beginners Guide with state Unchanged
```  

<span data-ttu-id="2357a-126">Ciò illustra tre punti:</span><span class="sxs-lookup"><span data-stu-id="2357a-126">This illustrates three points:</span></span>  

- <span data-ttu-id="2357a-127">Il nuovo post 'What' s New in EF' che è stato aggiunto all'oggetto locale raccolta diventa rilevata dal contesto nello stato Added.</span><span class="sxs-lookup"><span data-stu-id="2357a-127">The new post 'What's New in EF' that was added to the Local collection becomes tracked by the context in the Added state.</span></span> <span data-ttu-id="2357a-128">Da pertanto essere inserita nel database quando viene chiamato SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="2357a-128">It will therefore be inserted into the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="2357a-129">Il post che è stato rimosso dalla raccolta locale (Guida per principianti Entity Framework) è ora contrassegnato come eliminato nel contesto.</span><span class="sxs-lookup"><span data-stu-id="2357a-129">The post that was removed from the Local collection (EF Beginners Guide) is now marked as deleted in the context.</span></span> <span data-ttu-id="2357a-130">Si verranno pertanto eliminato dal database quando viene chiamato SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="2357a-130">It will therefore be deleted from the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="2357a-131">Altri post (Guida per principianti ASP.NET) caricato nel contesto di con la seconda query viene aggiunto automaticamente alla raccolta locale.</span><span class="sxs-lookup"><span data-stu-id="2357a-131">The additional post (ASP.NET Beginners Guide) loaded into the context with the second query is automatically added to the Local collection.</span></span>  

<span data-ttu-id="2357a-132">Un ultimo aspetto da notare circa locale è che, essendo che una prestazioni ObservableCollection non sono molto utile per un numero elevato di entità.</span><span class="sxs-lookup"><span data-stu-id="2357a-132">One final thing to note about Local is that because it is an ObservableCollection performance is not great for large numbers of entities.</span></span> <span data-ttu-id="2357a-133">Pertanto se si gestiscono migliaia di entità nel contesto di potrebbe non essere consigliabile usare locale.</span><span class="sxs-lookup"><span data-stu-id="2357a-133">Therefore if you are dealing with thousands of entities in your context it may not be advisable to use Local.</span></span>  

## <a name="using-local-for-wpf-data-binding"></a><span data-ttu-id="2357a-134">Utilizzo locale per l'associazione dati WPF</span><span class="sxs-lookup"><span data-stu-id="2357a-134">Using Local for WPF data binding</span></span>  

<span data-ttu-id="2357a-135">La proprietà locale di DbSet può essere utilizzata direttamente per il data binding in un'applicazione WPF perché è un'istanza della classe ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="2357a-135">The Local property on DbSet can be used directly for data binding in a WPF application because it is an instance of ObservableCollection.</span></span> <span data-ttu-id="2357a-136">Come descritto nelle sezioni precedenti, che questo significa che verrà automaticamente rimangano sincronizzati con il contenuto del contesto e il contenuto del contesto viene automaticamente sincronizzata con esso.</span><span class="sxs-lookup"><span data-stu-id="2357a-136">As described in the previous sections this means that it will automatically stay in sync with the contents of the context and the contents of the context will automatically stay in sync with it.</span></span> <span data-ttu-id="2357a-137">Si noti che è necessario pre-popolare la raccolta locale con i dati affinché sia disponibile alcuna operazione da associare alla poiché locale non causerà mai una query sul database.</span><span class="sxs-lookup"><span data-stu-id="2357a-137">Note that you do need to pre-populate the Local collection with data for there to be anything to bind to since Local never causes a database query.</span></span>  

<span data-ttu-id="2357a-138">Non è una posizione appropriata per un esempio di associazione dati WPF completo ma gli elementi chiave sono:</span><span class="sxs-lookup"><span data-stu-id="2357a-138">This is not an appropriate place for a full WPF data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="2357a-139">Installazione di origine del binding</span><span class="sxs-lookup"><span data-stu-id="2357a-139">Setup a binding source</span></span>  
- <span data-ttu-id="2357a-140">Associarlo alla proprietà locale del set di</span><span class="sxs-lookup"><span data-stu-id="2357a-140">Bind it to the Local property of your set</span></span>  
- <span data-ttu-id="2357a-141">Popolare locale usando una query al database.</span><span class="sxs-lookup"><span data-stu-id="2357a-141">Populate Local using a query to the database.</span></span>  

## <a name="wpf-binding-to-navigation-properties"></a><span data-ttu-id="2357a-142">Associazione di WPF per le proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="2357a-142">WPF binding to navigation properties</span></span>  

<span data-ttu-id="2357a-143">Se si esegue il master/dettaglio tale associazione è possibile associare la visualizzazione dettagli per una proprietà di navigazione di una delle entità.</span><span class="sxs-lookup"><span data-stu-id="2357a-143">If you are doing master/detail data binding you may want to bind the detail view to a navigation property of one of your entities.</span></span> <span data-ttu-id="2357a-144">Un modo semplice per eseguire questa operazione è usare una classe ObservableCollection per la proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="2357a-144">An easy way to make this work is to use an ObservableCollection for the navigation property.</span></span> <span data-ttu-id="2357a-145">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2357a-145">For example:</span></span>  

``` csharp
public class Blog
{
    private readonly ObservableCollection<Post> _posts =
        new ObservableCollection<Post>();

    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual ObservableCollection<Post> Posts
    {
        get { return _posts; }
    }
}
```  

## <a name="using-local-to-clean-up-entities-in-savechanges"></a><span data-ttu-id="2357a-146">Utilizzo locale per pulire le entità in SaveChanges</span><span class="sxs-lookup"><span data-stu-id="2357a-146">Using Local to clean up entities in SaveChanges</span></span>  

<span data-ttu-id="2357a-147">Nella maggior parte dei casi entità rimosse da una proprietà di navigazione verrà non essere contrassegnata automaticamente come eliminate nel contesto.</span><span class="sxs-lookup"><span data-stu-id="2357a-147">In most cases entities removed from a navigation property will not be automatically marked as deleted in the context.</span></span> <span data-ttu-id="2357a-148">Ad esempio, se si rimuove un oggetto Post dalla raccolta Blog.Posts quindi che post non essere eliminate automaticamente quando viene chiamato SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="2357a-148">For example, if you remove a Post object from the Blog.Posts collection then that post will not be automatically deleted when SaveChanges is called.</span></span> <span data-ttu-id="2357a-149">Se è necessario che venga eliminato potrebbe essere necessario trovare queste entità inesatti e contrassegnarli come eliminato prima di chiamare SaveChanges o come parte di un metodo SaveChanges sottoposto a override.</span><span class="sxs-lookup"><span data-stu-id="2357a-149">If you need it to be deleted then you may need to find these dangling entities and mark them as deleted before calling SaveChanges or as part of an overridden SaveChanges.</span></span> <span data-ttu-id="2357a-150">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2357a-150">For example:</span></span>  

``` csharp
public override int SaveChanges()
{
    foreach (var post in this.Posts.Local.ToList())
    {
        if (post.Blog == null)
        {
            this.Posts.Remove(post);
        }
    }

    return base.SaveChanges();
}
```  

<span data-ttu-id="2357a-151">Il codice precedente Usa la raccolta locale per trovare tutti i post e segni di quelli che non è un riferimento di blog come eliminato.</span><span class="sxs-lookup"><span data-stu-id="2357a-151">The code above uses the Local collection to find all posts and marks any that do not have a blog reference as deleted.</span></span> <span data-ttu-id="2357a-152">La chiamata ToList è necessaria perché in caso contrario, verrà modificata la raccolta per il metodo Remove chiamare mentre viene enumerato.</span><span class="sxs-lookup"><span data-stu-id="2357a-152">The ToList call is required because otherwise the collection will be modified by the Remove call while it is being enumerated.</span></span> <span data-ttu-id="2357a-153">Nella maggior parte delle situazioni è possibile eseguire una query direttamente in base alla proprietà locale senza usare ToList prima di tutto.</span><span class="sxs-lookup"><span data-stu-id="2357a-153">In most other situations you can query directly against the Local property without using ToList first.</span></span>  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a><span data-ttu-id="2357a-154">Utilizzo dell'associazione di dati locali e ToBindingList for Windows Forms</span><span class="sxs-lookup"><span data-stu-id="2357a-154">Using Local and ToBindingList for Windows Forms data binding</span></span>  

<span data-ttu-id="2357a-155">Windows Form non supporta l'associazione di dati di fedeltà con ObservableCollection direttamente.</span><span class="sxs-lookup"><span data-stu-id="2357a-155">Windows Forms does not support full fidelity data binding using ObservableCollection directly.</span></span> <span data-ttu-id="2357a-156">Tuttavia, è comunque possibile usare la proprietà DbSet locale per il data binding per ottenere tutti i vantaggi descritti nelle sezioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="2357a-156">However, you can still use the DbSet Local property for data binding to get all the benefits described in the previous sections.</span></span> <span data-ttu-id="2357a-157">Questo risultato viene ottenuto tramite il metodo di estensione ToBindingList che crea un' [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementazione supportato dalla classe ObservableCollection locale.</span><span class="sxs-lookup"><span data-stu-id="2357a-157">This is achieved through the ToBindingList extension method which creates an [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementation backed by the Local ObservableCollection.</span></span>  

<span data-ttu-id="2357a-158">Non è una posizione appropriata per un esempio di associazione dati di Windows Form completo ma gli elementi chiave sono:</span><span class="sxs-lookup"><span data-stu-id="2357a-158">This is not an appropriate place for a full Windows Forms data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="2357a-159">Configurare un'origine di associazione oggetto</span><span class="sxs-lookup"><span data-stu-id="2357a-159">Setup an object binding source</span></span>  
- <span data-ttu-id="2357a-160">Associarlo alla proprietà locale del set di uso Local.ToBindingList()</span><span class="sxs-lookup"><span data-stu-id="2357a-160">Bind it to the Local property of your set using Local.ToBindingList()</span></span>  
- <span data-ttu-id="2357a-161">Popolare locale usando una query al database</span><span class="sxs-lookup"><span data-stu-id="2357a-161">Populate Local using a query to the database</span></span>  

## <a name="getting-detailed-information-about-tracked-entities"></a><span data-ttu-id="2357a-162">Visualizzare informazioni dettagliate per le entità rilevate</span><span class="sxs-lookup"><span data-stu-id="2357a-162">Getting detailed information about tracked entities</span></span>  

<span data-ttu-id="2357a-163">Molti degli esempi in questa serie di utilizzare il metodo di ingresso per restituire un'istanza di DbEntityEntry per un'entità.</span><span class="sxs-lookup"><span data-stu-id="2357a-163">Many of the examples in this series use the Entry method to return a DbEntityEntry instance for an entity.</span></span> <span data-ttu-id="2357a-164">Questo oggetto voce funge quindi il punto di partenza per la raccolta di informazioni sull'entità, ad esempio lo stato corrente, nonché per l'esecuzione di operazioni sulle entità, ad esempio in modo esplicito il caricamento di un'entità correlata.</span><span class="sxs-lookup"><span data-stu-id="2357a-164">This entry object then acts as the starting point for gathering information about the entity such as its current state, as well as for performing operations on the entity such as explicitly loading a related entity.</span></span>  

<span data-ttu-id="2357a-165">I metodi di voci restituiscono oggetti DbEntityEntry per molte o tutte le entità rilevate dal contesto.</span><span class="sxs-lookup"><span data-stu-id="2357a-165">The Entries methods return DbEntityEntry objects for many or all entities being tracked by the context.</span></span> <span data-ttu-id="2357a-166">In questo modo è possibile raccogliere informazioni o eseguire operazioni su molte entità anziché solo una singola voce.</span><span class="sxs-lookup"><span data-stu-id="2357a-166">This allows you to gather information or perform operations on many entities rather than just a single entry.</span></span> <span data-ttu-id="2357a-167">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2357a-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load some entities into the context
    context.Blogs.Load();
    context.Authors.Load();
    context.Readers.Load();

    // Make some changes
    context.Blogs.Find(1).Title = "The New ADO.NET Blog";
    context.Blogs.Remove(context.Blogs.Find(2));
    context.Authors.Add(new Author { Name = "Jane Doe" });
    context.Readers.Find(1).Username = "johndoe1987";

    // Look at the state of all entities in the context
    Console.WriteLine("All tracked entities: ");
    foreach (var entry in context.ChangeTracker.Entries())
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Find modified entities of any type
    Console.WriteLine("\nAll modified entities: ");
    foreach (var entry in context.ChangeTracker.Entries()
                              .Where(e => e.State == EntityState.Modified))
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Get some information about just the tracked blogs
    Console.WriteLine("\nTracked blogs: ");
    foreach (var entry in context.ChangeTracker.Entries<Blog>())
    {
        Console.WriteLine(
            "Found Blog {0}: {1} with original Name {2}",
            entry.Entity.BlogId,  
            entry.Entity.Name,
            entry.Property(p => p.Name).OriginalValue);
    }

    // Find all people (author or reader)
    Console.WriteLine("\nPeople: ");
    foreach (var entry in context.ChangeTracker.Entries<IPerson>())
    {
        Console.WriteLine("Found Person {0}", entry.Entity.Name);
    }
}
```  

<span data-ttu-id="2357a-168">Si noterà che è stato introdotto una classe dell'autore e il lettore nell'esempio, entrambe queste classi implementano l'interfaccia IPerson.</span><span class="sxs-lookup"><span data-stu-id="2357a-168">You'll notice we are introducing a Author and Reader class into the example - both of these classes implement the IPerson interface.</span></span>  

``` csharp
public class Author : IPerson
{
    public int AuthorId { get; set; }
    public string Name { get; set; }
    public string Biography { get; set; }
}

public class Reader : IPerson
{
    public int ReaderId { get; set; }
    public string Name { get; set; }
    public string Username { get; set; }
}

public interface IPerson
{
    string Name { get; }
}
```  

<span data-ttu-id="2357a-169">Si supponga di che disporre i dati seguenti nel database:</span><span class="sxs-lookup"><span data-stu-id="2357a-169">Let's assume we have the following data in the database:</span></span>

<span data-ttu-id="2357a-170">Blog con BlogId = 1 e nome = 'Blog di ADO.NET'</span><span class="sxs-lookup"><span data-stu-id="2357a-170">Blog with BlogId = 1 and Name = 'ADO.NET Blog'</span></span>  
<span data-ttu-id="2357a-171">Blog con BlogId = 2 e nome = 'Blog di Visual Studio'</span><span class="sxs-lookup"><span data-stu-id="2357a-171">Blog with BlogId = 2 and Name = 'The Visual Studio Blog'</span></span>  
<span data-ttu-id="2357a-172">Blog con BlogId = 3 e nome = 'Blog di .NET Framework'</span><span class="sxs-lookup"><span data-stu-id="2357a-172">Blog with BlogId = 3 and Name = '.NET Framework Blog'</span></span>  
<span data-ttu-id="2357a-173">Autore con AuthorId = 1 e nome = 'Luca Bianchi'</span><span class="sxs-lookup"><span data-stu-id="2357a-173">Author with AuthorId = 1 and Name = 'Joe Bloggs'</span></span>  
<span data-ttu-id="2357a-174">Lettore con ReaderId = 1 e nome = "John Doe"</span><span class="sxs-lookup"><span data-stu-id="2357a-174">Reader with ReaderId = 1 and Name = 'John Doe'</span></span>  

<span data-ttu-id="2357a-175">L'output dall'esecuzione del codice sarebbe:</span><span class="sxs-lookup"><span data-stu-id="2357a-175">The output from running the code would be:</span></span>  

```  
All tracked entities:
Found entity of type Blog with state Modified
Found entity of type Blog with state Deleted
Found entity of type Blog with state Unchanged
Found entity of type Author with state Unchanged
Found entity of type Author with state Added
Found entity of type Reader with state Modified

All modified entities:
Found entity of type Blog with state Modified
Found entity of type Reader with state Modified

Tracked blogs:
Found Blog 1: The New ADO.NET Blog with original Name ADO.NET Blog
Found Blog 2: The Visual Studio Blog with original Name The Visual Studio Blog
Found Blog 3: .NET Framework Blog with original Name .NET Framework Blog

People:
Found Person John Doe
Found Person Joe Bloggs
Found Person Jane Doe
```  

<span data-ttu-id="2357a-176">Questi esempi vengono illustrati diversi aspetti:</span><span class="sxs-lookup"><span data-stu-id="2357a-176">These examples illustrate several points:</span></span>  

- <span data-ttu-id="2357a-177">I metodi di voci restituiscono le voci per le entità in tutti gli stati, tra cui Deleted.</span><span class="sxs-lookup"><span data-stu-id="2357a-177">The Entries methods return entries for entities in all states, including Deleted.</span></span> <span data-ttu-id="2357a-178">Eseguire il confronto locale che esclude eliminata le entità.</span><span class="sxs-lookup"><span data-stu-id="2357a-178">Compare this to Local which excludes Deleted entities.</span></span>  
- <span data-ttu-id="2357a-179">Quando viene utilizzato il metodo di voci non generiche, vengono restituite le voci per tutti i tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="2357a-179">Entries for all entity types are returned when the non-generic Entries method is used.</span></span> <span data-ttu-id="2357a-180">Quando viene utilizzato il metodo generico voci vengono restituite solo le voci per le entità che sono istanze del tipo generico.</span><span class="sxs-lookup"><span data-stu-id="2357a-180">When the generic entries method is used entries are only returned for entities that are instances of the generic type.</span></span> <span data-ttu-id="2357a-181">Questo è stato usato in precedenza per ottenere le voci per tutti i blog.</span><span class="sxs-lookup"><span data-stu-id="2357a-181">This was used above to get entries for all blogs.</span></span> <span data-ttu-id="2357a-182">È stato usato anche per ottenere le voci per tutte le entità che implementano IPerson.</span><span class="sxs-lookup"><span data-stu-id="2357a-182">It was also used to get entries for all entities that implement IPerson.</span></span> <span data-ttu-id="2357a-183">Ciò dimostra che il tipo generico non è necessario essere un tipo di entità effettivo.</span><span class="sxs-lookup"><span data-stu-id="2357a-183">This demonstrates that the generic type does not have to be an actual entity type.</span></span>  
- <span data-ttu-id="2357a-184">LINQ agli oggetti può essere utilizzato per filtrare i risultati restituiti.</span><span class="sxs-lookup"><span data-stu-id="2357a-184">LINQ to Objects can be used to filter the results returned.</span></span> <span data-ttu-id="2357a-185">Questo è stato usato in precedenza per individuare le entità di qualsiasi tipo fino a quando vengono modificati.</span><span class="sxs-lookup"><span data-stu-id="2357a-185">This was used above to find entities of any type as long as they are modified.</span></span>  

<span data-ttu-id="2357a-186">Si noti che le istanze di DbEntityEntry contengono sempre un'entità non null.</span><span class="sxs-lookup"><span data-stu-id="2357a-186">Note that DbEntityEntry instances always contain a non-null Entity.</span></span> <span data-ttu-id="2357a-187">Le voci di relazione e quelle di stub non sono rappresentate come istanze di DbEntityEntry in modo che non è necessario per filtrare per questi.</span><span class="sxs-lookup"><span data-stu-id="2357a-187">Relationship entries and stub entries are not represented as DbEntityEntry instances so there is no need to filter for these.</span></span>
