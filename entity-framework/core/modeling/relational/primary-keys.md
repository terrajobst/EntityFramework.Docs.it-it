---
title: Chiavi primarie - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
ms.technology: entity-framework-core
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: fcb1871149c0f20a2576864028b4171904de1982
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052721"
---
# <a name="primary-keys"></a>Chiavi primarie

> [!NOTE]  
> La configurazione di questa sezione è applicabile a database relazionali in generale. I metodi di estensione qui verranno rese disponibili quando si installa un provider di database relazionali (a causa di condiviso *Microsoft.EntityFrameworkCore.Relational* pacchetto).

Un vincolo di chiave primaria è stato introdotto per la chiave di ogni tipo di entità.

## <a name="conventions"></a>Convenzioni

Per convenzione, la chiave primaria nel database verrà denominata `PK_<type name>`.

## <a name="data-annotations"></a>Annotazioni dei dati

Nessun aspetti specifici del database relazionale di una chiave primaria possono essere configurati tramite le annotazioni dei dati.

## <a name="fluent-api"></a>Microsoft Office Fluent API

È possibile utilizzare l'API Fluent per configurare il nome del vincolo di chiave primaria nel database.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/KeyName.cs?highlight=9)] -->
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
