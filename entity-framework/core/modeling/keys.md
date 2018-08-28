---
title: Chiavi (primario) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 9e6946100ebabc6ba57cb792b3672219098b1e21
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994021"
---
# <a name="keys-primary"></a><span data-ttu-id="715d1-102">Chiavi (primarie)</span><span class="sxs-lookup"><span data-stu-id="715d1-102">Keys (primary)</span></span>

<span data-ttu-id="715d1-103">Una chiave funge da identificatore univoco primario per ogni istanza di entità.</span><span class="sxs-lookup"><span data-stu-id="715d1-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="715d1-104">Quando si usa un database relazionale viene eseguito il mapping al concetto di una *chiave primaria*.</span><span class="sxs-lookup"><span data-stu-id="715d1-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="715d1-105">È anche possibile configurare un identificatore univoco che non corrisponde alla chiave primaria (vedere [le chiavi alternative](alternate-keys.md) per altre informazioni).</span><span class="sxs-lookup"><span data-stu-id="715d1-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

## <a name="conventions"></a><span data-ttu-id="715d1-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="715d1-106">Conventions</span></span>

<span data-ttu-id="715d1-107">Per convenzione, una proprietà denominata `Id` o `<type name>Id` saranno configurati come la chiave di un'entità.</span><span class="sxs-lookup"><span data-stu-id="715d1-107">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="715d1-108">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="715d1-108">Data Annotations</span></span>

<span data-ttu-id="715d1-109">È possibile usare le annotazioni dei dati per configurare una singola proprietà per la chiave di un'entità.</span><span class="sxs-lookup"><span data-stu-id="715d1-109">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="715d1-110">API Fluent</span><span class="sxs-lookup"><span data-stu-id="715d1-110">Fluent API</span></span>

<span data-ttu-id="715d1-111">È possibile usare l'API Fluent per configurare una singola proprietà per la chiave di un'entità.</span><span class="sxs-lookup"><span data-stu-id="715d1-111">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

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

<span data-ttu-id="715d1-112">È anche possibile usare l'API Fluent per configurare più proprietà per la chiave di un'entità (nota come una chiave composta).</span><span class="sxs-lookup"><span data-stu-id="715d1-112">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="715d1-113">Le chiavi composte possono essere configurate solo usando l'API Fluent: convenzioni non installerà mai una chiave composta e non è possibile usare le annotazioni dei dati per configurarne uno.</span><span class="sxs-lookup"><span data-stu-id="715d1-113">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

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
