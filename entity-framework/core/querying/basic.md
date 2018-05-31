---
title: Query di base - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
ms.technology: entity-framework-core
uid: core/querying/basic
ms.openlocfilehash: 5070faf2aeeffad680e24e7de5a0ffa03a8f0064
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052551"
---
# <a name="basic-queries"></a><span data-ttu-id="be144-102">Query di base</span><span class="sxs-lookup"><span data-stu-id="be144-102">Basic Queries</span></span>

<span data-ttu-id="be144-103">Informazioni su come caricare le entità dal database usando Language Integrate Query (LINQ).</span><span class="sxs-lookup"><span data-stu-id="be144-103">Learn how to load entities from the database using Language Integrate Query (LINQ).</span></span>

> [!TIP]  
> <span data-ttu-id="be144-104">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="be144-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="101-linq-samples"></a><span data-ttu-id="be144-105">101 esempi di LINQ</span><span class="sxs-lookup"><span data-stu-id="be144-105">101 LINQ samples</span></span>

<span data-ttu-id="be144-106">Questa pagina illustra alcuni esempi per eseguire attività comuni con Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="be144-106">This page shows a few examples to achieve common tasks with Entity Framework Core.</span></span> <span data-ttu-id="be144-107">Per un set completo di esempi delle possibilità offerte da LINQ, vedere [101 esempi di LINQ](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span><span class="sxs-lookup"><span data-stu-id="be144-107">For an extensive set of samples showing what is possible with LINQ, see [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="be144-108">Caricamento di tutti i dati</span><span class="sxs-lookup"><span data-stu-id="be144-108">Loading all data</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a><span data-ttu-id="be144-109">Caricamento di una singola entità</span><span class="sxs-lookup"><span data-stu-id="be144-109">Loading a single entity</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a><span data-ttu-id="be144-110">Filtro</span><span class="sxs-lookup"><span data-stu-id="be144-110">Filtering</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
