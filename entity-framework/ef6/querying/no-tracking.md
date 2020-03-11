---
title: Query senza rilevamento-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: 44d58e14a2550bd08a8edd68b467237f6f5b5978
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417102"
---
# <a name="no-tracking-queries"></a><span data-ttu-id="d588a-102">senza rilevamento delle modifiche</span><span class="sxs-lookup"><span data-stu-id="d588a-102">No-Tracking Queries</span></span>
<span data-ttu-id="d588a-103">In alcuni casi può essere necessario recuperare le entità da una query, ma queste entità non vengono rilevate dal contesto.</span><span class="sxs-lookup"><span data-stu-id="d588a-103">Sometimes you may want to get entities back from a query but not have those entities be tracked by the context.</span></span> <span data-ttu-id="d588a-104">Ciò può comportare prestazioni migliori quando si eseguono query su un numero elevato di entità in scenari di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="d588a-104">This may result in better performance when querying for large numbers of entities in read-only scenarios.</span></span> <span data-ttu-id="d588a-105">Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.</span><span class="sxs-lookup"><span data-stu-id="d588a-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="d588a-106">Un nuovo metodo di estensione AsNoTracking consente l'esecuzione di qualsiasi query in questo modo.</span><span class="sxs-lookup"><span data-stu-id="d588a-106">A new extension method AsNoTracking allows any query to be run in this way.</span></span> <span data-ttu-id="d588a-107">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d588a-107">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs without tracking them
    var blogs1 = context.Blogs.AsNoTracking();

    // Query for some blogs without tracking them
    var blogs2 = context.Blogs
                        .Where(b => b.Name.Contains(".NET"))
                        .AsNoTracking()
                        .ToList();
}
```  
