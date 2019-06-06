---
title: Guida introduttiva ad ASP.NET Core - Nuovo database - EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 2eb1668b8c077fabc9cb21088452fd1bead7ff22
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452244"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="cfce3-102">Introduzione a EF Core in ASP.NET Core con un nuovo database</span><span class="sxs-lookup"><span data-stu-id="cfce3-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="cfce3-103">In questa esercitazione verrà creata un'applicazione ASP.NET Core MVC che esegue l'accesso ai dati di base usando Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="cfce3-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="cfce3-104">Nell'esercitazione vengono usate le migrazioni per creare il database dal modello di dati.</span><span class="sxs-lookup"><span data-stu-id="cfce3-104">The tutorial uses migrations to create the database from the data model.</span></span>

<span data-ttu-id="cfce3-105">È possibile eseguire l'esercitazione usando Visual Studio 2017 in Windows, oppure usando l'interfaccia della riga di comando di .NET Core in Windows, macOS o Linux.</span><span class="sxs-lookup"><span data-stu-id="cfce3-105">You can follow the tutorial by using Visual Studio 2017 on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="cfce3-106">Visualizzare l'esempio di questo articolo su GitHub:</span><span class="sxs-lookup"><span data-stu-id="cfce3-106">View this article's sample on GitHub:</span></span>
* [<span data-ttu-id="cfce3-107">Visual Studio 2017 con SQL Server</span><span class="sxs-lookup"><span data-stu-id="cfce3-107">Visual Studio 2017 with SQL Server</span></span>](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* <span data-ttu-id="cfce3-108">[Interfaccia della riga di comando di .NET Core con SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span><span class="sxs-lookup"><span data-stu-id="cfce3-108">[.NET Core CLI with SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cfce3-109">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="cfce3-109">Prerequisites</span></span>

<span data-ttu-id="cfce3-110">Installare il software seguente:</span><span class="sxs-lookup"><span data-stu-id="cfce3-110">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cfce3-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cfce3-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cfce3-112">[Visual Studio 2017 versione 15.7 o versione successiva](https://www.visualstudio.com/downloads/) con i carichi di lavoro seguenti:</span><span class="sxs-lookup"><span data-stu-id="cfce3-112">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="cfce3-113">**Sviluppo ASP.NET e Web** (in **Web e Cloud**)</span><span class="sxs-lookup"><span data-stu-id="cfce3-113">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="cfce3-114">**Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)</span><span class="sxs-lookup"><span data-stu-id="cfce3-114">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="cfce3-115">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="cfce3-115">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cfce3-116">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="cfce3-116">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="cfce3-117">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="cfce3-117">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="cfce3-118">Creare un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="cfce3-118">Create a new project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cfce3-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cfce3-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cfce3-120">Aprire Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="cfce3-120">Open Visual Studio 2017</span></span>
* <span data-ttu-id="cfce3-121">**File > Nuovo > Progetto**</span><span class="sxs-lookup"><span data-stu-id="cfce3-121">**File > New > Project**</span></span>
* <span data-ttu-id="cfce3-122">Selezionare **Installati > Visual C# > .NET Core** dal menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="cfce3-122">From the left menu, select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="cfce3-123">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="cfce3-123">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="cfce3-124">Immettere **EFGetStarted.AspNetCore.NewDb** come nome e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="cfce3-124">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="cfce3-125">Nella finestra di dialogo **Nuova applicazione Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="cfce3-125">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="cfce3-126">Assicurarsi che le opzioni **.NET Core** e **ASP.NET Core 2.1** siano selezionate negli elenchi a discesa</span><span class="sxs-lookup"><span data-stu-id="cfce3-126">Make sure that **.NET Core** and **ASP.NET Core 2.1** are selected in the drop-down lists</span></span>
  * <span data-ttu-id="cfce3-127">Selezionare il modello di progetto **Applicazione Web (MVC)**</span><span class="sxs-lookup"><span data-stu-id="cfce3-127">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="cfce3-128">Assicurarsi che l'opzione **Autenticazione** sia impostata su **Nessuna autenticazione**</span><span class="sxs-lookup"><span data-stu-id="cfce3-128">Make sure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="cfce3-129">Fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="cfce3-129">Click **OK**</span></span>

<span data-ttu-id="cfce3-130">Avviso: se si usa **Account utente individuali** invece di **Nessuna** per **Autenticazione**, un modello di Entity Framework Core verrà aggiunto al progetto in `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="cfce3-130">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to the project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="cfce3-131">Usando le tecniche illustrate in questa esercitazione, è possibile scegliere di aggiungere un secondo modello o di estendere questo modello esistente in modo che contenga le classi di entità.</span><span class="sxs-lookup"><span data-stu-id="cfce3-131">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cfce3-132">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="cfce3-132">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="cfce3-133">Eseguire il comando seguente per creare un progetto MVC:</span><span class="sxs-lookup"><span data-stu-id="cfce3-133">Run the following command to create an MVC project:</span></span>

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* <span data-ttu-id="cfce3-134">Modificare il percorso alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="cfce3-134">Change to the project directory.</span></span> <span data-ttu-id="cfce3-135">Per eseguire il nuovo progetto, immettere i successivi comandi.</span><span class="sxs-lookup"><span data-stu-id="cfce3-135">The next commands you enter need to run against the new project.</span></span>

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a><span data-ttu-id="cfce3-136">Installare Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="cfce3-136">Install Entity Framework Core</span></span>

<span data-ttu-id="cfce3-137">Per installare EF Core, installare il pacchetto per i provider di database di EF Core che si vuole usare come destinazione.</span><span class="sxs-lookup"><span data-stu-id="cfce3-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="cfce3-138">Per un elenco dei provider disponibili, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="cfce3-138">For a list of available providers, see [Database Providers](../../providers/index.md).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cfce3-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cfce3-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="cfce3-140">Per questa esercitazione, non è necessario installare un pacchetto di provider perché l'esercitazione usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="cfce3-140">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="cfce3-141">Il pacchetto del provider SQL Server è incluso nel [metapacchetto Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="cfce3-141">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cfce3-142">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="cfce3-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="cfce3-143">Questa esercitazione usa SQLite in quanto viene eseguita su tutte le piattaforme supportate da .NET Core.</span><span class="sxs-lookup"><span data-stu-id="cfce3-143">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span>

* <span data-ttu-id="cfce3-144">Eseguire il comando seguente per installare il provider SQLite:</span><span class="sxs-lookup"><span data-stu-id="cfce3-144">Run the following command to install the SQLite provider:</span></span>

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a><span data-ttu-id="cfce3-145">Creare il modello</span><span class="sxs-lookup"><span data-stu-id="cfce3-145">Create the model</span></span>

<span data-ttu-id="cfce3-146">Definire una classe di contesto e le classi di entità che costituiscono il modello.</span><span class="sxs-lookup"><span data-stu-id="cfce3-146">Define a context class and entity classes that make up the model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cfce3-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cfce3-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cfce3-148">Fare clic con il pulsante destro del mouse sulla cartella **Models** e scegliere **Aggiungi > Classe**.</span><span class="sxs-lookup"><span data-stu-id="cfce3-148">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="cfce3-149">Immettere **Model.cs** come nome e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="cfce3-149">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="cfce3-150">Sostituire tutti i contenuti del file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="cfce3-150">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cfce3-151">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="cfce3-151">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="cfce3-152">Nella cartella **Modelli** creare un file **Course.cs** con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="cfce3-152">In the **Models** folder create **Model.cs** with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

<span data-ttu-id="cfce3-153">Un'app di produzione inserisce generalmente ogni classe in un file separato.</span><span class="sxs-lookup"><span data-stu-id="cfce3-153">A production app would typically put each class in a separate file.</span></span> <span data-ttu-id="cfce3-154">Per ragioni di semplicità, in questa esercitazione tutte le classi vengono inserite in un solo file.</span><span class="sxs-lookup"><span data-stu-id="cfce3-154">For the sake of simplicity, this tutorial puts these classes in one file.</span></span>

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="cfce3-155">Registrare il contesto con l'inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="cfce3-155">Register the context with dependency injection</span></span>

<span data-ttu-id="cfce3-156">Per rendere `BloggingContext` disponibile per i controller MVC, registrarlo come servizio in `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="cfce3-156">To make `BloggingContext` available to MVC controllers, register it as a service in `Startup.cs`.</span></span>

<span data-ttu-id="cfce3-157">I servizi (come `BloggingContext`) vengono registrati con [l'inserimento delle dipendenze](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) durante l'avvio dell'applicazione in modo da poterli garantire automaticamente ai componenti che usano i servizi (ad esempio i controller MVC) con i parametri e le proprietà del costruttore.</span><span class="sxs-lookup"><span data-stu-id="cfce3-157">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup so that they can be provided automatically to components that consume services (such as MVC controllers) via constructor parameters and properties.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cfce3-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cfce3-158">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cfce3-159">In **Startup.cs** aggiungere le istruzioni `using` seguenti:</span><span class="sxs-lookup"><span data-stu-id="cfce3-159">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* <span data-ttu-id="cfce3-160">Aggiungere il codice evidenziato seguente al metodo `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="cfce3-160">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cfce3-161">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="cfce3-161">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="cfce3-162">In **Startup.cs** aggiungere le istruzioni `using` seguenti:</span><span class="sxs-lookup"><span data-stu-id="cfce3-162">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* <span data-ttu-id="cfce3-163">Aggiungere il codice evidenziato seguente al metodo `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="cfce3-163">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

<span data-ttu-id="cfce3-164">Una app di produzione inserisce generalmente la stringa di connessione in un file di configurazione o in una variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="cfce3-164">A production app would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="cfce3-165">Per ragioni di semplicità, per questa esercitazione viene definita nel codice.</span><span class="sxs-lookup"><span data-stu-id="cfce3-165">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="cfce3-166">Per altre informazioni, vedere [Connection Strings](../../miscellaneous/connection-strings.md) (Stringhe di connessione).</span><span class="sxs-lookup"><span data-stu-id="cfce3-166">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="cfce3-167">Creare il database</span><span class="sxs-lookup"><span data-stu-id="cfce3-167">Create the database</span></span>

<span data-ttu-id="cfce3-168">La procedura seguente usa le [migrazioni](xref:core/managing-schemas/migrations/index) per creare un database.</span><span class="sxs-lookup"><span data-stu-id="cfce3-168">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cfce3-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cfce3-169">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cfce3-170">**Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="cfce3-170">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="cfce3-171">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="cfce3-171">Run the following commands:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="cfce3-172">Se viene visualizzato l'errore `The term 'add-migration' is not recognized as the name of a cmdlet`, chiudere e riaprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cfce3-172">If you get an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>

  <span data-ttu-id="cfce3-173">Il comando `Add-Migration` esegue lo scaffolding di una migrazione per creare il set iniziale di tabelle per il modello.</span><span class="sxs-lookup"><span data-stu-id="cfce3-173">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="cfce3-174">Il comando `Update-Database` crea il database e ne applica la nuova migrazione.</span><span class="sxs-lookup"><span data-stu-id="cfce3-174">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cfce3-175">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="cfce3-175">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="cfce3-176">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="cfce3-176">Run the following commands:</span></span>

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="cfce3-177">Il comando `migrations` esegue lo scaffolding di una migrazione per creare il set iniziale di tabelle per il modello.</span><span class="sxs-lookup"><span data-stu-id="cfce3-177">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="cfce3-178">Il comando `database update` crea il database e ne applica la nuova migrazione.</span><span class="sxs-lookup"><span data-stu-id="cfce3-178">The `database update` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-a-controller"></a><span data-ttu-id="cfce3-179">Creare un controller</span><span class="sxs-lookup"><span data-stu-id="cfce3-179">Create a controller</span></span>

<span data-ttu-id="cfce3-180">Eseguire lo scaffolding di un controller e delle visualizzazioni per l'entità `Blog`.</span><span class="sxs-lookup"><span data-stu-id="cfce3-180">Scaffold a controller and views for the `Blog` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cfce3-181">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cfce3-181">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cfce3-182">Fare clic con il pulsante destro del mouse sulla cartella **Controllers** in **Esplora soluzioni** e scegliere **Aggiungi > Controller**.</span><span class="sxs-lookup"><span data-stu-id="cfce3-182">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="cfce3-183">Selezionare **Controller MVC con visualizzazioni, che usa Entity Framework** e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="cfce3-183">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="cfce3-184">Impostare **Classe modello** su **Blog** e **Classe contesto dati** su **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="cfce3-184">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="cfce3-185">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="cfce3-185">Click **Add**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cfce3-186">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="cfce3-186">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="cfce3-187">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="cfce3-187">Run the following commands:</span></span>

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  <span data-ttu-id="cfce3-188">I comandi `tool install` e `add package` installano gli strumenti che possano eseguire lo scaffolding di controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="cfce3-188">The `tool install` and `add package` commands install the tooling that can scaffold controllers and views.</span></span> <span data-ttu-id="cfce3-189">Il comando `restore` assicura che vengano scaricati tutti i pacchetti del progetto, mentre il comando `aspnet-codegenerator` esegue lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="cfce3-189">The `restore` command ensures that all of the project's packages are downloaded, and the `aspnet-codegenerator` command does the scaffolding.</span></span>
---

<span data-ttu-id="cfce3-190">Il motore di scaffolding crea i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="cfce3-190">The scaffolding engine creates the following files:</span></span>

* <span data-ttu-id="cfce3-191">Un controller (*Controllers/BlogsController.cs*)</span><span class="sxs-lookup"><span data-stu-id="cfce3-191">A controller (*Controllers/BlogsController.cs*)</span></span>
* <span data-ttu-id="cfce3-192">Visualizzazioni Razor per le pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice) (_Views/Blogs/\*.cshtml_)</span><span class="sxs-lookup"><span data-stu-id="cfce3-192">Razor views for Create, Delete, Details, Edit, and Index pages (_Views/Blogs/\*.cshtml_)</span></span>

## <a name="run-the-application"></a><span data-ttu-id="cfce3-193">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="cfce3-193">Run the application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cfce3-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cfce3-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="cfce3-195">**Debug** > **Avvia senza eseguire debug**</span><span class="sxs-lookup"><span data-stu-id="cfce3-195">**Debug** > **Start Without Debugging**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cfce3-196">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="cfce3-196">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet run
```
---

* <span data-ttu-id="cfce3-197">Passare a `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="cfce3-197">Navigate to `/Blogs`</span></span>

* <span data-ttu-id="cfce3-198">Usare il collegamento **Crea nuova** per creare alcune voci del blog.</span><span class="sxs-lookup"><span data-stu-id="cfce3-198">Use the **Create New** link to create some blog entries.</span></span>

  ![Pagina Crea](_static/create.png)

* <span data-ttu-id="cfce3-200">Testare i collegamenti **Dettagli**, **Modica** ed **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="cfce3-200">Test the **Details**, **Edit**, and **Delete** links.</span></span>

  ![Pagina Index (Indice)](_static/index-new-db.png)

## <a name="additional-tutorials"></a><span data-ttu-id="cfce3-202">Esercitazioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="cfce3-202">Additional tutorials</span></span>

* [<span data-ttu-id="cfce3-203">Introduzione a EF Core in ASP.NET Core con un nuovo database usando SQLite</span><span class="sxs-lookup"><span data-stu-id="cfce3-203">Get started with EF Core on .NET Core with a new database using SQLite</span></span>](xref:core/get-started/netcore/new-db-sqlite)
* <span data-ttu-id="cfce3-204">ASP.NET Core MVC:</span><span class="sxs-lookup"><span data-stu-id="cfce3-204">ASP.NET Core MVC:</span></span>
  * [<span data-ttu-id="cfce3-205">Introduzione ad ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="cfce3-205">Get started with ASP.NET Core MVC</span></span>](/aspnet/core/tutorials/first-mvc-app/start-mvc)
  * [<span data-ttu-id="cfce3-206">Introduzione a EF Core in un'app Web ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="cfce3-206">Get started with EF Core in an ASP.NET MVC web app</span></span>](/aspnet/core/data/ef-mvc/intro)
* <span data-ttu-id="cfce3-207">[Razor Pages](/aspnet/core/razor-pages/index):</span><span class="sxs-lookup"><span data-stu-id="cfce3-207">[Razor Pages](/aspnet/core/razor-pages/index):</span></span>
  * [<span data-ttu-id="cfce3-208">Introduzione all'uso di Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cfce3-208">Get started with Razor Pages in ASP.NET Core</span></span>](/aspnet/core/tutorials/razor-pages/razor-pages-start)
  * [<span data-ttu-id="cfce3-209">Razor Pages con Entity Framework Core in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cfce3-209">Razor Pages with Entity Framework Core in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
