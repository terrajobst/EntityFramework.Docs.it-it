---
title: Tabella di cronologia personalizzata migrazioni - Entity Framework Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.openlocfilehash: 7ee76cadd6fac4ec403918e88460e43067ae5815
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995696"
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
