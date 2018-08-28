---
title: Configurazione di un oggetto DbContext - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 393349c05ffaf42c6d2520e73abce23def6becc0
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995938"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="acb70-102">Configurazione di un DbContext</span><span class="sxs-lookup"><span data-stu-id="acb70-102">Configuring a DbContext</span></span>

<span data-ttu-id="acb70-103">Questo articolo illustra i modelli di base per la configurazione di un `DbContext` tramite un `DbContextOptions` per connettersi a un database con uno specifico provider di EF Core e comportamenti facoltativi.</span><span class="sxs-lookup"><span data-stu-id="acb70-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="acb70-104">Configurazione Design-time DbContext</span><span class="sxs-lookup"><span data-stu-id="acb70-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="acb70-105">Fase di progettazione di Entity Framework Core strumenti quali [migrazioni](xref:core/managing-schemas/migrations/index) devono essere in grado di individuare e creare un'istanza di lavoro di un `DbContext` tipo per raccogliere informazioni dettagliate sui tipi di entità dell'applicazione e sul relativo mapping a uno schema di database.</span><span class="sxs-lookup"><span data-stu-id="acb70-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="acb70-106">Questo processo può essere automatica, purché lo strumento è possibile creare con facilità il `DbContext` in modo tale che verrà configurato in modo analogo al modo in cui dovrebbe essere configurato in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="acb70-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="acb70-107">Mentre qualsiasi criterio di ricerca che fornisce le informazioni necessarie per la configurazione per il `DbContext` può funzionare in fase di esecuzione, gli strumenti che richiede l'uso di un `DbContext` in fase di progettazione può funzionare solo con un numero limitato di modelli.</span><span class="sxs-lookup"><span data-stu-id="acb70-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="acb70-108">Questi prerequisiti sono analizzati più dettagliatamente la [creare il contesto in fase di progettazione](xref:core/miscellaneous/cli/dbcontext-creation) sezione.</span><span class="sxs-lookup"><span data-stu-id="acb70-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="acb70-109">Configurazione di DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="acb70-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="acb70-110">`DbContext` deve avere un'istanza di `DbContextOptions` per poter eseguire qualsiasi operazione.</span><span class="sxs-lookup"><span data-stu-id="acb70-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="acb70-111">Il `DbContextOptions` istanza contiene le informazioni di configurazione, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="acb70-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="acb70-112">Il provider di database da utilizzare, in genere selezionata richiamando un metodo, ad esempio `UseSqlServer` o `UseSqlite`</span><span class="sxs-lookup"><span data-stu-id="acb70-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`</span></span>
- <span data-ttu-id="acb70-113">Qualsiasi stringa di connessione necessaria o identificatore dell'istanza del database, in genere passata come argomento al metodo di selezione del provider indicato in precedenza</span><span class="sxs-lookup"><span data-stu-id="acb70-113">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="acb70-114">Tutti i selettori comportamento opzionale a livello del provider, in genere anche concatenati all'interno della chiamata al metodo di selezione del provider</span><span class="sxs-lookup"><span data-stu-id="acb70-114">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="acb70-115">Tutti i selettori generali comportamento di EF Core, in genere concatenati dopo o prima del metodo di selezione del provider</span><span class="sxs-lookup"><span data-stu-id="acb70-115">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="acb70-116">L'esempio seguente configura il `DbContextOptions` per usare il provider SQL Server, contenuto in una connessione di `connectionString` variabile, un timeout del comando a livello del provider e un selettore di comportamento di EF Core che esegue tutte le query eseguite nel `DbContext` [senza registrazione](xref:core/querying/tracking#no-tracking-queries) per impostazione predefinita:</span><span class="sxs-lookup"><span data-stu-id="acb70-116">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="acb70-117">Metodi di selezione del provider e altri metodi di comportamento selettore indicato in precedenza sono i metodi di estensione su `DbContextOptions` o classi di opzioni specifiche del provider.</span><span class="sxs-lookup"><span data-stu-id="acb70-117">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="acb70-118">Per accedere a questi metodi di estensione potrebbe essere necessario disporre di uno spazio dei nomi (in genere `Microsoft.EntityFrameworkCore`) nell'ambito e includere le dipendenze aggiuntive di un pacchetto nel progetto.</span><span class="sxs-lookup"><span data-stu-id="acb70-118">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="acb70-119">Il `DbContextOptions` può essere fornito per il `DbContext` eseguendo l'override di `OnConfiguring` (metodo) o esternamente tramite un argomento del costruttore.</span><span class="sxs-lookup"><span data-stu-id="acb70-119">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="acb70-120">Se vengono utilizzati entrambi, `OnConfiguring` viene applicata per ultima ed possibile sovrascrivere le opzioni fornite per l'argomento del costruttore.</span><span class="sxs-lookup"><span data-stu-id="acb70-120">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="acb70-121">Argomento del costruttore</span><span class="sxs-lookup"><span data-stu-id="acb70-121">Constructor argument</span></span>

<span data-ttu-id="acb70-122">Codice del contesto con il costruttore:</span><span class="sxs-lookup"><span data-stu-id="acb70-122">Context code with constructor:</span></span>

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
> <span data-ttu-id="acb70-123">Il costruttore di base di DbContext accetta anche la versione non generica di `DbContextOptions`, ma usa la versione non generica non è consigliata per le applicazioni con più tipi di contesto.</span><span class="sxs-lookup"><span data-stu-id="acb70-123">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="acb70-124">Codice dell'applicazione per l'inizializzazione dall'argomento di costruttore:</span><span class="sxs-lookup"><span data-stu-id="acb70-124">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="acb70-125">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="acb70-125">OnConfiguring</span></span>

<span data-ttu-id="acb70-126">Codice del contesto con `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="acb70-126">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="acb70-127">Il codice dell'applicazione per inizializzare un `DbContext` che usa `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="acb70-127">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="acb70-128">Questo approccio non adatto all'esecuzione di test, a meno che il test completo del database di destinazione.</span><span class="sxs-lookup"><span data-stu-id="acb70-128">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="acb70-129">Utilizzo di DbContext con inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="acb70-129">Using DbContext with dependency injection</span></span>

<span data-ttu-id="acb70-130">EF Core supporta l'utilizzo di `DbContext` con un contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="acb70-130">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="acb70-131">Il tipo DbContext può essere aggiunti al contenitore del servizio tramite il `AddDbContext<TContext>` (metodo).</span><span class="sxs-lookup"><span data-stu-id="acb70-131">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="acb70-132">`AddDbContext<TContext>` renderà sia del tipo DbContext `TContext`e le corrispondenti `DbContextOptions<TContext>` disponibile per l'inserimento dal contenitore dei servizi.</span><span class="sxs-lookup"><span data-stu-id="acb70-132">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="acb70-133">Visualizzare [ulteriori informazioni](#more-reading) sotto per altre informazioni sull'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="acb70-133">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="acb70-134">Aggiunta di `Dbcontext` all'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="acb70-134">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="acb70-135">Ciò richiede l'aggiunta di un [argomento del costruttore](#constructor-argument) per il tipo DbContext che accetta `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="acb70-135">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="acb70-136">Codice contesto:</span><span class="sxs-lookup"><span data-stu-id="acb70-136">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="acb70-137">Codice dell'applicazione (in ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="acb70-137">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="acb70-138">Codice dell'applicazione (tramite ServiceProvider direttamente, meno comune):</span><span class="sxs-lookup"><span data-stu-id="acb70-138">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a><span data-ttu-id="acb70-139">Ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="acb70-139">More reading</span></span>

* <span data-ttu-id="acb70-140">Lettura [Guida introduttiva ad ASP.NET Core](../get-started/aspnetcore/index.md) per altre informazioni sull'uso di EF con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="acb70-140">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="acb70-141">Lettura [inserimento delle dipendenze](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) per altre informazioni sull'uso di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="acb70-141">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="acb70-142">Lettura [Testing](testing/index.md) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="acb70-142">Read [Testing](testing/index.md) for more information.</span></span>
