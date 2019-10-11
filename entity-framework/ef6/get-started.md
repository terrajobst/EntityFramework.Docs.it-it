---
title: Introduzione a Entity Framework 6 - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 66ce9113-81d2-480f-8c16-d00ec405b2f7
ms.openlocfilehash: bf54879ea94e597dfeac3e4bd70571dad290dd9e
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181403"
---
# <a name="get-started-with-entity-framework-6"></a><span data-ttu-id="15673-102">Introduzione a Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="15673-102">Get started with Entity Framework 6</span></span>

<span data-ttu-id="15673-103">Questa guida contiene una raccolta di collegamenti ad articoli della documentazione, procedure dettagliate e video selezionati per iniziare a usare rapidamente il prodotto.</span><span class="sxs-lookup"><span data-stu-id="15673-103">This guide contains a collection of links to selected documentation articles, walkthroughs and videos that can help you get started quickly.</span></span>

## <a name="fundamentals"></a><span data-ttu-id="15673-104">Concetti fondamentali</span><span class="sxs-lookup"><span data-stu-id="15673-104">Fundamentals</span></span>

* [<span data-ttu-id="15673-105">Ottenere Entity Framework</span><span class="sxs-lookup"><span data-stu-id="15673-105">Get Entity Framework</span></span>](~/ef6/fundamentals/install.md)

  <span data-ttu-id="15673-106">Viene descritto come aggiungere Entity Framework alle applicazioni e, se si vuole usare EF Designer, assicurarsi che venga installato in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="15673-106">Here you will learn how to add Entity Framework to your applications and, if you want to use the EF Designer, make sure you get it installed in Visual Studio.</span></span>

* <span data-ttu-id="15673-107">@no__t 0Creating un modello: Code First, progettazione EF e i flussi di lavoro EF @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="15673-107">[Creating a Model: Code First, the EF Designer, and the EF Workflows](~/ef6/modeling/index.md)</span></span>

  <span data-ttu-id="15673-108">Si preferisce specificare il modello EF scrivendo codice o tracciando caselle e linee?</span><span class="sxs-lookup"><span data-stu-id="15673-108">Do you prefer to specify your EF model writing code or drawing boxes and lines?</span></span>
<span data-ttu-id="15673-109">Si userà EF per mappare gli oggetti in un database esistente o si vuole che EF crei un database specifico per gli oggetti?</span><span class="sxs-lookup"><span data-stu-id="15673-109">Are you going to use EF to map your objects to an existing database or would you like EF to create a database tailored for your objects?</span></span>
<span data-ttu-id="15673-110">Ecco le informazioni su due approcci diversi per l'uso di EF6: EF designer e Code First.</span><span class="sxs-lookup"><span data-stu-id="15673-110">Here your learn about two different approaches to use EF6: EF Designer and Code First.</span></span>
<span data-ttu-id="15673-111">Seguire la discussione e guardare il video che illustra le differenze.</span><span class="sxs-lookup"><span data-stu-id="15673-111">Make sure you follow the discussion and watch the video about the difference.</span></span>

* [<span data-ttu-id="15673-112">Utilizzo di DbContext</span><span class="sxs-lookup"><span data-stu-id="15673-112">Working with DbContext</span></span>](~/ef6/fundamentals/working-with-dbcontext.md)

  <span data-ttu-id="15673-113">DbContext è il principale tipo di Entity Framework che è necessario imparare a usare.</span><span class="sxs-lookup"><span data-stu-id="15673-113">DbContext is the first and most important EF type that you need to learn how to use.</span></span> <span data-ttu-id="15673-114">Svolge la funzione di launchpad delle query del database e tiene traccia delle modifiche apportate agli oggetti in modo che possano essere salvate in modo permanente nel database.</span><span class="sxs-lookup"><span data-stu-id="15673-114">It serves as the launchpad for database queries and keeps track of changes you make to objects so that they can be persisted back to the database.</span></span>

* [<span data-ttu-id="15673-115">Poni una domanda</span><span class="sxs-lookup"><span data-stu-id="15673-115">Ask a Question</span></span>](~/ef6/resources/get-help.md)

  <span data-ttu-id="15673-116">È possibile ricevere assistenza dagli esperti e contribuire con le proprie risposte alla community.</span><span class="sxs-lookup"><span data-stu-id="15673-116">Find out how to get help from the experts and contribute your own answers to the community.</span></span>

* [<span data-ttu-id="15673-117">Collaborare</span><span class="sxs-lookup"><span data-stu-id="15673-117">Contribute</span></span>](https://github.com/aspnet/EntityFramework6/)

  <span data-ttu-id="15673-118">Entity Framework 6 usa un modello di sviluppo aperto.</span><span class="sxs-lookup"><span data-stu-id="15673-118">Entity Framework 6 uses an open development model.</span></span> <span data-ttu-id="15673-119">Nel repository GitHub sono disponibili informazioni su come migliorare ulteriormente Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="15673-119">Find out how you can help make EF even better by visiting our GitHub repository.</span></span>

## <a name="code-first-resources"></a><span data-ttu-id="15673-120">Risorse per Code First</span><span class="sxs-lookup"><span data-stu-id="15673-120">Code First resources</span></span>

  - [<span data-ttu-id="15673-121">Code First nel flusso di lavoro di un database esistente</span><span class="sxs-lookup"><span data-stu-id="15673-121">Code First to an Existing Database Workflow</span></span>](~/ef6/modeling/code-first/workflows/existing-database.md)
  - [<span data-ttu-id="15673-122">Code First nel flusso di lavoro di un nuovo database</span><span class="sxs-lookup"><span data-stu-id="15673-122">Code First to a New Database Workflow</span></span>](~/ef6/modeling/code-first/workflows/new-database.md)
  - [<span data-ttu-id="15673-123">Mapping delle enumerazioni usando Code First</span><span class="sxs-lookup"><span data-stu-id="15673-123">Mapping Enums Using Code First</span></span>](~/ef6/modeling/code-first/data-types/enums.md)
  - [<span data-ttu-id="15673-124">Mapping dei tipi spaziali usando Code First</span><span class="sxs-lookup"><span data-stu-id="15673-124">Mapping Spatial Types Using Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)
  - [<span data-ttu-id="15673-125">Creazione di convenzioni Code First personalizzate</span><span class="sxs-lookup"><span data-stu-id="15673-125">Writing Custom Code First Conventions</span></span>](~/ef6/modeling/code-first/conventions/custom.md)
  - [<span data-ttu-id="15673-126">Uso della configurazione Fluent di Code First con Visual Basic</span><span class="sxs-lookup"><span data-stu-id="15673-126">Using Code First Fluent Configuration with Visual Basic</span></span>](~/ef6/modeling/code-first/fluent/vb.md)
  - [<span data-ttu-id="15673-127">Migrazioni Code First</span><span class="sxs-lookup"><span data-stu-id="15673-127">Code First Migrations</span></span>](~/ef6/modeling/code-first/migrations/index.md)
  - [<span data-ttu-id="15673-128">Migrazioni Code First in ambienti team</span><span class="sxs-lookup"><span data-stu-id="15673-128">Code First Migrations in Team Environments</span></span>](~/ef6/modeling/code-first/migrations/teams.md)
  - <span data-ttu-id="15673-129">[Migrazioni Code First automatiche](~/ef6/modeling/code-first/migrations/automatic.md) (non consigliato)</span><span class="sxs-lookup"><span data-stu-id="15673-129">[Automatic Code First Migrations](~/ef6/modeling/code-first/migrations/automatic.md) (This is no longer recommended)</span></span>

## <a name="ef-designer-resources"></a><span data-ttu-id="15673-130">Risorse per EF Designer</span><span class="sxs-lookup"><span data-stu-id="15673-130">EF Designer resources</span></span>
  - [<span data-ttu-id="15673-131">Flusso di lavoro di Database First</span><span class="sxs-lookup"><span data-stu-id="15673-131">Database First Workflow</span></span>](~/ef6/modeling/designer/workflows/database-first.md)
  - [<span data-ttu-id="15673-132">Flusso di lavoro di Model First</span><span class="sxs-lookup"><span data-stu-id="15673-132">Model First Workflow</span></span>](~/ef6/modeling/designer/workflows/model-first.md)
  - [<span data-ttu-id="15673-133">Mapping delle enumerazioni</span><span class="sxs-lookup"><span data-stu-id="15673-133">Mapping Enums</span></span>](~/ef6/modeling/designer/data-types/enums.md)
  - [<span data-ttu-id="15673-134">Mapping dei tipi spaziali</span><span class="sxs-lookup"><span data-stu-id="15673-134">Mapping Spatial Types</span></span>](~/ef6/modeling/designer/data-types/spatial.md)
  - [<span data-ttu-id="15673-135">Mapping dell'ereditarietà della tabella per gerarchia</span><span class="sxs-lookup"><span data-stu-id="15673-135">Table-Per Hierarchy Inheritance Mapping</span></span>](~/ef6/modeling/designer/inheritance/tph.md)
  - [<span data-ttu-id="15673-136">Mapping dell'ereditarietà della tabella per tipo</span><span class="sxs-lookup"><span data-stu-id="15673-136">Table-Per Type Inheritance Mapping</span></span>](~/ef6/modeling/designer/inheritance/tpt.md)
  - [<span data-ttu-id="15673-137">Mapping di stored procedure per gli aggiornamenti</span><span class="sxs-lookup"><span data-stu-id="15673-137">Stored Procedure Mapping for Updates</span></span>](~/ef6/modeling/designer/stored-procedures/cud.md)
  - [<span data-ttu-id="15673-138">Mapping di stored procedure per le query</span><span class="sxs-lookup"><span data-stu-id="15673-138">Stored Procedure Mapping for Query</span></span>](~/ef6/modeling/designer/stored-procedures/query.md)
  - [<span data-ttu-id="15673-139">Suddivisione di entità</span><span class="sxs-lookup"><span data-stu-id="15673-139">Entity Splitting</span></span>](~/ef6/modeling/designer/entity-splitting.md)
  - [<span data-ttu-id="15673-140">Suddivisione di tabelle</span><span class="sxs-lookup"><span data-stu-id="15673-140">Table Splitting</span></span>](~/ef6/modeling/designer/table-splitting.md)
  - <span data-ttu-id="15673-141">[Query di definizione](~/ef6/modeling/designer/advanced/defining-query.md) (informazioni avanzate)</span><span class="sxs-lookup"><span data-stu-id="15673-141">[Defining Query](~/ef6/modeling/designer/advanced/defining-query.md) (Advanced)</span></span>
  - <span data-ttu-id="15673-142">[Funzioni con valori di tabella](~/ef6/modeling/designer/advanced/tvfs.md) (informazioni avanzate)</span><span class="sxs-lookup"><span data-stu-id="15673-142">[Table-Valued Functions](~/ef6/modeling/designer/advanced/tvfs.md) (Advanced)</span></span>

## <a name="other-resources"></a><span data-ttu-id="15673-143">Altre risorse</span><span class="sxs-lookup"><span data-stu-id="15673-143">Other resources</span></span>
  - [<span data-ttu-id="15673-144">Query e salvataggio asincroni</span><span class="sxs-lookup"><span data-stu-id="15673-144">Async Query and Save</span></span>](~/ef6/fundamentals/async.md)
  - [<span data-ttu-id="15673-145">Data binding con WinForms</span><span class="sxs-lookup"><span data-stu-id="15673-145">Databinding with WinForms</span></span>](~/ef6/fundamentals/databinding/winforms.md)
  - [<span data-ttu-id="15673-146">Data binding con WPF</span><span class="sxs-lookup"><span data-stu-id="15673-146">Databinding with WPF</span></span>](~/ef6/fundamentals/databinding/wpf.md)
  - <span data-ttu-id="15673-147">[Scenari disconnessi con entità con rilevamento automatico](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough.md) (non consigliato)</span><span class="sxs-lookup"><span data-stu-id="15673-147">[Disconnected scenarios with Self-Tracking Entities](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough.md) (This is no longer recommended)</span></span>
