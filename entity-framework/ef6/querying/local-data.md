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
# <a name="local-data"></a>Dati locali
Eseguendo una query LINQ direttamente un elemento DbSet inviano sempre una query al database, ma è possibile accedere ai dati che è attualmente in memoria usando la proprietà DbSet.Local. È anche possibile accedere EF tiene traccia delle informazioni aggiuntive sulle entità usando i metodi DbContext.Entry e DbContext.ChangeTracker.Entries. Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.  

## <a name="using-local-to-look-at-local-data"></a>Utilizzo locale per esaminare i dati locali  

La proprietà locale di DbSet consente un facile accesso alle entità del set che non viene rilevato dal contesto e non sono state contrassegnate come eliminate. L'accesso alla proprietà locale non causerà mai una query da inviare al database. Ciò significa che in genere viene utilizzato dopo che è già stata eseguita una query. Il metodo di estensione Load può essere utilizzato per eseguire una query in modo che il contesto tiene traccia dei risultati. Ad esempio:  

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

Se avessimo due blog nel database: 'ADO.NET Blog' con un BlogId pari a 1 - e "Blog di Visual Studio' con un BlogId pari a 2 è stato possibile prevedere il seguente output:  

```  
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

Ciò illustra tre punti:  

- Il nuovo blog 'My nuovo post di Blog' è incluso nella raccolta locale anche se non è ancora stato salvato nel database. Questo blog contiene una chiave primaria pari a zero perché il database non ha ancora generato un tasto effettivo per l'entità.  
- Blog di ADO.NET' ' non è incluso nella raccolta locale anche se si è ancora rilevato dal contesto. Questo avviene perché è stata rimossa alla classe DbSet, contrassegnandolo come eliminato.  
- Quando DbSet viene usato per eseguire una query il blog contrassegnato per l'eliminazione (Blog di ADO.NET) è incluso nei risultati e il nuovo post di blog (Blog di nuovo personale) non è ancora stato salvato nel database non è incluso nei risultati. Questo avviene perché DbSet sta eseguendo una query sul database e i risultati restituiti sempre riflettano gli elementi nel database.  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a>Utilizzo locale per aggiungere e rimuovere entità dal contesto  

La proprietà locale in DbSet restituisce un [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) con eventi collegati in modo che rimanga sincronizzato con il contenuto del contesto. Ciò significa che le entità possono essere aggiunto o rimosso dalla raccolta locale o alla classe DbSet. Significa anche che le query che offrono nuove entità nel contesto di comporterà la raccolta locale venga aggiornata con tali entità. Ad esempio:  

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

Assumendo che alcuni post con tag 'entity framework' e 'asp.net' l'output simile al seguente:  

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

Ciò illustra tre punti:  

- Il nuovo post 'What' s New in EF' che è stato aggiunto all'oggetto locale raccolta diventa rilevata dal contesto nello stato Added. Da pertanto essere inserita nel database quando viene chiamato SaveChanges.  
- Il post che è stato rimosso dalla raccolta locale (Guida per principianti Entity Framework) è ora contrassegnato come eliminato nel contesto. Si verranno pertanto eliminato dal database quando viene chiamato SaveChanges.  
- Altri post (Guida per principianti ASP.NET) caricato nel contesto di con la seconda query viene aggiunto automaticamente alla raccolta locale.  

Un ultimo aspetto da notare circa locale è che, essendo che una prestazioni ObservableCollection non sono molto utile per un numero elevato di entità. Pertanto se si gestiscono migliaia di entità nel contesto di potrebbe non essere consigliabile usare locale.  

## <a name="using-local-for-wpf-data-binding"></a>Utilizzo locale per l'associazione dati WPF  

La proprietà locale di DbSet può essere utilizzata direttamente per il data binding in un'applicazione WPF perché è un'istanza della classe ObservableCollection. Come descritto nelle sezioni precedenti, che questo significa che verrà automaticamente rimangano sincronizzati con il contenuto del contesto e il contenuto del contesto viene automaticamente sincronizzata con esso. Si noti che è necessario pre-popolare la raccolta locale con i dati affinché sia disponibile alcuna operazione da associare alla poiché locale non causerà mai una query sul database.  

Non è una posizione appropriata per un esempio di associazione dati WPF completo ma gli elementi chiave sono:  

- Installazione di origine del binding  
- Associarlo alla proprietà locale del set di  
- Popolare locale usando una query al database.  

## <a name="wpf-binding-to-navigation-properties"></a>Associazione di WPF per le proprietà di navigazione  

Se si esegue il master/dettaglio tale associazione è possibile associare la visualizzazione dettagli per una proprietà di navigazione di una delle entità. Un modo semplice per eseguire questa operazione è usare una classe ObservableCollection per la proprietà di navigazione. Ad esempio:  

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

## <a name="using-local-to-clean-up-entities-in-savechanges"></a>Utilizzo locale per pulire le entità in SaveChanges  

Nella maggior parte dei casi entità rimosse da una proprietà di navigazione verrà non essere contrassegnata automaticamente come eliminate nel contesto. Ad esempio, se si rimuove un oggetto Post dalla raccolta Blog.Posts quindi che post non essere eliminate automaticamente quando viene chiamato SaveChanges. Se è necessario che venga eliminato potrebbe essere necessario trovare queste entità inesatti e contrassegnarli come eliminato prima di chiamare SaveChanges o come parte di un metodo SaveChanges sottoposto a override. Ad esempio:  

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

Il codice precedente Usa la raccolta locale per trovare tutti i post e segni di quelli che non è un riferimento di blog come eliminato. La chiamata ToList è necessaria perché in caso contrario, verrà modificata la raccolta per il metodo Remove chiamare mentre viene enumerato. Nella maggior parte delle situazioni è possibile eseguire una query direttamente in base alla proprietà locale senza usare ToList prima di tutto.  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a>Utilizzo dell'associazione di dati locali e ToBindingList for Windows Forms  

Windows Form non supporta l'associazione di dati di fedeltà con ObservableCollection direttamente. Tuttavia, è comunque possibile usare la proprietà DbSet locale per il data binding per ottenere tutti i vantaggi descritti nelle sezioni precedenti. Questo risultato viene ottenuto tramite il metodo di estensione ToBindingList che crea un' [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementazione supportato dalla classe ObservableCollection locale.  

Non è una posizione appropriata per un esempio di associazione dati di Windows Form completo ma gli elementi chiave sono:  

- Configurare un'origine di associazione oggetto  
- Associarlo alla proprietà locale del set di uso Local.ToBindingList()  
- Popolare locale usando una query al database  

## <a name="getting-detailed-information-about-tracked-entities"></a>Visualizzare informazioni dettagliate per le entità rilevate  

Molti degli esempi in questa serie di utilizzare il metodo di ingresso per restituire un'istanza di DbEntityEntry per un'entità. Questo oggetto voce funge quindi il punto di partenza per la raccolta di informazioni sull'entità, ad esempio lo stato corrente, nonché per l'esecuzione di operazioni sulle entità, ad esempio in modo esplicito il caricamento di un'entità correlata.  

I metodi di voci restituiscono oggetti DbEntityEntry per molte o tutte le entità rilevate dal contesto. In questo modo è possibile raccogliere informazioni o eseguire operazioni su molte entità anziché solo una singola voce. Ad esempio:  

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

Si noterà che è stato introdotto una classe dell'autore e il lettore nell'esempio, entrambe queste classi implementano l'interfaccia IPerson.  

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

Si supponga di che disporre i dati seguenti nel database:

Blog con BlogId = 1 e nome = 'Blog di ADO.NET'  
Blog con BlogId = 2 e nome = 'Blog di Visual Studio'  
Blog con BlogId = 3 e nome = 'Blog di .NET Framework'  
Autore con AuthorId = 1 e nome = 'Luca Bianchi'  
Lettore con ReaderId = 1 e nome = "John Doe"  

L'output dall'esecuzione del codice sarebbe:  

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

Questi esempi vengono illustrati diversi aspetti:  

- I metodi di voci restituiscono le voci per le entità in tutti gli stati, tra cui Deleted. Eseguire il confronto locale che esclude eliminata le entità.  
- Quando viene utilizzato il metodo di voci non generiche, vengono restituite le voci per tutti i tipi di entità. Quando viene utilizzato il metodo generico voci vengono restituite solo le voci per le entità che sono istanze del tipo generico. Questo è stato usato in precedenza per ottenere le voci per tutti i blog. È stato usato anche per ottenere le voci per tutte le entità che implementano IPerson. Ciò dimostra che il tipo generico non è necessario essere un tipo di entità effettivo.  
- LINQ agli oggetti può essere utilizzato per filtrare i risultati restituiti. Questo è stato usato in precedenza per individuare le entità di qualsiasi tipo fino a quando vengono modificati.  

Si noti che le istanze di DbEntityEntry contengono sempre un'entità non null. Le voci di relazione e quelle di stub non sono rappresentate come istanze di DbEntityEntry in modo che non è necessario per filtrare per questi.
