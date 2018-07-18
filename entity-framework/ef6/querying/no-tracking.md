---
title: Query senza registrazione - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
caps.latest.revision: 3
ms.openlocfilehash: 8310f2dab9e7ed9197a8c3e875e47e4f7b72d279
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121130"
---
# <a name="no-tracking-queries"></a><span data-ttu-id="4144d-102">senza rilevamento delle modifiche</span><span class="sxs-lookup"><span data-stu-id="4144d-102">No-Tracking Queries</span></span>
<span data-ttu-id="4144d-103">In alcuni casi è possibile tornare entità da una query, ma non hanno tali entità di essere rilevate dal contesto.</span><span class="sxs-lookup"><span data-stu-id="4144d-103">Sometimes you may want to get entities back from a query but not have those entities be tracked by the context.</span></span> <span data-ttu-id="4144d-104">Questo può comportare prestazioni migliori quando si eseguono query per un numero elevato di entità in scenari di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="4144d-104">This may result in better performance when querying for large numbers of entities in read-only scenarios.</span></span> <span data-ttu-id="4144d-105">Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.</span><span class="sxs-lookup"><span data-stu-id="4144d-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="4144d-106">Un nuovo metodo di estensione AsNoTracking consente a qualsiasi query da eseguire in questo modo.</span><span class="sxs-lookup"><span data-stu-id="4144d-106">A new extension method AsNoTracking allows any query to be run in this way.</span></span> <span data-ttu-id="4144d-107">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4144d-107">For example:</span></span>  

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
