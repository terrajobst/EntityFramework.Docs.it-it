---
title: Strumenti ed estensioni - EF Core
author: ErikEJ
ms.date: 07/03/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 1edc7a20f54b2d26f899c93e98dfaf6d62c29f86
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490727"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="c7bf2-102">Strumenti ed estensioni di EF Core</span><span class="sxs-lookup"><span data-stu-id="c7bf2-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="c7bf2-103">Gli strumenti e le estensioni offrono funzionalità aggiuntive per Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-103">Tools and extensions provide additional functionality for Entity Framework Core.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="c7bf2-104">Le estensioni sono composte da diversi tipi di origine e non vengono mantenute nell'ambito del progetto di Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-104">Extensions are built by a variety of sources and not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="c7bf2-105">Quando si prende in considerazione un'estensione di terze parti, valutarne con cura gli aspetti relativi a qualità, licenze, compatibilità, supporto e così via, per essere certi che soddisfi i propri requisiti.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-105">When considering a third party extension, be sure to evaluate quality, licensing, compatibility, support, etc. to ensure they meet your requirements.</span></span>

## <a name="tools"></a><span data-ttu-id="c7bf2-106">Strumenti</span><span class="sxs-lookup"><span data-stu-id="c7bf2-106">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="c7bf2-107">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="c7bf2-107">LLBLGen Pro</span></span>

<span data-ttu-id="c7bf2-108">LLBLGen Pro è una soluzione per la modellazione delle entità con supporto per Entity Framework ed Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-108">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="c7bf2-109">Consente di definire facilmente il modello di entità e di eseguirne il mapping al database, usando l'approccio Database-First o Model-First, in modo da iniziare subito a scrivere le query.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-109">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span>

[<span data-ttu-id="c7bf2-110">Sito Web</span><span class="sxs-lookup"><span data-stu-id="c7bf2-110">website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="c7bf2-111">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="c7bf2-111">Devart Entity Developer</span></span>

<span data-ttu-id="c7bf2-112">Entity Developer è una potente finestra di progettazione ORM per ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access e LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-112">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="c7bf2-113">È possibile usare approcci di tipo Model-First e Database-First per progettare il modello ORM e generare per quest'ultimo codice C# o Visual Basic .NET.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-113">You can use  Model-First and Database-First approaches to design your ORM model and generate C# or Visual Basic .NET code for it.</span></span> <span data-ttu-id="c7bf2-114">Introduce nuove modalità di progettazione dei modelli ORM, ottimizza la produttività e semplifica lo sviluppo delle applicazioni di database.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-114">It introduces new approaches for designing ORM models, boosts productivity, and facilitates the development of database applications.</span></span>

[<span data-ttu-id="c7bf2-115">Sito Web</span><span class="sxs-lookup"><span data-stu-id="c7bf2-115">website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a><span data-ttu-id="c7bf2-116">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="c7bf2-116">EF Core Power Tools</span></span>

<span data-ttu-id="c7bf2-117">Estensione di Visual Studio 2017+.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-117">Visual Studio 2017+ extension.</span></span> <span data-ttu-id="c7bf2-118">È possibile decompilare DbContext e le classi POCO da un database esistente o un progetto di database di SQL Server e visualizzare e controllare DbContext in vari modi.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-118">You can reverse engineer of DbContext and POCO classes from an existing database or SQL Server Database project, and visualize and inspect your DbContext in various ways.</span></span>

[<span data-ttu-id="c7bf2-119">Wiki di GitHub</span><span class="sxs-lookup"><span data-stu-id="c7bf2-119">GitHub wiki</span></span>](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="c7bf2-120">Entity Framework Visual Editor</span><span class="sxs-lookup"><span data-stu-id="c7bf2-120">Entity Framework Visual Editor</span></span>

<span data-ttu-id="c7bf2-121">Estensione di Visual Studio 2017 che aggiunge una finestra di progettazione ORM per la progettazione visiva di classi di Entity Framework 6, Core 2.0 e Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-121">A Visual Studio 2017 extension that adds an ORM designer for visual design of Entity Framework 6, Core 2.0 and Core 2.1 classes.</span></span> <span data-ttu-id="c7bf2-122">Il codice viene generato usando i modelli T4, quindi può essere completamente personalizzato per soddisfare qualsiasi esigenza.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-122">Code is generated using T4 templates so can be completely customized to suit any needs.</span></span> <span data-ttu-id="c7bf2-123">Sono supportate l'ereditarietà e le associazioni unidirezionali e bidirezionali, così come le enumerazioni e la possibilità di usare una codifica a colori per le classi e di aggiungere blocchi di testo per spiegare parti potenzialmente arcane del progetto.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-123">Inheritance, unidirectional and bidirectional associations are all supported, as are enumerations and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span>

[<span data-ttu-id="c7bf2-124">Marketplace</span><span class="sxs-lookup"><span data-stu-id="c7bf2-124">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

## <a name="extensions"></a><span data-ttu-id="c7bf2-125">Estensioni</span><span class="sxs-lookup"><span data-stu-id="c7bf2-125">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="c7bf2-126">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="c7bf2-126">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="c7bf2-127">Plug-in per Microsoft.EntityFrameworkCore per supportare la registrazione automatica della cronologia delle modifiche dei dati.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-127">A plugin for Microsoft.EntityFrameworkCore to support automatically recording data changes history.</span></span>

[<span data-ttu-id="c7bf2-128">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="c7bf2-128">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a><span data-ttu-id="c7bf2-129">Microsoft.EntityFrameworkCore.DynamicLinq</span><span class="sxs-lookup"><span data-stu-id="c7bf2-129">Microsoft.EntityFrameworkCore.DynamicLinq</span></span>

<span data-ttu-id="c7bf2-130">Estensioni Linq dinamiche per Microsoft.EntityFrameworkCore con aggiunta del supporto asincrono</span><span class="sxs-lookup"><span data-stu-id="c7bf2-130">Dynamic Linq extensions for Microsoft.EntityFrameworkCore which adds Async support</span></span>

 [<span data-ttu-id="c7bf2-131">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="c7bf2-131">GitHub repository</span></span>](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a><span data-ttu-id="c7bf2-132">EFCore.Practices</span><span class="sxs-lookup"><span data-stu-id="c7bf2-132">EFCore.Practices</span></span>

<span data-ttu-id="c7bf2-133">Tentativo di acquisire alcune procedure consigliate in un'API che supporta i test, tra cui un piccolo framework per l'analisi delle query N+1.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-133">Attempt to capture some good or best practices in an API that supports testing – including a small framework to scan for N+1 queries.</span></span>

[<span data-ttu-id="c7bf2-134">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="c7bf2-134">GitHub repository</span></span>](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="c7bf2-135">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="c7bf2-135">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="c7bf2-136">Libreria con memorizzazione nella cache di secondo livello.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-136">Second Level Caching Library.</span></span> <span data-ttu-id="c7bf2-137">La memorizzazione nella cache di secondo livello è una cache della query.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-137">Second level caching is a query cache.</span></span> <span data-ttu-id="c7bf2-138">I risultati dei comandi EF verranno memorizzati nella cache, in modo che gli stessi comandi EF recupereranno i dati dalla cache anziché eseguirli di nuovo sul database.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-138">The results of EF commands will be stored in the cache, so that the same EF commands will retrieve their data from the cache rather than executing them against the database again.</span></span>

[<span data-ttu-id="c7bf2-139">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="c7bf2-139">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a><span data-ttu-id="c7bf2-140">Detached.EntityFramework</span><span class="sxs-lookup"><span data-stu-id="c7bf2-140">Detached.EntityFramework</span></span>

<span data-ttu-id="c7bf2-141">Carica e salva i grafici di un'intera entità scollegata, ovvero l'entità con i relativi elenchi ed entità figlio.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-141">Loads and saves entire detached entity graphs (the entity with their child entities and lists).</span></span> <span data-ttu-id="c7bf2-142">Ispirato da [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span><span class="sxs-lookup"><span data-stu-id="c7bf2-142">Inspired by [GraphDiff](https://github.com/refactorthis/GraphDiff/).</span></span> <span data-ttu-id="c7bf2-143">L'idea prevede anche l'aggiunta di alcuni plug-in per semplificare alcune attività ripetitive, ad esempio il controllo e l'impaginazione.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-143">The idea is also add some plugins to simplificate some repetitive tasks, like auditing and pagination.</span></span>

[<span data-ttu-id="c7bf2-144">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="c7bf2-144">GitHub repository</span></span>](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a><span data-ttu-id="c7bf2-145">EntityFrameworkCore.PrimaryKey</span><span class="sxs-lookup"><span data-stu-id="c7bf2-145">EntityFrameworkCore.PrimaryKey</span></span>

<span data-ttu-id="c7bf2-146">Recupero della chiave primaria (incluse le chiavi composte) da qualsiasi entità come dizionario.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-146">Retrieve the primary key (including composite keys) from any entity as a dictionary.</span></span>

[<span data-ttu-id="c7bf2-147">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="c7bf2-147">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a><span data-ttu-id="c7bf2-148">EntityFrameworkCore.Rx</span><span class="sxs-lookup"><span data-stu-id="c7bf2-148">EntityFrameworkCore.Rx</span></span>

<span data-ttu-id="c7bf2-149">Wrapper di estensione reattivi per gli oggetti osservabili delle entità di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-149">Reactive extension wrappers for hot observables of Entity Framework entities.</span></span>

[<span data-ttu-id="c7bf2-150">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="c7bf2-150">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a><span data-ttu-id="c7bf2-151">EntityFrameworkCore.Triggers</span><span class="sxs-lookup"><span data-stu-id="c7bf2-151">EntityFrameworkCore.Triggers</span></span>

<span data-ttu-id="c7bf2-152">Aggiungere trigger alle entità con eventi di inserimento, aggiornamento ed eliminazione.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-152">Add triggers to your entities with insert, update, and delete events.</span></span> <span data-ttu-id="c7bf2-153">Esistono tre eventi per ogni oggetto: prima, dopo e in caso di errore.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-153">There are three events for each: before, after, and upon failure.</span></span>

[<span data-ttu-id="c7bf2-154">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="c7bf2-154">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a><span data-ttu-id="c7bf2-155">EntityFrameworkCore.TypedOriginalValues</span><span class="sxs-lookup"><span data-stu-id="c7bf2-155">EntityFrameworkCore.TypedOriginalValues</span></span>

<span data-ttu-id="c7bf2-156">Ottenere l'accesso tipizzato all'elemento OriginalValue delle proprietà dell'entità.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-156">Get typed access to the OriginalValue of your entity properties.</span></span> <span data-ttu-id="c7bf2-157">Le proprietà semplici e complesse sono supportate, la navigazione e le raccolte non lo sono.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-157">Simple and complex properties are supported, navigation/collections are not.</span></span>

[<span data-ttu-id="c7bf2-158">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="c7bf2-158">GitHub repository</span></span>](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a><span data-ttu-id="c7bf2-159">Geco</span><span class="sxs-lookup"><span data-stu-id="c7bf2-159">Geco</span></span>

<span data-ttu-id="c7bf2-160">Geco offre un generatore di modelli di reverse engineering con supporto per pluralizzazione e singolarizzazione e modelli modificabili basati su stringhe C# 6.0 interpolate e in esecuzione su .Net Core.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-160">Geco provides a Reverse Model generator with support for Pluralization/Singularization and editable templates based on C# 6.0 interpolated strings and running on .Net Core.</span></span> <span data-ttu-id="c7bf2-161">Offre anche un generatore di script di inizializzazione con script merge SQL e uno strumento di esecuzione script.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-161">It also provides an Seed script generator with SQL Merge scripts and an script runner.</span></span>

[<span data-ttu-id="c7bf2-162">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="c7bf2-162">Github repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a><span data-ttu-id="c7bf2-163">LinqKit.Microsoft.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="c7bf2-163">LinqKit.Microsoft.EntityFrameworkCore</span></span>

<span data-ttu-id="c7bf2-164">LinqKit.Microsoft.EntityFrameworkCore è un set gratuito di estensioni per utenti esperti di LINQ to SQL ed EntityFrameworkCore.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-164">LinqKit.Microsoft.EntityFrameworkCore is a free set of extensions for LINQ to SQL and EntityFrameworkCore power users.</span></span> <span data-ttu-id="c7bf2-165">Con supporto Include(...) e IDbAsync.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-165">With Include(...) and IDbAsync support.</span></span>

[<span data-ttu-id="c7bf2-166">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="c7bf2-166">GitHub repository</span></span>](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="c7bf2-167">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="c7bf2-167">NeinLinq.EntityFrameworkCore</span></span>

<span data-ttu-id="c7bf2-168">NeinLinq.EntityFrameworkCore offre estensioni utili per l'uso dei provider LINQ, ad esempio Entity Framework, che supportano solo un subset limitato di funzioni .NET, riusando le funzioni, riscrivendo le query, rendendole anche null-safe, e compilando le query dinamiche usando predicati e selettori traducibili.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-168">NeinLinq.EntityFrameworkCore provides helpful extensions for using LINQ providers such as Entity Framework that support only a minor subset of .NET functions, reusing functions, rewriting queries, even making them null-safe, and building dynamic queries using translatable predicates and selectors.</span></span>

[<span data-ttu-id="c7bf2-169">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="c7bf2-169">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="c7bf2-170">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="c7bf2-170">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="c7bf2-171">Plug-in per Microsoft.EntityFrameworkCore supportare il repository, i modelli di unità di lavoro e più database con transazione distribuita supportata.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-171">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple database with distributed transaction supported.</span></span>

[<span data-ttu-id="c7bf2-172">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="c7bf2-172">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a><span data-ttu-id="c7bf2-173">EntityFramework.LazyLoading</span><span class="sxs-lookup"><span data-stu-id="c7bf2-173">EntityFramework.LazyLoading</span></span>

<span data-ttu-id="c7bf2-174">Caricamento lazy per EF Core 1.1</span><span class="sxs-lookup"><span data-stu-id="c7bf2-174">Lazy Loading for EF Core 1.1</span></span>

[<span data-ttu-id="c7bf2-175">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="c7bf2-175">GitHub repository</span></span>](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a><span data-ttu-id="c7bf2-176">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="c7bf2-176">EFCore.BulkExtensions</span></span>

<span data-ttu-id="c7bf2-177">Estensioni EntityFrameworkCore per operazioni in blocco (inserimento, aggiornamento, eliminazione).</span><span class="sxs-lookup"><span data-stu-id="c7bf2-177">EntityFrameworkCore extensions for Bulk operations (Insert, Update, Delete).</span></span>

[<span data-ttu-id="c7bf2-178">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="c7bf2-178">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="c7bf2-179">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="c7bf2-179">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="c7bf2-180">Aggiunge la pluralizzazione in fase di progettazione a EF Core.</span><span class="sxs-lookup"><span data-stu-id="c7bf2-180">Adds design-time pluralization to EF Core.</span></span>

[<span data-ttu-id="c7bf2-181">Repository GitHub</span><span class="sxs-lookup"><span data-stu-id="c7bf2-181">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)
