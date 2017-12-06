---
title: Indici - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: 683b580bb155e0416f13c5d63e3280078fbcee21
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a><span data-ttu-id="0ae9f-102">Indici</span><span class="sxs-lookup"><span data-stu-id="0ae9f-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="0ae9f-103">La configurazione di questa sezione è applicabile a database relazionali in generale.</span><span class="sxs-lookup"><span data-stu-id="0ae9f-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="0ae9f-104">I metodi di estensione qui verranno rese disponibili quando si installa un provider di database relazionali (a causa di condiviso *Microsoft.EntityFrameworkCore.Relational* pacchetto).</span><span class="sxs-lookup"><span data-stu-id="0ae9f-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="0ae9f-105">Esegue il mapping di un indice in un database relazionale per lo stesso concetto di un indice in base di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0ae9f-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="0ae9f-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="0ae9f-106">Conventions</span></span>

<span data-ttu-id="0ae9f-107">Per convenzione, gli indici sono denominati `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="0ae9f-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="0ae9f-108">Per gli indici composti `<property name>` diventa un elenco separato da un carattere di sottolineatura di nomi di proprietà.</span><span class="sxs-lookup"><span data-stu-id="0ae9f-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="0ae9f-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="0ae9f-109">Data Annotations</span></span>

<span data-ttu-id="0ae9f-110">Gli indici non possono essere configurati utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="0ae9f-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="0ae9f-111">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="0ae9f-111">Fluent API</span></span>

<span data-ttu-id="0ae9f-112">Per configurare il nome di un indice, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="0ae9f-112">You can use the Fluent API to configure the name of an index.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/IndexName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .HasName("Index_Url");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
