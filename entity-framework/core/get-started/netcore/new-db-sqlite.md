---
title: Guida introduttiva a .NET Core - Nuovo database - EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Introduzione a .NET Core con Entity Framework Core
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 06/05/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: e4eafed037325237345efbc3d7d42b32270a54e3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911502"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="4e0a2-104">Introduzione a EF Core nell'app console .NET Core con un nuovo database</span><span class="sxs-lookup"><span data-stu-id="4e0a2-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="4e0a2-105">In questa procedura dettagliata viene creata un'app console .NET Core che esegue l'accesso ai dati in un database SQLite usando Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-105">In this walkthrough, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="4e0a2-106">Vengono usate le migrazioni per creare il database dal modello.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-106">You use migrations to create the database from the model.</span></span> <span data-ttu-id="4e0a2-107">Per una versione di Visual Studio che usa ASP.NET Core MVC, vedere [ASP.NET Core - Nuovo database](xref:core/get-started/aspnetcore/new-db).</span><span class="sxs-lookup"><span data-stu-id="4e0a2-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!TIP]  
> <span data-ttu-id="4e0a2-108">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4e0a2-109">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="4e0a2-109">Prerequisites</span></span>

<span data-ttu-id="4e0a2-110">[.NET Core SDK](https://www.microsoft.com/net/core) 2.1</span><span class="sxs-lookup"><span data-stu-id="4e0a2-110">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.1</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="4e0a2-111">Creare un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="4e0a2-111">Create a new project</span></span>

* <span data-ttu-id="4e0a2-112">Creare un nuovo progetto console:</span><span class="sxs-lookup"><span data-stu-id="4e0a2-112">Create a new console project:</span></span>

``` Console
dotnet new console -o ConsoleApp.SQLite
cd ConsoleApp.SQLite/
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="4e0a2-113">Installare Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="4e0a2-113">Install Entity Framework Core</span></span>

<span data-ttu-id="4e0a2-114">Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-114">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="4e0a2-115">Questa procedura dettagliata usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-115">This walkthrough uses SQLite.</span></span> <span data-ttu-id="4e0a2-116">Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="4e0a2-116">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="4e0a2-117">Installare Microsoft.EntityFrameworkCore.Sqlite e Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="4e0a2-117">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="4e0a2-118">Eseguire `dotnet restore` per installare i nuovi pacchetti.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-118">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="4e0a2-119">Creare il modello</span><span class="sxs-lookup"><span data-stu-id="4e0a2-119">Create the model</span></span>

<span data-ttu-id="4e0a2-120">Definire un contesto e le classi di entità che costituiscono il modello.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-120">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="4e0a2-121">Creare un nuovo file *Model.cs* con i contenuti seguenti.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-121">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="4e0a2-122">Suggerimento: in un'applicazione reale si inserisce ogni classe in un file separato e la stringa di connessione in un file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-122">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="4e0a2-123">Per semplicità, in questa esercitazione tutto è contenuto in un solo file.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-123">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="4e0a2-124">Creare il database</span><span class="sxs-lookup"><span data-stu-id="4e0a2-124">Create the database</span></span>

<span data-ttu-id="4e0a2-125">Dopo avere creato un modello,usare le [migrazioni](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) per creare un database.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-125">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="4e0a2-126">Eseguire `dotnet ef migrations add InitialCreate` per effettuare lo scaffolding di una migrazione e creare il set iniziale di tabelle per il modello.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-126">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="4e0a2-127">Eseguire `dotnet ef database update` per applicare la nuova migrazione al database.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-127">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="4e0a2-128">Questo comando crea il database prima di applicare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-128">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="4e0a2-129">Il database SQLite *blogging.db*\* si trova nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-129">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="4e0a2-130">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="4e0a2-130">Use your model</span></span>

* <span data-ttu-id="4e0a2-131">Aprire *Program.cs* e sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4e0a2-131">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="4e0a2-132">Eseguire il test dell'app:</span><span class="sxs-lookup"><span data-stu-id="4e0a2-132">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="4e0a2-133">Un blog è salvato nel database e i dettagli di tutti i blog vengono visualizzati nella console.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-133">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="4e0a2-134">Modifica del modello:</span><span class="sxs-lookup"><span data-stu-id="4e0a2-134">Changing the model:</span></span>

- <span data-ttu-id="4e0a2-135">Se si apportano modifiche al modello, è possibile usare il comando `dotnet ef migrations add` per eseguire lo scaffolding di una nuova [migrazione](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) e poter apportare le corrispondenti modifiche dello schema al database.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-135">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="4e0a2-136">Dopo avere controllato il codice di scaffolding e avere apportato eventuali modifiche necessarie, è possibile usare il comando `dotnet ef database update` per applicare le modifiche al database.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-136">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="4e0a2-137">EF usa una tabella `__EFMigrationsHistory` nel database per tenere traccia delle migrazioni che sono già state applicate al database.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-137">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="4e0a2-138">SQLite non supporta tutte le migrazioni (modifiche dello schema) a causa di limitazioni in SQLite.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-138">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="4e0a2-139">Vedere [SQLite Limitations](../../providers/sqlite/limitations.md) (Limitazioni di SQLite).</span><span class="sxs-lookup"><span data-stu-id="4e0a2-139">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="4e0a2-140">Per una nuova fase di sviluppo, prendere in considerazione la possibilità di eliminare il database e di crearne uno nuovo invece di usare le migrazioni quando il modello viene modificato.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-140">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4e0a2-141">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="4e0a2-141">Additional Resources</span></span>

* <span data-ttu-id="4e0a2-142">[.NET Core - Nuovo database con SQLite](xref:core/get-started/netcore/new-db-sqlite): esercitazione su EF per la console multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="4e0a2-142">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="4e0a2-143">Introduzione ad ASP.NET Core MVC in Mac o Linux</span><span class="sxs-lookup"><span data-stu-id="4e0a2-143">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="4e0a2-144">Introduzione ad ASP.NET Core MVC con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4e0a2-144">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="4e0a2-145">Introduzione ad ASP.NET Core ed Entity Framework Core con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4e0a2-145">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
