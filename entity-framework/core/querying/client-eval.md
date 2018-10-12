---
title: Valutazione client rispetto a server - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: 47e22be274d02b5221c638d07151d9607aa7e24f
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250803"
---
# <a name="client-vs-server-evaluation"></a>Valutazione client rispetto a server

Entity Framework Core supporta la valutazione di parti della query nel client e il push di altre parti al database. In questo caso, è il provider di database a determinare quali parti della query verranno valutate nel database.

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) di questo articolo in GitHub.

## <a name="client-evaluation"></a>Valutazione client

Nell'esempio seguente viene usato un metodo helper per standardizzare gli URL per i blog restituiti da un database di SQL Server. Dato che il provider SQL Server non dispone di alcuna informazione sulle modalità di implementazione di questo metodo, non è possibile convertirlo in SQL. Tutti gli altri aspetti della query vengono valutati nel database, ma il passaggio del valore `URL` restituito tramite questo metodo viene eseguito sul client.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs?highlight=6)] -->
``` csharp
var blogs = context.Blogs
    .OrderByDescending(blog => blog.Rating)
    .Select(blog => new
    {
        Id = blog.BlogId,
        Url = StandardizeUrl(blog.Url)
    })
    .ToList();
```

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
public static string StandardizeUrl(string url)
{
    url = url.ToLower();

    if (!url.StartsWith("http://"))
    {
        url = string.Concat("http://", url);
    }

    return url;
}
```

## <a name="client-evaluation-performance-issues"></a>Problemi relativi alle prestazioni della valutazione client

Anche se la valutazione client può essere molto utile, in alcuni casi può comportare prestazioni insufficienti. Si consideri la query seguente, in cui il metodo helper viene ora usato in un filtro. Dato che questa operazione non può essere eseguita nel database, per tutti i dati viene eseguito il pull in memoria e quindi il filtro viene applicato sul client. A seconda della quantità di dati e della quantità di dati esclusi dal filtro, ciò potrebbe causare prestazioni scarse.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a>Registrazione della valutazione client

Per impostazione predefinita, EF Core registrerà un avviso quando viene eseguita la valutazione client. Vedere [Registrazione](../miscellaneous/logging.md) per altre informazioni sulla visualizzazione dell'output di registrazione. 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a>Comportamento facoltativo: generare un'eccezione per la valutazione client

È possibile modificare il comportamento quando si verifica la valutazione client, in modo da generare un'eccezione o non eseguire alcuna operazione. Questa operazione viene eseguita quando si configurano le opzioni per il contesto, in genere in `DbContext.OnConfiguring` oppure in `Startup.cs` se si usa ASP.NET Core.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
