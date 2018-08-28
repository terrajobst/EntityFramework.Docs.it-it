---
title: Mapping tabella - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 32c5e3cc0e498005ce8e6be1f1ee7e8ddf9b510d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994137"
---
# <a name="table-mapping"></a><span data-ttu-id="345f5-102">Mapping di tabella</span><span class="sxs-lookup"><span data-stu-id="345f5-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="345f5-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="345f5-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="345f5-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="345f5-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="345f5-105">Mapping tabella identifica quali i dati della tabella devono essere una query e salvati nel database.</span><span class="sxs-lookup"><span data-stu-id="345f5-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="345f5-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="345f5-106">Conventions</span></span>

<span data-ttu-id="345f5-107">Per convenzione, ogni entità verrà configurato per eseguire il mapping a una tabella con lo stesso nome di `DbSet<TEntity>` proprietà che espone l'entità nel contesto derivato.</span><span class="sxs-lookup"><span data-stu-id="345f5-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="345f5-108">Se nessun `DbSet<TEntity>` è incluso per l'entità specificata, viene usato il nome della classe.</span><span class="sxs-lookup"><span data-stu-id="345f5-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="345f5-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="345f5-109">Data Annotations</span></span>

<span data-ttu-id="345f5-110">È possibile usare le annotazioni dei dati per configurare la tabella che esegue il mapping a un tipo.</span><span class="sxs-lookup"><span data-stu-id="345f5-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="345f5-111">È anche possibile specificare uno schema a cui appartiene la tabella.</span><span class="sxs-lookup"><span data-stu-id="345f5-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="345f5-112">API Fluent</span><span class="sxs-lookup"><span data-stu-id="345f5-112">Fluent API</span></span>

<span data-ttu-id="345f5-113">È possibile usare l'API Fluent per configurare la tabella che esegue il mapping a un tipo.</span><span class="sxs-lookup"><span data-stu-id="345f5-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="345f5-114">È anche possibile specificare uno schema a cui appartiene la tabella.</span><span class="sxs-lookup"><span data-stu-id="345f5-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
