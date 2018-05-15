---
title: Guida introduttiva ad ASP.NET Core - Nuovo database - EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 80477ca57b8b3df6de8ba3595c9056c6b8412040
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="908a5-102">Introduzione a EF Core in ASP.NET Core con un nuovo database</span><span class="sxs-lookup"><span data-stu-id="908a5-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="908a5-103">In questa procedura dettagliata si compilerà un'applicazione ASP.NET Core MVC che esegue l'accesso ai dati di base usando Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="908a5-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="908a5-104">Si useranno le migrazioni per creare il database dal modello di EF Core.</span><span class="sxs-lookup"><span data-stu-id="908a5-104">You will use migrations to create the database from your EF Core model.</span></span> <span data-ttu-id="908a5-105">Per altre esercitazioni su Entity Framework Core, vedere [Risorse aggiuntive](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="908a5-105">See [Additional Resources](#additional-resources) for more Entity Framework Core tutorials.</span></span>

<span data-ttu-id="908a5-106">Per questa esercitazione sono necessari:</span><span class="sxs-lookup"><span data-stu-id="908a5-106">This tutorial requires:</span></span>
* <span data-ttu-id="908a5-107">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) con questi carichi di lavoro:</span><span class="sxs-lookup"><span data-stu-id="908a5-107">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="908a5-108">**Sviluppo ASP.NET e Web** (in **Web e Cloud**)</span><span class="sxs-lookup"><span data-stu-id="908a5-108">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="908a5-109">**Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)</span><span class="sxs-lookup"><span data-stu-id="908a5-109">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="908a5-110">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="908a5-110">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>

> [!TIP]  
> <span data-ttu-id="908a5-111">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="908a5-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) on GitHub.</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="908a5-112">Creare un nuovo progetto in Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="908a5-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="908a5-113">**File > Nuovo > Progetto**</span><span class="sxs-lookup"><span data-stu-id="908a5-113">**File > New > Project**</span></span>
* <span data-ttu-id="908a5-114">Scegliere **Installati > Modelli > Visual C# > .NET Core** dal menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="908a5-114">From the left menu select **Installed > Templates > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="908a5-115">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="908a5-115">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="908a5-116">Immettere **EFGetStarted.AspNetCore.NewDb** come nome e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="908a5-116">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="908a5-117">Nella finestra di dialogo **Nuova applicazione Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="908a5-117">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="908a5-118">Assicurarsi che le opzioni **.NET Core** e **ASP.NET Core 2.0** siano selezionate negli elenchi a discesa</span><span class="sxs-lookup"><span data-stu-id="908a5-118">Ensure the options **.NET Core** and **ASP.NET Core 2.0** are selected in the drop down lists</span></span>
  * <span data-ttu-id="908a5-119">Selezionare il modello di progetto **Applicazione Web (MVC)**</span><span class="sxs-lookup"><span data-stu-id="908a5-119">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="908a5-120">Assicurarsi che **Autenticazione** sia impostato su **Nessuna autenticazione**</span><span class="sxs-lookup"><span data-stu-id="908a5-120">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="908a5-121">Fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="908a5-121">Click **OK**</span></span>

<span data-ttu-id="908a5-122">Avviso: se si usa **Account utente individuali** invece di **Nessuna** per **Autenticazione**, un modello di Entity Framework Core verrà aggiunto al progetto in `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="908a5-122">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="908a5-123">Usando le tecniche illustrate in questa procedura dettaglia, è possibile scegliere di aggiungere un secondo modello o di estendere questo modello esistente in modo che contenga le classi di entità.</span><span class="sxs-lookup"><span data-stu-id="908a5-123">Using the techniques you will learn in this walkthrough, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="908a5-124">Installare Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="908a5-124">Install Entity Framework Core</span></span>

<span data-ttu-id="908a5-125">Installare il pacchetto per i provider di database di EF Core che si vuole specificare come destinazione.</span><span class="sxs-lookup"><span data-stu-id="908a5-125">Install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="908a5-126">Questa procedura dettagliata usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="908a5-126">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="908a5-127">Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="908a5-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="908a5-128">**Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="908a5-128">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="908a5-129">Eseguire `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="908a5-129">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="908a5-130">Verranno usati alcuni strumenti di Entity Framework Core per creare un database dal modello di EF Core,</span><span class="sxs-lookup"><span data-stu-id="908a5-130">We will be using some Entity Framework Core Tools to create a database from your EF Core model.</span></span> <span data-ttu-id="908a5-131">quindi verrà anche installato il pacchetto di strumenti:</span><span class="sxs-lookup"><span data-stu-id="908a5-131">So we will install the tools package as well:</span></span>

* <span data-ttu-id="908a5-132">Eseguire `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="908a5-132">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="908a5-133">Più avanti verranno usati alcuni strumenti di scaffolding di ASP.NET Core per creare controller e visualizzazioni,</span><span class="sxs-lookup"><span data-stu-id="908a5-133">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="908a5-134">quindi verrà anche installato il pacchetto di progettazioni:</span><span class="sxs-lookup"><span data-stu-id="908a5-134">So we will install this design package as well:</span></span>

* <span data-ttu-id="908a5-135">Eseguire `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span><span class="sxs-lookup"><span data-stu-id="908a5-135">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="908a5-136">Creare il modello</span><span class="sxs-lookup"><span data-stu-id="908a5-136">Create the model</span></span>

<span data-ttu-id="908a5-137">Definire un contesto e le classi di entità che costituiscono il modello:</span><span class="sxs-lookup"><span data-stu-id="908a5-137">Define a context and entity classes that make up the model:</span></span>

* <span data-ttu-id="908a5-138">Fare clic con il pulsante destro del mouse sulla cartella **Models** e scegliere **Aggiungi > Classe**.</span><span class="sxs-lookup"><span data-stu-id="908a5-138">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="908a5-139">Immettere **Model.cs** come nome e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="908a5-139">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="908a5-140">Sostituire tutti i contenuti del file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="908a5-140">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="908a5-141">Nota: in una vera app in genere si inserisce ogni classe del modello in un file separato.</span><span class="sxs-lookup"><span data-stu-id="908a5-141">Note: In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="908a5-142">Per ragioni di semplicità, in questa esercitazione tutte le classi vengono inserite in un solo file.</span><span class="sxs-lookup"><span data-stu-id="908a5-142">For the sake of simplicity, we are putting all the classes in one file for this tutorial.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="908a5-143">Registrare il contesto con l'inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="908a5-143">Register your context with dependency injection</span></span>

<span data-ttu-id="908a5-144">I servizi (ad esempio, `BloggingContext`) vengono registrati con l'[inserimento delle dipendenze](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="908a5-144">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="908a5-145">Questi servizi vengono quindi forniti ai componenti per cui sono necessari (ad esempio, i controller MVC) tramite proprietà o parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="908a5-145">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="908a5-146">`BloggingContext` verrà registrato come servizio per consentire ai controller MVC di usarlo.</span><span class="sxs-lookup"><span data-stu-id="908a5-146">In order for our MVC controllers to make use of `BloggingContext` we will register it as a service.</span></span>

* <span data-ttu-id="908a5-147">Aprire **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="908a5-147">Open **Startup.cs**</span></span>
* <span data-ttu-id="908a5-148">Aggiungere le istruzioni `using` seguenti:</span><span class="sxs-lookup"><span data-stu-id="908a5-148">Add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="908a5-149">Aggiungere il metodo `AddDbContext` per registrarlo come servizio:</span><span class="sxs-lookup"><span data-stu-id="908a5-149">Add the `AddDbContext` method to register it as a service:</span></span>

* <span data-ttu-id="908a5-150">Aggiungere al metodo `ConfigureServices` il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="908a5-150">Add the following code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

<span data-ttu-id="908a5-151">Nota: una vera app in genere inserisce la stringa di connessione in un file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="908a5-151">Note: A real app would generally put the connection string in a configuration file.</span></span> <span data-ttu-id="908a5-152">Per ragioni di semplicità, viene definita nel codice.</span><span class="sxs-lookup"><span data-stu-id="908a5-152">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="908a5-153">Per altre informazioni, vedere [Connection Strings](../../miscellaneous/connection-strings.md) (Stringhe di connessione).</span><span class="sxs-lookup"><span data-stu-id="908a5-153">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="908a5-154">Creare il database</span><span class="sxs-lookup"><span data-stu-id="908a5-154">Create your database</span></span>

<span data-ttu-id="908a5-155">Dopo avere creato un modello, è possibile usare le [migrazioni](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) per creare un database.</span><span class="sxs-lookup"><span data-stu-id="908a5-155">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="908a5-156">Aprire la console di Gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="908a5-156">Open the PMC:</span></span>

  <span data-ttu-id="908a5-157">**Strumenti –> Gestione pacchetti NuGet –> Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="908a5-157">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="908a5-158">Eseguire `Add-Migration InitialCreate` per effettuare lo scaffolding di una migrazione per poter creare il set iniziale di tabelle per il modello.</span><span class="sxs-lookup"><span data-stu-id="908a5-158">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="908a5-159">Se viene visualizzato l'errore `The term 'add-migration' is not recognized as the name of a cmdlet`, chiudere e riaprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="908a5-159">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="908a5-160">Eseguire `Update-Database` per applicare la nuova migrazione al database.</span><span class="sxs-lookup"><span data-stu-id="908a5-160">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="908a5-161">Questo comando crea il database prima di applicare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="908a5-161">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="908a5-162">Creare un controller</span><span class="sxs-lookup"><span data-stu-id="908a5-162">Create a controller</span></span>

<span data-ttu-id="908a5-163">Abilitare lo scaffolding nel progetto:</span><span class="sxs-lookup"><span data-stu-id="908a5-163">Enable scaffolding in the project:</span></span>

* <span data-ttu-id="908a5-164">Fare clic con il pulsante destro del mouse sulla cartella **Controllers** in **Esplora soluzioni** e scegliere **Aggiungi > Controller**.</span><span class="sxs-lookup"><span data-stu-id="908a5-164">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="908a5-165">Selezionare **Dipendenze minime** e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="908a5-165">Select **Minimal Dependencies** and click **Add**.</span></span>
* <span data-ttu-id="908a5-166">È possibile ignorare o eliminare il file *ScaffoldingReadMe.txt*.</span><span class="sxs-lookup"><span data-stu-id="908a5-166">You can ignore or delete the *ScaffoldingReadMe.txt* file.</span></span>

<span data-ttu-id="908a5-167">Quando è abilitato, è possibile eseguire lo scaffolding di un controller per l'entità `Blog`.</span><span class="sxs-lookup"><span data-stu-id="908a5-167">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="908a5-168">Fare clic con il pulsante destro del mouse sulla cartella **Controllers** in **Esplora soluzioni** e scegliere **Aggiungi > Controller**.</span><span class="sxs-lookup"><span data-stu-id="908a5-168">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="908a5-169">Selezionare **Controller MVC con visualizzazioni, che usa Entity Framework** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="908a5-169">Select **MVC Controller with views, using Entity Framework** and click **Ok**.</span></span>
* <span data-ttu-id="908a5-170">Impostare **Classe modello** su **Blog** e **Classe contesto dati** su **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="908a5-170">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="908a5-171">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="908a5-171">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="908a5-172">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="908a5-172">Run the application</span></span>

<span data-ttu-id="908a5-173">Premere F5 per eseguire e testare l'app.</span><span class="sxs-lookup"><span data-stu-id="908a5-173">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="908a5-174">Passare a `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="908a5-174">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="908a5-175">Usare il collegamento Crea per creare alcune voci del blog.</span><span class="sxs-lookup"><span data-stu-id="908a5-175">Use the create link to create some blog entries.</span></span> <span data-ttu-id="908a5-176">Testare i dettagli ed eliminare i collegamenti.</span><span class="sxs-lookup"><span data-stu-id="908a5-176">Test the details and delete links.</span></span>

![immagine](_static/create.png)

![immagine](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="908a5-179">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="908a5-179">Additional Resources</span></span>

* <span data-ttu-id="908a5-180">[EF - Nuovo database con SQLite](xref:core/get-started/netcore/new-db-sqlite): esercitazione su EF per la console multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="908a5-180">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="908a5-181">Introduzione ad ASP.NET Core MVC in Mac o Linux</span><span class="sxs-lookup"><span data-stu-id="908a5-181">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="908a5-182">Introduzione ad ASP.NET Core MVC con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="908a5-182">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="908a5-183">Introduzione ad ASP.NET Core ed Entity Framework Core con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="908a5-183">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
