---
title: Schema predefinito - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 800551bbadd0a9e8b5eb7070a8ccf6ed2407e3d2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995366"
---
# <a name="default-schema"></a>Schema predefinito

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Lo schema predefinito è lo schema del database che verranno creati gli oggetti in se uno schema non è configurato in modo esplicito per quell'oggetto.

## <a name="conventions"></a>Convenzioni

Per convenzione, il provider di database sceglierà lo schema predefinito più appropriato. Ad esempio, Microsoft SQL Server userà il `dbo` SQLite e lo schema non userà uno schema (poiché gli schemi non sono supportati in SQLite).

## <a name="data-annotations"></a>Annotazioni dei dati

Non è possibile impostare lo schema predefinito tramite le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per specificare uno schema predefinito.

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
