---
title: Configurazione di un elemento DbContext - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 6980acd53b0a74055af7a1e04b476f4625c327c9
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2018
---
# <a name="configuring-a-dbcontext"></a>Configurazione di un elemento DbContext

Questo articolo illustra i modelli di base per la configurazione di un `DbContext` tramite un `DbContextOptions` per connettersi a un database tramite un provider di Entity Framework Core specifico e comportamenti facoltativi.

## <a name="design-time-dbcontext-configuration"></a>Configurazione DbContext Design-time

Strumenti di Entity Framework Core in fase di progettazione, ad esempio [migrazioni](xref:core/managing-schemas/migrations/index) devono essere in grado di individuare e creare un'istanza funzionante di un `DbContext` tipo per raccogliere informazioni dettagliate sui tipi di entità e modalità di mapping a uno schema di database dell'applicazione. Questo processo può essere automatica, purché lo strumento è possibile creare facilmente il `DbContext` in modo che verrà configurato in modo analogo al modo in cui potrebbe essere configurato in fase di esecuzione.

Mentre qualsiasi criterio di ricerca che fornisce informazioni di configurazione necessarie per il `DbContext` può funzionare in fase di esecuzione, gli strumenti che richiedono l'utilizzo di un `DbContext` in fase di progettazione può funzionare solo con un numero limitato di modelli. Questi prerequisiti sono analizzati più dettagliatamente il [creare il contesto in fase di progettazione](xref:core/miscellaneous/cli/dbcontext-creation) sezione.

## <a name="configuring-dbcontextoptions"></a>Configurazione DbContextOptions

`DbContext` deve avere un'istanza di `DbContextOptions` per eseguire operazioni. Il `DbContextOptions` istanza contiene informazioni di configurazione, ad esempio:

- Il provider di database da utilizzare, in genere selezionata richiamando un metodo, ad esempio `UseSqlServer` o `UseSqlite`
- Qualsiasi stringa di connessione necessarie o l'identificatore dell'istanza del database, in genere passato come argomento al metodo di selezione del provider indicato in precedenza
- Tutti i selettori di provider a livello di comportamento facoltativo, in genere anche concatenati all'interno della chiamata al metodo di selezione del provider
- Tutti i selettori generale comportamento di Entity Framework Core, concatenati in genere dopo o prima del metodo di selezione del provider

Nell'esempio seguente viene configurata la `DbContextOptions` per utilizzare il provider SQL Server, una connessione è inclusa nel `connectionString` variabile, un timeout del comando a livello di provider e un selettore di comportamento EF Core eseguite tutte le query eseguite nel `DbContext` [no rilevamento](xref:core/querying/tracking#no-tracking-queries) per impostazione predefinita:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Metodi di selezione del provider e altri metodi di selettore di comportamento indicati in precedenza sono metodi di estensione in `DbContextOptions` o classi di opzioni specifiche del provider. Per accedere a questi metodi di estensione, è necessario disporre di uno spazio dei nomi (in genere `Microsoft.EntityFrameworkCore`) nell'ambito e includere le dipendenze aggiuntive del pacchetto nel progetto.

Il `DbContextOptions` possono essere forniti al `DbContext` eseguendo l'override di `OnConfiguring` metodo o esternamente tramite un argomento del costruttore.

Se si utilizzano entrambi, `OnConfiguring` viene applicata per ultima e possono sovrascrivere le opzioni fornite per l'argomento del costruttore.

### <a name="constructor-argument"></a>Argomento del costruttore

Codice di contesto con un costruttore:

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]  
> Il costruttore di base di DbContext accetta inoltre la versione non generica di `DbContextOptions`, ma utilizza la versione non generica non è consigliata per le applicazioni con più tipi di contesto.

Codice dell'applicazione per inizializzare dall'argomento del costruttore:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

Codice di contesto con `OnConfiguring`:

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

Il codice dell'applicazione per inizializzare un `DbContext` che utilizza `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Questo approccio non si presta a test, a meno che il test dell'intero database di destinazione.

### <a name="using-dbcontext-with-dependency-injection"></a>Con l'inserimento di dipendenze DbContext

Supporta l'utilizzo di Entity Framework Core `DbContext` con un contenitore dell'inserimento di dipendenza. Il tipo DbContext può essere aggiunti al contenitore del servizio con il `AddDbContext<TContext>` metodo.

`AddDbContext<TContext>` renderà sia il tipo DbContext, `TContext`e i corrispondenti `DbContextOptions<TContext>` disponibili per l'aggiunta dal contenitore del servizio.

Vedere [lettura più](#more-reading) seguito per ulteriori informazioni sull'inserimento di dipendenze.

Aggiunta di `Dbcontext` per l'inserimento di dipendenze:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Questo richiede l'aggiunta di un [argomento del costruttore](#constructor-argument) al tipo DbContext che accetta `DbContextOptions<TContext>`.

Codice di contesto:

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

Codice dell'applicazione (in ASP.NET Core):

``` csharp
public class MyController
{
    private readonly BloggingContext _context;

    public MyController(BloggingContext context)
    {
      _context = context;
    }

    ...
}
```

Codice dell'applicazione (tramite ServiceProvider direttamente, meno comune):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a>Lettura di più

* Lettura [introduzione su ASP.NET Core](../get-started/aspnetcore/index.md) per ulteriori informazioni sull'utilizzo di Entity Framework con ASP.NET Core.
* Lettura [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) per ulteriori informazioni sull'utilizzo DI.
* Lettura [Testing](testing/index.md) per ulteriori informazioni.
