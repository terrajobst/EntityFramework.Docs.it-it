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
# <a name="including--excluding-types"></a>Inclusione & esclusi i tipi

L'inclusione di un tipo nel modello significa che EF dispone di metadati relativi a tale tipo e tenterà di leggere e scrivere istanze da/nel database.

## <a name="conventions"></a>Convenzioni

Per convenzione, i tipi esposti nelle `DbSet` proprietà del contesto sono inclusi nel modello. Inoltre, sono inclusi anche i tipi citati `OnModelCreating` nel metodo. Infine, nel modello vengono inclusi anche tutti i tipi individuati in modo ricorsivo per esplorare le proprietà di navigazione dei tipi individuati.

**Ad esempio, nell'elenco di codice seguente vengono individuati tutti e tre i tipi:**

* `Blog`Poiché è esposto in una `DbSet` proprietà nel contesto

* `Post`Poiché viene individuato tramite la `Blog.Posts` proprietà di navigazione

* `AuditEntry`Poiché è indicato in`OnModelCreating`

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

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile utilizzare le annotazioni dei dati per escludere un tipo dal modello.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>API Fluent

È possibile utilizzare l'API Fluent per escludere un tipo dal modello.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
