---
title: Novità di EF Core 1.0 - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: 2cd2a54d75ed3f0caa8b674dfb56babcfcc13592
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417524"
---
# <a name="features-included-in-ef-core-10"></a><span data-ttu-id="2675c-102">Funzionalità incluse in EF Core 1.0</span><span class="sxs-lookup"><span data-stu-id="2675c-102">Features included in EF Core 1.0</span></span>

## <a name="platforms"></a><span data-ttu-id="2675c-103">Piattaforme</span><span class="sxs-lookup"><span data-stu-id="2675c-103">Platforms</span></span>

### <a name="net-framework-451"></a><span data-ttu-id="2675c-104">.NET Framework 4.5.1</span><span class="sxs-lookup"><span data-stu-id="2675c-104">.NET Framework 4.5.1</span></span>

<span data-ttu-id="2675c-105">Include la console, WPF, Windows Form, ASP.NET 4 e così via.</span><span class="sxs-lookup"><span data-stu-id="2675c-105">Includes Console, WPF, WinForms, ASP.NET 4, etc.</span></span>

### <a name="net-standard-13"></a><span data-ttu-id="2675c-106">.NET Standard 1.3</span><span class="sxs-lookup"><span data-stu-id="2675c-106">.NET Standard 1.3</span></span>

<span data-ttu-id="2675c-107">Include ASP.NET Core per .NET Framework e .NET Core in Windows, OSX e Linux.</span><span class="sxs-lookup"><span data-stu-id="2675c-107">Including ASP.NET Core targeting both .NET Framework and .NET Core on Windows, OSX, and Linux.</span></span>

## <a name="modelling"></a><span data-ttu-id="2675c-108">Modellazione</span><span class="sxs-lookup"><span data-stu-id="2675c-108">Modelling</span></span>

### <a name="basic-modelling"></a><span data-ttu-id="2675c-109">Modellazione di base</span><span class="sxs-lookup"><span data-stu-id="2675c-109">Basic modelling</span></span>

<span data-ttu-id="2675c-110">Basata sulle entità POCO con le proprietà get/set dei tipi scalari comuni (`int`, `string` e così via).</span><span class="sxs-lookup"><span data-stu-id="2675c-110">Based on POCO entities with get/set properties of common scalar types (`int`, `string`, etc.).</span></span>

### <a name="relationships-and-navigation-properties"></a><span data-ttu-id="2675c-111">Relazioni e proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="2675c-111">Relationships and navigation properties</span></span>

<span data-ttu-id="2675c-112">Nel modello basato su una chiave esterna si possono specificare relazioni uno-a-molti e uno-a-zero-o-a-uno.</span><span class="sxs-lookup"><span data-stu-id="2675c-112">One-to-many and One-to-zero-or-one relationships can be specified in the model based on a foreign key.</span></span> <span data-ttu-id="2675c-113">A queste relazioni possono essere associate le proprietà di navigazione di semplici tipi di raccolta o riferimento.</span><span class="sxs-lookup"><span data-stu-id="2675c-113">Navigation properties of simple collection or reference types can be associated with these relationships.</span></span>

### <a name="built-in-conventions"></a><span data-ttu-id="2675c-114">Convenzioni predefinite</span><span class="sxs-lookup"><span data-stu-id="2675c-114">Built-in conventions</span></span>

<span data-ttu-id="2675c-115">Compilano un modello iniziale basato sulla forma delle classi di entità.</span><span class="sxs-lookup"><span data-stu-id="2675c-115">These build an initial model based on the shape of the entity classes.</span></span>

### <a name="fluent-api"></a><span data-ttu-id="2675c-116">API Fluent</span><span class="sxs-lookup"><span data-stu-id="2675c-116">Fluent API</span></span>

<span data-ttu-id="2675c-117">Consente di eseguire l'override del metodo `OnModelCreating` nel contesto per ulteriori configurazioni del modello individuato dalla convenzione.</span><span class="sxs-lookup"><span data-stu-id="2675c-117">Allows you to override the `OnModelCreating` method on your context to further configure the model that was discovered by convention.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="2675c-118">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="2675c-118">Data annotations</span></span>

<span data-ttu-id="2675c-119">Sono attributi che possono essere aggiunti alle classi/proprietà dell'entità e influenzeranno il modello di EF.</span><span class="sxs-lookup"><span data-stu-id="2675c-119">Are attributes that can be added to your entity classes/properties and will influence the EF model.</span></span> <span data-ttu-id="2675c-120">Ad esempio, l'aggiunta di `[Required]` indicherà a EF che una proprietà è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="2675c-120">For example, adding `[Required]` will let EF know that a property is required.</span></span>

### <a name="relational-table-mapping"></a><span data-ttu-id="2675c-121">Mapping tabella relazionale</span><span class="sxs-lookup"><span data-stu-id="2675c-121">Relational Table mapping</span></span>

<span data-ttu-id="2675c-122">Consente di eseguire il mapping delle entità a tabelle/colonne.</span><span class="sxs-lookup"><span data-stu-id="2675c-122">Allows entities to be mapped to tables/columns.</span></span>

### <a name="key-value-generation"></a><span data-ttu-id="2675c-123">Generazione di valori di chiave</span><span class="sxs-lookup"><span data-stu-id="2675c-123">Key value generation</span></span>

<span data-ttu-id="2675c-124">Include la generazione lato client e la generazione del database.</span><span class="sxs-lookup"><span data-stu-id="2675c-124">Including client-side generation and database generation.</span></span>

### <a name="database-generated-values"></a><span data-ttu-id="2675c-125">Valori generati dal database</span><span class="sxs-lookup"><span data-stu-id="2675c-125">Database generated values</span></span>

<span data-ttu-id="2675c-126">Consente la generazione di valori dal database in caso di inserimento (valori predefiniti) o di aggiornamento (colonne calcolate).</span><span class="sxs-lookup"><span data-stu-id="2675c-126">Allows for values to be generated by the database on insert (default values) or update (computed columns).</span></span>

### <a name="sequences-in-sql-server"></a><span data-ttu-id="2675c-127">Sequenze in SQL Server</span><span class="sxs-lookup"><span data-stu-id="2675c-127">Sequences in SQL Server</span></span>

<span data-ttu-id="2675c-128">Consente la definizione di oggetti sequenza nel modello.</span><span class="sxs-lookup"><span data-stu-id="2675c-128">Allows for sequence objects to be defined in the model.</span></span>

### <a name="unique-constraints"></a><span data-ttu-id="2675c-129">Vincoli UNIQUE</span><span class="sxs-lookup"><span data-stu-id="2675c-129">Unique constraints</span></span>

<span data-ttu-id="2675c-130">Consente la definizione una chiave alternativa e di relazioni che specificano come destinazione tale chiave.</span><span class="sxs-lookup"><span data-stu-id="2675c-130">Allows for the definition of alternate keys and the ability to define relationships that target that key.</span></span>

### <a name="indexes"></a><span data-ttu-id="2675c-131">Indici</span><span class="sxs-lookup"><span data-stu-id="2675c-131">Indexes</span></span>

<span data-ttu-id="2675c-132">La definizione automatica di indici nel modello introduce gli indici nel database.</span><span class="sxs-lookup"><span data-stu-id="2675c-132">Defining indexes in the model automatically introduces indexes in the database.</span></span> <span data-ttu-id="2675c-133">Sono supportati anche gli indici univoci.</span><span class="sxs-lookup"><span data-stu-id="2675c-133">Unique indexes are also supported.</span></span>

### <a name="shadow-state-properties"></a><span data-ttu-id="2675c-134">Proprietà con stato shadow</span><span class="sxs-lookup"><span data-stu-id="2675c-134">Shadow state properties</span></span>

<span data-ttu-id="2675c-135">Consente di definire nel modello proprietà che non vengono dichiarate né archiviate nella classe .NET, ma di cui è possibile tenere traccia e che possono essere aggiornate da EF Core.</span><span class="sxs-lookup"><span data-stu-id="2675c-135">Allows for properties to be defined in the model that are not declared and are not stored in the .NET class but can be tracked and updated by EF Core.</span></span> <span data-ttu-id="2675c-136">Questa funzionalità viene in genere usata per le proprietà di una chiave esterna quando non si vuole esporle nell'oggetto.</span><span class="sxs-lookup"><span data-stu-id="2675c-136">Commonly used for foreign key properties when exposing these in the object is not desired.</span></span>

### <a name="table-per-hierarchy-inheritance-pattern"></a><span data-ttu-id="2675c-137">Modello di tabella per gerarchia di ereditarietà</span><span class="sxs-lookup"><span data-stu-id="2675c-137">Table-Per-Hierarchy inheritance pattern</span></span>

<span data-ttu-id="2675c-138">Consente di salvare le entità di una gerarchia di ereditarietà in una singola tabella usando una colonna discriminante per identificare il tipo di entità per un determinato record del database.</span><span class="sxs-lookup"><span data-stu-id="2675c-138">Allows entities in an inheritance hierarchy to be saved to a single table using a discriminator column to identify they entity type for a given record in the database.</span></span>

### <a name="model-validation"></a><span data-ttu-id="2675c-139">Convalida modello</span><span class="sxs-lookup"><span data-stu-id="2675c-139">Model validation</span></span>

<span data-ttu-id="2675c-140">Rileva i criteri non validi nel modello e visualizza utili messaggi di errore.</span><span class="sxs-lookup"><span data-stu-id="2675c-140">Detects invalid patterns in the model and provides helpful error messages.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="2675c-141">Rilevamento modifiche</span><span class="sxs-lookup"><span data-stu-id="2675c-141">Change tracking</span></span>

### <a name="snapshot-change-tracking"></a><span data-ttu-id="2675c-142">Rilevamento modifiche basato su snapshot</span><span class="sxs-lookup"><span data-stu-id="2675c-142">Snapshot change tracking</span></span>

<span data-ttu-id="2675c-143">Consente di rilevare automaticamente le modifiche nelle entità confrontando lo stato corrente con una copia (snapshot) dello stato originale.</span><span class="sxs-lookup"><span data-stu-id="2675c-143">Allows changes in entities to be detected automatically by comparing current state against a copy (snapshot) of the original state.</span></span>

### <a name="notification-change-tracking"></a><span data-ttu-id="2675c-144">Rilevamento modifiche con notifica</span><span class="sxs-lookup"><span data-stu-id="2675c-144">Notification change tracking</span></span>

<span data-ttu-id="2675c-145">Consente alle entità di notificare all'utilità di rilevamento modifiche quando vengono modificati i valori di una proprietà.</span><span class="sxs-lookup"><span data-stu-id="2675c-145">Allows your entities to notify the change tracker when property values are modified.</span></span>

### <a name="accessing-tracked-state"></a><span data-ttu-id="2675c-146">Accesso allo stato registrato</span><span class="sxs-lookup"><span data-stu-id="2675c-146">Accessing tracked state</span></span>

<span data-ttu-id="2675c-147">Tramite `DbContext.Entry` e `DbContext.ChangeTracker`.</span><span class="sxs-lookup"><span data-stu-id="2675c-147">Via `DbContext.Entry` and `DbContext.ChangeTracker`.</span></span>

### <a name="attaching-detached-entitiesgraphs"></a><span data-ttu-id="2675c-148">Collegamento di entità o grafi scollegati</span><span class="sxs-lookup"><span data-stu-id="2675c-148">Attaching detached entities/graphs</span></span>

<span data-ttu-id="2675c-149">La nuova API `DbContext.AttachGraph` consente di ricollegare le entità a un contesto per salvare entità nuove/modificate.</span><span class="sxs-lookup"><span data-stu-id="2675c-149">The new `DbContext.AttachGraph` API helps re-attach entities to a context in order to save new/modified entities.</span></span>

## <a name="saving-data"></a><span data-ttu-id="2675c-150">Salvataggio dei dati</span><span class="sxs-lookup"><span data-stu-id="2675c-150">Saving data</span></span>

### <a name="basic-save-functionality"></a><span data-ttu-id="2675c-151">Funzionalità di salvataggio di base</span><span class="sxs-lookup"><span data-stu-id="2675c-151">Basic save functionality</span></span>

<span data-ttu-id="2675c-152">Consente di rendere persistenti nel database le modifiche apportate alle istanze delle entità.</span><span class="sxs-lookup"><span data-stu-id="2675c-152">Allows changes to entity instances to be persisted to the database.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="2675c-153">Concorrenza ottimistica</span><span class="sxs-lookup"><span data-stu-id="2675c-153">Optimistic Concurrency</span></span>

<span data-ttu-id="2675c-154">Protegge dalla sovrascrittura delle modifiche apportate da un altro utente dopo che i dati sono stati recuperati dal database.</span><span class="sxs-lookup"><span data-stu-id="2675c-154">Protects against overwriting changes made by another user since data was fetched from the database.</span></span>

### <a name="async-savechanges"></a><span data-ttu-id="2675c-155">SaveChanges asincrono</span><span class="sxs-lookup"><span data-stu-id="2675c-155">Async SaveChanges</span></span>

<span data-ttu-id="2675c-156">Consente di liberare il thread corrente per elaborare altre richieste mentre il database elabora i comandi eseguiti da `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="2675c-156">Can free up the current thread to process other requests while the database processes the commands issued from `SaveChanges`.</span></span>

### <a name="database-transactions"></a><span data-ttu-id="2675c-157">Transazioni del database</span><span class="sxs-lookup"><span data-stu-id="2675c-157">Database Transactions</span></span>

<span data-ttu-id="2675c-158">Indica che `SaveChanges` è sempre atomico, ovvero che ha avuto esito completamente positivo o che non sono state apportate modifiche al database.</span><span class="sxs-lookup"><span data-stu-id="2675c-158">Means that `SaveChanges` is always atomic (meaning it either completely succeeds, or no changes are made to the database).</span></span> <span data-ttu-id="2675c-159">Esistono anche API relative alle transazioni che consentono la condivisione delle transazioni tra le istanze del contesto e così via.</span><span class="sxs-lookup"><span data-stu-id="2675c-159">There are also transaction related APIs to allow sharing transactions between context instances etc.</span></span>

### <a name="relational-batching-of-statements"></a><span data-ttu-id="2675c-160">Relazionale: invio in batch di istruzioni</span><span class="sxs-lookup"><span data-stu-id="2675c-160">Relational: Batching of statements</span></span>

<span data-ttu-id="2675c-161">Consente di ottenere migliori prestazioni inviando in batch più comandi INSERT/UPDATE/DELETE in un singolo round trip al database.</span><span class="sxs-lookup"><span data-stu-id="2675c-161">Provides better performance by batching up multiple INSERT/UPDATE/DELETE commands into a single roundtrip to the database.</span></span>

## <a name="query"></a><span data-ttu-id="2675c-162">Query</span><span class="sxs-lookup"><span data-stu-id="2675c-162">Query</span></span>

### <a name="basic-linq-support"></a><span data-ttu-id="2675c-163">Supporto LINQ di base</span><span class="sxs-lookup"><span data-stu-id="2675c-163">Basic LINQ support</span></span>

<span data-ttu-id="2675c-164">Consente di usare LINQ per recuperare i dati dal database.</span><span class="sxs-lookup"><span data-stu-id="2675c-164">Provides the ability to use LINQ to retrieve data from the database.</span></span>

### <a name="mixed-clientserver-evaluation"></a><span data-ttu-id="2675c-165">Valutazione client/server mista</span><span class="sxs-lookup"><span data-stu-id="2675c-165">Mixed client/server evaluation</span></span>

<span data-ttu-id="2675c-166">Consente alle query di contenere la logica che non può essere valutata nel database e deve quindi essere valutata dopo che i dati sono stati recuperati nella memoria.</span><span class="sxs-lookup"><span data-stu-id="2675c-166">Enables queries to contain logic that cannot be evaluated in the database, and must therefore be evaluated after the data is retrieved into memory.</span></span>

### <a name="notracking"></a><span data-ttu-id="2675c-167">NoTracking</span><span class="sxs-lookup"><span data-stu-id="2675c-167">NoTracking</span></span>

<span data-ttu-id="2675c-168">Consente un'esecuzione più rapida delle query quando il contesto non deve monitorare le modifiche alle istanze delle entità (utile quando i risultati sono di sola lettura).</span><span class="sxs-lookup"><span data-stu-id="2675c-168">Queries enables quicker query execution when the context does not need to monitor for changes to the entity instances (this is useful if the results are read-only).</span></span>

### <a name="eager-loading"></a><span data-ttu-id="2675c-169">Caricamento eager</span><span class="sxs-lookup"><span data-stu-id="2675c-169">Eager loading</span></span>

<span data-ttu-id="2675c-170">Fornisce i metodi `Include` e `ThenInclude` per identificare i dati correlati che devono anche essere recuperati durante le query.</span><span class="sxs-lookup"><span data-stu-id="2675c-170">Provides the `Include` and `ThenInclude` methods to identify related data that should also be fetched when querying.</span></span>

### <a name="async-query"></a><span data-ttu-id="2675c-171">Query asincrona</span><span class="sxs-lookup"><span data-stu-id="2675c-171">Async query</span></span>

<span data-ttu-id="2675c-172">Consente di liberare il thread corrente e le risorse associate per elaborare altre richieste mentre il database elabora la query.</span><span class="sxs-lookup"><span data-stu-id="2675c-172">Can free up the current thread (and it's associated resources) to process other requests while the database processes the query.</span></span>

### <a name="raw-sql-queries"></a><span data-ttu-id="2675c-173">Query SQL non elaborate</span><span class="sxs-lookup"><span data-stu-id="2675c-173">Raw SQL queries</span></span>

<span data-ttu-id="2675c-174">Questa funzionalità fornisce il metodo `DbSet.FromSql` per usare le query SQL non elaborate per recuperare i dati.</span><span class="sxs-lookup"><span data-stu-id="2675c-174">Provides the `DbSet.FromSql` method to use raw SQL queries to fetch data.</span></span> <span data-ttu-id="2675c-175">Queste query possono anche essere composte per l'uso di LINQ.</span><span class="sxs-lookup"><span data-stu-id="2675c-175">These queries can also be composed on using LINQ.</span></span>

## <a name="database-schema-management"></a><span data-ttu-id="2675c-176">Gestione dello schema del database</span><span class="sxs-lookup"><span data-stu-id="2675c-176">Database schema management</span></span>

### <a name="database-creationdeletion-apis"></a><span data-ttu-id="2675c-177">API di creazione/eliminazione del database</span><span class="sxs-lookup"><span data-stu-id="2675c-177">Database creation/deletion APIs</span></span>

<span data-ttu-id="2675c-178">Sono progettate principalmente per i test in cui si vuole creare/eliminare rapidamente il database senza usare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="2675c-178">Are mostly designed for testing where you want to quickly create/delete the database without using migrations.</span></span>

### <a name="relational-database-migrations"></a><span data-ttu-id="2675c-179">Migrazioni di database relazionali</span><span class="sxs-lookup"><span data-stu-id="2675c-179">Relational database migrations</span></span>

<span data-ttu-id="2675c-180">Consentono l'evoluzione di uno schema del database relazionale nel corso del tempo in base alle modifiche del modello.</span><span class="sxs-lookup"><span data-stu-id="2675c-180">Allow a relational database schema to evolve overtime as your model changes.</span></span>

### <a name="reverse-engineer-from-database"></a><span data-ttu-id="2675c-181">Reverse engineering dal database</span><span class="sxs-lookup"><span data-stu-id="2675c-181">Reverse engineer from database</span></span>

<span data-ttu-id="2675c-182">Esegue lo scaffolding di un modello di EF basato su uno schema del database relazionale esistente.</span><span class="sxs-lookup"><span data-stu-id="2675c-182">Scaffolds an EF model based on an existing relational database schema.</span></span>

## <a name="database-providers"></a><span data-ttu-id="2675c-183">Provider di database</span><span class="sxs-lookup"><span data-stu-id="2675c-183">Database providers</span></span>

### <a name="sql-server"></a><span data-ttu-id="2675c-184">SQL Server</span><span class="sxs-lookup"><span data-stu-id="2675c-184">SQL Server</span></span>

<span data-ttu-id="2675c-185">Si connette a Microsoft SQL Server 2008 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="2675c-185">Connects to Microsoft SQL Server 2008 onwards.</span></span>

### <a name="sqlite"></a><span data-ttu-id="2675c-186">SQLite</span><span class="sxs-lookup"><span data-stu-id="2675c-186">SQLite</span></span>

<span data-ttu-id="2675c-187">Si connette a un database SQLite 3.</span><span class="sxs-lookup"><span data-stu-id="2675c-187">Connects to a SQLite 3 database.</span></span>

### <a name="in-memory"></a><span data-ttu-id="2675c-188">In memoria</span><span class="sxs-lookup"><span data-stu-id="2675c-188">In-Memory</span></span>

<span data-ttu-id="2675c-189">È progettato per consentire facilmente il test senza connettersi a un vero database.</span><span class="sxs-lookup"><span data-stu-id="2675c-189">Is designed to easily enable testing without connecting to a real database.</span></span>

### <a name="3rd-party-providers"></a><span data-ttu-id="2675c-190">Provider di terze parti</span><span class="sxs-lookup"><span data-stu-id="2675c-190">3rd party providers</span></span>

<span data-ttu-id="2675c-191">Sono disponibili diversi provider per gli altri motori di database.</span><span class="sxs-lookup"><span data-stu-id="2675c-191">Several providers are available for other database engines.</span></span> <span data-ttu-id="2675c-192">Per un elenco completo, vedere [Database Providers](../providers/index.md) (Provider di database).</span><span class="sxs-lookup"><span data-stu-id="2675c-192">See [Database Providers](../providers/index.md) for a complete list.</span></span>
