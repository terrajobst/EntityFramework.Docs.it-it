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
ms.locfileid: "26053811"
---
<a name="custom-migrations-history-table"></a><span data-ttu-id="038e2-102">Tabella di cronologia di migrazioni personalizzato</span><span class="sxs-lookup"><span data-stu-id="038e2-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="038e2-103">Per impostazione predefinita, Core EF tiene traccia di quali migrazione applicata al database registrandoli in una tabella denominata `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="038e2-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="038e2-104">Per vari motivi, si desidera personalizzare la tabella per adattarlo alle proprie esigenze.</span><span class="sxs-lookup"><span data-stu-id="038e2-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="038e2-105">Se si personalizza la tabella di cronologia di migrazioni *dopo* applicando le migrazioni, l'utente è responsabile per l'aggiornamento della tabella esistente nel database.</span><span class="sxs-lookup"><span data-stu-id="038e2-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="038e2-106">Nome dello schema e della tabella</span><span class="sxs-lookup"><span data-stu-id="038e2-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="038e2-107">È possibile modificare lo schema e il nome di tabella utilizzando il `MigrationsHistoryTable()` metodo `OnConfiguring()` (o `ConfigureServices()` su ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="038e2-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="038e2-108">Di seguito è riportato un esempio di utilizzo del provider di Entity Framework Core di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="038e2-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="038e2-109">Altre modifiche</span><span class="sxs-lookup"><span data-stu-id="038e2-109">Other changes</span></span>
-------------
<span data-ttu-id="038e2-110">Per configurare altri aspetti della tabella, eseguire l'override e sostituire la specifica del provider `IHistoryRepository` servizio.</span><span class="sxs-lookup"><span data-stu-id="038e2-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="038e2-111">Di seguito è riportato un esempio di modificare il nome di colonna MigrationId di *Id* in SQL Server.</span><span class="sxs-lookup"><span data-stu-id="038e2-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="038e2-112">`SqlServerHistoryRepository`si trova all'interno di uno spazio dei nomi interno e potrebbe cambiare nelle versioni future.</span><span class="sxs-lookup"><span data-stu-id="038e2-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
