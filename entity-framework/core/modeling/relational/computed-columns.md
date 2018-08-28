---
title: Colonne calcolate - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: b88efdf69e5100e4eff55f3a41925d2d8e7c3178
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993953"
---
# <a name="computed-columns"></a>Colonne calcolate

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Una colonna calcolata è una colonna il cui valore viene calcolato nel database. Una colonna calcolata può utilizzare altre colonne nella tabella per la quale calcolare il relativo valore.

## <a name="conventions"></a>Convenzioni

Per convenzione, le colonne calcolate non vengono create nel modello.

## <a name="data-annotations"></a>Annotazioni dei dati

Le colonne calcolate non possono essere configurate con le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per specificare che una proprietà deve eseguire il mapping a una colonna calcolata.

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
