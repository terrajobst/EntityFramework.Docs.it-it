---
title: Tipi di entità con costruttori - EF Core
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: 5bf49718f02c1860871b1f4c255ec4d98fce2fc7
ms.sourcegitcommit: 960e42a01b3a2f76da82e074f64f52252a8afecc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2019
ms.locfileid: "65405252"
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="0fed0-102">Tipi di entità con costruttori</span><span class="sxs-lookup"><span data-stu-id="0fed0-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="0fed0-103">Questa funzionalità è stata introdotta in EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="0fed0-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="0fed0-104">A partire da EF Core 2.1, è ora possibile definire un costruttore con parametri che EF Core chiamare questo costruttore quando si crea un'istanza dell'entità.</span><span class="sxs-lookup"><span data-stu-id="0fed0-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="0fed0-105">I parametri del costruttore possono essere associati a proprietà mappate o a vari tipi di servizi per semplificare i comportamenti, ad esempio il caricamento lazy.</span><span class="sxs-lookup"><span data-stu-id="0fed0-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="0fed0-106">A partire da EF Core 2.1, tutti i binding di costruttore è per convenzione.</span><span class="sxs-lookup"><span data-stu-id="0fed0-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="0fed0-107">Configurazione di costruttori specifici da usare è pianificata per una versione futura.</span><span class="sxs-lookup"><span data-stu-id="0fed0-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="0fed0-108">Associazione alle proprietà mappate</span><span class="sxs-lookup"><span data-stu-id="0fed0-108">Binding to mapped properties</span></span>

<span data-ttu-id="0fed0-109">Si consideri un tipico modello/Post di Blog:</span><span class="sxs-lookup"><span data-stu-id="0fed0-109">Consider a typical Blog/Post model:</span></span>

``` csharp
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

<span data-ttu-id="0fed0-110">Quando Entity Framework Core crea istanze di questi tipi, ad esempio per i risultati di una query, verrà prima chiamare il costruttore senza parametri predefinito e quindi impostare ciascuna proprietà sul valore dal database.</span><span class="sxs-lookup"><span data-stu-id="0fed0-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="0fed0-111">Tuttavia, se EF Core consente di trovare un costruttore con parametri con i nomi dei parametri e i tipi che corrispondono a quelle di eseguito il mapping delle proprietà, quindi chiamerà invece il costruttore con parametri con valori di tali proprietà e non imposterà ogni proprietà in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="0fed0-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="0fed0-112">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="0fed0-112">For example:</span></span>

``` csharp
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
<span data-ttu-id="0fed0-113">Alcuni aspetti da considerare:</span><span class="sxs-lookup"><span data-stu-id="0fed0-113">Some things to note:</span></span>
* <span data-ttu-id="0fed0-114">Non tutte le proprietà devono avere i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="0fed0-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="0fed0-115">Ad esempio, la proprietà Post.Content non è impostata per qualsiasi parametro di costruttore, in modo che Entity Framework Core verrà impostato dopo la chiamata al costruttore in modo normale.</span><span class="sxs-lookup"><span data-stu-id="0fed0-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="0fed0-116">I tipi di parametro e nomi devono corrispondere i tipi di proprietà e i nomi, ad eccezione del fatto che le proprietà possono essere convenzione Pascal, mentre i parametri sono maiuscole/minuscole camel.</span><span class="sxs-lookup"><span data-stu-id="0fed0-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="0fed0-117">EF Core non è possibile impostare le proprietà di navigazione (ad esempio Blog o post precedente) usando un costruttore.</span><span class="sxs-lookup"><span data-stu-id="0fed0-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="0fed0-118">Il costruttore può essere public, private, o avere eventuali altri accessibilità.</span><span class="sxs-lookup"><span data-stu-id="0fed0-118">The constructor can be public, private, or have any other accessibility.</span></span> <span data-ttu-id="0fed0-119">Tuttavia, i proxy di caricamento lazy richiedono che il costruttore è accessibile dalla classe proxy che eredita.</span><span class="sxs-lookup"><span data-stu-id="0fed0-119">However, lazy-loading proxies require that the constructor is accessible from the inheriting proxy class.</span></span> <span data-ttu-id="0fed0-120">In genere, ciò significa rendendo pubblici o protetti.</span><span class="sxs-lookup"><span data-stu-id="0fed0-120">Usually this means making it either public or protected.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="0fed0-121">Proprietà di sola lettura</span><span class="sxs-lookup"><span data-stu-id="0fed0-121">Read-only properties</span></span>

<span data-ttu-id="0fed0-122">Una volta che vengono impostate tramite il costruttore è più sensato per rendere alcuni utenti di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="0fed0-122">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="0fed0-123">EF Core supporta, ma esistono alcuni aspetti da sapere:</span><span class="sxs-lookup"><span data-stu-id="0fed0-123">EF Core supports this, but there are some things to look out for:</span></span>
* <span data-ttu-id="0fed0-124">Per convenzione, le proprietà senza Setter non sono mappate.</span><span class="sxs-lookup"><span data-stu-id="0fed0-124">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="0fed0-125">(In questo modo tende a eseguire il mapping di proprietà che non devono essere associate, ad esempio le proprietà calcolate).</span><span class="sxs-lookup"><span data-stu-id="0fed0-125">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="0fed0-126">Usando i valori di chiave generati automaticamente richiede una proprietà chiave che è di lettura-scrittura, poiché il valore della chiave deve essere impostato per il generatore di chiavi durante l'inserimento di nuove entità.</span><span class="sxs-lookup"><span data-stu-id="0fed0-126">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="0fed0-127">Un modo semplice per evitare queste operazioni è usare Setter privati.</span><span class="sxs-lookup"><span data-stu-id="0fed0-127">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="0fed0-128">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="0fed0-128">For example:</span></span>
``` csharp
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
<span data-ttu-id="0fed0-129">EF Core considera una proprietà con setter privati di lettura / scrittura, il che significa che tutte le proprietà vengono mappate come indicato in precedenza e la chiave può ancora essere generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="0fed0-129">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="0fed0-130">Un'alternativa all'uso di Setter privati consiste nel rendere proprietà realmente di sola lettura e aggiungere mapping di più esplicito in OnModelCreating.</span><span class="sxs-lookup"><span data-stu-id="0fed0-130">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="0fed0-131">In modo analogo, alcune proprietà possono essere rimosso completamente e sostituite con solo campi.</span><span class="sxs-lookup"><span data-stu-id="0fed0-131">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="0fed0-132">Ad esempio, prendere in considerazione questi tipi di entità:</span><span class="sxs-lookup"><span data-stu-id="0fed0-132">For example, consider these entity types:</span></span>

``` csharp
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
<span data-ttu-id="0fed0-133">E questa configurazione in OnModelCreating:</span><span class="sxs-lookup"><span data-stu-id="0fed0-133">And this configuration in OnModelCreating:</span></span>
``` csharp
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
<span data-ttu-id="0fed0-134">Note importanti:</span><span class="sxs-lookup"><span data-stu-id="0fed0-134">Things to note:</span></span>
* <span data-ttu-id="0fed0-135">La chiave "property" è ora un campo.</span><span class="sxs-lookup"><span data-stu-id="0fed0-135">The key "property" is now a field.</span></span> <span data-ttu-id="0fed0-136">Non è un `readonly` campo in modo che possono essere usate chiavi generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="0fed0-136">It is not a `readonly` field so that store-generated keys can be used.</span></span>
* <span data-ttu-id="0fed0-137">Le altre proprietà sono proprietà di sola lettura impostata solo nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="0fed0-137">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="0fed0-138">Se il valore della chiave primaria è sempre impostato da Entity Framework o leggere dal database, quindi non è necessario includerla nel costruttore.</span><span class="sxs-lookup"><span data-stu-id="0fed0-138">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="0fed0-139">Questo consente di lasciare la chiave "property" un campo semplice e in modo chiaro che dovrebbe non essere impostato in modo esplicito durante la creazione di nuovi blog o post.</span><span class="sxs-lookup"><span data-stu-id="0fed0-139">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="0fed0-140">Questo codice verrà generato un avviso che indica '169' che non viene mai usato il campo.</span><span class="sxs-lookup"><span data-stu-id="0fed0-140">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="0fed0-141">Ciò può essere ignorato poiché in realtà EF Core Usa il campo in modo extralinguistic.</span><span class="sxs-lookup"><span data-stu-id="0fed0-141">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="0fed0-142">Inserimento di servizi</span><span class="sxs-lookup"><span data-stu-id="0fed0-142">Injecting services</span></span>

<span data-ttu-id="0fed0-143">EF Core può anche inserire "servizi" nel costruttore del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="0fed0-143">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="0fed0-144">Di seguito, ad esempio, può essere inserito:</span><span class="sxs-lookup"><span data-stu-id="0fed0-144">For example, the following can be injected:</span></span>
* <span data-ttu-id="0fed0-145">`DbContext` -l'istanza del contesto corrente, che può anche essere digitato come tipo DbContext derivato</span><span class="sxs-lookup"><span data-stu-id="0fed0-145">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="0fed0-146">`ILazyLoader` -il servizio di caricamento lazy, vedere la [documentazione di caricamento lazy](../querying/related-data.md) per altri dettagli</span><span class="sxs-lookup"><span data-stu-id="0fed0-146">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="0fed0-147">`Action<object, string>` -un delegato di caricamento lazy, vedere la [documentazione di caricamento lazy](../querying/related-data.md) per altri dettagli</span><span class="sxs-lookup"><span data-stu-id="0fed0-147">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="0fed0-148">`IEntityType` -i metadati di Entity Framework Core associati a questo tipo di entità</span><span class="sxs-lookup"><span data-stu-id="0fed0-148">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="0fed0-149">A partire da EF Core 2.1, solo i servizi noti da EF Core possono essere inseriti.</span><span class="sxs-lookup"><span data-stu-id="0fed0-149">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="0fed0-150">Supporto per l'inserimento di servizi delle applicazioni verrà preso in considerazione per una versione futura.</span><span class="sxs-lookup"><span data-stu-id="0fed0-150">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="0fed0-151">Ad esempio, un oggetto DbContext inserito è utilizzabile per accedere in modo selettivo del database per ottenere informazioni sulle entità correlate senza il caricamento di tutti.</span><span class="sxs-lookup"><span data-stu-id="0fed0-151">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="0fed0-152">Nell'esempio seguente questo viene usato per ottenere il numero di post di blog senza caricare i post di:</span><span class="sxs-lookup"><span data-stu-id="0fed0-152">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

``` csharp
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
<span data-ttu-id="0fed0-153">Alcuni aspetti da notare in questo:</span><span class="sxs-lookup"><span data-stu-id="0fed0-153">A few things to notice about this:</span></span>
* <span data-ttu-id="0fed0-154">Il costruttore è privato, poiché viene sempre chiamato da EF Core e non c'è un altro costruttore pubblico per uso generale.</span><span class="sxs-lookup"><span data-stu-id="0fed0-154">The constructor is private, since it is only ever called by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="0fed0-155">Il codice che usa il servizio inserito (vale a dire, il contesto) è difesa contro questa operazione in corso `null` per gestire i casi in cui Entity Framework Core non è la creazione dell'istanza.</span><span class="sxs-lookup"><span data-stu-id="0fed0-155">The code using the injected service (that is, the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="0fed0-156">Poiché service viene archiviato in una proprietà di lettura/scrittura verranno ripristinate quando l'entità è associata a una nuova istanza del contesto.</span><span class="sxs-lookup"><span data-stu-id="0fed0-156">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="0fed0-157">Inserimento di DbContext simile al seguente è spesso considerato un antipattern poiché associa i tipi di entità direttamente a EF Core.</span><span class="sxs-lookup"><span data-stu-id="0fed0-157">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="0fed0-158">Valutare con attenzione tutte le opzioni prima di usare l'inserimento del servizio simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="0fed0-158">Carefully consider all options before using service injection like this.</span></span>
