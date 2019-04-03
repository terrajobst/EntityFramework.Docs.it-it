---
title: Configurazione di un oggetto DbContext - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 9400fe8ea817b6aca0fb63c1de05ffe1dc997b2f
ms.sourcegitcommit: a8b04050033c5dc46c076b7e21b017749e0967a8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/02/2019
ms.locfileid: "58868009"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="084df-102">Configurazione di un DbContext</span><span class="sxs-lookup"><span data-stu-id="084df-102">Configuring a DbContext</span></span>

<span data-ttu-id="084df-103">Questo articolo illustra i modelli di base per la configurazione di un `DbContext` tramite un `DbContextOptions` per connettersi a un database con uno specifico provider di EF Core e comportamenti facoltativi.</span><span class="sxs-lookup"><span data-stu-id="084df-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="084df-104">Configurazione Design-time DbContext</span><span class="sxs-lookup"><span data-stu-id="084df-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="084df-105">Fase di progettazione di Entity Framework Core strumenti quali [migrazioni](xref:core/managing-schemas/migrations/index) devono essere in grado di individuare e creare un'istanza di lavoro di un `DbContext` tipo per raccogliere informazioni dettagliate sui tipi di entità dell'applicazione e sul relativo mapping a uno schema di database.</span><span class="sxs-lookup"><span data-stu-id="084df-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="084df-106">Questo processo può essere automatica, purché lo strumento è possibile creare con facilità il `DbContext` in modo tale che verrà configurato in modo analogo al modo in cui dovrebbe essere configurato in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="084df-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="084df-107">Mentre qualsiasi criterio di ricerca che fornisce le informazioni necessarie per la configurazione per il `DbContext` può funzionare in fase di esecuzione, gli strumenti che richiede l'uso di un `DbContext` in fase di progettazione può funzionare solo con un numero limitato di modelli.</span><span class="sxs-lookup"><span data-stu-id="084df-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="084df-108">Questi prerequisiti sono analizzati più dettagliatamente la [creare il contesto in fase di progettazione](xref:core/miscellaneous/cli/dbcontext-creation) sezione.</span><span class="sxs-lookup"><span data-stu-id="084df-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="084df-109">Configurazione di DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="084df-109">Configuring DbContextOptions</span></span>

`DbContext` <span data-ttu-id="084df-110">deve avere un'istanza di `DbContextOptions` per poter eseguire qualsiasi operazione.</span><span class="sxs-lookup"><span data-stu-id="084df-110">must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="084df-111">Il `DbContextOptions` istanza contiene le informazioni di configurazione, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="084df-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="084df-112">Il provider di database da utilizzare, in genere selezionata richiamando un metodo, ad esempio `UseSqlServer` o `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="084df-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="084df-113">Questi metodi di estensione richiedono che il pacchetto di provider corrispondente, ad esempio `Microsoft.EntityFrameworkCore.SqlServer` o `Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="084df-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="084df-114">I metodi sono definiti nel `Microsoft.EntityFrameworkCore` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="084df-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="084df-115">Qualsiasi stringa di connessione necessaria o identificatore dell'istanza del database, in genere passata come argomento al metodo di selezione del provider indicato in precedenza</span><span class="sxs-lookup"><span data-stu-id="084df-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="084df-116">Tutti i selettori comportamento opzionale a livello del provider, in genere anche concatenati all'interno della chiamata al metodo di selezione del provider</span><span class="sxs-lookup"><span data-stu-id="084df-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="084df-117">Tutti i selettori generali comportamento di EF Core, in genere concatenati dopo o prima del metodo di selezione del provider</span><span class="sxs-lookup"><span data-stu-id="084df-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="084df-118">L'esempio seguente configura il `DbContextOptions` per usare il provider SQL Server, contenuto in una connessione di `connectionString` variabile, un timeout del comando a livello del provider e un selettore di comportamento di EF Core che esegue tutte le query eseguite nel `DbContext` [senza registrazione](xref:core/querying/tracking#no-tracking-queries) per impostazione predefinita:</span><span class="sxs-lookup"><span data-stu-id="084df-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="084df-119">Metodi di selezione del provider e altri metodi di comportamento selettore indicato in precedenza sono i metodi di estensione su `DbContextOptions` o classi di opzioni specifiche del provider.</span><span class="sxs-lookup"><span data-stu-id="084df-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="084df-120">Per accedere a questi metodi di estensione potrebbe essere necessario disporre di uno spazio dei nomi (in genere `Microsoft.EntityFrameworkCore`) nell'ambito e includere le dipendenze aggiuntive di un pacchetto nel progetto.</span><span class="sxs-lookup"><span data-stu-id="084df-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="084df-121">Il `DbContextOptions` può essere fornito per il `DbContext` eseguendo l'override di `OnConfiguring` (metodo) o esternamente tramite un argomento del costruttore.</span><span class="sxs-lookup"><span data-stu-id="084df-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="084df-122">Se vengono utilizzati entrambi, `OnConfiguring` viene applicata per ultima ed possibile sovrascrivere le opzioni fornite per l'argomento del costruttore.</span><span class="sxs-lookup"><span data-stu-id="084df-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="084df-123">Argomento del costruttore</span><span class="sxs-lookup"><span data-stu-id="084df-123">Constructor argument</span></span>

<span data-ttu-id="084df-124">Codice del contesto con il costruttore:</span><span class="sxs-lookup"><span data-stu-id="084df-124">Context code with constructor:</span></span>

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
> <span data-ttu-id="084df-125">Il costruttore di base di DbContext accetta anche la versione non generica di `DbContextOptions`, ma usa la versione non generica non è consigliata per le applicazioni con più tipi di contesto.</span><span class="sxs-lookup"><span data-stu-id="084df-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="084df-126">Codice dell'applicazione per l'inizializzazione dall'argomento di costruttore:</span><span class="sxs-lookup"><span data-stu-id="084df-126">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="084df-127">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="084df-127">OnConfiguring</span></span>

<span data-ttu-id="084df-128">Codice del contesto con `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="084df-128">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="084df-129">Il codice dell'applicazione per inizializzare un `DbContext` che usa `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="084df-129">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="084df-130">Questo approccio non adatto all'esecuzione di test, a meno che il test completo del database di destinazione.</span><span class="sxs-lookup"><span data-stu-id="084df-130">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="084df-131">Utilizzo di DbContext con inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="084df-131">Using DbContext with dependency injection</span></span>

<span data-ttu-id="084df-132">EF Core supporta l'utilizzo di `DbContext` con un contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="084df-132">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="084df-133">Il tipo DbContext può essere aggiunti al contenitore del servizio tramite il `AddDbContext<TContext>` (metodo).</span><span class="sxs-lookup"><span data-stu-id="084df-133">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

`AddDbContext<TContext>` <span data-ttu-id="084df-134">renderà sia del tipo DbContext `TContext`e le corrispondenti `DbContextOptions<TContext>` disponibile per l'inserimento dal contenitore dei servizi.</span><span class="sxs-lookup"><span data-stu-id="084df-134">will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="084df-135">Visualizzare [ulteriori informazioni](#more-reading) sotto per altre informazioni sull'inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="084df-135">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="084df-136">Aggiunta di `Dbcontext` all'inserimento delle dipendenze:</span><span class="sxs-lookup"><span data-stu-id="084df-136">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="084df-137">Ciò richiede l'aggiunta di un [argomento del costruttore](#constructor-argument) per il tipo DbContext che accetta `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="084df-137">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="084df-138">Codice contesto:</span><span class="sxs-lookup"><span data-stu-id="084df-138">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="084df-139">Codice dell'applicazione (in ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="084df-139">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="084df-140">Codice dell'applicazione (tramite ServiceProvider direttamente, meno comune):</span><span class="sxs-lookup"><span data-stu-id="084df-140">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="084df-141">Come evitare problemi relativi al threading DbContext</span><span class="sxs-lookup"><span data-stu-id="084df-141">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="084df-142">Entity Framework Core non supporta più operazioni in parallelo in esecuzione sullo stesso `DbContext` istanza.</span><span class="sxs-lookup"><span data-stu-id="084df-142">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="084df-143">Accesso simultaneo può causare un comportamento indefinito, arresti anomali delle applicazioni e il danneggiamento dei dati.</span><span class="sxs-lookup"><span data-stu-id="084df-143">Concurrent access can result in undefined behavior, application crashes and data corruption.</span></span> <span data-ttu-id="084df-144">Questo è importante usare sempre motivo separare `DbContext` istanze per le operazioni eseguite in parallelo.</span><span class="sxs-lookup"><span data-stu-id="084df-144">Because of this it's important to always use separate `DbContext` instances for operations that execute in parallel.</span></span> 

<span data-ttu-id="084df-145">Sono disponibili gli errori comuni che possono inadvernetly causa accesso simultaneo nella stessa `DbContext` istanza:</span><span class="sxs-lookup"><span data-stu-id="084df-145">There are common mistakes that can inadvernetly cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="084df-146">Se si dimentica di attendere il completamento di un'operazione asincrona prima di avviare qualsiasi altra operazione in stesso DbContext</span><span class="sxs-lookup"><span data-stu-id="084df-146">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="084df-147">I metodi asincroni abilitato EF Core avviare le operazioni che accedono al database in modo non bloccante.</span><span class="sxs-lookup"><span data-stu-id="084df-147">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="084df-148">Tuttavia, se un chiamante non attende il completamento di uno di questi metodi e continua a eseguire altre operazioni sul `DbContext`, dello stato del `DbContext` può essere (e molto probabilmente verrà) danneggiato.</span><span class="sxs-lookup"><span data-stu-id="084df-148">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span> 

<span data-ttu-id="084df-149">Attendere sempre i metodi asincroni di EF Core immediatamente.</span><span class="sxs-lookup"><span data-stu-id="084df-149">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="084df-150">In modo implicito la condivisione di istanze DbContext attraverso più thread tramite inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="084df-150">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="084df-151">Il [ `AddDbContext` ](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) registra il metodo di estensione `DbContext` tipi con una [durata con ambito](https://docs .microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="084df-151">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs .microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span> 

<span data-ttu-id="084df-152">Ciò è protetta da problemi di accesso simultaneo in applicazioni ASP.NET Core, poiché è presente un solo thread per l'esecuzione di ogni richiesta client in un determinato momento e poiché ogni richiesta di ottenere un ambito di inserimento delle dipendenze separato (e pertanto un oggetto separato `DbContext` istanza).</span><span class="sxs-lookup"><span data-stu-id="084df-152">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="084df-153">Tuttavia necessario assicurarsi che qualsiasi codice eseguito in modo esplicito più thread in parallelo `DbContext` istanze non sono mai accesed contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="084df-153">However any code that explicitly executes multiple threads in paralell should ensure that `DbContext` instances aren't ever accesed concurrently.</span></span>

<span data-ttu-id="084df-154">Mediante l'inserimento di dipendenza, questo scopo, il contesto viene registrato come ambiti hanno come ambiti e la creazione (tramite `IServiceScopeFactory`) per ogni thread, o registrando il `DbContext` temporanei (usando l'overload di `AddDbContext` che accetta un `ServiceLifetime` parametro).</span><span class="sxs-lookup"><span data-stu-id="084df-154">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="084df-155">Ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="084df-155">More reading</span></span>

* <span data-ttu-id="084df-156">Lettura [Guida introduttiva ad ASP.NET Core](../get-started/aspnetcore/index.md) per altre informazioni sull'uso di EF con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="084df-156">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="084df-157">Lettura [inserimento delle dipendenze](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) per altre informazioni sull'uso di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="084df-157">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="084df-158">Lettura [Testing](testing/index.md) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="084df-158">Read [Testing](testing/index.md) for more information.</span></span>
