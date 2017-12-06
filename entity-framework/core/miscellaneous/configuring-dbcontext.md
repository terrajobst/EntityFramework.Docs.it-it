---
title: Configurazione di un elemento DbContext - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 96abf3b94be3e1d19f833644f1c2f6f13fe0e730
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2017
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="df772-102">Configurazione di un elemento DbContext</span><span class="sxs-lookup"><span data-stu-id="df772-102">Configuring a DbContext</span></span>

<span data-ttu-id="df772-103">Questo articolo illustra i modelli per la configurazione di un `DbContext` con `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="df772-103">This article shows patterns for configuring a `DbContext` with `DbContextOptions`.</span></span> <span data-ttu-id="df772-104">Le opzioni vengono utilizzate principalmente per selezionare e configurare l'archivio dati.</span><span class="sxs-lookup"><span data-stu-id="df772-104">Options are primarily used to select and configure the data store.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="df772-105">Configurazione DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="df772-105">Configuring DbContextOptions</span></span>

<span data-ttu-id="df772-106">`DbContext`deve avere un'istanza di `DbContextOptions` per l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="df772-106">`DbContext` must have an instance of `DbContextOptions` in order to execute.</span></span> <span data-ttu-id="df772-107">È possibile configurare eseguendo l'override `OnConfiguring`, o fornita esternamente tramite un argomento del costruttore.</span><span class="sxs-lookup"><span data-stu-id="df772-107">This can be configured by overriding `OnConfiguring`, or supplied externally via a constructor argument.</span></span>

<span data-ttu-id="df772-108">Se si utilizzano entrambi, `OnConfiguring` viene eseguita con le opzioni fornite, pertanto è additivo e può sovrascrivere le opzioni fornite per l'argomento del costruttore.</span><span class="sxs-lookup"><span data-stu-id="df772-108">If both are used, `OnConfiguring` is executed on the supplied options, meaning it is additive and can overwrite  options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="df772-109">Argomento del costruttore</span><span class="sxs-lookup"><span data-stu-id="df772-109">Constructor argument</span></span>

<span data-ttu-id="df772-110">Codice di contesto con un costruttore</span><span class="sxs-lookup"><span data-stu-id="df772-110">Context code with constructor</span></span>

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
> <span data-ttu-id="df772-111">Il costruttore di base di DbContext accetta inoltre la versione non generica di `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="df772-111">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`.</span></span> <span data-ttu-id="df772-112">Per le applicazioni con più tipi di contesto non è consigliabile usare la versione non generica.</span><span class="sxs-lookup"><span data-stu-id="df772-112">Using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="df772-113">Codice dell'applicazione per inizializzare dall'argomento del costruttore</span><span class="sxs-lookup"><span data-stu-id="df772-113">Application code to initialize from constructor argument</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="df772-114">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="df772-114">OnConfiguring</span></span>

> [!WARNING]  
> <span data-ttu-id="df772-115">`OnConfiguring`ultima e può essere sovrascritto ottenute dal costruttore o DI opzioni.</span><span class="sxs-lookup"><span data-stu-id="df772-115">`OnConfiguring` occurs last and can overwrite options obtained from DI or the constructor.</span></span> <span data-ttu-id="df772-116">Questo approccio non si presta a test (a meno che non tutto il database di destinazione).</span><span class="sxs-lookup"><span data-stu-id="df772-116">This approach does not lend itself to testing (unless you target the full database).</span></span>

<span data-ttu-id="df772-117">Codice di contesto con `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="df772-117">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="df772-118">Il codice dell'applicazione per inizializzare con `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="df772-118">Application code to initialize with `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="df772-119">Con l'inserimento di dipendenze DbContext</span><span class="sxs-lookup"><span data-stu-id="df772-119">Using DbContext with dependency injection</span></span>

<span data-ttu-id="df772-120">Supporta l'utilizzo di EF `DbContext` con un contenitore dell'inserimento di dipendenza.</span><span class="sxs-lookup"><span data-stu-id="df772-120">EF supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="df772-121">Il tipo DbContext può essere aggiunti al contenitore del servizio con `AddDbContext<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="df772-121">Your DbContext type can be added to the service container by using `AddDbContext<TContext>`.</span></span>

<span data-ttu-id="df772-122">`AddDbContext`renderà sia il tipo DbContext, `TContext`, e `DbContextOptions<TContext>` disponibili per l'aggiunta dal contenitore del servizio.</span><span class="sxs-lookup"><span data-stu-id="df772-122">`AddDbContext` will make both your DbContext type, `TContext`, and `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="df772-123">Vedere [lettura più](#more-reading) sotto per informazioni sull'inserimento di dipendenze.</span><span class="sxs-lookup"><span data-stu-id="df772-123">See [more reading](#more-reading) below for information on dependency injection.</span></span>

<span data-ttu-id="df772-124">Aggiunta di dbcontext per l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="df772-124">Adding dbcontext to dependency injection</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="df772-125">Questo richiede l'aggiunta di un [argomento del costruttore](#constructor-argument) al tipo DbContext che accetta `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="df772-125">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions`.</span></span>

<span data-ttu-id="df772-126">Codice di contesto:</span><span class="sxs-lookup"><span data-stu-id="df772-126">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="df772-127">Codice dell'applicazione (in ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="df772-127">Application code (in ASP.NET Core):</span></span>

``` csharp
public MyController(BloggingContext context)
```

<span data-ttu-id="df772-128">Codice dell'applicazione (tramite ServiceProvider direttamente, meno comune):</span><span class="sxs-lookup"><span data-stu-id="df772-128">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a><span data-ttu-id="df772-129">Utilizzo`IDesignTimeDbContextFactory<TContext>`</span><span class="sxs-lookup"><span data-stu-id="df772-129">Using `IDesignTimeDbContextFactory<TContext>`</span></span>

<span data-ttu-id="df772-130">In alternativa, per le opzioni sopra riportate, è inoltre possibile fornire un'implementazione di `IDesignTimeDbContextFactory<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="df772-130">As an alternative to the options above, you may also provide an implementation of `IDesignTimeDbContextFactory<TContext>`.</span></span> <span data-ttu-id="df772-131">Strumenti di Entity Framework è possono utilizzare questa factory per creare un'istanza del DbContext.</span><span class="sxs-lookup"><span data-stu-id="df772-131">EF tools can use this factory to create an instance of your DbContext.</span></span> <span data-ttu-id="df772-132">Questo potrebbe essere necessario per abilitare esperienze in fase di progettazione specifiche, ad esempio le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="df772-132">This may be required in order to enable specific design-time experiences such as migrations.</span></span>

<span data-ttu-id="df772-133">Implementare questa interfaccia per abilitare i servizi in fase di progettazione per i tipi di contesto che non dispongono di un costruttore predefinito pubblico.</span><span class="sxs-lookup"><span data-stu-id="df772-133">Implement this interface to enable design-time services for context types that do not have a public default constructor.</span></span> <span data-ttu-id="df772-134">Servizi in fase di progettazione rileverà automaticamente le implementazioni di questa interfaccia nello stesso assembly come contesto derivato.</span><span class="sxs-lookup"><span data-stu-id="df772-134">Design-time services will automatically discover implementations of this interface that are in the same assembly as the derived context.</span></span>

<span data-ttu-id="df772-135">Esempio:</span><span class="sxs-lookup"><span data-stu-id="df772-135">Example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

## <a name="more-reading"></a><span data-ttu-id="df772-136">Lettura di più</span><span class="sxs-lookup"><span data-stu-id="df772-136">More reading</span></span>

* <span data-ttu-id="df772-137">Lettura [introduzione su ASP.NET Core](../get-started/aspnetcore/index.md) per ulteriori informazioni sull'utilizzo di Entity Framework con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="df772-137">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="df772-138">Lettura [Dependency Injection](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) per ulteriori informazioni sull'utilizzo DI.</span><span class="sxs-lookup"><span data-stu-id="df772-138">Read [Dependency Injection](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) to learn more about using DI.</span></span>
* <span data-ttu-id="df772-139">Lettura [Testing](testing/index.md) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="df772-139">Read [Testing](testing/index.md) for more information.</span></span>
