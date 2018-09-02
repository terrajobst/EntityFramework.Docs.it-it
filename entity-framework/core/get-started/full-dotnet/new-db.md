---
title: Introduzione a .NET Framework - Nuovo database - EF Core
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 66d9011a5978fc3d8253a963bdb9df27848e9ff9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997587"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="c32f0-102">Introduzione a EF Core in .NET Framework con un nuovo database</span><span class="sxs-lookup"><span data-stu-id="c32f0-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="c32f0-103">In questa esercitazione verrà creata un'applicazione console che esegue l'accesso ai dati di base in un database di Microsoft SQL Server usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c32f0-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="c32f0-104">Vengono usate le migrazioni per creare il database da un modello.</span><span class="sxs-lookup"><span data-stu-id="c32f0-104">You use migrations to create the database from a model.</span></span>

<span data-ttu-id="c32f0-105">[Visualizzare l'esempio di questo articolo su GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span><span class="sxs-lookup"><span data-stu-id="c32f0-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c32f0-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c32f0-106">Prerequisites</span></span>

* [<span data-ttu-id="c32f0-107">Visual Studio 2017 versione 15.7 o successive</span><span class="sxs-lookup"><span data-stu-id="c32f0-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a><span data-ttu-id="c32f0-108">Creare un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="c32f0-108">Create a new project</span></span>

* <span data-ttu-id="c32f0-109">Aprire Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="c32f0-109">Open Visual Studio 2017</span></span>

* <span data-ttu-id="c32f0-110">**File > Nuovo > Progetto**</span><span class="sxs-lookup"><span data-stu-id="c32f0-110">**File > New > Project...**</span></span>

* <span data-ttu-id="c32f0-111">Scegliere **Installati > Visual C# > Desktop Windows** dal menu a sinistra</span><span class="sxs-lookup"><span data-stu-id="c32f0-111">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="c32f0-112">Selezionare il modello di progetto **App console (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="c32f0-112">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="c32f0-113">Assicurarsi che il progetto sia destinato a **.NET Framework 4.6.1** o versione successiva</span><span class="sxs-lookup"><span data-stu-id="c32f0-113">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="c32f0-114">Denominare il progetto *ConsoleApp.NewDb* e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="c32f0-114">Name the project *ConsoleApp.NewDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="c32f0-115">Installare Entity Framework</span><span class="sxs-lookup"><span data-stu-id="c32f0-115">Install Entity Framework</span></span>

<span data-ttu-id="c32f0-116">Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione.</span><span class="sxs-lookup"><span data-stu-id="c32f0-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="c32f0-117">Questa esercitazione usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c32f0-117">This tutorial uses SQL Server.</span></span> <span data-ttu-id="c32f0-118">Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="c32f0-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="c32f0-119">Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti</span><span class="sxs-lookup"><span data-stu-id="c32f0-119">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="c32f0-120">Eseguire `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="c32f0-120">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="c32f0-121">Più avanti in questa esercitazione si useranno alcuni degli strumenti in Entity Framework Tools per gestire il database.</span><span class="sxs-lookup"><span data-stu-id="c32f0-121">Later in this tutorial you use some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="c32f0-122">Installare quindi anche il pacchetto degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="c32f0-122">So install the tools package as well.</span></span>

* <span data-ttu-id="c32f0-123">Eseguire `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="c32f0-123">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="c32f0-124">Creare il modello</span><span class="sxs-lookup"><span data-stu-id="c32f0-124">Create the model</span></span>

<span data-ttu-id="c32f0-125">È ora possibile definire un contesto e le classi di entità che costituiscono il modello.</span><span class="sxs-lookup"><span data-stu-id="c32f0-125">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="c32f0-126">**Progetto > Aggiungi classe**</span><span class="sxs-lookup"><span data-stu-id="c32f0-126">**Project > Add Class...**</span></span>

* <span data-ttu-id="c32f0-127">Immettere *Model.cs* come nome e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="c32f0-127">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="c32f0-128">Sostituire tutti i contenuti del file con il codice seguente</span><span class="sxs-lookup"><span data-stu-id="c32f0-128">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> <span data-ttu-id="c32f0-129">In una vera applicazione ogni classe verrebbe inserita in un file separato e la stringa di connessione in un file di configurazione o in una variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="c32f0-129">In a real application you would put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="c32f0-130">Per ragioni di semplicità, in questa esercitazione viene inserito tutto in un solo file.</span><span class="sxs-lookup"><span data-stu-id="c32f0-130">For the sake of simplicity, everything is in a single code file for this tutorial.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="c32f0-131">Creare il database</span><span class="sxs-lookup"><span data-stu-id="c32f0-131">Create the database</span></span>

<span data-ttu-id="c32f0-132">Dopo avere creato un modello, è possibile usare le migrazioni per creare un database.</span><span class="sxs-lookup"><span data-stu-id="c32f0-132">Now that you have a model, you can use migrations to create a database.</span></span>

* <span data-ttu-id="c32f0-133">**Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="c32f0-133">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="c32f0-134">Eseguire `Add-Migration InitialCreate` per effettuare lo scaffolding di una migrazione per creare il set iniziale di tabelle per il modello.</span><span class="sxs-lookup"><span data-stu-id="c32f0-134">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for the model.</span></span>

* <span data-ttu-id="c32f0-135">Eseguire `Update-Database` per applicare la nuova migrazione al database.</span><span class="sxs-lookup"><span data-stu-id="c32f0-135">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="c32f0-136">Poiché il database non esiste ancora, verrà creato automaticamente prima di applicare la migrazione.</span><span class="sxs-lookup"><span data-stu-id="c32f0-136">Because the database doesn't exist yet, it will be created before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="c32f0-137">Se si apportano modifiche al modello, è possibile usare il comando `Add-Migration` per eseguire lo scaffolding di una nuova migrazione per apportare le modifiche dello schema corrispondenti nel database.</span><span class="sxs-lookup"><span data-stu-id="c32f0-137">If you make changes to the model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="c32f0-138">Dopo avere controllato il codice di scaffolding e avere apportato eventuali modifiche necessarie, è possibile usare il comando `Update-Database` per applicare le modifiche al database.</span><span class="sxs-lookup"><span data-stu-id="c32f0-138">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
> <span data-ttu-id="c32f0-139">EF usa una tabella `__EFMigrationsHistory` nel database per tenere traccia delle migrazioni che sono già state applicate al database.</span><span class="sxs-lookup"><span data-stu-id="c32f0-139">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="c32f0-140">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="c32f0-140">Use the model</span></span>

<span data-ttu-id="c32f0-141">È ora possibile usare il modello per eseguire l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="c32f0-141">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="c32f0-142">Aprire *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="c32f0-142">Open *Program.cs*</span></span>

* <span data-ttu-id="c32f0-143">Sostituire tutti i contenuti del file con il codice seguente</span><span class="sxs-lookup"><span data-stu-id="c32f0-143">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* <span data-ttu-id="c32f0-144">**Debug > Avvia senza eseguire debug**</span><span class="sxs-lookup"><span data-stu-id="c32f0-144">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="c32f0-145">Si può notare che un blog viene salvato nel database e quindi che i dettagli di tutti i blog vengono visualizzati nella console.</span><span class="sxs-lookup"><span data-stu-id="c32f0-145">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![immagine](_static/output-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="c32f0-147">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c32f0-147">Additional Resources</span></span>

* [<span data-ttu-id="c32f0-148">EF Core in .NET Framework con un database esistente</span><span class="sxs-lookup"><span data-stu-id="c32f0-148">EF Core on .NET Framework with an existing database</span></span>](xref:core/get-started/full-dotnet/existing-db)
* <span data-ttu-id="c32f0-149">[EF Core in .NET Core con un nuovo database - SQLite](xref:core/get-started/netcore/new-db-sqlite): esercitazione su EF per la console multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="c32f0-149">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
