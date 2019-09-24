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
# <a name="foreign-key-constraints"></a><span data-ttu-id="3dd82-102">Vincoli FOREIGN KEY</span><span class="sxs-lookup"><span data-stu-id="3dd82-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="3dd82-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="3dd82-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3dd82-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="3dd82-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3dd82-105">Un vincolo FOREIGN KEY viene introdotto per ogni relazione nel modello.</span><span class="sxs-lookup"><span data-stu-id="3dd82-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="3dd82-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="3dd82-106">Conventions</span></span>

<span data-ttu-id="3dd82-107">Per convenzione, i vincoli di chiave esterna `FK_<dependent type name>_<principal type name>_<foreign key property name>`vengono denominati.</span><span class="sxs-lookup"><span data-stu-id="3dd82-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="3dd82-108">Per le chiavi `<foreign key property name>` esterne composite diventa un elenco delimitato da caratteri di sottolineatura dei nomi delle proprietà di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="3dd82-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3dd82-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="3dd82-109">Data Annotations</span></span>

<span data-ttu-id="3dd82-110">Impossibile configurare nomi di vincoli di chiave esterna utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="3dd82-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="3dd82-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="3dd82-111">Fluent API</span></span>

<span data-ttu-id="3dd82-112">Per configurare il nome del vincolo FOREIGN KEY per una relazione, è possibile usare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="3dd82-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

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
