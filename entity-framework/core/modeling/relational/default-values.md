---
title: Valori predefiniti-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: 0d3613606f21a78e22cfe0ee752ea982a6a17f93
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196994"
---
# <a name="default-values"></a><span data-ttu-id="0ea93-102">Valori predefiniti</span><span class="sxs-lookup"><span data-stu-id="0ea93-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="0ea93-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="0ea93-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="0ea93-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="0ea93-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="0ea93-105">Il valore predefinito di una colonna è il valore che verrà inserito se viene inserita una nuova riga, ma non viene specificato alcun valore per la colonna.</span><span class="sxs-lookup"><span data-stu-id="0ea93-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="0ea93-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="0ea93-106">Conventions</span></span>

<span data-ttu-id="0ea93-107">Per convenzione, un valore predefinito non è configurato.</span><span class="sxs-lookup"><span data-stu-id="0ea93-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="0ea93-108">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="0ea93-108">Data Annotations</span></span>

<span data-ttu-id="0ea93-109">Non è possibile impostare un valore predefinito utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="0ea93-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="0ea93-110">API Fluent</span><span class="sxs-lookup"><span data-stu-id="0ea93-110">Fluent API</span></span>

<span data-ttu-id="0ea93-111">È possibile usare l'API Fluent per specificare il valore predefinito per una proprietà.</span><span class="sxs-lookup"><span data-stu-id="0ea93-111">You can use the Fluent API to specify the default value for a property.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultValue.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Rating)
            .HasDefaultValue(3);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public int Rating { get; set; }
}
```

<span data-ttu-id="0ea93-112">È inoltre possibile specificare un frammento SQL utilizzato per calcolare il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="0ea93-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultValueSql.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Created)
            .HasDefaultValueSql("getdate()");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public DateTime Created { get; set; }
}
```
