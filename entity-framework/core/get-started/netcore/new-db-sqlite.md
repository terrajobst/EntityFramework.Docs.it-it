---
title: Guida introduttiva a .NET Core - Nuovo database - EF Core
author: rick-anderson
ms.author: riande
description: Introduzione a .NET Core con Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 51f5752eebce5603c663072f7b36dfecd4ddf227
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993692"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="fda99-103">Introduzione a EF Core nell'app console .NET Core con un nuovo database</span><span class="sxs-lookup"><span data-stu-id="fda99-103">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="fda99-104">In questa esercitazione viene creata un'app console .NET Core che esegue l'accesso ai dati in un database SQLite usando Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="fda99-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="fda99-105">Vengono usate le migrazioni per creare il database dal modello.</span><span class="sxs-lookup"><span data-stu-id="fda99-105">You use migrations to create the database from the model.</span></span> <span data-ttu-id="fda99-106">Per una versione di Visual Studio che usa ASP.NET Core MVC, vedere [ASP.NET Core - Nuovo database](xref:core/get-started/aspnetcore/new-db).</span><span class="sxs-lookup"><span data-stu-id="fda99-106">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

<span data-ttu-id="fda99-107">Visualizzare l'esempio di questo articolo su GitHub https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span><span class="sxs-lookup"><span data-stu-id="fda99-107">View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fda99-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="fda99-108">Prerequisites</span></span>

* [<span data-ttu-id="fda99-109">.NET Core 2.1 SDK</span><span class="sxs-lookup"><span data-stu-id="fda99-109">The .NET Core 2.1 SDK</span></span>](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a><span data-ttu-id="fda99-110">Creare un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="fda99-110">Create a new project</span></span>

* <span data-ttu-id="fda99-111">Creare un nuovo progetto console:</span><span class="sxs-lookup"><span data-stu-id="fda99-111">Create a new console project:</span></span>

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  cd ConsoleApp.SQLite/
  ```

## <a name="install-entity-framework-core"></a><span data-ttu-id="fda99-112">Installare Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="fda99-112">Install Entity Framework Core</span></span>

<span data-ttu-id="fda99-113">Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione.</span><span class="sxs-lookup"><span data-stu-id="fda99-113">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="fda99-114">Questa procedura dettagliata usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="fda99-114">This walkthrough uses SQLite.</span></span> <span data-ttu-id="fda99-115">Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="fda99-115">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="fda99-116">Installare Microsoft.EntityFrameworkCore.Sqlite e Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="fda99-116">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* <span data-ttu-id="fda99-117">Eseguire `dotnet restore` per installare i nuovi pacchetti.</span><span class="sxs-lookup"><span data-stu-id="fda99-117">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="fda99-118">Creare il modello</span><span class="sxs-lookup"><span data-stu-id="fda99-118">Create the model</span></span>

<span data-ttu-id="fda99-119">Definire un contesto e le classi di entità che costituiscono il modello.</span><span class="sxs-lookup"><span data-stu-id="fda99-119">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="fda99-120">Creare un nuovo file *Model.cs* con i contenuti seguenti.</span><span class="sxs-lookup"><span data-stu-id="fda99-120">Create a new *Model.cs* file with the following contents.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="fda99-121">Suggerimento: in una vera applicazione, ogni classe verrebbe inserita in un file separato e la stringa di connessione verrebbe posizionata in un file di configurazione o in una variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="fda99-121">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="fda99-122">Per semplicità, in questa esercitazione tutto è contenuto in un solo file.</span><span class="sxs-lookup"><span data-stu-id="fda99-122">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="fda99-123">Creare il database</span><span class="sxs-lookup"><span data-stu-id="fda99-123">Create the database</span></span>

<span data-ttu-id="fda99-124">Dopo avere creato un modello,usare le [migrazioni](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) per creare un database.</span><span class="sxs-lookup"><span data-stu-id="fda99-124">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="fda99-125">Eseguire `dotnet ef migrations add InitialCreate` per effettuare lo scaffolding di una migrazione e creare il set iniziale di tabelle per il modello.</span><span class="sxs-lookup"><span data-stu-id="fda99-125">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="fda99-126">Eseguire `dotnet ef database update` per applicare la nuova migrazione al database.</span><span class="sxs-lookup"><span data-stu-id="fda99-126">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="fda99-127">Questo comando crea il database prima di applicare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="fda99-127">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="fda99-128">Il database SQLite *blogging.db*\* si trova nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="fda99-128">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="fda99-129">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="fda99-129">Use the model</span></span>

* <span data-ttu-id="fda99-130">Aprire *Program.cs* e sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="fda99-130">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="fda99-131">Eseguire il test dell'app:</span><span class="sxs-lookup"><span data-stu-id="fda99-131">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="fda99-132">Un blog è salvato nel database e i dettagli di tutti i blog vengono visualizzati nella console.</span><span class="sxs-lookup"><span data-stu-id="fda99-132">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="fda99-133">Modifica del modello:</span><span class="sxs-lookup"><span data-stu-id="fda99-133">Changing the model:</span></span>

- <span data-ttu-id="fda99-134">Se si apportano modifiche al modello, è possibile usare il comando `dotnet ef migrations add` per eseguire lo scaffolding di una nuova [migrazione](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="fda99-134">If you make changes to the model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).</span></span> <span data-ttu-id="fda99-135">Dopo avere controllato il codice di scaffolding e avere apportato le eventuali modifiche necessarie, è possibile usare il comando `dotnet ef database update` per applicare le modifiche dello schema al database.</span><span class="sxs-lookup"><span data-stu-id="fda99-135">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the schema changes to the database.</span></span>
- <span data-ttu-id="fda99-136">EF Core usa una tabella `__EFMigrationsHistory` nel database per tenere traccia delle migrazioni che sono già state applicate al database.</span><span class="sxs-lookup"><span data-stu-id="fda99-136">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="fda99-137">Il motore di database SQLite non supporta determinate modifiche dello schema, supportate dalla maggior parte degli altri database relazionali.</span><span class="sxs-lookup"><span data-stu-id="fda99-137">The SQLite database engine doesn't support certain schema changes that are supported by most other relational databases.</span></span> <span data-ttu-id="fda99-138">Ad esempio l'operazione `DropColumn` non è supportata.</span><span class="sxs-lookup"><span data-stu-id="fda99-138">For example, the `DropColumn` operation is not supported.</span></span> <span data-ttu-id="fda99-139">Le migrazioni EF Core generano codice per queste operazioni.</span><span class="sxs-lookup"><span data-stu-id="fda99-139">EF Core Migrations will generate code for these operations.</span></span> <span data-ttu-id="fda99-140">Se tuttavia si prova ad applicarle a un database o di generare uno script, EF Core produce eccezioni.</span><span class="sxs-lookup"><span data-stu-id="fda99-140">But if you try to apply them to a database or generate a script, EF Core throws exceptions.</span></span> <span data-ttu-id="fda99-141">Vedere [SQLite Limitations](../../providers/sqlite/limitations.md) (Limitazioni di SQLite).</span><span class="sxs-lookup"><span data-stu-id="fda99-141">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="fda99-142">Per lo sviluppo di nuovo codice, prendere in considerazione la possibilità di eliminare il database e di crearne uno nuovo invece di usare le migrazioni quando il modello viene modificato.</span><span class="sxs-lookup"><span data-stu-id="fda99-142">For new development, consider dropping the database and creating a new one rather than using migrations when the model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fda99-143">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="fda99-143">Additional Resources</span></span>

* [<span data-ttu-id="fda99-144">Introduzione ad ASP.NET Core MVC in Mac o Linux</span><span class="sxs-lookup"><span data-stu-id="fda99-144">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="fda99-145">Introduzione ad ASP.NET Core MVC con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fda99-145">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="fda99-146">Introduzione ad ASP.NET Core ed Entity Framework Core con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fda99-146">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
