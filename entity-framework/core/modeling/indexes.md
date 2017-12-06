---
title: Indici - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
ms.technology: entity-framework-core
uid: core/modeling/indexes
ms.openlocfilehash: f57b545d53613cec6887734bf434958ee8fff4d8
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a><span data-ttu-id="b020c-102">Indici</span><span class="sxs-lookup"><span data-stu-id="b020c-102">Indexes</span></span>

<span data-ttu-id="b020c-103">Gli indici sono un concetto comune tra molti archivi di dati.</span><span class="sxs-lookup"><span data-stu-id="b020c-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="b020c-104">Durante la relativa implementazione nell'archivio dati può variare, vengono utilizzati per rendere le ricerche in base a una colonna o set di colonne, più efficiente.</span><span class="sxs-lookup"><span data-stu-id="b020c-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="b020c-105">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="b020c-105">Conventions</span></span>

<span data-ttu-id="b020c-106">Per convenzione, viene creato un indice in ogni proprietà (o set di proprietà) che vengono utilizzate come chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="b020c-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="b020c-107">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="b020c-107">Data Annotations</span></span>

<span data-ttu-id="b020c-108">Gli indici possono non essere creati utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="b020c-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b020c-109">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="b020c-109">Fluent API</span></span>

<span data-ttu-id="b020c-110">È possibile utilizzare l'API Fluent per specificare un indice su una singola proprietà.</span><span class="sxs-lookup"><span data-stu-id="b020c-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="b020c-111">Per impostazione predefinita, gli indici non sono univoche.</span><span class="sxs-lookup"><span data-stu-id="b020c-111">By default, indexes are non-unique.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Index.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="b020c-112">È inoltre possibile specificare che un indice deve essere univoco, vale a dire che non due entità può avere i valori stessi per la proprietà specificata.</span><span class="sxs-lookup"><span data-stu-id="b020c-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="b020c-113">È inoltre possibile specificare un indice su più di una colonna.</span><span class="sxs-lookup"><span data-stu-id="b020c-113">You can also specify an index over more than one column.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .HasIndex(p => new { p.FirstName, p.LastName });
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

> [!TIP]  
> <span data-ttu-id="b020c-114">È presente un solo indice per un set distinto di proprietà.</span><span class="sxs-lookup"><span data-stu-id="b020c-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="b020c-115">Se si utilizza l'API Fluent per configurare un indice in un set di proprietà che dispone già di un indice definito, in base alla convenzione o configurazione precedente, quindi possibile che si desidera modificare la definizione dell'indice.</span><span class="sxs-lookup"><span data-stu-id="b020c-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="b020c-116">Ciò è utile se si desidera configurare ulteriormente un indice creato per convenzione.</span><span class="sxs-lookup"><span data-stu-id="b020c-116">This is useful if you want to further configure an index that was created by convention.</span></span>
