---
title: Caricamento di entità - EF6 correlati
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
caps.latest.revision: 3
ms.openlocfilehash: e7adc9aea11a7a8e9b87b4f9e9120aa7316588db
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121148"
---
# <a name="loading-related-entities"></a><span data-ttu-id="cacf0-102">Il caricamento di entità correlate</span><span class="sxs-lookup"><span data-stu-id="cacf0-102">Loading Related Entities</span></span>
<span data-ttu-id="cacf0-103">Entity Framework supporta tre modi per caricare i dati correlati - caricamento eager, il caricamento lazy e il caricamento esplicito.</span><span class="sxs-lookup"><span data-stu-id="cacf0-103">Entity Framework supports three ways to load related data - eager loading, lazy loading and explicit loading.</span></span> <span data-ttu-id="cacf0-104">Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.</span><span class="sxs-lookup"><span data-stu-id="cacf0-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="eagerly-loading"></a><span data-ttu-id="cacf0-105">Il caricamento in blocco</span><span class="sxs-lookup"><span data-stu-id="cacf0-105">Eagerly Loading</span></span>  

<span data-ttu-id="cacf0-106">Il caricamento eager è il processo in base al quale una query per un tipo di entità carica anche entità correlate come parte della query.</span><span class="sxs-lookup"><span data-stu-id="cacf0-106">Eager loading is the process whereby a query for one type of entity also loads related entities as part of the query.</span></span> <span data-ttu-id="cacf0-107">Il caricamento eager viene ottenuto mediante l'utilizzo del metodo Include.</span><span class="sxs-lookup"><span data-stu-id="cacf0-107">Eager loading is achieved by use of the Include method.</span></span> <span data-ttu-id="cacf0-108">Ad esempio, le query seguenti caricherà i blog e tutti i post correlati per ciascun blog.</span><span class="sxs-lookup"><span data-stu-id="cacf0-108">For example, the queries below will load blogs and all the posts related to each blog.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                          .Include(b => b.Posts)
                          .ToList();

    // Load one blogs and its related posts
    var blog1 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include(b => b.Posts)
                        .FirstOrDefault();

    // Load all blogs and related posts  
    // using a string to specify the relationship
    var blogs2 = context.Blogs
                          .Include("Posts")
                          .ToList();

    // Load one blog and its related posts  
    // using a string to specify the relationship
    var blog2 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include("Posts")
                        .FirstOrDefault();
}
```  

<span data-ttu-id="cacf0-109">Si noti che Include un metodo di estensione nello spazio dei nomi di Data. Entity pertanto assicurarsi che si utilizza tale spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="cacf0-109">Note that Include is an extension method in the System.Data.Entity namespace so make sure you are using that namespace.</span></span>  

### <a name="eagerly-loading-multiple-levels"></a><span data-ttu-id="cacf0-110">Accuratamente il caricamento di più livelli</span><span class="sxs-lookup"><span data-stu-id="cacf0-110">Eagerly loading multiple levels</span></span>  

<span data-ttu-id="cacf0-111">È anche possibile caricare rapidamente più livelli di entità correlate.</span><span class="sxs-lookup"><span data-stu-id="cacf0-111">It is also possible to eagerly load multiple levels of related entities.</span></span> <span data-ttu-id="cacf0-112">Le query seguenti mostrano esempi di come eseguire questa operazione per le proprietà di navigazione di riferimento sia alle raccolte.</span><span class="sxs-lookup"><span data-stu-id="cacf0-112">The queries below show examples of how to do this for both collection and reference navigation properties.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments
    var blogs1 = context.Blogs
                       .Include(b => b.Posts.Select(p => p.Comments))
                       .ToList();

    // Load all users their related profiles, and related avatar
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships
    var blogs2 = context.Blogs
                       .Include("Posts.Comments")
                       .ToList();

    // Load all users their related profiles, and related avatar  
    // using a string to specify the relationships
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

<span data-ttu-id="cacf0-113">Si noti che non è attualmente possibile filtrare quali entità correlate vengono caricate.</span><span class="sxs-lookup"><span data-stu-id="cacf0-113">Note that it is not currently possible to filter which related entities are loaded.</span></span> <span data-ttu-id="cacf0-114">Includere will inserire sempre in tutte le relative entità.</span><span class="sxs-lookup"><span data-stu-id="cacf0-114">Include will always bring in all related entities.</span></span>  

## <a name="lazy-loading"></a><span data-ttu-id="cacf0-115">Caricamento lazy</span><span class="sxs-lookup"><span data-stu-id="cacf0-115">Lazy Loading</span></span>  

<span data-ttu-id="cacf0-116">Il caricamento lazy è il processo in base al quale un'entità o una raccolta di entità viene caricata automaticamente dal database la prima volta che si accede a una proprietà che fanno riferimento alle entità/entità.</span><span class="sxs-lookup"><span data-stu-id="cacf0-116">Lazy loading is the process whereby an entity or collection of entities is automatically loaded from the database the first time that a property referring to the entity/entities is accessed.</span></span> <span data-ttu-id="cacf0-117">Quando si usano i tipi di entità POCO, il caricamento lazy viene realizzato mediante la creazione di istanze di tipi proxy derivato e quindi si esegue l'override proprietà virtuali per aggiungere l'hook del caricamento.</span><span class="sxs-lookup"><span data-stu-id="cacf0-117">When using POCO entity types, lazy loading is achieved by creating instances of derived proxy types and then overriding virtual properties to add the loading hook.</span></span> <span data-ttu-id="cacf0-118">Ad esempio, quando si usa la classe di entità Blog definita di seguito, i post correlati verranno caricati la prima volta che si accede alla proprietà di navigazione post:</span><span class="sxs-lookup"><span data-stu-id="cacf0-118">For example, when using the Blog entity class defined below, the related Posts will be loaded the first time the Posts navigation property is accessed:</span></span>  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public virtual ICollection<Post> Posts { get; set; }  
}
```  

### <a name="turn-lazy-loading-off-for-serialization"></a><span data-ttu-id="cacf0-119">Disattivare caricamento lazy off per la serializzazione</span><span class="sxs-lookup"><span data-stu-id="cacf0-119">Turn lazy loading off for serialization</span></span>  

<span data-ttu-id="cacf0-120">Serializzazione e il caricamento lazy non combinare correttamente e se non si presta attenzione possono finire l'esecuzione di query per l'intero database solo perché il caricamento lazy è abilitato.</span><span class="sxs-lookup"><span data-stu-id="cacf0-120">Lazy loading and serialization don’t mix well, and if you aren’t careful you can end up querying for your entire database just because lazy loading is enabled.</span></span> <span data-ttu-id="cacf0-121">La maggior parte dei serializzatori funzionano l'accesso a ogni proprietà in un'istanza di un tipo.</span><span class="sxs-lookup"><span data-stu-id="cacf0-121">Most serializers work by accessing each property on an instance of a type.</span></span> <span data-ttu-id="cacf0-122">Accesso a proprietà attiva il caricamento lazy, in modo più entità vengono serializzate.</span><span class="sxs-lookup"><span data-stu-id="cacf0-122">Property access triggers lazy loading, so more entities get serialized.</span></span> <span data-ttu-id="cacf0-123">Su tali entità sono accessibili e vengono caricate anche altre entità.</span><span class="sxs-lookup"><span data-stu-id="cacf0-123">On those entities properties are accessed, and even more entities are loaded.</span></span> <span data-ttu-id="cacf0-124">È buona norma attivare lazy caricamento disattivato prima di serializzazione di un'entità.</span><span class="sxs-lookup"><span data-stu-id="cacf0-124">It’s a good practice to turn lazy loading off before you serialize an entity.</span></span> <span data-ttu-id="cacf0-125">Le sezioni seguenti illustrano come eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="cacf0-125">The following sections show how to do this.</span></span>  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a><span data-ttu-id="cacf0-126">La disattivazione di caricamento lazy per le proprietà di navigazione specifici</span><span class="sxs-lookup"><span data-stu-id="cacf0-126">Turning off lazy loading for specific navigation properties</span></span>  

<span data-ttu-id="cacf0-127">Il caricamento lazy di raccolta post può essere disattivato impostando la proprietà post non virtuale:</span><span class="sxs-lookup"><span data-stu-id="cacf0-127">Lazy loading of the Posts collection can be turned off by making the Posts property non-virtual:</span></span>  

``` csharp
public class Blog
{  
    public int BlogId { get; set; }  
    public string Name { get; set; }  
    public string Url { get; set; }  
    public string Tags { get; set; }  

    public ICollection<Post> Posts { get; set; }  
}
```  

<span data-ttu-id="cacf0-128">Il caricamento dei post di raccolta è possibile ancora usando il caricamento eager (vedere *accuratamente il caricamento* sopra) o il metodo Load (vedere *caricamento esplicito* sotto).</span><span class="sxs-lookup"><span data-stu-id="cacf0-128">Loading of the Posts collection can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

### <a name="turn-off-lazy-loading-for-all-entities"></a><span data-ttu-id="cacf0-129">Disattivare il caricamento lazy per tutte le entità</span><span class="sxs-lookup"><span data-stu-id="cacf0-129">Turn off lazy loading for all entities</span></span>  

<span data-ttu-id="cacf0-130">Il caricamento lazy può essere disattivato per tutte le entità nel contesto impostando un flag della proprietà di configurazione.</span><span class="sxs-lookup"><span data-stu-id="cacf0-130">Lazy loading can be turned off for all entities in the context by setting a flag on the Configuration property.</span></span> <span data-ttu-id="cacf0-131">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cacf0-131">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

<span data-ttu-id="cacf0-132">Caricamento di entità correlate è possibile ancora usando il caricamento eager (vedere *accuratamente il caricamento* sopra) o il metodo Load (vedere *caricamento esplicito* sotto).</span><span class="sxs-lookup"><span data-stu-id="cacf0-132">Loading of related entities can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

## <a name="explicitly-loading"></a><span data-ttu-id="cacf0-133">Caricamento esplicito</span><span class="sxs-lookup"><span data-stu-id="cacf0-133">Explicitly Loading</span></span>  

<span data-ttu-id="cacf0-134">Anche con il caricamento lazy disabilitato è comunque possibile in modo differito caricare entità correlate, ma deve essere eseguita con una chiamata esplicita.</span><span class="sxs-lookup"><span data-stu-id="cacf0-134">Even with lazy loading disabled it is still possible to lazily load related entities, but it must be done with an explicit call.</span></span> <span data-ttu-id="cacf0-135">Per eseguire questa operazione è usare il metodo Load sulla voce dell'entità correlata.</span><span class="sxs-lookup"><span data-stu-id="cacf0-135">To do so you use the Load method on the related entity’s entry.</span></span> <span data-ttu-id="cacf0-136">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cacf0-136">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string  
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog  
    // using a string to specify the relationship
    context.Entry(blog).Collection("Posts").Load();
}
```  

<span data-ttu-id="cacf0-137">Si noti che il metodo di riferimento deve essere utilizzato quando un'entità dispone di una proprietà di navigazione a un'altra entità singola.</span><span class="sxs-lookup"><span data-stu-id="cacf0-137">Note that the Reference method should be used when an entity has a navigation property to another single entity.</span></span> <span data-ttu-id="cacf0-138">D'altra parte, il metodo di raccolta deve essere utilizzato quando un'entità dispone di una proprietà di navigazione a una raccolta di altre entità.</span><span class="sxs-lookup"><span data-stu-id="cacf0-138">On the other hand, the Collection method should be used when an entity has a navigation property to a collection of other entities.</span></span>  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a><span data-ttu-id="cacf0-139">Applicare filtri quando si caricano in modo esplicito le entità correlate</span><span class="sxs-lookup"><span data-stu-id="cacf0-139">Applying filters when explicitly loading related entities</span></span>  

<span data-ttu-id="cacf0-140">Il metodo di Query fornisce l'accesso per la query sottostante che useranno Entity Framework durante il caricamento di entità correlate.</span><span class="sxs-lookup"><span data-stu-id="cacf0-140">The Query method provides access to the underlying query that Entity Framework will use when loading related entities.</span></span> <span data-ttu-id="cacf0-141">È quindi possibile usare LINQ per applicare filtri alla query prima dell'esecuzione con una chiamata a un metodo di estensione LINQ, ad esempio ToList, carico e così via. Il metodo di Query può essere utilizzato con le proprietà di navigazione di riferimento sia insieme, ma è particolarmente utile per le raccolte in cui può essere utilizzato per caricare solo parte della raccolta.</span><span class="sxs-lookup"><span data-stu-id="cacf0-141">You can then use LINQ to apply filters to the query before executing it with a call to a LINQ extension method such as ToList, Load, etc. The Query method can be used with both reference and collection navigation properties but is most useful for collections where it can be used to load only part of the collection.</span></span> <span data-ttu-id="cacf0-142">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cacf0-142">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog  
    // using a string to specify the relationship  
    context.Entry(blog)
        .Collection("Posts")
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  

<span data-ttu-id="cacf0-143">Quando si usa il metodo di Query è in genere è consigliabile disattivare il caricamento lazy per la proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="cacf0-143">When using the Query method it is usually best to turn off lazy loading for the navigation property.</span></span> <span data-ttu-id="cacf0-144">Questo avviene perché in caso contrario, l'intera raccolta potrebbe automaticamente vengono caricata dal meccanismo di caricamento lazy prima o dopo che è stata eseguita la query filtrata.</span><span class="sxs-lookup"><span data-stu-id="cacf0-144">This is because otherwise the entire collection may get loaded automatically by the lazy loading mechanism either before or after the filtered query has been executed.</span></span>  

<span data-ttu-id="cacf0-145">Si noti che mentre la relazione può essere specificata come stringa anziché un'espressione lambda, all'oggetto IQueryable restituita non generica quando viene usata una stringa e, pertanto il metodo di Cast è in genere necessaria prima di qualcosa di utile poter eseguire con esso.</span><span class="sxs-lookup"><span data-stu-id="cacf0-145">Note that while the relationship can be specified as a string instead of a lambda expression, the returned IQueryable is not generic when a string is used and so the Cast method is usually needed before anything useful can be done with it.</span></span>  

## <a name="using-query-to-count-related-entities-without-loading-them"></a><span data-ttu-id="cacf0-146">Uso di Query per contare le entità correlate senza il caricamento</span><span class="sxs-lookup"><span data-stu-id="cacf0-146">Using Query to count related entities without loading them</span></span>  

<span data-ttu-id="cacf0-147">In alcuni casi è utile conoscere il numero di entità è correlato a un'altra entità nel database senza effettivamente incorrere nel costo di caricamento di tutte le entità.</span><span class="sxs-lookup"><span data-stu-id="cacf0-147">Sometimes it is useful to know how many entities are related to another entity in the database without actually incurring the cost of loading all those entities.</span></span> <span data-ttu-id="cacf0-148">Il metodo di Query con il metodo di conteggio di LINQ è utilizzabile a tale scopo.</span><span class="sxs-lookup"><span data-stu-id="cacf0-148">The Query method with the LINQ Count method can be used to do this.</span></span> <span data-ttu-id="cacf0-149">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cacf0-149">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has  
    var postCount = context.Entry(blog)
                          .Collection(b => b.Posts)
                          .Query()
                          .Count();
}
```  
