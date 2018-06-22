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
# <a name="computed-columns"></a><span data-ttu-id="3d794-102">Colonne calcolate</span><span class="sxs-lookup"><span data-stu-id="3d794-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="3d794-103">La configurazione di questa sezione è applicabile a database relazionali in generale.</span><span class="sxs-lookup"><span data-stu-id="3d794-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3d794-104">I metodi di estensione qui verranno rese disponibili quando si installa un provider di database relazionali (a causa di condiviso *Microsoft.EntityFrameworkCore.Relational* pacchetto).</span><span class="sxs-lookup"><span data-stu-id="3d794-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3d794-105">Una colonna calcolata è una colonna il cui valore viene calcolato nel database.</span><span class="sxs-lookup"><span data-stu-id="3d794-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="3d794-106">Una colonna calcolata può utilizzare altre colonne nella tabella per calcolare il relativo valore.</span><span class="sxs-lookup"><span data-stu-id="3d794-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="3d794-107">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="3d794-107">Conventions</span></span>

<span data-ttu-id="3d794-108">Per convenzione, le colonne calcolate non vengono create nel modello.</span><span class="sxs-lookup"><span data-stu-id="3d794-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3d794-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="3d794-109">Data Annotations</span></span>

<span data-ttu-id="3d794-110">Colonne calcolate non possono essere configurate con le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="3d794-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="3d794-111">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="3d794-111">Fluent API</span></span>

<span data-ttu-id="3d794-112">Per specificare che una proprietà deve eseguire il mapping a una colonna calcolata, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="3d794-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

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
