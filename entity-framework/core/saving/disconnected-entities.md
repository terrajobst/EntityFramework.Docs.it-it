---
title: "Entità disconnesse - Core EF"
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: 0ea02876b9594d54c971a7b70fcf7ce591e56ba0
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/22/2017
---
# <a name="disconnected-entities"></a><span data-ttu-id="4956e-102">Entità disconnesse</span><span class="sxs-lookup"><span data-stu-id="4956e-102">Disconnected entities</span></span>

<span data-ttu-id="4956e-103">Un'istanza di DbContext rileverà automaticamente le entità restituite dal database.</span><span class="sxs-lookup"><span data-stu-id="4956e-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="4956e-104">Le modifiche apportate a queste entità verranno quindi rilevate quando viene chiamato SaveChanges e il database verrà aggiornato in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="4956e-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="4956e-105">Vedere [salvare base](basic.md) e [dati correlati](related-data.md) per informazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="4956e-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="4956e-106">Tuttavia, talvolta le entità vengono interrogate utilizzando un'unica istanza di contesto e quindi salvati utilizzando un'istanza diversa.</span><span class="sxs-lookup"><span data-stu-id="4956e-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="4956e-107">Questo accade spesso "disconnessi" scenari, ad esempio un'applicazione web in cui le entità sono query inviate al client, modificate, inviate al server in una richiesta e quindi salvate.</span><span class="sxs-lookup"><span data-stu-id="4956e-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="4956e-108">In questo caso, il contesto della secondo istanza deve sapere se le entità sono nuovo (deve essere inserito) o esistente (deve essere aggiornato).</span><span class="sxs-lookup"><span data-stu-id="4956e-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

> [!TIP]  
> <span data-ttu-id="4956e-109">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="4956e-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) on GitHub.</span></span>

## <a name="identifying-new-entities"></a><span data-ttu-id="4956e-110">Identificazione di nuove entità</span><span class="sxs-lookup"><span data-stu-id="4956e-110">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="4956e-111">Client identifica nuove entità</span><span class="sxs-lookup"><span data-stu-id="4956e-111">Client identifies new entities</span></span>

<span data-ttu-id="4956e-112">Il caso più semplice da affrontare è quando il client comunica al server se l'entità è nuovo o esistente.</span><span class="sxs-lookup"><span data-stu-id="4956e-112">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="4956e-113">Ad esempio, spesso la richiesta di inserire una nuova entità è diversa dalla richiesta per aggiornare un'entità esistente.</span><span class="sxs-lookup"><span data-stu-id="4956e-113">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="4956e-114">Nella parte restante di questa sezione vengono illustrati i casi in cui è necessario determinare in altro modo per inserire o aggiornare.</span><span class="sxs-lookup"><span data-stu-id="4956e-114">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="4956e-115">Con le chiavi generate automaticamente</span><span class="sxs-lookup"><span data-stu-id="4956e-115">With auto-generated keys</span></span>

<span data-ttu-id="4956e-116">Il valore di una chiave generata automaticamente spesso può essere utilizzato per determinare se un'entità deve essere inserita o aggiornata.</span><span class="sxs-lookup"><span data-stu-id="4956e-116">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="4956e-117">Se la chiave non è stato impostato (ad esempio ancora il valore predefinito CLR di null, zero e così via), quindi l'entità deve essere nuovo e inserimento.</span><span class="sxs-lookup"><span data-stu-id="4956e-117">If the key has not been set (i.e. it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="4956e-118">D'altra parte, se è stato impostato il valore della chiave, quindi deve essere già stato precedentemente salvato e ora è necessario aggiornare.</span><span class="sxs-lookup"><span data-stu-id="4956e-118">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="4956e-119">In altre parole, se la chiave ha un valore, quindi l'entità è stata eseguita una query, inviato al client e ha ora tornare da aggiornare.</span><span class="sxs-lookup"><span data-stu-id="4956e-119">In other words, if the key has a value, then entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="4956e-120">È facile verificare la presenza di una chiave non impostata quando è noto il tipo di entità:</span><span class="sxs-lookup"><span data-stu-id="4956e-120">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="4956e-121">Tuttavia, EF dispone anche di un metodo incorporato per eseguire questa operazione per qualsiasi tipo di entità e tipo di chiave:</span><span class="sxs-lookup"><span data-stu-id="4956e-121">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="4956e-122">Chiavi sono impostate come entità vengono rilevate dal contesto, anche se l'entità è nello stato Added.</span><span class="sxs-lookup"><span data-stu-id="4956e-122">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="4956e-123">In questo modo, quando si attraversano un grafico di entità e decidere cosa fare con ogni tipo, ad esempio, quando si utilizza l'API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="4956e-123">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="4956e-124">Il valore della chiave deve essere utilizzato solo nel modo illustrato di seguito _prima_ viene effettuata qualsiasi chiamata a tenere traccia dell'entità.</span><span class="sxs-lookup"><span data-stu-id="4956e-124">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="4956e-125">Con altre chiavi</span><span class="sxs-lookup"><span data-stu-id="4956e-125">With other keys</span></span>

<span data-ttu-id="4956e-126">Un altro meccanismo è necessario per identificare nuove entità quando i valori di chiave non vengono generati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="4956e-126">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="4956e-127">Esistono due approcci generali per questo:</span><span class="sxs-lookup"><span data-stu-id="4956e-127">There are two general approaches to this:</span></span>
 * <span data-ttu-id="4956e-128">Query per l'entità</span><span class="sxs-lookup"><span data-stu-id="4956e-128">Query for the entity</span></span>
 * <span data-ttu-id="4956e-129">Passare un flag dal client</span><span class="sxs-lookup"><span data-stu-id="4956e-129">Pass a flag from the client</span></span>

<span data-ttu-id="4956e-130">Per eseguire una query per l'entità, è sufficiente utilizzare il metodo di ricerca:</span><span class="sxs-lookup"><span data-stu-id="4956e-130">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="4956e-131">Rientra nell'ambito di questo documento per visualizzare il codice completo per il passaggio di un flag da un client.</span><span class="sxs-lookup"><span data-stu-id="4956e-131">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="4956e-132">In un'app web, in genere significa che effettua richieste diverse per diverse azioni, o passando a uno stato della richiesta, quindi estraendolo nel controller.</span><span class="sxs-lookup"><span data-stu-id="4956e-132">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="4956e-133">Salvataggio delle entità singola</span><span class="sxs-lookup"><span data-stu-id="4956e-133">Saving single entities</span></span>

<span data-ttu-id="4956e-134">Se è noto o meno un'istruzione insert o update è necessaria, quindi Aggiungi o aggiorna può essere utilizzato in modo appropriato:</span><span class="sxs-lookup"><span data-stu-id="4956e-134">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="4956e-135">Tuttavia, se l'entità utilizza valori di chiave generato automaticamente, il metodo Update può essere utilizzato per entrambi i casi:</span><span class="sxs-lookup"><span data-stu-id="4956e-135">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="4956e-136">Il metodo di aggiornamento è in genere contrassegna l'entità per l'aggiornamento, inserimento non.</span><span class="sxs-lookup"><span data-stu-id="4956e-136">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="4956e-137">Tuttavia, l'entità dispone di una chiave generata automaticamente, se non è stato impostato alcun valore di chiave, quindi l'entità invece viene contrassegnato automaticamente per inserire.</span><span class="sxs-lookup"><span data-stu-id="4956e-137">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="4956e-138">Questo comportamento è stato introdotto in Entity Framework Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="4956e-138">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="4956e-139">Per le versioni precedenti è sempre necessario selezionare in modo esplicito l'opzione Aggiungi o Aggiorna.</span><span class="sxs-lookup"><span data-stu-id="4956e-139">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="4956e-140">Se l'entità non utilizza le chiavi generate automaticamente, quindi l'applicazione deve decidere se l'entità deve essere inserita o aggiornata: ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4956e-140">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="4956e-141">Ecco i passaggi sono:</span><span class="sxs-lookup"><span data-stu-id="4956e-141">The steps here are:</span></span>
* <span data-ttu-id="4956e-142">Se trova restituisce null, il database non contiene già il blog con tale ID, pertanto è chiamare aggiungere contrassegnarla per l'inserimento.</span><span class="sxs-lookup"><span data-stu-id="4956e-142">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="4956e-143">Se trova restituisce un'entità, quindi esista nel database e il contesto ora sta rilevando l'entità esistente</span><span class="sxs-lookup"><span data-stu-id="4956e-143">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="4956e-144">È quindi possibile utilizzare SetValues per impostare i valori per tutte le proprietà nell'entità a quelle fornite dal client.</span><span class="sxs-lookup"><span data-stu-id="4956e-144">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="4956e-145">La chiamata SetValues contrassegnerà l'entità da aggiornare in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="4956e-145">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="4956e-146">SetValues solo viene contrassegnato come modificato le proprietà che hanno valori diversi da quelli in entità rilevata.</span><span class="sxs-lookup"><span data-stu-id="4956e-146">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="4956e-147">Ciò significa che quando viene inviato l'aggiornamento, verranno aggiornate solo le colonne effettivamente modificati.</span><span class="sxs-lookup"><span data-stu-id="4956e-147">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="4956e-148">(E se è stato modificato nulla, verrà inviato alcun aggiornamento affatto.)</span><span class="sxs-lookup"><span data-stu-id="4956e-148">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="4956e-149">Utilizzo dei grafici</span><span class="sxs-lookup"><span data-stu-id="4956e-149">Working with graphs</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="4956e-150">Tutte le entità esistenti con nuove/all</span><span class="sxs-lookup"><span data-stu-id="4956e-150">All new/all existing entities</span></span>

<span data-ttu-id="4956e-151">Un esempio di utilizzo di grafici inserimento o aggiornamento di un blog con la raccolta di messaggi associati.</span><span class="sxs-lookup"><span data-stu-id="4956e-151">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="4956e-152">Se tutte le entità nel grafico devono essere inserite o devono essere aggiornati, il processo è identico a quello descritto sopra per singole entità.</span><span class="sxs-lookup"><span data-stu-id="4956e-152">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="4956e-153">Ad esempio, un grafico di blog e annunci simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="4956e-153">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="4956e-154">è possibile inserire simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="4956e-154">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="4956e-155">La chiamata al metodo Add contrassegna il blog che tutti i post da inserire.</span><span class="sxs-lookup"><span data-stu-id="4956e-155">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="4956e-156">Analogamente, se necessario aggiornare tutte le entità in un grafico, quindi aggiornamento può essere utilizzato:</span><span class="sxs-lookup"><span data-stu-id="4956e-156">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="4956e-157">Il blog e tutte le relative pubblicazioni verranno contrassegnate per essere aggiornati.</span><span class="sxs-lookup"><span data-stu-id="4956e-157">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="4956e-158">Combinazione di entità nuove ed esistenti</span><span class="sxs-lookup"><span data-stu-id="4956e-158">Mix of new and existing entities</span></span>

<span data-ttu-id="4956e-159">Con le chiavi generate automaticamente, Update può essere utilizzato nuovamente per gli inserimenti e aggiornamenti, anche se il grafico contiene una combinazione di entità che richiedono l'inserimento e quelli che richiedono l'aggiornamento:</span><span class="sxs-lookup"><span data-stu-id="4956e-159">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="4956e-160">Aggiornamento contrassegnerà qualsiasi entità nel grafico, blog o post, per l'inserimento, se non dispone di un set di valore della chiave, mentre tutte le altre entità sono contrassegnati per l'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="4956e-160">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="4956e-161">Come prima, se non si usa chiavi generate automaticamente, una query e alcune operazioni di elaborazione possono essere utilizzati:</span><span class="sxs-lookup"><span data-stu-id="4956e-161">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="4956e-162">Gestione delle eliminazioni</span><span class="sxs-lookup"><span data-stu-id="4956e-162">Handling deletes</span></span>

<span data-ttu-id="4956e-163">Delete può essere difficile da gestire dall'assenza di un'entità indica spesso che deve essere eliminato.</span><span class="sxs-lookup"><span data-stu-id="4956e-163">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="4956e-164">Un modo per risolvere questo problema consiste nell'utilizzare "eliminazioni reversibili" in modo che l'entità viene contrassegnato come eliminato anziché effettivamente in corso l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="4956e-164">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="4956e-165">Elimina quindi assume lo stesso come aggiornamenti.</span><span class="sxs-lookup"><span data-stu-id="4956e-165">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="4956e-166">Eliminazioni reversibili possono essere implementate utilizzando [i filtri di query](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="4956e-166">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="4956e-167">Per le eliminazioni true, un modello comune consiste nell'utilizzare un'estensione del modello di query per eseguire essenzialmente diff. un grafico Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4956e-167">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff. For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="4956e-168">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="4956e-168">TrackGraph</span></span>

<span data-ttu-id="4956e-169">Internamente, Add, collegamento e aggiornamento utilizzare attraversamento del grafico con una determinazione eseguita per ogni entità come se si deve essere contrassegnato come Added (per inserire), Modified (aggiornare), non modificato (non eseguire alcuna operazione), o eliminati (eliminare).</span><span class="sxs-lookup"><span data-stu-id="4956e-169">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="4956e-170">Questo meccanismo viene esposta tramite l'API TrackGraph.</span><span class="sxs-lookup"><span data-stu-id="4956e-170">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="4956e-171">Ad esempio, si supponga che, quando il client invia un grafico di entità imposta alcuni flag per ogni entità che indica la modalità di gestione.</span><span class="sxs-lookup"><span data-stu-id="4956e-171">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="4956e-172">TrackGraph può quindi essere utilizzato per elaborare questo flag:</span><span class="sxs-lookup"><span data-stu-id="4956e-172">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="4956e-173">I flag vengono visualizzati solo come parte dell'entità per semplicità dell'esempio.</span><span class="sxs-lookup"><span data-stu-id="4956e-173">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="4956e-174">In genere i flag potrebbero far parte di un DTO o qualche altro stato incluso nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="4956e-174">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
