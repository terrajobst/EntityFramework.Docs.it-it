---
title: Schema predefinito-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: ae903ed7200859430aecc55073651236759bc6ce
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197126"
---
# <a name="default-schema"></a>Schema predefinito

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Lo schema predefinito è lo schema del database in cui verranno creati gli oggetti se uno schema non è configurato in modo esplicito per tale oggetto.

## <a name="conventions"></a>Convenzioni

Per convenzione, il provider di database sceglierà lo schema predefinito più appropriato. Ad esempio, Microsoft SQL Server utilizzerà lo `dbo` schema e SQLite non utilizzerà uno schema (poiché gli schemi non sono supportati in SQLite).

## <a name="data-annotations"></a>Annotazioni dei dati

Non è possibile impostare lo schema predefinito utilizzando le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

Per specificare uno schema predefinito, è possibile usare l'API Fluent.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultSchema.cs?highlight=7)] -->
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
