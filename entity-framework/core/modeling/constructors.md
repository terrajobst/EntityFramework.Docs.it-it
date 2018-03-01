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
# <a name="entity-types-with-constructors"></a>Tipi di entità con costruttori

> [!NOTE]  
> Questa funzionalità è nuova in Entity Framework Core 2.1.

A partire da Entity Framework Core 2.1, è ora possibile definire un costruttore con parametri e avere EF Core di chiamare questo costruttore quando si crea un'istanza dell'entità. I parametri del costruttore possono essere associati a proprietà mappate o a vari tipi di servizi per semplificare i comportamenti, ad esempio il caricamento lazy.

> [!NOTE]  
> A partire da Entity Framework Core 2.1, tutti i binding di costruttore è per convenzione. Configurazione dei costruttori specifici da utilizzare è pianificato per una versione futura.

## <a name="binding-to-mapped-properties"></a>Binding a proprietà mappata

Si consideri un tipico modello/Post di Blog:

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

Quando EF Core crea istanze di questi tipi, ad esempio per i risultati di una query, verrà prima di chiamare il costruttore senza parametri predefinito e impostare ogni proprietà il valore dal database. Tuttavia, se EF Core consente di trovare un costruttore con parametri con i nomi dei parametri e tipi che corrispondono a quelle di mapping delle proprietà, quindi chiamerà invece il costruttore con parametri con valori di tali proprietà e non verrà impostata ogni proprietà in modo esplicito. Ad esempio:

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
Alcuni aspetti da notare:
* Non tutte le proprietà di cui è necessario disporre di parametri del costruttore. Ad esempio, la proprietà Post.Content non è impostata da un parametro di costruttore, in modo EF Core verrà impostato dopo la chiamata al costruttore in modo normale.
* Tipi di parametro e i nomi devono corrispondere i tipi di proprietà e nomi, ad eccezione del fatto che le proprietà possono essere base alla convezione Pascal mentre i parametri sono maiuscole/minuscole camel.
* Core EF non è possibile impostare le proprietà di navigazione (ad esempio Blog o post precedente) utilizzando un costruttore.
* Il costruttore può essere pubblico, privato, o qualsiasi altro accessibilità.

### <a name="read-only-properties"></a>Proprietà di sola lettura

Una volta che vengono impostate tramite il costruttore può senso per alcuni di essi rendere di sola lettura. È supportata da Entity Framework Core, ma ci sono alcuni aspetti da sapere:
* Per convenzione, le proprietà senza getter non sono mappate. (In questo modo tende a eseguire il mapping di proprietà che non devono essere associate, ad esempio le proprietà calcolate).
* Utilizzo di valori di chiave generati automaticamente richiede una proprietà chiave è di lettura / scrittura, poiché il valore della chiave deve essere impostata per il generatore di chiavi durante l'inserimento di nuove entità.

Un modo pratico per evitare queste operazioni consiste nell'utilizzare i metodi di impostazione private. Ad esempio:
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
Core EF considera una proprietà con un setter privata di lettura / scrittura, il che significa che tutte le proprietà vengono mappate come prima e la chiave può ancora essere generato dall'archivio.

Un'alternativa all'utilizzo di metodi di impostazione private consiste nel rendere proprietà effettivamente di sola lettura e aggiungere mapping di più esplicito in OnModelCreating. Analogamente, alcune proprietà è possibile rimuovere completamente e sostituite con solo i campi. Si consideri, ad esempio, questi tipi di entità:

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
E questa configurazione in OnModelCreating:
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
Aspetti da notare:
* La chiave "property" è un campo. Non è un `readonly` in modo che le chiavi generate dall'archivio possono essere utilizzate. 
* Le altre proprietà sono proprietà di sola lettura impostata solo nel costruttore.
* Se il valore della chiave primario è sempre impostato da Entity Framework o leggere dal database, quindi è necessario includerlo nel costruttore. Ciò lascia la chiave "property" come campo semplice che indica chiaramente che non deve essere impostato in modo esplicito durante la creazione di nuovi blog o post.

> [!NOTE]  
> Questo codice comporterà '169' che indica che il campo non viene mai utilizzato di avviso del compilatore. Può essere ignorato perché in realtà EF Core utilizza il campo in modo extralinguistic.

## <a name="injecting-services"></a>Inserimento di servizi

EF Core può inoltre inserire i "servizi" nel costruttore del tipo di entità. Di seguito, ad esempio, può essere inserite:
* `DbContext` -istanza del contesto corrente, che può anche essere tipizzata come il tipo DbContext derivato
* `ILazyLoader` -il servizio di caricamento lazy, vedere il [caricamento lazy documentazione](../querying/related-data.md) per ulteriori informazioni
* `Action<object, string>` -un delegato di caricamento lazy, vedere il [caricamento lazy documentazione](../querying/related-data.md) per ulteriori informazioni
* `IEntityType` -i metadati di base di EF associato a questo tipo di entità

> [!NOTE]  
> A partire da Entity Framework Core 2.1, possono essere inseriti solo i servizi noti EF Core. Supporto per i servizi delle applicazioni di inserimento viene considerato per una versione futura.

Ad esempio, un DbContext inserito consente di accedere in modo selettivo il database per ottenere informazioni sulle entità correlate senza caricare tutti. Nell'esempio riportato di seguito in questo viene utilizzato per ottenere il numero di post di blog senza caricare i post:

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
Alcuni aspetti da notare in questo:
* Il costruttore è privato, in quanto è sufficiente chiamare mai EF core e non è presente un altro costruttore pubblico per uso generale.
* Il codice che usa il servizio inserito (ad esempio il contesto) è difensivo su di esso viene `null` per gestire i casi in cui Core EF non crea l'istanza.
* Poiché il servizio viene archiviato in una proprietà di lettura/scrittura, verrà reimpostato quando l'entità è associata a una nuova istanza di contesto.

> [!WARNING]  
> Inserendo l'elemento DbContext simile al seguente è spesso considerato un anti-pattern poiché associa i tipi di entità direttamente a EF Core. Valutare attentamente tutte le opzioni prima di utilizzare l'inserimento del servizio analogo al seguente.
