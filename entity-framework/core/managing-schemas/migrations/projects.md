---
title: "Migrazioni di più progetti - Core a Entity Framework"
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: e059deb6c7555a4f6732fe7942855e95b3f9a34c
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
<a name="using-a-separate-project"></a><span data-ttu-id="354f6-102">Utilizzo di un progetto separato</span><span class="sxs-lookup"><span data-stu-id="354f6-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="354f6-103">Si consiglia di archiviare le migrazioni in un assembly diverso da quello contenente il `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="354f6-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="354f6-104">È anche possibile utilizzare questa strategia per gestire più set di migrazioni, ad esempio, una per lo sviluppo e un'altra per gli aggiornamenti di versione di rilascio.</span><span class="sxs-lookup"><span data-stu-id="354f6-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="354f6-105">Per eseguire questa operazione...</span><span class="sxs-lookup"><span data-stu-id="354f6-105">To do this...</span></span>

1. <span data-ttu-id="354f6-106">Creare una nuova libreria di classi.</span><span class="sxs-lookup"><span data-stu-id="354f6-106">Create a new class library.</span></span>

2. <span data-ttu-id="354f6-107">Aggiungere un riferimento all'assembly DbContext.</span><span class="sxs-lookup"><span data-stu-id="354f6-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="354f6-108">Spostare i file di snapshot di modello e le migrazioni alla libreria di classi.</span><span class="sxs-lookup"><span data-stu-id="354f6-108">Move the migrations and model snapshot files to the class library.</span></span>
   * <span data-ttu-id="354f6-109">Se non sono stati aggiunti, aggiungerne uno al progetto DbContext quindi spostarlo.</span><span class="sxs-lookup"><span data-stu-id="354f6-109">If you haven't added any, add one to the DbContext project then move it.</span></span>

4. <span data-ttu-id="354f6-110">Configurare l'assembly di migrazioni:</span><span class="sxs-lookup"><span data-stu-id="354f6-110">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="354f6-111">Aggiungere un riferimento all'assembly le migrazioni da assembly di avvio.</span><span class="sxs-lookup"><span data-stu-id="354f6-111">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="354f6-112">Se in questo modo una dipendenza circolare, aggiornare il percorso di output della libreria di classi:</span><span class="sxs-lookup"><span data-stu-id="354f6-112">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="354f6-113">Se sono state eseguite tutte le operazioni in modo corretto, sarà possibile aggiungere le migrazioni di nuovo al progetto.</span><span class="sxs-lookup"><span data-stu-id="354f6-113">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migraitons add NewMigration --project MyApp.Migrations
```
