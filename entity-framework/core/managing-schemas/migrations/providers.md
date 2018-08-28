---
title: Migrazioni con più provider - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.openlocfilehash: 7ae695037992323337a780cda29d8c8ed8a13458
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997973"
---
<a name="migrations-with-multiple-providers"></a>Migrazioni con più provider
==================================
Il [Entity Framework Core Tools] [ 1] solo lo scaffolding di migrazioni per il provider attivo. In alcuni casi, tuttavia, è possibile usare più di un provider (ad esempio Microsoft SQL Server e SQLite) con l'oggetto DbContext. Esistono due modi per gestire questo aspetto con le migrazioni. È possibile mantenere due set di migrazioni: uno per ogni provider, ovvero o di tipo merge in un singolo set che possa utilizzare entrambi.

<a name="two-migration-sets"></a>Due set di migrazione
------------------
Il primo approccio, si generano due migrazioni per ogni modifica del modello.

A tal fine ciò consiste nell'inserire ogni set di migrazione [in un assembly separato] [ 2] e cambiare manualmente il provider attivo (e l'assembly di migrazioni) tra l'aggiunta di due migrazioni.

Un altro approccio che consente di utilizzare gli strumenti più semplice consiste nel creare un nuovo tipo che deriva da DbContext ed esegue l'override al provider attivo. Questo tipo viene utilizzato in fase di progettazione quando si aggiunge o applicare le migrazioni di tempo.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Poiché ogni set di migrazione Usa tipi DbContext personalizzati, questo approccio non richiede l'uso di un assembly separato delle migrazioni.

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
> Non è necessario specificare la directory di output per le migrazioni successive perché vengono create come pari livello rispetto a quello più recente.

<a name="one-migration-set"></a>Migrazione di un set
-----------------
Se non sono quelli con due set di migrazioni, è possibile combinarli manualmente in un unico set che può essere applicato a entrambi i provider.

Le annotazioni possono coesistere poiché un provider ignora tutte le annotazioni che non comprende. Ad esempio, una colonna chiave primaria che funziona con Microsoft SQL Server e SQLite potrebbe essere simile al seguente.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Se operazioni possono essere applicate solo in un provider (o in modo diverso sono tra i provider), usare il `ActiveProvider` proprietà per indicare quali provider è attivo.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
