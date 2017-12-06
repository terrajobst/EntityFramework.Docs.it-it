---
title: Tra cui & esclusi i tipi - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
ms.technology: entity-framework-core
uid: core/modeling/included-types
ms.openlocfilehash: a8d7293a144968d2506bdcc76e55a1a0b1e3fd4b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="including--excluding-types"></a><span data-ttu-id="c35cb-102">Tra cui & esclusi i tipi</span><span class="sxs-lookup"><span data-stu-id="c35cb-102">Including & Excluding Types</span></span>

<span data-ttu-id="c35cb-103">Tra cui un tipo di modello significa EF contiene metadati su quel tipo e verrà eseguito un tentativo di leggere e scrivere istanze dal/al database.</span><span class="sxs-lookup"><span data-stu-id="c35cb-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="c35cb-104">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="c35cb-104">Conventions</span></span>

<span data-ttu-id="c35cb-105">Per convenzione, i tipi che vengono esposte in `DbSet` le proprietà del contesto vengono incluse nel modello.</span><span class="sxs-lookup"><span data-stu-id="c35cb-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="c35cb-106">Inoltre, i tipi menzionati nel `OnModelCreating` metodo sono inoltre incluse.</span><span class="sxs-lookup"><span data-stu-id="c35cb-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="c35cb-107">Infine, anche tutti i tipi che si trovano in modo ricorsivo esplorare le proprietà di navigazione dei tipi individuati vengono inclusi nel modello.</span><span class="sxs-lookup"><span data-stu-id="c35cb-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="c35cb-108">**Ad esempio, nel listato di codice seguente vengono individuati tutti i tre tipi:**</span><span class="sxs-lookup"><span data-stu-id="c35cb-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="c35cb-109">`Blog`Poiché è esposta in un `DbSet` proprietà nel contesto</span><span class="sxs-lookup"><span data-stu-id="c35cb-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="c35cb-110">`Post`Poiché viene individuato tramite il `Blog.Posts` proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="c35cb-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="c35cb-111">`AuditEntry`perché viene citata in`OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="c35cb-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="c35cb-112">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="c35cb-112">Data Annotations</span></span>

<span data-ttu-id="c35cb-113">È possibile utilizzare le annotazioni dei dati per escludere un tipo dal modello.</span><span class="sxs-lookup"><span data-stu-id="c35cb-113">You can use Data Annotations to exclude a type from the model.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="c35cb-114">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="c35cb-114">Fluent API</span></span>

<span data-ttu-id="c35cb-115">È possibile utilizzare l'API Fluent per escludere un tipo dal modello.</span><span class="sxs-lookup"><span data-stu-id="c35cb-115">You can use the Fluent API to exclude a type from the model.</span></span>

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
