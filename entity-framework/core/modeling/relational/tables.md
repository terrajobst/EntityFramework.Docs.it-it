---
title: Tabella di Mapping - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: 73957d9c77e6856bfb65e10e6b373c337101f7d9
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="table-mapping"></a><span data-ttu-id="0158b-102">Mapping di tabella</span><span class="sxs-lookup"><span data-stu-id="0158b-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="0158b-103">La configurazione di questa sezione è applicabile a database relazionali in generale.</span><span class="sxs-lookup"><span data-stu-id="0158b-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="0158b-104">I metodi di estensione qui verranno rese disponibili quando si installa un provider di database relazionali (a causa di condiviso *Microsoft.EntityFrameworkCore.Relational* pacchetto).</span><span class="sxs-lookup"><span data-stu-id="0158b-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="0158b-105">Mapping della tabella identifica i dati di tabella devono essere una query e salvati nel database.</span><span class="sxs-lookup"><span data-stu-id="0158b-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="0158b-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="0158b-106">Conventions</span></span>

<span data-ttu-id="0158b-107">Per convenzione, ogni entità verrà configurato per eseguire il mapping a una tabella con lo stesso nome di `DbSet<TEntity>` proprietà che espone l'entità al contesto derivata.</span><span class="sxs-lookup"><span data-stu-id="0158b-107">By convention, each entity will be setup to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="0158b-108">Se nessun `DbSet<TEntity>` è incluso per l'entità specificata, viene utilizzato il nome della classe.</span><span class="sxs-lookup"><span data-stu-id="0158b-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="0158b-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="0158b-109">Data Annotations</span></span>

<span data-ttu-id="0158b-110">Per configurare la tabella che esegue il mapping a un tipo, è possibile utilizzare le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="0158b-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="0158b-111">È inoltre possibile specificare uno schema a cui appartiene la tabella.</span><span class="sxs-lookup"><span data-stu-id="0158b-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="0158b-112">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="0158b-112">Fluent API</span></span>

<span data-ttu-id="0158b-113">Per configurare la tabella che esegue il mapping a un tipo, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="0158b-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="0158b-114">È inoltre possibile specificare uno schema a cui appartiene la tabella.</span><span class="sxs-lookup"><span data-stu-id="0158b-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```