---
title: Guida introduttiva alla piattaforma UWP - Nuovo database - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/01/2017
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="c934d-102">Introduzione a EF Core nella piattaforma UWP (Universal Windows Platform) con un nuovo database</span><span class="sxs-lookup"><span data-stu-id="c934d-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

> [!NOTE]
> <span data-ttu-id="c934d-103">Questa esercitazione usa EF Core 2.0.1 (rilasciato con ASP.NET Core e .NET Core SDK 2.0.3).</span><span class="sxs-lookup"><span data-stu-id="c934d-103">This tutorial uses EF Core 2.0.1 (released alongside ASP.NET Core and .NET Core SDK 2.0.3).</span></span> <span data-ttu-id="c934d-104">In EF Core 2.0.0 mancano alcune importanti correzioni di bug necessarie per un'esperienza ottimale con la piattaforma UWP.</span><span class="sxs-lookup"><span data-stu-id="c934d-104">EF Core 2.0.0 lacks some crucial bug fixes required for a good UWP experience.</span></span>

<span data-ttu-id="c934d-105">In questa procedura dettagliata verrà compilata un'applicazione UWP (Universal Windows Platform) che esegue l'accesso ai dati di base in un database SQLite locale usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c934d-105">In this walkthrough, you will build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c934d-106">**Considerare la possibilità di evitare i tipi anonimi nelle query LINQ sulla piattaforma UWP**.</span><span class="sxs-lookup"><span data-stu-id="c934d-106">**Consider avoiding anonymous types in LINQ queries on UWP**.</span></span> <span data-ttu-id="c934d-107">Per distribuire un'applicazione UWP nell'App Store, l'applicazione deve essere compilata con .NET Native.</span><span class="sxs-lookup"><span data-stu-id="c934d-107">Deploying a UWP application to the app store requires your application to be compiled with .NET Native.</span></span> <span data-ttu-id="c934d-108">Le query con tipi anonimi hanno prestazioni peggiori in .NET Native.</span><span class="sxs-lookup"><span data-stu-id="c934d-108">Queries with anonymous types have worse performance on .NET Native.</span></span>

> [!TIP]
> <span data-ttu-id="c934d-109">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="c934d-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c934d-110">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c934d-110">Prerequisites</span></span>

<span data-ttu-id="c934d-111">Per completare questa procedura dettagliata, sono necessari gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c934d-111">The following items are required to complete this walkthrough:</span></span>

* <span data-ttu-id="c934d-112">[Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span><span class="sxs-lookup"><span data-stu-id="c934d-112">[Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span></span>

* <span data-ttu-id="c934d-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="c934d-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

* <span data-ttu-id="c934d-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) versione 15.4 o successiva con il carico di lavoro **Sviluppo di app per la piattaforma UWP (Universal Windows Platform)**.</span><span class="sxs-lookup"><span data-stu-id="c934d-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.4 or later with the **Universal Windows Platform Development** workload.</span></span>

## <a name="create-a-new-model-project"></a><span data-ttu-id="c934d-115">Creare un nuovo progetto modello</span><span class="sxs-lookup"><span data-stu-id="c934d-115">Create a new model project</span></span>

> [!WARNING]
> <span data-ttu-id="c934d-116">A causa delle limitazioni nell'interazione degli strumenti di .NET Core con i progetti della piattaforma UWP, il modello deve essere inserito in un progetto non UWP per poter eseguire i comandi delle migrazioni nella console di Gestione pacchetti</span><span class="sxs-lookup"><span data-stu-id="c934d-116">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the Package Manager Console</span></span>

* <span data-ttu-id="c934d-117">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c934d-117">Open Visual Studio</span></span>

* <span data-ttu-id="c934d-118">File > Nuovo > Progetto</span><span class="sxs-lookup"><span data-stu-id="c934d-118">File > New > Project...</span></span>

* <span data-ttu-id="c934d-119">Scegliere Modelli > Visual C# dal menu a sinistra</span><span class="sxs-lookup"><span data-stu-id="c934d-119">From the left menu select Templates > Visual C#</span></span>

* <span data-ttu-id="c934d-120">Selezionare il modello di progetto **Libreria di classi (.NET Standard)**</span><span class="sxs-lookup"><span data-stu-id="c934d-120">Select the **Class Library (.NET Standard)** project template</span></span>

* <span data-ttu-id="c934d-121">Specificare un nome per il progetto e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="c934d-121">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="c934d-122">Installare Entity Framework</span><span class="sxs-lookup"><span data-stu-id="c934d-122">Install Entity Framework</span></span>

<span data-ttu-id="c934d-123">Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione.</span><span class="sxs-lookup"><span data-stu-id="c934d-123">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="c934d-124">Questa procedura dettagliata usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="c934d-124">This walkthrough uses SQLite.</span></span> <span data-ttu-id="c934d-125">Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="c934d-125">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="c934d-126">Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti</span><span class="sxs-lookup"><span data-stu-id="c934d-126">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="c934d-127">Eseguire `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="c934d-127">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="c934d-128">Più avanti in questa procedura dettagliata verrà anche usato Entity Framework Tools per gestire il database,</span><span class="sxs-lookup"><span data-stu-id="c934d-128">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="c934d-129">quindi verrà anche installato il pacchetto di strumenti.</span><span class="sxs-lookup"><span data-stu-id="c934d-129">So we will install the tools package as well.</span></span>

* <span data-ttu-id="c934d-130">Eseguire `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="c934d-130">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

* <span data-ttu-id="c934d-131">Modificare il file con estensione csproj e sostituire `<TargetFramework>netstandard2.0</TargetFramework>` con `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`</span><span class="sxs-lookup"><span data-stu-id="c934d-131">Edit the .csproj file and replace `<TargetFramework>netstandard2.0</TargetFramework>` with `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="c934d-132">Creare il modello</span><span class="sxs-lookup"><span data-stu-id="c934d-132">Create your model</span></span>

<span data-ttu-id="c934d-133">È ora possibile definire un contesto e le classi di entità che costituiscono il modello.</span><span class="sxs-lookup"><span data-stu-id="c934d-133">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="c934d-134">Progetto > Aggiungi classe</span><span class="sxs-lookup"><span data-stu-id="c934d-134">Project > Add Class...</span></span>

* <span data-ttu-id="c934d-135">Immettere *Model.cs* come nome e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="c934d-135">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="c934d-136">Sostituire tutti i contenuti del file con il codice seguente</span><span class="sxs-lookup"><span data-stu-id="c934d-136">Replace the contents of the file with the following code</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="c934d-137">Creare un nuovo progetto UWP</span><span class="sxs-lookup"><span data-stu-id="c934d-137">Create a new UWP project</span></span>

* <span data-ttu-id="c934d-138">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c934d-138">Open Visual Studio</span></span>

* <span data-ttu-id="c934d-139">File > Nuovo > Progetto</span><span class="sxs-lookup"><span data-stu-id="c934d-139">File > New > Project...</span></span>

* <span data-ttu-id="c934d-140">Scegliere Modelli > Visual C# > Universale di Windows dal menu a sinistra</span><span class="sxs-lookup"><span data-stu-id="c934d-140">From the left menu select Templates > Visual C# > Windows Universal</span></span>

* <span data-ttu-id="c934d-141">Selezionare il modello di progetto **App vuota (Windows universale)**</span><span class="sxs-lookup"><span data-stu-id="c934d-141">Select the **Blank App (Universal Windows)** project template</span></span>

* <span data-ttu-id="c934d-142">Specificare un nome per il progetto e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="c934d-142">Give the project a name and click **OK**</span></span>

* <span data-ttu-id="c934d-143">Impostare le versioni di destinazione e minima almeno su `Windows 10 Fall Creators Update (10.0; build 16299.0)`</span><span class="sxs-lookup"><span data-stu-id="c934d-143">Set the target and minimum versions to at least `Windows 10 Fall Creators Update (10.0; build 16299.0)`</span></span>

## <a name="create-your-database"></a><span data-ttu-id="c934d-144">Creare il database</span><span class="sxs-lookup"><span data-stu-id="c934d-144">Create your database</span></span>

<span data-ttu-id="c934d-145">Dopo avere creato un modello, è possibile usare le migrazioni per creare un database.</span><span class="sxs-lookup"><span data-stu-id="c934d-145">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="c934d-146">Strumenti –> Gestione pacchetti NuGet –> Console di Gestione pacchetti</span><span class="sxs-lookup"><span data-stu-id="c934d-146">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="c934d-147">Selezionare il progetto modello come progetto predefinito e impostarlo come progetto di avvio</span><span class="sxs-lookup"><span data-stu-id="c934d-147">Select the model project as the Default project and set it as the startup project</span></span>

* <span data-ttu-id="c934d-148">Eseguire `Add-Migration MyFirstMigration` per effettuare lo scaffolding di una migrazione per poter creare il set iniziale di tabelle per il modello.</span><span class="sxs-lookup"><span data-stu-id="c934d-148">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

<span data-ttu-id="c934d-149">Poiché si vuole creare il database nel dispositivo in cui viene eseguita l'app, verrà aggiunto il codice per applicare eventuali migrazioni in sospeso al database locale all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c934d-149">Since we want the database to be created on the device that the app runs on, we will add some code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="c934d-150">In questo modo, alla prima esecuzione dell'app, il database locale verrà creato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c934d-150">The first time that the app runs, this will take care of creating the local database for us.</span></span>

* <span data-ttu-id="c934d-151">In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **App.xaml** e scegliere **Visualizza codice**</span><span class="sxs-lookup"><span data-stu-id="c934d-151">Right-click on **App.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="c934d-152">Aggiungere l'istruzione using evidenziata all'inizio del file</span><span class="sxs-lookup"><span data-stu-id="c934d-152">Add the highlighted using to the start of the file</span></span>

* <span data-ttu-id="c934d-153">Aggiungere il codice evidenziato per applicare eventuali migrazioni in sospeso</span><span class="sxs-lookup"><span data-stu-id="c934d-153">Add the highlighted code to apply any pending migrations</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> <span data-ttu-id="c934d-154">Se si apporteranno modifiche al modello in futuro, sarà possibile usare il comando `Add-Migration` per eseguire lo scaffolding di una nuova migrazione e poter applicare le corrispondenti modifiche al database.</span><span class="sxs-lookup"><span data-stu-id="c934d-154">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="c934d-155">Eventuali migrazioni in sospeso verranno applicate al database locale in ogni dispositivo all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c934d-155">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="c934d-156">EF usa una tabella `__EFMigrationsHistory` nel database per tenere traccia delle migrazioni che sono già state applicate al database.</span><span class="sxs-lookup"><span data-stu-id="c934d-156">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="c934d-157">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="c934d-157">Use your model</span></span>

<span data-ttu-id="c934d-158">È ora possibile usare il modello per eseguire l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="c934d-158">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="c934d-159">Aprire *MainPage.xaml*</span><span class="sxs-lookup"><span data-stu-id="c934d-159">Open *MainPage.xaml*</span></span>

* <span data-ttu-id="c934d-160">Aggiungere il gestore di caricamento pagina e il contenuto dell'interfaccia utente evidenziati sotto</span><span class="sxs-lookup"><span data-stu-id="c934d-160">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="c934d-161">Verrà ora aggiunto il codice per associare l'interfaccia utente al database</span><span class="sxs-lookup"><span data-stu-id="c934d-161">Now we'll add code to wire up the UI with the database</span></span>

* <span data-ttu-id="c934d-162">In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **MainPage.xaml** e scegliere **Visualizza codice**</span><span class="sxs-lookup"><span data-stu-id="c934d-162">Right-click **MainPage.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="c934d-163">Aggiungere il codice evidenziato nel listato seguente</span><span class="sxs-lookup"><span data-stu-id="c934d-163">Add the highlighted code from the following listing</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

<span data-ttu-id="c934d-164">È ora possibile eseguire l'applicazione per vederla in azione.</span><span class="sxs-lookup"><span data-stu-id="c934d-164">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="c934d-165">Debug > Avvia senza eseguire debug</span><span class="sxs-lookup"><span data-stu-id="c934d-165">Debug > Start Without Debugging</span></span>

* <span data-ttu-id="c934d-166">L'applicazione verrà compilata e avviata</span><span class="sxs-lookup"><span data-stu-id="c934d-166">The application will build and launch</span></span>

* <span data-ttu-id="c934d-167">Immettere un URL e fare clic sul pulsante **Aggiungi**</span><span class="sxs-lookup"><span data-stu-id="c934d-167">Enter a URL and click the **Add** button</span></span>

![image](_static/create.png)

![image](_static/list.png)

## <a name="next-steps"></a><span data-ttu-id="c934d-170">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="c934d-170">Next steps</span></span>

> [!TIP]
> <span data-ttu-id="c934d-171">Le prestazioni di `SaveChanges()` possono essere migliorate implementando [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) nei tipi di entità e usando `ChangeTrackingStrategy.ChangingAndChangedNotifications`.</span><span class="sxs-lookup"><span data-stu-id="c934d-171">`SaveChanges()` performance can be improved by implementing [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types and using `ChangeTrackingStrategy.ChangingAndChangedNotifications`.</span></span>

<span data-ttu-id="c934d-172">È</span><span class="sxs-lookup"><span data-stu-id="c934d-172">Tada!</span></span> <span data-ttu-id="c934d-173">ora disponibile una semplice app UWP che esegue Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c934d-173">You now have a simple UWP app running Entity Framework.</span></span>

<span data-ttu-id="c934d-174">Per altre informazioni sulle funzionalità di Entity Framework, vedere gli altri articoli di questa documentazione.</span><span class="sxs-lookup"><span data-stu-id="c934d-174">Check out other articles in this documentation to learn more about Entity Framework's features.</span></span>
