---
title: Query senza registrazione - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: dba4127ade9481b40d4fd3c4323532ddfedf6980
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994240"
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
