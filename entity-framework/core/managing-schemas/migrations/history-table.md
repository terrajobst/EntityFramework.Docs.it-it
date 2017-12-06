---
title: Tabella di cronologia di migrazioni personalizzato - Core EF
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cb9892241f3d7f1fae6293bd60a8a5c3e7120969
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
<a name="custom-migrations-history-table"></a>Tabella di cronologia di migrazioni personalizzato
===============================
Per impostazione predefinita, Core EF tiene traccia di quali migrazione applicata al database registrandoli in una tabella denominata `__EFMigrationsHistory`. Per vari motivi, si desidera personalizzare la tabella per adattarlo alle proprie esigenze.

> [!IMPORTANT]
> Se si personalizza la tabella di cronologia di migrazioni *dopo* applicando le migrazioni, l'utente è responsabile per l'aggiornamento della tabella esistente nel database.

<a name="schema-and-table-name"></a>Nome dello schema e della tabella
----------------------
È possibile modificare lo schema e il nome di tabella utilizzando il `MigrationsHistoryTable()` metodo `OnConfiguring()` (o `ConfigureServices()` su ASP.NET Core). Di seguito è riportato un esempio di utilizzo del provider di Entity Framework Core di SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>Altre modifiche
-------------
Per configurare altri aspetti della tabella, eseguire l'override e sostituire la specifica del provider `IHistoryRepository` servizio. Di seguito è riportato un esempio di modificare il nome di colonna MigrationId di *Id* in SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository`si trova all'interno di uno spazio dei nomi interno e potrebbe cambiare nelle versioni future.

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
