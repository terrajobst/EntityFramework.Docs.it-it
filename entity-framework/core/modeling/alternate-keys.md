---
title: Chiavi alternative - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: b26d8bc1630af9e811d9c4e7da850a618bc8042e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996971"
---
# <a name="alternate-keys"></a><span data-ttu-id="6963b-102">Chiavi alternative</span><span class="sxs-lookup"><span data-stu-id="6963b-102">Alternate Keys</span></span>

<span data-ttu-id="6963b-103">Una chiave alternativa viene utilizzato come un identificatore univoco alternativo per ogni istanza di entità oltre alla chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="6963b-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="6963b-104">Le chiavi alternative utilizzabili come destinazione di una relazione.</span><span class="sxs-lookup"><span data-stu-id="6963b-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="6963b-105">Quando si usa un database relazionale viene eseguito il mapping al concetto di un indice o vincolo unique su una o più vincoli di chiave esterna che fanno riferimento le colonne e le colonne di chiavi alternative.</span><span class="sxs-lookup"><span data-stu-id="6963b-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="6963b-106">Se si desidera semplicemente garantire l'univocità di una colonna, quindi preferibile un indice univoco anziché una chiave alternativa, vedere [indici](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="6963b-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="6963b-107">In Entity Framework, le chiavi alternative offrono maggiori funzionalità rispetto agli indici univoci perché possono essere usati come destinazione di una chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="6963b-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="6963b-108">Le chiavi alternative vengono in genere introdotte automaticamente all'occorrenza e non è necessario configurarle manualmente.</span><span class="sxs-lookup"><span data-stu-id="6963b-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="6963b-109">Visualizzare [convenzioni](#conventions) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="6963b-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="6963b-110">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="6963b-110">Conventions</span></span>

<span data-ttu-id="6963b-111">Per convenzione, una chiave alternativa viene introdotto per l'utente quando si identifica una proprietà, non la chiave primaria, come la destinazione di una relazione.</span><span class="sxs-lookup"><span data-stu-id="6963b-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="6963b-112">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="6963b-112">Data Annotations</span></span>

<span data-ttu-id="6963b-113">Le chiavi alternative non possono essere configurate utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="6963b-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="6963b-114">API Fluent</span><span class="sxs-lookup"><span data-stu-id="6963b-114">Fluent API</span></span>

<span data-ttu-id="6963b-115">Per configurare una singola proprietà da una chiave alternativa, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="6963b-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

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

<span data-ttu-id="6963b-116">È anche possibile usare l'API Fluent per configurare più proprietà da una chiave alternativa (nota come una chiave composta alternativa).</span><span class="sxs-lookup"><span data-stu-id="6963b-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

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
