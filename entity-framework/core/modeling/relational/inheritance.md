---
title: "Ereditarietà (Database relazionale) - Core a Entity Framework"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 55286adf08a6a1c3286b7059d747a62e1feffd22
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/22/2017
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="cf4a6-102">Ereditarietà (Database relazionale)</span><span class="sxs-lookup"><span data-stu-id="cf4a6-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="cf4a6-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="cf4a6-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="cf4a6-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="cf4a6-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="cf4a6-105">Ereditarietà nel modello di Entity Framework viene utilizzato per controllare la modalità di rappresentazione di ereditarietà nelle classi di entità nel database.</span><span class="sxs-lookup"><span data-stu-id="cf4a6-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="cf4a6-106">Attualmente, solo il criterio di tabella per gerarchia (Implementerà) viene implementato in EF Core.</span><span class="sxs-lookup"><span data-stu-id="cf4a6-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="cf4a6-107">Altri modelli comuni come tabella per tipo (tabella per tipo) e -per-concreto-tipo di tabella (TPC) non sono ancora disponibili.</span><span class="sxs-lookup"><span data-stu-id="cf4a6-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="cf4a6-108">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="cf4a6-108">Conventions</span></span>

<span data-ttu-id="cf4a6-109">Per convenzione, ereditarietà verrà mappato usando il modello di tabella per ogni-gerarchia (TPH).</span><span class="sxs-lookup"><span data-stu-id="cf4a6-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="cf4a6-110">Implementerà Usa una singola tabella per archiviare i dati per tutti i tipi nella gerarchia.</span><span class="sxs-lookup"><span data-stu-id="cf4a6-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="cf4a6-111">Una colonna discriminatore viene utilizzata per identificare il tipo di ogni riga rappresenta.</span><span class="sxs-lookup"><span data-stu-id="cf4a6-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="cf4a6-112">Core EF installerà ereditarietà solo se due o più tipi ereditati sono inclusi in modo esplicito nel modello (vedere [ereditarietà](../inheritance.md) per altri dettagli).</span><span class="sxs-lookup"><span data-stu-id="cf4a6-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="cf4a6-113">Di seguito è riportato un esempio che illustra uno scenario semplice di ereditarietà e i dati archiviati in una tabella di database relazionale utilizzando il modello della tabella per gerarchia.</span><span class="sxs-lookup"><span data-stu-id="cf4a6-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="cf4a6-114">Il *discriminatore* colonna identifica il tipo di *Blog* viene archiviato in ogni riga.</span><span class="sxs-lookup"><span data-stu-id="cf4a6-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="cf4a6-116">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="cf4a6-116">Data Annotations</span></span>

<span data-ttu-id="cf4a6-117">Per configurare l'ereditarietà, è possibile utilizzare le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="cf4a6-117">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="cf4a6-118">API Fluent</span><span class="sxs-lookup"><span data-stu-id="cf4a6-118">Fluent API</span></span>

<span data-ttu-id="cf4a6-119">Per configurare il nome e il tipo di colonna discriminatore e i valori utilizzati per identificare ogni tipo nella gerarchia, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="cf4a6-119">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

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
