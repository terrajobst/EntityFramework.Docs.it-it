---
title: Uso di un progetto di migrazioni separate-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 0082b0af2905fe9e5c3c6509516f622c9d4f8370
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812039"
---
# <a name="using-a-separate-migrations-project"></a><span data-ttu-id="cd0f6-102">Uso di un progetto di migrazioni separate</span><span class="sxs-lookup"><span data-stu-id="cd0f6-102">Using a Separate Migrations Project</span></span>

<span data-ttu-id="cd0f6-103">Potrebbe essere necessario archiviare le migrazioni in un assembly diverso da quello che contiene il `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="cd0f6-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="cd0f6-104">È anche possibile usare questa strategia per gestire più set di migrazioni, ad esempio uno per lo sviluppo e un altro per gli aggiornamenti di versione a rilascio.</span><span class="sxs-lookup"><span data-stu-id="cd0f6-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="cd0f6-105">Per eseguire questa operazione...</span><span class="sxs-lookup"><span data-stu-id="cd0f6-105">To do this...</span></span>

1. <span data-ttu-id="cd0f6-106">Creare una nuova libreria di classi.</span><span class="sxs-lookup"><span data-stu-id="cd0f6-106">Create a new class library.</span></span>

2. <span data-ttu-id="cd0f6-107">Aggiungere un riferimento all'assembly DbContext.</span><span class="sxs-lookup"><span data-stu-id="cd0f6-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="cd0f6-108">Spostare le migrazioni e i file di snapshot del modello nella libreria di classi.</span><span class="sxs-lookup"><span data-stu-id="cd0f6-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="cd0f6-109">Se non si dispone di migrazioni esistenti, generarne una nel progetto che contiene il DbContext quindi spostarlo.</span><span class="sxs-lookup"><span data-stu-id="cd0f6-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span>
   > <span data-ttu-id="cd0f6-110">Questo è importante perché se l'assembly delle migrazioni non contiene una migrazione esistente, il comando Add-Migration non sarà in grado di trovare DbContext.</span><span class="sxs-lookup"><span data-stu-id="cd0f6-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="cd0f6-111">Configurare l'assembly delle migrazioni:</span><span class="sxs-lookup"><span data-stu-id="cd0f6-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="cd0f6-112">Aggiungere un riferimento all'assembly Migrations dall'assembly di avvio.</span><span class="sxs-lookup"><span data-stu-id="cd0f6-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="cd0f6-113">Se causa una dipendenza circolare, aggiornare il percorso di output della libreria di classi:</span><span class="sxs-lookup"><span data-stu-id="cd0f6-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="cd0f6-114">Se tutte le operazioni sono state effettuate correttamente, dovrebbe essere possibile aggiungere nuove migrazioni al progetto.</span><span class="sxs-lookup"><span data-stu-id="cd0f6-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
