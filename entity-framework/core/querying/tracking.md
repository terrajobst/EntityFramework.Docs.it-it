---
title: Query con rilevamento delle modifiche e senza rilevamento delle modifiche - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: d93be5c2b727d8fbaddd103f8f367c699ae80a7c
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921648"
---
# <a name="tracking-vs-no-tracking-queries"></a>Query con rilevamento delle modifiche e senza rilevamento delle modifiche

Dal comportamento di rilevamento delle modifiche dipende se Entity Framework Core conserverà o meno le informazioni relative a un'istanza di entità nel relativo strumento di rilevamento delle modifiche. Se un'entità viene inclusa nel rilevamento delle modifiche, qualsiasi modifica individuata per l'entità verrà salvata in modo permanente nel database durante `SaveChanges()`. Entity Framework Core correggerà anche le proprietà di navigazione tra le entità ottenute da una query con rilevamento delle modifiche e le entità caricate in precedenza nell'istanza di DbContext.

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) di questo articolo in GitHub.

## <a name="tracking-queries"></a>Query con rilevamento delle modifiche

Per impostazione predefinita, le query che restituiscono tipi di entità sono con rilevamento delle modifiche. Ciò significa che è possibile apportare modifiche alle istanze di entità e le modifiche vengono salvate in modo permanente da `SaveChanges()`.

Nell'esempio seguente la modifica della classificazione del blog verrà rilevata e salvata in modo permanente nel database durante `SaveChanges()`.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>Query senza registrazione

Le query senza rilevamento delle modifiche sono utili quando i risultati vengono usati in uno scenario di sola lettura. Sono più veloci da eseguire perché non è necessario configurare le informazioni per il rilevamento delle modifiche.

È possibile configurare una singola query in modo che sia senza rilevamento delle modifiche:

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

È anche possibile modificare il comportamento predefinito di rilevamento delle modifiche a livello di istanza di contesto:

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> Le query senza rilevamento delle modifiche eseguono comunque la risoluzione dell'identità all'interno della query in esecuzione. Se il set di risultati contiene la stessa entità più volte, verrà restituita la stessa istanza della classe di entità per ogni occorrenza nel set di risultati. Vengono tuttavia usati riferimenti deboli per tenere traccia delle entità già restituite. Se un risultato precedente con la stessa identità esce dall'ambito e viene eseguita la Garbage Collection, si potrebbe ottenere una nuova istanza di entità. Per altre informazioni, vedere [Funzionamento delle query](overview.md).

## <a name="tracking-and-projections"></a>Rilevamento delle modifiche e proiezioni

Anche se il tipo di risultato della query non è un tipo di entità, se il risultato contiene tipi di entità questi verranno comunque inclusi nel rilevamento delle modifiche per impostazione predefinita. Nella query seguente, che restituisce un tipo anonimo, le istanze di `Blog` nel set di risultati verranno incluse nel rilevamento delle modifiche.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=7)] -->
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

Se il set di risultati non contiene alcun tipo di entità, il rilevamento delle modifiche non viene eseguito. Nella query seguente, che restituisce un tipo anonimo con alcuni dei valori dell'entità (ma non le istanze del tipo di entità effettivo), il rilevamento delle modifiche non viene eseguito.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs)] -->
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
