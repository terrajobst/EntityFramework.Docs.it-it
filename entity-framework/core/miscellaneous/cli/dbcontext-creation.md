---
title: Creazione in fase di progettazione DbContext - Core EF
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 5fcd9e362d76127e7acadd9e552ef3ac90967a37
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="79892-102">Creazione in fase di progettazione DbContext</span><span class="sxs-lookup"><span data-stu-id="79892-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="79892-103">Alcuni degli strumenti di Entity Framework comandi richiedono un'istanza di DbContext deve essere creato in fase di progettazione ora (ad esempio, quando esegue i comandi di migrazioni).</span><span class="sxs-lookup"><span data-stu-id="79892-103">Some of the EF Tools commands require a DbContext instance to be created at design time (for example, when running Migrations commands).</span></span> <span data-ttu-id="79892-104">Sono disponibili vari modi che gli strumenti di tentare di crearlo.</span><span class="sxs-lookup"><span data-stu-id="79892-104">There are various ways the tools try to create it.</span></span>

<a name="from-application-services"></a><span data-ttu-id="79892-105">Dai servizi dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="79892-105">From application services</span></span>
-------------------------
<span data-ttu-id="79892-106">Se il progetto di avvio è un'applicazione ASP.NET di base, gli strumenti di provare a ottenere l'oggetto DbContext dal provider di servizi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="79892-106">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span> <span data-ttu-id="79892-107">Ottengono richiamando `Program.BuildWebHost()` e l'accesso di `IWebHost.Services` proprietà.</span><span class="sxs-lookup"><span data-stu-id="79892-107">They obtain it by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span> <span data-ttu-id="79892-108">Qualsiasi DbContext registrati mediante `IServiceCollection.AddDbContext<TContext>()` può essere trovato e create in questo modo.</span><span class="sxs-lookup"><span data-stu-id="79892-108">Any DbContext registered using `IServiceCollection.AddDbContext<TContext>()` can be found and created this way.</span></span> <span data-ttu-id="79892-109">Questo modello è stato [introdotto in ASP.NET 2.0 Core][1]</span><span class="sxs-lookup"><span data-stu-id="79892-109">This pattern was [introduced in ASP.NET Core 2.0][1]</span></span>

<a name="using-the-default-constructor"></a><span data-ttu-id="79892-110">Utilizzando il costruttore predefinito</span><span class="sxs-lookup"><span data-stu-id="79892-110">Using the default constructor</span></span>
-----------------------------
<span data-ttu-id="79892-111">Se l'elemento DbContext non è possibile ottenere dal provider del servizio di applicazione, gli strumenti di ricerca per il tipo DbContext all'interno del progetto.</span><span class="sxs-lookup"><span data-stu-id="79892-111">If the DbContext can't be obtained from the application service provider, the tools look for the DbContext type inside the project.</span></span> <span data-ttu-id="79892-112">Provare a creare utilizzando il costruttore predefinito.</span><span class="sxs-lookup"><span data-stu-id="79892-112">They try to create it using its default constructor.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="79892-113">Da una factory in fase di progettazione</span><span class="sxs-lookup"><span data-stu-id="79892-113">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="79892-114">È anche possibile stabilire gli strumenti come creare l'elemento DbContext implementando `IDesignTimeDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="79892-114">You can also tell the tools how to create your DbContext by implementing `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="79892-115">Se una classe che implementa questa interfaccia viene trovata all'interno del progetto, gli strumenti di ignorare le altre modalità di creazione DbContext.</span><span class="sxs-lookup"><span data-stu-id="79892-115">If a class implementing this interface is found inside your project, the tools bypass the other ways of creating the DbContext.</span></span>
<span data-ttu-id="79892-116">Essi utilizzano sempre la factory in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="79892-116">They always use the factory at design time.</span></span> <span data-ttu-id="79892-117">Una factory è particolarmente utile se è necessario configurare l'elemento DbContext in modo diverso per la fase di progettazione di in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="79892-117">A factory is especially useful if you need to configure the DbContext differently for design time than at runtime.</span></span>

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
> <span data-ttu-id="79892-118">Il `args` parametro non viene attualmente utilizzato.</span><span class="sxs-lookup"><span data-stu-id="79892-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="79892-119">È presente [un problema] [ 2] la possibilità di specificare gli argomenti in fase di progettazione da strumenti di rilevamento.</span><span class="sxs-lookup"><span data-stu-id="79892-119">There is [an issue][2] tracking the ability to specify design-time arguments from the tools.</span></span>

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
