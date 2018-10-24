---
title: Guida introduttiva alla piattaforma UWP - Nuovo database - EF Core
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 48d26adbe17e4734753a7ada547b9c13317bef0d
ms.sourcegitcommit: 8b42045cd21f80f425a92f5e4e9dd4972a31720b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/14/2018
ms.locfileid: "49315620"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="b0147-102">Introduzione a EF Core nella piattaforma UWP (Universal Windows Platform) con un nuovo database</span><span class="sxs-lookup"><span data-stu-id="b0147-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="b0147-103">In questa esercitazione si compila un'applicazione UWP (Universal Windows Platform) che esegue l'accesso ai dati di base in un database SQLite locale usando Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b0147-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="b0147-104">[Visualizzare l'esempio di questo articolo su GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="b0147-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b0147-105">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="b0147-105">Prerequisites</span></span>

* <span data-ttu-id="b0147-106">[Windows 10 Fall Creators Update (10.0; Build 16299) o successive](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="b0147-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="b0147-107">[Visual Studio 2017 versione 15.7 o successiva](https://www.visualstudio.com/downloads/) con il carico di lavoro **Sviluppo per la piattaforma UWP (Universal Windows Platform)**.</span><span class="sxs-lookup"><span data-stu-id="b0147-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="b0147-108">[.NET Core 2.1 SDK o versione successiva](https://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="b0147-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b0147-109">Questa esercitazione usa i comandi per le [migrazioni](xref:core/managing-schemas/migrations/index) di Entity Framework Core per creare e aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="b0147-109">This tutorial uses Entity Framework Core [migrations](xref:core/managing-schemas/migrations/index) commands to create and update the schema of the database.</span></span>
> <span data-ttu-id="b0147-110">Questi comandi non funzionano direttamente con i progetti UWP.</span><span class="sxs-lookup"><span data-stu-id="b0147-110">These commands don't work directly with UWP projects.</span></span>
> <span data-ttu-id="b0147-111">Per questo motivo, il modello di dati dell'applicazione viene inserito in un progetto di libreria condivisa e viene usata un'applicazione console .NET Core separata per eseguire i comandi.</span><span class="sxs-lookup"><span data-stu-id="b0147-111">For this reason, the application's data model is placed in a shared library project, and a separate .NET Core console application is used to run the commands.</span></span>

## <a name="create-a-library-project-to-hold-the-data-model"></a><span data-ttu-id="b0147-112">Creare un progetto di libreria per contenere il modello di dati</span><span class="sxs-lookup"><span data-stu-id="b0147-112">Create a library project to hold the data model</span></span>

* <span data-ttu-id="b0147-113">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b0147-113">Open Visual Studio</span></span>

* <span data-ttu-id="b0147-114">**File > Nuovo > Progetto**</span><span class="sxs-lookup"><span data-stu-id="b0147-114">**File > New > Project**</span></span>

* <span data-ttu-id="b0147-115">Scegliere **Installati > Visual C# > .NET Standard** dal menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="b0147-115">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="b0147-116">Selezionare il modello **Libreria di classi (.NET Standard)**.</span><span class="sxs-lookup"><span data-stu-id="b0147-116">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="b0147-117">Assegnare al progetto il nome *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="b0147-117">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="b0147-118">Assegnare alla soluzione il nome *Blogging*.</span><span class="sxs-lookup"><span data-stu-id="b0147-118">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="b0147-119">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b0147-119">Click **OK**.</span></span>

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a><span data-ttu-id="b0147-120">Installare il runtime di Entity Framework Core nel progetto del modello di dati</span><span class="sxs-lookup"><span data-stu-id="b0147-120">Install Entity Framework Core runtime in the data model project</span></span>

<span data-ttu-id="b0147-121">Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione.</span><span class="sxs-lookup"><span data-stu-id="b0147-121">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="b0147-122">Questa esercitazione usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="b0147-122">This tutorial uses SQLite.</span></span> <span data-ttu-id="b0147-123">Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="b0147-123">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="b0147-124">**Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="b0147-124">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="b0147-125">Assicurarsi che il progetto di libreria *Blogging.Model* sia selezionato come **progetto predefinito** nella Console di Gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="b0147-125">Make sure that the library project *Blogging.Model* is selected as the **Default Project** in the Package Manager Console.</span></span>

* <span data-ttu-id="b0147-126">Eseguire `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="b0147-126">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="b0147-127">Creare il modello di dati</span><span class="sxs-lookup"><span data-stu-id="b0147-127">Create the data model</span></span>

<span data-ttu-id="b0147-128">È ora possibile definire l'oggetto *DbContext* e le classi di entità che costituiscono il modello.</span><span class="sxs-lookup"><span data-stu-id="b0147-128">Now it's time to define the *DbContext* and entity classes that make up the model.</span></span>

* <span data-ttu-id="b0147-129">Eliminare *Class1.cs*.</span><span class="sxs-lookup"><span data-stu-id="b0147-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="b0147-130">Creare *Model.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b0147-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a><span data-ttu-id="b0147-131">Creare un nuovo progetto console per eseguire i comandi delle migrazioni</span><span class="sxs-lookup"><span data-stu-id="b0147-131">Create a new console project to run migrations commands</span></span>

* <span data-ttu-id="b0147-132">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla soluzione, quindi scegliere **Aggiungi > Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="b0147-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="b0147-133">Scegliere **Installati > Visual C# > .NET Core** dal menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="b0147-133">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="b0147-134">Selezionare il modello di progetto **App Console (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="b0147-134">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="b0147-135">Denominare il progetto *Blogging.Migrations.Startup* e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b0147-135">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="b0147-136">Aggiungere un riferimento progetto dal progetto *Blogging.Migrations.Startup* al progetto *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="b0147-136">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a><span data-ttu-id="b0147-137">Installare gli strumenti di Entity Framework Core nel progetto di avvio delle migrazioni</span><span class="sxs-lookup"><span data-stu-id="b0147-137">Install Entity Framework Core tools in the migrations startup project</span></span>

<span data-ttu-id="b0147-138">Per abilitare i comandi delle migrazioni di EF Core nella Console di Gestione pacchetti, installare il pacchetto di strumenti di EF Core nell'applicazione console.</span><span class="sxs-lookup"><span data-stu-id="b0147-138">To enable the EF Core migration commands in the Package Manager Console, install the EF Core tools package in the console application.</span></span>

* <span data-ttu-id="b0147-139">**Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="b0147-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="b0147-140">Eseguire `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span><span class="sxs-lookup"><span data-stu-id="b0147-140">Run `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="b0147-141">Creare la migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="b0147-141">Create the initial migration</span></span>

 <span data-ttu-id="b0147-142">Creare la migrazione iniziale, specificando l'applicazione console come progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="b0147-142">Create the initial migration, specifying the console application as the startup project.</span></span>

* <span data-ttu-id="b0147-143">Eseguire `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span><span class="sxs-lookup"><span data-stu-id="b0147-143">Run `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span></span>

<span data-ttu-id="b0147-144">Questo comando esegue lo scaffolding di una migrazione che crea il set iniziale di tabelle del database per il modello di dati.</span><span class="sxs-lookup"><span data-stu-id="b0147-144">This command scaffolds a migration that creates the initial set of database tables for your data model.</span></span>

## <a name="create-the-uwp-project"></a><span data-ttu-id="b0147-145">Creare il progetto UWP</span><span class="sxs-lookup"><span data-stu-id="b0147-145">Create the UWP project</span></span>

* <span data-ttu-id="b0147-146">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla soluzione, quindi scegliere **Aggiungi > Nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="b0147-146">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="b0147-147">Scegliere **Installati > Visual C# > Universale di Windows** dal menu a sinistra.</span><span class="sxs-lookup"><span data-stu-id="b0147-147">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="b0147-148">Selezionare il modello di progetto **App vuota (Universale di Windows)**.</span><span class="sxs-lookup"><span data-stu-id="b0147-148">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="b0147-149">Assegnare al progetto il nome *Blogging.UWP* e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="b0147-149">Name the project *Blogging.UWP*, and click **OK**</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b0147-150">Impostare le versioni di destinazione e minima almeno su **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span><span class="sxs-lookup"><span data-stu-id="b0147-150">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>
> <span data-ttu-id="b0147-151">Le versioni precedenti di Windows 10 non supportano .NET Standard 2.0, che è necessario per Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b0147-151">Previous  versions of Windows 10 do not support .NET Standard 2.0, which is required by Entity Framework Core.</span></span>

## <a name="add-code-to-create-the-database-on-application-startup"></a><span data-ttu-id="b0147-152">Aggiungere il codice per creare il database all'avvio dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="b0147-152">Add code to create the database on application startup</span></span>

<span data-ttu-id="b0147-153">Poiché si vuole creare il database nel dispositivo in cui viene eseguita l'app, aggiungere codice per applicare eventuali migrazioni in sospeso al database locale all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b0147-153">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="b0147-154">In questo modo il database locale viene creato alla prima esecuzione dell'app.</span><span class="sxs-lookup"><span data-stu-id="b0147-154">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="b0147-155">Aggiungere un riferimento progetto dal progetto *Blogging.UWP* al progetto *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="b0147-155">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="b0147-156">Aprire *App.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="b0147-156">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="b0147-157">Aggiungere il codice evidenziato per applicare eventuali migrazioni in sospeso.</span><span class="sxs-lookup"><span data-stu-id="b0147-157">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="b0147-158">Se si apportano modifiche al modello, usare il comando `Add-Migration` per eseguire lo scaffolding di una nuova migrazione e applicare le modifiche corrispondenti al database.</span><span class="sxs-lookup"><span data-stu-id="b0147-158">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="b0147-159">Eventuali migrazioni in sospeso verranno applicate al database locale in ogni dispositivo all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b0147-159">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="b0147-160">EF Core usa una tabella `__EFMigrationsHistory` nel database per tenere traccia delle migrazioni che sono già state applicate al database.</span><span class="sxs-lookup"><span data-stu-id="b0147-160">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-data-model"></a><span data-ttu-id="b0147-161">Usare il modello di dati</span><span class="sxs-lookup"><span data-stu-id="b0147-161">Use the data model</span></span>

<span data-ttu-id="b0147-162">È ora possibile usare EF Core per eseguire l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="b0147-162">You can now use EF Core to perform data access.</span></span>

* <span data-ttu-id="b0147-163">Aprire *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="b0147-163">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="b0147-164">Aggiungere il gestore di caricamento pagina e il contenuto dell'interfaccia utente evidenziati sotto</span><span class="sxs-lookup"><span data-stu-id="b0147-164">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="b0147-165">Ora aggiungere il codice per associare l'interfaccia utente al database</span><span class="sxs-lookup"><span data-stu-id="b0147-165">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="b0147-166">Aprire *MainPage.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="b0147-166">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="b0147-167">Aggiungere il codice evidenziato nel listato seguente:</span><span class="sxs-lookup"><span data-stu-id="b0147-167">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="b0147-168">È ora possibile eseguire l'applicazione per vederla in azione.</span><span class="sxs-lookup"><span data-stu-id="b0147-168">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="b0147-169">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto *Blogging.UWP* e selezionare **Distribuisci**.</span><span class="sxs-lookup"><span data-stu-id="b0147-169">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="b0147-170">Impostare *Blogging.UWP* come progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="b0147-170">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="b0147-171">**Debug > Avvia senza eseguire debug**</span><span class="sxs-lookup"><span data-stu-id="b0147-171">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="b0147-172">L'app viene compilata ed eseguita.</span><span class="sxs-lookup"><span data-stu-id="b0147-172">The app builds and runs.</span></span>

* <span data-ttu-id="b0147-173">Immettere un URL e fare clic sul pulsante **Aggiungi**</span><span class="sxs-lookup"><span data-stu-id="b0147-173">Enter a URL and click the **Add** button</span></span>

  ![immagine](_static/create.png)

  ![immagine](_static/list.png)

  <span data-ttu-id="b0147-176">È</span><span class="sxs-lookup"><span data-stu-id="b0147-176">Tada!</span></span> <span data-ttu-id="b0147-177">ora disponibile una semplice app UWP che esegue Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b0147-177">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b0147-178">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="b0147-178">Next steps</span></span>

<span data-ttu-id="b0147-179">Per le informazioni sulle prestazioni e la compatibilità che è necessario conoscere quando si usa Entity Framework Core con UWP, vedere [Implementazioni di .NET supportate da Entity Framework Core](../../platforms/index.md#universal-windows-platform).</span><span class="sxs-lookup"><span data-stu-id="b0147-179">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="b0147-180">Per altre informazioni sulle funzionalità di Entity Framework Core, vedere gli altri articoli di questa documentazione.</span><span class="sxs-lookup"><span data-stu-id="b0147-180">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
