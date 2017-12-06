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
# <a name="tracking-vs-no-tracking-queries"></a>Rilevamento di Visual Studio. Query di rilevamento di No

Controlli di comportamento di rilevamento Entity Framework Core conserverà le informazioni relative a un'istanza di entità nel rilevamento delle modifiche o meno. Se viene rilevata un'entità, le modifiche rilevate nell'entità verranno rese persistenti nel database durante `SaveChanges()`. Entity Framework Core verrà correzione anche le proprietà di navigazione tra le entità che vengono ottenute da una query di rilevamento e che sono stati caricati in precedenza nell'istanza di DbContext.

> [!TIP]  
> È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) su GitHub.

## <a name="tracking-queries"></a>Query di rilevamento

Per impostazione predefinita, le query che restituiscono tipi di entità sono di rilevamento. In questo modo è possibile apportare modifiche alle istanze di entità e le modifiche sono rese persistenti dal `SaveChanges()`.

Nell'esempio seguente, rilevata e resi persistenti nel database durante la modifica per la classificazione di blog `SaveChanges()`.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>Query di rilevamento di No

Nessuna query di rilevamento è utile quando i risultati vengono usati in uno scenario di sola lettura. Sono più veloci per l'esecuzione perché non è necessario per informazioni sul rilevamento delle modifiche di configurazione.

È possibile scambiare una singola query di rilevamento non essere:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

È inoltre possibile modificare il comportamento a livello di istanza di contesto di rilevamento predefinito:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> Nessuna query di rilevamento comunque esegue la risoluzione di identità all'interno della query di eseguire. Se il set di risultati contiene la stessa entità più volte, restituirà la stessa istanza della classe di entità per ogni occorrenza del set di risultati. Riferimenti deboli, tuttavia, vengono utilizzati per tenere traccia delle entità che sono già state restituite. Se un risultato precedente con la stessa identità abbandona l'ambito e l'esecuzione della garbage collection, è possibile ottenere una nuova istanza di entità. Per ulteriori informazioni, vedere [come funziona la Query](overview.md).

## <a name="tracking-and-projections"></a>Rilevamento e le proiezioni

Anche se il tipo di risultato della query non è un tipo di entità, se il risultato contiene i tipi di entità sono comunque essere registrati per impostazione predefinita. Nella query seguente, che restituisce un tipo anonimo, le istanze di `Blog` nel risultato di rilevamento dei set.

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

Se il set di risultati non contiene alcun tipo di entità, non viene eseguita alcuna registrazione. Nella query seguente, che restituisce un tipo anonimo con alcuni dei valori dell'entità (ma non le istanze del tipo di entità effettivo), non c'è alcun rilevamento eseguita.

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
