---
title: Mapping di colonna - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
ms.technology: entity-framework-core
uid: core/modeling/relational/columns
ms.openlocfilehash: 697b966dbac892e332fe65feaa4dd11f00dd8298
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052901"
---
# <a name="column-mapping"></a><span data-ttu-id="fd01a-102">Mapping di colonne</span><span class="sxs-lookup"><span data-stu-id="fd01a-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="fd01a-103">La configurazione di questa sezione è applicabile a database relazionali in generale.</span><span class="sxs-lookup"><span data-stu-id="fd01a-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="fd01a-104">I metodi di estensione qui verranno rese disponibili quando si installa un provider di database relazionali (a causa di condiviso *Microsoft.EntityFrameworkCore.Relational* pacchetto).</span><span class="sxs-lookup"><span data-stu-id="fd01a-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="fd01a-105">Mapping della colonna identifica i dati della colonna devono essere una query e salvati nel database.</span><span class="sxs-lookup"><span data-stu-id="fd01a-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="fd01a-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="fd01a-106">Conventions</span></span>

<span data-ttu-id="fd01a-107">Per convenzione, ogni proprietà verrà configurato per eseguire il mapping a una colonna con lo stesso nome della proprietà.</span><span class="sxs-lookup"><span data-stu-id="fd01a-107">By convention, each property will be setup to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="fd01a-108">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="fd01a-108">Data Annotations</span></span>

<span data-ttu-id="fd01a-109">È possibile utilizzare le annotazioni dei dati per configurare la colonna a cui viene eseguito il mapping di una proprietà.</span><span class="sxs-lookup"><span data-stu-id="fd01a-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=3)] -->
``` csharp
public class Blog
{
    [Column("blog_id")]
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="fd01a-110">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="fd01a-110">Fluent API</span></span>

<span data-ttu-id="fd01a-111">Per configurare la colonna a cui viene eseguito il mapping di una proprietà, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="fd01a-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.BlogId)
            .HasColumnName("blog_id");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
