---
title: Indici-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: b6f11401b69bd8e8795f6b22e5392ba16fc9ba2e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197254"
---
# <a name="indexes"></a><span data-ttu-id="5692a-102">Indexes</span><span class="sxs-lookup"><span data-stu-id="5692a-102">Indexes</span></span>

<span data-ttu-id="5692a-103">Gli indici sono un concetto comune in molti archivi dati.</span><span class="sxs-lookup"><span data-stu-id="5692a-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="5692a-104">Mentre la loro implementazione nell'archivio dati può variare, vengono usati per rendere più efficienti le ricerche basate su una colonna o un set di colonne.</span><span class="sxs-lookup"><span data-stu-id="5692a-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="5692a-105">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="5692a-105">Conventions</span></span>

<span data-ttu-id="5692a-106">Per convenzione, viene creato un indice in ogni proprietà o set di proprietà utilizzate come chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="5692a-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5692a-107">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="5692a-107">Data Annotations</span></span>

<span data-ttu-id="5692a-108">Non è possibile creare indici utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="5692a-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="5692a-109">API Fluent</span><span class="sxs-lookup"><span data-stu-id="5692a-109">Fluent API</span></span>

<span data-ttu-id="5692a-110">È possibile usare l'API Fluent per specificare un indice in una singola proprietà.</span><span class="sxs-lookup"><span data-stu-id="5692a-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="5692a-111">Per impostazione predefinita, gli indici non sono univoci.</span><span class="sxs-lookup"><span data-stu-id="5692a-111">By default, indexes are non-unique.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Index.cs?highlight=7,8)] -->
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

<span data-ttu-id="5692a-112">È inoltre possibile specificare che un indice deve essere univoco, pertanto due entità non possono avere gli stessi valori per le proprietà specificate.</span><span class="sxs-lookup"><span data-stu-id="5692a-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="5692a-113">È inoltre possibile specificare un indice per più di una colonna.</span><span class="sxs-lookup"><span data-stu-id="5692a-113">You can also specify an index over more than one column.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexComposite.cs?highlight=7,8)] -->
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
> <span data-ttu-id="5692a-114">È disponibile un solo indice per set di proprietà distinti.</span><span class="sxs-lookup"><span data-stu-id="5692a-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="5692a-115">Se si usa l'API Fluent per configurare un indice in un set di proprietà in cui è già definito un indice, per convenzione o per la configurazione precedente, si modificherà la definizione di tale indice.</span><span class="sxs-lookup"><span data-stu-id="5692a-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="5692a-116">Questa opzione è utile se si desidera configurare ulteriormente un indice creato per convenzione.</span><span class="sxs-lookup"><span data-stu-id="5692a-116">This is useful if you want to further configure an index that was created by convention.</span></span>
