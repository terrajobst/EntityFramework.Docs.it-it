---
title: Chiavi alternative-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: 87df5d174a1db12fb3ab763ac76c3b863a83087e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197470"
---
# <a name="alternate-keys"></a><span data-ttu-id="5662d-102">Chiavi alternative</span><span class="sxs-lookup"><span data-stu-id="5662d-102">Alternate Keys</span></span>

<span data-ttu-id="5662d-103">Una chiave alternativa funge da identificatore univoco alternativo per ogni istanza di entità oltre alla chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="5662d-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="5662d-104">È possibile utilizzare chiavi alternative come destinazione di una relazione.</span><span class="sxs-lookup"><span data-stu-id="5662d-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="5662d-105">Quando si utilizza un database relazionale, viene eseguito il mapping al concetto di indice/vincolo univoco nelle colonne chiave alternative e di uno o più vincoli di chiave esterna che fanno riferimento alle colonne.</span><span class="sxs-lookup"><span data-stu-id="5662d-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="5662d-106">Se si vuole semplicemente applicare l'univocità di una colonna, è necessario un indice univoco anziché una chiave alternativa, vedere [indici](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="5662d-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="5662d-107">In EF i tasti alternativi forniscono funzionalità maggiori rispetto agli indici univoci, perché possono essere usati come destinazione di una chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="5662d-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="5662d-108">Quando necessario, vengono in genere introdotte chiavi alternative e non è necessario configurarle manualmente.</span><span class="sxs-lookup"><span data-stu-id="5662d-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="5662d-109">Per ulteriori informazioni, vedere [convenzioni](#conventions) .</span><span class="sxs-lookup"><span data-stu-id="5662d-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="5662d-110">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="5662d-110">Conventions</span></span>

<span data-ttu-id="5662d-111">Per convenzione, viene introdotta una chiave alternativa quando si identifica una proprietà, che non è la chiave primaria, come destinazione di una relazione.</span><span class="sxs-lookup"><span data-stu-id="5662d-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/AlternateKey.cs?highlight=12)] -->
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

## <a name="data-annotations"></a><span data-ttu-id="5662d-112">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="5662d-112">Data Annotations</span></span>

<span data-ttu-id="5662d-113">Non è possibile configurare chiavi alternative usando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="5662d-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="5662d-114">API Fluent</span><span class="sxs-lookup"><span data-stu-id="5662d-114">Fluent API</span></span>

<span data-ttu-id="5662d-115">È possibile usare l'API Fluent per configurare una singola proprietà in modo che sia una chiave alternativa.</span><span class="sxs-lookup"><span data-stu-id="5662d-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?highlight=7,8)] -->
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

<span data-ttu-id="5662d-116">È anche possibile usare l'API Fluent per configurare più proprietà in modo che siano una chiave alternativa (nota come chiave alternativa composita).</span><span class="sxs-lookup"><span data-stu-id="5662d-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?highlight=7,8)] -->
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
