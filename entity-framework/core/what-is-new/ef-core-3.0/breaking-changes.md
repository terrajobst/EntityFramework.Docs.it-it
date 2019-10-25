---
title: Modifiche che causano un'interruzione in EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: b2e3881e3454377dab7851cba999ed6b891def4e
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812130"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="9c735-102">Modifiche di rilievo incluse nel EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="9c735-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="9c735-103">Le modifiche alle API e al comportamento seguenti possono causare l'interruzione delle applicazioni esistenti durante l'aggiornamento a 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="9c735-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="9c735-104">Le modifiche che si prevede abbiano impatto solo sui provider di database sono documentate nelle [modifiche che influiscono sul provider](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="9c735-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="9c735-105">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="9c735-105">Summary</span></span>

| <span data-ttu-id="9c735-106">**Modifica di rilievo**</span><span class="sxs-lookup"><span data-stu-id="9c735-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="9c735-107">**Impatto**</span><span class="sxs-lookup"><span data-stu-id="9c735-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="9c735-108">Le query LINQ non vengono più valutate nel client</span><span class="sxs-lookup"><span data-stu-id="9c735-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="9c735-109">High</span><span class="sxs-lookup"><span data-stu-id="9c735-109">High</span></span>       |
| [<span data-ttu-id="9c735-110">EF Core 3.0 usa come destinazione .NET Standard 2.1 invece che .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="9c735-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="9c735-111">High</span><span class="sxs-lookup"><span data-stu-id="9c735-111">High</span></span>      |
| [<span data-ttu-id="9c735-112">Lo strumento da riga di comando di EF Core, dotnet ef, non è più incluso in .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="9c735-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="9c735-113">High</span><span class="sxs-lookup"><span data-stu-id="9c735-113">High</span></span>      |
| [<span data-ttu-id="9c735-114">DetectChanges rispetta i valori di chiave generati dall'archivio</span><span class="sxs-lookup"><span data-stu-id="9c735-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="9c735-115">High</span><span class="sxs-lookup"><span data-stu-id="9c735-115">High</span></span>      |
| [<span data-ttu-id="9c735-116">I metodi FromSql, ExecuteSql ed ExecuteSqlAsync sono stati rinominati</span><span class="sxs-lookup"><span data-stu-id="9c735-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="9c735-117">High</span><span class="sxs-lookup"><span data-stu-id="9c735-117">High</span></span>      |
| [<span data-ttu-id="9c735-118">I tipi di query vengono consolidati con tipi di entità</span><span class="sxs-lookup"><span data-stu-id="9c735-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="9c735-119">High</span><span class="sxs-lookup"><span data-stu-id="9c735-119">High</span></span>      |
| [<span data-ttu-id="9c735-120">Entity Framework Core non è più incluso nel framework condiviso di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9c735-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="9c735-121">Medio</span><span class="sxs-lookup"><span data-stu-id="9c735-121">Medium</span></span>      |
| [<span data-ttu-id="9c735-122">Le eliminazioni a catena vengono ora eseguite immediatamente per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="9c735-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="9c735-123">Medio</span><span class="sxs-lookup"><span data-stu-id="9c735-123">Medium</span></span>      |
| [<span data-ttu-id="9c735-124">Il caricamento eager delle entità correlate ora si verifica in una singola query</span><span class="sxs-lookup"><span data-stu-id="9c735-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="9c735-125">Medio</span><span class="sxs-lookup"><span data-stu-id="9c735-125">Medium</span></span>      |
| [<span data-ttu-id="9c735-126">Semantica più chiara per DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="9c735-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="9c735-127">Medio</span><span class="sxs-lookup"><span data-stu-id="9c735-127">Medium</span></span>      |
| [<span data-ttu-id="9c735-128">L'API di configurazione per le relazioni di tipo di proprietà è stata modificata</span><span class="sxs-lookup"><span data-stu-id="9c735-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="9c735-129">Medio</span><span class="sxs-lookup"><span data-stu-id="9c735-129">Medium</span></span>      |
| [<span data-ttu-id="9c735-130">Ogni proprietà usa la generazione di chiavi di tipo intero in memoria indipendenti</span><span class="sxs-lookup"><span data-stu-id="9c735-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="9c735-131">Medio</span><span class="sxs-lookup"><span data-stu-id="9c735-131">Medium</span></span>      |
| [<span data-ttu-id="9c735-132">Le query senza rilevamento delle modifiche non eseguono più la risoluzione delle identità</span><span class="sxs-lookup"><span data-stu-id="9c735-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="9c735-133">Medio</span><span class="sxs-lookup"><span data-stu-id="9c735-133">Medium</span></span>      |
| [<span data-ttu-id="9c735-134">Modifiche dell'API dei metadati</span><span class="sxs-lookup"><span data-stu-id="9c735-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="9c735-135">Medio</span><span class="sxs-lookup"><span data-stu-id="9c735-135">Medium</span></span>      |
| [<span data-ttu-id="9c735-136">Modifiche dell'API dei metadati specifiche del provider</span><span class="sxs-lookup"><span data-stu-id="9c735-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="9c735-137">Medio</span><span class="sxs-lookup"><span data-stu-id="9c735-137">Medium</span></span>      |
| [<span data-ttu-id="9c735-138">Il metodo UseRowNumberForPaging è stato rimosso</span><span class="sxs-lookup"><span data-stu-id="9c735-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="9c735-139">Medio</span><span class="sxs-lookup"><span data-stu-id="9c735-139">Medium</span></span>      |
| [<span data-ttu-id="9c735-140">Non è possibile comporre il metodo dati da tabelle se usato con stored procedure</span><span class="sxs-lookup"><span data-stu-id="9c735-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="9c735-141">Medio</span><span class="sxs-lookup"><span data-stu-id="9c735-141">Medium</span></span>      |
| [<span data-ttu-id="9c735-142">I metodi FromSql possono essere specificati solo in radici di query</span><span class="sxs-lookup"><span data-stu-id="9c735-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="9c735-143">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-143">Low</span></span>      |
| [<span data-ttu-id="9c735-144">~~L'esecuzione di query viene registrata a livello di debug~~ - Modifica annullata</span><span class="sxs-lookup"><span data-stu-id="9c735-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="9c735-145">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-145">Low</span></span>      |
| [<span data-ttu-id="9c735-146">I valori di chiave temporanei non sono più impostati nelle istanze di entità</span><span class="sxs-lookup"><span data-stu-id="9c735-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="9c735-147">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-147">Low</span></span>      |
| [<span data-ttu-id="9c735-148">Le entità dipendenti che condividono la tabella con l'entità di sicurezza sono ora facoltative</span><span class="sxs-lookup"><span data-stu-id="9c735-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="9c735-149">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-149">Low</span></span>      |
| [<span data-ttu-id="9c735-150">Tutte le entità che condividono una tabella con una colonna di token di concorrenza devono eseguirne il mapping a una proprietà</span><span class="sxs-lookup"><span data-stu-id="9c735-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="9c735-151">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-151">Low</span></span>      |
| [<span data-ttu-id="9c735-152">Per le proprietà ereditate da tipi senza mapping viene ora eseguito il mapping a una singola colonna per tutti i tipi derivati</span><span class="sxs-lookup"><span data-stu-id="9c735-152">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="9c735-153">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-153">Low</span></span>      |
| [<span data-ttu-id="9c735-154">La convenzione di proprietà di chiave esterna non ha più lo stesso nome della proprietà dell'entità di sicurezza</span><span class="sxs-lookup"><span data-stu-id="9c735-154">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="9c735-155">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-155">Low</span></span>      |
| [<span data-ttu-id="9c735-156">La connessione di database viene ora chiusa se non viene più usata prima del completamento di TransactionScope</span><span class="sxs-lookup"><span data-stu-id="9c735-156">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="9c735-157">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-157">Low</span></span>      |
| [<span data-ttu-id="9c735-158">I campi sottostanti vengono usati per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="9c735-158">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="9c735-159">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-159">Low</span></span>      |
| [<span data-ttu-id="9c735-160">Viene generata un'eccezione se vengono trovati più campi sottostanti compatibili</span><span class="sxs-lookup"><span data-stu-id="9c735-160">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="9c735-161">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-161">Low</span></span>      |
| [<span data-ttu-id="9c735-162">I nomi delle proprietà solo campo devono corrispondere al nome di campo</span><span class="sxs-lookup"><span data-stu-id="9c735-162">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="9c735-163">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-163">Low</span></span>      |
| [<span data-ttu-id="9c735-164">AddDbContext/AddDbContextPool non chiamano più AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="9c735-164">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="9c735-165">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-165">Low</span></span>      |
| [<span data-ttu-id="9c735-166">DbContext.Entry esegue ora un DetectChanges locale</span><span class="sxs-lookup"><span data-stu-id="9c735-166">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="9c735-167">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-167">Low</span></span>      |
| [<span data-ttu-id="9c735-168">Le chiavi matrice di byte e di stringhe non vengono generate dal client per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="9c735-168">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="9c735-169">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-169">Low</span></span>      |
| [<span data-ttu-id="9c735-170">ILoggerFactory è ora un servizio con ambito</span><span class="sxs-lookup"><span data-stu-id="9c735-170">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="9c735-171">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-171">Low</span></span>      |
| [<span data-ttu-id="9c735-172">I proxy di caricamento lazy non presuppongono più che le proprietà di navigazione vengano caricate completamente</span><span class="sxs-lookup"><span data-stu-id="9c735-172">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="9c735-173">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-173">Low</span></span>      |
| [<span data-ttu-id="9c735-174">La creazione di un numero eccessivo di provider di servizi interni è ora un errore per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="9c735-174">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="9c735-175">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-175">Low</span></span>      |
| [<span data-ttu-id="9c735-176">Nuovo comportamento per la chiamata di HasOne/HasMany con una singola stringa</span><span class="sxs-lookup"><span data-stu-id="9c735-176">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="9c735-177">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-177">Low</span></span>      |
| [<span data-ttu-id="9c735-178">Il tipo restituito per diversi metodi asincroni è cambiato da Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="9c735-178">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="9c735-179">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-179">Low</span></span>      |
| [<span data-ttu-id="9c735-180">L'annotazione Relational:TypeMapping è ora TypeMapping</span><span class="sxs-lookup"><span data-stu-id="9c735-180">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="9c735-181">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-181">Low</span></span>      |
| [<span data-ttu-id="9c735-182">ToTable in un tipo derivato genera un'eccezione</span><span class="sxs-lookup"><span data-stu-id="9c735-182">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="9c735-183">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-183">Low</span></span>      |
| [<span data-ttu-id="9c735-184">EF Core non invia più pragma per l'imposizione della chiave esterna di SQLite</span><span class="sxs-lookup"><span data-stu-id="9c735-184">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="9c735-185">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-185">Low</span></span>      |
| [<span data-ttu-id="9c735-186">Microsoft.EntityFrameworkCore.Sqlite dipende ora da SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="9c735-186">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="9c735-187">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-187">Low</span></span>      |
| [<span data-ttu-id="9c735-188">I valori Guid vengono ora archiviati come TEXT in SQLite</span><span class="sxs-lookup"><span data-stu-id="9c735-188">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="9c735-189">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-189">Low</span></span>      |
| [<span data-ttu-id="9c735-190">I valori char vengono ora archiviati come testo in SQLite</span><span class="sxs-lookup"><span data-stu-id="9c735-190">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="9c735-191">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-191">Low</span></span>      |
| [<span data-ttu-id="9c735-192">Gli ID di migrazione vengono ora generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica</span><span class="sxs-lookup"><span data-stu-id="9c735-192">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="9c735-193">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-193">Low</span></span>      |
| [<span data-ttu-id="9c735-194">Info/metadati dell'estensione rimossi da IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="9c735-194">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="9c735-195">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-195">Low</span></span>      |
| [<span data-ttu-id="9c735-196">LogQueryPossibleExceptionWithAggregateOperator è stato rinominato</span><span class="sxs-lookup"><span data-stu-id="9c735-196">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="9c735-197">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-197">Low</span></span>      |
| [<span data-ttu-id="9c735-198">Chiarimenti per l'API per i nomi di vincolo di chiave esterna</span><span class="sxs-lookup"><span data-stu-id="9c735-198">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="9c735-199">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-199">Low</span></span>      |
| [<span data-ttu-id="9c735-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync sono diventati pubblici</span><span class="sxs-lookup"><span data-stu-id="9c735-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="9c735-201">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-201">Low</span></span>      |
| [<span data-ttu-id="9c735-202">Microsoft.EntityFrameworkCore.Design è ora un pacchetto DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="9c735-202">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="9c735-203">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-203">Low</span></span>      |
| [<span data-ttu-id="9c735-204">Aggiornamento di SQLitePCL.raw alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="9c735-204">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="9c735-205">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-205">Low</span></span>      |
| [<span data-ttu-id="9c735-206">NetTopologySuite aggiornato alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="9c735-206">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="9c735-207">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-207">Low</span></span>      |
| [<span data-ttu-id="9c735-208">Viene usato Microsoft. Data. SqlClient al posto di System. Data. SqlClient</span><span class="sxs-lookup"><span data-stu-id="9c735-208">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="9c735-209">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-209">Low</span></span>      |
| [<span data-ttu-id="9c735-210">Devono essere configurare più relazioni ambigue che fanno riferimento a se stesse</span><span class="sxs-lookup"><span data-stu-id="9c735-210">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="9c735-211">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-211">Low</span></span>      |
| [<span data-ttu-id="9c735-212">DbFunction. Schema è una stringa null o vuota che lo configura in modo che sia nello schema predefinito del modello</span><span class="sxs-lookup"><span data-stu-id="9c735-212">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="9c735-213">Basso</span><span class="sxs-lookup"><span data-stu-id="9c735-213">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="9c735-214">Le query LINQ non vengono più valutate nel client</span><span class="sxs-lookup"><span data-stu-id="9c735-214">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="9c735-215">[Problema n. 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Vedere anche il problema n. 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="9c735-215">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="9c735-216">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-216">**Old behavior**</span></span>

<span data-ttu-id="9c735-217">Nelle versioni precedenti alla versione 3.0, quando EF Core non era in grado di convertire un'espressione inclusa in una query in SQL o in un parametro, l'espressione veniva automaticamente valutata nel client.</span><span class="sxs-lookup"><span data-stu-id="9c735-217">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="9c735-218">Per impostazione predefinita, la valutazione client di espressioni potenzialmente dispendiose si limitava ad attivare solo un avviso.</span><span class="sxs-lookup"><span data-stu-id="9c735-218">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="9c735-219">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-219">**New behavior**</span></span>

<span data-ttu-id="9c735-220">A partire dalla versione 3.0, EF Core consente solo la valutazione delle espressioni nella proiezione di primo livello (l'ultima chiamata `Select()` nella query) nel client.</span><span class="sxs-lookup"><span data-stu-id="9c735-220">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="9c735-221">Quando le espressioni in altre posizioni all'interno della query non possono essere convertite in SQL o in un parametro, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="9c735-221">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="9c735-222">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-222">**Why**</span></span>

<span data-ttu-id="9c735-223">La valutazione client automatica delle query consente di eseguire numerose query anche nel caso in cui parti importanti delle query non possono essere convertite.</span><span class="sxs-lookup"><span data-stu-id="9c735-223">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="9c735-224">Questo comportamento può causare un comportamento imprevisto e potenzialmente dannoso che può diventare evidente solo in produzione.</span><span class="sxs-lookup"><span data-stu-id="9c735-224">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="9c735-225">Ad esempio, una condizione in una chiamata `Where()` che non può essere convertita può causare il trasferimento di tutte le righe della tabella del server di database e l'applicazione del filtro nel client.</span><span class="sxs-lookup"><span data-stu-id="9c735-225">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="9c735-226">È probabile che questa situazione non venga rilevata se la tabella contiene solo alcune righe in fase di sviluppo, ma che abbia un grande impatto quando l'applicazione passa in produzione dove la tabella può contenere milioni di righe.</span><span class="sxs-lookup"><span data-stu-id="9c735-226">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="9c735-227">Gli avvisi di valutazione client inoltre si sono rivelati molto facili da ignorare durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="9c735-227">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="9c735-228">Inoltre, la valutazione client automatica può causare problemi in cui il miglioramento della conversione di query per espressioni specifiche causa modifiche impreviste che causano un'interruzione da una versione all'altra.</span><span class="sxs-lookup"><span data-stu-id="9c735-228">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="9c735-229">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-229">**Mitigations**</span></span>

<span data-ttu-id="9c735-230">Se una query non può essere convertita completamente, riscrivere la query in un formato che possa essere convertito o usare `AsEnumerable()`, `ToList()` o un elemento simile per riportare in modo esplicito i dati nel client dove possono essere quindi ulteriormente elaborati usando LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="9c735-230">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="9c735-231">EF Core 3.0 usa come destinazione .NET Standard 2.1 invece che .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="9c735-231">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="9c735-232">Problema n. 15498</span><span class="sxs-lookup"><span data-stu-id="9c735-232">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="9c735-233">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-233">**Old behavior**</span></span>

<span data-ttu-id="9c735-234">Prima della versione 3.0, EF Core usava come destinazione .NET Standard 2.0 e poteva essere eseguito in tutte le piattaforme che supportano tale standard, incluso .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9c735-234">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="9c735-235">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-235">**New behavior**</span></span>

<span data-ttu-id="9c735-236">A partire dalla versione 3.0, EF Core usa come destinazione .NET Standard 2.1 e verrà eseguito in tutte le piattaforme che supportano questo standard.</span><span class="sxs-lookup"><span data-stu-id="9c735-236">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="9c735-237">.NET Framework non è incluso.</span><span class="sxs-lookup"><span data-stu-id="9c735-237">This does not include .NET Framework.</span></span>

<span data-ttu-id="9c735-238">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-238">**Why**</span></span>

<span data-ttu-id="9c735-239">Questo comportamento deriva da una decisione strategica per le tecnologie .NET, che mira a concentrare le energie su .NET Core e su altre piattaforme .NET moderne, ad esempio Xamarin.</span><span class="sxs-lookup"><span data-stu-id="9c735-239">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="9c735-240">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-240">**Mitigations**</span></span>

<span data-ttu-id="9c735-241">Valutare il passaggio a una piattaforma .NET moderna.</span><span class="sxs-lookup"><span data-stu-id="9c735-241">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="9c735-242">Se non è possibile, continuare a usare EF Core 2.1 o EF Core 2.2, che supportano entrambe .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9c735-242">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="9c735-243">Entity Framework Core non è più incluso nel framework condiviso di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9c735-243">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="9c735-244">Annunci problema n. 325</span><span class="sxs-lookup"><span data-stu-id="9c735-244">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="9c735-245">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-245">**Old behavior**</span></span>

<span data-ttu-id="9c735-246">Nelle versioni precedenti ad ASP.NET Core 3.0, quando si aggiungeva un riferimento a un pacchetto in `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, veniva inserito EF Core e alcuni dei provider di dati di EF Core come il provider di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9c735-246">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="9c735-247">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-247">**New behavior**</span></span>

<span data-ttu-id="9c735-248">A partire dalla versione 3.0, il framework condiviso di ASP.NET Core non include EF Core o provider di dati di EF Core.</span><span class="sxs-lookup"><span data-stu-id="9c735-248">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="9c735-249">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-249">**Why**</span></span>

<span data-ttu-id="9c735-250">Prima di questa modifica, per ottenere EF Core erano necessarie procedure diverse, a seconda che l'applicazione avesse o meno come destinazione ASP.NET Core e SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9c735-250">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="9c735-251">Inoltre, l'aggiornamento di ASP.NET Core forzava l'aggiornamento di EF Core e del provider di SQL Server, non sempre auspicabile.</span><span class="sxs-lookup"><span data-stu-id="9c735-251">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="9c735-252">Con questa modifica, la procedura per ottenere EF Core è la stessa in tutti i provider, le implementazioni .NET supportate e i tipi di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="9c735-252">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="9c735-253">Gli sviluppatori possono ora controllare anche in modo preciso quando vengono aggiornati EF Core e i provider di dati di EF Core.</span><span class="sxs-lookup"><span data-stu-id="9c735-253">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="9c735-254">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-254">**Mitigations**</span></span>

<span data-ttu-id="9c735-255">Per usare EF Core in un'applicazione ASP.NET Core 3.0 o in un'altra applicazione supportata, aggiungere in modo esplicito un riferimento al pacchetto al provider di database di EF Core che verrà usato dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9c735-255">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="9c735-256">Lo strumento della riga di comando di EF Core, dotnet ef, non è più incluso in .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="9c735-256">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="9c735-257">Problema n. 14016</span><span class="sxs-lookup"><span data-stu-id="9c735-257">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="9c735-258">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-258">**Old behavior**</span></span>

<span data-ttu-id="9c735-259">Prima della versione 3.0, lo strumento `dotnet ef` era incluso in .NET Core SDK ed era immediatamente disponibile dalla riga di comando di qualsiasi progetto senza richiedere passaggi aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="9c735-259">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="9c735-260">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-260">**New behavior**</span></span>

<span data-ttu-id="9c735-261">A partire dalla versione 3.0, .NET SDK non include lo strumento `dotnet ef` pertanto, prima di poterlo usare, è necessario installarlo in modo esplicito come strumento locale o globale.</span><span class="sxs-lookup"><span data-stu-id="9c735-261">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="9c735-262">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-262">**Why**</span></span>

<span data-ttu-id="9c735-263">Questa modifica consente di distribuire e aggiornare `dotnet ef` come uno strumento della riga di comando di .NET in NuGet, coerentemente con il fatto che anche EF Core 3.0 viene distribuito come pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="9c735-263">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="9c735-264">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-264">**Mitigations**</span></span>

<span data-ttu-id="9c735-265">Per essere in grado di gestire le migrazioni o eseguire lo scaffolding di `DbContext`, installare `dotnet-ef` come strumento globale:</span><span class="sxs-lookup"><span data-stu-id="9c735-265">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="9c735-266">È inoltre possibile ottenerlo come strumento locale quando si ripristinano le dipendenze di un progetto che lo dichiara come dipendenza di strumenti utilizzando un [file manifesto dello strumento](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="9c735-266">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="9c735-267">I metodi FromSql, ExecuteSql ed ExecuteSqlAsync sono stati rinominati</span><span class="sxs-lookup"><span data-stu-id="9c735-267">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="9c735-268">Problema n. 10996</span><span class="sxs-lookup"><span data-stu-id="9c735-268">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="9c735-269">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-269">**Old behavior**</span></span>

<span data-ttu-id="9c735-270">Prima di EF Core 3.0, erano disponibili overload per questi nomi di metodo per supportare l'uso con una stringa normale o una stringa che deve essere interpolata in SQL e parametri.</span><span class="sxs-lookup"><span data-stu-id="9c735-270">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="9c735-271">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-271">**New behavior**</span></span>

<span data-ttu-id="9c735-272">A partire da EF Core 3.0, usare `FromSqlRaw`, `ExecuteSqlRaw` e `ExecuteSqlRawAsync` per creare una query con parametri in cui i parametri vengono passati separatamente dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="9c735-272">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="9c735-273">Esempio:</span><span class="sxs-lookup"><span data-stu-id="9c735-273">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="9c735-274">Usare `FromSqlInterpolated`, `ExecuteSqlInterpolated`, e `ExecuteSqlInterpolatedAsync` per creare una query con parametri in cui i parametri vengono passati come parte di una stringa di query interpolata.</span><span class="sxs-lookup"><span data-stu-id="9c735-274">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="9c735-275">Esempio:</span><span class="sxs-lookup"><span data-stu-id="9c735-275">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="9c735-276">Si noti che entrambe le query precedenti produrranno lo stesso codice SQL con parametri con gli stessi parametri SQL.</span><span class="sxs-lookup"><span data-stu-id="9c735-276">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="9c735-277">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-277">**Why**</span></span>

<span data-ttu-id="9c735-278">Con gli overload di metodi come questi, è molto facile chiamare accidentalmente il metodo con stringa non elaborata anche se l'intento era chiamare il metodo con stringa interpolata e viceversa.</span><span class="sxs-lookup"><span data-stu-id="9c735-278">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="9c735-279">Il risultato potrebbero essere query senza parametri, quando invece è prevista la parametrizzazione.</span><span class="sxs-lookup"><span data-stu-id="9c735-279">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="9c735-280">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-280">**Mitigations**</span></span>

<span data-ttu-id="9c735-281">Passare all'uso dei nuovi nomi di metodo.</span><span class="sxs-lookup"><span data-stu-id="9c735-281">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="9c735-282">Non è possibile comporre il metodo dati da tabelle se usato con stored procedure</span><span class="sxs-lookup"><span data-stu-id="9c735-282">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="9c735-283">Rilevamento del problema #15392</span><span class="sxs-lookup"><span data-stu-id="9c735-283">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="9c735-284">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-284">**Old behavior**</span></span>

<span data-ttu-id="9c735-285">Prima di EF Core 3,0, il metodo dati da tabelle ha tentato di rilevare se il SQL passato può essere composto in base a.</span><span class="sxs-lookup"><span data-stu-id="9c735-285">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="9c735-286">Ha fatto la valutazione del client quando SQL era non componibile come un stored procedure.</span><span class="sxs-lookup"><span data-stu-id="9c735-286">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="9c735-287">La query seguente ha funzionato eseguendo il stored procedure sul server e FirstOrDefault sul lato client.</span><span class="sxs-lookup"><span data-stu-id="9c735-287">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="9c735-288">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-288">**New behavior**</span></span>

<span data-ttu-id="9c735-289">A partire da EF Core 3,0, EF Core non tenterà di analizzare SQL.</span><span class="sxs-lookup"><span data-stu-id="9c735-289">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="9c735-290">Quindi, se si esegue la composizione dopo FromSqlRaw/FromSqlInterpolated, EF Core comporrà il SQL causando una sottoquery.</span><span class="sxs-lookup"><span data-stu-id="9c735-290">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="9c735-291">Quindi, se si usa un stored procedure con Composition, si otterrà un'eccezione per la sintassi SQL non valida.</span><span class="sxs-lookup"><span data-stu-id="9c735-291">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="9c735-292">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-292">**Why**</span></span>

<span data-ttu-id="9c735-293">EF Core 3,0 non supporta la valutazione automatica del client perché è stata soggetta a errori, come illustrato [qui](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="9c735-293">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="9c735-294">**Mitigazione**</span><span class="sxs-lookup"><span data-stu-id="9c735-294">**Mitigation**</span></span>

<span data-ttu-id="9c735-295">Se si usa un stored procedure in FromSqlRaw/FromSqlInterpolated, è noto che non è possibile componerlo, quindi è possibile aggiungere __AsEnumerable/AsAsyncEnumerable__ subito dopo la chiamata al metodo dati da tabelle per evitare qualsiasi composizione sul lato server.</span><span class="sxs-lookup"><span data-stu-id="9c735-295">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="9c735-296">I metodi FromSql possono essere specificati solo in radici di query</span><span class="sxs-lookup"><span data-stu-id="9c735-296">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="9c735-297">Problema n. 15704</span><span class="sxs-lookup"><span data-stu-id="9c735-297">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="9c735-298">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-298">**Old behavior**</span></span>

<span data-ttu-id="9c735-299">Prima di EF Core 3.0, il metodo `FromSql` poteva essere specificato in un punto qualsiasi nella query.</span><span class="sxs-lookup"><span data-stu-id="9c735-299">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="9c735-300">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-300">**New behavior**</span></span>

<span data-ttu-id="9c735-301">A partire da EF Core 3.0, i nuovi metodi `FromSqlRaw` e `FromSqlInterpolated` (che sostituiscono`FromSql`) possono essere specificati solo per radici di query, ad esempio direttamente in `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="9c735-301">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="9c735-302">Qualsiasi tentativo di specificarli altrove causerà un errore di compilazione.</span><span class="sxs-lookup"><span data-stu-id="9c735-302">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="9c735-303">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-303">**Why**</span></span>

<span data-ttu-id="9c735-304">La specifica di `FromSql` in qualsiasi posizione diversa da un `DbSet` non ha alcun significato aggiuntivo oppure valore aggiunto e può causare ambiguità in determinati scenari.</span><span class="sxs-lookup"><span data-stu-id="9c735-304">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="9c735-305">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-305">**Mitigations**</span></span>

<span data-ttu-id="9c735-306">Le chiamate di `FromSql` devono essere spostate in modo da comparire direttamente nel `DbSet` a cui si applicano.</span><span class="sxs-lookup"><span data-stu-id="9c735-306">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="9c735-307">Le query senza rilevamento delle modifiche non eseguono più la risoluzione delle identità</span><span class="sxs-lookup"><span data-stu-id="9c735-307">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="9c735-308">Problema n. 13518</span><span class="sxs-lookup"><span data-stu-id="9c735-308">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="9c735-309">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-309">**Old behavior**</span></span>

<span data-ttu-id="9c735-310">Prima di EF Core 3.0, la stessa istanza di un'entità poteva essere usata per ogni occorrenza di un entità con tipo e ID specifici.</span><span class="sxs-lookup"><span data-stu-id="9c735-310">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="9c735-311">Questo comportamento corrisponde a quello delle query con rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="9c735-311">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="9c735-312">Ad esempio, questa query:</span><span class="sxs-lookup"><span data-stu-id="9c735-312">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="9c735-313">restituisce la stessa istanza di `Category` per ogni `Product` associato alla categoria specificata.</span><span class="sxs-lookup"><span data-stu-id="9c735-313">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="9c735-314">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-314">**New behavior**</span></span>

<span data-ttu-id="9c735-315">A partire da EF Core 3.0, vengono create istanze di entità diverse quando un'entità con un determinato tipo e ID viene rilevata in posizioni diverse nel grafico restituito.</span><span class="sxs-lookup"><span data-stu-id="9c735-315">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="9c735-316">La query precedente, ad esempio, ora restituirà una nuova istanza di `Category` per ogni `Product` anche quando due prodotti sono associati alla stessa categoria.</span><span class="sxs-lookup"><span data-stu-id="9c735-316">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="9c735-317">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-317">**Why**</span></span>

<span data-ttu-id="9c735-318">La risoluzione delle identità (ovvero il processo per determinare che un'entità ha lo stesso tipo e ID di un'entità rilevata in precedenza) aggiunge un ulteriore sovraccarico della memoria e delle prestazioni,</span><span class="sxs-lookup"><span data-stu-id="9c735-318">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="9c735-319">il che va ad annullare il vantaggio derivante dall'uso delle query senza rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="9c735-319">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="9c735-320">Inoltre, anche se in alcuni casi la risoluzione delle identità può essere utile, tuttavia non è necessaria se le entità devono essere serializzate e inviate a un client, come avviene comunemente con le query senza rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="9c735-320">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="9c735-321">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-321">**Mitigations**</span></span>

<span data-ttu-id="9c735-322">Se la risoluzione delle identità è necessaria, usare una query con rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="9c735-322">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="9c735-323">~~L'esecuzione di query viene registrata a livello di debug~~ - Modifica annullata</span><span class="sxs-lookup"><span data-stu-id="9c735-323">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="9c735-324">Problema n. 14523</span><span class="sxs-lookup"><span data-stu-id="9c735-324">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="9c735-325">Questa modifica è stata annullata perché la nuova configurazione in EF Core 3.0 consente all'applicazione di specificare il livello di log per qualsiasi evento.</span><span class="sxs-lookup"><span data-stu-id="9c735-325">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="9c735-326">Ad esempio, per impostare la registrazione di SQL sul livello `Debug`, configurare il livello in modo esplicito in `OnConfiguring` o `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="9c735-326">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="9c735-327">I valori di chiave temporanei non sono più impostati nelle istanze di entità</span><span class="sxs-lookup"><span data-stu-id="9c735-327">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="9c735-328">Problema n. 12378</span><span class="sxs-lookup"><span data-stu-id="9c735-328">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="9c735-329">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-329">**Old behavior**</span></span>

<span data-ttu-id="9c735-330">Nelle versioni precedenti a EF Core 3.0 i valori temporanei venivano assegnati a tutte le proprietà di chiave per cui veniva in seguito generato un valore reale dal database.</span><span class="sxs-lookup"><span data-stu-id="9c735-330">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="9c735-331">In genere questi valori temporanei erano numeri negativi elevati.</span><span class="sxs-lookup"><span data-stu-id="9c735-331">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="9c735-332">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-332">**New behavior**</span></span>

<span data-ttu-id="9c735-333">A partire dalla versione 3.0, EF Core archivia il valore di chiave temporaneo come parte delle informazioni di rilevamento dell'entità e non modifica la proprietà di chiave.</span><span class="sxs-lookup"><span data-stu-id="9c735-333">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="9c735-334">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-334">**Why**</span></span>

<span data-ttu-id="9c735-335">Questa modifica è stata apportata per impedire che i valori di chiave temporanei diventino erroneamente permanenti quando un'entità rilevata in precedenza da un'istanza `DbContext` viene spostata in un'altra istanza `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="9c735-335">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="9c735-336">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-336">**Mitigations**</span></span>

<span data-ttu-id="9c735-337">Le applicazioni che assegnano valori di chiave primaria in chiavi esterne per creare associazioni tra le entità possono dipendere dal comportamento precedente se le chiavi primarie vengono generate dall'archivio e appartengono a entità con stato `Added`.</span><span class="sxs-lookup"><span data-stu-id="9c735-337">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="9c735-338">Questo può essere evitato:</span><span class="sxs-lookup"><span data-stu-id="9c735-338">This can be avoided by:</span></span>
* <span data-ttu-id="9c735-339">Non usando chiavi generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="9c735-339">Not using store-generated keys.</span></span>
* <span data-ttu-id="9c735-340">Impostando le proprietà di navigazione in modo da creare relazioni anziché impostando valori di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="9c735-340">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="9c735-341">Ottenendo i valori di chiave temporanei effettivi dalle informazioni di rilevamento dell'entità.</span><span class="sxs-lookup"><span data-stu-id="9c735-341">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="9c735-342">Ad esempio, `context.Entry(blog).Property(e => e.Id).CurrentValue` restituisce il valore temporaneo anche quando `blog.Id` non è stato impostato.</span><span class="sxs-lookup"><span data-stu-id="9c735-342">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="9c735-343">DetectChanges rispetta i valori di chiave generati dall'archivio</span><span class="sxs-lookup"><span data-stu-id="9c735-343">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="9c735-344">Problema n. 14616</span><span class="sxs-lookup"><span data-stu-id="9c735-344">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="9c735-345">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-345">**Old behavior**</span></span>

<span data-ttu-id="9c735-346">Nelle versioni precedenti a EF Core 3.0 un'entità non rilevata individuata da `DetectChanges` veniva rilevata nello stato `Added` e inserita come nuova riga quando veniva eseguita una chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="9c735-346">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="9c735-347">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-347">**New behavior**</span></span>

<span data-ttu-id="9c735-348">A partire da EF Core 3.0, se un'entità usa valori di chiave generati e viene impostato un valore di chiave, l'entità viene rilevata nello stato `Modified`.</span><span class="sxs-lookup"><span data-stu-id="9c735-348">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="9c735-349">Ciò significa che si presuppone l'esistenza di una riga per l'entità che viene aggiornata quando viene eseguita una chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="9c735-349">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="9c735-350">Se il valore di chiave non viene impostato o se il tipo di entità non usa chiavi generate, la nuova entità viene rilevata come `Added` come nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="9c735-350">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="9c735-351">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-351">**Why**</span></span>

<span data-ttu-id="9c735-352">Questa modifica è stata apportata per rendere più semplice e coerente l'uso di grafici di entità disconnesse con chiavi generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="9c735-352">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="9c735-353">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-353">**Mitigations**</span></span>

<span data-ttu-id="9c735-354">Questa modifica può interrompere un'applicazione se un tipo di entità è configurato per l'uso di chiavi generate ma i valori di chiave sono impostati in modo esplicito per le nuove istanze.</span><span class="sxs-lookup"><span data-stu-id="9c735-354">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="9c735-355">La correzione consiste nel configurare in modo esplicito le proprietà di chiave per non usare valori generati.</span><span class="sxs-lookup"><span data-stu-id="9c735-355">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="9c735-356">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="9c735-356">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="9c735-357">Oppure con annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="9c735-357">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="9c735-358">Le eliminazioni a catena vengono ora eseguite immediatamente per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="9c735-358">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="9c735-359">Problema n. 10114</span><span class="sxs-lookup"><span data-stu-id="9c735-359">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="9c735-360">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-360">**Old behavior**</span></span>

<span data-ttu-id="9c735-361">Nelle versioni precedenti alla versione 3.0, EF Core applicava azioni a catena (eliminazione delle entità dipendenti quando veniva eliminata un'entità di sicurezza obbligatoria o veniva recisa la relazione con un'entità di sicurezza obbligatoria) solo dopo la chiamata a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="9c735-361">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="9c735-362">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-362">**New behavior**</span></span>

<span data-ttu-id="9c735-363">A partire dalla versione 3.0, EF Core applica le azioni a catena non appena viene rilevata la condizione di attivazione.</span><span class="sxs-lookup"><span data-stu-id="9c735-363">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="9c735-364">Ad esempio, la chiamata a `context.Remove()` per eliminare un'entità di sicurezza causa anche l'impostazione immediata di tutti i dipendenti obbligatori correlati rilevati su `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="9c735-364">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="9c735-365">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-365">**Why**</span></span>

<span data-ttu-id="9c735-366">Questa modifica è stata apportata per migliorare l'esperienza di associazione di dati e degli scenari di controllo in cui è importante individuare le entità che verranno eliminate _prima_ della chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="9c735-366">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="9c735-367">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-367">**Mitigations**</span></span>

<span data-ttu-id="9c735-368">Il comportamento precedente può essere ripristinato tramite le impostazioni in `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="9c735-368">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="9c735-369">Esempio:</span><span class="sxs-lookup"><span data-stu-id="9c735-369">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="9c735-370">Il caricamento eager delle entità correlate ora si verifica in una singola query</span><span class="sxs-lookup"><span data-stu-id="9c735-370">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="9c735-371">Rilevamento del problema #18022</span><span class="sxs-lookup"><span data-stu-id="9c735-371">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="9c735-372">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-372">**Old behavior**</span></span>

<span data-ttu-id="9c735-373">Prima del 3,0, il caricamento immediato delle esplorazioni delle raccolte tramite gli operatori `Include` ha causato la generazione di più query sul database relazionale, una per ogni tipo di entità correlato.</span><span class="sxs-lookup"><span data-stu-id="9c735-373">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="9c735-374">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-374">**New behavior**</span></span>

<span data-ttu-id="9c735-375">A partire da 3,0, EF Core genera una singola query con JOIN nei database relazionali.</span><span class="sxs-lookup"><span data-stu-id="9c735-375">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="9c735-376">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-376">**Why**</span></span>

<span data-ttu-id="9c735-377">L'invio di più query per l'implementazione di una singola query LINQ ha causato numerosi problemi, incluse le prestazioni negative in quanto sono stati necessari più round trip di database e i dati coerenza problemi poiché ogni query poteva osservare uno stato diverso del database.</span><span class="sxs-lookup"><span data-stu-id="9c735-377">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="9c735-378">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-378">**Mitigations**</span></span>

<span data-ttu-id="9c735-379">Sebbene tecnicamente non si tratti di una modifica di rilievo, potrebbe avere un impatto significativo sulle prestazioni dell'applicazione quando una singola query contiene un numero elevato di operatori `Include` sulle esplorazioni della raccolta.</span><span class="sxs-lookup"><span data-stu-id="9c735-379">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="9c735-380">Per ulteriori informazioni e per riscrivere le query in modo più efficiente, [vedere il commento](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) .</span><span class="sxs-lookup"><span data-stu-id="9c735-380">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="9c735-381">Semantica più chiara per DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="9c735-381">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="9c735-382">Problema n. 12661</span><span class="sxs-lookup"><span data-stu-id="9c735-382">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="9c735-383">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-383">**Old behavior**</span></span>

<span data-ttu-id="9c735-384">Prima della versione 3.0, `DeleteBehavior.Restrict` creava chiavi esterne nel database con la semantica `Restrict`, ma modificava anche la correzione interna in modo non ovvio.</span><span class="sxs-lookup"><span data-stu-id="9c735-384">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="9c735-385">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-385">**New behavior**</span></span>

<span data-ttu-id="9c735-386">A partire dalla versione 3.0, `DeleteBehavior.Restrict` assicura che le chiavi esterne vengano create con la semantica `Restrict`, ovvero non a cascata e con generazione di un'eccezione in caso di violazione di vincolo, senza influire sulla correzione interna di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9c735-386">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="9c735-387">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-387">**Why**</span></span>

<span data-ttu-id="9c735-388">Questa modifica è stata apportata per migliorare l'esperienza di uso di `DeleteBehavior` in modo intuitivo, senza effetti collaterali imprevisti.</span><span class="sxs-lookup"><span data-stu-id="9c735-388">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="9c735-389">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-389">**Mitigations**</span></span>

<span data-ttu-id="9c735-390">Il comportamento precedente può essere ripristinato tramite `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="9c735-390">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="9c735-391">I tipi di query vengono consolidati con tipi di entità</span><span class="sxs-lookup"><span data-stu-id="9c735-391">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="9c735-392">Problema n. 14194</span><span class="sxs-lookup"><span data-stu-id="9c735-392">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="9c735-393">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-393">**Old behavior**</span></span>

<span data-ttu-id="9c735-394">Nelle versioni precedenti a EF Core 3.0 i [tipi di query](xref:core/modeling/keyless-entity-types) erano uno strumento per eseguire query su dati che non definiscono una chiave primaria in modo strutturato.</span><span class="sxs-lookup"><span data-stu-id="9c735-394">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="9c735-395">Veniva infatti usato un tipo di query per eseguire il mapping di tipi di entità senza chiavi (più probabilmente da una vista, ma anche da una tabella), mentre veniva usato un tipo di entità normale quando era disponibile una chiave (più probabilmente da una tabella, ma anche da una vista).</span><span class="sxs-lookup"><span data-stu-id="9c735-395">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="9c735-396">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-396">**New behavior**</span></span>

<span data-ttu-id="9c735-397">Un tipo di query diventa ora semplicemente un tipo di entità senza chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="9c735-397">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="9c735-398">I tipi di entità senza chiave hanno la stessa funzionalità dei tipi di query nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="9c735-398">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="9c735-399">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-399">**Why**</span></span>

<span data-ttu-id="9c735-400">Questa modifica è stata apportata per ridurre la confusione riguardo lo scopo dei tipi di query.</span><span class="sxs-lookup"><span data-stu-id="9c735-400">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="9c735-401">In particolare, si tratta di tipi di entità senza chiave che sono intrinsecamente di sola lettura per questo motivo ma non dovrebbero essere usati solo perché un tipo di entità deve essere di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="9c735-401">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="9c735-402">Analogamente, spesso vengono mappati alle viste solo perché le viste spesso non definiscono le chiavi.</span><span class="sxs-lookup"><span data-stu-id="9c735-402">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="9c735-403">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-403">**Mitigations**</span></span>

<span data-ttu-id="9c735-404">Le parti dell'API seguenti sono ora obsolete:</span><span class="sxs-lookup"><span data-stu-id="9c735-404">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="9c735-405">**`ModelBuilder.Query<>()`** - È necessario chiamare `ModelBuilder.Entity<>().HasNoKey()` per contrassegnare un tipo di entità come tipo senza chiavi.</span><span class="sxs-lookup"><span data-stu-id="9c735-405">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="9c735-406">Non ne viene eseguita la configurazione per convenzione per evitare una configurazione errata quando è prevista una chiave primaria che tuttavia non corrisponde alla convenzione.</span><span class="sxs-lookup"><span data-stu-id="9c735-406">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="9c735-407">**`DbQuery<>`** - Usare `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="9c735-407">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="9c735-408">**`DbContext.Query<>()`** - Usare `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="9c735-408">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="9c735-409">L'API di configurazione per le relazioni di tipo di proprietà è stata modificata</span><span class="sxs-lookup"><span data-stu-id="9c735-409">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="9c735-410">[Problema n. 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema n. 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema n. 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="9c735-410">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="9c735-411">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-411">**Old behavior**</span></span>

<span data-ttu-id="9c735-412">Nelle versioni precedenti a EF Core 3.0 la configurazione della relazione di proprietà veniva eseguita direttamente dopo la chiamata a `OwnsOne` o `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="9c735-412">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="9c735-413">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-413">**New behavior**</span></span>

<span data-ttu-id="9c735-414">A partire da EF Core 3.0, è disponibile l'API Fluent per configurare una proprietà di navigazione per il proprietario usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="9c735-414">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="9c735-415">Esempio:</span><span class="sxs-lookup"><span data-stu-id="9c735-415">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="9c735-416">La configurazione correlata alla relazione tra proprietario e elemento di proprietà deve essere ora concatenata dopo `WithOwner()` in modo analogo a come vengono configurate altre relazioni.</span><span class="sxs-lookup"><span data-stu-id="9c735-416">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="9c735-417">La configurazione per il tipo di proprietà viene comunque concatenata dopo `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="9c735-417">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="9c735-418">Esempio:</span><span class="sxs-lookup"><span data-stu-id="9c735-418">For example:</span></span>

```C#
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

<span data-ttu-id="9c735-419">Inoltre, la chiamata a `Entity()`, `HasOne()` o `Set()` con una destinazione di tipo di proprietà genera ora un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="9c735-419">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="9c735-420">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-420">**Why**</span></span>

<span data-ttu-id="9c735-421">Questa modifica è stata apportata per creare una separazione più netta tra la configurazione del tipo di proprietà e la _relazione_ con il tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="9c735-421">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="9c735-422">Ciò consente di eliminare ambiguità e confusione su metodi come `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="9c735-422">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="9c735-423">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-423">**Mitigations**</span></span>

<span data-ttu-id="9c735-424">Modificare la configurazione delle relazioni dei tipi di proprietà per usare la superficie della nuova API come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="9c735-424">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="9c735-425">Le entità dipendenti che condividono la tabella con l'entità di sicurezza sono ora facoltative</span><span class="sxs-lookup"><span data-stu-id="9c735-425">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="9c735-426">Problema n. 9005</span><span class="sxs-lookup"><span data-stu-id="9c735-426">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="9c735-427">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-427">**Old behavior**</span></span>

<span data-ttu-id="9c735-428">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="9c735-428">Consider the following model:</span></span>
```C#
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
<span data-ttu-id="9c735-429">Prima di EF Core 3.0, se `OrderDetails` è di proprietà di `Order` o viene mappato in modo esplicito alla stessa tabella, era sempre necessaria un'istanza di `OrderDetails` per l'aggiunta di un nuovo `Order`.</span><span class="sxs-lookup"><span data-stu-id="9c735-429">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="9c735-430">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-430">**New behavior**</span></span>

<span data-ttu-id="9c735-431">A partire dalla versione 3.0, EF Core consente di aggiungere un `Order` senza un `OrderDetails` ed esegue il mapping di tutte le proprietà di `OrderDetails`, tranne che la chiave primaria, a colonne che ammettono valori Null.</span><span class="sxs-lookup"><span data-stu-id="9c735-431">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="9c735-432">In fase di query, EF Core imposta `OrderDetails` su `null` se una delle relative proprietà obbligatorie non ha un valore o se non sono presenti proprietà obbligatorie oltre alla chiave primaria e tutte le proprietà sono `null`.</span><span class="sxs-lookup"><span data-stu-id="9c735-432">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="9c735-433">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-433">**Mitigations**</span></span>

<span data-ttu-id="9c735-434">Se il modello include una tabella condivisa dipendente con tutte le colonne facoltative, ma è previsto che la navigazione che punta a essa non sia `null`, l'applicazione deve essere modificata per gestire casi in cui la navigazione è `null`.</span><span class="sxs-lookup"><span data-stu-id="9c735-434">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="9c735-435">Se questo non è possibile, è consigliabile aggiungere una proprietà obbligatoria al tipo di entità o assegnare un valore non `null` ad almeno una proprietà.</span><span class="sxs-lookup"><span data-stu-id="9c735-435">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="9c735-436">Tutte le entità che condividono una tabella con una colonna di token di concorrenza devono eseguirne il mapping a una proprietà</span><span class="sxs-lookup"><span data-stu-id="9c735-436">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="9c735-437">Problema n. 14154</span><span class="sxs-lookup"><span data-stu-id="9c735-437">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="9c735-438">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-438">**Old behavior**</span></span>

<span data-ttu-id="9c735-439">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="9c735-439">Consider the following model:</span></span>
```C#
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
<span data-ttu-id="9c735-440">Prima di EF Core 3.0, se `OrderDetails` è di proprietà di `Order` o è mappato in modo esplicito alla stessa tabella, il solo aggiornamento di `OrderDetails` non aggiornerà il valore `Version` nel client e l'aggiornamento successivo avrà esito negativo.</span><span class="sxs-lookup"><span data-stu-id="9c735-440">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="9c735-441">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-441">**New behavior**</span></span>

<span data-ttu-id="9c735-442">A partire dalla versione 3.0, EF Core propaga il nuovo valore `Version` a `Order` se è proprietario di `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="9c735-442">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="9c735-443">In caso contrario, viene generata un'eccezione durante la convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="9c735-443">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="9c735-444">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-444">**Why**</span></span>

<span data-ttu-id="9c735-445">Questa modifica è stata apportata per evitare un valore del token di concorrenza non aggiornato quando viene aggiornata solo una delle entità mappate alla stessa tabella.</span><span class="sxs-lookup"><span data-stu-id="9c735-445">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="9c735-446">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-446">**Mitigations**</span></span>

<span data-ttu-id="9c735-447">Tutte le entità che condividono la tabella devono includere una proprietà mappata alla colonna del token di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="9c735-447">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="9c735-448">È possibile crearne una in stato shadow:</span><span class="sxs-lookup"><span data-stu-id="9c735-448">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="9c735-449">Per le proprietà ereditate da tipi senza mapping viene ora eseguito il mapping a una singola colonna per tutti i tipi derivati</span><span class="sxs-lookup"><span data-stu-id="9c735-449">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="9c735-450">Problema n. 13998</span><span class="sxs-lookup"><span data-stu-id="9c735-450">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="9c735-451">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-451">**Old behavior**</span></span>

<span data-ttu-id="9c735-452">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="9c735-452">Consider the following model:</span></span>
```C#
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

<span data-ttu-id="9c735-453">Prima di EF Core 3.0, per la proprietà `ShippingAddress` sarebbe stato eseguito il mapping a colonne separate per `BulkOrder` e `Order` per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="9c735-453">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="9c735-454">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-454">**New behavior**</span></span>

<span data-ttu-id="9c735-455">A partire dalla versione 3.0, EF Core crea solo una colonna per `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="9c735-455">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="9c735-456">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-456">**Why**</span></span>

<span data-ttu-id="9c735-457">Il comportamento precedente non era previsto.</span><span class="sxs-lookup"><span data-stu-id="9c735-457">The old behavoir was unexpected.</span></span>

<span data-ttu-id="9c735-458">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-458">**Mitigations**</span></span>

<span data-ttu-id="9c735-459">È ancora possibile eseguire il mapping esplicito della proprietà a una colonna separata per i tipi derivati:</span><span class="sxs-lookup"><span data-stu-id="9c735-459">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```C#
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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="9c735-460">La convenzione di proprietà di chiave esterna non ha più lo stesso nome della proprietà dell'entità di sicurezza</span><span class="sxs-lookup"><span data-stu-id="9c735-460">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="9c735-461">Problema n. 13274</span><span class="sxs-lookup"><span data-stu-id="9c735-461">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="9c735-462">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-462">**Old behavior**</span></span>

<span data-ttu-id="9c735-463">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="9c735-463">Consider the following model:</span></span>
```C#
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
<span data-ttu-id="9c735-464">Nelle versioni precedenti a EF Core 3.0 veniva usata la proprietà `CustomerId` per la chiave esterna per convenzione.</span><span class="sxs-lookup"><span data-stu-id="9c735-464">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="9c735-465">Tuttavia, se `Order` è un tipo di proprietà, `CustomerId` sarebbe la chiave primaria e ciò non è in genere auspicabile.</span><span class="sxs-lookup"><span data-stu-id="9c735-465">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="9c735-466">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-466">**New behavior**</span></span>

<span data-ttu-id="9c735-467">A partire da 3.0, EF Core non tenta di usare le proprietà per le chiavi esterne per convenzione se hanno lo stesso nome della proprietà dell'entità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="9c735-467">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="9c735-468">Viene ancora eseguita la corrispondenza tra i criteri del nome del tipo dell'entità di sicurezza concatenato al nome della proprietà dell'entità di sicurezza e il nome di navigazione concatenato al nome della proprietà dell'entità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="9c735-468">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="9c735-469">Esempio:</span><span class="sxs-lookup"><span data-stu-id="9c735-469">For example:</span></span>

```C#
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

```C#
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

<span data-ttu-id="9c735-470">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-470">**Why**</span></span>

<span data-ttu-id="9c735-471">Questa modifica è stata apportata per evitare una definizione errata della proprietà di chiave primaria nel tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="9c735-471">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="9c735-472">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-472">**Mitigations**</span></span>

<span data-ttu-id="9c735-473">Se la proprietà è stata progettata per essere la chiave esterna e di conseguenza parte della chiave primaria, configurarla in modo esplicito come chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="9c735-473">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="9c735-474">La connessione al database viene ora chiusa se non viene più usata prima del completamento di TransactionScope</span><span class="sxs-lookup"><span data-stu-id="9c735-474">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="9c735-475">Problema n. 14218</span><span class="sxs-lookup"><span data-stu-id="9c735-475">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="9c735-476">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-476">**Old behavior**</span></span>

<span data-ttu-id="9c735-477">Prima di EF Core 3.0, se il contesto apre la connessione all'interno di un `TransactionScope`, la connessione rimane aperta mentre è attivo il `TransactionScope` corrente.</span><span class="sxs-lookup"><span data-stu-id="9c735-477">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```C#
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

<span data-ttu-id="9c735-478">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-478">**New behavior**</span></span>

<span data-ttu-id="9c735-479">A partire dalla versione 3.0, EF Core chiude la connessione non appena non viene più usata.</span><span class="sxs-lookup"><span data-stu-id="9c735-479">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="9c735-480">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-480">**Why**</span></span>

<span data-ttu-id="9c735-481">Questa modifica consente di usare più contesti nello stesso `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="9c735-481">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="9c735-482">Il nuovo comportamento corrisponde anche a EF6.</span><span class="sxs-lookup"><span data-stu-id="9c735-482">The new behavior also matches EF6.</span></span>

<span data-ttu-id="9c735-483">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-483">**Mitigations**</span></span>

<span data-ttu-id="9c735-484">Se la connessione deve rimanere aperta, la chiamata esplicita di `OpenConnection()` garantirà che EF Core non la chiuda prematuramente:</span><span class="sxs-lookup"><span data-stu-id="9c735-484">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```C#
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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="9c735-485">Ogni proprietà usa la generazione di chiavi di tipo intero in memoria indipendenti</span><span class="sxs-lookup"><span data-stu-id="9c735-485">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="9c735-486">Problema n. 6872</span><span class="sxs-lookup"><span data-stu-id="9c735-486">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="9c735-487">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-487">**Old behavior**</span></span>

<span data-ttu-id="9c735-488">Nelle versioni precedenti a EF Core 3.0 veniva usato un unico generatore di valori condiviso per tutte le proprietà di chiavi di tipo intero in memoria.</span><span class="sxs-lookup"><span data-stu-id="9c735-488">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="9c735-489">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-489">**New behavior**</span></span>

<span data-ttu-id="9c735-490">A partire da EF Core 3.0, ogni proprietà di chiave di tipo intero riceve un generatore di valori quando viene usato il database in memoria.</span><span class="sxs-lookup"><span data-stu-id="9c735-490">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="9c735-491">Inoltre, se il database viene eliminato, la generazione di chiavi viene reimpostata per tutte le tabelle.</span><span class="sxs-lookup"><span data-stu-id="9c735-491">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="9c735-492">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-492">**Why**</span></span>

<span data-ttu-id="9c735-493">Questa modifica è stata apportata per allineare maggiormente la generazione di chiavi in memoria alla generazione di chiavi del database reale e per migliorare la possibilità di isolare i test l'uno dall'altro quando viene usato il database in memoria.</span><span class="sxs-lookup"><span data-stu-id="9c735-493">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="9c735-494">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-494">**Mitigations**</span></span>

<span data-ttu-id="9c735-495">Ciò può interrompere un'applicazione che si basa sull'impostazione di valori di chiave in memoria specifici.</span><span class="sxs-lookup"><span data-stu-id="9c735-495">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="9c735-496">È consigliabile non basare l'applicazione su valori di chiave specifici o eseguire l'aggiornamento per passare al nuovo comportamento.</span><span class="sxs-lookup"><span data-stu-id="9c735-496">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="9c735-497">I campi sottostanti vengono usati per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="9c735-497">Backing fields are used by default</span></span>

[<span data-ttu-id="9c735-498">Problema n. 12430</span><span class="sxs-lookup"><span data-stu-id="9c735-498">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="9c735-499">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-499">**Old behavior**</span></span>

<span data-ttu-id="9c735-500">Nelle versioni precedenti alla versione 3.0, anche se il campo sottostante di una proprietà era noto, per impostazione predefinita EF Core eseguiva la lettura e la scrittura del valore della proprietà usando i metodi getter e setter della proprietà.</span><span class="sxs-lookup"><span data-stu-id="9c735-500">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="9c735-501">L'eccezione era costituita dall'esecuzione di query in cui il campo sottostante, se noto, veniva impostato direttamente.</span><span class="sxs-lookup"><span data-stu-id="9c735-501">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="9c735-502">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-502">**New behavior**</span></span>

<span data-ttu-id="9c735-503">A partire da EF Core 3.0, se il campo sottostante di una proprietà è noto, la lettura e la scrittura della proprietà vengono sempre eseguite usando il campo sottostante.</span><span class="sxs-lookup"><span data-stu-id="9c735-503">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="9c735-504">Ciò potrebbe causare un'interruzione dell'applicazione se l'applicazione si basa su un comportamento aggiuntivo codificato nei metodi getter o setter.</span><span class="sxs-lookup"><span data-stu-id="9c735-504">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="9c735-505">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-505">**Why**</span></span>

<span data-ttu-id="9c735-506">Questa modifica è stata apportata per impedire a EF Core di attivare per errore la logica di business per impostazione predefinita quando si eseguono operazioni di database che interessano le entità.</span><span class="sxs-lookup"><span data-stu-id="9c735-506">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="9c735-507">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-507">**Mitigations**</span></span>

<span data-ttu-id="9c735-508">È possibile ripristinare il comportamento delle versioni precedenti alla versione 3.0 tramite la configurazione della modalità di accesso delle proprietà in `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9c735-508">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="9c735-509">Esempio:</span><span class="sxs-lookup"><span data-stu-id="9c735-509">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="9c735-510">Viene generata un'eccezione se vengono trovati più campi sottostanti compatibili</span><span class="sxs-lookup"><span data-stu-id="9c735-510">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="9c735-511">Problema n. 12523</span><span class="sxs-lookup"><span data-stu-id="9c735-511">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="9c735-512">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-512">**Old behavior**</span></span>

<span data-ttu-id="9c735-513">Nelle versioni precedenti a EF Core 3.0, se più campi soddisfacevano le regole di ricerca del campo sottostante di una proprietà, veniva selezionato un solo campo in base a un ordine di precedenza.</span><span class="sxs-lookup"><span data-stu-id="9c735-513">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="9c735-514">Ciò poteva causare l'uso di un campo non corretto nei casi ambigui.</span><span class="sxs-lookup"><span data-stu-id="9c735-514">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="9c735-515">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-515">**New behavior**</span></span>

<span data-ttu-id="9c735-516">A partire da EF Core 3.0, se più campi corrispondono alla stessa proprietà, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="9c735-516">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="9c735-517">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-517">**Why**</span></span>

<span data-ttu-id="9c735-518">Questa modifica è stata apportata per evitare di usare automaticamente un campo rispetto a un altro quando un solo campo può essere quello corretto.</span><span class="sxs-lookup"><span data-stu-id="9c735-518">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="9c735-519">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-519">**Mitigations**</span></span>

<span data-ttu-id="9c735-520">Per le proprietà con campi sottostanti ambigui, il campo da usare deve essere specificato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="9c735-520">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="9c735-521">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="9c735-521">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="9c735-522">I nomi delle proprietà solo campo devono corrispondere al nome di campo</span><span class="sxs-lookup"><span data-stu-id="9c735-522">Field-only property names should match the field name</span></span>

<span data-ttu-id="9c735-523">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-523">**Old behavior**</span></span>

<span data-ttu-id="9c735-524">Prima di EF Core 3,0, una proprietà può essere specificata da un valore stringa e, se non è stata trovata alcuna proprietà con tale nome nel tipo .NET, EF Core tenterà di associarla a un campo usando le regole di convenzione.</span><span class="sxs-lookup"><span data-stu-id="9c735-524">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
```C#
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```
```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="9c735-525">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-525">**New behavior**</span></span>

<span data-ttu-id="9c735-526">A partire da EF Core 3.0, una proprietà solo campo deve corrispondere esattamente al nome del campo.</span><span class="sxs-lookup"><span data-stu-id="9c735-526">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="9c735-527">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-527">**Why**</span></span>

<span data-ttu-id="9c735-528">Questa modifica è stata introdotta per evitare di usare lo stesso campo per due proprietà con nome simile. Le regole di corrispondenza per le proprietà solo campo sono state anche uniformate a quelle per le proprietà mappate a proprietà CLR.</span><span class="sxs-lookup"><span data-stu-id="9c735-528">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="9c735-529">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-529">**Mitigations**</span></span>

<span data-ttu-id="9c735-530">Le proprietà solo campo devono avere lo stesso nome del campo a cui vengono mappate.</span><span class="sxs-lookup"><span data-stu-id="9c735-530">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="9c735-531">In una versione futura di EF Core dopo il 3,0, si prevede di abilitare di nuovo in modo esplicito il nome di un campo diverso dal nome della proprietà (vedere il problema [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="9c735-531">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="9c735-532">AddDbContext/AddDbContextPool non chiamano più AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="9c735-532">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="9c735-533">Problema n. 14756</span><span class="sxs-lookup"><span data-stu-id="9c735-533">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="9c735-534">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-534">**Old behavior**</span></span>

<span data-ttu-id="9c735-535">Prima di EF Core 3.0, la chiamata di `AddDbContext` oppure `AddDbContextPool` comporta anche la registrazione dei servizi di registrazione e memorizzazione nella cache con inserimento delle dipendenze tramite chiamate a [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="9c735-535">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="9c735-536">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-536">**New behavior**</span></span>

<span data-ttu-id="9c735-537">A partire da EF Core 3.0, `AddDbContext` e `AddDbContextPool` non registreranno più questi servizi con inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="9c735-537">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="9c735-538">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-538">**Why**</span></span>

<span data-ttu-id="9c735-539">EF Core 3.0 non richiede che questi servizi siano inclusi nel contenitore di inserimento delle dipendenze dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9c735-539">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="9c735-540">Tuttavia, se `ILoggerFactory` è registrato nel contenitore di inserimento delle dipendenze dell'applicazione, verrà ancora usato da EF Core.</span><span class="sxs-lookup"><span data-stu-id="9c735-540">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="9c735-541">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-541">**Mitigations**</span></span>

<span data-ttu-id="9c735-542">Se l'applicazione necessita di questi servizi, registrarli in modo esplicito con il contenitore di inserimento delle dipendenze usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="9c735-542">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="9c735-543">DbContext.Entry esegue ora un DetectChanges locale</span><span class="sxs-lookup"><span data-stu-id="9c735-543">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="9c735-544">Problema n. 13552</span><span class="sxs-lookup"><span data-stu-id="9c735-544">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="9c735-545">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-545">**Old behavior**</span></span>

<span data-ttu-id="9c735-546">Nelle versioni precedenti a EF Core 3.0 la chiamata a `DbContext.Entry` causava il rilevamento delle modifiche per tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="9c735-546">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="9c735-547">Ciò garantiva l'aggiornamento dello stato esposto in `EntityEntry`.</span><span class="sxs-lookup"><span data-stu-id="9c735-547">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="9c735-548">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-548">**New behavior**</span></span>

<span data-ttu-id="9c735-549">A partire da EF Core 3.0, la chiamata a `DbContext.Entry` causa ora solo il tentativo di rilevare le modifiche nell'entità specificata e in tutte le relative entità di sicurezza rilevate.</span><span class="sxs-lookup"><span data-stu-id="9c735-549">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="9c735-550">Ciò significa che le modifiche apportate altrove potrebbero non essere state rilevate tramite la chiamata al metodo e ciò potrebbe avere implicazioni sullo stato dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9c735-550">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="9c735-551">Si noti che se `ChangeTracker.AutoDetectChangesEnabled` è impostato su `false`, verrà disabilitato anche questo tipo di rilevamento delle modifiche locali.</span><span class="sxs-lookup"><span data-stu-id="9c735-551">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="9c735-552">Gli altri metodi che causano il rilevamento delle modifiche, ad esempio `ChangeTracker.Entries` e `SaveChanges`, causano ancora un `DetectChanges` completo di tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="9c735-552">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="9c735-553">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-553">**Why**</span></span>

<span data-ttu-id="9c735-554">Questa modifica è stata apportata per migliorare le prestazioni predefinite dell'uso di `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="9c735-554">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="9c735-555">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-555">**Mitigations**</span></span>

<span data-ttu-id="9c735-556">Chiamare `ChgangeTracker.DetectChanges()` in modo esplicito prima di chiamare `Entry` per garantire il comportamento precedente alla versione 3.0.</span><span class="sxs-lookup"><span data-stu-id="9c735-556">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="9c735-557">Le chiavi matrice di byte e di stringhe non vengono generate dal client per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="9c735-557">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="9c735-558">Problema n. 14617</span><span class="sxs-lookup"><span data-stu-id="9c735-558">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="9c735-559">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-559">**Old behavior**</span></span>

<span data-ttu-id="9c735-560">Nelle versioni precedenti a EF Core 3.0 le proprietà di chiave `string` e `byte[]` potevano essere usate senza impostare in modo esplicito un valore non Null.</span><span class="sxs-lookup"><span data-stu-id="9c735-560">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="9c735-561">In questi casi, il valore di chiave veniva generato nel client come GUID, serializzato in byte per `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="9c735-561">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="9c735-562">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-562">**New behavior**</span></span>

<span data-ttu-id="9c735-563">A partire da EF Core 3.0 viene generata un'eccezione che indica che non è stato impostato alcun valore di chiave.</span><span class="sxs-lookup"><span data-stu-id="9c735-563">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="9c735-564">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-564">**Why**</span></span>

<span data-ttu-id="9c735-565">Questa modifica è stata apportata poiché i valori `string`/`byte[]` generati dal client non risultano in genere utili e il comportamento predefinito rendeva complesso ragionare sui valori di chiave generati in un modo comune.</span><span class="sxs-lookup"><span data-stu-id="9c735-565">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="9c735-566">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-566">**Mitigations**</span></span>

<span data-ttu-id="9c735-567">È possibile ripristinare il comportamento precedente alla versione 3.0 specificando in modo esplicito che le proprietà di chiave devono usare i valori generati se non viene impostato alcun altro valore non Null.</span><span class="sxs-lookup"><span data-stu-id="9c735-567">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="9c735-568">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="9c735-568">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="9c735-569">Oppure con annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="9c735-569">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="9c735-570">ILoggerFactory è ora un servizio con ambito</span><span class="sxs-lookup"><span data-stu-id="9c735-570">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="9c735-571">Problema n. 14698</span><span class="sxs-lookup"><span data-stu-id="9c735-571">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="9c735-572">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-572">**Old behavior**</span></span>

<span data-ttu-id="9c735-573">Nelle versioni precedenti a EF Core 3.0 `ILoggerFactory` veniva registrato come servizio singleton.</span><span class="sxs-lookup"><span data-stu-id="9c735-573">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="9c735-574">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-574">**New behavior**</span></span>

<span data-ttu-id="9c735-575">A partire da EF Core 3.0, `ILoggerFactory` viene registrato come servizio con ambito.</span><span class="sxs-lookup"><span data-stu-id="9c735-575">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="9c735-576">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-576">**Why**</span></span>

<span data-ttu-id="9c735-577">Questa modifica è stata apportata per consentire l'associazione di un logger a un'istanza `DbContext` che abilita altre funzionalità e rimuove alcuni casi di comportamento anomalo, ad esempio un'esplosione dei provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="9c735-577">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="9c735-578">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-578">**Mitigations**</span></span>

<span data-ttu-id="9c735-579">Questa modifica non dovrebbe influire sul codice dell'applicazione a meno che non vengano registrati e usati servizi personalizzati nel provider di servizi interno di EF Core.</span><span class="sxs-lookup"><span data-stu-id="9c735-579">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="9c735-580">Questa condizione, tuttavia, non è comune.</span><span class="sxs-lookup"><span data-stu-id="9c735-580">This isn't common.</span></span>
<span data-ttu-id="9c735-581">In questi casi la maggior parte delle operazioni continuano a essere eseguite correttamente, ma qualsiasi servizio singleton dipendente da `ILoggerFactory` dovrà essere modificato per ottenere `ILoggerFactory` in modo diverso.</span><span class="sxs-lookup"><span data-stu-id="9c735-581">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="9c735-582">Se si verificano situazioni simili, inviare una segnalazione nello [strumento di gestione dei problemi in GitHub relativo a EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) per comunicare in che modo viene usato `ILoggerFactory` per consentirci di comprendere meglio come evitare ulteriori interruzioni in futuro.</span><span class="sxs-lookup"><span data-stu-id="9c735-582">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="9c735-583">I proxy di caricamento lazy non presuppongono più che le proprietà di navigazione vengano caricate completamente</span><span class="sxs-lookup"><span data-stu-id="9c735-583">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="9c735-584">Problema n. 12780</span><span class="sxs-lookup"><span data-stu-id="9c735-584">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="9c735-585">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-585">**Old behavior**</span></span>

<span data-ttu-id="9c735-586">Nelle versioni precedenti a EF Core 3.0, quando `DbContext` veniva eliminato non esisteva alcun metodo per scoprire se una determinata proprietà di navigazione in un'entità ottenuta da un contesto specifico veniva caricata completamente o meno.</span><span class="sxs-lookup"><span data-stu-id="9c735-586">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="9c735-587">I proxy presupponevano invece che venisse caricata una navigazione di riferimento se era presente un valore non Null e che venisse caricata una navigazione di raccolta se era presente un valore.</span><span class="sxs-lookup"><span data-stu-id="9c735-587">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="9c735-588">In questi casi il tentativo di eseguire un caricamento lazy non avrebbe avuto alcun esito.</span><span class="sxs-lookup"><span data-stu-id="9c735-588">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="9c735-589">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-589">**New behavior**</span></span>

<span data-ttu-id="9c735-590">A partire da Entity Framework Core 3.0, i proxy tengono traccia del caricamento o mancato caricamento di una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="9c735-590">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="9c735-591">Ciò significa che il tentativo di accedere a una proprietà di navigazione caricata dopo l'eliminazione del contesto non avrà mai alcun esito, anche quando la navigazione caricata è vuota o ha valore Null.</span><span class="sxs-lookup"><span data-stu-id="9c735-591">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="9c735-592">Al contrario, il tentativo di accedere a una proprietà di navigazione non caricata genera un'eccezione se il contesto viene eliminato anche se la proprietà di navigazione è una raccolta non vuota.</span><span class="sxs-lookup"><span data-stu-id="9c735-592">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="9c735-593">Se si verifica questa situazione significa che il codice dell'applicazione sta tentando di usare il caricamento lazy in un momento non valido e l'applicazione deve essere modificata in modo da non eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="9c735-593">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="9c735-594">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-594">**Why**</span></span>

<span data-ttu-id="9c735-595">Questa modifica è stata apportata per rendere coerente e corretto il comportamento durante un tentativo di caricamento lazy in un'istanza `DbContext` eliminata.</span><span class="sxs-lookup"><span data-stu-id="9c735-595">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="9c735-596">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-596">**Mitigations**</span></span>

<span data-ttu-id="9c735-597">Aggiornare il codice dell'applicazione per fare in modo che non venga tentato il caricamento lazy con un contesto eliminato oppure specificare una configurazione in modo che non venga eseguita alcuna operazione come descritto nel messaggio di eccezione.</span><span class="sxs-lookup"><span data-stu-id="9c735-597">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="9c735-598">La creazione di un numero eccessivo di provider di servizi interni è ora un errore per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="9c735-598">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="9c735-599">Problema n. 10236</span><span class="sxs-lookup"><span data-stu-id="9c735-599">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="9c735-600">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-600">**Old behavior**</span></span>

<span data-ttu-id="9c735-601">Nelle versioni precedenti a EF Core 3.0 veniva registrato un avviso per le applicazioni che creavano un numero eccessivo di provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="9c735-601">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="9c735-602">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-602">**New behavior**</span></span>

<span data-ttu-id="9c735-603">A partire da EF Core 3.0, l'avviso viene considerato un errore e viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="9c735-603">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="9c735-604">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-604">**Why**</span></span>

<span data-ttu-id="9c735-605">Questa modifica è stata apportata per gestire meglio il codice dell'applicazione tramite un'esposizione più esplicita di questa situazione di errore.</span><span class="sxs-lookup"><span data-stu-id="9c735-605">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="9c735-606">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-606">**Mitigations**</span></span>

<span data-ttu-id="9c735-607">L'azione più appropriata quando si verifica questo errore consiste nell'individuare la causa radice e nell'interrompere la creazione di numerosi provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="9c735-607">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="9c735-608">È possibile tuttavia convertire nuovamente l'errore in avviso o ignorarlo tramite una configurazione in `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9c735-608">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="9c735-609">Esempio:</span><span class="sxs-lookup"><span data-stu-id="9c735-609">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="9c735-610">Nuovo comportamento per la chiamata di HasOne/HasMany con una singola stringa</span><span class="sxs-lookup"><span data-stu-id="9c735-610">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="9c735-611">Problema n. 9171</span><span class="sxs-lookup"><span data-stu-id="9c735-611">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="9c735-612">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-612">**Old behavior**</span></span>

<span data-ttu-id="9c735-613">Prima di EF Core 3.0, il codice che chiama `HasOne` o `HasMany` con una singola stringa era interpretato in modo poco chiaro.</span><span class="sxs-lookup"><span data-stu-id="9c735-613">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="9c735-614">Esempio:</span><span class="sxs-lookup"><span data-stu-id="9c735-614">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="9c735-615">Apparentemente, il codice mette in relazione `Samurai` con un altro tipo di entità tramite la proprietà di navigazione `Entrance`, che può essere privata.</span><span class="sxs-lookup"><span data-stu-id="9c735-615">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="9c735-616">In realtà, il codice tenta di creare una relazione con un tipo di entità denominato `Entrance` senza proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="9c735-616">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="9c735-617">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-617">**New behavior**</span></span>

<span data-ttu-id="9c735-618">A partire da EF Core 3.0, il codice sopra riportato ora esegue quello che avrebbe dovuto fare in precedenza.</span><span class="sxs-lookup"><span data-stu-id="9c735-618">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="9c735-619">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-619">**Why**</span></span>

<span data-ttu-id="9c735-620">Il comportamento precedente era molto poco chiaro, soprattutto durante la lettura del codice di configurazione e la ricerca di errori.</span><span class="sxs-lookup"><span data-stu-id="9c735-620">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="9c735-621">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-621">**Mitigations**</span></span>

<span data-ttu-id="9c735-622">Questa modifica causerà problemi solo nelle applicazioni che configurano relazioni in modo esplicito usando stringhe per i nomi dei tipi e senza specificare in modo esplicito la proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="9c735-622">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="9c735-623">Non è uno scenario comune.</span><span class="sxs-lookup"><span data-stu-id="9c735-623">This is not common.</span></span>
<span data-ttu-id="9c735-624">Il comportamento precedente può essere ottenuto passando esplicitamente `null` per il nome della proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="9c735-624">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="9c735-625">Esempio:</span><span class="sxs-lookup"><span data-stu-id="9c735-625">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="9c735-626">Il tipo restituito per diversi metodi asincroni è cambiato da Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="9c735-626">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="9c735-627">Problema n. 15184</span><span class="sxs-lookup"><span data-stu-id="9c735-627">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="9c735-628">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-628">**Old behavior**</span></span>

<span data-ttu-id="9c735-629">I metodi asincroni seguenti in precedenza restituivano il tipo `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="9c735-629">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="9c735-630">`ValueGenerator.NextValueAsync()` (e classi derivate)</span><span class="sxs-lookup"><span data-stu-id="9c735-630">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="9c735-631">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-631">**New behavior**</span></span>

<span data-ttu-id="9c735-632">I metodi indicati in precedenza ora restituiscono il tipo `ValueTask<T>` sullo stesso `T` come in precedenza.</span><span class="sxs-lookup"><span data-stu-id="9c735-632">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="9c735-633">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-633">**Why**</span></span>

<span data-ttu-id="9c735-634">Questa modifica riduce il numero delle allocazioni di heap sostenute quando si richiamano questi metodi, con un miglioramento generale delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="9c735-634">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="9c735-635">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-635">**Mitigations**</span></span>

<span data-ttu-id="9c735-636">Le applicazioni semplicemente in attesa delle API precedenti devono solo essere ricompilate e non sono richieste modifiche del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="9c735-636">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="9c735-637">Per scenari di utilizzo più complessi (ad esempio, il passaggio del tipo `Task` restituito a `Task.WhenAny()`) è richiesto in genere che il tipo `ValueTask<T>` restituito venga convertito in `Task<T>` chiamando `AsTask()` su di esso.</span><span class="sxs-lookup"><span data-stu-id="9c735-637">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="9c735-638">Si noti che in questo modo si annulla la riduzione delle allocazioni consentita da questa modifica.</span><span class="sxs-lookup"><span data-stu-id="9c735-638">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="9c735-639">L'annotazione Relational:TypeMapping è ora TypeMapping</span><span class="sxs-lookup"><span data-stu-id="9c735-639">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="9c735-640">Problema n. 9913</span><span class="sxs-lookup"><span data-stu-id="9c735-640">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="9c735-641">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-641">**Old behavior**</span></span>

<span data-ttu-id="9c735-642">Il nome di annotazione delle annotazioni di mapping del tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="9c735-642">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="9c735-643">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-643">**New behavior**</span></span>

<span data-ttu-id="9c735-644">Il nome di annotazione delle annotazioni di mapping del tipo è ora "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="9c735-644">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="9c735-645">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-645">**Why**</span></span>

<span data-ttu-id="9c735-646">Il mapping dei tipi non viene più usato solo per i provider di database relazionali.</span><span class="sxs-lookup"><span data-stu-id="9c735-646">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="9c735-647">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-647">**Mitigations**</span></span>

<span data-ttu-id="9c735-648">Ciò causa un'interruzione solo nelle applicazioni che accedono al mapping dei tipi direttamente come annotazione. Questa situazione non è comune.</span><span class="sxs-lookup"><span data-stu-id="9c735-648">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="9c735-649">L'azione più appropriata per risolvere il problema consiste nell'usare la superficie dell'API per accedere al mapping dei tipi anziché l'annotazione.</span><span class="sxs-lookup"><span data-stu-id="9c735-649">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="9c735-650">ToTable in un tipo derivato genera un'eccezione</span><span class="sxs-lookup"><span data-stu-id="9c735-650">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="9c735-651">Problema n. 11811</span><span class="sxs-lookup"><span data-stu-id="9c735-651">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="9c735-652">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-652">**Old behavior**</span></span>

<span data-ttu-id="9c735-653">Nelle versioni precedenti a EF Core 3.0, la chiamata a `ToTable()` in un tipo derivato veniva ignorata poiché soltanto la strategia di mapping dell'ereditarietà era una tabella per gerarchia dove la chiamata non era valida.</span><span class="sxs-lookup"><span data-stu-id="9c735-653">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="9c735-654">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-654">**New behavior**</span></span>

<span data-ttu-id="9c735-655">A partire da EF Core 3.0 e in preparazione all'aggiunta del supporto per la tabella per tipo e per TPC in una versione successiva, la chiamata a `ToTable()` in un tipo derivato genera un'eccezione per evitare una modifica del mapping imprevista in futuro.</span><span class="sxs-lookup"><span data-stu-id="9c735-655">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="9c735-656">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-656">**Why**</span></span>

<span data-ttu-id="9c735-657">Attualmente il mapping di un tipo derivato in una tabella diversa non è un'operazione valida.</span><span class="sxs-lookup"><span data-stu-id="9c735-657">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="9c735-658">Questa modifica consente di evitare interruzioni future quando l'operazione diventerà un'operazione valida.</span><span class="sxs-lookup"><span data-stu-id="9c735-658">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="9c735-659">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-659">**Mitigations**</span></span>

<span data-ttu-id="9c735-660">Rimuovere qualsiasi tentativo di mapping di tipi derivati in altre tabelle.</span><span class="sxs-lookup"><span data-stu-id="9c735-660">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="9c735-661">ForSqlServerHasIndex sostituito con HasIndex</span><span class="sxs-lookup"><span data-stu-id="9c735-661">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="9c735-662">Problema n. 12366</span><span class="sxs-lookup"><span data-stu-id="9c735-662">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="9c735-663">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-663">**Old behavior**</span></span>

<span data-ttu-id="9c735-664">Nelle versioni precedenti a EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offriva un metodo per configurare le colonne usate con `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="9c735-664">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="9c735-665">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-665">**New behavior**</span></span>

<span data-ttu-id="9c735-666">A partire da EF Core 3.0, l'uso di `Include` in un indice è supportato a livello relazionale.</span><span class="sxs-lookup"><span data-stu-id="9c735-666">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="9c735-667">Usare `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="9c735-667">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="9c735-668">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-668">**Why**</span></span>

<span data-ttu-id="9c735-669">Questa modifica è stata apportata per consolidare l'API per gli indici con `Include` in un'unica posizione per tutti i provider di database.</span><span class="sxs-lookup"><span data-stu-id="9c735-669">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="9c735-670">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-670">**Mitigations**</span></span>

<span data-ttu-id="9c735-671">Usare la nuova API, come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="9c735-671">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="9c735-672">Modifiche dell'API dei metadati</span><span class="sxs-lookup"><span data-stu-id="9c735-672">Metadata API changes</span></span>

[<span data-ttu-id="9c735-673">Problema n. 214</span><span class="sxs-lookup"><span data-stu-id="9c735-673">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="9c735-674">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-674">**New behavior**</span></span>

<span data-ttu-id="9c735-675">Le proprietà seguenti sono state convertite in metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="9c735-675">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="9c735-676">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-676">**Why**</span></span>

<span data-ttu-id="9c735-677">Questa modifica semplifica l'implementazione delle interfacce menzionate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="9c735-677">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="9c735-678">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-678">**Mitigations**</span></span>

<span data-ttu-id="9c735-679">Usare i nuovi metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="9c735-679">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="9c735-680">Modifiche dell'API dei metadati specifiche del provider</span><span class="sxs-lookup"><span data-stu-id="9c735-680">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="9c735-681">Problema n. 214</span><span class="sxs-lookup"><span data-stu-id="9c735-681">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="9c735-682">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-682">**New behavior**</span></span>

<span data-ttu-id="9c735-683">I metodi di estensione specifici del provider verranno resi flat:</span><span class="sxs-lookup"><span data-stu-id="9c735-683">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="9c735-684">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-684">**Why**</span></span>

<span data-ttu-id="9c735-685">Questa modifica semplifica l'implementazione dei metodi di estensione menzionati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="9c735-685">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="9c735-686">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-686">**Mitigations**</span></span>

<span data-ttu-id="9c735-687">Usare i nuovi metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="9c735-687">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="9c735-688">EF Core non invia più pragma per l'imposizione della chiave esterna di SQLite</span><span class="sxs-lookup"><span data-stu-id="9c735-688">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="9c735-689">Problema n. 12151</span><span class="sxs-lookup"><span data-stu-id="9c735-689">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="9c735-690">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-690">**Old behavior**</span></span>

<span data-ttu-id="9c735-691">Nelle versioni precedenti a EF Core 3.0, EF Core inviava `PRAGMA foreign_keys = 1` quando veniva aperta una connessione a SQLite.</span><span class="sxs-lookup"><span data-stu-id="9c735-691">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="9c735-692">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-692">**New behavior**</span></span>

<span data-ttu-id="9c735-693">A partire da EF Core 3.0, EF Core non invia più `PRAGMA foreign_keys = 1` quando viene aperta una connessione a SQLite.</span><span class="sxs-lookup"><span data-stu-id="9c735-693">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="9c735-694">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-694">**Why**</span></span>

<span data-ttu-id="9c735-695">Questa modifica è stata apportata poiché EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` per impostazione predefinita. Ciò significa che l'imposizione della chiave esterna è abilitata per impostazione predefinita e non deve essere abilitata in modo esplicito ogni volta che viene aperta una connessione.</span><span class="sxs-lookup"><span data-stu-id="9c735-695">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="9c735-696">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-696">**Mitigations**</span></span>

<span data-ttu-id="9c735-697">Le chiavi esterne sono abilitate per impostazione predefinita in SQLitePCLRaw.bundle_e_sqlite3, usato per impostazione predefinita per EF Core.</span><span class="sxs-lookup"><span data-stu-id="9c735-697">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="9c735-698">Per gli altri casi, è possibile abilitare le chiavi esterne specificando `Foreign Keys=True` nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="9c735-698">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="9c735-699">Microsoft.EntityFrameworkCore.Sqlite dipende ora da SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="9c735-699">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="9c735-700">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-700">**Old behavior**</span></span>

<span data-ttu-id="9c735-701">Nelle versioni precedenti a EF Core 3.0, EF Core usava `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="9c735-701">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="9c735-702">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-702">**New behavior**</span></span>

<span data-ttu-id="9c735-703">A partire da EF Core 3.0, EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="9c735-703">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="9c735-704">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-704">**Why**</span></span>

<span data-ttu-id="9c735-705">Questa modifica è stata apportata per rendere coerente la versione di SQLite usata in iOS con le altre piattaforme.</span><span class="sxs-lookup"><span data-stu-id="9c735-705">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="9c735-706">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-706">**Mitigations**</span></span>

<span data-ttu-id="9c735-707">Per usare la versione di SQLite nativa in iOS, configurare `Microsoft.Data.Sqlite` per l'uso di un'aggregazione `SQLitePCLRaw` diversa.</span><span class="sxs-lookup"><span data-stu-id="9c735-707">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="9c735-708">I valori Guid vengono ora archiviati come TEXT in SQLite</span><span class="sxs-lookup"><span data-stu-id="9c735-708">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="9c735-709">Problema n. 15078</span><span class="sxs-lookup"><span data-stu-id="9c735-709">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="9c735-710">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-710">**Old behavior**</span></span>

<span data-ttu-id="9c735-711">I valori Guid in precedenza venivano archiviati come valori BLOB in SQLite.</span><span class="sxs-lookup"><span data-stu-id="9c735-711">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="9c735-712">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-712">**New behavior**</span></span>

<span data-ttu-id="9c735-713">I valori Guid vengono ora archiviati come TEXT.</span><span class="sxs-lookup"><span data-stu-id="9c735-713">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="9c735-714">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-714">**Why**</span></span>

<span data-ttu-id="9c735-715">Il formato binario dei valori Guid non è standardizzato.</span><span class="sxs-lookup"><span data-stu-id="9c735-715">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="9c735-716">L'archiviazione dei valori come TEXT rende il database più compatibile con altre tecnologie.</span><span class="sxs-lookup"><span data-stu-id="9c735-716">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="9c735-717">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-717">**Mitigations**</span></span>

<span data-ttu-id="9c735-718">È possibile eseguire la migrazione dei database esistenti al nuovo formato eseguendo SQL nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="9c735-718">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="9c735-719">In EF Core è anche possibile continuare a usare il comportamento precedente configurando un convertitore di valori per queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="9c735-719">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="9c735-720">Microsoft.Data.Sqlite rimane in grado di leggere i valori Guid sia da colonne BLOB che TEXT. Tuttavia, poiché è stato modificato il formato predefinito per i parametri e le costanti, probabilmente sarà necessario intervenire per la maggior parte degli scenari che coinvolgono valori Guid.</span><span class="sxs-lookup"><span data-stu-id="9c735-720">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="9c735-721">I valori char vengono ora archiviati come testo in SQLite</span><span class="sxs-lookup"><span data-stu-id="9c735-721">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="9c735-722">Problema n. 15020</span><span class="sxs-lookup"><span data-stu-id="9c735-722">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="9c735-723">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-723">**Old behavior**</span></span>

<span data-ttu-id="9c735-724">I valori char in precedenza venivano archiviati come valori interi in SQLite.</span><span class="sxs-lookup"><span data-stu-id="9c735-724">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="9c735-725">Un valore char di *A* veniva ad esempio archiviato come valore intero 65.</span><span class="sxs-lookup"><span data-stu-id="9c735-725">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="9c735-726">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-726">**New behavior**</span></span>

<span data-ttu-id="9c735-727">I valori char vengono ora archiviati come testo.</span><span class="sxs-lookup"><span data-stu-id="9c735-727">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="9c735-728">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-728">**Why**</span></span>

<span data-ttu-id="9c735-729">L'archiviazione dei valori come testo è un'operazione più naturale e rende il database più compatibile con altre tecnologie.</span><span class="sxs-lookup"><span data-stu-id="9c735-729">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="9c735-730">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-730">**Mitigations**</span></span>

<span data-ttu-id="9c735-731">È possibile eseguire la migrazione dei database esistenti al nuovo formato eseguendo SQL nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="9c735-731">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="9c735-732">In EF Core è anche possibile continuare a usare il comportamento precedente configurando un convertitore di valori per queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="9c735-732">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="9c735-733">Microsoft.Data.Sqlite rimane comunque in grado di leggere i valori di caratteri presenti sia nelle colonne di valori interi sia in quelle di testo, quindi alcuni scenari potrebbero non richiedere alcuna azione.</span><span class="sxs-lookup"><span data-stu-id="9c735-733">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="9c735-734">Gli ID di migrazione vengono ora generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica</span><span class="sxs-lookup"><span data-stu-id="9c735-734">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="9c735-735">Problema n. 12978</span><span class="sxs-lookup"><span data-stu-id="9c735-735">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="9c735-736">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-736">**Old behavior**</span></span>

<span data-ttu-id="9c735-737">Gli ID di migrazione venivano inavvertitamente generati usando il calendario delle impostazioni cultura correnti.</span><span class="sxs-lookup"><span data-stu-id="9c735-737">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="9c735-738">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-738">**New behavior**</span></span>

<span data-ttu-id="9c735-739">Gli ID di migrazione ora vengono sempre generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica (calendario gregoriano).</span><span class="sxs-lookup"><span data-stu-id="9c735-739">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="9c735-740">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-740">**Why**</span></span>

<span data-ttu-id="9c735-741">L'ordine delle migrazioni è importante quando si esegue l'aggiornamento del database o si risolvono i conflitti di unione.</span><span class="sxs-lookup"><span data-stu-id="9c735-741">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="9c735-742">L'uso del calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica evita i problemi che possono verificarsi quando i membri del team hanno calendari di sistema diversi.</span><span class="sxs-lookup"><span data-stu-id="9c735-742">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="9c735-743">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-743">**Mitigations**</span></span>

<span data-ttu-id="9c735-744">Questa modifica interessa gli utenti che usano un calendario non gregoriano in cui l'anno ha un'estensione superiore al calendario gregoriano (come il calendario buddista tailandese).</span><span class="sxs-lookup"><span data-stu-id="9c735-744">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="9c735-745">Gli ID di migrazione esistenti dovranno essere aggiornati in modo che le nuove migrazioni vengano collocate dopo le migrazioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="9c735-745">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="9c735-746">L'ID di migrazione è disponibile nell'attributo di migrazione presente nei file di progettazione delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="9c735-746">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="9c735-747">È necessario aggiornare anche la tabella della cronologia delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="9c735-747">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="9c735-748">Il metodo UseRowNumberForPaging è stato rimosso</span><span class="sxs-lookup"><span data-stu-id="9c735-748">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="9c735-749">Problema n. 16400</span><span class="sxs-lookup"><span data-stu-id="9c735-749">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="9c735-750">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-750">**Old behavior**</span></span>

<span data-ttu-id="9c735-751">Prima di EF Core 3.0 si poteva usare `UseRowNumberForPaging` per generare codice SQL per la suddivisione in pagine compatibile con SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="9c735-751">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="9c735-752">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-752">**New behavior**</span></span>

<span data-ttu-id="9c735-753">A partire da EF Core 3.0, EF genererà solo codice SQL per la suddivisione in pagine compatibile solo con versioni successive di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9c735-753">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="9c735-754">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-754">**Why**</span></span>

<span data-ttu-id="9c735-755">Questa modifica è stata apportata perché [SQL Server 2008 non è più un prodotto supportato](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) e l'aggiornamento di questa funzionalità per interagire con le modifiche apportate per le query in EF Core 3.0 è un lavoro significativo.</span><span class="sxs-lookup"><span data-stu-id="9c735-755">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="9c735-756">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-756">**Mitigations**</span></span>

<span data-ttu-id="9c735-757">È consigliabile eseguire l'aggiornamento a una versione più recente di SQL Server o usare un livello di compatibilità superiore, in modo che il codice SQL generato sia supportato.</span><span class="sxs-lookup"><span data-stu-id="9c735-757">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="9c735-758">Detto questo, se non è possibile procedere in questo modo, [aggiungere un commento per il problema](https://github.com/aspnet/EntityFrameworkCore/issues/16400) con indicazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="9c735-758">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="9c735-759">Microsoft potrebbe rivedere questa decisione in base ai commenti e suggerimenti.</span><span class="sxs-lookup"><span data-stu-id="9c735-759">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="9c735-760">Info/metadati dell'estensione rimossi da IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="9c735-760">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="9c735-761">Problema n. 16119</span><span class="sxs-lookup"><span data-stu-id="9c735-761">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="9c735-762">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-762">**Old behavior**</span></span>

<span data-ttu-id="9c735-763">`IDbContextOptionsExtension` conteneva metodi per fornire i metadati relativi all'estensione.</span><span class="sxs-lookup"><span data-stu-id="9c735-763">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="9c735-764">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-764">**New behavior**</span></span>

<span data-ttu-id="9c735-765">Questi metodi sono stati spostati in una nuova classe di base astratta `DbContextOptionsExtensionInfo`, restituita da una nuova proprietà `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="9c735-765">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="9c735-766">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-766">**Why**</span></span>

<span data-ttu-id="9c735-767">Nelle versioni dalla 2.0 alla 3.0 è stato necessario aggiungere o modificare questi metodi più volte.</span><span class="sxs-lookup"><span data-stu-id="9c735-767">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="9c735-768">Suddividendoli in una nuova classe di base astratta sarà più facile apportare questo tipo di modifiche senza compromettere il funzionamento delle estensioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="9c735-768">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="9c735-769">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-769">**Mitigations**</span></span>

<span data-ttu-id="9c735-770">Aggiornare le estensioni per seguire il nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="9c735-770">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="9c735-771">Sono disponibili esempi nelle numerose implementazioni di `IDbContextOptionsExtension` per diversi tipi di estensioni nel codice sorgente di EF Core.</span><span class="sxs-lookup"><span data-stu-id="9c735-771">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="9c735-772">LogQueryPossibleExceptionWithAggregateOperator è stato rinominato</span><span class="sxs-lookup"><span data-stu-id="9c735-772">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="9c735-773">Problema n. 10985</span><span class="sxs-lookup"><span data-stu-id="9c735-773">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="9c735-774">**Modifica**</span><span class="sxs-lookup"><span data-stu-id="9c735-774">**Change**</span></span>

<span data-ttu-id="9c735-775">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` è stato rinominato in `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="9c735-775">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="9c735-776">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-776">**Why**</span></span>

<span data-ttu-id="9c735-777">Allineamento del nome di questo evento di avviso con tutti gli altri eventi di avviso.</span><span class="sxs-lookup"><span data-stu-id="9c735-777">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="9c735-778">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-778">**Mitigations**</span></span>

<span data-ttu-id="9c735-779">Usare il nuovo nome.</span><span class="sxs-lookup"><span data-stu-id="9c735-779">Use the new name.</span></span> <span data-ttu-id="9c735-780">(Si noti che il numero di ID evento non è stato modificato.)</span><span class="sxs-lookup"><span data-stu-id="9c735-780">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="9c735-781">Chiarimenti per l'API per i nomi di vincolo di chiave esterna</span><span class="sxs-lookup"><span data-stu-id="9c735-781">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="9c735-782">Problema n. 10730</span><span class="sxs-lookup"><span data-stu-id="9c735-782">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="9c735-783">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-783">**Old behavior**</span></span>

<span data-ttu-id="9c735-784">Prima di EF Core 3.0, si faceva riferimento ai nomi di vincolo di chiave esterna semplicemente con "Name".</span><span class="sxs-lookup"><span data-stu-id="9c735-784">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="9c735-785">Esempio:</span><span class="sxs-lookup"><span data-stu-id="9c735-785">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="9c735-786">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-786">**New behavior**</span></span>

<span data-ttu-id="9c735-787">A partire da EF Core 3.0, si fa ora riferimento ai nomi di vincolo di chiave esterna con "ConstraintName".</span><span class="sxs-lookup"><span data-stu-id="9c735-787">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="9c735-788">Esempio:</span><span class="sxs-lookup"><span data-stu-id="9c735-788">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="9c735-789">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-789">**Why**</span></span>

<span data-ttu-id="9c735-790">Questa modifica introduce coerenza per la denominazione in quest'area e chiarisce anche che si tratta del nome del vincolo di chiave esterna e non del nome della colonna o della proprietà per cui è definita la chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="9c735-790">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="9c735-791">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-791">**Mitigations**</span></span>

<span data-ttu-id="9c735-792">Usare il nuovo nome.</span><span class="sxs-lookup"><span data-stu-id="9c735-792">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="9c735-793">IRelationalDatabaseCreator.HasTables/HasTablesAsync sono diventati pubblici</span><span class="sxs-lookup"><span data-stu-id="9c735-793">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="9c735-794">Problema n. 15997</span><span class="sxs-lookup"><span data-stu-id="9c735-794">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="9c735-795">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-795">**Old behavior**</span></span>

<span data-ttu-id="9c735-796">Prima di EF Core 3.0 questi metodi erano protetti.</span><span class="sxs-lookup"><span data-stu-id="9c735-796">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="9c735-797">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-797">**New behavior**</span></span>

<span data-ttu-id="9c735-798">A partire da EF Core 3.0 questi metodi sono pubblici.</span><span class="sxs-lookup"><span data-stu-id="9c735-798">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="9c735-799">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-799">**Why**</span></span>

<span data-ttu-id="9c735-800">Questi metodi vengono usati da EF per determinare se un database viene creato, ma vuoto.</span><span class="sxs-lookup"><span data-stu-id="9c735-800">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="9c735-801">Ciò risulta utile all'esterno di EF quando occorre determinare se applicare o meno le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="9c735-801">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="9c735-802">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-802">**Mitigations**</span></span>

<span data-ttu-id="9c735-803">Modificare l'accessibilità di eventuali override.</span><span class="sxs-lookup"><span data-stu-id="9c735-803">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="9c735-804">Microsoft.EntityFrameworkCore.Design è ora un pacchetto DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="9c735-804">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="9c735-805">Problema n. 11506</span><span class="sxs-lookup"><span data-stu-id="9c735-805">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="9c735-806">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-806">**Old behavior**</span></span>

<span data-ttu-id="9c735-807">Prima di EF Core 3.0, Microsoft.EntityFrameworkCore.Design era un pacchetto NuGet normale ed era possibile fare riferimento al relativo assembly dai progetti dipendenti.</span><span class="sxs-lookup"><span data-stu-id="9c735-807">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="9c735-808">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-808">**New behavior**</span></span>

<span data-ttu-id="9c735-809">A partire da EF Core 3.0 è un pacchetto DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="9c735-809">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="9c735-810">Questo significa che la dipendenza non verrà trasferita in modo transitivo in altri progetti e che non è più possibile fare riferimento all'assembly per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="9c735-810">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="9c735-811">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-811">**Why**</span></span>

<span data-ttu-id="9c735-812">Questo pacchetto è destinato solo all'uso in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="9c735-812">This package is only intended to be used at design time.</span></span> <span data-ttu-id="9c735-813">Le applicazioni distribuite non devono farvi riferimento.</span><span class="sxs-lookup"><span data-stu-id="9c735-813">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="9c735-814">Questa raccomandazione è rafforzata dall'impostazione del pacchetto come DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="9c735-814">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="9c735-815">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-815">**Mitigations**</span></span>

<span data-ttu-id="9c735-816">Se è necessario fare riferimento a questo pacchetto per eseguire l'override del comportamento in fase di progettazione di EF Core, è possibile aggiornare i metadati dell'elemento PackageReference nel progetto.</span><span class="sxs-lookup"><span data-stu-id="9c735-816">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="9c735-817">In presenza di riferimenti transitivi al pacchetto tramite Microsoft.EntityFrameworkCore.Tools, sarà necessario aggiungere un PackageReference esplicito al pacchetto per modificare i relativi metadati.</span><span class="sxs-lookup"><span data-stu-id="9c735-817">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="9c735-818">Aggiornamento di SQLitePCL.raw alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="9c735-818">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="9c735-819">Problema n. 14824</span><span class="sxs-lookup"><span data-stu-id="9c735-819">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="9c735-820">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-820">**Old behavior**</span></span>

<span data-ttu-id="9c735-821">Microsoft.EntityFrameworkCore.Sqlite dipendeva in precedenza dalla versione 1.1.12 di SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="9c735-821">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="9c735-822">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-822">**New behavior**</span></span>

<span data-ttu-id="9c735-823">Il pacchetto è stato aggiornato in base alla versione 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="9c735-823">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="9c735-824">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-824">**Why**</span></span>

<span data-ttu-id="9c735-825">La versione 2.0.0 di SQLitePCL.raw è destinata a .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="9c735-825">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="9c735-826">Era in precedenza destinata a .NET Standard 1.1 e ciò richiedeva un notevole impegno di chiusura di pacchetti transitivi per il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="9c735-826">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="9c735-827">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-827">**Mitigations**</span></span>

<span data-ttu-id="9c735-828">SQLitePCL.raw versione 2.0.0 include alcune modifiche che causano un'interruzione.</span><span class="sxs-lookup"><span data-stu-id="9c735-828">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="9c735-829">Per informazioni dettagliate, vedere le [note sulla versione](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="9c735-829">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="9c735-830">NetTopologySuite aggiornato alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="9c735-830">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="9c735-831">Problema n. 14825</span><span class="sxs-lookup"><span data-stu-id="9c735-831">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="9c735-832">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-832">**Old behavior**</span></span>

<span data-ttu-id="9c735-833">I pacchetti spaziali dipendevano in precedenza dalla versione 1.15.1 di NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="9c735-833">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="9c735-834">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-834">**New behavior**</span></span>

<span data-ttu-id="9c735-835">Il pacchetto è stato aggiornato in modo da dipendere dalla versione 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="9c735-835">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="9c735-836">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-836">**Why**</span></span>

<span data-ttu-id="9c735-837">La versione 2.0.0 di NetTopologySuite risolve vari problemi di usabilità riscontrati dagli utenti di EF Core.</span><span class="sxs-lookup"><span data-stu-id="9c735-837">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="9c735-838">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-838">**Mitigations**</span></span>

<span data-ttu-id="9c735-839">NetTopologySuite versione 2.0.0 include alcune modifiche che causano un'interruzione.</span><span class="sxs-lookup"><span data-stu-id="9c735-839">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="9c735-840">Per informazioni dettagliate, vedere le [note sulla versione](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="9c735-840">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="9c735-841">Viene usato Microsoft. Data. SqlClient al posto di System. Data. SqlClient</span><span class="sxs-lookup"><span data-stu-id="9c735-841">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="9c735-842">Rilevamento del problema #15636</span><span class="sxs-lookup"><span data-stu-id="9c735-842">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="9c735-843">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-843">**Old behavior**</span></span>

<span data-ttu-id="9c735-844">Microsoft. EntityFrameworkCore. SqlServer in precedenza dipende da System. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="9c735-844">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="9c735-845">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-845">**New behavior**</span></span>

<span data-ttu-id="9c735-846">Il pacchetto è stato aggiornato in base a Microsoft. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="9c735-846">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="9c735-847">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-847">**Why**</span></span>

<span data-ttu-id="9c735-848">Microsoft. Data. SqlClient è il driver di accesso ai dati principale per SQL Server in futuro e System. Data. SqlClient non è più l'obiettivo dello sviluppo.</span><span class="sxs-lookup"><span data-stu-id="9c735-848">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="9c735-849">Alcune funzionalità importanti, ad esempio Always Encrypted, sono disponibili solo in Microsoft. Data. SqlClient.</span><span class="sxs-lookup"><span data-stu-id="9c735-849">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="9c735-850">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-850">**Mitigations**</span></span>

<span data-ttu-id="9c735-851">Se il codice prende una dipendenza diretta da System. Data. SqlClient, è necessario modificarlo in modo che faccia riferimento a Microsoft. Data. SqlClient. Poiché i due pacchetti gestiscono un livello molto elevato di compatibilità API, questo dovrebbe essere solo una semplice modifica del pacchetto e dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="9c735-851">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="9c735-852">Devono essere configurare più relazioni ambigue che fanno riferimento a se stesse</span><span class="sxs-lookup"><span data-stu-id="9c735-852">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="9c735-853">Problema n. 13573</span><span class="sxs-lookup"><span data-stu-id="9c735-853">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="9c735-854">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-854">**Old behavior**</span></span>

<span data-ttu-id="9c735-855">Un tipo di entità con più proprietà di navigazione unidirezionale che fanno riferimento a se stesse e più chiavi esterne corrispondenti è stato erroneamente configurato come relazione singola.</span><span class="sxs-lookup"><span data-stu-id="9c735-855">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="9c735-856">Esempio:</span><span class="sxs-lookup"><span data-stu-id="9c735-856">For example:</span></span>

```C#
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

<span data-ttu-id="9c735-857">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-857">**New behavior**</span></span>

<span data-ttu-id="9c735-858">Questo scenario viene ora rilevato nella compilazione del modello e viene generata un'eccezione indicante che il modello è ambiguo.</span><span class="sxs-lookup"><span data-stu-id="9c735-858">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="9c735-859">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-859">**Why**</span></span>

<span data-ttu-id="9c735-860">Il modello risultante era ambiguo e sarà probabilmente errato in questo caso.</span><span class="sxs-lookup"><span data-stu-id="9c735-860">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="9c735-861">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-861">**Mitigations**</span></span>

<span data-ttu-id="9c735-862">Usare la configurazione completa della relazione.</span><span class="sxs-lookup"><span data-stu-id="9c735-862">Use full configuration of the relationship.</span></span> <span data-ttu-id="9c735-863">Esempio:</span><span class="sxs-lookup"><span data-stu-id="9c735-863">For example:</span></span>

```C#
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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="9c735-864">DbFunction. Schema è una stringa null o vuota che lo configura in modo che sia nello schema predefinito del modello</span><span class="sxs-lookup"><span data-stu-id="9c735-864">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="9c735-865">Rilevamento del problema #12757</span><span class="sxs-lookup"><span data-stu-id="9c735-865">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="9c735-866">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="9c735-866">**Old behavior**</span></span>

<span data-ttu-id="9c735-867">Un DbFunction configurato con schema come stringa vuota è stato trattato come funzione predefinita senza uno schema.</span><span class="sxs-lookup"><span data-stu-id="9c735-867">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="9c735-868">Il codice seguente, ad esempio, eseguirà il mapping della funzione CLR `DatePart` alla funzione predefinita `DATEPART` in SqlServer.</span><span class="sxs-lookup"><span data-stu-id="9c735-868">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="9c735-869">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="9c735-869">**New behavior**</span></span>

<span data-ttu-id="9c735-870">Tutti i mapping di DbFunction sono considerati mappati alle funzioni definite dall'utente.</span><span class="sxs-lookup"><span data-stu-id="9c735-870">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="9c735-871">Pertanto, il valore stringa vuoto inserisce la funzione all'interno dello schema predefinito per il modello.</span><span class="sxs-lookup"><span data-stu-id="9c735-871">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="9c735-872">Che potrebbe essere lo schema configurato in modo esplicito tramite l'API Fluent `modelBuilder.HasDefaultSchema()` o `dbo` in caso contrario.</span><span class="sxs-lookup"><span data-stu-id="9c735-872">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="9c735-873">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="9c735-873">**Why**</span></span>

<span data-ttu-id="9c735-874">Lo schema precedentemente vuoto era un modo per trattare la funzione è incorporata, ma tale logica è applicabile solo per SqlServer, in cui le funzioni predefinite non appartengono ad alcuno schema.</span><span class="sxs-lookup"><span data-stu-id="9c735-874">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="9c735-875">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="9c735-875">**Mitigations**</span></span>

<span data-ttu-id="9c735-876">Configurare manualmente la conversione di DbFunction per eseguirne il mapping a una funzione predefinita.</span><span class="sxs-lookup"><span data-stu-id="9c735-876">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
