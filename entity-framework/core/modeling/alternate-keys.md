---
title: Chiavi alternative - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
ms.technology: entity-framework-core
uid: core/modeling/alternate-keys
ms.openlocfilehash: 09f86a8932b71ec8f30ee90a088091a00233c20f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052471"
---
# <a name="alternate-keys"></a><span data-ttu-id="1b908-102">Chiavi alternative</span><span class="sxs-lookup"><span data-stu-id="1b908-102">Alternate Keys</span></span>

<span data-ttu-id="1b908-103">Una chiave alternativa viene utilizzato come un identificatore univoco alternativo per ogni istanza di entità oltre alla chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="1b908-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="1b908-104">Le chiavi alternative utilizzabile come destinazione di una relazione.</span><span class="sxs-lookup"><span data-stu-id="1b908-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="1b908-105">Quando si utilizza un database relazionale esegue il mapping al concetto di un indice o vincolo unique per le colonne di chiave alternative e uno o più vincoli di chiave esterna che fanno riferimento le colonne.</span><span class="sxs-lookup"><span data-stu-id="1b908-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="1b908-106">Se si desidera applicare l'univocità di una colonna, è necessario un indice univoco anziché di una chiave alternativa, vedere [indici](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="1b908-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="1b908-107">In Entity Framework, le chiavi alternative offrono maggiori funzionalità rispetto agli indici univoci perché possono essere usati come destinazione di una chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="1b908-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="1b908-108">Le chiavi alternative vengono in genere introdotti automaticamente quando necessario e non è necessario configurarle manualmente.</span><span class="sxs-lookup"><span data-stu-id="1b908-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="1b908-109">Vedere [convenzioni](#conventions) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="1b908-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="1b908-110">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="1b908-110">Conventions</span></span>

<span data-ttu-id="1b908-111">Per convenzione, una chiave alternativa è stato introdotto per l'utente quando si rileva una proprietà, che non è la chiave primaria, come destinazione di una relazione.</span><span class="sxs-lookup"><span data-stu-id="1b908-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/AlternateKey.cs?highlight=12)] -->
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
            .HasForeignKey(p => p.BlogUrl)
            .HasPrincipalKey(b => b.Url);
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

    public string BlogUrl { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="1b908-112">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="1b908-112">Data Annotations</span></span>

<span data-ttu-id="1b908-113">Le chiavi alternative non possono essere configurate utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="1b908-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="1b908-114">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="1b908-114">Fluent API</span></span>

<span data-ttu-id="1b908-115">Per configurare una singola proprietà di una chiave alternativa, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="1b908-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => c.LicensePlate);
    }
}

class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```

<span data-ttu-id="1b908-116">È inoltre possibile utilizzare l'API Fluent per configurare le proprietà più di una chiave alternativa (nota come una chiave composta alternativa).</span><span class="sxs-lookup"><span data-stu-id="1b908-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```
