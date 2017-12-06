---
title: Chiavi primarie - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
ms.technology: entity-framework-core
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: fcb1871149c0f20a2576864028b4171904de1982
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="primary-keys"></a><span data-ttu-id="c418e-102">Chiavi primarie</span><span class="sxs-lookup"><span data-stu-id="c418e-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="c418e-103">La configurazione di questa sezione è applicabile a database relazionali in generale.</span><span class="sxs-lookup"><span data-stu-id="c418e-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="c418e-104">I metodi di estensione qui verranno rese disponibili quando si installa un provider di database relazionali (a causa di condiviso *Microsoft.EntityFrameworkCore.Relational* pacchetto).</span><span class="sxs-lookup"><span data-stu-id="c418e-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="c418e-105">Un vincolo di chiave primaria è stato introdotto per la chiave di ogni tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="c418e-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="c418e-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="c418e-106">Conventions</span></span>

<span data-ttu-id="c418e-107">Per convenzione, la chiave primaria nel database verrà denominata `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="c418e-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c418e-108">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="c418e-108">Data Annotations</span></span>

<span data-ttu-id="c418e-109">Nessun aspetti specifici del database relazionale di una chiave primaria possono essere configurati tramite le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="c418e-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c418e-110">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="c418e-110">Fluent API</span></span>

<span data-ttu-id="c418e-111">È possibile utilizzare l'API Fluent per configurare il nome del vincolo di chiave primaria nel database.</span><span class="sxs-lookup"><span data-stu-id="c418e-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/KeyName.cs?highlight=9)] -->
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
