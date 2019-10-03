---
title: Provider di database Microsoft SQL Server-tabelle con ottimizzazione per la memoria-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 7383e74b3f83172f9b8e0eaf9bd09d4e187e87f8
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813488"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="7a5cf-102">Supporto per le tabelle con ottimizzazione per la memoria nel provider SQL Server EF Core database</span><span class="sxs-lookup"><span data-stu-id="7a5cf-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="7a5cf-103">Le [tabelle con ottimizzazione](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) per la memoria sono una funzionalità di SQL Server in cui l'intera tabella risiede in memoria.</span><span class="sxs-lookup"><span data-stu-id="7a5cf-103">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="7a5cf-104">Una seconda copia dei dati della tabella viene mantenuta su disco, ma solo per motivi di durabilità.</span><span class="sxs-lookup"><span data-stu-id="7a5cf-104">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="7a5cf-105">I dati nelle tabelle ottimizzate per la memoria vengono letti dal disco solo durante il recupero del database.</span><span class="sxs-lookup"><span data-stu-id="7a5cf-105">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="7a5cf-106">Ad esempio, dopo il riavvio del server.</span><span class="sxs-lookup"><span data-stu-id="7a5cf-106">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="7a5cf-107">Configurazione di una tabella ottimizzata per la memoria</span><span class="sxs-lookup"><span data-stu-id="7a5cf-107">Configuring a memory-optimized table</span></span>

<span data-ttu-id="7a5cf-108">È possibile specificare che la tabella a cui viene eseguito il mapping di un'entità è ottimizzata per la memoria.</span><span class="sxs-lookup"><span data-stu-id="7a5cf-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="7a5cf-109">Quando si usa EF Core per creare e gestire un database in base al modello (con le migrazioni o `Database.EnsureCreated()`), per queste entità verrà creata una tabella ottimizzata per la memoria.</span><span class="sxs-lookup"><span data-stu-id="7a5cf-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
