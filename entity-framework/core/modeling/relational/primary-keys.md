---
title: Chiavi primarie-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: bdb31964d717c64bddc28e7f1ce55b261e285a9b
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196944"
---
# <a name="primary-keys"></a><span data-ttu-id="b72d5-102">Chiavi primarie</span><span class="sxs-lookup"><span data-stu-id="b72d5-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="b72d5-103">La configurazione di questa sezione è applicabile in generale ai database relazionali.</span><span class="sxs-lookup"><span data-stu-id="b72d5-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="b72d5-104">I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).</span><span class="sxs-lookup"><span data-stu-id="b72d5-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="b72d5-105">Per la chiave di ogni tipo di entità è stato introdotto un vincolo PRIMARY KEY.</span><span class="sxs-lookup"><span data-stu-id="b72d5-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="b72d5-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="b72d5-106">Conventions</span></span>

<span data-ttu-id="b72d5-107">Per convenzione, la chiave primaria nel database verrà denominata `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="b72d5-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="b72d5-108">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="b72d5-108">Data Annotations</span></span>

<span data-ttu-id="b72d5-109">Non è possibile configurare aspetti specifici di un database relazionale di una chiave primaria usando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="b72d5-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b72d5-110">API Fluent</span><span class="sxs-lookup"><span data-stu-id="b72d5-110">Fluent API</span></span>

<span data-ttu-id="b72d5-111">È possibile utilizzare l'API Fluent per configurare il nome del vincolo PRIMARY KEY nel database.</span><span class="sxs-lookup"><span data-stu-id="b72d5-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/KeyName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasKey(b => b.BlogId)
            .HasName("PrimaryKey_BlogId");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
