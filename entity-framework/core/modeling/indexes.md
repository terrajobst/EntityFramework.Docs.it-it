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
ms.locfileid: "26054885"
---
# <a name="indexes"></a>Indici

Gli indici sono un concetto comune tra molti archivi di dati. Durante la relativa implementazione nell'archivio dati può variare, vengono utilizzati per rendere le ricerche in base a una colonna o set di colonne, più efficiente.

## <a name="conventions"></a>Convenzioni

Per convenzione, viene creato un indice in ogni proprietà (o set di proprietà) che vengono utilizzate come chiave esterna.

## <a name="data-annotations"></a>Annotazioni dei dati

Gli indici possono non essere creati utilizzando le annotazioni dei dati.

## <a name="fluent-api"></a>Microsoft Office Fluent API

È possibile utilizzare l'API Fluent per specificare un indice su una singola proprietà. Per impostazione predefinita, gli indici non sono univoche.

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

È inoltre possibile specificare che un indice deve essere univoco, vale a dire che non due entità può avere i valori stessi per la proprietà specificata.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

È inoltre possibile specificare un indice su più di una colonna.

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
> È presente un solo indice per un set distinto di proprietà. Se si utilizza l'API Fluent per configurare un indice in un set di proprietà che dispone già di un indice definito, in base alla convenzione o configurazione precedente, quindi possibile che si desidera modificare la definizione dell'indice. Ciò è utile se si desidera configurare ulteriormente un indice creato per convenzione.
