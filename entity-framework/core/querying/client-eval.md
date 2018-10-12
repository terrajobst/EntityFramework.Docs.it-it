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
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="57ee7-102">Valutazione client rispetto a server</span><span class="sxs-lookup"><span data-stu-id="57ee7-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="57ee7-103">Entity Framework Core supporta la valutazione di parti della query nel client e il push di altre parti al database.</span><span class="sxs-lookup"><span data-stu-id="57ee7-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="57ee7-104">In questo caso, è il provider di database a determinare quali parti della query verranno valutate nel database.</span><span class="sxs-lookup"><span data-stu-id="57ee7-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="57ee7-105">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="57ee7-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="57ee7-106">Valutazione client</span><span class="sxs-lookup"><span data-stu-id="57ee7-106">Client evaluation</span></span>

<span data-ttu-id="57ee7-107">Nell'esempio seguente viene usato un metodo helper per standardizzare gli URL per i blog restituiti da un database di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="57ee7-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="57ee7-108">Dato che il provider SQL Server non dispone di alcuna informazione sulle modalità di implementazione di questo metodo, non è possibile convertirlo in SQL.</span><span class="sxs-lookup"><span data-stu-id="57ee7-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="57ee7-109">Tutti gli altri aspetti della query vengono valutati nel database, ma il passaggio del valore `URL` restituito tramite questo metodo viene eseguito sul client.</span><span class="sxs-lookup"><span data-stu-id="57ee7-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

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

## <a name="client-evaluation-performance-issues"></a><span data-ttu-id="57ee7-110">Problemi relativi alle prestazioni della valutazione client</span><span class="sxs-lookup"><span data-stu-id="57ee7-110">Client evaluation performance issues</span></span>

<span data-ttu-id="57ee7-111">Anche se la valutazione client può essere molto utile, in alcuni casi può comportare prestazioni insufficienti.</span><span class="sxs-lookup"><span data-stu-id="57ee7-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="57ee7-112">Si consideri la query seguente, in cui il metodo helper viene ora usato in un filtro.</span><span class="sxs-lookup"><span data-stu-id="57ee7-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="57ee7-113">Dato che questa operazione non può essere eseguita nel database, per tutti i dati viene eseguito il pull in memoria e quindi il filtro viene applicato sul client.</span><span class="sxs-lookup"><span data-stu-id="57ee7-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="57ee7-114">A seconda della quantità di dati e della quantità di dati esclusi dal filtro, ciò potrebbe causare prestazioni scarse.</span><span class="sxs-lookup"><span data-stu-id="57ee7-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a><span data-ttu-id="57ee7-115">Registrazione della valutazione client</span><span class="sxs-lookup"><span data-stu-id="57ee7-115">Client evaluation logging</span></span>

<span data-ttu-id="57ee7-116">Per impostazione predefinita, EF Core registrerà un avviso quando viene eseguita la valutazione client.</span><span class="sxs-lookup"><span data-stu-id="57ee7-116">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="57ee7-117">Vedere [Registrazione](../miscellaneous/logging.md) per altre informazioni sulla visualizzazione dell'output di registrazione.</span><span class="sxs-lookup"><span data-stu-id="57ee7-117">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a><span data-ttu-id="57ee7-118">Comportamento facoltativo: generare un'eccezione per la valutazione client</span><span class="sxs-lookup"><span data-stu-id="57ee7-118">Optional behavior: throw an exception for client evaluation</span></span>

<span data-ttu-id="57ee7-119">È possibile modificare il comportamento quando si verifica la valutazione client, in modo da generare un'eccezione o non eseguire alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="57ee7-119">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="57ee7-120">Questa operazione viene eseguita quando si configurano le opzioni per il contesto, in genere in `DbContext.OnConfiguring` oppure in `Startup.cs` se si usa ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="57ee7-120">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
