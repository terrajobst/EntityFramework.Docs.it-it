---
title: L'aggiornamento da EF Core 1.0 RC1 a RC2 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 83b98fda5ac9491994b5b3fb333c9951ec01188a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996897"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="8d514-102">L'aggiornamento da EF Core 1.0 RC1 a 1.0 RC2</span><span class="sxs-lookup"><span data-stu-id="8d514-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="8d514-103">Questo articolo fornisce indicazioni per lo spostamento di un'applicazione compilata con i pacchetti RC1 a RC2.</span><span class="sxs-lookup"><span data-stu-id="8d514-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="8d514-104">Versioni e i nomi dei pacchetti</span><span class="sxs-lookup"><span data-stu-id="8d514-104">Package Names and Versions</span></span>

<span data-ttu-id="8d514-105">Confronto tra RC1 e RC2, è stato modificato da "Entity Framework 7" a "Entity Framework Core".</span><span class="sxs-lookup"><span data-stu-id="8d514-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="8d514-106">È possibile leggere altre informazioni sui motivi per la modifica in [questo post di Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="8d514-106">You can read more about the reasons for the change in [this post by Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="8d514-107">Grazie a questa modifica, i nomi di pacchetto modificato da `EntityFramework.*` al `Microsoft.EntityFrameworkCore.*` e la versioni da `7.0.0-rc1-final` al `1.0.0-rc2-final` (o `1.0.0-preview1-final` per gli strumenti).</span><span class="sxs-lookup"><span data-stu-id="8d514-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="8d514-108">**Sarà necessario rimuovere completamente i pacchetti RC1 e quindi installare il RC2 quelle.**</span><span class="sxs-lookup"><span data-stu-id="8d514-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="8d514-109">Di seguito è riportato il mapping per alcuni pacchetti comuni.</span><span class="sxs-lookup"><span data-stu-id="8d514-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="8d514-110">Pacchetto di RC1</span><span class="sxs-lookup"><span data-stu-id="8d514-110">RC1 Package</span></span>                                               | <span data-ttu-id="8d514-111">Equivalente di RC2</span><span class="sxs-lookup"><span data-stu-id="8d514-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="8d514-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="8d514-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="8d514-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="8d514-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="8d514-114">EntityFramework.SQLite                    7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="8d514-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="8d514-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="8d514-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="8d514-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="8d514-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="8d514-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span><span class="sxs-lookup"><span data-stu-id="8d514-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="8d514-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="8d514-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="8d514-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="8d514-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="8d514-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="8d514-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="8d514-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="8d514-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="8d514-122">EntityFramework.InMemory 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="8d514-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="8d514-123">Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="8d514-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="8d514-124">EntityFramework.IBMDataServer             7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="8d514-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="8d514-125">Non è ancora disponibile per RC2</span><span class="sxs-lookup"><span data-stu-id="8d514-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="8d514-126">EntityFramework.Commands 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="8d514-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="8d514-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span><span class="sxs-lookup"><span data-stu-id="8d514-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="8d514-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="8d514-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="8d514-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="8d514-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="8d514-130">Spazi dei nomi</span><span class="sxs-lookup"><span data-stu-id="8d514-130">Namespaces</span></span>

<span data-ttu-id="8d514-131">Insieme ai nomi di pacchetto, gli spazi dei nomi è stato modificato da `Microsoft.Data.Entity.*` a `Microsoft.EntityFrameworkCore.*`.</span><span class="sxs-lookup"><span data-stu-id="8d514-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="8d514-132">È possibile gestire questa modifica con una ricerca e sostituzione di `using Microsoft.Data.Entity` con `using Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="8d514-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="8d514-133">Tabella delle modifiche di convenzione di denominazione</span><span class="sxs-lookup"><span data-stu-id="8d514-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="8d514-134">Una modifica significativa funzionale è stato scelto in RC2 è stato per utilizzare il nome del `DbSet<TEntity>` proprietà per un'entità specificata come nome della tabella viene eseguito il mapping, anziché solo il nome della classe.</span><span class="sxs-lookup"><span data-stu-id="8d514-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="8d514-135">Altre informazioni su questa modifica in [il problema correlato annuncio](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="8d514-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="8d514-136">Per le applicazioni esistenti RC1, è consigliabile aggiungere il codice seguente all'inizio del `OnModelCreating` metodo per mantenere la strategia di denominazione RC1:</span><span class="sxs-lookup"><span data-stu-id="8d514-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="8d514-137">Se si desidera adottare il nuova strategia di denominazione, è consigliabile correttamente completare il resto dei passaggi di aggiornamento quindi il codice di rimozione e la creazione di una migrazione per applicare la tabella di Rinomina.</span><span class="sxs-lookup"><span data-stu-id="8d514-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="8d514-138">AddDbContext / Startup.cs cambia (solo progetti ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="8d514-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="8d514-139">Nella versione RC1, era necessario aggiungere servizi di Entity Framework per il provider di servizi di applicazione - in `Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="8d514-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="8d514-140">In RC2, è possibile rimuovere le chiamate a `AddEntityFramework()`, `AddSqlServer()`e così via.:</span><span class="sxs-lookup"><span data-stu-id="8d514-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="8d514-141">È anche necessario aggiungere un costruttore, del contesto derivato, che accetta opzioni del contesto e li passa al costruttore di base.</span><span class="sxs-lookup"><span data-stu-id="8d514-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="8d514-142">Ciò è necessario perché sono state rimosse alcune della magia scary che li in inciampato dietro le quinte:</span><span class="sxs-lookup"><span data-stu-id="8d514-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="8d514-143">Passando IServiceProvider</span><span class="sxs-lookup"><span data-stu-id="8d514-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="8d514-144">Se si dispone di codice RC1 che passa un' `IServiceProvider` nel contesto, questa è stata spostata in `DbContextOptions`, non è un parametro del costruttore separato.</span><span class="sxs-lookup"><span data-stu-id="8d514-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="8d514-145">Usare `DbContextOptionsBuilder.UseInternalServiceProvider(...)` per impostare il provider del servizio.</span><span class="sxs-lookup"><span data-stu-id="8d514-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="8d514-146">Test</span><span class="sxs-lookup"><span data-stu-id="8d514-146">Testing</span></span>

<span data-ttu-id="8d514-147">Lo scenario più comune per eseguire questa operazione è stata per controllare l'ambito di un database InMemory durante il test.</span><span class="sxs-lookup"><span data-stu-id="8d514-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="8d514-148">Vedere aggiornato [Testing](testing/index.md) per un esempio di questa operazione con RC2.</span><span class="sxs-lookup"><span data-stu-id="8d514-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="8d514-149">La risoluzione dei servizi interni dal Provider di servizi di applicazione (solo progetti ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="8d514-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="8d514-150">Se si dispone di un'applicazione ASP.NET Core e si desidera risolvere i servizi interni dal provider del servizio dell'applicazione a Entity Framework, si verifica un overload di `AddDbContext` che consente di configurare questa impostazione:</span><span class="sxs-lookup"><span data-stu-id="8d514-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> <span data-ttu-id="8d514-151">È consigliabile consentire Entity Framework gestire internamente dai propri servizi, a meno che non esista un motivo per combinare i servizi di Entity Framework interni in provider di servizi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8d514-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="8d514-152">Il motivo principale che è possibile eseguire questa operazione consiste nell'usare il provider di servizi di applicazione per sostituire i servizi usati da EF internamente</span><span class="sxs-lookup"><span data-stu-id="8d514-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="8d514-153">I comandi DNX = > CLI di .NET (solo progetti ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="8d514-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="8d514-154">Se è stato usato in precedenza il `dnx ef` comandi per i progetti ASP.NET 5, questi sono state spostate `dotnet ef` comandi.</span><span class="sxs-lookup"><span data-stu-id="8d514-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="8d514-155">Si applica comunque la stessa sintassi del comando.</span><span class="sxs-lookup"><span data-stu-id="8d514-155">The same command syntax still applies.</span></span> <span data-ttu-id="8d514-156">È possibile usare `dotnet ef --help` per informazioni sulla sintassi.</span><span class="sxs-lookup"><span data-stu-id="8d514-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="8d514-157">La modalità di registrazione comandi è stato modificato in RC2, a causa di DNX sostituita dall'interfaccia CLI di .NET.</span><span class="sxs-lookup"><span data-stu-id="8d514-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="8d514-158">I comandi sono ora registrati un `tools` sezione `project.json`:</span><span class="sxs-lookup"><span data-stu-id="8d514-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

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
> <span data-ttu-id="8d514-159">Se si usa Visual Studio, è ora possibile usare Console di gestione pacchetti per eseguire i comandi di Entity Framework per progetti ASP.NET Core (ciò non è supportato nella versione RC1).</span><span class="sxs-lookup"><span data-stu-id="8d514-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="8d514-160">È comunque necessario registrare i comandi nel `tools` sezione del `project.json` per eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="8d514-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="8d514-161">I comandi di gestione pacchetti è necessario PowerShell 5</span><span class="sxs-lookup"><span data-stu-id="8d514-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="8d514-162">Se si usano i comandi di Entity Framework nella Console di gestione pacchetti in Visual Studio, è necessario assicurarsi di che aver installato PowerShell 5.</span><span class="sxs-lookup"><span data-stu-id="8d514-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="8d514-163">Questo è un requisito temporaneo che verrà rimossa nella prossima versione (vedere [problema #5327](https://github.com/aspnet/EntityFramework/issues/5327) per altri dettagli).</span><span class="sxs-lookup"><span data-stu-id="8d514-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="8d514-164">Utilizzo di "imports" in Project. JSON</span><span class="sxs-lookup"><span data-stu-id="8d514-164">Using "imports" in project.json</span></span>

<span data-ttu-id="8d514-165">Alcune delle dipendenze di EF Core non supportano .NET Standard ancora.</span><span class="sxs-lookup"><span data-stu-id="8d514-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="8d514-166">EF Core nei progetti .NET Standard e .NET Core può essere necessario aggiungere "imports" al file Project. JSON come soluzione alternativa temporanea.</span><span class="sxs-lookup"><span data-stu-id="8d514-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="8d514-167">Durante l'aggiunta di Entity Framework, il ripristino di NuGet visualizzerà questo messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="8d514-167">When adding EF, NuGet restore will display this error message:</span></span>

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

<span data-ttu-id="8d514-168">La soluzione alternativa consiste nell'importare manualmente il profilo portatile "portabile-net451 + win8".</span><span class="sxs-lookup"><span data-stu-id="8d514-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="8d514-169">Questo NuGet trattare questo file binari che corrispondono a ciò impone offrono come framework compatibili con .NET Standard, anche se non sono.</span><span class="sxs-lookup"><span data-stu-id="8d514-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="8d514-170">Sebbene "portabile-net451 + win8" non è compatibile con .NET Standard al 100%, è sufficientemente compatibile per la transizione dalla libreria di classi Portabile di .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="8d514-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="8d514-171">Le importazioni possono essere rimosse quando le dipendenze di Entity Framework esegue infine l'aggiornamento a .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="8d514-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="8d514-172">È possibile aggiungere più Framework di "imports" nella sintassi di matrice.</span><span class="sxs-lookup"><span data-stu-id="8d514-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="8d514-173">Altre importazioni potrebbero essere necessari se si aggiungono altre librerie al progetto.</span><span class="sxs-lookup"><span data-stu-id="8d514-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="8d514-174">Visualizzare [problema #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="8d514-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
