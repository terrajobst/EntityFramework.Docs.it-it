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
# <a name="no-tracking-queries"></a>senza rilevamento delle modifiche
In alcuni casi è possibile tornare entità da una query, ma non hanno tali entità di essere rilevate dal contesto. Questo può comportare prestazioni migliori quando si eseguono query per un numero elevato di entità in scenari di sola lettura. Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.  

Un nuovo metodo di estensione AsNoTracking consente a qualsiasi query da eseguire in questo modo. Ad esempio:  

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
