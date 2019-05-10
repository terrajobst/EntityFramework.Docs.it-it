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
# <a name="entity-types-with-constructors"></a>Tipi di entità con costruttori

> [!NOTE]  
> Questa funzionalità è stata introdotta in EF Core 2.1.

A partire da EF Core 2.1, è ora possibile definire un costruttore con parametri che EF Core chiamare questo costruttore quando si crea un'istanza dell'entità. I parametri del costruttore possono essere associati a proprietà mappate o a vari tipi di servizi per semplificare i comportamenti, ad esempio il caricamento lazy.

> [!NOTE]  
> A partire da EF Core 2.1, tutti i binding di costruttore è per convenzione. Configurazione di costruttori specifici da usare è pianificata per una versione futura.

## <a name="binding-to-mapped-properties"></a>Associazione alle proprietà mappate

Si consideri un tipico modello/Post di Blog:

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

Quando Entity Framework Core crea istanze di questi tipi, ad esempio per i risultati di una query, verrà prima chiamare il costruttore senza parametri predefinito e quindi impostare ciascuna proprietà sul valore dal database. Tuttavia, se EF Core consente di trovare un costruttore con parametri con i nomi dei parametri e i tipi che corrispondono a quelle di eseguito il mapping delle proprietà, quindi chiamerà invece il costruttore con parametri con valori di tali proprietà e non imposterà ogni proprietà in modo esplicito. Ad esempio:

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
Alcuni aspetti da considerare:
* Non tutte le proprietà devono avere i parametri del costruttore. Ad esempio, la proprietà Post.Content non è impostata per qualsiasi parametro di costruttore, in modo che Entity Framework Core verrà impostato dopo la chiamata al costruttore in modo normale.
* I tipi di parametro e nomi devono corrispondere i tipi di proprietà e i nomi, ad eccezione del fatto che le proprietà possono essere convenzione Pascal, mentre i parametri sono maiuscole/minuscole camel.
* EF Core non è possibile impostare le proprietà di navigazione (ad esempio Blog o post precedente) usando un costruttore.
* Il costruttore può essere public, private, o avere eventuali altri accessibilità. Tuttavia, i proxy di caricamento lazy richiedono che il costruttore è accessibile dalla classe proxy che eredita. In genere, ciò significa rendendo pubblici o protetti.

### <a name="read-only-properties"></a>Proprietà di sola lettura

Una volta che vengono impostate tramite il costruttore è più sensato per rendere alcuni utenti di sola lettura. EF Core supporta, ma esistono alcuni aspetti da sapere:
* Per convenzione, le proprietà senza Setter non sono mappate. (In questo modo tende a eseguire il mapping di proprietà che non devono essere associate, ad esempio le proprietà calcolate).
* Usando i valori di chiave generati automaticamente richiede una proprietà chiave che è di lettura-scrittura, poiché il valore della chiave deve essere impostato per il generatore di chiavi durante l'inserimento di nuove entità.

Un modo semplice per evitare queste operazioni è usare Setter privati. Ad esempio:
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
EF Core considera una proprietà con setter privati di lettura / scrittura, il che significa che tutte le proprietà vengono mappate come indicato in precedenza e la chiave può ancora essere generate dall'archivio.

Un'alternativa all'uso di Setter privati consiste nel rendere proprietà realmente di sola lettura e aggiungere mapping di più esplicito in OnModelCreating. In modo analogo, alcune proprietà possono essere rimosso completamente e sostituite con solo campi. Ad esempio, prendere in considerazione questi tipi di entità:

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
E questa configurazione in OnModelCreating:
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
Note importanti:
* La chiave "property" è ora un campo. Non è un `readonly` campo in modo che possono essere usate chiavi generate dall'archivio.
* Le altre proprietà sono proprietà di sola lettura impostata solo nel costruttore.
* Se il valore della chiave primaria è sempre impostato da Entity Framework o leggere dal database, quindi non è necessario includerla nel costruttore. Questo consente di lasciare la chiave "property" un campo semplice e in modo chiaro che dovrebbe non essere impostato in modo esplicito durante la creazione di nuovi blog o post.

> [!NOTE]  
> Questo codice verrà generato un avviso che indica '169' che non viene mai usato il campo. Ciò può essere ignorato poiché in realtà EF Core Usa il campo in modo extralinguistic.

## <a name="injecting-services"></a>Inserimento di servizi

EF Core può anche inserire "servizi" nel costruttore del tipo di entità. Di seguito, ad esempio, può essere inserito:
* `DbContext` -l'istanza del contesto corrente, che può anche essere digitato come tipo DbContext derivato
* `ILazyLoader` -il servizio di caricamento lazy, vedere la [documentazione di caricamento lazy](../querying/related-data.md) per altri dettagli
* `Action<object, string>` -un delegato di caricamento lazy, vedere la [documentazione di caricamento lazy](../querying/related-data.md) per altri dettagli
* `IEntityType` -i metadati di Entity Framework Core associati a questo tipo di entità

> [!NOTE]  
> A partire da EF Core 2.1, solo i servizi noti da EF Core possono essere inseriti. Supporto per l'inserimento di servizi delle applicazioni verrà preso in considerazione per una versione futura.

Ad esempio, un oggetto DbContext inserito è utilizzabile per accedere in modo selettivo del database per ottenere informazioni sulle entità correlate senza il caricamento di tutti. Nell'esempio seguente questo viene usato per ottenere il numero di post di blog senza caricare i post di:

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
Alcuni aspetti da notare in questo:
* Il costruttore è privato, poiché viene sempre chiamato da EF Core e non c'è un altro costruttore pubblico per uso generale.
* Il codice che usa il servizio inserito (vale a dire, il contesto) è difesa contro questa operazione in corso `null` per gestire i casi in cui Entity Framework Core non è la creazione dell'istanza.
* Poiché service viene archiviato in una proprietà di lettura/scrittura verranno ripristinate quando l'entità è associata a una nuova istanza del contesto.

> [!WARNING]  
> Inserimento di DbContext simile al seguente è spesso considerato un antipattern poiché associa i tipi di entità direttamente a EF Core. Valutare con attenzione tutte le opzioni prima di usare l'inserimento del servizio simile al seguente.
