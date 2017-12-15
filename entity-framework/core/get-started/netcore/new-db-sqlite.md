---
title: Guida introduttiva a .NET Core - Nuovo database - EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Introduzione a .NET Core con Entity Framework Core
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 22fc0446dee71dd0d2402b47d76cc8b7307fbe5f
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="c2b39-104">Introduzione a EF Core nell'app console .NET Core con un nuovo database</span><span class="sxs-lookup"><span data-stu-id="c2b39-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="c2b39-105">In questa procedura dettagliata verrà creata un'app console .NET Core che esegue l'accesso ai dati di base in un database SQLite usando Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c2b39-105">In this walkthrough, you will create a .NET Core console app that performs basic data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="c2b39-106">Si useranno le migrazioni per creare il database dal modello.</span><span class="sxs-lookup"><span data-stu-id="c2b39-106">You will use migrations to create the database from your model.</span></span> <span data-ttu-id="c2b39-107">Per una versione di Visual Studio che usa ASP.NET Core MVC, vedere [ASP.NET Core - Nuovo database](xref:core/get-started/aspnetcore/new-db).</span><span class="sxs-lookup"><span data-stu-id="c2b39-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!NOTE]  
> <span data-ttu-id="c2b39-108">[.NET Core SDK](https://www.microsoft.com/net/download/core) non supporta più `project.json` o Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="c2b39-108">The [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or Visual Studio 2015.</span></span> <span data-ttu-id="c2b39-109">È consigliabile [eseguire la migrazione da project.json a csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span><span class="sxs-lookup"><span data-stu-id="c2b39-109">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="c2b39-110">Se si usa Visual Studio, è consigliabile eseguire la migrazione a [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="c2b39-110">If you are using Visual Studio, we recommend you migrate to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

> [!TIP]  
> <span data-ttu-id="c2b39-111">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="c2b39-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c2b39-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c2b39-112">Prerequisites</span></span>

<span data-ttu-id="c2b39-113">Per completare la procedura dettagliata, sono necessari i prerequisiti seguenti:</span><span class="sxs-lookup"><span data-stu-id="c2b39-113">The following prerequisites are needed to complete this walkthrough:</span></span>
* <span data-ttu-id="c2b39-114">Sistema operativo che supporta .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c2b39-114">An operating system that supports .NET Core.</span></span>
* <span data-ttu-id="c2b39-115">[.NET Core SDK](https://www.microsoft.com/net/core) 2.0, anche se le istruzioni possono essere usate per creare un'applicazione con una versione precedente con pochissime modifiche.</span><span class="sxs-lookup"><span data-stu-id="c2b39-115">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.0 (although the instructions can be used to create an application with a previous version with very few modifications).</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="c2b39-116">Creare un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="c2b39-116">Create a new project</span></span>

* <span data-ttu-id="c2b39-117">Creare una nuova cartella `ConsoleApp.SQLite` per il progetto e usare il comando `dotnet` per popolarla con un 'app .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c2b39-117">Create a new `ConsoleApp.SQLite` folder for your project and use the `dotnet` command to populate it with a .NET Core app.</span></span>

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="c2b39-118">Installare Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c2b39-118">Install Entity Framework Core</span></span>

<span data-ttu-id="c2b39-119">Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione.</span><span class="sxs-lookup"><span data-stu-id="c2b39-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="c2b39-120">Questa procedura dettagliata usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="c2b39-120">This walkthrough uses SQLite.</span></span> <span data-ttu-id="c2b39-121">Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="c2b39-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="c2b39-122">Installare Microsoft.EntityFrameworkCore.Sqlite e Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="c2b39-122">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="c2b39-123">Modificare manualmente `ConsoleApp.SQLite.csproj` per aggiungere DotNetCliToolReference a Microsoft.EntityFrameworkCore.Tools.DotNet:</span><span class="sxs-lookup"><span data-stu-id="c2b39-123">Manually edit `ConsoleApp.SQLite.csproj` to add a DotNetCliToolReference to Microsoft.EntityFrameworkCore.Tools.DotNet:</span></span>

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

 <span data-ttu-id="c2b39-124">Nota: una versione futura di `dotnet` supporterà DotNetCliToolReferences tramite `dotnet add tool`</span><span class="sxs-lookup"><span data-stu-id="c2b39-124">Note: A future version of `dotnet` will support DotNetCliToolReferences via `dotnet add tool`</span></span>

<span data-ttu-id="c2b39-125">`ConsoleApp.SQLite.csproj` conterrà ora quanto segue:</span><span class="sxs-lookup"><span data-stu-id="c2b39-125">`ConsoleApp.SQLite.csproj` should now contain the following:</span></span>

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 <span data-ttu-id="c2b39-126">Nota: i numeri di versione usati sopra erano corretti al momento della pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="c2b39-126">Note: The version numbers used above were correct at the time of publishing.</span></span>

*  <span data-ttu-id="c2b39-127">Eseguire `dotnet restore` per installare i nuovi pacchetti.</span><span class="sxs-lookup"><span data-stu-id="c2b39-127">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="c2b39-128">Creare il modello</span><span class="sxs-lookup"><span data-stu-id="c2b39-128">Create the model</span></span>

<span data-ttu-id="c2b39-129">Definire un contesto e le classi di entità che costituiscono il modello.</span><span class="sxs-lookup"><span data-stu-id="c2b39-129">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="c2b39-130">Creare un nuovo file *Model.cs* con i contenuti seguenti.</span><span class="sxs-lookup"><span data-stu-id="c2b39-130">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="c2b39-131">Suggerimento: in una vera applicazione si inserisce ogni classe in un file separato e la stringa di connessione in un file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c2b39-131">Tip: In a real application you would put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="c2b39-132">Per semplicità, in questa esercitazione viene inserito tutto in un solo file.</span><span class="sxs-lookup"><span data-stu-id="c2b39-132">To keep the tutorial simple, we are putting everything in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="c2b39-133">Creare il database</span><span class="sxs-lookup"><span data-stu-id="c2b39-133">Create the database</span></span>

<span data-ttu-id="c2b39-134">Dopo avere creato un modello, è possibile usare le [migrazioni](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) per creare un database.</span><span class="sxs-lookup"><span data-stu-id="c2b39-134">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="c2b39-135">Eseguire `dotnet ef migrations add InitialCreate` per effettuare lo scaffolding di una migrazione e creare il set iniziale di tabelle per il modello.</span><span class="sxs-lookup"><span data-stu-id="c2b39-135">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="c2b39-136">Eseguire `dotnet ef database update` per applicare la nuova migrazione al database.</span><span class="sxs-lookup"><span data-stu-id="c2b39-136">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="c2b39-137">Questo comando crea il database prima di applicare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="c2b39-137">This command creates the database before applying migrations.</span></span>

> [!NOTE]  
> <span data-ttu-id="c2b39-138">Quando si usano i percorsi relativi con SQLite, il percorso sarà relativo all'assembly principale dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c2b39-138">When using relative paths with SQLite, the path will be relative to the application's main assembly.</span></span> <span data-ttu-id="c2b39-139">In questo esempio il file binario principale è `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, quindi il database SQLite sarà in `bin/Debug/netcoreapp2.0/blogging.db`.</span><span class="sxs-lookup"><span data-stu-id="c2b39-139">In this sample, the main binary is `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, so the SQLite database will be in `bin/Debug/netcoreapp2.0/blogging.db`.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="c2b39-140">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="c2b39-140">Use your model</span></span>

* <span data-ttu-id="c2b39-141">Aprire *Program.cs* e sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c2b39-141">Open *Program.cs* and replace the contents with the following code:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="c2b39-142">Eseguire il test dell'app:</span><span class="sxs-lookup"><span data-stu-id="c2b39-142">Test the app:</span></span>

 `dotnet run`

 <span data-ttu-id="c2b39-143">Un blog è salvato nel database e i dettagli di tutti i blog vengono visualizzati nella console.</span><span class="sxs-lookup"><span data-stu-id="c2b39-143">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="c2b39-144">Modifica del modello:</span><span class="sxs-lookup"><span data-stu-id="c2b39-144">Changing the model:</span></span>

- <span data-ttu-id="c2b39-145">Se si apportano modifiche al modello, è possibile usare il comando `dotnet ef migrations add` per eseguire lo scaffolding di una nuova [migrazione](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) e poter apportare le corrispondenti modifiche dello schema al database.</span><span class="sxs-lookup"><span data-stu-id="c2b39-145">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="c2b39-146">Dopo avere controllato il codice di scaffolding e avere apportato eventuali modifiche necessarie, è possibile usare il comando `dotnet ef database update` per applicare le modifiche al database.</span><span class="sxs-lookup"><span data-stu-id="c2b39-146">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="c2b39-147">EF usa una tabella `__EFMigrationsHistory` nel database per tenere traccia delle migrazioni che sono già state applicate al database.</span><span class="sxs-lookup"><span data-stu-id="c2b39-147">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="c2b39-148">SQLite non supporta tutte le migrazioni (modifiche dello schema) a causa di limitazioni in SQLite.</span><span class="sxs-lookup"><span data-stu-id="c2b39-148">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="c2b39-149">Vedere [SQLite Limitations](../../providers/sqlite/limitations.md) (Limitazioni di SQLite).</span><span class="sxs-lookup"><span data-stu-id="c2b39-149">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="c2b39-150">Per una nuova fase di sviluppo, prendere in considerazione la possibilità di eliminare il database e di crearne uno nuovo invece di usare le migrazioni quando il modello viene modificato.</span><span class="sxs-lookup"><span data-stu-id="c2b39-150">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c2b39-151">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c2b39-151">Additional Resources</span></span>

* <span data-ttu-id="c2b39-152">[.NET Core - Nuovo database con SQLite](xref:core/get-started/netcore/new-db-sqlite): esercitazione su EF per la console multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="c2b39-152">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="c2b39-153">Introduzione ad ASP.NET Core MVC in Mac o Linux</span><span class="sxs-lookup"><span data-stu-id="c2b39-153">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="c2b39-154">Introduzione ad ASP.NET Core MVC con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c2b39-154">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="c2b39-155">Introduzione ad ASP.NET Core ed Entity Framework Core con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c2b39-155">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
