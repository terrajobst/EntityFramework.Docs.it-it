---
title: L'aggiornamento da Entity Framework Core 1.0 RC1 a RC2 - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
ms.technology: entity-framework-core
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: e76886729248101ccac024568cf9abcd945fca33
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678627"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="0eb22-102">L'aggiornamento da Entity Framework Core 1.0 RC1 a 1.0 RC2</span><span class="sxs-lookup"><span data-stu-id="0eb22-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="0eb22-103">In questo articolo vengono fornite indicazioni per lo spostamento di un'applicazione compilata con i pacchetti RC1 a RC2.</span><span class="sxs-lookup"><span data-stu-id="0eb22-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="0eb22-104">Versioni e i nomi di pacchetto</span><span class="sxs-lookup"><span data-stu-id="0eb22-104">Package Names and Versions</span></span>

<span data-ttu-id="0eb22-105">Tra RC1 e RC2, è stato modificato da "Entity Framework 7" a "Entity Framework Core".</span><span class="sxs-lookup"><span data-stu-id="0eb22-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="0eb22-106">È possibile leggere altre informazioni sui motivi per la modifica in [questo post di Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="0eb22-106">You can read more about the reasons for the change in [this post by Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="0eb22-107">A causa di questa modifica, i nomi dei pacchetti impostate da `EntityFramework.*` a `Microsoft.EntityFrameworkCore.*` e la versioni da `7.0.0-rc1-final` a `1.0.0-rc2-final` (o `1.0.0-preview1-final` per gli strumenti).</span><span class="sxs-lookup"><span data-stu-id="0eb22-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="0eb22-108">**È necessario rimuovere completamente i pacchetti RC1 e quindi installare il RC2 quelle.**</span><span class="sxs-lookup"><span data-stu-id="0eb22-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="0eb22-109">Di seguito è riportato il mapping per alcuni pacchetti comuni.</span><span class="sxs-lookup"><span data-stu-id="0eb22-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="0eb22-110">Pacchetto di RC1</span><span class="sxs-lookup"><span data-stu-id="0eb22-110">RC1 Package</span></span>                                               | <span data-ttu-id="0eb22-111">Equivalente di RC2</span><span class="sxs-lookup"><span data-stu-id="0eb22-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="0eb22-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="0eb22-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="0eb22-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="0eb22-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="0eb22-114">EntityFramework.SQLite                    7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="0eb22-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="0eb22-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="0eb22-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="0eb22-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="0eb22-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="0eb22-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span><span class="sxs-lookup"><span data-stu-id="0eb22-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="0eb22-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="0eb22-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="0eb22-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="0eb22-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="0eb22-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="0eb22-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="0eb22-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="0eb22-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="0eb22-122">EntityFramework.InMemory                  7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="0eb22-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="0eb22-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="0eb22-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="0eb22-124">EntityFramework.IBMDataServer             7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="0eb22-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="0eb22-125">Non è ancora disponibile per RC2</span><span class="sxs-lookup"><span data-stu-id="0eb22-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="0eb22-126">EntityFramework.Commands                  7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="0eb22-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="0eb22-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span><span class="sxs-lookup"><span data-stu-id="0eb22-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="0eb22-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="0eb22-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="0eb22-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="0eb22-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="0eb22-130">Spazi dei nomi</span><span class="sxs-lookup"><span data-stu-id="0eb22-130">Namespaces</span></span>

<span data-ttu-id="0eb22-131">Insieme a nomi di pacchetto, gli spazi dei nomi è stato modificato da `Microsoft.Data.Entity.*` a `Microsoft.EntityFrameworkCore.*`.</span><span class="sxs-lookup"><span data-stu-id="0eb22-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="0eb22-132">È possibile gestire questa modifica con una ricerca/sostituzione di `using Microsoft.Data.Entity` con `using Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="0eb22-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="0eb22-133">Tabella delle modifiche di convenzione di denominazione</span><span class="sxs-lookup"><span data-stu-id="0eb22-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="0eb22-134">Una modifica significativa di funzionalità preso in RC2 consiste nell'utilizzare il nome della `DbSet<TEntity>` proprietà per un'entità specificata come nome della tabella viene eseguito il mapping, anziché solo il nome della classe.</span><span class="sxs-lookup"><span data-stu-id="0eb22-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="0eb22-135">Altre informazioni su questa modifica in [il problema correlato annuncio](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="0eb22-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="0eb22-136">Per le applicazioni esistenti RC1, è consigliabile aggiungere il codice seguente all'inizio del `OnModelCreating` metodo per mantenere la strategia di denominazione RC1:</span><span class="sxs-lookup"><span data-stu-id="0eb22-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="0eb22-137">Se si desidera adottare la strategia di denominazione di nuovo, sarebbe consigliabile correttamente completato il resto dei passaggi di aggiornamento quindi il codice di rimozione e la creazione di una migrazione per applicare la tabella rinominata.</span><span class="sxs-lookup"><span data-stu-id="0eb22-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="0eb22-138">AddDbContext / Startup.cs modifica (solo per progetti ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="0eb22-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="0eb22-139">Nella versione RC1, è necessario aggiungere i servizi di Entity Framework per il provider di servizi di applicazione, `Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="0eb22-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="0eb22-140">In RC2, è possibile rimuovere le chiamate a `AddEntityFramework()`, `AddSqlServer()`e così via.:</span><span class="sxs-lookup"><span data-stu-id="0eb22-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="0eb22-141">È inoltre necessario aggiungere un costruttore, del contesto derivato, che accetta le opzioni di contesto e li passa al costruttore di base.</span><span class="sxs-lookup"><span data-stu-id="0eb22-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="0eb22-142">Ciò è necessario perché sono state rimosse alcune il trucco sconvolgere che snuck in background:</span><span class="sxs-lookup"><span data-stu-id="0eb22-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="0eb22-143">Passando IServiceProvider</span><span class="sxs-lookup"><span data-stu-id="0eb22-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="0eb22-144">Se si dispone di codice RC1 che passa un `IServiceProvider` al contesto, questo ha spostato in `DbContextOptions`, non è un parametro di costruttore separato.</span><span class="sxs-lookup"><span data-stu-id="0eb22-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="0eb22-145">Utilizzare `DbContextOptionsBuilder.UseInternalServiceProvider(...)` per impostare il provider del servizio.</span><span class="sxs-lookup"><span data-stu-id="0eb22-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="0eb22-146">Test</span><span class="sxs-lookup"><span data-stu-id="0eb22-146">Testing</span></span>

<span data-ttu-id="0eb22-147">Lo scenario più comune per effettuare questa operazione consiste nel controllare l'ambito di un database InMemory durante il test.</span><span class="sxs-lookup"><span data-stu-id="0eb22-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="0eb22-148">Vedere aggiornato [Testing](testing/index.md) articolo per un esempio di eseguire questa operazione con RC2.</span><span class="sxs-lookup"><span data-stu-id="0eb22-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="0eb22-149">Risolvere i servizi interni dal Provider di servizi di applicazione (solo progetti ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="0eb22-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="0eb22-150">Se si dispone di un'applicazione ASP.NET Core e si desidera EF per risolvere i servizi interni dal provider del servizio dell'applicazione, si verifica un overload di `AddDbContext` che consente di configurare questa impostazione:</span><span class="sxs-lookup"><span data-stu-id="0eb22-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> <span data-ttu-id="0eb22-151">È consigliabile consentire EF internamente gestire i propri servizi, a meno che non esista un motivo per combinare i servizi EF interni in provider di servizi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="0eb22-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="0eb22-152">Il motivo principale, che si consiglia di eseguire questa operazione consiste nell'utilizzare il provider di servizi di applicazione per sostituire i servizi EF utilizzato internamente</span><span class="sxs-lookup"><span data-stu-id="0eb22-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="0eb22-153">I comandi DNX = > CLI .NET (solo progetti ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="0eb22-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="0eb22-154">Se è stata utilizzata in precedenza il `dnx ef` comandi per i progetti ASP.NET 5, queste sono ora spostati `dotnet ef` comandi.</span><span class="sxs-lookup"><span data-stu-id="0eb22-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="0eb22-155">Si applica comunque la stessa sintassi del comando.</span><span class="sxs-lookup"><span data-stu-id="0eb22-155">The same command syntax still applies.</span></span> <span data-ttu-id="0eb22-156">È possibile utilizzare `dotnet ef --help` per informazioni sulla sintassi.</span><span class="sxs-lookup"><span data-stu-id="0eb22-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="0eb22-157">Il modo in cui sono registrati i comandi è stato modificato in RC2, a causa di DNX verrà sostituito da .NET CLI.</span><span class="sxs-lookup"><span data-stu-id="0eb22-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="0eb22-158">I comandi sono ora registrati un `tools` sezione `project.json`:</span><span class="sxs-lookup"><span data-stu-id="0eb22-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> <span data-ttu-id="0eb22-159">Se si utilizza Visual Studio, è ora possibile utilizzare Console di gestione pacchetti per eseguire i comandi di Entity Framework per i progetti ASP.NET Core (ciò non è supportato nella versione RC1).</span><span class="sxs-lookup"><span data-stu-id="0eb22-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="0eb22-160">È necessario registrare i comandi di `tools` sezione di `project.json` per eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="0eb22-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="0eb22-161">I comandi di gestione pacchetti richiedono PowerShell 5</span><span class="sxs-lookup"><span data-stu-id="0eb22-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="0eb22-162">Se si utilizzano i comandi di Entity Framework nella Console di gestione pacchetti in Visual Studio, è necessario verificare di che aver installato PowerShell 5.</span><span class="sxs-lookup"><span data-stu-id="0eb22-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="0eb22-163">Questo è un requisito temporaneo che verrà rimossa nella versione successiva (vedere [emettere &#5327;](https://github.com/aspnet/EntityFramework/issues/5327) per altri dettagli).</span><span class="sxs-lookup"><span data-stu-id="0eb22-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="0eb22-164">Utilizzo di "Importa" in Project</span><span class="sxs-lookup"><span data-stu-id="0eb22-164">Using "imports" in project.json</span></span>

<span data-ttu-id="0eb22-165">Alcune delle dipendenze del Core EF non supporta .NET Standard ancora.</span><span class="sxs-lookup"><span data-stu-id="0eb22-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="0eb22-166">Core EF nei progetti .NET Standard e .NET Core potrebbe essere necessario aggiungere "Importa" per JSON come soluzione alternativa temporanea.</span><span class="sxs-lookup"><span data-stu-id="0eb22-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="0eb22-167">Quando si aggiungono EF, ripristino NuGet visualizzerà questo messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="0eb22-167">When adding EF, NuGet restore will display this error message:</span></span>

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

<span data-ttu-id="0eb22-168">La soluzione alternativa consiste nell'importare manualmente il profilo portabile "portabile net451 + win8".</span><span class="sxs-lookup"><span data-stu-id="0eb22-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="0eb22-169">Questa forza NuGet da trattare questo file binari che corrispondono a questo fornita come un framework compatibile con .NET Standard, anche se non lo siano.</span><span class="sxs-lookup"><span data-stu-id="0eb22-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="0eb22-170">Sebbene "portabile net451 + win8" non è compatibile con .NET Standard al 100%, è sufficiente compatibile per la transizione da libreria di classi Portabile per .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="0eb22-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="0eb22-171">Le importazioni possono essere rimossi quando le dipendenze di EF infine l'aggiornamento a .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="0eb22-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="0eb22-172">È possibile aggiungere più Framework di "Importa" nella sintassi di matrice.</span><span class="sxs-lookup"><span data-stu-id="0eb22-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="0eb22-173">Altre importazioni potrebbero essere necessari se si aggiungono librerie aggiuntive al progetto.</span><span class="sxs-lookup"><span data-stu-id="0eb22-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="0eb22-174">Vedere [emettere #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="0eb22-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
