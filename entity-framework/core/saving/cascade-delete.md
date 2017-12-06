---
title: Delete - Core EF a catena
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: a9481fe851cc264ab3eaecad052c2e683ae57a44
ms.sourcegitcommit: 5367516f063cb42804ec92c31cdf76322554f2b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/08/2017
---
# <a name="cascade-delete"></a>Eliminazione a catena

Nella terminologia dei database, eliminazione a catena in genere viene utilizzato per descrivere una caratteristica che consente l'eliminazione di una riga per attivare automaticamente l'eliminazione di righe correlate. Un concetto strettamente correlato rientrano anche i comportamenti di eliminazione EF Core è l'eliminazione automatica di un'entità figlio quando si tratta di una relazione a un elemento padre è stata interrotta--questo i comunemente noto come "eliminazione orfani".

Core EF implementa alcuni comportamenti diversi delete e consente la configurazione dei comportamenti di eliminazione di relazioni singoli. Componenti di base di Entity Framework implementa inoltre convenzioni che consente di configurare automaticamente i comportamenti di eliminazione predefiniti utili per ogni relazione basata su [requiredness della relazione] (... /Modeling/Relationships.MD#Required-and-Optional-Relationships).

## <a name="delete-behaviors"></a>Eliminare i comportamenti
Eliminare i comportamenti vengono definiti nel *DeleteBehavior* enumeratore digitare e può essere passato al *OnDelete* fluent API per controllare se l'eliminazione di un'entità principale o padre o il interruzione del relazione con l'entità dipendente/figlio deve ha un effetto collaterale sulle entità dipendente/figlio.

Esistono tre azioni EF quando viene eliminata un'entità principale o padre o la relazione per l'elemento figlio viene interrotto:
* Figlio/dipendente può essere eliminato.
* Valori di chiave esterna del figlio possono essere impostati su null
* L'elemento figlio rimane invariato

> [!NOTE]  
> Il comportamento di eliminazione configurato nel modello di Entity Framework Core viene applicato solo quando l'entità principale viene eliminato usando EF Core e le entità dipendenti vengono caricate in memoria (ad esempio per il rilevamento dipendenti). Un comportamento di propagazione corrispondente deve essere il programma di installazione del database per garantire che i dati che non viene rilevati dal contesto è necessario azione applicata. Se si utilizza Entity Framework Core per creare il database, questo comportamento cascade verrà configurato automaticamente.

Per la seconda azione precedente, l'impostazione di un valore di chiave esterna su null non è valido se la chiave esterna non ammette valori null. (Una chiave esterna non ammettono valori null è equivalente a una relazione obbligatoria). In questi casi, Core EF rileva che la proprietà di chiave esterna è stata contrassegnata come null fino a quando non viene chiamato SaveChanges, nel qual caso viene generata un'eccezione in quanto la modifica non può essere reso persistente nel database. È simile a ottenere una violazione del vincolo dal database.

Sono disponibili quattro eliminazione comportamenti, elencati nelle tabelle seguenti. Per le relazioni facoltative (chiave esterna che ammette valori null), _è_ possibile salvare un null valore di chiave esterna, che determina gli effetti seguenti:

| Nome del comportamento | Effetto sul dipendente/figlio in memoria | Effetto sul dipendente/figlio nel database
|-|-|-
| **CASCADE** | Le entità vengono eliminate. | Le entità vengono eliminate.
| **ClientSetNull** (predefinito) | Proprietà di chiave esterna sono impostate su null | None
| **SetNull** | Proprietà di chiave esterna sono impostate su null | Proprietà di chiave esterna sono impostate su null
| **Limitare** | None | None

Per le relazioni necessarie (chiave esterna non nullable) è _non_ possibile salvare un null valore di chiave esterna, che determina gli effetti seguenti:

| Nome del comportamento | Effetto sul dipendente/figlio in memoria | Effetto sul dipendente/figlio nel database
|-|-|-
| **CASCADE** (predefinito) | Le entità vengono eliminate. | Le entità vengono eliminate.
| **ClientSetNull** | Genera SaveChanges | None
| **SetNull** | Genera SaveChanges | Genera SaveChanges
| **Limitare** | None | None

Nelle tabelle precedenti, *Nessuno* può comportare una violazione del vincolo. Ad esempio, se viene eliminata un'entità principale/figlio, ma viene eseguita alcuna azione per modificare la chiave esterna di un dipendente/figlio, quindi il database verrà probabilmente genera eccezioni in SaveChanges a causa di una violazione del vincolo foreign.

In generale:
* Se si dispone di entità che non può esistere senza un padre e desiderate EF da svolgere per eliminare automaticamente gli elementi figlio, quindi utilizzare *Cascade*.
  * Le entità che non possono esistere senza un elemento padre in genere assicurarsi utilizzare relazioni necessarie, per cui *Cascade* è l'impostazione predefinita.
* Se si dispone di entità che possono o non dispone di un padre ed EF di occuparsi di Annulla della chiave esterna per l'utente desiderato, quindi utilizzare *ClientSetNull*
  * Entità che possono esistere senza un elemento padre in genere assicurarsi utilizzare relazioni facoltative, per cui *ClientSetNull* è l'impostazione predefinita.
  * Se si desidera il database anche provare a propagare anche i valori null per le chiavi esterne figlio quando l'entità figlio non viene caricato, quindi utilizzare *SetNull*. Si noti tuttavia che il database deve supportare questo e configurazione del database, questo può comportare altre restrizioni, in pratica spesso rendendo questa opzione causa. Questo è il motivo *SetNull* non è il valore predefinito.
* Se non si desidera Core EF mai eliminare automaticamente un'entità o null automaticamente della chiave esterna, quindi utilizzare *Restrict*. Si noti che questa operazione richiede che il codice sincronizzare entità figlio e i relativi valori di chiave esterna manualmente in caso contrario vincolo verranno generate eccezioni.

> [!NOTE]
> In Entity Framework Core, a differenza di EF6, effetti a catena non vengono eseguiti immediatamente, ma invece solo quando viene chiamato SaveChanges.

> [!NOTE]  
> **Le modifiche in EF Core 2.0:** nelle versioni precedenti, *Restrict* causerebbe facoltativa proprietà di chiave esterna nelle entità dipendenti rilevate sia impostato su null e il comportamento per le relazioni facoltative di eliminare il valore predefinito è stato. In Entity Framework Core 2.0 il *ClientSetNull* è stato introdotto per rappresentare tale comportamento ed è diventato il valore predefinito per le relazioni facoltative. Il comportamento di *Restrict* regolato per mai avere effetti collaterali su entità dipendente.

## <a name="entity-deletion-examples"></a>Esempi di eliminazione di entità

Il codice seguente fa parte di un [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) che può essere scaricato un'esecuzione. Nell'esempio viene illustrato cosa accade per ogni comportamento di eliminazione per le relazioni obbligatori e facoltative, quando viene eliminata un'entità padre.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Si analizzerà ogni variazione alla capire cosa succede.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade con relazione obbligatorio o facoltativo

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

* Blog è contrassegnato come eliminato
* Post di rimanere inizialmente non modificato dal momento che a catena non vengono eseguiti fino a quando SaveChanges
* SaveChanges invia eliminazioni per oggetti dipendenti (POST) o gli elementi figlio e quindi l'entità/elemento padre (blog)
* Dopo il salvataggio, tutte le entità vengono disconnesse poiché ora sono state eliminate dal database

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relazione obbligatoria

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

* Blog è contrassegnato come eliminato
* Post di rimanere inizialmente non modificato dal momento che a catena non vengono eseguiti fino a quando SaveChanges
* SaveChanges tenta di impostare il post di chiave esterna su null, ma l'operazione non riesce perché la chiave esterna non ammette valori null

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con una relazione facoltativa

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

* Blog è contrassegnato come eliminato
* Post di rimanere inizialmente non modificato dal momento che a catena non vengono eseguiti fino a quando SaveChanges
* Tentativi SaveChanges imposta la chiave esterna di entrambi dipendenti/figli (POST) su null prima di eliminare l'entità/elemento padre (blog)
* Dopo il salvataggio, l'entità/elemento padre (blog) vengono eliminata, ma vengono ancora rilevati dipendenti o gli elementi figlio (POST)
* Rilevati oggetti dipendenti o gli elementi figlio (POST) sono ora consentiti valori null della chiave esterna e rimozione del riferimento a entità/padre eliminato (blog)

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict con relazione obbligatorio o facoltativo

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

* Blog è contrassegnato come eliminato
* Post di rimanere inizialmente non modificato dal momento che a catena non vengono eseguiti fino a quando SaveChanges
* Poiché *Restrict* indica EF per impostare automaticamente la chiave esterna su null, rimane non null e SaveChanges genera senza salvare le modifiche

## <a name="delete-orphans-examples"></a>Esempi di orfani di eliminazione

Il codice seguente fa parte di un [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) che può essere scaricato un'esecuzione. Nell'esempio viene illustrato cosa accade per ogni comportamento di eliminazione per le relazioni obbligatori e facoltative, quando viene interrotta la relazione tra un elemento padre/principale e i relativi elementi figlio o dipendenti. In questo esempio, la relazione viene interrotto rimuovendo gli oggetti dipendenti o elementi figlio (POST) dalla proprietà di navigazione di raccolta nell'entità/elemento padre (blog). Tuttavia, il comportamento è lo stesso se il riferimento dipendente/figlio/padre principale viene invece impostato su null.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Si analizzerà ogni variazione alla capire cosa succede.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade con relazione obbligatorio o facoltativo

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

* Post sono contrassegnati come modificati perché l'interruzione della relazione ha causato la chiave esterna deve essere contrassegnato come null
  * Se la chiave esterna non ammette valori null, quindi il valore effettivo non cambierà anche se è contrassegnato come null
* SaveChanges invia eliminazioni per oggetti dipendenti (POST) o gli elementi figlio
* Dopo il salvataggio, gli oggetti dipendenti o elementi figlio (POST) sono scollegati dall'ora sono state eliminate dal database

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con relazione obbligatoria

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

* Post sono contrassegnati come modificati perché l'interruzione della relazione ha causato la chiave esterna deve essere contrassegnato come null
  * Se la chiave esterna non ammette valori null, quindi il valore effettivo non cambierà anche se è contrassegnato come null
* SaveChanges tenta di impostare il post di chiave esterna su null, ma l'operazione non riesce perché la chiave esterna non ammette valori null

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull o DeleteBehavior.SetNull con una relazione facoltativa

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

* Post sono contrassegnati come modificati perché l'interruzione della relazione ha causato la chiave esterna deve essere contrassegnato come null
  * Se la chiave esterna non ammette valori null, quindi il valore effettivo non cambierà anche se è contrassegnato come null
* SaveChanges imposta la chiave esterna di entrambi dipendenti/figli (POST) su null
* Dopo il salvataggio, gli oggetti dipendenti o elementi figlio (POST) sono ora consentiti valori null della chiave esterna e rimozione del riferimento a entità/padre eliminato (blog)

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict con relazione obbligatorio o facoltativo

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

* Post sono contrassegnati come modificati perché l'interruzione della relazione ha causato la chiave esterna deve essere contrassegnato come null
  * Se la chiave esterna non ammette valori null, quindi il valore effettivo non cambierà anche se è contrassegnato come null
* Poiché *Restrict* indica EF per impostare automaticamente la chiave esterna su null, rimane non null e SaveChanges genera senza salvare le modifiche

## <a name="cascading-to-untracked-entities"></a>Per le entità non viene tenuta tracciate di propagazione

Quando si chiama *SaveChanges*, le regole di delete cascade verranno applicate a tutte le entità che vengono rilevate dal contesto. Si tratta della situazione in tutti gli esempi illustrati in precedenza, che è il motivo per cui SQL è stato generato per eliminare l'entità/elemento padre (blog) e tutti i dipendenti/figlio (POST):

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Se solo l'entità viene caricato, ad esempio, quando viene eseguita una query per un blog senza un `Include(b => b.Posts)` includere inoltre post - quindi SaveChanges genererà solo SQL per eliminare l'entità/elemento padre:

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Gli oggetti dipendenti o elementi figlio (POST) verrà eliminati solo se il database ha un comportamento di propagazione corrispondente configurato. Se si utilizza Entity Framework per creare il database, questo comportamento cascade verrà configurato automaticamente.
