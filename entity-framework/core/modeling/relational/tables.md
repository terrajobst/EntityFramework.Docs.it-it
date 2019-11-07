---
title: Mapping tabella-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 474c49aca4c65cd5d58b184b1f3c2d30e7abff84
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656096"
---
# <a name="table-mapping"></a>Mapping di tabella

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Il mapping delle tabelle identifica i dati della tabella da cui devono essere eseguite query e salvati nel database.

## <a name="conventions"></a>Convenzioni

Per convenzione, ogni entità viene configurata in modo da eseguire il mapping a una tabella con lo stesso nome della proprietà `DbSet<TEntity>` che espone l'entità nel contesto derivato. Se non è incluso alcun `DbSet<TEntity>` per l'entità specificata, viene usato il nome della classe.

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile utilizzare le annotazioni dei dati per configurare la tabella a cui viene eseguito il mapping di un tipo.

``` csharp
using System.ComponentModel.DataAnnotations.Schema;

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

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare la tabella a cui viene eseguito il mapping di un tipo.

``` csharp
using Microsoft.EntityFrameworkCore;

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

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/TableAndSchema.cs?name=Table&highlight=2)]
