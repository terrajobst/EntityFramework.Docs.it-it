---
title: Provider di database Microsoft SQL Server-tabelle con ottimizzazione per la memoria-EF Core
description: Come usare le tabelle ottimizzate per la memoria con il provider di database Entity Framework Core di SQL Server
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: a504fb3487aea6dd36abf204a7427095e3d29118
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417791"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a><span data-ttu-id="00644-103">Supporto per le tabelle con ottimizzazione per la memoria nel provider SQL Server EF Core database</span><span class="sxs-lookup"><span data-stu-id="00644-103">Memory-Optimized Tables support in SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="00644-104">Le [tabelle con ottimizzazione](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) per la memoria sono una funzionalità di SQL Server in cui l'intera tabella risiede in memoria.</span><span class="sxs-lookup"><span data-stu-id="00644-104">[Memory-Optimized Tables](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) are a feature of SQL Server where the entire table resides in memory.</span></span> <span data-ttu-id="00644-105">Una seconda copia dei dati della tabella viene mantenuta su disco, ma solo per motivi di durabilità.</span><span class="sxs-lookup"><span data-stu-id="00644-105">A second copy of the table data is maintained on disk, but only for durability purposes.</span></span> <span data-ttu-id="00644-106">I dati nelle tabelle ottimizzate per la memoria vengono letti dal disco solo durante il recupero del database.</span><span class="sxs-lookup"><span data-stu-id="00644-106">Data in memory-optimized tables is only read from disk during database recovery.</span></span> <span data-ttu-id="00644-107">Ad esempio dopo un riavvio del server.</span><span class="sxs-lookup"><span data-stu-id="00644-107">For example, after a server restart.</span></span>

## <a name="configuring-a-memory-optimized-table"></a><span data-ttu-id="00644-108">Configurazione di una tabella ottimizzata per la memoria</span><span class="sxs-lookup"><span data-stu-id="00644-108">Configuring a memory-optimized table</span></span>

<span data-ttu-id="00644-109">È possibile specificare che la tabella a cui viene eseguito il mapping di un'entità è ottimizzata per la memoria.</span><span class="sxs-lookup"><span data-stu-id="00644-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="00644-110">Quando si usa EF Core per creare e gestire un database basato sul modello (con [migrazioni](xref:core/managing-schemas/migrations/index) o [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), verrà creata una tabella ottimizzata per la memoria per queste entità.</span><span class="sxs-lookup"><span data-stu-id="00644-110">When using EF Core to create and maintain a database based on your model (either with [migrations](xref:core/managing-schemas/migrations/index) or [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), a memory-optimized table will be created for these entities.</span></span>

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
