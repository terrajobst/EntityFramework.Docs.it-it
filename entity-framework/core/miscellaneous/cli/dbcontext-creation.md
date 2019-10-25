---
title: Creazione DbContext della fase di progettazione-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: c36dae150085b1ab509288f6fabfdd8ed7201ca8
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812026"
---
# <a name="design-time-dbcontext-creation"></a><span data-ttu-id="f2b61-102">Creazione di DbContext in fase di progettazione</span><span class="sxs-lookup"><span data-stu-id="f2b61-102">Design-time DbContext Creation</span></span>

<span data-ttu-id="f2b61-103">Alcuni dei comandi degli strumenti EF Core (ad esempio, i comandi delle [migrazioni][1] ) richiedono la creazione di un'istanza di `DbContext` derivata in fase di progettazione per raccogliere informazioni dettagliate sui tipi di entità dell'applicazione e su come eseguono il mapping a uno schema del database.</span><span class="sxs-lookup"><span data-stu-id="f2b61-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="f2b61-104">Nella maggior parte dei casi, è preferibile che il `DbContext` creato venga configurato in modo analogo a come verrebbe [configurato][2]in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f2b61-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="f2b61-105">Esistono diversi modi in cui gli strumenti tentano di creare la `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="f2b61-105">There are various ways the tools try to create the `DbContext`:</span></span>

## <a name="from-application-services"></a><span data-ttu-id="f2b61-106">Da servizi applicazioni</span><span class="sxs-lookup"><span data-stu-id="f2b61-106">From application services</span></span>

<span data-ttu-id="f2b61-107">Se il progetto di avvio usa il [ASP.NET Core host Web][3] o l' [host generico .NET Core][4], gli strumenti tentano di ottenere l'oggetto DbContext dal provider di servizi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f2b61-107">If your startup project uses the [ASP.NET Core Web Host][3] or [.NET Core Generic Host][4], the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="f2b61-108">Gli strumenti tentano innanzitutto di ottenere il provider di servizi richiamando `Program.CreateHostBuilder()`, chiamando `Build()`e quindi accedendo alla proprietà `Services`.</span><span class="sxs-lookup"><span data-stu-id="f2b61-108">The tools first try to obtain the service provider by invoking `Program.CreateHostBuilder()`, calling `Build()`, then accessing the `Services` property.</span></span>

``` csharp
public class Program
{
    public static void Main(string[] args)
        => CreateHostBuilder(args).Build().Run();

    // EF Core uses this method at design time to access the DbContext
    public static IHostBuilder CreateHostBuilder(string[] args)
        => Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(
                webBuilder => webBuilder.UseStartup<Startup>());
}

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
        => services.AddDbContext<ApplicationDbContext>();
}

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

> [!NOTE]
> <span data-ttu-id="f2b61-109">Quando si crea una nuova applicazione ASP.NET Core, questo hook è incluso per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="f2b61-109">When you create a new ASP.NET Core application, this hook is included by default.</span></span>

<span data-ttu-id="f2b61-110">Il `DbContext` stesso e tutte le dipendenze nel costruttore devono essere registrate come servizi nel provider di servizi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f2b61-110">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="f2b61-111">Questa operazione può essere eseguita facilmente con [un costruttore sul `DbContext` che accetta un'istanza di `DbContextOptions<TContext>` come argomento][5] e usando il [Metodo`AddDbContext<TContext>`][6].</span><span class="sxs-lookup"><span data-stu-id="f2b61-111">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][5] and using the [`AddDbContext<TContext>` method][6].</span></span>

## <a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="f2b61-112">Uso di un costruttore senza parametri</span><span class="sxs-lookup"><span data-stu-id="f2b61-112">Using a constructor with no parameters</span></span>

<span data-ttu-id="f2b61-113">Se il DbContext non può essere ottenuto dal provider di servizi dell'applicazione, gli strumenti cercano il tipo di `DbContext` derivato all'interno del progetto.</span><span class="sxs-lookup"><span data-stu-id="f2b61-113">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="f2b61-114">Tentano quindi di creare un'istanza usando un costruttore senza parametri.</span><span class="sxs-lookup"><span data-stu-id="f2b61-114">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="f2b61-115">Può essere il costruttore predefinito se il `DbContext` viene configurato utilizzando il metodo [`OnConfiguring`][7] .</span><span class="sxs-lookup"><span data-stu-id="f2b61-115">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][7] method.</span></span>

## <a name="from-a-design-time-factory"></a><span data-ttu-id="f2b61-116">Da una factory della fase di progettazione</span><span class="sxs-lookup"><span data-stu-id="f2b61-116">From a design-time factory</span></span>

<span data-ttu-id="f2b61-117">È anche possibile indicare agli strumenti come creare il DbContext implementando l'interfaccia `IDesignTimeDbContextFactory<TContext>`: se una classe che implementa questa interfaccia si trova nello stesso progetto della `DbContext` derivata o nel progetto di avvio dell'applicazione, gli strumenti ignorano l'altro modi per creare il DbContext e usare invece la factory in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="f2b61-117">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
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

> [!NOTE]
> <span data-ttu-id="f2b61-118">Il parametro `args` non è attualmente utilizzato.</span><span class="sxs-lookup"><span data-stu-id="f2b61-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="f2b61-119">Si è verificato [un problema][8] durante il rilevamento della possibilità di specificare gli argomenti della fase di progettazione dagli strumenti.</span><span class="sxs-lookup"><span data-stu-id="f2b61-119">There is [an issue][8] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="f2b61-120">Una factory della fase di progettazione può essere particolarmente utile se è necessario configurare il DbContext in modo diverso per la fase di progettazione rispetto al runtime, se il costruttore `DbContext` accetta parametri aggiuntivi non è registrato in DI, se non si usa o se per qualche motivo preferisce non avere un metodo di `BuildWebHost` nella classe `Main` dell'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f2b61-120">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
