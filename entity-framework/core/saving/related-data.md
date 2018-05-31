---
title: Salvataggio di dati correlati - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
ms.technology: entity-framework-core
uid: core/saving/related-data
ms.openlocfilehash: b0ed25267c85e82db18d8a89693b6040db7e4b34
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2018
ms.locfileid: "31006650"
---
# <a name="saving-related-data"></a><span data-ttu-id="d5b7b-102">Salvataggio di dati correlati</span><span class="sxs-lookup"><span data-stu-id="d5b7b-102">Saving Related Data</span></span>

<span data-ttu-id="d5b7b-103">Oltre alle entità isolate, è anche possibile usare le relazioni definite nel modello.</span><span class="sxs-lookup"><span data-stu-id="d5b7b-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="d5b7b-104">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="d5b7b-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="d5b7b-105">Aggiunta di un grafo delle nuove entità</span><span class="sxs-lookup"><span data-stu-id="d5b7b-105">Adding a graph of new entities</span></span>

<span data-ttu-id="d5b7b-106">Se si creano varie nuove entità correlate, l'aggiunta di una di esse al contesto comporterà l'aggiunta anche delle altre.</span><span class="sxs-lookup"><span data-stu-id="d5b7b-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="d5b7b-107">Nell'esempio seguente il blog e i tre post correlati vengono tutti inseriti nel database.</span><span class="sxs-lookup"><span data-stu-id="d5b7b-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="d5b7b-108">I post vengono rilevati e aggiunti, in quanto sono raggiungibili tramite la proprietà di navigazione `Blog.Posts`.</span><span class="sxs-lookup"><span data-stu-id="d5b7b-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> <span data-ttu-id="d5b7b-109">Usare la proprietà EntityEntry.State per impostare lo stato di una singola entità.</span><span class="sxs-lookup"><span data-stu-id="d5b7b-109">Use the EntityEntry.State property to set the state of just a single entity.</span></span> <span data-ttu-id="d5b7b-110">Ad esempio `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="d5b7b-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="adding-a-related-entity"></a><span data-ttu-id="d5b7b-111">Aggiunta di un'entità correlata</span><span class="sxs-lookup"><span data-stu-id="d5b7b-111">Adding a related entity</span></span>

<span data-ttu-id="d5b7b-112">Se si fa riferimento a una nuova entità dalla proprietà di navigazione di un'entità già inclusa nel rilevamento delle modifiche dal contesto, l'entità verrà individuata e inserita nel database.</span><span class="sxs-lookup"><span data-stu-id="d5b7b-112">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="d5b7b-113">Nell'esempio seguente l'entità `post` viene inserita perché viene aggiunta alla proprietà `Posts` dell'entità `blog` recuperata dal database.</span><span class="sxs-lookup"><span data-stu-id="d5b7b-113">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="d5b7b-114">Modifica delle relazioni</span><span class="sxs-lookup"><span data-stu-id="d5b7b-114">Changing relationships</span></span>

<span data-ttu-id="d5b7b-115">Se si modifica la proprietà di navigazione di un'entità, le modifiche corrispondenti verranno apportate alla colonna di chiave esterna nel database.</span><span class="sxs-lookup"><span data-stu-id="d5b7b-115">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="d5b7b-116">Nell'esempio seguente l'entità `post` viene aggiornata in modo che appartenga alla nuova entità`blog` perché la relativa proprietà di navigazione `Blog` è impostata in modo da puntare a `blog`.</span><span class="sxs-lookup"><span data-stu-id="d5b7b-116">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="d5b7b-117">Si noti che anche l'entità `blog` verrà inserita nel database perché è una nuova entità a cui fa riferimento la proprietà di navigazione di un'entità già inclusa nel rilevamento delle modifiche dal contesto (`post`).</span><span class="sxs-lookup"><span data-stu-id="d5b7b-117">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="d5b7b-118">Rimozione delle relazioni</span><span class="sxs-lookup"><span data-stu-id="d5b7b-118">Removing relationships</span></span>

<span data-ttu-id="d5b7b-119">È possibile rimuovere una relazione impostando una navigazione di riferimento su `null` oppure rimuovendo l'entità correlata dalla navigazione di una raccolta.</span><span class="sxs-lookup"><span data-stu-id="d5b7b-119">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="d5b7b-120">La rimozione di una relazione può avere effetti collaterali sull'entità dipendente, a seconda del comportamento di eliminazione a catena configurato nella relazione.</span><span class="sxs-lookup"><span data-stu-id="d5b7b-120">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="d5b7b-121">Per impostazione predefinita, per le relazioni obbligatorie viene configurato un comportamento di eliminazione a catena e l'entità figlio/dipendente verrà eliminata dal database.</span><span class="sxs-lookup"><span data-stu-id="d5b7b-121">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="d5b7b-122">Per le relazioni facoltative, l'eliminazione a catena non viene configurata per impostazione predefinita, ma la proprietà di chiave esterna verrà impostata su Null.</span><span class="sxs-lookup"><span data-stu-id="d5b7b-122">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="d5b7b-123">Vedere [Relazioni obbligatorie e facoltative](../modeling/relationships.md#required-and-optional-relationships) per informazioni su come configurare l'obbligatorietà delle relazioni.</span><span class="sxs-lookup"><span data-stu-id="d5b7b-123">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="d5b7b-124">Vedere [Eliminazione a catena](cascade-delete.md) per altri dettagli su come funzionano i comportamenti di eliminazione, su come è possibile configurarli in modo esplicito e sulla modalità di selezione convenzionale.</span><span class="sxs-lookup"><span data-stu-id="d5b7b-124">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="d5b7b-125">Nell'esempio seguente viene configurata un'eliminazione a catena per la relazione tra `Blog` e `Post`, pertanto l'entità `post` viene eliminata dal database.</span><span class="sxs-lookup"><span data-stu-id="d5b7b-125">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
