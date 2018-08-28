---
title: .NET core CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 81427b010f63bdd591ffb9117c1556722313c3fa
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995297"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="18b2b-102">Strumenti da riga di comando di EF Core .NET</span><span class="sxs-lookup"><span data-stu-id="18b2b-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="18b2b-103">Gli strumenti della riga di comando .NET di Entity Framework Core sono un'estensione per il multipiattaforma **dotnet** comando, che fa parte delle [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="18b2b-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="18b2b-104">Se si usa Visual Studio, è consigliabile [gli strumenti della console di gestione pacchetti] [ 1] ma poiché forniscono un'esperienza più integrata.</span><span class="sxs-lookup"><span data-stu-id="18b2b-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="18b2b-105">Installazione degli strumenti</span><span class="sxs-lookup"><span data-stu-id="18b2b-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="18b2b-106">.NET Core SDK 2.1.300 versione e include più recente **dotnet ef** comandi che sono compatibili con EF Core 2.0 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="18b2b-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="18b2b-107">Se si usa le versioni recenti di .NET Core SDK e runtime di Entity Framework Core, pertanto è richiesta alcuna installazione ed è possibile ignorare il resto di questa sezione.</span><span class="sxs-lookup"><span data-stu-id="18b2b-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="18b2b-108">D'altra parte, il **dotnet ef** strumento contenuto nella versione di .NET Core SDK 2.1.300 e versioni successiva non è compatibile con la versione di EF Core 1.0 e 1.1.</span><span class="sxs-lookup"><span data-stu-id="18b2b-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="18b2b-109">Prima di poter utilizzare un progetto che Usa queste versioni precedenti di EF Core in un computer che dispone di .NET Core SDK 2.1.300 installati o versioni successive, è necessario installare anche versione 2.1.200 o precedente del SDK e configurare l'applicazione per usare tale versione precedente, modificando il  [Global. JSON](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span><span class="sxs-lookup"><span data-stu-id="18b2b-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="18b2b-110">In genere, questo file è incluso nella directory della soluzione (quello precedente del progetto).</span><span class="sxs-lookup"><span data-stu-id="18b2b-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="18b2b-111">È quindi possibile procedere con le istruzioni di installazione riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="18b2b-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="18b2b-112">Per le versioni precedenti di .NET Core SDK, è possibile installare gli strumenti della riga di comando di EF Core .NET attenendosi alla procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="18b2b-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="18b2b-113">Modificare il file di progetto e aggiungere l'entityframeworkcore come elemento DotNetCliToolReference (vedere sotto)</span><span class="sxs-lookup"><span data-stu-id="18b2b-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="18b2b-114">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="18b2b-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="18b2b-115">Il progetto risultante dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="18b2b-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="18b2b-116">Un riferimento al pacchetto con `PrivateAssets="All"` significa che non viene esposto ai progetti che fanno riferimento a questo progetto, che è particolarmente utile per i pacchetti che in genere vengono usati solo durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="18b2b-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="18b2b-117">Se si ha tutto per bene, deve essere in grado di eseguire correttamente il comando seguente in un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="18b2b-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="18b2b-118">Usando gli strumenti</span><span class="sxs-lookup"><span data-stu-id="18b2b-118">Using the tools</span></span>
---------------
<span data-ttu-id="18b2b-119">Ogni volta che si richiama un comando, sono disponibili due progetti:</span><span class="sxs-lookup"><span data-stu-id="18b2b-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="18b2b-120">Il progetto di destinazione è dove vengono aggiunti, in alcuni casi rimossi, tutti i file.</span><span class="sxs-lookup"><span data-stu-id="18b2b-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="18b2b-121">Il progetto di destinazione per impostazione predefinita per il progetto nella directory corrente, ma può essere modificato utilizzando la <nobr> **-project** </nobr> opzione.</span><span class="sxs-lookup"><span data-stu-id="18b2b-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="18b2b-122">Il progetto di avvio è quello emulato dagli strumenti quando si esegue il codice del progetto.</span><span class="sxs-lookup"><span data-stu-id="18b2b-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="18b2b-123">Anche il progetto nella directory corrente per impostazione predefinita, ma può essere modificato utilizzando la **-progetto di avvio** opzione.</span><span class="sxs-lookup"><span data-stu-id="18b2b-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="18b2b-124">L'aggiornamento del database dell'applicazione web con EF Core installato in un progetto diverso, ad esempio, si presenta come segue: `dotnet ef database update --project {project-path}` (dalla directory dell'app web)</span><span class="sxs-lookup"><span data-stu-id="18b2b-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="18b2b-125">Opzioni comuni:</span><span class="sxs-lookup"><span data-stu-id="18b2b-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="18b2b-126">--json</span><span class="sxs-lookup"><span data-stu-id="18b2b-126">--json</span></span>                           | <span data-ttu-id="18b2b-127">Mostra l'output JSON.</span><span class="sxs-lookup"><span data-stu-id="18b2b-127">Show JSON output.</span></span>           |
| <span data-ttu-id="18b2b-128">-c</span><span class="sxs-lookup"><span data-stu-id="18b2b-128">-c</span></span> | <span data-ttu-id="18b2b-129">-contesto \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="18b2b-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="18b2b-130">DbContext da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="18b2b-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="18b2b-131">-p</span><span class="sxs-lookup"><span data-stu-id="18b2b-131">-p</span></span> | <span data-ttu-id="18b2b-132">-progetto \<progetto ></span><span class="sxs-lookup"><span data-stu-id="18b2b-132">--project \<PROJECT></span></span>             | <span data-ttu-id="18b2b-133">Il progetto da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="18b2b-133">The project to use.</span></span>         |
| <span data-ttu-id="18b2b-134">-s</span><span class="sxs-lookup"><span data-stu-id="18b2b-134">-s</span></span> | <span data-ttu-id="18b2b-135">-progetto di avvio \<progetto ></span><span class="sxs-lookup"><span data-stu-id="18b2b-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="18b2b-136">Il progetto di avvio da usare.</span><span class="sxs-lookup"><span data-stu-id="18b2b-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="18b2b-137">-framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="18b2b-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="18b2b-138">Il framework di destinazione.</span><span class="sxs-lookup"><span data-stu-id="18b2b-138">The target framework.</span></span>       |
|    | <span data-ttu-id="18b2b-139">-configurazione \<CONFIGURATION ></span><span class="sxs-lookup"><span data-stu-id="18b2b-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="18b2b-140">Configurazione da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="18b2b-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="18b2b-141">-runtime \<identificatore ></span><span class="sxs-lookup"><span data-stu-id="18b2b-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="18b2b-142">Usare il runtime.</span><span class="sxs-lookup"><span data-stu-id="18b2b-142">The runtime to use.</span></span>         |
| <span data-ttu-id="18b2b-143">-h</span><span class="sxs-lookup"><span data-stu-id="18b2b-143">-h</span></span> | <span data-ttu-id="18b2b-144">-Guida</span><span class="sxs-lookup"><span data-stu-id="18b2b-144">--help</span></span>                           | <span data-ttu-id="18b2b-145">Mostra le informazioni della Guida.</span><span class="sxs-lookup"><span data-stu-id="18b2b-145">Show help information.</span></span>      |
| <span data-ttu-id="18b2b-146">-v</span><span class="sxs-lookup"><span data-stu-id="18b2b-146">-v</span></span> | <span data-ttu-id="18b2b-147">-verbose</span><span class="sxs-lookup"><span data-stu-id="18b2b-147">--verbose</span></span>                        | <span data-ttu-id="18b2b-148">Visualizzare output dettagliato.</span><span class="sxs-lookup"><span data-stu-id="18b2b-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="18b2b-149">-Nessun colore</span><span class="sxs-lookup"><span data-stu-id="18b2b-149">--no-color</span></span>                       | <span data-ttu-id="18b2b-150">Non leggano l'output.</span><span class="sxs-lookup"><span data-stu-id="18b2b-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="18b2b-151">--prefix-output</span><span class="sxs-lookup"><span data-stu-id="18b2b-151">--prefix-output</span></span>                  | <span data-ttu-id="18b2b-152">Prefisso con il livello di output.</span><span class="sxs-lookup"><span data-stu-id="18b2b-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="18b2b-153">Per specificare l'ambiente di ASP.NET Core, impostare il **ASPNETCORE_ENVIRONMENT** variabile di ambiente prima dell'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="18b2b-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="18b2b-154">Comandi:</span><span class="sxs-lookup"><span data-stu-id="18b2b-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="18b2b-155">eliminazione del database ef dotnet</span><span class="sxs-lookup"><span data-stu-id="18b2b-155">dotnet ef database drop</span></span>

<span data-ttu-id="18b2b-156">Elimina il database.</span><span class="sxs-lookup"><span data-stu-id="18b2b-156">Drops the database.</span></span>

<span data-ttu-id="18b2b-157">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="18b2b-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="18b2b-158">-f</span><span class="sxs-lookup"><span data-stu-id="18b2b-158">-f</span></span> | <span data-ttu-id="18b2b-159">-force</span><span class="sxs-lookup"><span data-stu-id="18b2b-159">--force</span></span>   | <span data-ttu-id="18b2b-160">Non verificare.</span><span class="sxs-lookup"><span data-stu-id="18b2b-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="18b2b-161">-dry-run</span><span class="sxs-lookup"><span data-stu-id="18b2b-161">--dry-run</span></span> | <span data-ttu-id="18b2b-162">Mostra in quale database verrà rimossa, ma non eliminarlo.</span><span class="sxs-lookup"><span data-stu-id="18b2b-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="18b2b-163">aggiornamento del database ef dotnet</span><span class="sxs-lookup"><span data-stu-id="18b2b-163">dotnet ef database update</span></span>

<span data-ttu-id="18b2b-164">Aggiorna il database per una migrazione specificata.</span><span class="sxs-lookup"><span data-stu-id="18b2b-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="18b2b-165">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="18b2b-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="18b2b-166">\<MIGRAZIONE &GT;</span><span class="sxs-lookup"><span data-stu-id="18b2b-166">\<MIGRATION></span></span> | <span data-ttu-id="18b2b-167">La migrazione di destinazione.</span><span class="sxs-lookup"><span data-stu-id="18b2b-167">The target migration.</span></span> <span data-ttu-id="18b2b-168">Se è 0, verranno ripristinate tutte le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="18b2b-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="18b2b-169">Per impostazione predefinita all'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="18b2b-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="18b2b-170">informazioni sull'oggetto dbcontext di Entity Framework di dotnet</span><span class="sxs-lookup"><span data-stu-id="18b2b-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="18b2b-171">Ottiene informazioni su un tipo DbContext.</span><span class="sxs-lookup"><span data-stu-id="18b2b-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="18b2b-172">elenco di dbcontext di Entity Framework dotnet</span><span class="sxs-lookup"><span data-stu-id="18b2b-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="18b2b-173">Elenca i tipi DbContext disponibili.</span><span class="sxs-lookup"><span data-stu-id="18b2b-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="18b2b-174">scaffolding dbcontext di Entity Framework di dotnet</span><span class="sxs-lookup"><span data-stu-id="18b2b-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="18b2b-175">Esegue lo scaffolding di un tipi DbContext ed entità per un database.</span><span class="sxs-lookup"><span data-stu-id="18b2b-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="18b2b-176">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="18b2b-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="18b2b-177">\<CONNESSIONE &GT;</span><span class="sxs-lookup"><span data-stu-id="18b2b-177">\<CONNECTION></span></span> | <span data-ttu-id="18b2b-178">La stringa di connessione al database.</span><span class="sxs-lookup"><span data-stu-id="18b2b-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="18b2b-179">\<PROVIDER &GT;</span><span class="sxs-lookup"><span data-stu-id="18b2b-179">\<PROVIDER></span></span>   | <span data-ttu-id="18b2b-180">Il provider da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="18b2b-180">The provider to use.</span></span> <span data-ttu-id="18b2b-181">(ad esempio, l'entityframeworkcore)</span><span class="sxs-lookup"><span data-stu-id="18b2b-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="18b2b-182">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="18b2b-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="18b2b-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="18b2b-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="18b2b-184">: le annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="18b2b-184">--data-annotations</span></span>                      | <span data-ttu-id="18b2b-185">Usare gli attributi per configurare il modello (se possibile).</span><span class="sxs-lookup"><span data-stu-id="18b2b-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="18b2b-186">Se omesso, viene utilizzato solo l'API fluent.</span><span class="sxs-lookup"><span data-stu-id="18b2b-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="18b2b-187">-c</span><span class="sxs-lookup"><span data-stu-id="18b2b-187">-c</span></span>              | <span data-ttu-id="18b2b-188">-contesto \<nome ></span><span class="sxs-lookup"><span data-stu-id="18b2b-188">--context \<NAME></span></span>                       | <span data-ttu-id="18b2b-189">Il nome di DbContext.</span><span class="sxs-lookup"><span data-stu-id="18b2b-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="18b2b-190">-contesto-dir \<percorso ></span><span class="sxs-lookup"><span data-stu-id="18b2b-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="18b2b-191">La directory in cui inserire file DbContext in.</span><span class="sxs-lookup"><span data-stu-id="18b2b-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="18b2b-192">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="18b2b-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="18b2b-193">-f</span><span class="sxs-lookup"><span data-stu-id="18b2b-193">-f</span></span>              | <span data-ttu-id="18b2b-194">-force</span><span class="sxs-lookup"><span data-stu-id="18b2b-194">--force</span></span>                                 | <span data-ttu-id="18b2b-195">Sovrascrivi file esistenti.</span><span class="sxs-lookup"><span data-stu-id="18b2b-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="18b2b-196">-o</span><span class="sxs-lookup"><span data-stu-id="18b2b-196">-o</span></span>              | <span data-ttu-id="18b2b-197">-output-dir \<percorso ></span><span class="sxs-lookup"><span data-stu-id="18b2b-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="18b2b-198">La directory in cui inserire file nella.</span><span class="sxs-lookup"><span data-stu-id="18b2b-198">The directory to put files in.</span></span> <span data-ttu-id="18b2b-199">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="18b2b-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="18b2b-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span><span class="sxs-lookup"><span data-stu-id="18b2b-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="18b2b-201">Gli schemi delle tabelle per generare i tipi di entità per.</span><span class="sxs-lookup"><span data-stu-id="18b2b-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="18b2b-202">-t</span><span class="sxs-lookup"><span data-stu-id="18b2b-202">-t</span></span>              | <span data-ttu-id="18b2b-203">-tabella \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="18b2b-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="18b2b-204">Generare tipi di entità per le tabelle.</span><span class="sxs-lookup"><span data-stu-id="18b2b-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="18b2b-205">-usare nomi di database</span><span class="sxs-lookup"><span data-stu-id="18b2b-205">--use-database-names</span></span>                    | <span data-ttu-id="18b2b-206">Usare nomi di tabella e colonna direttamente dal database.</span><span class="sxs-lookup"><span data-stu-id="18b2b-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="18b2b-207">aggiungere dotnet migrazioni di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="18b2b-207">dotnet ef migrations add</span></span>

<span data-ttu-id="18b2b-208">Aggiunge una nuova migrazione.</span><span class="sxs-lookup"><span data-stu-id="18b2b-208">Adds a new migration.</span></span>

<span data-ttu-id="18b2b-209">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="18b2b-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="18b2b-210">\<NOME &GT;</span><span class="sxs-lookup"><span data-stu-id="18b2b-210">\<NAME></span></span> | <span data-ttu-id="18b2b-211">Il nome della migrazione.</span><span class="sxs-lookup"><span data-stu-id="18b2b-211">The name of the migration.</span></span> |

<span data-ttu-id="18b2b-212">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="18b2b-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="18b2b-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="18b2b-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="18b2b-214"><nobr>-output-dir \<percorso ></nobr></span><span class="sxs-lookup"><span data-stu-id="18b2b-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="18b2b-215">La directory e sub-spazio dei nomi da usare.</span><span class="sxs-lookup"><span data-stu-id="18b2b-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="18b2b-216">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="18b2b-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="18b2b-217">Il valore predefinito è "Migrazione".</span><span class="sxs-lookup"><span data-stu-id="18b2b-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="18b2b-218">elenco di dotnet ef migrations</span><span class="sxs-lookup"><span data-stu-id="18b2b-218">dotnet ef migrations list</span></span>

<span data-ttu-id="18b2b-219">Elenca le migrazioni disponibili.</span><span class="sxs-lookup"><span data-stu-id="18b2b-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="18b2b-220">rimuovere le migrazioni di Entity Framework di dotnet</span><span class="sxs-lookup"><span data-stu-id="18b2b-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="18b2b-221">Rimuove l'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="18b2b-221">Removes the last migration.</span></span>

<span data-ttu-id="18b2b-222">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="18b2b-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="18b2b-223">-f</span><span class="sxs-lookup"><span data-stu-id="18b2b-223">-f</span></span> | <span data-ttu-id="18b2b-224">-force</span><span class="sxs-lookup"><span data-stu-id="18b2b-224">--force</span></span> | <span data-ttu-id="18b2b-225">Se è stato applicato al database, ripristinare la migrazione.</span><span class="sxs-lookup"><span data-stu-id="18b2b-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="18b2b-226">script di dotnet ef migrations</span><span class="sxs-lookup"><span data-stu-id="18b2b-226">dotnet ef migrations script</span></span>

<span data-ttu-id="18b2b-227">Genera uno script SQL dalle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="18b2b-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="18b2b-228">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="18b2b-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="18b2b-229">\<DA &GT;</span><span class="sxs-lookup"><span data-stu-id="18b2b-229">\<FROM></span></span> | <span data-ttu-id="18b2b-230">La migrazione inizia.</span><span class="sxs-lookup"><span data-stu-id="18b2b-230">The starting migration.</span></span> <span data-ttu-id="18b2b-231">Il valore predefinito è 0 (il database iniziale).</span><span class="sxs-lookup"><span data-stu-id="18b2b-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="18b2b-232">\<PER &GT;</span><span class="sxs-lookup"><span data-stu-id="18b2b-232">\<TO></span></span>   | <span data-ttu-id="18b2b-233">La migrazione finale.</span><span class="sxs-lookup"><span data-stu-id="18b2b-233">The ending migration.</span></span> <span data-ttu-id="18b2b-234">Per impostazione predefinita all'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="18b2b-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="18b2b-235">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="18b2b-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="18b2b-236">-o</span><span class="sxs-lookup"><span data-stu-id="18b2b-236">-o</span></span> | <span data-ttu-id="18b2b-237">-output \<FILE ></span><span class="sxs-lookup"><span data-stu-id="18b2b-237">--output \<FILE></span></span> | <span data-ttu-id="18b2b-238">File in cui scrivere il risultato.</span><span class="sxs-lookup"><span data-stu-id="18b2b-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="18b2b-239">-i</span><span class="sxs-lookup"><span data-stu-id="18b2b-239">-i</span></span> | <span data-ttu-id="18b2b-240">-idempotenti</span><span class="sxs-lookup"><span data-stu-id="18b2b-240">--idempotent</span></span>     | <span data-ttu-id="18b2b-241">Generare uno script che può essere utilizzato in un database in ogni operazione di migrazione.</span><span class="sxs-lookup"><span data-stu-id="18b2b-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
