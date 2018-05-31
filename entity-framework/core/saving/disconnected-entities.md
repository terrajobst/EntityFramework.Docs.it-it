---
title: Entità disconnesse - EF Core
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: 0b145217d40027c4b8e4746e9c5651652a28c9eb
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152416"
---
# <a name="disconnected-entities"></a><span data-ttu-id="a24ee-102">Entità disconnesse</span><span class="sxs-lookup"><span data-stu-id="a24ee-102">Disconnected entities</span></span>

<span data-ttu-id="a24ee-103">Un'istanza di DbContext sottoporrà automaticamente a rilevamento delle modifiche le entità restituite dal database.</span><span class="sxs-lookup"><span data-stu-id="a24ee-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="a24ee-104">Le modifiche apportate a queste entità verranno quindi rilevate quando viene chiamato SaveChanges e il database verrà aggiornato in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="a24ee-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="a24ee-105">Vedere [Salvataggio di base](basic.md) e [Dati correlati](related-data.md) per informazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="a24ee-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="a24ee-106">Tuttavia, a volte le entità vengono sottoposte a query usando un'istanza di contesto e poi salvate con un'istanza diversa.</span><span class="sxs-lookup"><span data-stu-id="a24ee-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="a24ee-107">Questo accade spesso in scenari "disconnessi", ad esempio un'applicazione Web in cui le entità vengono recuperate tramite query, inviate al client, modificate, inviate al server in una richiesta e quindi salvate.</span><span class="sxs-lookup"><span data-stu-id="a24ee-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="a24ee-108">In questo caso, la seconda istanza del contesto deve sapere se le entità sono nuove (devono essere inserite) o esistenti (devono essere aggiornate).</span><span class="sxs-lookup"><span data-stu-id="a24ee-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

> [!TIP]  
> <span data-ttu-id="a24ee-109">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="a24ee-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) on GitHub.</span></span>

> [!TIP]
> <span data-ttu-id="a24ee-110">EF Core può eseguire il rilevamento delle modifiche per una sola istanza di qualsiasi entità con un determinato valore di chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="a24ee-110">EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="a24ee-111">Il modo migliore per evitare che ciò diventi un problema consiste nell'usare un contesto di breve durata per ogni unità di lavoro, in modo che il contesto sia inizialmente vuoto, abbia entità collegate, salvi queste entità, per poi essere eliminato e rimosso.</span><span class="sxs-lookup"><span data-stu-id="a24ee-111">The best way to avoid this being an issue is to use a short-lived context for each unit-of-work such that the context starts empty, has entities attached to it, saves those entities, and then the context is disposed and discarded.</span></span>

## <a name="identifying-new-entities"></a><span data-ttu-id="a24ee-112">Identificazione di nuove entità</span><span class="sxs-lookup"><span data-stu-id="a24ee-112">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="a24ee-113">Nuove identità identificate dal client</span><span class="sxs-lookup"><span data-stu-id="a24ee-113">Client identifies new entities</span></span>

<span data-ttu-id="a24ee-114">Il caso più semplice da affrontare è quando il client comunica al server se l'entità è nuova o esistente.</span><span class="sxs-lookup"><span data-stu-id="a24ee-114">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="a24ee-115">Ad esempio, spesso la richiesta di inserire una nuova entità è diversa dalla richiesta di aggiornare un'entità esistente.</span><span class="sxs-lookup"><span data-stu-id="a24ee-115">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="a24ee-116">Nella parte restante di questa sezione vengono illustrati i casi in cui è necessario determinare in altro modo se eseguire un inserimento o un aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="a24ee-116">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="a24ee-117">Con chiavi generate automaticamente</span><span class="sxs-lookup"><span data-stu-id="a24ee-117">With auto-generated keys</span></span>

<span data-ttu-id="a24ee-118">Il valore di una chiave generata automaticamente può essere spesso usato per determinare se un'entità deve essere inserita o aggiornata.</span><span class="sxs-lookup"><span data-stu-id="a24ee-118">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="a24ee-119">Se la chiave non è stata impostata (ad esempio, ha ancora il valore predefinito di CLR Null, zero e così via), l'entità deve essere nuova e quindi inserita.</span><span class="sxs-lookup"><span data-stu-id="a24ee-119">If the key has not been set (i.e. it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="a24ee-120">D'altra parte, se il valore della chiave è stato impostato, l'entità deve essere già stata salvata in precedenza e ora richiede un aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="a24ee-120">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="a24ee-121">In altre parole, se la chiave ha un valore, allora l'entità è stata già sottoposta a query e inviata al client e ora ritorna per l'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="a24ee-121">In other words, if the key has a value, then entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="a24ee-122">È facile verificare la presenza di una chiave non impostata quando è noto il tipo di entità:</span><span class="sxs-lookup"><span data-stu-id="a24ee-122">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="a24ee-123">Tuttavia, EF include anche un modo predefinito per eseguire questa operazione per qualsiasi tipo di entità e tipo di chiave:</span><span class="sxs-lookup"><span data-stu-id="a24ee-123">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="a24ee-124">Le chiavi vengono impostate non appena le entità vengono incluse nel rilevamento delle modifiche dal contesto, anche se l'entità risulta con lo stato Added.</span><span class="sxs-lookup"><span data-stu-id="a24ee-124">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="a24ee-125">Ciò è utile durante l'attraversamento di un grafo di entità e per decidere come procedere con ognuna, ad esempio quando di usa l'API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="a24ee-125">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="a24ee-126">Il valore della chiave deve essere usato solo nel modo illustrato di seguito _prima_ che venga effettuata qualsiasi chiamata per il rilevamento delle modifiche dell'entità.</span><span class="sxs-lookup"><span data-stu-id="a24ee-126">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="a24ee-127">Con altre chiavi</span><span class="sxs-lookup"><span data-stu-id="a24ee-127">With other keys</span></span>

<span data-ttu-id="a24ee-128">È necessario un altro meccanismo per identificare le nuove entità quando i valori di chiave non vengono generati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="a24ee-128">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="a24ee-129">Esistono due approcci generali a questo scopo:</span><span class="sxs-lookup"><span data-stu-id="a24ee-129">There are two general approaches to this:</span></span>
 * <span data-ttu-id="a24ee-130">Recuperare l'entità tramite query</span><span class="sxs-lookup"><span data-stu-id="a24ee-130">Query for the entity</span></span>
 * <span data-ttu-id="a24ee-131">Passare un flag dal client</span><span class="sxs-lookup"><span data-stu-id="a24ee-131">Pass a flag from the client</span></span>

<span data-ttu-id="a24ee-132">Per eseguire una query per l'entità, è sufficiente usare il metodo Find:</span><span class="sxs-lookup"><span data-stu-id="a24ee-132">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="a24ee-133">Esula dagli scopi di questo documento mostrare il codice completo per il passaggio di un flag da un client.</span><span class="sxs-lookup"><span data-stu-id="a24ee-133">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="a24ee-134">In un'app Web, in genere significa effettuare richieste diverse per azioni diverse oppure passare uno stato nella richiesta e quindi estrarlo nel controller.</span><span class="sxs-lookup"><span data-stu-id="a24ee-134">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="a24ee-135">Salvataggio di singole entità</span><span class="sxs-lookup"><span data-stu-id="a24ee-135">Saving single entities</span></span>

<span data-ttu-id="a24ee-136">Quando è noto se è necessario eseguire un inserimento o un aggiornamento, è possibile usare in modo appropriato Add o Update:</span><span class="sxs-lookup"><span data-stu-id="a24ee-136">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="a24ee-137">Tuttavia, se l'entità usa valori di chiave generati automaticamente, è possibile usare il metodo Update per entrambi i casi:</span><span class="sxs-lookup"><span data-stu-id="a24ee-137">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="a24ee-138">Il metodo Update contrassegna in genere l'entità per l'aggiornamento e non per l'inserimento.</span><span class="sxs-lookup"><span data-stu-id="a24ee-138">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="a24ee-139">Tuttavia, se l'entità ha una chiave generata automaticamente e non è stato impostato alcun valore per la chiave, l'entità viene invece contrassegnata automaticamente per l'inserimento.</span><span class="sxs-lookup"><span data-stu-id="a24ee-139">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="a24ee-140">Questo comportamento è stato introdotto in EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="a24ee-140">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="a24ee-141">Per le versioni precedenti è sempre necessario scegliere in modo esplicito Add o Update.</span><span class="sxs-lookup"><span data-stu-id="a24ee-141">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="a24ee-142">Se l'entità non usa chiavi generate automaticamente, l'applicazione deve quindi decidere se l'entità deve essere inserita o aggiornata. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a24ee-142">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="a24ee-143">La procedura è la seguente:</span><span class="sxs-lookup"><span data-stu-id="a24ee-143">The steps here are:</span></span>
* <span data-ttu-id="a24ee-144">Se Find restituisce Null, il database non contiene già il blog con tale ID, pertanto si chiama Add per contrassegnarlo per l'inserimento.</span><span class="sxs-lookup"><span data-stu-id="a24ee-144">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="a24ee-145">Se Find restituisce un'entità, significa che il blog esiste nel database e il contesto esegue il rilevamento delle modifiche per l'entità esistente</span><span class="sxs-lookup"><span data-stu-id="a24ee-145">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="a24ee-146">È quindi possibile usare SetValues per impostare i valori per tutte le proprietà dell'entità sui valori provenienti dal client.</span><span class="sxs-lookup"><span data-stu-id="a24ee-146">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="a24ee-147">La chiamata di SetValues contrassegnerà l'entità per l'aggiornamento in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="a24ee-147">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="a24ee-148">SetValues contrassegnerà come modificate solo le proprietà con valori diversi da quelli nell'entità con rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="a24ee-148">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="a24ee-149">Questo significa che quando viene inviato l'aggiornamento, verranno aggiornate solo le colonne effettivamente modificate.</span><span class="sxs-lookup"><span data-stu-id="a24ee-149">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="a24ee-150">(In assenza di modifiche non verrà inviato alcun aggiornamento.)</span><span class="sxs-lookup"><span data-stu-id="a24ee-150">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="a24ee-151">Utilizzo dei grafi</span><span class="sxs-lookup"><span data-stu-id="a24ee-151">Working with graphs</span></span>

### <a name="identity-resolution"></a><span data-ttu-id="a24ee-152">Risoluzione di identità</span><span class="sxs-lookup"><span data-stu-id="a24ee-152">Identity resolution</span></span>

<span data-ttu-id="a24ee-153">Come indicato in precedenza, EF Core può eseguire il rilevamento delle modifiche per una sola istanza di qualsiasi entità con un determinato valore di chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="a24ee-153">As noted above, EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="a24ee-154">Quando si utilizzano i grafi, il grafo deve essere creato idealmente in modo da mantenere questa invariante e da usare il contesto per una sola unità di lavoro.</span><span class="sxs-lookup"><span data-stu-id="a24ee-154">When working with graphs the graph should ideally be created such that this invariant is maintained, and the context should be used for only one unit-of-work.</span></span> <span data-ttu-id="a24ee-155">Se il grafo contiene duplicati, sarà necessario elaborare il grafo prima di inviarlo a EF per consolidare più istanze in una unica.</span><span class="sxs-lookup"><span data-stu-id="a24ee-155">If the graph does contain duplicates, then it will be necessary to process the graph before sending it to EF to consolidate multiple instances into one.</span></span> <span data-ttu-id="a24ee-156">Questa operazione potrebbe non essere semplice se le istanze hanno valori e relazioni in conflitto, quindi è consigliabile eseguire il consolidamento dei duplicati non appena possibile nella pipeline dell'applicazione per evitare la risoluzione dei conflitti.</span><span class="sxs-lookup"><span data-stu-id="a24ee-156">This may not be trivial where instances have conflicting values and relationships, so consolidating duplicates should be done as soon as possible in your application pipeline to avoid conflict resolution.</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="a24ee-157">Tutte le entità nuove/esistenti</span><span class="sxs-lookup"><span data-stu-id="a24ee-157">All new/all existing entities</span></span>

<span data-ttu-id="a24ee-158">Un esempio di utilizzo dei grafi è l'inserimento o l'aggiornamento di un blog con la raccolta di post associati.</span><span class="sxs-lookup"><span data-stu-id="a24ee-158">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="a24ee-159">Se tutte le entità nel grafo devono essere inserite o devono essere tutte aggiornate, il processo è identico a quello sopra descritto per singole entità.</span><span class="sxs-lookup"><span data-stu-id="a24ee-159">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="a24ee-160">Ad esempio, un grafo di blog e post creato come il seguente:</span><span class="sxs-lookup"><span data-stu-id="a24ee-160">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="a24ee-161">può essere inserito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="a24ee-161">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="a24ee-162">La chiamata di Add contrassegnerà il blog e tutti i post per l'inserimento.</span><span class="sxs-lookup"><span data-stu-id="a24ee-162">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="a24ee-163">Analogamente, se tutte le entità in un grafo devono essere aggiornate, si può usare Update:</span><span class="sxs-lookup"><span data-stu-id="a24ee-163">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="a24ee-164">Il blog e tutti i relativi post verranno contrassegnati per l'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="a24ee-164">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="a24ee-165">Combinazione di entità nuove ed esistenti</span><span class="sxs-lookup"><span data-stu-id="a24ee-165">Mix of new and existing entities</span></span>

<span data-ttu-id="a24ee-166">Con le chiavi generate automaticamente, è possibile usare Update sia per gli inserimenti che per gli aggiornamenti, anche se il grafo contiene una combinazione di entità che richiedono l'inserimento e che richiedono l'aggiornamento:</span><span class="sxs-lookup"><span data-stu-id="a24ee-166">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="a24ee-167">Update contrassegnerà qualsiasi entità nel grafo, blog o post, per l'inserimento, se non dispone di valore di chiave impostato, mentre tutte le altre entità verranno contrassegnate per l'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="a24ee-167">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="a24ee-168">Come prima, se non si usano chiavi generate automaticamente, è possibile usare una query e alcune operazioni di elaborazione:</span><span class="sxs-lookup"><span data-stu-id="a24ee-168">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="a24ee-169">Gestione delle eliminazioni</span><span class="sxs-lookup"><span data-stu-id="a24ee-169">Handling deletes</span></span>

<span data-ttu-id="a24ee-170">L'eliminazione può essere difficile da gestire, dato che l'assenza di un'entità indica spesso che deve essere eliminata.</span><span class="sxs-lookup"><span data-stu-id="a24ee-170">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="a24ee-171">Un modo per risolvere questo problema consiste nell'usare "eliminazioni temporanee" in modo che l'entità venga contrassegnata come eliminata anziché essere effettivamente eliminata.</span><span class="sxs-lookup"><span data-stu-id="a24ee-171">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="a24ee-172">Le eliminazioni diventano quindi uguali agli aggiornamenti.</span><span class="sxs-lookup"><span data-stu-id="a24ee-172">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="a24ee-173">Le eliminazioni temporanee possono essere implementate usando [filtri di query](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="a24ee-173">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="a24ee-174">Per le vere eliminazioni, un modello comune consiste nell'usare un'estensione del modello di query per eseguire essenzialmente un confronto delle differenze del grafo. Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a24ee-174">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff. For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="a24ee-175">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="a24ee-175">TrackGraph</span></span>

<span data-ttu-id="a24ee-176">Internamente, Add, Attach e Update usano l'attraversamento del grafo determinando per ogni entità se deve essere contrassegnata come Added (per l'inserimento), Modified (per l'aggiornamento), Unchanged (per non eseguire alcuna operazione) o Delete (per l'eliminazione).</span><span class="sxs-lookup"><span data-stu-id="a24ee-176">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="a24ee-177">Questo meccanismo viene esposto tramite l'API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="a24ee-177">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="a24ee-178">Ad esempio, si supponga che quando il client invia un grafo delle entità imposti alcuni flag per ogni entità per indicare come deve essere gestita.</span><span class="sxs-lookup"><span data-stu-id="a24ee-178">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="a24ee-179">TrackGraph può quindi essere usato per elaborare questo flag:</span><span class="sxs-lookup"><span data-stu-id="a24ee-179">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="a24ee-180">I flag vengono visualizzati solo come parte dell'entità per semplicità dell'esempio.</span><span class="sxs-lookup"><span data-stu-id="a24ee-180">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="a24ee-181">In genere, i flag farebbero parte di un DTO o qualche altro stato incluso nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="a24ee-181">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
