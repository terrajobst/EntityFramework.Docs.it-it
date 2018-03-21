---
title: "Migrazioni con più provider - Entity Framework Core"
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d950e74ed4cef7d4274aabcf3eda7b0b735574c6
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/20/2018
---
<a name="migrations-with-multiple-providers"></a>Migrazioni con più provider
==================================
Il [strumenti di base di EF] [ 1] lo scaffolding solo le migrazioni per il provider attivo. In alcuni casi, tuttavia, è possibile utilizzare più di un provider (ad esempio Microsoft SQL Server e SQLite) con l'elemento DbContext. Esistono due modi per gestire questo comportamento con le migrazioni. È possibile mantenere due set di migrazioni: uno per ogni provider-- o di tipo merge in un singolo set che può utilizzare entrambi.

<a name="two-migration-sets"></a>Due set di migrazione
------------------
Il primo approccio è generare due migrazioni per ogni modifica del modello.

Un modo per eseguire questo consiste nell'inserire ogni insieme di migrazione [in un assembly separato] [ 2] e passare manualmente il provider attivo (e l'assembly di migrazioni) tra l'aggiunta di due migrazioni.

Un altro approccio che rende più semplice l'uso con gli strumenti di consiste nel creare un nuovo tipo che deriva da di DbContext e sostituisce il provider attivo. Questo tipo viene utilizzato in fase di progettazione quando si aggiunge o applicare le migrazioni di tempo.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Poiché ogni set di migrazione utilizza un proprio i tipi DbContext, questo approccio non richiede l'utilizzo di un assembly separato migrazioni.

Quando si aggiunge una nuova migrazione, specificare i tipi di contesto.

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

<a name="one-migration-set"></a>Set di una migrazione
-----------------
Se non sono quelli desiderati con due set di migrazioni, è possibile combinarle manualmente in un unico gruppo che può essere applicato a entrambi i provider.

Le annotazioni possono coesistere poiché un provider ignora tutte le annotazioni che non riconosce. Ad esempio, una colonna chiave primaria che funziona con Microsoft SQL Server e SQLite potrebbe essere simile al seguente.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Se operazioni possono essere applicate solo in un provider (o si tratta in modo diverso tra provider), usare il `ActiveProvider` proprietà per indicare quale provider è attivo.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
