---
title: Rilevamento e query senza rilevamento - Entity Framework CoreTracking vs.
author: smitpatel
ms.date: 10/10/2019
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: a6c71c12f429f1324abe91d1b2cef96312bec051
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417649"
---
# <a name="tracking-vs-no-tracking-queries"></a>Rilevamento e query senza rilevamentoTracking vs.

Controlli del comportamento di rilevamento se Entity Framework Core manterrà le informazioni su un'istanza di entità nel rilevamento delle modifiche. Se un'entità viene inclusa nel rilevamento delle modifiche, qualsiasi modifica individuata per l'entità verrà salvata in modo permanente nel database durante `SaveChanges()`. EF Core correggerà anche le proprietà di navigazione tra le entità nel risultato di una query di rilevamento e le entità che si trovano nel rilevamento delle modifiche.

> [!NOTE]
> [I tipi di entità senza chiave](xref:core/modeling/keyless-entity-types) non vengono mai rilevati. Ovunque in questo articolo vengano menzionati i tipi di entità, si riferisce ai tipi di entità per i quali è stata definita una chiave.

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) di questo articolo in GitHub.

## <a name="tracking-queries"></a>Query con rilevamento delle modifiche

Per impostazione predefinita, le query che restituiscono tipi di entità sono con rilevamento delle modifiche. Ciò significa che è possibile apportare modifiche a `SaveChanges()`tali istanze di entità e rendere persistenti tali modifiche da . Nell'esempio seguente la modifica della classificazione del blog verrà rilevata e salvata in modo permanente nel database durante `SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#Tracking)]

## <a name="no-tracking-queries"></a>Query senza registrazione

Le query senza rilevamento delle modifiche sono utili quando i risultati vengono usati in uno scenario di sola lettura. Sono più veloci da eseguire perché non è necessario impostare le informazioni sul rilevamento delle modifiche. Se non è necessario aggiornare le entità recuperate dal database, è necessario utilizzare una query senza rilevamento. È possibile scambiare una singola query per non tenere traccia.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#NoTracking)]

È anche possibile modificare il comportamento predefinito di rilevamento delle modifiche a livello di istanza di contesto:

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ContextDefaultTrackingBehavior)]

## <a name="identity-resolution"></a>Risoluzione di identità

Poiché una query di rilevamento utilizza il rilevamento delle modifiche, EF Core eseguirà la risoluzione delle identità in una query di rilevamento. Durante la materializzazione di un'entità, Entity Framework Core restituirà la stessa istanza di entità dal rilevamento delle modifiche se è già in fase di rilevamento. Se il risultato contiene la stessa entità più volte, si ottiene la stessa istanza per ogni occorrenza. Le query senza monitoraggio non utilizzano il rilevamento delle modifiche e non eseguono la risoluzione delle identità. In questo modo si ottiene una nuova istanza dell'entità anche quando la stessa entità è contenuta nel risultato più volte. Questo comportamento era diverso nelle versioni precedenti a EF Core 3.0, vedere [le versioni precedenti.](#previous-versions)

## <a name="tracking-and-custom-projections"></a>Monitoraggio e proiezioni personalizzate

Anche se il tipo di risultato della query non è un tipo di entità, Entity Framework Core continuerà a tenere traccia dei tipi di entità contenuti nel risultato per impostazione predefinita. Nella query seguente, che restituisce un tipo anonimo, le istanze di `Blog` nel set di risultati verranno incluse nel rilevamento delle modifiche.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection1)]

Se il set di risultati contiene tipi di entità che escono dalla composizione LINQ, Entity Framework Core ne terrà traccia.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

Se il set di risultati non contiene tipi di entità, non viene eseguito alcun rilevamento. Nella query seguente viene restituito un tipo anonimo con alcuni valori dell'entità (ma nessuna istanza del tipo di entità effettivo). Non sono presenti entità rilevate che escono dalla query.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection3)]

 EF Core supporta l'esecuzione della valutazione del client nella proiezione di primo livello. Se Entity Framework Core materializza un'istanza di entità per la valutazione del client, verrà tenuta traccia di tale istanza. Qui, dal momento `blog` che stiamo passando `StandardizeURL`entità al metodo client , EF Core terrà traccia anche le istanze del blog.

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientMethod)]

EF Core non tiene traccia delle istanze di entità senza chiave contenute nel risultato. Ma Entity Framework Core tiene traccia di tutte le altre istanze di tipi di entità con chiave in base alle regole precedenti.

Alcune delle regole di cui sopra hanno funzionato in modo diverso prima di EF Core 3.0. Per ulteriori informazioni, vedere [versioni precedenti](#previous-versions).

## <a name="previous-versions"></a>Versioni precedenti

Prima della versione 3.0, EF Core aveva alcune differenze nel modo in cui è stato eseguito il rilevamento. Differenze degne di nota sono le seguenti:

- Come spiegato nella pagina [Valutazione client e server,](xref:core/querying/client-eval) valutazione client supportata da EF Core in qualsiasi parte della query precedente alla versione 3.0.As explained in Client vs Server Evaluation page, EF Core supported client evaluation in any part of the query before version 3.0. La valutazione del client ha causato la materializzazione delle entità, che non facevano parte del risultato. Così EF Core analizzato il risultato per rilevare cosa tenere traccia. Questo progetto ha avuto alcune differenze come segue:
  - La valutazione del client nella proiezione, che ha causato la materializzazione ma non ha restituito l'istanza dell'entità materializzata non è stata rilevata. L'esempio seguente non `blog` tiene traccia delle entità.
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#ClientProjection)]

  - EF Core non tiene traccia degli oggetti che escono dalla composizione LINQ in alcuni casi. Nell'esempio seguente non `Post`è stato tracciato .
    [!code-csharp[Main](../../../samples/core/Querying/Tracking/Sample.cs#CustomProjection2)]

- Ogni volta che i risultati della query contenevano tipi di entità senza chiave, l'intera query è stata resa senza rilevamento. Ciò significa che i tipi di entità con chiavi, che sono in risultato non sono stati registrati neanche.
- EF Core ha esito la risoluzione delle identità in query senza rilevamento. Utilizzava riferimenti deboli per tenere traccia delle entità che erano già state restituite. Pertanto, se un set di risultati contiene la stessa entità più volte, si otterrebbe la stessa istanza per ogni occorrenza. Anche se un risultato precedente con la stessa identità è uscito dall'ambito e ottenuto garbage collection, EF Core restituito una nuova istanza.
