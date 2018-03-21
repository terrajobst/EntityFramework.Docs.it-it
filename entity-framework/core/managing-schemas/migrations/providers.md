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
<a name="migrations-with-multiple-providers"></a><span data-ttu-id="367aa-102">Migrazioni con più provider</span><span class="sxs-lookup"><span data-stu-id="367aa-102">Migrations with Multiple Providers</span></span>
==================================
<span data-ttu-id="367aa-103">Il [strumenti di base di EF] [ 1] lo scaffolding solo le migrazioni per il provider attivo.</span><span class="sxs-lookup"><span data-stu-id="367aa-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="367aa-104">In alcuni casi, tuttavia, è possibile utilizzare più di un provider (ad esempio Microsoft SQL Server e SQLite) con l'elemento DbContext.</span><span class="sxs-lookup"><span data-stu-id="367aa-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="367aa-105">Esistono due modi per gestire questo comportamento con le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="367aa-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="367aa-106">È possibile mantenere due set di migrazioni: uno per ogni provider-- o di tipo merge in un singolo set che può utilizzare entrambi.</span><span class="sxs-lookup"><span data-stu-id="367aa-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

<a name="two-migration-sets"></a><span data-ttu-id="367aa-107">Due set di migrazione</span><span class="sxs-lookup"><span data-stu-id="367aa-107">Two migration sets</span></span>
------------------
<span data-ttu-id="367aa-108">Il primo approccio è generare due migrazioni per ogni modifica del modello.</span><span class="sxs-lookup"><span data-stu-id="367aa-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="367aa-109">Un modo per eseguire questo consiste nell'inserire ogni insieme di migrazione [in un assembly separato] [ 2] e passare manualmente il provider attivo (e l'assembly di migrazioni) tra l'aggiunta di due migrazioni.</span><span class="sxs-lookup"><span data-stu-id="367aa-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="367aa-110">Un altro approccio che rende più semplice l'uso con gli strumenti di consiste nel creare un nuovo tipo che deriva da di DbContext e sostituisce il provider attivo.</span><span class="sxs-lookup"><span data-stu-id="367aa-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="367aa-111">Questo tipo viene utilizzato in fase di progettazione quando si aggiunge o applicare le migrazioni di tempo.</span><span class="sxs-lookup"><span data-stu-id="367aa-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="367aa-112">Poiché ogni set di migrazione utilizza un proprio i tipi DbContext, questo approccio non richiede l'utilizzo di un assembly separato migrazioni.</span><span class="sxs-lookup"><span data-stu-id="367aa-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="367aa-113">Quando si aggiunge una nuova migrazione, specificare i tipi di contesto.</span><span class="sxs-lookup"><span data-stu-id="367aa-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="367aa-114">Non è necessario specificare la directory di output per migrazioni successive perché vengono create come elementi di pari livello all'ultimo segnalibro.</span><span class="sxs-lookup"><span data-stu-id="367aa-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

<a name="one-migration-set"></a><span data-ttu-id="367aa-115">Set di una migrazione</span><span class="sxs-lookup"><span data-stu-id="367aa-115">One migration set</span></span>
-----------------
<span data-ttu-id="367aa-116">Se non sono quelli desiderati con due set di migrazioni, è possibile combinarle manualmente in un unico gruppo che può essere applicato a entrambi i provider.</span><span class="sxs-lookup"><span data-stu-id="367aa-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="367aa-117">Le annotazioni possono coesistere poiché un provider ignora tutte le annotazioni che non riconosce.</span><span class="sxs-lookup"><span data-stu-id="367aa-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="367aa-118">Ad esempio, una colonna chiave primaria che funziona con Microsoft SQL Server e SQLite potrebbe essere simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="367aa-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="367aa-119">Se operazioni possono essere applicate solo in un provider (o si tratta in modo diverso tra provider), usare il `ActiveProvider` proprietà per indicare quale provider è attivo.</span><span class="sxs-lookup"><span data-stu-id="367aa-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
