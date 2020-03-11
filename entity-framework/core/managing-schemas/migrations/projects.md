---
title: Uso di un progetto di migrazioni separate-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 89b7f50fe750c2953aa75efcdffcb1a5199ce90c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416815"
---
# <a name="using-a-separate-migrations-project"></a><span data-ttu-id="597f1-102">Uso di un progetto di migrazioni separate</span><span class="sxs-lookup"><span data-stu-id="597f1-102">Using a Separate Migrations Project</span></span>

<span data-ttu-id="597f1-103">Potrebbe essere necessario archiviare le migrazioni in un assembly diverso da quello che contiene il `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="597f1-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="597f1-104">È anche possibile usare questa strategia per gestire più set di migrazioni, ad esempio uno per lo sviluppo e un altro per gli aggiornamenti di versione a rilascio.</span><span class="sxs-lookup"><span data-stu-id="597f1-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="597f1-105">Per</span><span class="sxs-lookup"><span data-stu-id="597f1-105">To do this...</span></span>

1. <span data-ttu-id="597f1-106">Creare una nuova libreria di classi.</span><span class="sxs-lookup"><span data-stu-id="597f1-106">Create a new class library.</span></span>

2. <span data-ttu-id="597f1-107">Aggiungere un riferimento all'assembly DbContext.</span><span class="sxs-lookup"><span data-stu-id="597f1-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="597f1-108">Spostare le migrazioni e i file di snapshot del modello nella libreria di classi.</span><span class="sxs-lookup"><span data-stu-id="597f1-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="597f1-109">Se non si dispone di migrazioni esistenti, generarne una nel progetto che contiene il DbContext quindi spostarlo.</span><span class="sxs-lookup"><span data-stu-id="597f1-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span>
   > <span data-ttu-id="597f1-110">Questo è importante perché se l'assembly delle migrazioni non contiene una migrazione esistente, il comando Add-Migration non sarà in grado di trovare DbContext.</span><span class="sxs-lookup"><span data-stu-id="597f1-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="597f1-111">Configurare l'assembly delle migrazioni:</span><span class="sxs-lookup"><span data-stu-id="597f1-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="597f1-112">Aggiungere un riferimento all'assembly Migrations dall'assembly di avvio.</span><span class="sxs-lookup"><span data-stu-id="597f1-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="597f1-113">Se causa una dipendenza circolare, aggiornare il percorso di output della libreria di classi:</span><span class="sxs-lookup"><span data-stu-id="597f1-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="597f1-114">Se tutte le operazioni sono state effettuate correttamente, dovrebbe essere possibile aggiungere nuove migrazioni al progetto.</span><span class="sxs-lookup"><span data-stu-id="597f1-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

## <a name="net-core-cli"></a>[<span data-ttu-id="597f1-115">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="597f1-115">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add NewMigration --project MyApp.Migrations
```

## <a name="visual-studio"></a>[<span data-ttu-id="597f1-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="597f1-116">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

***
