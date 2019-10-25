---
title: Tabella di cronologia migrazioni personalizzate-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0db393ff3101564f8d8081d0a57b264c2c459df7
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812077"
---
# <a name="custom-migrations-history-table"></a>Tabella di cronologia migrazioni personalizzate

Per impostazione predefinita, EF Core tiene traccia di quali migrazioni sono state applicate al database mediante la registrazione in una tabella denominata `__EFMigrationsHistory`. Per diversi motivi, potrebbe essere necessario personalizzare questa tabella per adattarla alle proprie esigenze.

> [!IMPORTANT]
> Se si Personalizza la tabella di cronologia delle migrazioni *dopo aver* applicato le migrazioni, si è responsabili dell'aggiornamento della tabella esistente nel database.

## <a name="schema-and-table-name"></a>Nome schema e tabella

È possibile modificare il nome dello schema e della tabella usando il metodo `MigrationsHistoryTable()` in `OnConfiguring()` (o `ConfigureServices()` in ASP.NET Core). Di seguito è riportato un esempio che usa il provider di EF Core SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

## <a name="other-changes"></a>Altre modifiche

Per configurare altri aspetti della tabella, eseguire l'override e sostituire il servizio `IHistoryRepository` specifico del provider. Ecco un esempio di modifica del nome della colonna MigrationId in *ID* in SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository` si trova all'interno di uno spazio dei nomi interno e può cambiare nelle versioni future.

``` csharp
class MyHistoryRepository : SqlServerHistoryRepository
{
    public MyHistoryRepository(HistoryRepositoryDependencies dependencies)
        : base(dependencies)
    {
    }

    protected override void ConfigureTable(EntityTypeBuilder<HistoryRow> history)
    {
        base.ConfigureTable(history);

        history.Property(h => h.MigrationId).HasColumnName("Id");
    }
}
```
