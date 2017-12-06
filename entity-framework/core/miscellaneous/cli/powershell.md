---
title: Console di gestione pacchetti (Visual Studio) - Core a Entity Framework
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: b4ecb27edf94e7b9ad6c7fe65a891dcbf1593309
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="1e1ea-102">Strumenti di Entity Framework Core Package Manager Console</span><span class="sxs-lookup"><span data-stu-id="1e1ea-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="1e1ea-103">Gli strumenti di Entity Framework Core Package Manager Console (PMC) eseguito all'interno di Visual Studio tramite NuGet [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="1e1ea-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="1e1ea-104">Questi strumenti sono compatibili con i progetti sia .NET Framework e .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="1e1ea-105">Non si usa Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="1e1ea-105">Not using Visual Studio?</span></span> <span data-ttu-id="1e1ea-106">Il [strumenti da riga di comando di base EF] [ 1] sono multipiattaforma e vengono eseguiti all'interno di un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="1e1ea-107">Installazione degli strumenti</span><span class="sxs-lookup"><span data-stu-id="1e1ea-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="1e1ea-108">Installare gli strumenti di Entity Framework Core Package Manager Console installando il pacchetto Microsoft.EntityFrameworkCore.Tools NuGet.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="1e1ea-109">È possibile installarlo eseguendo il comando seguente all'interno di [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="1e1ea-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="1e1ea-110">Se tutto funziona correttamente, deve essere in grado di eseguire questo comando:</span><span class="sxs-lookup"><span data-stu-id="1e1ea-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="1e1ea-111">Se il progetto di avvio è destinato a .NET Standard [cross-destinazione un framework supportati] [ 3] prima di utilizzare gli strumenti.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1e1ea-112">Se si utilizza **Windows universale** o **Xamarin**, spostare il codice di Entity Framework in una libreria di classi .NET Standard e [cross-destinazione un framework supportati] [ 3] prima di utilizzare gli strumenti.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="1e1ea-113">Specificare la libreria di classi come progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="1e1ea-114">Utilizzo degli strumenti</span><span class="sxs-lookup"><span data-stu-id="1e1ea-114">Using the tools</span></span>
---------------
<span data-ttu-id="1e1ea-115">Ogni volta che si richiama un comando, vengono utilizzate due progetti:</span><span class="sxs-lookup"><span data-stu-id="1e1ea-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="1e1ea-116">Il progetto di destinazione è in cui vengono aggiunti tutti i file (o in alcuni casi rimossi).</span><span class="sxs-lookup"><span data-stu-id="1e1ea-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="1e1ea-117">Per impostazione predefinita il progetto di destinazione di **progetto predefinito** selezionato nella Console di gestione pacchetti, ma può anche essere specificato utilizzando il progetto parametro-.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="1e1ea-118">Il progetto di avvio viene emulato dagli strumenti quando si esegue il codice del progetto.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="1e1ea-119">Per impostazione predefinita uno **imposta come progetto di avvio** in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="1e1ea-120">Può anche essere specificato utilizzando il parametro - proprietà.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="1e1ea-121">Parametri comuni:</span><span class="sxs-lookup"><span data-stu-id="1e1ea-121">Common parameters:</span></span>

|                           |                             |
| ------------------------- | --------------------------- |
| <span data-ttu-id="1e1ea-122">-Contesto \<stringa ></span><span class="sxs-lookup"><span data-stu-id="1e1ea-122">-Context \<String></span></span>        | <span data-ttu-id="1e1ea-123">L'elemento DbContext da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="1e1ea-124">-Progetto \<stringa ></span><span class="sxs-lookup"><span data-stu-id="1e1ea-124">-Project \<String></span></span>        | <span data-ttu-id="1e1ea-125">Il progetto da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-125">The project to use.</span></span>         |
| <span data-ttu-id="1e1ea-126">Proprietà - \<stringa ></span><span class="sxs-lookup"><span data-stu-id="1e1ea-126">-StartupProject \<String></span></span> | <span data-ttu-id="1e1ea-127">Il progetto di avvio da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-127">The startup project to use.</span></span> |
| <span data-ttu-id="1e1ea-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="1e1ea-128">-Verbose</span></span>                  | <span data-ttu-id="1e1ea-129">Mostra l'output dettagliato.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-129">Show verbose output.</span></span>        |

<span data-ttu-id="1e1ea-130">Per visualizzare informazioni della Guida su un comando, utilizzare PowerShell `Get-Help` comando.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="1e1ea-131">I parametri di contesto, del progetto e proprietà supportano l'espansione tramite tab.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="1e1ea-132">Impostare **env:ASPNETCORE_ENVIRONMENT** prima di eseguire per specificare l'ambiente di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="1e1ea-133">Comandi</span><span class="sxs-lookup"><span data-stu-id="1e1ea-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="1e1ea-134">Aggiungere la migrazione</span><span class="sxs-lookup"><span data-stu-id="1e1ea-134">Add-Migration</span></span>

<span data-ttu-id="1e1ea-135">Aggiunge una nuova migrazione.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-135">Adds a new migration.</span></span>

<span data-ttu-id="1e1ea-136">Parametri:</span><span class="sxs-lookup"><span data-stu-id="1e1ea-136">Parameters:</span></span>

|                                    |                                                                                 |
| ---------------------------------- | ------------------------------------------------------------------------------- |
| <span data-ttu-id="1e1ea-137">***-Nome*** \<stringa ></span><span class="sxs-lookup"><span data-stu-id="1e1ea-137">***-Name*** \<String></span></span>              | <span data-ttu-id="1e1ea-138">Il nome della migrazione.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-138">The name of the migration.</span></span>                                                      |
| <span data-ttu-id="1e1ea-139"><nobr>-OutputDir \<stringa ></nobr></span><span class="sxs-lookup"><span data-stu-id="1e1ea-139"><nobr>-OutputDir \<String></nobr></span></span>  | <span data-ttu-id="1e1ea-140">La directory e spazio dei nomi secondario da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="1e1ea-141">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="1e1ea-142">Valore predefinito è "Migrazione".</span><span class="sxs-lookup"><span data-stu-id="1e1ea-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="1e1ea-143">I parametri in **grassetto** sono necessari e quelle in *corsivo* sono posizionali.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="1e1ea-144">Eliminazione di Database</span><span class="sxs-lookup"><span data-stu-id="1e1ea-144">Drop-Database</span></span>

<span data-ttu-id="1e1ea-145">Elimina il database.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-145">Drops the database.</span></span>

<span data-ttu-id="1e1ea-146">Parametri:</span><span class="sxs-lookup"><span data-stu-id="1e1ea-146">Parameters:</span></span>

|          |                                                          |
| -------- | -------------------------------------------------------- |
| <span data-ttu-id="1e1ea-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="1e1ea-147">-WhatIf</span></span>  | <span data-ttu-id="1e1ea-148">Mostra il database verrà rimossa, ma non l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="1e1ea-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="1e1ea-149">Get-DbContext</span></span>

<span data-ttu-id="1e1ea-150">Ottiene informazioni sul tipo DbContext.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="1e1ea-151">Remove-migrazione</span><span class="sxs-lookup"><span data-stu-id="1e1ea-151">Remove-Migration</span></span>

<span data-ttu-id="1e1ea-152">Rimuove l'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-152">Removes the last migration.</span></span>

<span data-ttu-id="1e1ea-153">Parametri:</span><span class="sxs-lookup"><span data-stu-id="1e1ea-153">Parameters:</span></span>

|        |                                                                       |
| ------ | --------------------------------------------------------------------- |
| <span data-ttu-id="1e1ea-154">-Force</span><span class="sxs-lookup"><span data-stu-id="1e1ea-154">-Force</span></span> | <span data-ttu-id="1e1ea-155">Non verificare se la migrazione è stata applicata al database.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-155">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="1e1ea-156">Lo scaffolding DbContext</span><span class="sxs-lookup"><span data-stu-id="1e1ea-156">Scaffold-DbContext</span></span>

<span data-ttu-id="1e1ea-157">Strutture una tipi DbContext ed entità per un database.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="1e1ea-158">Parametri:</span><span class="sxs-lookup"><span data-stu-id="1e1ea-158">Parameters:</span></span>

|                                          |                                                                           |
| ---------------------------------------- | ------------------------------------------------------------------------- |
| <span data-ttu-id="1e1ea-159"><nobr>***-Connessione*** \<stringa ></nobr></span><span class="sxs-lookup"><span data-stu-id="1e1ea-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="1e1ea-160">La stringa di connessione al database.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-160">The connection string to the database.</span></span>                                    |
| <span data-ttu-id="1e1ea-161">***-Provider*** \<stringa ></span><span class="sxs-lookup"><span data-stu-id="1e1ea-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="1e1ea-162">Il provider da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-162">The provider to use.</span></span> <span data-ttu-id="1e1ea-163">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="1e1ea-163">(E.g.</span></span> <span data-ttu-id="1e1ea-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="1e1ea-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>       |
| <span data-ttu-id="1e1ea-165">-OutputDir \<stringa ></span><span class="sxs-lookup"><span data-stu-id="1e1ea-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="1e1ea-166">Della directory in cui inserire i file in.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-166">The directory to put files in.</span></span> <span data-ttu-id="1e1ea-167">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-167">Paths are relative to the project directory.</span></span> |
| <span data-ttu-id="1e1ea-168">-Contesto \<stringa ></span><span class="sxs-lookup"><span data-stu-id="1e1ea-168">-Context \<String></span></span>                       | <span data-ttu-id="1e1ea-169">Il nome di DbContext per generare.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-169">The name of the DbContext to generate.</span></span>                                    |
| <span data-ttu-id="1e1ea-170">-Gli schemi \<String [] ></span><span class="sxs-lookup"><span data-stu-id="1e1ea-170">-Schemas \<String[]></span></span>                     | <span data-ttu-id="1e1ea-171">Gli schemi delle tabelle per generare i tipi di entità per.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-171">The schemas of tables to generate entity types for.</span></span>                       |
| <span data-ttu-id="1e1ea-172">-Tabelle \<String [] ></span><span class="sxs-lookup"><span data-stu-id="1e1ea-172">-Tables \<String[]></span></span>                      | <span data-ttu-id="1e1ea-173">Generare tipi di entità per le tabelle.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-173">The tables to generate entity types for.</span></span>                                  |
| <span data-ttu-id="1e1ea-174">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="1e1ea-174">-DataAnnotations</span></span>                         | <span data-ttu-id="1e1ea-175">Utilizzare gli attributi per configurare il modello (dove possibile).</span><span class="sxs-lookup"><span data-stu-id="1e1ea-175">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="1e1ea-176">Se omesso, viene utilizzato solo l'API fluent.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-176">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="1e1ea-177">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="1e1ea-177">-UseDatabaseNames</span></span>                        | <span data-ttu-id="1e1ea-178">Utilizzare nomi di tabella e colonna direttamente dal database.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-178">Use table and column names directly from the database.</span></span>                    |
| <span data-ttu-id="1e1ea-179">-Force</span><span class="sxs-lookup"><span data-stu-id="1e1ea-179">-Force</span></span>                                   | <span data-ttu-id="1e1ea-180">Sovrascrivi file esistenti.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-180">Overwrite existing files.</span></span>                                                 |

### <a name="script-migration"></a><span data-ttu-id="1e1ea-181">Migrazione di script</span><span class="sxs-lookup"><span data-stu-id="1e1ea-181">Script-Migration</span></span>

<span data-ttu-id="1e1ea-182">Genera uno script SQL dalla migrazione.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-182">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="1e1ea-183">Parametri:</span><span class="sxs-lookup"><span data-stu-id="1e1ea-183">Parameters:</span></span>

|                   |                                                                    |
| ----------------- | ------------------------------------------------------------------ |
| <span data-ttu-id="1e1ea-184">*-Da* \<stringa ></span><span class="sxs-lookup"><span data-stu-id="1e1ea-184">*-From* \<String></span></span> | <span data-ttu-id="1e1ea-185">La migrazione inizia.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-185">The starting migration.</span></span> <span data-ttu-id="1e1ea-186">Il valore predefinito 0 (il database iniziale).</span><span class="sxs-lookup"><span data-stu-id="1e1ea-186">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="1e1ea-187">*-A* \<stringa ></span><span class="sxs-lookup"><span data-stu-id="1e1ea-187">*-To* \<String></span></span>   | <span data-ttu-id="1e1ea-188">La migrazione finale.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-188">The ending migration.</span></span> <span data-ttu-id="1e1ea-189">Per impostazione predefinita all'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-189">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="1e1ea-190">-Idempotente</span><span class="sxs-lookup"><span data-stu-id="1e1ea-190">-Idempotent</span></span>       | <span data-ttu-id="1e1ea-191">Generare uno script che può essere usato in un database in ogni operazione di migrazione.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-191">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="1e1ea-192">-Output \<stringa ></span><span class="sxs-lookup"><span data-stu-id="1e1ea-192">-Output \<String></span></span> | <span data-ttu-id="1e1ea-193">File in cui scrivere il risultato.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-193">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="1e1ea-194">To, From, e i parametri di Output supportano l'espansione tramite tab.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-194">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="1e1ea-195">Update-Database</span><span class="sxs-lookup"><span data-stu-id="1e1ea-195">Update-Database</span></span>

|                                     |                                                                                |
| ----------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="1e1ea-196"><nobr>*-Migrazione* \<stringa ></nobr></span><span class="sxs-lookup"><span data-stu-id="1e1ea-196"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="1e1ea-197">La migrazione di destinazione.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-197">The target migration.</span></span> <span data-ttu-id="1e1ea-198">Se è '0', verranno annullate tutte le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-198">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="1e1ea-199">Per impostazione predefinita all'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-199">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="1e1ea-200">Il parametro di migrazione supporta l'espansione tramite tab.</span><span class="sxs-lookup"><span data-stu-id="1e1ea-200">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
