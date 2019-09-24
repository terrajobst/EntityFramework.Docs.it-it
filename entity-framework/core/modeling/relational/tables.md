---
title: Mapping tabella-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 62dce317b901bc862b3c7d20ed1d176805bb24dd
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196963"
---
# <a name="table-mapping"></a><span data-ttu-id="05818-102">Mapping tabella</span><span class="sxs-lookup"><span data-stu-id="05818-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="05818-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="05818-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="05818-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="05818-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="05818-105">Il mapping delle tabelle identifica i dati della tabella da cui devono essere eseguite query e salvati nel database.</span><span class="sxs-lookup"><span data-stu-id="05818-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="05818-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="05818-106">Conventions</span></span>

<span data-ttu-id="05818-107">Per convenzione, ogni entità viene configurata in modo da eseguire il mapping a una tabella con lo stesso nome della proprietà `DbSet<TEntity>` che espone l'entità nel contesto derivato.</span><span class="sxs-lookup"><span data-stu-id="05818-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="05818-108">Se per `DbSet<TEntity>` l'entità specificata non è incluso alcun oggetto, viene utilizzato il nome della classe.</span><span class="sxs-lookup"><span data-stu-id="05818-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="05818-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="05818-109">Data Annotations</span></span>

<span data-ttu-id="05818-110">È possibile utilizzare le annotazioni dei dati per configurare la tabella a cui viene eseguito il mapping di un tipo.</span><span class="sxs-lookup"><span data-stu-id="05818-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="05818-111">È inoltre possibile specificare uno schema a cui appartiene la tabella.</span><span class="sxs-lookup"><span data-stu-id="05818-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="05818-112">API Fluent</span><span class="sxs-lookup"><span data-stu-id="05818-112">Fluent API</span></span>

<span data-ttu-id="05818-113">È possibile usare l'API Fluent per configurare la tabella a cui viene eseguito il mapping di un tipo.</span><span class="sxs-lookup"><span data-stu-id="05818-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="05818-114">È inoltre possibile specificare uno schema a cui appartiene la tabella.</span><span class="sxs-lookup"><span data-stu-id="05818-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
