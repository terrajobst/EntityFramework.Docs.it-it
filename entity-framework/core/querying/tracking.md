---
title: Query con rilevamento delle modifiche e senza rilevamento delle modifiche - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: 985adc795f379199a3bacc985843f32f3168cf64
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993355"
---
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="7fbbe-102">Query con rilevamento delle modifiche e senza rilevamento delle modifiche</span><span class="sxs-lookup"><span data-stu-id="7fbbe-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="7fbbe-103">Dal comportamento di rilevamento delle modifiche dipende se Entity Framework Core conserverà o meno le informazioni relative a un'istanza di entità nel relativo strumento di rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="7fbbe-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="7fbbe-104">Se un'entità viene inclusa nel rilevamento delle modifiche, qualsiasi modifica individuata per l'entità verrà salvata in modo permanente nel database durante `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="7fbbe-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="7fbbe-105">Entity Framework Core correggerà anche le proprietà di navigazione tra le entità ottenute da una query con rilevamento delle modifiche e le entità caricate in precedenza nell'istanza di DbContext.</span><span class="sxs-lookup"><span data-stu-id="7fbbe-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="7fbbe-106">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="7fbbe-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="7fbbe-107">Query con rilevamento delle modifiche</span><span class="sxs-lookup"><span data-stu-id="7fbbe-107">Tracking queries</span></span>

<span data-ttu-id="7fbbe-108">Per impostazione predefinita, le query che restituiscono tipi di entità sono con rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="7fbbe-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="7fbbe-109">Ciò significa che è possibile apportare modifiche alle istanze di entità e le modifiche vengono salvate in modo permanente da `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="7fbbe-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="7fbbe-110">Nell'esempio seguente la modifica della classificazione del blog verrà rilevata e salvata in modo permanente nel database durante `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="7fbbe-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="7fbbe-111">Query senza registrazione</span><span class="sxs-lookup"><span data-stu-id="7fbbe-111">No-tracking queries</span></span>

<span data-ttu-id="7fbbe-112">Le query senza rilevamento delle modifiche sono utili quando i risultati vengono usati in uno scenario di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="7fbbe-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="7fbbe-113">Sono più veloci da eseguire perché non è necessario configurare le informazioni per il rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="7fbbe-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="7fbbe-114">È possibile configurare una singola query in modo che sia senza rilevamento delle modifiche:</span><span class="sxs-lookup"><span data-stu-id="7fbbe-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="7fbbe-115">È anche possibile modificare il comportamento predefinito di rilevamento delle modifiche a livello di istanza di contesto:</span><span class="sxs-lookup"><span data-stu-id="7fbbe-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="7fbbe-116">Le query senza rilevamento delle modifiche eseguono comunque la risoluzione dell'identità all'interno della query in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="7fbbe-116">No tracking queries still perform identity resolution within the excuting query.</span></span> <span data-ttu-id="7fbbe-117">Se il set di risultati contiene la stessa entità più volte, verrà restituita la stessa istanza della classe di entità per ogni occorrenza nel set di risultati.</span><span class="sxs-lookup"><span data-stu-id="7fbbe-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="7fbbe-118">Vengono tuttavia usati riferimenti deboli per tenere traccia delle entità già restituite.</span><span class="sxs-lookup"><span data-stu-id="7fbbe-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="7fbbe-119">Se un risultato precedente con la stessa identità esce dall'ambito e viene eseguita la Garbage Collection, si potrebbe ottenere una nuova istanza di entità.</span><span class="sxs-lookup"><span data-stu-id="7fbbe-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="7fbbe-120">Per altre informazioni, vedere [Funzionamento delle query](overview.md).</span><span class="sxs-lookup"><span data-stu-id="7fbbe-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="7fbbe-121">Rilevamento delle modifiche e proiezioni</span><span class="sxs-lookup"><span data-stu-id="7fbbe-121">Tracking and projections</span></span>

<span data-ttu-id="7fbbe-122">Anche se il tipo di risultato della query non è un tipo di entità, se il risultato contiene tipi di entità questi verranno comunque inclusi nel rilevamento delle modifiche per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="7fbbe-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="7fbbe-123">Nella query seguente, che restituisce un tipo anonimo, le istanze di `Blog` nel set di risultati verranno incluse nel rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="7fbbe-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

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

<span data-ttu-id="7fbbe-124">Se il set di risultati non contiene alcun tipo di entità, il rilevamento delle modifiche non viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="7fbbe-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="7fbbe-125">Nella query seguente, che restituisce un tipo anonimo con alcuni dei valori dell'entità (ma non le istanze del tipo di entità effettivo), il rilevamento delle modifiche non viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="7fbbe-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

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
