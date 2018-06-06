---
title: .NET core - CLI EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 721235b07e695efd8df43294e1f4e90c28ae83d7
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754496"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="8e0e7-102">Strumenti da riga di comando di EF Core .NET</span><span class="sxs-lookup"><span data-stu-id="8e0e7-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="8e0e7-103">Gli strumenti da riga di comando .NET di Entity Framework Core sono un'estensione della piattaforma incrociata **dotnet** comando, che fa parte di [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="8e0e7-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="8e0e7-104">Se si utilizza Visual Studio, è consigliabile [gli strumenti PMC] [ 1] poiché forniscono invece un'esperienza più integrata.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="8e0e7-105">Installazione degli strumenti</span><span class="sxs-lookup"><span data-stu-id="8e0e7-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="8e0e7-106">.NET Core SDK versione 2.1.300 e include più recente **dotnet ef** comandi che sono compatibili con Entity Framework Core 2.0 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="8e0e7-107">Pertanto se si utilizza le versioni recenti di .NET Core SDK e il runtime di Entity Framework Core, è richiesta alcuna installazione ed è possibile ignorare il resto di questa sezione.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="8e0e7-108">D'altro canto, la **dotnet ef** strumento autonomo nella versione di .NET Core SDK 2.1.300 e più recente non è compatibile con la versione dei componenti di base di Entity Framework 1.0 e 1.1.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="8e0e7-109">Prima di poter utilizzare un progetto che Usa queste versioni precedenti di Entity Framework Core in un computer che dispone di .NET Core SDK 2.1.300 o installata più recente, è necessario installare anche versione 2.1.200 o precedente del SDK e configurare l'applicazione di usare tale versione precedente modificando il relativo  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="8e0e7-110">Questo file è normalmente incluso nella directory della soluzione (uno di sopra del progetto).</span><span class="sxs-lookup"><span data-stu-id="8e0e7-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="8e0e7-111">È quindi possibile procedere con l'istruzione di installazione riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="8e0e7-112">Per le versioni precedenti di .NET Core SDK, è possibile installare gli strumenti della riga di comando di EF Core .NET utilizzando i seguenti passaggi:</span><span class="sxs-lookup"><span data-stu-id="8e0e7-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="8e0e7-113">Modificare il file di progetto e aggiungere Microsoft.EntityFrameworkCore.Tools.DotNet come elemento DotNetCliToolReference (vedere sotto)</span><span class="sxs-lookup"><span data-stu-id="8e0e7-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="8e0e7-114">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="8e0e7-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="8e0e7-115">Il progetto risultante dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="8e0e7-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="8e0e7-116">Un riferimento al pacchetto con `PrivateAssets="All"` significa che non viene esposto ai progetti che fanno riferimento a questo progetto, che risulta particolarmente utile per i pacchetti vengono in genere utilizzati solo durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="8e0e7-117">Se si ha tutti gli elementi a destra, deve essere in grado di eseguire correttamente il comando seguente in un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="8e0e7-118">Utilizzo degli strumenti</span><span class="sxs-lookup"><span data-stu-id="8e0e7-118">Using the tools</span></span>
---------------
<span data-ttu-id="8e0e7-119">Ogni volta che si richiama un comando, vengono utilizzate due progetti:</span><span class="sxs-lookup"><span data-stu-id="8e0e7-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="8e0e7-120">Il progetto di destinazione è dove vengono aggiunti, in alcuni casi rimossi, tutti i file.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="8e0e7-121">Il progetto di destinazione del progetto nella directory corrente per impostazione predefinita, ma può essere modificato utilizzando il <nobr> **-progetto** </nobr> opzione.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="8e0e7-122">Il progetto di avvio è quello emulato dagli strumenti quando si esegue il codice del progetto.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="8e0e7-123">Anche il progetto nella directory corrente per impostazione predefinita, ma può essere modificato utilizzando il **-progetto di avvio** opzione.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="8e0e7-124">Ad esempio, l'aggiornamento del database dell'applicazione web che include Core EF installato in un progetto diverso avrà un aspetto simile al seguente: `dotnet ef database update --project {project-path}` (dalla directory dell'app web)</span><span class="sxs-lookup"><span data-stu-id="8e0e7-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="8e0e7-125">Opzioni comuni:</span><span class="sxs-lookup"><span data-stu-id="8e0e7-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="8e0e7-126">--json</span><span class="sxs-lookup"><span data-stu-id="8e0e7-126">--json</span></span>                           | <span data-ttu-id="8e0e7-127">Mostra l'output JSON.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-127">Show JSON output.</span></span>           |
| <span data-ttu-id="8e0e7-128">-c</span><span class="sxs-lookup"><span data-stu-id="8e0e7-128">-c</span></span> | <span data-ttu-id="8e0e7-129">-contesto \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="8e0e7-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="8e0e7-130">L'elemento DbContext da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="8e0e7-131">-p</span><span class="sxs-lookup"><span data-stu-id="8e0e7-131">-p</span></span> | <span data-ttu-id="8e0e7-132">-progetto \<progetto ></span><span class="sxs-lookup"><span data-stu-id="8e0e7-132">--project \<PROJECT></span></span>             | <span data-ttu-id="8e0e7-133">Il progetto da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-133">The project to use.</span></span>         |
| <span data-ttu-id="8e0e7-134">-s</span><span class="sxs-lookup"><span data-stu-id="8e0e7-134">-s</span></span> | <span data-ttu-id="8e0e7-135">-progetto di avvio \<progetto ></span><span class="sxs-lookup"><span data-stu-id="8e0e7-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="8e0e7-136">Il progetto di avvio da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="8e0e7-137">-framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="8e0e7-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="8e0e7-138">Il framework di destinazione.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-138">The target framework.</span></span>       |
|    | <span data-ttu-id="8e0e7-139">-configurazione \<configurazione ></span><span class="sxs-lookup"><span data-stu-id="8e0e7-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="8e0e7-140">Configurazione da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="8e0e7-141">-runtime \<identificatore ></span><span class="sxs-lookup"><span data-stu-id="8e0e7-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="8e0e7-142">Il runtime da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-142">The runtime to use.</span></span>         |
| <span data-ttu-id="8e0e7-143">-h</span><span class="sxs-lookup"><span data-stu-id="8e0e7-143">-h</span></span> | <span data-ttu-id="8e0e7-144">-Guida in linea</span><span class="sxs-lookup"><span data-stu-id="8e0e7-144">--help</span></span>                           | <span data-ttu-id="8e0e7-145">Mostra le informazioni della Guida.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-145">Show help information.</span></span>      |
| <span data-ttu-id="8e0e7-146">-v</span><span class="sxs-lookup"><span data-stu-id="8e0e7-146">-v</span></span> | <span data-ttu-id="8e0e7-147">-verbose</span><span class="sxs-lookup"><span data-stu-id="8e0e7-147">--verbose</span></span>                        | <span data-ttu-id="8e0e7-148">Mostra l'output dettagliato.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="8e0e7-149">-Nessun colore</span><span class="sxs-lookup"><span data-stu-id="8e0e7-149">--no-color</span></span>                       | <span data-ttu-id="8e0e7-150">Non usa l'output.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="8e0e7-151">--prefix-output</span><span class="sxs-lookup"><span data-stu-id="8e0e7-151">--prefix-output</span></span>                  | <span data-ttu-id="8e0e7-152">Prefisso con livello di output.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="8e0e7-153">Per specificare l'ambiente di ASP.NET Core, impostare il **ASPNETCORE_ENVIRONMENT** variabile di ambiente prima dell'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="8e0e7-154">Comandi:</span><span class="sxs-lookup"><span data-stu-id="8e0e7-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="8e0e7-155">eliminazione del database ef dotnet</span><span class="sxs-lookup"><span data-stu-id="8e0e7-155">dotnet ef database drop</span></span>

<span data-ttu-id="8e0e7-156">Elimina il database.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-156">Drops the database.</span></span>

<span data-ttu-id="8e0e7-157">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="8e0e7-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="8e0e7-158">-f</span><span class="sxs-lookup"><span data-stu-id="8e0e7-158">-f</span></span> | <span data-ttu-id="8e0e7-159">-force</span><span class="sxs-lookup"><span data-stu-id="8e0e7-159">--force</span></span>   | <span data-ttu-id="8e0e7-160">Non verificare.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="8e0e7-161">-esecuzione</span><span class="sxs-lookup"><span data-stu-id="8e0e7-161">--dry-run</span></span> | <span data-ttu-id="8e0e7-162">Mostra il database verrà rimossa, ma non l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="8e0e7-163">aggiornamento del database ef dotnet</span><span class="sxs-lookup"><span data-stu-id="8e0e7-163">dotnet ef database update</span></span>

<span data-ttu-id="8e0e7-164">Aggiorna il database in una migrazione specificata.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="8e0e7-165">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="8e0e7-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="8e0e7-166">\<MIGRAZIONE &GT;</span><span class="sxs-lookup"><span data-stu-id="8e0e7-166">\<MIGRATION></span></span> | <span data-ttu-id="8e0e7-167">La migrazione di destinazione.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-167">The target migration.</span></span> <span data-ttu-id="8e0e7-168">Se è 0, verranno annullate tutte le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="8e0e7-169">Per impostazione predefinita all'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="8e0e7-170">informazioni di dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="8e0e7-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="8e0e7-171">Ottiene informazioni sul tipo DbContext.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="8e0e7-172">elenco di dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="8e0e7-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="8e0e7-173">Elenca i tipi DbContext disponibili.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="8e0e7-174">scaffolding di dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="8e0e7-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="8e0e7-175">Strutture una tipi DbContext ed entità per un database.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="8e0e7-176">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="8e0e7-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="8e0e7-177">\<CONNESSIONE &GT;</span><span class="sxs-lookup"><span data-stu-id="8e0e7-177">\<CONNECTION></span></span> | <span data-ttu-id="8e0e7-178">La stringa di connessione al database.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="8e0e7-179">\<PROVIDER &GT;</span><span class="sxs-lookup"><span data-stu-id="8e0e7-179">\<PROVIDER></span></span>   | <span data-ttu-id="8e0e7-180">Il provider da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-180">The provider to use.</span></span> <span data-ttu-id="8e0e7-181">(ad esempio, Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="8e0e7-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="8e0e7-182">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="8e0e7-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8e0e7-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="8e0e7-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="8e0e7-184">--le annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="8e0e7-184">--data-annotations</span></span>                      | <span data-ttu-id="8e0e7-185">Utilizzare gli attributi per configurare il modello (dove possibile).</span><span class="sxs-lookup"><span data-stu-id="8e0e7-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="8e0e7-186">Se omesso, viene utilizzato solo l'API fluent.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="8e0e7-187">-c</span><span class="sxs-lookup"><span data-stu-id="8e0e7-187">-c</span></span>              | <span data-ttu-id="8e0e7-188">-contesto \<nome ></span><span class="sxs-lookup"><span data-stu-id="8e0e7-188">--context \<NAME></span></span>                       | <span data-ttu-id="8e0e7-189">Il nome di DbContext.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="8e0e7-190">-dir contesto \<percorso ></span><span class="sxs-lookup"><span data-stu-id="8e0e7-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="8e0e7-191">Della directory in cui inserire file DbContext in.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="8e0e7-192">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="8e0e7-193">-f</span><span class="sxs-lookup"><span data-stu-id="8e0e7-193">-f</span></span>              | <span data-ttu-id="8e0e7-194">-force</span><span class="sxs-lookup"><span data-stu-id="8e0e7-194">--force</span></span>                                 | <span data-ttu-id="8e0e7-195">Sovrascrivi file esistenti.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="8e0e7-196">-o</span><span class="sxs-lookup"><span data-stu-id="8e0e7-196">-o</span></span>              | <span data-ttu-id="8e0e7-197">-output-dir \<percorso ></span><span class="sxs-lookup"><span data-stu-id="8e0e7-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="8e0e7-198">Della directory in cui inserire i file in.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-198">The directory to put files in.</span></span> <span data-ttu-id="8e0e7-199">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="8e0e7-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span><span class="sxs-lookup"><span data-stu-id="8e0e7-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="8e0e7-201">Gli schemi delle tabelle per generare i tipi di entità per.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="8e0e7-202">-t</span><span class="sxs-lookup"><span data-stu-id="8e0e7-202">-t</span></span>              | <span data-ttu-id="8e0e7-203">-tabella \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="8e0e7-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="8e0e7-204">Generare tipi di entità per le tabelle.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="8e0e7-205">-Utilizzare nomi di database</span><span class="sxs-lookup"><span data-stu-id="8e0e7-205">--use-database-names</span></span>                    | <span data-ttu-id="8e0e7-206">Utilizzare nomi di tabella e colonna direttamente dal database.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="8e0e7-207">aggiungere le migrazioni di ef dotnet</span><span class="sxs-lookup"><span data-stu-id="8e0e7-207">dotnet ef migrations add</span></span>

<span data-ttu-id="8e0e7-208">Aggiunge una nuova migrazione.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-208">Adds a new migration.</span></span>

<span data-ttu-id="8e0e7-209">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="8e0e7-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="8e0e7-210">\<NOME &GT;</span><span class="sxs-lookup"><span data-stu-id="8e0e7-210">\<NAME></span></span> | <span data-ttu-id="8e0e7-211">Il nome della migrazione.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-211">The name of the migration.</span></span> |

<span data-ttu-id="8e0e7-212">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="8e0e7-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="8e0e7-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="8e0e7-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="8e0e7-214"><nobr>-output-dir \<percorso ></nobr></span><span class="sxs-lookup"><span data-stu-id="8e0e7-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="8e0e7-215">La directory e spazio dei nomi secondario da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="8e0e7-216">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="8e0e7-217">Valore predefinito è "Migrazione".</span><span class="sxs-lookup"><span data-stu-id="8e0e7-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="8e0e7-218">elenco di migrazioni ef dotnet</span><span class="sxs-lookup"><span data-stu-id="8e0e7-218">dotnet ef migrations list</span></span>

<span data-ttu-id="8e0e7-219">Elenca le migrazioni disponibili.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="8e0e7-220">rimuovere le migrazioni di ef dotnet</span><span class="sxs-lookup"><span data-stu-id="8e0e7-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="8e0e7-221">Rimuove l'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-221">Removes the last migration.</span></span>

<span data-ttu-id="8e0e7-222">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="8e0e7-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="8e0e7-223">-f</span><span class="sxs-lookup"><span data-stu-id="8e0e7-223">-f</span></span> | <span data-ttu-id="8e0e7-224">-force</span><span class="sxs-lookup"><span data-stu-id="8e0e7-224">--force</span></span> | <span data-ttu-id="8e0e7-225">Ripristinare la migrazione, se è stato applicato al database.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="8e0e7-226">script di migrazioni ef dotnet</span><span class="sxs-lookup"><span data-stu-id="8e0e7-226">dotnet ef migrations script</span></span>

<span data-ttu-id="8e0e7-227">Genera uno script SQL dalla migrazione.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="8e0e7-228">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="8e0e7-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="8e0e7-229">\<DA &GT;</span><span class="sxs-lookup"><span data-stu-id="8e0e7-229">\<FROM></span></span> | <span data-ttu-id="8e0e7-230">La migrazione inizia.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-230">The starting migration.</span></span> <span data-ttu-id="8e0e7-231">Il valore predefinito 0 (il database iniziale).</span><span class="sxs-lookup"><span data-stu-id="8e0e7-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="8e0e7-232">\<A &GT;</span><span class="sxs-lookup"><span data-stu-id="8e0e7-232">\<TO></span></span>   | <span data-ttu-id="8e0e7-233">La migrazione finale.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-233">The ending migration.</span></span> <span data-ttu-id="8e0e7-234">Per impostazione predefinita all'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="8e0e7-235">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="8e0e7-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="8e0e7-236">-o</span><span class="sxs-lookup"><span data-stu-id="8e0e7-236">-o</span></span> | <span data-ttu-id="8e0e7-237">-output \<FILE ></span><span class="sxs-lookup"><span data-stu-id="8e0e7-237">--output \<FILE></span></span> | <span data-ttu-id="8e0e7-238">File in cui scrivere il risultato.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="8e0e7-239">-i</span><span class="sxs-lookup"><span data-stu-id="8e0e7-239">-i</span></span> | <span data-ttu-id="8e0e7-240">idempotente.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-240">--idempotent</span></span>     | <span data-ttu-id="8e0e7-241">Generare uno script che può essere usato in un database in ogni operazione di migrazione.</span><span class="sxs-lookup"><span data-stu-id="8e0e7-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
