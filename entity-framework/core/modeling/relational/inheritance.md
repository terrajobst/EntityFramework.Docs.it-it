---
title: Ereditarietà (database relazionale)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: c660107619470a726fe13ad8eee2850749e6dcd9
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812088"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="8fcb6-102">Ereditarietà (database relazionale)</span><span class="sxs-lookup"><span data-stu-id="8fcb6-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="8fcb6-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="8fcb6-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="8fcb6-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="8fcb6-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="8fcb6-105">L'ereditarietà nel modello EF viene utilizzata per controllare il modo in cui l'ereditarietà nelle classi di entità viene rappresentata nel database.</span><span class="sxs-lookup"><span data-stu-id="8fcb6-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="8fcb6-106">Attualmente, in EF Core viene implementato solo il modello tabella per gerarchia (TPH).</span><span class="sxs-lookup"><span data-stu-id="8fcb6-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="8fcb6-107">Altri modelli comuni, ad esempio tabella per tipo (TPT) e tabella per tipo concreto (TPC), non sono ancora disponibili.</span><span class="sxs-lookup"><span data-stu-id="8fcb6-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="8fcb6-108">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="8fcb6-108">Conventions</span></span>

<span data-ttu-id="8fcb6-109">Per convenzione, verrà eseguito il mapping dell'ereditarietà utilizzando il modello tabella per gerarchia (TPH).</span><span class="sxs-lookup"><span data-stu-id="8fcb6-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="8fcb6-110">TPH usa una singola tabella per archiviare i dati per tutti i tipi nella gerarchia.</span><span class="sxs-lookup"><span data-stu-id="8fcb6-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="8fcb6-111">Una colonna discriminatore viene utilizzata per identificare il tipo rappresentato da ogni riga.</span><span class="sxs-lookup"><span data-stu-id="8fcb6-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="8fcb6-112">EF Core configurerà l'ereditarietà solo se due o più tipi ereditati vengono inclusi in modo esplicito nel modello. per ulteriori informazioni, vedere [ereditarietà](../inheritance.md) .</span><span class="sxs-lookup"><span data-stu-id="8fcb6-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="8fcb6-113">Di seguito è riportato un esempio che mostra uno scenario di ereditarietà semplice e i dati archiviati in una tabella di database relazionale usando il modello TPH.</span><span class="sxs-lookup"><span data-stu-id="8fcb6-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="8fcb6-114">La colonna *discriminatore* identifica il tipo di *Blog* archiviato in ogni riga.</span><span class="sxs-lookup"><span data-stu-id="8fcb6-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/InheritanceDbSets.cs)] -->
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

>[!NOTE]
> <span data-ttu-id="8fcb6-116">Le colonne del database vengono rese automaticamente Nullable quando necessario quando si usa il mapping di TPH.</span><span class="sxs-lookup"><span data-stu-id="8fcb6-116">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="8fcb6-117">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="8fcb6-117">Data Annotations</span></span>

<span data-ttu-id="8fcb6-118">Non è possibile utilizzare le annotazioni dei dati per configurare l'ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="8fcb6-118">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="8fcb6-119">API Fluent</span><span class="sxs-lookup"><span data-stu-id="8fcb6-119">Fluent API</span></span>

<span data-ttu-id="8fcb6-120">È possibile utilizzare l'API Fluent per configurare il nome e il tipo della colonna discriminatore e i valori utilizzati per identificare ogni tipo nella gerarchia.</span><span class="sxs-lookup"><span data-stu-id="8fcb6-120">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
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

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="8fcb6-121">Configurazione della proprietà Discriminator</span><span class="sxs-lookup"><span data-stu-id="8fcb6-121">Configuring the discriminator property</span></span>

<span data-ttu-id="8fcb6-122">Negli esempi precedenti, il discriminatore viene creato come [proprietà shadow](xref:core/modeling/shadow-properties) nell'entità di base della gerarchia.</span><span class="sxs-lookup"><span data-stu-id="8fcb6-122">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="8fcb6-123">Poiché si tratta di una proprietà nel modello, può essere configurata esattamente come le altre proprietà.</span><span class="sxs-lookup"><span data-stu-id="8fcb6-123">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="8fcb6-124">Ad esempio, per impostare la lunghezza massima quando viene usato il discriminatore predefinito per convenzione:</span><span class="sxs-lookup"><span data-stu-id="8fcb6-124">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="8fcb6-125">È anche possibile eseguire il mapping del discriminatore a una proprietà CLR effettiva nell'entità.</span><span class="sxs-lookup"><span data-stu-id="8fcb6-125">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="8fcb6-126">Esempio:</span><span class="sxs-lookup"><span data-stu-id="8fcb6-126">For example:</span></span>

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

<span data-ttu-id="8fcb6-127">Combinando questi due elementi, è possibile eseguire il mapping del discriminatore a una proprietà reale e configurarlo:</span><span class="sxs-lookup"><span data-stu-id="8fcb6-127">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>

```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
