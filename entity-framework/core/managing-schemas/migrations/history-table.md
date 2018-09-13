---
title: Tabella di cronologia personalizzata migrazioni - Entity Framework Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
ms.openlocfilehash: 1a253972a8f4e410421ec8a77c079e588d368819
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488816"
---
<a name="custom-migrations-history-table"></a>Tabella di cronologia migrazioni personalizzato
===============================
Per impostazione predefinita, EF Core tiene traccia di quali migrazioni sono state applicate al database registrandoli in una tabella denominata `__EFMigrationsHistory`. Per vari motivi, è possibile personalizzare questa tabella in base alle proprie esigenze.

> [!IMPORTANT]
> Se si personalizza la tabella di cronologia migrazioni *dopo* applicare le migrazioni, è responsabile per l'aggiornamento la tabella esistente nel database.

<a name="schema-and-table-name"></a>Nome dello schema e della tabella
----------------------
È possibile modificare lo schema e nome della tabella usando il `MigrationsHistoryTable()` nel metodo `OnConfiguring()` (o `ConfigureServices()` in ASP.NET Core). Di seguito è riportato un esempio con il provider di EF Core di SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>Altre modifiche
-------------
Per configurare altri aspetti della tabella, eseguire l'override e sostituire le specifiche del provider `IHistoryRepository` servizio. Di seguito è riportato un esempio di modificare il nome della colonna MigrationId *Id* in SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository` si trova all'interno di uno spazio dei nomi interna e potrebbe cambiare nelle versioni future.

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
