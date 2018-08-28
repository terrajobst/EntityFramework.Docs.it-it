---
title: 'Microsoft SQL Server Database, tabelle ottimizzate per la memoria: Provider EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/memory-optimized-tables
ms.openlocfilehash: 63d2cbf8b69e4f1945ad60914e284fb42c48e8db
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995802"
---
# <a name="memory-optimized-tables-support-in-sql-server-ef-core-database-provider"></a>Le tabelle ottimizzate per la memoria supportano nel Provider di Database di SQL Server EF Core

> [!NOTE]  
>
> Questa funzionalità è stata introdotta in EF Core 1.1.

[Tabelle con ottimizzazione della memoria](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/memory-optimized-tables) sono una funzionalità di SQL Server in cui l'intera tabella risiede in memoria. Una seconda copia dei dati della tabella viene mantenuta su disco, ma solo per motivi di durabilità. I dati nelle tabelle ottimizzate per la memoria vengono letti solo dal disco durante il recupero del database. Ad esempio, dopo un riavvio del server.

## <a name="configuring-a-memory-optimized-table"></a>Configurazione di una tabella con ottimizzazione per la memoria

È possibile specificare che la tabella a cui viene eseguito il mapping di un'entità è ottimizzata per la memoria. Quando si usa EF Core per creare e gestire un database in base al modello (con le migrazioni o `Database.EnsureCreated()`), per queste entità verrà creata una tabella ottimizzata per la memoria.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .ForSqlServerIsMemoryOptimized();
}
```
