---
title: Tag delle query - EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 73C7A627-C8E9-452D-9CD5-AFCC8FEFE395
uid: core/querying/tags
ms.openlocfilehash: 3a4d516cab5836c659e42d825c4f1bf89355d671
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688754"
---
# <a name="query-tags"></a><span data-ttu-id="e23cf-102">Tag delle query</span><span class="sxs-lookup"><span data-stu-id="e23cf-102">Query tags</span></span>
> [!NOTE]
> <span data-ttu-id="e23cf-103">Questa funzionalità è stata introdotta in EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="e23cf-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="e23cf-104">Questa funzionalità consente di correlare le query LINQ nel codice con query SQL generate acquisite nei log.</span><span class="sxs-lookup"><span data-stu-id="e23cf-104">This feature helps correlate LINQ queries in code with generated SQL queries captured in logs.</span></span>
<span data-ttu-id="e23cf-105">È possibile annotare una query LINQ usando il nuovo metodo `TagWith()`:</span><span class="sxs-lookup"><span data-stu-id="e23cf-105">You annotate a LINQ query using the new `TagWith()` method:</span></span> 

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="e23cf-106">Questa query LINQ viene convertita nell'istruzione SQL seguente:</span><span class="sxs-lookup"><span data-stu-id="e23cf-106">This LINQ query is translated to the following SQL statement:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="e23cf-107">È possibile chiamare `TagWith()` più volte sulla stessa query.</span><span class="sxs-lookup"><span data-stu-id="e23cf-107">It's possible to call `TagWith()` many times on the same query.</span></span>
<span data-ttu-id="e23cf-108">I tag delle query sono cumulativi.</span><span class="sxs-lookup"><span data-stu-id="e23cf-108">Query tags are cumulative.</span></span>
<span data-ttu-id="e23cf-109">Considerati i metodi seguenti, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e23cf-109">For example, given the following methods:</span></span>

``` csharp
IQueryable<Friend> GetNearestFriends(Point myLocation) =>
    from f in context.Friends.TagWith("GetNearestFriends")
    orderby f.Location.Distance(myLocation) descending
    select f;

IQueryable<T> Limit<T>(IQueryable<T> source, int limit) =>
    source.TagWith("Limit").Take(limit);
```

<span data-ttu-id="e23cf-110">La query seguente:</span><span class="sxs-lookup"><span data-stu-id="e23cf-110">The following query:</span></span>   

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).ToList();
```

<span data-ttu-id="e23cf-111">Viene convertita in:</span><span class="sxs-lookup"><span data-stu-id="e23cf-111">Translates to:</span></span>

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="e23cf-112">È anche possibile usare stringhe con più righe come tag di query.</span><span class="sxs-lookup"><span data-stu-id="e23cf-112">It's also possible to use multi-line strings as query tags.</span></span>
<span data-ttu-id="e23cf-113">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e23cf-113">For example:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

<span data-ttu-id="e23cf-114">Produce il codice SQL seguente:</span><span class="sxs-lookup"><span data-stu-id="e23cf-114">Produces the following SQL:</span></span>

``` sql
-- GetNearestFriends

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a><span data-ttu-id="e23cf-115">Limitazioni note</span><span class="sxs-lookup"><span data-stu-id="e23cf-115">Known limitations</span></span>
<span data-ttu-id="e23cf-116">**I tag delle query non sono parametrizzabili:** EF Core considera sempre i tag delle query nella query LINQ come valori letterali stringa inclusi nel codice SQL generato.</span><span class="sxs-lookup"><span data-stu-id="e23cf-116">**Query tags aren't parameterizable:** EF Core always treats query tags in the LINQ query as string literals that are included in the generated SQL.</span></span>
<span data-ttu-id="e23cf-117">Non sono consentite query compilate che accettano i tag di query come parametri.</span><span class="sxs-lookup"><span data-stu-id="e23cf-117">Compiled queries that take query tags as parameters aren't allowed.</span></span>