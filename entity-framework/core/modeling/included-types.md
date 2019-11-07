---
title: Inclusione & esclusi i tipi-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: 1249e71c02e58afe7fe06b3fdcf523dfa0c9b17c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655731"
---
# <a name="including--excluding-types"></a><span data-ttu-id="c4c72-102">Inclusione ed esclusione di tipi</span><span class="sxs-lookup"><span data-stu-id="c4c72-102">Including & Excluding Types</span></span>

<span data-ttu-id="c4c72-103">L'inclusione di un tipo nel modello significa che EF dispone di metadati relativi a tale tipo e tenterà di leggere e scrivere istanze da/nel database.</span><span class="sxs-lookup"><span data-stu-id="c4c72-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="c4c72-104">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="c4c72-104">Conventions</span></span>

<span data-ttu-id="c4c72-105">Per convenzione, i tipi esposti in `DbSet` proprietà del contesto sono inclusi nel modello.</span><span class="sxs-lookup"><span data-stu-id="c4c72-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="c4c72-106">Sono inoltre inclusi i tipi indicati nel metodo `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="c4c72-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="c4c72-107">Infine, nel modello vengono inclusi anche tutti i tipi individuati in modo ricorsivo per esplorare le proprietà di navigazione dei tipi individuati.</span><span class="sxs-lookup"><span data-stu-id="c4c72-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="c4c72-108">**Ad esempio, nell'elenco di codice seguente vengono individuati tutti e tre i tipi:**</span><span class="sxs-lookup"><span data-stu-id="c4c72-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="c4c72-109">`Blog` perché è esposto in una proprietà `DbSet` nel contesto</span><span class="sxs-lookup"><span data-stu-id="c4c72-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="c4c72-110">`Post` perché viene individuato tramite la proprietà di navigazione `Blog.Posts`</span><span class="sxs-lookup"><span data-stu-id="c4c72-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="c4c72-111">`AuditEntry` perché è indicato in `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="c4c72-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/IncludedTypes.cs?name=IncludedTypes&highlight=3,7,16)]

## <a name="data-annotations"></a><span data-ttu-id="c4c72-112">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="c4c72-112">Data Annotations</span></span>

<span data-ttu-id="c4c72-113">È possibile utilizzare le annotazioni dei dati per escludere un tipo dal modello.</span><span class="sxs-lookup"><span data-stu-id="c4c72-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="c4c72-114">API Fluent</span><span class="sxs-lookup"><span data-stu-id="c4c72-114">Fluent API</span></span>

<span data-ttu-id="c4c72-115">È possibile utilizzare l'API Fluent per escludere un tipo dal modello.</span><span class="sxs-lookup"><span data-stu-id="c4c72-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
