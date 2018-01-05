---
title: Creazione in fase di progettazione DbContext - Core EF
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: a899c474cc45437bff7c82ce5bddeb915b15c3b0
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/22/2017
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="20354-102">Creazione in fase di progettazione DbContext</span><span class="sxs-lookup"><span data-stu-id="20354-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="20354-103">Alcuni dei comandi strumenti di base di Entity Framework (ad esempio, il [migrazioni] [ 1] comandi) richiedono un oggetto derivato `DbContext` istanza deve essere creato in fase di progettazione per raccogliere i dettagli relativi all'applicazione tipi di entità e il relativo mapping a uno schema di database.</span><span class="sxs-lookup"><span data-stu-id="20354-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="20354-104">Nella maggior parte dei casi, è consigliabile che il `DbContext` in tal modo creato è configurato in modo analogo a come sarebbe [configurato in fase di esecuzione][2].</span><span class="sxs-lookup"><span data-stu-id="20354-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="20354-105">Sono disponibili vari modi gli strumenti di provare a creare il `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="20354-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="20354-106">Dai servizi dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="20354-106">From application services</span></span>
-------------------------
<span data-ttu-id="20354-107">Se il progetto di avvio è un'applicazione ASP.NET di base, gli strumenti di provare a ottenere l'oggetto DbContext dal provider di servizi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="20354-107">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="20354-108">Lo strumento innanzitutto provare a ottenere il provider di servizi richiamando `Program.BuildWebHost()` e l'accesso di `IWebHost.Services` proprietà.</span><span class="sxs-lookup"><span data-stu-id="20354-108">The tool first try to obtain the service provider by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span>

> [!NOTE]
> <span data-ttu-id="20354-109">Quando si crea una nuova applicazione ASP.NET Core 2.0, questo hook è incluso per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="20354-109">When you create a new ASP.NET Core 2.0 application, this hook is included by default.</span></span> <span data-ttu-id="20354-110">Nelle versioni precedenti di EF Core e ASP.NET Core, gli strumenti di provano a richiamare `Startup.ConfigureServices` direttamente per ottenere i provider di servizi dell'applicazione, ma questo modello non funziona correttamente nelle applicazioni ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="20354-110">In previous versions of EF Core and ASP.NET Core, the tools try to invoke `Startup.ConfigureServices` directly in order to obtain the application's service provider, but this pattern no longer works correctly in ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="20354-111">Se si esegue l'aggiornamento di un'applicazione ASP.NET di base 1. x 2.0, è possibile [modificare il `Program` classe seguire il nuovo modello][3].</span><span class="sxs-lookup"><span data-stu-id="20354-111">If you are upgrading an ASP.NET Core 1.x application to 2.0, you can [modify your `Program` class to follow the new pattern][3].</span></span>

<span data-ttu-id="20354-112">Il `DbContext` devono essere registrati come servizi in provider del servizio dell'applicazione stessa e tutte le dipendenze nel relativo costruttore.</span><span class="sxs-lookup"><span data-stu-id="20354-112">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="20354-113">Ciò può essere facilmente ottenuto con [un costruttore nel `DbContext` che accetta un'istanza di `DbContextOptions<TContext>` come argomento] [ 4] e l'utilizzo di [ `AddDbContext<TContext>` (metodo)] [5].</span><span class="sxs-lookup"><span data-stu-id="20354-113">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as a argument][4] and using the [`AddDbContext<TContext>` method][5].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="20354-114">Utilizzando un costruttore senza parametri</span><span class="sxs-lookup"><span data-stu-id="20354-114">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="20354-115">Se l'elemento DbContext non è possibile ottenere dal provider del servizio di applicazione, gli strumenti di cercare derivata `DbContext` tipo all'interno del progetto.</span><span class="sxs-lookup"><span data-stu-id="20354-115">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="20354-116">Quindi provare a creare un'istanza con un costruttore senza parametri.</span><span class="sxs-lookup"><span data-stu-id="20354-116">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="20354-117">Può essere il costruttore predefinito se la `DbContext` viene configurato usando il [ `OnConfiguring` ] [ 6] metodo.</span><span class="sxs-lookup"><span data-stu-id="20354-117">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][6] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="20354-118">Da una factory in fase di progettazione</span><span class="sxs-lookup"><span data-stu-id="20354-118">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="20354-119">È anche possibile stabilire gli strumenti come creare l'elemento DbContext implementando il `IDesignTimeDbContextFactory<TContext>` interfaccia: se una classe che implementa questa interfaccia viene trovata in uno stesso progetto derivata `DbContext` o nel progetto di avvio dell'applicazione, ignorare gli strumenti altri modi di creare invece di DbContext e si usa la factory in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="20354-119">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

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

> [!NOTE]
> <span data-ttu-id="20354-120">Il `args` parametro non viene attualmente utilizzato.</span><span class="sxs-lookup"><span data-stu-id="20354-120">The `args` parameter is currently unused.</span></span> <span data-ttu-id="20354-121">È presente [un problema] [ 7] la possibilità di specificare gli argomenti in fase di progettazione da strumenti di rilevamento.</span><span class="sxs-lookup"><span data-stu-id="20354-121">There is [an issue][7] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="20354-122">Una factory in fase di progettazione può essere particolarmente utile se è necessario configurare l'elemento DbContext in modo diverso per la fase di progettazione a fase di esecuzione, se il `DbContext` accetta costruttore parametri aggiuntivi non sono registrati in DI, se non si utilizza DI affatto o se per alcuni motivo si preferisce non hanno un `BuildWebHost` nell'applicazione ASP.NET Core (metodo)</span><span class="sxs-lookup"><span data-stu-id="20354-122">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's</span></span>  
<span data-ttu-id="20354-123">Classe `Main`.</span><span class="sxs-lookup"><span data-stu-id="20354-123">`Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332