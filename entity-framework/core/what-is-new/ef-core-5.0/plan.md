---
title: Pianificare Entity Framework Core 5,0
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: c5b7300c61c2f668b6f9393ae51bf9ebddf330a7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417876"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="38c9c-102">Pianificare Entity Framework Core 5,0</span><span class="sxs-lookup"><span data-stu-id="38c9c-102">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="38c9c-103">Come descritto nel [processo di pianificazione](../release-planning.md), l'input è stato raccolto dagli stakeholder in un piano provvisorio per la versione di EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="38c9c-103">As described in the [planning process](../release-planning.md), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="38c9c-104">Questo piano è ancora in fase di elaborazione.</span><span class="sxs-lookup"><span data-stu-id="38c9c-104">This plan is still a work-in-progress.</span></span> <span data-ttu-id="38c9c-105">Niente qui è un impegno.</span><span class="sxs-lookup"><span data-stu-id="38c9c-105">Nothing here is a commitment.</span></span> <span data-ttu-id="38c9c-106">Questo piano è un punto di partenza che si evolverà Man mano che si apprenderanno altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="38c9c-106">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="38c9c-107">È possibile che venga effettuato il pull di alcuni elementi attualmente non pianificati per 5,0.</span><span class="sxs-lookup"><span data-stu-id="38c9c-107">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="38c9c-108">È possibile che vengano apportate alcune operazioni attualmente pianificate per 5,0.</span><span class="sxs-lookup"><span data-stu-id="38c9c-108">Some things currently planned for 5.0 may get punted out.</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="38c9c-109">Numero di versione e data di rilascio.</span><span class="sxs-lookup"><span data-stu-id="38c9c-109">Version number and release date.</span></span>

<span data-ttu-id="38c9c-110">EF Core 5,0 è attualmente pianificata per la versione allo [stesso tempo di .net 5,0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="38c9c-110">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="38c9c-111">È stata scelta la versione "5,0" per l'allineamento con .NET 5,0.</span><span class="sxs-lookup"><span data-stu-id="38c9c-111">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="38c9c-112">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="38c9c-112">Supported platforms</span></span>

<span data-ttu-id="38c9c-113">EF Core 5,0 è pianificato per essere eseguito su qualsiasi piattaforma .NET 5,0 in base alla [convergenza di queste piattaforme con .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="38c9c-113">EF Core 5.0 is planned to run on any .NET 5.0 platform based on the [convergence of these platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="38c9c-114">Ciò significa che in termini di .NET Standard e il TFM effettivo usato è ancora da definire.</span><span class="sxs-lookup"><span data-stu-id="38c9c-114">What this means in terms of .NET Standard and the actual TFM used is still TBD.</span></span>

<span data-ttu-id="38c9c-115">EF Core 5,0 non viene eseguito su .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="38c9c-115">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="38c9c-116">Modifiche di rilievo</span><span class="sxs-lookup"><span data-stu-id="38c9c-116">Breaking changes</span></span>

<span data-ttu-id="38c9c-117">EF Core 5,0 conterrà alcune modifiche di rilievo, ma saranno molto meno gravi rispetto a quanto accadeva per EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="38c9c-117">EF Core 5.0 will contain some breaking changes, but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="38c9c-118">Il nostro obiettivo è consentire l'aggiornamento della maggior parte delle applicazioni senza interruzioni.</span><span class="sxs-lookup"><span data-stu-id="38c9c-118">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="38c9c-119">Si prevede che verranno apportate alcune modifiche di rilievo per i provider di database, in particolare per quanto riguarda il supporto di TPT.</span><span class="sxs-lookup"><span data-stu-id="38c9c-119">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="38c9c-120">Tuttavia, si prevede che il lavoro per aggiornare un provider per 5,0 sarà inferiore a quello necessario per l'aggiornamento a 3,0.</span><span class="sxs-lookup"><span data-stu-id="38c9c-120">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="38c9c-121">Themes</span><span class="sxs-lookup"><span data-stu-id="38c9c-121">Themes</span></span>

<span data-ttu-id="38c9c-122">Sono state estratte alcune aree o temi principali che costituiranno la base per gli investimenti di grandi dimensioni in EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="38c9c-122">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="38c9c-123">Proprietà di navigazione molti-a-molti (a. k. a "Ignora navigazione")</span><span class="sxs-lookup"><span data-stu-id="38c9c-123">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="38c9c-124">Sviluppatori leader: @smitpatel e @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="38c9c-124">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="38c9c-125">Rilevato da [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span><span class="sxs-lookup"><span data-stu-id="38c9c-125">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="38c9c-126">Dimensioni della maglietta: L</span><span class="sxs-lookup"><span data-stu-id="38c9c-126">T-shirt size: L</span></span>

<span data-ttu-id="38c9c-127">Stato: in corso</span><span class="sxs-lookup"><span data-stu-id="38c9c-127">Status: In-progress</span></span>

<span data-ttu-id="38c9c-128">Many-to-many è la [funzionalità più richiesta](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~ 407 voti) nel backlog di GitHub.</span><span class="sxs-lookup"><span data-stu-id="38c9c-128">Many-to-many is the [most requested feature](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~407 votes) on the GitHub backlog.</span></span>

<span data-ttu-id="38c9c-129">Il supporto per le relazioni molti-a-molti nell'intero viene registrato come [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508).</span><span class="sxs-lookup"><span data-stu-id="38c9c-129">Support for many-to-many relationships in their entirety is tracked as [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508).</span></span> <span data-ttu-id="38c9c-130">Questo può essere suddiviso in tre aree principali:</span><span class="sxs-lookup"><span data-stu-id="38c9c-130">This can be broken down into three major areas:</span></span>

* <span data-ttu-id="38c9c-131">Ignorare le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="38c9c-131">Skip navigation properties.</span></span> <span data-ttu-id="38c9c-132">Che consentono di utilizzare il modello per le query e così via, senza riferimento all'entità sottostante della tabella di join.</span><span class="sxs-lookup"><span data-stu-id="38c9c-132">These allow the model to be used for queries, etc. without reference to the underlying join table entity.</span></span> <span data-ttu-id="38c9c-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span><span class="sxs-lookup"><span data-stu-id="38c9c-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span></span>
* <span data-ttu-id="38c9c-134">Tipi di entità del contenitore delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="38c9c-134">Property-bag entity types.</span></span> <span data-ttu-id="38c9c-135">Che consentono di usare un tipo CLR standard (ad esempio `Dictionary`) per le istanze di entità in modo che non sia necessario un tipo CLR esplicito per ogni tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="38c9c-135">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span> <span data-ttu-id="38c9c-136">(Estendi per 5,0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span><span class="sxs-lookup"><span data-stu-id="38c9c-136">(Stretch for 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span></span>
* <span data-ttu-id="38c9c-137">Sugar per semplificare la configurazione di relazioni molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="38c9c-137">Sugar for easy configuration of many-to-many relationships.</span></span> <span data-ttu-id="38c9c-138">(Estendi per 5,0).</span><span class="sxs-lookup"><span data-stu-id="38c9c-138">(Stretch for 5.0.)</span></span>

<span data-ttu-id="38c9c-139">Il blocco più significativo per coloro che desiderano il supporto molti-a-molti non è in grado di utilizzare le relazioni "naturali", senza fare riferimento alla tabella di join, nella logica di business, ad esempio le query.</span><span class="sxs-lookup"><span data-stu-id="38c9c-139">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span> <span data-ttu-id="38c9c-140">Il tipo di entità tabella di join può ancora esistere, ma non dovrebbe essere in grado di ottenere la logica di business.</span><span class="sxs-lookup"><span data-stu-id="38c9c-140">The join table entity type may still exist, but it should not get in the way of business logic.</span></span> <span data-ttu-id="38c9c-141">Questo è il motivo per cui abbiamo scelto di affrontare le proprietà di navigazione Skip per 5,0.</span><span class="sxs-lookup"><span data-stu-id="38c9c-141">This is why we have chosen to tackle skip navigation properties for 5.0.</span></span>

<span data-ttu-id="38c9c-142">In questo momento le altre parti di molti-a-molti vengono seguite come obiettivo di estensione per EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="38c9c-142">At this time the other parts of many-to-many are being pursued as a stretch goal for EF Core 5.0.</span></span> <span data-ttu-id="38c9c-143">Ciò significa che non sono attualmente nel piano per 5,0, ma se le cose vanno bene ci auguriamo di poterle inserire.</span><span class="sxs-lookup"><span data-stu-id="38c9c-143">This means they are not currently in the plan for 5.0, but if things go well we hope to pull them in.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="38c9c-144">Mapping di ereditarietà tabella per tipo (TPT)</span><span class="sxs-lookup"><span data-stu-id="38c9c-144">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="38c9c-145">Sviluppatore principale: @AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="38c9c-145">Lead developer: @AndriySvyryd</span></span>

<span data-ttu-id="38c9c-146">Rilevato da [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span><span class="sxs-lookup"><span data-stu-id="38c9c-146">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="38c9c-147">Dimensioni della maglietta: XL</span><span class="sxs-lookup"><span data-stu-id="38c9c-147">T-shirt size: XL</span></span>

<span data-ttu-id="38c9c-148">Stato: non avviato</span><span class="sxs-lookup"><span data-stu-id="38c9c-148">Status: Not started</span></span>

<span data-ttu-id="38c9c-149">Stiamo eseguendo TPT perché si tratta di una funzionalità estremamente richiesta (~ 254 voti, 3 in generale) e perché richiede alcune modifiche di basso livello che si ritiene siano appropriate per la natura fondamentale del piano generale .NET 5.</span><span class="sxs-lookup"><span data-stu-id="38c9c-149">We're doing TPT because it is both a highly requested feature (~254 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="38c9c-150">Si prevede che questo provocherà modifiche di rilievo per i provider di database, anche se queste devono essere molto meno gravi rispetto alle modifiche necessarie per 3,0.</span><span class="sxs-lookup"><span data-stu-id="38c9c-150">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="38c9c-151">Inclusione filtrato</span><span class="sxs-lookup"><span data-stu-id="38c9c-151">Filtered Include</span></span>

<span data-ttu-id="38c9c-152">Sviluppatore principale: @maumar</span><span class="sxs-lookup"><span data-stu-id="38c9c-152">Lead developer: @maumar</span></span>

<span data-ttu-id="38c9c-153">Rilevato da [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span><span class="sxs-lookup"><span data-stu-id="38c9c-153">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="38c9c-154">Dimensioni T-Shirt: M</span><span class="sxs-lookup"><span data-stu-id="38c9c-154">T-shirt size: M</span></span>

<span data-ttu-id="38c9c-155">Stato: non avviato</span><span class="sxs-lookup"><span data-stu-id="38c9c-155">Status: Not started</span></span>

<span data-ttu-id="38c9c-156">L'inclusione filtrata è una funzionalità molto richiesta (~ 317 voti, 2 nel complesso) che non è una grande quantità di lavoro e che riteniamo che lo sblocco o renderà più semplice molti scenari che attualmente richiedono filtri a livello di modello o query più complesse.</span><span class="sxs-lookup"><span data-stu-id="38c9c-156">Filtered Include is a highly-requested feature (~317 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="38c9c-157">Razionalizzare ToTable, ToQuery, toview, dati da tabelle e così via.</span><span class="sxs-lookup"><span data-stu-id="38c9c-157">Rationalize ToTable, ToQuery, ToView, FromSql, etc.</span></span>

<span data-ttu-id="38c9c-158">Sviluppatori leader: @maumar e @smitpatel</span><span class="sxs-lookup"><span data-stu-id="38c9c-158">Lead developers: @maumar and @smitpatel</span></span>

<span data-ttu-id="38c9c-159">Rilevato da [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span><span class="sxs-lookup"><span data-stu-id="38c9c-159">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="38c9c-160">Dimensioni della maglietta: L</span><span class="sxs-lookup"><span data-stu-id="38c9c-160">T-shirt size: L</span></span>

<span data-ttu-id="38c9c-161">Stato: non avviato</span><span class="sxs-lookup"><span data-stu-id="38c9c-161">Status: Not started</span></span>

<span data-ttu-id="38c9c-162">Sono stati apportati miglioramenti alle versioni precedenti per il supporto di SQL non elaborato, tipi autochiave e aree correlate.</span><span class="sxs-lookup"><span data-stu-id="38c9c-162">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="38c9c-163">Tuttavia, vi sono Gap e incoerenze nel modo in cui tutto funziona insieme nel suo complesso.</span><span class="sxs-lookup"><span data-stu-id="38c9c-163">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="38c9c-164">L'obiettivo di 5,0 è correggerli e creare un'esperienza ottimale per la definizione, la migrazione e l'utilizzo di tipi diversi di entità e delle query e degli elementi di database associati.</span><span class="sxs-lookup"><span data-stu-id="38c9c-164">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="38c9c-165">Questo può comportare anche aggiornamenti all'API di query compilata.</span><span class="sxs-lookup"><span data-stu-id="38c9c-165">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="38c9c-166">Si noti che questo elemento può comportare alcune modifiche sostanziali a livello di applicazione, poiché alcune delle funzionalità attualmente disponibili sono troppo permissive, in modo da consentire agli utenti di raggiungere rapidamente i pozzi di errore.</span><span class="sxs-lookup"><span data-stu-id="38c9c-166">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="38c9c-167">È probabile che si stiano bloccando alcune di queste funzionalità insieme a indicazioni sulle operazioni da eseguire.</span><span class="sxs-lookup"><span data-stu-id="38c9c-167">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="38c9c-168">Miglioramenti generali delle query</span><span class="sxs-lookup"><span data-stu-id="38c9c-168">General query enhancements</span></span>

<span data-ttu-id="38c9c-169">Sviluppatori leader: @smitpatel e @maumar</span><span class="sxs-lookup"><span data-stu-id="38c9c-169">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="38c9c-170">Rilevato da [problemi contrassegnati con `area-query` nell'attività cardine 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="38c9c-170">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="38c9c-171">Dimensioni della maglietta: XL</span><span class="sxs-lookup"><span data-stu-id="38c9c-171">T-shirt size: XL</span></span>

<span data-ttu-id="38c9c-172">Stato: in corso</span><span class="sxs-lookup"><span data-stu-id="38c9c-172">Status: In-progress</span></span>

<span data-ttu-id="38c9c-173">Il codice di traduzione query è stato riscritto in maniera estensiva per EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="38c9c-173">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="38c9c-174">Il codice di query si trova in genere in uno stato molto più solido a causa di questo problema.</span><span class="sxs-lookup"><span data-stu-id="38c9c-174">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="38c9c-175">Per 5,0 non è prevista la creazione di modifiche di query principali, al di fuori di quelle necessarie per supportare TPT e ignorare le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="38c9c-175">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="38c9c-176">Tuttavia, è ancora necessario un lavoro significativo per risolvere alcuni debiti tecnici lasciati dalla revisione 3,0.</span><span class="sxs-lookup"><span data-stu-id="38c9c-176">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="38c9c-177">Si prevede inoltre di correggere molti bug e implementare piccoli miglioramenti per migliorare ulteriormente l'esperienza complessiva della query.</span><span class="sxs-lookup"><span data-stu-id="38c9c-177">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="38c9c-178">Migrazioni ed esperienza di distribuzione</span><span class="sxs-lookup"><span data-stu-id="38c9c-178">Migrations and deployment experience</span></span>

<span data-ttu-id="38c9c-179">Sviluppatori leader: @bricelam</span><span class="sxs-lookup"><span data-stu-id="38c9c-179">Lead developers: @bricelam</span></span>

<span data-ttu-id="38c9c-180">Rilevato da [#19587](https://github.com/dotnet/efcore/issues/19587)</span><span class="sxs-lookup"><span data-stu-id="38c9c-180">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="38c9c-181">Dimensioni della maglietta: L</span><span class="sxs-lookup"><span data-stu-id="38c9c-181">T-shirt size: L</span></span>

<span data-ttu-id="38c9c-182">Stato: in corso</span><span class="sxs-lookup"><span data-stu-id="38c9c-182">Status: In-progress</span></span>

<span data-ttu-id="38c9c-183">Attualmente, molti sviluppatori migrano i database al momento dell'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="38c9c-183">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="38c9c-184">Questa operazione è semplice, ma non è consigliata perché:</span><span class="sxs-lookup"><span data-stu-id="38c9c-184">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="38c9c-185">Più thread/processi/server possono tentare di eseguire la migrazione simultanea del database</span><span class="sxs-lookup"><span data-stu-id="38c9c-185">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="38c9c-186">Le applicazioni potrebbero provare ad accedere allo stato incoerente durante l'operazione</span><span class="sxs-lookup"><span data-stu-id="38c9c-186">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="38c9c-187">In genere le autorizzazioni per il database per modificare lo schema non devono essere concesse per l'esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="38c9c-187">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="38c9c-188">È difficile ripristinare uno stato pulito se si verificano problemi</span><span class="sxs-lookup"><span data-stu-id="38c9c-188">It's hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="38c9c-189">Si vuole offrire un'esperienza migliore che consenta di eseguire la migrazione del database in fase di distribuzione in modo semplice.</span><span class="sxs-lookup"><span data-stu-id="38c9c-189">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="38c9c-190">Questa operazione dovrebbe:</span><span class="sxs-lookup"><span data-stu-id="38c9c-190">This should:</span></span>

* <span data-ttu-id="38c9c-191">Lavorare su Linux, Mac e Windows</span><span class="sxs-lookup"><span data-stu-id="38c9c-191">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="38c9c-192">Esperienza ottimale nella riga di comando</span><span class="sxs-lookup"><span data-stu-id="38c9c-192">Be a good experience on the command line</span></span>
* <span data-ttu-id="38c9c-193">Supportare scenari con i contenitori</span><span class="sxs-lookup"><span data-stu-id="38c9c-193">Support scenarios with containers</span></span>
* <span data-ttu-id="38c9c-194">Usare gli strumenti e i flussi di distribuzione reali usati di frequente</span><span class="sxs-lookup"><span data-stu-id="38c9c-194">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="38c9c-195">Integrazione in almeno Visual Studio</span><span class="sxs-lookup"><span data-stu-id="38c9c-195">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="38c9c-196">Il risultato è probabilmente costituito da numerosi miglioramenti di EF Core (ad esempio, migliori migrazioni in SQLite), insieme a linee guida e collaborazioni a lungo termine con altri team per migliorare le esperienze end-to-end che vanno oltre il solo EF.</span><span class="sxs-lookup"><span data-stu-id="38c9c-196">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="38c9c-197">Esperienza delle piattaforme EF Core</span><span class="sxs-lookup"><span data-stu-id="38c9c-197">EF Core platforms experience</span></span> 

<span data-ttu-id="38c9c-198">Sviluppatori leader: @roji e @bricelam</span><span class="sxs-lookup"><span data-stu-id="38c9c-198">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="38c9c-199">Rilevato da [#19588](https://github.com/dotnet/efcore/issues/19588)</span><span class="sxs-lookup"><span data-stu-id="38c9c-199">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="38c9c-200">Dimensioni della maglietta: L</span><span class="sxs-lookup"><span data-stu-id="38c9c-200">T-shirt size: L</span></span>

<span data-ttu-id="38c9c-201">Stato: non avviato</span><span class="sxs-lookup"><span data-stu-id="38c9c-201">Status: Not started</span></span>

<span data-ttu-id="38c9c-202">Sono disponibili indicazioni utili per l'uso di EF Core in applicazioni Web tradizionali di tipo MVC.</span><span class="sxs-lookup"><span data-stu-id="38c9c-202">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="38c9c-203">Linee guida per altre piattaforme e modelli di applicazione mancanti o non aggiornati.</span><span class="sxs-lookup"><span data-stu-id="38c9c-203">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="38c9c-204">Per EF Core 5,0, si prevede di analizzare, migliorare e documentare l'esperienza di utilizzo di EF Core con:</span><span class="sxs-lookup"><span data-stu-id="38c9c-204">For EF Core 5.0, we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="38c9c-205">Blazor</span><span class="sxs-lookup"><span data-stu-id="38c9c-205">Blazor</span></span>
* <span data-ttu-id="38c9c-206">Novell, incluso l'uso della storia AOT/linker</span><span class="sxs-lookup"><span data-stu-id="38c9c-206">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="38c9c-207">WinForms/WPF/WinUI e possibilmente altri U.I.</span><span class="sxs-lookup"><span data-stu-id="38c9c-207">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="38c9c-208">frameworks</span><span class="sxs-lookup"><span data-stu-id="38c9c-208">frameworks</span></span>

<span data-ttu-id="38c9c-209">È probabile che si verifichino molti piccoli miglioramenti in EF Core, insieme a linee guida e collaborazioni a lungo termine con altri team per migliorare le esperienze end-to-end che vanno oltre la semplice EF.</span><span class="sxs-lookup"><span data-stu-id="38c9c-209">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="38c9c-210">Le aree specifiche da esaminare sono:</span><span class="sxs-lookup"><span data-stu-id="38c9c-210">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="38c9c-211">Distribuzione, inclusa l'esperienza di utilizzo degli strumenti EF, ad esempio per le migrazioni</span><span class="sxs-lookup"><span data-stu-id="38c9c-211">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="38c9c-212">Modelli di applicazione, inclusi Novell e blazer, e probabilmente altri</span><span class="sxs-lookup"><span data-stu-id="38c9c-212">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="38c9c-213">Esperienze di SQLite, tra cui l'esperienza spaziale e le ricompilazioni di tabelle</span><span class="sxs-lookup"><span data-stu-id="38c9c-213">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="38c9c-214">Esperienza AOT e collegamento</span><span class="sxs-lookup"><span data-stu-id="38c9c-214">AOT and linking experiences</span></span>
* <span data-ttu-id="38c9c-215">Integrazione di diagnostica, inclusi i contatori delle prestazioni</span><span class="sxs-lookup"><span data-stu-id="38c9c-215">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="38c9c-216">Prestazioni</span><span class="sxs-lookup"><span data-stu-id="38c9c-216">Performance</span></span>

<span data-ttu-id="38c9c-217">Sviluppatore principale: @roji</span><span class="sxs-lookup"><span data-stu-id="38c9c-217">Lead developer: @roji</span></span>

<span data-ttu-id="38c9c-218">Rilevato da [problemi contrassegnati con `area-perf` nell'attività cardine 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="38c9c-218">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="38c9c-219">Dimensioni della maglietta: L</span><span class="sxs-lookup"><span data-stu-id="38c9c-219">T-shirt size: L</span></span>

<span data-ttu-id="38c9c-220">Stato: in corso</span><span class="sxs-lookup"><span data-stu-id="38c9c-220">Status: In-progress</span></span>

<span data-ttu-id="38c9c-221">Per EF Core, si prevede di migliorare la nostra suite di benchmark delle prestazioni e di apportare miglioramenti al runtime.</span><span class="sxs-lookup"><span data-stu-id="38c9c-221">For EF Core, we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="38c9c-222">Si prevede inoltre di completare la nuova API batch ADO.NET, che è stata prototipata durante il ciclo di rilascio 3,0.</span><span class="sxs-lookup"><span data-stu-id="38c9c-222">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="38c9c-223">Inoltre, a livello di ADO.NET, vengono pianificati miglioramenti delle prestazioni aggiuntivi per il provider npgsql.</span><span class="sxs-lookup"><span data-stu-id="38c9c-223">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="38c9c-224">Nell'ambito di questo lavoro si prevede anche di aggiungere i contatori delle prestazioni di ADO.NET/EF core e altri dati di diagnostica in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="38c9c-224">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="38c9c-225">Documentazione dell'architettura/collaboratore</span><span class="sxs-lookup"><span data-stu-id="38c9c-225">Architectural/contributor documentation</span></span>

<span data-ttu-id="38c9c-226">Documento principale: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="38c9c-226">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="38c9c-227">Rilevato da [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span><span class="sxs-lookup"><span data-stu-id="38c9c-227">Tracked by [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="38c9c-228">Dimensioni della maglietta: L</span><span class="sxs-lookup"><span data-stu-id="38c9c-228">T-shirt size: L</span></span>

<span data-ttu-id="38c9c-229">Stato: non avviato</span><span class="sxs-lookup"><span data-stu-id="38c9c-229">Status: Not started</span></span>

<span data-ttu-id="38c9c-230">L'idea è quella di semplificare la comprensione di ciò che accade negli elementi interni del EF Core.</span><span class="sxs-lookup"><span data-stu-id="38c9c-230">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="38c9c-231">Questa operazione può essere utile per chiunque usi EF Core, ma la motivazione principale consiste nel rendere più semplice per gli utenti esterni:</span><span class="sxs-lookup"><span data-stu-id="38c9c-231">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="38c9c-232">Contribuisci al codice EF Core</span><span class="sxs-lookup"><span data-stu-id="38c9c-232">Contribute to the EF Core code</span></span>
* <span data-ttu-id="38c9c-233">Creare provider di database</span><span class="sxs-lookup"><span data-stu-id="38c9c-233">Create database providers</span></span>
* <span data-ttu-id="38c9c-234">Compila altre estensioni</span><span class="sxs-lookup"><span data-stu-id="38c9c-234">Build other extensions</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="38c9c-235">Documentazione di Microsoft. Data. sqlite</span><span class="sxs-lookup"><span data-stu-id="38c9c-235">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="38c9c-236">Documento principale: @bricelam</span><span class="sxs-lookup"><span data-stu-id="38c9c-236">Lead documenter: @bricelam</span></span>

<span data-ttu-id="38c9c-237">Rilevato da [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span><span class="sxs-lookup"><span data-stu-id="38c9c-237">Tracked by [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="38c9c-238">Dimensioni T-Shirt: M</span><span class="sxs-lookup"><span data-stu-id="38c9c-238">T-shirt size: M</span></span>

<span data-ttu-id="38c9c-239">Stato: completato.</span><span class="sxs-lookup"><span data-stu-id="38c9c-239">Status: Completed.</span></span> <span data-ttu-id="38c9c-240">La nuova documentazione è disponibile nel [sito Microsoft docs](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span><span class="sxs-lookup"><span data-stu-id="38c9c-240">The new documentation is [live on the Microsoft docs site](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="38c9c-241">Il team EF possiede anche il provider Microsoft. Data. sqlite ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="38c9c-241">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="38c9c-242">Si prevede di documentare completamente questo provider nell'ambito della versione 5,0.</span><span class="sxs-lookup"><span data-stu-id="38c9c-242">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="38c9c-243">Documentazione generale</span><span class="sxs-lookup"><span data-stu-id="38c9c-243">General documentation</span></span>

<span data-ttu-id="38c9c-244">Documento principale: @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="38c9c-244">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="38c9c-245">Rilevato da [problemi nel repository docs nell'attività cardine 5,0](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="38c9c-245">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="38c9c-246">Dimensioni della maglietta: L</span><span class="sxs-lookup"><span data-stu-id="38c9c-246">T-shirt size: L</span></span>

<span data-ttu-id="38c9c-247">Stato: in corso</span><span class="sxs-lookup"><span data-stu-id="38c9c-247">Status: In-progress</span></span>

<span data-ttu-id="38c9c-248">È già in corso l'aggiornamento della documentazione per le versioni 3,0 e 3,1.</span><span class="sxs-lookup"><span data-stu-id="38c9c-248">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="38c9c-249">Stiamo lavorando anche:</span><span class="sxs-lookup"><span data-stu-id="38c9c-249">We are also working on:</span></span>
  * <span data-ttu-id="38c9c-250">Revisione dei documenti introduttivi per renderli più accessibili o più facili da seguire</span><span class="sxs-lookup"><span data-stu-id="38c9c-250">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
  * <span data-ttu-id="38c9c-251">Riorganizzazione dei documenti per semplificare la ricerca e l'aggiunta di riferimenti incrociati</span><span class="sxs-lookup"><span data-stu-id="38c9c-251">Reorganization of docs to make things easier to find and to add cross-references</span></span>
  * <span data-ttu-id="38c9c-252">Aggiunta di dettagli e chiarimenti ai documenti esistenti</span><span class="sxs-lookup"><span data-stu-id="38c9c-252">Adding more details and clarifications to existing docs</span></span>
  * <span data-ttu-id="38c9c-253">Aggiornamento degli esempi e aggiunta di altri esempi</span><span class="sxs-lookup"><span data-stu-id="38c9c-253">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="38c9c-254">Correzione dei bug</span><span class="sxs-lookup"><span data-stu-id="38c9c-254">Fixing bugs</span></span>

<span data-ttu-id="38c9c-255">Rilevato da [problemi contrassegnati con `type-bug` nell'attività cardine 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span><span class="sxs-lookup"><span data-stu-id="38c9c-255">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="38c9c-256">Sviluppatori: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="38c9c-256">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="38c9c-257">Dimensioni della maglietta: L</span><span class="sxs-lookup"><span data-stu-id="38c9c-257">T-shirt size: L</span></span>

<span data-ttu-id="38c9c-258">Stato: in corso</span><span class="sxs-lookup"><span data-stu-id="38c9c-258">Status: In-progress</span></span>

<span data-ttu-id="38c9c-259">Al momento della stesura di questo articolo, sono stati risolti 135 bug nella versione 5,0 (con 62 già corretti), ma si è verificata una sovrapposizione significativa con la sezione relativa ai _miglioramenti generali delle query_ precedente.</span><span class="sxs-lookup"><span data-stu-id="38c9c-259">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="38c9c-260">La velocità in ingresso (problemi che finiscono come lavoro in un'attività cardine) è stata di circa 23 problemi al mese nel corso della versione 3,0.</span><span class="sxs-lookup"><span data-stu-id="38c9c-260">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="38c9c-261">Non tutti questi dovranno essere corretti in 5,0.</span><span class="sxs-lookup"><span data-stu-id="38c9c-261">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="38c9c-262">Come stima approssimativa si prevede di correggere altri 150 problemi nell'intervallo di tempo 5,0.</span><span class="sxs-lookup"><span data-stu-id="38c9c-262">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="38c9c-263">Piccoli miglioramenti</span><span class="sxs-lookup"><span data-stu-id="38c9c-263">Small enhancements</span></span>

<span data-ttu-id="38c9c-264">Rilevato da [problemi contrassegnati con `type-enhancement` nell'attività cardine 5,0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span><span class="sxs-lookup"><span data-stu-id="38c9c-264">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="38c9c-265">Sviluppatori: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span><span class="sxs-lookup"><span data-stu-id="38c9c-265">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="38c9c-266">Dimensioni della maglietta: L</span><span class="sxs-lookup"><span data-stu-id="38c9c-266">T-shirt size: L</span></span>

<span data-ttu-id="38c9c-267">Stato: in corso</span><span class="sxs-lookup"><span data-stu-id="38c9c-267">Status: In-progress</span></span>

<span data-ttu-id="38c9c-268">Oltre alle funzionalità più importanti descritte in precedenza, sono stati riportati anche numerosi miglioramenti più piccoli pianificati per 5,0 per correggere i "tagli di carta".</span><span class="sxs-lookup"><span data-stu-id="38c9c-268">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="38c9c-269">Si noti che molti di questi miglioramenti sono trattati anche dai temi più generali illustrati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="38c9c-269">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="38c9c-270">Sotto la riga</span><span class="sxs-lookup"><span data-stu-id="38c9c-270">Below-the-line</span></span>

<span data-ttu-id="38c9c-271">Rilevato da [problemi contrassegnati con `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span><span class="sxs-lookup"><span data-stu-id="38c9c-271">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="38c9c-272">Si tratta di correzioni di bug e miglioramenti che **non** sono attualmente pianificati per la versione 5,0, ma verranno esaminati come obiettivi di estensione a seconda dello stato di avanzamento del lavoro precedente.</span><span class="sxs-lookup"><span data-stu-id="38c9c-272">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="38c9c-273">Inoltre, consideriamo sempre i [problemi più votati](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) durante la pianificazione.</span><span class="sxs-lookup"><span data-stu-id="38c9c-273">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="38c9c-274">Il taglio di uno qualsiasi di questi problemi da un rilascio è sempre penoso, ma è necessario un piano realistico per le risorse disponibili.</span><span class="sxs-lookup"><span data-stu-id="38c9c-274">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="feedback"></a><span data-ttu-id="38c9c-275">Commenti e suggerimenti</span><span class="sxs-lookup"><span data-stu-id="38c9c-275">Feedback</span></span>

<span data-ttu-id="38c9c-276">I commenti e i suggerimenti dei clienti sulla pianificazione sono importanti.</span><span class="sxs-lookup"><span data-stu-id="38c9c-276">Your feedback on planning is important.</span></span> <span data-ttu-id="38c9c-277">Il modo migliore per indicare l'importanza di un problema consiste nel votare (Pollice in su) per tale problema in GitHub.</span><span class="sxs-lookup"><span data-stu-id="38c9c-277">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="38c9c-278">Questi dati vengono quindi inseriti nel [processo di pianificazione](../release-planning.md) per la versione successiva.</span><span class="sxs-lookup"><span data-stu-id="38c9c-278">This data will then feed into the [planning process](../release-planning.md) for the next release.</span></span>
