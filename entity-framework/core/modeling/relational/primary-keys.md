---
title: Chiavi primarie-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: bdb31964d717c64bddc28e7f1ce55b261e285a9b
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196944"
---
# <a name="primary-keys"></a>Chiavi primarie

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Per la chiave di ogni tipo di entità è stato introdotto un vincolo PRIMARY KEY.

## <a name="conventions"></a>Convenzioni

Per convenzione, la chiave primaria nel database verrà denominata `PK_<type name>`.

## <a name="data-annotations"></a>Annotazioni dei dati

Non è possibile configurare aspetti specifici di un database relazionale di una chiave primaria usando le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

È possibile utilizzare l'API Fluent per configurare il nome del vincolo PRIMARY KEY nel database.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/KeyName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasKey(b => b.BlogId)
            .HasName("PrimaryKey_BlogId");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
