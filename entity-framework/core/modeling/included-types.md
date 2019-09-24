---
title: Inclusione & esclusi i tipi-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: ca83b1c432bdf4853dba81e12ec4a739bc8218dc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197384"
---
# <a name="including--excluding-types"></a><span data-ttu-id="e3d98-102">Inclusione & esclusi i tipi</span><span class="sxs-lookup"><span data-stu-id="e3d98-102">Including & Excluding Types</span></span>

<span data-ttu-id="e3d98-103">L'inclusione di un tipo nel modello significa che EF dispone di metadati relativi a tale tipo e tenterà di leggere e scrivere istanze da/nel database.</span><span class="sxs-lookup"><span data-stu-id="e3d98-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="e3d98-104">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="e3d98-104">Conventions</span></span>

<span data-ttu-id="e3d98-105">Per convenzione, i tipi esposti nelle `DbSet` proprietà del contesto sono inclusi nel modello.</span><span class="sxs-lookup"><span data-stu-id="e3d98-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="e3d98-106">Inoltre, sono inclusi anche i tipi citati `OnModelCreating` nel metodo.</span><span class="sxs-lookup"><span data-stu-id="e3d98-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="e3d98-107">Infine, nel modello vengono inclusi anche tutti i tipi individuati in modo ricorsivo per esplorare le proprietà di navigazione dei tipi individuati.</span><span class="sxs-lookup"><span data-stu-id="e3d98-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="e3d98-108">**Ad esempio, nell'elenco di codice seguente vengono individuati tutti e tre i tipi:**</span><span class="sxs-lookup"><span data-stu-id="e3d98-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="e3d98-109">`Blog`Poiché è esposto in una `DbSet` proprietà nel contesto</span><span class="sxs-lookup"><span data-stu-id="e3d98-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="e3d98-110">`Post`Poiché viene individuato tramite la `Blog.Posts` proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="e3d98-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="e3d98-111">`AuditEntry`Poiché è indicato in`OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="e3d98-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/IncludedTypes.cs?highlight=3,7,16)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<AuditEntry>();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}

public class AuditEntry
{
    public int AuditEntryId { get; set; }
    public string Username { get; set; }
    public string Action { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="e3d98-112">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="e3d98-112">Data Annotations</span></span>

<span data-ttu-id="e3d98-113">È possibile utilizzare le annotazioni dei dati per escludere un tipo dal modello.</span><span class="sxs-lookup"><span data-stu-id="e3d98-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="e3d98-114">API Fluent</span><span class="sxs-lookup"><span data-stu-id="e3d98-114">Fluent API</span></span>

<span data-ttu-id="e3d98-115">È possibile utilizzare l'API Fluent per escludere un tipo dal modello.</span><span class="sxs-lookup"><span data-stu-id="e3d98-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
