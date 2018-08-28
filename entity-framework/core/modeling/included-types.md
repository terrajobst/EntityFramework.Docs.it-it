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
# <a name="including--excluding-types"></a>Inclusione ed esclusione di tipi

Includere un tipo i mezzi di modello EF con metadati su quel tipo e tentano di leggere e scrivere istanze da e verso il database.

## <a name="conventions"></a>Convenzioni

Per convenzione, i tipi esposti nella `DbSet` le proprietà nel contesto sono incluse nel modello. Inoltre, i tipi menzionati nel `OnModelCreating` metodo sono inoltre inclusi. Infine, tutti i tipi che si trovano in modo ricorsivo esplorando le proprietà di navigazione dei tipi individuati sono inoltre inclusi nel modello.

**Ad esempio, nel listato di codice seguente vengono individuati tutti i tre tipi:**

* `Blog` quanto è esposta un `DbSet` proprietà di contesto

* `Post` Poiché viene individuato tramite il `Blog.Posts` proprietà di navigazione

* `AuditEntry` perché viene citata in `OnModelCreating`

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

È possibile usare le annotazioni dei dati per escludere un tipo dal modello.

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

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per escludere un tipo dal modello.

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
