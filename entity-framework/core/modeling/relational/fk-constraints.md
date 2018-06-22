---
title: Vincoli di chiave esterna - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
ms.technology: entity-framework-core
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 726f03e2ee4cd3ec851c9a861b75dd12f9203e9c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052741"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="8dd6d-102">Vincoli di chiave esterna</span><span class="sxs-lookup"><span data-stu-id="8dd6d-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="8dd6d-103">La configurazione di questa sezione è applicabile a database relazionali in generale.</span><span class="sxs-lookup"><span data-stu-id="8dd6d-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="8dd6d-104">I metodi di estensione qui verranno rese disponibili quando si installa un provider di database relazionali (a causa di condiviso *Microsoft.EntityFrameworkCore.Relational* pacchetto).</span><span class="sxs-lookup"><span data-stu-id="8dd6d-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="8dd6d-105">Un vincolo foreign key è stato introdotto per ogni relazione nel modello.</span><span class="sxs-lookup"><span data-stu-id="8dd6d-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="8dd6d-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="8dd6d-106">Conventions</span></span>

<span data-ttu-id="8dd6d-107">Per convenzione, i vincoli di chiave esterna sono denominati `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="8dd6d-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="8dd6d-108">Per le chiavi esterne composte `<foreign key property name>` diventa un elenco separato da un carattere di sottolineatura di nomi di proprietà di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="8dd6d-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="8dd6d-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="8dd6d-109">Data Annotations</span></span>

<span data-ttu-id="8dd6d-110">I nomi di vincolo di chiave esterna non possono essere configurati utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="8dd6d-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="8dd6d-111">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="8dd6d-111">Fluent API</span></span>

<span data-ttu-id="8dd6d-112">Per configurare il nome del vincolo di chiave esterna per una relazione, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="8dd6d-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/RelationshipConstraintName.cs?highlight=12)] -->
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
