---
title: Guida introduttiva a .NET Core - Nuovo database - EF Core
author: rick-anderson
ms.author: riande
description: Introduzione a .NET Core con Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: ec20040917a2bca8177924b6905b1cd79e5cd9da
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834735"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="7f05f-103">Introduzione a EF Core nell'app console .NET Core con un nuovo database</span><span class="sxs-lookup"><span data-stu-id="7f05f-103">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="7f05f-104">In questa esercitazione viene creata un'app console .NET Core che esegue l'accesso ai dati in un database SQLite usando Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7f05f-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="7f05f-105">Vengono usate le migrazioni per creare il database dal modello.</span><span class="sxs-lookup"><span data-stu-id="7f05f-105">You use migrations to create the database from the model.</span></span> <span data-ttu-id="7f05f-106">Per una versione di Visual Studio che usa ASP.NET Core MVC, vedere [ASP.NET Core - Nuovo database](xref:core/get-started/aspnetcore/new-db).</span><span class="sxs-lookup"><span data-stu-id="7f05f-106">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

<span data-ttu-id="7f05f-107">Visualizzare l'esempio di questo articolo su GitHub https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span><span class="sxs-lookup"><span data-stu-id="7f05f-107">View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7f05f-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="7f05f-108">Prerequisites</span></span>

* [<span data-ttu-id="7f05f-109">.NET Core 2.1 SDK</span><span class="sxs-lookup"><span data-stu-id="7f05f-109">The .NET Core 2.1 SDK</span></span>](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a><span data-ttu-id="7f05f-110">Creare un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="7f05f-110">Create a new project</span></span>

* <span data-ttu-id="7f05f-111">Creare un nuovo progetto console:</span><span class="sxs-lookup"><span data-stu-id="7f05f-111">Create a new console project:</span></span>

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a><span data-ttu-id="7f05f-112">Selezionare la directory corrente</span><span class="sxs-lookup"><span data-stu-id="7f05f-112">Change the current directory</span></span>

<span data-ttu-id="7f05f-113">Nei passaggi successivi è necessario eseguire i comandi `dotnet` nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7f05f-113">In subsequent steps, we need to issue `dotnet` commands against the application.</span></span>

* <span data-ttu-id="7f05f-114">La directory corrente viene modificata nella directory dell'applicazione, come di seguito riportato:</span><span class="sxs-lookup"><span data-stu-id="7f05f-114">We change the current directory to the application's directory like this:</span></span>

  ``` Console
  cd ConsoleApp.SQLite/
  ```
## <a name="install-entity-framework-core"></a><span data-ttu-id="7f05f-115">Installare Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="7f05f-115">Install Entity Framework Core</span></span>

<span data-ttu-id="7f05f-116">Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione.</span><span class="sxs-lookup"><span data-stu-id="7f05f-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="7f05f-117">Questa procedura dettagliata usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="7f05f-117">This walkthrough uses SQLite.</span></span> <span data-ttu-id="7f05f-118">Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="7f05f-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="7f05f-119">Installare Microsoft.EntityFrameworkCore.Sqlite e Microsoft.EntityFrameworkCore.Design</span><span class="sxs-lookup"><span data-stu-id="7f05f-119">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* <span data-ttu-id="7f05f-120">Eseguire `dotnet restore` per installare i nuovi pacchetti.</span><span class="sxs-lookup"><span data-stu-id="7f05f-120">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="7f05f-121">Creare il modello</span><span class="sxs-lookup"><span data-stu-id="7f05f-121">Create the model</span></span>

<span data-ttu-id="7f05f-122">Definire un contesto e le classi di entità che costituiscono il modello.</span><span class="sxs-lookup"><span data-stu-id="7f05f-122">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="7f05f-123">Creare un nuovo file *Model.cs* con i contenuti seguenti.</span><span class="sxs-lookup"><span data-stu-id="7f05f-123">Create a new *Model.cs* file with the following contents.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="7f05f-124">Suggerimento: in una vera applicazione, ogni classe verrebbe inserita in un file separato e la stringa di connessione verrebbe posizionata in un file di configurazione o in una variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="7f05f-124">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="7f05f-125">Per semplicità, in questa esercitazione tutto è contenuto in un solo file.</span><span class="sxs-lookup"><span data-stu-id="7f05f-125">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="7f05f-126">Creare il database</span><span class="sxs-lookup"><span data-stu-id="7f05f-126">Create the database</span></span>

<span data-ttu-id="7f05f-127">Dopo avere creato un modello,usare le [migrazioni](xref:core/managing-schemas/migrations/index) per creare un database.</span><span class="sxs-lookup"><span data-stu-id="7f05f-127">Once you have a model, you use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

* <span data-ttu-id="7f05f-128">Eseguire `dotnet ef migrations add InitialCreate` per effettuare lo scaffolding di una migrazione e creare il set iniziale di tabelle per il modello.</span><span class="sxs-lookup"><span data-stu-id="7f05f-128">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="7f05f-129">Eseguire `dotnet ef database update` per applicare la nuova migrazione al database.</span><span class="sxs-lookup"><span data-stu-id="7f05f-129">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="7f05f-130">Questo comando crea il database prima di applicare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="7f05f-130">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="7f05f-131">Il database SQLite *blogging.db*\* si trova nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="7f05f-131">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="7f05f-132">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="7f05f-132">Use the model</span></span>

* <span data-ttu-id="7f05f-133">Aprire *Program.cs* e sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7f05f-133">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="7f05f-134">Testare l'app dalla console.</span><span class="sxs-lookup"><span data-stu-id="7f05f-134">Test the app from the console.</span></span> <span data-ttu-id="7f05f-135">Vedere la [nota di Visual Studio](#vs) per eseguire l'app da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7f05f-135">See the [Visual Studio note](#vs) to run the app from Visual Studio.</span></span>

  `dotnet run`

  <span data-ttu-id="7f05f-136">Un blog è salvato nel database e i dettagli di tutti i blog vengono visualizzati nella console.</span><span class="sxs-lookup"><span data-stu-id="7f05f-136">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="7f05f-137">Modifica del modello:</span><span class="sxs-lookup"><span data-stu-id="7f05f-137">Changing the model:</span></span>

- <span data-ttu-id="7f05f-138">Se si apportano modifiche al modello, è possibile usare il comando `dotnet ef migrations add` per eseguire lo scaffolding di una nuova [migrazione](xref:core/managing-schemas/migrations/index).</span><span class="sxs-lookup"><span data-stu-id="7f05f-138">If you make changes to the model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](xref:core/managing-schemas/migrations/index).</span></span> <span data-ttu-id="7f05f-139">Dopo avere controllato il codice di scaffolding e avere apportato le eventuali modifiche necessarie, è possibile usare il comando `dotnet ef database update` per applicare le modifiche dello schema al database.</span><span class="sxs-lookup"><span data-stu-id="7f05f-139">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the schema changes to the database.</span></span>
- <span data-ttu-id="7f05f-140">EF Core usa una tabella `__EFMigrationsHistory` nel database per tenere traccia delle migrazioni che sono già state applicate al database.</span><span class="sxs-lookup"><span data-stu-id="7f05f-140">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="7f05f-141">Il motore di database SQLite non supporta determinate modifiche dello schema, supportate dalla maggior parte degli altri database relazionali.</span><span class="sxs-lookup"><span data-stu-id="7f05f-141">The SQLite database engine doesn't support certain schema changes that are supported by most other relational databases.</span></span> <span data-ttu-id="7f05f-142">Ad esempio l'operazione `DropColumn` non è supportata.</span><span class="sxs-lookup"><span data-stu-id="7f05f-142">For example, the `DropColumn` operation is not supported.</span></span> <span data-ttu-id="7f05f-143">Le migrazioni EF Core generano codice per queste operazioni.</span><span class="sxs-lookup"><span data-stu-id="7f05f-143">EF Core Migrations will generate code for these operations.</span></span> <span data-ttu-id="7f05f-144">Se tuttavia si prova ad applicarle a un database o di generare uno script, EF Core produce eccezioni.</span><span class="sxs-lookup"><span data-stu-id="7f05f-144">But if you try to apply them to a database or generate a script, EF Core throws exceptions.</span></span> <span data-ttu-id="7f05f-145">Vedere [SQLite Limitations](../../providers/sqlite/limitations.md) (Limitazioni di SQLite).</span><span class="sxs-lookup"><span data-stu-id="7f05f-145">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="7f05f-146">Per lo sviluppo di nuovo codice, prendere in considerazione la possibilità di eliminare il database e di crearne uno nuovo invece di usare le migrazioni quando il modello viene modificato.</span><span class="sxs-lookup"><span data-stu-id="7f05f-146">For new development, consider dropping the database and creating a new one rather than using migrations when the model changes.</span></span>

<a name="vs"></a>
### <a name="run-from-visual-studio"></a><span data-ttu-id="7f05f-147">Eseguire da Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f05f-147">Run from Visual Studio</span></span>

<span data-ttu-id="7f05f-148">Per eseguire questo esempio da Visual Studio, è necessario impostare la directory di lavoro manualmente in modo che diventi la radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="7f05f-148">To run this sample from Visual Studio, you must set the working directory manually to be the root of the project.</span></span> <span data-ttu-id="7f05f-149">Se la directory di lavoro non viene impostata, viene generata l'eccezione `Microsoft.Data.Sqlite.SqliteException` seguente: `SQLite Error 1: 'no such table: Blogs'`.</span><span class="sxs-lookup"><span data-stu-id="7f05f-149">If  you don't set the working directory, the following `Microsoft.Data.Sqlite.SqliteException` is thrown: `SQLite Error 1: 'no such table: Blogs'`.</span></span>

<span data-ttu-id="7f05f-150">Per impostare la directory di lavoro:</span><span class="sxs-lookup"><span data-stu-id="7f05f-150">To set the working directory:</span></span>

* <span data-ttu-id="7f05f-151">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="7f05f-151">In **Solution Explorer**, right click the project and then select **Properties**.</span></span>
* <span data-ttu-id="7f05f-152">Selezionare la scheda **Debug** nel riquadro sinistro.</span><span class="sxs-lookup"><span data-stu-id="7f05f-152">Select the **Debug** tab in the left pane.</span></span>
* <span data-ttu-id="7f05f-153">Impostare la **directory di lavoro** sul percorso del progetto.</span><span class="sxs-lookup"><span data-stu-id="7f05f-153">Set **Working directory** to the project directory.</span></span>
* <span data-ttu-id="7f05f-154">Salvare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="7f05f-154">Save the changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7f05f-155">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="7f05f-155">Additional Resources</span></span>

* [<span data-ttu-id="7f05f-156">Esercitazione: Introduzione a EF Core in .ASP.NET Core con un nuovo database usando SQLite</span><span class="sxs-lookup"><span data-stu-id="7f05f-156">Tutorial: Get started with EF Core on ASP.NET Core with a new database using SQLite</span></span>](xref:core/get-started/aspnetcore/new-db)
* [<span data-ttu-id="7f05f-157">Esercitazione: Introduzione all'uso di Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7f05f-157">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="7f05f-158">Esercitazione: Razor Pages con EF Core in ASP.NET Core ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7f05f-158">Tutorial: Razor Pages with Entity Framework Core in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
