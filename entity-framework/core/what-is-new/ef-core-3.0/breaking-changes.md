---
title: Modifiche che causano un'interruzione in EF Core 3.0 - EF Core
author: ajcvickers
ms.date: 12/03/2019
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: d614103169837238810fabd0a8889043c851ef14
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824872"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="2672f-102">Modifiche di rilievo incluse nel EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="2672f-102">Breaking changes included in EF Core 3.0</span></span>

<span data-ttu-id="2672f-103">Le modifiche alle API e al comportamento seguenti possono causare l'interruzione delle applicazioni esistenti durante l'aggiornamento a 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="2672f-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="2672f-104">Le modifiche che si prevede abbiano impatto solo sui provider di database sono documentate nelle [modifiche che influiscono sul provider](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="2672f-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="2672f-105">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="2672f-105">Summary</span></span>

| <span data-ttu-id="2672f-106">**Modifica di rilievo**</span><span class="sxs-lookup"><span data-stu-id="2672f-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="2672f-107">**Impatto**</span><span class="sxs-lookup"><span data-stu-id="2672f-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="2672f-108">Le query LINQ non vengono più valutate nel client</span><span class="sxs-lookup"><span data-stu-id="2672f-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="2672f-109">High</span><span class="sxs-lookup"><span data-stu-id="2672f-109">High</span></span>       |
| [<span data-ttu-id="2672f-110">EF Core 3.0 usa come destinazione .NET Standard 2.1 invece che .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="2672f-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="2672f-111">High</span><span class="sxs-lookup"><span data-stu-id="2672f-111">High</span></span>      |
| [<span data-ttu-id="2672f-112">Lo strumento da riga di comando di EF Core, dotnet ef, non è più incluso in .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="2672f-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="2672f-113">High</span><span class="sxs-lookup"><span data-stu-id="2672f-113">High</span></span>      |
| [<span data-ttu-id="2672f-114">DetectChanges rispetta i valori di chiave generati dall'archivio</span><span class="sxs-lookup"><span data-stu-id="2672f-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="2672f-115">High</span><span class="sxs-lookup"><span data-stu-id="2672f-115">High</span></span>      |
| [<span data-ttu-id="2672f-116">I metodi FromSql, ExecuteSql ed ExecuteSqlAsync sono stati rinominati</span><span class="sxs-lookup"><span data-stu-id="2672f-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="2672f-117">High</span><span class="sxs-lookup"><span data-stu-id="2672f-117">High</span></span>      |
| [<span data-ttu-id="2672f-118">I tipi di query vengono consolidati con tipi di entità</span><span class="sxs-lookup"><span data-stu-id="2672f-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="2672f-119">High</span><span class="sxs-lookup"><span data-stu-id="2672f-119">High</span></span>      |
| [<span data-ttu-id="2672f-120">Entity Framework Core non è più incluso nel framework condiviso di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2672f-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="2672f-121">Media</span><span class="sxs-lookup"><span data-stu-id="2672f-121">Medium</span></span>      |
| [<span data-ttu-id="2672f-122">Le eliminazioni a catena vengono ora eseguite immediatamente per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="2672f-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="2672f-123">Media</span><span class="sxs-lookup"><span data-stu-id="2672f-123">Medium</span></span>      |
| [<span data-ttu-id="2672f-124">Il caricamento eager delle entità correlate ora si verifica in una singola query</span><span class="sxs-lookup"><span data-stu-id="2672f-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="2672f-125">Media</span><span class="sxs-lookup"><span data-stu-id="2672f-125">Medium</span></span>      |
| [<span data-ttu-id="2672f-126">Semantica più chiara per DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="2672f-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="2672f-127">Media</span><span class="sxs-lookup"><span data-stu-id="2672f-127">Medium</span></span>      |
| [<span data-ttu-id="2672f-128">L'API di configurazione per le relazioni di tipo di proprietà è stata modificata</span><span class="sxs-lookup"><span data-stu-id="2672f-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="2672f-129">Media</span><span class="sxs-lookup"><span data-stu-id="2672f-129">Medium</span></span>      |
| [<span data-ttu-id="2672f-130">Ogni proprietà usa la generazione di chiavi di tipo intero in memoria indipendenti</span><span class="sxs-lookup"><span data-stu-id="2672f-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="2672f-131">Media</span><span class="sxs-lookup"><span data-stu-id="2672f-131">Medium</span></span>      |
| [<span data-ttu-id="2672f-132">Le query senza rilevamento delle modifiche non eseguono più la risoluzione delle identità</span><span class="sxs-lookup"><span data-stu-id="2672f-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="2672f-133">Media</span><span class="sxs-lookup"><span data-stu-id="2672f-133">Medium</span></span>      |
| [<span data-ttu-id="2672f-134">Modifiche dell'API dei metadati</span><span class="sxs-lookup"><span data-stu-id="2672f-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="2672f-135">Media</span><span class="sxs-lookup"><span data-stu-id="2672f-135">Medium</span></span>      |
| [<span data-ttu-id="2672f-136">Modifiche dell'API dei metadati specifiche del provider</span><span class="sxs-lookup"><span data-stu-id="2672f-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="2672f-137">Media</span><span class="sxs-lookup"><span data-stu-id="2672f-137">Medium</span></span>      |
| [<span data-ttu-id="2672f-138">Il metodo UseRowNumberForPaging è stato rimosso</span><span class="sxs-lookup"><span data-stu-id="2672f-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="2672f-139">Media</span><span class="sxs-lookup"><span data-stu-id="2672f-139">Medium</span></span>      |
| [<span data-ttu-id="2672f-140">Non è possibile comporre il metodo dati da tabelle se usato con stored procedure</span><span class="sxs-lookup"><span data-stu-id="2672f-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="2672f-141">Media</span><span class="sxs-lookup"><span data-stu-id="2672f-141">Medium</span></span>      |
| [<span data-ttu-id="2672f-142">I metodi FromSql possono essere specificati solo in radici di query</span><span class="sxs-lookup"><span data-stu-id="2672f-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="2672f-143">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-143">Low</span></span>      |
| [<span data-ttu-id="2672f-144">~~L'esecuzione di query viene registrata a livello di debug~~ - Modifica annullata</span><span class="sxs-lookup"><span data-stu-id="2672f-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="2672f-145">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-145">Low</span></span>      |
| [<span data-ttu-id="2672f-146">I valori di chiave temporanei non sono più impostati nelle istanze di entità</span><span class="sxs-lookup"><span data-stu-id="2672f-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="2672f-147">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-147">Low</span></span>      |
| [<span data-ttu-id="2672f-148">Le entità dipendenti che condividono la tabella con l'entità di sicurezza sono ora facoltative</span><span class="sxs-lookup"><span data-stu-id="2672f-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="2672f-149">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-149">Low</span></span>      |
| [<span data-ttu-id="2672f-150">Tutte le entità che condividono una tabella con una colonna di token di concorrenza devono eseguirne il mapping a una proprietà</span><span class="sxs-lookup"><span data-stu-id="2672f-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="2672f-151">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-151">Low</span></span>      |
| [<span data-ttu-id="2672f-152">Non è possibile eseguire query sulle entità di proprietà senza il proprietario utilizzando una query di rilevamento</span><span class="sxs-lookup"><span data-stu-id="2672f-152">Owned entities cannot be queried without the owner using a tracking query</span></span>](#owned-query) | <span data-ttu-id="2672f-153">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-153">Low</span></span>      |
| [<span data-ttu-id="2672f-154">Per le proprietà ereditate da tipi senza mapping viene ora eseguito il mapping a una singola colonna per tutti i tipi derivati</span><span class="sxs-lookup"><span data-stu-id="2672f-154">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="2672f-155">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-155">Low</span></span>      |
| [<span data-ttu-id="2672f-156">La convenzione di proprietà di chiave esterna non ha più lo stesso nome della proprietà dell'entità di sicurezza</span><span class="sxs-lookup"><span data-stu-id="2672f-156">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="2672f-157">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-157">Low</span></span>      |
| [<span data-ttu-id="2672f-158">La connessione di database viene ora chiusa se non viene più usata prima del completamento di TransactionScope</span><span class="sxs-lookup"><span data-stu-id="2672f-158">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="2672f-159">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-159">Low</span></span>      |
| [<span data-ttu-id="2672f-160">I campi sottostanti vengono usati per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="2672f-160">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="2672f-161">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-161">Low</span></span>      |
| [<span data-ttu-id="2672f-162">Viene generata un'eccezione se vengono trovati più campi sottostanti compatibili</span><span class="sxs-lookup"><span data-stu-id="2672f-162">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="2672f-163">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-163">Low</span></span>      |
| [<span data-ttu-id="2672f-164">I nomi delle proprietà solo campo devono corrispondere al nome di campo</span><span class="sxs-lookup"><span data-stu-id="2672f-164">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="2672f-165">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-165">Low</span></span>      |
| [<span data-ttu-id="2672f-166">AddDbContext/AddDbContextPool non chiamano più AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="2672f-166">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="2672f-167">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-167">Low</span></span>      |
| [<span data-ttu-id="2672f-168">AddEntityFramework \* aggiunge IMemoryCache con un limite di dimensioni</span><span class="sxs-lookup"><span data-stu-id="2672f-168">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>](#addentityframework-adds-imemorycache-with-a-size-limit) | <span data-ttu-id="2672f-169">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-169">Low</span></span>      |
| [<span data-ttu-id="2672f-170">DbContext.Entry esegue ora un DetectChanges locale</span><span class="sxs-lookup"><span data-stu-id="2672f-170">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="2672f-171">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-171">Low</span></span>      |
| [<span data-ttu-id="2672f-172">Le chiavi matrice di byte e di stringhe non vengono generate dal client per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="2672f-172">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="2672f-173">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-173">Low</span></span>      |
| [<span data-ttu-id="2672f-174">ILoggerFactory è ora un servizio con ambito</span><span class="sxs-lookup"><span data-stu-id="2672f-174">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="2672f-175">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-175">Low</span></span>      |
| [<span data-ttu-id="2672f-176">I proxy di caricamento lazy non presuppongono più che le proprietà di navigazione vengano caricate completamente</span><span class="sxs-lookup"><span data-stu-id="2672f-176">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="2672f-177">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-177">Low</span></span>      |
| [<span data-ttu-id="2672f-178">La creazione di un numero eccessivo di provider di servizi interni è ora un errore per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="2672f-178">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="2672f-179">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-179">Low</span></span>      |
| [<span data-ttu-id="2672f-180">Nuovo comportamento per la chiamata di HasOne/HasMany con una singola stringa</span><span class="sxs-lookup"><span data-stu-id="2672f-180">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="2672f-181">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-181">Low</span></span>      |
| [<span data-ttu-id="2672f-182">Il tipo restituito per diversi metodi asincroni è cambiato da Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="2672f-182">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="2672f-183">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-183">Low</span></span>      |
| [<span data-ttu-id="2672f-184">L'annotazione Relational:TypeMapping è ora TypeMapping</span><span class="sxs-lookup"><span data-stu-id="2672f-184">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="2672f-185">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-185">Low</span></span>      |
| [<span data-ttu-id="2672f-186">ToTable in un tipo derivato genera un'eccezione</span><span class="sxs-lookup"><span data-stu-id="2672f-186">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="2672f-187">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-187">Low</span></span>      |
| [<span data-ttu-id="2672f-188">EF Core non invia più pragma per l'imposizione della chiave esterna di SQLite</span><span class="sxs-lookup"><span data-stu-id="2672f-188">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="2672f-189">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-189">Low</span></span>      |
| [<span data-ttu-id="2672f-190">Microsoft.EntityFrameworkCore.Sqlite dipende ora da SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="2672f-190">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="2672f-191">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-191">Low</span></span>      |
| [<span data-ttu-id="2672f-192">I valori Guid vengono ora archiviati come TEXT in SQLite</span><span class="sxs-lookup"><span data-stu-id="2672f-192">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="2672f-193">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-193">Low</span></span>      |
| [<span data-ttu-id="2672f-194">I valori char vengono ora archiviati come testo in SQLite</span><span class="sxs-lookup"><span data-stu-id="2672f-194">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="2672f-195">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-195">Low</span></span>      |
| [<span data-ttu-id="2672f-196">Gli ID di migrazione vengono ora generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica</span><span class="sxs-lookup"><span data-stu-id="2672f-196">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="2672f-197">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-197">Low</span></span>      |
| [<span data-ttu-id="2672f-198">Info/metadati dell'estensione rimossi da IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="2672f-198">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="2672f-199">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-199">Low</span></span>      |
| [<span data-ttu-id="2672f-200">LogQueryPossibleExceptionWithAggregateOperator è stato rinominato</span><span class="sxs-lookup"><span data-stu-id="2672f-200">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="2672f-201">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-201">Low</span></span>      |
| [<span data-ttu-id="2672f-202">Chiarimenti per l'API per i nomi di vincolo di chiave esterna</span><span class="sxs-lookup"><span data-stu-id="2672f-202">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="2672f-203">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-203">Low</span></span>      |
| [<span data-ttu-id="2672f-204">IRelationalDatabaseCreator.HasTables/HasTablesAsync sono diventati pubblici</span><span class="sxs-lookup"><span data-stu-id="2672f-204">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="2672f-205">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-205">Low</span></span>      |
| [<span data-ttu-id="2672f-206">Microsoft.EntityFrameworkCore.Design è ora un pacchetto DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="2672f-206">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="2672f-207">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-207">Low</span></span>      |
| [<span data-ttu-id="2672f-208">Aggiornamento di SQLitePCL.raw alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2672f-208">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="2672f-209">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-209">Low</span></span>      |
| [<span data-ttu-id="2672f-210">NetTopologySuite aggiornato alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2672f-210">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="2672f-211">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-211">Low</span></span>      |
| [<span data-ttu-id="2672f-212">Viene usato Microsoft. Data. SqlClient al posto di System. Data. SqlClient</span><span class="sxs-lookup"><span data-stu-id="2672f-212">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="2672f-213">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-213">Low</span></span>      |
| [<span data-ttu-id="2672f-214">Devono essere configurare più relazioni ambigue che fanno riferimento a se stesse</span><span class="sxs-lookup"><span data-stu-id="2672f-214">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="2672f-215">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-215">Low</span></span>      |
| [<span data-ttu-id="2672f-216">DbFunction. Schema è una stringa null o vuota che lo configura in modo che sia nello schema predefinito del modello</span><span class="sxs-lookup"><span data-stu-id="2672f-216">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="2672f-217">Basso</span><span class="sxs-lookup"><span data-stu-id="2672f-217">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="2672f-218">Le query LINQ non vengono più valutate nel client</span><span class="sxs-lookup"><span data-stu-id="2672f-218">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="2672f-219">[Problema n. 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Vedere anche il problema n. 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="2672f-219">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="2672f-220">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-220">**Old behavior**</span></span>

<span data-ttu-id="2672f-221">Nelle versioni precedenti alla versione 3.0, quando EF Core non era in grado di convertire un'espressione inclusa in una query in SQL o in un parametro, l'espressione veniva automaticamente valutata nel client.</span><span class="sxs-lookup"><span data-stu-id="2672f-221">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="2672f-222">Per impostazione predefinita, la valutazione client di espressioni potenzialmente dispendiose si limitava ad attivare solo un avviso.</span><span class="sxs-lookup"><span data-stu-id="2672f-222">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="2672f-223">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-223">**New behavior**</span></span>

<span data-ttu-id="2672f-224">A partire dalla versione 3.0, EF Core consente solo la valutazione delle espressioni nella proiezione di primo livello (l'ultima chiamata `Select()` nella query) nel client.</span><span class="sxs-lookup"><span data-stu-id="2672f-224">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="2672f-225">Quando le espressioni in altre posizioni all'interno della query non possono essere convertite in SQL o in un parametro, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="2672f-225">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="2672f-226">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-226">**Why**</span></span>

<span data-ttu-id="2672f-227">La valutazione client automatica delle query consente di eseguire numerose query anche nel caso in cui parti importanti delle query non possono essere convertite.</span><span class="sxs-lookup"><span data-stu-id="2672f-227">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="2672f-228">Questo comportamento può causare un comportamento imprevisto e potenzialmente dannoso che può diventare evidente solo in produzione.</span><span class="sxs-lookup"><span data-stu-id="2672f-228">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="2672f-229">Ad esempio, una condizione in una chiamata `Where()` che non può essere convertita può causare il trasferimento di tutte le righe della tabella del server di database e l'applicazione del filtro nel client.</span><span class="sxs-lookup"><span data-stu-id="2672f-229">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="2672f-230">È probabile che questa situazione non venga rilevata se la tabella contiene solo alcune righe in fase di sviluppo, ma che abbia un grande impatto quando l'applicazione passa in produzione dove la tabella può contenere milioni di righe.</span><span class="sxs-lookup"><span data-stu-id="2672f-230">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="2672f-231">Gli avvisi di valutazione client inoltre si sono rivelati molto facili da ignorare durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="2672f-231">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="2672f-232">Inoltre, la valutazione client automatica può causare problemi in cui il miglioramento della conversione di query per espressioni specifiche causa modifiche impreviste che causano un'interruzione da una versione all'altra.</span><span class="sxs-lookup"><span data-stu-id="2672f-232">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="2672f-233">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-233">**Mitigations**</span></span>

<span data-ttu-id="2672f-234">Se una query non può essere convertita completamente, riscrivere la query in un formato che possa essere convertito o usare `AsEnumerable()`, `ToList()` o un elemento simile per riportare in modo esplicito i dati nel client dove possono essere quindi ulteriormente elaborati usando LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="2672f-234">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="2672f-235">EF Core 3.0 usa come destinazione .NET Standard 2.1 invece che .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="2672f-235">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="2672f-236">Problema n. 15498</span><span class="sxs-lookup"><span data-stu-id="2672f-236">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="2672f-237">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-237">**Old behavior**</span></span>

<span data-ttu-id="2672f-238">Prima della versione 3.0, EF Core usava come destinazione .NET Standard 2.0 e poteva essere eseguito in tutte le piattaforme che supportano tale standard, incluso .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2672f-238">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="2672f-239">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-239">**New behavior**</span></span>

<span data-ttu-id="2672f-240">A partire dalla versione 3.0, EF Core usa come destinazione .NET Standard 2.1 e verrà eseguito in tutte le piattaforme che supportano questo standard.</span><span class="sxs-lookup"><span data-stu-id="2672f-240">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="2672f-241">.NET Framework non è incluso.</span><span class="sxs-lookup"><span data-stu-id="2672f-241">This does not include .NET Framework.</span></span>

<span data-ttu-id="2672f-242">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-242">**Why**</span></span>

<span data-ttu-id="2672f-243">Questo comportamento deriva da una decisione strategica per le tecnologie .NET, che mira a concentrare le energie su .NET Core e su altre piattaforme .NET moderne, ad esempio Xamarin.</span><span class="sxs-lookup"><span data-stu-id="2672f-243">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="2672f-244">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-244">**Mitigations**</span></span>

<span data-ttu-id="2672f-245">Valutare il passaggio a una piattaforma .NET moderna.</span><span class="sxs-lookup"><span data-stu-id="2672f-245">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="2672f-246">Se non è possibile, continuare a usare EF Core 2.1 o EF Core 2.2, che supportano entrambe .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2672f-246">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="2672f-247">Entity Framework Core non è più incluso nel framework condiviso di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2672f-247">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="2672f-248">Annunci problema n. 325</span><span class="sxs-lookup"><span data-stu-id="2672f-248">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="2672f-249">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-249">**Old behavior**</span></span>

<span data-ttu-id="2672f-250">Nelle versioni precedenti ad ASP.NET Core 3.0, quando si aggiungeva un riferimento a un pacchetto in `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, veniva inserito EF Core e alcuni dei provider di dati di EF Core come il provider di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2672f-250">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="2672f-251">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-251">**New behavior**</span></span>

<span data-ttu-id="2672f-252">A partire dalla versione 3.0, il framework condiviso di ASP.NET Core non include EF Core o provider di dati di EF Core.</span><span class="sxs-lookup"><span data-stu-id="2672f-252">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="2672f-253">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-253">**Why**</span></span>

<span data-ttu-id="2672f-254">Prima di questa modifica, per ottenere EF Core erano necessarie procedure diverse, a seconda che l'applicazione avesse o meno come destinazione ASP.NET Core e SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2672f-254">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="2672f-255">Inoltre, l'aggiornamento di ASP.NET Core forzava l'aggiornamento di EF Core e del provider di SQL Server, non sempre auspicabile.</span><span class="sxs-lookup"><span data-stu-id="2672f-255">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="2672f-256">Con questa modifica, la procedura per ottenere EF Core è la stessa in tutti i provider, le implementazioni .NET supportate e i tipi di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="2672f-256">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="2672f-257">Gli sviluppatori possono ora controllare anche in modo preciso quando vengono aggiornati EF Core e i provider di dati di EF Core.</span><span class="sxs-lookup"><span data-stu-id="2672f-257">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="2672f-258">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-258">**Mitigations**</span></span>

<span data-ttu-id="2672f-259">Per usare EF Core in un'applicazione ASP.NET Core 3.0 o in un'altra applicazione supportata, aggiungere in modo esplicito un riferimento al pacchetto al provider di database di EF Core che verrà usato dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2672f-259">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="2672f-260">Lo strumento della riga di comando di EF Core, dotnet ef, non è più incluso in .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="2672f-260">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="2672f-261">Problema n. 14016</span><span class="sxs-lookup"><span data-stu-id="2672f-261">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="2672f-262">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-262">**Old behavior**</span></span>

<span data-ttu-id="2672f-263">Prima della versione 3.0, lo strumento `dotnet ef` era incluso in .NET Core SDK ed era immediatamente disponibile dalla riga di comando di qualsiasi progetto senza richiedere passaggi aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="2672f-263">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="2672f-264">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-264">**New behavior**</span></span>

<span data-ttu-id="2672f-265">A partire dalla versione 3.0, .NET SDK non include lo strumento `dotnet ef` pertanto, prima di poterlo usare, è necessario installarlo in modo esplicito come strumento locale o globale.</span><span class="sxs-lookup"><span data-stu-id="2672f-265">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="2672f-266">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-266">**Why**</span></span>

<span data-ttu-id="2672f-267">Questa modifica consente di distribuire e aggiornare `dotnet ef` come uno strumento della riga di comando di .NET in NuGet, coerentemente con il fatto che anche EF Core 3.0 viene distribuito come pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="2672f-267">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="2672f-268">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-268">**Mitigations**</span></span>

<span data-ttu-id="2672f-269">Per essere in grado di gestire le migrazioni o eseguire lo scaffolding di `DbContext`, installare `dotnet-ef` come strumento globale:</span><span class="sxs-lookup"><span data-stu-id="2672f-269">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="2672f-270">È inoltre possibile ottenerlo come strumento locale quando si ripristinano le dipendenze di un progetto che lo dichiara come dipendenza di strumenti utilizzando un [file manifesto dello strumento](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="2672f-270">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="2672f-271">I metodi FromSql, ExecuteSql ed ExecuteSqlAsync sono stati rinominati</span><span class="sxs-lookup"><span data-stu-id="2672f-271">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="2672f-272">Problema n. 10996</span><span class="sxs-lookup"><span data-stu-id="2672f-272">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="2672f-273">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-273">**Old behavior**</span></span>

<span data-ttu-id="2672f-274">Prima di EF Core 3.0, erano disponibili overload per questi nomi di metodo per supportare l'uso con una stringa normale o una stringa che deve essere interpolata in SQL e parametri.</span><span class="sxs-lookup"><span data-stu-id="2672f-274">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="2672f-275">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-275">**New behavior**</span></span>

<span data-ttu-id="2672f-276">A partire da EF Core 3.0, usare `FromSqlRaw`, `ExecuteSqlRaw` e `ExecuteSqlRawAsync` per creare una query con parametri in cui i parametri vengono passati separatamente dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="2672f-276">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="2672f-277">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2672f-277">For example:</span></span>

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="2672f-278">Usare `FromSqlInterpolated`, `ExecuteSqlInterpolated`, e `ExecuteSqlInterpolatedAsync` per creare una query con parametri in cui i parametri vengono passati come parte di una stringa di query interpolata.</span><span class="sxs-lookup"><span data-stu-id="2672f-278">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="2672f-279">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2672f-279">For example:</span></span>

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="2672f-280">Si noti che entrambe le query precedenti produrranno lo stesso codice SQL con parametri con gli stessi parametri SQL.</span><span class="sxs-lookup"><span data-stu-id="2672f-280">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="2672f-281">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-281">**Why**</span></span>

<span data-ttu-id="2672f-282">Con gli overload di metodi come questi, è molto facile chiamare accidentalmente il metodo con stringa non elaborata anche se l'intento era chiamare il metodo con stringa interpolata e viceversa.</span><span class="sxs-lookup"><span data-stu-id="2672f-282">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="2672f-283">Il risultato potrebbero essere query senza parametri, quando invece è prevista la parametrizzazione.</span><span class="sxs-lookup"><span data-stu-id="2672f-283">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="2672f-284">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-284">**Mitigations**</span></span>

<span data-ttu-id="2672f-285">Passare all'uso dei nuovi nomi di metodo.</span><span class="sxs-lookup"><span data-stu-id="2672f-285">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="2672f-286">Non è possibile comporre il metodo dati da tabelle se usato con stored procedure</span><span class="sxs-lookup"><span data-stu-id="2672f-286">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="2672f-287">Rilevamento del problema #15392</span><span class="sxs-lookup"><span data-stu-id="2672f-287">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="2672f-288">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-288">**Old behavior**</span></span>

<span data-ttu-id="2672f-289">Prima di EF Core 3,0, il metodo dati da tabelle ha tentato di rilevare se il SQL passato può essere composto in base a.</span><span class="sxs-lookup"><span data-stu-id="2672f-289">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="2672f-290">Ha fatto la valutazione del client quando SQL era non componibile come un stored procedure.</span><span class="sxs-lookup"><span data-stu-id="2672f-290">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="2672f-291">La query seguente ha funzionato eseguendo il stored procedure sul server e FirstOrDefault sul lato client.</span><span class="sxs-lookup"><span data-stu-id="2672f-291">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="2672f-292">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-292">**New behavior**</span></span>

<span data-ttu-id="2672f-293">A partire da EF Core 3,0, EF Core non tenterà di analizzare SQL.</span><span class="sxs-lookup"><span data-stu-id="2672f-293">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="2672f-294">Quindi, se si esegue la composizione dopo FromSqlRaw/FromSqlInterpolated, EF Core comporrà il SQL causando una sottoquery.</span><span class="sxs-lookup"><span data-stu-id="2672f-294">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="2672f-295">Quindi, se si usa un stored procedure con Composition, si otterrà un'eccezione per la sintassi SQL non valida.</span><span class="sxs-lookup"><span data-stu-id="2672f-295">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="2672f-296">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-296">**Why**</span></span>

<span data-ttu-id="2672f-297">EF Core 3,0 non supporta la valutazione automatica del client perché è stata soggetta a errori, come illustrato [qui](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="2672f-297">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="2672f-298">**Mitigazione**</span><span class="sxs-lookup"><span data-stu-id="2672f-298">**Mitigation**</span></span>

<span data-ttu-id="2672f-299">Se si usa un stored procedure in FromSqlRaw/FromSqlInterpolated, è noto che non è possibile componerlo, quindi è possibile aggiungere __AsEnumerable/AsAsyncEnumerable__ subito dopo la chiamata al metodo dati da tabelle per evitare qualsiasi composizione sul lato server.</span><span class="sxs-lookup"><span data-stu-id="2672f-299">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="2672f-300">I metodi FromSql possono essere specificati solo in radici di query</span><span class="sxs-lookup"><span data-stu-id="2672f-300">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="2672f-301">Problema n. 15704</span><span class="sxs-lookup"><span data-stu-id="2672f-301">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="2672f-302">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-302">**Old behavior**</span></span>

<span data-ttu-id="2672f-303">Prima di EF Core 3.0, il metodo `FromSql` poteva essere specificato in un punto qualsiasi nella query.</span><span class="sxs-lookup"><span data-stu-id="2672f-303">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="2672f-304">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-304">**New behavior**</span></span>

<span data-ttu-id="2672f-305">A partire da EF Core 3.0, i nuovi metodi `FromSqlRaw` e `FromSqlInterpolated` (che sostituiscono`FromSql`) possono essere specificati solo per radici di query, ad esempio direttamente in `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="2672f-305">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="2672f-306">Qualsiasi tentativo di specificarli altrove causerà un errore di compilazione.</span><span class="sxs-lookup"><span data-stu-id="2672f-306">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="2672f-307">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-307">**Why**</span></span>

<span data-ttu-id="2672f-308">La specifica di `FromSql` in qualsiasi posizione diversa da un `DbSet` non ha alcun significato aggiuntivo oppure valore aggiunto e può causare ambiguità in determinati scenari.</span><span class="sxs-lookup"><span data-stu-id="2672f-308">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="2672f-309">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-309">**Mitigations**</span></span>

<span data-ttu-id="2672f-310">Le chiamate di `FromSql` devono essere spostate in modo da comparire direttamente nel `DbSet` a cui si applicano.</span><span class="sxs-lookup"><span data-stu-id="2672f-310">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="2672f-311">Le query senza rilevamento delle modifiche non eseguono più la risoluzione delle identità</span><span class="sxs-lookup"><span data-stu-id="2672f-311">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="2672f-312">Problema n. 13518</span><span class="sxs-lookup"><span data-stu-id="2672f-312">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="2672f-313">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-313">**Old behavior**</span></span>

<span data-ttu-id="2672f-314">Prima di EF Core 3.0, la stessa istanza di un'entità poteva essere usata per ogni occorrenza di un entità con tipo e ID specifici.</span><span class="sxs-lookup"><span data-stu-id="2672f-314">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="2672f-315">Questo comportamento corrisponde a quello delle query con rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="2672f-315">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="2672f-316">Ad esempio, questa query:</span><span class="sxs-lookup"><span data-stu-id="2672f-316">For example, this query:</span></span>

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="2672f-317">restituisce la stessa istanza di `Category` per ogni `Product` associato alla categoria specificata.</span><span class="sxs-lookup"><span data-stu-id="2672f-317">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="2672f-318">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-318">**New behavior**</span></span>

<span data-ttu-id="2672f-319">A partire da EF Core 3.0, vengono create istanze di entità diverse quando un'entità con un determinato tipo e ID viene rilevata in posizioni diverse nel grafico restituito.</span><span class="sxs-lookup"><span data-stu-id="2672f-319">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="2672f-320">La query precedente, ad esempio, ora restituirà una nuova istanza di `Category` per ogni `Product` anche quando due prodotti sono associati alla stessa categoria.</span><span class="sxs-lookup"><span data-stu-id="2672f-320">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="2672f-321">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-321">**Why**</span></span>

<span data-ttu-id="2672f-322">La risoluzione delle identità (ovvero il processo per determinare che un'entità ha lo stesso tipo e ID di un'entità rilevata in precedenza) aggiunge un ulteriore sovraccarico della memoria e delle prestazioni,</span><span class="sxs-lookup"><span data-stu-id="2672f-322">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="2672f-323">il che va ad annullare il vantaggio derivante dall'uso delle query senza rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="2672f-323">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="2672f-324">Inoltre, anche se in alcuni casi la risoluzione delle identità può essere utile, tuttavia non è necessaria se le entità devono essere serializzate e inviate a un client, come avviene comunemente con le query senza rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="2672f-324">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="2672f-325">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-325">**Mitigations**</span></span>

<span data-ttu-id="2672f-326">Se la risoluzione delle identità è necessaria, usare una query con rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="2672f-326">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="2672f-327">~~L'esecuzione di query viene registrata a livello di debug~~ - Modifica annullata</span><span class="sxs-lookup"><span data-stu-id="2672f-327">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="2672f-328">Problema n. 14523</span><span class="sxs-lookup"><span data-stu-id="2672f-328">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="2672f-329">Questa modifica è stata annullata perché la nuova configurazione in EF Core 3.0 consente all'applicazione di specificare il livello di log per qualsiasi evento.</span><span class="sxs-lookup"><span data-stu-id="2672f-329">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="2672f-330">Ad esempio, per impostare la registrazione di SQL sul livello `Debug`, configurare il livello in modo esplicito in `OnConfiguring` o `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="2672f-330">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="2672f-331">I valori di chiave temporanei non sono più impostati nelle istanze di entità</span><span class="sxs-lookup"><span data-stu-id="2672f-331">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="2672f-332">Problema n. 12378</span><span class="sxs-lookup"><span data-stu-id="2672f-332">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="2672f-333">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-333">**Old behavior**</span></span>

<span data-ttu-id="2672f-334">Nelle versioni precedenti a EF Core 3.0 i valori temporanei venivano assegnati a tutte le proprietà di chiave per cui veniva in seguito generato un valore reale dal database.</span><span class="sxs-lookup"><span data-stu-id="2672f-334">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="2672f-335">In genere questi valori temporanei erano numeri negativi elevati.</span><span class="sxs-lookup"><span data-stu-id="2672f-335">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="2672f-336">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-336">**New behavior**</span></span>

<span data-ttu-id="2672f-337">A partire dalla versione 3.0, EF Core archivia il valore di chiave temporaneo come parte delle informazioni di rilevamento dell'entità e non modifica la proprietà di chiave.</span><span class="sxs-lookup"><span data-stu-id="2672f-337">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="2672f-338">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-338">**Why**</span></span>

<span data-ttu-id="2672f-339">Questa modifica è stata apportata per impedire che i valori di chiave temporanei diventino erroneamente permanenti quando un'entità rilevata in precedenza da un'istanza `DbContext` viene spostata in un'altra istanza `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="2672f-339">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="2672f-340">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-340">**Mitigations**</span></span>

<span data-ttu-id="2672f-341">Le applicazioni che assegnano valori di chiave primaria in chiavi esterne per creare associazioni tra le entità possono dipendere dal comportamento precedente se le chiavi primarie vengono generate dall'archivio e appartengono a entità con stato `Added`.</span><span class="sxs-lookup"><span data-stu-id="2672f-341">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="2672f-342">Questo può essere evitato:</span><span class="sxs-lookup"><span data-stu-id="2672f-342">This can be avoided by:</span></span>
* <span data-ttu-id="2672f-343">Non usando chiavi generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="2672f-343">Not using store-generated keys.</span></span>
* <span data-ttu-id="2672f-344">Impostando le proprietà di navigazione in modo da creare relazioni anziché impostando valori di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="2672f-344">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="2672f-345">Ottenendo i valori di chiave temporanei effettivi dalle informazioni di rilevamento dell'entità.</span><span class="sxs-lookup"><span data-stu-id="2672f-345">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="2672f-346">Ad esempio, `context.Entry(blog).Property(e => e.Id).CurrentValue` restituisce il valore temporaneo anche quando `blog.Id` non è stato impostato.</span><span class="sxs-lookup"><span data-stu-id="2672f-346">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="2672f-347">DetectChanges rispetta i valori di chiave generati dall'archivio</span><span class="sxs-lookup"><span data-stu-id="2672f-347">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="2672f-348">Problema n. 14616</span><span class="sxs-lookup"><span data-stu-id="2672f-348">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="2672f-349">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-349">**Old behavior**</span></span>

<span data-ttu-id="2672f-350">Nelle versioni precedenti a EF Core 3.0 un'entità non rilevata individuata da `DetectChanges` veniva rilevata nello stato `Added` e inserita come nuova riga quando veniva eseguita una chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="2672f-350">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="2672f-351">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-351">**New behavior**</span></span>

<span data-ttu-id="2672f-352">A partire da EF Core 3.0, se un'entità usa valori di chiave generati e viene impostato un valore di chiave, l'entità viene rilevata nello stato `Modified`.</span><span class="sxs-lookup"><span data-stu-id="2672f-352">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="2672f-353">Ciò significa che si presuppone l'esistenza di una riga per l'entità che viene aggiornata quando viene eseguita una chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="2672f-353">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="2672f-354">Se il valore di chiave non viene impostato o se il tipo di entità non usa chiavi generate, la nuova entità viene rilevata come `Added` come nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="2672f-354">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="2672f-355">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-355">**Why**</span></span>

<span data-ttu-id="2672f-356">Questa modifica è stata apportata per rendere più semplice e coerente l'uso di grafici di entità disconnesse con chiavi generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="2672f-356">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="2672f-357">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-357">**Mitigations**</span></span>

<span data-ttu-id="2672f-358">Questa modifica può interrompere un'applicazione se un tipo di entità è configurato per l'uso di chiavi generate ma i valori di chiave sono impostati in modo esplicito per le nuove istanze.</span><span class="sxs-lookup"><span data-stu-id="2672f-358">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="2672f-359">La correzione consiste nel configurare in modo esplicito le proprietà di chiave per non usare valori generati.</span><span class="sxs-lookup"><span data-stu-id="2672f-359">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="2672f-360">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="2672f-360">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="2672f-361">Oppure con annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="2672f-361">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="2672f-362">Le eliminazioni a catena vengono ora eseguite immediatamente per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="2672f-362">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="2672f-363">Problema n. 10114</span><span class="sxs-lookup"><span data-stu-id="2672f-363">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="2672f-364">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-364">**Old behavior**</span></span>

<span data-ttu-id="2672f-365">Nelle versioni precedenti alla versione 3.0, EF Core applicava azioni a catena (eliminazione delle entità dipendenti quando veniva eliminata un'entità di sicurezza obbligatoria o veniva recisa la relazione con un'entità di sicurezza obbligatoria) solo dopo la chiamata a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="2672f-365">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="2672f-366">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-366">**New behavior**</span></span>

<span data-ttu-id="2672f-367">A partire dalla versione 3.0, EF Core applica le azioni a catena non appena viene rilevata la condizione di attivazione.</span><span class="sxs-lookup"><span data-stu-id="2672f-367">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="2672f-368">Ad esempio, la chiamata a `context.Remove()` per eliminare un'entità di sicurezza causa anche l'impostazione immediata di tutti i dipendenti obbligatori correlati rilevati su `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="2672f-368">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="2672f-369">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-369">**Why**</span></span>

<span data-ttu-id="2672f-370">Questa modifica è stata apportata per migliorare l'esperienza di associazione di dati e degli scenari di controllo in cui è importante individuare le entità che verranno eliminate _prima_ della chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="2672f-370">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="2672f-371">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-371">**Mitigations**</span></span>

<span data-ttu-id="2672f-372">Il comportamento precedente può essere ripristinato tramite le impostazioni in `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="2672f-372">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="2672f-373">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2672f-373">For example:</span></span>

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="2672f-374">Il caricamento eager delle entità correlate ora si verifica in una singola query</span><span class="sxs-lookup"><span data-stu-id="2672f-374">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="2672f-375">Rilevamento del problema #18022</span><span class="sxs-lookup"><span data-stu-id="2672f-375">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="2672f-376">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-376">**Old behavior**</span></span>

<span data-ttu-id="2672f-377">Prima del 3,0, il caricamento con entusiasmo delle esplorazioni della raccolta tramite operatori `Include` causava la generazione di più query nel database relazionale, una per ogni tipo di entità correlato.</span><span class="sxs-lookup"><span data-stu-id="2672f-377">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="2672f-378">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-378">**New behavior**</span></span>

<span data-ttu-id="2672f-379">A partire da 3,0, EF Core genera una singola query con JOIN nei database relazionali.</span><span class="sxs-lookup"><span data-stu-id="2672f-379">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="2672f-380">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-380">**Why**</span></span>

<span data-ttu-id="2672f-381">L'invio di più query per l'implementazione di una singola query LINQ ha causato numerosi problemi, incluse le prestazioni negative in quanto sono stati necessari più round trip di database e i dati coerenza problemi poiché ogni query poteva osservare uno stato diverso del database.</span><span class="sxs-lookup"><span data-stu-id="2672f-381">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="2672f-382">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-382">**Mitigations**</span></span>

<span data-ttu-id="2672f-383">Sebbene tecnicamente non si tratti di una modifica di rilievo, potrebbe avere un impatto significativo sulle prestazioni dell'applicazione quando una singola query contiene un numero elevato di `Include` operatore per le esplorazioni della raccolta.</span><span class="sxs-lookup"><span data-stu-id="2672f-383">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="2672f-384">Per ulteriori informazioni e per riscrivere le query in modo più efficiente, [vedere il commento](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) .</span><span class="sxs-lookup"><span data-stu-id="2672f-384">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="2672f-385">Semantica più chiara per DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="2672f-385">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="2672f-386">Problema n. 12661</span><span class="sxs-lookup"><span data-stu-id="2672f-386">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="2672f-387">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-387">**Old behavior**</span></span>

<span data-ttu-id="2672f-388">Prima della versione 3.0, `DeleteBehavior.Restrict` creava chiavi esterne nel database con la semantica `Restrict`, ma modificava anche la correzione interna in modo non ovvio.</span><span class="sxs-lookup"><span data-stu-id="2672f-388">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="2672f-389">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-389">**New behavior**</span></span>

<span data-ttu-id="2672f-390">A partire dalla versione 3.0, `DeleteBehavior.Restrict` assicura che le chiavi esterne vengano create con la semantica `Restrict`, ovvero non a cascata e con generazione di un'eccezione in caso di violazione di vincolo, senza influire sulla correzione interna di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2672f-390">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="2672f-391">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-391">**Why**</span></span>

<span data-ttu-id="2672f-392">Questa modifica è stata apportata per migliorare l'esperienza di uso di `DeleteBehavior` in modo intuitivo, senza effetti collaterali imprevisti.</span><span class="sxs-lookup"><span data-stu-id="2672f-392">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="2672f-393">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-393">**Mitigations**</span></span>

<span data-ttu-id="2672f-394">Il comportamento precedente può essere ripristinato tramite `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="2672f-394">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="2672f-395">I tipi di query vengono consolidati con tipi di entità</span><span class="sxs-lookup"><span data-stu-id="2672f-395">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="2672f-396">Problema n. 14194</span><span class="sxs-lookup"><span data-stu-id="2672f-396">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="2672f-397">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-397">**Old behavior**</span></span>

<span data-ttu-id="2672f-398">Nelle versioni precedenti a EF Core 3.0 i [tipi di query](xref:core/modeling/keyless-entity-types) erano uno strumento per eseguire query su dati che non definiscono una chiave primaria in modo strutturato.</span><span class="sxs-lookup"><span data-stu-id="2672f-398">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="2672f-399">Veniva infatti usato un tipo di query per eseguire il mapping di tipi di entità senza chiavi (più probabilmente da una vista, ma anche da una tabella), mentre veniva usato un tipo di entità normale quando era disponibile una chiave (più probabilmente da una tabella, ma anche da una vista).</span><span class="sxs-lookup"><span data-stu-id="2672f-399">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="2672f-400">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-400">**New behavior**</span></span>

<span data-ttu-id="2672f-401">Un tipo di query diventa ora semplicemente un tipo di entità senza chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="2672f-401">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="2672f-402">I tipi di entità senza chiave hanno la stessa funzionalità dei tipi di query nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="2672f-402">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="2672f-403">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-403">**Why**</span></span>

<span data-ttu-id="2672f-404">Questa modifica è stata apportata per ridurre la confusione riguardo lo scopo dei tipi di query.</span><span class="sxs-lookup"><span data-stu-id="2672f-404">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="2672f-405">In particolare, si tratta di tipi di entità senza chiave che sono intrinsecamente di sola lettura per questo motivo ma non dovrebbero essere usati solo perché un tipo di entità deve essere di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="2672f-405">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="2672f-406">Analogamente, spesso vengono mappati alle viste solo perché le viste spesso non definiscono le chiavi.</span><span class="sxs-lookup"><span data-stu-id="2672f-406">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="2672f-407">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-407">**Mitigations**</span></span>

<span data-ttu-id="2672f-408">Le parti dell'API seguenti sono ora obsolete:</span><span class="sxs-lookup"><span data-stu-id="2672f-408">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="2672f-409">**`ModelBuilder.Query<>()`** - È necessario chiamare `ModelBuilder.Entity<>().HasNoKey()` per contrassegnare un tipo di entità come tipo senza chiavi.</span><span class="sxs-lookup"><span data-stu-id="2672f-409">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="2672f-410">Non ne viene eseguita la configurazione per convenzione per evitare una configurazione errata quando è prevista una chiave primaria che tuttavia non corrisponde alla convenzione.</span><span class="sxs-lookup"><span data-stu-id="2672f-410">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="2672f-411">**`DbQuery<>`** - Usare `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="2672f-411">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="2672f-412">**`DbContext.Query<>()`** - Usare `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="2672f-412">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="2672f-413">L'API di configurazione per le relazioni di tipo di proprietà è stata modificata</span><span class="sxs-lookup"><span data-stu-id="2672f-413">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="2672f-414">[Problema n. 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema n. 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema n. 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="2672f-414">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="2672f-415">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-415">**Old behavior**</span></span>

<span data-ttu-id="2672f-416">Nelle versioni precedenti a EF Core 3.0 la configurazione della relazione di proprietà veniva eseguita direttamente dopo la chiamata a `OwnsOne` o `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="2672f-416">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="2672f-417">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-417">**New behavior**</span></span>

<span data-ttu-id="2672f-418">A partire da EF Core 3.0, è disponibile l'API Fluent per configurare una proprietà di navigazione per il proprietario usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="2672f-418">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="2672f-419">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2672f-419">For example:</span></span>

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="2672f-420">La configurazione correlata alla relazione tra proprietario e elemento di proprietà deve essere ora concatenata dopo `WithOwner()` in modo analogo a come vengono configurate altre relazioni.</span><span class="sxs-lookup"><span data-stu-id="2672f-420">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="2672f-421">La configurazione per il tipo di proprietà viene comunque concatenata dopo `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="2672f-421">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="2672f-422">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2672f-422">For example:</span></span>

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

<span data-ttu-id="2672f-423">Inoltre, la chiamata a `Entity()`, `HasOne()` o `Set()` con una destinazione di tipo di proprietà genera ora un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="2672f-423">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="2672f-424">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-424">**Why**</span></span>

<span data-ttu-id="2672f-425">Questa modifica è stata apportata per creare una separazione più netta tra la configurazione del tipo di proprietà e la _relazione_ con il tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="2672f-425">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="2672f-426">Ciò consente di eliminare ambiguità e confusione su metodi come `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="2672f-426">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="2672f-427">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-427">**Mitigations**</span></span>

<span data-ttu-id="2672f-428">Modificare la configurazione delle relazioni dei tipi di proprietà per usare la superficie della nuova API come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="2672f-428">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="2672f-429">Le entità dipendenti che condividono la tabella con l'entità di sicurezza sono ora facoltative</span><span class="sxs-lookup"><span data-stu-id="2672f-429">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="2672f-430">Problema n. 9005</span><span class="sxs-lookup"><span data-stu-id="2672f-430">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="2672f-431">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-431">**Old behavior**</span></span>

<span data-ttu-id="2672f-432">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="2672f-432">Consider the following model:</span></span>
```csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```
<span data-ttu-id="2672f-433">Prima di EF Core 3.0, se `OrderDetails` è di proprietà di `Order` o viene mappato in modo esplicito alla stessa tabella, era sempre necessaria un'istanza di `OrderDetails` per l'aggiunta di un nuovo `Order`.</span><span class="sxs-lookup"><span data-stu-id="2672f-433">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="2672f-434">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-434">**New behavior**</span></span>

<span data-ttu-id="2672f-435">A partire dalla versione 3.0, EF Core consente di aggiungere un `Order` senza un `OrderDetails` ed esegue il mapping di tutte le proprietà di `OrderDetails`, tranne che la chiave primaria, a colonne che ammettono valori Null.</span><span class="sxs-lookup"><span data-stu-id="2672f-435">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="2672f-436">In fase di query, EF Core imposta `OrderDetails` su `null` se una delle relative proprietà obbligatorie non ha un valore o se non sono presenti proprietà obbligatorie oltre alla chiave primaria e tutte le proprietà sono `null`.</span><span class="sxs-lookup"><span data-stu-id="2672f-436">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="2672f-437">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-437">**Mitigations**</span></span>

<span data-ttu-id="2672f-438">Se il modello include una tabella condivisa dipendente con tutte le colonne facoltative, ma è previsto che la navigazione che punta a essa non sia `null`, l'applicazione deve essere modificata per gestire casi in cui la navigazione è `null`.</span><span class="sxs-lookup"><span data-stu-id="2672f-438">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="2672f-439">Se questo non è possibile, è consigliabile aggiungere una proprietà obbligatoria al tipo di entità o assegnare un valore non `null` ad almeno una proprietà.</span><span class="sxs-lookup"><span data-stu-id="2672f-439">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="2672f-440">Tutte le entità che condividono una tabella con una colonna di token di concorrenza devono eseguirne il mapping a una proprietà</span><span class="sxs-lookup"><span data-stu-id="2672f-440">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="2672f-441">Problema n. 14154</span><span class="sxs-lookup"><span data-stu-id="2672f-441">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="2672f-442">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-442">**Old behavior**</span></span>

<span data-ttu-id="2672f-443">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="2672f-443">Consider the following model:</span></span>
```csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
<span data-ttu-id="2672f-444">Prima di EF Core 3.0, se `OrderDetails` è di proprietà di `Order` o è mappato in modo esplicito alla stessa tabella, il solo aggiornamento di `OrderDetails` non aggiornerà il valore `Version` nel client e l'aggiornamento successivo avrà esito negativo.</span><span class="sxs-lookup"><span data-stu-id="2672f-444">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="2672f-445">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-445">**New behavior**</span></span>

<span data-ttu-id="2672f-446">A partire dalla versione 3.0, EF Core propaga il nuovo valore `Version` a `Order` se è proprietario di `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="2672f-446">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="2672f-447">In caso contrario, viene generata un'eccezione durante la convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="2672f-447">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="2672f-448">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-448">**Why**</span></span>

<span data-ttu-id="2672f-449">Questa modifica è stata apportata per evitare un valore del token di concorrenza non aggiornato quando viene aggiornata solo una delle entità mappate alla stessa tabella.</span><span class="sxs-lookup"><span data-stu-id="2672f-449">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="2672f-450">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-450">**Mitigations**</span></span>

<span data-ttu-id="2672f-451">Tutte le entità che condividono la tabella devono includere una proprietà mappata alla colonna del token di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="2672f-451">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="2672f-452">È possibile crearne una in stato shadow:</span><span class="sxs-lookup"><span data-stu-id="2672f-452">It's possible the create one in shadow-state:</span></span>
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a><span data-ttu-id="2672f-453">Non è possibile eseguire query sulle entità di proprietà senza il proprietario utilizzando una query di rilevamento</span><span class="sxs-lookup"><span data-stu-id="2672f-453">Owned entities cannot be queried without the owner using a tracking query</span></span>

[<span data-ttu-id="2672f-454">Rilevamento del problema #18876</span><span class="sxs-lookup"><span data-stu-id="2672f-454">Tracking Issue #18876</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

<span data-ttu-id="2672f-455">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-455">**Old behavior**</span></span>

<span data-ttu-id="2672f-456">Prima di EF Core 3,0, le entità di proprietà potevano essere sottoposte a query come qualsiasi altra navigazione.</span><span class="sxs-lookup"><span data-stu-id="2672f-456">Before EF Core 3.0, the owned entities could be queried as any other navigation.</span></span>

```csharp
context.People.Select(p => p.Address);
```

<span data-ttu-id="2672f-457">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-457">**New behavior**</span></span>

<span data-ttu-id="2672f-458">A partire da 3,0, EF Core genererà un'operazione se una query di rilevamento proietta un'entità di proprietà senza proprietario.</span><span class="sxs-lookup"><span data-stu-id="2672f-458">Starting with 3.0, EF Core will throw if a tracking query projects an owned entity without the owner.</span></span>

<span data-ttu-id="2672f-459">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-459">**Why**</span></span>

<span data-ttu-id="2672f-460">Le entità di proprietà non possono essere modificate senza il proprietario, quindi nella maggior parte dei casi l'esecuzione di query in questo modo è un errore.</span><span class="sxs-lookup"><span data-stu-id="2672f-460">Owned entities cannot be manipulated without the owner, so in the vast majority of cases querying them in this way is an error.</span></span>

<span data-ttu-id="2672f-461">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-461">**Mitigations**</span></span>

<span data-ttu-id="2672f-462">Se l'entità di proprietà deve essere rilevata in modo da essere modificata in un secondo momento, il proprietario deve essere incluso nella query.</span><span class="sxs-lookup"><span data-stu-id="2672f-462">If the owned entity should be tracked to be modified in any way later then the owner should be included in the query.</span></span>

<span data-ttu-id="2672f-463">In caso contrario, aggiungere una chiamata `AsNoTracking()`:</span><span class="sxs-lookup"><span data-stu-id="2672f-463">Otherwise add an `AsNoTracking()` call:</span></span>

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="2672f-464">Per le proprietà ereditate da tipi senza mapping viene ora eseguito il mapping a una singola colonna per tutti i tipi derivati</span><span class="sxs-lookup"><span data-stu-id="2672f-464">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="2672f-465">Problema n. 13998</span><span class="sxs-lookup"><span data-stu-id="2672f-465">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="2672f-466">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-466">**Old behavior**</span></span>

<span data-ttu-id="2672f-467">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="2672f-467">Consider the following model:</span></span>
```csharp
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

<span data-ttu-id="2672f-468">Prima di EF Core 3.0, per la proprietà `ShippingAddress` sarebbe stato eseguito il mapping a colonne separate per `BulkOrder` e `Order` per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="2672f-468">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="2672f-469">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-469">**New behavior**</span></span>

<span data-ttu-id="2672f-470">A partire dalla versione 3.0, EF Core crea solo una colonna per `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="2672f-470">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="2672f-471">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-471">**Why**</span></span>

<span data-ttu-id="2672f-472">Il comportamento precedente non era previsto.</span><span class="sxs-lookup"><span data-stu-id="2672f-472">The old behavoir was unexpected.</span></span>

<span data-ttu-id="2672f-473">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-473">**Mitigations**</span></span>

<span data-ttu-id="2672f-474">È ancora possibile eseguire il mapping esplicito della proprietà a una colonna separata per i tipi derivati:</span><span class="sxs-lookup"><span data-stu-id="2672f-474">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

<a name="fkp"></a>

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="2672f-475">La convenzione di proprietà di chiave esterna non ha più lo stesso nome della proprietà dell'entità di sicurezza</span><span class="sxs-lookup"><span data-stu-id="2672f-475">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="2672f-476">Problema n. 13274</span><span class="sxs-lookup"><span data-stu-id="2672f-476">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="2672f-477">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-477">**Old behavior**</span></span>

<span data-ttu-id="2672f-478">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="2672f-478">Consider the following model:</span></span>
```csharp
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```
<span data-ttu-id="2672f-479">Nelle versioni precedenti a EF Core 3.0 veniva usata la proprietà `CustomerId` per la chiave esterna per convenzione.</span><span class="sxs-lookup"><span data-stu-id="2672f-479">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="2672f-480">Tuttavia, se `Order` è un tipo di proprietà, `CustomerId` sarebbe la chiave primaria e ciò non è in genere auspicabile.</span><span class="sxs-lookup"><span data-stu-id="2672f-480">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="2672f-481">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-481">**New behavior**</span></span>

<span data-ttu-id="2672f-482">A partire da 3.0, EF Core non tenta di usare le proprietà per le chiavi esterne per convenzione se hanno lo stesso nome della proprietà dell'entità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="2672f-482">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="2672f-483">Viene ancora eseguita la corrispondenza tra i criteri del nome del tipo dell'entità di sicurezza concatenato al nome della proprietà dell'entità di sicurezza e il nome di navigazione concatenato al nome della proprietà dell'entità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="2672f-483">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="2672f-484">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2672f-484">For example:</span></span>

```csharp
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```csharp
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

<span data-ttu-id="2672f-485">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-485">**Why**</span></span>

<span data-ttu-id="2672f-486">Questa modifica è stata apportata per evitare una definizione errata della proprietà di chiave primaria nel tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="2672f-486">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="2672f-487">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-487">**Mitigations**</span></span>

<span data-ttu-id="2672f-488">Se la proprietà è stata progettata per essere la chiave esterna e di conseguenza parte della chiave primaria, configurarla in modo esplicito come chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="2672f-488">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="2672f-489">La connessione al database viene ora chiusa se non viene più usata prima del completamento di TransactionScope</span><span class="sxs-lookup"><span data-stu-id="2672f-489">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="2672f-490">Problema n. 14218</span><span class="sxs-lookup"><span data-stu-id="2672f-490">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="2672f-491">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-491">**Old behavior**</span></span>

<span data-ttu-id="2672f-492">Prima di EF Core 3.0, se il contesto apre la connessione all'interno di un `TransactionScope`, la connessione rimane aperta mentre è attivo il `TransactionScope` corrente.</span><span class="sxs-lookup"><span data-stu-id="2672f-492">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```csharp
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point

        var categories = context.ProductCategories().ToList();
    }
}
```

<span data-ttu-id="2672f-493">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-493">**New behavior**</span></span>

<span data-ttu-id="2672f-494">A partire dalla versione 3.0, EF Core chiude la connessione non appena non viene più usata.</span><span class="sxs-lookup"><span data-stu-id="2672f-494">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="2672f-495">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-495">**Why**</span></span>

<span data-ttu-id="2672f-496">Questa modifica consente di usare più contesti nello stesso `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="2672f-496">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="2672f-497">Il nuovo comportamento corrisponde anche a EF6.</span><span class="sxs-lookup"><span data-stu-id="2672f-497">The new behavior also matches EF6.</span></span>

<span data-ttu-id="2672f-498">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-498">**Mitigations**</span></span>

<span data-ttu-id="2672f-499">Se la connessione deve rimanere aperta, la chiamata esplicita di `OpenConnection()` garantirà che EF Core non la chiuda prematuramente:</span><span class="sxs-lookup"><span data-stu-id="2672f-499">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```csharp
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

<a name="each"></a>

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="2672f-500">Ogni proprietà usa la generazione di chiavi di tipo intero in memoria indipendenti</span><span class="sxs-lookup"><span data-stu-id="2672f-500">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="2672f-501">Problema n. 6872</span><span class="sxs-lookup"><span data-stu-id="2672f-501">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="2672f-502">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-502">**Old behavior**</span></span>

<span data-ttu-id="2672f-503">Nelle versioni precedenti a EF Core 3.0 veniva usato un unico generatore di valori condiviso per tutte le proprietà di chiavi di tipo intero in memoria.</span><span class="sxs-lookup"><span data-stu-id="2672f-503">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="2672f-504">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-504">**New behavior**</span></span>

<span data-ttu-id="2672f-505">A partire da EF Core 3.0, ogni proprietà di chiave di tipo intero riceve un generatore di valori quando viene usato il database in memoria.</span><span class="sxs-lookup"><span data-stu-id="2672f-505">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="2672f-506">Inoltre, se il database viene eliminato, la generazione di chiavi viene reimpostata per tutte le tabelle.</span><span class="sxs-lookup"><span data-stu-id="2672f-506">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="2672f-507">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-507">**Why**</span></span>

<span data-ttu-id="2672f-508">Questa modifica è stata apportata per allineare maggiormente la generazione di chiavi in memoria alla generazione di chiavi del database reale e per migliorare la possibilità di isolare i test l'uno dall'altro quando viene usato il database in memoria.</span><span class="sxs-lookup"><span data-stu-id="2672f-508">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="2672f-509">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-509">**Mitigations**</span></span>

<span data-ttu-id="2672f-510">Ciò può interrompere un'applicazione che si basa sull'impostazione di valori di chiave in memoria specifici.</span><span class="sxs-lookup"><span data-stu-id="2672f-510">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="2672f-511">È consigliabile non basare l'applicazione su valori di chiave specifici o eseguire l'aggiornamento per passare al nuovo comportamento.</span><span class="sxs-lookup"><span data-stu-id="2672f-511">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="2672f-512">I campi sottostanti vengono usati per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="2672f-512">Backing fields are used by default</span></span>

[<span data-ttu-id="2672f-513">Problema n. 12430</span><span class="sxs-lookup"><span data-stu-id="2672f-513">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="2672f-514">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-514">**Old behavior**</span></span>

<span data-ttu-id="2672f-515">Nelle versioni precedenti alla versione 3.0, anche se il campo sottostante di una proprietà era noto, per impostazione predefinita EF Core eseguiva la lettura e la scrittura del valore della proprietà usando i metodi getter e setter della proprietà.</span><span class="sxs-lookup"><span data-stu-id="2672f-515">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="2672f-516">L'eccezione era costituita dall'esecuzione di query in cui il campo sottostante, se noto, veniva impostato direttamente.</span><span class="sxs-lookup"><span data-stu-id="2672f-516">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="2672f-517">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-517">**New behavior**</span></span>

<span data-ttu-id="2672f-518">A partire da EF Core 3.0, se il campo sottostante di una proprietà è noto, la lettura e la scrittura della proprietà vengono sempre eseguite usando il campo sottostante.</span><span class="sxs-lookup"><span data-stu-id="2672f-518">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="2672f-519">Ciò potrebbe causare un'interruzione dell'applicazione se l'applicazione si basa su un comportamento aggiuntivo codificato nei metodi getter o setter.</span><span class="sxs-lookup"><span data-stu-id="2672f-519">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="2672f-520">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-520">**Why**</span></span>

<span data-ttu-id="2672f-521">Questa modifica è stata apportata per impedire a EF Core di attivare per errore la logica di business per impostazione predefinita quando si eseguono operazioni di database che interessano le entità.</span><span class="sxs-lookup"><span data-stu-id="2672f-521">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="2672f-522">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-522">**Mitigations**</span></span>

<span data-ttu-id="2672f-523">È possibile ripristinare il comportamento delle versioni precedenti alla versione 3.0 tramite la configurazione della modalità di accesso delle proprietà in `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2672f-523">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="2672f-524">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2672f-524">For example:</span></span>

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="2672f-525">Viene generata un'eccezione se vengono trovati più campi sottostanti compatibili</span><span class="sxs-lookup"><span data-stu-id="2672f-525">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="2672f-526">Problema n. 12523</span><span class="sxs-lookup"><span data-stu-id="2672f-526">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="2672f-527">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-527">**Old behavior**</span></span>

<span data-ttu-id="2672f-528">Nelle versioni precedenti a EF Core 3.0, se più campi soddisfacevano le regole di ricerca del campo sottostante di una proprietà, veniva selezionato un solo campo in base a un ordine di precedenza.</span><span class="sxs-lookup"><span data-stu-id="2672f-528">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="2672f-529">Ciò poteva causare l'uso di un campo non corretto nei casi ambigui.</span><span class="sxs-lookup"><span data-stu-id="2672f-529">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="2672f-530">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-530">**New behavior**</span></span>

<span data-ttu-id="2672f-531">A partire da EF Core 3.0, se più campi corrispondono alla stessa proprietà, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="2672f-531">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="2672f-532">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-532">**Why**</span></span>

<span data-ttu-id="2672f-533">Questa modifica è stata apportata per evitare di usare automaticamente un campo rispetto a un altro quando un solo campo può essere quello corretto.</span><span class="sxs-lookup"><span data-stu-id="2672f-533">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="2672f-534">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-534">**Mitigations**</span></span>

<span data-ttu-id="2672f-535">Per le proprietà con campi sottostanti ambigui, il campo da usare deve essere specificato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="2672f-535">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="2672f-536">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="2672f-536">For example, using the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="2672f-537">I nomi delle proprietà solo campo devono corrispondere al nome di campo</span><span class="sxs-lookup"><span data-stu-id="2672f-537">Field-only property names should match the field name</span></span>

<span data-ttu-id="2672f-538">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-538">**Old behavior**</span></span>

<span data-ttu-id="2672f-539">Prima di EF Core 3,0, una proprietà può essere specificata da un valore stringa e, se non è stata trovata alcuna proprietà con tale nome nel tipo .NET, EF Core tenterà di associarla a un campo usando le regole di convenzione.</span><span class="sxs-lookup"><span data-stu-id="2672f-539">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>

```csharp
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="2672f-540">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-540">**New behavior**</span></span>

<span data-ttu-id="2672f-541">A partire da EF Core 3.0, una proprietà solo campo deve corrispondere esattamente al nome del campo.</span><span class="sxs-lookup"><span data-stu-id="2672f-541">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="2672f-542">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-542">**Why**</span></span>

<span data-ttu-id="2672f-543">Questa modifica è stata introdotta per evitare di usare lo stesso campo per due proprietà con nome simile. Le regole di corrispondenza per le proprietà solo campo sono state anche uniformate a quelle per le proprietà mappate a proprietà CLR.</span><span class="sxs-lookup"><span data-stu-id="2672f-543">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="2672f-544">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-544">**Mitigations**</span></span>

<span data-ttu-id="2672f-545">Le proprietà solo campo devono avere lo stesso nome del campo a cui vengono mappate.</span><span class="sxs-lookup"><span data-stu-id="2672f-545">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="2672f-546">In una versione futura di EF Core dopo il 3,0, si prevede di abilitare di nuovo in modo esplicito il nome di un campo diverso dal nome della proprietà (vedere il problema [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="2672f-546">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="2672f-547">AddDbContext/AddDbContextPool non chiamano più AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="2672f-547">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="2672f-548">Problema n. 14756</span><span class="sxs-lookup"><span data-stu-id="2672f-548">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="2672f-549">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-549">**Old behavior**</span></span>

<span data-ttu-id="2672f-550">Prima DI EF Core 3,0, la chiamata `AddDbContext` o `AddDbContextPool` registrerà anche i servizi di memorizzazione nella cache di registrazione e di memoria con le chiamate a [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="2672f-550">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with DI through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="2672f-551">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-551">**New behavior**</span></span>

<span data-ttu-id="2672f-552">A partire da EF Core 3.0, `AddDbContext` e `AddDbContextPool` non registreranno più questi servizi con inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="2672f-552">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="2672f-553">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-553">**Why**</span></span>

<span data-ttu-id="2672f-554">EF Core 3.0 non richiede che questi servizi siano inclusi nel contenitore di inserimento delle dipendenze dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2672f-554">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="2672f-555">Tuttavia, se `ILoggerFactory` è registrato nel contenitore di inserimento delle dipendenze dell'applicazione, verrà ancora usato da EF Core.</span><span class="sxs-lookup"><span data-stu-id="2672f-555">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="2672f-556">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-556">**Mitigations**</span></span>

<span data-ttu-id="2672f-557">Se l'applicazione necessita di questi servizi, registrarli in modo esplicito con il contenitore di inserimento delle dipendenze usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="2672f-557">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a><span data-ttu-id="2672f-558">AddEntityFramework \* aggiunge IMemoryCache con un limite di dimensioni</span><span class="sxs-lookup"><span data-stu-id="2672f-558">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>

[<span data-ttu-id="2672f-559">Rilevamento del problema #12905</span><span class="sxs-lookup"><span data-stu-id="2672f-559">Tracking Issue #12905</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

<span data-ttu-id="2672f-560">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-560">**Old behavior**</span></span>

<span data-ttu-id="2672f-561">Prima di EF Core 3,0, la chiamata ai metodi DI `AddEntityFramework*` registrava anche i servizi DI memorizzazione nella cache DI memoria con DI DI senza un limite DI dimensioni.</span><span class="sxs-lookup"><span data-stu-id="2672f-561">Before EF Core 3.0, calling `AddEntityFramework*` methods would also register memory caching services with DI without a size limit.</span></span>

<span data-ttu-id="2672f-562">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-562">**New behavior**</span></span>

<span data-ttu-id="2672f-563">A partire da EF Core 3,0, `AddEntityFramework*` registrerà un servizio IMemoryCache con un limite di dimensioni.</span><span class="sxs-lookup"><span data-stu-id="2672f-563">Starting with EF Core 3.0, `AddEntityFramework*` will register an IMemoryCache service with a size limit.</span></span> <span data-ttu-id="2672f-564">Se altri servizi aggiunti successivamente dipendono da IMemoryCache, è possibile raggiungere rapidamente il limite predefinito che causa eccezioni o prestazioni ridotte.</span><span class="sxs-lookup"><span data-stu-id="2672f-564">If any other services added afterwards depend on IMemoryCache they can quickly reach the default limit causing exceptions or degraded performance.</span></span>

<span data-ttu-id="2672f-565">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-565">**Why**</span></span>

<span data-ttu-id="2672f-566">L'utilizzo di IMemoryCache senza un limite può comportare l'utilizzo di memoria non controllata se è presente un bug nella logica di memorizzazione nella cache delle query o se le query vengono generate in modo dinamico.</span><span class="sxs-lookup"><span data-stu-id="2672f-566">Using IMemoryCache without a limit could result in uncontrolled memory usage if there is a bug in query caching logic or the queries are generated dynamically.</span></span> <span data-ttu-id="2672f-567">La presenza di un limite predefinito attenua un potenziale attacco DoS.</span><span class="sxs-lookup"><span data-stu-id="2672f-567">Having a default limit mitigates a potential DoS attack.</span></span>

<span data-ttu-id="2672f-568">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-568">**Mitigations**</span></span>

<span data-ttu-id="2672f-569">Nella maggior parte dei casi, non è necessario chiamare `AddEntityFramework*` se viene chiamato anche `AddDbContext` o `AddDbContextPool`.</span><span class="sxs-lookup"><span data-stu-id="2672f-569">In most cases calling `AddEntityFramework*` is not necessary if `AddDbContext` or `AddDbContextPool` is called as well.</span></span> <span data-ttu-id="2672f-570">Pertanto, la migliore mitigazione consiste nel rimuovere la chiamata `AddEntityFramework*`.</span><span class="sxs-lookup"><span data-stu-id="2672f-570">Therefore, the best mitigation is to remove the `AddEntityFramework*` call.</span></span>

<span data-ttu-id="2672f-571">Se l'applicazione necessita di questi servizi, registrare un'implementazione di IMemoryCache in modo esplicito con il contenitore DI inserimento in anticipo usando [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="2672f-571">If your application needs these services, then register a IMemoryCache implementation explicitly with the DI container beforehand using [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="2672f-572">DbContext.Entry esegue ora un DetectChanges locale</span><span class="sxs-lookup"><span data-stu-id="2672f-572">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="2672f-573">Problema n. 13552</span><span class="sxs-lookup"><span data-stu-id="2672f-573">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="2672f-574">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-574">**Old behavior**</span></span>

<span data-ttu-id="2672f-575">Nelle versioni precedenti a EF Core 3.0 la chiamata a `DbContext.Entry` causava il rilevamento delle modifiche per tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="2672f-575">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="2672f-576">Ciò garantiva l'aggiornamento dello stato esposto in `EntityEntry`.</span><span class="sxs-lookup"><span data-stu-id="2672f-576">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="2672f-577">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-577">**New behavior**</span></span>

<span data-ttu-id="2672f-578">A partire da EF Core 3.0, la chiamata a `DbContext.Entry` causa ora solo il tentativo di rilevare le modifiche nell'entità specificata e in tutte le relative entità di sicurezza rilevate.</span><span class="sxs-lookup"><span data-stu-id="2672f-578">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="2672f-579">Ciò significa che le modifiche apportate altrove potrebbero non essere state rilevate tramite la chiamata al metodo e ciò potrebbe avere implicazioni sullo stato dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="2672f-579">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="2672f-580">Si noti che se `ChangeTracker.AutoDetectChangesEnabled` è impostato su `false`, verrà disabilitato anche questo tipo di rilevamento delle modifiche locali.</span><span class="sxs-lookup"><span data-stu-id="2672f-580">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="2672f-581">Gli altri metodi che causano il rilevamento delle modifiche, ad esempio `ChangeTracker.Entries` e `SaveChanges`, causano ancora un `DetectChanges` completo di tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="2672f-581">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="2672f-582">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-582">**Why**</span></span>

<span data-ttu-id="2672f-583">Questa modifica è stata apportata per migliorare le prestazioni predefinite dell'uso di `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="2672f-583">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="2672f-584">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-584">**Mitigations**</span></span>

<span data-ttu-id="2672f-585">Chiamare `ChgangeTracker.DetectChanges()` in modo esplicito prima di chiamare `Entry` per garantire il comportamento precedente alla versione 3.0.</span><span class="sxs-lookup"><span data-stu-id="2672f-585">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="2672f-586">Le chiavi matrice di byte e di stringhe non vengono generate dal client per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="2672f-586">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="2672f-587">Problema n. 14617</span><span class="sxs-lookup"><span data-stu-id="2672f-587">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="2672f-588">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-588">**Old behavior**</span></span>

<span data-ttu-id="2672f-589">Nelle versioni precedenti a EF Core 3.0 le proprietà di chiave `string` e `byte[]` potevano essere usate senza impostare in modo esplicito un valore non Null.</span><span class="sxs-lookup"><span data-stu-id="2672f-589">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="2672f-590">In questi casi, il valore di chiave veniva generato nel client come GUID, serializzato in byte per `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="2672f-590">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="2672f-591">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-591">**New behavior**</span></span>

<span data-ttu-id="2672f-592">A partire da EF Core 3.0 viene generata un'eccezione che indica che non è stato impostato alcun valore di chiave.</span><span class="sxs-lookup"><span data-stu-id="2672f-592">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="2672f-593">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-593">**Why**</span></span>

<span data-ttu-id="2672f-594">Questa modifica è stata apportata poiché i valori `string`/`byte[]` generati dal client non risultano in genere utili e il comportamento predefinito rendeva complesso ragionare sui valori di chiave generati in un modo comune.</span><span class="sxs-lookup"><span data-stu-id="2672f-594">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="2672f-595">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-595">**Mitigations**</span></span>

<span data-ttu-id="2672f-596">È possibile ripristinare il comportamento precedente alla versione 3.0 specificando in modo esplicito che le proprietà di chiave devono usare i valori generati se non viene impostato alcun altro valore non Null.</span><span class="sxs-lookup"><span data-stu-id="2672f-596">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="2672f-597">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="2672f-597">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="2672f-598">Oppure con annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="2672f-598">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="2672f-599">ILoggerFactory è ora un servizio con ambito</span><span class="sxs-lookup"><span data-stu-id="2672f-599">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="2672f-600">Problema n. 14698</span><span class="sxs-lookup"><span data-stu-id="2672f-600">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="2672f-601">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-601">**Old behavior**</span></span>

<span data-ttu-id="2672f-602">Nelle versioni precedenti a EF Core 3.0 `ILoggerFactory` veniva registrato come servizio singleton.</span><span class="sxs-lookup"><span data-stu-id="2672f-602">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="2672f-603">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-603">**New behavior**</span></span>

<span data-ttu-id="2672f-604">A partire da EF Core 3.0, `ILoggerFactory` viene registrato come servizio con ambito.</span><span class="sxs-lookup"><span data-stu-id="2672f-604">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="2672f-605">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-605">**Why**</span></span>

<span data-ttu-id="2672f-606">Questa modifica è stata apportata per consentire l'associazione di un logger a un'istanza `DbContext` che abilita altre funzionalità e rimuove alcuni casi di comportamento anomalo, ad esempio un'esplosione dei provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="2672f-606">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="2672f-607">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-607">**Mitigations**</span></span>

<span data-ttu-id="2672f-608">Questa modifica non dovrebbe influire sul codice dell'applicazione a meno che non vengano registrati e usati servizi personalizzati nel provider di servizi interno di EF Core.</span><span class="sxs-lookup"><span data-stu-id="2672f-608">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="2672f-609">Questa condizione, tuttavia, non è comune.</span><span class="sxs-lookup"><span data-stu-id="2672f-609">This isn't common.</span></span>
<span data-ttu-id="2672f-610">In questi casi la maggior parte delle operazioni continuano a essere eseguite correttamente, ma qualsiasi servizio singleton dipendente da `ILoggerFactory` dovrà essere modificato per ottenere `ILoggerFactory` in modo diverso.</span><span class="sxs-lookup"><span data-stu-id="2672f-610">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="2672f-611">Se si verificano situazioni simili, inviare una segnalazione nello [strumento di gestione dei problemi in GitHub relativo a EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) per comunicare in che modo viene usato `ILoggerFactory` per consentirci di comprendere meglio come evitare ulteriori interruzioni in futuro.</span><span class="sxs-lookup"><span data-stu-id="2672f-611">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="2672f-612">I proxy di caricamento lazy non presuppongono più che le proprietà di navigazione vengano caricate completamente</span><span class="sxs-lookup"><span data-stu-id="2672f-612">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="2672f-613">Problema n. 12780</span><span class="sxs-lookup"><span data-stu-id="2672f-613">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="2672f-614">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-614">**Old behavior**</span></span>

<span data-ttu-id="2672f-615">Nelle versioni precedenti a EF Core 3.0, quando `DbContext` veniva eliminato non esisteva alcun metodo per scoprire se una determinata proprietà di navigazione in un'entità ottenuta da un contesto specifico veniva caricata completamente o meno.</span><span class="sxs-lookup"><span data-stu-id="2672f-615">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="2672f-616">I proxy presupponevano invece che venisse caricata una navigazione di riferimento se era presente un valore non Null e che venisse caricata una navigazione di raccolta se era presente un valore.</span><span class="sxs-lookup"><span data-stu-id="2672f-616">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="2672f-617">In questi casi il tentativo di eseguire un caricamento lazy non avrebbe avuto alcun esito.</span><span class="sxs-lookup"><span data-stu-id="2672f-617">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="2672f-618">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-618">**New behavior**</span></span>

<span data-ttu-id="2672f-619">A partire da Entity Framework Core 3.0, i proxy tengono traccia del caricamento o mancato caricamento di una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="2672f-619">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="2672f-620">Ciò significa che il tentativo di accedere a una proprietà di navigazione caricata dopo l'eliminazione del contesto non avrà mai alcun esito, anche quando la navigazione caricata è vuota o ha valore Null.</span><span class="sxs-lookup"><span data-stu-id="2672f-620">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="2672f-621">Al contrario, il tentativo di accedere a una proprietà di navigazione non caricata genera un'eccezione se il contesto viene eliminato anche se la proprietà di navigazione è una raccolta non vuota.</span><span class="sxs-lookup"><span data-stu-id="2672f-621">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="2672f-622">Se si verifica questa situazione significa che il codice dell'applicazione sta tentando di usare il caricamento lazy in un momento non valido e l'applicazione deve essere modificata in modo da non eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="2672f-622">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="2672f-623">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-623">**Why**</span></span>

<span data-ttu-id="2672f-624">Questa modifica è stata apportata per rendere coerente e corretto il comportamento durante un tentativo di caricamento lazy in un'istanza `DbContext` eliminata.</span><span class="sxs-lookup"><span data-stu-id="2672f-624">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="2672f-625">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-625">**Mitigations**</span></span>

<span data-ttu-id="2672f-626">Aggiornare il codice dell'applicazione per fare in modo che non venga tentato il caricamento lazy con un contesto eliminato oppure specificare una configurazione in modo che non venga eseguita alcuna operazione come descritto nel messaggio di eccezione.</span><span class="sxs-lookup"><span data-stu-id="2672f-626">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="2672f-627">La creazione di un numero eccessivo di provider di servizi interni è ora un errore per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="2672f-627">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="2672f-628">Problema n. 10236</span><span class="sxs-lookup"><span data-stu-id="2672f-628">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="2672f-629">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-629">**Old behavior**</span></span>

<span data-ttu-id="2672f-630">Nelle versioni precedenti a EF Core 3.0 veniva registrato un avviso per le applicazioni che creavano un numero eccessivo di provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="2672f-630">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="2672f-631">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-631">**New behavior**</span></span>

<span data-ttu-id="2672f-632">A partire da EF Core 3.0, l'avviso viene considerato un errore e viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="2672f-632">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="2672f-633">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-633">**Why**</span></span>

<span data-ttu-id="2672f-634">Questa modifica è stata apportata per gestire meglio il codice dell'applicazione tramite un'esposizione più esplicita di questa situazione di errore.</span><span class="sxs-lookup"><span data-stu-id="2672f-634">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="2672f-635">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-635">**Mitigations**</span></span>

<span data-ttu-id="2672f-636">L'azione più appropriata quando si verifica questo errore consiste nell'individuare la causa radice e nell'interrompere la creazione di numerosi provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="2672f-636">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="2672f-637">È possibile tuttavia convertire nuovamente l'errore in avviso o ignorarlo tramite una configurazione in `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="2672f-637">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="2672f-638">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2672f-638">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="2672f-639">Nuovo comportamento per la chiamata di HasOne/HasMany con una singola stringa</span><span class="sxs-lookup"><span data-stu-id="2672f-639">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="2672f-640">Problema n. 9171</span><span class="sxs-lookup"><span data-stu-id="2672f-640">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="2672f-641">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-641">**Old behavior**</span></span>

<span data-ttu-id="2672f-642">Prima di EF Core 3.0, il codice che chiama `HasOne` o `HasMany` con una singola stringa era interpretato in modo poco chiaro.</span><span class="sxs-lookup"><span data-stu-id="2672f-642">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="2672f-643">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2672f-643">For example:</span></span>
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="2672f-644">Apparentemente, il codice mette in relazione `Samurai` con un altro tipo di entità tramite la proprietà di navigazione `Entrance`, che può essere privata.</span><span class="sxs-lookup"><span data-stu-id="2672f-644">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="2672f-645">In realtà, il codice tenta di creare una relazione con un tipo di entità denominato `Entrance` senza proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="2672f-645">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="2672f-646">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-646">**New behavior**</span></span>

<span data-ttu-id="2672f-647">A partire da EF Core 3.0, il codice sopra riportato ora esegue quello che avrebbe dovuto fare in precedenza.</span><span class="sxs-lookup"><span data-stu-id="2672f-647">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="2672f-648">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-648">**Why**</span></span>

<span data-ttu-id="2672f-649">Il comportamento precedente era molto poco chiaro, soprattutto durante la lettura del codice di configurazione e la ricerca di errori.</span><span class="sxs-lookup"><span data-stu-id="2672f-649">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="2672f-650">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-650">**Mitigations**</span></span>

<span data-ttu-id="2672f-651">Questa modifica causerà problemi solo nelle applicazioni che configurano relazioni in modo esplicito usando stringhe per i nomi dei tipi e senza specificare in modo esplicito la proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="2672f-651">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="2672f-652">Non è uno scenario comune.</span><span class="sxs-lookup"><span data-stu-id="2672f-652">This is not common.</span></span>
<span data-ttu-id="2672f-653">Il comportamento precedente può essere ottenuto passando esplicitamente `null` per il nome della proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="2672f-653">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="2672f-654">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2672f-654">For example:</span></span>

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="2672f-655">Il tipo restituito per diversi metodi asincroni è cambiato da Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="2672f-655">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="2672f-656">Problema n. 15184</span><span class="sxs-lookup"><span data-stu-id="2672f-656">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="2672f-657">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-657">**Old behavior**</span></span>

<span data-ttu-id="2672f-658">I metodi asincroni seguenti in precedenza restituivano il tipo `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="2672f-658">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="2672f-659">`ValueGenerator.NextValueAsync()` (e classi derivate)</span><span class="sxs-lookup"><span data-stu-id="2672f-659">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="2672f-660">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-660">**New behavior**</span></span>

<span data-ttu-id="2672f-661">I metodi indicati in precedenza ora restituiscono il tipo `ValueTask<T>` sullo stesso `T` come in precedenza.</span><span class="sxs-lookup"><span data-stu-id="2672f-661">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="2672f-662">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-662">**Why**</span></span>

<span data-ttu-id="2672f-663">Questa modifica riduce il numero delle allocazioni di heap sostenute quando si richiamano questi metodi, con un miglioramento generale delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="2672f-663">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="2672f-664">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-664">**Mitigations**</span></span>

<span data-ttu-id="2672f-665">Le applicazioni semplicemente in attesa delle API precedenti devono solo essere ricompilate e non sono richieste modifiche del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="2672f-665">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="2672f-666">Per scenari di utilizzo più complessi (ad esempio, il passaggio del tipo `Task` restituito a `Task.WhenAny()`) è richiesto in genere che il tipo `ValueTask<T>` restituito venga convertito in `Task<T>` chiamando `AsTask()` su di esso.</span><span class="sxs-lookup"><span data-stu-id="2672f-666">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="2672f-667">Si noti che in questo modo si annulla la riduzione delle allocazioni consentita da questa modifica.</span><span class="sxs-lookup"><span data-stu-id="2672f-667">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="2672f-668">L'annotazione Relational:TypeMapping è ora TypeMapping</span><span class="sxs-lookup"><span data-stu-id="2672f-668">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="2672f-669">Problema n. 9913</span><span class="sxs-lookup"><span data-stu-id="2672f-669">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="2672f-670">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-670">**Old behavior**</span></span>

<span data-ttu-id="2672f-671">Il nome di annotazione delle annotazioni di mapping del tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="2672f-671">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="2672f-672">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-672">**New behavior**</span></span>

<span data-ttu-id="2672f-673">Il nome di annotazione delle annotazioni di mapping del tipo è ora "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="2672f-673">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="2672f-674">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-674">**Why**</span></span>

<span data-ttu-id="2672f-675">Il mapping dei tipi non viene più usato solo per i provider di database relazionali.</span><span class="sxs-lookup"><span data-stu-id="2672f-675">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="2672f-676">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-676">**Mitigations**</span></span>

<span data-ttu-id="2672f-677">Ciò causa un'interruzione solo nelle applicazioni che accedono al mapping dei tipi direttamente come annotazione. Questa situazione non è comune.</span><span class="sxs-lookup"><span data-stu-id="2672f-677">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="2672f-678">L'azione più appropriata per risolvere il problema consiste nell'usare la superficie dell'API per accedere al mapping dei tipi anziché l'annotazione.</span><span class="sxs-lookup"><span data-stu-id="2672f-678">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="2672f-679">ToTable in un tipo derivato genera un'eccezione</span><span class="sxs-lookup"><span data-stu-id="2672f-679">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="2672f-680">Problema n. 11811</span><span class="sxs-lookup"><span data-stu-id="2672f-680">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="2672f-681">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-681">**Old behavior**</span></span>

<span data-ttu-id="2672f-682">Nelle versioni precedenti a EF Core 3.0, la chiamata a `ToTable()` in un tipo derivato veniva ignorata poiché soltanto la strategia di mapping dell'ereditarietà era una tabella per gerarchia dove la chiamata non era valida.</span><span class="sxs-lookup"><span data-stu-id="2672f-682">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="2672f-683">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-683">**New behavior**</span></span>

<span data-ttu-id="2672f-684">A partire da EF Core 3.0 e in preparazione all'aggiunta del supporto per la tabella per tipo e per TPC in una versione successiva, la chiamata a `ToTable()` in un tipo derivato genera un'eccezione per evitare una modifica del mapping imprevista in futuro.</span><span class="sxs-lookup"><span data-stu-id="2672f-684">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="2672f-685">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-685">**Why**</span></span>

<span data-ttu-id="2672f-686">Attualmente il mapping di un tipo derivato in una tabella diversa non è un'operazione valida.</span><span class="sxs-lookup"><span data-stu-id="2672f-686">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="2672f-687">Questa modifica consente di evitare interruzioni future quando l'operazione diventerà un'operazione valida.</span><span class="sxs-lookup"><span data-stu-id="2672f-687">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="2672f-688">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-688">**Mitigations**</span></span>

<span data-ttu-id="2672f-689">Rimuovere qualsiasi tentativo di mapping di tipi derivati in altre tabelle.</span><span class="sxs-lookup"><span data-stu-id="2672f-689">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="2672f-690">ForSqlServerHasIndex sostituito con HasIndex</span><span class="sxs-lookup"><span data-stu-id="2672f-690">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="2672f-691">Problema n. 12366</span><span class="sxs-lookup"><span data-stu-id="2672f-691">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="2672f-692">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-692">**Old behavior**</span></span>

<span data-ttu-id="2672f-693">Nelle versioni precedenti a EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offriva un metodo per configurare le colonne usate con `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="2672f-693">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="2672f-694">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-694">**New behavior**</span></span>

<span data-ttu-id="2672f-695">A partire da EF Core 3.0, l'uso di `Include` in un indice è supportato a livello relazionale.</span><span class="sxs-lookup"><span data-stu-id="2672f-695">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="2672f-696">Usare `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="2672f-696">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="2672f-697">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-697">**Why**</span></span>

<span data-ttu-id="2672f-698">Questa modifica è stata apportata per consolidare l'API per gli indici con `Include` in un'unica posizione per tutti i provider di database.</span><span class="sxs-lookup"><span data-stu-id="2672f-698">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="2672f-699">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-699">**Mitigations**</span></span>

<span data-ttu-id="2672f-700">Usare la nuova API, come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="2672f-700">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="2672f-701">Modifiche dell'API dei metadati</span><span class="sxs-lookup"><span data-stu-id="2672f-701">Metadata API changes</span></span>

[<span data-ttu-id="2672f-702">Problema n. 214</span><span class="sxs-lookup"><span data-stu-id="2672f-702">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="2672f-703">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-703">**New behavior**</span></span>

<span data-ttu-id="2672f-704">Le proprietà seguenti sono state convertite in metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="2672f-704">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="2672f-705">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-705">**Why**</span></span>

<span data-ttu-id="2672f-706">Questa modifica semplifica l'implementazione delle interfacce menzionate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="2672f-706">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="2672f-707">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-707">**Mitigations**</span></span>

<span data-ttu-id="2672f-708">Usare i nuovi metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="2672f-708">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="2672f-709">Modifiche dell'API dei metadati specifiche del provider</span><span class="sxs-lookup"><span data-stu-id="2672f-709">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="2672f-710">Problema n. 214</span><span class="sxs-lookup"><span data-stu-id="2672f-710">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="2672f-711">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-711">**New behavior**</span></span>

<span data-ttu-id="2672f-712">I metodi di estensione specifici del provider verranno resi flat:</span><span class="sxs-lookup"><span data-stu-id="2672f-712">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="2672f-713">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-713">**Why**</span></span>

<span data-ttu-id="2672f-714">Questa modifica semplifica l'implementazione dei metodi di estensione menzionati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="2672f-714">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="2672f-715">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-715">**Mitigations**</span></span>

<span data-ttu-id="2672f-716">Usare i nuovi metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="2672f-716">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="2672f-717">EF Core non invia più pragma per l'imposizione della chiave esterna di SQLite</span><span class="sxs-lookup"><span data-stu-id="2672f-717">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="2672f-718">Problema n. 12151</span><span class="sxs-lookup"><span data-stu-id="2672f-718">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="2672f-719">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-719">**Old behavior**</span></span>

<span data-ttu-id="2672f-720">Nelle versioni precedenti a EF Core 3.0, EF Core inviava `PRAGMA foreign_keys = 1` quando veniva aperta una connessione a SQLite.</span><span class="sxs-lookup"><span data-stu-id="2672f-720">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="2672f-721">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-721">**New behavior**</span></span>

<span data-ttu-id="2672f-722">A partire da EF Core 3.0, EF Core non invia più `PRAGMA foreign_keys = 1` quando viene aperta una connessione a SQLite.</span><span class="sxs-lookup"><span data-stu-id="2672f-722">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="2672f-723">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-723">**Why**</span></span>

<span data-ttu-id="2672f-724">Questa modifica è stata apportata poiché EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` per impostazione predefinita. Ciò significa che l'imposizione della chiave esterna è abilitata per impostazione predefinita e non deve essere abilitata in modo esplicito ogni volta che viene aperta una connessione.</span><span class="sxs-lookup"><span data-stu-id="2672f-724">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="2672f-725">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-725">**Mitigations**</span></span>

<span data-ttu-id="2672f-726">Le chiavi esterne sono abilitate per impostazione predefinita in SQLitePCLRaw.bundle_e_sqlite3, usato per impostazione predefinita per EF Core.</span><span class="sxs-lookup"><span data-stu-id="2672f-726">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="2672f-727">Per gli altri casi, è possibile abilitare le chiavi esterne specificando `Foreign Keys=True` nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="2672f-727">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="2672f-728">Microsoft.EntityFrameworkCore.Sqlite dipende ora da SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="2672f-728">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="2672f-729">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-729">**Old behavior**</span></span>

<span data-ttu-id="2672f-730">Nelle versioni precedenti a EF Core 3.0, EF Core usava `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="2672f-730">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="2672f-731">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-731">**New behavior**</span></span>

<span data-ttu-id="2672f-732">A partire da EF Core 3.0, EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="2672f-732">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="2672f-733">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-733">**Why**</span></span>

<span data-ttu-id="2672f-734">Questa modifica è stata apportata per rendere coerente la versione di SQLite usata in iOS con le altre piattaforme.</span><span class="sxs-lookup"><span data-stu-id="2672f-734">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="2672f-735">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-735">**Mitigations**</span></span>

<span data-ttu-id="2672f-736">Per usare la versione di SQLite nativa in iOS, configurare `Microsoft.Data.Sqlite` per l'uso di un'aggregazione `SQLitePCLRaw` diversa.</span><span class="sxs-lookup"><span data-stu-id="2672f-736">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="2672f-737">I valori Guid vengono ora archiviati come TEXT in SQLite</span><span class="sxs-lookup"><span data-stu-id="2672f-737">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="2672f-738">Problema n. 15078</span><span class="sxs-lookup"><span data-stu-id="2672f-738">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="2672f-739">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-739">**Old behavior**</span></span>

<span data-ttu-id="2672f-740">I valori Guid in precedenza venivano archiviati come valori BLOB in SQLite.</span><span class="sxs-lookup"><span data-stu-id="2672f-740">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="2672f-741">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-741">**New behavior**</span></span>

<span data-ttu-id="2672f-742">I valori Guid vengono ora archiviati come TEXT.</span><span class="sxs-lookup"><span data-stu-id="2672f-742">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="2672f-743">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-743">**Why**</span></span>

<span data-ttu-id="2672f-744">Il formato binario dei valori Guid non è standardizzato.</span><span class="sxs-lookup"><span data-stu-id="2672f-744">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="2672f-745">L'archiviazione dei valori come TEXT rende il database più compatibile con altre tecnologie.</span><span class="sxs-lookup"><span data-stu-id="2672f-745">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="2672f-746">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-746">**Mitigations**</span></span>

<span data-ttu-id="2672f-747">È possibile eseguire la migrazione dei database esistenti al nuovo formato eseguendo SQL nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="2672f-747">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

<span data-ttu-id="2672f-748">In EF Core è anche possibile continuare a usare il comportamento precedente configurando un convertitore di valori per queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="2672f-748">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="2672f-749">Microsoft.Data.Sqlite rimane in grado di leggere i valori Guid sia da colonne BLOB che TEXT. Tuttavia, poiché è stato modificato il formato predefinito per i parametri e le costanti, probabilmente sarà necessario intervenire per la maggior parte degli scenari che coinvolgono valori Guid.</span><span class="sxs-lookup"><span data-stu-id="2672f-749">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="2672f-750">I valori char vengono ora archiviati come testo in SQLite</span><span class="sxs-lookup"><span data-stu-id="2672f-750">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="2672f-751">Problema n. 15020</span><span class="sxs-lookup"><span data-stu-id="2672f-751">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="2672f-752">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-752">**Old behavior**</span></span>

<span data-ttu-id="2672f-753">I valori char in precedenza venivano archiviati come valori interi in SQLite.</span><span class="sxs-lookup"><span data-stu-id="2672f-753">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="2672f-754">Un valore char di *A* veniva ad esempio archiviato come valore intero 65.</span><span class="sxs-lookup"><span data-stu-id="2672f-754">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="2672f-755">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-755">**New behavior**</span></span>

<span data-ttu-id="2672f-756">I valori char vengono ora archiviati come testo.</span><span class="sxs-lookup"><span data-stu-id="2672f-756">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="2672f-757">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-757">**Why**</span></span>

<span data-ttu-id="2672f-758">L'archiviazione dei valori come testo è un'operazione più naturale e rende il database più compatibile con altre tecnologie.</span><span class="sxs-lookup"><span data-stu-id="2672f-758">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="2672f-759">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-759">**Mitigations**</span></span>

<span data-ttu-id="2672f-760">È possibile eseguire la migrazione dei database esistenti al nuovo formato eseguendo SQL nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="2672f-760">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="2672f-761">In EF Core è anche possibile continuare a usare il comportamento precedente configurando un convertitore di valori per queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="2672f-761">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="2672f-762">Microsoft.Data.Sqlite rimane comunque in grado di leggere i valori di caratteri presenti sia nelle colonne di valori interi sia in quelle di testo, quindi alcuni scenari potrebbero non richiedere alcuna azione.</span><span class="sxs-lookup"><span data-stu-id="2672f-762">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="2672f-763">Gli ID di migrazione vengono ora generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica</span><span class="sxs-lookup"><span data-stu-id="2672f-763">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="2672f-764">Problema n. 12978</span><span class="sxs-lookup"><span data-stu-id="2672f-764">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="2672f-765">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-765">**Old behavior**</span></span>

<span data-ttu-id="2672f-766">Gli ID di migrazione venivano inavvertitamente generati usando il calendario delle impostazioni cultura correnti.</span><span class="sxs-lookup"><span data-stu-id="2672f-766">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="2672f-767">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-767">**New behavior**</span></span>

<span data-ttu-id="2672f-768">Gli ID di migrazione ora vengono sempre generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica (calendario gregoriano).</span><span class="sxs-lookup"><span data-stu-id="2672f-768">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="2672f-769">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-769">**Why**</span></span>

<span data-ttu-id="2672f-770">L'ordine delle migrazioni è importante quando si esegue l'aggiornamento del database o si risolvono i conflitti di unione.</span><span class="sxs-lookup"><span data-stu-id="2672f-770">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="2672f-771">L'uso del calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica evita i problemi che possono verificarsi quando i membri del team hanno calendari di sistema diversi.</span><span class="sxs-lookup"><span data-stu-id="2672f-771">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="2672f-772">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-772">**Mitigations**</span></span>

<span data-ttu-id="2672f-773">Questa modifica interessa gli utenti che usano un calendario non gregoriano in cui l'anno ha un'estensione superiore al calendario gregoriano (come il calendario buddista tailandese).</span><span class="sxs-lookup"><span data-stu-id="2672f-773">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="2672f-774">Gli ID di migrazione esistenti dovranno essere aggiornati in modo che le nuove migrazioni vengano collocate dopo le migrazioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="2672f-774">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="2672f-775">L'ID di migrazione è disponibile nell'attributo di migrazione presente nei file di progettazione delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="2672f-775">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="2672f-776">È necessario aggiornare anche la tabella della cronologia delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="2672f-776">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="2672f-777">Il metodo UseRowNumberForPaging è stato rimosso</span><span class="sxs-lookup"><span data-stu-id="2672f-777">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="2672f-778">Problema n. 16400</span><span class="sxs-lookup"><span data-stu-id="2672f-778">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="2672f-779">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-779">**Old behavior**</span></span>

<span data-ttu-id="2672f-780">Prima di EF Core 3.0 si poteva usare `UseRowNumberForPaging` per generare codice SQL per la suddivisione in pagine compatibile con SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="2672f-780">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="2672f-781">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-781">**New behavior**</span></span>

<span data-ttu-id="2672f-782">A partire da EF Core 3.0, EF genererà solo codice SQL per la suddivisione in pagine compatibile solo con versioni successive di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2672f-782">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="2672f-783">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-783">**Why**</span></span>

<span data-ttu-id="2672f-784">Questa modifica è stata apportata perché [SQL Server 2008 non è più un prodotto supportato](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) e l'aggiornamento di questa funzionalità per interagire con le modifiche apportate per le query in EF Core 3.0 è un lavoro significativo.</span><span class="sxs-lookup"><span data-stu-id="2672f-784">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="2672f-785">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-785">**Mitigations**</span></span>

<span data-ttu-id="2672f-786">È consigliabile eseguire l'aggiornamento a una versione più recente di SQL Server o usare un livello di compatibilità superiore, in modo che il codice SQL generato sia supportato.</span><span class="sxs-lookup"><span data-stu-id="2672f-786">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="2672f-787">Detto questo, se non è possibile procedere in questo modo, [aggiungere un commento per il problema](https://github.com/aspnet/EntityFrameworkCore/issues/16400) con indicazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="2672f-787">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="2672f-788">Microsoft potrebbe rivedere questa decisione in base ai commenti e suggerimenti.</span><span class="sxs-lookup"><span data-stu-id="2672f-788">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="2672f-789">Info/metadati dell'estensione rimossi da IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="2672f-789">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="2672f-790">Problema n. 16119</span><span class="sxs-lookup"><span data-stu-id="2672f-790">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="2672f-791">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-791">**Old behavior**</span></span>

<span data-ttu-id="2672f-792">`IDbContextOptionsExtension` conteneva metodi per fornire i metadati relativi all'estensione.</span><span class="sxs-lookup"><span data-stu-id="2672f-792">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="2672f-793">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-793">**New behavior**</span></span>

<span data-ttu-id="2672f-794">Questi metodi sono stati spostati in una nuova classe di base astratta `DbContextOptionsExtensionInfo`, restituita da una nuova proprietà `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="2672f-794">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="2672f-795">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-795">**Why**</span></span>

<span data-ttu-id="2672f-796">Nelle versioni dalla 2.0 alla 3.0 è stato necessario aggiungere o modificare questi metodi più volte.</span><span class="sxs-lookup"><span data-stu-id="2672f-796">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="2672f-797">Suddividendoli in una nuova classe di base astratta sarà più facile apportare questo tipo di modifiche senza compromettere il funzionamento delle estensioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="2672f-797">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="2672f-798">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-798">**Mitigations**</span></span>

<span data-ttu-id="2672f-799">Aggiornare le estensioni per seguire il nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="2672f-799">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="2672f-800">Sono disponibili esempi nelle numerose implementazioni di `IDbContextOptionsExtension` per diversi tipi di estensioni nel codice sorgente di EF Core.</span><span class="sxs-lookup"><span data-stu-id="2672f-800">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="2672f-801">LogQueryPossibleExceptionWithAggregateOperator è stato rinominato</span><span class="sxs-lookup"><span data-stu-id="2672f-801">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="2672f-802">Problema n. 10985</span><span class="sxs-lookup"><span data-stu-id="2672f-802">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="2672f-803">**Modifica**</span><span class="sxs-lookup"><span data-stu-id="2672f-803">**Change**</span></span>

<span data-ttu-id="2672f-804">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` è stato rinominato in `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="2672f-804">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="2672f-805">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-805">**Why**</span></span>

<span data-ttu-id="2672f-806">Allineamento del nome di questo evento di avviso con tutti gli altri eventi di avviso.</span><span class="sxs-lookup"><span data-stu-id="2672f-806">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="2672f-807">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-807">**Mitigations**</span></span>

<span data-ttu-id="2672f-808">Usare il nuovo nome.</span><span class="sxs-lookup"><span data-stu-id="2672f-808">Use the new name.</span></span> <span data-ttu-id="2672f-809">(Si noti che il numero di ID evento non è stato modificato.)</span><span class="sxs-lookup"><span data-stu-id="2672f-809">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="2672f-810">Chiarimenti per l'API per i nomi di vincolo di chiave esterna</span><span class="sxs-lookup"><span data-stu-id="2672f-810">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="2672f-811">Problema n. 10730</span><span class="sxs-lookup"><span data-stu-id="2672f-811">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="2672f-812">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-812">**Old behavior**</span></span>

<span data-ttu-id="2672f-813">Prima di EF Core 3.0, si faceva riferimento ai nomi di vincolo di chiave esterna semplicemente con "Name".</span><span class="sxs-lookup"><span data-stu-id="2672f-813">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="2672f-814">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2672f-814">For example:</span></span>

```csharp
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="2672f-815">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-815">**New behavior**</span></span>

<span data-ttu-id="2672f-816">A partire da EF Core 3.0, si fa ora riferimento ai nomi di vincolo di chiave esterna con "ConstraintName".</span><span class="sxs-lookup"><span data-stu-id="2672f-816">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="2672f-817">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2672f-817">For example:</span></span>

```csharp
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="2672f-818">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-818">**Why**</span></span>

<span data-ttu-id="2672f-819">Questa modifica introduce coerenza per la denominazione in quest'area e chiarisce anche che si tratta del nome del vincolo di chiave esterna e non del nome della colonna o della proprietà per cui è definita la chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="2672f-819">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="2672f-820">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-820">**Mitigations**</span></span>

<span data-ttu-id="2672f-821">Usare il nuovo nome.</span><span class="sxs-lookup"><span data-stu-id="2672f-821">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="2672f-822">IRelationalDatabaseCreator.HasTables/HasTablesAsync sono diventati pubblici</span><span class="sxs-lookup"><span data-stu-id="2672f-822">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="2672f-823">Problema n. 15997</span><span class="sxs-lookup"><span data-stu-id="2672f-823">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="2672f-824">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-824">**Old behavior**</span></span>

<span data-ttu-id="2672f-825">Prima di EF Core 3.0 questi metodi erano protetti.</span><span class="sxs-lookup"><span data-stu-id="2672f-825">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="2672f-826">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-826">**New behavior**</span></span>

<span data-ttu-id="2672f-827">A partire da EF Core 3.0 questi metodi sono pubblici.</span><span class="sxs-lookup"><span data-stu-id="2672f-827">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="2672f-828">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-828">**Why**</span></span>

<span data-ttu-id="2672f-829">Questi metodi vengono usati da EF per determinare se un database viene creato, ma vuoto.</span><span class="sxs-lookup"><span data-stu-id="2672f-829">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="2672f-830">Ciò risulta utile all'esterno di EF quando occorre determinare se applicare o meno le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="2672f-830">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="2672f-831">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-831">**Mitigations**</span></span>

<span data-ttu-id="2672f-832">Modificare l'accessibilità di eventuali override.</span><span class="sxs-lookup"><span data-stu-id="2672f-832">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="2672f-833">Microsoft.EntityFrameworkCore.Design è ora un pacchetto DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="2672f-833">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="2672f-834">Problema n. 11506</span><span class="sxs-lookup"><span data-stu-id="2672f-834">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="2672f-835">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-835">**Old behavior**</span></span>

<span data-ttu-id="2672f-836">Prima di EF Core 3.0, Microsoft.EntityFrameworkCore.Design era un pacchetto NuGet normale ed era possibile fare riferimento al relativo assembly dai progetti dipendenti.</span><span class="sxs-lookup"><span data-stu-id="2672f-836">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="2672f-837">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-837">**New behavior**</span></span>

<span data-ttu-id="2672f-838">A partire da EF Core 3.0 è un pacchetto DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="2672f-838">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="2672f-839">Questo significa che la dipendenza non verrà trasferita in modo transitivo in altri progetti e che non è più possibile fare riferimento all'assembly per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="2672f-839">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="2672f-840">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-840">**Why**</span></span>

<span data-ttu-id="2672f-841">Questo pacchetto è destinato solo all'uso in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="2672f-841">This package is only intended to be used at design time.</span></span> <span data-ttu-id="2672f-842">Le applicazioni distribuite non devono farvi riferimento.</span><span class="sxs-lookup"><span data-stu-id="2672f-842">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="2672f-843">Questa raccomandazione è rafforzata dall'impostazione del pacchetto come DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="2672f-843">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="2672f-844">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-844">**Mitigations**</span></span>

<span data-ttu-id="2672f-845">Se è necessario fare riferimento a questo pacchetto per eseguire l'override del comportamento in fase di progettazione di EF Core, è possibile aggiornare i metadati dell'elemento PackageReference nel progetto.</span><span class="sxs-lookup"><span data-stu-id="2672f-845">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="2672f-846">In presenza di riferimenti transitivi al pacchetto tramite Microsoft.EntityFrameworkCore.Tools, sarà necessario aggiungere un PackageReference esplicito al pacchetto per modificare i relativi metadati.</span><span class="sxs-lookup"><span data-stu-id="2672f-846">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="2672f-847">Aggiornamento di SQLitePCL.raw alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2672f-847">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="2672f-848">Problema n. 14824</span><span class="sxs-lookup"><span data-stu-id="2672f-848">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="2672f-849">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-849">**Old behavior**</span></span>

<span data-ttu-id="2672f-850">Microsoft.EntityFrameworkCore.Sqlite dipendeva in precedenza dalla versione 1.1.12 di SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="2672f-850">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="2672f-851">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-851">**New behavior**</span></span>

<span data-ttu-id="2672f-852">Il pacchetto è stato aggiornato in base alla versione 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="2672f-852">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="2672f-853">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-853">**Why**</span></span>

<span data-ttu-id="2672f-854">La versione 2.0.0 di SQLitePCL.raw è destinata a .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="2672f-854">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="2672f-855">Era in precedenza destinata a .NET Standard 1.1 e ciò richiedeva un notevole impegno di chiusura di pacchetti transitivi per il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="2672f-855">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="2672f-856">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-856">**Mitigations**</span></span>

<span data-ttu-id="2672f-857">SQLitePCL.raw versione 2.0.0 include alcune modifiche che causano un'interruzione.</span><span class="sxs-lookup"><span data-stu-id="2672f-857">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="2672f-858">Per informazioni dettagliate, vedere le [note sulla versione](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="2672f-858">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="2672f-859">NetTopologySuite aggiornato alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="2672f-859">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="2672f-860">Problema n. 14825</span><span class="sxs-lookup"><span data-stu-id="2672f-860">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="2672f-861">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-861">**Old behavior**</span></span>

<span data-ttu-id="2672f-862">I pacchetti spaziali dipendevano in precedenza dalla versione 1.15.1 di NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="2672f-862">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="2672f-863">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-863">**New behavior**</span></span>

<span data-ttu-id="2672f-864">Il pacchetto è stato aggiornato in modo da dipendere dalla versione 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="2672f-864">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="2672f-865">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-865">**Why**</span></span>

<span data-ttu-id="2672f-866">La versione 2.0.0 di NetTopologySuite risolve vari problemi di usabilità riscontrati dagli utenti di EF Core.</span><span class="sxs-lookup"><span data-stu-id="2672f-866">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="2672f-867">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-867">**Mitigations**</span></span>

<span data-ttu-id="2672f-868">NetTopologySuite versione 2.0.0 include alcune modifiche che causano un'interruzione.</span><span class="sxs-lookup"><span data-stu-id="2672f-868">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="2672f-869">Per informazioni dettagliate, vedere le [note sulla versione](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="2672f-869">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="2672f-870">Viene usato Microsoft. Data. SqlClient al posto di System. Data. SqlClient</span><span class="sxs-lookup"><span data-stu-id="2672f-870">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="2672f-871">Rilevamento del problema #15636</span><span class="sxs-lookup"><span data-stu-id="2672f-871">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="2672f-872">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-872">**Old behavior**</span></span>

<span data-ttu-id="2672f-873">Microsoft. EntityFrameworkCore. SqlServer in precedenza dipende da System. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="2672f-873">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="2672f-874">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-874">**New behavior**</span></span>

<span data-ttu-id="2672f-875">Il pacchetto è stato aggiornato in base a Microsoft. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="2672f-875">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="2672f-876">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-876">**Why**</span></span>

<span data-ttu-id="2672f-877">Microsoft. Data. SqlClient è il driver di accesso ai dati principale per SQL Server in futuro e System. Data. SqlClient non è più l'obiettivo dello sviluppo.</span><span class="sxs-lookup"><span data-stu-id="2672f-877">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="2672f-878">Alcune funzionalità importanti, ad esempio Always Encrypted, sono disponibili solo in Microsoft. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="2672f-878">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="2672f-879">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-879">**Mitigations**</span></span>

<span data-ttu-id="2672f-880">Se il codice prende una dipendenza diretta da System. Data. SqlClient, è necessario modificarlo in modo che faccia riferimento a Microsoft. Data. SqlClient. Poiché i due pacchetti gestiscono un livello molto elevato di compatibilità API, questo dovrebbe essere solo una semplice modifica del pacchetto e dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="2672f-880">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="2672f-881">Devono essere configurare più relazioni ambigue che fanno riferimento a se stesse</span><span class="sxs-lookup"><span data-stu-id="2672f-881">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="2672f-882">Problema n. 13573</span><span class="sxs-lookup"><span data-stu-id="2672f-882">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="2672f-883">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-883">**Old behavior**</span></span>

<span data-ttu-id="2672f-884">Un tipo di entità con più proprietà di navigazione unidirezionale che fanno riferimento a se stesse e più chiavi esterne corrispondenti è stato erroneamente configurato come relazione singola.</span><span class="sxs-lookup"><span data-stu-id="2672f-884">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="2672f-885">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2672f-885">For example:</span></span>

```csharp
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

<span data-ttu-id="2672f-886">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-886">**New behavior**</span></span>

<span data-ttu-id="2672f-887">Questo scenario viene ora rilevato nella compilazione del modello e viene generata un'eccezione indicante che il modello è ambiguo.</span><span class="sxs-lookup"><span data-stu-id="2672f-887">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="2672f-888">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-888">**Why**</span></span>

<span data-ttu-id="2672f-889">Il modello risultante era ambiguo e sarà probabilmente errato in questo caso.</span><span class="sxs-lookup"><span data-stu-id="2672f-889">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="2672f-890">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-890">**Mitigations**</span></span>

<span data-ttu-id="2672f-891">Usare la configurazione completa della relazione.</span><span class="sxs-lookup"><span data-stu-id="2672f-891">Use full configuration of the relationship.</span></span> <span data-ttu-id="2672f-892">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2672f-892">For example:</span></span>

```csharp
modelBuilder
     .Entity<User>()
     .HasOne(e => e.CreatedBy)
     .WithMany();
 
 modelBuilder
     .Entity<User>()
     .HasOne(e => e.UpdatedBy)
     .WithMany();
```

<a name="udf-empty-string"></a>
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="2672f-893">DbFunction. Schema è una stringa null o vuota che lo configura in modo che sia nello schema predefinito del modello</span><span class="sxs-lookup"><span data-stu-id="2672f-893">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="2672f-894">Rilevamento del problema #12757</span><span class="sxs-lookup"><span data-stu-id="2672f-894">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="2672f-895">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="2672f-895">**Old behavior**</span></span>

<span data-ttu-id="2672f-896">Un DbFunction configurato con schema come stringa vuota è stato trattato come funzione predefinita senza uno schema.</span><span class="sxs-lookup"><span data-stu-id="2672f-896">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="2672f-897">Il codice seguente, ad esempio, eseguirà il mapping `DatePart` funzione CLR per `DATEPART` funzione predefinita in SqlServer.</span><span class="sxs-lookup"><span data-stu-id="2672f-897">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="2672f-898">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="2672f-898">**New behavior**</span></span>

<span data-ttu-id="2672f-899">Tutti i mapping di DbFunction sono considerati mappati alle funzioni definite dall'utente.</span><span class="sxs-lookup"><span data-stu-id="2672f-899">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="2672f-900">Pertanto, il valore stringa vuoto inserisce la funzione all'interno dello schema predefinito per il modello.</span><span class="sxs-lookup"><span data-stu-id="2672f-900">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="2672f-901">Che potrebbe essere lo schema configurato in modo esplicito tramite l'API Fluent `modelBuilder.HasDefaultSchema()` o `dbo` in caso contrario.</span><span class="sxs-lookup"><span data-stu-id="2672f-901">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="2672f-902">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="2672f-902">**Why**</span></span>

<span data-ttu-id="2672f-903">Lo schema precedentemente vuoto era un modo per trattare la funzione è incorporata, ma tale logica è applicabile solo per SqlServer, in cui le funzioni predefinite non appartengono ad alcuno schema.</span><span class="sxs-lookup"><span data-stu-id="2672f-903">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="2672f-904">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="2672f-904">**Mitigations**</span></span>

<span data-ttu-id="2672f-905">Configurare manualmente la conversione di DbFunction per eseguirne il mapping a una funzione predefinita.</span><span class="sxs-lookup"><span data-stu-id="2672f-905">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
