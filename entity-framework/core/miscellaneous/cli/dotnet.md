---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: c70003fc7e2c5381a22d26baacd3d76f32489328
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152414"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="dd902-102">Strumenti da riga di comando di EF Core .NET</span><span class="sxs-lookup"><span data-stu-id="dd902-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="dd902-103">Gli strumenti della riga di comando .NET di Entity Framework Core sono un'estensione per il multipiattaforma **dotnet** comando, che fa parte delle [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="dd902-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="dd902-104">Se si usa Visual Studio, è consigliabile [gli strumenti della console di gestione pacchetti] [ 1] ma poiché forniscono un'esperienza più integrata.</span><span class="sxs-lookup"><span data-stu-id="dd902-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="dd902-105">Installazione degli strumenti</span><span class="sxs-lookup"><span data-stu-id="dd902-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="dd902-106">.NET Core SDK 2.1.300 versione e include più recente **dotnet ef** comandi che sono compatibili con EF Core 2.0 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="dd902-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="dd902-107">Se si usa le versioni recenti di .NET Core SDK e runtime di Entity Framework Core, pertanto è richiesta alcuna installazione ed è possibile ignorare il resto di questa sezione.</span><span class="sxs-lookup"><span data-stu-id="dd902-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="dd902-108">D'altra parte, il **dotnet ef** strumento contenuto nella versione di .NET Core SDK 2.1.300 e versioni successiva non è compatibile con la versione di EF Core 1.0 e 1.1.</span><span class="sxs-lookup"><span data-stu-id="dd902-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="dd902-109">Prima di poter utilizzare un progetto che Usa queste versioni precedenti di EF Core in un computer che dispone di .NET Core SDK 2.1.300 installati o versioni successive, è necessario installare anche versione 2.1.200 o precedente del SDK e configurare l'applicazione per usare tale versione precedente, modificando il  [Global. JSON](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span><span class="sxs-lookup"><span data-stu-id="dd902-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="dd902-110">In genere, questo file è incluso nella directory della soluzione (quello precedente del progetto).</span><span class="sxs-lookup"><span data-stu-id="dd902-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="dd902-111">È quindi possibile procedere con le istruzioni di installazione riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="dd902-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="dd902-112">Per le versioni precedenti di .NET Core SDK, è possibile installare gli strumenti della riga di comando di EF Core .NET attenendosi alla procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="dd902-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="dd902-113">Modificare il file di progetto e aggiungere l'entityframeworkcore come elemento DotNetCliToolReference (vedere sotto)</span><span class="sxs-lookup"><span data-stu-id="dd902-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="dd902-114">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="dd902-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="dd902-115">Il progetto risultante dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="dd902-115">The resulting project should look something like this:</span></span>

``` xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                      Version="2.0.0"
                      PrivateAssets="All" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                            Version="2.0.0" />
  </ItemGroup>
</Project>
```

> [!NOTE]
> <span data-ttu-id="dd902-116">Un riferimento al pacchetto con `PrivateAssets="All"` significa che non viene esposto ai progetti che fanno riferimento a questo progetto, che è particolarmente utile per i pacchetti che in genere vengono usati solo durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="dd902-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="dd902-117">Se si ha tutto per bene, deve essere in grado di eseguire correttamente il comando seguente in un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="dd902-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="dd902-118">Usando gli strumenti</span><span class="sxs-lookup"><span data-stu-id="dd902-118">Using the tools</span></span>
---------------
<span data-ttu-id="dd902-119">Ogni volta che si richiama un comando, sono disponibili due progetti:</span><span class="sxs-lookup"><span data-stu-id="dd902-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="dd902-120">Il progetto di destinazione è dove vengono aggiunti, in alcuni casi rimossi, tutti i file.</span><span class="sxs-lookup"><span data-stu-id="dd902-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="dd902-121">Il progetto di destinazione per impostazione predefinita per il progetto nella directory corrente, ma può essere modificato utilizzando la <nobr> **`--project`** </nobr> opzione.</span><span class="sxs-lookup"><span data-stu-id="dd902-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**`--project`**</nobr> option.</span></span>

<span data-ttu-id="dd902-122">Il progetto di avvio è quello emulato dagli strumenti quando si esegue il codice del progetto.</span><span class="sxs-lookup"><span data-stu-id="dd902-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="dd902-123">Anche il progetto nella directory corrente per impostazione predefinita, ma può essere modificato utilizzando la **`--startup-project`** opzione.</span><span class="sxs-lookup"><span data-stu-id="dd902-123">It also defaults to the project in the current directory, but can be changed using the **`--startup-project`** option.</span></span>

> [!NOTE]
> <span data-ttu-id="dd902-124">L'aggiornamento del database dell'applicazione web con EF Core installato in un progetto diverso, ad esempio, si presenta come segue: `dotnet ef database update --project {project-path}` (dalla directory dell'app web)</span><span class="sxs-lookup"><span data-stu-id="dd902-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="dd902-125">Opzioni comuni:</span><span class="sxs-lookup"><span data-stu-id="dd902-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | `--json`                           | <span data-ttu-id="dd902-126">Mostra l'output JSON.</span><span class="sxs-lookup"><span data-stu-id="dd902-126">Show JSON output.</span></span>           |
| <span data-ttu-id="dd902-127">-c</span><span class="sxs-lookup"><span data-stu-id="dd902-127">-c</span></span> | `--context <DBCONTEXT>`           | <span data-ttu-id="dd902-128">DbContext da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="dd902-128">The DbContext to use.</span></span>       |
| <span data-ttu-id="dd902-129">-p</span><span class="sxs-lookup"><span data-stu-id="dd902-129">-p</span></span> | `--project <PROJECT>`             | <span data-ttu-id="dd902-130">Il progetto da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="dd902-130">The project to use.</span></span>         |
| <span data-ttu-id="dd902-131">-s</span><span class="sxs-lookup"><span data-stu-id="dd902-131">-s</span></span> | `--startup-project <PROJECT>`     | <span data-ttu-id="dd902-132">Il progetto di avvio da usare.</span><span class="sxs-lookup"><span data-stu-id="dd902-132">The startup project to use.</span></span> |
|    | `--framework <FRAMEWORK>`         | <span data-ttu-id="dd902-133">Il framework di destinazione.</span><span class="sxs-lookup"><span data-stu-id="dd902-133">The target framework.</span></span>       |
|    | `--configuration <CONFIGURATION>` | <span data-ttu-id="dd902-134">Configurazione da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="dd902-134">The configuration to use.</span></span>   |
|    | `--runtime <IDENTIFIER>`          | <span data-ttu-id="dd902-135">Usare il runtime.</span><span class="sxs-lookup"><span data-stu-id="dd902-135">The runtime to use.</span></span>         |
| <span data-ttu-id="dd902-136">-h</span><span class="sxs-lookup"><span data-stu-id="dd902-136">-h</span></span> | `--help`                           | <span data-ttu-id="dd902-137">Mostra le informazioni della Guida.</span><span class="sxs-lookup"><span data-stu-id="dd902-137">Show help information.</span></span>      |
| <span data-ttu-id="dd902-138">-v</span><span class="sxs-lookup"><span data-stu-id="dd902-138">-v</span></span> | `--verbose`                        | <span data-ttu-id="dd902-139">Visualizzare output dettagliato.</span><span class="sxs-lookup"><span data-stu-id="dd902-139">Show verbose output.</span></span>        |
|    | `--no-color`                       | <span data-ttu-id="dd902-140">Non leggano l'output.</span><span class="sxs-lookup"><span data-stu-id="dd902-140">Don't colorize output.</span></span>      |
|    | `--prefix-output`                  | <span data-ttu-id="dd902-141">Prefisso con il livello di output.</span><span class="sxs-lookup"><span data-stu-id="dd902-141">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="dd902-142">Per specificare l'ambiente di ASP.NET Core, impostare il **ASPNETCORE_ENVIRONMENT** variabile di ambiente prima dell'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="dd902-142">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="dd902-143">Comandi:</span><span class="sxs-lookup"><span data-stu-id="dd902-143">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="dd902-144">eliminazione del database ef dotnet</span><span class="sxs-lookup"><span data-stu-id="dd902-144">dotnet ef database drop</span></span>

<span data-ttu-id="dd902-145">Elimina il database.</span><span class="sxs-lookup"><span data-stu-id="dd902-145">Drops the database.</span></span>

<span data-ttu-id="dd902-146">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="dd902-146">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="dd902-147">-f</span><span class="sxs-lookup"><span data-stu-id="dd902-147">-f</span></span> | `--force`   | <span data-ttu-id="dd902-148">Non verificare.</span><span class="sxs-lookup"><span data-stu-id="dd902-148">Don't confirm.</span></span>                                           |
|    | `--dry-run` | <span data-ttu-id="dd902-149">Mostra in quale database verrà rimossa, ma non eliminarlo.</span><span class="sxs-lookup"><span data-stu-id="dd902-149">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="dd902-150">aggiornamento del database ef dotnet</span><span class="sxs-lookup"><span data-stu-id="dd902-150">dotnet ef database update</span></span>

<span data-ttu-id="dd902-151">Aggiorna il database per una migrazione specificata.</span><span class="sxs-lookup"><span data-stu-id="dd902-151">Updates the database to a specified migration.</span></span>

<span data-ttu-id="dd902-152">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="dd902-152">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| `<MIGRATION>` | <span data-ttu-id="dd902-153">La migrazione di destinazione.</span><span class="sxs-lookup"><span data-stu-id="dd902-153">The target migration.</span></span> <span data-ttu-id="dd902-154">Se è 0, verranno ripristinate tutte le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="dd902-154">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="dd902-155">Per impostazione predefinita all'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="dd902-155">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="dd902-156">informazioni sull'oggetto dbcontext di Entity Framework di dotnet</span><span class="sxs-lookup"><span data-stu-id="dd902-156">dotnet ef dbcontext info</span></span>

<span data-ttu-id="dd902-157">Ottiene informazioni su un tipo DbContext.</span><span class="sxs-lookup"><span data-stu-id="dd902-157">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="dd902-158">elenco di dbcontext di Entity Framework dotnet</span><span class="sxs-lookup"><span data-stu-id="dd902-158">dotnet ef dbcontext list</span></span>

<span data-ttu-id="dd902-159">Elenca i tipi DbContext disponibili.</span><span class="sxs-lookup"><span data-stu-id="dd902-159">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="dd902-160">scaffolding dbcontext di Entity Framework di dotnet</span><span class="sxs-lookup"><span data-stu-id="dd902-160">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="dd902-161">Esegue lo scaffolding di un tipi DbContext ed entità per un database.</span><span class="sxs-lookup"><span data-stu-id="dd902-161">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="dd902-162">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="dd902-162">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| `<CONNECTION>` | <span data-ttu-id="dd902-163">La stringa di connessione al database.</span><span class="sxs-lookup"><span data-stu-id="dd902-163">The connection string to the database.</span></span>                                      |
| `<PROVIDER>`   | <span data-ttu-id="dd902-164">Il provider da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="dd902-164">The provider to use.</span></span> <span data-ttu-id="dd902-165">(ad esempio, l'entityframeworkcore)</span><span class="sxs-lookup"><span data-stu-id="dd902-165">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="dd902-166">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="dd902-166">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="dd902-167"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="dd902-167"><nobr>-d</nobr></span></span> | `--data-annotations`                      | <span data-ttu-id="dd902-168">Usare gli attributi per configurare il modello (se possibile).</span><span class="sxs-lookup"><span data-stu-id="dd902-168">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="dd902-169">Se omesso, viene utilizzato solo l'API fluent.</span><span class="sxs-lookup"><span data-stu-id="dd902-169">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="dd902-170">-c</span><span class="sxs-lookup"><span data-stu-id="dd902-170">-c</span></span>              | `--context <NAME>`                       | <span data-ttu-id="dd902-171">Il nome di DbContext.</span><span class="sxs-lookup"><span data-stu-id="dd902-171">The name of the DbContext.</span></span>                                                                       |
|                 | `--context-dir <PATH>`                   | <span data-ttu-id="dd902-172">La directory in cui inserire file DbContext in.</span><span class="sxs-lookup"><span data-stu-id="dd902-172">The directory to put DbContext file in.</span></span> <span data-ttu-id="dd902-173">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="dd902-173">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="dd902-174">-f</span><span class="sxs-lookup"><span data-stu-id="dd902-174">-f</span></span>              | `--force`                                 | <span data-ttu-id="dd902-175">Sovrascrivi file esistenti.</span><span class="sxs-lookup"><span data-stu-id="dd902-175">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="dd902-176">-o</span><span class="sxs-lookup"><span data-stu-id="dd902-176">-o</span></span>              | `--output-dir <PATH>`                    | <span data-ttu-id="dd902-177">La directory in cui inserire file nella.</span><span class="sxs-lookup"><span data-stu-id="dd902-177">The directory to put files in.</span></span> <span data-ttu-id="dd902-178">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="dd902-178">Paths are relative to the project directory.</span></span>                      |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | <span data-ttu-id="dd902-179">Gli schemi delle tabelle per generare i tipi di entità per.</span><span class="sxs-lookup"><span data-stu-id="dd902-179">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="dd902-180">-t</span><span class="sxs-lookup"><span data-stu-id="dd902-180">-t</span></span>              | <span data-ttu-id="dd902-181">`--table <TABLE_NAME>`...</span><span class="sxs-lookup"><span data-stu-id="dd902-181">`--table <TABLE_NAME>`...</span></span>                | <span data-ttu-id="dd902-182">Generare tipi di entità per le tabelle.</span><span class="sxs-lookup"><span data-stu-id="dd902-182">The tables to generate entity types for.</span></span>                                                         |
|                 | `--use-database-names`                    | <span data-ttu-id="dd902-183">Usare nomi di tabella e colonna direttamente dal database.</span><span class="sxs-lookup"><span data-stu-id="dd902-183">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="dd902-184">aggiungere dotnet migrazioni di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="dd902-184">dotnet ef migrations add</span></span>

<span data-ttu-id="dd902-185">Aggiunge una nuova migrazione.</span><span class="sxs-lookup"><span data-stu-id="dd902-185">Adds a new migration.</span></span>

<span data-ttu-id="dd902-186">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="dd902-186">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| `<NAME>` | <span data-ttu-id="dd902-187">Il nome della migrazione.</span><span class="sxs-lookup"><span data-stu-id="dd902-187">The name of the migration.</span></span> |

<span data-ttu-id="dd902-188">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="dd902-188">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="dd902-189"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="dd902-189"><nobr>-o</nobr></span></span> | <span data-ttu-id="dd902-190"><nobr> `--output-dir <PATH>` </nobr></span><span class="sxs-lookup"><span data-stu-id="dd902-190"><nobr> `--output-dir <PATH>` </nobr></span></span> | <span data-ttu-id="dd902-191">La directory e sub-spazio dei nomi da usare.</span><span class="sxs-lookup"><span data-stu-id="dd902-191">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="dd902-192">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="dd902-192">Paths are relative to the project directory.</span></span> <span data-ttu-id="dd902-193">Il valore predefinito è "Migrazione".</span><span class="sxs-lookup"><span data-stu-id="dd902-193">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="dd902-194">elenco di dotnet ef migrations</span><span class="sxs-lookup"><span data-stu-id="dd902-194">dotnet ef migrations list</span></span>

<span data-ttu-id="dd902-195">Elenca le migrazioni disponibili.</span><span class="sxs-lookup"><span data-stu-id="dd902-195">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="dd902-196">rimuovere le migrazioni di Entity Framework di dotnet</span><span class="sxs-lookup"><span data-stu-id="dd902-196">dotnet ef migrations remove</span></span>

<span data-ttu-id="dd902-197">Rimuove l'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="dd902-197">Removes the last migration.</span></span>

<span data-ttu-id="dd902-198">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="dd902-198">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="dd902-199">-f</span><span class="sxs-lookup"><span data-stu-id="dd902-199">-f</span></span> | `--force` | <span data-ttu-id="dd902-200">Se è stato applicato al database, ripristinare la migrazione.</span><span class="sxs-lookup"><span data-stu-id="dd902-200">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="dd902-201">script di dotnet ef migrations</span><span class="sxs-lookup"><span data-stu-id="dd902-201">dotnet ef migrations script</span></span>

<span data-ttu-id="dd902-202">Genera uno script SQL dalle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="dd902-202">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="dd902-203">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="dd902-203">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| `<FROM>` | <span data-ttu-id="dd902-204">La migrazione inizia.</span><span class="sxs-lookup"><span data-stu-id="dd902-204">The starting migration.</span></span> <span data-ttu-id="dd902-205">Il valore predefinito è 0 (il database iniziale).</span><span class="sxs-lookup"><span data-stu-id="dd902-205">Defaults to 0 (the initial database).</span></span> |
| `<TO>`   | <span data-ttu-id="dd902-206">La migrazione finale.</span><span class="sxs-lookup"><span data-stu-id="dd902-206">The ending migration.</span></span> <span data-ttu-id="dd902-207">Per impostazione predefinita all'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="dd902-207">Defaults to the last migration.</span></span>         |

<span data-ttu-id="dd902-208">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="dd902-208">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="dd902-209">-o</span><span class="sxs-lookup"><span data-stu-id="dd902-209">-o</span></span> | `--output <FILE>` | <span data-ttu-id="dd902-210">File in cui scrivere il risultato.</span><span class="sxs-lookup"><span data-stu-id="dd902-210">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="dd902-211">-i</span><span class="sxs-lookup"><span data-stu-id="dd902-211">-i</span></span> | `--idempotent`     | <span data-ttu-id="dd902-212">Generare uno script che può essere utilizzato in un database in ogni operazione di migrazione.</span><span class="sxs-lookup"><span data-stu-id="dd902-212">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
