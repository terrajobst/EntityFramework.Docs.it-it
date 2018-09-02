---
title: Eliminazione a catena - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: afe00ddb1b487c6b1b2ea42708c9967a57cea04b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995242"
---
# <a name="cascade-delete"></a><span data-ttu-id="e327e-102">Eliminazione a catena</span><span class="sxs-lookup"><span data-stu-id="e327e-102">Cascade Delete</span></span>

<span data-ttu-id="e327e-103">Nella terminologia dei database, il termine eliminazione a catena viene usato comunemente per descrivere una caratteristica che consente di attivare automaticamente l'eliminazione di righe correlate in seguito all'eliminazione di una riga.</span><span class="sxs-lookup"><span data-stu-id="e327e-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="e327e-104">Un concetto strettamente correlato, anch'esso coperto dai comportamenti di eliminazione di EF Core, è l'eliminazione automatica di un'entità figlio quando la relazione con un'entità padre è stata interrotta, operazione comunemente nota come "eliminazione degli orfani".</span><span class="sxs-lookup"><span data-stu-id="e327e-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this is commonly known as "deleting orphans".</span></span>

<span data-ttu-id="e327e-105">EF Core implementa vari comportamenti di eliminazione diversi e consente di configurare i comportamenti di eliminazione di singole relazioni.</span><span class="sxs-lookup"><span data-stu-id="e327e-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="e327e-106">EF Core implementa anche convenzioni per la configurazione automatica di comportamenti di eliminazione predefiniti utili per ogni relazione in base alla [obbligatorietà della relazione](../modeling/relationships.md#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="e327e-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship](../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="e327e-107">Comportamenti di eliminazione</span><span class="sxs-lookup"><span data-stu-id="e327e-107">Delete behaviors</span></span>
<span data-ttu-id="e327e-108">I comportamenti di eliminazione vengono definiti nel tipo di enumeratore *DeleteBehavior* e possono essere passati all'API fluent *OnDelete* per controllare se l'eliminazione di un'entità principale/padre o l'interruzione della relazione con le entità dipendenti/figlio deve avere un effetto collaterale sulle entità dipendenti/figlio.</span><span class="sxs-lookup"><span data-stu-id="e327e-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="e327e-109">Esistono tre azioni che EF può eseguire quando viene eliminata un'entità principale/padre o viene interrotta la relazione con l'entità figlio:</span><span class="sxs-lookup"><span data-stu-id="e327e-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>
* <span data-ttu-id="e327e-110">L'entità figlio/dipendente può essere eliminata</span><span class="sxs-lookup"><span data-stu-id="e327e-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="e327e-111">I valori di chiave esterna dell'entità figlio possono essere impostati su Null</span><span class="sxs-lookup"><span data-stu-id="e327e-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="e327e-112">L'entità figlio rimane invariata</span><span class="sxs-lookup"><span data-stu-id="e327e-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="e327e-113">Il comportamento di eliminazione configurato nel modello di EF Core viene applicato solo quando l'entità principale viene eliminata usando EF Core e le entità dipendenti vengono caricate in memoria (come nel caso delle entità dipendenti con rilevamento delle modifiche).</span><span class="sxs-lookup"><span data-stu-id="e327e-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (that is, for tracked dependents).</span></span> <span data-ttu-id="e327e-114">È necessario configurare un comportamento a catena corrispondente nel database per garantire l'applicazione dell'azione necessaria ai dati non sottoposti a rilevamento delle modifiche dal contesto.</span><span class="sxs-lookup"><span data-stu-id="e327e-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="e327e-115">Se si usa EF Core per creare il database, questo comportamento a catena verrà configurato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="e327e-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="e327e-116">Per la seconda azione indicata in precedenza, l'impostazione di un valore di chiave esterna su Null non è valida se la chiave esterna non ammette valori Null.</span><span class="sxs-lookup"><span data-stu-id="e327e-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="e327e-117">(Una chiave esterna che non ammette valori Null equivale a una relazione obbligatoria.) In questi casi, EF Core rileva che proprietà di chiave esterna è stata contrassegnata come Null fino a quando non viene chiamato SaveChanges e in quel momento viene generata un'eccezione in quanto la modifica non può essere salvata in modo permanente nel database.</span><span class="sxs-lookup"><span data-stu-id="e327e-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="e327e-118">La situazione è simile alla segnalazione di una violazione di vincolo dal database.</span><span class="sxs-lookup"><span data-stu-id="e327e-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="e327e-119">Sono disponibili quattro comportamenti di eliminazione, elencati nelle tabelle seguenti.</span><span class="sxs-lookup"><span data-stu-id="e327e-119">There are four delete behaviors, as listed in the tables below.</span></span>

### <a name="optional-relationships"></a><span data-ttu-id="e327e-120">Relazioni facoltative</span><span class="sxs-lookup"><span data-stu-id="e327e-120">Optional relationships</span></span>
<span data-ttu-id="e327e-121">Per le relazioni facoltative (chiave esterna che ammette valori Null), _è_ possibile salvare un valore di chiave esterna Null, con gli effetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="e327e-121">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="e327e-122">Nome del comportamento</span><span class="sxs-lookup"><span data-stu-id="e327e-122">Behavior Name</span></span>               | <span data-ttu-id="e327e-123">Effetto sull'entità dipendente/figlio in memoria</span><span class="sxs-lookup"><span data-stu-id="e327e-123">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="e327e-124">Effetto sull'entità dipendente/figlio nel database</span><span class="sxs-lookup"><span data-stu-id="e327e-124">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="e327e-125">**Cascade**</span><span class="sxs-lookup"><span data-stu-id="e327e-125">**Cascade**</span></span>                 | <span data-ttu-id="e327e-126">Le entità vengono eliminate</span><span class="sxs-lookup"><span data-stu-id="e327e-126">Entities are deleted</span></span>                   | <span data-ttu-id="e327e-127">Le entità vengono eliminate</span><span class="sxs-lookup"><span data-stu-id="e327e-127">Entities are deleted</span></span>                   |
| <span data-ttu-id="e327e-128">**ClientSetNull** (impostazione predefinita)</span><span class="sxs-lookup"><span data-stu-id="e327e-128">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="e327e-129">Le proprietà di chiave esterna vengono impostate su Null</span><span class="sxs-lookup"><span data-stu-id="e327e-129">Foreign key properties are set to null</span></span> | <span data-ttu-id="e327e-130">nessuno</span><span class="sxs-lookup"><span data-stu-id="e327e-130">None</span></span>                                   |
| <span data-ttu-id="e327e-131">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="e327e-131">**SetNull**</span></span>                 | <span data-ttu-id="e327e-132">Le proprietà di chiave esterna vengono impostate su Null</span><span class="sxs-lookup"><span data-stu-id="e327e-132">Foreign key properties are set to null</span></span> | <span data-ttu-id="e327e-133">Le proprietà di chiave esterna vengono impostate su Null</span><span class="sxs-lookup"><span data-stu-id="e327e-133">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="e327e-134">**Restrict**</span><span class="sxs-lookup"><span data-stu-id="e327e-134">**Restrict**</span></span>                | <span data-ttu-id="e327e-135">nessuno</span><span class="sxs-lookup"><span data-stu-id="e327e-135">None</span></span>                                   | <span data-ttu-id="e327e-136">nessuno</span><span class="sxs-lookup"><span data-stu-id="e327e-136">None</span></span>                                   |

### <a name="required-relationships"></a><span data-ttu-id="e327e-137">Relazioni obbligatorie</span><span class="sxs-lookup"><span data-stu-id="e327e-137">Required relationships</span></span>
<span data-ttu-id="e327e-138">Per le relazioni obbligatorie (chiave esterna che non ammette valori Null), _non_ è possibile salvare un valore di chiave esterna Null, con gli effetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="e327e-138">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="e327e-139">Nome del comportamento</span><span class="sxs-lookup"><span data-stu-id="e327e-139">Behavior Name</span></span>         | <span data-ttu-id="e327e-140">Effetto sull'entità dipendente/figlio in memoria</span><span class="sxs-lookup"><span data-stu-id="e327e-140">Effect on dependent/child in memory</span></span> | <span data-ttu-id="e327e-141">Effetto sull'entità dipendente/figlio nel database</span><span class="sxs-lookup"><span data-stu-id="e327e-141">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="e327e-142">**Cascade** (impostazione predefinita)</span><span class="sxs-lookup"><span data-stu-id="e327e-142">**Cascade** (Default)</span></span> | <span data-ttu-id="e327e-143">Le entità vengono eliminate</span><span class="sxs-lookup"><span data-stu-id="e327e-143">Entities are deleted</span></span>                | <span data-ttu-id="e327e-144">Le entità vengono eliminate</span><span class="sxs-lookup"><span data-stu-id="e327e-144">Entities are deleted</span></span>                  |
| <span data-ttu-id="e327e-145">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="e327e-145">**ClientSetNull**</span></span>     | <span data-ttu-id="e327e-146">SaveChanges genera un'eccezione</span><span class="sxs-lookup"><span data-stu-id="e327e-146">SaveChanges throws</span></span>                  | <span data-ttu-id="e327e-147">nessuno</span><span class="sxs-lookup"><span data-stu-id="e327e-147">None</span></span>                                  |
| <span data-ttu-id="e327e-148">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="e327e-148">**SetNull**</span></span>           | <span data-ttu-id="e327e-149">SaveChanges genera un'eccezione</span><span class="sxs-lookup"><span data-stu-id="e327e-149">SaveChanges throws</span></span>                  | <span data-ttu-id="e327e-150">SaveChanges genera un'eccezione</span><span class="sxs-lookup"><span data-stu-id="e327e-150">SaveChanges throws</span></span>                    |
| <span data-ttu-id="e327e-151">**Restrict**</span><span class="sxs-lookup"><span data-stu-id="e327e-151">**Restrict**</span></span>          | <span data-ttu-id="e327e-152">nessuno</span><span class="sxs-lookup"><span data-stu-id="e327e-152">None</span></span>                                | <span data-ttu-id="e327e-153">nessuno</span><span class="sxs-lookup"><span data-stu-id="e327e-153">None</span></span>                                  |

<span data-ttu-id="e327e-154">Nelle tabelle precedenti, la condizione *Nessuno* può comportare una violazione di vincolo.</span><span class="sxs-lookup"><span data-stu-id="e327e-154">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="e327e-155">Ad esempio, se viene eliminata un'entità principale/figlio, ma non viene eseguita alcuna azione per modificare la chiave esterna di un'entità dipendente/figlio, il database genererà probabilmente eccezioni durante SaveChanges a causa di una violazione del vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="e327e-155">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="e327e-156">In generale:</span><span class="sxs-lookup"><span data-stu-id="e327e-156">At a high level:</span></span>
* <span data-ttu-id="e327e-157">In presenza di entità che non possono esistere senza un padre e se si vuole che EF gestisca automaticamente l'eliminazione delle entità figlio, usare *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="e327e-157">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="e327e-158">Le entità che non possono esistere senza un'entità padre in genere usano relazioni obbligatorie, per le quali *Cascade* è l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="e327e-158">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="e327e-159">Con entità che possono o meno avere un'entità padre e se si vuole che EF gestisca automaticamente l'impostazione su Null della chiave esterna, usare *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="e327e-159">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="e327e-160">Le entità che possono esistere senza un'entità padre in genere usano relazioni facoltative, per le quali *ClientSetNull* è l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="e327e-160">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="e327e-161">Se si vuole che il database tenti inoltre la propagazione dei valori Null alle chiavi esterne figlio, anche quando l'entità figlio non viene caricata, usare *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="e327e-161">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="e327e-162">Si noti, tuttavia, che il database deve supportare queste operazioni e la configurazione del database in questo modo può comportare altre restrizioni, rendendo spesso questa opzione impraticabile.</span><span class="sxs-lookup"><span data-stu-id="e327e-162">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="e327e-163">Questo è il motivo per cui *SetNull* non è l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="e327e-163">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="e327e-164">Se non si vuole che Core EF elimini mai automaticamente un'entità o imposti automaticamente su Null la chiave esterna, usare *Restrict*.</span><span class="sxs-lookup"><span data-stu-id="e327e-164">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="e327e-165">Si noti che questa operazione richiede che il codice mantenga sincronizzati manualmente le entità figlio e i relativi valori di chiave esterna. In caso contrario verranno generate eccezioni per i vincoli.</span><span class="sxs-lookup"><span data-stu-id="e327e-165">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="e327e-166">In EF Core, a differenza di EF6, gli effetti a catena non vengono eseguiti immediatamente, ma solo quando viene chiamato SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="e327e-166">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="e327e-167">**Modifiche in EF Core 2.0:** nelle versioni precedenti *Restrict* causerebbe l'impostazione su Null delle proprietà di chiave esterna facoltative nelle entità dipendenti sottoposte a rilevamento delle modifiche e questo era il comportamento di eliminazione predefinito per le relazioni facoltative.</span><span class="sxs-lookup"><span data-stu-id="e327e-167">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="e327e-168">In EF Core 2.0 è stato introdotto il valore *ClientSetNull* per rappresentare tale comportamento ed è diventato il valore predefinito per le relazioni facoltative.</span><span class="sxs-lookup"><span data-stu-id="e327e-168">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="e327e-169">Il comportamento di *Restrict* è stato modificato in modo da non produrre mai effetti collaterali sulle entità dipendenti.</span><span class="sxs-lookup"><span data-stu-id="e327e-169">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="e327e-170">Esempi di eliminazione di entità</span><span class="sxs-lookup"><span data-stu-id="e327e-170">Entity deletion examples</span></span>

<span data-ttu-id="e327e-171">Il codice seguente fa parte di un [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) che può essere scaricato ed eseguito.</span><span class="sxs-lookup"><span data-stu-id="e327e-171">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="e327e-172">L'esempio mostra cosa accade per ogni comportamento di eliminazione sia per le relazioni obbligatorie che facoltative, quando viene eliminata un'entità padre.</span><span class="sxs-lookup"><span data-stu-id="e327e-172">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="e327e-173">Ogni variazione verrà analizzata per capire il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="e327e-173">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="e327e-174">DeleteBehavior.Cascade con relazione obbligatoria o facoltativa</span><span class="sxs-lookup"><span data-stu-id="e327e-174">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="e327e-175">Il blog viene contrassegnato come eliminato (Deleted)</span><span class="sxs-lookup"><span data-stu-id="e327e-175">Blog is marked as Deleted</span></span>
* <span data-ttu-id="e327e-176">I post rimangono invariati (Unchanged) dato che le operazioni a catena non vengono eseguite fino a SaveChanges</span><span class="sxs-lookup"><span data-stu-id="e327e-176">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="e327e-177">SaveChanges invia eliminazioni per entrambe le entità dipendenti/figlio (post) e poi per l'entità principale/padre (blog)</span><span class="sxs-lookup"><span data-stu-id="e327e-177">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="e327e-178">Dopo il salvataggio, tutte le entità vengono scollegate perché a questo punto sono state eliminate dal database</span><span class="sxs-lookup"><span data-stu-id="e327e-178">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="e327e-179">DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relazione obbligatoria</span><span class="sxs-lookup"><span data-stu-id="e327e-179">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="e327e-180">Il blog viene contrassegnato come eliminato (Deleted)</span><span class="sxs-lookup"><span data-stu-id="e327e-180">Blog is marked as Deleted</span></span>
* <span data-ttu-id="e327e-181">I post rimangono invariati (Unchanged) dato che le operazioni a catena non vengono eseguite fino a SaveChanges</span><span class="sxs-lookup"><span data-stu-id="e327e-181">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="e327e-182">SaveChanges tenta di impostare la chiave esterna del post su Null, ma l'operazione non riesce perché la chiave esterna non ammette valori Null</span><span class="sxs-lookup"><span data-stu-id="e327e-182">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="e327e-183">DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relazione facoltativa</span><span class="sxs-lookup"><span data-stu-id="e327e-183">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="e327e-184">Il blog viene contrassegnato come eliminato (Deleted)</span><span class="sxs-lookup"><span data-stu-id="e327e-184">Blog is marked as Deleted</span></span>
* <span data-ttu-id="e327e-185">I post rimangono invariati (Unchanged) dato che le operazioni a catena non vengono eseguite fino a SaveChanges</span><span class="sxs-lookup"><span data-stu-id="e327e-185">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="e327e-186">SaveChanges tenta di impostare la chiave esterna di entrambe le entità dipendenti/figlio (post) su Null prima di eliminare l'entità principale/padre (blog)</span><span class="sxs-lookup"><span data-stu-id="e327e-186">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="e327e-187">Dopo il salvataggio, l'entità principale/padre (blog) viene eliminata, ma le entità dipendenti/figlio (post) sono ancora incluse nel rilevamento delle modifiche</span><span class="sxs-lookup"><span data-stu-id="e327e-187">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="e327e-188">Le entità dipendenti/figlio (post) incluse nel rilevamento delle modifiche hanno ora valori di chiave esterna Null e il riferimento all'entità principale/padre (blog) è stato rimosso</span><span class="sxs-lookup"><span data-stu-id="e327e-188">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="e327e-189">DeleteBehavior.Restrict con relazione obbligatoria o facoltativa</span><span class="sxs-lookup"><span data-stu-id="e327e-189">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="e327e-190">Il blog viene contrassegnato come eliminato (Deleted)</span><span class="sxs-lookup"><span data-stu-id="e327e-190">Blog is marked as Deleted</span></span>
* <span data-ttu-id="e327e-191">I post rimangono invariati (Unchanged) dato che le operazioni a catena non vengono eseguite fino a SaveChanges</span><span class="sxs-lookup"><span data-stu-id="e327e-191">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="e327e-192">Poiché *Restrict* indica a EF di non impostare automaticamente la chiave esterna su Null, la chiave rimane non Null e SaveChanges genera un'eccezione senza salvare le modifiche</span><span class="sxs-lookup"><span data-stu-id="e327e-192">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="e327e-193">Esempi di eliminazione degli orfani</span><span class="sxs-lookup"><span data-stu-id="e327e-193">Delete orphans examples</span></span>

<span data-ttu-id="e327e-194">Il codice seguente fa parte di un [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) che può essere scaricato ed eseguito.</span><span class="sxs-lookup"><span data-stu-id="e327e-194">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="e327e-195">L'esempio illustra le conseguenze di ogni comportamento di eliminazione sia per le relazioni facoltative che per quelle obbligatorie, quando viene interrotta la relazione tra un'entità padre/principale e le relative entità figlio/dipendenti.</span><span class="sxs-lookup"><span data-stu-id="e327e-195">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="e327e-196">In questo esempio, la relazione viene interrotta rimuovendo gli oggetti dipendenti o elementi figlio (POST) dalla proprietà di navigazione della raccolta nell'entità principale/padre (blog).</span><span class="sxs-lookup"><span data-stu-id="e327e-196">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="e327e-197">Tuttavia, il comportamento è lo stesso se il riferimento da entità dipendente/figlio a entità principale/padre viene invece impostato su Null.</span><span class="sxs-lookup"><span data-stu-id="e327e-197">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="e327e-198">Ogni variazione verrà analizzata per capire il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="e327e-198">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="e327e-199">DeleteBehavior.Cascade con relazione obbligatoria o facoltativa</span><span class="sxs-lookup"><span data-stu-id="e327e-199">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="e327e-200">I post vengono contrassegnati come modificati perché, a causa dell'interruzione della relazione, la chiave esterna è stata contrassegnata come Null</span><span class="sxs-lookup"><span data-stu-id="e327e-200">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="e327e-201">Se la chiave esterna non ammette valori Null, il valore effettivo non cambierà anche se è contrassegnato come Null</span><span class="sxs-lookup"><span data-stu-id="e327e-201">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="e327e-202">SaveChanges invia eliminazioni per le entità dipendenti/figlio (post)</span><span class="sxs-lookup"><span data-stu-id="e327e-202">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="e327e-203">Dopo il salvataggio, le entità dipendenti/figlio (post) vengono scollegate perché a questo punto sono state eliminate dal database</span><span class="sxs-lookup"><span data-stu-id="e327e-203">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="e327e-204">DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relazione obbligatoria</span><span class="sxs-lookup"><span data-stu-id="e327e-204">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="e327e-205">I post vengono contrassegnati come modificati perché, a causa dell'interruzione della relazione, la chiave esterna è stata contrassegnata come Null</span><span class="sxs-lookup"><span data-stu-id="e327e-205">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="e327e-206">Se la chiave esterna non ammette valori Null, il valore effettivo non cambierà anche se è contrassegnato come Null</span><span class="sxs-lookup"><span data-stu-id="e327e-206">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="e327e-207">SaveChanges tenta di impostare la chiave esterna del post su Null, ma l'operazione non riesce perché la chiave esterna non ammette valori Null</span><span class="sxs-lookup"><span data-stu-id="e327e-207">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="e327e-208">DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relazione facoltativa</span><span class="sxs-lookup"><span data-stu-id="e327e-208">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="e327e-209">I post vengono contrassegnati come modificati perché, a causa dell'interruzione della relazione, la chiave esterna è stata contrassegnata come Null</span><span class="sxs-lookup"><span data-stu-id="e327e-209">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="e327e-210">Se la chiave esterna non ammette valori Null, il valore effettivo non cambierà anche se è contrassegnato come Null</span><span class="sxs-lookup"><span data-stu-id="e327e-210">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="e327e-211">SaveChanges imposta la chiave esterna di entrambe le entità dipendenti/figlio (post) su Null</span><span class="sxs-lookup"><span data-stu-id="e327e-211">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="e327e-212">Dopo il salvataggio, le entità dipendenti/figlio (post) hanno ora valori di chiave esterna Null e il riferimento all'entità principale/padre (blog) è stato rimosso</span><span class="sxs-lookup"><span data-stu-id="e327e-212">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="e327e-213">DeleteBehavior.Restrict con relazione obbligatoria o facoltativa</span><span class="sxs-lookup"><span data-stu-id="e327e-213">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="e327e-214">I post vengono contrassegnati come modificati perché, a causa dell'interruzione della relazione, la chiave esterna è stata contrassegnata come Null</span><span class="sxs-lookup"><span data-stu-id="e327e-214">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="e327e-215">Se la chiave esterna non ammette valori Null, il valore effettivo non cambierà anche se è contrassegnato come Null</span><span class="sxs-lookup"><span data-stu-id="e327e-215">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="e327e-216">Poiché *Restrict* indica a EF di non impostare automaticamente la chiave esterna su Null, la chiave rimane non Null e SaveChanges genera un'eccezione senza salvare le modifiche</span><span class="sxs-lookup"><span data-stu-id="e327e-216">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="e327e-217">Applicazione a catena alle entità senza rilevamento delle modifiche</span><span class="sxs-lookup"><span data-stu-id="e327e-217">Cascading to untracked entities</span></span>

<span data-ttu-id="e327e-218">Quando si chiama *SaveChanges*, le regole di eliminazione a catena verranno applicate a tutte le entità incluse nel rilevamento delle modifiche dal contesto.</span><span class="sxs-lookup"><span data-stu-id="e327e-218">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="e327e-219">Si tratta della situazione esistente in tutti gli esempi illustrati in precedenza, che è il motivo per cui vengono generate istruzioni SQL per eliminare sia l'entità principale/padre (blog) che tutte le entità dipendenti/figlio (post):</span><span class="sxs-lookup"><span data-stu-id="e327e-219">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="e327e-220">Se viene caricata solo l'entità principale, ad esempio, quando viene eseguita una query per un blog senza `Include(b => b.Posts)` per includere anche i post, SaveChanges genererà solo le istruzioni SQL per eliminare l'entità principale/padre:</span><span class="sxs-lookup"><span data-stu-id="e327e-220">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="e327e-221">Le entità dipendenti/figlio (post) verranno eliminate solo se per il database è configurato un comportamento a catena corrispondente.</span><span class="sxs-lookup"><span data-stu-id="e327e-221">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="e327e-222">Se si usa EF per creare il database, questo comportamento a catena verrà configurato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="e327e-222">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
