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
# <a name="saving-related-data"></a><span data-ttu-id="d07f0-102">Salvataggio di dati correlati</span><span class="sxs-lookup"><span data-stu-id="d07f0-102">Saving Related Data</span></span>

<span data-ttu-id="d07f0-103">Oltre alle entità di tipo isolata, è inoltre possibile rendere utilizzare le relazioni definite nel modello.</span><span class="sxs-lookup"><span data-stu-id="d07f0-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="d07f0-104">È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) su GitHub.</span><span class="sxs-lookup"><span data-stu-id="d07f0-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="d07f0-105">Aggiunta di un grafico di nuove entità</span><span class="sxs-lookup"><span data-stu-id="d07f0-105">Adding a graph of new entities</span></span>

<span data-ttu-id="d07f0-106">Se si crea diverse nuove entità correlate, l'aggiunta di uno di essi al contesto comporterà ad altri utenti di essere aggiunte.</span><span class="sxs-lookup"><span data-stu-id="d07f0-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="d07f0-107">Nell'esempio seguente, blog e tre i post correlati vengono inseriti nel database.</span><span class="sxs-lookup"><span data-stu-id="d07f0-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="d07f0-108">I post vengono rilevati e aggiunti, in quanto sono raggiungibile tramite il `Blog.Posts` proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="d07f0-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

## <a name="adding-a-related-entity"></a><span data-ttu-id="d07f0-109">Aggiunta di un'entità correlata</span><span class="sxs-lookup"><span data-stu-id="d07f0-109">Adding a related entity</span></span>

<span data-ttu-id="d07f0-110">Se si fa riferimento a una nuova entità dalla proprietà di navigazione di un'entità che è già rilevata dal contesto, l'entità verrà individuato e inserito nel database.</span><span class="sxs-lookup"><span data-stu-id="d07f0-110">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="d07f0-111">Nell'esempio seguente, il `post` viene inserita l'entità perché è in aggiunta a di `Posts` proprietà del `blog` entità a cui è stato recuperato dal database.</span><span class="sxs-lookup"><span data-stu-id="d07f0-111">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="d07f0-112">Modifica delle relazioni</span><span class="sxs-lookup"><span data-stu-id="d07f0-112">Changing relationships</span></span>

<span data-ttu-id="d07f0-113">Se si modifica la proprietà di navigazione di un'entità, la colonna chiave esterna nel database verranno apportate le modifiche corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="d07f0-113">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="d07f0-114">Nell'esempio seguente, il `post` entità viene aggiornata per appartengono al nuovo `blog` entità perché il relativo `Blog` proprietà di navigazione è impostato per puntare alla `blog`.</span><span class="sxs-lookup"><span data-stu-id="d07f0-114">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="d07f0-115">Si noti che `blog` inoltre essere inseriti nel database perché è una nuova entità che fa riferimento la proprietà di navigazione di un'entità che è già rilevata dal contesto (`post`).</span><span class="sxs-lookup"><span data-stu-id="d07f0-115">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="d07f0-116">Rimozione di relazioni</span><span class="sxs-lookup"><span data-stu-id="d07f0-116">Removing relationships</span></span>

<span data-ttu-id="d07f0-117">È possibile rimuovere una relazione impostando una navigazione di riferimento su `null`, o la rimozione dell'entità correlata da una navigazione della raccolta.</span><span class="sxs-lookup"><span data-stu-id="d07f0-117">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="d07f0-118">Rimozione di una relazione può avere effetti collaterali nell'entità dipendenti, in base l'opzione cascade delete comportamento configurato nella relazione.</span><span class="sxs-lookup"><span data-stu-id="d07f0-118">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="d07f0-119">Per impostazione predefinita, per le relazioni necessarie, è configurato un comportamento di delete cascade e l'entità figlio/dipendenti verrà eliminato dal database.</span><span class="sxs-lookup"><span data-stu-id="d07f0-119">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="d07f0-120">Per le relazioni facoltative, eliminazione a catena non è configurato per impostazione predefinita, ma verrà impostata la proprietà di chiave esterna su null.</span><span class="sxs-lookup"><span data-stu-id="d07f0-120">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="d07f0-121">Vedere [relazioni obbligatori e facoltative](../modeling/relationships.md#required-and-optional-relationships) per informazioni su come configurare requiredness di relazioni.</span><span class="sxs-lookup"><span data-stu-id="d07f0-121">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="d07f0-122">Vedere [eliminazione a catena](cascade-delete.md) per ulteriori informazioni su come eliminazione a catena comportamenti e lavoro, come è possibile configurarle in modo esplicito la modalità di selezione per convenzione.</span><span class="sxs-lookup"><span data-stu-id="d07f0-122">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="d07f0-123">Nell'esempio seguente, è configurata un'eliminazione a catena della relazione tra `Blog` e `Post`, pertanto il `post` entità viene eliminata dal database.</span><span class="sxs-lookup"><span data-stu-id="d07f0-123">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
