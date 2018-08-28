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
# <a name="computed-columns"></a><span data-ttu-id="01daf-102">Colonne calcolate</span><span class="sxs-lookup"><span data-stu-id="01daf-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="01daf-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="01daf-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="01daf-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="01daf-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="01daf-105">Una colonna calcolata è una colonna il cui valore viene calcolato nel database.</span><span class="sxs-lookup"><span data-stu-id="01daf-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="01daf-106">Una colonna calcolata può utilizzare altre colonne nella tabella per la quale calcolare il relativo valore.</span><span class="sxs-lookup"><span data-stu-id="01daf-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="01daf-107">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="01daf-107">Conventions</span></span>

<span data-ttu-id="01daf-108">Per convenzione, le colonne calcolate non vengono create nel modello.</span><span class="sxs-lookup"><span data-stu-id="01daf-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="01daf-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="01daf-109">Data Annotations</span></span>

<span data-ttu-id="01daf-110">Le colonne calcolate non possono essere configurate con le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="01daf-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="01daf-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="01daf-111">Fluent API</span></span>

<span data-ttu-id="01daf-112">È possibile usare l'API Fluent per specificare che una proprietà deve eseguire il mapping a una colonna calcolata.</span><span class="sxs-lookup"><span data-stu-id="01daf-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

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
