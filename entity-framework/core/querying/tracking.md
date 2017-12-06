---
title: Rilevamento di Visual Studio. Query di rilevamento No - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
ms.technology: entity-framework-core
uid: core/querying/tracking
ms.openlocfilehash: 9a22c893f3b1e9991560e25e0252287a2844b39e
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2017
---
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="36fe0-102">Rilevamento di Visual Studio. Query di rilevamento di No</span><span class="sxs-lookup"><span data-stu-id="36fe0-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="36fe0-103">Controlli di comportamento di rilevamento Entity Framework Core conserverà le informazioni relative a un'istanza di entità nel rilevamento delle modifiche o meno.</span><span class="sxs-lookup"><span data-stu-id="36fe0-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="36fe0-104">Se viene rilevata un'entità, le modifiche rilevate nell'entità verranno rese persistenti nel database durante `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="36fe0-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="36fe0-105">Entity Framework Core verrà correzione anche le proprietà di navigazione tra le entità che vengono ottenute da una query di rilevamento e che sono stati caricati in precedenza nell'istanza di DbContext.</span><span class="sxs-lookup"><span data-stu-id="36fe0-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="36fe0-106">È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) su GitHub.</span><span class="sxs-lookup"><span data-stu-id="36fe0-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="36fe0-107">Query di rilevamento</span><span class="sxs-lookup"><span data-stu-id="36fe0-107">Tracking queries</span></span>

<span data-ttu-id="36fe0-108">Per impostazione predefinita, le query che restituiscono tipi di entità sono di rilevamento.</span><span class="sxs-lookup"><span data-stu-id="36fe0-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="36fe0-109">In questo modo è possibile apportare modifiche alle istanze di entità e le modifiche sono rese persistenti dal `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="36fe0-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="36fe0-110">Nell'esempio seguente, rilevata e resi persistenti nel database durante la modifica per la classificazione di blog `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="36fe0-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="36fe0-111">Query di rilevamento di No</span><span class="sxs-lookup"><span data-stu-id="36fe0-111">No-tracking queries</span></span>

<span data-ttu-id="36fe0-112">Nessuna query di rilevamento è utile quando i risultati vengono usati in uno scenario di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="36fe0-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="36fe0-113">Sono più veloci per l'esecuzione perché non è necessario per informazioni sul rilevamento delle modifiche di configurazione.</span><span class="sxs-lookup"><span data-stu-id="36fe0-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="36fe0-114">È possibile scambiare una singola query di rilevamento non essere:</span><span class="sxs-lookup"><span data-stu-id="36fe0-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="36fe0-115">È inoltre possibile modificare il comportamento a livello di istanza di contesto di rilevamento predefinito:</span><span class="sxs-lookup"><span data-stu-id="36fe0-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="36fe0-116">Nessuna query di rilevamento comunque esegue la risoluzione di identità all'interno della query di eseguire.</span><span class="sxs-lookup"><span data-stu-id="36fe0-116">No tracking queries still perform identity resolution within the excuting query.</span></span> <span data-ttu-id="36fe0-117">Se il set di risultati contiene la stessa entità più volte, restituirà la stessa istanza della classe di entità per ogni occorrenza del set di risultati.</span><span class="sxs-lookup"><span data-stu-id="36fe0-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="36fe0-118">Riferimenti deboli, tuttavia, vengono utilizzati per tenere traccia delle entità che sono già state restituite.</span><span class="sxs-lookup"><span data-stu-id="36fe0-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="36fe0-119">Se un risultato precedente con la stessa identità abbandona l'ambito e l'esecuzione della garbage collection, è possibile ottenere una nuova istanza di entità.</span><span class="sxs-lookup"><span data-stu-id="36fe0-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="36fe0-120">Per ulteriori informazioni, vedere [come funziona la Query](overview.md).</span><span class="sxs-lookup"><span data-stu-id="36fe0-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="36fe0-121">Rilevamento e le proiezioni</span><span class="sxs-lookup"><span data-stu-id="36fe0-121">Tracking and projections</span></span>

<span data-ttu-id="36fe0-122">Anche se il tipo di risultato della query non è un tipo di entità, se il risultato contiene i tipi di entità sono comunque essere registrati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="36fe0-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="36fe0-123">Nella query seguente, che restituisce un tipo anonimo, le istanze di `Blog` nel risultato di rilevamento dei set.</span><span class="sxs-lookup"><span data-stu-id="36fe0-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=7)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Blog = b,
                Posts = b.Posts.Count()
            });
}
```

<span data-ttu-id="36fe0-124">Se il set di risultati non contiene alcun tipo di entità, non viene eseguita alcuna registrazione.</span><span class="sxs-lookup"><span data-stu-id="36fe0-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="36fe0-125">Nella query seguente, che restituisce un tipo anonimo con alcuni dei valori dell'entità (ma non le istanze del tipo di entità effettivo), non c'è alcun rilevamento eseguita.</span><span class="sxs-lookup"><span data-stu-id="36fe0-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Id = b.BlogId,
                Url = b.Url
            });
}
```
