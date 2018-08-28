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
<a name="custom-migrations-history-table"></a><span data-ttu-id="db1df-102">Tabella di cronologia migrazioni personalizzato</span><span class="sxs-lookup"><span data-stu-id="db1df-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="db1df-103">Per impostazione predefinita, EF Core tiene traccia di quali migrazioni sono state applicate al database registrandoli in una tabella denominata `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="db1df-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="db1df-104">Per vari motivi, è possibile personalizzare questa tabella in base alle proprie esigenze.</span><span class="sxs-lookup"><span data-stu-id="db1df-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="db1df-105">Se si personalizza la tabella di cronologia migrazioni *dopo* applicare le migrazioni, è responsabile per l'aggiornamento la tabella esistente nel database.</span><span class="sxs-lookup"><span data-stu-id="db1df-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="db1df-106">Nome dello schema e della tabella</span><span class="sxs-lookup"><span data-stu-id="db1df-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="db1df-107">È possibile modificare lo schema e nome della tabella usando il `MigrationsHistoryTable()` nel metodo `OnConfiguring()` (o `ConfigureServices()` in ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="db1df-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="db1df-108">Di seguito è riportato un esempio con il provider di EF Core di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="db1df-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="db1df-109">Altre modifiche</span><span class="sxs-lookup"><span data-stu-id="db1df-109">Other changes</span></span>
-------------
<span data-ttu-id="db1df-110">Per configurare altri aspetti della tabella, eseguire l'override e sostituire le specifiche del provider `IHistoryRepository` servizio.</span><span class="sxs-lookup"><span data-stu-id="db1df-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="db1df-111">Di seguito è riportato un esempio di modificare il nome della colonna MigrationId *Id* in SQL Server.</span><span class="sxs-lookup"><span data-stu-id="db1df-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="db1df-112">`SqlServerHistoryRepository` si trova all'interno di uno spazio dei nomi interna e potrebbe cambiare nelle versioni future.</span><span class="sxs-lookup"><span data-stu-id="db1df-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
