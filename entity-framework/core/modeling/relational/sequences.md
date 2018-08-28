---
title: Sequenze - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: eb9d9896966af0ad6b778047a1ed6af7358e8eb2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994516"
---
# <a name="sequences"></a><span data-ttu-id="ae2a3-102">Sequenze</span><span class="sxs-lookup"><span data-stu-id="ae2a3-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="ae2a3-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="ae2a3-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="ae2a3-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="ae2a3-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="ae2a3-105">Una sequenza che genera l'errore un valori numerici in sequenza nel database.</span><span class="sxs-lookup"><span data-stu-id="ae2a3-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="ae2a3-106">Le sequenze non sono associate a una tabella specifica.</span><span class="sxs-lookup"><span data-stu-id="ae2a3-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="ae2a3-107">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="ae2a3-107">Conventions</span></span>

<span data-ttu-id="ae2a3-108">Per convenzione, le sequenze non vengano inserite al modello.</span><span class="sxs-lookup"><span data-stu-id="ae2a3-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="ae2a3-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="ae2a3-109">Data Annotations</span></span>

<span data-ttu-id="ae2a3-110">Non è possibile configurare una sequenza utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="ae2a3-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ae2a3-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="ae2a3-111">Fluent API</span></span>

<span data-ttu-id="ae2a3-112">È possibile usare l'API Fluent per creare una sequenza nel modello.</span><span class="sxs-lookup"><span data-stu-id="ae2a3-112">You can use the Fluent API to create a sequence in the model.</span></span>

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

<span data-ttu-id="ae2a3-113">È anche possibile configurare altri aspetti della sequenza, quali lo schema, il valore iniziale e incremento.</span><span class="sxs-lookup"><span data-stu-id="ae2a3-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

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

<span data-ttu-id="ae2a3-114">Una volta che viene introdotta una sequenza, è possibile usarlo per generare valori per le proprietà nel modello.</span><span class="sxs-lookup"><span data-stu-id="ae2a3-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="ae2a3-115">Ad esempio, è possibile usare [valori predefiniti](default-values.md) per inserire il valore successivo dalla sequenza.</span><span class="sxs-lookup"><span data-stu-id="ae2a3-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

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
