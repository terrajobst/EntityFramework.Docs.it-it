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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Supporto per le tabelle con ottimizzazione per la memoria nel provider SQL Server EF Core database

Le [tabelle con ottimizzazione](/sql/relational-databases/in-memory-oltp/memory-optimized-tables) per la memoria sono una funzionalità di SQL Server in cui l'intera tabella risiede in memoria. Una seconda copia dei dati della tabella viene mantenuta su disco, ma solo per motivi di durabilità. I dati nelle tabelle ottimizzate per la memoria vengono letti dal disco solo durante il recupero del database. Ad esempio dopo un riavvio del server.

## <a name="configuring-a-memory-optimized-table"></a>Configurazione di una tabella ottimizzata per la memoria

È possibile specificare che la tabella a cui viene eseguito il mapping di un'entità è ottimizzata per la memoria. Quando si usa EF Core per creare e gestire un database basato sul modello (con [migrazioni](xref:core/managing-schemas/migrations/index) o [EnsureCreated](/dotnet/api/Microsoft.EntityFrameworkCore.Storage.IDatabaseCreator.EnsureCreated)), verrà creata una tabella ottimizzata per la memoria per queste entità.

[!code-csharp[IsMemoryOptimized](../../../../samples/core/SqlServer/InMemory/InMemoryContext.cs?name=IsMemoryOptimized)]
