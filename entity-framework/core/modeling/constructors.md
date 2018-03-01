---
title: "Tipi di entità con costruttori di EF Core."
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 2632488569c538a11c7a31a9a866d2fadb29eeb5
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2018
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="93722-102">Tipi di entità con costruttori</span><span class="sxs-lookup"><span data-stu-id="93722-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="93722-103">Questa funzionalità è nuova in Entity Framework Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="93722-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="93722-104">A partire da Entity Framework Core 2.1, è ora possibile definire un costruttore con parametri e avere EF Core di chiamare questo costruttore quando si crea un'istanza dell'entità.</span><span class="sxs-lookup"><span data-stu-id="93722-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="93722-105">I parametri del costruttore possono essere associati a proprietà mappate o a vari tipi di servizi per semplificare i comportamenti, ad esempio il caricamento lazy.</span><span class="sxs-lookup"><span data-stu-id="93722-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="93722-106">A partire da Entity Framework Core 2.1, tutti i binding di costruttore è per convenzione.</span><span class="sxs-lookup"><span data-stu-id="93722-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="93722-107">Configurazione dei costruttori specifici da utilizzare è pianificato per una versione futura.</span><span class="sxs-lookup"><span data-stu-id="93722-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="93722-108">Binding a proprietà mappata</span><span class="sxs-lookup"><span data-stu-id="93722-108">Binding to mapped properties</span></span>

<span data-ttu-id="93722-109">Si consideri un tipico modello/Post di Blog:</span><span class="sxs-lookup"><span data-stu-id="93722-109">Consider a typical Blog/Post model:</span></span>

```Csharp
public class Blog
{
    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }
    
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="93722-110">Quando EF Core crea istanze di questi tipi, ad esempio per i risultati di una query, verrà prima di chiamare il costruttore senza parametri predefinito e impostare ogni proprietà il valore dal database.</span><span class="sxs-lookup"><span data-stu-id="93722-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="93722-111">Tuttavia, se EF Core consente di trovare un costruttore con parametri con i nomi dei parametri e tipi che corrispondono a quelle di mapping delle proprietà, quindi chiamerà invece il costruttore con parametri con valori di tali proprietà e non verrà impostata ogni proprietà in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="93722-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="93722-112">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="93722-112">For example:</span></span>

```Csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```
<span data-ttu-id="93722-113">Alcuni aspetti da notare:</span><span class="sxs-lookup"><span data-stu-id="93722-113">Some things to note:</span></span>
* <span data-ttu-id="93722-114">Non tutte le proprietà di cui è necessario disporre di parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="93722-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="93722-115">Ad esempio, la proprietà Post.Content non è impostata da un parametro di costruttore, in modo EF Core verrà impostato dopo la chiamata al costruttore in modo normale.</span><span class="sxs-lookup"><span data-stu-id="93722-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="93722-116">Tipi di parametro e i nomi devono corrispondere i tipi di proprietà e nomi, ad eccezione del fatto che le proprietà possono essere base alla convezione Pascal mentre i parametri sono maiuscole/minuscole camel.</span><span class="sxs-lookup"><span data-stu-id="93722-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="93722-117">Core EF non è possibile impostare le proprietà di navigazione (ad esempio Blog o post precedente) utilizzando un costruttore.</span><span class="sxs-lookup"><span data-stu-id="93722-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="93722-118">Il costruttore può essere pubblico, privato, o qualsiasi altro accessibilità.</span><span class="sxs-lookup"><span data-stu-id="93722-118">The constructor can be public, private, or have any other accessibility.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="93722-119">Proprietà di sola lettura</span><span class="sxs-lookup"><span data-stu-id="93722-119">Read-only properties</span></span>

<span data-ttu-id="93722-120">Una volta che vengono impostate tramite il costruttore può senso per alcuni di essi rendere di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="93722-120">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="93722-121">È supportata da Entity Framework Core, ma ci sono alcuni aspetti da sapere:</span><span class="sxs-lookup"><span data-stu-id="93722-121">EF Core supports this, but there are some things to look out for:</span></span>
* <span data-ttu-id="93722-122">Per convenzione, le proprietà senza getter non sono mappate.</span><span class="sxs-lookup"><span data-stu-id="93722-122">Properties without getters are not mapped by convention.</span></span> <span data-ttu-id="93722-123">(In questo modo tende a eseguire il mapping di proprietà che non devono essere associate, ad esempio le proprietà calcolate).</span><span class="sxs-lookup"><span data-stu-id="93722-123">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="93722-124">Utilizzo di valori di chiave generati automaticamente richiede una proprietà chiave è di lettura / scrittura, poiché il valore della chiave deve essere impostata per il generatore di chiavi durante l'inserimento di nuove entità.</span><span class="sxs-lookup"><span data-stu-id="93722-124">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="93722-125">Un modo pratico per evitare queste operazioni consiste nell'utilizzare i metodi di impostazione private.</span><span class="sxs-lookup"><span data-stu-id="93722-125">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="93722-126">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="93722-126">For example:</span></span>
```Csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; private set; }

    public string Name { get; private set; }
    public string Author { get; private set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; private set; }

    public string Title { get; private set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; private set; }

    public Blog Blog { get; set; }
}

```
<span data-ttu-id="93722-127">Core EF considera una proprietà con un setter privata di lettura / scrittura, il che significa che tutte le proprietà vengono mappate come prima e la chiave può ancora essere generato dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="93722-127">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="93722-128">Un'alternativa all'utilizzo di metodi di impostazione private consiste nel rendere proprietà effettivamente di sola lettura e aggiungere mapping di più esplicito in OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="93722-128">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="93722-129">Analogamente, alcune proprietà è possibile rimuovere completamente e sostituite con solo i campi.</span><span class="sxs-lookup"><span data-stu-id="93722-129">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="93722-130">Si consideri, ad esempio, questi tipi di entità:</span><span class="sxs-lookup"><span data-stu-id="93722-130">For example, consider these entity types:</span></span>

```Csharp
public class Blog
{
    private int _id;

    public Blog(string name, string author)
    {
        Name = name;
        Author = author;
    }

    public string Name { get; }
    public string Author { get; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    private int _id;

    public Post(string title, DateTime postedOn)
    {
        Title = title;
        PostedOn = postedOn;
    }

    public string Title { get; }
    public string Content { get; set; }
    public DateTime PostedOn { get; }

    public Blog Blog { get; set; }
}
```
<span data-ttu-id="93722-131">E questa configurazione in OnModelCreating:</span><span class="sxs-lookup"><span data-stu-id="93722-131">And this configuration in OnModelCreating:</span></span>
```Csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Author);
            b.Property(e => e.Name);
        });

    modelBuilder.Entity<Post>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Title);
            b.Property(e => e.PostedOn);
        });
}
```
<span data-ttu-id="93722-132">Aspetti da notare:</span><span class="sxs-lookup"><span data-stu-id="93722-132">Things to note:</span></span>
* <span data-ttu-id="93722-133">La chiave "property" è un campo.</span><span class="sxs-lookup"><span data-stu-id="93722-133">The key "property" is now a field.</span></span> <span data-ttu-id="93722-134">Non è un `readonly` in modo che le chiavi generate dall'archivio possono essere utilizzate.</span><span class="sxs-lookup"><span data-stu-id="93722-134">It is not a `readonly` field so that store-generated keys can be used.</span></span> 
* <span data-ttu-id="93722-135">Le altre proprietà sono proprietà di sola lettura impostata solo nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="93722-135">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="93722-136">Se il valore della chiave primario è sempre impostato da Entity Framework o leggere dal database, quindi è necessario includerlo nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="93722-136">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="93722-137">Ciò lascia la chiave "property" come campo semplice che indica chiaramente che non deve essere impostato in modo esplicito durante la creazione di nuovi blog o post.</span><span class="sxs-lookup"><span data-stu-id="93722-137">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="93722-138">Questo codice comporterà '169' che indica che il campo non viene mai utilizzato di avviso del compilatore.</span><span class="sxs-lookup"><span data-stu-id="93722-138">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="93722-139">Può essere ignorato perché in realtà EF Core utilizza il campo in modo extralinguistic.</span><span class="sxs-lookup"><span data-stu-id="93722-139">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="93722-140">Inserimento di servizi</span><span class="sxs-lookup"><span data-stu-id="93722-140">Injecting services</span></span>

<span data-ttu-id="93722-141">EF Core può inoltre inserire i "servizi" nel costruttore del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="93722-141">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="93722-142">Di seguito, ad esempio, può essere inserite:</span><span class="sxs-lookup"><span data-stu-id="93722-142">For example, the following can be injected:</span></span>
* <span data-ttu-id="93722-143">`DbContext` -istanza del contesto corrente, che può anche essere tipizzata come il tipo DbContext derivato</span><span class="sxs-lookup"><span data-stu-id="93722-143">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="93722-144">`ILazyLoader` -il servizio di caricamento lazy, vedere il [caricamento lazy documentazione](../querying/related-data.md) per ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="93722-144">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="93722-145">`Action<object, string>` -un delegato di caricamento lazy, vedere il [caricamento lazy documentazione](../querying/related-data.md) per ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="93722-145">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="93722-146">`IEntityType` -i metadati di base di EF associato a questo tipo di entità</span><span class="sxs-lookup"><span data-stu-id="93722-146">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="93722-147">A partire da Entity Framework Core 2.1, possono essere inseriti solo i servizi noti EF Core.</span><span class="sxs-lookup"><span data-stu-id="93722-147">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="93722-148">Supporto per i servizi delle applicazioni di inserimento viene considerato per una versione futura.</span><span class="sxs-lookup"><span data-stu-id="93722-148">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="93722-149">Ad esempio, un DbContext inserito consente di accedere in modo selettivo il database per ottenere informazioni sulle entità correlate senza caricare tutti.</span><span class="sxs-lookup"><span data-stu-id="93722-149">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="93722-150">Nell'esempio riportato di seguito in questo viene utilizzato per ottenere il numero di post di blog senza caricare i post:</span><span class="sxs-lookup"><span data-stu-id="93722-150">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

```Csharp
public class Blog
{
    public Blog()
    {
    }

    private Blog(BloggingContext context)
    {
        Context = context;
    }

    private BloggingContext Context { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; set; }

    public int PostsCount
        => Posts?.Count
           ?? Context?.Set<Post>().Count(p => Id == EF.Property<int?>(p, "BlogId"))
           ?? 0;
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```
<span data-ttu-id="93722-151">Alcuni aspetti da notare in questo:</span><span class="sxs-lookup"><span data-stu-id="93722-151">A few things to notice about this:</span></span>
* <span data-ttu-id="93722-152">Il costruttore è privato, in quanto è sufficiente chiamare mai EF core e non è presente un altro costruttore pubblico per uso generale.</span><span class="sxs-lookup"><span data-stu-id="93722-152">The constructor is private, since it is only ever call by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="93722-153">Il codice che usa il servizio inserito (ad esempio il contesto) è difensivo su di esso viene `null` per gestire i casi in cui Core EF non crea l'istanza.</span><span class="sxs-lookup"><span data-stu-id="93722-153">The code using the injected service (i.e. the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="93722-154">Poiché il servizio viene archiviato in una proprietà di lettura/scrittura, verrà reimpostato quando l'entità è associata a una nuova istanza di contesto.</span><span class="sxs-lookup"><span data-stu-id="93722-154">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="93722-155">Inserendo l'elemento DbContext simile al seguente è spesso considerato un anti-pattern poiché associa i tipi di entità direttamente a EF Core.</span><span class="sxs-lookup"><span data-stu-id="93722-155">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="93722-156">Valutare attentamente tutte le opzioni prima di utilizzare l'inserimento del servizio analogo al seguente.</span><span class="sxs-lookup"><span data-stu-id="93722-156">Carefully consider all options before using service injection like this.</span></span>
