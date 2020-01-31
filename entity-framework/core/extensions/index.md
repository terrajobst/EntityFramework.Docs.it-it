---
title: Strumenti ed estensioni - EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 99f59153a452a2f4aad5811110ebc5b5da7717ef
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888046"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="b3409-102">Strumenti ed estensioni di EF Core</span><span class="sxs-lookup"><span data-stu-id="b3409-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="b3409-103">Questi strumenti ed estensioni offrono funzionalità aggiuntive per Entity Framework Core 2.1 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="b3409-103">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="b3409-104">Le estensioni sono composte da diversi tipi di origine e non vengono mantenute nell'ambito del progetto di Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b3409-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="b3409-105">Quando si prende in considerazione un'estensione di terze parti, valutarne con cura gli aspetti relativi a qualità, licenze, compatibilità, supporto e così via, per essere certi che soddisfi i propri requisiti.</span><span class="sxs-lookup"><span data-stu-id="b3409-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="b3409-106">In particolare, un'estensione compilata per una versione precedente di EF Core potrebbe necessitare di un aggiornamento prima che funzioni con le versioni più recenti.</span><span class="sxs-lookup"><span data-stu-id="b3409-106">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="b3409-107">Strumenti</span><span class="sxs-lookup"><span data-stu-id="b3409-107">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="b3409-108">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="b3409-108">LLBLGen Pro</span></span>

<span data-ttu-id="b3409-109">LLBLGen Pro è una soluzione per la modellazione delle entità con supporto per Entity Framework ed Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b3409-109">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="b3409-110">Consente di definire facilmente il modello di entità e di eseguirne il mapping al database, usando l'approccio Database-First o Model-First, in modo da iniziare subito a scrivere le query.</span><span class="sxs-lookup"><span data-stu-id="b3409-110">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="b3409-111">Per EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b3409-111">For EF Core: 2.</span></span>

[<span data-ttu-id="b3409-112">Sito Web</span><span class="sxs-lookup"><span data-stu-id="b3409-112">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="b3409-113">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="b3409-113">Devart Entity Developer</span></span>

<span data-ttu-id="b3409-114">Entity Developer è una potente finestra di progettazione ORM per ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access e LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="b3409-114">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="b3409-115">Supporta la progettazione visiva di modelli EF Core, usando l'approccio con precedenza del modello o precedenza del database e la generazione di codice C# o Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="b3409-115">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="b3409-116">Per EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b3409-116">For EF Core: 2.</span></span>

[<span data-ttu-id="b3409-117">Sito Web</span><span class="sxs-lookup"><span data-stu-id="b3409-117">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a><span data-ttu-id="b3409-118">ORM nHydrate per Entity Framework</span><span class="sxs-lookup"><span data-stu-id="b3409-118">nHydrate ORM for Entity Framework</span></span>

<span data-ttu-id="b3409-119">ORM che consente di creare classi estendibili e fortemente tipizzate per Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b3409-119">An ORM that creates strongly-typed, extendable classes for Entity Framework.</span></span> <span data-ttu-id="b3409-120">Il codice generato è Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b3409-120">The generated code is Entity Framework Core.</span></span> <span data-ttu-id="b3409-121">Non c'è alcuna differenza.</span><span class="sxs-lookup"><span data-stu-id="b3409-121">There is no difference.</span></span> <span data-ttu-id="b3409-122">Non si tratta di una sostituzione per EF o di un ORM personalizzato.</span><span class="sxs-lookup"><span data-stu-id="b3409-122">This is not a replacement for EF or a custom ORM.</span></span> <span data-ttu-id="b3409-123">Si tratta di un livello di modellazione visivo che consente a un team di gestire schemi di database complessi.</span><span class="sxs-lookup"><span data-stu-id="b3409-123">It is a visual, modeling layer that allows a team to manage complex database schemas.</span></span> <span data-ttu-id="b3409-124">Funziona bene con software SCM come Git, consentendo l'accesso multiutente al modello con conflitti minimi.</span><span class="sxs-lookup"><span data-stu-id="b3409-124">It works well with SCM software like Git, allowing multi-user access to your model with minimal conflicts.</span></span> <span data-ttu-id="b3409-125">Il programma di installazione tiene traccia delle modifiche del modello e crea gli script di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="b3409-125">The installer tracks model changes and creates upgrade scripts.</span></span> <span data-ttu-id="b3409-126">Per EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="b3409-126">For EF Core: 3.</span></span>

[<span data-ttu-id="b3409-127">Sito GitHub</span><span class="sxs-lookup"><span data-stu-id="b3409-127">Github site</span></span>](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a><span data-ttu-id="b3409-128">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="b3409-128">EF Core Power Tools</span></span>

<span data-ttu-id="b3409-129">EF Core Power Tools è un'estensione di Visual Studio che espone varie attività di progettazione di EF Core in un'interfaccia utente intuitiva.</span><span class="sxs-lookup"><span data-stu-id="b3409-129">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="b3409-130">Include la decompilazione di classi DbContext e classi di entità da database esistenti e [pacchetti di applicazione livello dati SQL Server](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), la gestione delle migrazioni del database e le visualizzazioni del modello.</span><span class="sxs-lookup"><span data-stu-id="b3409-130">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="b3409-131">Per EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="b3409-131">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="b3409-132">Wiki di GitHub</span><span class="sxs-lookup"><span data-stu-id="b3409-132">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="b3409-133">Entity Framework Visual Editor</span><span class="sxs-lookup"><span data-stu-id="b3409-133">Entity Framework Visual Editor</span></span>

<span data-ttu-id="b3409-134">Entity Framework Visual Editor è un'estensione di Visual Studio che aggiunge una finestra di progettazione ORM per la progettazione visiva di classi di Entity Framework 6 ed EF Core.</span><span class="sxs-lookup"><span data-stu-id="b3409-134">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="b3409-135">Il codice viene generato usando i modelli T4, pertanto può essere personalizzato per soddisfare qualsiasi esigenza.</span><span class="sxs-lookup"><span data-stu-id="b3409-135">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="b3409-136">Supporta l'ereditarietà, le associazioni unidirezionali e bidirezionali, le enumerazioni e la possibilità di usare una codifica a colori per le classi e di aggiungere blocchi di testo, per spiegare parti potenzialmente molto complesse del progetto.</span><span class="sxs-lookup"><span data-stu-id="b3409-136">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="b3409-137">Per EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b3409-137">For EF Core: 2.</span></span>

[<span data-ttu-id="b3409-138">Marketplace</span><span class="sxs-lookup"><span data-stu-id="b3409-138">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="b3409-139">CatFactory</span><span class="sxs-lookup"><span data-stu-id="b3409-139">CatFactory</span></span>

<span data-ttu-id="b3409-140">CatFactory è un motore di scaffolding per .NET Core in grado di automatizzare la generazione di classi, entità, configurazioni di mapping e classi repository DbContext da un database SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b3409-140">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="b3409-141">Per EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b3409-141">For EF Core: 2.</span></span>

[<span data-ttu-id="b3409-142">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="b3409-142">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="b3409-143">Generatore Entity Framework Core di LoreSoft</span><span class="sxs-lookup"><span data-stu-id="b3409-143">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="b3409-144">Il Generatore Entity Framework Core (efg) è uno strumento della riga di comando di .NET Core che genera modelli EF Core da un database esistente con modalità simili a `dotnet ef dbcontext scaffold`, ma supporta anche la [rigenerazione](https://efg.loresoft.com/en/latest/regeneration/) del codice sicuro tramite la sostituzione dell'area o tramite l'analisi dei file di mapping.</span><span class="sxs-lookup"><span data-stu-id="b3409-144">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="b3409-145">Lo strumento supporta la generazione di modelli di visualizzazione, la convalida e il codice mapper degli oggetti.</span><span class="sxs-lookup"><span data-stu-id="b3409-145">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="b3409-146">Per EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b3409-146">For EF Core: 2.</span></span>

<span data-ttu-id="b3409-147">[Esercitazione](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentazione](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="b3409-147">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="b3409-148">Estensioni</span><span class="sxs-lookup"><span data-stu-id="b3409-148">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="b3409-149">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="b3409-149">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="b3409-150">Libreria di plug-in che consente di registrare automaticamente le modifiche ai dati eseguite da EF Core in una tabella di cronologia.</span><span class="sxs-lookup"><span data-stu-id="b3409-150">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="b3409-151">Per EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b3409-151">For EF Core: 2.</span></span>

[<span data-ttu-id="b3409-152">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="b3409-152">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="b3409-153">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="b3409-153">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="b3409-154">Estensione che consente di archiviare i risultati delle query di EF Core in una cache di secondo livello, in modo che le esecuzioni successive delle stesse query evitino l'accesso al database e recuperino i dati direttamente dalla cache.</span><span class="sxs-lookup"><span data-stu-id="b3409-154">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span> <span data-ttu-id="b3409-155">Per EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b3409-155">For EF Core: 2.</span></span>

[<span data-ttu-id="b3409-156">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="b3409-156">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a><span data-ttu-id="b3409-157">Geco</span><span class="sxs-lookup"><span data-stu-id="b3409-157">Geco</span></span>

<span data-ttu-id="b3409-158">Geco (Generator Console) è un generatore di codice semplice basato su un progetto di console, che viene eseguito in .NET Core e usa stringhe interpolate C# per la generazione di codice.</span><span class="sxs-lookup"><span data-stu-id="b3409-158">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="b3409-159">Geco include un generatore di modelli inversi per EF Core con supporto di pluralizzazione, singolarizzazione e modelli modificabili.</span><span class="sxs-lookup"><span data-stu-id="b3409-159">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="b3409-160">Include anche un generatore di script per dati di inizializzazione e uno strumento di pulizia database.</span><span class="sxs-lookup"><span data-stu-id="b3409-160">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="b3409-161">Per EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b3409-161">For EF Core: 2.</span></span>

[<span data-ttu-id="b3409-162">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="b3409-162">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="b3409-163">EntityFrameworkCore.Scaffolding.Handlebars</span><span class="sxs-lookup"><span data-stu-id="b3409-163">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="b3409-164">Consente la personalizzazione di classi decompilate da un database esistente usando la toolchain di Entity Framework Core con modelli Handlebars.</span><span class="sxs-lookup"><span data-stu-id="b3409-164">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="b3409-165">Per EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="b3409-165">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="b3409-166">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="b3409-166">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="b3409-167">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="b3409-167">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="b3409-168">NeinLinq estende i provider LINQ, ad esempio Entity Framework, e consente il riuso delle funzioni, la riscrittura di query e la creazione di query dinamiche usando predicati e selettori traducibili.</span><span class="sxs-lookup"><span data-stu-id="b3409-168">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="b3409-169">Per EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="b3409-169">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="b3409-170">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="b3409-170">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="b3409-171">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="b3409-171">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="b3409-172">Plug-in per Microsoft.EntityFrameworkCore che supporta repository, modelli di unità di lavoro e più database con la transazione distribuita supportata.</span><span class="sxs-lookup"><span data-stu-id="b3409-172">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="b3409-173">Per EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b3409-173">For EF Core: 2.</span></span>

[<span data-ttu-id="b3409-174">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="b3409-174">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="b3409-175">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="b3409-175">EFCore.BulkExtensions</span></span>

<span data-ttu-id="b3409-176">Estensioni EF Core per operazioni in blocco (inserimento, aggiornamento, eliminazione).</span><span class="sxs-lookup"><span data-stu-id="b3409-176">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="b3409-177">Per EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="b3409-177">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="b3409-178">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="b3409-178">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="b3409-179">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="b3409-179">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="b3409-180">Aggiunge la pluralizzazione in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="b3409-180">Adds design-time pluralization.</span></span> <span data-ttu-id="b3409-181">Per EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b3409-181">For EF Core: 2.</span></span>

[<span data-ttu-id="b3409-182">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="b3409-182">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="b3409-183">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="b3409-183">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="b3409-184">Ripresa dell'attributo [Index] (con estensione per la compilazione del modello).</span><span class="sxs-lookup"><span data-stu-id="b3409-184">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="b3409-185">Per EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="b3409-185">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="b3409-186">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="b3409-186">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="b3409-187">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="b3409-187">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="b3409-188">Definisce un wrapper per il Provider di database in memoria EF Core.</span><span class="sxs-lookup"><span data-stu-id="b3409-188">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="b3409-189">Ne rende il funzionamento più simile a quello di un provider relazionale.</span><span class="sxs-lookup"><span data-stu-id="b3409-189">Makes it act more like a relational provider.</span></span> <span data-ttu-id="b3409-190">Per EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b3409-190">For EF Core: 2.</span></span>

[<span data-ttu-id="b3409-191">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="b3409-191">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="b3409-192">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="b3409-192">EFCore.TemporalSupport</span></span>

<span data-ttu-id="b3409-193">Implementazione di supporto temporale.</span><span class="sxs-lookup"><span data-stu-id="b3409-193">An implementation of temporal support.</span></span> <span data-ttu-id="b3409-194">Per EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b3409-194">For EF Core: 2.</span></span>

[<span data-ttu-id="b3409-195">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="b3409-195">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="b3409-196">EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="b3409-196">EfCoreTemporalTable</span></span>

<span data-ttu-id="b3409-197">Consente di eseguire facilmente query temporali nel database preferito usando metodi di estensione introdotti: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span><span class="sxs-lookup"><span data-stu-id="b3409-197">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="b3409-198">Per EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="b3409-198">For EF Core: 3.</span></span>

[<span data-ttu-id="b3409-199">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="b3409-199">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="b3409-200">EFCore.TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="b3409-200">EFCore.TimeTraveler</span></span>

<span data-ttu-id="b3409-201">Consente query Entity Framework Core complete sulla [cronologia temporale di SQL Server](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) usando il codice, le entità e i mapping di EF Core già definiti.</span><span class="sxs-lookup"><span data-stu-id="b3409-201">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="b3409-202">Consente lo spostamento cronologico tramite il wrapping del codice in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span><span class="sxs-lookup"><span data-stu-id="b3409-202">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="b3409-203">Per EF Core: 3.</span><span class="sxs-lookup"><span data-stu-id="b3409-203">For EF Core: 3.</span></span>

[<span data-ttu-id="b3409-204">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="b3409-204">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="b3409-205">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="b3409-205">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="b3409-206">Libreria di estensioni per Entity Framework Core che consente agli sviluppatori che usano SQL Server di usare facilmente le tabelle temporali.</span><span class="sxs-lookup"><span data-stu-id="b3409-206">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="b3409-207">Per EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b3409-207">For EF Core: 2.</span></span>

[<span data-ttu-id="b3409-208">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="b3409-208">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="b3409-209">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="b3409-209">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="b3409-210">Cache della query di secondo livello ad alte prestazioni.</span><span class="sxs-lookup"><span data-stu-id="b3409-210">A high-performance second-level query cache.</span></span> <span data-ttu-id="b3409-211">Per EF Core: 2.</span><span class="sxs-lookup"><span data-stu-id="b3409-211">For EF Core: 2.</span></span>

[<span data-ttu-id="b3409-212">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="b3409-212">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="b3409-213">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="b3409-213">Entity Framework Plus</span></span>

<span data-ttu-id="b3409-214">Estende DbContext con funzionalità quali: filtro di inclusione, controllo, memorizzazione nella cache, query Future, eliminazione in batch, aggiornamento in batch e molte altre.</span><span class="sxs-lookup"><span data-stu-id="b3409-214">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="b3409-215">Per EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="b3409-215">For EF Core: 2, 3.</span></span>

<span data-ttu-id="b3409-216">[Sito Web](https://entityframework-plus.net/)
[Repository GitHub](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="b3409-216">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="b3409-217">Entity Framework Extensions</span><span class="sxs-lookup"><span data-stu-id="b3409-217">Entity Framework Extensions</span></span>

<span data-ttu-id="b3409-218">Estende DbContext con operazioni in blocco ad alte prestazioni: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge e altre.</span><span class="sxs-lookup"><span data-stu-id="b3409-218">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="b3409-219">Per EF Core: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="b3409-219">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="b3409-220">Sito Web</span><span class="sxs-lookup"><span data-stu-id="b3409-220">Website</span></span>](https://entityframework-extensions.net/)

### <a name="expressionify"></a><span data-ttu-id="b3409-221">Expressionify</span><span class="sxs-lookup"><span data-stu-id="b3409-221">Expressionify</span></span>

<span data-ttu-id="b3409-222">Aggiunta del supporto per la chiamata di metodi di estensione nelle espressioni lambda LINQ.</span><span class="sxs-lookup"><span data-stu-id="b3409-222">Add support for calling extension methods in linq lambdas.</span></span> <span data-ttu-id="b3409-223">Per EF Core: 3.1</span><span class="sxs-lookup"><span data-stu-id="b3409-223">For EF Core: 3.1</span></span>

[<span data-ttu-id="b3409-224">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="b3409-224">GitHub repository</span></span>](https://github.com/ClaveConsulting/Expressionify)
