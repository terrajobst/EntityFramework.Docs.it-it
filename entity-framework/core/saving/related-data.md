---
title: Salvare i dati - Core EF correlati.
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
ms.technology: entity-framework-core
uid: core/saving/related-data
ms.openlocfilehash: 078879163002cb66e0f0f439415789963181ec15
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="saving-related-data"></a>Salvataggio di dati correlati

Oltre alle entità di tipo isolata, è inoltre possibile rendere utilizzare le relazioni definite nel modello.

> [!TIP]  
> È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) su GitHub.

## <a name="adding-a-graph-of-new-entities"></a>Aggiunta di un grafico di nuove entità

Se si crea diverse nuove entità correlate, l'aggiunta di uno di essi al contesto comporterà ad altri utenti di essere aggiunte.

Nell'esempio seguente, blog e tre i post correlati vengono inseriti nel database. I post vengono rilevati e aggiunti, in quanto sono raggiungibile tramite il `Blog.Posts` proprietà di navigazione.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

## <a name="adding-a-related-entity"></a>Aggiunta di un'entità correlata

Se si fa riferimento a una nuova entità dalla proprietà di navigazione di un'entità che è già rilevata dal contesto, l'entità verrà individuato e inserito nel database.

Nell'esempio seguente, il `post` viene inserita l'entità perché è in aggiunta a di `Posts` proprietà del `blog` entità a cui è stato recuperato dal database.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>Modifica delle relazioni

Se si modifica la proprietà di navigazione di un'entità, la colonna chiave esterna nel database verranno apportate le modifiche corrispondenti.

Nell'esempio seguente, il `post` entità viene aggiornata per appartengono al nuovo `blog` entità perché il relativo `Blog` proprietà di navigazione è impostato per puntare alla `blog`. Si noti che `blog` inoltre essere inseriti nel database perché è una nuova entità che fa riferimento la proprietà di navigazione di un'entità che è già rilevata dal contesto (`post`).

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>Rimozione di relazioni

È possibile rimuovere una relazione impostando una navigazione di riferimento su `null`, o la rimozione dell'entità correlata da una navigazione della raccolta.

Rimozione di una relazione può avere effetti collaterali nell'entità dipendenti, in base l'opzione cascade delete comportamento configurato nella relazione.

Per impostazione predefinita, per le relazioni necessarie, è configurato un comportamento di delete cascade e l'entità figlio/dipendenti verrà eliminato dal database. Per le relazioni facoltative, eliminazione a catena non è configurato per impostazione predefinita, ma verrà impostata la proprietà di chiave esterna su null.

Vedere [relazioni obbligatori e facoltative](../modeling/relationships.md#required-and-optional-relationships) per informazioni su come configurare requiredness di relazioni.

Vedere [eliminazione a catena](cascade-delete.md) per ulteriori informazioni su come eliminazione a catena comportamenti e lavoro, come è possibile configurarle in modo esplicito la modalità di selezione per convenzione.

Nell'esempio seguente, è configurata un'eliminazione a catena della relazione tra `Blog` e `Post`, pertanto il `post` entità viene eliminata dal database.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
