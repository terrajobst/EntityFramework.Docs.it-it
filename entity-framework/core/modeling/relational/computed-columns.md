---
title: Colonne calcolate-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: da106c94698a202744d7cd465aa84d0d72802833
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197239"
---
# <a name="computed-columns"></a>Colonne calcolate

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Una colonna calcolata è una colonna il cui valore viene calcolato nel database. Per calcolare il valore di una colonna calcolata, è possibile utilizzare altre colonne della tabella.

## <a name="conventions"></a>Convenzioni

Per convenzione, le colonne calcolate non vengono create nel modello.

## <a name="data-annotations"></a>Annotazioni dei dati

Le colonne calcolate non possono essere configurate con le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

È possibile utilizzare l'API Fluent per specificare che una proprietà deve essere mappata a una colonna calcolata.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/ComputedColumn.cs?highlight=9)] -->
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
