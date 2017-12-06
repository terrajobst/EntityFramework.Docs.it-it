---
title: Visual Studio di client. Valutazione di Server - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
ms.technology: entity-framework-core
uid: core/querying/client-eval
ms.openlocfilehash: e1852b780041e9e92fb4d25129175346e3a601a3
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="client-vs-server-evaluation"></a>Visual Studio di client. Valutazione di server

Entity Framework Core supporta parti della query valutata sul client e le parti vengano applicate al database. In questo caso, il provider del database per determinare quali parti della query verranno valutate nel database.

> [!TIP]  
> È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) su GitHub.

## <a name="client-evaluation"></a>Valutazione client

Nell'esempio seguente viene utilizzato un metodo di supporto per standardizzare l'URL per i blog che vengono restituiti da un database di SQL Server. Poiché il provider SQL Server non dispone di alcun approfondite sulle modalità di implementazione di questo metodo, non è possibile convertirla in SQL. Tutti gli altri aspetti della query vengono valutati nel database, ma il passaggio restituito `URL` tramite questo metodo viene eseguito sul client.

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

## <a name="disabling-client-evaluation"></a>La disabilitazione di valutazione client

Durante la valutazione di client può essere molto utile, in alcuni casi può comportare una riduzione delle prestazioni. Si consideri la query seguente, in cui il metodo helper viene ora usato in un filtro. Poiché questa operazione non può essere eseguita nel database, tutti i dati vengono spostati in memoria e quindi il filtro viene applicato sul client. A seconda della quantità di dati e la quantità di dati vengono filtrati, ciò potrebbe causare un peggioramento delle prestazioni.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

Per impostazione predefinita, Core EF registrerà un avviso quando viene eseguita la valutazione dei client. Vedere [registrazione](../miscellaneous/logging.md) per ulteriori informazioni sulla visualizzazione dell'output di registrazione. È possibile modificare il comportamento quando si verifica di valutazione client per generare o non eseguono alcuna operazione. Questa operazione viene eseguita quando si configura le opzioni per il contesto, in genere in `DbContext.OnConfiguring`, o in `Startup.cs` se si utilizza ASP.NET Core.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
