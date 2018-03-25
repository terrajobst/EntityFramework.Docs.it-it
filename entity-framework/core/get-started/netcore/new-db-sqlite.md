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
ms.openlocfilehash: 2511dfa3f3262bb12c2058dc1c402b7dcc4c670d
ms.sourcegitcommit: 90139dbd6f485473afda0788a5a314c9aa601ea0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/23/2018
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="c9bf1-104">Introduzione a EF Core nell'app console .NET Core con un nuovo database</span><span class="sxs-lookup"><span data-stu-id="c9bf1-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="c9bf1-105">In questa procedura dettagliata verrà creata un'app console .NET Core che esegue l'accesso ai dati di base in un database SQLite usando Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-105">In this walkthrough, you will create a .NET Core console app that performs basic data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="c9bf1-106">Si useranno le migrazioni per creare il database dal modello.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-106">You will use migrations to create the database from your model.</span></span> <span data-ttu-id="c9bf1-107">Per una versione di Visual Studio che usa ASP.NET Core MVC, vedere [ASP.NET Core - Nuovo database](xref:core/get-started/aspnetcore/new-db).</span><span class="sxs-lookup"><span data-stu-id="c9bf1-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!TIP]  
> <span data-ttu-id="c9bf1-108">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c9bf1-109">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c9bf1-109">Prerequisites</span></span>

<span data-ttu-id="c9bf1-110">Per completare la procedura dettagliata, sono necessari i prerequisiti seguenti:</span><span class="sxs-lookup"><span data-stu-id="c9bf1-110">The following prerequisites are needed to complete this walkthrough:</span></span>
* <span data-ttu-id="c9bf1-111">Sistema operativo che supporta .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-111">An operating system that supports .NET Core.</span></span>
* <span data-ttu-id="c9bf1-112">[.NET Core SDK](https://www.microsoft.com/net/core) 2.0, anche se le istruzioni possono essere usate per creare un'applicazione con una versione precedente con pochissime modifiche.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-112">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.0 (although the instructions can be used to create an application with a previous version with very few modifications).</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="c9bf1-113">Creare un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="c9bf1-113">Create a new project</span></span>

* <span data-ttu-id="c9bf1-114">Creare una nuova cartella `ConsoleApp.SQLite` per il progetto e usare il comando `dotnet` per popolarla con un 'app .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-114">Create a new `ConsoleApp.SQLite` folder for your project and use the `dotnet` command to populate it with a .NET Core app.</span></span>

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="c9bf1-115">Installare Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c9bf1-115">Install Entity Framework Core</span></span>

<span data-ttu-id="c9bf1-116">Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="c9bf1-117">Questa procedura dettagliata usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-117">This walkthrough uses SQLite.</span></span> <span data-ttu-id="c9bf1-118">Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="c9bf1-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="c9bf1-119">Installare Microsoft.EntityFrameworkCore.Sqlite e Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="c9bf1-119">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="c9bf1-120">Modificare manualmente `ConsoleApp.SQLite.csproj` per aggiungere DotNetCliToolReference a Microsoft.EntityFrameworkCore.Tools.DotNet:</span><span class="sxs-lookup"><span data-stu-id="c9bf1-120">Manually edit `ConsoleApp.SQLite.csproj` to add a DotNetCliToolReference to Microsoft.EntityFrameworkCore.Tools.DotNet:</span></span>

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

<span data-ttu-id="c9bf1-121">`ConsoleApp.SQLite.csproj` conterrà ora quanto segue:</span><span class="sxs-lookup"><span data-stu-id="c9bf1-121">`ConsoleApp.SQLite.csproj` should now contain the following:</span></span>

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 <span data-ttu-id="c9bf1-122">Nota: i numeri di versione usati sopra erano corretti al momento della pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-122">Note: The version numbers used above were correct at the time of publishing.</span></span>

*  <span data-ttu-id="c9bf1-123">Eseguire `dotnet restore` per installare i nuovi pacchetti.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-123">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="c9bf1-124">Creare il modello</span><span class="sxs-lookup"><span data-stu-id="c9bf1-124">Create the model</span></span>

<span data-ttu-id="c9bf1-125">Definire un contesto e le classi di entità che costituiscono il modello.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-125">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="c9bf1-126">Creare un nuovo file *Model.cs* con i contenuti seguenti.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-126">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="c9bf1-127">Suggerimento: in una vera applicazione si inserisce ogni classe in un file separato e la stringa di connessione in un file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-127">Tip: In a real application you would put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="c9bf1-128">Per semplicità, in questa esercitazione viene inserito tutto in un solo file.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-128">To keep the tutorial simple, we are putting everything in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="c9bf1-129">Creare il database</span><span class="sxs-lookup"><span data-stu-id="c9bf1-129">Create the database</span></span>

<span data-ttu-id="c9bf1-130">Dopo avere creato un modello, è possibile usare le [migrazioni](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) per creare un database.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-130">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="c9bf1-131">Eseguire `dotnet ef migrations add InitialCreate` per effettuare lo scaffolding di una migrazione e creare il set iniziale di tabelle per il modello.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-131">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="c9bf1-132">Eseguire `dotnet ef database update` per applicare la nuova migrazione al database.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-132">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="c9bf1-133">Questo comando crea il database prima di applicare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-133">This command creates the database before applying migrations.</span></span>

> [!NOTE]  
> <span data-ttu-id="c9bf1-134">Quando si usano i percorsi relativi con SQLite, il percorso sarà relativo all'assembly principale dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-134">When using relative paths with SQLite, the path will be relative to the application's main assembly.</span></span> <span data-ttu-id="c9bf1-135">In questo esempio il file binario principale è `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, quindi il database SQLite sarà in `bin/Debug/netcoreapp2.0/blogging.db`.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-135">In this sample, the main binary is `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, so the SQLite database will be in `bin/Debug/netcoreapp2.0/blogging.db`.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="c9bf1-136">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="c9bf1-136">Use your model</span></span>

* <span data-ttu-id="c9bf1-137">Aprire *Program.cs* e sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c9bf1-137">Open *Program.cs* and replace the contents with the following code:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="c9bf1-138">Eseguire il test dell'app:</span><span class="sxs-lookup"><span data-stu-id="c9bf1-138">Test the app:</span></span>

 `dotnet run`

 <span data-ttu-id="c9bf1-139">Un blog è salvato nel database e i dettagli di tutti i blog vengono visualizzati nella console.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-139">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="c9bf1-140">Modifica del modello:</span><span class="sxs-lookup"><span data-stu-id="c9bf1-140">Changing the model:</span></span>

- <span data-ttu-id="c9bf1-141">Se si apportano modifiche al modello, è possibile usare il comando `dotnet ef migrations add` per eseguire lo scaffolding di una nuova [migrazione](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) e poter apportare le corrispondenti modifiche dello schema al database.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-141">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="c9bf1-142">Dopo avere controllato il codice di scaffolding e avere apportato eventuali modifiche necessarie, è possibile usare il comando `dotnet ef database update` per applicare le modifiche al database.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-142">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="c9bf1-143">EF usa una tabella `__EFMigrationsHistory` nel database per tenere traccia delle migrazioni che sono già state applicate al database.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-143">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="c9bf1-144">SQLite non supporta tutte le migrazioni (modifiche dello schema) a causa di limitazioni in SQLite.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-144">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="c9bf1-145">Vedere [SQLite Limitations](../../providers/sqlite/limitations.md) (Limitazioni di SQLite).</span><span class="sxs-lookup"><span data-stu-id="c9bf1-145">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="c9bf1-146">Per una nuova fase di sviluppo, prendere in considerazione la possibilità di eliminare il database e di crearne uno nuovo invece di usare le migrazioni quando il modello viene modificato.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-146">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c9bf1-147">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c9bf1-147">Additional Resources</span></span>

* <span data-ttu-id="c9bf1-148">[.NET Core - Nuovo database con SQLite](xref:core/get-started/netcore/new-db-sqlite): esercitazione su EF per la console multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="c9bf1-148">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="c9bf1-149">Introduzione ad ASP.NET Core MVC in Mac o Linux</span><span class="sxs-lookup"><span data-stu-id="c9bf1-149">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="c9bf1-150">Introduzione ad ASP.NET Core MVC con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c9bf1-150">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="c9bf1-151">Introduzione ad ASP.NET Core ed Entity Framework Core con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c9bf1-151">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
