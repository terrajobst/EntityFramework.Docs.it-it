---
title: Microsoft SQL Server Database, tabelle con ottimizzazione per la memoria - Provider EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 83a0decffc5946d036903b8b8add59f0ea31b21f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052641"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="e884d-102">Le tabelle con ottimizzazione per la memoria supportano nel Provider di Database di SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="e884d-102">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

> [!NOTE]  
>
> <span data-ttu-id="e884d-103">Questa funzionalità è stato introdotto in Entity Framework Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="e884d-103">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="e884d-104">[Tabelle con ottimizzazione della memoria](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) sono una funzionalità di SQL Server in cui l'intera tabella risiede in memoria.</span><span class="sxs-lookup"><span data-stu-id="e884d-104">[Memory-Optimized Tables](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="e884d-105">Una seconda copia dei dati della tabella viene mantenuta su disco, ma solo per motivi di durabilità.</span><span class="sxs-lookup"><span data-stu-id="e884d-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="e884d-106">I dati nelle tabelle ottimizzate per la memoria vengono letti dal disco solo durante il recupero del database.</span><span class="sxs-lookup"><span data-stu-id="e884d-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="e884d-107">Ad esempio dopo un riavvio del server.</span><span class="sxs-lookup"><span data-stu-id="e884d-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="e884d-108">Configurazione di una tabella con ottimizzazione per la memoria</span><span class="sxs-lookup"><span data-stu-id="e884d-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="e884d-109">È possibile specificare che la tabella a che viene eseguito il mapping di un'entità è con ottimizzazione per la memoria.</span><span class="sxs-lookup"><span data-stu-id="e884d-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="e884d-110">Quando l'utilizzo di Entity Framework Core per creare e gestire un database in base al modello (con le migrazioni o `Database.EnsureCreated()`), verrà creata una tabella con ottimizzazione per la memoria per queste entità.</span><span class="sxs-lookup"><span data-stu-id="e884d-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
