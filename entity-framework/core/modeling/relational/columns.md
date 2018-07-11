---
title: Mapping colonne - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
ms.technology: entity-framework-core
uid: core/modeling/relational/columns
ms.openlocfilehash: ac3ab2ce3faa54eb8e862d01dcecb48cb0d1f811
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949411"
---
# <a name="column-mapping"></a><span data-ttu-id="18ea7-102">Mapping di colonne</span><span class="sxs-lookup"><span data-stu-id="18ea7-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="18ea7-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="18ea7-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="18ea7-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="18ea7-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="18ea7-105">Mapping di colonne identifica quali dati di colonna devono essere una query e salvati nel database.</span><span class="sxs-lookup"><span data-stu-id="18ea7-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="18ea7-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="18ea7-106">Conventions</span></span>

<span data-ttu-id="18ea7-107">Per convenzione, ogni proprietà essere configurerà per eseguire il mapping a una colonna con lo stesso nome della proprietà.</span><span class="sxs-lookup"><span data-stu-id="18ea7-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="18ea7-108">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="18ea7-108">Data Annotations</span></span>

<span data-ttu-id="18ea7-109">È possibile usare le annotazioni dei dati per configurare la colonna a cui viene mappata una proprietà.</span><span class="sxs-lookup"><span data-stu-id="18ea7-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=3)] -->
``` csharp
public class Blog
{
    [Column("blog_id")]
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="18ea7-110">API Fluent</span><span class="sxs-lookup"><span data-stu-id="18ea7-110">Fluent API</span></span>

<span data-ttu-id="18ea7-111">È possibile usare l'API Fluent per configurare la colonna a cui viene mappata una proprietà.</span><span class="sxs-lookup"><span data-stu-id="18ea7-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

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
