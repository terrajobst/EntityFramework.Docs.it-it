---
title: Chiavi (primario) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 6272e323b83ccab2ed060a2ebbde1d1e8e353d66
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319166"
---
# <a name="keys-primary"></a>Chiavi (primarie)

Una chiave funge da identificatore univoco primario per ogni istanza di entità. Quando si usa un database relazionale viene eseguito il mapping al concetto di una *chiave primaria*. È anche possibile configurare un identificatore univoco che non corrisponde alla chiave primaria (vedere [le chiavi alternative](alternate-keys.md) per altre informazioni). 

Uno dei metodi descritti di seguito è utilizzabile per il programma di installazione/creare una chiave primaria.

## <a name="conventions"></a>Convenzioni

Per convenzione, una proprietà denominata `Id` o `<type name>Id` saranno configurati come la chiave di un'entità.

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

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile usare le annotazioni dei dati per configurare una singola proprietà per la chiave di un'entità.

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

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare una singola proprietà per la chiave di un'entità.

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

È anche possibile usare l'API Fluent per configurare più proprietà per la chiave di un'entità (nota come una chiave composta). Le chiavi composte possono essere configurate solo usando l'API Fluent: convenzioni non installerà mai una chiave composta e non è possibile usare le annotazioni dei dati per configurarne uno.

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
