---
title: Tra cui & esclusione delle proprietà - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
ms.technology: entity-framework-core
uid: core/modeling/included-properties
ms.openlocfilehash: a6eaea4319f6a4d30c223265bf75a88731a38443
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052491"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="d8afb-102">Tra cui & esclusione delle proprietà</span><span class="sxs-lookup"><span data-stu-id="d8afb-102">Including & Excluding Properties</span></span>

<span data-ttu-id="d8afb-103">Tra cui una proprietà nel modello indica che EF i metadati relativi a tale proprietà e verrà eseguito un tentativo di lettura e scrittura di valori dal/al database.</span><span class="sxs-lookup"><span data-stu-id="d8afb-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="d8afb-104">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="d8afb-104">Conventions</span></span>

<span data-ttu-id="d8afb-105">Per convenzione, le proprietà pubbliche con un metodo Get e set verranno inclusi nel modello.</span><span class="sxs-lookup"><span data-stu-id="d8afb-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d8afb-106">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="d8afb-106">Data Annotations</span></span>

<span data-ttu-id="d8afb-107">È possibile utilizzare le annotazioni dei dati per escludere una proprietà dal modello.</span><span class="sxs-lookup"><span data-stu-id="d8afb-107">You can use Data Annotations to exclude a property from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=6)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    [NotMapped]
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="d8afb-108">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="d8afb-108">Fluent API</span></span>

<span data-ttu-id="d8afb-109">È possibile utilizzare l'API Fluent per escludere una proprietà dal modello.</span><span class="sxs-lookup"><span data-stu-id="d8afb-109">You can use the Fluent API to exclude a property from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Ignore(b => b.LoadedFromDatabase);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public DateTime LoadedFromDatabase { get; set; }
}
```
