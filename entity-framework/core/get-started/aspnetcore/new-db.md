---
title: Guida introduttiva ad ASP.NET Core - Nuovo database - EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: c6a86dd943dc7fe6f600455fe6743ea01a062aab
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996064"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="81e5d-102">Introduzione a EF Core in ASP.NET Core con un nuovo database</span><span class="sxs-lookup"><span data-stu-id="81e5d-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="81e5d-103">In questa esercitazione verrà creata un'applicazione ASP.NET Core MVC che esegue l'accesso ai dati di base usando Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="81e5d-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="81e5d-104">Si useranno le migrazioni per creare il database dal modello di EF Core.</span><span class="sxs-lookup"><span data-stu-id="81e5d-104">You use migrations to create the database from your EF Core model.</span></span>

<span data-ttu-id="81e5d-105">[Visualizzare l'esempio di questo articolo su GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span><span class="sxs-lookup"><span data-stu-id="81e5d-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81e5d-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="81e5d-106">Prerequisites</span></span>

<span data-ttu-id="81e5d-107">Installare il software seguente:</span><span class="sxs-lookup"><span data-stu-id="81e5d-107">Install the following software:</span></span>

* <span data-ttu-id="81e5d-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) con questi carichi di lavoro:</span><span class="sxs-lookup"><span data-stu-id="81e5d-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="81e5d-109">**Sviluppo ASP.NET e Web** (in **Web e Cloud**)</span><span class="sxs-lookup"><span data-stu-id="81e5d-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="81e5d-110">**Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)</span><span class="sxs-lookup"><span data-stu-id="81e5d-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="81e5d-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="81e5d-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="81e5d-112">Creare un nuovo progetto in Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="81e5d-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="81e5d-113">Aprire Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="81e5d-113">Open Visual Studio 2017</span></span>
* <span data-ttu-id="81e5d-114">**File > Nuovo > Progetto**</span><span class="sxs-lookup"><span data-stu-id="81e5d-114">**File > New > Project**</span></span>
* <span data-ttu-id="81e5d-115">Scegliere **Installati > Visual C# > .NET Core** dal menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="81e5d-115">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="81e5d-116">Selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="81e5d-116">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="81e5d-117">Immettere **EFGetStarted.AspNetCore.NewDb** come nome e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="81e5d-117">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="81e5d-118">Nella finestra di dialogo **Nuova applicazione Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="81e5d-118">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="81e5d-119">Assicurarsi che le opzioni **.NET Core** e **ASP.NET Core 2.1** siano selezionate negli elenchi a discesa</span><span class="sxs-lookup"><span data-stu-id="81e5d-119">Ensure the options **.NET Core** and **ASP.NET Core 2.1** are selected in the drop down lists</span></span>
  * <span data-ttu-id="81e5d-120">Selezionare il modello di progetto **Applicazione Web (MVC)**</span><span class="sxs-lookup"><span data-stu-id="81e5d-120">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="81e5d-121">Assicurarsi che **Autenticazione** sia impostato su **Nessuna autenticazione**</span><span class="sxs-lookup"><span data-stu-id="81e5d-121">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="81e5d-122">Fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="81e5d-122">Click **OK**</span></span>

<span data-ttu-id="81e5d-123">Avviso: se si usa **Account utente individuali** invece di **Nessuna** per **Autenticazione**, un modello di Entity Framework Core verrà aggiunto al progetto in `Models\IdentityModel.cs`.</span><span class="sxs-lookup"><span data-stu-id="81e5d-123">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="81e5d-124">Usando le tecniche illustrate in questa esercitazione, è possibile scegliere di aggiungere un secondo modello o di estendere questo modello esistente in modo che contenga le classi di entità.</span><span class="sxs-lookup"><span data-stu-id="81e5d-124">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="81e5d-125">Installare Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="81e5d-125">Install Entity Framework Core</span></span>

<span data-ttu-id="81e5d-126">Per installare EF Core, installare il pacchetto per i provider di database di EF Core che si vuole usare come destinazione.</span><span class="sxs-lookup"><span data-stu-id="81e5d-126">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="81e5d-127">Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="81e5d-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="81e5d-128">Per questa esercitazione, non è necessario installare un pacchetto di provider perché l'esercitazione usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="81e5d-128">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="81e5d-129">Il pacchetto del provider SQL Server è incluso nel [metapacchetto Microsoft.AspnetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="81e5d-129">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="create-the-model"></a><span data-ttu-id="81e5d-130">Creare il modello</span><span class="sxs-lookup"><span data-stu-id="81e5d-130">Create the model</span></span>

<span data-ttu-id="81e5d-131">Definire una classe di contesto e le classi di entità che costituiscono il modello:</span><span class="sxs-lookup"><span data-stu-id="81e5d-131">Define a context class and entity classes that make up the model:</span></span>

* <span data-ttu-id="81e5d-132">Fare clic con il pulsante destro del mouse sulla cartella **Models** e scegliere **Aggiungi > Classe**.</span><span class="sxs-lookup"><span data-stu-id="81e5d-132">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="81e5d-133">Immettere **Model.cs** come nome e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="81e5d-133">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="81e5d-134">Sostituire tutti i contenuti del file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="81e5d-134">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="81e5d-135">In una vera app in genere si inserisce ogni classe del modello in un file separato.</span><span class="sxs-lookup"><span data-stu-id="81e5d-135">In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="81e5d-136">Per ragioni di semplicità, in questa esercitazione tutte le classi vengono inserite in un solo file.</span><span class="sxs-lookup"><span data-stu-id="81e5d-136">For the sake of simplicity, this tutorial puts all the classes in one file.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="81e5d-137">Registrare il contesto con l'inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="81e5d-137">Register your context with dependency injection</span></span>

<span data-ttu-id="81e5d-138">I servizi (ad esempio, `BloggingContext`) vengono registrati con l'[inserimento delle dipendenze](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="81e5d-138">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="81e5d-139">Questi servizi vengono quindi forniti ai componenti per cui sono necessari (ad esempio, i controller MVC) tramite proprietà o parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="81e5d-139">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="81e5d-140">Per rendere `BloggingContext` disponibile per i controller MVC, registrarlo come servizio.</span><span class="sxs-lookup"><span data-stu-id="81e5d-140">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="81e5d-141">Aprire **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="81e5d-141">Open **Startup.cs**</span></span>
* <span data-ttu-id="81e5d-142">Aggiungere le istruzioni `using` seguenti:</span><span class="sxs-lookup"><span data-stu-id="81e5d-142">Add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="81e5d-143">Chiamare il metodo `AddDbContext` per registrare il contesto come servizio.</span><span class="sxs-lookup"><span data-stu-id="81e5d-143">Call the `AddDbContext` method to register the context as a service.</span></span>

* <span data-ttu-id="81e5d-144">Aggiungere il codice evidenziato seguente al metodo `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="81e5d-144">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

<span data-ttu-id="81e5d-145">Nota: una vera app in genere inserisce la stringa di connessione in un file di configurazione o in una variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="81e5d-145">Note: A real app would generally put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="81e5d-146">Per ragioni di semplicità, per questa esercitazione viene definita nel codice.</span><span class="sxs-lookup"><span data-stu-id="81e5d-146">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="81e5d-147">Per altre informazioni, vedere [Connection Strings](../../miscellaneous/connection-strings.md) (Stringhe di connessione).</span><span class="sxs-lookup"><span data-stu-id="81e5d-147">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="81e5d-148">Creare il database</span><span class="sxs-lookup"><span data-stu-id="81e5d-148">Create the database</span></span>

<span data-ttu-id="81e5d-149">Dopo avere creato un modello, è possibile usare le [migrazioni](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) per creare un database.</span><span class="sxs-lookup"><span data-stu-id="81e5d-149">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="81e5d-150">**Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="81e5d-150">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="81e5d-151">Eseguire `Add-Migration InitialCreate` per effettuare lo scaffolding di una migrazione per poter creare il set iniziale di tabelle per il modello.</span><span class="sxs-lookup"><span data-stu-id="81e5d-151">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="81e5d-152">Se viene visualizzato l'errore `The term 'add-migration' is not recognized as the name of a cmdlet`, chiudere e riaprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="81e5d-152">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="81e5d-153">Eseguire `Update-Database` per applicare la nuova migrazione al database.</span><span class="sxs-lookup"><span data-stu-id="81e5d-153">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="81e5d-154">Questo comando crea il database prima di applicare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="81e5d-154">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="81e5d-155">Creare un controller</span><span class="sxs-lookup"><span data-stu-id="81e5d-155">Create a controller</span></span>

<span data-ttu-id="81e5d-156">Eseguire lo scaffolding di un controller e delle visualizzazioni per l'entità `Blog`.</span><span class="sxs-lookup"><span data-stu-id="81e5d-156">Scaffold a controller and views for the `Blog` entity.</span></span>

* <span data-ttu-id="81e5d-157">Fare clic con il pulsante destro del mouse sulla cartella **Controllers** in **Esplora soluzioni** e scegliere **Aggiungi > Controller**.</span><span class="sxs-lookup"><span data-stu-id="81e5d-157">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="81e5d-158">Selezionare **Controller MVC con visualizzazioni, che usa Entity Framework** e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="81e5d-158">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="81e5d-159">Impostare **Classe modello** su **Blog** e **Classe contesto dati** su **BloggingContext**.</span><span class="sxs-lookup"><span data-stu-id="81e5d-159">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="81e5d-160">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="81e5d-160">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="81e5d-161">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="81e5d-161">Run the application</span></span>

<span data-ttu-id="81e5d-162">Premere F5 per eseguire e testare l'app.</span><span class="sxs-lookup"><span data-stu-id="81e5d-162">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="81e5d-163">Passare a `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="81e5d-163">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="81e5d-164">Usare il collegamento Crea per creare alcune voci del blog.</span><span class="sxs-lookup"><span data-stu-id="81e5d-164">Use the create link to create some blog entries.</span></span> <span data-ttu-id="81e5d-165">Testare i dettagli ed eliminare i collegamenti.</span><span class="sxs-lookup"><span data-stu-id="81e5d-165">Test the details and delete links.</span></span>

![immagine](_static/create.png)

![immagine](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="81e5d-168">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="81e5d-168">Additional Resources</span></span>

* <span data-ttu-id="81e5d-169">[EF - Nuovo database con SQLite](xref:core/get-started/netcore/new-db-sqlite): esercitazione su EF per la console multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="81e5d-169">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="81e5d-170">Introduzione ad ASP.NET Core MVC in Mac o Linux</span><span class="sxs-lookup"><span data-stu-id="81e5d-170">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="81e5d-171">Introduzione ad ASP.NET Core MVC con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="81e5d-171">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="81e5d-172">Introduzione ad ASP.NET Core ed Entity Framework Core con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="81e5d-172">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
