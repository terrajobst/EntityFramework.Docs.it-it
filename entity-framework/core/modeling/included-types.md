---
title: Inclusione ed esclusione di tipi - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: a5a14f62524754fed179e9a41fac5e29faf185ca
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996150"
---
# <a name="including--excluding-types"></a><span data-ttu-id="80ef9-102">Inclusione ed esclusione di tipi</span><span class="sxs-lookup"><span data-stu-id="80ef9-102">Including & Excluding Types</span></span>

<span data-ttu-id="80ef9-103">Includere un tipo i mezzi di modello EF con metadati su quel tipo e tentano di leggere e scrivere istanze da e verso il database.</span><span class="sxs-lookup"><span data-stu-id="80ef9-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="80ef9-104">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="80ef9-104">Conventions</span></span>

<span data-ttu-id="80ef9-105">Per convenzione, i tipi esposti nella `DbSet` le proprietà nel contesto sono incluse nel modello.</span><span class="sxs-lookup"><span data-stu-id="80ef9-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="80ef9-106">Inoltre, i tipi menzionati nel `OnModelCreating` metodo sono inoltre inclusi.</span><span class="sxs-lookup"><span data-stu-id="80ef9-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="80ef9-107">Infine, tutti i tipi che si trovano in modo ricorsivo esplorando le proprietà di navigazione dei tipi individuati sono inoltre inclusi nel modello.</span><span class="sxs-lookup"><span data-stu-id="80ef9-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="80ef9-108">**Ad esempio, nel listato di codice seguente vengono individuati tutti i tre tipi:**</span><span class="sxs-lookup"><span data-stu-id="80ef9-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="80ef9-109">`Blog` quanto è esposta un `DbSet` proprietà di contesto</span><span class="sxs-lookup"><span data-stu-id="80ef9-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="80ef9-110">`Post` Poiché viene individuato tramite il `Blog.Posts` proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="80ef9-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="80ef9-111">`AuditEntry` perché viene citata in `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="80ef9-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/IncludedTypes.cs?highlight=3,7,16)] -->
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

## <a name="data-annotations"></a><span data-ttu-id="80ef9-112">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="80ef9-112">Data Annotations</span></span>

<span data-ttu-id="80ef9-113">È possibile usare le annotazioni dei dati per escludere un tipo dal modello.</span><span class="sxs-lookup"><span data-stu-id="80ef9-113">You can use Data Annotations to exclude a type from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=9)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

[NotMapped]
public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="80ef9-114">API Fluent</span><span class="sxs-lookup"><span data-stu-id="80ef9-114">Fluent API</span></span>

<span data-ttu-id="80ef9-115">È possibile usare l'API Fluent per escludere un tipo dal modello.</span><span class="sxs-lookup"><span data-stu-id="80ef9-115">You can use the Fluent API to exclude a type from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Ignore<BlogMetadata>();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```
