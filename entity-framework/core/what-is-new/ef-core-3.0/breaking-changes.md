---
title: Modifiche che causano un'interruzione in EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: c73663412efcd93c04892f193d4f5a2485724e22
ms.sourcegitcommit: 755a15a789631cc4ea581e2262a2dcc49c219eef
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/25/2019
ms.locfileid: "68497534"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="99e15-102">Modifiche che causano un'interruzione incluse in EF Core 3.0 (attualmente in anteprima)</span><span class="sxs-lookup"><span data-stu-id="99e15-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="99e15-103">Si tenga presente che i set di funzionalità e le pianificazioni delle versioni future sono sempre soggette a modifiche e che questa pagina, nonostante l'impegno profuso per mantenerla aggiornata, potrebbe non riflettere sempre i piani più recenti.</span><span class="sxs-lookup"><span data-stu-id="99e15-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="99e15-104">Le modifiche all'API e al comportamento seguenti possono potenzialmente interrompere le applicazioni sviluppate per EF Core 2.2.x durante l'aggiornamento alla versione 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="99e15-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="99e15-105">Le modifiche che si prevede abbiano impatto solo sui provider di database sono documentate nelle [modifiche che influiscono sul provider](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="99e15-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="99e15-106">Le interruzioni nelle nuove funzionalità introdotte da un'anteprima 3.0 a un'altra anteprima 3.0 non sono documentate.</span><span class="sxs-lookup"><span data-stu-id="99e15-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="99e15-107">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="99e15-107">Summary</span></span>

| <span data-ttu-id="99e15-108">**Modifica di rilievo**</span><span class="sxs-lookup"><span data-stu-id="99e15-108">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="99e15-109">**Impatto**</span><span class="sxs-lookup"><span data-stu-id="99e15-109">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="99e15-110">Le query LINQ non vengono più valutate nel client</span><span class="sxs-lookup"><span data-stu-id="99e15-110">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="99e15-111">High</span><span class="sxs-lookup"><span data-stu-id="99e15-111">High</span></span>       |
| [<span data-ttu-id="99e15-112">Lo strumento da riga di comando di EF Core, dotnet ef, non è più incluso in .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="99e15-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="99e15-113">High</span><span class="sxs-lookup"><span data-stu-id="99e15-113">High</span></span>      |
| [<span data-ttu-id="99e15-114">I metodi FromSql, ExecuteSql ed ExecuteSqlAsync sono stati rinominati</span><span class="sxs-lookup"><span data-stu-id="99e15-114">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="99e15-115">High</span><span class="sxs-lookup"><span data-stu-id="99e15-115">High</span></span>      |
| [<span data-ttu-id="99e15-116">I tipi di query vengono consolidati con tipi di entità</span><span class="sxs-lookup"><span data-stu-id="99e15-116">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="99e15-117">High</span><span class="sxs-lookup"><span data-stu-id="99e15-117">High</span></span>      |
| [<span data-ttu-id="99e15-118">Entity Framework Core non è più incluso nel framework condiviso di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="99e15-118">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="99e15-119">Medio</span><span class="sxs-lookup"><span data-stu-id="99e15-119">Medium</span></span>      |
| [<span data-ttu-id="99e15-120">Le eliminazioni a catena vengono ora eseguite immediatamente per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="99e15-120">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="99e15-121">Medio</span><span class="sxs-lookup"><span data-stu-id="99e15-121">Medium</span></span>      |
| [<span data-ttu-id="99e15-122">Semantica più chiara per DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="99e15-122">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="99e15-123">Medio</span><span class="sxs-lookup"><span data-stu-id="99e15-123">Medium</span></span>      |
| [<span data-ttu-id="99e15-124">L'API di configurazione per le relazioni di tipo di proprietà è stata modificata</span><span class="sxs-lookup"><span data-stu-id="99e15-124">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="99e15-125">Medio</span><span class="sxs-lookup"><span data-stu-id="99e15-125">Medium</span></span>      |
| [<span data-ttu-id="99e15-126">Ogni proprietà usa la generazione di chiavi di tipo intero in memoria indipendenti</span><span class="sxs-lookup"><span data-stu-id="99e15-126">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="99e15-127">Medio</span><span class="sxs-lookup"><span data-stu-id="99e15-127">Medium</span></span>      |
| [<span data-ttu-id="99e15-128">Modifiche dell'API dei metadati</span><span class="sxs-lookup"><span data-stu-id="99e15-128">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="99e15-129">Medio</span><span class="sxs-lookup"><span data-stu-id="99e15-129">Medium</span></span>      |
| [<span data-ttu-id="99e15-130">Modifiche dell'API dei metadati specifiche del provider</span><span class="sxs-lookup"><span data-stu-id="99e15-130">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="99e15-131">Medio</span><span class="sxs-lookup"><span data-stu-id="99e15-131">Medium</span></span>      |
| [<span data-ttu-id="99e15-132">Il metodo UseRowNumberForPaging è stato rimosso</span><span class="sxs-lookup"><span data-stu-id="99e15-132">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="99e15-133">Medio</span><span class="sxs-lookup"><span data-stu-id="99e15-133">Medium</span></span>      |
| [<span data-ttu-id="99e15-134">I metodi FromSql possono essere specificati solo in radici di query</span><span class="sxs-lookup"><span data-stu-id="99e15-134">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="99e15-135">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-135">Low</span></span>      |
| [<span data-ttu-id="99e15-136">~~L'esecuzione di query viene registrata a livello di debug~~ - Modifica annullata</span><span class="sxs-lookup"><span data-stu-id="99e15-136">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="99e15-137">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-137">Low</span></span>      |
| [<span data-ttu-id="99e15-138">I valori di chiave temporanei non sono più impostati nelle istanze di entità</span><span class="sxs-lookup"><span data-stu-id="99e15-138">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="99e15-139">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-139">Low</span></span>      |
| [<span data-ttu-id="99e15-140">DetectChanges rispetta i valori di chiave generati dall'archivio</span><span class="sxs-lookup"><span data-stu-id="99e15-140">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="99e15-141">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-141">Low</span></span>      |
| [<span data-ttu-id="99e15-142">Le entità dipendenti che condividono la tabella con l'entità di sicurezza sono ora facoltative</span><span class="sxs-lookup"><span data-stu-id="99e15-142">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="99e15-143">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-143">Low</span></span>      |
| [<span data-ttu-id="99e15-144">Tutte le entità che condividono una tabella con una colonna di token di concorrenza devono eseguirne il mapping a una proprietà</span><span class="sxs-lookup"><span data-stu-id="99e15-144">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="99e15-145">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-145">Low</span></span>      |
| [<span data-ttu-id="99e15-146">Per le proprietà ereditate da tipi senza mapping viene ora eseguito il mapping a una singola colonna per tutti i tipi derivati</span><span class="sxs-lookup"><span data-stu-id="99e15-146">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="99e15-147">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-147">Low</span></span>      |
| [<span data-ttu-id="99e15-148">La convenzione di proprietà di chiave esterna non ha più lo stesso nome della proprietà dell'entità di sicurezza</span><span class="sxs-lookup"><span data-stu-id="99e15-148">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="99e15-149">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-149">Low</span></span>      |
| [<span data-ttu-id="99e15-150">La connessione di database viene ora chiusa se non viene più usata prima del completamento di TransactionScope</span><span class="sxs-lookup"><span data-stu-id="99e15-150">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="99e15-151">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-151">Low</span></span>      |
| [<span data-ttu-id="99e15-152">I campi sottostanti vengono usati per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="99e15-152">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="99e15-153">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-153">Low</span></span>      |
| [<span data-ttu-id="99e15-154">Viene generata un'eccezione se vengono trovati più campi sottostanti compatibili</span><span class="sxs-lookup"><span data-stu-id="99e15-154">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="99e15-155">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-155">Low</span></span>      |
| [<span data-ttu-id="99e15-156">I nomi delle proprietà solo campo devono corrispondere al nome di campo</span><span class="sxs-lookup"><span data-stu-id="99e15-156">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="99e15-157">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-157">Low</span></span>      |
| [<span data-ttu-id="99e15-158">AddDbContext/AddDbContextPool non chiamano più AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="99e15-158">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="99e15-159">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-159">Low</span></span>      |
| [<span data-ttu-id="99e15-160">DbContext.Entry esegue ora un DetectChanges locale</span><span class="sxs-lookup"><span data-stu-id="99e15-160">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="99e15-161">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-161">Low</span></span>      |
| [<span data-ttu-id="99e15-162">Le chiavi matrice di byte e di stringhe non vengono generate dal client per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="99e15-162">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="99e15-163">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-163">Low</span></span>      |
| [<span data-ttu-id="99e15-164">ILoggerFactory è ora un servizio con ambito</span><span class="sxs-lookup"><span data-stu-id="99e15-164">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="99e15-165">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-165">Low</span></span>      |
| [<span data-ttu-id="99e15-166">I proxy di caricamento lazy non presuppongono più che le proprietà di navigazione vengano caricate completamente</span><span class="sxs-lookup"><span data-stu-id="99e15-166">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="99e15-167">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-167">Low</span></span>      |
| [<span data-ttu-id="99e15-168">La creazione di un numero eccessivo di provider di servizi interni è ora un errore per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="99e15-168">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="99e15-169">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-169">Low</span></span>      |
| [<span data-ttu-id="99e15-170">Nuovo comportamento per la chiamata di HasOne/HasMany con una singola stringa</span><span class="sxs-lookup"><span data-stu-id="99e15-170">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="99e15-171">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-171">Low</span></span>      |
| [<span data-ttu-id="99e15-172">Il tipo restituito per diversi metodi asincroni è cambiato da Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="99e15-172">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="99e15-173">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-173">Low</span></span>      |
| [<span data-ttu-id="99e15-174">L'annotazione Relational:TypeMapping è ora TypeMapping</span><span class="sxs-lookup"><span data-stu-id="99e15-174">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="99e15-175">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-175">Low</span></span>      |
| [<span data-ttu-id="99e15-176">ToTable in un tipo derivato genera un'eccezione</span><span class="sxs-lookup"><span data-stu-id="99e15-176">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="99e15-177">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-177">Low</span></span>      |
| [<span data-ttu-id="99e15-178">EF Core non invia più pragma per l'imposizione della chiave esterna di SQLite</span><span class="sxs-lookup"><span data-stu-id="99e15-178">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="99e15-179">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-179">Low</span></span>      |
| [<span data-ttu-id="99e15-180">Microsoft.EntityFrameworkCore.Sqlite dipende ora da SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="99e15-180">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="99e15-181">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-181">Low</span></span>      |
| [<span data-ttu-id="99e15-182">I valori Guid vengono ora archiviati come TEXT in SQLite</span><span class="sxs-lookup"><span data-stu-id="99e15-182">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="99e15-183">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-183">Low</span></span>      |
| [<span data-ttu-id="99e15-184">I valori char vengono ora archiviati come testo in SQLite</span><span class="sxs-lookup"><span data-stu-id="99e15-184">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="99e15-185">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-185">Low</span></span>      |
| [<span data-ttu-id="99e15-186">Gli ID di migrazione vengono ora generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica</span><span class="sxs-lookup"><span data-stu-id="99e15-186">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="99e15-187">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-187">Low</span></span>      |
| [<span data-ttu-id="99e15-188">Info/metadati dell'estensione rimossi da IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="99e15-188">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="99e15-189">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-189">Low</span></span>      |
| [<span data-ttu-id="99e15-190">LogQueryPossibleExceptionWithAggregateOperator è stato rinominato</span><span class="sxs-lookup"><span data-stu-id="99e15-190">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="99e15-191">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-191">Low</span></span>      |
| [<span data-ttu-id="99e15-192">Chiarimenti per l'API per i nomi di vincolo di chiave esterna</span><span class="sxs-lookup"><span data-stu-id="99e15-192">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="99e15-193">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-193">Low</span></span>      |
| [<span data-ttu-id="99e15-194">IRelationalDatabaseCreator.HasTables/HasTablesAsync sono diventati pubblici</span><span class="sxs-lookup"><span data-stu-id="99e15-194">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="99e15-195">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-195">Low</span></span>      |
| [<span data-ttu-id="99e15-196">Microsoft.EntityFrameworkCore.Design è ora un pacchetto DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="99e15-196">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="99e15-197">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-197">Low</span></span>      |
| [<span data-ttu-id="99e15-198">Aggiornamento di SQLitePCL.raw alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="99e15-198">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="99e15-199">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-199">Low</span></span>      |
| [<span data-ttu-id="99e15-200">NetTopologySuite aggiornato alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="99e15-200">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="99e15-201">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-201">Low</span></span>      |
| [<span data-ttu-id="99e15-202">Devono essere configurare più relazioni ambigue che fanno riferimento a se stesse</span><span class="sxs-lookup"><span data-stu-id="99e15-202">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="99e15-203">Bassa</span><span class="sxs-lookup"><span data-stu-id="99e15-203">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="99e15-204">Le query LINQ non vengono più valutate nel client</span><span class="sxs-lookup"><span data-stu-id="99e15-204">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="99e15-205">[Problema n. 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Vedere anche il problema n. 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="99e15-205">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="99e15-206">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-206">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-207">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-207">**Old behavior**</span></span>

<span data-ttu-id="99e15-208">Nelle versioni precedenti alla versione 3.0, quando EF Core non era in grado di convertire un'espressione inclusa in una query in SQL o in un parametro, l'espressione veniva automaticamente valutata nel client.</span><span class="sxs-lookup"><span data-stu-id="99e15-208">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="99e15-209">Per impostazione predefinita, la valutazione client di espressioni potenzialmente dispendiose si limitava ad attivare solo un avviso.</span><span class="sxs-lookup"><span data-stu-id="99e15-209">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="99e15-210">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-210">**New behavior**</span></span>

<span data-ttu-id="99e15-211">A partire dalla versione 3.0, EF Core consente solo la valutazione delle espressioni nella proiezione di primo livello (l'ultima chiamata `Select()` nella query) nel client.</span><span class="sxs-lookup"><span data-stu-id="99e15-211">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="99e15-212">Quando le espressioni in altre posizioni all'interno della query non possono essere convertite in SQL o in un parametro, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="99e15-212">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="99e15-213">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-213">**Why**</span></span>

<span data-ttu-id="99e15-214">La valutazione client automatica delle query consente di eseguire numerose query anche nel caso in cui parti importanti delle query non possono essere convertite.</span><span class="sxs-lookup"><span data-stu-id="99e15-214">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="99e15-215">Questo comportamento può causare un comportamento imprevisto e potenzialmente dannoso che può diventare evidente solo in produzione.</span><span class="sxs-lookup"><span data-stu-id="99e15-215">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="99e15-216">Ad esempio, una condizione in una chiamata `Where()` che non può essere convertita può causare il trasferimento di tutte le righe della tabella del server di database e l'applicazione del filtro nel client.</span><span class="sxs-lookup"><span data-stu-id="99e15-216">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="99e15-217">È probabile che questa situazione non venga rilevata se la tabella contiene solo alcune righe in fase di sviluppo, ma che abbia un grande impatto quando l'applicazione passa in produzione dove la tabella può contenere milioni di righe.</span><span class="sxs-lookup"><span data-stu-id="99e15-217">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="99e15-218">Gli avvisi di valutazione client inoltre si sono rivelati molto facili da ignorare durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="99e15-218">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="99e15-219">Inoltre, la valutazione client automatica può causare problemi in cui il miglioramento della conversione di query per espressioni specifiche causa modifiche impreviste che causano un'interruzione da una versione all'altra.</span><span class="sxs-lookup"><span data-stu-id="99e15-219">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="99e15-220">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-220">**Mitigations**</span></span>

<span data-ttu-id="99e15-221">Se una query non può essere convertita completamente, riscrivere la query in un formato che possa essere convertito o usare `AsEnumerable()`, `ToList()` o un elemento simile per riportare in modo esplicito i dati nel client dove possono essere quindi ulteriormente elaborati usando LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="99e15-221">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="99e15-222">Entity Framework Core non è più incluso nel framework condiviso di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="99e15-222">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="99e15-223">Annunci problema n. 325</span><span class="sxs-lookup"><span data-stu-id="99e15-223">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="99e15-224">Questa modifica è stata introdotta in ASP.NET Core 3.0 anteprima 1.</span><span class="sxs-lookup"><span data-stu-id="99e15-224">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="99e15-225">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-225">**Old behavior**</span></span>

<span data-ttu-id="99e15-226">Nelle versioni precedenti ad ASP.NET Core 3.0, quando si aggiungeva un riferimento a un pacchetto in `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, veniva inserito EF Core e alcuni dei provider di dati di EF Core come il provider di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="99e15-226">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="99e15-227">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-227">**New behavior**</span></span>

<span data-ttu-id="99e15-228">A partire dalla versione 3.0, il framework condiviso di ASP.NET Core non include EF Core o provider di dati di EF Core.</span><span class="sxs-lookup"><span data-stu-id="99e15-228">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="99e15-229">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-229">**Why**</span></span>

<span data-ttu-id="99e15-230">Prima di questa modifica, per ottenere EF Core erano necessarie procedure diverse, a seconda che l'applicazione avesse o meno come destinazione ASP.NET Core e SQL Server.</span><span class="sxs-lookup"><span data-stu-id="99e15-230">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="99e15-231">Inoltre, l'aggiornamento di ASP.NET Core forzava l'aggiornamento di EF Core e del provider di SQL Server, non sempre auspicabile.</span><span class="sxs-lookup"><span data-stu-id="99e15-231">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="99e15-232">Con questa modifica, la procedura per ottenere EF Core è la stessa in tutti i provider, le implementazioni .NET supportate e i tipi di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="99e15-232">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="99e15-233">Gli sviluppatori possono ora controllare anche in modo preciso quando vengono aggiornati EF Core e i provider di dati di EF Core.</span><span class="sxs-lookup"><span data-stu-id="99e15-233">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="99e15-234">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-234">**Mitigations**</span></span>

<span data-ttu-id="99e15-235">Per usare EF Core in un'applicazione ASP.NET Core 3.0 o in un'altra applicazione supportata, aggiungere in modo esplicito un riferimento al pacchetto al provider di database di EF Core che verrà usato dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="99e15-235">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="99e15-236">Lo strumento della riga di comando di EF Core, dotnet ef, non è più incluso in .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="99e15-236">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="99e15-237">Problema n. 14016</span><span class="sxs-lookup"><span data-stu-id="99e15-237">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="99e15-238">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4 e nella versione corrispondente di .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="99e15-238">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="99e15-239">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-239">**Old behavior**</span></span>

<span data-ttu-id="99e15-240">Prima della versione 3.0, lo strumento `dotnet ef` era incluso in .NET Core SDK ed era immediatamente disponibile dalla riga di comando di qualsiasi progetto senza richiedere passaggi aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="99e15-240">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="99e15-241">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-241">**New behavior**</span></span>

<span data-ttu-id="99e15-242">A partire dalla versione 3.0, .NET SDK non include lo strumento `dotnet ef` pertanto, prima di poterlo usare, è necessario installarlo in modo esplicito come strumento locale o globale.</span><span class="sxs-lookup"><span data-stu-id="99e15-242">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="99e15-243">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-243">**Why**</span></span>

<span data-ttu-id="99e15-244">Questa modifica consente di distribuire e aggiornare `dotnet ef` come uno strumento della riga di comando di .NET in NuGet, coerentemente con il fatto che anche EF Core 3.0 viene distribuito come pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="99e15-244">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="99e15-245">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-245">**Mitigations**</span></span>

<span data-ttu-id="99e15-246">Per essere in grado di gestire le migrazioni o eseguire lo scaffolding di `DbContext`, installare `dotnet-ef` come strumento globale:</span><span class="sxs-lookup"><span data-stu-id="99e15-246">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="99e15-247">È inoltre possibile ottenerlo come strumento locale quando si ripristinano le dipendenze di un progetto che lo dichiara come dipendenza di strumenti utilizzando un [file manifesto dello strumento](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="99e15-247">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="99e15-248">I metodi FromSql, ExecuteSql ed ExecuteSqlAsync sono stati rinominati</span><span class="sxs-lookup"><span data-stu-id="99e15-248">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="99e15-249">Problema n. 10996</span><span class="sxs-lookup"><span data-stu-id="99e15-249">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="99e15-250">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-250">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-251">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-251">**Old behavior**</span></span>

<span data-ttu-id="99e15-252">Prima di EF Core 3.0, erano disponibili overload per questi nomi di metodo per supportare l'uso con una stringa normale o una stringa che deve essere interpolata in SQL e parametri.</span><span class="sxs-lookup"><span data-stu-id="99e15-252">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="99e15-253">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-253">**New behavior**</span></span>

<span data-ttu-id="99e15-254">A partire da EF Core 3.0, usare `FromSqlRaw`, `ExecuteSqlRaw` e `ExecuteSqlRawAsync` per creare una query con parametri in cui i parametri vengono passati separatamente dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="99e15-254">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="99e15-255">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="99e15-255">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="99e15-256">Usare `FromSqlInterpolated`, `ExecuteSqlInterpolated`, e `ExecuteSqlInterpolatedAsync` per creare una query con parametri in cui i parametri vengono passati come parte di una stringa di query interpolata.</span><span class="sxs-lookup"><span data-stu-id="99e15-256">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="99e15-257">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="99e15-257">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="99e15-258">Si noti che entrambe le query precedenti produrranno lo stesso codice SQL con parametri con gli stessi parametri SQL.</span><span class="sxs-lookup"><span data-stu-id="99e15-258">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="99e15-259">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-259">**Why**</span></span>

<span data-ttu-id="99e15-260">Con gli overload di metodi come questi, è molto facile chiamare accidentalmente il metodo con stringa non elaborata anche se l'intento era chiamare il metodo con stringa interpolata e viceversa.</span><span class="sxs-lookup"><span data-stu-id="99e15-260">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="99e15-261">Il risultato potrebbero essere query senza parametri, quando invece è prevista la parametrizzazione.</span><span class="sxs-lookup"><span data-stu-id="99e15-261">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="99e15-262">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-262">**Mitigations**</span></span>

<span data-ttu-id="99e15-263">Passare all'uso dei nuovi nomi di metodo.</span><span class="sxs-lookup"><span data-stu-id="99e15-263">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="99e15-264">I metodi FromSql possono essere specificati solo in radici di query</span><span class="sxs-lookup"><span data-stu-id="99e15-264">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="99e15-265">Problema n. 15704</span><span class="sxs-lookup"><span data-stu-id="99e15-265">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="99e15-266">Questa modifica è stata introdotta in EF Core 3.0 anteprima 6.</span><span class="sxs-lookup"><span data-stu-id="99e15-266">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="99e15-267">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-267">**Old behavior**</span></span>

<span data-ttu-id="99e15-268">Prima di EF Core 3.0, il metodo `FromSql` poteva essere specificato in un punto qualsiasi nella query.</span><span class="sxs-lookup"><span data-stu-id="99e15-268">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="99e15-269">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-269">**New behavior**</span></span>

<span data-ttu-id="99e15-270">A partire da EF Core 3.0, i nuovi metodi `FromSqlRaw` e `FromSqlInterpolated` (che sostituiscono`FromSql`) possono essere specificati solo per radici di query, ad esempio direttamente in `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="99e15-270">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="99e15-271">Qualsiasi tentativo di specificarli altrove causerà un errore di compilazione.</span><span class="sxs-lookup"><span data-stu-id="99e15-271">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="99e15-272">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-272">**Why**</span></span>

<span data-ttu-id="99e15-273">La specifica di `FromSql` in qualsiasi posizione diversa da un `DbSet` non ha alcun significato aggiuntivo oppure valore aggiunto e può causare ambiguità in determinati scenari.</span><span class="sxs-lookup"><span data-stu-id="99e15-273">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="99e15-274">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-274">**Mitigations**</span></span>

<span data-ttu-id="99e15-275">Le chiamate di `FromSql` devono essere spostate in modo da comparire direttamente nel `DbSet` a cui si applicano.</span><span class="sxs-lookup"><span data-stu-id="99e15-275">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="99e15-276">~~L'esecuzione di query viene registrata a livello di debug~~ - Modifica annullata</span><span class="sxs-lookup"><span data-stu-id="99e15-276">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="99e15-277">Problema n. 14523</span><span class="sxs-lookup"><span data-stu-id="99e15-277">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="99e15-278">Questa modifica è stata annullata in EF Core 3.0 anteprima 7.</span><span class="sxs-lookup"><span data-stu-id="99e15-278">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="99e15-279">Questa modifica è stata annullata perché la nuova configurazione in EF Core 3.0 consente all'applicazione di specificare il livello di log per qualsiasi evento.</span><span class="sxs-lookup"><span data-stu-id="99e15-279">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="99e15-280">Ad esempio, per impostare la registrazione di SQL sul livello `Debug`, configurare il livello in modo esplicito in `OnConfiguring` o `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="99e15-280">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="99e15-281">I valori di chiave temporanei non sono più impostati nelle istanze di entità</span><span class="sxs-lookup"><span data-stu-id="99e15-281">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="99e15-282">Problema n. 12378</span><span class="sxs-lookup"><span data-stu-id="99e15-282">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="99e15-283">Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.</span><span class="sxs-lookup"><span data-stu-id="99e15-283">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="99e15-284">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-284">**Old behavior**</span></span>

<span data-ttu-id="99e15-285">Nelle versioni precedenti a EF Core 3.0 i valori temporanei venivano assegnati a tutte le proprietà di chiave per cui veniva in seguito generato un valore reale dal database.</span><span class="sxs-lookup"><span data-stu-id="99e15-285">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="99e15-286">In genere questi valori temporanei erano numeri negativi elevati.</span><span class="sxs-lookup"><span data-stu-id="99e15-286">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="99e15-287">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-287">**New behavior**</span></span>

<span data-ttu-id="99e15-288">A partire dalla versione 3.0, EF Core archivia il valore di chiave temporaneo come parte delle informazioni di rilevamento dell'entità e non modifica la proprietà di chiave.</span><span class="sxs-lookup"><span data-stu-id="99e15-288">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="99e15-289">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-289">**Why**</span></span>

<span data-ttu-id="99e15-290">Questa modifica è stata apportata per impedire che i valori di chiave temporanei diventino erroneamente permanenti quando un'entità rilevata in precedenza da un'istanza `DbContext` viene spostata in un'altra istanza `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="99e15-290">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="99e15-291">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-291">**Mitigations**</span></span>

<span data-ttu-id="99e15-292">Le applicazioni che assegnano valori di chiave primaria in chiavi esterne per creare associazioni tra le entità possono dipendere dal comportamento precedente se le chiavi primarie vengono generate dall'archivio e appartengono a entità con stato `Added`.</span><span class="sxs-lookup"><span data-stu-id="99e15-292">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="99e15-293">Questo può essere evitato:</span><span class="sxs-lookup"><span data-stu-id="99e15-293">This can be avoided by:</span></span>
* <span data-ttu-id="99e15-294">Non usando chiavi generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="99e15-294">Not using store-generated keys.</span></span>
* <span data-ttu-id="99e15-295">Impostando le proprietà di navigazione in modo da creare relazioni anziché impostando valori di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="99e15-295">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="99e15-296">Ottenendo i valori di chiave temporanei effettivi dalle informazioni di rilevamento dell'entità.</span><span class="sxs-lookup"><span data-stu-id="99e15-296">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="99e15-297">Ad esempio, `context.Entry(blog).Property(e => e.Id).CurrentValue` restituisce il valore temporaneo anche quando `blog.Id` non è stato impostato.</span><span class="sxs-lookup"><span data-stu-id="99e15-297">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="99e15-298">DetectChanges rispetta i valori di chiave generati dall'archivio</span><span class="sxs-lookup"><span data-stu-id="99e15-298">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="99e15-299">Problema n. 14616</span><span class="sxs-lookup"><span data-stu-id="99e15-299">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="99e15-300">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="99e15-300">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="99e15-301">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-301">**Old behavior**</span></span>

<span data-ttu-id="99e15-302">Nelle versioni precedenti a EF Core 3.0 un'entità non rilevata individuata da `DetectChanges` veniva rilevata nello stato `Added` e inserita come nuova riga quando veniva eseguita una chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="99e15-302">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="99e15-303">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-303">**New behavior**</span></span>

<span data-ttu-id="99e15-304">A partire da EF Core 3.0, se un'entità usa valori di chiave generati e viene impostato un valore di chiave, l'entità viene rilevata nello stato `Modified`.</span><span class="sxs-lookup"><span data-stu-id="99e15-304">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="99e15-305">Ciò significa che si presuppone l'esistenza di una riga per l'entità che viene aggiornata quando viene eseguita una chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="99e15-305">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="99e15-306">Se il valore di chiave non viene impostato o se il tipo di entità non usa chiavi generate, la nuova entità viene rilevata come `Added` come nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="99e15-306">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="99e15-307">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-307">**Why**</span></span>

<span data-ttu-id="99e15-308">Questa modifica è stata apportata per rendere più semplice e coerente l'uso di grafici di entità disconnesse con chiavi generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="99e15-308">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="99e15-309">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-309">**Mitigations**</span></span>

<span data-ttu-id="99e15-310">Questa modifica può interrompere un'applicazione se un tipo di entità è configurato per l'uso di chiavi generate ma i valori di chiave sono impostati in modo esplicito per le nuove istanze.</span><span class="sxs-lookup"><span data-stu-id="99e15-310">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="99e15-311">La correzione consiste nel configurare in modo esplicito le proprietà di chiave per non usare valori generati.</span><span class="sxs-lookup"><span data-stu-id="99e15-311">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="99e15-312">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="99e15-312">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="99e15-313">Oppure con annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="99e15-313">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="99e15-314">Le eliminazioni a catena vengono ora eseguite immediatamente per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="99e15-314">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="99e15-315">Problema n. 10114</span><span class="sxs-lookup"><span data-stu-id="99e15-315">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="99e15-316">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="99e15-316">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="99e15-317">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-317">**Old behavior**</span></span>

<span data-ttu-id="99e15-318">Nelle versioni precedenti alla versione 3.0, EF Core applicava azioni a catena (eliminazione delle entità dipendenti quando veniva eliminata un'entità di sicurezza obbligatoria o veniva recisa la relazione con un'entità di sicurezza obbligatoria) solo dopo la chiamata a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="99e15-318">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="99e15-319">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-319">**New behavior**</span></span>

<span data-ttu-id="99e15-320">A partire dalla versione 3.0, EF Core applica le azioni a catena non appena viene rilevata la condizione di attivazione.</span><span class="sxs-lookup"><span data-stu-id="99e15-320">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="99e15-321">Ad esempio, la chiamata a `context.Remove()` per eliminare un'entità di sicurezza causa anche l'impostazione immediata di tutti i dipendenti obbligatori correlati rilevati su `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="99e15-321">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="99e15-322">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-322">**Why**</span></span>

<span data-ttu-id="99e15-323">Questa modifica è stata apportata per migliorare l'esperienza di associazione di dati e degli scenari di controllo in cui è importante individuare le entità che verranno eliminate _prima_ della chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="99e15-323">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="99e15-324">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-324">**Mitigations**</span></span>

<span data-ttu-id="99e15-325">Il comportamento precedente può essere ripristinato tramite le impostazioni in `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="99e15-325">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="99e15-326">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="99e15-326">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="99e15-327">Semantica più chiara per DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="99e15-327">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="99e15-328">Problema n. 12661</span><span class="sxs-lookup"><span data-stu-id="99e15-328">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="99e15-329">Questa modifica è stata introdotta in EF Core 3.0 anteprima 5.</span><span class="sxs-lookup"><span data-stu-id="99e15-329">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="99e15-330">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-330">**Old behavior**</span></span>

<span data-ttu-id="99e15-331">Prima della versione 3.0, `DeleteBehavior.Restrict` creava chiavi esterne nel database con la semantica `Restrict`, ma modificava anche la correzione interna in modo non ovvio.</span><span class="sxs-lookup"><span data-stu-id="99e15-331">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="99e15-332">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-332">**New behavior**</span></span>

<span data-ttu-id="99e15-333">A partire dalla versione 3.0, `DeleteBehavior.Restrict` assicura che le chiavi esterne vengano create con la semantica `Restrict`, ovvero non a cascata e con generazione di un'eccezione in caso di violazione di vincolo, senza influire sulla correzione interna di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="99e15-333">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="99e15-334">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-334">**Why**</span></span>

<span data-ttu-id="99e15-335">Questa modifica è stata apportata per migliorare l'esperienza di uso di `DeleteBehavior` in modo intuitivo, senza effetti collaterali imprevisti.</span><span class="sxs-lookup"><span data-stu-id="99e15-335">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="99e15-336">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-336">**Mitigations**</span></span>

<span data-ttu-id="99e15-337">Il comportamento precedente può essere ripristinato tramite `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="99e15-337">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="99e15-338">I tipi di query vengono consolidati con tipi di entità</span><span class="sxs-lookup"><span data-stu-id="99e15-338">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="99e15-339">Problema n. 14194</span><span class="sxs-lookup"><span data-stu-id="99e15-339">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="99e15-340">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="99e15-340">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="99e15-341">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-341">**Old behavior**</span></span>

<span data-ttu-id="99e15-342">Nelle versioni precedenti a EF Core 3.0 i [tipi di query](xref:core/modeling/query-types) erano uno strumento per eseguire query su dati che non definiscono una chiave primaria in modo strutturato.</span><span class="sxs-lookup"><span data-stu-id="99e15-342">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="99e15-343">Veniva infatti usato un tipo di query per eseguire il mapping di tipi di entità senza chiavi (più probabilmente da una vista, ma anche da una tabella), mentre veniva usato un tipo di entità normale quando era disponibile una chiave (più probabilmente da una tabella, ma anche da una vista).</span><span class="sxs-lookup"><span data-stu-id="99e15-343">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="99e15-344">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-344">**New behavior**</span></span>

<span data-ttu-id="99e15-345">Un tipo di query diventa ora semplicemente un tipo di entità senza chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="99e15-345">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="99e15-346">I tipi di entità senza chiave hanno la stessa funzionalità dei tipi di query nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="99e15-346">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="99e15-347">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-347">**Why**</span></span>

<span data-ttu-id="99e15-348">Questa modifica è stata apportata per ridurre la confusione riguardo lo scopo dei tipi di query.</span><span class="sxs-lookup"><span data-stu-id="99e15-348">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="99e15-349">In particolare, si tratta di tipi di entità senza chiave che sono intrinsecamente di sola lettura per questo motivo ma non dovrebbero essere usati solo perché un tipo di entità deve essere di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="99e15-349">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="99e15-350">Analogamente, spesso vengono mappati alle viste solo perché le viste spesso non definiscono le chiavi.</span><span class="sxs-lookup"><span data-stu-id="99e15-350">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="99e15-351">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-351">**Mitigations**</span></span>

<span data-ttu-id="99e15-352">Le parti dell'API seguenti sono ora obsolete:</span><span class="sxs-lookup"><span data-stu-id="99e15-352">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="99e15-353">**`ModelBuilder.Query<>()`** - È necessario chiamare `ModelBuilder.Entity<>().HasNoKey()` per contrassegnare un tipo di entità come tipo senza chiavi.</span><span class="sxs-lookup"><span data-stu-id="99e15-353">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="99e15-354">Non ne viene eseguita la configurazione per convenzione per evitare una configurazione errata quando è prevista una chiave primaria che tuttavia non corrisponde alla convenzione.</span><span class="sxs-lookup"><span data-stu-id="99e15-354">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="99e15-355">**`DbQuery<>`** - Usare `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="99e15-355">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="99e15-356">**`DbContext.Query<>()`** - Usare `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="99e15-356">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="99e15-357">L'API di configurazione per le relazioni di tipo di proprietà è stata modificata</span><span class="sxs-lookup"><span data-stu-id="99e15-357">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="99e15-358">[Problema n. 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema n. 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema n. 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="99e15-358">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="99e15-359">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="99e15-359">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="99e15-360">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-360">**Old behavior**</span></span>

<span data-ttu-id="99e15-361">Nelle versioni precedenti a EF Core 3.0 la configurazione della relazione di proprietà veniva eseguita direttamente dopo la chiamata a `OwnsOne` o `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="99e15-361">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="99e15-362">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-362">**New behavior**</span></span>

<span data-ttu-id="99e15-363">A partire da EF Core 3.0, è disponibile l'API Fluent per configurare una proprietà di navigazione per il proprietario usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="99e15-363">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="99e15-364">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="99e15-364">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="99e15-365">La configurazione correlata alla relazione tra proprietario e elemento di proprietà deve essere ora concatenata dopo `WithOwner()` in modo analogo a come vengono configurate altre relazioni.</span><span class="sxs-lookup"><span data-stu-id="99e15-365">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="99e15-366">La configurazione per il tipo di proprietà viene comunque concatenata dopo `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="99e15-366">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="99e15-367">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="99e15-367">For example:</span></span>

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

<span data-ttu-id="99e15-368">Inoltre, la chiamata a `Entity()`, `HasOne()` o `Set()` con una destinazione di tipo di proprietà genera ora un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="99e15-368">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="99e15-369">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-369">**Why**</span></span>

<span data-ttu-id="99e15-370">Questa modifica è stata apportata per creare una separazione più netta tra la configurazione del tipo di proprietà e la _relazione_ con il tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="99e15-370">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="99e15-371">Ciò consente di eliminare ambiguità e confusione su metodi come `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="99e15-371">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="99e15-372">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-372">**Mitigations**</span></span>

<span data-ttu-id="99e15-373">Modificare la configurazione delle relazioni dei tipi di proprietà per usare la superficie della nuova API come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="99e15-373">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="99e15-374">Le entità dipendenti che condividono la tabella con l'entità di sicurezza sono ora facoltative</span><span class="sxs-lookup"><span data-stu-id="99e15-374">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="99e15-375">Problema n. 9005</span><span class="sxs-lookup"><span data-stu-id="99e15-375">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="99e15-376">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-376">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-377">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-377">**Old behavior**</span></span>

<span data-ttu-id="99e15-378">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="99e15-378">Consider the following model:</span></span>
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
<span data-ttu-id="99e15-379">Prima di EF Core 3.0, se `OrderDetails` è di proprietà di `Order` o viene mappato in modo esplicito alla stessa tabella, era sempre necessaria un'istanza di `OrderDetails` per l'aggiunta di un nuovo `Order`.</span><span class="sxs-lookup"><span data-stu-id="99e15-379">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="99e15-380">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-380">**New behavior**</span></span>

<span data-ttu-id="99e15-381">A partire dalla versione 3.0, EF Core consente di aggiungere un `Order` senza un `OrderDetails` ed esegue il mapping di tutte le proprietà di `OrderDetails`, tranne che la chiave primaria, a colonne che ammettono valori Null.</span><span class="sxs-lookup"><span data-stu-id="99e15-381">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="99e15-382">In fase di query, EF Core imposta `OrderDetails` su `null` se una delle relative proprietà obbligatorie non ha un valore o se non sono presenti proprietà obbligatorie oltre alla chiave primaria e tutte le proprietà sono `null`.</span><span class="sxs-lookup"><span data-stu-id="99e15-382">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="99e15-383">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-383">**Mitigations**</span></span>

<span data-ttu-id="99e15-384">Se il modello include una tabella condivisa dipendente con tutte le colonne facoltative, ma è previsto che la navigazione che punta a essa non sia `null`, l'applicazione deve essere modificata per gestire casi in cui la navigazione è `null`.</span><span class="sxs-lookup"><span data-stu-id="99e15-384">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="99e15-385">Se questo non è possibile, è consigliabile aggiungere una proprietà obbligatoria al tipo di entità o assegnare un valore non `null` ad almeno una proprietà.</span><span class="sxs-lookup"><span data-stu-id="99e15-385">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="99e15-386">Tutte le entità che condividono una tabella con una colonna di token di concorrenza devono eseguirne il mapping a una proprietà</span><span class="sxs-lookup"><span data-stu-id="99e15-386">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="99e15-387">Problema n. 14154</span><span class="sxs-lookup"><span data-stu-id="99e15-387">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="99e15-388">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-388">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-389">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-389">**Old behavior**</span></span>

<span data-ttu-id="99e15-390">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="99e15-390">Consider the following model:</span></span>
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
<span data-ttu-id="99e15-391">Prima di EF Core 3.0, se `OrderDetails` è di proprietà di `Order` o è mappato in modo esplicito alla stessa tabella, il solo aggiornamento di `OrderDetails` non aggiornerà il valore `Version` nel client e l'aggiornamento successivo avrà esito negativo.</span><span class="sxs-lookup"><span data-stu-id="99e15-391">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="99e15-392">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-392">**New behavior**</span></span>

<span data-ttu-id="99e15-393">A partire dalla versione 3.0, EF Core propaga il nuovo valore `Version` a `Order` se è proprietario di `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="99e15-393">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="99e15-394">In caso contrario, viene generata un'eccezione durante la convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="99e15-394">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="99e15-395">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-395">**Why**</span></span>

<span data-ttu-id="99e15-396">Questa modifica è stata apportata per evitare un valore del token di concorrenza non aggiornato quando viene aggiornata solo una delle entità mappate alla stessa tabella.</span><span class="sxs-lookup"><span data-stu-id="99e15-396">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="99e15-397">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-397">**Mitigations**</span></span>

<span data-ttu-id="99e15-398">Tutte le entità che condividono la tabella devono includere una proprietà mappata alla colonna del token di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="99e15-398">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="99e15-399">È possibile crearne una in stato shadow:</span><span class="sxs-lookup"><span data-stu-id="99e15-399">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="99e15-400">Per le proprietà ereditate da tipi senza mapping viene ora eseguito il mapping a una singola colonna per tutti i tipi derivati</span><span class="sxs-lookup"><span data-stu-id="99e15-400">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="99e15-401">Problema n. 13998</span><span class="sxs-lookup"><span data-stu-id="99e15-401">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="99e15-402">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-402">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-403">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-403">**Old behavior**</span></span>

<span data-ttu-id="99e15-404">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="99e15-404">Consider the following model:</span></span>
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

<span data-ttu-id="99e15-405">Prima di EF Core 3.0, per la proprietà `ShippingAddress` sarebbe stato eseguito il mapping a colonne separate per `BulkOrder` e `Order` per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="99e15-405">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="99e15-406">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-406">**New behavior**</span></span>

<span data-ttu-id="99e15-407">A partire dalla versione 3.0, EF Core crea solo una colonna per `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="99e15-407">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="99e15-408">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-408">**Why**</span></span>

<span data-ttu-id="99e15-409">Il comportamento precedente non era previsto.</span><span class="sxs-lookup"><span data-stu-id="99e15-409">The old behavoir was unexpected.</span></span>

<span data-ttu-id="99e15-410">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-410">**Mitigations**</span></span>

<span data-ttu-id="99e15-411">È ancora possibile eseguire il mapping esplicito della proprietà a una colonna separata per i tipi derivati:</span><span class="sxs-lookup"><span data-stu-id="99e15-411">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="99e15-412">La convenzione di proprietà di chiave esterna non ha più lo stesso nome della proprietà dell'entità di sicurezza</span><span class="sxs-lookup"><span data-stu-id="99e15-412">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="99e15-413">Problema n. 13274</span><span class="sxs-lookup"><span data-stu-id="99e15-413">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="99e15-414">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="99e15-414">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="99e15-415">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-415">**Old behavior**</span></span>

<span data-ttu-id="99e15-416">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="99e15-416">Consider the following model:</span></span>
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
<span data-ttu-id="99e15-417">Nelle versioni precedenti a EF Core 3.0 veniva usata la proprietà `CustomerId` per la chiave esterna per convenzione.</span><span class="sxs-lookup"><span data-stu-id="99e15-417">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="99e15-418">Tuttavia, se `Order` è un tipo di proprietà, `CustomerId` sarebbe la chiave primaria e ciò non è in genere auspicabile.</span><span class="sxs-lookup"><span data-stu-id="99e15-418">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="99e15-419">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-419">**New behavior**</span></span>

<span data-ttu-id="99e15-420">A partire da 3.0, EF Core non tenta di usare le proprietà per le chiavi esterne per convenzione se hanno lo stesso nome della proprietà dell'entità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="99e15-420">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="99e15-421">Viene ancora eseguita la corrispondenza tra i criteri del nome del tipo dell'entità di sicurezza concatenato al nome della proprietà dell'entità di sicurezza e il nome di navigazione concatenato al nome della proprietà dell'entità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="99e15-421">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="99e15-422">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="99e15-422">For example:</span></span>

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

<span data-ttu-id="99e15-423">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-423">**Why**</span></span>

<span data-ttu-id="99e15-424">Questa modifica è stata apportata per evitare una definizione errata della proprietà di chiave primaria nel tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="99e15-424">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="99e15-425">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-425">**Mitigations**</span></span>

<span data-ttu-id="99e15-426">Se la proprietà è stata progettata per essere la chiave esterna e di conseguenza parte della chiave primaria, configurarla in modo esplicito come chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="99e15-426">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="99e15-427">La connessione al database viene ora chiusa se non viene più usata prima del completamento di TransactionScope</span><span class="sxs-lookup"><span data-stu-id="99e15-427">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="99e15-428">Problema n. 14218</span><span class="sxs-lookup"><span data-stu-id="99e15-428">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="99e15-429">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-429">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-430">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-430">**Old behavior**</span></span>

<span data-ttu-id="99e15-431">Prima di EF Core 3.0, se il contesto apre la connessione all'interno di un `TransactionScope`, la connessione rimane aperta mentre è attivo il `TransactionScope` corrente.</span><span class="sxs-lookup"><span data-stu-id="99e15-431">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="99e15-432">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-432">**New behavior**</span></span>

<span data-ttu-id="99e15-433">A partire dalla versione 3.0, EF Core chiude la connessione non appena non viene più usata.</span><span class="sxs-lookup"><span data-stu-id="99e15-433">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="99e15-434">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-434">**Why**</span></span>

<span data-ttu-id="99e15-435">Questa modifica consente di usare più contesti nello stesso `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="99e15-435">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="99e15-436">Il nuovo comportamento corrisponde anche a EF6.</span><span class="sxs-lookup"><span data-stu-id="99e15-436">The new behavior also matches EF6.</span></span>

<span data-ttu-id="99e15-437">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-437">**Mitigations**</span></span>

<span data-ttu-id="99e15-438">Se la connessione deve rimanere aperta, la chiamata esplicita di `OpenConnection()` garantirà che EF Core non la chiuda prematuramente:</span><span class="sxs-lookup"><span data-stu-id="99e15-438">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="99e15-439">Ogni proprietà usa la generazione di chiavi di tipo intero in memoria indipendenti</span><span class="sxs-lookup"><span data-stu-id="99e15-439">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="99e15-440">Problema n. 6872</span><span class="sxs-lookup"><span data-stu-id="99e15-440">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="99e15-441">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-441">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-442">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-442">**Old behavior**</span></span>

<span data-ttu-id="99e15-443">Nelle versioni precedenti a EF Core 3.0 veniva usato un unico generatore di valori condiviso per tutte le proprietà di chiavi di tipo intero in memoria.</span><span class="sxs-lookup"><span data-stu-id="99e15-443">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="99e15-444">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-444">**New behavior**</span></span>

<span data-ttu-id="99e15-445">A partire da EF Core 3.0, ogni proprietà di chiave di tipo intero riceve un generatore di valori quando viene usato il database in memoria.</span><span class="sxs-lookup"><span data-stu-id="99e15-445">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="99e15-446">Inoltre, se il database viene eliminato, la generazione di chiavi viene reimpostata per tutte le tabelle.</span><span class="sxs-lookup"><span data-stu-id="99e15-446">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="99e15-447">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-447">**Why**</span></span>

<span data-ttu-id="99e15-448">Questa modifica è stata apportata per allineare maggiormente la generazione di chiavi in memoria alla generazione di chiavi del database reale e per migliorare la possibilità di isolare i test l'uno dall'altro quando viene usato il database in memoria.</span><span class="sxs-lookup"><span data-stu-id="99e15-448">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="99e15-449">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-449">**Mitigations**</span></span>

<span data-ttu-id="99e15-450">Ciò può interrompere un'applicazione che si basa sull'impostazione di valori di chiave in memoria specifici.</span><span class="sxs-lookup"><span data-stu-id="99e15-450">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="99e15-451">È consigliabile non basare l'applicazione su valori di chiave specifici o eseguire l'aggiornamento per passare al nuovo comportamento.</span><span class="sxs-lookup"><span data-stu-id="99e15-451">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="99e15-452">I campi sottostanti vengono usati per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="99e15-452">Backing fields are used by default</span></span>

[<span data-ttu-id="99e15-453">Problema n. 12430</span><span class="sxs-lookup"><span data-stu-id="99e15-453">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="99e15-454">Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.</span><span class="sxs-lookup"><span data-stu-id="99e15-454">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="99e15-455">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-455">**Old behavior**</span></span>

<span data-ttu-id="99e15-456">Nelle versioni precedenti alla versione 3.0, anche se il campo sottostante di una proprietà era noto, per impostazione predefinita EF Core eseguiva la lettura e la scrittura del valore della proprietà usando i metodi getter e setter della proprietà.</span><span class="sxs-lookup"><span data-stu-id="99e15-456">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="99e15-457">L'eccezione era costituita dall'esecuzione di query in cui il campo sottostante, se noto, veniva impostato direttamente.</span><span class="sxs-lookup"><span data-stu-id="99e15-457">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="99e15-458">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-458">**New behavior**</span></span>

<span data-ttu-id="99e15-459">A partire da EF Core 3.0, se il campo sottostante di una proprietà è noto, la lettura e la scrittura della proprietà vengono sempre eseguite usando il campo sottostante.</span><span class="sxs-lookup"><span data-stu-id="99e15-459">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="99e15-460">Ciò potrebbe causare un'interruzione dell'applicazione se l'applicazione si basa su un comportamento aggiuntivo codificato nei metodi getter o setter.</span><span class="sxs-lookup"><span data-stu-id="99e15-460">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="99e15-461">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-461">**Why**</span></span>

<span data-ttu-id="99e15-462">Questa modifica è stata apportata per impedire a EF Core di attivare per errore la logica di business per impostazione predefinita quando si eseguono operazioni di database che interessano le entità.</span><span class="sxs-lookup"><span data-stu-id="99e15-462">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="99e15-463">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-463">**Mitigations**</span></span>

<span data-ttu-id="99e15-464">È possibile ripristinare il comportamento delle versioni precedenti alla versione 3.0 tramite la configurazione della modalità di accesso delle proprietà in `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="99e15-464">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="99e15-465">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="99e15-465">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="99e15-466">Viene generata un'eccezione se vengono trovati più campi sottostanti compatibili</span><span class="sxs-lookup"><span data-stu-id="99e15-466">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="99e15-467">Problema n. 12523</span><span class="sxs-lookup"><span data-stu-id="99e15-467">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="99e15-468">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-468">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-469">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-469">**Old behavior**</span></span>

<span data-ttu-id="99e15-470">Nelle versioni precedenti a EF Core 3.0, se più campi soddisfacevano le regole di ricerca del campo sottostante di una proprietà, veniva selezionato un solo campo in base a un ordine di precedenza.</span><span class="sxs-lookup"><span data-stu-id="99e15-470">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="99e15-471">Ciò poteva causare l'uso di un campo non corretto nei casi ambigui.</span><span class="sxs-lookup"><span data-stu-id="99e15-471">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="99e15-472">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-472">**New behavior**</span></span>

<span data-ttu-id="99e15-473">A partire da EF Core 3.0, se più campi corrispondono alla stessa proprietà, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="99e15-473">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="99e15-474">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-474">**Why**</span></span>

<span data-ttu-id="99e15-475">Questa modifica è stata apportata per evitare di usare automaticamente un campo rispetto a un altro quando un solo campo può essere quello corretto.</span><span class="sxs-lookup"><span data-stu-id="99e15-475">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="99e15-476">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-476">**Mitigations**</span></span>

<span data-ttu-id="99e15-477">Per le proprietà con campi sottostanti ambigui, il campo da usare deve essere specificato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="99e15-477">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="99e15-478">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="99e15-478">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="99e15-479">I nomi delle proprietà solo campo devono corrispondere al nome di campo</span><span class="sxs-lookup"><span data-stu-id="99e15-479">Field-only property names should match the field name</span></span>

<span data-ttu-id="99e15-480">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-480">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-481">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-481">**Old behavior**</span></span>

<span data-ttu-id="99e15-482">Prima di EF Core 3.0, era possibile specificare una proprietà con un valore stringa e se non veniva trovata alcuna proprietà con tale nome nel tipo CLR, EF Core tentava di trovare una corrispondenza con un campo tramite regole di convenzione.</span><span class="sxs-lookup"><span data-stu-id="99e15-482">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="99e15-483">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-483">**New behavior**</span></span>

<span data-ttu-id="99e15-484">A partire da EF Core 3.0, una proprietà solo campo deve corrispondere esattamente al nome del campo.</span><span class="sxs-lookup"><span data-stu-id="99e15-484">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="99e15-485">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-485">**Why**</span></span>

<span data-ttu-id="99e15-486">Questa modifica è stata introdotta per evitare di usare lo stesso campo per due proprietà con nome simile. Le regole di corrispondenza per le proprietà solo campo sono state anche uniformate a quelle per le proprietà mappate a proprietà CLR.</span><span class="sxs-lookup"><span data-stu-id="99e15-486">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="99e15-487">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-487">**Mitigations**</span></span>

<span data-ttu-id="99e15-488">Le proprietà solo campo devono avere lo stesso nome del campo a cui vengono mappate.</span><span class="sxs-lookup"><span data-stu-id="99e15-488">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="99e15-489">In un'anteprima successiva di EF Core 3.0 è prevista la riabilitazione della possibilità di configurare in modo esplicito un nome di campo diverso dal nome della proprietà:</span><span class="sxs-lookup"><span data-stu-id="99e15-489">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="99e15-490">AddDbContext/AddDbContextPool non chiamano più AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="99e15-490">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="99e15-491">Problema n. 14756</span><span class="sxs-lookup"><span data-stu-id="99e15-491">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="99e15-492">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-492">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-493">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-493">**Old behavior**</span></span>

<span data-ttu-id="99e15-494">Prima di EF Core 3.0, la chiamata di `AddDbContext` oppure `AddDbContextPool` comporta anche la registrazione dei servizi di registrazione e memorizzazione nella cache con inserimento delle dipendenze tramite chiamate a [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="99e15-494">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="99e15-495">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-495">**New behavior**</span></span>

<span data-ttu-id="99e15-496">A partire da EF Core 3.0, `AddDbContext` e `AddDbContextPool` non registreranno più questi servizi con inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="99e15-496">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="99e15-497">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-497">**Why**</span></span>

<span data-ttu-id="99e15-498">EF Core 3.0 non richiede che questi servizi siano inclusi nel contenitore di inserimento delle dipendenze dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="99e15-498">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="99e15-499">Tuttavia, se `ILoggerFactory` è registrato nel contenitore di inserimento delle dipendenze dell'applicazione, verrà ancora usato da EF Core.</span><span class="sxs-lookup"><span data-stu-id="99e15-499">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="99e15-500">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-500">**Mitigations**</span></span>

<span data-ttu-id="99e15-501">Se l'applicazione necessita di questi servizi, registrarli in modo esplicito con il contenitore di inserimento delle dipendenze usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="99e15-501">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="99e15-502">DbContext.Entry esegue ora un DetectChanges locale</span><span class="sxs-lookup"><span data-stu-id="99e15-502">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="99e15-503">Problema n. 13552</span><span class="sxs-lookup"><span data-stu-id="99e15-503">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="99e15-504">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="99e15-504">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="99e15-505">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-505">**Old behavior**</span></span>

<span data-ttu-id="99e15-506">Nelle versioni precedenti a EF Core 3.0 la chiamata a `DbContext.Entry` causava il rilevamento delle modifiche per tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="99e15-506">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="99e15-507">Ciò garantiva l'aggiornamento dello stato esposto in `EntityEntry`.</span><span class="sxs-lookup"><span data-stu-id="99e15-507">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="99e15-508">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-508">**New behavior**</span></span>

<span data-ttu-id="99e15-509">A partire da EF Core 3.0, la chiamata a `DbContext.Entry` causa ora solo il tentativo di rilevare le modifiche nell'entità specificata e in tutte le relative entità di sicurezza rilevate.</span><span class="sxs-lookup"><span data-stu-id="99e15-509">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="99e15-510">Ciò significa che le modifiche apportate altrove potrebbero non essere state rilevate tramite la chiamata al metodo e ciò potrebbe avere implicazioni sullo stato dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="99e15-510">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="99e15-511">Si noti che se `ChangeTracker.AutoDetectChangesEnabled` è impostato su `false`, verrà disabilitato anche questo tipo di rilevamento delle modifiche locali.</span><span class="sxs-lookup"><span data-stu-id="99e15-511">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="99e15-512">Gli altri metodi che causano il rilevamento delle modifiche, ad esempio `ChangeTracker.Entries` e `SaveChanges`, causano ancora un `DetectChanges` completo di tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="99e15-512">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="99e15-513">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-513">**Why**</span></span>

<span data-ttu-id="99e15-514">Questa modifica è stata apportata per migliorare le prestazioni predefinite dell'uso di `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="99e15-514">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="99e15-515">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-515">**Mitigations**</span></span>

<span data-ttu-id="99e15-516">Chiamare `ChgangeTracker.DetectChanges()` in modo esplicito prima di chiamare `Entry` per garantire il comportamento precedente alla versione 3.0.</span><span class="sxs-lookup"><span data-stu-id="99e15-516">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="99e15-517">Le chiavi matrice di byte e di stringhe non vengono generate dal client per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="99e15-517">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="99e15-518">Problema n. 14617</span><span class="sxs-lookup"><span data-stu-id="99e15-518">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="99e15-519">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-519">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-520">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-520">**Old behavior**</span></span>

<span data-ttu-id="99e15-521">Nelle versioni precedenti a EF Core 3.0 le proprietà di chiave `string` e `byte[]` potevano essere usate senza impostare in modo esplicito un valore non Null.</span><span class="sxs-lookup"><span data-stu-id="99e15-521">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="99e15-522">In questi casi, il valore di chiave veniva generato nel client come GUID, serializzato in byte per `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="99e15-522">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="99e15-523">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-523">**New behavior**</span></span>

<span data-ttu-id="99e15-524">A partire da EF Core 3.0 viene generata un'eccezione che indica che non è stato impostato alcun valore di chiave.</span><span class="sxs-lookup"><span data-stu-id="99e15-524">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="99e15-525">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-525">**Why**</span></span>

<span data-ttu-id="99e15-526">Questa modifica è stata apportata poiché i valori `string`/`byte[]` generati dal client non risultano in genere utili e il comportamento predefinito rendeva complesso ragionare sui valori di chiave generati in un modo comune.</span><span class="sxs-lookup"><span data-stu-id="99e15-526">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="99e15-527">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-527">**Mitigations**</span></span>

<span data-ttu-id="99e15-528">È possibile ripristinare il comportamento precedente alla versione 3.0 specificando in modo esplicito che le proprietà di chiave devono usare i valori generati se non viene impostato alcun altro valore non Null.</span><span class="sxs-lookup"><span data-stu-id="99e15-528">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="99e15-529">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="99e15-529">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="99e15-530">Oppure con annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="99e15-530">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="99e15-531">ILoggerFactory è ora un servizio con ambito</span><span class="sxs-lookup"><span data-stu-id="99e15-531">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="99e15-532">Problema n. 14698</span><span class="sxs-lookup"><span data-stu-id="99e15-532">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="99e15-533">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="99e15-533">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="99e15-534">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-534">**Old behavior**</span></span>

<span data-ttu-id="99e15-535">Nelle versioni precedenti a EF Core 3.0 `ILoggerFactory` veniva registrato come servizio singleton.</span><span class="sxs-lookup"><span data-stu-id="99e15-535">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="99e15-536">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-536">**New behavior**</span></span>

<span data-ttu-id="99e15-537">A partire da EF Core 3.0, `ILoggerFactory` viene registrato come servizio con ambito.</span><span class="sxs-lookup"><span data-stu-id="99e15-537">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="99e15-538">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-538">**Why**</span></span>

<span data-ttu-id="99e15-539">Questa modifica è stata apportata per consentire l'associazione di un logger a un'istanza `DbContext` che abilita altre funzionalità e rimuove alcuni casi di comportamento anomalo, ad esempio un'esplosione dei provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="99e15-539">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="99e15-540">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-540">**Mitigations**</span></span>

<span data-ttu-id="99e15-541">Questa modifica non dovrebbe influire sul codice dell'applicazione a meno che non vengano registrati e usati servizi personalizzati nel provider di servizi interno di EF Core.</span><span class="sxs-lookup"><span data-stu-id="99e15-541">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="99e15-542">Questa condizione, tuttavia, non è comune.</span><span class="sxs-lookup"><span data-stu-id="99e15-542">This isn't common.</span></span>
<span data-ttu-id="99e15-543">In questi casi la maggior parte delle operazioni continuano a essere eseguite correttamente, ma qualsiasi servizio singleton dipendente da `ILoggerFactory` dovrà essere modificato per ottenere `ILoggerFactory` in modo diverso.</span><span class="sxs-lookup"><span data-stu-id="99e15-543">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="99e15-544">Se si verificano situazioni simili, inviare una segnalazione nello [strumento di gestione dei problemi in GitHub relativo a EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) per comunicare in che modo viene usato `ILoggerFactory` per consentirci di comprendere meglio come evitare ulteriori interruzioni in futuro.</span><span class="sxs-lookup"><span data-stu-id="99e15-544">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="99e15-545">I proxy di caricamento lazy non presuppongono più che le proprietà di navigazione vengano caricate completamente</span><span class="sxs-lookup"><span data-stu-id="99e15-545">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="99e15-546">Problema n. 12780</span><span class="sxs-lookup"><span data-stu-id="99e15-546">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="99e15-547">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-547">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-548">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-548">**Old behavior**</span></span>

<span data-ttu-id="99e15-549">Nelle versioni precedenti a EF Core 3.0, quando `DbContext` veniva eliminato non esisteva alcun metodo per scoprire se una determinata proprietà di navigazione in un'entità ottenuta da un contesto specifico veniva caricata completamente o meno.</span><span class="sxs-lookup"><span data-stu-id="99e15-549">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="99e15-550">I proxy presupponevano invece che venisse caricata una navigazione di riferimento se era presente un valore non Null e che venisse caricata una navigazione di raccolta se era presente un valore.</span><span class="sxs-lookup"><span data-stu-id="99e15-550">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="99e15-551">In questi casi il tentativo di eseguire un caricamento lazy non avrebbe avuto alcun esito.</span><span class="sxs-lookup"><span data-stu-id="99e15-551">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="99e15-552">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-552">**New behavior**</span></span>

<span data-ttu-id="99e15-553">A partire da Entity Framework Core 3.0, i proxy tengono traccia del caricamento o mancato caricamento di una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="99e15-553">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="99e15-554">Ciò significa che il tentativo di accedere a una proprietà di navigazione caricata dopo l'eliminazione del contesto non avrà mai alcun esito, anche quando la navigazione caricata è vuota o ha valore Null.</span><span class="sxs-lookup"><span data-stu-id="99e15-554">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="99e15-555">Al contrario, il tentativo di accedere a una proprietà di navigazione non caricata genera un'eccezione se il contesto viene eliminato anche se la proprietà di navigazione è una raccolta non vuota.</span><span class="sxs-lookup"><span data-stu-id="99e15-555">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="99e15-556">Se si verifica questa situazione significa che il codice dell'applicazione sta tentando di usare il caricamento lazy in un momento non valido e l'applicazione deve essere modificata in modo da non eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="99e15-556">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="99e15-557">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-557">**Why**</span></span>

<span data-ttu-id="99e15-558">Questa modifica è stata apportata per rendere coerente e corretto il comportamento durante un tentativo di caricamento lazy in un'istanza `DbContext` eliminata.</span><span class="sxs-lookup"><span data-stu-id="99e15-558">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="99e15-559">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-559">**Mitigations**</span></span>

<span data-ttu-id="99e15-560">Aggiornare il codice dell'applicazione per fare in modo che non venga tentato il caricamento lazy con un contesto eliminato oppure specificare una configurazione in modo che non venga eseguita alcuna operazione come descritto nel messaggio di eccezione.</span><span class="sxs-lookup"><span data-stu-id="99e15-560">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="99e15-561">La creazione di un numero eccessivo di provider di servizi interni è ora un errore per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="99e15-561">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="99e15-562">Problema n. 10236</span><span class="sxs-lookup"><span data-stu-id="99e15-562">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="99e15-563">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="99e15-563">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="99e15-564">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-564">**Old behavior**</span></span>

<span data-ttu-id="99e15-565">Nelle versioni precedenti a EF Core 3.0 veniva registrato un avviso per le applicazioni che creavano un numero eccessivo di provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="99e15-565">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="99e15-566">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-566">**New behavior**</span></span>

<span data-ttu-id="99e15-567">A partire da EF Core 3.0, l'avviso viene considerato un errore e viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="99e15-567">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="99e15-568">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-568">**Why**</span></span>

<span data-ttu-id="99e15-569">Questa modifica è stata apportata per gestire meglio il codice dell'applicazione tramite un'esposizione più esplicita di questa situazione di errore.</span><span class="sxs-lookup"><span data-stu-id="99e15-569">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="99e15-570">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-570">**Mitigations**</span></span>

<span data-ttu-id="99e15-571">L'azione più appropriata quando si verifica questo errore consiste nell'individuare la causa radice e nell'interrompere la creazione di numerosi provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="99e15-571">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="99e15-572">È possibile tuttavia convertire nuovamente l'errore in avviso o ignorarlo tramite una configurazione in `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="99e15-572">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="99e15-573">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="99e15-573">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="99e15-574">Nuovo comportamento per la chiamata di HasOne/HasMany con una singola stringa</span><span class="sxs-lookup"><span data-stu-id="99e15-574">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="99e15-575">Problema n. 9171</span><span class="sxs-lookup"><span data-stu-id="99e15-575">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="99e15-576">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-576">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-577">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-577">**Old behavior**</span></span>

<span data-ttu-id="99e15-578">Prima di EF Core 3.0, il codice che chiama `HasOne` o `HasMany` con una singola stringa era interpretato in modo poco chiaro.</span><span class="sxs-lookup"><span data-stu-id="99e15-578">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="99e15-579">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="99e15-579">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="99e15-580">Apparentemente, il codice mette in relazione `Samurai` con un altro tipo di entità tramite la proprietà di navigazione `Entrance`, che può essere privata.</span><span class="sxs-lookup"><span data-stu-id="99e15-580">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="99e15-581">In realtà, il codice tenta di creare una relazione con un tipo di entità denominato `Entrance` senza proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="99e15-581">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="99e15-582">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-582">**New behavior**</span></span>

<span data-ttu-id="99e15-583">A partire da EF Core 3.0, il codice sopra riportato ora esegue quello che avrebbe dovuto fare in precedenza.</span><span class="sxs-lookup"><span data-stu-id="99e15-583">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="99e15-584">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-584">**Why**</span></span>

<span data-ttu-id="99e15-585">Il comportamento precedente era molto poco chiaro, soprattutto durante la lettura del codice di configurazione e la ricerca di errori.</span><span class="sxs-lookup"><span data-stu-id="99e15-585">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="99e15-586">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-586">**Mitigations**</span></span>

<span data-ttu-id="99e15-587">Questa modifica causerà problemi solo nelle applicazioni che configurano relazioni in modo esplicito usando stringhe per i nomi dei tipi e senza specificare in modo esplicito la proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="99e15-587">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="99e15-588">Non è uno scenario comune.</span><span class="sxs-lookup"><span data-stu-id="99e15-588">This is not common.</span></span>
<span data-ttu-id="99e15-589">Il comportamento precedente può essere ottenuto passando esplicitamente `null` per il nome della proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="99e15-589">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="99e15-590">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="99e15-590">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="99e15-591">Il tipo restituito per diversi metodi asincroni è cambiato da Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="99e15-591">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="99e15-592">Problema n. 15184</span><span class="sxs-lookup"><span data-stu-id="99e15-592">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="99e15-593">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-593">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-594">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-594">**Old behavior**</span></span>

<span data-ttu-id="99e15-595">I metodi asincroni seguenti in precedenza restituivano il tipo `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="99e15-595">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="99e15-596">`ValueGenerator.NextValueAsync()` (e classi derivate)</span><span class="sxs-lookup"><span data-stu-id="99e15-596">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="99e15-597">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-597">**New behavior**</span></span>

<span data-ttu-id="99e15-598">I metodi indicati in precedenza ora restituiscono il tipo `ValueTask<T>` sullo stesso `T` come in precedenza.</span><span class="sxs-lookup"><span data-stu-id="99e15-598">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="99e15-599">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-599">**Why**</span></span>

<span data-ttu-id="99e15-600">Questa modifica riduce il numero delle allocazioni di heap sostenute quando si richiamano questi metodi, con un miglioramento generale delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="99e15-600">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="99e15-601">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-601">**Mitigations**</span></span>

<span data-ttu-id="99e15-602">Le applicazioni semplicemente in attesa delle API precedenti devono solo essere ricompilate e non sono richieste modifiche del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="99e15-602">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="99e15-603">Per scenari di utilizzo più complessi (ad esempio, il passaggio del tipo `Task` restituito a `Task.WhenAny()`) è richiesto in genere che il tipo `ValueTask<T>` restituito venga convertito in `Task<T>` chiamando `AsTask()` su di esso.</span><span class="sxs-lookup"><span data-stu-id="99e15-603">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="99e15-604">Si noti che in questo modo si annulla la riduzione delle allocazioni consentita da questa modifica.</span><span class="sxs-lookup"><span data-stu-id="99e15-604">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="99e15-605">L'annotazione Relational:TypeMapping è ora TypeMapping</span><span class="sxs-lookup"><span data-stu-id="99e15-605">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="99e15-606">Problema n. 9913</span><span class="sxs-lookup"><span data-stu-id="99e15-606">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="99e15-607">Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.</span><span class="sxs-lookup"><span data-stu-id="99e15-607">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="99e15-608">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-608">**Old behavior**</span></span>

<span data-ttu-id="99e15-609">Il nome di annotazione delle annotazioni di mapping del tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="99e15-609">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="99e15-610">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-610">**New behavior**</span></span>

<span data-ttu-id="99e15-611">Il nome di annotazione delle annotazioni di mapping del tipo è ora "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="99e15-611">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="99e15-612">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-612">**Why**</span></span>

<span data-ttu-id="99e15-613">Il mapping dei tipi non viene più usato solo per i provider di database relazionali.</span><span class="sxs-lookup"><span data-stu-id="99e15-613">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="99e15-614">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-614">**Mitigations**</span></span>

<span data-ttu-id="99e15-615">Ciò causa un'interruzione solo nelle applicazioni che accedono al mapping dei tipi direttamente come annotazione. Questa situazione non è comune.</span><span class="sxs-lookup"><span data-stu-id="99e15-615">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="99e15-616">L'azione più appropriata per risolvere il problema consiste nell'usare la superficie dell'API per accedere al mapping dei tipi anziché l'annotazione.</span><span class="sxs-lookup"><span data-stu-id="99e15-616">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="99e15-617">ToTable in un tipo derivato genera un'eccezione</span><span class="sxs-lookup"><span data-stu-id="99e15-617">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="99e15-618">Problema n. 11811</span><span class="sxs-lookup"><span data-stu-id="99e15-618">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="99e15-619">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="99e15-619">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="99e15-620">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-620">**Old behavior**</span></span>

<span data-ttu-id="99e15-621">Nelle versioni precedenti a EF Core 3.0, la chiamata a `ToTable()` in un tipo derivato veniva ignorata poiché soltanto la strategia di mapping dell'ereditarietà era una tabella per gerarchia dove la chiamata non era valida.</span><span class="sxs-lookup"><span data-stu-id="99e15-621">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="99e15-622">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-622">**New behavior**</span></span>

<span data-ttu-id="99e15-623">A partire da EF Core 3.0 e in preparazione all'aggiunta del supporto per la tabella per tipo e per TPC in una versione successiva, la chiamata a `ToTable()` in un tipo derivato genera un'eccezione per evitare una modifica del mapping imprevista in futuro.</span><span class="sxs-lookup"><span data-stu-id="99e15-623">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="99e15-624">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-624">**Why**</span></span>

<span data-ttu-id="99e15-625">Attualmente il mapping di un tipo derivato in una tabella diversa non è un'operazione valida.</span><span class="sxs-lookup"><span data-stu-id="99e15-625">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="99e15-626">Questa modifica consente di evitare interruzioni future quando l'operazione diventerà un'operazione valida.</span><span class="sxs-lookup"><span data-stu-id="99e15-626">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="99e15-627">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-627">**Mitigations**</span></span>

<span data-ttu-id="99e15-628">Rimuovere qualsiasi tentativo di mapping di tipi derivati in altre tabelle.</span><span class="sxs-lookup"><span data-stu-id="99e15-628">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="99e15-629">ForSqlServerHasIndex sostituito con HasIndex</span><span class="sxs-lookup"><span data-stu-id="99e15-629">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="99e15-630">Problema n. 12366</span><span class="sxs-lookup"><span data-stu-id="99e15-630">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="99e15-631">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="99e15-631">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="99e15-632">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-632">**Old behavior**</span></span>

<span data-ttu-id="99e15-633">Nelle versioni precedenti a EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offriva un metodo per configurare le colonne usate con `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="99e15-633">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="99e15-634">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-634">**New behavior**</span></span>

<span data-ttu-id="99e15-635">A partire da EF Core 3.0, l'uso di `Include` in un indice è supportato a livello relazionale.</span><span class="sxs-lookup"><span data-stu-id="99e15-635">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="99e15-636">Usare `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="99e15-636">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="99e15-637">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-637">**Why**</span></span>

<span data-ttu-id="99e15-638">Questa modifica è stata apportata per consolidare l'API per gli indici con `Include` in un'unica posizione per tutti i provider di database.</span><span class="sxs-lookup"><span data-stu-id="99e15-638">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="99e15-639">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-639">**Mitigations**</span></span>

<span data-ttu-id="99e15-640">Usare la nuova API, come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="99e15-640">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="99e15-641">Modifiche dell'API dei metadati</span><span class="sxs-lookup"><span data-stu-id="99e15-641">Metadata API changes</span></span>

[<span data-ttu-id="99e15-642">Problema n. 214</span><span class="sxs-lookup"><span data-stu-id="99e15-642">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="99e15-643">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-643">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-644">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-644">**New behavior**</span></span>

<span data-ttu-id="99e15-645">Le proprietà seguenti sono state convertite in metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="99e15-645">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="99e15-646">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-646">**Why**</span></span>

<span data-ttu-id="99e15-647">Questa modifica semplifica l'implementazione delle interfacce menzionate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="99e15-647">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="99e15-648">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-648">**Mitigations**</span></span>

<span data-ttu-id="99e15-649">Usare i nuovi metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="99e15-649">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="99e15-650">Modifiche dell'API dei metadati specifiche del provider</span><span class="sxs-lookup"><span data-stu-id="99e15-650">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="99e15-651">Problema n. 214</span><span class="sxs-lookup"><span data-stu-id="99e15-651">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="99e15-652">Questa modifica è stata introdotta in EF Core 3.0 anteprima 6.</span><span class="sxs-lookup"><span data-stu-id="99e15-652">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="99e15-653">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-653">**New behavior**</span></span>

<span data-ttu-id="99e15-654">I metodi di estensione specifici del provider verranno resi flat:</span><span class="sxs-lookup"><span data-stu-id="99e15-654">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="99e15-655">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-655">**Why**</span></span>

<span data-ttu-id="99e15-656">Questa modifica semplifica l'implementazione dei metodi di estensione menzionati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="99e15-656">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="99e15-657">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-657">**Mitigations**</span></span>

<span data-ttu-id="99e15-658">Usare i nuovi metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="99e15-658">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="99e15-659">EF Core non invia più pragma per l'imposizione della chiave esterna di SQLite</span><span class="sxs-lookup"><span data-stu-id="99e15-659">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="99e15-660">Problema n. 12151</span><span class="sxs-lookup"><span data-stu-id="99e15-660">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="99e15-661">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="99e15-661">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="99e15-662">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-662">**Old behavior**</span></span>

<span data-ttu-id="99e15-663">Nelle versioni precedenti a EF Core 3.0, EF Core inviava `PRAGMA foreign_keys = 1` quando veniva aperta una connessione a SQLite.</span><span class="sxs-lookup"><span data-stu-id="99e15-663">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="99e15-664">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-664">**New behavior**</span></span>

<span data-ttu-id="99e15-665">A partire da EF Core 3.0, EF Core non invia più `PRAGMA foreign_keys = 1` quando viene aperta una connessione a SQLite.</span><span class="sxs-lookup"><span data-stu-id="99e15-665">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="99e15-666">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-666">**Why**</span></span>

<span data-ttu-id="99e15-667">Questa modifica è stata apportata poiché EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` per impostazione predefinita. Ciò significa che l'imposizione della chiave esterna è abilitata per impostazione predefinita e non deve essere abilitata in modo esplicito ogni volta che viene aperta una connessione.</span><span class="sxs-lookup"><span data-stu-id="99e15-667">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="99e15-668">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-668">**Mitigations**</span></span>

<span data-ttu-id="99e15-669">Le chiavi esterne sono abilitate per impostazione predefinita in SQLitePCLRaw.bundle_e_sqlite3, usato per impostazione predefinita per EF Core.</span><span class="sxs-lookup"><span data-stu-id="99e15-669">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="99e15-670">Per gli altri casi, è possibile abilitare le chiavi esterne specificando `Foreign Keys=True` nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="99e15-670">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="99e15-671">Microsoft.EntityFrameworkCore.Sqlite dipende ora da SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="99e15-671">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="99e15-672">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-672">**Old behavior**</span></span>

<span data-ttu-id="99e15-673">Nelle versioni precedenti a EF Core 3.0, EF Core usava `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="99e15-673">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="99e15-674">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-674">**New behavior**</span></span>

<span data-ttu-id="99e15-675">A partire da EF Core 3.0, EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="99e15-675">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="99e15-676">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-676">**Why**</span></span>

<span data-ttu-id="99e15-677">Questa modifica è stata apportata per rendere coerente la versione di SQLite usata in iOS con le altre piattaforme.</span><span class="sxs-lookup"><span data-stu-id="99e15-677">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="99e15-678">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-678">**Mitigations**</span></span>

<span data-ttu-id="99e15-679">Per usare la versione di SQLite nativa in iOS, configurare `Microsoft.Data.Sqlite` per l'uso di un'aggregazione `SQLitePCLRaw` diversa.</span><span class="sxs-lookup"><span data-stu-id="99e15-679">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="99e15-680">I valori Guid vengono ora archiviati come TEXT in SQLite</span><span class="sxs-lookup"><span data-stu-id="99e15-680">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="99e15-681">Problema n. 15078</span><span class="sxs-lookup"><span data-stu-id="99e15-681">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="99e15-682">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-682">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-683">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-683">**Old behavior**</span></span>

<span data-ttu-id="99e15-684">I valori Guid in precedenza venivano archiviati come valori BLOB in SQLite.</span><span class="sxs-lookup"><span data-stu-id="99e15-684">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="99e15-685">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-685">**New behavior**</span></span>

<span data-ttu-id="99e15-686">I valori Guid vengono ora archiviati come TEXT.</span><span class="sxs-lookup"><span data-stu-id="99e15-686">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="99e15-687">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-687">**Why**</span></span>

<span data-ttu-id="99e15-688">Il formato binario dei valori Guid non è standardizzato.</span><span class="sxs-lookup"><span data-stu-id="99e15-688">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="99e15-689">L'archiviazione dei valori come TEXT rende il database più compatibile con altre tecnologie.</span><span class="sxs-lookup"><span data-stu-id="99e15-689">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="99e15-690">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-690">**Mitigations**</span></span>

<span data-ttu-id="99e15-691">È possibile eseguire la migrazione dei database esistenti al nuovo formato eseguendo SQL nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="99e15-691">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="99e15-692">In EF Core è anche possibile continuare a usare il comportamento precedente configurando un convertitore di valori per queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="99e15-692">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="99e15-693">Microsoft.Data.Sqlite rimane in grado di leggere i valori Guid sia da colonne BLOB che TEXT. Tuttavia, poiché è stato modificato il formato predefinito per i parametri e le costanti, probabilmente sarà necessario intervenire per la maggior parte degli scenari che coinvolgono valori Guid.</span><span class="sxs-lookup"><span data-stu-id="99e15-693">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="99e15-694">I valori char vengono ora archiviati come testo in SQLite</span><span class="sxs-lookup"><span data-stu-id="99e15-694">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="99e15-695">Problema n. 15020</span><span class="sxs-lookup"><span data-stu-id="99e15-695">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="99e15-696">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-696">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-697">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-697">**Old behavior**</span></span>

<span data-ttu-id="99e15-698">I valori char in precedenza venivano archiviati come valori interi in SQLite.</span><span class="sxs-lookup"><span data-stu-id="99e15-698">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="99e15-699">Un valore char di *A* veniva ad esempio archiviato come valore intero 65.</span><span class="sxs-lookup"><span data-stu-id="99e15-699">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="99e15-700">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-700">**New behavior**</span></span>

<span data-ttu-id="99e15-701">I valori char vengono ora archiviati come testo.</span><span class="sxs-lookup"><span data-stu-id="99e15-701">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="99e15-702">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-702">**Why**</span></span>

<span data-ttu-id="99e15-703">L'archiviazione dei valori come testo è un'operazione più naturale e rende il database più compatibile con altre tecnologie.</span><span class="sxs-lookup"><span data-stu-id="99e15-703">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="99e15-704">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-704">**Mitigations**</span></span>

<span data-ttu-id="99e15-705">È possibile eseguire la migrazione dei database esistenti al nuovo formato eseguendo SQL nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="99e15-705">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="99e15-706">In EF Core è anche possibile continuare a usare il comportamento precedente configurando un convertitore di valori per queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="99e15-706">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="99e15-707">Microsoft.Data.Sqlite rimane comunque in grado di leggere i valori di caratteri presenti sia nelle colonne di valori interi sia in quelle di testo, quindi alcuni scenari potrebbero non richiedere alcuna azione.</span><span class="sxs-lookup"><span data-stu-id="99e15-707">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="99e15-708">Gli ID di migrazione vengono ora generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica</span><span class="sxs-lookup"><span data-stu-id="99e15-708">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="99e15-709">Problema n. 12978</span><span class="sxs-lookup"><span data-stu-id="99e15-709">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="99e15-710">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-710">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-711">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-711">**Old behavior**</span></span>

<span data-ttu-id="99e15-712">Gli ID di migrazione venivano inavvertitamente generati usando il calendario delle impostazioni cultura correnti.</span><span class="sxs-lookup"><span data-stu-id="99e15-712">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="99e15-713">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-713">**New behavior**</span></span>

<span data-ttu-id="99e15-714">Gli ID di migrazione ora vengono sempre generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica (calendario gregoriano).</span><span class="sxs-lookup"><span data-stu-id="99e15-714">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="99e15-715">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-715">**Why**</span></span>

<span data-ttu-id="99e15-716">L'ordine delle migrazioni è importante quando si esegue l'aggiornamento del database o si risolvono i conflitti di unione.</span><span class="sxs-lookup"><span data-stu-id="99e15-716">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="99e15-717">L'uso del calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica evita i problemi che possono verificarsi quando i membri del team hanno calendari di sistema diversi.</span><span class="sxs-lookup"><span data-stu-id="99e15-717">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="99e15-718">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-718">**Mitigations**</span></span>

<span data-ttu-id="99e15-719">Questa modifica interessa gli utenti che usano un calendario non gregoriano in cui l'anno ha un'estensione superiore al calendario gregoriano (come il calendario buddista tailandese).</span><span class="sxs-lookup"><span data-stu-id="99e15-719">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="99e15-720">Gli ID di migrazione esistenti dovranno essere aggiornati in modo che le nuove migrazioni vengano collocate dopo le migrazioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="99e15-720">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="99e15-721">L'ID di migrazione è disponibile nell'attributo di migrazione presente nei file di progettazione delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="99e15-721">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="99e15-722">È necessario aggiornare anche la tabella della cronologia delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="99e15-722">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="99e15-723">Il metodo UseRowNumberForPaging è stato rimosso</span><span class="sxs-lookup"><span data-stu-id="99e15-723">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="99e15-724">Problema n. 16400</span><span class="sxs-lookup"><span data-stu-id="99e15-724">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="99e15-725">Questa modifica è stata introdotta in EF Core 3.0 anteprima 6.</span><span class="sxs-lookup"><span data-stu-id="99e15-725">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="99e15-726">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-726">**Old behavior**</span></span>

<span data-ttu-id="99e15-727">Prima di EF Core 3.0 si poteva usare `UseRowNumberForPaging` per generare codice SQL per la suddivisione in pagine compatibile con SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="99e15-727">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="99e15-728">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-728">**New behavior**</span></span>

<span data-ttu-id="99e15-729">A partire da EF Core 3.0, EF genererà solo codice SQL per la suddivisione in pagine compatibile solo con versioni successive di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="99e15-729">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="99e15-730">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-730">**Why**</span></span>

<span data-ttu-id="99e15-731">Questa modifica è stata apportata perché [SQL Server 2008 non è più un prodotto supportato](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) e l'aggiornamento di questa funzionalità per interagire con le modifiche apportate per le query in EF Core 3.0 è un lavoro significativo.</span><span class="sxs-lookup"><span data-stu-id="99e15-731">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="99e15-732">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-732">**Mitigations**</span></span>

<span data-ttu-id="99e15-733">È consigliabile eseguire l'aggiornamento a una versione più recente di SQL Server o usare un livello di compatibilità superiore, in modo che il codice SQL generato sia supportato.</span><span class="sxs-lookup"><span data-stu-id="99e15-733">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="99e15-734">Detto questo, se non è possibile procedere in questo modo, [aggiungere un commento per il problema](https://github.com/aspnet/EntityFrameworkCore/issues/16400) con indicazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="99e15-734">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="99e15-735">Microsoft potrebbe rivedere questa decisione in base ai commenti e suggerimenti.</span><span class="sxs-lookup"><span data-stu-id="99e15-735">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="99e15-736">Info/metadati dell'estensione rimossi da IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="99e15-736">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="99e15-737">Problema n. 16119</span><span class="sxs-lookup"><span data-stu-id="99e15-737">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="99e15-738">Questa modifica è stata introdotta in EF Core 3.0 anteprima 7.</span><span class="sxs-lookup"><span data-stu-id="99e15-738">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="99e15-739">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-739">**Old behavior**</span></span>

<span data-ttu-id="99e15-740">`IDbContextOptionsExtension` conteneva metodi per fornire i metadati relativi all'estensione.</span><span class="sxs-lookup"><span data-stu-id="99e15-740">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="99e15-741">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-741">**New behavior**</span></span>

<span data-ttu-id="99e15-742">Questi metodi sono stati spostati in una nuova classe di base astratta `DbContextOptionsExtensionInfo`, restituita da una nuova proprietà `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="99e15-742">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="99e15-743">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-743">**Why**</span></span>

<span data-ttu-id="99e15-744">Nelle versioni dalla 2.0 alla 3.0 è stato necessario aggiungere o modificare questi metodi più volte.</span><span class="sxs-lookup"><span data-stu-id="99e15-744">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="99e15-745">Suddividendoli in una nuova classe di base astratta sarà più facile apportare questo tipo di modifiche senza compromettere il funzionamento delle estensioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="99e15-745">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="99e15-746">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-746">**Mitigations**</span></span>

<span data-ttu-id="99e15-747">Aggiornare le estensioni per seguire il nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="99e15-747">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="99e15-748">Sono disponibili esempi nelle numerose implementazioni di `IDbContextOptionsExtension` per diversi tipi di estensioni nel codice sorgente di EF Core.</span><span class="sxs-lookup"><span data-stu-id="99e15-748">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="99e15-749">LogQueryPossibleExceptionWithAggregateOperator è stato rinominato</span><span class="sxs-lookup"><span data-stu-id="99e15-749">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="99e15-750">Problema n. 10985</span><span class="sxs-lookup"><span data-stu-id="99e15-750">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="99e15-751">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-751">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-752">**Modifica**</span><span class="sxs-lookup"><span data-stu-id="99e15-752">**Change**</span></span>

<span data-ttu-id="99e15-753">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` è stato rinominato in `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="99e15-753">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="99e15-754">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-754">**Why**</span></span>

<span data-ttu-id="99e15-755">Allineamento del nome di questo evento di avviso con tutti gli altri eventi di avviso.</span><span class="sxs-lookup"><span data-stu-id="99e15-755">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="99e15-756">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-756">**Mitigations**</span></span>

<span data-ttu-id="99e15-757">Usare il nuovo nome.</span><span class="sxs-lookup"><span data-stu-id="99e15-757">Use the new name.</span></span> <span data-ttu-id="99e15-758">(Si noti che il numero di ID evento non è stato modificato.)</span><span class="sxs-lookup"><span data-stu-id="99e15-758">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="99e15-759">Chiarimenti per l'API per i nomi di vincolo di chiave esterna</span><span class="sxs-lookup"><span data-stu-id="99e15-759">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="99e15-760">Problema n. 10730</span><span class="sxs-lookup"><span data-stu-id="99e15-760">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="99e15-761">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-761">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-762">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-762">**Old behavior**</span></span>

<span data-ttu-id="99e15-763">Prima di EF Core 3.0, si faceva riferimento ai nomi di vincolo di chiave esterna semplicemente con "Name".</span><span class="sxs-lookup"><span data-stu-id="99e15-763">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="99e15-764">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="99e15-764">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="99e15-765">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-765">**New behavior**</span></span>

<span data-ttu-id="99e15-766">A partire da EF Core 3.0, si fa ora riferimento ai nomi di vincolo di chiave esterna con "ConstraintName".</span><span class="sxs-lookup"><span data-stu-id="99e15-766">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="99e15-767">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="99e15-767">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="99e15-768">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-768">**Why**</span></span>

<span data-ttu-id="99e15-769">Questa modifica introduce coerenza per la denominazione in quest'area e chiarisce anche che si tratta del nome del vincolo di chiave esterna e non del nome della colonna o della proprietà per cui è definita la chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="99e15-769">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="99e15-770">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-770">**Mitigations**</span></span>

<span data-ttu-id="99e15-771">Usare il nuovo nome.</span><span class="sxs-lookup"><span data-stu-id="99e15-771">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="99e15-772">IRelationalDatabaseCreator.HasTables/HasTablesAsync sono diventati pubblici</span><span class="sxs-lookup"><span data-stu-id="99e15-772">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="99e15-773">Problema n. 15997</span><span class="sxs-lookup"><span data-stu-id="99e15-773">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="99e15-774">Questa modifica è stata introdotta in EF Core 3.0 anteprima 7.</span><span class="sxs-lookup"><span data-stu-id="99e15-774">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="99e15-775">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-775">**Old behavior**</span></span>

<span data-ttu-id="99e15-776">Prima di EF Core 3.0 questi metodi erano protetti.</span><span class="sxs-lookup"><span data-stu-id="99e15-776">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="99e15-777">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-777">**New behavior**</span></span>

<span data-ttu-id="99e15-778">A partire da EF Core 3.0 questi metodi sono pubblici.</span><span class="sxs-lookup"><span data-stu-id="99e15-778">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="99e15-779">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-779">**Why**</span></span>

<span data-ttu-id="99e15-780">Questi metodi vengono usati da EF per determinare se un database viene creato, ma vuoto.</span><span class="sxs-lookup"><span data-stu-id="99e15-780">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="99e15-781">Ciò risulta utile all'esterno di EF quando occorre determinare se applicare o meno le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="99e15-781">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="99e15-782">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-782">**Mitigations**</span></span>

<span data-ttu-id="99e15-783">Modificare l'accessibilità di eventuali override.</span><span class="sxs-lookup"><span data-stu-id="99e15-783">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="99e15-784">Microsoft.EntityFrameworkCore.Design è ora un pacchetto DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="99e15-784">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="99e15-785">Problema n. 11506</span><span class="sxs-lookup"><span data-stu-id="99e15-785">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="99e15-786">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="99e15-786">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="99e15-787">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-787">**Old behavior**</span></span>

<span data-ttu-id="99e15-788">Prima di EF Core 3.0, Microsoft.EntityFrameworkCore.Design era un pacchetto NuGet normale ed era possibile fare riferimento al relativo assembly dai progetti dipendenti.</span><span class="sxs-lookup"><span data-stu-id="99e15-788">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="99e15-789">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-789">**New behavior**</span></span>

<span data-ttu-id="99e15-790">A partire da EF Core 3.0 è un pacchetto DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="99e15-790">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="99e15-791">Questo significa che la dipendenza non verrà trasferita in modo transitivo in altri progetti e che non è più possibile fare riferimento all'assembly per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="99e15-791">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="99e15-792">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-792">**Why**</span></span>

<span data-ttu-id="99e15-793">Questo pacchetto è destinato solo all'uso in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="99e15-793">This package is only intended to be used at design time.</span></span> <span data-ttu-id="99e15-794">Le applicazioni distribuite non devono farvi riferimento.</span><span class="sxs-lookup"><span data-stu-id="99e15-794">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="99e15-795">Questa raccomandazione è rafforzata dall'impostazione del pacchetto come DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="99e15-795">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="99e15-796">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-796">**Mitigations**</span></span>

<span data-ttu-id="99e15-797">Se è necessario fare riferimento a questo pacchetto per eseguire l'override del comportamento in fase di progettazione di EF Core, è possibile aggiornare i metadati dell'elemento PackageReference nel progetto.</span><span class="sxs-lookup"><span data-stu-id="99e15-797">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="99e15-798">In presenza di riferimenti transitivi al pacchetto tramite Microsoft.EntityFrameworkCore.Tools, sarà necessario aggiungere un PackageReference esplicito al pacchetto per modificare i relativi metadati.</span><span class="sxs-lookup"><span data-stu-id="99e15-798">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="99e15-799">Aggiornamento di SQLitePCL.raw alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="99e15-799">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="99e15-800">Problema n. 14824</span><span class="sxs-lookup"><span data-stu-id="99e15-800">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="99e15-801">Questa modifica è stata introdotta in EF Core 3.0 anteprima 7.</span><span class="sxs-lookup"><span data-stu-id="99e15-801">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="99e15-802">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-802">**Old behavior**</span></span>

<span data-ttu-id="99e15-803">Microsoft.EntityFrameworkCore.Sqlite dipendeva in precedenza dalla versione 1.1.12 di SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="99e15-803">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="99e15-804">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-804">**New behavior**</span></span>

<span data-ttu-id="99e15-805">Il pacchetto è stato aggiornato in modo da dipendere dalla versione 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="99e15-805">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="99e15-806">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-806">**Why**</span></span>

<span data-ttu-id="99e15-807">La versione 2.0.0 di SQLitePCL.raw è destinata a .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="99e15-807">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="99e15-808">Era in precedenza destinata a .NET Standard 1.1 e ciò richiedeva un notevole impegno di chiusura di pacchetti transitivi per il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="99e15-808">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="99e15-809">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-809">**Mitigations**</span></span>

<span data-ttu-id="99e15-810">SQLitePCL.raw versione 2.0.0 include alcune modifiche che causano un'interruzione.</span><span class="sxs-lookup"><span data-stu-id="99e15-810">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="99e15-811">Per informazioni dettagliate, vedere le [note sulla versione](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="99e15-811">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="99e15-812">NetTopologySuite aggiornato alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="99e15-812">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="99e15-813">Problema n. 14825</span><span class="sxs-lookup"><span data-stu-id="99e15-813">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="99e15-814">Questa modifica è stata introdotta in EF Core 3.0 anteprima 7.</span><span class="sxs-lookup"><span data-stu-id="99e15-814">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="99e15-815">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-815">**Old behavior**</span></span>

<span data-ttu-id="99e15-816">I pacchetti spaziali dipendevano in precedenza dalla versione 1.15.1 di NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="99e15-816">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="99e15-817">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-817">**New behavior**</span></span>

<span data-ttu-id="99e15-818">Il pacchetto è stato aggiornato in modo da dipendere dalla versione 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="99e15-818">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="99e15-819">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-819">**Why**</span></span>

<span data-ttu-id="99e15-820">La versione 2.0.0 di NetTopologySuite risolve vari problemi di usabilità riscontrati dagli utenti di EF Core.</span><span class="sxs-lookup"><span data-stu-id="99e15-820">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="99e15-821">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-821">**Mitigations**</span></span>

<span data-ttu-id="99e15-822">NetTopologySuite versione 2.0.0 include alcune modifiche che causano un'interruzione.</span><span class="sxs-lookup"><span data-stu-id="99e15-822">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="99e15-823">Per informazioni dettagliate, vedere le [note sulla versione](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="99e15-823">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="99e15-824">Devono essere configurare più relazioni ambigue che fanno riferimento a se stesse</span><span class="sxs-lookup"><span data-stu-id="99e15-824">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="99e15-825">Problema n. 13573</span><span class="sxs-lookup"><span data-stu-id="99e15-825">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="99e15-826">Questa modifica è stata introdotta in EF Core 3.0 anteprima 6.</span><span class="sxs-lookup"><span data-stu-id="99e15-826">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="99e15-827">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="99e15-827">**Old behavior**</span></span>

<span data-ttu-id="99e15-828">Un tipo di entità con più proprietà di navigazione unidirezionale che fanno riferimento a se stesse e più chiavi esterne corrispondenti è stato erroneamente configurato come relazione singola.</span><span class="sxs-lookup"><span data-stu-id="99e15-828">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="99e15-829">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="99e15-829">For example:</span></span>

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

<span data-ttu-id="99e15-830">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="99e15-830">**New behavior**</span></span>

<span data-ttu-id="99e15-831">Questo scenario viene ora rilevato nella compilazione del modello e viene generata un'eccezione indicante che il modello è ambiguo.</span><span class="sxs-lookup"><span data-stu-id="99e15-831">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="99e15-832">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="99e15-832">**Why**</span></span>

<span data-ttu-id="99e15-833">Il modello risultante era ambiguo e sarà probabilmente errato in questo caso.</span><span class="sxs-lookup"><span data-stu-id="99e15-833">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="99e15-834">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="99e15-834">**Mitigations**</span></span>

<span data-ttu-id="99e15-835">Usare la configurazione completa della relazione.</span><span class="sxs-lookup"><span data-stu-id="99e15-835">Use full configuration of the relationship.</span></span> <span data-ttu-id="99e15-836">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="99e15-836">For example:</span></span>

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
