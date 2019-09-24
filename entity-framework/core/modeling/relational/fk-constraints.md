---
title: Vincoli FOREIGN KEY-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: d7ed4466f4df9ec01267b048ba1bbcc6e8bbdad5
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197065"
---
# <a name="foreign-key-constraints"></a>Vincoli FOREIGN KEY

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Un vincolo FOREIGN KEY viene introdotto per ogni relazione nel modello.

## <a name="conventions"></a>Convenzioni

Per convenzione, i vincoli di chiave esterna `FK_<dependent type name>_<principal type name>_<foreign key property name>`vengono denominati. Per le chiavi `<foreign key property name>` esterne composite diventa un elenco delimitato da caratteri di sottolineatura dei nomi delle proprietà di chiave esterna.

## <a name="data-annotations"></a>Annotazioni dei dati

Impossibile configurare nomi di vincoli di chiave esterna utilizzando le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

Per configurare il nome del vincolo FOREIGN KEY per una relazione, è possibile usare l'API Fluent.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogId)
            .HasConstraintName("ForeignKey_Post_Blog");
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

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```
