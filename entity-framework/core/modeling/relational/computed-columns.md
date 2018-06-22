---
title: Colonne calcolate - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
ms.technology: entity-framework-core
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 95312504286bd34cc666b5a21273835c4b35d379
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052481"
---
# <a name="computed-columns"></a>Colonne calcolate

> [!NOTE]  
> La configurazione di questa sezione è applicabile a database relazionali in generale. I metodi di estensione qui verranno rese disponibili quando si installa un provider di database relazionali (a causa di condiviso *Microsoft.EntityFrameworkCore.Relational* pacchetto).

Una colonna calcolata è una colonna il cui valore viene calcolato nel database. Una colonna calcolata può utilizzare altre colonne nella tabella per calcolare il relativo valore.

## <a name="conventions"></a>Convenzioni

Per convenzione, le colonne calcolate non vengono create nel modello.

## <a name="data-annotations"></a>Annotazioni dei dati

Colonne calcolate non possono essere configurate con le annotazioni dei dati.

## <a name="fluent-api"></a>Microsoft Office Fluent API

Per specificare che una proprietà deve eseguire il mapping a una colonna calcolata, è possibile utilizzare l'API Fluent.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/ComputedColumn.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .Property(p => p.DisplayName)
            .HasComputedColumnSql("[LastName] + ', ' + [FirstName]");
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string DisplayName { get; set; }
}
```
