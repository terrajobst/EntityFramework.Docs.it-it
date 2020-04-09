---
title: Pianificare Entity Framework Core 5.0
author: ajcvickers
ms.date: 01/14/2020
uid: core/what-is-new/ef-core-5.0/plan.md
ms.openlocfilehash: 8b4ca32524869019c04d5a4d4d55967f68181cd7
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136228"
---
# <a name="plan-for-entity-framework-core-50"></a><span data-ttu-id="4d274-102">Pianificare Entity Framework Core 5.0</span><span class="sxs-lookup"><span data-stu-id="4d274-102">Plan for Entity Framework Core 5.0</span></span>

<span data-ttu-id="4d274-103">Come descritto nel processo di [pianificazione](../release-planning.md), abbiamo raccolto input dalle parti interessate in un piano provvisorio per la versione di EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="4d274-103">As described in the [planning process](../release-planning.md), we have gathered input from stakeholders into a tentative plan for the EF Core 5.0 release.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="4d274-104">Questo piano è ancora in corso.</span><span class="sxs-lookup"><span data-stu-id="4d274-104">This plan is still a work-in-progress.</span></span> <span data-ttu-id="4d274-105">Niente qui è un impegno.</span><span class="sxs-lookup"><span data-stu-id="4d274-105">Nothing here is a commitment.</span></span> <span data-ttu-id="4d274-106">Questo piano è un punto di partenza che si evolverà man mano che impariamo di più.</span><span class="sxs-lookup"><span data-stu-id="4d274-106">This plan is a starting point that will evolve as we learn more.</span></span> <span data-ttu-id="4d274-107">Alcune cose attualmente non previste per 5.0 possono essere tirate dentro.</span><span class="sxs-lookup"><span data-stu-id="4d274-107">Some things not currently planned for 5.0 may get pulled in.</span></span> <span data-ttu-id="4d274-108">Alcune cose attualmente pianificate per 5.0 potrebbero essere spuntate.</span><span class="sxs-lookup"><span data-stu-id="4d274-108">Some things currently planned for 5.0 may get punted out.</span></span>

### <a name="version-number-and-release-date"></a><span data-ttu-id="4d274-109">Numero di versione e data di rilascio.</span><span class="sxs-lookup"><span data-stu-id="4d274-109">Version number and release date.</span></span>

<span data-ttu-id="4d274-110">EF Core 5.0 è attualmente pianificato per il rilascio [contemporaneamente a .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="4d274-110">EF Core 5.0 is currently scheduled for release at [the same time as .NET 5.0](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="4d274-111">La versione "5.0" è stata scelta per l'allineamento con .NET 5.0.</span><span class="sxs-lookup"><span data-stu-id="4d274-111">The version "5.0" was chosen to align with .NET 5.0.</span></span>

### <a name="supported-platforms"></a><span data-ttu-id="4d274-112">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="4d274-112">Supported platforms</span></span>

<span data-ttu-id="4d274-113">EF Core 5.0 è pianificato per l'esecuzione su qualsiasi piattaforma .NET 5.0 basata sulla [convergenza di queste piattaforme in .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span><span class="sxs-lookup"><span data-stu-id="4d274-113">EF Core 5.0 is planned to run on any .NET 5.0 platform based on the [convergence of these platforms to .NET Core](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="4d274-114">Ciò significa in termini di .NET Standard e il TFM effettivo utilizzato è ancora TBD.</span><span class="sxs-lookup"><span data-stu-id="4d274-114">What this means in terms of .NET Standard and the actual TFM used is still TBD.</span></span>

<span data-ttu-id="4d274-115">EF Core 5.0 non verrà eseguito in .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="4d274-115">EF Core 5.0 will not run on .NET Framework.</span></span>

### <a name="breaking-changes"></a><span data-ttu-id="4d274-116">Modifiche di rilievo</span><span class="sxs-lookup"><span data-stu-id="4d274-116">Breaking changes</span></span>

<span data-ttu-id="4d274-117">EF Core 5.0 conterrà alcune modifiche di rilievo, ma questi saranno molto meno gravi rispetto a EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="4d274-117">EF Core 5.0 will contain some breaking changes, but these will be much less severe than was the case for EF Core 3.0.</span></span> <span data-ttu-id="4d274-118">Il nostro obiettivo è consentire alla maggior parte delle applicazioni di aggiornare senza rompersi.</span><span class="sxs-lookup"><span data-stu-id="4d274-118">Our goal is to allow the vast majority of applications to update without breaking.</span></span>

<span data-ttu-id="4d274-119">Si prevede che ci saranno alcune modifiche di rilievo per i fornitori di database, soprattutto intorno al supporto TPT.</span><span class="sxs-lookup"><span data-stu-id="4d274-119">It is expected that there will be some breaking changes for database providers, especially around TPT support.</span></span> <span data-ttu-id="4d274-120">Tuttavia, prevediamo che il lavoro per aggiornare un provider per 5.0 sarà inferiore a quello richiesto per l'aggiornamento per 3.0.</span><span class="sxs-lookup"><span data-stu-id="4d274-120">However, we expect the work to update a provider for 5.0 will be less than was required to update for 3.0.</span></span>

## <a name="themes"></a><span data-ttu-id="4d274-121">Themes</span><span class="sxs-lookup"><span data-stu-id="4d274-121">Themes</span></span>

<span data-ttu-id="4d274-122">Abbiamo estratto alcune aree o temi importanti che costituiranno la base per i grandi investimenti in EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="4d274-122">We have extracted a few major areas or themes which will form the basis for the large investments in EF Core 5.0.</span></span>

## <a name="many-to-many-navigation-properties-aka-skip-navigations"></a><span data-ttu-id="4d274-123">Proprietà di navigazione molti-a-molti (ovvero "ignora navigazione")</span><span class="sxs-lookup"><span data-stu-id="4d274-123">Many-to-many navigation properties (a.k.a "skip navigations")</span></span>

<span data-ttu-id="4d274-124">Sviluppatori principali: @smitpatel e@AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="4d274-124">Lead developers: @smitpatel and @AndriySvyryd</span></span>

<span data-ttu-id="4d274-125">Tracciato da [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span><span class="sxs-lookup"><span data-stu-id="4d274-125">Tracked by [#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003)</span></span>

<span data-ttu-id="4d274-126">Taglia t: L</span><span class="sxs-lookup"><span data-stu-id="4d274-126">T-shirt size: L</span></span>

<span data-ttu-id="4d274-127">Stato: In corso</span><span class="sxs-lookup"><span data-stu-id="4d274-127">Status: In-progress</span></span>

<span data-ttu-id="4d274-128">Molti-a-molti è la [funzionalità più richiesta](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (407 voti) nel backlog di GitHub.</span><span class="sxs-lookup"><span data-stu-id="4d274-128">Many-to-many is the [most requested feature](https://github.com/aspnet/EntityFrameworkCore/issues/1368) (~407 votes) on the GitHub backlog.</span></span>

<span data-ttu-id="4d274-129">Il supporto per le relazioni molti-a-molti nella loro interezza viene tracciato come [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508).</span><span class="sxs-lookup"><span data-stu-id="4d274-129">Support for many-to-many relationships in their entirety is tracked as [#10508](https://github.com/aspnet/EntityFrameworkCore/issues/10508).</span></span> <span data-ttu-id="4d274-130">Questo può essere suddiviso in tre aree principali:</span><span class="sxs-lookup"><span data-stu-id="4d274-130">This can be broken down into three major areas:</span></span>

* <span data-ttu-id="4d274-131">Ignorare le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="4d274-131">Skip navigation properties.</span></span> <span data-ttu-id="4d274-132">Questi consentono di utilizzare il modello per le query e così via senza riferimento all'entità della tabella di join sottostante.</span><span class="sxs-lookup"><span data-stu-id="4d274-132">These allow the model to be used for queries, etc. without reference to the underlying join table entity.</span></span> <span data-ttu-id="4d274-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span><span class="sxs-lookup"><span data-stu-id="4d274-133">([#19003](https://github.com/aspnet/EntityFrameworkCore/issues/19003))</span></span>
* <span data-ttu-id="4d274-134">Tipi di entità property-bag.</span><span class="sxs-lookup"><span data-stu-id="4d274-134">Property-bag entity types.</span></span> <span data-ttu-id="4d274-135">Questi consentono di utilizzare un tipo `Dictionary`CLR standard (ad esempio ) per le istanze di entità in modo che un tipo CLR esplicito non è necessario per ogni tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="4d274-135">These allow a standard CLR type (e.g. `Dictionary`) to be used for entity instances such that an explicit CLR type is not needed for each entity type.</span></span> <span data-ttu-id="4d274-136">(Allunga per la 5.0: [#9914.)](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span><span class="sxs-lookup"><span data-stu-id="4d274-136">(Stretch for 5.0: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914).)</span></span>
* <span data-ttu-id="4d274-137">Lo zucchero per una facile configurazione delle relazioni molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="4d274-137">Sugar for easy configuration of many-to-many relationships.</span></span> <span data-ttu-id="4d274-138">(Allunga per 5.0.)</span><span class="sxs-lookup"><span data-stu-id="4d274-138">(Stretch for 5.0.)</span></span>

<span data-ttu-id="4d274-139">Riteniamo che il blocco più significativo per coloro che desiderano un supporto molti-a-molti sia non essere in grado di utilizzare le relazioni "naturali", senza fare riferimento alla tabella di join, nella logica di business, ad esempio le query.</span><span class="sxs-lookup"><span data-stu-id="4d274-139">We believe that the most significant blocker for those wanting many-to-many support is not being able to use the "natural" relationships, without referring to the join table, in business logic such as queries.</span></span> <span data-ttu-id="4d274-140">Il tipo di entità della tabella di join può esistere ancora, ma non deve ostacolarlo nella logica di business.</span><span class="sxs-lookup"><span data-stu-id="4d274-140">The join table entity type may still exist, but it should not get in the way of business logic.</span></span> <span data-ttu-id="4d274-141">Questo è il motivo per cui abbiamo scelto di affrontare saltare le proprietà di navigazione per 5.0.</span><span class="sxs-lookup"><span data-stu-id="4d274-141">This is why we have chosen to tackle skip navigation properties for 5.0.</span></span>

<span data-ttu-id="4d274-142">In questo momento le altre parti di molti-a-molti vengono perseguite come obiettivo di estensione per EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="4d274-142">At this time the other parts of many-to-many are being pursued as a stretch goal for EF Core 5.0.</span></span> <span data-ttu-id="4d274-143">Ciò significa che non sono attualmente nel piano per 5.0, ma se le cose vanno bene speriamo di tirarli dentro.</span><span class="sxs-lookup"><span data-stu-id="4d274-143">This means they are not currently in the plan for 5.0, but if things go well we hope to pull them in.</span></span>

## <a name="table-per-type-tpt-inheritance-mapping"></a><span data-ttu-id="4d274-144">Mapping di ereditarietà tabella per tipo (TPT)</span><span class="sxs-lookup"><span data-stu-id="4d274-144">Table-per-type (TPT) inheritance mapping</span></span>

<span data-ttu-id="4d274-145">Sviluppatore capo:@AndriySvyryd</span><span class="sxs-lookup"><span data-stu-id="4d274-145">Lead developer: @AndriySvyryd</span></span>

<span data-ttu-id="4d274-146">Tracciato da [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span><span class="sxs-lookup"><span data-stu-id="4d274-146">Tracked by [#2266](https://github.com/aspnet/EntityFrameworkCore/issues/2266)</span></span>

<span data-ttu-id="4d274-147">Taglia t: XL</span><span class="sxs-lookup"><span data-stu-id="4d274-147">T-shirt size: XL</span></span>

<span data-ttu-id="4d274-148">Stato: In corso</span><span class="sxs-lookup"><span data-stu-id="4d274-148">Status: In-progress</span></span>

<span data-ttu-id="4d274-149">Stiamo facendo TPT perché è sia una funzionalità molto richiesta (254 voti; 3rd complessivo) e perché richiede alcune modifiche di basso livello che riteniamo appropriate per la natura fondamentale del piano .NET 5 complessivo.</span><span class="sxs-lookup"><span data-stu-id="4d274-149">We're doing TPT because it is both a highly requested feature (~254 votes; 3rd overall) and because it requires some low-level changes that we feel are appropriate for the foundational nature of the overall .NET 5 plan.</span></span> <span data-ttu-id="4d274-150">Prevediamo che questo si traduca in modifiche di rilievo per i provider di database, anche se queste dovrebbero essere molto meno gravi rispetto alle modifiche necessarie per 3.0.</span><span class="sxs-lookup"><span data-stu-id="4d274-150">We expect this to result in breaking changes for database providers, although these should be much less severe than the changes required for 3.0.</span></span>

## <a name="filtered-include"></a><span data-ttu-id="4d274-151">Includi filtrato</span><span class="sxs-lookup"><span data-stu-id="4d274-151">Filtered Include</span></span>

<span data-ttu-id="4d274-152">Sviluppatore capo:@maumar</span><span class="sxs-lookup"><span data-stu-id="4d274-152">Lead developer: @maumar</span></span>

<span data-ttu-id="4d274-153">Tracciato da [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span><span class="sxs-lookup"><span data-stu-id="4d274-153">Tracked by [#1833](https://github.com/aspnet/EntityFrameworkCore/issues/1833)</span></span>

<span data-ttu-id="4d274-154">T-shirt taglia: M</span><span class="sxs-lookup"><span data-stu-id="4d274-154">T-shirt size: M</span></span>

<span data-ttu-id="4d274-155">Stato: In corso</span><span class="sxs-lookup"><span data-stu-id="4d274-155">Status: In-progress</span></span>

<span data-ttu-id="4d274-156">L'inclusione filtrata è una funzionalità molto richiesta (317 voti, 2a nel complesso) che non è una grande quantità di lavoro e che riteniamo che sbloccherà o renderà più semplici molti scenari che attualmente richiedono filtri a livello di modello o query più complesse.</span><span class="sxs-lookup"><span data-stu-id="4d274-156">Filtered Include is a highly-requested feature (~317 votes; 2nd overall) that isn't a huge amount of work, and that we believe will unblock or make easier many scenarios that currently require model-level filters or more complex queries.</span></span>

## <a name="rationalize-totable-toquery-toview-fromsql-etc"></a><span data-ttu-id="4d274-157">Razionalizzare ToTable, ToQuery, ToView, FromSql e così via.</span><span class="sxs-lookup"><span data-stu-id="4d274-157">Rationalize ToTable, ToQuery, ToView, FromSql, etc.</span></span>

<span data-ttu-id="4d274-158">Sviluppatori principali: @maumar e@smitpatel</span><span class="sxs-lookup"><span data-stu-id="4d274-158">Lead developers: @maumar and @smitpatel</span></span>

<span data-ttu-id="4d274-159">Tracciato da [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span><span class="sxs-lookup"><span data-stu-id="4d274-159">Tracked by [#17270](https://github.com/aspnet/EntityFrameworkCore/issues/17270)</span></span>

<span data-ttu-id="4d274-160">Taglia t: L</span><span class="sxs-lookup"><span data-stu-id="4d274-160">T-shirt size: L</span></span>

<span data-ttu-id="4d274-161">Stato: In corso</span><span class="sxs-lookup"><span data-stu-id="4d274-161">Status: In-progress</span></span>

<span data-ttu-id="4d274-162">Nelle versioni precedenti sono stati compiuti progressi verso il supporto di SQL non elaborato, tipi senza chiave e aree correlate.</span><span class="sxs-lookup"><span data-stu-id="4d274-162">We have made progress in previous releases towards supporting raw SQL, keyless types, and related areas.</span></span> <span data-ttu-id="4d274-163">Tuttavia, ci sono sia lacune che incoerenze nel modo in cui tutto funziona insieme nel suo complesso.</span><span class="sxs-lookup"><span data-stu-id="4d274-163">However, there are both gaps and inconsistencies in the way everything works together as a whole.</span></span> <span data-ttu-id="4d274-164">L'obiettivo per la 5.0 è risolvere questi problemi e creare una buona esperienza per la definizione, la migrazione e l'utilizzo di diversi tipi di entità e delle query associate e degli elementi del database.</span><span class="sxs-lookup"><span data-stu-id="4d274-164">The goal for 5.0 is to fix these and create a good experience for defining, migrating, and using different types of entities and their associated queries and database artifacts.</span></span> <span data-ttu-id="4d274-165">Ciò può comportare anche aggiornamenti all'API di query compilata.</span><span class="sxs-lookup"><span data-stu-id="4d274-165">This may also involve updates to the compiled query API.</span></span>

<span data-ttu-id="4d274-166">Si noti che questo elemento può comportare alcune modifiche di interruzione a livello di applicazione poiché alcune delle funzionalità attualmente disponibili sono troppo permissive in modo che possa portare rapidamente le persone in pozzi di errore.</span><span class="sxs-lookup"><span data-stu-id="4d274-166">Note that this item may result in some application-level breaking changes since some of the functionality we currently have is too permissive such that it can quickly lead people into pits of failure.</span></span> <span data-ttu-id="4d274-167">Probabilmente finiremo per bloccare alcune di queste funzionalità insieme a indicazioni su cosa fare invece.</span><span class="sxs-lookup"><span data-stu-id="4d274-167">We will likely end up blocking some of this functionality together with guidance on what to do instead.</span></span>

## <a name="general-query-enhancements"></a><span data-ttu-id="4d274-168">Miglioramenti generali delle query</span><span class="sxs-lookup"><span data-stu-id="4d274-168">General query enhancements</span></span>

<span data-ttu-id="4d274-169">Sviluppatori principali: @smitpatel e@maumar</span><span class="sxs-lookup"><span data-stu-id="4d274-169">Lead developers: @smitpatel and @maumar</span></span>

<span data-ttu-id="4d274-170">Tracciato da [problemi `area-query` etichettati con nella fase cardine 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="4d274-170">Tracked by [issues labeled with `area-query` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-query+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="4d274-171">Taglia t: XL</span><span class="sxs-lookup"><span data-stu-id="4d274-171">T-shirt size: XL</span></span>

<span data-ttu-id="4d274-172">Stato: In corso</span><span class="sxs-lookup"><span data-stu-id="4d274-172">Status: In-progress</span></span>

<span data-ttu-id="4d274-173">Il codice di conversione della query è stato ampiamente riscritto per EF Core 3.0.The query translation code was extensively rewritten for EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="4d274-173">The query translation code was extensively rewritten for EF Core 3.0.</span></span> <span data-ttu-id="4d274-174">Il codice di query è in genere in uno stato molto più affidabile a causa di questo.</span><span class="sxs-lookup"><span data-stu-id="4d274-174">The query code is generally in a much more robust state because of this.</span></span> <span data-ttu-id="4d274-175">Per la 5.0 non è in programma di apportare modifiche importanti alle query, al di fuori di quelle necessarie per supportare TPT e ignorare le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="4d274-175">For 5.0 we aren't planning on making major query changes, outside those needed to support TPT and skip navigation properties.</span></span> <span data-ttu-id="4d274-176">Tuttavia, è ancora necessario lavorare in modo significativo per fissare alcuni debiti tecnici rimasti dalla revisione del 3.0.</span><span class="sxs-lookup"><span data-stu-id="4d274-176">However, there is still significant work needed to fix some technical debt left over from the 3.0 overhaul.</span></span> <span data-ttu-id="4d274-177">Si prevede inoltre di correggere molti bug e implementare piccoli miglioramenti per migliorare ulteriormente l'esperienza complessiva delle query.</span><span class="sxs-lookup"><span data-stu-id="4d274-177">We also plan to fix many bugs and implement small enhancements to further improve the overall query experience.</span></span>

## <a name="migrations-and-deployment-experience"></a><span data-ttu-id="4d274-178">Migrazioni ed esperienza di distribuzione</span><span class="sxs-lookup"><span data-stu-id="4d274-178">Migrations and deployment experience</span></span>

<span data-ttu-id="4d274-179">Sviluppatori principali:@bricelam</span><span class="sxs-lookup"><span data-stu-id="4d274-179">Lead developers: @bricelam</span></span>

<span data-ttu-id="4d274-180">Tracciato da [#19587](https://github.com/dotnet/efcore/issues/19587)</span><span class="sxs-lookup"><span data-stu-id="4d274-180">Tracked by [#19587](https://github.com/dotnet/efcore/issues/19587)</span></span>

<span data-ttu-id="4d274-181">Taglia t: L</span><span class="sxs-lookup"><span data-stu-id="4d274-181">T-shirt size: L</span></span>

<span data-ttu-id="4d274-182">Stato: In corso</span><span class="sxs-lookup"><span data-stu-id="4d274-182">Status: In-progress</span></span>

<span data-ttu-id="4d274-183">Attualmente, molti sviluppatori eseguire la migrazione dei database in fase di avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4d274-183">Currently, many developers migrate their databases at application startup time.</span></span> <span data-ttu-id="4d274-184">Questo è facile, ma non è raccomandato perché:</span><span class="sxs-lookup"><span data-stu-id="4d274-184">This is easy but is not recommended because:</span></span>

* <span data-ttu-id="4d274-185">Più thread/processi/server possono tentare di eseguire contemporaneamente la migrazione del database</span><span class="sxs-lookup"><span data-stu-id="4d274-185">Multiple threads/processes/servers may attempt to migrate the database concurrently</span></span>
* <span data-ttu-id="4d274-186">Le applicazioni possono tentare di accedere a uno stato incoerente mentre questo accade</span><span class="sxs-lookup"><span data-stu-id="4d274-186">Applications may try to access inconsistent state while this is happening</span></span>
* <span data-ttu-id="4d274-187">In genere le autorizzazioni del database per modificare lo schema non devono essere concesse per l'esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="4d274-187">Usually the database permissions to modify the schema should not be granted for application execution</span></span>
* <span data-ttu-id="4d274-188">E 'difficile tornare a uno stato pulito se qualcosa va storto</span><span class="sxs-lookup"><span data-stu-id="4d274-188">It's hard to revert back to a clean state if something goes wrong</span></span>

<span data-ttu-id="4d274-189">Vogliamo offrire un'esperienza migliore in questo caso che consenta un modo semplice per eseguire la migrazione del database in fase di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="4d274-189">We want to deliver a better experience here that allows an easy way to migrate the database at deployment time.</span></span> <span data-ttu-id="4d274-190">Questo dovrebbe:</span><span class="sxs-lookup"><span data-stu-id="4d274-190">This should:</span></span>

* <span data-ttu-id="4d274-191">Lavorare su Linux, Mac e Windows</span><span class="sxs-lookup"><span data-stu-id="4d274-191">Work on Linux, Mac, and Windows</span></span>
* <span data-ttu-id="4d274-192">Essere una buona esperienza nella riga di comando</span><span class="sxs-lookup"><span data-stu-id="4d274-192">Be a good experience on the command line</span></span>
* <span data-ttu-id="4d274-193">Scenari di supporto con i contenitoriSupport scenarios with containers</span><span class="sxs-lookup"><span data-stu-id="4d274-193">Support scenarios with containers</span></span>
* <span data-ttu-id="4d274-194">Utilizzare flussi/flussi di distribuzione reali di uso comune</span><span class="sxs-lookup"><span data-stu-id="4d274-194">Work with commonly used real-world deployment tools/flows</span></span>
* <span data-ttu-id="4d274-195">Integrazione in almeno Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4d274-195">Integrate into at least Visual Studio</span></span>

<span data-ttu-id="4d274-196">Il risultato è probabilmente molti piccoli miglioramenti in EF Core (ad esempio, migliori migrazioni su SQLite), insieme a linee guida e collaborazioni a lungo termine con altri team per migliorare le esperienze end-to-end che vanno oltre solo EF.</span><span class="sxs-lookup"><span data-stu-id="4d274-196">The result is likely to be many small improvements in EF Core (for example, better Migrations on SQLite), together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

## <a name="ef-core-platforms-experience"></a><span data-ttu-id="4d274-197">Esperienza nelle piattaforme EF Core</span><span class="sxs-lookup"><span data-stu-id="4d274-197">EF Core platforms experience</span></span> 

<span data-ttu-id="4d274-198">Sviluppatori principali: @roji e@bricelam</span><span class="sxs-lookup"><span data-stu-id="4d274-198">Lead developers: @roji and @bricelam</span></span>

<span data-ttu-id="4d274-199">Tracciato da [#19588](https://github.com/dotnet/efcore/issues/19588)</span><span class="sxs-lookup"><span data-stu-id="4d274-199">Tracked by [#19588](https://github.com/dotnet/efcore/issues/19588)</span></span>

<span data-ttu-id="4d274-200">Taglia t: L</span><span class="sxs-lookup"><span data-stu-id="4d274-200">T-shirt size: L</span></span>

<span data-ttu-id="4d274-201">Stato: Non avviato</span><span class="sxs-lookup"><span data-stu-id="4d274-201">Status: Not started</span></span>

<span data-ttu-id="4d274-202">Sono disponibili indicazioni utili per l'utilizzo di EF Core nelle applicazioni Web tradizionali di tipo MVC.</span><span class="sxs-lookup"><span data-stu-id="4d274-202">We have good guidance for using EF Core in traditional MVC-like web applications.</span></span> <span data-ttu-id="4d274-203">Linee guida per altre piattaforme e modelli di applicazione mancanti o non aggiornate.</span><span class="sxs-lookup"><span data-stu-id="4d274-203">Guidance for other platforms and application models is either missing or out-of-date.</span></span> <span data-ttu-id="4d274-204">Per EF Core 5.0, si prevede di analizzare, migliorare e documentare l'esperienza di utilizzo di EF Core con:</span><span class="sxs-lookup"><span data-stu-id="4d274-204">For EF Core 5.0, we plan to investigate, improve, and document the experience of using EF Core with:</span></span>

* <span data-ttu-id="4d274-205">Blazor</span><span class="sxs-lookup"><span data-stu-id="4d274-205">Blazor</span></span>
* <span data-ttu-id="4d274-206">Xamarin, incluso l'utilizzo della storia AOT/linker</span><span class="sxs-lookup"><span data-stu-id="4d274-206">Xamarin, including using the AOT/linker story</span></span>
* <span data-ttu-id="4d274-207">WinForms/WPF/WinUI ed eventualmente altri U.I.</span><span class="sxs-lookup"><span data-stu-id="4d274-207">WinForms/WPF/WinUI and possibly other U.I.</span></span> <span data-ttu-id="4d274-208">frameworks</span><span class="sxs-lookup"><span data-stu-id="4d274-208">frameworks</span></span>

<span data-ttu-id="4d274-209">Questo è probabile che si tratti di molti piccoli miglioramenti in EF Core, insieme a linee guida e collaborazioni a lungo termine con altri team per migliorare le esperienze end-to-end che vanno oltre il solo EF.</span><span class="sxs-lookup"><span data-stu-id="4d274-209">This is likely to be many small improvements in EF Core, together with guidance and longer-term collaborations with other teams to improve end-to-end experiences that go beyond just EF.</span></span>

<span data-ttu-id="4d274-210">Le aree specifiche che prevediamo di esaminare sono:</span><span class="sxs-lookup"><span data-stu-id="4d274-210">Specific areas we plan to look at are:</span></span>

* <span data-ttu-id="4d274-211">Distribuzione, inclusa l'esperienza per l'utilizzo di strumenti di EF, ad esempio per le migrazioni</span><span class="sxs-lookup"><span data-stu-id="4d274-211">Deployment, including the experience for using EF tooling such as for Migrations</span></span>
* <span data-ttu-id="4d274-212">Modelli applicativi, tra cui Xamarin e Blazor, e probabilmente altri</span><span class="sxs-lookup"><span data-stu-id="4d274-212">Application models, including Xamarin and Blazor, and probably others</span></span>
* <span data-ttu-id="4d274-213">Esperienze SQLite, tra cui l'esperienza spaziale e le ricostruzioni delle tabelle</span><span class="sxs-lookup"><span data-stu-id="4d274-213">SQLite experiences, including the spatial experience and table rebuilds</span></span>
* <span data-ttu-id="4d274-214">AOT e esperienze di collegamento</span><span class="sxs-lookup"><span data-stu-id="4d274-214">AOT and linking experiences</span></span>
* <span data-ttu-id="4d274-215">Integrazione diagnostica, inclusi i contatori perf</span><span class="sxs-lookup"><span data-stu-id="4d274-215">Diagnostics integration, including perf counters</span></span>

## <a name="performance"></a><span data-ttu-id="4d274-216">Prestazioni</span><span class="sxs-lookup"><span data-stu-id="4d274-216">Performance</span></span>

<span data-ttu-id="4d274-217">Sviluppatore capo:@roji</span><span class="sxs-lookup"><span data-stu-id="4d274-217">Lead developer: @roji</span></span>

<span data-ttu-id="4d274-218">Tracciato da [problemi `area-perf` etichettati con nella fase cardine 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="4d274-218">Tracked by [issues labeled with `area-perf` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+label%3Aarea-perf+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="4d274-219">Taglia t: L</span><span class="sxs-lookup"><span data-stu-id="4d274-219">T-shirt size: L</span></span>

<span data-ttu-id="4d274-220">Stato: In corso</span><span class="sxs-lookup"><span data-stu-id="4d274-220">Status: In-progress</span></span>

<span data-ttu-id="4d274-221">Per EF Core, si prevede di migliorare la nostra suite di benchmark delle prestazioni e apportare miglioramenti diretti delle prestazioni al runtime.</span><span class="sxs-lookup"><span data-stu-id="4d274-221">For EF Core, we plan to improve our suite of performance benchmarks and make directed performance improvements to the runtime.</span></span> <span data-ttu-id="4d274-222">Inoltre, prevediamo di completare la nuova API di batch ADO.NET con prototipo durante il ciclo di rilascio 3.0.</span><span class="sxs-lookup"><span data-stu-id="4d274-222">In addition, we plan to complete the new ADO.NET batching API which was prototyped during the 3.0 release cycle.</span></span> <span data-ttu-id="4d274-223">Anche a livello di ADO.NET, si pianificano ulteriori miglioramenti delle prestazioni per il provider Npgsql.</span><span class="sxs-lookup"><span data-stu-id="4d274-223">Also at the ADO.NET layer, we plan additional performance improvements to the Npgsql provider.</span></span>

<span data-ttu-id="4d274-224">Come parte di questo lavoro si prevede anche di aggiungere i contatori delle prestazioni di ADO.NET/EF Core e altre funzionalità di diagnostica in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="4d274-224">As part of this work we also plan to add ADO.NET/EF Core performance counters and other diagnostics as appropriate.</span></span>

## <a name="architecturalcontributor-documentation"></a><span data-ttu-id="4d274-225">Documentazione architettonica/collaboratore</span><span class="sxs-lookup"><span data-stu-id="4d274-225">Architectural/contributor documentation</span></span>

<span data-ttu-id="4d274-226">Documentatore principale:@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="4d274-226">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="4d274-227">Tracciato da [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span><span class="sxs-lookup"><span data-stu-id="4d274-227">Tracked by [#1920](https://github.com/dotnet/EntityFramework.Docs/issues/1920)</span></span>

<span data-ttu-id="4d274-228">Taglia t: L</span><span class="sxs-lookup"><span data-stu-id="4d274-228">T-shirt size: L</span></span>

<span data-ttu-id="4d274-229">Stato: In corso</span><span class="sxs-lookup"><span data-stu-id="4d274-229">Status: In-progress</span></span>

<span data-ttu-id="4d274-230">L'idea qui è quello di rendere più facile capire cosa sta succedendo negli elementi interni di EF Core.</span><span class="sxs-lookup"><span data-stu-id="4d274-230">The idea here is to make it easier to understand what is going on in the internals of EF Core.</span></span> <span data-ttu-id="4d274-231">Questo può essere utile a chiunque utilizzi EF Core, ma la motivazione principale è quello di rendere più semplice per gli utenti esterni:This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span><span class="sxs-lookup"><span data-stu-id="4d274-231">This can be useful to anyone using EF Core, but the primary motivation is to make it easier for external people to:</span></span>

* <span data-ttu-id="4d274-232">Contribuire al codice di base di EFContribute to the EF Core code</span><span class="sxs-lookup"><span data-stu-id="4d274-232">Contribute to the EF Core code</span></span>
* <span data-ttu-id="4d274-233">Creare provider di databaseCreate database providers</span><span class="sxs-lookup"><span data-stu-id="4d274-233">Create database providers</span></span>
* <span data-ttu-id="4d274-234">Creare altre estensioni</span><span class="sxs-lookup"><span data-stu-id="4d274-234">Build other extensions</span></span>

## <a name="microsoftdatasqlite-documentation"></a><span data-ttu-id="4d274-235">Documentazione di Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="4d274-235">Microsoft.Data.Sqlite documentation</span></span>

<span data-ttu-id="4d274-236">Documentatore principale:@bricelam</span><span class="sxs-lookup"><span data-stu-id="4d274-236">Lead documenter: @bricelam</span></span>

<span data-ttu-id="4d274-237">Tracciato da [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span><span class="sxs-lookup"><span data-stu-id="4d274-237">Tracked by [#1675](https://github.com/dotnet/EntityFramework.Docs/issues/1675)</span></span>

<span data-ttu-id="4d274-238">T-shirt taglia: M</span><span class="sxs-lookup"><span data-stu-id="4d274-238">T-shirt size: M</span></span>

<span data-ttu-id="4d274-239">Stato: Completato.</span><span class="sxs-lookup"><span data-stu-id="4d274-239">Status: Completed.</span></span> <span data-ttu-id="4d274-240">La nuova documentazione è [disponibile nel sito Microsoft Docs](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span><span class="sxs-lookup"><span data-stu-id="4d274-240">The new documentation is [live on the Microsoft docs site](https://docs.microsoft.com/dotnet/standard/data/sqlite/?tabs=netcore-cli).</span></span>

<span data-ttu-id="4d274-241">Il team di Entity Framework è inoltre proprietario del provider di ADO.NET Microsoft.Data.Sqlite.</span><span class="sxs-lookup"><span data-stu-id="4d274-241">The EF Team also owns the Microsoft.Data.Sqlite ADO.NET provider.</span></span> <span data-ttu-id="4d274-242">Prevediamo di documentare completamente questo provider come parte della versione 5.0.</span><span class="sxs-lookup"><span data-stu-id="4d274-242">We plan to fully document this provider as part of the 5.0 release.</span></span>

## <a name="general-documentation"></a><span data-ttu-id="4d274-243">Documentazione generale</span><span class="sxs-lookup"><span data-stu-id="4d274-243">General documentation</span></span>

<span data-ttu-id="4d274-244">Documentatore principale:@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="4d274-244">Lead documenter: @ajcvickers</span></span>

<span data-ttu-id="4d274-245">Tracciata dai [problemi nel repository dei documenti nell'attività cardine 5.0](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span><span class="sxs-lookup"><span data-stu-id="4d274-245">Tracked by [issues in the docs repo in the 5.0 milestone](https://github.com/dotnet/EntityFramework.Docs/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+)</span></span>

<span data-ttu-id="4d274-246">Taglia t: L</span><span class="sxs-lookup"><span data-stu-id="4d274-246">T-shirt size: L</span></span>

<span data-ttu-id="4d274-247">Stato: In corso</span><span class="sxs-lookup"><span data-stu-id="4d274-247">Status: In-progress</span></span>

<span data-ttu-id="4d274-248">È già in corso l'aggiornamento della documentazione per le versioni 3.0 e 3.1.</span><span class="sxs-lookup"><span data-stu-id="4d274-248">We are already in the process of updating documentation for the 3.0 and 3.1 releases.</span></span> <span data-ttu-id="4d274-249">Stiamo anche lavorando su:</span><span class="sxs-lookup"><span data-stu-id="4d274-249">We are also working on:</span></span>
  * <span data-ttu-id="4d274-250">Una revisione della documentazione introduttiva per renderli più accessibile/più facili da seguire</span><span class="sxs-lookup"><span data-stu-id="4d274-250">An overhaul of the getting started docs to make them more approachable/easier to follow</span></span>
  * <span data-ttu-id="4d274-251">Riorganizzazione dei documenti per semplificare la ricerca e aggiungere riferimenti incrociati</span><span class="sxs-lookup"><span data-stu-id="4d274-251">Reorganization of docs to make things easier to find and to add cross-references</span></span>
  * <span data-ttu-id="4d274-252">Aggiunta di ulteriori dettagli e chiarimenti ai documenti esistenti</span><span class="sxs-lookup"><span data-stu-id="4d274-252">Adding more details and clarifications to existing docs</span></span>
  * <span data-ttu-id="4d274-253">Aggiornamento degli esempi e aggiunta di altri esempi</span><span class="sxs-lookup"><span data-stu-id="4d274-253">Updating the samples and adding more examples</span></span>

## <a name="fixing-bugs"></a><span data-ttu-id="4d274-254">Correzione di bug</span><span class="sxs-lookup"><span data-stu-id="4d274-254">Fixing bugs</span></span>

<span data-ttu-id="4d274-255">Tracciato da [problemi `type-bug` etichettati con nella fase cardine 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span><span class="sxs-lookup"><span data-stu-id="4d274-255">Tracked by [issues labeled with `type-bug` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-bug+)</span></span>

<span data-ttu-id="4d274-256">Sviluppatori: @roji @maumar, @bricelam @smitpatel, @AndriySvyryd, , , ,@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="4d274-256">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="4d274-257">Taglia t: L</span><span class="sxs-lookup"><span data-stu-id="4d274-257">T-shirt size: L</span></span>

<span data-ttu-id="4d274-258">Stato: In corso</span><span class="sxs-lookup"><span data-stu-id="4d274-258">Status: In-progress</span></span>

<span data-ttu-id="4d274-259">Al momento della scrittura, abbiamo 135 bug triati per essere corretti nella versione 5.0 (con 62 già risolti), ma c'è una sovrapposizione significativa con la sezione Miglioramenti delle _query generali_ sopra.</span><span class="sxs-lookup"><span data-stu-id="4d274-259">At the time of writing, we have 135 bugs triaged to be fixed in the 5.0 release (with 62 already fixed), but there is significant overlap with the _General query enhancements_ section above.</span></span>

<span data-ttu-id="4d274-260">La tariffa in entrata (problemi che finiscono come lavoro in una pietra miliare) è stata di circa 23 numeri al mese nel corso della versione 3.0.</span><span class="sxs-lookup"><span data-stu-id="4d274-260">The incoming rate (issues that end up as work in a milestone) was about 23 issues per month over the course of the 3.0 release.</span></span> <span data-ttu-id="4d274-261">Non tutti questi dovranno essere risolti in 5.0.</span><span class="sxs-lookup"><span data-stu-id="4d274-261">Not all of these will need to be fixed in 5.0.</span></span> <span data-ttu-id="4d274-262">Come stima approssimativa abbiamo intenzione di risolvere altri 150 problemi nell'intervallo di tempo 5.0.</span><span class="sxs-lookup"><span data-stu-id="4d274-262">As a rough estimate we plan to fix an additional 150 issues in the 5.0 time frame.</span></span>

## <a name="small-enhancements"></a><span data-ttu-id="4d274-263">Piccoli miglioramenti</span><span class="sxs-lookup"><span data-stu-id="4d274-263">Small enhancements</span></span>

<span data-ttu-id="4d274-264">Tracciato da [problemi `type-enhancement` etichettati con nella fase cardine 5.0](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span><span class="sxs-lookup"><span data-stu-id="4d274-264">Tracked by [issues labeled with `type-enhancement` in the 5.0 milestone](https://github.com/dotnet/efcore/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A5.0.0+label%3Atype-enhancement+)</span></span>

<span data-ttu-id="4d274-265">Sviluppatori: @roji @maumar, @bricelam @smitpatel, @AndriySvyryd, , , ,@ajcvickers</span><span class="sxs-lookup"><span data-stu-id="4d274-265">Developers: @roji, @maumar, @bricelam, @smitpatel, @AndriySvyryd, @ajcvickers</span></span>

<span data-ttu-id="4d274-266">Taglia t: L</span><span class="sxs-lookup"><span data-stu-id="4d274-266">T-shirt size: L</span></span>

<span data-ttu-id="4d274-267">Stato: In corso</span><span class="sxs-lookup"><span data-stu-id="4d274-267">Status: In-progress</span></span>

<span data-ttu-id="4d274-268">Oltre alle caratteristiche più grandi descritte in precedenza, abbiamo anche molti piccoli miglioramenti programmati per la 5.0 per risolvere i "tagli di carta".</span><span class="sxs-lookup"><span data-stu-id="4d274-268">In addition to the bigger features outlined above, we also have many smaller improvements scheduled for 5.0 to fix "paper-cuts".</span></span> <span data-ttu-id="4d274-269">Si noti che molti di questi miglioramenti sono trattati anche dai temi più generali descritti in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4d274-269">Note that many of these enhancements are also covered by the more general themes outlined above.</span></span>

## <a name="below-the-line"></a><span data-ttu-id="4d274-270">Sotto la linea</span><span class="sxs-lookup"><span data-stu-id="4d274-270">Below-the-line</span></span>

<span data-ttu-id="4d274-271">Tracciata da [problemi `consider-for-next-release` etichettati con](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span><span class="sxs-lookup"><span data-stu-id="4d274-271">Tracked by [issues labeled with `consider-for-next-release`](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+label%3Aconsider-for-next-release)</span></span>

<span data-ttu-id="4d274-272">Si tratta di correzioni di bug e miglioramenti che **non** sono attualmente pianificati per la versione 5.0, ma esamineremo come obiettivi di stretching a seconda dei progressi compiuti sul lavoro di cui sopra.</span><span class="sxs-lookup"><span data-stu-id="4d274-272">These are bug fixes and enhancements that are **not** currently scheduled for the 5.0 release, but we will look at as stretch goals depending on the progress made on the work above.</span></span>

<span data-ttu-id="4d274-273">Inoltre, consideriamo sempre le [questioni più votate](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) durante la pianificazione.</span><span class="sxs-lookup"><span data-stu-id="4d274-273">In addition, we always consider the [most voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc) when planning.</span></span> <span data-ttu-id="4d274-274">Tagliare uno di questi problemi da un rilascio è sempre doloroso, ma abbiamo bisogno di un piano realistico per le risorse che abbiamo.</span><span class="sxs-lookup"><span data-stu-id="4d274-274">Cutting any of these issues from a release is always painful, but we do need a realistic plan for the resources we have.</span></span>

## <a name="feedback"></a><span data-ttu-id="4d274-275">Commenti e suggerimenti</span><span class="sxs-lookup"><span data-stu-id="4d274-275">Feedback</span></span>

<span data-ttu-id="4d274-276">I commenti e i suggerimenti dei clienti sulla pianificazione sono importanti.</span><span class="sxs-lookup"><span data-stu-id="4d274-276">Your feedback on planning is important.</span></span> <span data-ttu-id="4d274-277">Il modo migliore per indicare l'importanza di un problema consiste nel votare (Pollice in su) per tale problema in GitHub.</span><span class="sxs-lookup"><span data-stu-id="4d274-277">The best way to indicate the importance of an issue is to vote (thumbs-up) for that issue on GitHub.</span></span> <span data-ttu-id="4d274-278">Questi dati verranno quindi inseriti nel processo di [pianificazione](../release-planning.md) per la versione successiva.</span><span class="sxs-lookup"><span data-stu-id="4d274-278">This data will then feed into the [planning process](../release-planning.md) for the next release.</span></span>
