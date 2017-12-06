---
title: Tabella di Mapping - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: 73957d9c77e6856bfb65e10e6b373c337101f7d9
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="table-mapping"></a>Mapping di tabella

> [!NOTE]  
> La configurazione di questa sezione è applicabile a database relazionali in generale. I metodi di estensione qui verranno rese disponibili quando si installa un provider di database relazionali (a causa di condiviso *Microsoft.EntityFrameworkCore.Relational* pacchetto).

Mapping della tabella identifica i dati di tabella devono essere una query e salvati nel database.

## <a name="conventions"></a>Convenzioni

Per convenzione, ogni entità verrà configurato per eseguire il mapping a una tabella con lo stesso nome di `DbSet<TEntity>` proprietà che espone l'entità al contesto derivata. Se nessun `DbSet<TEntity>` è incluso per l'entità specificata, viene utilizzato il nome della classe.

## <a name="data-annotations"></a>Annotazioni dei dati

Per configurare la tabella che esegue il mapping a un tipo, è possibile utilizzare le annotazioni dei dati.

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

È inoltre possibile specificare uno schema a cui appartiene la tabella.

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Microsoft Office Fluent API

Per configurare la tabella che esegue il mapping a un tipo, è possibile utilizzare l'API Fluent.

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

È inoltre possibile specificare uno schema a cui appartiene la tabella.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
