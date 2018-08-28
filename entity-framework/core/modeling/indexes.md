---
title: Indici - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 87fe893243377e3ab83d419ae9bedf813ca50c3f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995480"
---
# <a name="indexes"></a><span data-ttu-id="562f3-102">Indici</span><span class="sxs-lookup"><span data-stu-id="562f3-102">Indexes</span></span>

<span data-ttu-id="562f3-103">Gli indici sono un concetto comune ampiamente tra molti archivi dati.</span><span class="sxs-lookup"><span data-stu-id="562f3-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="562f3-104">Mentre la relativa implementazione nell'archivio dati può variare, vengono usate per effettuare ricerche basate su una colonna o set di colonne, più efficiente.</span><span class="sxs-lookup"><span data-stu-id="562f3-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="562f3-105">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="562f3-105">Conventions</span></span>

<span data-ttu-id="562f3-106">Per convenzione, viene creato un indice in ogni proprietà (o set di proprietà) che vengono utilizzate come chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="562f3-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="562f3-107">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="562f3-107">Data Annotations</span></span>

<span data-ttu-id="562f3-108">Gli indici non è possibile creare tramite le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="562f3-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="562f3-109">API Fluent</span><span class="sxs-lookup"><span data-stu-id="562f3-109">Fluent API</span></span>

<span data-ttu-id="562f3-110">È possibile usare l'API Fluent per specificare un indice su una singola proprietà.</span><span class="sxs-lookup"><span data-stu-id="562f3-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="562f3-111">Per impostazione predefinita, gli indici non sono univoche.</span><span class="sxs-lookup"><span data-stu-id="562f3-111">By default, indexes are non-unique.</span></span>

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

<span data-ttu-id="562f3-112">È anche possibile specificare che un indice deve essere univoco, vale a dire che nessun due entità può avere i valori stessi per la proprietà specificata.</span><span class="sxs-lookup"><span data-stu-id="562f3-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="562f3-113">È anche possibile specificare un indice su più colonne.</span><span class="sxs-lookup"><span data-stu-id="562f3-113">You can also specify an index over more than one column.</span></span>

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
> <span data-ttu-id="562f3-114">È presente un solo indice per ogni serie distinta di proprietà.</span><span class="sxs-lookup"><span data-stu-id="562f3-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="562f3-115">Se si usa l'API Fluent per configurare un indice in un set di proprietà che dispone già di un indice definito, in base alla convenzione o configurazione precedente, quindi possibile che si desidera modificare la definizione di tale indice.</span><span class="sxs-lookup"><span data-stu-id="562f3-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="562f3-116">Ciò è utile se si desidera un'ulteriore configurazione di un indice creato per convenzione.</span><span class="sxs-lookup"><span data-stu-id="562f3-116">This is useful if you want to further configure an index that was created by convention.</span></span>
