---
title: Introduzione a .NET Framework - Database esistente - EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: d5c548927b736199c7d6fddc9c74139ca5f6614e
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614415"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="9653f-102">Introduzione a EF Core in .NET Framework con un database esistente</span><span class="sxs-lookup"><span data-stu-id="9653f-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="9653f-103">In questa esercitazione verrà creata un'applicazione console che esegue l'accesso ai dati di base in un database di Microsoft SQL Server usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9653f-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="9653f-104">Si creerà un modello di Entity Framework tramite reverse engineering di un database esistente.</span><span class="sxs-lookup"><span data-stu-id="9653f-104">You create an Entity Framework model by reverse engineering an existing database.</span></span>

<span data-ttu-id="9653f-105">[Visualizzare l'esempio di questo articolo su GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="9653f-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9653f-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="9653f-106">Prerequisites</span></span>

* [<span data-ttu-id="9653f-107">Visual Studio 2017 versione 15.7 o successive</span><span class="sxs-lookup"><span data-stu-id="9653f-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a><span data-ttu-id="9653f-108">Creare il database Blogging</span><span class="sxs-lookup"><span data-stu-id="9653f-108">Create Blogging database</span></span>

<span data-ttu-id="9653f-109">Questa esercitazione usa un database **Blogging** nell'istanza di LocalDb come database esistente.</span><span class="sxs-lookup"><span data-stu-id="9653f-109">This tutorial uses a **Blogging** database on the LocalDb instance as the existing database.</span></span> <span data-ttu-id="9653f-110">Se il database **Blogging** è già stato creato durante un'altra esercitazione, ignorare questi passaggi.</span><span class="sxs-lookup"><span data-stu-id="9653f-110">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="9653f-111">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9653f-111">Open Visual Studio</span></span>

* <span data-ttu-id="9653f-112">**Strumenti > Connetti al database**</span><span class="sxs-lookup"><span data-stu-id="9653f-112">**Tools > Connect to Database...**</span></span>

* <span data-ttu-id="9653f-113">Selezionare **Microsoft SQL Server** e fare clic su **Continua**</span><span class="sxs-lookup"><span data-stu-id="9653f-113">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="9653f-114">Immettere **(localdb)\mssqllocaldb** in **Nome server**</span><span class="sxs-lookup"><span data-stu-id="9653f-114">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="9653f-115">Immettere **master** in **Nome database** e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="9653f-115">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="9653f-116">Il database master viene ora visualizzato sotto **Connessioni dati** in **Esplora server**</span><span class="sxs-lookup"><span data-stu-id="9653f-116">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="9653f-117">Fare clic con il pulsante destro del mouse sul database in **Esplora server** e scegliere **Nuova query**</span><span class="sxs-lookup"><span data-stu-id="9653f-117">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="9653f-118">Copiare lo script riportato di seguito nell'editor di query</span><span class="sxs-lookup"><span data-stu-id="9653f-118">Copy the script listed below into the query editor</span></span>

* <span data-ttu-id="9653f-119">Fare clic con il pulsante destro del mouse sull'editor di query e scegliere **Esegui**</span><span class="sxs-lookup"><span data-stu-id="9653f-119">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="9653f-120">Creare un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="9653f-120">Create a new project</span></span>

* <span data-ttu-id="9653f-121">Aprire Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="9653f-121">Open Visual Studio 2017</span></span>

* <span data-ttu-id="9653f-122">**File > Nuovo > Progetto**</span><span class="sxs-lookup"><span data-stu-id="9653f-122">**File > New > Project...**</span></span>

* <span data-ttu-id="9653f-123">Scegliere **Installati > Visual C# > Desktop Windows** dal menu a sinistra</span><span class="sxs-lookup"><span data-stu-id="9653f-123">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="9653f-124">Selezionare il modello di progetto **App console (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="9653f-124">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="9653f-125">Assicurarsi che il progetto sia destinato a **.NET Framework 4.6.1** o versione successiva</span><span class="sxs-lookup"><span data-stu-id="9653f-125">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="9653f-126">Denominare il progetto *ConsoleApp.ExistingDb* e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="9653f-126">Name the project *ConsoleApp.ExistingDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="9653f-127">Installare Entity Framework</span><span class="sxs-lookup"><span data-stu-id="9653f-127">Install Entity Framework</span></span>

<span data-ttu-id="9653f-128">Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione.</span><span class="sxs-lookup"><span data-stu-id="9653f-128">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="9653f-129">Questa esercitazione usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9653f-129">This tutorial uses SQL Server.</span></span> <span data-ttu-id="9653f-130">Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="9653f-130">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="9653f-131">**Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="9653f-131">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="9653f-132">Eseguire `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="9653f-132">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="9653f-133">Nel passaggio successivo verranno usati alcuni strumenti di Entity Framework per il reverse engineering del database.</span><span class="sxs-lookup"><span data-stu-id="9653f-133">In the next step, you use some Entity Framework Tools to reverse engineer the database.</span></span> <span data-ttu-id="9653f-134">Installare quindi anche il pacchetto degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="9653f-134">So install the tools package as well.</span></span>

* <span data-ttu-id="9653f-135">Eseguire `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="9653f-135">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-the-model"></a><span data-ttu-id="9653f-136">Reverse engineering del modello</span><span class="sxs-lookup"><span data-stu-id="9653f-136">Reverse engineer the model</span></span>

<span data-ttu-id="9653f-137">È ora possibile creare il modello di EF in base a un database esistente.</span><span class="sxs-lookup"><span data-stu-id="9653f-137">Now it's time to create the EF model based on an existing database.</span></span>

* <span data-ttu-id="9653f-138">**Strumenti –> Gestione pacchetti NuGet –> Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="9653f-138">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>

* <span data-ttu-id="9653f-139">Eseguire il comando seguente per creare un modello dal database esistente</span><span class="sxs-lookup"><span data-stu-id="9653f-139">Run the following command to create a model from the existing database</span></span>

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> <span data-ttu-id="9653f-140">È possibile specificare le tabelle per cui generare le entità aggiungendo l'argomento `-Tables` al comando precedente.</span><span class="sxs-lookup"><span data-stu-id="9653f-140">You can specify the tables to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="9653f-141">Ad esempio `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="9653f-141">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="9653f-142">Il processo di reverse engineering ha creato le classi di entità (`Blog` e `Post`) e un contesto derivato (`BloggingContext`) in base allo schema del database esistente.</span><span class="sxs-lookup"><span data-stu-id="9653f-142">The reverse engineer process created entity classes (`Blog` and `Post`) and a derived context (`BloggingContext`) based on the schema of the existing database.</span></span>

<span data-ttu-id="9653f-143">Le classi di entità sono semplici oggetti C# che rappresentano i dati di cui verranno eseguite query e che verranno salvati.</span><span class="sxs-lookup"><span data-stu-id="9653f-143">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="9653f-144">Di seguito sono riportate le classi di entità `Blog` e `Post`:</span><span class="sxs-lookup"><span data-stu-id="9653f-144">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> <span data-ttu-id="9653f-145">Per abilitare il caricamento lazy, è possibile impostare proprietà di navigazione `virtual` (Blog.Post e Post.Blog).</span><span class="sxs-lookup"><span data-stu-id="9653f-145">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

<span data-ttu-id="9653f-146">Il contesto rappresenta una sessione con il database.</span><span class="sxs-lookup"><span data-stu-id="9653f-146">The context represents a session with the database.</span></span> <span data-ttu-id="9653f-147">Contiene i metodi che è possibile usare per eseguire query e salvare istanze delle classi di entità.</span><span class="sxs-lookup"><span data-stu-id="9653f-147">It has methods that you can use to query and save instances of the entity classes.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a><span data-ttu-id="9653f-148">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="9653f-148">Use the model</span></span>

<span data-ttu-id="9653f-149">È ora possibile usare il modello per eseguire l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="9653f-149">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="9653f-150">Aprire *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="9653f-150">Open *Program.cs*</span></span>

* <span data-ttu-id="9653f-151">Sostituire tutti i contenuti del file con il codice seguente</span><span class="sxs-lookup"><span data-stu-id="9653f-151">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* <span data-ttu-id="9653f-152">Debug > Avvia senza eseguire debug</span><span class="sxs-lookup"><span data-stu-id="9653f-152">Debug > Start Without Debugging</span></span>

  <span data-ttu-id="9653f-153">Si può notare che un blog viene salvato nel database e quindi che i dettagli di tutti i blog vengono visualizzati nella console.</span><span class="sxs-lookup"><span data-stu-id="9653f-153">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![immagine](_static/output-existing-db.png)

## <a name="additional-resources"></a><span data-ttu-id="9653f-155">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="9653f-155">Additional Resources</span></span>

* [<span data-ttu-id="9653f-156">EF Core in .NET Framework con un nuovo database</span><span class="sxs-lookup"><span data-stu-id="9653f-156">EF Core on .NET Framework with a new database</span></span>](xref:core/get-started/full-dotnet/new-db)
* <span data-ttu-id="9653f-157">[EF Core in .NET Core con un nuovo database - SQLite](xref:core/get-started/netcore/new-db-sqlite): esercitazione su EF per la console multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="9653f-157">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
