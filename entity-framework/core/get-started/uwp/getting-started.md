---
title: Guida introduttiva alla piattaforma UWP - Nuovo database - EF Core
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996910"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="354e0-102">Introduzione a EF Core nella piattaforma UWP (Universal Windows Platform) con un nuovo database</span><span class="sxs-lookup"><span data-stu-id="354e0-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="354e0-103">In questa esercitazione si compila un'applicazione UWP (Universal Windows Platform) che esegue l'accesso ai dati di base in un database SQLite locale usando Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="354e0-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="354e0-104">[Visualizzare l'esempio di questo articolo su GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="354e0-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="354e0-105">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="354e0-105">Prerequisites</span></span>

* <span data-ttu-id="354e0-106">[Windows 10 Fall Creators Update (10.0; Build 16299) o successive](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="354e0-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="354e0-107">[Visual Studio 2017 versione 15.7 o successiva](https://www.visualstudio.com/downloads/) con il carico di lavoro **Sviluppo per la piattaforma UWP (Universal Windows Platform)**.</span><span class="sxs-lookup"><span data-stu-id="354e0-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="354e0-108">[.NET Core 2.1 SDK o versione successiva](https://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="354e0-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

## <a name="create-a-model-project"></a><span data-ttu-id="354e0-109">Creare un progetto modello</span><span class="sxs-lookup"><span data-stu-id="354e0-109">Create a model project</span></span>

> [!IMPORTANT]
> <span data-ttu-id="354e0-110">A causa delle limitazioni nell'interazione degli strumenti di .NET Core con i progetti della piattaforma UWP, il modello deve essere inserito in un progetto non UWP per poter eseguire i comandi delle migrazioni nella **console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="354e0-110">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the **Package Manager Console** (PMC)</span></span>

* <span data-ttu-id="354e0-111">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="354e0-111">Open Visual Studio</span></span>

* <span data-ttu-id="354e0-112">**File > Nuovo > Progetto**</span><span class="sxs-lookup"><span data-stu-id="354e0-112">**File > New > Project**</span></span>

* <span data-ttu-id="354e0-113">Scegliere **Installati > Visual C# > .NET Standard** dal menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="354e0-113">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="354e0-114">Selezionare il modello **Libreria di classi (.NET Standard)**.</span><span class="sxs-lookup"><span data-stu-id="354e0-114">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="354e0-115">Assegnare al progetto il nome *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="354e0-115">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="354e0-116">Assegnare alla soluzione il nome *Blogging*.</span><span class="sxs-lookup"><span data-stu-id="354e0-116">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="354e0-117">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="354e0-117">Click **OK**.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="354e0-118">Installare Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="354e0-118">Install Entity Framework Core</span></span>

<span data-ttu-id="354e0-119">Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione.</span><span class="sxs-lookup"><span data-stu-id="354e0-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="354e0-120">Questa esercitazione usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="354e0-120">This tutorial uses SQLite.</span></span> <span data-ttu-id="354e0-121">Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="354e0-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="354e0-122">**Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="354e0-122">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="354e0-123">Eseguire `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="354e0-123">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="354e0-124">Più avanti in questa esercitazione si usano alcuni strumenti di Entity Framework Core per gestire il database.</span><span class="sxs-lookup"><span data-stu-id="354e0-124">Later in this tutorial you will be using some Entity Framework Core tools to maintain the database.</span></span> <span data-ttu-id="354e0-125">Installare quindi anche il pacchetto degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="354e0-125">So install the tools package as well.</span></span>

* <span data-ttu-id="354e0-126">Eseguire `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="354e0-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="354e0-127">Creare il modello</span><span class="sxs-lookup"><span data-stu-id="354e0-127">Create the model</span></span>

<span data-ttu-id="354e0-128">È ora possibile definire un contesto e le classi di entità che costituiscono il modello.</span><span class="sxs-lookup"><span data-stu-id="354e0-128">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="354e0-129">Eliminare *Class1.cs*.</span><span class="sxs-lookup"><span data-stu-id="354e0-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="354e0-130">Creare *Model.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="354e0-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="354e0-131">Creare un nuovo progetto UWP</span><span class="sxs-lookup"><span data-stu-id="354e0-131">Create a new UWP project</span></span>

* <span data-ttu-id="354e0-132">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla soluzione, quindi scegliere **Aggiungi > Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="354e0-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="354e0-133">Scegliere **Installati > Visual C# > Universale di Windows** dal menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="354e0-133">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="354e0-134">Selezionare il modello di progetto **App vuota (Universale di Windows)**.</span><span class="sxs-lookup"><span data-stu-id="354e0-134">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="354e0-135">Assegnare al progetto il nome *Blogging.UWP* e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="354e0-135">Name the project *Blogging.UWP*, and click **OK**</span></span>

* <span data-ttu-id="354e0-136">Impostare le versioni di destinazione e minima almeno su **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span><span class="sxs-lookup"><span data-stu-id="354e0-136">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="354e0-137">Creare la migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="354e0-137">Create the initial migration</span></span>

<span data-ttu-id="354e0-138">Ora che è presente un modello, è possibile configurare l'app in modo che crei un database alla prima esecuzione.</span><span class="sxs-lookup"><span data-stu-id="354e0-138">Now that you have a model, set up the app to create a database the first time it runs.</span></span> <span data-ttu-id="354e0-139">In questa sezione si crea la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="354e0-139">In this section, you create the initial migration.</span></span> <span data-ttu-id="354e0-140">Nella sezione seguente si aggiunge il codice che applica questa migrazione all'avvio dell'app.</span><span class="sxs-lookup"><span data-stu-id="354e0-140">In the following section, you add code that applies this migration when the app starts.</span></span>

<span data-ttu-id="354e0-141">Gli strumenti di migrazione richiedono un progetto di avvio non UWP. Per iniziare, creare tale progetto.</span><span class="sxs-lookup"><span data-stu-id="354e0-141">Migrations tools require a non-UWP startup project, so create that first.</span></span>

* <span data-ttu-id="354e0-142">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla soluzione, quindi scegliere **Aggiungi > Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="354e0-142">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="354e0-143">Scegliere **Installati > Visual C# > .NET Core** dal menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="354e0-143">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="354e0-144">Selezionare il modello di progetto **App Console (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="354e0-144">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="354e0-145">Denominare il progetto *Blogging.Migrations.Startup* e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="354e0-145">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="354e0-146">Aggiungere un riferimento progetto dal progetto *Blogging.Migrations.Startup* al progetto *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="354e0-146">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

<span data-ttu-id="354e0-147">Ora è possibile creare la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="354e0-147">Now you can create your initial migration.</span></span>

* <span data-ttu-id="354e0-148">**Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="354e0-148">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="354e0-149">Selezionare il progetto *Blogging.Model* come **Progetto predefinito**.</span><span class="sxs-lookup"><span data-stu-id="354e0-149">Select the *Blogging.Model* project as the **Default project**.</span></span>

* <span data-ttu-id="354e0-150">In **Esplora soluzioni** impostare il progetto *Blogging.Migrations.Startup* come progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="354e0-150">In **Solution Explorer**, set the *Blogging.Migrations.Startup* project as the startup project.</span></span>

* <span data-ttu-id="354e0-151">Eseguire `Add-Migration InitialCreate`.</span><span class="sxs-lookup"><span data-stu-id="354e0-151">Run `Add-Migration InitialCreate`.</span></span>

  <span data-ttu-id="354e0-152">Questo comando esegue lo scaffolding di una migrazione che crea il set iniziale di tabelle per il modello.</span><span class="sxs-lookup"><span data-stu-id="354e0-152">This command scaffolds a migration that creates the initial set of tables for your model.</span></span>

## <a name="create-the-database-on-app-startup"></a><span data-ttu-id="354e0-153">Creare il database all'avvio dell'app</span><span class="sxs-lookup"><span data-stu-id="354e0-153">Create the database on app startup</span></span>

<span data-ttu-id="354e0-154">Poiché si vuole creare il database nel dispositivo in cui viene eseguita l'app, aggiungere codice per applicare eventuali migrazioni in sospeso al database locale all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="354e0-154">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="354e0-155">In questo modo il database locale viene creato alla prima esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="354e0-155">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="354e0-156">Aggiungere un riferimento progetto dal progetto *Blogging.UWP* al progetto *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="354e0-156">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="354e0-157">Aprire *App.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="354e0-157">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="354e0-158">Aggiungere il codice evidenziato per applicare eventuali migrazioni in sospeso.</span><span class="sxs-lookup"><span data-stu-id="354e0-158">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="354e0-159">Se si apportano modifiche al modello, usare il comando `Add-Migration` per eseguire lo scaffolding di una nuova migrazione e applicare le modifiche corrispondenti al database.</span><span class="sxs-lookup"><span data-stu-id="354e0-159">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="354e0-160">Eventuali migrazioni in sospeso verranno applicate al database locale in ogni dispositivo all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="354e0-160">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="354e0-161">EF usa una tabella `__EFMigrationsHistory` nel database per tenere traccia delle migrazioni che sono già state applicate al database.</span><span class="sxs-lookup"><span data-stu-id="354e0-161">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="354e0-162">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="354e0-162">Use the model</span></span>

<span data-ttu-id="354e0-163">È ora possibile usare il modello per eseguire l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="354e0-163">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="354e0-164">Aprire *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="354e0-164">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="354e0-165">Aggiungere il gestore di caricamento pagina e il contenuto dell'interfaccia utente evidenziati sotto</span><span class="sxs-lookup"><span data-stu-id="354e0-165">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="354e0-166">Ora aggiungere il codice per associare l'interfaccia utente al database</span><span class="sxs-lookup"><span data-stu-id="354e0-166">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="354e0-167">Aprire *MainPage.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="354e0-167">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="354e0-168">Aggiungere il codice evidenziato nel listato seguente:</span><span class="sxs-lookup"><span data-stu-id="354e0-168">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="354e0-169">È ora possibile eseguire l'applicazione per vederla in azione.</span><span class="sxs-lookup"><span data-stu-id="354e0-169">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="354e0-170">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto *Blogging.UWP* e selezionare **Distribuisci**.</span><span class="sxs-lookup"><span data-stu-id="354e0-170">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="354e0-171">Impostare *Blogging.UWP* come progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="354e0-171">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="354e0-172">**Debug > Avvia senza eseguire debug**</span><span class="sxs-lookup"><span data-stu-id="354e0-172">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="354e0-173">L'app viene compilata ed eseguita.</span><span class="sxs-lookup"><span data-stu-id="354e0-173">The app builds and runs.</span></span>

* <span data-ttu-id="354e0-174">Immettere un URL e fare clic sul pulsante **Aggiungi**</span><span class="sxs-lookup"><span data-stu-id="354e0-174">Enter a URL and click the **Add** button</span></span>

  ![immagine](_static/create.png)

  ![immagine](_static/list.png)

  <span data-ttu-id="354e0-177">È</span><span class="sxs-lookup"><span data-stu-id="354e0-177">Tada!</span></span> <span data-ttu-id="354e0-178">ora disponibile una semplice app UWP che esegue Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="354e0-178">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="354e0-179">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="354e0-179">Next steps</span></span>

<span data-ttu-id="354e0-180">Per le informazioni sulle prestazioni e la compatibilità che è necessario conoscere quando si usa Entity Framework Core con UWP, vedere [Implementazioni di .NET supportate da Entity Framework Core](../../platforms/index.md#universal-windows-platform).</span><span class="sxs-lookup"><span data-stu-id="354e0-180">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="354e0-181">Per altre informazioni sulle funzionalità di Entity Framework Core, vedere gli altri articoli di questa documentazione.</span><span class="sxs-lookup"><span data-stu-id="354e0-181">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
