---
title: .NET core - CLI EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 8a52cb8259bb381729a33a8161aec4b73f69f45d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="70e89-102">Strumenti da riga di comando di EF Core .NET</span><span class="sxs-lookup"><span data-stu-id="70e89-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="70e89-103">Gli strumenti da riga di comando .NET di Entity Framework Core sono un'estensione della piattaforma incrociata **dotnet** comando, che fa parte di [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="70e89-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="70e89-104">Se si utilizza Visual Studio, è consigliabile [gli strumenti PMC] [ 1] poiché forniscono invece un'esperienza più integrata.</span><span class="sxs-lookup"><span data-stu-id="70e89-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="70e89-105">Installazione degli strumenti</span><span class="sxs-lookup"><span data-stu-id="70e89-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="70e89-106">Installare gli strumenti da riga di comando di EF Core .NET attenendosi alla procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="70e89-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="70e89-107">Modificare il file di progetto e aggiungere Microsoft.EntityFrameworkCore.Tools.DotNet come elemento DotNetCliToolReference (vedere sotto)</span><span class="sxs-lookup"><span data-stu-id="70e89-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="70e89-108">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="70e89-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="70e89-109">Il progetto risultante dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="70e89-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="70e89-110">Un riferimento al pacchetto con `PrivateAssets="All"` significa che non viene esposto ai progetti che fanno riferimento a questo progetto, che risulta particolarmente utile per i pacchetti vengono in genere utilizzati solo durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="70e89-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="70e89-111">Se si ha tutti gli elementi a destra, deve essere in grado di eseguire correttamente il comando seguente in un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="70e89-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="70e89-112">Utilizzo degli strumenti</span><span class="sxs-lookup"><span data-stu-id="70e89-112">Using the tools</span></span>
---------------
<span data-ttu-id="70e89-113">Ogni volta che si richiama un comando, vengono utilizzate due progetti:</span><span class="sxs-lookup"><span data-stu-id="70e89-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="70e89-114">Il progetto di destinazione è dove vengono aggiunti, in alcuni casi rimossi, tutti i file.</span><span class="sxs-lookup"><span data-stu-id="70e89-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="70e89-115">Il progetto di destinazione del progetto nella directory corrente per impostazione predefinita, ma può essere modificato utilizzando il <nobr> **-progetto** </nobr> opzione.</span><span class="sxs-lookup"><span data-stu-id="70e89-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="70e89-116">Il progetto di avvio è quello emulato dagli strumenti quando si esegue il codice del progetto.</span><span class="sxs-lookup"><span data-stu-id="70e89-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="70e89-117">Anche il progetto nella directory corrente per impostazione predefinita, ma può essere modificato utilizzando il **-progetto di avvio** opzione.</span><span class="sxs-lookup"><span data-stu-id="70e89-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

<span data-ttu-id="70e89-118">Opzioni comuni:</span><span class="sxs-lookup"><span data-stu-id="70e89-118">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="70e89-119">--json</span><span class="sxs-lookup"><span data-stu-id="70e89-119">--json</span></span>                           | <span data-ttu-id="70e89-120">Mostra l'output JSON.</span><span class="sxs-lookup"><span data-stu-id="70e89-120">Show JSON output.</span></span>           |
| <span data-ttu-id="70e89-121">-c</span><span class="sxs-lookup"><span data-stu-id="70e89-121">-c</span></span> | <span data-ttu-id="70e89-122">-contesto \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="70e89-122">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="70e89-123">L'elemento DbContext da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="70e89-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="70e89-124">-p</span><span class="sxs-lookup"><span data-stu-id="70e89-124">-p</span></span> | <span data-ttu-id="70e89-125">-progetto \<progetto ></span><span class="sxs-lookup"><span data-stu-id="70e89-125">--project \<PROJECT></span></span>             | <span data-ttu-id="70e89-126">Il progetto da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="70e89-126">The project to use.</span></span>         |
| <span data-ttu-id="70e89-127">-s</span><span class="sxs-lookup"><span data-stu-id="70e89-127">-s</span></span> | <span data-ttu-id="70e89-128">-progetto di avvio \<progetto ></span><span class="sxs-lookup"><span data-stu-id="70e89-128">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="70e89-129">Il progetto di avvio da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="70e89-129">The startup project to use.</span></span> |
|    | <span data-ttu-id="70e89-130">-framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="70e89-130">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="70e89-131">Il framework di destinazione.</span><span class="sxs-lookup"><span data-stu-id="70e89-131">The target framework.</span></span>       |
|    | <span data-ttu-id="70e89-132">-configurazione \<configurazione ></span><span class="sxs-lookup"><span data-stu-id="70e89-132">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="70e89-133">Configurazione da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="70e89-133">The configuration to use.</span></span>   |
|    | <span data-ttu-id="70e89-134">-runtime \<identificatore ></span><span class="sxs-lookup"><span data-stu-id="70e89-134">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="70e89-135">Il runtime da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="70e89-135">The runtime to use.</span></span>         |
| <span data-ttu-id="70e89-136">-h</span><span class="sxs-lookup"><span data-stu-id="70e89-136">-h</span></span> | <span data-ttu-id="70e89-137">-Guida in linea</span><span class="sxs-lookup"><span data-stu-id="70e89-137">--help</span></span>                           | <span data-ttu-id="70e89-138">Mostra le informazioni della Guida.</span><span class="sxs-lookup"><span data-stu-id="70e89-138">Show help information.</span></span>      |
| <span data-ttu-id="70e89-139">-v</span><span class="sxs-lookup"><span data-stu-id="70e89-139">-v</span></span> | <span data-ttu-id="70e89-140">-verbose</span><span class="sxs-lookup"><span data-stu-id="70e89-140">--verbose</span></span>                        | <span data-ttu-id="70e89-141">Mostra l'output dettagliato.</span><span class="sxs-lookup"><span data-stu-id="70e89-141">Show verbose output.</span></span>        |
|    | <span data-ttu-id="70e89-142">-Nessun colore</span><span class="sxs-lookup"><span data-stu-id="70e89-142">--no-color</span></span>                       | <span data-ttu-id="70e89-143">Non usa l'output.</span><span class="sxs-lookup"><span data-stu-id="70e89-143">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="70e89-144">--prefix-output</span><span class="sxs-lookup"><span data-stu-id="70e89-144">--prefix-output</span></span>                  | <span data-ttu-id="70e89-145">Prefisso con livello di output.</span><span class="sxs-lookup"><span data-stu-id="70e89-145">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="70e89-146">Per specificare l'ambiente di ASP.NET Core, impostare il **ASPNETCORE_ENVIRONMENT** variabile di ambiente prima dell'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="70e89-146">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="70e89-147">Comandi:</span><span class="sxs-lookup"><span data-stu-id="70e89-147">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="70e89-148">eliminazione del database ef dotnet</span><span class="sxs-lookup"><span data-stu-id="70e89-148">dotnet ef database drop</span></span>

<span data-ttu-id="70e89-149">Elimina il database.</span><span class="sxs-lookup"><span data-stu-id="70e89-149">Drops the database.</span></span>

<span data-ttu-id="70e89-150">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="70e89-150">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="70e89-151">-f</span><span class="sxs-lookup"><span data-stu-id="70e89-151">-f</span></span> | <span data-ttu-id="70e89-152">-force</span><span class="sxs-lookup"><span data-stu-id="70e89-152">--force</span></span>   | <span data-ttu-id="70e89-153">Non verificare.</span><span class="sxs-lookup"><span data-stu-id="70e89-153">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="70e89-154">--dry-run</span><span class="sxs-lookup"><span data-stu-id="70e89-154">--dry-run</span></span> | <span data-ttu-id="70e89-155">Mostra il database verrà rimossa, ma non l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="70e89-155">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="70e89-156">aggiornamento del database ef dotnet</span><span class="sxs-lookup"><span data-stu-id="70e89-156">dotnet ef database update</span></span>

<span data-ttu-id="70e89-157">Aggiorna il database in una migrazione specificata.</span><span class="sxs-lookup"><span data-stu-id="70e89-157">Updates the database to a specified migration.</span></span>

<span data-ttu-id="70e89-158">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="70e89-158">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="70e89-159">\<MIGRAZIONE ></span><span class="sxs-lookup"><span data-stu-id="70e89-159">\<MIGRATION></span></span> | <span data-ttu-id="70e89-160">La migrazione di destinazione.</span><span class="sxs-lookup"><span data-stu-id="70e89-160">The target migration.</span></span> <span data-ttu-id="70e89-161">Se è 0, verranno annullate tutte le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="70e89-161">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="70e89-162">Per impostazione predefinita all'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="70e89-162">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="70e89-163">informazioni di dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="70e89-163">dotnet ef dbcontext info</span></span>

<span data-ttu-id="70e89-164">Ottiene informazioni sul tipo DbContext.</span><span class="sxs-lookup"><span data-stu-id="70e89-164">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="70e89-165">elenco di dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="70e89-165">dotnet ef dbcontext list</span></span>

<span data-ttu-id="70e89-166">Elenca i tipi DbContext disponibili.</span><span class="sxs-lookup"><span data-stu-id="70e89-166">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="70e89-167">scaffolding di dbcontext ef dotnet</span><span class="sxs-lookup"><span data-stu-id="70e89-167">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="70e89-168">Strutture una tipi DbContext ed entità per un database.</span><span class="sxs-lookup"><span data-stu-id="70e89-168">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="70e89-169">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="70e89-169">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="70e89-170">\<CONNESSIONE ></span><span class="sxs-lookup"><span data-stu-id="70e89-170">\<CONNECTION></span></span> | <span data-ttu-id="70e89-171">La stringa di connessione al database.</span><span class="sxs-lookup"><span data-stu-id="70e89-171">The connection string to the database.</span></span>                              |
| <span data-ttu-id="70e89-172">\<PROVIDER></span><span class="sxs-lookup"><span data-stu-id="70e89-172">\<PROVIDER></span></span>   | <span data-ttu-id="70e89-173">Il provider da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="70e89-173">The provider to use.</span></span> <span data-ttu-id="70e89-174">(Ad esempio</span><span class="sxs-lookup"><span data-stu-id="70e89-174">(E.g.</span></span> <span data-ttu-id="70e89-175">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="70e89-175">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="70e89-176">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="70e89-176">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="70e89-177"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="70e89-177"><nobr>-d</nobr></span></span> | <span data-ttu-id="70e89-178">--le annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="70e89-178">--data-annotations</span></span>                      | <span data-ttu-id="70e89-179">Utilizzare gli attributi per configurare il modello (dove possibile).</span><span class="sxs-lookup"><span data-stu-id="70e89-179">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="70e89-180">Se omesso, viene utilizzato solo l'API fluent.</span><span class="sxs-lookup"><span data-stu-id="70e89-180">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="70e89-181">-c</span><span class="sxs-lookup"><span data-stu-id="70e89-181">-c</span></span>              | <span data-ttu-id="70e89-182">-contesto \<nome ></span><span class="sxs-lookup"><span data-stu-id="70e89-182">--context \<NAME></span></span>                       | <span data-ttu-id="70e89-183">Il nome di DbContext.</span><span class="sxs-lookup"><span data-stu-id="70e89-183">The name of the DbContext.</span></span>                                                                       |
| <span data-ttu-id="70e89-184">-f</span><span class="sxs-lookup"><span data-stu-id="70e89-184">-f</span></span>              | <span data-ttu-id="70e89-185">-force</span><span class="sxs-lookup"><span data-stu-id="70e89-185">--force</span></span>                                 | <span data-ttu-id="70e89-186">Sovrascrivi file esistenti.</span><span class="sxs-lookup"><span data-stu-id="70e89-186">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="70e89-187">-o</span><span class="sxs-lookup"><span data-stu-id="70e89-187">-o</span></span>              | <span data-ttu-id="70e89-188">-output-dir \<percorso ></span><span class="sxs-lookup"><span data-stu-id="70e89-188">--output-dir \<PATH></span></span>                    | <span data-ttu-id="70e89-189">Della directory in cui inserire i file in.</span><span class="sxs-lookup"><span data-stu-id="70e89-189">The directory to put files in.</span></span> <span data-ttu-id="70e89-190">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="70e89-190">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="70e89-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span><span class="sxs-lookup"><span data-stu-id="70e89-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="70e89-192">Gli schemi delle tabelle per generare i tipi di entità per.</span><span class="sxs-lookup"><span data-stu-id="70e89-192">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="70e89-193">-t</span><span class="sxs-lookup"><span data-stu-id="70e89-193">-t</span></span>              | <span data-ttu-id="70e89-194">-tabella \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="70e89-194">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="70e89-195">Generare tipi di entità per le tabelle.</span><span class="sxs-lookup"><span data-stu-id="70e89-195">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="70e89-196">-Utilizzare nomi di database</span><span class="sxs-lookup"><span data-stu-id="70e89-196">--use-database-names</span></span>                    | <span data-ttu-id="70e89-197">Utilizzare nomi di tabella e colonna direttamente dal database.</span><span class="sxs-lookup"><span data-stu-id="70e89-197">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="70e89-198">aggiungere le migrazioni di ef dotnet</span><span class="sxs-lookup"><span data-stu-id="70e89-198">dotnet ef migrations add</span></span>

<span data-ttu-id="70e89-199">Aggiunge una nuova migrazione.</span><span class="sxs-lookup"><span data-stu-id="70e89-199">Adds a new migration.</span></span>

<span data-ttu-id="70e89-200">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="70e89-200">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="70e89-201">\<NOME ></span><span class="sxs-lookup"><span data-stu-id="70e89-201">\<NAME></span></span> | <span data-ttu-id="70e89-202">Il nome della migrazione.</span><span class="sxs-lookup"><span data-stu-id="70e89-202">The name of the migration.</span></span> |

<span data-ttu-id="70e89-203">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="70e89-203">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="70e89-204"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="70e89-204"><nobr>-o</nobr></span></span> | <span data-ttu-id="70e89-205"><nobr>-output-dir \<percorso ></nobr></span><span class="sxs-lookup"><span data-stu-id="70e89-205"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="70e89-206">La directory e spazio dei nomi secondario da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="70e89-206">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="70e89-207">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="70e89-207">Paths are relative to the project directory.</span></span> <span data-ttu-id="70e89-208">Valore predefinito è "Migrazione".</span><span class="sxs-lookup"><span data-stu-id="70e89-208">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="70e89-209">elenco di migrazioni ef dotnet</span><span class="sxs-lookup"><span data-stu-id="70e89-209">dotnet ef migrations list</span></span>

<span data-ttu-id="70e89-210">Elenca le migrazioni disponibili.</span><span class="sxs-lookup"><span data-stu-id="70e89-210">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="70e89-211">rimuovere le migrazioni di ef dotnet</span><span class="sxs-lookup"><span data-stu-id="70e89-211">dotnet ef migrations remove</span></span>

<span data-ttu-id="70e89-212">Rimuove l'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="70e89-212">Removes the last migration.</span></span>

<span data-ttu-id="70e89-213">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="70e89-213">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="70e89-214">-f</span><span class="sxs-lookup"><span data-stu-id="70e89-214">-f</span></span> | <span data-ttu-id="70e89-215">-force</span><span class="sxs-lookup"><span data-stu-id="70e89-215">--force</span></span> | <span data-ttu-id="70e89-216">Non verificare se la migrazione è stata applicata al database.</span><span class="sxs-lookup"><span data-stu-id="70e89-216">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="70e89-217">script di migrazioni ef dotnet</span><span class="sxs-lookup"><span data-stu-id="70e89-217">dotnet ef migrations script</span></span>

<span data-ttu-id="70e89-218">Genera uno script SQL dalla migrazione.</span><span class="sxs-lookup"><span data-stu-id="70e89-218">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="70e89-219">Argomenti:</span><span class="sxs-lookup"><span data-stu-id="70e89-219">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="70e89-220">\<DA ></span><span class="sxs-lookup"><span data-stu-id="70e89-220">\<FROM></span></span> | <span data-ttu-id="70e89-221">La migrazione inizia.</span><span class="sxs-lookup"><span data-stu-id="70e89-221">The starting migration.</span></span> <span data-ttu-id="70e89-222">Il valore predefinito 0 (il database iniziale).</span><span class="sxs-lookup"><span data-stu-id="70e89-222">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="70e89-223">\<TO></span><span class="sxs-lookup"><span data-stu-id="70e89-223">\<TO></span></span>   | <span data-ttu-id="70e89-224">La migrazione finale.</span><span class="sxs-lookup"><span data-stu-id="70e89-224">The ending migration.</span></span> <span data-ttu-id="70e89-225">Per impostazione predefinita all'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="70e89-225">Defaults to the last migration.</span></span>         |

<span data-ttu-id="70e89-226">Opzioni:</span><span class="sxs-lookup"><span data-stu-id="70e89-226">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="70e89-227">-o</span><span class="sxs-lookup"><span data-stu-id="70e89-227">-o</span></span> | <span data-ttu-id="70e89-228">-output \<FILE ></span><span class="sxs-lookup"><span data-stu-id="70e89-228">--output \<FILE></span></span> | <span data-ttu-id="70e89-229">File in cui scrivere il risultato.</span><span class="sxs-lookup"><span data-stu-id="70e89-229">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="70e89-230">-i</span><span class="sxs-lookup"><span data-stu-id="70e89-230">-i</span></span> | <span data-ttu-id="70e89-231">idempotente.</span><span class="sxs-lookup"><span data-stu-id="70e89-231">--idempotent</span></span>     | <span data-ttu-id="70e89-232">Generare uno script che può essere usato in un database in ogni operazione di migrazione.</span><span class="sxs-lookup"><span data-stu-id="70e89-232">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
