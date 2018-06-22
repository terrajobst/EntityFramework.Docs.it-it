---
title: Sequenza - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
ms.technology: entity-framework-core
uid: core/modeling/relational/sequences
ms.openlocfilehash: 98a40aeecbec0fd9fb9cc108d6b5f98178dea403
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052711"
---
# <a name="sequences"></a><span data-ttu-id="cea22-102">Sequenze</span><span class="sxs-lookup"><span data-stu-id="cea22-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="cea22-103">La configurazione di questa sezione è applicabile a database relazionali in generale.</span><span class="sxs-lookup"><span data-stu-id="cea22-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="cea22-104">I metodi di estensione qui verranno rese disponibili quando si installa un provider di database relazionali (a causa di condiviso *Microsoft.EntityFrameworkCore.Relational* pacchetto).</span><span class="sxs-lookup"><span data-stu-id="cea22-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="cea22-105">Una sequenza genera un valori numerici in sequenza nel database.</span><span class="sxs-lookup"><span data-stu-id="cea22-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="cea22-106">Le sequenze non sono associate a una tabella specifica.</span><span class="sxs-lookup"><span data-stu-id="cea22-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="cea22-107">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="cea22-107">Conventions</span></span>

<span data-ttu-id="cea22-108">Per convenzione, le sequenze non verranno introdotti per il modello.</span><span class="sxs-lookup"><span data-stu-id="cea22-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="cea22-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="cea22-109">Data Annotations</span></span>

<span data-ttu-id="cea22-110">Non è possibile configurare una sequenza utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="cea22-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="cea22-111">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="cea22-111">Fluent API</span></span>

<span data-ttu-id="cea22-112">Per creare una sequenza nel modello, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="cea22-112">You can use the Fluent API to create a sequence in the model.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Sequence.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="cea22-113">È inoltre possibile configurare altri aspetti della sequenza, ad esempio il relativo schema, il valore iniziale e incremento.</span><span class="sxs-lookup"><span data-stu-id="cea22-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceConfigured.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);
    }
}
```

<span data-ttu-id="cea22-114">Una volta che viene introdotta una sequenza, è possibile utilizzare, per generare i valori per le proprietà nel modello.</span><span class="sxs-lookup"><span data-stu-id="cea22-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="cea22-115">Ad esempio, è possibile utilizzare [valori predefiniti](default-values.md) per inserire il valore successivo dalla sequenza.</span><span class="sxs-lookup"><span data-stu-id="cea22-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceUsed.cs?highlight=11,12,13)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);

        modelBuilder.Entity<Order>()
            .Property(o => o.OrderNo)
            .HasDefaultValueSql("NEXT VALUE FOR shared.OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```
