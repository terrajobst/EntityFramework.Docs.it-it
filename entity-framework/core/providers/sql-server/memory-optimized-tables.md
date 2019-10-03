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
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Supporto per le tabelle con ottimizzazione per la memoria nel provider SQL Server EF Core database

Le [tabelle con ottimizzazione](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) per la memoria sono una funzionalità di SQL Server in cui l'intera tabella risiede in memoria. Una seconda copia dei dati della tabella viene mantenuta su disco, ma solo per motivi di durabilità. I dati nelle tabelle ottimizzate per la memoria vengono letti dal disco solo durante il recupero del database. Ad esempio, dopo il riavvio del server.

## <a name="configuring-a-memory-optimized-table"></a>Configurazione di una tabella ottimizzata per la memoria

È possibile specificare che la tabella a cui viene eseguito il mapping di un'entità è ottimizzata per la memoria. Quando si usa EF Core per creare e gestire un database in base al modello (con le migrazioni o `Database.EnsureCreated()`), per queste entità verrà creata una tabella ottimizzata per la memoria.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
