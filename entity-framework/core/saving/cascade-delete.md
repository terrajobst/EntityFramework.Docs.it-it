---
title: Delete - Core EF a catena
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: 1ab9d114e27aac0bec972df631a426c8ce87a518
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2018
---
# <a name="cascade-delete"></a><span data-ttu-id="48328-102">Eliminazione a catena</span><span class="sxs-lookup"><span data-stu-id="48328-102">Cascade Delete</span></span>

<span data-ttu-id="48328-103">Nella terminologia dei database, eliminazione a catena in genere viene utilizzato per descrivere una caratteristica che consente l'eliminazione di una riga per attivare automaticamente l'eliminazione di righe correlate.</span><span class="sxs-lookup"><span data-stu-id="48328-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="48328-104">Un concetto strettamente correlato rientrano anche i comportamenti di eliminazione EF Core è l'eliminazione automatica di un'entità figlio quando si tratta di una relazione a un elemento padre è stata interrotta--questo i comunemente noto come "eliminazione orfani".</span><span class="sxs-lookup"><span data-stu-id="48328-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this i commonly known as "deleting orphans".</span></span>

<span data-ttu-id="48328-105">Core EF implementa alcuni comportamenti diversi delete e consente la configurazione dei comportamenti di eliminazione di relazioni singoli.</span><span class="sxs-lookup"><span data-stu-id="48328-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="48328-106">Componenti di base di Entity Framework implementa inoltre convenzioni che consente di configurare automaticamente i comportamenti di eliminazione predefiniti utili per ogni relazione basata su [requiredness della relazione] (../modeling/relationships.md#required-and-optional-relationships).</span><span class="sxs-lookup"><span data-stu-id="48328-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship] (../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="48328-107">Eliminare i comportamenti</span><span class="sxs-lookup"><span data-stu-id="48328-107">Delete behaviors</span></span>
<span data-ttu-id="48328-108">Eliminare i comportamenti vengono definiti nel *DeleteBehavior* enumeratore digitare e può essere passato al *OnDelete* fluent API per controllare se l'eliminazione di un'entità principale o padre o il interruzione del relazione con l'entità dipendente/figlio deve ha un effetto collaterale sulle entità dipendente/figlio.</span><span class="sxs-lookup"><span data-stu-id="48328-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="48328-109">Esistono tre azioni EF quando viene eliminata un'entità principale o padre o la relazione per l'elemento figlio viene interrotto:</span><span class="sxs-lookup"><span data-stu-id="48328-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>
* <span data-ttu-id="48328-110">Figlio/dipendente può essere eliminato.</span><span class="sxs-lookup"><span data-stu-id="48328-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="48328-111">Valori di chiave esterna del figlio possono essere impostati su null</span><span class="sxs-lookup"><span data-stu-id="48328-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="48328-112">L'elemento figlio rimane invariato</span><span class="sxs-lookup"><span data-stu-id="48328-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="48328-113">Il comportamento di eliminazione configurato nel modello di Entity Framework Core viene applicato solo quando l'entità principale viene eliminato usando EF Core e le entità dipendenti vengono caricate in memoria (ad esempio per il rilevamento dipendenti).</span><span class="sxs-lookup"><span data-stu-id="48328-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (i.e. for tracked dependents).</span></span> <span data-ttu-id="48328-114">Un comportamento di propagazione corrispondente deve essere il programma di installazione del database per garantire che i dati che non viene rilevati dal contesto è necessario azione applicata.</span><span class="sxs-lookup"><span data-stu-id="48328-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="48328-115">Se si utilizza Entity Framework Core per creare il database, questo comportamento cascade verrà configurato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="48328-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="48328-116">Per la seconda azione precedente, l'impostazione di un valore di chiave esterna su null non è valido se la chiave esterna non ammette valori null.</span><span class="sxs-lookup"><span data-stu-id="48328-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="48328-117">(Una chiave esterna non ammettono valori null è equivalente a una relazione obbligatoria). In questi casi, Core EF rileva che la proprietà di chiave esterna è stata contrassegnata come null fino a quando non viene chiamato SaveChanges, nel qual caso viene generata un'eccezione in quanto la modifica non può essere reso persistente nel database.</span><span class="sxs-lookup"><span data-stu-id="48328-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="48328-118">È simile a ottenere una violazione del vincolo dal database.</span><span class="sxs-lookup"><span data-stu-id="48328-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="48328-119">Sono disponibili quattro eliminazione comportamenti, elencati nelle tabelle seguenti.</span><span class="sxs-lookup"><span data-stu-id="48328-119">There are four delete behaviors, as listed in the tables below.</span></span> <span data-ttu-id="48328-120">Per le relazioni facoltative (chiave esterna che ammette valori null), _è_ possibile salvare un null valore di chiave esterna, che determina gli effetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="48328-120">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="48328-121">Nome del comportamento</span><span class="sxs-lookup"><span data-stu-id="48328-121">Behavior Name</span></span>               | <span data-ttu-id="48328-122">Effetto sul dipendente/figlio in memoria</span><span class="sxs-lookup"><span data-stu-id="48328-122">Effect on dependent/child in memory</span></span>    | <span data-ttu-id="48328-123">Effetto sul dipendente/figlio nel database</span><span class="sxs-lookup"><span data-stu-id="48328-123">Effect on dependent/child in database</span></span>  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| <span data-ttu-id="48328-124">**Cascade**</span><span class="sxs-lookup"><span data-stu-id="48328-124">**Cascade**</span></span>                 | <span data-ttu-id="48328-125">Le entità vengono eliminate.</span><span class="sxs-lookup"><span data-stu-id="48328-125">Entities are deleted</span></span>                   | <span data-ttu-id="48328-126">Le entità vengono eliminate.</span><span class="sxs-lookup"><span data-stu-id="48328-126">Entities are deleted</span></span>                   |
| <span data-ttu-id="48328-127">**ClientSetNull** (predefinito)</span><span class="sxs-lookup"><span data-stu-id="48328-127">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="48328-128">Proprietà di chiave esterna sono impostate su null</span><span class="sxs-lookup"><span data-stu-id="48328-128">Foreign key properties are set to null</span></span> | <span data-ttu-id="48328-129">nessuno</span><span class="sxs-lookup"><span data-stu-id="48328-129">None</span></span>                                   |
| <span data-ttu-id="48328-130">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="48328-130">**SetNull**</span></span>                 | <span data-ttu-id="48328-131">Proprietà di chiave esterna sono impostate su null</span><span class="sxs-lookup"><span data-stu-id="48328-131">Foreign key properties are set to null</span></span> | <span data-ttu-id="48328-132">Proprietà di chiave esterna sono impostate su null</span><span class="sxs-lookup"><span data-stu-id="48328-132">Foreign key properties are set to null</span></span> |
| <span data-ttu-id="48328-133">**Limitare**</span><span class="sxs-lookup"><span data-stu-id="48328-133">**Restrict**</span></span>                | <span data-ttu-id="48328-134">nessuno</span><span class="sxs-lookup"><span data-stu-id="48328-134">None</span></span>                                   | <span data-ttu-id="48328-135">nessuno</span><span class="sxs-lookup"><span data-stu-id="48328-135">None</span></span>                                   |

<span data-ttu-id="48328-136">Per le relazioni necessarie (chiave esterna non nullable) è _non_ possibile salvare un null valore di chiave esterna, che determina gli effetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="48328-136">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="48328-137">Nome del comportamento</span><span class="sxs-lookup"><span data-stu-id="48328-137">Behavior Name</span></span>         | <span data-ttu-id="48328-138">Effetto sul dipendente/figlio in memoria</span><span class="sxs-lookup"><span data-stu-id="48328-138">Effect on dependent/child in memory</span></span> | <span data-ttu-id="48328-139">Effetto sul dipendente/figlio nel database</span><span class="sxs-lookup"><span data-stu-id="48328-139">Effect on dependent/child in database</span></span> |
|:----------------------|:------------------------------------|:--------------------------------------|
| <span data-ttu-id="48328-140">**CASCADE** (predefinito)</span><span class="sxs-lookup"><span data-stu-id="48328-140">**Cascade** (Default)</span></span> | <span data-ttu-id="48328-141">Le entità vengono eliminate.</span><span class="sxs-lookup"><span data-stu-id="48328-141">Entities are deleted</span></span>                | <span data-ttu-id="48328-142">Le entità vengono eliminate.</span><span class="sxs-lookup"><span data-stu-id="48328-142">Entities are deleted</span></span>                  |
| <span data-ttu-id="48328-143">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="48328-143">**ClientSetNull**</span></span>     | <span data-ttu-id="48328-144">Genera SaveChanges</span><span class="sxs-lookup"><span data-stu-id="48328-144">SaveChanges throws</span></span>                  | <span data-ttu-id="48328-145">nessuno</span><span class="sxs-lookup"><span data-stu-id="48328-145">None</span></span>                                  |
| <span data-ttu-id="48328-146">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="48328-146">**SetNull**</span></span>           | <span data-ttu-id="48328-147">Genera SaveChanges</span><span class="sxs-lookup"><span data-stu-id="48328-147">SaveChanges throws</span></span>                  | <span data-ttu-id="48328-148">Genera SaveChanges</span><span class="sxs-lookup"><span data-stu-id="48328-148">SaveChanges throws</span></span>                    |
| <span data-ttu-id="48328-149">**Limitare**</span><span class="sxs-lookup"><span data-stu-id="48328-149">**Restrict**</span></span>          | <span data-ttu-id="48328-150">nessuno</span><span class="sxs-lookup"><span data-stu-id="48328-150">None</span></span>                                | <span data-ttu-id="48328-151">nessuno</span><span class="sxs-lookup"><span data-stu-id="48328-151">None</span></span>                                  |

<span data-ttu-id="48328-152">Nelle tabelle precedenti, *Nessuno* può comportare una violazione del vincolo.</span><span class="sxs-lookup"><span data-stu-id="48328-152">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="48328-153">Ad esempio, se viene eliminata un'entità principale/figlio, ma viene eseguita alcuna azione per modificare la chiave esterna di un dipendente/figlio, quindi il database verrà probabilmente genera eccezioni in SaveChanges a causa di una violazione del vincolo foreign.</span><span class="sxs-lookup"><span data-stu-id="48328-153">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="48328-154">In generale:</span><span class="sxs-lookup"><span data-stu-id="48328-154">At a high level:</span></span>
* <span data-ttu-id="48328-155">Se si dispone di entità che non può esistere senza un padre e desiderate EF da svolgere per eliminare automaticamente gli elementi figlio, quindi utilizzare *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="48328-155">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="48328-156">Le entità che non possono esistere senza un elemento padre in genere assicurarsi utilizzare relazioni necessarie, per cui *Cascade* è l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="48328-156">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="48328-157">Se si dispone di entità che possono o non dispone di un padre ed EF di occuparsi di Annulla della chiave esterna per l'utente desiderato, quindi utilizzare *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="48328-157">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="48328-158">Entità che possono esistere senza un elemento padre in genere assicurarsi utilizzare relazioni facoltative, per cui *ClientSetNull* è l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="48328-158">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="48328-159">Se si desidera il database anche provare a propagare anche i valori null per le chiavi esterne figlio quando l'entità figlio non viene caricato, quindi utilizzare *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="48328-159">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="48328-160">Si noti tuttavia che il database deve supportare questo e configurazione del database, questo può comportare altre restrizioni, in pratica spesso rendendo questa opzione causa.</span><span class="sxs-lookup"><span data-stu-id="48328-160">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="48328-161">Questo è il motivo *SetNull* non è il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="48328-161">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="48328-162">Se non si desidera Core EF mai eliminare automaticamente un'entità o null automaticamente della chiave esterna, quindi utilizzare *Restrict*.</span><span class="sxs-lookup"><span data-stu-id="48328-162">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="48328-163">Si noti che questa operazione richiede che il codice sincronizzare entità figlio e i relativi valori di chiave esterna manualmente in caso contrario vincolo verranno generate eccezioni.</span><span class="sxs-lookup"><span data-stu-id="48328-163">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="48328-164">In Entity Framework Core, a differenza di EF6, effetti a catena non vengono eseguiti immediatamente, ma invece solo quando viene chiamato SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="48328-164">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="48328-165">**Le modifiche in EF Core 2.0:** nelle versioni precedenti, *Restrict* causerebbe facoltativa proprietà di chiave esterna nelle entità dipendenti rilevate sia impostato su null e il comportamento per le relazioni facoltative di eliminare il valore predefinito è stato.</span><span class="sxs-lookup"><span data-stu-id="48328-165">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="48328-166">In Entity Framework Core 2.0 il *ClientSetNull* è stato introdotto per rappresentare tale comportamento ed è diventato il valore predefinito per le relazioni facoltative.</span><span class="sxs-lookup"><span data-stu-id="48328-166">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="48328-167">Il comportamento di *Restrict* regolato per mai avere effetti collaterali su entità dipendente.</span><span class="sxs-lookup"><span data-stu-id="48328-167">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="48328-168">Esempi di eliminazione di entità</span><span class="sxs-lookup"><span data-stu-id="48328-168">Entity deletion examples</span></span>

<span data-ttu-id="48328-169">Il codice seguente fa parte di un [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) che può essere scaricato ed eseguito.</span><span class="sxs-lookup"><span data-stu-id="48328-169">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded and run.</span></span> <span data-ttu-id="48328-170">Nell'esempio viene illustrato cosa accade per ogni comportamento di eliminazione per le relazioni obbligatori e facoltative, quando viene eliminata un'entità padre.</span><span class="sxs-lookup"><span data-stu-id="48328-170">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="48328-171">Si analizzerà ogni variazione alla capire cosa succede.</span><span class="sxs-lookup"><span data-stu-id="48328-171">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="48328-172">DeleteBehavior.Cascade con relazione obbligatorio o facoltativo</span><span class="sxs-lookup"><span data-stu-id="48328-172">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="48328-173">Blog è contrassegnato come eliminato</span><span class="sxs-lookup"><span data-stu-id="48328-173">Blog is marked as Deleted</span></span>
* <span data-ttu-id="48328-174">Post di rimanere inizialmente non modificato dal momento che a catena non vengono eseguiti fino a quando SaveChanges</span><span class="sxs-lookup"><span data-stu-id="48328-174">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="48328-175">SaveChanges invia eliminazioni per oggetti dipendenti (POST) o gli elementi figlio e quindi l'entità/elemento padre (blog)</span><span class="sxs-lookup"><span data-stu-id="48328-175">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="48328-176">Dopo il salvataggio, tutte le entità vengono disconnesse poiché ora sono state eliminate dal database</span><span class="sxs-lookup"><span data-stu-id="48328-176">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="48328-177">DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relazione obbligatoria</span><span class="sxs-lookup"><span data-stu-id="48328-177">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="48328-178">Blog è contrassegnato come eliminato</span><span class="sxs-lookup"><span data-stu-id="48328-178">Blog is marked as Deleted</span></span>
* <span data-ttu-id="48328-179">Post di rimanere inizialmente non modificato dal momento che a catena non vengono eseguiti fino a quando SaveChanges</span><span class="sxs-lookup"><span data-stu-id="48328-179">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="48328-180">SaveChanges tenta di impostare il post di chiave esterna su null, ma l'operazione non riesce perché la chiave esterna non ammette valori null</span><span class="sxs-lookup"><span data-stu-id="48328-180">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="48328-181">DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con una relazione facoltativa</span><span class="sxs-lookup"><span data-stu-id="48328-181">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="48328-182">Blog è contrassegnato come eliminato</span><span class="sxs-lookup"><span data-stu-id="48328-182">Blog is marked as Deleted</span></span>
* <span data-ttu-id="48328-183">Post di rimanere inizialmente non modificato dal momento che a catena non vengono eseguiti fino a quando SaveChanges</span><span class="sxs-lookup"><span data-stu-id="48328-183">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="48328-184">Tentativi SaveChanges imposta la chiave esterna di entrambi dipendenti/figli (POST) su null prima di eliminare l'entità/elemento padre (blog)</span><span class="sxs-lookup"><span data-stu-id="48328-184">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="48328-185">Dopo il salvataggio, l'entità/elemento padre (blog) vengono eliminata, ma vengono ancora rilevati dipendenti o gli elementi figlio (POST)</span><span class="sxs-lookup"><span data-stu-id="48328-185">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="48328-186">Rilevati oggetti dipendenti o gli elementi figlio (POST) sono ora consentiti valori null della chiave esterna e rimozione del riferimento a entità/padre eliminato (blog)</span><span class="sxs-lookup"><span data-stu-id="48328-186">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="48328-187">DeleteBehavior.Restrict con relazione obbligatorio o facoltativo</span><span class="sxs-lookup"><span data-stu-id="48328-187">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="48328-188">Blog è contrassegnato come eliminato</span><span class="sxs-lookup"><span data-stu-id="48328-188">Blog is marked as Deleted</span></span>
* <span data-ttu-id="48328-189">Post di rimanere inizialmente non modificato dal momento che a catena non vengono eseguiti fino a quando SaveChanges</span><span class="sxs-lookup"><span data-stu-id="48328-189">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="48328-190">Poiché *Restrict* indica EF per impostare automaticamente la chiave esterna su null, rimane non null e SaveChanges genera senza salvare le modifiche</span><span class="sxs-lookup"><span data-stu-id="48328-190">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="48328-191">Esempi di orfani di eliminazione</span><span class="sxs-lookup"><span data-stu-id="48328-191">Delete orphans examples</span></span>

<span data-ttu-id="48328-192">Il codice seguente fa parte di un [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) che può essere scaricato un'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="48328-192">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded an run.</span></span> <span data-ttu-id="48328-193">Nell'esempio viene illustrato cosa accade per ogni comportamento di eliminazione per le relazioni obbligatori e facoltative, quando viene interrotta la relazione tra un elemento padre/principale e i relativi elementi figlio o dipendenti.</span><span class="sxs-lookup"><span data-stu-id="48328-193">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="48328-194">In questo esempio, la relazione viene interrotto rimuovendo gli oggetti dipendenti o elementi figlio (POST) dalla proprietà di navigazione di raccolta nell'entità/elemento padre (blog).</span><span class="sxs-lookup"><span data-stu-id="48328-194">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="48328-195">Tuttavia, il comportamento è lo stesso se il riferimento dipendente/figlio/padre principale viene invece impostato su null.</span><span class="sxs-lookup"><span data-stu-id="48328-195">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="48328-196">Si analizzerà ogni variazione alla capire cosa succede.</span><span class="sxs-lookup"><span data-stu-id="48328-196">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="48328-197">DeleteBehavior.Cascade con relazione obbligatorio o facoltativo</span><span class="sxs-lookup"><span data-stu-id="48328-197">DeleteBehavior.Cascade with required or optional relationship</span></span>

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

* <span data-ttu-id="48328-198">Post sono contrassegnati come modificati perché l'interruzione della relazione ha causato la chiave esterna deve essere contrassegnato come null</span><span class="sxs-lookup"><span data-stu-id="48328-198">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="48328-199">Se la chiave esterna non ammette valori null, quindi il valore effettivo non cambierà anche se è contrassegnato come null</span><span class="sxs-lookup"><span data-stu-id="48328-199">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="48328-200">SaveChanges invia eliminazioni per oggetti dipendenti (POST) o gli elementi figlio</span><span class="sxs-lookup"><span data-stu-id="48328-200">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="48328-201">Dopo il salvataggio, gli oggetti dipendenti o elementi figlio (POST) sono scollegati dall'ora sono state eliminate dal database</span><span class="sxs-lookup"><span data-stu-id="48328-201">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="48328-202">DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relazione obbligatoria</span><span class="sxs-lookup"><span data-stu-id="48328-202">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

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

* <span data-ttu-id="48328-203">Post sono contrassegnati come modificati perché l'interruzione della relazione ha causato la chiave esterna deve essere contrassegnato come null</span><span class="sxs-lookup"><span data-stu-id="48328-203">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="48328-204">Se la chiave esterna non ammette valori null, quindi il valore effettivo non cambierà anche se è contrassegnato come null</span><span class="sxs-lookup"><span data-stu-id="48328-204">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="48328-205">SaveChanges tenta di impostare il post di chiave esterna su null, ma l'operazione non riesce perché la chiave esterna non ammette valori null</span><span class="sxs-lookup"><span data-stu-id="48328-205">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="48328-206">DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con una relazione facoltativa</span><span class="sxs-lookup"><span data-stu-id="48328-206">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

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

* <span data-ttu-id="48328-207">Post sono contrassegnati come modificati perché l'interruzione della relazione ha causato la chiave esterna deve essere contrassegnato come null</span><span class="sxs-lookup"><span data-stu-id="48328-207">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="48328-208">Se la chiave esterna non ammette valori null, quindi il valore effettivo non cambierà anche se è contrassegnato come null</span><span class="sxs-lookup"><span data-stu-id="48328-208">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="48328-209">SaveChanges imposta la chiave esterna di entrambi dipendenti/figli (POST) su null</span><span class="sxs-lookup"><span data-stu-id="48328-209">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="48328-210">Dopo il salvataggio, gli oggetti dipendenti o elementi figlio (POST) sono ora consentiti valori null della chiave esterna e rimozione del riferimento a entità/padre eliminato (blog)</span><span class="sxs-lookup"><span data-stu-id="48328-210">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="48328-211">DeleteBehavior.Restrict con relazione obbligatorio o facoltativo</span><span class="sxs-lookup"><span data-stu-id="48328-211">DeleteBehavior.Restrict with required or optional relationship</span></span>

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

* <span data-ttu-id="48328-212">Post sono contrassegnati come modificati perché l'interruzione della relazione ha causato la chiave esterna deve essere contrassegnato come null</span><span class="sxs-lookup"><span data-stu-id="48328-212">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="48328-213">Se la chiave esterna non ammette valori null, quindi il valore effettivo non cambierà anche se è contrassegnato come null</span><span class="sxs-lookup"><span data-stu-id="48328-213">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="48328-214">Poiché *Restrict* indica EF per impostare automaticamente la chiave esterna su null, rimane non null e SaveChanges genera senza salvare le modifiche</span><span class="sxs-lookup"><span data-stu-id="48328-214">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="48328-215">Per le entità non viene tenuta tracciate di propagazione</span><span class="sxs-lookup"><span data-stu-id="48328-215">Cascading to untracked entities</span></span>

<span data-ttu-id="48328-216">Quando si chiama *SaveChanges*, le regole di delete cascade verranno applicate a tutte le entità che vengono rilevate dal contesto.</span><span class="sxs-lookup"><span data-stu-id="48328-216">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="48328-217">Si tratta della situazione in tutti gli esempi illustrati in precedenza, che è il motivo per cui SQL è stato generato per eliminare l'entità/elemento padre (blog) e tutti i dipendenti/figlio (POST):</span><span class="sxs-lookup"><span data-stu-id="48328-217">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="48328-218">Se solo l'entità viene caricato, ad esempio, quando viene eseguita una query per un blog senza un `Include(b => b.Posts)` includere inoltre post - quindi SaveChanges genererà solo SQL per eliminare l'entità/elemento padre:</span><span class="sxs-lookup"><span data-stu-id="48328-218">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="48328-219">Gli oggetti dipendenti o elementi figlio (POST) verrà eliminati solo se il database ha un comportamento di propagazione corrispondente configurato.</span><span class="sxs-lookup"><span data-stu-id="48328-219">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="48328-220">Se si utilizza Entity Framework per creare il database, questo comportamento cascade verrà configurato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="48328-220">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
