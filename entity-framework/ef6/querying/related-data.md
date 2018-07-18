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
# <a name="loading-related-entities"></a>Il caricamento di entità correlate
Entity Framework supporta tre modi per caricare i dati correlati - caricamento eager, il caricamento lazy e il caricamento esplicito. Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.  

## <a name="eagerly-loading"></a>Il caricamento in blocco  

Il caricamento eager è il processo in base al quale una query per un tipo di entità carica anche entità correlate come parte della query. Il caricamento eager viene ottenuto mediante l'utilizzo del metodo Include. Ad esempio, le query seguenti caricherà i blog e tutti i post correlati per ciascun blog.  

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

Si noti che Include un metodo di estensione nello spazio dei nomi di Data. Entity pertanto assicurarsi che si utilizza tale spazio dei nomi.  

### <a name="eagerly-loading-multiple-levels"></a>Accuratamente il caricamento di più livelli  

È anche possibile caricare rapidamente più livelli di entità correlate. Le query seguenti mostrano esempi di come eseguire questa operazione per le proprietà di navigazione di riferimento sia alle raccolte.  

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

Si noti che non è attualmente possibile filtrare quali entità correlate vengono caricate. Includere will inserire sempre in tutte le relative entità.  

## <a name="lazy-loading"></a>Caricamento lazy  

Il caricamento lazy è il processo in base al quale un'entità o una raccolta di entità viene caricata automaticamente dal database la prima volta che si accede a una proprietà che fanno riferimento alle entità/entità. Quando si usano i tipi di entità POCO, il caricamento lazy viene realizzato mediante la creazione di istanze di tipi proxy derivato e quindi si esegue l'override proprietà virtuali per aggiungere l'hook del caricamento. Ad esempio, quando si usa la classe di entità Blog definita di seguito, i post correlati verranno caricati la prima volta che si accede alla proprietà di navigazione post:  

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

### <a name="turn-lazy-loading-off-for-serialization"></a>Disattivare caricamento lazy off per la serializzazione  

Serializzazione e il caricamento lazy non combinare correttamente e se non si presta attenzione possono finire l'esecuzione di query per l'intero database solo perché il caricamento lazy è abilitato. La maggior parte dei serializzatori funzionano l'accesso a ogni proprietà in un'istanza di un tipo. Accesso a proprietà attiva il caricamento lazy, in modo più entità vengono serializzate. Su tali entità sono accessibili e vengono caricate anche altre entità. È buona norma attivare lazy caricamento disattivato prima di serializzazione di un'entità. Le sezioni seguenti illustrano come eseguire questa operazione.  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a>La disattivazione di caricamento lazy per le proprietà di navigazione specifici  

Il caricamento lazy di raccolta post può essere disattivato impostando la proprietà post non virtuale:  

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

Il caricamento dei post di raccolta è possibile ancora usando il caricamento eager (vedere *accuratamente il caricamento* sopra) o il metodo Load (vedere *caricamento esplicito* sotto).  

### <a name="turn-off-lazy-loading-for-all-entities"></a>Disattivare il caricamento lazy per tutte le entità  

Il caricamento lazy può essere disattivato per tutte le entità nel contesto impostando un flag della proprietà di configurazione. Ad esempio:  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

Caricamento di entità correlate è possibile ancora usando il caricamento eager (vedere *accuratamente il caricamento* sopra) o il metodo Load (vedere *caricamento esplicito* sotto).  

## <a name="explicitly-loading"></a>Caricamento esplicito  

Anche con il caricamento lazy disabilitato è comunque possibile in modo differito caricare entità correlate, ma deve essere eseguita con una chiamata esplicita. Per eseguire questa operazione è usare il metodo Load sulla voce dell'entità correlata. Ad esempio:  

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

Si noti che il metodo di riferimento deve essere utilizzato quando un'entità dispone di una proprietà di navigazione a un'altra entità singola. D'altra parte, il metodo di raccolta deve essere utilizzato quando un'entità dispone di una proprietà di navigazione a una raccolta di altre entità.  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a>Applicare filtri quando si caricano in modo esplicito le entità correlate  

Il metodo di Query fornisce l'accesso per la query sottostante che useranno Entity Framework durante il caricamento di entità correlate. È quindi possibile usare LINQ per applicare filtri alla query prima dell'esecuzione con una chiamata a un metodo di estensione LINQ, ad esempio ToList, carico e così via. Il metodo di Query può essere utilizzato con le proprietà di navigazione di riferimento sia insieme, ma è particolarmente utile per le raccolte in cui può essere utilizzato per caricare solo parte della raccolta. Ad esempio:  

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

Quando si usa il metodo di Query è in genere è consigliabile disattivare il caricamento lazy per la proprietà di navigazione. Questo avviene perché in caso contrario, l'intera raccolta potrebbe automaticamente vengono caricata dal meccanismo di caricamento lazy prima o dopo che è stata eseguita la query filtrata.  

Si noti che mentre la relazione può essere specificata come stringa anziché un'espressione lambda, all'oggetto IQueryable restituita non generica quando viene usata una stringa e, pertanto il metodo di Cast è in genere necessaria prima di qualcosa di utile poter eseguire con esso.  

## <a name="using-query-to-count-related-entities-without-loading-them"></a>Uso di Query per contare le entità correlate senza il caricamento  

In alcuni casi è utile conoscere il numero di entità è correlato a un'altra entità nel database senza effettivamente incorrere nel costo di caricamento di tutte le entità. Il metodo di Query con il metodo di conteggio di LINQ è utilizzabile a tale scopo. Ad esempio:  

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
