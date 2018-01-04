---
title: Configurazione di un elemento DbContext - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: de26e3b28851d4dc4e50f0490093dd05ad489b31
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/22/2017
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="0e7d6-102">Configurazione di un elemento DbContext</span><span class="sxs-lookup"><span data-stu-id="0e7d6-102">Configuring a DbContext</span></span>

<span data-ttu-id="0e7d6-103">Questo articolo illustra i modelli di base per la configurazione di un `DbContext` tramite un `DbContextOptions` per connettersi a un database tramite un provider di Entity Framework Core specifico e comportamenti facoltativi.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="0e7d6-104">Configurazione DbContext Design-time</span><span class="sxs-lookup"><span data-stu-id="0e7d6-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="0e7d6-105">Strumenti di Entity Framework Core in fase di progettazione, ad esempio [migrazioni](xref:core/managing-schemas/migrations/index) devono essere in grado di individuare e creare un'istanza funzionante di un `DbContext` tipo per raccogliere informazioni dettagliate sui tipi di entità e modalità di mapping a uno schema di database dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="0e7d6-106">Questo processo può essere automatica, purché lo strumento è possibile creare facilmente il `DbContext` in modo che verrà configurato in modo analogo a come potrebbe essere configurato in fase di run.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at runt-time.</span></span>

<span data-ttu-id="0e7d6-107">Mentre qualsiasi criterio di ricerca che fornisce informazioni di configurazione necessarie per il `DbContext` può funzionare in fase di esecuzione, gli strumenti che richiedono l'utilizzo di un `DbContext` in fase di progettazione può funzionare solo con un numero limitato di modelli.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="0e7d6-108">Questi prerequisiti sono analizzati più dettagliatamente il [creare il contesto in fase di progettazione](xref:core/miscellaneous/cli/dbcontext-creation) sezione.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="0e7d6-109">Configurazione DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="0e7d6-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="0e7d6-110">`DbContext`deve avere un'istanza di `DbContextOptions` per eseguire operazioni.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="0e7d6-111">Il `DbContextOptions` istanza contiene informazioni di configurazione, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="0e7d6-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="0e7d6-112">Il provider di database da utilizzare, in genere selezionata richiamando un metodo, ad esempio `UseSqlServer` o`UseSqlite`</span><span class="sxs-lookup"><span data-stu-id="0e7d6-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`</span></span>
- <span data-ttu-id="0e7d6-113">Qualsiasi stringa di connessione necessarie o l'identificatore dell'istanza del database, in genere passato come argomento al metodo di selezione del provider indicato in precedenza</span><span class="sxs-lookup"><span data-stu-id="0e7d6-113">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="0e7d6-114">Tutti i selettori di provider a livello di comportamento facoltativo, in genere anche concatenati all'interno della chiamata al metodo di selezione del provider</span><span class="sxs-lookup"><span data-stu-id="0e7d6-114">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="0e7d6-115">Tutti i selettori generale comportamento di Entity Framework Core, concatenati in genere dopo o prima del metodo di selezione del provider</span><span class="sxs-lookup"><span data-stu-id="0e7d6-115">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="0e7d6-116">Nell'esempio seguente viene configurata la `DbContextOptions` per utilizzare il provider SQL Server, una connessione è inclusa nel `connectionString` variabile, un timeout del comando a livello di provider e un selettore di comportamento EF Core eseguite tutte le query eseguite nel `DbContext` [no rilevamento](xref:core/querying/tracking#no-tracking-queries) per impostazione predefinita:</span><span class="sxs-lookup"><span data-stu-id="0e7d6-116">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="0e7d6-117">Metodi di selezione del provider e altri metodi di selettore di comportamento indicati in precedenza sono metodi di estensione in `DbContextOptions` o classi di opzioni specifiche del provider.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-117">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="0e7d6-118">Per accedere a questi metodi di estensione, è necessario disporre di uno spazio dei nomi (in genere `Microsoft.EntityFrameworkCore`) nell'ambito e includere le dipendenze aggiuntive del pacchetto nel progetto.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-118">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="0e7d6-119">Il `DbContextOptions` possono essere forniti al `DbContext` eseguendo l'override di `OnConfiguring` metodo o esternamente tramite un argomento del costruttore.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-119">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="0e7d6-120">Se si utilizzano entrambi, `OnConfiguring` viene applicata per ultima e possono sovrascrivere le opzioni fornite per l'argomento del costruttore.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-120">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="0e7d6-121">Argomento del costruttore</span><span class="sxs-lookup"><span data-stu-id="0e7d6-121">Constructor argument</span></span>

<span data-ttu-id="0e7d6-122">Codice di contesto con un costruttore:</span><span class="sxs-lookup"><span data-stu-id="0e7d6-122">Context code with constructor:</span></span>

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
> <span data-ttu-id="0e7d6-123">Il costruttore di base di DbContext accetta inoltre la versione non generica di `DbContextOptions`, ma utilizza la versione non generica non è consigliata per le applicazioni con più tipi di contesto.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-123">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="0e7d6-124">Codice dell'applicazione per inizializzare dall'argomento del costruttore:</span><span class="sxs-lookup"><span data-stu-id="0e7d6-124">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="0e7d6-125">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="0e7d6-125">OnConfiguring</span></span>

<span data-ttu-id="0e7d6-126">Codice di contesto con `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="0e7d6-126">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="0e7d6-127">Il codice dell'applicazione per inizializzare un `DbContext` che utilizza `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="0e7d6-127">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="0e7d6-128">Questo approccio non si presta a test, a meno che il test dell'intero database di destinazione.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-128">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="0e7d6-129">Con l'inserimento di dipendenze DbContext</span><span class="sxs-lookup"><span data-stu-id="0e7d6-129">Using DbContext with dependency injection</span></span>

<span data-ttu-id="0e7d6-130">Supporta l'utilizzo di Entity Framework Core `DbContext` con un contenitore dell'inserimento di dipendenza.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-130">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="0e7d6-131">Il tipo DbContext può essere aggiunti al contenitore del servizio con il `AddDbContext<TContext>` metodo.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-131">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="0e7d6-132">`AddDbContext<TContext>`renderà sia il tipo DbContext, `TContext`e i corrispondenti `DbContextOptions<TContext>` disponibili per l'aggiunta dal contenitore del servizio.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-132">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="0e7d6-133">Vedere [lettura più](#more-reading) seguito per ulteriori informazioni sull'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-133">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="0e7d6-134">Aggiunta di `Dbcontext` per l'inserimento di dipendenze:</span><span class="sxs-lookup"><span data-stu-id="0e7d6-134">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="0e7d6-135">Questo richiede l'aggiunta di un [argomento del costruttore](#constructor-argument) al tipo DbContext che accetta `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-135">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="0e7d6-136">Codice di contesto:</span><span class="sxs-lookup"><span data-stu-id="0e7d6-136">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="0e7d6-137">Codice dell'applicazione (in ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="0e7d6-137">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="0e7d6-138">Codice dell'applicazione (tramite ServiceProvider direttamente, meno comune):</span><span class="sxs-lookup"><span data-stu-id="0e7d6-138">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a><span data-ttu-id="0e7d6-139">Lettura di più</span><span class="sxs-lookup"><span data-stu-id="0e7d6-139">More reading</span></span>

* <span data-ttu-id="0e7d6-140">Lettura [introduzione su ASP.NET Core](../get-started/aspnetcore/index.md) per ulteriori informazioni sull'utilizzo di Entity Framework con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-140">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="0e7d6-141">Lettura [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) per ulteriori informazioni sull'utilizzo DI.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-141">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="0e7d6-142">Lettura [Testing](testing/index.md) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="0e7d6-142">Read [Testing](testing/index.md) for more information.</span></span>
