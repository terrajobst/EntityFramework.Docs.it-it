---
title: Schema - Core EF predefinito
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
ms.technology: entity-framework-core
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 26106deb2d4e35ecf33e97790a83f9af77991aed
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="default-schema"></a>Schema predefinito

> [!NOTE]  
> La configurazione di questa sezione è applicabile a database relazionali in generale. I metodi di estensione qui verranno rese disponibili quando si installa un provider di database relazionali (a causa di condiviso *Microsoft.EntityFrameworkCore.Relational* pacchetto).

Lo schema predefinito è lo schema del database che verranno creati gli oggetti in se uno schema non è configurato in modo esplicito per l'oggetto.

## <a name="conventions"></a>Convenzioni

Per convenzione, il provider del database scegliere lo schema predefinito più appropriato. Ad esempio, Microsoft SQL Server utilizzerà il `dbo` SQLite e lo schema non utilizzerà uno schema (poiché gli schemi non sono supportati in SQLite).

## <a name="data-annotations"></a>Annotazioni dei dati

Non è possibile impostare lo schema predefinito utilizzando le annotazioni dei dati.

## <a name="fluent-api"></a>Microsoft Office Fluent API

È possibile utilizzare l'API Fluent per specificare uno schema predefinito.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultSchema.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasDefaultSchema("blogging");
    }
}
```
