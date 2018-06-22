---
title: Chiavi (primaria) - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
ms.technology: entity-framework-core
uid: core/modeling/keys
ms.openlocfilehash: f3bf3c7f2a28e065b350fe000a5164406cd5ca08
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052571"
---
# <a name="keys-primary"></a><span data-ttu-id="a6feb-102">Chiavi (primaria)</span><span class="sxs-lookup"><span data-stu-id="a6feb-102">Keys (primary)</span></span>

<span data-ttu-id="a6feb-103">Una chiave funge da identificatore univoco primario per ogni istanza di entità.</span><span class="sxs-lookup"><span data-stu-id="a6feb-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="a6feb-104">Quando si utilizza un database relazionale esegue il mapping al concetto di un *chiave primaria*.</span><span class="sxs-lookup"><span data-stu-id="a6feb-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="a6feb-105">È inoltre possibile configurare un identificatore univoco che non è la chiave primaria (vedere [chiavi alternative](alternate-keys.md) per altre informazioni).</span><span class="sxs-lookup"><span data-stu-id="a6feb-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

## <a name="conventions"></a><span data-ttu-id="a6feb-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="a6feb-106">Conventions</span></span>

<span data-ttu-id="a6feb-107">Per convenzione, una proprietà denominata `Id` o `<type name>Id` saranno configurati come chiave di un'entità.</span><span class="sxs-lookup"><span data-stu-id="a6feb-107">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="a6feb-108">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="a6feb-108">Data Annotations</span></span>

<span data-ttu-id="a6feb-109">Per configurare una singola proprietà da utilizzare come chiave di un'entità, è possibile utilizzare le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="a6feb-109">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=3,4)] -->
``` csharp
class Car
{
    [Key]
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="a6feb-110">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="a6feb-110">Fluent API</span></span>

<span data-ttu-id="a6feb-111">Per configurare una singola proprietà da utilizzare come chiave di un'entità, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="a6feb-111">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => c.LicensePlate);
    }
}

class Car
{
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<span data-ttu-id="a6feb-112">È inoltre possibile utilizzare l'API Fluent per configurare più proprietà da utilizzare come chiave di un'entità (nota come una chiave composta).</span><span class="sxs-lookup"><span data-stu-id="a6feb-112">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="a6feb-113">Le chiavi composte possono essere configurate solo tramite l'API Fluent: convenzioni non installerà mai una chiave composta e non è possibile utilizzare le annotazioni dei dati di configurare uno.</span><span class="sxs-lookup"><span data-stu-id="a6feb-113">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public string State { get; set; }
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```
