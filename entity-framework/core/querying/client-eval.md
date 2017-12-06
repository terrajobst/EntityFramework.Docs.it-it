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
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="e39aa-102">Visual Studio di client. Valutazione di server</span><span class="sxs-lookup"><span data-stu-id="e39aa-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="e39aa-103">Entity Framework Core supporta parti della query valutata sul client e le parti vengano applicate al database.</span><span class="sxs-lookup"><span data-stu-id="e39aa-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="e39aa-104">In questo caso, il provider del database per determinare quali parti della query verranno valutate nel database.</span><span class="sxs-lookup"><span data-stu-id="e39aa-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="e39aa-105">È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) su GitHub.</span><span class="sxs-lookup"><span data-stu-id="e39aa-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="e39aa-106">Valutazione client</span><span class="sxs-lookup"><span data-stu-id="e39aa-106">Client evaluation</span></span>

<span data-ttu-id="e39aa-107">Nell'esempio seguente viene utilizzato un metodo di supporto per standardizzare l'URL per i blog che vengono restituiti da un database di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e39aa-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="e39aa-108">Poiché il provider SQL Server non dispone di alcun approfondite sulle modalità di implementazione di questo metodo, non è possibile convertirla in SQL.</span><span class="sxs-lookup"><span data-stu-id="e39aa-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="e39aa-109">Tutti gli altri aspetti della query vengono valutati nel database, ma il passaggio restituito `URL` tramite questo metodo viene eseguito sul client.</span><span class="sxs-lookup"><span data-stu-id="e39aa-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

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

## <a name="disabling-client-evaluation"></a><span data-ttu-id="e39aa-110">La disabilitazione di valutazione client</span><span class="sxs-lookup"><span data-stu-id="e39aa-110">Disabling client evaluation</span></span>

<span data-ttu-id="e39aa-111">Durante la valutazione di client può essere molto utile, in alcuni casi può comportare una riduzione delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="e39aa-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="e39aa-112">Si consideri la query seguente, in cui il metodo helper viene ora usato in un filtro.</span><span class="sxs-lookup"><span data-stu-id="e39aa-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="e39aa-113">Poiché questa operazione non può essere eseguita nel database, tutti i dati vengono spostati in memoria e quindi il filtro viene applicato sul client.</span><span class="sxs-lookup"><span data-stu-id="e39aa-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="e39aa-114">A seconda della quantità di dati e la quantità di dati vengono filtrati, ciò potrebbe causare un peggioramento delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="e39aa-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

<span data-ttu-id="e39aa-115">Per impostazione predefinita, Core EF registrerà un avviso quando viene eseguita la valutazione dei client.</span><span class="sxs-lookup"><span data-stu-id="e39aa-115">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="e39aa-116">Vedere [registrazione](../miscellaneous/logging.md) per ulteriori informazioni sulla visualizzazione dell'output di registrazione.</span><span class="sxs-lookup"><span data-stu-id="e39aa-116">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="e39aa-117">È possibile modificare il comportamento quando si verifica di valutazione client per generare o non eseguono alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="e39aa-117">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="e39aa-118">Questa operazione viene eseguita quando si configura le opzioni per il contesto, in genere in `DbContext.OnConfiguring`, o in `Startup.cs` se si utilizza ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e39aa-118">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
