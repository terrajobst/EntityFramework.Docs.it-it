---
title: Query senza registrazione - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: 44d58e14a2550bd08a8edd68b467237f6f5b5978
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490121"
---
# <a name="no-tracking-queries"></a><span data-ttu-id="125cf-102">senza rilevamento delle modifiche</span><span class="sxs-lookup"><span data-stu-id="125cf-102">No-Tracking Queries</span></span>
<span data-ttu-id="125cf-103">In alcuni casi è possibile tornare entità da una query, ma non hanno tali entità di essere rilevate dal contesto.</span><span class="sxs-lookup"><span data-stu-id="125cf-103">Sometimes you may want to get entities back from a query but not have those entities be tracked by the context.</span></span> <span data-ttu-id="125cf-104">Questo può comportare prestazioni migliori quando si eseguono query per un numero elevato di entità in scenari di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="125cf-104">This may result in better performance when querying for large numbers of entities in read-only scenarios.</span></span> <span data-ttu-id="125cf-105">Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.</span><span class="sxs-lookup"><span data-stu-id="125cf-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="125cf-106">Un nuovo metodo di estensione AsNoTracking consente a qualsiasi query da eseguire in questo modo.</span><span class="sxs-lookup"><span data-stu-id="125cf-106">A new extension method AsNoTracking allows any query to be run in this way.</span></span> <span data-ttu-id="125cf-107">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="125cf-107">For example:</span></span>  

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
