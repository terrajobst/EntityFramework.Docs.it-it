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
# <a name="indexes"></a>Indici

Gli indici sono un concetto comune ampiamente tra molti archivi dati. Mentre la relativa implementazione nell'archivio dati può variare, vengono usate per effettuare ricerche basate su una colonna o set di colonne, più efficiente.

## <a name="conventions"></a>Convenzioni

Per convenzione, viene creato un indice in ogni proprietà (o set di proprietà) che vengono utilizzate come chiave esterna.

## <a name="data-annotations"></a>Annotazioni dei dati

Gli indici non è possibile creare tramite le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per specificare un indice su una singola proprietà. Per impostazione predefinita, gli indici non sono univoche.

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

È anche possibile specificare che un indice deve essere univoco, vale a dire che nessun due entità può avere i valori stessi per la proprietà specificata.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

È anche possibile specificare un indice su più colonne.

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
> È presente un solo indice per ogni serie distinta di proprietà. Se si usa l'API Fluent per configurare un indice in un set di proprietà che dispone già di un indice definito, in base alla convenzione o configurazione precedente, quindi possibile che si desidera modificare la definizione di tale indice. Ciò è utile se si desidera un'ulteriore configurazione di un indice creato per convenzione.
