---
title: Modifiche che causano un'interruzione in EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 690c7828cfe5019f4e7ae904c92430fab4726cb9
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2019
ms.locfileid: "72446010"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="3ebc6-102">Modifiche di rilievo incluse nel EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="3ebc6-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="3ebc6-103">Le modifiche alle API e al comportamento seguenti possono causare l'interruzione delle applicazioni esistenti durante l'aggiornamento a 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="3ebc6-104">Le modifiche che si prevede abbiano impatto solo sui provider di database sono documentate nelle [modifiche che influiscono sul provider](xref:core/providers/provider-log).</span><span class="sxs-lookup"><span data-stu-id="3ebc6-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="3ebc6-105">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="3ebc6-105">Summary</span></span>

| <span data-ttu-id="3ebc6-106">**Modifica di rilievo**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="3ebc6-107">**Impatto**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="3ebc6-108">Le query LINQ non vengono più valutate nel client</span><span class="sxs-lookup"><span data-stu-id="3ebc6-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="3ebc6-109">High</span><span class="sxs-lookup"><span data-stu-id="3ebc6-109">High</span></span>       |
| [<span data-ttu-id="3ebc6-110">EF Core 3.0 usa come destinazione .NET Standard 2.1 invece che .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="3ebc6-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="3ebc6-111">High</span><span class="sxs-lookup"><span data-stu-id="3ebc6-111">High</span></span>      |
| [<span data-ttu-id="3ebc6-112">Lo strumento da riga di comando di EF Core, dotnet ef, non è più incluso in .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="3ebc6-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="3ebc6-113">High</span><span class="sxs-lookup"><span data-stu-id="3ebc6-113">High</span></span>      |
| [<span data-ttu-id="3ebc6-114">DetectChanges rispetta i valori di chiave generati dall'archivio</span><span class="sxs-lookup"><span data-stu-id="3ebc6-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="3ebc6-115">High</span><span class="sxs-lookup"><span data-stu-id="3ebc6-115">High</span></span>      |
| [<span data-ttu-id="3ebc6-116">I metodi FromSql, ExecuteSql ed ExecuteSqlAsync sono stati rinominati</span><span class="sxs-lookup"><span data-stu-id="3ebc6-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="3ebc6-117">High</span><span class="sxs-lookup"><span data-stu-id="3ebc6-117">High</span></span>      |
| [<span data-ttu-id="3ebc6-118">I tipi di query vengono consolidati con tipi di entità</span><span class="sxs-lookup"><span data-stu-id="3ebc6-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="3ebc6-119">High</span><span class="sxs-lookup"><span data-stu-id="3ebc6-119">High</span></span>      |
| [<span data-ttu-id="3ebc6-120">Entity Framework Core non è più incluso nel framework condiviso di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ebc6-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="3ebc6-121">Medio</span><span class="sxs-lookup"><span data-stu-id="3ebc6-121">Medium</span></span>      |
| [<span data-ttu-id="3ebc6-122">Le eliminazioni a catena vengono ora eseguite immediatamente per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="3ebc6-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="3ebc6-123">Medio</span><span class="sxs-lookup"><span data-stu-id="3ebc6-123">Medium</span></span>      |
| [<span data-ttu-id="3ebc6-124">Il caricamento eager delle entità correlate ora si verifica in una singola query</span><span class="sxs-lookup"><span data-stu-id="3ebc6-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="3ebc6-125">Medio</span><span class="sxs-lookup"><span data-stu-id="3ebc6-125">Medium</span></span>      |
| [<span data-ttu-id="3ebc6-126">Semantica più chiara per DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="3ebc6-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="3ebc6-127">Medio</span><span class="sxs-lookup"><span data-stu-id="3ebc6-127">Medium</span></span>      |
| [<span data-ttu-id="3ebc6-128">L'API di configurazione per le relazioni di tipo di proprietà è stata modificata</span><span class="sxs-lookup"><span data-stu-id="3ebc6-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="3ebc6-129">Medio</span><span class="sxs-lookup"><span data-stu-id="3ebc6-129">Medium</span></span>      |
| [<span data-ttu-id="3ebc6-130">Ogni proprietà usa la generazione di chiavi di tipo intero in memoria indipendenti</span><span class="sxs-lookup"><span data-stu-id="3ebc6-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="3ebc6-131">Medio</span><span class="sxs-lookup"><span data-stu-id="3ebc6-131">Medium</span></span>      |
| [<span data-ttu-id="3ebc6-132">Le query senza rilevamento delle modifiche non eseguono più la risoluzione delle identità</span><span class="sxs-lookup"><span data-stu-id="3ebc6-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="3ebc6-133">Medio</span><span class="sxs-lookup"><span data-stu-id="3ebc6-133">Medium</span></span>      |
| [<span data-ttu-id="3ebc6-134">Modifiche dell'API dei metadati</span><span class="sxs-lookup"><span data-stu-id="3ebc6-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="3ebc6-135">Medio</span><span class="sxs-lookup"><span data-stu-id="3ebc6-135">Medium</span></span>      |
| [<span data-ttu-id="3ebc6-136">Modifiche dell'API dei metadati specifiche del provider</span><span class="sxs-lookup"><span data-stu-id="3ebc6-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="3ebc6-137">Medio</span><span class="sxs-lookup"><span data-stu-id="3ebc6-137">Medium</span></span>      |
| [<span data-ttu-id="3ebc6-138">Il metodo UseRowNumberForPaging è stato rimosso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="3ebc6-139">Medio</span><span class="sxs-lookup"><span data-stu-id="3ebc6-139">Medium</span></span>      |
| [<span data-ttu-id="3ebc6-140">Non è possibile comporre il metodo dati da tabelle se usato con stored procedure</span><span class="sxs-lookup"><span data-stu-id="3ebc6-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="3ebc6-141">Medio</span><span class="sxs-lookup"><span data-stu-id="3ebc6-141">Medium</span></span>      |
| [<span data-ttu-id="3ebc6-142">I metodi FromSql possono essere specificati solo in radici di query</span><span class="sxs-lookup"><span data-stu-id="3ebc6-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="3ebc6-143">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-143">Low</span></span>      |
| [<span data-ttu-id="3ebc6-144">~~L'esecuzione di query viene registrata a livello di debug~~ - Modifica annullata</span><span class="sxs-lookup"><span data-stu-id="3ebc6-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="3ebc6-145">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-145">Low</span></span>      |
| [<span data-ttu-id="3ebc6-146">I valori di chiave temporanei non sono più impostati nelle istanze di entità</span><span class="sxs-lookup"><span data-stu-id="3ebc6-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="3ebc6-147">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-147">Low</span></span>      |
| [<span data-ttu-id="3ebc6-148">Le entità dipendenti che condividono la tabella con l'entità di sicurezza sono ora facoltative</span><span class="sxs-lookup"><span data-stu-id="3ebc6-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="3ebc6-149">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-149">Low</span></span>      |
| [<span data-ttu-id="3ebc6-150">Tutte le entità che condividono una tabella con una colonna di token di concorrenza devono eseguirne il mapping a una proprietà</span><span class="sxs-lookup"><span data-stu-id="3ebc6-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="3ebc6-151">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-151">Low</span></span>      |
| [<span data-ttu-id="3ebc6-152">Per le proprietà ereditate da tipi senza mapping viene ora eseguito il mapping a una singola colonna per tutti i tipi derivati</span><span class="sxs-lookup"><span data-stu-id="3ebc6-152">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="3ebc6-153">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-153">Low</span></span>      |
| [<span data-ttu-id="3ebc6-154">La convenzione di proprietà di chiave esterna non ha più lo stesso nome della proprietà dell'entità di sicurezza</span><span class="sxs-lookup"><span data-stu-id="3ebc6-154">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="3ebc6-155">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-155">Low</span></span>      |
| [<span data-ttu-id="3ebc6-156">La connessione di database viene ora chiusa se non viene più usata prima del completamento di TransactionScope</span><span class="sxs-lookup"><span data-stu-id="3ebc6-156">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="3ebc6-157">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-157">Low</span></span>      |
| [<span data-ttu-id="3ebc6-158">I campi sottostanti vengono usati per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="3ebc6-158">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="3ebc6-159">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-159">Low</span></span>      |
| [<span data-ttu-id="3ebc6-160">Viene generata un'eccezione se vengono trovati più campi sottostanti compatibili</span><span class="sxs-lookup"><span data-stu-id="3ebc6-160">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="3ebc6-161">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-161">Low</span></span>      |
| [<span data-ttu-id="3ebc6-162">I nomi delle proprietà solo campo devono corrispondere al nome di campo</span><span class="sxs-lookup"><span data-stu-id="3ebc6-162">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="3ebc6-163">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-163">Low</span></span>      |
| [<span data-ttu-id="3ebc6-164">AddDbContext/AddDbContextPool non chiamano più AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="3ebc6-164">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="3ebc6-165">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-165">Low</span></span>      |
| [<span data-ttu-id="3ebc6-166">DbContext.Entry esegue ora un DetectChanges locale</span><span class="sxs-lookup"><span data-stu-id="3ebc6-166">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="3ebc6-167">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-167">Low</span></span>      |
| [<span data-ttu-id="3ebc6-168">Le chiavi matrice di byte e di stringhe non vengono generate dal client per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="3ebc6-168">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="3ebc6-169">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-169">Low</span></span>      |
| [<span data-ttu-id="3ebc6-170">ILoggerFactory è ora un servizio con ambito</span><span class="sxs-lookup"><span data-stu-id="3ebc6-170">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="3ebc6-171">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-171">Low</span></span>      |
| [<span data-ttu-id="3ebc6-172">I proxy di caricamento lazy non presuppongono più che le proprietà di navigazione vengano caricate completamente</span><span class="sxs-lookup"><span data-stu-id="3ebc6-172">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="3ebc6-173">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-173">Low</span></span>      |
| [<span data-ttu-id="3ebc6-174">La creazione di un numero eccessivo di provider di servizi interni è ora un errore per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="3ebc6-174">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="3ebc6-175">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-175">Low</span></span>      |
| [<span data-ttu-id="3ebc6-176">Nuovo comportamento per la chiamata di HasOne/HasMany con una singola stringa</span><span class="sxs-lookup"><span data-stu-id="3ebc6-176">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="3ebc6-177">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-177">Low</span></span>      |
| [<span data-ttu-id="3ebc6-178">Il tipo restituito per diversi metodi asincroni è cambiato da Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="3ebc6-178">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="3ebc6-179">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-179">Low</span></span>      |
| [<span data-ttu-id="3ebc6-180">L'annotazione Relational:TypeMapping è ora TypeMapping</span><span class="sxs-lookup"><span data-stu-id="3ebc6-180">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="3ebc6-181">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-181">Low</span></span>      |
| [<span data-ttu-id="3ebc6-182">ToTable in un tipo derivato genera un'eccezione</span><span class="sxs-lookup"><span data-stu-id="3ebc6-182">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="3ebc6-183">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-183">Low</span></span>      |
| [<span data-ttu-id="3ebc6-184">EF Core non invia più pragma per l'imposizione della chiave esterna di SQLite</span><span class="sxs-lookup"><span data-stu-id="3ebc6-184">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="3ebc6-185">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-185">Low</span></span>      |
| [<span data-ttu-id="3ebc6-186">Microsoft.EntityFrameworkCore.Sqlite dipende ora da SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="3ebc6-186">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="3ebc6-187">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-187">Low</span></span>      |
| [<span data-ttu-id="3ebc6-188">I valori Guid vengono ora archiviati come TEXT in SQLite</span><span class="sxs-lookup"><span data-stu-id="3ebc6-188">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="3ebc6-189">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-189">Low</span></span>      |
| [<span data-ttu-id="3ebc6-190">I valori char vengono ora archiviati come testo in SQLite</span><span class="sxs-lookup"><span data-stu-id="3ebc6-190">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="3ebc6-191">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-191">Low</span></span>      |
| [<span data-ttu-id="3ebc6-192">Gli ID di migrazione vengono ora generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica</span><span class="sxs-lookup"><span data-stu-id="3ebc6-192">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="3ebc6-193">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-193">Low</span></span>      |
| [<span data-ttu-id="3ebc6-194">Info/metadati dell'estensione rimossi da IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="3ebc6-194">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="3ebc6-195">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-195">Low</span></span>      |
| [<span data-ttu-id="3ebc6-196">LogQueryPossibleExceptionWithAggregateOperator è stato rinominato</span><span class="sxs-lookup"><span data-stu-id="3ebc6-196">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="3ebc6-197">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-197">Low</span></span>      |
| [<span data-ttu-id="3ebc6-198">Chiarimenti per l'API per i nomi di vincolo di chiave esterna</span><span class="sxs-lookup"><span data-stu-id="3ebc6-198">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="3ebc6-199">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-199">Low</span></span>      |
| [<span data-ttu-id="3ebc6-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync sono diventati pubblici</span><span class="sxs-lookup"><span data-stu-id="3ebc6-200">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="3ebc6-201">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-201">Low</span></span>      |
| [<span data-ttu-id="3ebc6-202">Microsoft.EntityFrameworkCore.Design è ora un pacchetto DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="3ebc6-202">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="3ebc6-203">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-203">Low</span></span>      |
| [<span data-ttu-id="3ebc6-204">Aggiornamento di SQLitePCL.raw alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="3ebc6-204">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="3ebc6-205">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-205">Low</span></span>      |
| [<span data-ttu-id="3ebc6-206">NetTopologySuite aggiornato alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="3ebc6-206">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="3ebc6-207">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-207">Low</span></span>      |
| [<span data-ttu-id="3ebc6-208">Devono essere configurare più relazioni ambigue che fanno riferimento a se stesse</span><span class="sxs-lookup"><span data-stu-id="3ebc6-208">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="3ebc6-209">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-209">Low</span></span>      |
| [<span data-ttu-id="3ebc6-210">DbFunction. Schema è una stringa null o vuota che lo configura in modo che sia nello schema predefinito del modello</span><span class="sxs-lookup"><span data-stu-id="3ebc6-210">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="3ebc6-211">Basso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-211">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="3ebc6-212">Le query LINQ non vengono più valutate nel client</span><span class="sxs-lookup"><span data-stu-id="3ebc6-212">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="3ebc6-213">[Problema n. 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Vedere anche il problema n. 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="3ebc6-213">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="3ebc6-214">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-214">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-215">Nelle versioni precedenti alla versione 3.0, quando EF Core non era in grado di convertire un'espressione inclusa in una query in SQL o in un parametro, l'espressione veniva automaticamente valutata nel client.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-215">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="3ebc6-216">Per impostazione predefinita, la valutazione client di espressioni potenzialmente dispendiose si limitava ad attivare solo un avviso.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-216">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="3ebc6-217">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-217">**New behavior**</span></span>

<span data-ttu-id="3ebc6-218">A partire dalla versione 3.0, EF Core consente solo la valutazione delle espressioni nella proiezione di primo livello (l'ultima chiamata `Select()` nella query) nel client.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-218">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="3ebc6-219">Quando le espressioni in altre posizioni all'interno della query non possono essere convertite in SQL o in un parametro, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-219">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="3ebc6-220">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-220">**Why**</span></span>

<span data-ttu-id="3ebc6-221">La valutazione client automatica delle query consente di eseguire numerose query anche nel caso in cui parti importanti delle query non possono essere convertite.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-221">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="3ebc6-222">Questo comportamento può causare un comportamento imprevisto e potenzialmente dannoso che può diventare evidente solo in produzione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-222">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="3ebc6-223">Ad esempio, una condizione in una chiamata `Where()` che non può essere convertita può causare il trasferimento di tutte le righe della tabella del server di database e l'applicazione del filtro nel client.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-223">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="3ebc6-224">È probabile che questa situazione non venga rilevata se la tabella contiene solo alcune righe in fase di sviluppo, ma che abbia un grande impatto quando l'applicazione passa in produzione dove la tabella può contenere milioni di righe.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-224">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="3ebc6-225">Gli avvisi di valutazione client inoltre si sono rivelati molto facili da ignorare durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-225">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="3ebc6-226">Inoltre, la valutazione client automatica può causare problemi in cui il miglioramento della conversione di query per espressioni specifiche causa modifiche impreviste che causano un'interruzione da una versione all'altra.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-226">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="3ebc6-227">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-227">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-228">Se una query non può essere convertita completamente, riscrivere la query in un formato che possa essere convertito o usare `AsEnumerable()`, `ToList()` o un elemento simile per riportare in modo esplicito i dati nel client dove possono essere quindi ulteriormente elaborati usando LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-228">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="3ebc6-229">EF Core 3.0 usa come destinazione .NET Standard 2.1 invece che .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="3ebc6-229">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="3ebc6-230">Problema n. 15498</span><span class="sxs-lookup"><span data-stu-id="3ebc6-230">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="3ebc6-231">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-231">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-232">Prima della versione 3.0, EF Core usava come destinazione .NET Standard 2.0 e poteva essere eseguito in tutte le piattaforme che supportano tale standard, incluso .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-232">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="3ebc6-233">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-233">**New behavior**</span></span>

<span data-ttu-id="3ebc6-234">A partire dalla versione 3.0, EF Core usa come destinazione .NET Standard 2.1 e verrà eseguito in tutte le piattaforme che supportano questo standard.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-234">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="3ebc6-235">.NET Framework non è incluso.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-235">This does not include .NET Framework.</span></span>

<span data-ttu-id="3ebc6-236">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-236">**Why**</span></span>

<span data-ttu-id="3ebc6-237">Questo comportamento deriva da una decisione strategica per le tecnologie .NET, che mira a concentrare le energie su .NET Core e su altre piattaforme .NET moderne, ad esempio Xamarin.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-237">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="3ebc6-238">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-238">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-239">Valutare il passaggio a una piattaforma .NET moderna.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-239">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="3ebc6-240">Se non è possibile, continuare a usare EF Core 2.1 o EF Core 2.2, che supportano entrambe .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-240">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="3ebc6-241">Entity Framework Core non è più incluso nel framework condiviso di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ebc6-241">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="3ebc6-242">Annunci problema n. 325</span><span class="sxs-lookup"><span data-stu-id="3ebc6-242">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="3ebc6-243">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-243">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-244">Nelle versioni precedenti ad ASP.NET Core 3.0, quando si aggiungeva un riferimento a un pacchetto in `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, veniva inserito EF Core e alcuni dei provider di dati di EF Core come il provider di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-244">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="3ebc6-245">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-245">**New behavior**</span></span>

<span data-ttu-id="3ebc6-246">A partire dalla versione 3.0, il framework condiviso di ASP.NET Core non include EF Core o provider di dati di EF Core.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-246">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="3ebc6-247">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-247">**Why**</span></span>

<span data-ttu-id="3ebc6-248">Prima di questa modifica, per ottenere EF Core erano necessarie procedure diverse, a seconda che l'applicazione avesse o meno come destinazione ASP.NET Core e SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-248">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="3ebc6-249">Inoltre, l'aggiornamento di ASP.NET Core forzava l'aggiornamento di EF Core e del provider di SQL Server, non sempre auspicabile.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-249">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="3ebc6-250">Con questa modifica, la procedura per ottenere EF Core è la stessa in tutti i provider, le implementazioni .NET supportate e i tipi di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-250">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="3ebc6-251">Gli sviluppatori possono ora controllare anche in modo preciso quando vengono aggiornati EF Core e i provider di dati di EF Core.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-251">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="3ebc6-252">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-252">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-253">Per usare EF Core in un'applicazione ASP.NET Core 3.0 o in un'altra applicazione supportata, aggiungere in modo esplicito un riferimento al pacchetto al provider di database di EF Core che verrà usato dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-253">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="3ebc6-254">Lo strumento della riga di comando di EF Core, dotnet ef, non è più incluso in .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="3ebc6-254">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="3ebc6-255">Problema n. 14016</span><span class="sxs-lookup"><span data-stu-id="3ebc6-255">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="3ebc6-256">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-256">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-257">Prima della versione 3.0, lo strumento `dotnet ef` era incluso in .NET Core SDK ed era immediatamente disponibile dalla riga di comando di qualsiasi progetto senza richiedere passaggi aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-257">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="3ebc6-258">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-258">**New behavior**</span></span>

<span data-ttu-id="3ebc6-259">A partire dalla versione 3.0, .NET SDK non include lo strumento `dotnet ef` pertanto, prima di poterlo usare, è necessario installarlo in modo esplicito come strumento locale o globale.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-259">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="3ebc6-260">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-260">**Why**</span></span>

<span data-ttu-id="3ebc6-261">Questa modifica consente di distribuire e aggiornare `dotnet ef` come uno strumento della riga di comando di .NET in NuGet, coerentemente con il fatto che anche EF Core 3.0 viene distribuito come pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-261">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="3ebc6-262">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-262">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-263">Per essere in grado di gestire le migrazioni o eseguire lo scaffolding di `DbContext`, installare `dotnet-ef` come strumento globale:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-263">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="3ebc6-264">È inoltre possibile ottenerlo come strumento locale quando si ripristinano le dipendenze di un progetto che lo dichiara come dipendenza di strumenti utilizzando un [file manifesto dello strumento](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="3ebc6-264">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="3ebc6-265">I metodi FromSql, ExecuteSql ed ExecuteSqlAsync sono stati rinominati</span><span class="sxs-lookup"><span data-stu-id="3ebc6-265">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="3ebc6-266">Problema n. 10996</span><span class="sxs-lookup"><span data-stu-id="3ebc6-266">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="3ebc6-267">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-267">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-268">Prima di EF Core 3.0, erano disponibili overload per questi nomi di metodo per supportare l'uso con una stringa normale o una stringa che deve essere interpolata in SQL e parametri.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-268">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="3ebc6-269">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-269">**New behavior**</span></span>

<span data-ttu-id="3ebc6-270">A partire da EF Core 3.0, usare `FromSqlRaw`, `ExecuteSqlRaw` e `ExecuteSqlRawAsync` per creare una query con parametri in cui i parametri vengono passati separatamente dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-270">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="3ebc6-271">Esempio:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-271">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="3ebc6-272">Usare `FromSqlInterpolated`, `ExecuteSqlInterpolated`, e `ExecuteSqlInterpolatedAsync` per creare una query con parametri in cui i parametri vengono passati come parte di una stringa di query interpolata.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-272">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="3ebc6-273">Esempio:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-273">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="3ebc6-274">Si noti che entrambe le query precedenti produrranno lo stesso codice SQL con parametri con gli stessi parametri SQL.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-274">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="3ebc6-275">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-275">**Why**</span></span>

<span data-ttu-id="3ebc6-276">Con gli overload di metodi come questi, è molto facile chiamare accidentalmente il metodo con stringa non elaborata anche se l'intento era chiamare il metodo con stringa interpolata e viceversa.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-276">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="3ebc6-277">Il risultato potrebbero essere query senza parametri, quando invece è prevista la parametrizzazione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-277">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="3ebc6-278">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-278">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-279">Passare all'uso dei nuovi nomi di metodo.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-279">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="3ebc6-280">Non è possibile comporre il metodo dati da tabelle se usato con stored procedure</span><span class="sxs-lookup"><span data-stu-id="3ebc6-280">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="3ebc6-281">Rilevamento del problema #15392</span><span class="sxs-lookup"><span data-stu-id="3ebc6-281">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="3ebc6-282">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-282">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-283">Prima di EF Core 3,0, il metodo dati da tabelle ha tentato di rilevare se il SQL passato può essere composto in base a.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-283">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="3ebc6-284">Ha fatto la valutazione del client quando SQL era non componibile come un stored procedure.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-284">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="3ebc6-285">La query seguente ha funzionato eseguendo il stored procedure sul server e FirstOrDefault sul lato client.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-285">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="3ebc6-286">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-286">**New behavior**</span></span>

<span data-ttu-id="3ebc6-287">A partire da EF Core 3,0, EF Core non tenterà di analizzare SQL.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-287">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="3ebc6-288">Quindi, se si esegue la composizione dopo FromSqlRaw/FromSqlInterpolated, EF Core comporrà il SQL causando una sottoquery.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-288">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="3ebc6-289">Quindi, se si usa un stored procedure con Composition, si otterrà un'eccezione per la sintassi SQL non valida.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-289">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="3ebc6-290">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-290">**Why**</span></span>

<span data-ttu-id="3ebc6-291">EF Core 3,0 non supporta la valutazione automatica del client perché è stata soggetta a errori, come illustrato [qui](#linq-queries-are-no-longer-evaluated-on-the-client).</span><span class="sxs-lookup"><span data-stu-id="3ebc6-291">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="3ebc6-292">**Mitigazione**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-292">**Mitigation**</span></span>

<span data-ttu-id="3ebc6-293">Se si usa un stored procedure in FromSqlRaw/FromSqlInterpolated, è noto che non è possibile componerlo, quindi è possibile aggiungere __AsEnumerable/AsAsyncEnumerable__ subito dopo la chiamata al metodo dati da tabelle per evitare qualsiasi composizione sul lato server.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-293">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```C#
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="3ebc6-294">I metodi FromSql possono essere specificati solo in radici di query</span><span class="sxs-lookup"><span data-stu-id="3ebc6-294">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="3ebc6-295">Problema n. 15704</span><span class="sxs-lookup"><span data-stu-id="3ebc6-295">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="3ebc6-296">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-296">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-297">Prima di EF Core 3.0, il metodo `FromSql` poteva essere specificato in un punto qualsiasi nella query.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-297">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="3ebc6-298">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-298">**New behavior**</span></span>

<span data-ttu-id="3ebc6-299">A partire da EF Core 3.0, i nuovi metodi `FromSqlRaw` e `FromSqlInterpolated` (che sostituiscono`FromSql`) possono essere specificati solo per radici di query, ad esempio direttamente in `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-299">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="3ebc6-300">Qualsiasi tentativo di specificarli altrove causerà un errore di compilazione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-300">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="3ebc6-301">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-301">**Why**</span></span>

<span data-ttu-id="3ebc6-302">La specifica di `FromSql` in qualsiasi posizione diversa da un `DbSet` non ha alcun significato aggiuntivo oppure valore aggiunto e può causare ambiguità in determinati scenari.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-302">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="3ebc6-303">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-303">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-304">Le chiamate di `FromSql` devono essere spostate in modo da comparire direttamente nel `DbSet` a cui si applicano.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-304">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="3ebc6-305">Le query senza rilevamento delle modifiche non eseguono più la risoluzione delle identità</span><span class="sxs-lookup"><span data-stu-id="3ebc6-305">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="3ebc6-306">Problema n. 13518</span><span class="sxs-lookup"><span data-stu-id="3ebc6-306">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="3ebc6-307">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-307">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-308">Prima di EF Core 3.0, la stessa istanza di un'entità poteva essere usata per ogni occorrenza di un entità con tipo e ID specifici.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-308">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="3ebc6-309">Questo comportamento corrisponde a quello delle query con rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-309">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="3ebc6-310">Ad esempio, questa query:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-310">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="3ebc6-311">restituisce la stessa istanza di `Category` per ogni `Product` associato alla categoria specificata.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-311">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="3ebc6-312">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-312">**New behavior**</span></span>

<span data-ttu-id="3ebc6-313">A partire da EF Core 3.0, vengono create istanze di entità diverse quando un'entità con un determinato tipo e ID viene rilevata in posizioni diverse nel grafico restituito.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-313">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="3ebc6-314">La query precedente, ad esempio, ora restituirà una nuova istanza di `Category` per ogni `Product` anche quando due prodotti sono associati alla stessa categoria.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-314">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="3ebc6-315">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-315">**Why**</span></span>

<span data-ttu-id="3ebc6-316">La risoluzione delle identità (ovvero il processo per determinare che un'entità ha lo stesso tipo e ID di un'entità rilevata in precedenza) aggiunge un ulteriore sovraccarico della memoria e delle prestazioni,</span><span class="sxs-lookup"><span data-stu-id="3ebc6-316">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="3ebc6-317">il che va ad annullare il vantaggio derivante dall'uso delle query senza rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-317">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="3ebc6-318">Inoltre, anche se in alcuni casi la risoluzione delle identità può essere utile, tuttavia non è necessaria se le entità devono essere serializzate e inviate a un client, come avviene comunemente con le query senza rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-318">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="3ebc6-319">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-319">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-320">Se la risoluzione delle identità è necessaria, usare una query con rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-320">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="3ebc6-321">~~L'esecuzione di query viene registrata a livello di debug~~ - Modifica annullata</span><span class="sxs-lookup"><span data-stu-id="3ebc6-321">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="3ebc6-322">Problema n. 14523</span><span class="sxs-lookup"><span data-stu-id="3ebc6-322">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="3ebc6-323">Questa modifica è stata annullata perché la nuova configurazione in EF Core 3.0 consente all'applicazione di specificare il livello di log per qualsiasi evento.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-323">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="3ebc6-324">Ad esempio, per impostare la registrazione di SQL sul livello `Debug`, configurare il livello in modo esplicito in `OnConfiguring` o `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-324">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="3ebc6-325">I valori di chiave temporanei non sono più impostati nelle istanze di entità</span><span class="sxs-lookup"><span data-stu-id="3ebc6-325">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="3ebc6-326">Problema n. 12378</span><span class="sxs-lookup"><span data-stu-id="3ebc6-326">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="3ebc6-327">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-327">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-328">Nelle versioni precedenti a EF Core 3.0 i valori temporanei venivano assegnati a tutte le proprietà di chiave per cui veniva in seguito generato un valore reale dal database.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-328">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="3ebc6-329">In genere questi valori temporanei erano numeri negativi elevati.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-329">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="3ebc6-330">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-330">**New behavior**</span></span>

<span data-ttu-id="3ebc6-331">A partire dalla versione 3.0, EF Core archivia il valore di chiave temporaneo come parte delle informazioni di rilevamento dell'entità e non modifica la proprietà di chiave.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-331">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="3ebc6-332">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-332">**Why**</span></span>

<span data-ttu-id="3ebc6-333">Questa modifica è stata apportata per impedire che i valori di chiave temporanei diventino erroneamente permanenti quando un'entità rilevata in precedenza da un'istanza `DbContext` viene spostata in un'altra istanza `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-333">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="3ebc6-334">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-334">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-335">Le applicazioni che assegnano valori di chiave primaria in chiavi esterne per creare associazioni tra le entità possono dipendere dal comportamento precedente se le chiavi primarie vengono generate dall'archivio e appartengono a entità con stato `Added`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-335">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="3ebc6-336">Questo può essere evitato:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-336">This can be avoided by:</span></span>
* <span data-ttu-id="3ebc6-337">Non usando chiavi generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-337">Not using store-generated keys.</span></span>
* <span data-ttu-id="3ebc6-338">Impostando le proprietà di navigazione in modo da creare relazioni anziché impostando valori di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-338">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="3ebc6-339">Ottenendo i valori di chiave temporanei effettivi dalle informazioni di rilevamento dell'entità.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-339">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="3ebc6-340">Ad esempio, `context.Entry(blog).Property(e => e.Id).CurrentValue` restituisce il valore temporaneo anche quando `blog.Id` non è stato impostato.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-340">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="3ebc6-341">DetectChanges rispetta i valori di chiave generati dall'archivio</span><span class="sxs-lookup"><span data-stu-id="3ebc6-341">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="3ebc6-342">Problema n. 14616</span><span class="sxs-lookup"><span data-stu-id="3ebc6-342">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="3ebc6-343">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-343">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-344">Nelle versioni precedenti a EF Core 3.0 un'entità non rilevata individuata da `DetectChanges` veniva rilevata nello stato `Added` e inserita come nuova riga quando veniva eseguita una chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-344">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="3ebc6-345">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-345">**New behavior**</span></span>

<span data-ttu-id="3ebc6-346">A partire da EF Core 3.0, se un'entità usa valori di chiave generati e viene impostato un valore di chiave, l'entità viene rilevata nello stato `Modified`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-346">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="3ebc6-347">Ciò significa che si presuppone l'esistenza di una riga per l'entità che viene aggiornata quando viene eseguita una chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-347">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="3ebc6-348">Se il valore di chiave non viene impostato o se il tipo di entità non usa chiavi generate, la nuova entità viene rilevata come `Added` come nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-348">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="3ebc6-349">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-349">**Why**</span></span>

<span data-ttu-id="3ebc6-350">Questa modifica è stata apportata per rendere più semplice e coerente l'uso di grafici di entità disconnesse con chiavi generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-350">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="3ebc6-351">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-351">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-352">Questa modifica può interrompere un'applicazione se un tipo di entità è configurato per l'uso di chiavi generate ma i valori di chiave sono impostati in modo esplicito per le nuove istanze.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-352">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="3ebc6-353">La correzione consiste nel configurare in modo esplicito le proprietà di chiave per non usare valori generati.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-353">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="3ebc6-354">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-354">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="3ebc6-355">Oppure con annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-355">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="3ebc6-356">Le eliminazioni a catena vengono ora eseguite immediatamente per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="3ebc6-356">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="3ebc6-357">Problema n. 10114</span><span class="sxs-lookup"><span data-stu-id="3ebc6-357">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="3ebc6-358">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-358">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-359">Nelle versioni precedenti alla versione 3.0, EF Core applicava azioni a catena (eliminazione delle entità dipendenti quando veniva eliminata un'entità di sicurezza obbligatoria o veniva recisa la relazione con un'entità di sicurezza obbligatoria) solo dopo la chiamata a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-359">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="3ebc6-360">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-360">**New behavior**</span></span>

<span data-ttu-id="3ebc6-361">A partire dalla versione 3.0, EF Core applica le azioni a catena non appena viene rilevata la condizione di attivazione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-361">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="3ebc6-362">Ad esempio, la chiamata a `context.Remove()` per eliminare un'entità di sicurezza causa anche l'impostazione immediata di tutti i dipendenti obbligatori correlati rilevati su `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-362">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="3ebc6-363">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-363">**Why**</span></span>

<span data-ttu-id="3ebc6-364">Questa modifica è stata apportata per migliorare l'esperienza di associazione di dati e degli scenari di controllo in cui è importante individuare le entità che verranno eliminate _prima_ della chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-364">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="3ebc6-365">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-365">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-366">Il comportamento precedente può essere ripristinato tramite le impostazioni in `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-366">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="3ebc6-367">Esempio:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-367">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="3ebc6-368">Il caricamento eager delle entità correlate ora si verifica in una singola query</span><span class="sxs-lookup"><span data-stu-id="3ebc6-368">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="3ebc6-369">Rilevamento del problema #18022</span><span class="sxs-lookup"><span data-stu-id="3ebc6-369">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="3ebc6-370">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-370">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-371">Prima del 3,0, il caricamento immediato delle esplorazioni delle raccolte tramite gli operatori `Include` ha causato la generazione di più query sul database relazionale, una per ogni tipo di entità correlato.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-371">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="3ebc6-372">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-372">**New behavior**</span></span>

<span data-ttu-id="3ebc6-373">A partire da 3,0, EF Core genera una singola query con JOIN nei database relazionali.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-373">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="3ebc6-374">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-374">**Why**</span></span>

<span data-ttu-id="3ebc6-375">L'invio di più query per l'implementazione di una singola query LINQ ha causato numerosi problemi, incluse le prestazioni negative in quanto sono stati necessari più round trip di database e i dati coerenza problemi poiché ogni query poteva osservare uno stato diverso del database.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-375">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="3ebc6-376">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-376">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-377">Sebbene tecnicamente non si tratti di una modifica di rilievo, potrebbe avere un impatto significativo sulle prestazioni dell'applicazione quando una singola query contiene un numero elevato di operatori `Include` sulle esplorazioni della raccolta.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-377">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="3ebc6-378">Per ulteriori informazioni e per riscrivere le query in modo più efficiente, [vedere il commento](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) .</span><span class="sxs-lookup"><span data-stu-id="3ebc6-378">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="3ebc6-379">Semantica più chiara per DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="3ebc6-379">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="3ebc6-380">Problema n. 12661</span><span class="sxs-lookup"><span data-stu-id="3ebc6-380">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="3ebc6-381">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-381">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-382">Prima della versione 3.0, `DeleteBehavior.Restrict` creava chiavi esterne nel database con la semantica `Restrict`, ma modificava anche la correzione interna in modo non ovvio.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-382">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="3ebc6-383">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-383">**New behavior**</span></span>

<span data-ttu-id="3ebc6-384">A partire dalla versione 3.0, `DeleteBehavior.Restrict` assicura che le chiavi esterne vengano create con la semantica `Restrict`, ovvero non a cascata e con generazione di un'eccezione in caso di violazione di vincolo, senza influire sulla correzione interna di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-384">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="3ebc6-385">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-385">**Why**</span></span>

<span data-ttu-id="3ebc6-386">Questa modifica è stata apportata per migliorare l'esperienza di uso di `DeleteBehavior` in modo intuitivo, senza effetti collaterali imprevisti.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-386">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="3ebc6-387">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-387">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-388">Il comportamento precedente può essere ripristinato tramite `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-388">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="3ebc6-389">I tipi di query vengono consolidati con tipi di entità</span><span class="sxs-lookup"><span data-stu-id="3ebc6-389">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="3ebc6-390">Problema n. 14194</span><span class="sxs-lookup"><span data-stu-id="3ebc6-390">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="3ebc6-391">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-391">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-392">Nelle versioni precedenti a EF Core 3.0 i [tipi di query](xref:core/modeling/keyless-entity-types) erano uno strumento per eseguire query su dati che non definiscono una chiave primaria in modo strutturato.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-392">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="3ebc6-393">Veniva infatti usato un tipo di query per eseguire il mapping di tipi di entità senza chiavi (più probabilmente da una vista, ma anche da una tabella), mentre veniva usato un tipo di entità normale quando era disponibile una chiave (più probabilmente da una tabella, ma anche da una vista).</span><span class="sxs-lookup"><span data-stu-id="3ebc6-393">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="3ebc6-394">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-394">**New behavior**</span></span>

<span data-ttu-id="3ebc6-395">Un tipo di query diventa ora semplicemente un tipo di entità senza chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-395">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="3ebc6-396">I tipi di entità senza chiave hanno la stessa funzionalità dei tipi di query nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-396">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="3ebc6-397">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-397">**Why**</span></span>

<span data-ttu-id="3ebc6-398">Questa modifica è stata apportata per ridurre la confusione riguardo lo scopo dei tipi di query.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-398">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="3ebc6-399">In particolare, si tratta di tipi di entità senza chiave che sono intrinsecamente di sola lettura per questo motivo ma non dovrebbero essere usati solo perché un tipo di entità deve essere di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-399">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="3ebc6-400">Analogamente, spesso vengono mappati alle viste solo perché le viste spesso non definiscono le chiavi.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-400">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="3ebc6-401">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-401">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-402">Le parti dell'API seguenti sono ora obsolete:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-402">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="3ebc6-403">**`ModelBuilder.Query<>()`** - È necessario chiamare `ModelBuilder.Entity<>().HasNoKey()` per contrassegnare un tipo di entità come tipo senza chiavi.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-403">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="3ebc6-404">Non ne viene eseguita la configurazione per convenzione per evitare una configurazione errata quando è prevista una chiave primaria che tuttavia non corrisponde alla convenzione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-404">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="3ebc6-405">**`DbQuery<>`** - Usare `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-405">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="3ebc6-406">**`DbContext.Query<>()`** - Usare `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-406">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="3ebc6-407">L'API di configurazione per le relazioni di tipo di proprietà è stata modificata</span><span class="sxs-lookup"><span data-stu-id="3ebc6-407">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="3ebc6-408">[Problema n. 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema n. 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema n. 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="3ebc6-408">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="3ebc6-409">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-409">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-410">Nelle versioni precedenti a EF Core 3.0 la configurazione della relazione di proprietà veniva eseguita direttamente dopo la chiamata a `OwnsOne` o `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-410">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="3ebc6-411">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-411">**New behavior**</span></span>

<span data-ttu-id="3ebc6-412">A partire da EF Core 3.0, è disponibile l'API Fluent per configurare una proprietà di navigazione per il proprietario usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-412">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="3ebc6-413">Esempio:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-413">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="3ebc6-414">La configurazione correlata alla relazione tra proprietario e elemento di proprietà deve essere ora concatenata dopo `WithOwner()` in modo analogo a come vengono configurate altre relazioni.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-414">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="3ebc6-415">La configurazione per il tipo di proprietà viene comunque concatenata dopo `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-415">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="3ebc6-416">Esempio:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-416">For example:</span></span>

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

<span data-ttu-id="3ebc6-417">Inoltre, la chiamata a `Entity()`, `HasOne()` o `Set()` con una destinazione di tipo di proprietà genera ora un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-417">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="3ebc6-418">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-418">**Why**</span></span>

<span data-ttu-id="3ebc6-419">Questa modifica è stata apportata per creare una separazione più netta tra la configurazione del tipo di proprietà e la _relazione_ con il tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-419">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="3ebc6-420">Ciò consente di eliminare ambiguità e confusione su metodi come `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-420">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="3ebc6-421">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-421">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-422">Modificare la configurazione delle relazioni dei tipi di proprietà per usare la superficie della nuova API come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-422">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="3ebc6-423">Le entità dipendenti che condividono la tabella con l'entità di sicurezza sono ora facoltative</span><span class="sxs-lookup"><span data-stu-id="3ebc6-423">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="3ebc6-424">Problema n. 9005</span><span class="sxs-lookup"><span data-stu-id="3ebc6-424">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="3ebc6-425">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-425">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-426">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-426">Consider the following model:</span></span>
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
<span data-ttu-id="3ebc6-427">Prima di EF Core 3.0, se `OrderDetails` è di proprietà di `Order` o viene mappato in modo esplicito alla stessa tabella, era sempre necessaria un'istanza di `OrderDetails` per l'aggiunta di un nuovo `Order`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-427">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="3ebc6-428">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-428">**New behavior**</span></span>

<span data-ttu-id="3ebc6-429">A partire dalla versione 3.0, EF Core consente di aggiungere un `Order` senza un `OrderDetails` ed esegue il mapping di tutte le proprietà di `OrderDetails`, tranne che la chiave primaria, a colonne che ammettono valori Null.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-429">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="3ebc6-430">In fase di query, EF Core imposta `OrderDetails` su `null` se una delle relative proprietà obbligatorie non ha un valore o se non sono presenti proprietà obbligatorie oltre alla chiave primaria e tutte le proprietà sono `null`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-430">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="3ebc6-431">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-431">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-432">Se il modello include una tabella condivisa dipendente con tutte le colonne facoltative, ma è previsto che la navigazione che punta a essa non sia `null`, l'applicazione deve essere modificata per gestire casi in cui la navigazione è `null`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-432">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="3ebc6-433">Se questo non è possibile, è consigliabile aggiungere una proprietà obbligatoria al tipo di entità o assegnare un valore non `null` ad almeno una proprietà.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-433">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="3ebc6-434">Tutte le entità che condividono una tabella con una colonna di token di concorrenza devono eseguirne il mapping a una proprietà</span><span class="sxs-lookup"><span data-stu-id="3ebc6-434">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="3ebc6-435">Problema n. 14154</span><span class="sxs-lookup"><span data-stu-id="3ebc6-435">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="3ebc6-436">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-436">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-437">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-437">Consider the following model:</span></span>
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
<span data-ttu-id="3ebc6-438">Prima di EF Core 3.0, se `OrderDetails` è di proprietà di `Order` o è mappato in modo esplicito alla stessa tabella, il solo aggiornamento di `OrderDetails` non aggiornerà il valore `Version` nel client e l'aggiornamento successivo avrà esito negativo.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-438">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="3ebc6-439">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-439">**New behavior**</span></span>

<span data-ttu-id="3ebc6-440">A partire dalla versione 3.0, EF Core propaga il nuovo valore `Version` a `Order` se è proprietario di `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-440">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="3ebc6-441">In caso contrario, viene generata un'eccezione durante la convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-441">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="3ebc6-442">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-442">**Why**</span></span>

<span data-ttu-id="3ebc6-443">Questa modifica è stata apportata per evitare un valore del token di concorrenza non aggiornato quando viene aggiornata solo una delle entità mappate alla stessa tabella.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-443">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="3ebc6-444">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-444">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-445">Tutte le entità che condividono la tabella devono includere una proprietà mappata alla colonna del token di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-445">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="3ebc6-446">È possibile crearne una in stato shadow:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-446">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="3ebc6-447">Per le proprietà ereditate da tipi senza mapping viene ora eseguito il mapping a una singola colonna per tutti i tipi derivati</span><span class="sxs-lookup"><span data-stu-id="3ebc6-447">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="3ebc6-448">Problema n. 13998</span><span class="sxs-lookup"><span data-stu-id="3ebc6-448">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="3ebc6-449">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-449">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-450">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-450">Consider the following model:</span></span>
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

<span data-ttu-id="3ebc6-451">Prima di EF Core 3.0, per la proprietà `ShippingAddress` sarebbe stato eseguito il mapping a colonne separate per `BulkOrder` e `Order` per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-451">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="3ebc6-452">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-452">**New behavior**</span></span>

<span data-ttu-id="3ebc6-453">A partire dalla versione 3.0, EF Core crea solo una colonna per `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-453">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="3ebc6-454">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-454">**Why**</span></span>

<span data-ttu-id="3ebc6-455">Il comportamento precedente non era previsto.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-455">The old behavoir was unexpected.</span></span>

<span data-ttu-id="3ebc6-456">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-456">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-457">È ancora possibile eseguire il mapping esplicito della proprietà a una colonna separata per i tipi derivati:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-457">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="3ebc6-458">La convenzione di proprietà di chiave esterna non ha più lo stesso nome della proprietà dell'entità di sicurezza</span><span class="sxs-lookup"><span data-stu-id="3ebc6-458">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="3ebc6-459">Problema n. 13274</span><span class="sxs-lookup"><span data-stu-id="3ebc6-459">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="3ebc6-460">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-460">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-461">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-461">Consider the following model:</span></span>
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
<span data-ttu-id="3ebc6-462">Nelle versioni precedenti a EF Core 3.0 veniva usata la proprietà `CustomerId` per la chiave esterna per convenzione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-462">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="3ebc6-463">Tuttavia, se `Order` è un tipo di proprietà, `CustomerId` sarebbe la chiave primaria e ciò non è in genere auspicabile.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-463">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="3ebc6-464">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-464">**New behavior**</span></span>

<span data-ttu-id="3ebc6-465">A partire da 3.0, EF Core non tenta di usare le proprietà per le chiavi esterne per convenzione se hanno lo stesso nome della proprietà dell'entità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-465">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="3ebc6-466">Viene ancora eseguita la corrispondenza tra i criteri del nome del tipo dell'entità di sicurezza concatenato al nome della proprietà dell'entità di sicurezza e il nome di navigazione concatenato al nome della proprietà dell'entità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-466">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="3ebc6-467">Esempio:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-467">For example:</span></span>

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

<span data-ttu-id="3ebc6-468">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-468">**Why**</span></span>

<span data-ttu-id="3ebc6-469">Questa modifica è stata apportata per evitare una definizione errata della proprietà di chiave primaria nel tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-469">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="3ebc6-470">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-470">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-471">Se la proprietà è stata progettata per essere la chiave esterna e di conseguenza parte della chiave primaria, configurarla in modo esplicito come chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-471">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="3ebc6-472">La connessione al database viene ora chiusa se non viene più usata prima del completamento di TransactionScope</span><span class="sxs-lookup"><span data-stu-id="3ebc6-472">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="3ebc6-473">Problema n. 14218</span><span class="sxs-lookup"><span data-stu-id="3ebc6-473">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="3ebc6-474">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-474">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-475">Prima di EF Core 3.0, se il contesto apre la connessione all'interno di un `TransactionScope`, la connessione rimane aperta mentre è attivo il `TransactionScope` corrente.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-475">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="3ebc6-476">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-476">**New behavior**</span></span>

<span data-ttu-id="3ebc6-477">A partire dalla versione 3.0, EF Core chiude la connessione non appena non viene più usata.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-477">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="3ebc6-478">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-478">**Why**</span></span>

<span data-ttu-id="3ebc6-479">Questa modifica consente di usare più contesti nello stesso `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-479">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="3ebc6-480">Il nuovo comportamento corrisponde anche a EF6.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-480">The new behavior also matches EF6.</span></span>

<span data-ttu-id="3ebc6-481">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-481">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-482">Se la connessione deve rimanere aperta, la chiamata esplicita di `OpenConnection()` garantirà che EF Core non la chiuda prematuramente:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-482">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="3ebc6-483">Ogni proprietà usa la generazione di chiavi di tipo intero in memoria indipendenti</span><span class="sxs-lookup"><span data-stu-id="3ebc6-483">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="3ebc6-484">Problema n. 6872</span><span class="sxs-lookup"><span data-stu-id="3ebc6-484">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="3ebc6-485">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-485">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-486">Nelle versioni precedenti a EF Core 3.0 veniva usato un unico generatore di valori condiviso per tutte le proprietà di chiavi di tipo intero in memoria.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-486">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="3ebc6-487">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-487">**New behavior**</span></span>

<span data-ttu-id="3ebc6-488">A partire da EF Core 3.0, ogni proprietà di chiave di tipo intero riceve un generatore di valori quando viene usato il database in memoria.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-488">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="3ebc6-489">Inoltre, se il database viene eliminato, la generazione di chiavi viene reimpostata per tutte le tabelle.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-489">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="3ebc6-490">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-490">**Why**</span></span>

<span data-ttu-id="3ebc6-491">Questa modifica è stata apportata per allineare maggiormente la generazione di chiavi in memoria alla generazione di chiavi del database reale e per migliorare la possibilità di isolare i test l'uno dall'altro quando viene usato il database in memoria.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-491">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="3ebc6-492">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-492">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-493">Ciò può interrompere un'applicazione che si basa sull'impostazione di valori di chiave in memoria specifici.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-493">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="3ebc6-494">È consigliabile non basare l'applicazione su valori di chiave specifici o eseguire l'aggiornamento per passare al nuovo comportamento.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-494">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="3ebc6-495">I campi sottostanti vengono usati per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="3ebc6-495">Backing fields are used by default</span></span>

[<span data-ttu-id="3ebc6-496">Problema n. 12430</span><span class="sxs-lookup"><span data-stu-id="3ebc6-496">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="3ebc6-497">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-497">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-498">Nelle versioni precedenti alla versione 3.0, anche se il campo sottostante di una proprietà era noto, per impostazione predefinita EF Core eseguiva la lettura e la scrittura del valore della proprietà usando i metodi getter e setter della proprietà.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-498">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="3ebc6-499">L'eccezione era costituita dall'esecuzione di query in cui il campo sottostante, se noto, veniva impostato direttamente.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-499">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="3ebc6-500">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-500">**New behavior**</span></span>

<span data-ttu-id="3ebc6-501">A partire da EF Core 3.0, se il campo sottostante di una proprietà è noto, la lettura e la scrittura della proprietà vengono sempre eseguite usando il campo sottostante.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-501">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="3ebc6-502">Ciò potrebbe causare un'interruzione dell'applicazione se l'applicazione si basa su un comportamento aggiuntivo codificato nei metodi getter o setter.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-502">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="3ebc6-503">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-503">**Why**</span></span>

<span data-ttu-id="3ebc6-504">Questa modifica è stata apportata per impedire a EF Core di attivare per errore la logica di business per impostazione predefinita quando si eseguono operazioni di database che interessano le entità.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-504">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="3ebc6-505">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-505">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-506">È possibile ripristinare il comportamento delle versioni precedenti alla versione 3.0 tramite la configurazione della modalità di accesso delle proprietà in `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-506">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="3ebc6-507">Esempio:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-507">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="3ebc6-508">Viene generata un'eccezione se vengono trovati più campi sottostanti compatibili</span><span class="sxs-lookup"><span data-stu-id="3ebc6-508">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="3ebc6-509">Problema n. 12523</span><span class="sxs-lookup"><span data-stu-id="3ebc6-509">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="3ebc6-510">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-510">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-511">Nelle versioni precedenti a EF Core 3.0, se più campi soddisfacevano le regole di ricerca del campo sottostante di una proprietà, veniva selezionato un solo campo in base a un ordine di precedenza.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-511">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="3ebc6-512">Ciò poteva causare l'uso di un campo non corretto nei casi ambigui.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-512">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="3ebc6-513">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-513">**New behavior**</span></span>

<span data-ttu-id="3ebc6-514">A partire da EF Core 3.0, se più campi corrispondono alla stessa proprietà, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-514">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="3ebc6-515">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-515">**Why**</span></span>

<span data-ttu-id="3ebc6-516">Questa modifica è stata apportata per evitare di usare automaticamente un campo rispetto a un altro quando un solo campo può essere quello corretto.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-516">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="3ebc6-517">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-517">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-518">Per le proprietà con campi sottostanti ambigui, il campo da usare deve essere specificato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-518">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="3ebc6-519">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-519">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="3ebc6-520">I nomi delle proprietà solo campo devono corrispondere al nome di campo</span><span class="sxs-lookup"><span data-stu-id="3ebc6-520">Field-only property names should match the field name</span></span>

<span data-ttu-id="3ebc6-521">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-521">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-522">Prima di EF Core 3,0, una proprietà può essere specificata da un valore stringa e, se non è stata trovata alcuna proprietà con tale nome nel tipo .NET, EF Core tenterà di associarla a un campo usando le regole di convenzione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-522">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="3ebc6-523">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-523">**New behavior**</span></span>

<span data-ttu-id="3ebc6-524">A partire da EF Core 3.0, una proprietà solo campo deve corrispondere esattamente al nome del campo.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-524">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="3ebc6-525">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-525">**Why**</span></span>

<span data-ttu-id="3ebc6-526">Questa modifica è stata introdotta per evitare di usare lo stesso campo per due proprietà con nome simile. Le regole di corrispondenza per le proprietà solo campo sono state anche uniformate a quelle per le proprietà mappate a proprietà CLR.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-526">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="3ebc6-527">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-527">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-528">Le proprietà solo campo devono avere lo stesso nome del campo a cui vengono mappate.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-528">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="3ebc6-529">In una versione futura di EF Core dopo il 3,0, si prevede di abilitare di nuovo in modo esplicito il nome di un campo diverso dal nome della proprietà (vedere il problema [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="3ebc6-529">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="3ebc6-530">AddDbContext/AddDbContextPool non chiamano più AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="3ebc6-530">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="3ebc6-531">Problema n. 14756</span><span class="sxs-lookup"><span data-stu-id="3ebc6-531">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="3ebc6-532">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-532">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-533">Prima di EF Core 3.0, la chiamata di `AddDbContext` oppure `AddDbContextPool` comporta anche la registrazione dei servizi di registrazione e memorizzazione nella cache con inserimento delle dipendenze tramite chiamate a [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="3ebc6-533">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="3ebc6-534">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-534">**New behavior**</span></span>

<span data-ttu-id="3ebc6-535">A partire da EF Core 3.0, `AddDbContext` e `AddDbContextPool` non registreranno più questi servizi con inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-535">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="3ebc6-536">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-536">**Why**</span></span>

<span data-ttu-id="3ebc6-537">EF Core 3.0 non richiede che questi servizi siano inclusi nel contenitore di inserimento delle dipendenze dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-537">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="3ebc6-538">Tuttavia, se `ILoggerFactory` è registrato nel contenitore di inserimento delle dipendenze dell'applicazione, verrà ancora usato da EF Core.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-538">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="3ebc6-539">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-539">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-540">Se l'applicazione necessita di questi servizi, registrarli in modo esplicito con il contenitore di inserimento delle dipendenze usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="3ebc6-540">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="3ebc6-541">DbContext.Entry esegue ora un DetectChanges locale</span><span class="sxs-lookup"><span data-stu-id="3ebc6-541">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="3ebc6-542">Problema n. 13552</span><span class="sxs-lookup"><span data-stu-id="3ebc6-542">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="3ebc6-543">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-543">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-544">Nelle versioni precedenti a EF Core 3.0 la chiamata a `DbContext.Entry` causava il rilevamento delle modifiche per tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-544">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="3ebc6-545">Ciò garantiva l'aggiornamento dello stato esposto in `EntityEntry`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-545">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="3ebc6-546">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-546">**New behavior**</span></span>

<span data-ttu-id="3ebc6-547">A partire da EF Core 3.0, la chiamata a `DbContext.Entry` causa ora solo il tentativo di rilevare le modifiche nell'entità specificata e in tutte le relative entità di sicurezza rilevate.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-547">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="3ebc6-548">Ciò significa che le modifiche apportate altrove potrebbero non essere state rilevate tramite la chiamata al metodo e ciò potrebbe avere implicazioni sullo stato dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-548">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="3ebc6-549">Si noti che se `ChangeTracker.AutoDetectChangesEnabled` è impostato su `false`, verrà disabilitato anche questo tipo di rilevamento delle modifiche locali.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-549">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="3ebc6-550">Gli altri metodi che causano il rilevamento delle modifiche, ad esempio `ChangeTracker.Entries` e `SaveChanges`, causano ancora un `DetectChanges` completo di tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-550">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="3ebc6-551">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-551">**Why**</span></span>

<span data-ttu-id="3ebc6-552">Questa modifica è stata apportata per migliorare le prestazioni predefinite dell'uso di `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-552">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="3ebc6-553">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-553">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-554">Chiamare `ChgangeTracker.DetectChanges()` in modo esplicito prima di chiamare `Entry` per garantire il comportamento precedente alla versione 3.0.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-554">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="3ebc6-555">Le chiavi matrice di byte e di stringhe non vengono generate dal client per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="3ebc6-555">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="3ebc6-556">Problema n. 14617</span><span class="sxs-lookup"><span data-stu-id="3ebc6-556">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="3ebc6-557">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-557">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-558">Nelle versioni precedenti a EF Core 3.0 le proprietà di chiave `string` e `byte[]` potevano essere usate senza impostare in modo esplicito un valore non Null.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-558">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="3ebc6-559">In questi casi, il valore di chiave veniva generato nel client come GUID, serializzato in byte per `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-559">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="3ebc6-560">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-560">**New behavior**</span></span>

<span data-ttu-id="3ebc6-561">A partire da EF Core 3.0 viene generata un'eccezione che indica che non è stato impostato alcun valore di chiave.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-561">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="3ebc6-562">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-562">**Why**</span></span>

<span data-ttu-id="3ebc6-563">Questa modifica è stata apportata poiché i valori `string`/`byte[]` generati dal client non risultano in genere utili e il comportamento predefinito rendeva complesso ragionare sui valori di chiave generati in un modo comune.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-563">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="3ebc6-564">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-564">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-565">È possibile ripristinare il comportamento precedente alla versione 3.0 specificando in modo esplicito che le proprietà di chiave devono usare i valori generati se non viene impostato alcun altro valore non Null.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-565">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="3ebc6-566">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-566">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="3ebc6-567">Oppure con annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-567">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="3ebc6-568">ILoggerFactory è ora un servizio con ambito</span><span class="sxs-lookup"><span data-stu-id="3ebc6-568">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="3ebc6-569">Problema n. 14698</span><span class="sxs-lookup"><span data-stu-id="3ebc6-569">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="3ebc6-570">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-570">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-571">Nelle versioni precedenti a EF Core 3.0 `ILoggerFactory` veniva registrato come servizio singleton.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-571">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="3ebc6-572">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-572">**New behavior**</span></span>

<span data-ttu-id="3ebc6-573">A partire da EF Core 3.0, `ILoggerFactory` viene registrato come servizio con ambito.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-573">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="3ebc6-574">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-574">**Why**</span></span>

<span data-ttu-id="3ebc6-575">Questa modifica è stata apportata per consentire l'associazione di un logger a un'istanza `DbContext` che abilita altre funzionalità e rimuove alcuni casi di comportamento anomalo, ad esempio un'esplosione dei provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-575">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="3ebc6-576">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-576">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-577">Questa modifica non dovrebbe influire sul codice dell'applicazione a meno che non vengano registrati e usati servizi personalizzati nel provider di servizi interno di EF Core.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-577">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="3ebc6-578">Questa condizione, tuttavia, non è comune.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-578">This isn't common.</span></span>
<span data-ttu-id="3ebc6-579">In questi casi la maggior parte delle operazioni continuano a essere eseguite correttamente, ma qualsiasi servizio singleton dipendente da `ILoggerFactory` dovrà essere modificato per ottenere `ILoggerFactory` in modo diverso.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-579">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="3ebc6-580">Se si verificano situazioni simili, inviare una segnalazione nello [strumento di gestione dei problemi in GitHub relativo a EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) per comunicare in che modo viene usato `ILoggerFactory` per consentirci di comprendere meglio come evitare ulteriori interruzioni in futuro.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-580">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="3ebc6-581">I proxy di caricamento lazy non presuppongono più che le proprietà di navigazione vengano caricate completamente</span><span class="sxs-lookup"><span data-stu-id="3ebc6-581">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="3ebc6-582">Problema n. 12780</span><span class="sxs-lookup"><span data-stu-id="3ebc6-582">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="3ebc6-583">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-583">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-584">Nelle versioni precedenti a EF Core 3.0, quando `DbContext` veniva eliminato non esisteva alcun metodo per scoprire se una determinata proprietà di navigazione in un'entità ottenuta da un contesto specifico veniva caricata completamente o meno.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-584">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="3ebc6-585">I proxy presupponevano invece che venisse caricata una navigazione di riferimento se era presente un valore non Null e che venisse caricata una navigazione di raccolta se era presente un valore.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-585">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="3ebc6-586">In questi casi il tentativo di eseguire un caricamento lazy non avrebbe avuto alcun esito.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-586">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="3ebc6-587">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-587">**New behavior**</span></span>

<span data-ttu-id="3ebc6-588">A partire da Entity Framework Core 3.0, i proxy tengono traccia del caricamento o mancato caricamento di una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-588">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="3ebc6-589">Ciò significa che il tentativo di accedere a una proprietà di navigazione caricata dopo l'eliminazione del contesto non avrà mai alcun esito, anche quando la navigazione caricata è vuota o ha valore Null.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-589">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="3ebc6-590">Al contrario, il tentativo di accedere a una proprietà di navigazione non caricata genera un'eccezione se il contesto viene eliminato anche se la proprietà di navigazione è una raccolta non vuota.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-590">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="3ebc6-591">Se si verifica questa situazione significa che il codice dell'applicazione sta tentando di usare il caricamento lazy in un momento non valido e l'applicazione deve essere modificata in modo da non eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-591">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="3ebc6-592">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-592">**Why**</span></span>

<span data-ttu-id="3ebc6-593">Questa modifica è stata apportata per rendere coerente e corretto il comportamento durante un tentativo di caricamento lazy in un'istanza `DbContext` eliminata.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-593">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="3ebc6-594">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-594">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-595">Aggiornare il codice dell'applicazione per fare in modo che non venga tentato il caricamento lazy con un contesto eliminato oppure specificare una configurazione in modo che non venga eseguita alcuna operazione come descritto nel messaggio di eccezione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-595">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="3ebc6-596">La creazione di un numero eccessivo di provider di servizi interni è ora un errore per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="3ebc6-596">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="3ebc6-597">Problema n. 10236</span><span class="sxs-lookup"><span data-stu-id="3ebc6-597">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="3ebc6-598">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-598">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-599">Nelle versioni precedenti a EF Core 3.0 veniva registrato un avviso per le applicazioni che creavano un numero eccessivo di provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-599">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="3ebc6-600">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-600">**New behavior**</span></span>

<span data-ttu-id="3ebc6-601">A partire da EF Core 3.0, l'avviso viene considerato un errore e viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-601">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="3ebc6-602">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-602">**Why**</span></span>

<span data-ttu-id="3ebc6-603">Questa modifica è stata apportata per gestire meglio il codice dell'applicazione tramite un'esposizione più esplicita di questa situazione di errore.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-603">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="3ebc6-604">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-604">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-605">L'azione più appropriata quando si verifica questo errore consiste nell'individuare la causa radice e nell'interrompere la creazione di numerosi provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-605">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="3ebc6-606">È possibile tuttavia convertire nuovamente l'errore in avviso o ignorarlo tramite una configurazione in `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-606">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="3ebc6-607">Esempio:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-607">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="3ebc6-608">Nuovo comportamento per la chiamata di HasOne/HasMany con una singola stringa</span><span class="sxs-lookup"><span data-stu-id="3ebc6-608">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="3ebc6-609">Problema n. 9171</span><span class="sxs-lookup"><span data-stu-id="3ebc6-609">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="3ebc6-610">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-610">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-611">Prima di EF Core 3.0, il codice che chiama `HasOne` o `HasMany` con una singola stringa era interpretato in modo poco chiaro.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-611">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="3ebc6-612">Esempio:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-612">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="3ebc6-613">Apparentemente, il codice mette in relazione `Samurai` con un altro tipo di entità tramite la proprietà di navigazione `Entrance`, che può essere privata.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-613">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="3ebc6-614">In realtà, il codice tenta di creare una relazione con un tipo di entità denominato `Entrance` senza proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-614">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="3ebc6-615">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-615">**New behavior**</span></span>

<span data-ttu-id="3ebc6-616">A partire da EF Core 3.0, il codice sopra riportato ora esegue quello che avrebbe dovuto fare in precedenza.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-616">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="3ebc6-617">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-617">**Why**</span></span>

<span data-ttu-id="3ebc6-618">Il comportamento precedente era molto poco chiaro, soprattutto durante la lettura del codice di configurazione e la ricerca di errori.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-618">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="3ebc6-619">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-619">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-620">Questa modifica causerà problemi solo nelle applicazioni che configurano relazioni in modo esplicito usando stringhe per i nomi dei tipi e senza specificare in modo esplicito la proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-620">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="3ebc6-621">Non è uno scenario comune.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-621">This is not common.</span></span>
<span data-ttu-id="3ebc6-622">Il comportamento precedente può essere ottenuto passando esplicitamente `null` per il nome della proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-622">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="3ebc6-623">Esempio:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-623">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="3ebc6-624">Il tipo restituito per diversi metodi asincroni è cambiato da Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="3ebc6-624">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="3ebc6-625">Problema n. 15184</span><span class="sxs-lookup"><span data-stu-id="3ebc6-625">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="3ebc6-626">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-626">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-627">I metodi asincroni seguenti in precedenza restituivano il tipo `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-627">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="3ebc6-628">`ValueGenerator.NextValueAsync()` (e classi derivate)</span><span class="sxs-lookup"><span data-stu-id="3ebc6-628">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="3ebc6-629">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-629">**New behavior**</span></span>

<span data-ttu-id="3ebc6-630">I metodi indicati in precedenza ora restituiscono il tipo `ValueTask<T>` sullo stesso `T` come in precedenza.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-630">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="3ebc6-631">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-631">**Why**</span></span>

<span data-ttu-id="3ebc6-632">Questa modifica riduce il numero delle allocazioni di heap sostenute quando si richiamano questi metodi, con un miglioramento generale delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-632">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="3ebc6-633">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-633">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-634">Le applicazioni semplicemente in attesa delle API precedenti devono solo essere ricompilate e non sono richieste modifiche del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-634">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="3ebc6-635">Per scenari di utilizzo più complessi (ad esempio, il passaggio del tipo `Task` restituito a `Task.WhenAny()`) è richiesto in genere che il tipo `ValueTask<T>` restituito venga convertito in `Task<T>` chiamando `AsTask()` su di esso.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-635">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="3ebc6-636">Si noti che in questo modo si annulla la riduzione delle allocazioni consentita da questa modifica.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-636">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="3ebc6-637">L'annotazione Relational:TypeMapping è ora TypeMapping</span><span class="sxs-lookup"><span data-stu-id="3ebc6-637">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="3ebc6-638">Problema n. 9913</span><span class="sxs-lookup"><span data-stu-id="3ebc6-638">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="3ebc6-639">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-639">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-640">Il nome di annotazione delle annotazioni di mapping del tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="3ebc6-640">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="3ebc6-641">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-641">**New behavior**</span></span>

<span data-ttu-id="3ebc6-642">Il nome di annotazione delle annotazioni di mapping del tipo è ora "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="3ebc6-642">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="3ebc6-643">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-643">**Why**</span></span>

<span data-ttu-id="3ebc6-644">Il mapping dei tipi non viene più usato solo per i provider di database relazionali.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-644">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="3ebc6-645">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-645">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-646">Ciò causa un'interruzione solo nelle applicazioni che accedono al mapping dei tipi direttamente come annotazione. Questa situazione non è comune.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-646">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="3ebc6-647">L'azione più appropriata per risolvere il problema consiste nell'usare la superficie dell'API per accedere al mapping dei tipi anziché l'annotazione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-647">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="3ebc6-648">ToTable in un tipo derivato genera un'eccezione</span><span class="sxs-lookup"><span data-stu-id="3ebc6-648">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="3ebc6-649">Problema n. 11811</span><span class="sxs-lookup"><span data-stu-id="3ebc6-649">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="3ebc6-650">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-650">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-651">Nelle versioni precedenti a EF Core 3.0, la chiamata a `ToTable()` in un tipo derivato veniva ignorata poiché soltanto la strategia di mapping dell'ereditarietà era una tabella per gerarchia dove la chiamata non era valida.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-651">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="3ebc6-652">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-652">**New behavior**</span></span>

<span data-ttu-id="3ebc6-653">A partire da EF Core 3.0 e in preparazione all'aggiunta del supporto per la tabella per tipo e per TPC in una versione successiva, la chiamata a `ToTable()` in un tipo derivato genera un'eccezione per evitare una modifica del mapping imprevista in futuro.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-653">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="3ebc6-654">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-654">**Why**</span></span>

<span data-ttu-id="3ebc6-655">Attualmente il mapping di un tipo derivato in una tabella diversa non è un'operazione valida.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-655">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="3ebc6-656">Questa modifica consente di evitare interruzioni future quando l'operazione diventerà un'operazione valida.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-656">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="3ebc6-657">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-657">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-658">Rimuovere qualsiasi tentativo di mapping di tipi derivati in altre tabelle.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-658">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="3ebc6-659">ForSqlServerHasIndex sostituito con HasIndex</span><span class="sxs-lookup"><span data-stu-id="3ebc6-659">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="3ebc6-660">Problema n. 12366</span><span class="sxs-lookup"><span data-stu-id="3ebc6-660">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="3ebc6-661">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-661">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-662">Nelle versioni precedenti a EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offriva un metodo per configurare le colonne usate con `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-662">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="3ebc6-663">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-663">**New behavior**</span></span>

<span data-ttu-id="3ebc6-664">A partire da EF Core 3.0, l'uso di `Include` in un indice è supportato a livello relazionale.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-664">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="3ebc6-665">Usare `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-665">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="3ebc6-666">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-666">**Why**</span></span>

<span data-ttu-id="3ebc6-667">Questa modifica è stata apportata per consolidare l'API per gli indici con `Include` in un'unica posizione per tutti i provider di database.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-667">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="3ebc6-668">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-668">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-669">Usare la nuova API, come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-669">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="3ebc6-670">Modifiche dell'API dei metadati</span><span class="sxs-lookup"><span data-stu-id="3ebc6-670">Metadata API changes</span></span>

[<span data-ttu-id="3ebc6-671">Problema n. 214</span><span class="sxs-lookup"><span data-stu-id="3ebc6-671">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="3ebc6-672">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-672">**New behavior**</span></span>

<span data-ttu-id="3ebc6-673">Le proprietà seguenti sono state convertite in metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-673">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="3ebc6-674">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-674">**Why**</span></span>

<span data-ttu-id="3ebc6-675">Questa modifica semplifica l'implementazione delle interfacce menzionate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-675">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="3ebc6-676">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-676">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-677">Usare i nuovi metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-677">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="3ebc6-678">Modifiche dell'API dei metadati specifiche del provider</span><span class="sxs-lookup"><span data-stu-id="3ebc6-678">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="3ebc6-679">Problema n. 214</span><span class="sxs-lookup"><span data-stu-id="3ebc6-679">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="3ebc6-680">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-680">**New behavior**</span></span>

<span data-ttu-id="3ebc6-681">I metodi di estensione specifici del provider verranno resi flat:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-681">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="3ebc6-682">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-682">**Why**</span></span>

<span data-ttu-id="3ebc6-683">Questa modifica semplifica l'implementazione dei metodi di estensione menzionati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-683">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="3ebc6-684">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-684">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-685">Usare i nuovi metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-685">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="3ebc6-686">EF Core non invia più pragma per l'imposizione della chiave esterna di SQLite</span><span class="sxs-lookup"><span data-stu-id="3ebc6-686">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="3ebc6-687">Problema n. 12151</span><span class="sxs-lookup"><span data-stu-id="3ebc6-687">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="3ebc6-688">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-688">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-689">Nelle versioni precedenti a EF Core 3.0, EF Core inviava `PRAGMA foreign_keys = 1` quando veniva aperta una connessione a SQLite.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-689">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="3ebc6-690">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-690">**New behavior**</span></span>

<span data-ttu-id="3ebc6-691">A partire da EF Core 3.0, EF Core non invia più `PRAGMA foreign_keys = 1` quando viene aperta una connessione a SQLite.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-691">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="3ebc6-692">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-692">**Why**</span></span>

<span data-ttu-id="3ebc6-693">Questa modifica è stata apportata poiché EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` per impostazione predefinita. Ciò significa che l'imposizione della chiave esterna è abilitata per impostazione predefinita e non deve essere abilitata in modo esplicito ogni volta che viene aperta una connessione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-693">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="3ebc6-694">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-694">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-695">Le chiavi esterne sono abilitate per impostazione predefinita in SQLitePCLRaw.bundle_e_sqlite3, usato per impostazione predefinita per EF Core.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-695">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="3ebc6-696">Per gli altri casi, è possibile abilitare le chiavi esterne specificando `Foreign Keys=True` nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-696">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="3ebc6-697">Microsoft.EntityFrameworkCore.Sqlite dipende ora da SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="3ebc6-697">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="3ebc6-698">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-698">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-699">Nelle versioni precedenti a EF Core 3.0, EF Core usava `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-699">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="3ebc6-700">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-700">**New behavior**</span></span>

<span data-ttu-id="3ebc6-701">A partire da EF Core 3.0, EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-701">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="3ebc6-702">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-702">**Why**</span></span>

<span data-ttu-id="3ebc6-703">Questa modifica è stata apportata per rendere coerente la versione di SQLite usata in iOS con le altre piattaforme.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-703">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="3ebc6-704">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-704">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-705">Per usare la versione di SQLite nativa in iOS, configurare `Microsoft.Data.Sqlite` per l'uso di un'aggregazione `SQLitePCLRaw` diversa.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-705">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="3ebc6-706">I valori Guid vengono ora archiviati come TEXT in SQLite</span><span class="sxs-lookup"><span data-stu-id="3ebc6-706">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="3ebc6-707">Problema n. 15078</span><span class="sxs-lookup"><span data-stu-id="3ebc6-707">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="3ebc6-708">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-708">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-709">I valori Guid in precedenza venivano archiviati come valori BLOB in SQLite.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-709">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="3ebc6-710">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-710">**New behavior**</span></span>

<span data-ttu-id="3ebc6-711">I valori Guid vengono ora archiviati come TEXT.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-711">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="3ebc6-712">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-712">**Why**</span></span>

<span data-ttu-id="3ebc6-713">Il formato binario dei valori Guid non è standardizzato.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-713">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="3ebc6-714">L'archiviazione dei valori come TEXT rende il database più compatibile con altre tecnologie.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-714">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="3ebc6-715">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-715">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-716">È possibile eseguire la migrazione dei database esistenti al nuovo formato eseguendo SQL nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-716">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="3ebc6-717">In EF Core è anche possibile continuare a usare il comportamento precedente configurando un convertitore di valori per queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-717">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="3ebc6-718">Microsoft.Data.Sqlite rimane in grado di leggere i valori Guid sia da colonne BLOB che TEXT. Tuttavia, poiché è stato modificato il formato predefinito per i parametri e le costanti, probabilmente sarà necessario intervenire per la maggior parte degli scenari che coinvolgono valori Guid.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-718">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="3ebc6-719">I valori char vengono ora archiviati come testo in SQLite</span><span class="sxs-lookup"><span data-stu-id="3ebc6-719">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="3ebc6-720">Problema n. 15020</span><span class="sxs-lookup"><span data-stu-id="3ebc6-720">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="3ebc6-721">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-721">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-722">I valori char in precedenza venivano archiviati come valori interi in SQLite.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-722">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="3ebc6-723">Un valore char di *A* veniva ad esempio archiviato come valore intero 65.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-723">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="3ebc6-724">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-724">**New behavior**</span></span>

<span data-ttu-id="3ebc6-725">I valori char vengono ora archiviati come testo.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-725">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="3ebc6-726">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-726">**Why**</span></span>

<span data-ttu-id="3ebc6-727">L'archiviazione dei valori come testo è un'operazione più naturale e rende il database più compatibile con altre tecnologie.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-727">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="3ebc6-728">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-728">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-729">È possibile eseguire la migrazione dei database esistenti al nuovo formato eseguendo SQL nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-729">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="3ebc6-730">In EF Core è anche possibile continuare a usare il comportamento precedente configurando un convertitore di valori per queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-730">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="3ebc6-731">Microsoft.Data.Sqlite rimane comunque in grado di leggere i valori di caratteri presenti sia nelle colonne di valori interi sia in quelle di testo, quindi alcuni scenari potrebbero non richiedere alcuna azione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-731">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="3ebc6-732">Gli ID di migrazione vengono ora generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica</span><span class="sxs-lookup"><span data-stu-id="3ebc6-732">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="3ebc6-733">Problema n. 12978</span><span class="sxs-lookup"><span data-stu-id="3ebc6-733">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="3ebc6-734">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-734">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-735">Gli ID di migrazione venivano inavvertitamente generati usando il calendario delle impostazioni cultura correnti.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-735">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="3ebc6-736">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-736">**New behavior**</span></span>

<span data-ttu-id="3ebc6-737">Gli ID di migrazione ora vengono sempre generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica (calendario gregoriano).</span><span class="sxs-lookup"><span data-stu-id="3ebc6-737">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="3ebc6-738">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-738">**Why**</span></span>

<span data-ttu-id="3ebc6-739">L'ordine delle migrazioni è importante quando si esegue l'aggiornamento del database o si risolvono i conflitti di unione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-739">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="3ebc6-740">L'uso del calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica evita i problemi che possono verificarsi quando i membri del team hanno calendari di sistema diversi.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-740">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="3ebc6-741">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-741">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-742">Questa modifica interessa gli utenti che usano un calendario non gregoriano in cui l'anno ha un'estensione superiore al calendario gregoriano (come il calendario buddista tailandese).</span><span class="sxs-lookup"><span data-stu-id="3ebc6-742">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="3ebc6-743">Gli ID di migrazione esistenti dovranno essere aggiornati in modo che le nuove migrazioni vengano collocate dopo le migrazioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-743">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="3ebc6-744">L'ID di migrazione è disponibile nell'attributo di migrazione presente nei file di progettazione delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-744">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="3ebc6-745">È necessario aggiornare anche la tabella della cronologia delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-745">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="3ebc6-746">Il metodo UseRowNumberForPaging è stato rimosso</span><span class="sxs-lookup"><span data-stu-id="3ebc6-746">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="3ebc6-747">Problema n. 16400</span><span class="sxs-lookup"><span data-stu-id="3ebc6-747">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="3ebc6-748">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-748">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-749">Prima di EF Core 3.0 si poteva usare `UseRowNumberForPaging` per generare codice SQL per la suddivisione in pagine compatibile con SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-749">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="3ebc6-750">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-750">**New behavior**</span></span>

<span data-ttu-id="3ebc6-751">A partire da EF Core 3.0, EF genererà solo codice SQL per la suddivisione in pagine compatibile solo con versioni successive di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-751">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="3ebc6-752">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-752">**Why**</span></span>

<span data-ttu-id="3ebc6-753">Questa modifica è stata apportata perché [SQL Server 2008 non è più un prodotto supportato](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) e l'aggiornamento di questa funzionalità per interagire con le modifiche apportate per le query in EF Core 3.0 è un lavoro significativo.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-753">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="3ebc6-754">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-754">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-755">È consigliabile eseguire l'aggiornamento a una versione più recente di SQL Server o usare un livello di compatibilità superiore, in modo che il codice SQL generato sia supportato.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-755">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="3ebc6-756">Detto questo, se non è possibile procedere in questo modo, [aggiungere un commento per il problema](https://github.com/aspnet/EntityFrameworkCore/issues/16400) con indicazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-756">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="3ebc6-757">Microsoft potrebbe rivedere questa decisione in base ai commenti e suggerimenti.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-757">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="3ebc6-758">Info/metadati dell'estensione rimossi da IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="3ebc6-758">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="3ebc6-759">Problema n. 16119</span><span class="sxs-lookup"><span data-stu-id="3ebc6-759">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="3ebc6-760">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-760">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-761">`IDbContextOptionsExtension` conteneva metodi per fornire i metadati relativi all'estensione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-761">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="3ebc6-762">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-762">**New behavior**</span></span>

<span data-ttu-id="3ebc6-763">Questi metodi sono stati spostati in una nuova classe di base astratta `DbContextOptionsExtensionInfo`, restituita da una nuova proprietà `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-763">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="3ebc6-764">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-764">**Why**</span></span>

<span data-ttu-id="3ebc6-765">Nelle versioni dalla 2.0 alla 3.0 è stato necessario aggiungere o modificare questi metodi più volte.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-765">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="3ebc6-766">Suddividendoli in una nuova classe di base astratta sarà più facile apportare questo tipo di modifiche senza compromettere il funzionamento delle estensioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-766">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="3ebc6-767">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-767">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-768">Aggiornare le estensioni per seguire il nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-768">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="3ebc6-769">Sono disponibili esempi nelle numerose implementazioni di `IDbContextOptionsExtension` per diversi tipi di estensioni nel codice sorgente di EF Core.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-769">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="3ebc6-770">LogQueryPossibleExceptionWithAggregateOperator è stato rinominato</span><span class="sxs-lookup"><span data-stu-id="3ebc6-770">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="3ebc6-771">Problema n. 10985</span><span class="sxs-lookup"><span data-stu-id="3ebc6-771">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="3ebc6-772">**Modifica**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-772">**Change**</span></span>

<span data-ttu-id="3ebc6-773">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` è stato rinominato in `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-773">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="3ebc6-774">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-774">**Why**</span></span>

<span data-ttu-id="3ebc6-775">Allineamento del nome di questo evento di avviso con tutti gli altri eventi di avviso.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-775">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="3ebc6-776">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-776">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-777">Usare il nuovo nome.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-777">Use the new name.</span></span> <span data-ttu-id="3ebc6-778">(Si noti che il numero di ID evento non è stato modificato.)</span><span class="sxs-lookup"><span data-stu-id="3ebc6-778">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="3ebc6-779">Chiarimenti per l'API per i nomi di vincolo di chiave esterna</span><span class="sxs-lookup"><span data-stu-id="3ebc6-779">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="3ebc6-780">Problema n. 10730</span><span class="sxs-lookup"><span data-stu-id="3ebc6-780">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="3ebc6-781">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-781">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-782">Prima di EF Core 3.0, si faceva riferimento ai nomi di vincolo di chiave esterna semplicemente con "Name".</span><span class="sxs-lookup"><span data-stu-id="3ebc6-782">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="3ebc6-783">Esempio:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-783">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="3ebc6-784">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-784">**New behavior**</span></span>

<span data-ttu-id="3ebc6-785">A partire da EF Core 3.0, si fa ora riferimento ai nomi di vincolo di chiave esterna con "ConstraintName".</span><span class="sxs-lookup"><span data-stu-id="3ebc6-785">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="3ebc6-786">Esempio:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-786">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="3ebc6-787">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-787">**Why**</span></span>

<span data-ttu-id="3ebc6-788">Questa modifica introduce coerenza per la denominazione in quest'area e chiarisce anche che si tratta del nome del vincolo di chiave esterna e non del nome della colonna o della proprietà per cui è definita la chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-788">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="3ebc6-789">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-789">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-790">Usare il nuovo nome.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-790">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="3ebc6-791">IRelationalDatabaseCreator.HasTables/HasTablesAsync sono diventati pubblici</span><span class="sxs-lookup"><span data-stu-id="3ebc6-791">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="3ebc6-792">Problema n. 15997</span><span class="sxs-lookup"><span data-stu-id="3ebc6-792">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="3ebc6-793">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-793">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-794">Prima di EF Core 3.0 questi metodi erano protetti.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-794">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="3ebc6-795">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-795">**New behavior**</span></span>

<span data-ttu-id="3ebc6-796">A partire da EF Core 3.0 questi metodi sono pubblici.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-796">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="3ebc6-797">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-797">**Why**</span></span>

<span data-ttu-id="3ebc6-798">Questi metodi vengono usati da EF per determinare se un database viene creato, ma vuoto.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-798">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="3ebc6-799">Ciò risulta utile all'esterno di EF quando occorre determinare se applicare o meno le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-799">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="3ebc6-800">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-800">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-801">Modificare l'accessibilità di eventuali override.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-801">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="3ebc6-802">Microsoft.EntityFrameworkCore.Design è ora un pacchetto DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="3ebc6-802">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="3ebc6-803">Problema n. 11506</span><span class="sxs-lookup"><span data-stu-id="3ebc6-803">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="3ebc6-804">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-804">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-805">Prima di EF Core 3.0, Microsoft.EntityFrameworkCore.Design era un pacchetto NuGet normale ed era possibile fare riferimento al relativo assembly dai progetti dipendenti.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-805">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="3ebc6-806">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-806">**New behavior**</span></span>

<span data-ttu-id="3ebc6-807">A partire da EF Core 3.0 è un pacchetto DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-807">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="3ebc6-808">Questo significa che la dipendenza non verrà trasferita in modo transitivo in altri progetti e che non è più possibile fare riferimento all'assembly per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-808">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="3ebc6-809">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-809">**Why**</span></span>

<span data-ttu-id="3ebc6-810">Questo pacchetto è destinato solo all'uso in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-810">This package is only intended to be used at design time.</span></span> <span data-ttu-id="3ebc6-811">Le applicazioni distribuite non devono farvi riferimento.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-811">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="3ebc6-812">Questa raccomandazione è rafforzata dall'impostazione del pacchetto come DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-812">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="3ebc6-813">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-813">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-814">Se è necessario fare riferimento a questo pacchetto per eseguire l'override del comportamento in fase di progettazione di EF Core, è possibile aggiornare i metadati dell'elemento PackageReference nel progetto.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-814">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="3ebc6-815">In presenza di riferimenti transitivi al pacchetto tramite Microsoft.EntityFrameworkCore.Tools, sarà necessario aggiungere un PackageReference esplicito al pacchetto per modificare i relativi metadati.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-815">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="3ebc6-816">Aggiornamento di SQLitePCL.raw alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="3ebc6-816">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="3ebc6-817">Problema n. 14824</span><span class="sxs-lookup"><span data-stu-id="3ebc6-817">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="3ebc6-818">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-818">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-819">Microsoft.EntityFrameworkCore.Sqlite dipendeva in precedenza dalla versione 1.1.12 di SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-819">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="3ebc6-820">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-820">**New behavior**</span></span>

<span data-ttu-id="3ebc6-821">Il pacchetto è stato aggiornato in modo da dipendere dalla versione 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-821">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="3ebc6-822">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-822">**Why**</span></span>

<span data-ttu-id="3ebc6-823">La versione 2.0.0 di SQLitePCL.raw è destinata a .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-823">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="3ebc6-824">Era in precedenza destinata a .NET Standard 1.1 e ciò richiedeva un notevole impegno di chiusura di pacchetti transitivi per il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-824">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="3ebc6-825">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-825">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-826">SQLitePCL.raw versione 2.0.0 include alcune modifiche che causano un'interruzione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-826">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="3ebc6-827">Per informazioni dettagliate, vedere le [note sulla versione](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="3ebc6-827">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="3ebc6-828">NetTopologySuite aggiornato alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="3ebc6-828">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="3ebc6-829">Problema n. 14825</span><span class="sxs-lookup"><span data-stu-id="3ebc6-829">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="3ebc6-830">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-830">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-831">I pacchetti spaziali dipendevano in precedenza dalla versione 1.15.1 di NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-831">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="3ebc6-832">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-832">**New behavior**</span></span>

<span data-ttu-id="3ebc6-833">Il pacchetto è stato aggiornato in modo da dipendere dalla versione 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-833">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="3ebc6-834">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-834">**Why**</span></span>

<span data-ttu-id="3ebc6-835">La versione 2.0.0 di NetTopologySuite risolve vari problemi di usabilità riscontrati dagli utenti di EF Core.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-835">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="3ebc6-836">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-836">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-837">NetTopologySuite versione 2.0.0 include alcune modifiche che causano un'interruzione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-837">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="3ebc6-838">Per informazioni dettagliate, vedere le [note sulla versione](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="3ebc6-838">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="3ebc6-839">Devono essere configurare più relazioni ambigue che fanno riferimento a se stesse</span><span class="sxs-lookup"><span data-stu-id="3ebc6-839">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="3ebc6-840">Problema n. 13573</span><span class="sxs-lookup"><span data-stu-id="3ebc6-840">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="3ebc6-841">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-841">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-842">Un tipo di entità con più proprietà di navigazione unidirezionale che fanno riferimento a se stesse e più chiavi esterne corrispondenti è stato erroneamente configurato come relazione singola.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-842">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="3ebc6-843">Esempio:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-843">For example:</span></span>

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

<span data-ttu-id="3ebc6-844">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-844">**New behavior**</span></span>

<span data-ttu-id="3ebc6-845">Questo scenario viene ora rilevato nella compilazione del modello e viene generata un'eccezione indicante che il modello è ambiguo.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-845">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="3ebc6-846">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-846">**Why**</span></span>

<span data-ttu-id="3ebc6-847">Il modello risultante era ambiguo e sarà probabilmente errato in questo caso.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-847">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="3ebc6-848">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-848">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-849">Usare la configurazione completa della relazione.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-849">Use full configuration of the relationship.</span></span> <span data-ttu-id="3ebc6-850">Esempio:</span><span class="sxs-lookup"><span data-stu-id="3ebc6-850">For example:</span></span>

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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="3ebc6-851">DbFunction. Schema è una stringa null o vuota che lo configura in modo che sia nello schema predefinito del modello</span><span class="sxs-lookup"><span data-stu-id="3ebc6-851">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="3ebc6-852">Rilevamento del problema #12757</span><span class="sxs-lookup"><span data-stu-id="3ebc6-852">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="3ebc6-853">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-853">**Old behavior**</span></span>

<span data-ttu-id="3ebc6-854">Un DbFunction configurato con schema come stringa vuota è stato trattato come funzione predefinita senza uno schema.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-854">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="3ebc6-855">Il codice seguente, ad esempio, eseguirà il mapping della funzione CLR `DatePart` alla funzione predefinita `DATEPART` in SqlServer.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-855">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="3ebc6-856">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-856">**New behavior**</span></span>

<span data-ttu-id="3ebc6-857">Tutti i mapping di DbFunction sono considerati mappati alle funzioni definite dall'utente.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-857">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="3ebc6-858">Pertanto, il valore stringa vuoto inserisce la funzione all'interno dello schema predefinito per il modello.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-858">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="3ebc6-859">Che potrebbe essere lo schema configurato in modo esplicito tramite l'API Fluent `modelBuilder.HasDefaultSchema()` o `dbo` in caso contrario.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-859">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="3ebc6-860">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-860">**Why**</span></span>

<span data-ttu-id="3ebc6-861">Lo schema precedentemente vuoto era un modo per trattare la funzione è incorporata, ma tale logica è applicabile solo per SqlServer, in cui le funzioni predefinite non appartengono ad alcuno schema.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-861">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="3ebc6-862">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="3ebc6-862">**Mitigations**</span></span>

<span data-ttu-id="3ebc6-863">Configurare manualmente la conversione di DbFunction per eseguirne il mapping a una funzione predefinita.</span><span class="sxs-lookup"><span data-stu-id="3ebc6-863">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
