---
title: "Ereditarietà (Database relazionale) - Core a Entity Framework"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 22eed0002b5903d3cfd18a7e4af0fcd2d46a5c4c
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2018
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="b167f-102">Ereditarietà (Database relazionale)</span><span class="sxs-lookup"><span data-stu-id="b167f-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="b167f-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="b167f-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="b167f-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="b167f-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="b167f-105">Ereditarietà nel modello di Entity Framework viene utilizzato per controllare la modalità di rappresentazione di ereditarietà nelle classi di entità nel database.</span><span class="sxs-lookup"><span data-stu-id="b167f-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="b167f-106">Attualmente, solo il criterio di tabella per gerarchia (Implementerà) viene implementato in EF Core.</span><span class="sxs-lookup"><span data-stu-id="b167f-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="b167f-107">Altri modelli comuni come tabella per tipo (tabella per tipo) e -per-concreto-tipo di tabella (TPC) non sono ancora disponibili.</span><span class="sxs-lookup"><span data-stu-id="b167f-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="b167f-108">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="b167f-108">Conventions</span></span>

<span data-ttu-id="b167f-109">Per convenzione, ereditarietà verrà mappato usando il modello di tabella per ogni-gerarchia (TPH).</span><span class="sxs-lookup"><span data-stu-id="b167f-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="b167f-110">Implementerà Usa una singola tabella per archiviare i dati per tutti i tipi nella gerarchia.</span><span class="sxs-lookup"><span data-stu-id="b167f-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="b167f-111">Una colonna discriminatore viene utilizzata per identificare il tipo di ogni riga rappresenta.</span><span class="sxs-lookup"><span data-stu-id="b167f-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="b167f-112">Core EF installerà ereditarietà solo se due o più tipi ereditati sono inclusi in modo esplicito nel modello (vedere [ereditarietà](../inheritance.md) per altri dettagli).</span><span class="sxs-lookup"><span data-stu-id="b167f-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="b167f-113">Di seguito è riportato un esempio che illustra uno scenario semplice di ereditarietà e i dati archiviati in una tabella di database relazionale utilizzando il modello della tabella per gerarchia.</span><span class="sxs-lookup"><span data-stu-id="b167f-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="b167f-114">Il *discriminatore* colonna identifica il tipo di *Blog* viene archiviato in ogni riga.</span><span class="sxs-lookup"><span data-stu-id="b167f-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<RssBlog> RssBlogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

![immagine](_static/inheritance-tph-data.png)

## <a name="data-annotations"></a><span data-ttu-id="b167f-116">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="b167f-116">Data Annotations</span></span>

<span data-ttu-id="b167f-117">Per configurare l'ereditarietà, è possibile utilizzare le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="b167f-117">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b167f-118">API Fluent</span><span class="sxs-lookup"><span data-stu-id="b167f-118">Fluent API</span></span>

<span data-ttu-id="b167f-119">Per configurare il nome e il tipo di colonna discriminatore e i valori utilizzati per identificare ogni tipo nella gerarchia, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="b167f-119">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("blog_type")
            .HasValue<Blog>("blog_base")
            .HasValue<RssBlog>("blog_rss");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="b167f-120">Configurazione proprietà discriminator</span><span class="sxs-lookup"><span data-stu-id="b167f-120">Configuring the discriminator property</span></span>

<span data-ttu-id="b167f-121">Negli esempi precedenti, viene creato il discriminatore come un [nascondere proprietà](xref:core/modeling/shadow-properties) su entità di base della gerarchia.</span><span class="sxs-lookup"><span data-stu-id="b167f-121">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="b167f-122">Poiché si tratta di una proprietà nel modello, può essere configurato come altre proprietà.</span><span class="sxs-lookup"><span data-stu-id="b167f-122">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="b167f-123">Ad esempio, per impostare la lunghezza massima, quando viene utilizzato il valore predefinito, del discriminatore per convenzione:</span><span class="sxs-lookup"><span data-stu-id="b167f-123">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="b167f-124">Il discriminatore può anche essere mappato a una proprietà CLR effettiva nell'entità.</span><span class="sxs-lookup"><span data-stu-id="b167f-124">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="b167f-125">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b167f-125">For example:</span></span>
```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

<span data-ttu-id="b167f-126">Combinando queste due operazioni è possibile eseguire il mapping del discriminatore per una proprietà reale sia configurarlo:</span><span class="sxs-lookup"><span data-stu-id="b167f-126">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
