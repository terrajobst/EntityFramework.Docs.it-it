---
title: Caricamento di dati correlati - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: 05833055f4744940364da4fdea7ded9a90d67508
ms.sourcegitcommit: a3aec015e0ad7ee31e0f75f00bbf2d286a3ac5c1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/18/2018
ms.locfileid: "42447711"
---
# <a name="loading-related-data"></a>Caricamento di dati correlati

Entity Framework Core consente di usare le proprietà di navigazione nel modello per caricare entità correlate. Esistono tre modelli di O/RM (Object-Relational Mapping) comuni usati per caricare i dati correlati.
* **Caricamento eager** significa che i dati correlati vengono caricati dal database come parte della query iniziale.
* **Caricamento esplicito** significa che i dati correlati vengono caricati in modo esplicito dal database in un secondo momento.
* **Caricamento lazy** significa che i dati correlati vengono caricati in modo trasparente dal database quando si accede alla proprietà di navigazione.

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) di questo articolo in GitHub.

## <a name="eager-loading"></a>Caricamento eager

È possibile usare il metodo `Include` per specificare i dati correlati da includere nei risultati della query. Nell'esempio seguente la proprietà `Posts` dei blog restituiti nei risultati verrà popolata con i post correlati.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core correggerà automaticamente le proprietà di navigazione per qualsiasi altra entità caricata in precedenza nell'istanza di contesto. Anche se i dati per una proprietà di navigazione non vengono inclusi in modo esplicito, la proprietà può comunque essere popolata se alcune o tutte le entità correlate sono state caricate in precedenza.


È possibile includere dati correlati da più relazioni in una singola query.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Inclusione di più livelli

È possibile eseguire il drill-down delle relazioni per includere più livelli di dati correlati tramite il metodo `ThenInclude`. L'esempio seguente carica tutti i blog, i post correlati e l'autore di ogni post.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> Le versioni correnti di Visual Studio offrono opzioni di completamento del codice non corrette, a causa delle quali espressioni corrette possono essere contrassegnate con errori di sintassi quando si usa il metodo `ThenInclude` dopo una proprietà di navigazione della raccolta. Si tratta di un sintomo di un bug di IntelliSense registrato in https://github.com/dotnet/roslyn/issues/8237. È possibile ignorare questi errori di sintassi spuri, purché il codice sia corretto e possa essere compilato correttamente. 

È possibile concatenare più chiamate a `ThenInclude` per continuare a includere ulteriori livelli di dati correlati.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

È possibile combinare tutto ciò per includere dati correlati da più livelli e più nodi radice nella stessa query.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

È possibile che si vogliano includere più entità correlate per una delle entità incluse. Ad esempio, quando si eseguono query per `Blog`, è necessario includere `Posts` e poi si può anche decidere di includere `Author` e `Tags` per `Posts`. A tale scopo, occorre specificare ogni percorso di inclusione iniziando dalla radice. Ad esempio, `Blog -> Posts -> Author` e `Blog -> Posts -> Tags`. Questo non significa che si otterranno join ridondanti. Nella maggior parte dei casi EF consoliderà i join durante la generazione di SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a>Inclusione per i tipi derivati

È possibile includere dati correlati dalle navigazioni definite solo per un tipo derivato mediante `Include` e `ThenInclude`. 

Dato il modello seguente:

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

Il contenuto della navigazione `School` di tutte le entità People che sono Student può essere caricato in modalità eager usando vari modelli:

- Cast
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- Operatore `as`
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- Overload di `Include` che accetta parametri di tipo `string`
  ```csharp
  context.People.Include("Student").ToList()
  ```

### <a name="ignored-includes"></a>Inclusioni ignorate

Se si modifica la query in modo che non restituisca più istanze del tipo di entità con cui è iniziata la query, gli operatori di inclusione vengono ignorati.

Nell'esempio seguente gli operatori di inclusione sono basati su `Blog`, tuttavia, l'operatore `Select` viene usato per modificare la query per restituire un tipo anonimo. In questo caso, gli operatori di inclusione non hanno effetto.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

Per impostazione predefinita, EF Core registrerà un avviso quando gli operatori di inclusione vengono ignorati. Vedere [Registrazione](../miscellaneous/logging.md) per altre informazioni sulla visualizzazione dell'output di registrazione. È possibile modificare il comportamento quando un operatore di inclusione viene ignorato per generare un'eccezione o non eseguire alcuna operazione. Questa operazione viene eseguita quando si configurano le opzioni per il contesto, in genere in `DbContext.OnConfiguring` oppure in `Startup.cs` se si usa ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>Caricamento esplicito

> [!NOTE]  
> Questa funzionalità è stata introdotta in EF Core 1.1.

È possibile caricare in modo esplicito una proprietà di navigazione tramite l'API `DbContext.Entry(...)`.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

È anche possibile caricare in modo esplicito una proprietà di navigazione eseguendo una query separata che restituisce le entità correlate. Se è abilitato il rilevamento delle modifiche, durante il caricamento di un'entità EF Core imposterà automaticamente le proprietà di navigazione dell'entità appena caricata in modo da fare riferimento alle entità già caricate e imposterà le proprietà di navigazione delle entità già caricate in modo da fare riferimento all'entità appena caricata.

### <a name="querying-related-entities"></a>Esecuzione di query su entità correlate

È anche possibile ottenere una query LINQ che rappresenta il contenuto di una proprietà di navigazione.

Ciò consente di eseguire operazioni quali l'esecuzione di un operatore di aggregazione sulle entità correlate senza caricarle in memoria.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

È anche possibile filtrare le entità correlate che vengono caricate in memoria.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Caricamento lazy

> [!NOTE]  
> Questa funzionalità è stata introdotta in EF Core 2.1.

Il modo più semplice per usare il caricamento lazy consiste nell'installare il pacchetto [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) e abilitarlo con una chiamata a `UseLazyLoadingProxies`. Ad esempio:
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
O quando si usa AddDbContext:
```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```
EF Core abiliterà quindi il caricamento lazy per qualsiasi proprietà di navigazione che può essere sottoposta a override, ovvero deve essere `virtual` e in una classe ereditabile. Ad esempio, nelle entità seguenti, le proprietà di navigazione `Post.Blog` e `Blog.Posts` vengono caricate in modalità lazy.
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
### <a name="lazy-loading-without-proxies"></a>Caricamento lazy senza proxy

I proxy di caricamento lazy operano inserendo il servizio `ILazyLoader` in un'entità, come descritto in [Costruttori di tipi di entità](../modeling/constructors.md). Ad esempio:
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
In questo caso non è richiesto che i tipi di entità vengano ereditati o che le proprietà di navigazione siano virtuali e le istanze di entità possono essere create con `new` per eseguire il caricamento lazy dopo il collegamento a un contesto. Tuttavia, è necessario un riferimento al servizio `ILazyLoader`, che viene definito nel pacchetto [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/). Questo pacchetto contiene un set minimo di tipi in modo che vi sia un impatto minimo per le dipendenze. Tuttavia, per evitare completamente di dipendere da pacchetti di EF Core nei tipi di entità, è possibile inserire il metodo `ILazyLoader.Load` come delegato. Ad esempio:
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
Il codice precedente usa un metodo di estensione `Load` per chiarire l'uso del delegato:
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
> Il parametro del costruttore per il delegato di caricamento lazy deve essere chiamato "lazyLoader". La possibilità di configurare l'uso di un nome diverso è pianificata per una versione futura.

## <a name="related-data-and-serialization"></a>Dati correlati e serializzazione

Dato che EF Core correggerà automaticamente le proprietà di navigazione, è possibile che il grafo degli oggetti contenga cicli. Ad esempio, il caricamento di un blog e dei post correlati risulterà in un oggetto blog che fa riferimento a una raccolta di post. Ognuno di tali post avrà un riferimento al blog.

Alcuni framework di serializzazione non consentono tali cicli. Ad esempio, Json.NET genererà l'eccezione seguente se viene rilevato un ciclo.

> Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'. (Newtonsoft.Json.JsonSerializationException: ciclo autoreferenziale rilevato per la proprietà 'Blog' con tipo 'MyApplication.Models.Blog')

Se si usa ASP.NET Core, è possibile configurare Json.NET per ignorare i cicli trovati nel grafo degli oggetti. Questa operazione viene eseguita nel metodo `ConfigureServices(...)` in `Startup.cs`.

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
