---
title: "Migrazioni con più provider - Core a Entity Framework"
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 6b278a5ae270b6a84269dffd72eeca609b168cdd
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2017
---
<a name="migrations-with-multiple-providers"></a>Migrazioni con più provider
==================================
Il [strumenti di base di EF] [ 1] solo lo scaffolding le migrazioni per il provider attivo. In alcuni casi, tuttavia, si consiglia di utilizzare più di un provider (ad esempio Microsoft SQL Server e SQLite) con l'elemento DbContext. Esistono due modi per gestire questo problema con le migrazioni. È possibile mantenere due set di migrazioni: uno per ogni provider, o in un singolo set di unione può utilizzare entrambi.

<a name="two-migration-sets"></a>Due set di migrazione
------------------
Il primo approccio è generare due migrazioni per ogni modifica del modello.

Un modo per eseguire questo consiste nell'inserire ogni set di migrazione [in un assembly separato] [ 2] e passare manualmente il provider attivo (e l'assembly di migrazioni) tra l'aggiunta di due migrazioni.

Un altro approccio che facilita l'utilizzo con gli strumenti di consiste nel creare un nuovo tipo deriva dal DbContext e sovrascrive il provider attivo. Questo tipo viene utilizzato in fase di progettazione di tempo durante l'aggiunta o l'applicazione delle migrazioni.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Poiché ogni set di migrazione utilizza i propri tipi DbContext, questo approccio non richiede l'utilizzo di un assembly separato migrazioni.

Quando si aggiunge una nuova migrazione, è possibile specificare i tipi di contesto.

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> Non è necessario specificare la directory di output per migrazioni successive perché vengono create come elementi di pari livello all'ultimo segnalibro.

<a name="one-migration-set"></a>Migrazione di un set
-----------------
Se non sono quelli con due set di migrazioni, è possibile combinarli manualmente in un unico gruppo che può essere applicato a entrambi i provider.

Le annotazioni possono coesistere poiché un provider ignora tutte le annotazioni che non riconosce. Ad esempio, una colonna chiave primaria che funziona con Microsoft SQL Server e SQLite potrebbe essere simile al seguente.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Se operazioni possono essere applicate solo in un provider (o si in modo diverso tra provider), utilizzare il `ActiveProvider` proprietà in modo che il provider è attivo.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
