---
title: Pianificare Entity Framework Core 5,0
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 0472841fdcd105ec8ea38db062c6768510b8735d
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125382"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="ac5f2-102">Pianificare Entity Framework Core 5,0</span><span class="sxs-lookup"><span data-stu-id="ac5f2-102">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="ac5f2-103">Come descritto nel [processo di pianificazione](../release-planning.md), l'input è stato raccolto dagli stakeholder in un piano provvisorio per la versione di EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-103">As described in the [planning process](../release-planning.md), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="ac5f2-104">Questo piano è ancora in fase di elaborazione.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-104">This plan is still a work-in-progress.</span></span> <span data-ttu-id="ac5f2-105">Niente qui è un impegno.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-105">Nothing here is a commitment.</span></span> <span data-ttu-id="ac5f2-106">Questo piano è un punto di partenza che si evolverà Man mano che si apprenderanno altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-106">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="ac5f2-107">È possibile che venga effettuato il pull di alcuni elementi attualmente non pianificati per 5,0.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-107">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="ac5f2-108">È possibile che vengano apportate alcune operazioni attualmente pianificate per 5,0.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-108">Some things currently planned for 5.0 may get punted out.</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="ac5f2-109">Numero di versione e data di rilascio.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-109">Version number and release date.</span></span>

<span data-ttu-id="ac5f2-110">EF Core 5,0 è attualmente pianificata per la versione allo [stesso tempo di .net 5,0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="ac5f2-110">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="ac5f2-111">È stata scelta la versione "5,0" per l'allineamento con .NET 5,0.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-111">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="ac5f2-112">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="ac5f2-112">Supported platforms</span></span>

<span data-ttu-id="ac5f2-113">EF Core 5,0 è pianificato per essere eseguito su qualsiasi piattaforma .NET 5,0 in base alla [convergenza di queste piattaforme con .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="ac5f2-113">EF Core 5.0 is planned to run on any .NET 5.0 platform based on the [convergence of these platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="ac5f2-114">Ciò significa che in termini di .NET Standard e il TFM effettivo usato è ancora da definire.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-114">What this means in terms of .NET Standard and the actual TFM used is still TBD.</span></span>

<span data-ttu-id="ac5f2-115">EF Core 5,0 non viene eseguito su .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-115">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="ac5f2-116">Modifiche che causano un'interruzione</span><span class="sxs-lookup"><span data-stu-id="ac5f2-116">Breaking changes</span></span>

<span data-ttu-id="ac5f2-117">EF Core 5,0 conterrà alcune modifiche di rilievo, ma saranno molto meno gravi rispetto a quanto accadeva per EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-117">EF Core 5.0 will contain some breaking changes, but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="ac5f2-118">Il nostro obiettivo è consentire l'aggiornamento della maggior parte delle applicazioni senza interruzioni.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-118">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="ac5f2-119">Si prevede che verranno apportate alcune modifiche di rilievo per i provider di database, in particolare per quanto riguarda il supporto di TPT.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-119">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="ac5f2-120">Tuttavia, si prevede che il lavoro per aggiornare un provider per 5,0 sarà inferiore a quello necessario per l'aggiornamento a 3,0.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-120">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="ac5f2-121">Themes</span><span class="sxs-lookup"><span data-stu-id="ac5f2-121">Themes</span></span>

<span data-ttu-id="ac5f2-122">Sono state estratte alcune aree o temi principali che costituiranno la base per gli investimenti di grandi dimensioni in EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-122">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="ac5f2-123">Proprietà di navigazione molti-a-molti (a. k. a "Ignora navigazione")</span><span class="sxs-lookup"><span data-stu-id="ac5f2-123">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="ac5f2-124">Sviluppatori leader: @smitpatel e @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="ac5f2-124">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="ac5f2-125">Rilevato da [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span><span class="sxs-lookup"><span data-stu-id="ac5f2-125">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="ac5f2-126">Dimensioni della maglietta: L</span><span class="sxs-lookup"><span data-stu-id="ac5f2-126">T-shirt size: L</span></span>

<span data-ttu-id="ac5f2-127">Stato: in corso</span><span class="sxs-lookup"><span data-stu-id="ac5f2-127">Status: In-progress</span></span>

<span data-ttu-id="ac5f2-128">Many-to-many è la funzionalità più richiesta (~ 407 voti) nel backlog di GitHub.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-128">Many-to-many is the most requested feature (~407 votes) on the GitHub backlog.</span></span> <span data-ttu-id="ac5f2-129">Il supporto per le relazioni molti-a-molti può essere suddiviso in tre aree principali:</span><span class="sxs-lookup"><span data-stu-id="ac5f2-129">Support for many-to-many relationships can be broken down into three major areas:</span></span>

* <span data-ttu-id="ac5f2-130">Ignorare le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-130">Skip navigation properties.</span></span> <span data-ttu-id="ac5f2-131">Che consentono di utilizzare il modello per le query e così via, senza riferimento all'entità sottostante della tabella di join.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-131">These allow the model to be used for queries, etc. without reference to the underlying join table entity.</span></span>
* <span data-ttu-id="ac5f2-132">Tipi di entità del contenitore delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-132">Property-bag entity types.</span></span> <span data-ttu-id="ac5f2-133">Che consentono di usare un tipo CLR standard (ad esempio `Dictionary`) per le istanze di entità in modo che non sia necessario un tipo CLR esplicito per ogni tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-133">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span>
* <span data-ttu-id="ac5f2-134">Sugar per semplificare la configurazione di relazioni molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-134">Sugar for easy configuration of many-to-many relationships.</span></span>

<span data-ttu-id="ac5f2-135">Il blocco più significativo per coloro che desiderano il supporto molti-a-molti non è in grado di utilizzare le relazioni "naturali", senza fare riferimento alla tabella di join, nella logica di business, ad esempio le query.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-135">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span> <span data-ttu-id="ac5f2-136">Il tipo di entità tabella di join può ancora esistere, ma non dovrebbe essere in grado di ottenere la logica di business.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-136">The join table entity type may still exist, but it should not get in the way of business logic.</span></span> <span data-ttu-id="ac5f2-137">Questo è il motivo per cui abbiamo scelto di affrontare le proprietà di navigazione Skip per 5,0.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-137">This is why we have chosen to tackle skip navigation properties for 5.0.</span></span>

<span data-ttu-id="ac5f2-138">In questo momento le altre parti di molti-a-molti vengono seguite come obiettivo di estensione per EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-138">At this time the other parts of many-to-many are being pursued as a stretch goal for EF Core 5.0.</span></span> <span data-ttu-id="ac5f2-139">Ciò significa che non sono attualmente nel piano per 5,0, ma se le cose vanno bene ci auguriamo di poterle inserire.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-139">This means they are not currently in the plan for 5.0, but if things go well we hope to pull them in.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="ac5f2-140">Mapping di ereditarietà tabella per tipo (TPT)</span><span class="sxs-lookup"><span data-stu-id="ac5f2-140">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="ac5f2-141">Sviluppatore principale: @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="ac5f2-141">Lead developer: @AndriySvyryd</span></span>

<span data-ttu-id="ac5f2-142">Rilevato da [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span><span class="sxs-lookup"><span data-stu-id="ac5f2-142">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="ac5f2-143">Dimensioni della maglietta: XL</span><span class="sxs-lookup"><span data-stu-id="ac5f2-143">T-shirt size: XL</span></span>

<span data-ttu-id="ac5f2-144">Stato: non avviato</span><span class="sxs-lookup"><span data-stu-id="ac5f2-144">Status: Not started</span></span>

<span data-ttu-id="ac5f2-145">Stiamo eseguendo TPT perché si tratta di una funzionalità estremamente richiesta (~ 254 voti, 3 in generale) e perché richiede alcune modifiche di basso livello che si ritiene siano appropriate per la natura fondamentale del piano generale .NET 5.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-145">We're doing TPT because it is both a highly requested feature (~254 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="ac5f2-146">Si prevede che questo provocherà modifiche di rilievo per i provider di database, anche se queste devono essere molto meno gravi rispetto alle modifiche necessarie per 3,0.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-146">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="ac5f2-147">Inclusione filtrato</span><span class="sxs-lookup"><span data-stu-id="ac5f2-147">Filtered Include</span></span>

<span data-ttu-id="ac5f2-148">Sviluppatore principale: @maumar</span><span class="sxs-lookup"><span data-stu-id="ac5f2-148">Lead developer: @maumar</span></span>

<span data-ttu-id="ac5f2-149">Rilevato da [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span><span class="sxs-lookup"><span data-stu-id="ac5f2-149">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="ac5f2-150">Dimensioni T-Shirt: M</span><span class="sxs-lookup"><span data-stu-id="ac5f2-150">T-shirt size: M</span></span>

<span data-ttu-id="ac5f2-151">Stato: non avviato</span><span class="sxs-lookup"><span data-stu-id="ac5f2-151">Status: Not started</span></span>

<span data-ttu-id="ac5f2-152">L'inclusione filtrata è una funzionalità molto richiesta (~ 317 voti, 2 nel complesso) che non è una grande quantità di lavoro e che riteniamo che lo sblocco o renderà più semplice molti scenari che attualmente richiedono filtri a livello di modello o query più complesse.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-152">Filtered Include is a highly-requested feature (~317 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="ac5f2-153">Razionalizzare ToTable, ToQuery, toview, dati da tabelle e così via.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-153">Rationalize ToTable, ToQuery, ToView, FromSql, etc.</span></span>

<span data-ttu-id="ac5f2-154">Sviluppatori leader: @maumar e @smitpatel</span><span class="sxs-lookup"><span data-stu-id="ac5f2-154">Lead developers: @maumar and @smitpatel</span></span>

<span data-ttu-id="ac5f2-155">Rilevato da [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span><span class="sxs-lookup"><span data-stu-id="ac5f2-155">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="ac5f2-156">Dimensioni della maglietta: L</span><span class="sxs-lookup"><span data-stu-id="ac5f2-156">T-shirt size: L</span></span>

<span data-ttu-id="ac5f2-157">Stato: non avviato</span><span class="sxs-lookup"><span data-stu-id="ac5f2-157">Status: Not started</span></span>

<span data-ttu-id="ac5f2-158">Sono stati apportati miglioramenti alle versioni precedenti per il supporto di SQL non elaborato, tipi autochiave e aree correlate.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-158">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="ac5f2-159">Tuttavia, vi sono Gap e incoerenze nel modo in cui tutto funziona insieme nel suo complesso.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-159">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="ac5f2-160">L'obiettivo di 5,0 è correggerli e creare un'esperienza ottimale per la definizione, la migrazione e l'utilizzo di tipi diversi di entità e delle query e degli elementi di database associati.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-160">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="ac5f2-161">Questo può comportare anche aggiornamenti all'API di query compilata.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-161">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="ac5f2-162">Si noti che questo elemento può comportare alcune modifiche sostanziali a livello di applicazione, poiché alcune delle funzionalità attualmente disponibili sono troppo permissive, in modo da consentire agli utenti di raggiungere rapidamente i pozzi di errore.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-162">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="ac5f2-163">È probabile che si stiano bloccando alcune di queste funzionalità insieme a indicazioni sulle operazioni da eseguire.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-163">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="ac5f2-164">Miglioramenti generali delle query</span><span class="sxs-lookup"><span data-stu-id="ac5f2-164">General query enhancements</span></span>

<span data-ttu-id="ac5f2-165">Sviluppatori leader: @smitpatel e @maumar</span><span class="sxs-lookup"><span data-stu-id="ac5f2-165">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="ac5f2-166">Rilevato da [problemi contrassegnati con `area-query` nell'attività cardine 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="ac5f2-166">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="ac5f2-167">Dimensioni della maglietta: XL</span><span class="sxs-lookup"><span data-stu-id="ac5f2-167">T-shirt size: XL</span></span>

<span data-ttu-id="ac5f2-168">Stato: in corso</span><span class="sxs-lookup"><span data-stu-id="ac5f2-168">Status: In-progress</span></span>

<span data-ttu-id="ac5f2-169">Il codice di traduzione query è stato riscritto in maniera estensiva per EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-169">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="ac5f2-170">Il codice di query si trova in genere in uno stato molto più solido a causa di questo problema.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-170">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="ac5f2-171">Per 5,0 non è prevista la creazione di modifiche di query principali, al di fuori di quelle necessarie per supportare TPT e ignorare le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-171">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="ac5f2-172">Tuttavia, è ancora necessario un lavoro significativo per risolvere alcuni debiti tecnici lasciati dalla revisione 3,0.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-172">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="ac5f2-173">Si prevede inoltre di correggere molti bug e implementare piccoli miglioramenti per migliorare ulteriormente l'esperienza complessiva della query.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-173">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="ac5f2-174">Migrazioni ed esperienza di distribuzione</span><span class="sxs-lookup"><span data-stu-id="ac5f2-174">Migrations and deployment experience</span></span>

<span data-ttu-id="ac5f2-175">Sviluppatori leader: @bricelam</span><span class="sxs-lookup"><span data-stu-id="ac5f2-175">Lead developers: @bricelam</span></span>

<span data-ttu-id="ac5f2-176">Rilevato da [#19587](https://github.com/dotnet/efcore/issues/19587)</span><span class="sxs-lookup"><span data-stu-id="ac5f2-176">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="ac5f2-177">Dimensioni della maglietta: L</span><span class="sxs-lookup"><span data-stu-id="ac5f2-177">T-shirt size: L</span></span>

<span data-ttu-id="ac5f2-178">Stato: in corso</span><span class="sxs-lookup"><span data-stu-id="ac5f2-178">Status: In-progress</span></span>

<span data-ttu-id="ac5f2-179">Attualmente, molti sviluppatori migrano i database al momento dell'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-179">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="ac5f2-180">Questa operazione è semplice, ma non è consigliata perché:</span><span class="sxs-lookup"><span data-stu-id="ac5f2-180">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="ac5f2-181">Più thread/processi/server possono tentare di eseguire la migrazione simultanea del database</span><span class="sxs-lookup"><span data-stu-id="ac5f2-181">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="ac5f2-182">Le applicazioni potrebbero provare ad accedere allo stato incoerente durante l'operazione</span><span class="sxs-lookup"><span data-stu-id="ac5f2-182">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="ac5f2-183">In genere le autorizzazioni per il database per modificare lo schema non devono essere concesse per l'esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="ac5f2-183">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="ac5f2-184">È difficile ripristinare uno stato pulito se si verifica un problema</span><span class="sxs-lookup"><span data-stu-id="ac5f2-184">Its hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="ac5f2-185">Si vuole offrire un'esperienza migliore che consenta di eseguire la migrazione del database in fase di distribuzione in modo semplice.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-185">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="ac5f2-186">Questa operazione dovrebbe:</span><span class="sxs-lookup"><span data-stu-id="ac5f2-186">This should:</span></span>

* <span data-ttu-id="ac5f2-187">Lavorare su Linux, Mac e Windows</span><span class="sxs-lookup"><span data-stu-id="ac5f2-187">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="ac5f2-188">Esperienza ottimale nella riga di comando</span><span class="sxs-lookup"><span data-stu-id="ac5f2-188">Be a good experience on the command line</span></span>
* <span data-ttu-id="ac5f2-189">Supportare scenari con i contenitori</span><span class="sxs-lookup"><span data-stu-id="ac5f2-189">Support scenarios with containers</span></span>
* <span data-ttu-id="ac5f2-190">Usare gli strumenti e i flussi di distribuzione reali usati di frequente</span><span class="sxs-lookup"><span data-stu-id="ac5f2-190">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="ac5f2-191">Integrazione in almeno Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac5f2-191">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="ac5f2-192">Il risultato è probabilmente costituito da numerosi miglioramenti di EF Core (ad esempio, migliori migrazioni in SQLite), insieme a linee guida e collaborazioni a lungo termine con altri team per migliorare le esperienze end-to-end che vanno oltre il solo EF.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-192">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="ac5f2-193">Esperienza delle piattaforme EF Core</span><span class="sxs-lookup"><span data-stu-id="ac5f2-193">EF Core platforms experience</span></span> 

<span data-ttu-id="ac5f2-194">Sviluppatori leader: @roji e @bricelam</span><span class="sxs-lookup"><span data-stu-id="ac5f2-194">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="ac5f2-195">Rilevato da [#19588](https://github.com/dotnet/efcore/issues/19588)</span><span class="sxs-lookup"><span data-stu-id="ac5f2-195">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="ac5f2-196">Dimensioni della maglietta: L</span><span class="sxs-lookup"><span data-stu-id="ac5f2-196">T-shirt size: L</span></span>

<span data-ttu-id="ac5f2-197">Stato: non avviato</span><span class="sxs-lookup"><span data-stu-id="ac5f2-197">Status: Not started</span></span>

<span data-ttu-id="ac5f2-198">Sono disponibili indicazioni utili per l'uso di EF Core in applicazioni Web tradizionali di tipo MVC.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-198">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="ac5f2-199">Linee guida per altre piattaforme e modelli di applicazione mancanti o non aggiornati.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-199">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="ac5f2-200">Per EF Core 5,0 si prevede di esaminare, migliorare e documentare l'esperienza di utilizzo di EF Core con:</span><span class="sxs-lookup"><span data-stu-id="ac5f2-200">For EF Core 5.0 we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="ac5f2-201">Blazor</span><span class="sxs-lookup"><span data-stu-id="ac5f2-201">Blazor</span></span>
* <span data-ttu-id="ac5f2-202">Novell, incluso l'uso della storia AOT/linker</span><span class="sxs-lookup"><span data-stu-id="ac5f2-202">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="ac5f2-203">WinForms/WPF/WinUI e possibilmente altri U.I.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-203">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="ac5f2-204">frameworks</span><span class="sxs-lookup"><span data-stu-id="ac5f2-204">frameworks</span></span>

<span data-ttu-id="ac5f2-205">È probabile che si verifichino molti piccoli miglioramenti in EF Core, insieme a linee guida e collaborazioni a lungo termine con altri team per migliorare le esperienze end-to-end che vanno oltre la semplice EF.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-205">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="ac5f2-206">Le aree specifiche da esaminare sono:</span><span class="sxs-lookup"><span data-stu-id="ac5f2-206">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="ac5f2-207">Distribuzione, inclusa l'esperienza di utilizzo degli strumenti EF, ad esempio per le migrazioni</span><span class="sxs-lookup"><span data-stu-id="ac5f2-207">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="ac5f2-208">Modelli di applicazione, inclusi Novell e blazer, e probabilmente altri</span><span class="sxs-lookup"><span data-stu-id="ac5f2-208">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="ac5f2-209">Esperienze di SQLite, tra cui l'esperienza spaziale e le ricompilazioni di tabelle</span><span class="sxs-lookup"><span data-stu-id="ac5f2-209">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="ac5f2-210">Esperienza AOT e collegamento</span><span class="sxs-lookup"><span data-stu-id="ac5f2-210">AOT and linking experiences</span></span>
* <span data-ttu-id="ac5f2-211">Integrazione di diagnostica, inclusi i contatori delle prestazioni</span><span class="sxs-lookup"><span data-stu-id="ac5f2-211">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="ac5f2-212">Prestazioni</span><span class="sxs-lookup"><span data-stu-id="ac5f2-212">Performance</span></span>

<span data-ttu-id="ac5f2-213">Sviluppatore principale: @roji</span><span class="sxs-lookup"><span data-stu-id="ac5f2-213">Lead developer: @roji</span></span>

<span data-ttu-id="ac5f2-214">Rilevato da [problemi contrassegnati con `area-perf` nell'attività cardine 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="ac5f2-214">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="ac5f2-215">Dimensioni della maglietta: L</span><span class="sxs-lookup"><span data-stu-id="ac5f2-215">T-shirt size: L</span></span>

<span data-ttu-id="ac5f2-216">Stato: in corso</span><span class="sxs-lookup"><span data-stu-id="ac5f2-216">Status: In-progress</span></span>

<span data-ttu-id="ac5f2-217">Per EF Core si prevede di migliorare la nostra suite di benchmark delle prestazioni e di apportare miglioramenti al runtime.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-217">For EF Core we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="ac5f2-218">Si prevede inoltre di completare la nuova API batch ADO.NET, che è stata prototipata durante il ciclo di rilascio 3,0.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-218">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="ac5f2-219">Inoltre, a livello di ADO.NET, vengono pianificati miglioramenti delle prestazioni aggiuntivi per il provider npgsql.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-219">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="ac5f2-220">Nell'ambito di questo lavoro si prevede anche di aggiungere i contatori delle prestazioni di ADO.NET/EF core e altri dati di diagnostica in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-220">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="ac5f2-221">Documentazione dell'architettura/collaboratore</span><span class="sxs-lookup"><span data-stu-id="ac5f2-221">Architectural/contributor documentation</span></span>

<span data-ttu-id="ac5f2-222">Documento principale: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="ac5f2-222">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="ac5f2-223">Rilevato da [#1920](https://github.com/aspnet/EntityFramework.Docs/issues/1920)</span><span class="sxs-lookup"><span data-stu-id="ac5f2-223">Tracked by [#1920](https://github.com/aspnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="ac5f2-224">Dimensioni della maglietta: L</span><span class="sxs-lookup"><span data-stu-id="ac5f2-224">T-shirt size: L</span></span>

<span data-ttu-id="ac5f2-225">Stato: non avviato</span><span class="sxs-lookup"><span data-stu-id="ac5f2-225">Status: Not started</span></span>

<span data-ttu-id="ac5f2-226">L'idea è quella di semplificare la comprensione di ciò che accade negli elementi interni del EF Core.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-226">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="ac5f2-227">Questa operazione può essere utile per chiunque usi EF Core, ma la motivazione principale consiste nel rendere più semplice per gli utenti esterni:</span><span class="sxs-lookup"><span data-stu-id="ac5f2-227">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="ac5f2-228">Contribuisci al codice EF Core</span><span class="sxs-lookup"><span data-stu-id="ac5f2-228">Contribute to the EF Core code</span></span>
* <span data-ttu-id="ac5f2-229">Creare provider di database</span><span class="sxs-lookup"><span data-stu-id="ac5f2-229">Create database providers</span></span>
* <span data-ttu-id="ac5f2-230">Compila altre estensioni</span><span class="sxs-lookup"><span data-stu-id="ac5f2-230">Build other extensions</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="ac5f2-231">Documentazione di Microsoft. Data. sqlite</span><span class="sxs-lookup"><span data-stu-id="ac5f2-231">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="ac5f2-232">Documento principale: @bricelam</span><span class="sxs-lookup"><span data-stu-id="ac5f2-232">Lead documenter: @bricelam</span></span>

<span data-ttu-id="ac5f2-233">Rilevato da [#1675](https://github.com/aspnet/EntityFramework.Docs/issues/1675)</span><span class="sxs-lookup"><span data-stu-id="ac5f2-233">Tracked by [#1675](https://github.com/aspnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="ac5f2-234">Dimensioni T-Shirt: M</span><span class="sxs-lookup"><span data-stu-id="ac5f2-234">T-shirt size: M</span></span>

<span data-ttu-id="ac5f2-235">Stato: completato.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-235">Status: Completed.</span></span> <span data-ttu-id="ac5f2-236">La nuova documentazione è disponibile nel [sito Microsoft docs](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span><span class="sxs-lookup"><span data-stu-id="ac5f2-236">The new documentation is [live on the Microsoft docs site](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="ac5f2-237">Il team EF possiede anche il provider Microsoft. Data. sqlite ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-237">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="ac5f2-238">Si prevede di documentare completamente questo provider nell'ambito della versione 5,0.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-238">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="ac5f2-239">Documentazione generale</span><span class="sxs-lookup"><span data-stu-id="ac5f2-239">General documentation</span></span>

<span data-ttu-id="ac5f2-240">Documento principale: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="ac5f2-240">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="ac5f2-241">Rilevato da [problemi nel repository docs nell'attività cardine 5,0](https://github.com/aspnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="ac5f2-241">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/aspnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="ac5f2-242">Dimensioni della maglietta: L</span><span class="sxs-lookup"><span data-stu-id="ac5f2-242">T-shirt size: L</span></span>

<span data-ttu-id="ac5f2-243">Stato: in corso</span><span class="sxs-lookup"><span data-stu-id="ac5f2-243">Status: In-progress</span></span>

<span data-ttu-id="ac5f2-244">È già in corso l'aggiornamento della documentazione per le versioni 3,0 e 3,1.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-244">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="ac5f2-245">Stiamo lavorando anche:</span><span class="sxs-lookup"><span data-stu-id="ac5f2-245">We are also working on:</span></span>
  * <span data-ttu-id="ac5f2-246">Revisione dei documenti introduttivi per renderli più accessibili o più facili da seguire</span><span class="sxs-lookup"><span data-stu-id="ac5f2-246">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
  * <span data-ttu-id="ac5f2-247">Riorganizzazione dei documenti per semplificare la ricerca e l'aggiunta di riferimenti incrociati</span><span class="sxs-lookup"><span data-stu-id="ac5f2-247">Reorganization of docs to make things easier to find and to add cross-references</span></span>
  * <span data-ttu-id="ac5f2-248">Aggiunta di dettagli e chiarimenti ai documenti esistenti</span><span class="sxs-lookup"><span data-stu-id="ac5f2-248">Adding more details and clarifications to existing docs</span></span>
  * <span data-ttu-id="ac5f2-249">Aggiornamento degli esempi e aggiunta di altri esempi</span><span class="sxs-lookup"><span data-stu-id="ac5f2-249">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="ac5f2-250">Correzione dei bug</span><span class="sxs-lookup"><span data-stu-id="ac5f2-250">Fixing bugs</span></span>

<span data-ttu-id="ac5f2-251">Rilevato da [problemi contrassegnati con `type-bug` nell'attività cardine 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span><span class="sxs-lookup"><span data-stu-id="ac5f2-251">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="ac5f2-252">Sviluppatori: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="ac5f2-252">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="ac5f2-253">Dimensioni della maglietta: L</span><span class="sxs-lookup"><span data-stu-id="ac5f2-253">T-shirt size: L</span></span>

<span data-ttu-id="ac5f2-254">Stato: in corso</span><span class="sxs-lookup"><span data-stu-id="ac5f2-254">Status: In-progress</span></span>

<span data-ttu-id="ac5f2-255">Al momento della stesura di questo articolo, sono stati risolti 135 bug nella versione 5,0 (con 62 già corretti), ma si è verificata una sovrapposizione significativa con la sezione relativa ai _miglioramenti generali delle query_ precedente.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-255">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="ac5f2-256">La velocità in ingresso (problemi che finiscono come lavoro in un'attività cardine) è stata di circa 23 problemi al mese nel corso della versione 3,0.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-256">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="ac5f2-257">Non tutti questi dovranno essere corretti in 5,0.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-257">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="ac5f2-258">Come stima approssimativa si prevede di correggere altri 150 problemi nell'intervallo di tempo 5,0.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-258">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="ac5f2-259">Piccoli miglioramenti</span><span class="sxs-lookup"><span data-stu-id="ac5f2-259">Small enhancements</span></span>

<span data-ttu-id="ac5f2-260">Rilevato da [problemi contrassegnati con `type-enhancement` nell'attività cardine 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span><span class="sxs-lookup"><span data-stu-id="ac5f2-260">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="ac5f2-261">Sviluppatori: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="ac5f2-261">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="ac5f2-262">Dimensioni della maglietta: L</span><span class="sxs-lookup"><span data-stu-id="ac5f2-262">T-shirt size: L</span></span>

<span data-ttu-id="ac5f2-263">Stato: in corso</span><span class="sxs-lookup"><span data-stu-id="ac5f2-263">Status: In-progress</span></span>

<span data-ttu-id="ac5f2-264">Oltre alle funzionalità più importanti descritte in precedenza, sono stati riportati anche numerosi miglioramenti più piccoli pianificati per 5,0 per correggere i "tagli di carta".</span><span class="sxs-lookup"><span data-stu-id="ac5f2-264">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="ac5f2-265">Si noti che molti di questi miglioramenti sono trattati anche dai temi più generali illustrati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-265">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="ac5f2-266">Sotto la riga</span><span class="sxs-lookup"><span data-stu-id="ac5f2-266">Below-the-line</span></span>

<span data-ttu-id="ac5f2-267">Rilevato da [problemi contrassegnati con `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span><span class="sxs-lookup"><span data-stu-id="ac5f2-267">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="ac5f2-268">Si tratta di correzioni di bug e miglioramenti che **non** sono attualmente pianificati per la versione 5,0, ma verranno esaminati come obiettivi di estensione a seconda dello stato di avanzamento del lavoro precedente.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-268">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="ac5f2-269">Inoltre, consideriamo sempre i [problemi più votati](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) durante la pianificazione.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-269">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="ac5f2-270">Il taglio di uno qualsiasi di questi problemi da un rilascio è sempre penoso, ma è necessario un piano realistico per le risorse disponibili.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-270">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="feedback"></a><span data-ttu-id="ac5f2-271">Feedback</span><span class="sxs-lookup"><span data-stu-id="ac5f2-271">Feedback</span></span>

<span data-ttu-id="ac5f2-272">Il feedback sulla pianificazione è importante.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-272">Your feedback on planning is important.</span></span> <span data-ttu-id="ac5f2-273">Il modo migliore per indicare l'importanza di un problema consiste nel votare (Thumb) per il problema in GitHub.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-273">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="ac5f2-274">Questi dati vengono quindi inseriti nel [processo di pianificazione](../release-planning.md) per la versione successiva.</span><span class="sxs-lookup"><span data-stu-id="ac5f2-274">This data will then feed into the [planning process](../release-planning.md) for the next release.</span></span>
