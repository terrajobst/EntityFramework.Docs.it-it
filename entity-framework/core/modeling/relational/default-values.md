---
title: Valori predefiniti - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
ms.technology: entity-framework-core
uid: core/modeling/relational/default-values
ms.openlocfilehash: 73b916b6d9f9c984c8ea010f2319eafa7d031a58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052761"
---
# <a name="default-values"></a><span data-ttu-id="99ab7-102">Valori predefiniti</span><span class="sxs-lookup"><span data-stu-id="99ab7-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="99ab7-103">La configurazione di questa sezione è applicabile a database relazionali in generale.</span><span class="sxs-lookup"><span data-stu-id="99ab7-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="99ab7-104">I metodi di estensione qui verranno rese disponibili quando si installa un provider di database relazionali (a causa di condiviso *Microsoft.EntityFrameworkCore.Relational* pacchetto).</span><span class="sxs-lookup"><span data-stu-id="99ab7-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="99ab7-105">Il valore predefinito di una colonna è il valore che verrà inserito se viene inserita una nuova riga ma viene specificato alcun valore per la colonna.</span><span class="sxs-lookup"><span data-stu-id="99ab7-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="99ab7-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="99ab7-106">Conventions</span></span>

<span data-ttu-id="99ab7-107">Per convenzione, un valore predefinito non è configurato.</span><span class="sxs-lookup"><span data-stu-id="99ab7-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="99ab7-108">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="99ab7-108">Data Annotations</span></span>

<span data-ttu-id="99ab7-109">Non è possibile impostare un valore predefinito mediante le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="99ab7-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="99ab7-110">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="99ab7-110">Fluent API</span></span>

<span data-ttu-id="99ab7-111">Per specificare il valore predefinito per una proprietà, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="99ab7-111">You can use the Fluent API to specify the default value for a property.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValue.cs?highlight=9)] -->
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

<span data-ttu-id="99ab7-112">È inoltre possibile specificare un frammento SQL utilizzato per calcolare il valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="99ab7-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValueSql.cs?highlight=9)] -->
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
