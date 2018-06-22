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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Le tabelle con ottimizzazione per la memoria supportano nel Provider di Database di SQL Server EF Core

> [!NOTE]  
>
> Questa funzionalità è stato introdotto in Entity Framework Core 1.1.

[Tabelle con ottimizzazione della memoria](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) sono una funzionalità di SQL Server in cui l'intera tabella risiede in memoria. Una seconda copia dei dati della tabella viene mantenuta su disco, ma solo per motivi di durabilità. I dati nelle tabelle ottimizzate per la memoria vengono letti dal disco solo durante il recupero del database. Ad esempio dopo un riavvio del server.

## <a name="configuring-a-memory-optimized-table"></a>Configurazione di una tabella con ottimizzazione per la memoria

È possibile specificare che la tabella a che viene eseguito il mapping di un'entità è con ottimizzazione per la memoria. Quando l'utilizzo di Entity Framework Core per creare e gestire un database in base al modello (con le migrazioni o `Database.EnsureCreated()`), verrà creata una tabella con ottimizzazione per la memoria per queste entità.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
