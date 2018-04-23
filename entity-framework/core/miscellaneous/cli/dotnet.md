---
title: .NET core - CLI EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 396d31c9d0c0f47d299f49e82e557ed29b8420e7
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="b0b0b-102">Strumenti da riga di comando di EF Core .NET</span><span class="sxs-lookup"><span data-stu-id="b0b0b-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="b0b0b-103">Gli strumenti da riga di comando .NET di Entity Framework Core sono un'estensione della piattaforma incrociata **dotnet** comando, che fa parte di [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="b0b0b-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="b0b0b-104">Se si utilizza Visual Studio, è consigliabile [gli strumenti PMC] [ 1] poiché forniscono invece un'esperienza più integrata.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="b0b0b-105">Installazione degli strumenti</span><span class="sxs-lookup"><span data-stu-id="b0b0b-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="b0b0b-106">Installare gli strumenti da riga di comando di EF Core .NET attenendosi alla procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="b0b0b-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="b0b0b-107">Modificare il file di progetto e aggiungere Microsoft.EntityFrameworkCore.Tools.DotNet come elemento DotNetCliToolReference (vedere sotto)</span><span class="sxs-lookup"><span data-stu-id="b0b0b-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="b0b0b-108">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="b0b0b-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="b0b0b-109">Il progetto risultante dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b0b0b-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="b0b0b-110">Un riferimento al pacchetto con `PrivateAssets="All"` significa che non viene esposto ai progetti che fanno riferimento a questo progetto, che risulta particolarmente utile per i pacchetti vengono in genere utilizzati solo durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="b0b0b-111">Se si ha tutti gli elementi a destra, deve essere in grado di eseguire correttamente il comando seguente in un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="b0b0b-112">Utilizzo degli strumenti</span><span class="sxs-lookup"><span data-stu-id="b0b0b-112">Using the tools</span></span>
---------------
<span data-ttu-id="b0b0b-113">Ogni volta che si richiama un comando, vengono utilizzate due progetti:</span><span class="sxs-lookup"><span data-stu-id="b0b0b-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="b0b0b-114">Il progetto di destinazione è dove vengono aggiunti, in alcuni casi rimossi, tutti i file.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="b0b0b-115">Il progetto di destinazione del progetto nella directory corrente per impostazione predefinita, ma può essere modificato utilizzando il <nobr> **-progetto** </nobr> opzione.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="b0b0b-116">Il progetto di avvio è quello emulato dagli strumenti quando si esegue il codice del progetto.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="b0b0b-117">Anche il progetto nella directory corrente per impostazione predefinita, ma può essere modificato utilizzando il **-progetto di avvio** opzione.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="b0b0b-118">Ad esempio, l'aggiornamento del database dell'applicazione web che include Core EF installato in un progetto diverso avrà un aspetto simile al seguente: `dotnet ef database update --project {project-path}` (dalla directory dell'app web)</span><span class="sxs-lookup"><span data-stu-id="b0b0b-118">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="b0b0b-119">Opzioni comuni:</span><span class="sxs-lookup"><span data-stu-id="b0b0b-119">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="b0b0b-120">--json</span><span class="sxs-lookup"><span data-stu-id="b0b0b-120">--json</span></span>                           | <span data-ttu-id="b0b0b-121">Mostra l'output JSON.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-121">Show JSON output.</span></span>           |
| <span data-ttu-id="b0b0b-122">-c</span><span class="sxs-lookup"><span data-stu-id="b0b0b-122">-c</span></span> | <span data-ttu-id="b0b0b-123">-contesto \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="b0b0b-123">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="b0b0b-124">L'elemento DbContext da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-124">The DbContext to use.</span></span>       |
| <span data-ttu-id="b0b0b-125">-p</span><span class="sxs-lookup"><span data-stu-id="b0b0b-125">-p</span></span> | <span data-ttu-id="b0b0b-126">-progetto \<progetto ></span><span class="sxs-lookup"><span data-stu-id="b0b0b-126">--project \<PROJECT></span></span>             | <span data-ttu-id="b0b0b-127">Il progetto da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-127">The project to use.</span></span>         |
| <span data-ttu-id="b0b0b-128">-s</span><span class="sxs-lookup"><span data-stu-id="b0b0b-128">-s</span></span> | <span data-ttu-id="b0b0b-129">-progetto di avvio \<progetto ></span><span class="sxs-lookup"><span data-stu-id="b0b0b-129">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="b0b0b-130">Il progetto di avvio da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-130">The startup project to use.</span></span> |
|    | <span data-ttu-id="b0b0b-131">-framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="b0b0b-131">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="b0b0b-132">Il framework di destinazione.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-132">The target framework.</span></span>       |
|    | <span data-ttu-id="b0b0b-133">-configurazione \<configurazione ></span><span class="sxs-lookup"><span data-stu-id="b0b0b-133">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="b0b0b-134">Configurazione da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-134">The configuration to use.</span></span>   |
|    | <span data-ttu-id="b0b0b-135">-runtime \<identificatore ></span><span class="sxs-lookup"><span data-stu-id="b0b0b-135">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="b0b0b-136">Il runtime da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-136">The runtime to use.</span></span>         |
| <span data-ttu-id="b0b0b-137">-h</span><span class="sxs-lookup"><span data-stu-id="b0b0b-137">-h</span></span> | <span data-ttu-id="b0b0b-138">-Guida in linea</span><span class="sxs-lookup"><span data-stu-id="b0b0b-138">--help</span></span>                           | <span data-ttu-id="b0b0b-139">Mostra le informazioni della Guida.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-139">Show help information.</span></span>      |
| <span data-ttu-id="b0b0b-140">-v</span><span class="sxs-lookup"><span data-stu-id="b0b0b-140">-v</span></span> | <span data-ttu-id="b0b0b-141">-verbose</span><span class="sxs-lookup"><span data-stu-id="b0b0b-141">--verbose</span></span>                        | <span data-ttu-id="b0b0b-142">Mostra l'output dettagliato.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-142">Show verbose output.</span></span>        |
|    | <span data-ttu-id="b0b0b-143">-Nessun colore</span><span class="sxs-lookup"><span data-stu-id="b0b0b-143">--no-color</span></span>                       | <span data-ttu-id="b0b0b-144">Non usa l'output.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-144">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="b0b0b-145">--prefix-output</span><span class="sxs-lookup"><span data-stu-id="b0b0b-145">--prefix-output</span></span>                  | <span data-ttu-id="b0b0b-146">Prefisso con livello di output.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-146">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="b0b0b-147">Per specificare l'ambiente di ASP.NET Core, impostare il **ASPNETCORE_ENVIRONMENT** variabile di ambiente prima dell'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-147">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="b0b0b-148">Comandi:</span><span class="sxs-lookup"><span data-stu-id="b0b0b-148">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="b0b0b-149">eliminazione del database ef dotnet</span><span class="sxs-lookup"><span data-stu-id="b0b0b-149">dotnet ef database drop</span></span>

<span data-ttu-id="b0b0b-150">Elimina il database.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-150">Drops the database.</span></span>

<span data-ttu-id="b0b0b-151">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="b0b0b-151">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="b0b0b-152">-f</span><span class="sxs-lookup"><span data-stu-id="b0b0b-152">-f</span></span> | <span data-ttu-id="b0b0b-153">-force</span><span class="sxs-lookup"><span data-stu-id="b0b0b-153">--force</span></span>   | <span data-ttu-id="b0b0b-154">Non verificare.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-154">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="b0b0b-155">-esecuzione</span><span class="sxs-lookup"><span data-stu-id="b0b0b-155">--dry-run</span></span> | <span data-ttu-id="b0b0b-156">Mostra il database verrà rimossa, ma non l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-156">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="b0b0b-157">aggiornamento del database ef dotnet</span><span class="sxs-lookup"><span data-stu-id="b0b0b-157">dotnet ef database update</span></span>

<span data-ttu-id="b0b0b-158">Aggiorna il database in una migrazione specificata.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-158">Updates the database to a specified migration.</span></span>

<span data-ttu-id="b0b0b-159">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="b0b0b-159">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="b0b0b-160">\<MIGRAZIONE &GT;</span><span class="sxs-lookup"><span data-stu-id="b0b0b-160">\<MIGRATION></span></span> | <span data-ttu-id="b0b0b-161">La migrazione di destinazione.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-161">The target migration.</span></span> <span data-ttu-id="b0b0b-162">Se è 0, verranno annullate tutte le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-162">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="b0b0b-163">Per impostazione predefinita all'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-163">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="b0b0b-164">informazioni di dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="b0b0b-164">dotnet ef dbcontext info</span></span>

<span data-ttu-id="b0b0b-165">Ottiene informazioni sul tipo DbContext.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-165">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="b0b0b-166">elenco di dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="b0b0b-166">dotnet ef dbcontext list</span></span>

<span data-ttu-id="b0b0b-167">Elenca i tipi DbContext disponibili.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-167">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="b0b0b-168">scaffolding di dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="b0b0b-168">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="b0b0b-169">Strutture una tipi DbContext ed entità per un database.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-169">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="b0b0b-170">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="b0b0b-170">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="b0b0b-171">\<CONNESSIONE &GT;</span><span class="sxs-lookup"><span data-stu-id="b0b0b-171">\<CONNECTION></span></span> | <span data-ttu-id="b0b0b-172">La stringa di connessione al database.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-172">The connection string to the database.</span></span>                              |
| <span data-ttu-id="b0b0b-173">\<PROVIDER &GT;</span><span class="sxs-lookup"><span data-stu-id="b0b0b-173">\<PROVIDER></span></span>   | <span data-ttu-id="b0b0b-174">Il provider da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-174">The provider to use.</span></span> <span data-ttu-id="b0b0b-175">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="b0b0b-175">(E.g.</span></span> <span data-ttu-id="b0b0b-176">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="b0b0b-176">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="b0b0b-177">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="b0b0b-177">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b0b0b-178"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="b0b0b-178"><nobr>-d</nobr></span></span> | <span data-ttu-id="b0b0b-179">--le annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="b0b0b-179">--data-annotations</span></span>                      | <span data-ttu-id="b0b0b-180">Utilizzare gli attributi per configurare il modello (dove possibile).</span><span class="sxs-lookup"><span data-stu-id="b0b0b-180">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="b0b0b-181">Se omesso, viene utilizzato solo l'API fluent.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-181">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="b0b0b-182">-c</span><span class="sxs-lookup"><span data-stu-id="b0b0b-182">-c</span></span>              | <span data-ttu-id="b0b0b-183">-contesto \<nome ></span><span class="sxs-lookup"><span data-stu-id="b0b0b-183">--context \<NAME></span></span>                       | <span data-ttu-id="b0b0b-184">Il nome di DbContext.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-184">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="b0b0b-185">-dir contesto \<percorso ></span><span class="sxs-lookup"><span data-stu-id="b0b0b-185">--context-dir \<PATH></span></span>                   | <span data-ttu-id="b0b0b-186">Della directory in cui inserire file DbContext in.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-186">The directory to put DbContext file in.</span></span> <span data-ttu-id="b0b0b-187">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-187">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="b0b0b-188">-f</span><span class="sxs-lookup"><span data-stu-id="b0b0b-188">-f</span></span>              | <span data-ttu-id="b0b0b-189">-force</span><span class="sxs-lookup"><span data-stu-id="b0b0b-189">--force</span></span>                                 | <span data-ttu-id="b0b0b-190">Sovrascrivi file esistenti.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-190">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="b0b0b-191">-o</span><span class="sxs-lookup"><span data-stu-id="b0b0b-191">-o</span></span>              | <span data-ttu-id="b0b0b-192">-output-dir \<percorso ></span><span class="sxs-lookup"><span data-stu-id="b0b0b-192">--output-dir \<PATH></span></span>                    | <span data-ttu-id="b0b0b-193">Della directory in cui inserire i file in.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-193">The directory to put files in.</span></span> <span data-ttu-id="b0b0b-194">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-194">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="b0b0b-195"><nobr>--schema \<SCHEMA_NAME>...</nobr></span><span class="sxs-lookup"><span data-stu-id="b0b0b-195"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="b0b0b-196">Gli schemi delle tabelle per generare i tipi di entità per.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-196">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="b0b0b-197">-t</span><span class="sxs-lookup"><span data-stu-id="b0b0b-197">-t</span></span>              | <span data-ttu-id="b0b0b-198">-tabella \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="b0b0b-198">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="b0b0b-199">Generare tipi di entità per le tabelle.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-199">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="b0b0b-200">-Utilizzare nomi di database</span><span class="sxs-lookup"><span data-stu-id="b0b0b-200">--use-database-names</span></span>                    | <span data-ttu-id="b0b0b-201">Utilizzare nomi di tabella e colonna direttamente dal database.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-201">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="b0b0b-202">aggiungere le migrazioni di ef dotnet</span><span class="sxs-lookup"><span data-stu-id="b0b0b-202">dotnet ef migrations add</span></span>

<span data-ttu-id="b0b0b-203">Aggiunge una nuova migrazione.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-203">Adds a new migration.</span></span>

<span data-ttu-id="b0b0b-204">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="b0b0b-204">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="b0b0b-205">\<NOME &GT;</span><span class="sxs-lookup"><span data-stu-id="b0b0b-205">\<NAME></span></span> | <span data-ttu-id="b0b0b-206">Il nome della migrazione.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-206">The name of the migration.</span></span> |

<span data-ttu-id="b0b0b-207">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="b0b0b-207">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b0b0b-208"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="b0b0b-208"><nobr>-o</nobr></span></span> | <span data-ttu-id="b0b0b-209"><nobr>-output-dir \<percorso ></nobr></span><span class="sxs-lookup"><span data-stu-id="b0b0b-209"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="b0b0b-210">La directory e spazio dei nomi secondario da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-210">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="b0b0b-211">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-211">Paths are relative to the project directory.</span></span> <span data-ttu-id="b0b0b-212">Valore predefinito è "Migrazione".</span><span class="sxs-lookup"><span data-stu-id="b0b0b-212">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="b0b0b-213">elenco di migrazioni ef dotnet</span><span class="sxs-lookup"><span data-stu-id="b0b0b-213">dotnet ef migrations list</span></span>

<span data-ttu-id="b0b0b-214">Elenca le migrazioni disponibili.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-214">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="b0b0b-215">rimuovere le migrazioni di ef dotnet</span><span class="sxs-lookup"><span data-stu-id="b0b0b-215">dotnet ef migrations remove</span></span>

<span data-ttu-id="b0b0b-216">Rimuove l'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-216">Removes the last migration.</span></span>

<span data-ttu-id="b0b0b-217">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="b0b0b-217">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="b0b0b-218">-f</span><span class="sxs-lookup"><span data-stu-id="b0b0b-218">-f</span></span> | <span data-ttu-id="b0b0b-219">-force</span><span class="sxs-lookup"><span data-stu-id="b0b0b-219">--force</span></span> | <span data-ttu-id="b0b0b-220">Ripristinare la migrazione, se è stato applicato al database.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-220">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="b0b0b-221">script di migrazioni ef dotnet</span><span class="sxs-lookup"><span data-stu-id="b0b0b-221">dotnet ef migrations script</span></span>

<span data-ttu-id="b0b0b-222">Genera uno script SQL dalla migrazione.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-222">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="b0b0b-223">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="b0b0b-223">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="b0b0b-224">\<DA &GT;</span><span class="sxs-lookup"><span data-stu-id="b0b0b-224">\<FROM></span></span> | <span data-ttu-id="b0b0b-225">La migrazione inizia.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-225">The starting migration.</span></span> <span data-ttu-id="b0b0b-226">Il valore predefinito 0 (il database iniziale).</span><span class="sxs-lookup"><span data-stu-id="b0b0b-226">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="b0b0b-227">\<A &GT;</span><span class="sxs-lookup"><span data-stu-id="b0b0b-227">\<TO></span></span>   | <span data-ttu-id="b0b0b-228">La migrazione finale.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-228">The ending migration.</span></span> <span data-ttu-id="b0b0b-229">Per impostazione predefinita all'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-229">Defaults to the last migration.</span></span>         |

<span data-ttu-id="b0b0b-230">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="b0b0b-230">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="b0b0b-231">-o</span><span class="sxs-lookup"><span data-stu-id="b0b0b-231">-o</span></span> | <span data-ttu-id="b0b0b-232">-output \<FILE ></span><span class="sxs-lookup"><span data-stu-id="b0b0b-232">--output \<FILE></span></span> | <span data-ttu-id="b0b0b-233">File in cui scrivere il risultato.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-233">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="b0b0b-234">-i</span><span class="sxs-lookup"><span data-stu-id="b0b0b-234">-i</span></span> | <span data-ttu-id="b0b0b-235">idempotente.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-235">--idempotent</span></span>     | <span data-ttu-id="b0b0b-236">Generare uno script che può essere usato in un database in ogni operazione di migrazione.</span><span class="sxs-lookup"><span data-stu-id="b0b0b-236">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
