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
# <a name="no-tracking-queries"></a>senza rilevamento delle modifiche
In alcuni casi può essere necessario recuperare le entità da una query, ma queste entità non vengono rilevate dal contesto. Ciò può comportare prestazioni migliori quando si eseguono query su un numero elevato di entità in scenari di sola lettura. Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.  

Un nuovo metodo di estensione AsNoTracking consente l'esecuzione di qualsiasi query in questo modo. Ad esempio:  

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
