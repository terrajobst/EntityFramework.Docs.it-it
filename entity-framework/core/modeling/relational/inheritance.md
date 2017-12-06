---
title: "Ereditarietà (Database relazionale) - Core a Entity Framework"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: a7f697dfe2b93c7b93a2dd14945732db4f37628c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="ba783-102">Ereditarietà (Database relazionale)</span><span class="sxs-lookup"><span data-stu-id="ba783-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="ba783-103">La configurazione di questa sezione è applicabile a database relazionali in generale.</span><span class="sxs-lookup"><span data-stu-id="ba783-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="ba783-104">I metodi di estensione qui verranno rese disponibili quando si installa un provider di database relazionali (a causa di condiviso *Microsoft.EntityFrameworkCore.Relational* pacchetto).</span><span class="sxs-lookup"><span data-stu-id="ba783-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="ba783-105">Ereditarietà nel modello di Entity Framework viene utilizzato per controllare la modalità di rappresentazione di ereditarietà nelle classi di entità nel database.</span><span class="sxs-lookup"><span data-stu-id="ba783-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="ba783-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="ba783-106">Conventions</span></span>

<span data-ttu-id="ba783-107">Per convenzione, ereditarietà verrà mappato usando il modello di tabella per ogni-gerarchia (TPH).</span><span class="sxs-lookup"><span data-stu-id="ba783-107">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="ba783-108">Implementerà Usa una singola tabella per archiviare i dati per tutti i tipi nella gerarchia.</span><span class="sxs-lookup"><span data-stu-id="ba783-108">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="ba783-109">Una colonna discriminatore viene utilizzata per identificare il tipo di ogni riga rappresenta.</span><span class="sxs-lookup"><span data-stu-id="ba783-109">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="ba783-110">EF installerà ereditarietà solo se due o più tipi ereditati sono inclusi in modo esplicito nel modello (vedere [ereditarietà](../inheritance.md) per altri dettagli).</span><span class="sxs-lookup"><span data-stu-id="ba783-110">EF will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="ba783-111">Di seguito è riportato un esempio che illustra uno scenario semplice di ereditarietà e i dati archiviati in una tabella di database relazionale utilizzando il modello della tabella per gerarchia.</span><span class="sxs-lookup"><span data-stu-id="ba783-111">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="ba783-112">Il *discriminatore* colonna identifica il tipo di *Blog* viene archiviato in ogni riga.</span><span class="sxs-lookup"><span data-stu-id="ba783-112">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="ba783-114">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="ba783-114">Data Annotations</span></span>

<span data-ttu-id="ba783-115">Per configurare l'ereditarietà, è possibile utilizzare le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="ba783-115">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ba783-116">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="ba783-116">Fluent API</span></span>

<span data-ttu-id="ba783-117">Per configurare il nome e il tipo di colonna discriminatore e i valori utilizzati per identificare ogni tipo nella gerarchia, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="ba783-117">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

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
