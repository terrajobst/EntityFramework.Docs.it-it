---
title: Modifiche che causano un'interruzione in EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 04487291f24bb702dad4b497c34234afdd5e3c9a
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005590"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="4f72a-102">Modifiche di rilievo incluse nel EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="4f72a-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="4f72a-103">Le modifiche alle API e al comportamento seguenti possono causare l'interruzione delle applicazioni esistenti durante l'aggiornamento a 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="4f72a-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="4f72a-104">Le modifiche che si prevede abbiano impatto solo sui provider di database sono documentate nelle [modifiche che influiscono sul provider](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="4f72a-104">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="4f72a-105">Le interruzioni da un'anteprima 3,0 a un'altra anteprima 3,0 non sono documentate qui.</span><span class="sxs-lookup"><span data-stu-id="4f72a-105">Breaks from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="4f72a-106">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="4f72a-106">Summary</span></span>

| <span data-ttu-id="4f72a-107">**Modifica di rilievo**</span><span class="sxs-lookup"><span data-stu-id="4f72a-107">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="4f72a-108">**Impatto**</span><span class="sxs-lookup"><span data-stu-id="4f72a-108">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="4f72a-109">Le query LINQ non vengono più valutate nel client</span><span class="sxs-lookup"><span data-stu-id="4f72a-109">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="4f72a-110">Alto</span><span class="sxs-lookup"><span data-stu-id="4f72a-110">High</span></span>       |
| [<span data-ttu-id="4f72a-111">EF Core 3.0 usa come destinazione .NET Standard 2.1 invece che .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="4f72a-111">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="4f72a-112">Alto</span><span class="sxs-lookup"><span data-stu-id="4f72a-112">High</span></span>      |
| [<span data-ttu-id="4f72a-113">Lo strumento da riga di comando di EF Core, dotnet ef, non è più incluso in .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="4f72a-113">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="4f72a-114">Alto</span><span class="sxs-lookup"><span data-stu-id="4f72a-114">High</span></span>      |
| [<span data-ttu-id="4f72a-115">I metodi FromSql, ExecuteSql ed ExecuteSqlAsync sono stati rinominati</span><span class="sxs-lookup"><span data-stu-id="4f72a-115">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="4f72a-116">Alto</span><span class="sxs-lookup"><span data-stu-id="4f72a-116">High</span></span>      |
| [<span data-ttu-id="4f72a-117">I tipi di query vengono consolidati con tipi di entità</span><span class="sxs-lookup"><span data-stu-id="4f72a-117">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="4f72a-118">Alto</span><span class="sxs-lookup"><span data-stu-id="4f72a-118">High</span></span>      |
| [<span data-ttu-id="4f72a-119">Entity Framework Core non è più incluso nel framework condiviso di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f72a-119">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="4f72a-120">Medio</span><span class="sxs-lookup"><span data-stu-id="4f72a-120">Medium</span></span>      |
| [<span data-ttu-id="4f72a-121">Le eliminazioni a catena vengono ora eseguite immediatamente per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="4f72a-121">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="4f72a-122">Medio</span><span class="sxs-lookup"><span data-stu-id="4f72a-122">Medium</span></span>      |
| [<span data-ttu-id="4f72a-123">Semantica più chiara per DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="4f72a-123">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="4f72a-124">Medio</span><span class="sxs-lookup"><span data-stu-id="4f72a-124">Medium</span></span>      |
| [<span data-ttu-id="4f72a-125">L'API di configurazione per le relazioni di tipo di proprietà è stata modificata</span><span class="sxs-lookup"><span data-stu-id="4f72a-125">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="4f72a-126">Medio</span><span class="sxs-lookup"><span data-stu-id="4f72a-126">Medium</span></span>      |
| [<span data-ttu-id="4f72a-127">Ogni proprietà usa la generazione di chiavi di tipo intero in memoria indipendenti</span><span class="sxs-lookup"><span data-stu-id="4f72a-127">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="4f72a-128">Medio</span><span class="sxs-lookup"><span data-stu-id="4f72a-128">Medium</span></span>      |
| [<span data-ttu-id="4f72a-129">Le query senza rilevamento delle modifiche non eseguono più la risoluzione delle identità</span><span class="sxs-lookup"><span data-stu-id="4f72a-129">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="4f72a-130">Medio</span><span class="sxs-lookup"><span data-stu-id="4f72a-130">Medium</span></span>      |
| [<span data-ttu-id="4f72a-131">Modifiche dell'API dei metadati</span><span class="sxs-lookup"><span data-stu-id="4f72a-131">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="4f72a-132">Medio</span><span class="sxs-lookup"><span data-stu-id="4f72a-132">Medium</span></span>      |
| [<span data-ttu-id="4f72a-133">Modifiche dell'API dei metadati specifiche del provider</span><span class="sxs-lookup"><span data-stu-id="4f72a-133">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="4f72a-134">Medio</span><span class="sxs-lookup"><span data-stu-id="4f72a-134">Medium</span></span>      |
| [<span data-ttu-id="4f72a-135">Il metodo UseRowNumberForPaging è stato rimosso</span><span class="sxs-lookup"><span data-stu-id="4f72a-135">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="4f72a-136">Medio</span><span class="sxs-lookup"><span data-stu-id="4f72a-136">Medium</span></span>      |
| [<span data-ttu-id="4f72a-137">I metodi FromSql possono essere specificati solo in radici di query</span><span class="sxs-lookup"><span data-stu-id="4f72a-137">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="4f72a-138">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-138">Low</span></span>      |
| [<span data-ttu-id="4f72a-139">~~L'esecuzione di query viene registrata a livello di debug~~ - Modifica annullata</span><span class="sxs-lookup"><span data-stu-id="4f72a-139">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="4f72a-140">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-140">Low</span></span>      |
| [<span data-ttu-id="4f72a-141">I valori di chiave temporanei non sono più impostati nelle istanze di entità</span><span class="sxs-lookup"><span data-stu-id="4f72a-141">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="4f72a-142">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-142">Low</span></span>      |
| [<span data-ttu-id="4f72a-143">DetectChanges rispetta i valori di chiave generati dall'archivio</span><span class="sxs-lookup"><span data-stu-id="4f72a-143">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="4f72a-144">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-144">Low</span></span>      |
| [<span data-ttu-id="4f72a-145">Le entità dipendenti che condividono la tabella con l'entità di sicurezza sono ora facoltative</span><span class="sxs-lookup"><span data-stu-id="4f72a-145">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="4f72a-146">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-146">Low</span></span>      |
| [<span data-ttu-id="4f72a-147">Tutte le entità che condividono una tabella con una colonna di token di concorrenza devono eseguirne il mapping a una proprietà</span><span class="sxs-lookup"><span data-stu-id="4f72a-147">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="4f72a-148">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-148">Low</span></span>      |
| [<span data-ttu-id="4f72a-149">Per le proprietà ereditate da tipi senza mapping viene ora eseguito il mapping a una singola colonna per tutti i tipi derivati</span><span class="sxs-lookup"><span data-stu-id="4f72a-149">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="4f72a-150">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-150">Low</span></span>      |
| [<span data-ttu-id="4f72a-151">La convenzione di proprietà di chiave esterna non ha più lo stesso nome della proprietà dell'entità di sicurezza</span><span class="sxs-lookup"><span data-stu-id="4f72a-151">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="4f72a-152">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-152">Low</span></span>      |
| [<span data-ttu-id="4f72a-153">La connessione di database viene ora chiusa se non viene più usata prima del completamento di TransactionScope</span><span class="sxs-lookup"><span data-stu-id="4f72a-153">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="4f72a-154">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-154">Low</span></span>      |
| [<span data-ttu-id="4f72a-155">I campi sottostanti vengono usati per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="4f72a-155">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="4f72a-156">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-156">Low</span></span>      |
| [<span data-ttu-id="4f72a-157">Viene generata un'eccezione se vengono trovati più campi sottostanti compatibili</span><span class="sxs-lookup"><span data-stu-id="4f72a-157">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="4f72a-158">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-158">Low</span></span>      |
| [<span data-ttu-id="4f72a-159">I nomi delle proprietà solo campo devono corrispondere al nome di campo</span><span class="sxs-lookup"><span data-stu-id="4f72a-159">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="4f72a-160">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-160">Low</span></span>      |
| [<span data-ttu-id="4f72a-161">AddDbContext/AddDbContextPool non chiamano più AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="4f72a-161">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="4f72a-162">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-162">Low</span></span>      |
| [<span data-ttu-id="4f72a-163">DbContext.Entry esegue ora un DetectChanges locale</span><span class="sxs-lookup"><span data-stu-id="4f72a-163">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="4f72a-164">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-164">Low</span></span>      |
| [<span data-ttu-id="4f72a-165">Le chiavi matrice di byte e di stringhe non vengono generate dal client per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="4f72a-165">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="4f72a-166">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-166">Low</span></span>      |
| [<span data-ttu-id="4f72a-167">ILoggerFactory è ora un servizio con ambito</span><span class="sxs-lookup"><span data-stu-id="4f72a-167">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="4f72a-168">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-168">Low</span></span>      |
| [<span data-ttu-id="4f72a-169">I proxy di caricamento lazy non presuppongono più che le proprietà di navigazione vengano caricate completamente</span><span class="sxs-lookup"><span data-stu-id="4f72a-169">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="4f72a-170">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-170">Low</span></span>      |
| [<span data-ttu-id="4f72a-171">La creazione di un numero eccessivo di provider di servizi interni è ora un errore per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="4f72a-171">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="4f72a-172">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-172">Low</span></span>      |
| [<span data-ttu-id="4f72a-173">Nuovo comportamento per la chiamata di HasOne/HasMany con una singola stringa</span><span class="sxs-lookup"><span data-stu-id="4f72a-173">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="4f72a-174">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-174">Low</span></span>      |
| [<span data-ttu-id="4f72a-175">Il tipo restituito per diversi metodi asincroni è cambiato da Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="4f72a-175">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="4f72a-176">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-176">Low</span></span>      |
| [<span data-ttu-id="4f72a-177">L'annotazione Relational:TypeMapping è ora TypeMapping</span><span class="sxs-lookup"><span data-stu-id="4f72a-177">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="4f72a-178">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-178">Low</span></span>      |
| [<span data-ttu-id="4f72a-179">ToTable in un tipo derivato genera un'eccezione</span><span class="sxs-lookup"><span data-stu-id="4f72a-179">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="4f72a-180">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-180">Low</span></span>      |
| [<span data-ttu-id="4f72a-181">EF Core non invia più pragma per l'imposizione della chiave esterna di SQLite</span><span class="sxs-lookup"><span data-stu-id="4f72a-181">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="4f72a-182">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-182">Low</span></span>      |
| [<span data-ttu-id="4f72a-183">Microsoft.EntityFrameworkCore.Sqlite dipende ora da SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="4f72a-183">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="4f72a-184">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-184">Low</span></span>      |
| [<span data-ttu-id="4f72a-185">I valori Guid vengono ora archiviati come TEXT in SQLite</span><span class="sxs-lookup"><span data-stu-id="4f72a-185">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="4f72a-186">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-186">Low</span></span>      |
| [<span data-ttu-id="4f72a-187">I valori char vengono ora archiviati come testo in SQLite</span><span class="sxs-lookup"><span data-stu-id="4f72a-187">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="4f72a-188">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-188">Low</span></span>      |
| [<span data-ttu-id="4f72a-189">Gli ID di migrazione vengono ora generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica</span><span class="sxs-lookup"><span data-stu-id="4f72a-189">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="4f72a-190">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-190">Low</span></span>      |
| [<span data-ttu-id="4f72a-191">Info/metadati dell'estensione rimossi da IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="4f72a-191">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="4f72a-192">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-192">Low</span></span>      |
| [<span data-ttu-id="4f72a-193">LogQueryPossibleExceptionWithAggregateOperator è stato rinominato</span><span class="sxs-lookup"><span data-stu-id="4f72a-193">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="4f72a-194">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-194">Low</span></span>      |
| [<span data-ttu-id="4f72a-195">Chiarimenti per l'API per i nomi di vincolo di chiave esterna</span><span class="sxs-lookup"><span data-stu-id="4f72a-195">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="4f72a-196">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-196">Low</span></span>      |
| [<span data-ttu-id="4f72a-197">IRelationalDatabaseCreator.HasTables/HasTablesAsync sono diventati pubblici</span><span class="sxs-lookup"><span data-stu-id="4f72a-197">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="4f72a-198">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-198">Low</span></span>      |
| [<span data-ttu-id="4f72a-199">Microsoft.EntityFrameworkCore.Design è ora un pacchetto DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="4f72a-199">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="4f72a-200">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-200">Low</span></span>      |
| [<span data-ttu-id="4f72a-201">Aggiornamento di SQLitePCL.raw alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="4f72a-201">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="4f72a-202">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-202">Low</span></span>      |
| [<span data-ttu-id="4f72a-203">NetTopologySuite aggiornato alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="4f72a-203">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="4f72a-204">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-204">Low</span></span>      |
| [<span data-ttu-id="4f72a-205">Devono essere configurare più relazioni ambigue che fanno riferimento a se stesse</span><span class="sxs-lookup"><span data-stu-id="4f72a-205">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="4f72a-206">Basso</span><span class="sxs-lookup"><span data-stu-id="4f72a-206">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="4f72a-207">Le query LINQ non vengono più valutate nel client</span><span class="sxs-lookup"><span data-stu-id="4f72a-207">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="4f72a-208">[Problema n. 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Vedere anche il problema n. 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="4f72a-208">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="4f72a-209">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-209">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-210">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-210">**Old behavior**</span></span>

<span data-ttu-id="4f72a-211">Nelle versioni precedenti alla versione 3.0, quando EF Core non era in grado di convertire un'espressione inclusa in una query in SQL o in un parametro, l'espressione veniva automaticamente valutata nel client.</span><span class="sxs-lookup"><span data-stu-id="4f72a-211">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="4f72a-212">Per impostazione predefinita, la valutazione client di espressioni potenzialmente dispendiose si limitava ad attivare solo un avviso.</span><span class="sxs-lookup"><span data-stu-id="4f72a-212">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="4f72a-213">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-213">**New behavior**</span></span>

<span data-ttu-id="4f72a-214">A partire dalla versione 3.0, EF Core consente solo la valutazione delle espressioni nella proiezione di primo livello (l'ultima chiamata `Select()` nella query) nel client.</span><span class="sxs-lookup"><span data-stu-id="4f72a-214">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="4f72a-215">Quando le espressioni in altre posizioni all'interno della query non possono essere convertite in SQL o in un parametro, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-215">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="4f72a-216">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-216">**Why**</span></span>

<span data-ttu-id="4f72a-217">La valutazione client automatica delle query consente di eseguire numerose query anche nel caso in cui parti importanti delle query non possono essere convertite.</span><span class="sxs-lookup"><span data-stu-id="4f72a-217">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="4f72a-218">Questo comportamento può causare un comportamento imprevisto e potenzialmente dannoso che può diventare evidente solo in produzione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-218">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="4f72a-219">Ad esempio, una condizione in una chiamata `Where()` che non può essere convertita può causare il trasferimento di tutte le righe della tabella del server di database e l'applicazione del filtro nel client.</span><span class="sxs-lookup"><span data-stu-id="4f72a-219">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="4f72a-220">È probabile che questa situazione non venga rilevata se la tabella contiene solo alcune righe in fase di sviluppo, ma che abbia un grande impatto quando l'applicazione passa in produzione dove la tabella può contenere milioni di righe.</span><span class="sxs-lookup"><span data-stu-id="4f72a-220">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="4f72a-221">Gli avvisi di valutazione client inoltre si sono rivelati molto facili da ignorare durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="4f72a-221">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="4f72a-222">Inoltre, la valutazione client automatica può causare problemi in cui il miglioramento della conversione di query per espressioni specifiche causa modifiche impreviste che causano un'interruzione da una versione all'altra.</span><span class="sxs-lookup"><span data-stu-id="4f72a-222">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="4f72a-223">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-223">**Mitigations**</span></span>

<span data-ttu-id="4f72a-224">Se una query non può essere convertita completamente, riscrivere la query in un formato che possa essere convertito o usare `AsEnumerable()`, `ToList()` o un elemento simile per riportare in modo esplicito i dati nel client dove possono essere quindi ulteriormente elaborati usando LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="4f72a-224">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="4f72a-225">EF Core 3.0 usa come destinazione .NET Standard 2.1 invece che .NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="4f72a-225">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="4f72a-226">Problema n. 15498</span><span class="sxs-lookup"><span data-stu-id="4f72a-226">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="4f72a-227">Questa modifica è stata introdotta in EF Core 3.0 anteprima 7.</span><span class="sxs-lookup"><span data-stu-id="4f72a-227">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="4f72a-228">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-228">**Old behavior**</span></span>

<span data-ttu-id="4f72a-229">Prima della versione 3.0, EF Core usava come destinazione .NET Standard 2.0 e poteva essere eseguito in tutte le piattaforme che supportano tale standard, incluso .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4f72a-229">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="4f72a-230">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-230">**New behavior**</span></span>

<span data-ttu-id="4f72a-231">A partire dalla versione 3.0, EF Core usa come destinazione .NET Standard 2.1 e verrà eseguito in tutte le piattaforme che supportano questo standard.</span><span class="sxs-lookup"><span data-stu-id="4f72a-231">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="4f72a-232">.NET Framework non è incluso.</span><span class="sxs-lookup"><span data-stu-id="4f72a-232">This does not include .NET Framework.</span></span>

<span data-ttu-id="4f72a-233">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-233">**Why**</span></span>

<span data-ttu-id="4f72a-234">Questo comportamento deriva da una decisione strategica per le tecnologie .NET, che mira a concentrare le energie su .NET Core e su altre piattaforme .NET moderne, ad esempio Xamarin.</span><span class="sxs-lookup"><span data-stu-id="4f72a-234">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="4f72a-235">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-235">**Mitigations**</span></span>

<span data-ttu-id="4f72a-236">Valutare il passaggio a una piattaforma .NET moderna.</span><span class="sxs-lookup"><span data-stu-id="4f72a-236">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="4f72a-237">Se non è possibile, continuare a usare EF Core 2.1 o EF Core 2.2, che supportano entrambe .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4f72a-237">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="4f72a-238">Entity Framework Core non è più incluso nel framework condiviso di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f72a-238">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="4f72a-239">Annunci problema n. 325</span><span class="sxs-lookup"><span data-stu-id="4f72a-239">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="4f72a-240">Questa modifica è stata introdotta in ASP.NET Core 3.0 anteprima 1.</span><span class="sxs-lookup"><span data-stu-id="4f72a-240">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="4f72a-241">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-241">**Old behavior**</span></span>

<span data-ttu-id="4f72a-242">Nelle versioni precedenti ad ASP.NET Core 3.0, quando si aggiungeva un riferimento a un pacchetto in `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, veniva inserito EF Core e alcuni dei provider di dati di EF Core come il provider di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4f72a-242">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="4f72a-243">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-243">**New behavior**</span></span>

<span data-ttu-id="4f72a-244">A partire dalla versione 3.0, il framework condiviso di ASP.NET Core non include EF Core o provider di dati di EF Core.</span><span class="sxs-lookup"><span data-stu-id="4f72a-244">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="4f72a-245">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-245">**Why**</span></span>

<span data-ttu-id="4f72a-246">Prima di questa modifica, per ottenere EF Core erano necessarie procedure diverse, a seconda che l'applicazione avesse o meno come destinazione ASP.NET Core e SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4f72a-246">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="4f72a-247">Inoltre, l'aggiornamento di ASP.NET Core forzava l'aggiornamento di EF Core e del provider di SQL Server, non sempre auspicabile.</span><span class="sxs-lookup"><span data-stu-id="4f72a-247">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="4f72a-248">Con questa modifica, la procedura per ottenere EF Core è la stessa in tutti i provider, le implementazioni .NET supportate e i tipi di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="4f72a-248">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="4f72a-249">Gli sviluppatori possono ora controllare anche in modo preciso quando vengono aggiornati EF Core e i provider di dati di EF Core.</span><span class="sxs-lookup"><span data-stu-id="4f72a-249">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="4f72a-250">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-250">**Mitigations**</span></span>

<span data-ttu-id="4f72a-251">Per usare EF Core in un'applicazione ASP.NET Core 3.0 o in un'altra applicazione supportata, aggiungere in modo esplicito un riferimento al pacchetto al provider di database di EF Core che verrà usato dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-251">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="4f72a-252">Lo strumento della riga di comando di EF Core, dotnet ef, non è più incluso in .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="4f72a-252">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="4f72a-253">Problema n. 14016</span><span class="sxs-lookup"><span data-stu-id="4f72a-253">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="4f72a-254">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4 e nella versione corrispondente di .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="4f72a-254">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="4f72a-255">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-255">**Old behavior**</span></span>

<span data-ttu-id="4f72a-256">Prima della versione 3.0, lo strumento `dotnet ef` era incluso in .NET Core SDK ed era immediatamente disponibile dalla riga di comando di qualsiasi progetto senza richiedere passaggi aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="4f72a-256">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="4f72a-257">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-257">**New behavior**</span></span>

<span data-ttu-id="4f72a-258">A partire dalla versione 3.0, .NET SDK non include lo strumento `dotnet ef` pertanto, prima di poterlo usare, è necessario installarlo in modo esplicito come strumento locale o globale.</span><span class="sxs-lookup"><span data-stu-id="4f72a-258">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="4f72a-259">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-259">**Why**</span></span>

<span data-ttu-id="4f72a-260">Questa modifica consente di distribuire e aggiornare `dotnet ef` come uno strumento della riga di comando di .NET in NuGet, coerentemente con il fatto che anche EF Core 3.0 viene distribuito come pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="4f72a-260">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="4f72a-261">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-261">**Mitigations**</span></span>

<span data-ttu-id="4f72a-262">Per essere in grado di gestire le migrazioni o eseguire lo scaffolding di `DbContext`, installare `dotnet-ef` come strumento globale:</span><span class="sxs-lookup"><span data-stu-id="4f72a-262">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="4f72a-263">È inoltre possibile ottenerlo come strumento locale quando si ripristinano le dipendenze di un progetto che lo dichiara come dipendenza di strumenti utilizzando un [file manifesto dello strumento](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="4f72a-263">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="4f72a-264">I metodi FromSql, ExecuteSql ed ExecuteSqlAsync sono stati rinominati</span><span class="sxs-lookup"><span data-stu-id="4f72a-264">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="4f72a-265">Problema n. 10996</span><span class="sxs-lookup"><span data-stu-id="4f72a-265">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="4f72a-266">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-266">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-267">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-267">**Old behavior**</span></span>

<span data-ttu-id="4f72a-268">Prima di EF Core 3.0, erano disponibili overload per questi nomi di metodo per supportare l'uso con una stringa normale o una stringa che deve essere interpolata in SQL e parametri.</span><span class="sxs-lookup"><span data-stu-id="4f72a-268">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="4f72a-269">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-269">**New behavior**</span></span>

<span data-ttu-id="4f72a-270">A partire da EF Core 3.0, usare `FromSqlRaw`, `ExecuteSqlRaw` e `ExecuteSqlRawAsync` per creare una query con parametri in cui i parametri vengono passati separatamente dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="4f72a-270">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="4f72a-271">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4f72a-271">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="4f72a-272">Usare `FromSqlInterpolated`, `ExecuteSqlInterpolated`, e `ExecuteSqlInterpolatedAsync` per creare una query con parametri in cui i parametri vengono passati come parte di una stringa di query interpolata.</span><span class="sxs-lookup"><span data-stu-id="4f72a-272">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="4f72a-273">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4f72a-273">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="4f72a-274">Si noti che entrambe le query precedenti produrranno lo stesso codice SQL con parametri con gli stessi parametri SQL.</span><span class="sxs-lookup"><span data-stu-id="4f72a-274">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="4f72a-275">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-275">**Why**</span></span>

<span data-ttu-id="4f72a-276">Con gli overload di metodi come questi, è molto facile chiamare accidentalmente il metodo con stringa non elaborata anche se l'intento era chiamare il metodo con stringa interpolata e viceversa.</span><span class="sxs-lookup"><span data-stu-id="4f72a-276">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="4f72a-277">Il risultato potrebbero essere query senza parametri, quando invece è prevista la parametrizzazione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-277">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="4f72a-278">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-278">**Mitigations**</span></span>

<span data-ttu-id="4f72a-279">Passare all'uso dei nuovi nomi di metodo.</span><span class="sxs-lookup"><span data-stu-id="4f72a-279">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="4f72a-280">I metodi FromSql possono essere specificati solo in radici di query</span><span class="sxs-lookup"><span data-stu-id="4f72a-280">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="4f72a-281">Problema n. 15704</span><span class="sxs-lookup"><span data-stu-id="4f72a-281">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="4f72a-282">Questa modifica è stata introdotta in EF Core 3.0 anteprima 6.</span><span class="sxs-lookup"><span data-stu-id="4f72a-282">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="4f72a-283">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-283">**Old behavior**</span></span>

<span data-ttu-id="4f72a-284">Prima di EF Core 3.0, il metodo `FromSql` poteva essere specificato in un punto qualsiasi nella query.</span><span class="sxs-lookup"><span data-stu-id="4f72a-284">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="4f72a-285">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-285">**New behavior**</span></span>

<span data-ttu-id="4f72a-286">A partire da EF Core 3.0, i nuovi metodi `FromSqlRaw` e `FromSqlInterpolated` (che sostituiscono`FromSql`) possono essere specificati solo per radici di query, ad esempio direttamente in `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-286">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="4f72a-287">Qualsiasi tentativo di specificarli altrove causerà un errore di compilazione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-287">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="4f72a-288">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-288">**Why**</span></span>

<span data-ttu-id="4f72a-289">La specifica di `FromSql` in qualsiasi posizione diversa da un `DbSet` non ha alcun significato aggiuntivo oppure valore aggiunto e può causare ambiguità in determinati scenari.</span><span class="sxs-lookup"><span data-stu-id="4f72a-289">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="4f72a-290">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-290">**Mitigations**</span></span>

<span data-ttu-id="4f72a-291">Le chiamate di `FromSql` devono essere spostate in modo da comparire direttamente nel `DbSet` a cui si applicano.</span><span class="sxs-lookup"><span data-stu-id="4f72a-291">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="4f72a-292">Le query senza rilevamento delle modifiche non eseguono più la risoluzione delle identità</span><span class="sxs-lookup"><span data-stu-id="4f72a-292">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="4f72a-293">Problema n. 13518</span><span class="sxs-lookup"><span data-stu-id="4f72a-293">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="4f72a-294">Questa modifica è stata introdotta in EF Core 3.0 anteprima 6.</span><span class="sxs-lookup"><span data-stu-id="4f72a-294">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="4f72a-295">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-295">**Old behavior**</span></span>

<span data-ttu-id="4f72a-296">Prima di EF Core 3.0, la stessa istanza di un'entità poteva essere usata per ogni occorrenza di un entità con tipo e ID specifici.</span><span class="sxs-lookup"><span data-stu-id="4f72a-296">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="4f72a-297">Questo comportamento corrisponde a quello delle query con rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="4f72a-297">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="4f72a-298">Ad esempio, questa query:</span><span class="sxs-lookup"><span data-stu-id="4f72a-298">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="4f72a-299">restituisce la stessa istanza di `Category` per ogni `Product` associato alla categoria specificata.</span><span class="sxs-lookup"><span data-stu-id="4f72a-299">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="4f72a-300">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-300">**New behavior**</span></span>

<span data-ttu-id="4f72a-301">A partire da EF Core 3.0, vengono create istanze di entità diverse quando un'entità con un determinato tipo e ID viene rilevata in posizioni diverse nel grafico restituito.</span><span class="sxs-lookup"><span data-stu-id="4f72a-301">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="4f72a-302">La query precedente, ad esempio, ora restituirà una nuova istanza di `Category` per ogni `Product` anche quando due prodotti sono associati alla stessa categoria.</span><span class="sxs-lookup"><span data-stu-id="4f72a-302">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="4f72a-303">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-303">**Why**</span></span>

<span data-ttu-id="4f72a-304">La risoluzione delle identità (ovvero il processo per determinare che un'entità ha lo stesso tipo e ID di un'entità rilevata in precedenza) aggiunge un ulteriore sovraccarico della memoria e delle prestazioni,</span><span class="sxs-lookup"><span data-stu-id="4f72a-304">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="4f72a-305">il che va ad annullare il vantaggio derivante dall'uso delle query senza rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="4f72a-305">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="4f72a-306">Inoltre, anche se in alcuni casi la risoluzione delle identità può essere utile, tuttavia non è necessaria se le entità devono essere serializzate e inviate a un client, come avviene comunemente con le query senza rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="4f72a-306">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="4f72a-307">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-307">**Mitigations**</span></span>

<span data-ttu-id="4f72a-308">Se la risoluzione delle identità è necessaria, usare una query con rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="4f72a-308">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="4f72a-309">~~L'esecuzione di query viene registrata a livello di debug~~ - Modifica annullata</span><span class="sxs-lookup"><span data-stu-id="4f72a-309">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="4f72a-310">Problema n. 14523</span><span class="sxs-lookup"><span data-stu-id="4f72a-310">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="4f72a-311">Questa modifica è stata annullata in EF Core 3.0 anteprima 7.</span><span class="sxs-lookup"><span data-stu-id="4f72a-311">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="4f72a-312">Questa modifica è stata annullata perché la nuova configurazione in EF Core 3.0 consente all'applicazione di specificare il livello di log per qualsiasi evento.</span><span class="sxs-lookup"><span data-stu-id="4f72a-312">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="4f72a-313">Ad esempio, per impostare la registrazione di SQL sul livello `Debug`, configurare il livello in modo esplicito in `OnConfiguring` o `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="4f72a-313">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="4f72a-314">I valori di chiave temporanei non sono più impostati nelle istanze di entità</span><span class="sxs-lookup"><span data-stu-id="4f72a-314">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="4f72a-315">Problema n. 12378</span><span class="sxs-lookup"><span data-stu-id="4f72a-315">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="4f72a-316">Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.</span><span class="sxs-lookup"><span data-stu-id="4f72a-316">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="4f72a-317">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-317">**Old behavior**</span></span>

<span data-ttu-id="4f72a-318">Nelle versioni precedenti a EF Core 3.0 i valori temporanei venivano assegnati a tutte le proprietà di chiave per cui veniva in seguito generato un valore reale dal database.</span><span class="sxs-lookup"><span data-stu-id="4f72a-318">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="4f72a-319">In genere questi valori temporanei erano numeri negativi elevati.</span><span class="sxs-lookup"><span data-stu-id="4f72a-319">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="4f72a-320">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-320">**New behavior**</span></span>

<span data-ttu-id="4f72a-321">A partire dalla versione 3.0, EF Core archivia il valore di chiave temporaneo come parte delle informazioni di rilevamento dell'entità e non modifica la proprietà di chiave.</span><span class="sxs-lookup"><span data-stu-id="4f72a-321">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="4f72a-322">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-322">**Why**</span></span>

<span data-ttu-id="4f72a-323">Questa modifica è stata apportata per impedire che i valori di chiave temporanei diventino erroneamente permanenti quando un'entità rilevata in precedenza da un'istanza `DbContext` viene spostata in un'altra istanza `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-323">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="4f72a-324">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-324">**Mitigations**</span></span>

<span data-ttu-id="4f72a-325">Le applicazioni che assegnano valori di chiave primaria in chiavi esterne per creare associazioni tra le entità possono dipendere dal comportamento precedente se le chiavi primarie vengono generate dall'archivio e appartengono a entità con stato `Added`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-325">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="4f72a-326">Questo può essere evitato:</span><span class="sxs-lookup"><span data-stu-id="4f72a-326">This can be avoided by:</span></span>
* <span data-ttu-id="4f72a-327">Non usando chiavi generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="4f72a-327">Not using store-generated keys.</span></span>
* <span data-ttu-id="4f72a-328">Impostando le proprietà di navigazione in modo da creare relazioni anziché impostando valori di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="4f72a-328">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="4f72a-329">Ottenendo i valori di chiave temporanei effettivi dalle informazioni di rilevamento dell'entità.</span><span class="sxs-lookup"><span data-stu-id="4f72a-329">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="4f72a-330">Ad esempio, `context.Entry(blog).Property(e => e.Id).CurrentValue` restituisce il valore temporaneo anche quando `blog.Id` non è stato impostato.</span><span class="sxs-lookup"><span data-stu-id="4f72a-330">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="4f72a-331">DetectChanges rispetta i valori di chiave generati dall'archivio</span><span class="sxs-lookup"><span data-stu-id="4f72a-331">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="4f72a-332">Problema n. 14616</span><span class="sxs-lookup"><span data-stu-id="4f72a-332">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="4f72a-333">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="4f72a-333">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="4f72a-334">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-334">**Old behavior**</span></span>

<span data-ttu-id="4f72a-335">Nelle versioni precedenti a EF Core 3.0 un'entità non rilevata individuata da `DetectChanges` veniva rilevata nello stato `Added` e inserita come nuova riga quando veniva eseguita una chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-335">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="4f72a-336">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-336">**New behavior**</span></span>

<span data-ttu-id="4f72a-337">A partire da EF Core 3.0, se un'entità usa valori di chiave generati e viene impostato un valore di chiave, l'entità viene rilevata nello stato `Modified`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-337">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="4f72a-338">Ciò significa che si presuppone l'esistenza di una riga per l'entità che viene aggiornata quando viene eseguita una chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-338">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="4f72a-339">Se il valore di chiave non viene impostato o se il tipo di entità non usa chiavi generate, la nuova entità viene rilevata come `Added` come nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="4f72a-339">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="4f72a-340">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-340">**Why**</span></span>

<span data-ttu-id="4f72a-341">Questa modifica è stata apportata per rendere più semplice e coerente l'uso di grafici di entità disconnesse con chiavi generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="4f72a-341">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="4f72a-342">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-342">**Mitigations**</span></span>

<span data-ttu-id="4f72a-343">Questa modifica può interrompere un'applicazione se un tipo di entità è configurato per l'uso di chiavi generate ma i valori di chiave sono impostati in modo esplicito per le nuove istanze.</span><span class="sxs-lookup"><span data-stu-id="4f72a-343">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="4f72a-344">La correzione consiste nel configurare in modo esplicito le proprietà di chiave per non usare valori generati.</span><span class="sxs-lookup"><span data-stu-id="4f72a-344">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="4f72a-345">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="4f72a-345">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="4f72a-346">Oppure con annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="4f72a-346">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="4f72a-347">Le eliminazioni a catena vengono ora eseguite immediatamente per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="4f72a-347">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="4f72a-348">Problema n. 10114</span><span class="sxs-lookup"><span data-stu-id="4f72a-348">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="4f72a-349">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="4f72a-349">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="4f72a-350">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-350">**Old behavior**</span></span>

<span data-ttu-id="4f72a-351">Nelle versioni precedenti alla versione 3.0, EF Core applicava azioni a catena (eliminazione delle entità dipendenti quando veniva eliminata un'entità di sicurezza obbligatoria o veniva recisa la relazione con un'entità di sicurezza obbligatoria) solo dopo la chiamata a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="4f72a-351">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="4f72a-352">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-352">**New behavior**</span></span>

<span data-ttu-id="4f72a-353">A partire dalla versione 3.0, EF Core applica le azioni a catena non appena viene rilevata la condizione di attivazione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-353">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="4f72a-354">Ad esempio, la chiamata a `context.Remove()` per eliminare un'entità di sicurezza causa anche l'impostazione immediata di tutti i dipendenti obbligatori correlati rilevati su `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-354">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="4f72a-355">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-355">**Why**</span></span>

<span data-ttu-id="4f72a-356">Questa modifica è stata apportata per migliorare l'esperienza di associazione di dati e degli scenari di controllo in cui è importante individuare le entità che verranno eliminate _prima_ della chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-356">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="4f72a-357">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-357">**Mitigations**</span></span>

<span data-ttu-id="4f72a-358">Il comportamento precedente può essere ripristinato tramite le impostazioni in `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-358">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="4f72a-359">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4f72a-359">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="4f72a-360">Semantica più chiara per DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="4f72a-360">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="4f72a-361">Problema n. 12661</span><span class="sxs-lookup"><span data-stu-id="4f72a-361">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="4f72a-362">Questa modifica è stata introdotta in EF Core 3.0 anteprima 5.</span><span class="sxs-lookup"><span data-stu-id="4f72a-362">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="4f72a-363">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-363">**Old behavior**</span></span>

<span data-ttu-id="4f72a-364">Prima della versione 3.0, `DeleteBehavior.Restrict` creava chiavi esterne nel database con la semantica `Restrict`, ma modificava anche la correzione interna in modo non ovvio.</span><span class="sxs-lookup"><span data-stu-id="4f72a-364">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="4f72a-365">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-365">**New behavior**</span></span>

<span data-ttu-id="4f72a-366">A partire dalla versione 3.0, `DeleteBehavior.Restrict` assicura che le chiavi esterne vengano create con la semantica `Restrict`, ovvero non a cascata e con generazione di un'eccezione in caso di violazione di vincolo, senza influire sulla correzione interna di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="4f72a-366">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="4f72a-367">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-367">**Why**</span></span>

<span data-ttu-id="4f72a-368">Questa modifica è stata apportata per migliorare l'esperienza di uso di `DeleteBehavior` in modo intuitivo, senza effetti collaterali imprevisti.</span><span class="sxs-lookup"><span data-stu-id="4f72a-368">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="4f72a-369">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-369">**Mitigations**</span></span>

<span data-ttu-id="4f72a-370">Il comportamento precedente può essere ripristinato tramite `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-370">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="4f72a-371">I tipi di query vengono consolidati con tipi di entità</span><span class="sxs-lookup"><span data-stu-id="4f72a-371">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="4f72a-372">Problema n. 14194</span><span class="sxs-lookup"><span data-stu-id="4f72a-372">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="4f72a-373">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="4f72a-373">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="4f72a-374">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-374">**Old behavior**</span></span>

<span data-ttu-id="4f72a-375">Nelle versioni precedenti a EF Core 3.0 i [tipi di query](xref:core/modeling/query-types) erano uno strumento per eseguire query su dati che non definiscono una chiave primaria in modo strutturato.</span><span class="sxs-lookup"><span data-stu-id="4f72a-375">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="4f72a-376">Veniva infatti usato un tipo di query per eseguire il mapping di tipi di entità senza chiavi (più probabilmente da una vista, ma anche da una tabella), mentre veniva usato un tipo di entità normale quando era disponibile una chiave (più probabilmente da una tabella, ma anche da una vista).</span><span class="sxs-lookup"><span data-stu-id="4f72a-376">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="4f72a-377">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-377">**New behavior**</span></span>

<span data-ttu-id="4f72a-378">Un tipo di query diventa ora semplicemente un tipo di entità senza chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="4f72a-378">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="4f72a-379">I tipi di entità senza chiave hanno la stessa funzionalità dei tipi di query nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="4f72a-379">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="4f72a-380">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-380">**Why**</span></span>

<span data-ttu-id="4f72a-381">Questa modifica è stata apportata per ridurre la confusione riguardo lo scopo dei tipi di query.</span><span class="sxs-lookup"><span data-stu-id="4f72a-381">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="4f72a-382">In particolare, si tratta di tipi di entità senza chiave che sono intrinsecamente di sola lettura per questo motivo ma non dovrebbero essere usati solo perché un tipo di entità deve essere di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="4f72a-382">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="4f72a-383">Analogamente, spesso vengono mappati alle viste solo perché le viste spesso non definiscono le chiavi.</span><span class="sxs-lookup"><span data-stu-id="4f72a-383">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="4f72a-384">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-384">**Mitigations**</span></span>

<span data-ttu-id="4f72a-385">Le parti dell'API seguenti sono ora obsolete:</span><span class="sxs-lookup"><span data-stu-id="4f72a-385">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="4f72a-386">**`ModelBuilder.Query<>()`** - È necessario chiamare `ModelBuilder.Entity<>().HasNoKey()` per contrassegnare un tipo di entità come tipo senza chiavi.</span><span class="sxs-lookup"><span data-stu-id="4f72a-386">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="4f72a-387">Non ne viene eseguita la configurazione per convenzione per evitare una configurazione errata quando è prevista una chiave primaria che tuttavia non corrisponde alla convenzione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-387">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="4f72a-388">**`DbQuery<>`** - Usare `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-388">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="4f72a-389">**`DbContext.Query<>()`** - Usare `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-389">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="4f72a-390">L'API di configurazione per le relazioni di tipo di proprietà è stata modificata</span><span class="sxs-lookup"><span data-stu-id="4f72a-390">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="4f72a-391">[Problema n. 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema n. 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema n. 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="4f72a-391">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="4f72a-392">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="4f72a-392">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="4f72a-393">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-393">**Old behavior**</span></span>

<span data-ttu-id="4f72a-394">Nelle versioni precedenti a EF Core 3.0 la configurazione della relazione di proprietà veniva eseguita direttamente dopo la chiamata a `OwnsOne` o `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-394">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="4f72a-395">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-395">**New behavior**</span></span>

<span data-ttu-id="4f72a-396">A partire da EF Core 3.0, è disponibile l'API Fluent per configurare una proprietà di navigazione per il proprietario usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-396">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="4f72a-397">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4f72a-397">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="4f72a-398">La configurazione correlata alla relazione tra proprietario e elemento di proprietà deve essere ora concatenata dopo `WithOwner()` in modo analogo a come vengono configurate altre relazioni.</span><span class="sxs-lookup"><span data-stu-id="4f72a-398">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="4f72a-399">La configurazione per il tipo di proprietà viene comunque concatenata dopo `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-399">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="4f72a-400">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4f72a-400">For example:</span></span>

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

<span data-ttu-id="4f72a-401">Inoltre, la chiamata a `Entity()`, `HasOne()` o `Set()` con una destinazione di tipo di proprietà genera ora un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-401">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="4f72a-402">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-402">**Why**</span></span>

<span data-ttu-id="4f72a-403">Questa modifica è stata apportata per creare una separazione più netta tra la configurazione del tipo di proprietà e la _relazione_ con il tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="4f72a-403">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="4f72a-404">Ciò consente di eliminare ambiguità e confusione su metodi come `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-404">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="4f72a-405">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-405">**Mitigations**</span></span>

<span data-ttu-id="4f72a-406">Modificare la configurazione delle relazioni dei tipi di proprietà per usare la superficie della nuova API come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="4f72a-406">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="4f72a-407">Le entità dipendenti che condividono la tabella con l'entità di sicurezza sono ora facoltative</span><span class="sxs-lookup"><span data-stu-id="4f72a-407">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="4f72a-408">Problema n. 9005</span><span class="sxs-lookup"><span data-stu-id="4f72a-408">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="4f72a-409">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-409">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-410">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-410">**Old behavior**</span></span>

<span data-ttu-id="4f72a-411">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="4f72a-411">Consider the following model:</span></span>
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
<span data-ttu-id="4f72a-412">Prima di EF Core 3.0, se `OrderDetails` è di proprietà di `Order` o viene mappato in modo esplicito alla stessa tabella, era sempre necessaria un'istanza di `OrderDetails` per l'aggiunta di un nuovo `Order`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-412">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="4f72a-413">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-413">**New behavior**</span></span>

<span data-ttu-id="4f72a-414">A partire dalla versione 3.0, EF Core consente di aggiungere un `Order` senza un `OrderDetails` ed esegue il mapping di tutte le proprietà di `OrderDetails`, tranne che la chiave primaria, a colonne che ammettono valori Null.</span><span class="sxs-lookup"><span data-stu-id="4f72a-414">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="4f72a-415">In fase di query, EF Core imposta `OrderDetails` su `null` se una delle relative proprietà obbligatorie non ha un valore o se non sono presenti proprietà obbligatorie oltre alla chiave primaria e tutte le proprietà sono `null`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-415">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="4f72a-416">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-416">**Mitigations**</span></span>

<span data-ttu-id="4f72a-417">Se il modello include una tabella condivisa dipendente con tutte le colonne facoltative, ma è previsto che la navigazione che punta a essa non sia `null`, l'applicazione deve essere modificata per gestire casi in cui la navigazione è `null`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-417">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="4f72a-418">Se questo non è possibile, è consigliabile aggiungere una proprietà obbligatoria al tipo di entità o assegnare un valore non `null` ad almeno una proprietà.</span><span class="sxs-lookup"><span data-stu-id="4f72a-418">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="4f72a-419">Tutte le entità che condividono una tabella con una colonna di token di concorrenza devono eseguirne il mapping a una proprietà</span><span class="sxs-lookup"><span data-stu-id="4f72a-419">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="4f72a-420">Problema n. 14154</span><span class="sxs-lookup"><span data-stu-id="4f72a-420">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="4f72a-421">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-421">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-422">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-422">**Old behavior**</span></span>

<span data-ttu-id="4f72a-423">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="4f72a-423">Consider the following model:</span></span>
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
<span data-ttu-id="4f72a-424">Prima di EF Core 3.0, se `OrderDetails` è di proprietà di `Order` o è mappato in modo esplicito alla stessa tabella, il solo aggiornamento di `OrderDetails` non aggiornerà il valore `Version` nel client e l'aggiornamento successivo avrà esito negativo.</span><span class="sxs-lookup"><span data-stu-id="4f72a-424">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="4f72a-425">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-425">**New behavior**</span></span>

<span data-ttu-id="4f72a-426">A partire dalla versione 3.0, EF Core propaga il nuovo valore `Version` a `Order` se è proprietario di `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-426">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="4f72a-427">In caso contrario, viene generata un'eccezione durante la convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="4f72a-427">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="4f72a-428">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-428">**Why**</span></span>

<span data-ttu-id="4f72a-429">Questa modifica è stata apportata per evitare un valore del token di concorrenza non aggiornato quando viene aggiornata solo una delle entità mappate alla stessa tabella.</span><span class="sxs-lookup"><span data-stu-id="4f72a-429">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="4f72a-430">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-430">**Mitigations**</span></span>

<span data-ttu-id="4f72a-431">Tutte le entità che condividono la tabella devono includere una proprietà mappata alla colonna del token di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="4f72a-431">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="4f72a-432">È possibile crearne una in stato shadow:</span><span class="sxs-lookup"><span data-stu-id="4f72a-432">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="4f72a-433">Per le proprietà ereditate da tipi senza mapping viene ora eseguito il mapping a una singola colonna per tutti i tipi derivati</span><span class="sxs-lookup"><span data-stu-id="4f72a-433">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="4f72a-434">Problema n. 13998</span><span class="sxs-lookup"><span data-stu-id="4f72a-434">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="4f72a-435">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-435">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-436">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-436">**Old behavior**</span></span>

<span data-ttu-id="4f72a-437">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="4f72a-437">Consider the following model:</span></span>
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

<span data-ttu-id="4f72a-438">Prima di EF Core 3.0, per la proprietà `ShippingAddress` sarebbe stato eseguito il mapping a colonne separate per `BulkOrder` e `Order` per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="4f72a-438">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="4f72a-439">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-439">**New behavior**</span></span>

<span data-ttu-id="4f72a-440">A partire dalla versione 3.0, EF Core crea solo una colonna per `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-440">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="4f72a-441">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-441">**Why**</span></span>

<span data-ttu-id="4f72a-442">Il comportamento precedente non era previsto.</span><span class="sxs-lookup"><span data-stu-id="4f72a-442">The old behavoir was unexpected.</span></span>

<span data-ttu-id="4f72a-443">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-443">**Mitigations**</span></span>

<span data-ttu-id="4f72a-444">È ancora possibile eseguire il mapping esplicito della proprietà a una colonna separata per i tipi derivati:</span><span class="sxs-lookup"><span data-stu-id="4f72a-444">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="4f72a-445">La convenzione di proprietà di chiave esterna non ha più lo stesso nome della proprietà dell'entità di sicurezza</span><span class="sxs-lookup"><span data-stu-id="4f72a-445">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="4f72a-446">Problema n. 13274</span><span class="sxs-lookup"><span data-stu-id="4f72a-446">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="4f72a-447">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="4f72a-447">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="4f72a-448">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-448">**Old behavior**</span></span>

<span data-ttu-id="4f72a-449">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="4f72a-449">Consider the following model:</span></span>
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
<span data-ttu-id="4f72a-450">Nelle versioni precedenti a EF Core 3.0 veniva usata la proprietà `CustomerId` per la chiave esterna per convenzione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-450">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="4f72a-451">Tuttavia, se `Order` è un tipo di proprietà, `CustomerId` sarebbe la chiave primaria e ciò non è in genere auspicabile.</span><span class="sxs-lookup"><span data-stu-id="4f72a-451">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="4f72a-452">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-452">**New behavior**</span></span>

<span data-ttu-id="4f72a-453">A partire da 3.0, EF Core non tenta di usare le proprietà per le chiavi esterne per convenzione se hanno lo stesso nome della proprietà dell'entità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="4f72a-453">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="4f72a-454">Viene ancora eseguita la corrispondenza tra i criteri del nome del tipo dell'entità di sicurezza concatenato al nome della proprietà dell'entità di sicurezza e il nome di navigazione concatenato al nome della proprietà dell'entità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="4f72a-454">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="4f72a-455">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4f72a-455">For example:</span></span>

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

<span data-ttu-id="4f72a-456">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-456">**Why**</span></span>

<span data-ttu-id="4f72a-457">Questa modifica è stata apportata per evitare una definizione errata della proprietà di chiave primaria nel tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="4f72a-457">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="4f72a-458">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-458">**Mitigations**</span></span>

<span data-ttu-id="4f72a-459">Se la proprietà è stata progettata per essere la chiave esterna e di conseguenza parte della chiave primaria, configurarla in modo esplicito come chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="4f72a-459">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="4f72a-460">La connessione al database viene ora chiusa se non viene più usata prima del completamento di TransactionScope</span><span class="sxs-lookup"><span data-stu-id="4f72a-460">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="4f72a-461">Problema n. 14218</span><span class="sxs-lookup"><span data-stu-id="4f72a-461">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="4f72a-462">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-462">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-463">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-463">**Old behavior**</span></span>

<span data-ttu-id="4f72a-464">Prima di EF Core 3.0, se il contesto apre la connessione all'interno di un `TransactionScope`, la connessione rimane aperta mentre è attivo il `TransactionScope` corrente.</span><span class="sxs-lookup"><span data-stu-id="4f72a-464">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="4f72a-465">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-465">**New behavior**</span></span>

<span data-ttu-id="4f72a-466">A partire dalla versione 3.0, EF Core chiude la connessione non appena non viene più usata.</span><span class="sxs-lookup"><span data-stu-id="4f72a-466">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="4f72a-467">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-467">**Why**</span></span>

<span data-ttu-id="4f72a-468">Questa modifica consente di usare più contesti nello stesso `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-468">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="4f72a-469">Il nuovo comportamento corrisponde anche a EF6.</span><span class="sxs-lookup"><span data-stu-id="4f72a-469">The new behavior also matches EF6.</span></span>

<span data-ttu-id="4f72a-470">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-470">**Mitigations**</span></span>

<span data-ttu-id="4f72a-471">Se la connessione deve rimanere aperta, la chiamata esplicita di `OpenConnection()` garantirà che EF Core non la chiuda prematuramente:</span><span class="sxs-lookup"><span data-stu-id="4f72a-471">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="4f72a-472">Ogni proprietà usa la generazione di chiavi di tipo intero in memoria indipendenti</span><span class="sxs-lookup"><span data-stu-id="4f72a-472">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="4f72a-473">Problema n. 6872</span><span class="sxs-lookup"><span data-stu-id="4f72a-473">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="4f72a-474">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-474">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-475">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-475">**Old behavior**</span></span>

<span data-ttu-id="4f72a-476">Nelle versioni precedenti a EF Core 3.0 veniva usato un unico generatore di valori condiviso per tutte le proprietà di chiavi di tipo intero in memoria.</span><span class="sxs-lookup"><span data-stu-id="4f72a-476">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="4f72a-477">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-477">**New behavior**</span></span>

<span data-ttu-id="4f72a-478">A partire da EF Core 3.0, ogni proprietà di chiave di tipo intero riceve un generatore di valori quando viene usato il database in memoria.</span><span class="sxs-lookup"><span data-stu-id="4f72a-478">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="4f72a-479">Inoltre, se il database viene eliminato, la generazione di chiavi viene reimpostata per tutte le tabelle.</span><span class="sxs-lookup"><span data-stu-id="4f72a-479">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="4f72a-480">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-480">**Why**</span></span>

<span data-ttu-id="4f72a-481">Questa modifica è stata apportata per allineare maggiormente la generazione di chiavi in memoria alla generazione di chiavi del database reale e per migliorare la possibilità di isolare i test l'uno dall'altro quando viene usato il database in memoria.</span><span class="sxs-lookup"><span data-stu-id="4f72a-481">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="4f72a-482">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-482">**Mitigations**</span></span>

<span data-ttu-id="4f72a-483">Ciò può interrompere un'applicazione che si basa sull'impostazione di valori di chiave in memoria specifici.</span><span class="sxs-lookup"><span data-stu-id="4f72a-483">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="4f72a-484">È consigliabile non basare l'applicazione su valori di chiave specifici o eseguire l'aggiornamento per passare al nuovo comportamento.</span><span class="sxs-lookup"><span data-stu-id="4f72a-484">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="4f72a-485">I campi sottostanti vengono usati per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="4f72a-485">Backing fields are used by default</span></span>

[<span data-ttu-id="4f72a-486">Problema n. 12430</span><span class="sxs-lookup"><span data-stu-id="4f72a-486">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="4f72a-487">Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.</span><span class="sxs-lookup"><span data-stu-id="4f72a-487">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="4f72a-488">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-488">**Old behavior**</span></span>

<span data-ttu-id="4f72a-489">Nelle versioni precedenti alla versione 3.0, anche se il campo sottostante di una proprietà era noto, per impostazione predefinita EF Core eseguiva la lettura e la scrittura del valore della proprietà usando i metodi getter e setter della proprietà.</span><span class="sxs-lookup"><span data-stu-id="4f72a-489">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="4f72a-490">L'eccezione era costituita dall'esecuzione di query in cui il campo sottostante, se noto, veniva impostato direttamente.</span><span class="sxs-lookup"><span data-stu-id="4f72a-490">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="4f72a-491">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-491">**New behavior**</span></span>

<span data-ttu-id="4f72a-492">A partire da EF Core 3.0, se il campo sottostante di una proprietà è noto, la lettura e la scrittura della proprietà vengono sempre eseguite usando il campo sottostante.</span><span class="sxs-lookup"><span data-stu-id="4f72a-492">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="4f72a-493">Ciò potrebbe causare un'interruzione dell'applicazione se l'applicazione si basa su un comportamento aggiuntivo codificato nei metodi getter o setter.</span><span class="sxs-lookup"><span data-stu-id="4f72a-493">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="4f72a-494">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-494">**Why**</span></span>

<span data-ttu-id="4f72a-495">Questa modifica è stata apportata per impedire a EF Core di attivare per errore la logica di business per impostazione predefinita quando si eseguono operazioni di database che interessano le entità.</span><span class="sxs-lookup"><span data-stu-id="4f72a-495">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="4f72a-496">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-496">**Mitigations**</span></span>

<span data-ttu-id="4f72a-497">È possibile ripristinare il comportamento delle versioni precedenti alla versione 3.0 tramite la configurazione della modalità di accesso delle proprietà in `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-497">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="4f72a-498">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4f72a-498">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="4f72a-499">Viene generata un'eccezione se vengono trovati più campi sottostanti compatibili</span><span class="sxs-lookup"><span data-stu-id="4f72a-499">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="4f72a-500">Problema n. 12523</span><span class="sxs-lookup"><span data-stu-id="4f72a-500">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="4f72a-501">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-501">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-502">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-502">**Old behavior**</span></span>

<span data-ttu-id="4f72a-503">Nelle versioni precedenti a EF Core 3.0, se più campi soddisfacevano le regole di ricerca del campo sottostante di una proprietà, veniva selezionato un solo campo in base a un ordine di precedenza.</span><span class="sxs-lookup"><span data-stu-id="4f72a-503">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="4f72a-504">Ciò poteva causare l'uso di un campo non corretto nei casi ambigui.</span><span class="sxs-lookup"><span data-stu-id="4f72a-504">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="4f72a-505">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-505">**New behavior**</span></span>

<span data-ttu-id="4f72a-506">A partire da EF Core 3.0, se più campi corrispondono alla stessa proprietà, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-506">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="4f72a-507">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-507">**Why**</span></span>

<span data-ttu-id="4f72a-508">Questa modifica è stata apportata per evitare di usare automaticamente un campo rispetto a un altro quando un solo campo può essere quello corretto.</span><span class="sxs-lookup"><span data-stu-id="4f72a-508">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="4f72a-509">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-509">**Mitigations**</span></span>

<span data-ttu-id="4f72a-510">Per le proprietà con campi sottostanti ambigui, il campo da usare deve essere specificato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="4f72a-510">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="4f72a-511">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="4f72a-511">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="4f72a-512">I nomi delle proprietà solo campo devono corrispondere al nome di campo</span><span class="sxs-lookup"><span data-stu-id="4f72a-512">Field-only property names should match the field name</span></span>

<span data-ttu-id="4f72a-513">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-513">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-514">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-514">**Old behavior**</span></span>

<span data-ttu-id="4f72a-515">Prima di EF Core 3.0, era possibile specificare una proprietà con un valore stringa e se non veniva trovata alcuna proprietà con tale nome nel tipo CLR, EF Core tentava di trovare una corrispondenza con un campo tramite regole di convenzione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-515">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="4f72a-516">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-516">**New behavior**</span></span>

<span data-ttu-id="4f72a-517">A partire da EF Core 3.0, una proprietà solo campo deve corrispondere esattamente al nome del campo.</span><span class="sxs-lookup"><span data-stu-id="4f72a-517">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="4f72a-518">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-518">**Why**</span></span>

<span data-ttu-id="4f72a-519">Questa modifica è stata introdotta per evitare di usare lo stesso campo per due proprietà con nome simile. Le regole di corrispondenza per le proprietà solo campo sono state anche uniformate a quelle per le proprietà mappate a proprietà CLR.</span><span class="sxs-lookup"><span data-stu-id="4f72a-519">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="4f72a-520">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-520">**Mitigations**</span></span>

<span data-ttu-id="4f72a-521">Le proprietà solo campo devono avere lo stesso nome del campo a cui vengono mappate.</span><span class="sxs-lookup"><span data-stu-id="4f72a-521">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="4f72a-522">In una versione futura di EF Core dopo il 3,0, si prevede di abilitare di nuovo in modo esplicito il nome di un campo diverso dal nome della proprietà (vedere il problema [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="4f72a-522">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="4f72a-523">AddDbContext/AddDbContextPool non chiamano più AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="4f72a-523">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="4f72a-524">Problema n. 14756</span><span class="sxs-lookup"><span data-stu-id="4f72a-524">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="4f72a-525">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-525">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-526">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-526">**Old behavior**</span></span>

<span data-ttu-id="4f72a-527">Prima di EF Core 3.0, la chiamata di `AddDbContext` oppure `AddDbContextPool` comporta anche la registrazione dei servizi di registrazione e memorizzazione nella cache con inserimento delle dipendenze tramite chiamate a [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="4f72a-527">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="4f72a-528">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-528">**New behavior**</span></span>

<span data-ttu-id="4f72a-529">A partire da EF Core 3.0, `AddDbContext` e `AddDbContextPool` non registreranno più questi servizi con inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="4f72a-529">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="4f72a-530">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-530">**Why**</span></span>

<span data-ttu-id="4f72a-531">EF Core 3.0 non richiede che questi servizi siano inclusi nel contenitore di inserimento delle dipendenze dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-531">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="4f72a-532">Tuttavia, se `ILoggerFactory` è registrato nel contenitore di inserimento delle dipendenze dell'applicazione, verrà ancora usato da EF Core.</span><span class="sxs-lookup"><span data-stu-id="4f72a-532">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="4f72a-533">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-533">**Mitigations**</span></span>

<span data-ttu-id="4f72a-534">Se l'applicazione necessita di questi servizi, registrarli in modo esplicito con il contenitore di inserimento delle dipendenze usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="4f72a-534">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="4f72a-535">DbContext.Entry esegue ora un DetectChanges locale</span><span class="sxs-lookup"><span data-stu-id="4f72a-535">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="4f72a-536">Problema n. 13552</span><span class="sxs-lookup"><span data-stu-id="4f72a-536">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="4f72a-537">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="4f72a-537">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="4f72a-538">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-538">**Old behavior**</span></span>

<span data-ttu-id="4f72a-539">Nelle versioni precedenti a EF Core 3.0 la chiamata a `DbContext.Entry` causava il rilevamento delle modifiche per tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="4f72a-539">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="4f72a-540">Ciò garantiva l'aggiornamento dello stato esposto in `EntityEntry`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-540">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="4f72a-541">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-541">**New behavior**</span></span>

<span data-ttu-id="4f72a-542">A partire da EF Core 3.0, la chiamata a `DbContext.Entry` causa ora solo il tentativo di rilevare le modifiche nell'entità specificata e in tutte le relative entità di sicurezza rilevate.</span><span class="sxs-lookup"><span data-stu-id="4f72a-542">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="4f72a-543">Ciò significa che le modifiche apportate altrove potrebbero non essere state rilevate tramite la chiamata al metodo e ciò potrebbe avere implicazioni sullo stato dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-543">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="4f72a-544">Si noti che se `ChangeTracker.AutoDetectChangesEnabled` è impostato su `false`, verrà disabilitato anche questo tipo di rilevamento delle modifiche locali.</span><span class="sxs-lookup"><span data-stu-id="4f72a-544">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="4f72a-545">Gli altri metodi che causano il rilevamento delle modifiche, ad esempio `ChangeTracker.Entries` e `SaveChanges`, causano ancora un `DetectChanges` completo di tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="4f72a-545">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="4f72a-546">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-546">**Why**</span></span>

<span data-ttu-id="4f72a-547">Questa modifica è stata apportata per migliorare le prestazioni predefinite dell'uso di `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-547">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="4f72a-548">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-548">**Mitigations**</span></span>

<span data-ttu-id="4f72a-549">Chiamare `ChgangeTracker.DetectChanges()` in modo esplicito prima di chiamare `Entry` per garantire il comportamento precedente alla versione 3.0.</span><span class="sxs-lookup"><span data-stu-id="4f72a-549">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="4f72a-550">Le chiavi matrice di byte e di stringhe non vengono generate dal client per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="4f72a-550">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="4f72a-551">Problema n. 14617</span><span class="sxs-lookup"><span data-stu-id="4f72a-551">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="4f72a-552">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-552">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-553">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-553">**Old behavior**</span></span>

<span data-ttu-id="4f72a-554">Nelle versioni precedenti a EF Core 3.0 le proprietà di chiave `string` e `byte[]` potevano essere usate senza impostare in modo esplicito un valore non Null.</span><span class="sxs-lookup"><span data-stu-id="4f72a-554">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="4f72a-555">In questi casi, il valore di chiave veniva generato nel client come GUID, serializzato in byte per `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-555">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="4f72a-556">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-556">**New behavior**</span></span>

<span data-ttu-id="4f72a-557">A partire da EF Core 3.0 viene generata un'eccezione che indica che non è stato impostato alcun valore di chiave.</span><span class="sxs-lookup"><span data-stu-id="4f72a-557">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="4f72a-558">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-558">**Why**</span></span>

<span data-ttu-id="4f72a-559">Questa modifica è stata apportata poiché i valori `string`/`byte[]` generati dal client non risultano in genere utili e il comportamento predefinito rendeva complesso ragionare sui valori di chiave generati in un modo comune.</span><span class="sxs-lookup"><span data-stu-id="4f72a-559">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="4f72a-560">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-560">**Mitigations**</span></span>

<span data-ttu-id="4f72a-561">È possibile ripristinare il comportamento precedente alla versione 3.0 specificando in modo esplicito che le proprietà di chiave devono usare i valori generati se non viene impostato alcun altro valore non Null.</span><span class="sxs-lookup"><span data-stu-id="4f72a-561">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="4f72a-562">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="4f72a-562">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="4f72a-563">Oppure con annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="4f72a-563">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="4f72a-564">ILoggerFactory è ora un servizio con ambito</span><span class="sxs-lookup"><span data-stu-id="4f72a-564">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="4f72a-565">Problema n. 14698</span><span class="sxs-lookup"><span data-stu-id="4f72a-565">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="4f72a-566">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="4f72a-566">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="4f72a-567">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-567">**Old behavior**</span></span>

<span data-ttu-id="4f72a-568">Nelle versioni precedenti a EF Core 3.0 `ILoggerFactory` veniva registrato come servizio singleton.</span><span class="sxs-lookup"><span data-stu-id="4f72a-568">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="4f72a-569">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-569">**New behavior**</span></span>

<span data-ttu-id="4f72a-570">A partire da EF Core 3.0, `ILoggerFactory` viene registrato come servizio con ambito.</span><span class="sxs-lookup"><span data-stu-id="4f72a-570">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="4f72a-571">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-571">**Why**</span></span>

<span data-ttu-id="4f72a-572">Questa modifica è stata apportata per consentire l'associazione di un logger a un'istanza `DbContext` che abilita altre funzionalità e rimuove alcuni casi di comportamento anomalo, ad esempio un'esplosione dei provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="4f72a-572">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="4f72a-573">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-573">**Mitigations**</span></span>

<span data-ttu-id="4f72a-574">Questa modifica non dovrebbe influire sul codice dell'applicazione a meno che non vengano registrati e usati servizi personalizzati nel provider di servizi interno di EF Core.</span><span class="sxs-lookup"><span data-stu-id="4f72a-574">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="4f72a-575">Questa condizione, tuttavia, non è comune.</span><span class="sxs-lookup"><span data-stu-id="4f72a-575">This isn't common.</span></span>
<span data-ttu-id="4f72a-576">In questi casi la maggior parte delle operazioni continuano a essere eseguite correttamente, ma qualsiasi servizio singleton dipendente da `ILoggerFactory` dovrà essere modificato per ottenere `ILoggerFactory` in modo diverso.</span><span class="sxs-lookup"><span data-stu-id="4f72a-576">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="4f72a-577">Se si verificano situazioni simili, inviare una segnalazione nello [strumento di gestione dei problemi in GitHub relativo a EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) per comunicare in che modo viene usato `ILoggerFactory` per consentirci di comprendere meglio come evitare ulteriori interruzioni in futuro.</span><span class="sxs-lookup"><span data-stu-id="4f72a-577">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="4f72a-578">I proxy di caricamento lazy non presuppongono più che le proprietà di navigazione vengano caricate completamente</span><span class="sxs-lookup"><span data-stu-id="4f72a-578">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="4f72a-579">Problema n. 12780</span><span class="sxs-lookup"><span data-stu-id="4f72a-579">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="4f72a-580">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-580">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-581">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-581">**Old behavior**</span></span>

<span data-ttu-id="4f72a-582">Nelle versioni precedenti a EF Core 3.0, quando `DbContext` veniva eliminato non esisteva alcun metodo per scoprire se una determinata proprietà di navigazione in un'entità ottenuta da un contesto specifico veniva caricata completamente o meno.</span><span class="sxs-lookup"><span data-stu-id="4f72a-582">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="4f72a-583">I proxy presupponevano invece che venisse caricata una navigazione di riferimento se era presente un valore non Null e che venisse caricata una navigazione di raccolta se era presente un valore.</span><span class="sxs-lookup"><span data-stu-id="4f72a-583">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="4f72a-584">In questi casi il tentativo di eseguire un caricamento lazy non avrebbe avuto alcun esito.</span><span class="sxs-lookup"><span data-stu-id="4f72a-584">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="4f72a-585">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-585">**New behavior**</span></span>

<span data-ttu-id="4f72a-586">A partire da Entity Framework Core 3.0, i proxy tengono traccia del caricamento o mancato caricamento di una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-586">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="4f72a-587">Ciò significa che il tentativo di accedere a una proprietà di navigazione caricata dopo l'eliminazione del contesto non avrà mai alcun esito, anche quando la navigazione caricata è vuota o ha valore Null.</span><span class="sxs-lookup"><span data-stu-id="4f72a-587">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="4f72a-588">Al contrario, il tentativo di accedere a una proprietà di navigazione non caricata genera un'eccezione se il contesto viene eliminato anche se la proprietà di navigazione è una raccolta non vuota.</span><span class="sxs-lookup"><span data-stu-id="4f72a-588">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="4f72a-589">Se si verifica questa situazione significa che il codice dell'applicazione sta tentando di usare il caricamento lazy in un momento non valido e l'applicazione deve essere modificata in modo da non eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-589">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="4f72a-590">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-590">**Why**</span></span>

<span data-ttu-id="4f72a-591">Questa modifica è stata apportata per rendere coerente e corretto il comportamento durante un tentativo di caricamento lazy in un'istanza `DbContext` eliminata.</span><span class="sxs-lookup"><span data-stu-id="4f72a-591">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="4f72a-592">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-592">**Mitigations**</span></span>

<span data-ttu-id="4f72a-593">Aggiornare il codice dell'applicazione per fare in modo che non venga tentato il caricamento lazy con un contesto eliminato oppure specificare una configurazione in modo che non venga eseguita alcuna operazione come descritto nel messaggio di eccezione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-593">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="4f72a-594">La creazione di un numero eccessivo di provider di servizi interni è ora un errore per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="4f72a-594">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="4f72a-595">Problema n. 10236</span><span class="sxs-lookup"><span data-stu-id="4f72a-595">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="4f72a-596">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="4f72a-596">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="4f72a-597">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-597">**Old behavior**</span></span>

<span data-ttu-id="4f72a-598">Nelle versioni precedenti a EF Core 3.0 veniva registrato un avviso per le applicazioni che creavano un numero eccessivo di provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="4f72a-598">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="4f72a-599">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-599">**New behavior**</span></span>

<span data-ttu-id="4f72a-600">A partire da EF Core 3.0, l'avviso viene considerato un errore e viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-600">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="4f72a-601">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-601">**Why**</span></span>

<span data-ttu-id="4f72a-602">Questa modifica è stata apportata per gestire meglio il codice dell'applicazione tramite un'esposizione più esplicita di questa situazione di errore.</span><span class="sxs-lookup"><span data-stu-id="4f72a-602">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="4f72a-603">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-603">**Mitigations**</span></span>

<span data-ttu-id="4f72a-604">L'azione più appropriata quando si verifica questo errore consiste nell'individuare la causa radice e nell'interrompere la creazione di numerosi provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="4f72a-604">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="4f72a-605">È possibile tuttavia convertire nuovamente l'errore in avviso o ignorarlo tramite una configurazione in `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-605">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="4f72a-606">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4f72a-606">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="4f72a-607">Nuovo comportamento per la chiamata di HasOne/HasMany con una singola stringa</span><span class="sxs-lookup"><span data-stu-id="4f72a-607">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="4f72a-608">Problema n. 9171</span><span class="sxs-lookup"><span data-stu-id="4f72a-608">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="4f72a-609">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-609">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-610">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-610">**Old behavior**</span></span>

<span data-ttu-id="4f72a-611">Prima di EF Core 3.0, il codice che chiama `HasOne` o `HasMany` con una singola stringa era interpretato in modo poco chiaro.</span><span class="sxs-lookup"><span data-stu-id="4f72a-611">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="4f72a-612">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4f72a-612">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="4f72a-613">Apparentemente, il codice mette in relazione `Samurai` con un altro tipo di entità tramite la proprietà di navigazione `Entrance`, che può essere privata.</span><span class="sxs-lookup"><span data-stu-id="4f72a-613">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="4f72a-614">In realtà, il codice tenta di creare una relazione con un tipo di entità denominato `Entrance` senza proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-614">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="4f72a-615">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-615">**New behavior**</span></span>

<span data-ttu-id="4f72a-616">A partire da EF Core 3.0, il codice sopra riportato ora esegue quello che avrebbe dovuto fare in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4f72a-616">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="4f72a-617">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-617">**Why**</span></span>

<span data-ttu-id="4f72a-618">Il comportamento precedente era molto poco chiaro, soprattutto durante la lettura del codice di configurazione e la ricerca di errori.</span><span class="sxs-lookup"><span data-stu-id="4f72a-618">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="4f72a-619">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-619">**Mitigations**</span></span>

<span data-ttu-id="4f72a-620">Questa modifica causerà problemi solo nelle applicazioni che configurano relazioni in modo esplicito usando stringhe per i nomi dei tipi e senza specificare in modo esplicito la proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-620">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="4f72a-621">Non è uno scenario comune.</span><span class="sxs-lookup"><span data-stu-id="4f72a-621">This is not common.</span></span>
<span data-ttu-id="4f72a-622">Il comportamento precedente può essere ottenuto passando esplicitamente `null` per il nome della proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-622">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="4f72a-623">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4f72a-623">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="4f72a-624">Il tipo restituito per diversi metodi asincroni è cambiato da Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="4f72a-624">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="4f72a-625">Problema n. 15184</span><span class="sxs-lookup"><span data-stu-id="4f72a-625">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="4f72a-626">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-626">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-627">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-627">**Old behavior**</span></span>

<span data-ttu-id="4f72a-628">I metodi asincroni seguenti in precedenza restituivano il tipo `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="4f72a-628">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="4f72a-629">`ValueGenerator.NextValueAsync()` (e classi derivate)</span><span class="sxs-lookup"><span data-stu-id="4f72a-629">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="4f72a-630">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-630">**New behavior**</span></span>

<span data-ttu-id="4f72a-631">I metodi indicati in precedenza ora restituiscono il tipo `ValueTask<T>` sullo stesso `T` come in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4f72a-631">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="4f72a-632">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-632">**Why**</span></span>

<span data-ttu-id="4f72a-633">Questa modifica riduce il numero delle allocazioni di heap sostenute quando si richiamano questi metodi, con un miglioramento generale delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="4f72a-633">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="4f72a-634">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-634">**Mitigations**</span></span>

<span data-ttu-id="4f72a-635">Le applicazioni semplicemente in attesa delle API precedenti devono solo essere ricompilate e non sono richieste modifiche del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="4f72a-635">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="4f72a-636">Per scenari di utilizzo più complessi (ad esempio, il passaggio del tipo `Task` restituito a `Task.WhenAny()`) è richiesto in genere che il tipo `ValueTask<T>` restituito venga convertito in `Task<T>` chiamando `AsTask()` su di esso.</span><span class="sxs-lookup"><span data-stu-id="4f72a-636">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="4f72a-637">Si noti che in questo modo si annulla la riduzione delle allocazioni consentita da questa modifica.</span><span class="sxs-lookup"><span data-stu-id="4f72a-637">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="4f72a-638">L'annotazione Relational:TypeMapping è ora TypeMapping</span><span class="sxs-lookup"><span data-stu-id="4f72a-638">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="4f72a-639">Problema n. 9913</span><span class="sxs-lookup"><span data-stu-id="4f72a-639">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="4f72a-640">Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.</span><span class="sxs-lookup"><span data-stu-id="4f72a-640">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="4f72a-641">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-641">**Old behavior**</span></span>

<span data-ttu-id="4f72a-642">Il nome di annotazione delle annotazioni di mapping del tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="4f72a-642">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="4f72a-643">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-643">**New behavior**</span></span>

<span data-ttu-id="4f72a-644">Il nome di annotazione delle annotazioni di mapping del tipo è ora "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="4f72a-644">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="4f72a-645">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-645">**Why**</span></span>

<span data-ttu-id="4f72a-646">Il mapping dei tipi non viene più usato solo per i provider di database relazionali.</span><span class="sxs-lookup"><span data-stu-id="4f72a-646">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="4f72a-647">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-647">**Mitigations**</span></span>

<span data-ttu-id="4f72a-648">Ciò causa un'interruzione solo nelle applicazioni che accedono al mapping dei tipi direttamente come annotazione. Questa situazione non è comune.</span><span class="sxs-lookup"><span data-stu-id="4f72a-648">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="4f72a-649">L'azione più appropriata per risolvere il problema consiste nell'usare la superficie dell'API per accedere al mapping dei tipi anziché l'annotazione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-649">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="4f72a-650">ToTable in un tipo derivato genera un'eccezione</span><span class="sxs-lookup"><span data-stu-id="4f72a-650">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="4f72a-651">Problema n. 11811</span><span class="sxs-lookup"><span data-stu-id="4f72a-651">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="4f72a-652">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="4f72a-652">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="4f72a-653">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-653">**Old behavior**</span></span>

<span data-ttu-id="4f72a-654">Nelle versioni precedenti a EF Core 3.0, la chiamata a `ToTable()` in un tipo derivato veniva ignorata poiché soltanto la strategia di mapping dell'ereditarietà era una tabella per gerarchia dove la chiamata non era valida.</span><span class="sxs-lookup"><span data-stu-id="4f72a-654">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="4f72a-655">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-655">**New behavior**</span></span>

<span data-ttu-id="4f72a-656">A partire da EF Core 3.0 e in preparazione all'aggiunta del supporto per la tabella per tipo e per TPC in una versione successiva, la chiamata a `ToTable()` in un tipo derivato genera un'eccezione per evitare una modifica del mapping imprevista in futuro.</span><span class="sxs-lookup"><span data-stu-id="4f72a-656">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="4f72a-657">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-657">**Why**</span></span>

<span data-ttu-id="4f72a-658">Attualmente il mapping di un tipo derivato in una tabella diversa non è un'operazione valida.</span><span class="sxs-lookup"><span data-stu-id="4f72a-658">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="4f72a-659">Questa modifica consente di evitare interruzioni future quando l'operazione diventerà un'operazione valida.</span><span class="sxs-lookup"><span data-stu-id="4f72a-659">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="4f72a-660">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-660">**Mitigations**</span></span>

<span data-ttu-id="4f72a-661">Rimuovere qualsiasi tentativo di mapping di tipi derivati in altre tabelle.</span><span class="sxs-lookup"><span data-stu-id="4f72a-661">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="4f72a-662">ForSqlServerHasIndex sostituito con HasIndex</span><span class="sxs-lookup"><span data-stu-id="4f72a-662">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="4f72a-663">Problema n. 12366</span><span class="sxs-lookup"><span data-stu-id="4f72a-663">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="4f72a-664">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="4f72a-664">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="4f72a-665">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-665">**Old behavior**</span></span>

<span data-ttu-id="4f72a-666">Nelle versioni precedenti a EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offriva un metodo per configurare le colonne usate con `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-666">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="4f72a-667">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-667">**New behavior**</span></span>

<span data-ttu-id="4f72a-668">A partire da EF Core 3.0, l'uso di `Include` in un indice è supportato a livello relazionale.</span><span class="sxs-lookup"><span data-stu-id="4f72a-668">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="4f72a-669">Usare `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-669">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="4f72a-670">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-670">**Why**</span></span>

<span data-ttu-id="4f72a-671">Questa modifica è stata apportata per consolidare l'API per gli indici con `Include` in un'unica posizione per tutti i provider di database.</span><span class="sxs-lookup"><span data-stu-id="4f72a-671">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="4f72a-672">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-672">**Mitigations**</span></span>

<span data-ttu-id="4f72a-673">Usare la nuova API, come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4f72a-673">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="4f72a-674">Modifiche dell'API dei metadati</span><span class="sxs-lookup"><span data-stu-id="4f72a-674">Metadata API changes</span></span>

[<span data-ttu-id="4f72a-675">Problema n. 214</span><span class="sxs-lookup"><span data-stu-id="4f72a-675">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="4f72a-676">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-676">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-677">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-677">**New behavior**</span></span>

<span data-ttu-id="4f72a-678">Le proprietà seguenti sono state convertite in metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="4f72a-678">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="4f72a-679">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-679">**Why**</span></span>

<span data-ttu-id="4f72a-680">Questa modifica semplifica l'implementazione delle interfacce menzionate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4f72a-680">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="4f72a-681">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-681">**Mitigations**</span></span>

<span data-ttu-id="4f72a-682">Usare i nuovi metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-682">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="4f72a-683">Modifiche dell'API dei metadati specifiche del provider</span><span class="sxs-lookup"><span data-stu-id="4f72a-683">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="4f72a-684">Problema n. 214</span><span class="sxs-lookup"><span data-stu-id="4f72a-684">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="4f72a-685">Questa modifica è stata introdotta in EF Core 3.0 anteprima 6.</span><span class="sxs-lookup"><span data-stu-id="4f72a-685">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="4f72a-686">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-686">**New behavior**</span></span>

<span data-ttu-id="4f72a-687">I metodi di estensione specifici del provider verranno resi flat:</span><span class="sxs-lookup"><span data-stu-id="4f72a-687">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="4f72a-688">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-688">**Why**</span></span>

<span data-ttu-id="4f72a-689">Questa modifica semplifica l'implementazione dei metodi di estensione menzionati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4f72a-689">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="4f72a-690">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-690">**Mitigations**</span></span>

<span data-ttu-id="4f72a-691">Usare i nuovi metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-691">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="4f72a-692">EF Core non invia più pragma per l'imposizione della chiave esterna di SQLite</span><span class="sxs-lookup"><span data-stu-id="4f72a-692">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="4f72a-693">Problema n. 12151</span><span class="sxs-lookup"><span data-stu-id="4f72a-693">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="4f72a-694">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="4f72a-694">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="4f72a-695">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-695">**Old behavior**</span></span>

<span data-ttu-id="4f72a-696">Nelle versioni precedenti a EF Core 3.0, EF Core inviava `PRAGMA foreign_keys = 1` quando veniva aperta una connessione a SQLite.</span><span class="sxs-lookup"><span data-stu-id="4f72a-696">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="4f72a-697">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-697">**New behavior**</span></span>

<span data-ttu-id="4f72a-698">A partire da EF Core 3.0, EF Core non invia più `PRAGMA foreign_keys = 1` quando viene aperta una connessione a SQLite.</span><span class="sxs-lookup"><span data-stu-id="4f72a-698">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="4f72a-699">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-699">**Why**</span></span>

<span data-ttu-id="4f72a-700">Questa modifica è stata apportata poiché EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` per impostazione predefinita. Ciò significa che l'imposizione della chiave esterna è abilitata per impostazione predefinita e non deve essere abilitata in modo esplicito ogni volta che viene aperta una connessione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-700">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="4f72a-701">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-701">**Mitigations**</span></span>

<span data-ttu-id="4f72a-702">Le chiavi esterne sono abilitate per impostazione predefinita in SQLitePCLRaw.bundle_e_sqlite3, usato per impostazione predefinita per EF Core.</span><span class="sxs-lookup"><span data-stu-id="4f72a-702">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="4f72a-703">Per gli altri casi, è possibile abilitare le chiavi esterne specificando `Foreign Keys=True` nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-703">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="4f72a-704">Microsoft.EntityFrameworkCore.Sqlite dipende ora da SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="4f72a-704">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="4f72a-705">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-705">**Old behavior**</span></span>

<span data-ttu-id="4f72a-706">Nelle versioni precedenti a EF Core 3.0, EF Core usava `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-706">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="4f72a-707">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-707">**New behavior**</span></span>

<span data-ttu-id="4f72a-708">A partire da EF Core 3.0, EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-708">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="4f72a-709">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-709">**Why**</span></span>

<span data-ttu-id="4f72a-710">Questa modifica è stata apportata per rendere coerente la versione di SQLite usata in iOS con le altre piattaforme.</span><span class="sxs-lookup"><span data-stu-id="4f72a-710">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="4f72a-711">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-711">**Mitigations**</span></span>

<span data-ttu-id="4f72a-712">Per usare la versione di SQLite nativa in iOS, configurare `Microsoft.Data.Sqlite` per l'uso di un'aggregazione `SQLitePCLRaw` diversa.</span><span class="sxs-lookup"><span data-stu-id="4f72a-712">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="4f72a-713">I valori Guid vengono ora archiviati come TEXT in SQLite</span><span class="sxs-lookup"><span data-stu-id="4f72a-713">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="4f72a-714">Problema n. 15078</span><span class="sxs-lookup"><span data-stu-id="4f72a-714">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="4f72a-715">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-715">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-716">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-716">**Old behavior**</span></span>

<span data-ttu-id="4f72a-717">I valori Guid in precedenza venivano archiviati come valori BLOB in SQLite.</span><span class="sxs-lookup"><span data-stu-id="4f72a-717">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="4f72a-718">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-718">**New behavior**</span></span>

<span data-ttu-id="4f72a-719">I valori Guid vengono ora archiviati come TEXT.</span><span class="sxs-lookup"><span data-stu-id="4f72a-719">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="4f72a-720">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-720">**Why**</span></span>

<span data-ttu-id="4f72a-721">Il formato binario dei valori Guid non è standardizzato.</span><span class="sxs-lookup"><span data-stu-id="4f72a-721">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="4f72a-722">L'archiviazione dei valori come TEXT rende il database più compatibile con altre tecnologie.</span><span class="sxs-lookup"><span data-stu-id="4f72a-722">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="4f72a-723">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-723">**Mitigations**</span></span>

<span data-ttu-id="4f72a-724">È possibile eseguire la migrazione dei database esistenti al nuovo formato eseguendo SQL nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="4f72a-724">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="4f72a-725">In EF Core è anche possibile continuare a usare il comportamento precedente configurando un convertitore di valori per queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="4f72a-725">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="4f72a-726">Microsoft.Data.Sqlite rimane in grado di leggere i valori Guid sia da colonne BLOB che TEXT. Tuttavia, poiché è stato modificato il formato predefinito per i parametri e le costanti, probabilmente sarà necessario intervenire per la maggior parte degli scenari che coinvolgono valori Guid.</span><span class="sxs-lookup"><span data-stu-id="4f72a-726">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="4f72a-727">I valori char vengono ora archiviati come testo in SQLite</span><span class="sxs-lookup"><span data-stu-id="4f72a-727">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="4f72a-728">Problema n. 15020</span><span class="sxs-lookup"><span data-stu-id="4f72a-728">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="4f72a-729">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-729">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-730">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-730">**Old behavior**</span></span>

<span data-ttu-id="4f72a-731">I valori char in precedenza venivano archiviati come valori interi in SQLite.</span><span class="sxs-lookup"><span data-stu-id="4f72a-731">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="4f72a-732">Un valore char di *A* veniva ad esempio archiviato come valore intero 65.</span><span class="sxs-lookup"><span data-stu-id="4f72a-732">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="4f72a-733">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-733">**New behavior**</span></span>

<span data-ttu-id="4f72a-734">I valori char vengono ora archiviati come testo.</span><span class="sxs-lookup"><span data-stu-id="4f72a-734">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="4f72a-735">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-735">**Why**</span></span>

<span data-ttu-id="4f72a-736">L'archiviazione dei valori come testo è un'operazione più naturale e rende il database più compatibile con altre tecnologie.</span><span class="sxs-lookup"><span data-stu-id="4f72a-736">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="4f72a-737">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-737">**Mitigations**</span></span>

<span data-ttu-id="4f72a-738">È possibile eseguire la migrazione dei database esistenti al nuovo formato eseguendo SQL nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="4f72a-738">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="4f72a-739">In EF Core è anche possibile continuare a usare il comportamento precedente configurando un convertitore di valori per queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="4f72a-739">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="4f72a-740">Microsoft.Data.Sqlite rimane comunque in grado di leggere i valori di caratteri presenti sia nelle colonne di valori interi sia in quelle di testo, quindi alcuni scenari potrebbero non richiedere alcuna azione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-740">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="4f72a-741">Gli ID di migrazione vengono ora generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica</span><span class="sxs-lookup"><span data-stu-id="4f72a-741">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="4f72a-742">Problema n. 12978</span><span class="sxs-lookup"><span data-stu-id="4f72a-742">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="4f72a-743">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-743">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-744">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-744">**Old behavior**</span></span>

<span data-ttu-id="4f72a-745">Gli ID di migrazione venivano inavvertitamente generati usando il calendario delle impostazioni cultura correnti.</span><span class="sxs-lookup"><span data-stu-id="4f72a-745">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="4f72a-746">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-746">**New behavior**</span></span>

<span data-ttu-id="4f72a-747">Gli ID di migrazione ora vengono sempre generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica (calendario gregoriano).</span><span class="sxs-lookup"><span data-stu-id="4f72a-747">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="4f72a-748">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-748">**Why**</span></span>

<span data-ttu-id="4f72a-749">L'ordine delle migrazioni è importante quando si esegue l'aggiornamento del database o si risolvono i conflitti di unione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-749">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="4f72a-750">L'uso del calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica evita i problemi che possono verificarsi quando i membri del team hanno calendari di sistema diversi.</span><span class="sxs-lookup"><span data-stu-id="4f72a-750">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="4f72a-751">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-751">**Mitigations**</span></span>

<span data-ttu-id="4f72a-752">Questa modifica interessa gli utenti che usano un calendario non gregoriano in cui l'anno ha un'estensione superiore al calendario gregoriano (come il calendario buddista tailandese).</span><span class="sxs-lookup"><span data-stu-id="4f72a-752">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="4f72a-753">Gli ID di migrazione esistenti dovranno essere aggiornati in modo che le nuove migrazioni vengano collocate dopo le migrazioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="4f72a-753">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="4f72a-754">L'ID di migrazione è disponibile nell'attributo di migrazione presente nei file di progettazione delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="4f72a-754">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="4f72a-755">È necessario aggiornare anche la tabella della cronologia delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="4f72a-755">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="4f72a-756">Il metodo UseRowNumberForPaging è stato rimosso</span><span class="sxs-lookup"><span data-stu-id="4f72a-756">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="4f72a-757">Problema n. 16400</span><span class="sxs-lookup"><span data-stu-id="4f72a-757">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="4f72a-758">Questa modifica è stata introdotta in EF Core 3.0 anteprima 6.</span><span class="sxs-lookup"><span data-stu-id="4f72a-758">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="4f72a-759">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-759">**Old behavior**</span></span>

<span data-ttu-id="4f72a-760">Prima di EF Core 3.0 si poteva usare `UseRowNumberForPaging` per generare codice SQL per la suddivisione in pagine compatibile con SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="4f72a-760">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="4f72a-761">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-761">**New behavior**</span></span>

<span data-ttu-id="4f72a-762">A partire da EF Core 3.0, EF genererà solo codice SQL per la suddivisione in pagine compatibile solo con versioni successive di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4f72a-762">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="4f72a-763">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-763">**Why**</span></span>

<span data-ttu-id="4f72a-764">Questa modifica è stata apportata perché [SQL Server 2008 non è più un prodotto supportato](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) e l'aggiornamento di questa funzionalità per interagire con le modifiche apportate per le query in EF Core 3.0 è un lavoro significativo.</span><span class="sxs-lookup"><span data-stu-id="4f72a-764">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="4f72a-765">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-765">**Mitigations**</span></span>

<span data-ttu-id="4f72a-766">È consigliabile eseguire l'aggiornamento a una versione più recente di SQL Server o usare un livello di compatibilità superiore, in modo che il codice SQL generato sia supportato.</span><span class="sxs-lookup"><span data-stu-id="4f72a-766">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="4f72a-767">Detto questo, se non è possibile procedere in questo modo, [aggiungere un commento per il problema](https://github.com/aspnet/EntityFrameworkCore/issues/16400) con indicazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="4f72a-767">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="4f72a-768">Microsoft potrebbe rivedere questa decisione in base ai commenti e suggerimenti.</span><span class="sxs-lookup"><span data-stu-id="4f72a-768">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="4f72a-769">Info/metadati dell'estensione rimossi da IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="4f72a-769">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="4f72a-770">Problema n. 16119</span><span class="sxs-lookup"><span data-stu-id="4f72a-770">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="4f72a-771">Questa modifica è stata introdotta in EF Core 3.0 anteprima 7.</span><span class="sxs-lookup"><span data-stu-id="4f72a-771">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="4f72a-772">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-772">**Old behavior**</span></span>

<span data-ttu-id="4f72a-773">`IDbContextOptionsExtension` conteneva metodi per fornire i metadati relativi all'estensione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-773">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="4f72a-774">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-774">**New behavior**</span></span>

<span data-ttu-id="4f72a-775">Questi metodi sono stati spostati in una nuova classe di base astratta `DbContextOptionsExtensionInfo`, restituita da una nuova proprietà `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-775">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="4f72a-776">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-776">**Why**</span></span>

<span data-ttu-id="4f72a-777">Nelle versioni dalla 2.0 alla 3.0 è stato necessario aggiungere o modificare questi metodi più volte.</span><span class="sxs-lookup"><span data-stu-id="4f72a-777">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="4f72a-778">Suddividendoli in una nuova classe di base astratta sarà più facile apportare questo tipo di modifiche senza compromettere il funzionamento delle estensioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="4f72a-778">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="4f72a-779">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-779">**Mitigations**</span></span>

<span data-ttu-id="4f72a-780">Aggiornare le estensioni per seguire il nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="4f72a-780">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="4f72a-781">Sono disponibili esempi nelle numerose implementazioni di `IDbContextOptionsExtension` per diversi tipi di estensioni nel codice sorgente di EF Core.</span><span class="sxs-lookup"><span data-stu-id="4f72a-781">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="4f72a-782">LogQueryPossibleExceptionWithAggregateOperator è stato rinominato</span><span class="sxs-lookup"><span data-stu-id="4f72a-782">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="4f72a-783">Problema n. 10985</span><span class="sxs-lookup"><span data-stu-id="4f72a-783">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="4f72a-784">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-784">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-785">**Modifica**</span><span class="sxs-lookup"><span data-stu-id="4f72a-785">**Change**</span></span>

<span data-ttu-id="4f72a-786">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` è stato rinominato in `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="4f72a-786">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="4f72a-787">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-787">**Why**</span></span>

<span data-ttu-id="4f72a-788">Allineamento del nome di questo evento di avviso con tutti gli altri eventi di avviso.</span><span class="sxs-lookup"><span data-stu-id="4f72a-788">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="4f72a-789">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-789">**Mitigations**</span></span>

<span data-ttu-id="4f72a-790">Usare il nuovo nome.</span><span class="sxs-lookup"><span data-stu-id="4f72a-790">Use the new name.</span></span> <span data-ttu-id="4f72a-791">(Si noti che il numero di ID evento non è stato modificato.)</span><span class="sxs-lookup"><span data-stu-id="4f72a-791">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="4f72a-792">Chiarimenti per l'API per i nomi di vincolo di chiave esterna</span><span class="sxs-lookup"><span data-stu-id="4f72a-792">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="4f72a-793">Problema n. 10730</span><span class="sxs-lookup"><span data-stu-id="4f72a-793">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="4f72a-794">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-794">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-795">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-795">**Old behavior**</span></span>

<span data-ttu-id="4f72a-796">Prima di EF Core 3.0, si faceva riferimento ai nomi di vincolo di chiave esterna semplicemente con "Name".</span><span class="sxs-lookup"><span data-stu-id="4f72a-796">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="4f72a-797">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4f72a-797">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="4f72a-798">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-798">**New behavior**</span></span>

<span data-ttu-id="4f72a-799">A partire da EF Core 3.0, si fa ora riferimento ai nomi di vincolo di chiave esterna con "ConstraintName".</span><span class="sxs-lookup"><span data-stu-id="4f72a-799">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="4f72a-800">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4f72a-800">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="4f72a-801">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-801">**Why**</span></span>

<span data-ttu-id="4f72a-802">Questa modifica introduce coerenza per la denominazione in quest'area e chiarisce anche che si tratta del nome del vincolo di chiave esterna e non del nome della colonna o della proprietà per cui è definita la chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="4f72a-802">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="4f72a-803">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-803">**Mitigations**</span></span>

<span data-ttu-id="4f72a-804">Usare il nuovo nome.</span><span class="sxs-lookup"><span data-stu-id="4f72a-804">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="4f72a-805">IRelationalDatabaseCreator.HasTables/HasTablesAsync sono diventati pubblici</span><span class="sxs-lookup"><span data-stu-id="4f72a-805">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="4f72a-806">Problema n. 15997</span><span class="sxs-lookup"><span data-stu-id="4f72a-806">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="4f72a-807">Questa modifica è stata introdotta in EF Core 3.0 anteprima 7.</span><span class="sxs-lookup"><span data-stu-id="4f72a-807">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="4f72a-808">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-808">**Old behavior**</span></span>

<span data-ttu-id="4f72a-809">Prima di EF Core 3.0 questi metodi erano protetti.</span><span class="sxs-lookup"><span data-stu-id="4f72a-809">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="4f72a-810">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-810">**New behavior**</span></span>

<span data-ttu-id="4f72a-811">A partire da EF Core 3.0 questi metodi sono pubblici.</span><span class="sxs-lookup"><span data-stu-id="4f72a-811">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="4f72a-812">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-812">**Why**</span></span>

<span data-ttu-id="4f72a-813">Questi metodi vengono usati da EF per determinare se un database viene creato, ma vuoto.</span><span class="sxs-lookup"><span data-stu-id="4f72a-813">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="4f72a-814">Ciò risulta utile all'esterno di EF quando occorre determinare se applicare o meno le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="4f72a-814">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="4f72a-815">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-815">**Mitigations**</span></span>

<span data-ttu-id="4f72a-816">Modificare l'accessibilità di eventuali override.</span><span class="sxs-lookup"><span data-stu-id="4f72a-816">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="4f72a-817">Microsoft.EntityFrameworkCore.Design è ora un pacchetto DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="4f72a-817">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="4f72a-818">Problema n. 11506</span><span class="sxs-lookup"><span data-stu-id="4f72a-818">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="4f72a-819">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="4f72a-819">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="4f72a-820">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-820">**Old behavior**</span></span>

<span data-ttu-id="4f72a-821">Prima di EF Core 3.0, Microsoft.EntityFrameworkCore.Design era un pacchetto NuGet normale ed era possibile fare riferimento al relativo assembly dai progetti dipendenti.</span><span class="sxs-lookup"><span data-stu-id="4f72a-821">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="4f72a-822">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-822">**New behavior**</span></span>

<span data-ttu-id="4f72a-823">A partire da EF Core 3.0 è un pacchetto DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="4f72a-823">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="4f72a-824">Questo significa che la dipendenza non verrà trasferita in modo transitivo in altri progetti e che non è più possibile fare riferimento all'assembly per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="4f72a-824">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="4f72a-825">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-825">**Why**</span></span>

<span data-ttu-id="4f72a-826">Questo pacchetto è destinato solo all'uso in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-826">This package is only intended to be used at design time.</span></span> <span data-ttu-id="4f72a-827">Le applicazioni distribuite non devono farvi riferimento.</span><span class="sxs-lookup"><span data-stu-id="4f72a-827">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="4f72a-828">Questa raccomandazione è rafforzata dall'impostazione del pacchetto come DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="4f72a-828">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="4f72a-829">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-829">**Mitigations**</span></span>

<span data-ttu-id="4f72a-830">Se è necessario fare riferimento a questo pacchetto per eseguire l'override del comportamento in fase di progettazione di EF Core, è possibile aggiornare i metadati dell'elemento PackageReference nel progetto.</span><span class="sxs-lookup"><span data-stu-id="4f72a-830">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="4f72a-831">In presenza di riferimenti transitivi al pacchetto tramite Microsoft.EntityFrameworkCore.Tools, sarà necessario aggiungere un PackageReference esplicito al pacchetto per modificare i relativi metadati.</span><span class="sxs-lookup"><span data-stu-id="4f72a-831">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="4f72a-832">Aggiornamento di SQLitePCL.raw alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="4f72a-832">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="4f72a-833">Problema n. 14824</span><span class="sxs-lookup"><span data-stu-id="4f72a-833">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="4f72a-834">Questa modifica è stata introdotta in EF Core 3.0 anteprima 7.</span><span class="sxs-lookup"><span data-stu-id="4f72a-834">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="4f72a-835">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-835">**Old behavior**</span></span>

<span data-ttu-id="4f72a-836">Microsoft.EntityFrameworkCore.Sqlite dipendeva in precedenza dalla versione 1.1.12 di SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="4f72a-836">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="4f72a-837">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-837">**New behavior**</span></span>

<span data-ttu-id="4f72a-838">Il pacchetto è stato aggiornato in modo da dipendere dalla versione 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="4f72a-838">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="4f72a-839">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-839">**Why**</span></span>

<span data-ttu-id="4f72a-840">La versione 2.0.0 di SQLitePCL.raw è destinata a .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="4f72a-840">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="4f72a-841">Era in precedenza destinata a .NET Standard 1.1 e ciò richiedeva un notevole impegno di chiusura di pacchetti transitivi per il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="4f72a-841">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="4f72a-842">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-842">**Mitigations**</span></span>

<span data-ttu-id="4f72a-843">SQLitePCL.raw versione 2.0.0 include alcune modifiche che causano un'interruzione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-843">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="4f72a-844">Per informazioni dettagliate, vedere le [note sulla versione](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="4f72a-844">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="4f72a-845">NetTopologySuite aggiornato alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="4f72a-845">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="4f72a-846">Problema n. 14825</span><span class="sxs-lookup"><span data-stu-id="4f72a-846">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="4f72a-847">Questa modifica è stata introdotta in EF Core 3.0 anteprima 7.</span><span class="sxs-lookup"><span data-stu-id="4f72a-847">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="4f72a-848">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-848">**Old behavior**</span></span>

<span data-ttu-id="4f72a-849">I pacchetti spaziali dipendevano in precedenza dalla versione 1.15.1 di NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="4f72a-849">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="4f72a-850">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-850">**New behavior**</span></span>

<span data-ttu-id="4f72a-851">Il pacchetto è stato aggiornato in modo da dipendere dalla versione 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="4f72a-851">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="4f72a-852">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-852">**Why**</span></span>

<span data-ttu-id="4f72a-853">La versione 2.0.0 di NetTopologySuite risolve vari problemi di usabilità riscontrati dagli utenti di EF Core.</span><span class="sxs-lookup"><span data-stu-id="4f72a-853">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="4f72a-854">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-854">**Mitigations**</span></span>

<span data-ttu-id="4f72a-855">NetTopologySuite versione 2.0.0 include alcune modifiche che causano un'interruzione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-855">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="4f72a-856">Per informazioni dettagliate, vedere le [note sulla versione](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="4f72a-856">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="4f72a-857">Devono essere configurare più relazioni ambigue che fanno riferimento a se stesse</span><span class="sxs-lookup"><span data-stu-id="4f72a-857">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="4f72a-858">Problema n. 13573</span><span class="sxs-lookup"><span data-stu-id="4f72a-858">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="4f72a-859">Questa modifica è stata introdotta in EF Core 3.0 anteprima 6.</span><span class="sxs-lookup"><span data-stu-id="4f72a-859">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="4f72a-860">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="4f72a-860">**Old behavior**</span></span>

<span data-ttu-id="4f72a-861">Un tipo di entità con più proprietà di navigazione unidirezionale che fanno riferimento a se stesse e più chiavi esterne corrispondenti è stato erroneamente configurato come relazione singola.</span><span class="sxs-lookup"><span data-stu-id="4f72a-861">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="4f72a-862">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4f72a-862">For example:</span></span>

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

<span data-ttu-id="4f72a-863">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="4f72a-863">**New behavior**</span></span>

<span data-ttu-id="4f72a-864">Questo scenario viene ora rilevato nella compilazione del modello e viene generata un'eccezione indicante che il modello è ambiguo.</span><span class="sxs-lookup"><span data-stu-id="4f72a-864">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="4f72a-865">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="4f72a-865">**Why**</span></span>

<span data-ttu-id="4f72a-866">Il modello risultante era ambiguo e sarà probabilmente errato in questo caso.</span><span class="sxs-lookup"><span data-stu-id="4f72a-866">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="4f72a-867">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="4f72a-867">**Mitigations**</span></span>

<span data-ttu-id="4f72a-868">Usare la configurazione completa della relazione.</span><span class="sxs-lookup"><span data-stu-id="4f72a-868">Use full configuration of the relationship.</span></span> <span data-ttu-id="4f72a-869">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4f72a-869">For example:</span></span>

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
