---
title: Guida introduttiva ad ASP.NET Core - Nuovo database - EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 2248c60045a914c902f1c958a86c69b283abd722
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022236"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="8c025-102">Introduzione a EF Core in ASP.NET Core con un nuovo database</span><span class="sxs-lookup"><span data-stu-id="8c025-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="8c025-103">In questa esercitazione verrà creata un'applicazione ASP.NET Core MVC che esegue l'accesso ai dati di base usando Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="8c025-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="8c025-104">Nell'esercitazione vengono usate le migrazioni per creare il database dal modello di dati.</span><span class="sxs-lookup"><span data-stu-id="8c025-104">The tutorial uses migrations to create the database from the data model.</span></span>

<span data-ttu-id="8c025-105">È possibile eseguire l'esercitazione usando Visual Studio 2017 in Windows, oppure usando l'interfaccia della riga di comando di .NET Core in Windows, macOS o Linux.</span><span class="sxs-lookup"><span data-stu-id="8c025-105">You can follow the tutorial by using Visual Studio 2017 on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="8c025-106">Visualizzare l'esempio di questo articolo su GitHub:</span><span class="sxs-lookup"><span data-stu-id="8c025-106">View this article's sample on GitHub:</span></span>
* [<span data-ttu-id="8c025-107">Visual Studio 2017 con SQL Server</span><span class="sxs-lookup"><span data-stu-id="8c025-107">Visual Studio 2017 with SQL Server</span></span>](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* <span data-ttu-id="8c025-108">[Interfaccia della riga di comando di .NET Core con SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span><span class="sxs-lookup"><span data-stu-id="8c025-108">[.NET Core CLI with SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8c025-109">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="8c025-109">Prerequisites</span></span>

<span data-ttu-id="8c025-110">Installare il software seguente:</span><span class="sxs-lookup"><span data-stu-id="8c025-110">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8c025-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8c025-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8c025-112">[Visual Studio 2017 versione 15.7 o versione successiva](https://www.visualstudio.com/downloads/) con i carichi di lavoro seguenti:</span><span class="sxs-lookup"><span data-stu-id="8c025-112">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="8c025-113">**Sviluppo ASP.NET e Web** (in **Web e Cloud**)</span><span class="sxs-lookup"><span data-stu-id="8c025-113">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="8c025-114">**Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)</span><span class="sxs-lookup"><span data-stu-id="8c025-114">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="8c025-115">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="8c025-115">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8c025-116">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="8c025-116">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="8c025-117">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="8c025-117">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="8c025-118">Creare un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="8c025-118">Create a new project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8c025-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8c025-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8c025-120">Aprire Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="8c025-120">Open Visual Studio 2017</span></span>
* <span data-ttu-id="8c025-121">**File > Nuovo > Progetto**</span><span class="sxs-lookup"><span data-stu-id="8c025-121">**File > New > Project**</span></span>
* <span data-ttu-id="8c025-122">Selezionare **Installati > Visual C# > .NET Core** dal menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="8c025-122">From the left menu, select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="8c025-123">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8c025-123">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="8c025-124">Immettere **EFGetStarted.AspNetCore.NewDb** come nome e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="8c025-124">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="8c025-125">Nella finestra di dialogo **Nuova applicazione Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="8c025-125">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="8c025-126">Assicurarsi che le opzioni **.NET Core** e **ASP.NET Core 2.1** siano selezionate negli elenchi a discesa</span><span class="sxs-lookup"><span data-stu-id="8c025-126">Make sure that **.NET Core** and **ASP.NET Core 2.1** are selected in the drop-down lists</span></span>
  * <span data-ttu-id="8c025-127">Selezionare il modello di progetto **Applicazione Web (MVC)**</span><span class="sxs-lookup"><span data-stu-id="8c025-127">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="8c025-128">Assicurarsi che l'opzione **Autenticazione** sia impostata su **Nessuna autenticazione**</span><span class="sxs-lookup"><span data-stu-id="8c025-128">Make sure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="8c025-129">Fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="8c025-129">Click **OK**</span></span>

<span data-ttu-id="8c025-130">Avviso: se si usa **Account utente individuali** invece di **Nessuna** per **Autenticazione**, un modello di Entity Framework Core verrà aggiunto al progetto in `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="8c025-130">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to the project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="8c025-131">Usando le tecniche illustrate in questa esercitazione, è possibile scegliere di aggiungere un secondo modello o di estendere questo modello esistente in modo che contenga le classi di entità.</span><span class="sxs-lookup"><span data-stu-id="8c025-131">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8c025-132">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="8c025-132">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="8c025-133">Eseguire il comando seguente per creare un progetto MVC:</span><span class="sxs-lookup"><span data-stu-id="8c025-133">Run the following command to create an MVC project:</span></span>

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* <span data-ttu-id="8c025-134">Modificare il percorso alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="8c025-134">Change to the project directory.</span></span> <span data-ttu-id="8c025-135">Per eseguire il nuovo progetto, immettere i successivi comandi.</span><span class="sxs-lookup"><span data-stu-id="8c025-135">The next commands you enter need to run against the new project.</span></span>

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a><span data-ttu-id="8c025-136">Installare Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="8c025-136">Install Entity Framework Core</span></span>

<span data-ttu-id="8c025-137">Per installare EF Core, installare il pacchetto per i provider di database di EF Core che si vuole usare come destinazione.</span><span class="sxs-lookup"><span data-stu-id="8c025-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="8c025-138">Per un elenco dei provider disponibili, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="8c025-138">For a list of available providers, see [Database Providers](../../providers/index.md).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8c025-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8c025-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8c025-140">Per questa esercitazione, non è necessario installare un pacchetto di provider perché l'esercitazione usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8c025-140">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="8c025-141">Il pacchetto del provider SQL Server è incluso nel [metapacchetto Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="8c025-141">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8c025-142">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="8c025-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="8c025-143">Questa esercitazione usa SQLite in quanto viene eseguita su tutte le piattaforme supportate da .NET Core.</span><span class="sxs-lookup"><span data-stu-id="8c025-143">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span>

* <span data-ttu-id="8c025-144">Eseguire il comando seguente per installare il provider SQLite:</span><span class="sxs-lookup"><span data-stu-id="8c025-144">Run the following command to install the SQLite provider:</span></span>

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a><span data-ttu-id="8c025-145">Creare il modello</span><span class="sxs-lookup"><span data-stu-id="8c025-145">Create the model</span></span>

<span data-ttu-id="8c025-146">Definire una classe di contesto e le classi di entità che costituiscono il modello.</span><span class="sxs-lookup"><span data-stu-id="8c025-146">Define a context class and entity classes that make up the model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8c025-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8c025-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8c025-148">Fare clic con il pulsante destro del mouse sulla cartella **Models** e scegliere **Aggiungi > Classe**.</span><span class="sxs-lookup"><span data-stu-id="8c025-148">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="8c025-149">Immettere **Model.cs** come nome e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="8c025-149">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="8c025-150">Sostituire tutti i contenuti del file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="8c025-150">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8c025-151">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="8c025-151">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="8c025-152">Nella cartella **Modelli** creare un file **Course.cs** con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="8c025-152">In the **Models** folder create **Model.cs** with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

<span data-ttu-id="8c025-153">Un'app di produzione inserisce generalmente ogni classe in un file separato.</span><span class="sxs-lookup"><span data-stu-id="8c025-153">A production app would typically put each class in a separate file.</span></span> <span data-ttu-id="8c025-154">Per ragioni di semplicità, in questa esercitazione tutte le classi vengono inserite in un solo file.</span><span class="sxs-lookup"><span data-stu-id="8c025-154">For the sake of simplicity, this tutorial puts these classes in one file.</span></span>

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="8c025-155">Registrare il contesto con l'inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="8c025-155">Register the context with dependency injection</span></span>

<span data-ttu-id="8c025-156">I servizi (ad esempio, `BloggingContext`) vengono registrati con l'[inserimento delle dipendenze](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8c025-156">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="8c025-157">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio i controller MVC) tramite i parametri del costruttore o le proprietà.</span><span class="sxs-lookup"><span data-stu-id="8c025-157">Components that require these services (such as MVC controllers) are provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="8c025-158">Per rendere `BloggingContext` disponibile per i controller MVC, registrarlo come servizio.</span><span class="sxs-lookup"><span data-stu-id="8c025-158">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8c025-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8c025-159">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8c025-160">In **Startup.cs** aggiungere le istruzioni `using` seguenti:</span><span class="sxs-lookup"><span data-stu-id="8c025-160">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* <span data-ttu-id="8c025-161">Aggiungere il codice evidenziato seguente al metodo `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8c025-161">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8c025-162">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="8c025-162">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="8c025-163">In **Startup.cs** aggiungere le istruzioni `using` seguenti:</span><span class="sxs-lookup"><span data-stu-id="8c025-163">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* <span data-ttu-id="8c025-164">Aggiungere il codice evidenziato seguente al metodo `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8c025-164">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

<span data-ttu-id="8c025-165">Una app di produzione inserisce generalmente la stringa di connessione in un file di configurazione o in una variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="8c025-165">A production app would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="8c025-166">Per ragioni di semplicità, per questa esercitazione viene definita nel codice.</span><span class="sxs-lookup"><span data-stu-id="8c025-166">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="8c025-167">Per altre informazioni, vedere [Connection Strings](../../miscellaneous/connection-strings.md) (Stringhe di connessione).</span><span class="sxs-lookup"><span data-stu-id="8c025-167">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="8c025-168">Creare il database</span><span class="sxs-lookup"><span data-stu-id="8c025-168">Create the database</span></span>

<span data-ttu-id="8c025-169">La procedura seguente usa le [migrazioni](xref:core/managing-schemas/migrations/index) per creare un database.</span><span class="sxs-lookup"><span data-stu-id="8c025-169">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8c025-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8c025-170">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8c025-171">**Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="8c025-171">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="8c025-172">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="8c025-172">Run the following commands:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="8c025-173">Se viene visualizzato l'errore `The term 'add-migration' is not recognized as the name of a cmdlet`, chiudere e riaprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8c025-173">If you get an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>

  <span data-ttu-id="8c025-174">Il comando `Add-Migration` esegue lo scaffolding di una migrazione per creare il set iniziale di tabelle per il modello.</span><span class="sxs-lookup"><span data-stu-id="8c025-174">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="8c025-175">Il comando `Update-Database` crea il database e ne applica la nuova migrazione.</span><span class="sxs-lookup"><span data-stu-id="8c025-175">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8c025-176">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="8c025-176">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="8c025-177">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="8c025-177">Run the following commands:</span></span>

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="8c025-178">Il comando `migrations` esegue lo scaffolding di una migrazione per creare il set iniziale di tabelle per il modello.</span><span class="sxs-lookup"><span data-stu-id="8c025-178">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="8c025-179">Il comando `database update` crea il database e ne applica la nuova migrazione.</span><span class="sxs-lookup"><span data-stu-id="8c025-179">The `database update` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-a-controller"></a><span data-ttu-id="8c025-180">Creare un controller</span><span class="sxs-lookup"><span data-stu-id="8c025-180">Create a controller</span></span>

<span data-ttu-id="8c025-181">Eseguire lo scaffolding di un controller e delle visualizzazioni per l'entità `Blog`.</span><span class="sxs-lookup"><span data-stu-id="8c025-181">Scaffold a controller and views for the `Blog` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8c025-182">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8c025-182">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8c025-183">Fare clic con il pulsante destro del mouse sulla cartella **Controllers** in **Esplora soluzioni** e scegliere **Aggiungi > Controller**.</span><span class="sxs-lookup"><span data-stu-id="8c025-183">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="8c025-184">Selezionare **Controller MVC con visualizzazioni, che usa Entity Framework** e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="8c025-184">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="8c025-185">Impostare **Classe modello** su **Blog** e **Classe contesto dati** su **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="8c025-185">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="8c025-186">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="8c025-186">Click **Add**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8c025-187">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="8c025-187">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="8c025-188">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="8c025-188">Run the following commands:</span></span>

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  <span data-ttu-id="8c025-189">I comandi `tool install` e `add package` installano gli strumenti che possano eseguire lo scaffolding di controller e visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="8c025-189">The `tool install` and `add package` commands install the tooling that can scaffold controllers and views.</span></span> <span data-ttu-id="8c025-190">Il comando `restore` assicura che vengano scaricati tutti i pacchetti del progetto, mentre il comando `aspnet-codegenerator` esegue lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="8c025-190">The `restore` command ensures that all of the project's packages are downloaded, and the `aspnet-codegenerator` command does the scaffolding.</span></span>
---

<span data-ttu-id="8c025-191">Il motore di scaffolding crea i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="8c025-191">The scaffolding engine creates the following files:</span></span>

* <span data-ttu-id="8c025-192">Un controller (*Controllers/BlogsController.cs*)</span><span class="sxs-lookup"><span data-stu-id="8c025-192">A controller (*Controllers/BlogsController.cs*)</span></span>
* <span data-ttu-id="8c025-193">Visualizzazioni Razor per le pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice) (_Views/Movies/\*.cshtml_)</span><span class="sxs-lookup"><span data-stu-id="8c025-193">Razor views for Create, Delete, Details, Edit, and Index pages (_Views/Movies/\*.cshtml_)</span></span>

## <a name="run-the-application"></a><span data-ttu-id="8c025-194">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="8c025-194">Run the application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8c025-195">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8c025-195">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8c025-196">**Debug** > **Avvia senza eseguire debug**</span><span class="sxs-lookup"><span data-stu-id="8c025-196">**Debug** > **Start Without Debugging**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="8c025-197">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="8c025-197">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet run
```
---

* <span data-ttu-id="8c025-198">Passare a `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="8c025-198">Navigate to `/Blogs`</span></span>

* <span data-ttu-id="8c025-199">Usare il collegamento **Crea nuova** per creare alcune voci del blog.</span><span class="sxs-lookup"><span data-stu-id="8c025-199">Use the **Create New** link to create some blog entries.</span></span>

  ![Pagina Crea](_static/create.png)

* <span data-ttu-id="8c025-201">Testare i collegamenti **Dettagli**, **Modica** ed **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="8c025-201">Test the **Details**, **Edit**, and **Delete** links.</span></span>

  ![Pagina Index (Indice)](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="8c025-203">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="8c025-203">Additional Resources</span></span>

* [<span data-ttu-id="8c025-204">Esercitazione: Introduzione a EF Core in ASP.NET Core con un nuovo database usando SQLite</span><span class="sxs-lookup"><span data-stu-id="8c025-204">Tutorial: Get started with EF Core on .NET Core with a new database using SQLite</span></span>](xref:core/get-started/netcore/new-db-sqlite)
* [<span data-ttu-id="8c025-205">Esercitazione: Introduzione all'uso di Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8c025-205">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="8c025-206">Esercitazione: Razor Pages con EF Core in ASP.NET Core ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8c025-206">Tutorial: Razor Pages with Entity Framework Core in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
