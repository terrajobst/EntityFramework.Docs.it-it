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
ms.locfileid: "26052601"
---
# <a name="including--excluding-types"></a>Tra cui & esclusi i tipi

Tra cui un tipo di modello significa EF contiene metadati su quel tipo e verrà eseguito un tentativo di leggere e scrivere istanze dal/al database.

## <a name="conventions"></a>Convenzioni

Per convenzione, i tipi che vengono esposte in `DbSet` le proprietà del contesto vengono incluse nel modello. Inoltre, i tipi menzionati nel `OnModelCreating` metodo sono inoltre incluse. Infine, anche tutti i tipi che si trovano in modo ricorsivo esplorare le proprietà di navigazione dei tipi individuati vengono inclusi nel modello.

**Ad esempio, nel listato di codice seguente vengono individuati tutti i tre tipi:**

* `Blog`Poiché è esposta in un `DbSet` proprietà nel contesto

* `Post`Poiché viene individuato tramite il `Blog.Posts` proprietà di navigazione

* `AuditEntry`perché viene citata in`OnModelCreating`

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

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile utilizzare le annotazioni dei dati per escludere un tipo dal modello.

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

## <a name="fluent-api"></a>Microsoft Office Fluent API

È possibile utilizzare l'API Fluent per escludere un tipo dal modello.

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
