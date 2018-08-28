---
title: Vincoli di chiave esterna - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: a83f72b5d832e349fb4a5fb3b2de0b82bd79ef2a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993988"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="7f57c-102">Vincoli di chiave esterna</span><span class="sxs-lookup"><span data-stu-id="7f57c-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="7f57c-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="7f57c-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="7f57c-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="7f57c-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="7f57c-105">Un vincolo foreign key è stato introdotto per ogni relazione nel modello.</span><span class="sxs-lookup"><span data-stu-id="7f57c-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="7f57c-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="7f57c-106">Conventions</span></span>

<span data-ttu-id="7f57c-107">Per convenzione, i vincoli di chiave esterni sono denominati `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span><span class="sxs-lookup"><span data-stu-id="7f57c-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="7f57c-108">Per una chiave esterna composta `<foreign key property name>` diventa un elenco separato da un carattere di sottolineatura di nomi di proprietà di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="7f57c-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="7f57c-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="7f57c-109">Data Annotations</span></span>

<span data-ttu-id="7f57c-110">I nomi di vincolo di chiave esterna non possono essere configurati tramite le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="7f57c-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="7f57c-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="7f57c-111">Fluent API</span></span>

<span data-ttu-id="7f57c-112">È possibile utilizzare l'API Fluent per configurare il nome del vincolo di chiave esterna per una relazione.</span><span class="sxs-lookup"><span data-stu-id="7f57c-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

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
