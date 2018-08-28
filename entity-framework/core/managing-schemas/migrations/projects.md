---
title: Migrazioni con progetti multipli - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 76e88dd486b1c53dc69a24e35710511bf9cb673b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997987"
---
<a name="using-a-separate-project"></a><span data-ttu-id="13c05-102">Usando un progetto separato</span><span class="sxs-lookup"><span data-stu-id="13c05-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="13c05-103">È possibile archiviare le migrazioni in un assembly diverso da quello contenente il `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="13c05-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="13c05-104">È anche possibile usare questa strategia per gestire più set di migrazioni, ad esempio, uno per lo sviluppo e altro per gli aggiornamenti di versione di rilascio.</span><span class="sxs-lookup"><span data-stu-id="13c05-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="13c05-105">Per eseguire questa operazione...</span><span class="sxs-lookup"><span data-stu-id="13c05-105">To do this...</span></span>

1. <span data-ttu-id="13c05-106">Creare una nuova libreria di classi.</span><span class="sxs-lookup"><span data-stu-id="13c05-106">Create a new class library.</span></span>

2. <span data-ttu-id="13c05-107">Aggiungere un riferimento all'assembly di DbContext.</span><span class="sxs-lookup"><span data-stu-id="13c05-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="13c05-108">Spostare le migrazioni e i file di snapshot del modello alla libreria di classi.</span><span class="sxs-lookup"><span data-stu-id="13c05-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="13c05-109">Se non si dispone di alcuna migrazione esistente, generarne uno del progetto contenente l'oggetto DbContext, spostarlo.</span><span class="sxs-lookup"><span data-stu-id="13c05-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span> <span data-ttu-id="13c05-110">Questo è importante perché se l'assembly di migrazioni non contiene una migrazione esistente, il comando Add-Migration sarà Impossibile trovare l'oggetto DbContext.</span><span class="sxs-lookup"><span data-stu-id="13c05-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="13c05-111">Configurare l'assembly delle migrazioni:</span><span class="sxs-lookup"><span data-stu-id="13c05-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="13c05-112">Aggiungere un riferimento all'assembly le migrazioni da assembly di avvio.</span><span class="sxs-lookup"><span data-stu-id="13c05-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="13c05-113">Se in questo modo una dipendenza circolare, aggiornare il percorso di output della libreria di classi:</span><span class="sxs-lookup"><span data-stu-id="13c05-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="13c05-114">Se è stato eseguito tutto correttamente, sarà possibile aggiungere le nuove migrazioni al progetto.</span><span class="sxs-lookup"><span data-stu-id="13c05-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
