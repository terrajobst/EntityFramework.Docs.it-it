---
title: Console di gestione pacchetti (Visual Studio) - Core a Entity Framework
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: a53455a78db4bc504c45abafdacf9a15381f608e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812560"
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="857e4-102">Strumenti di Entity Framework Core Package Manager Console</span><span class="sxs-lookup"><span data-stu-id="857e4-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="857e4-103">Gli strumenti di Entity Framework Core Package Manager Console (PMC) eseguito all'interno di Visual Studio tramite NuGet [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="857e4-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="857e4-104">Questi strumenti funzionano sia con i progetti .NET Framework che con i progetti .NET Core.</span><span class="sxs-lookup"><span data-stu-id="857e4-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="857e4-105">Non si usa Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="857e4-105">Not using Visual Studio?</span></span> <span data-ttu-id="857e4-106">Il [strumenti da riga di comando di base EF] [ 1] sono multipiattaforma e vengono eseguiti all'interno di un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="857e4-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="857e4-107">Installazione degli strumenti</span><span class="sxs-lookup"><span data-stu-id="857e4-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="857e4-108">Installare gli strumenti di Entity Framework Core Package Manager Console installando il pacchetto Microsoft.EntityFrameworkCore.Tools NuGet.</span><span class="sxs-lookup"><span data-stu-id="857e4-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="857e4-109">È possibile installarlo eseguendo il comando seguente all'interno di [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="857e4-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="857e4-110">Se tutto funziona correttamente, deve essere in grado di eseguire questo comando:</span><span class="sxs-lookup"><span data-stu-id="857e4-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="857e4-111">Se il progetto di avvio è destinato a .NET Standard [cross-destinazione un framework supportati] [ 3] prima di utilizzare gli strumenti.</span><span class="sxs-lookup"><span data-stu-id="857e4-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="857e4-112">Se si utilizza **Windows universale** o **Xamarin**, spostare il codice di Entity Framework in una libreria di classi .NET Standard e [cross-destinazione un framework supportati] [ 3] prima di utilizzare gli strumenti.</span><span class="sxs-lookup"><span data-stu-id="857e4-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="857e4-113">Specificare la libreria di classi come progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="857e4-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="857e4-114">Utilizzo degli strumenti</span><span class="sxs-lookup"><span data-stu-id="857e4-114">Using the tools</span></span>
---------------
<span data-ttu-id="857e4-115">Ogni volta che si richiama un comando, vengono utilizzate due progetti:</span><span class="sxs-lookup"><span data-stu-id="857e4-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="857e4-116">Il progetto di destinazione è dove vengono aggiunti, in alcuni casi rimossi, tutti i file.</span><span class="sxs-lookup"><span data-stu-id="857e4-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="857e4-117">Per impostazione predefinita il progetto di destinazione di **progetto predefinito** selezionato nella Console di gestione pacchetti, ma può anche essere specificato utilizzando il progetto parametro-.</span><span class="sxs-lookup"><span data-stu-id="857e4-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="857e4-118">Il progetto di avvio è quello emulato dagli strumenti quando si esegue il codice del progetto.</span><span class="sxs-lookup"><span data-stu-id="857e4-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="857e4-119">Per impostazione predefinita uno **imposta come progetto di avvio** in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="857e4-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="857e4-120">Può anche essere specificato utilizzando il parametro - proprietà.</span><span class="sxs-lookup"><span data-stu-id="857e4-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="857e4-121">Parametri comuni:</span><span class="sxs-lookup"><span data-stu-id="857e4-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="857e4-122">-Contesto \<stringa ></span><span class="sxs-lookup"><span data-stu-id="857e4-122">-Context \<String></span></span>        | <span data-ttu-id="857e4-123">L'elemento DbContext da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="857e4-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="857e4-124">-Progetto \<stringa ></span><span class="sxs-lookup"><span data-stu-id="857e4-124">-Project \<String></span></span>        | <span data-ttu-id="857e4-125">Il progetto da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="857e4-125">The project to use.</span></span>         |
| <span data-ttu-id="857e4-126">Proprietà - \<stringa ></span><span class="sxs-lookup"><span data-stu-id="857e4-126">-StartupProject \<String></span></span> | <span data-ttu-id="857e4-127">Il progetto di avvio da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="857e4-127">The startup project to use.</span></span> |
| <span data-ttu-id="857e4-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="857e4-128">-Verbose</span></span>                  | <span data-ttu-id="857e4-129">Mostra l'output dettagliato.</span><span class="sxs-lookup"><span data-stu-id="857e4-129">Show verbose output.</span></span>        |

<span data-ttu-id="857e4-130">Per visualizzare informazioni della Guida su un comando, utilizzare PowerShell `Get-Help` comando.</span><span class="sxs-lookup"><span data-stu-id="857e4-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="857e4-131">I parametri di contesto, del progetto e proprietà supportano l'espansione tramite tab.</span><span class="sxs-lookup"><span data-stu-id="857e4-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="857e4-132">Impostare **env:ASPNETCORE_ENVIRONMENT** prima di eseguire per specificare l'ambiente di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="857e4-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="857e4-133">Comandi:</span><span class="sxs-lookup"><span data-stu-id="857e4-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="857e4-134">Aggiungere la migrazione</span><span class="sxs-lookup"><span data-stu-id="857e4-134">Add-Migration</span></span>

<span data-ttu-id="857e4-135">Aggiunge una nuova migrazione.</span><span class="sxs-lookup"><span data-stu-id="857e4-135">Adds a new migration.</span></span>

<span data-ttu-id="857e4-136">Parametri:</span><span class="sxs-lookup"><span data-stu-id="857e4-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="857e4-137">***-Nome*** \<stringa ></span><span class="sxs-lookup"><span data-stu-id="857e4-137">***-Name*** \<String></span></span>             | <span data-ttu-id="857e4-138">Il nome della migrazione.</span><span class="sxs-lookup"><span data-stu-id="857e4-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="857e4-139"><nobr>-OutputDir \<stringa ></nobr></span><span class="sxs-lookup"><span data-stu-id="857e4-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="857e4-140">La directory e spazio dei nomi secondario da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="857e4-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="857e4-141">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="857e4-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="857e4-142">Valore predefinito è "Migrazione".</span><span class="sxs-lookup"><span data-stu-id="857e4-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="857e4-143">I parametri in **grassetto** sono necessari e quelle in *corsivo* sono posizionali.</span><span class="sxs-lookup"><span data-stu-id="857e4-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="857e4-144">Eliminazione di Database</span><span class="sxs-lookup"><span data-stu-id="857e4-144">Drop-Database</span></span>

<span data-ttu-id="857e4-145">Elimina il database.</span><span class="sxs-lookup"><span data-stu-id="857e4-145">Drops the database.</span></span>

<span data-ttu-id="857e4-146">Parametri:</span><span class="sxs-lookup"><span data-stu-id="857e4-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="857e4-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="857e4-147">-WhatIf</span></span> | <span data-ttu-id="857e4-148">Mostra il database verrà rimossa, ma non l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="857e4-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="857e4-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="857e4-149">Get-DbContext</span></span>

<span data-ttu-id="857e4-150">Ottiene informazioni sul tipo DbContext.</span><span class="sxs-lookup"><span data-stu-id="857e4-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="857e4-151">Remove-migrazione</span><span class="sxs-lookup"><span data-stu-id="857e4-151">Remove-Migration</span></span>

<span data-ttu-id="857e4-152">Rimuove l'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="857e4-152">Removes the last migration.</span></span>

<span data-ttu-id="857e4-153">Parametri:</span><span class="sxs-lookup"><span data-stu-id="857e4-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="857e4-154">-Force</span><span class="sxs-lookup"><span data-stu-id="857e4-154">-Force</span></span> | <span data-ttu-id="857e4-155">Ripristinare la migrazione, se è stato applicato al database.</span><span class="sxs-lookup"><span data-stu-id="857e4-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="857e4-156">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="857e4-156">Scaffold-DbContext</span></span>

<span data-ttu-id="857e4-157">Strutture una tipi DbContext ed entità per un database.</span><span class="sxs-lookup"><span data-stu-id="857e4-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="857e4-158">Parametri:</span><span class="sxs-lookup"><span data-stu-id="857e4-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="857e4-159"><nobr>***-Connessione*** \<stringa ></nobr></span><span class="sxs-lookup"><span data-stu-id="857e4-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="857e4-160">La stringa di connessione al database.</span><span class="sxs-lookup"><span data-stu-id="857e4-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="857e4-161">***-Provider*** \<stringa ></span><span class="sxs-lookup"><span data-stu-id="857e4-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="857e4-162">Il provider da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="857e4-162">The provider to use.</span></span> <span data-ttu-id="857e4-163">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="857e4-163">(E.g.</span></span> <span data-ttu-id="857e4-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="857e4-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>                              |
| <span data-ttu-id="857e4-165">-OutputDir \<stringa ></span><span class="sxs-lookup"><span data-stu-id="857e4-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="857e4-166">Della directory in cui inserire i file in.</span><span class="sxs-lookup"><span data-stu-id="857e4-166">The directory to put files in.</span></span> <span data-ttu-id="857e4-167">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="857e4-167">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="857e4-168">-ContextDir \<stringa ></span><span class="sxs-lookup"><span data-stu-id="857e4-168">-ContextDir \<String></span></span>                    | <span data-ttu-id="857e4-169">Della directory in cui inserire file DbContext in.</span><span class="sxs-lookup"><span data-stu-id="857e4-169">The directory to put DbContext file in.</span></span> <span data-ttu-id="857e4-170">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="857e4-170">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="857e4-171">-Contesto \<stringa ></span><span class="sxs-lookup"><span data-stu-id="857e4-171">-Context \<String></span></span>                       | <span data-ttu-id="857e4-172">Il nome di DbContext per generare.</span><span class="sxs-lookup"><span data-stu-id="857e4-172">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="857e4-173">-Gli schemi \<String [] ></span><span class="sxs-lookup"><span data-stu-id="857e4-173">-Schemas \<String[]></span></span>                     | <span data-ttu-id="857e4-174">Gli schemi delle tabelle per generare i tipi di entità per.</span><span class="sxs-lookup"><span data-stu-id="857e4-174">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="857e4-175">-Tabelle \<String [] ></span><span class="sxs-lookup"><span data-stu-id="857e4-175">-Tables \<String[]></span></span>                      | <span data-ttu-id="857e4-176">Generare tipi di entità per le tabelle.</span><span class="sxs-lookup"><span data-stu-id="857e4-176">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="857e4-177">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="857e4-177">-DataAnnotations</span></span>                         | <span data-ttu-id="857e4-178">Utilizzare gli attributi per configurare il modello (dove possibile).</span><span class="sxs-lookup"><span data-stu-id="857e4-178">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="857e4-179">Se omesso, viene utilizzato solo l'API fluent.</span><span class="sxs-lookup"><span data-stu-id="857e4-179">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="857e4-180">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="857e4-180">-UseDatabaseNames</span></span>                        | <span data-ttu-id="857e4-181">Utilizzare nomi di tabella e colonna direttamente dal database.</span><span class="sxs-lookup"><span data-stu-id="857e4-181">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="857e4-182">-Force</span><span class="sxs-lookup"><span data-stu-id="857e4-182">-Force</span></span>                                   | <span data-ttu-id="857e4-183">Sovrascrivi file esistenti.</span><span class="sxs-lookup"><span data-stu-id="857e4-183">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="857e4-184">Migrazione di script</span><span class="sxs-lookup"><span data-stu-id="857e4-184">Script-Migration</span></span>

<span data-ttu-id="857e4-185">Genera uno script SQL dalla migrazione.</span><span class="sxs-lookup"><span data-stu-id="857e4-185">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="857e4-186">Parametri:</span><span class="sxs-lookup"><span data-stu-id="857e4-186">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="857e4-187">*-Da* \<stringa ></span><span class="sxs-lookup"><span data-stu-id="857e4-187">*-From* \<String></span></span> | <span data-ttu-id="857e4-188">La migrazione inizia.</span><span class="sxs-lookup"><span data-stu-id="857e4-188">The starting migration.</span></span> <span data-ttu-id="857e4-189">Il valore predefinito 0 (il database iniziale).</span><span class="sxs-lookup"><span data-stu-id="857e4-189">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="857e4-190">*-A* \<stringa ></span><span class="sxs-lookup"><span data-stu-id="857e4-190">*-To* \<String></span></span>   | <span data-ttu-id="857e4-191">La migrazione finale.</span><span class="sxs-lookup"><span data-stu-id="857e4-191">The ending migration.</span></span> <span data-ttu-id="857e4-192">Per impostazione predefinita all'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="857e4-192">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="857e4-193">-Idempotente</span><span class="sxs-lookup"><span data-stu-id="857e4-193">-Idempotent</span></span>       | <span data-ttu-id="857e4-194">Generare uno script che può essere usato in un database in ogni operazione di migrazione.</span><span class="sxs-lookup"><span data-stu-id="857e4-194">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="857e4-195">-Output \<stringa ></span><span class="sxs-lookup"><span data-stu-id="857e4-195">-Output \<String></span></span> | <span data-ttu-id="857e4-196">File in cui scrivere il risultato.</span><span class="sxs-lookup"><span data-stu-id="857e4-196">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="857e4-197">To, From, e i parametri di Output supportano l'espansione tramite tab.</span><span class="sxs-lookup"><span data-stu-id="857e4-197">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="857e4-198">Update-Database</span><span class="sxs-lookup"><span data-stu-id="857e4-198">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="857e4-199"><nobr>*-Migrazione* \<stringa ></nobr></span><span class="sxs-lookup"><span data-stu-id="857e4-199"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="857e4-200">La migrazione di destinazione.</span><span class="sxs-lookup"><span data-stu-id="857e4-200">The target migration.</span></span> <span data-ttu-id="857e4-201">Se è '0', verranno annullate tutte le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="857e4-201">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="857e4-202">Per impostazione predefinita all'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="857e4-202">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="857e4-203">Il parametro di migrazione supporta l'espansione tramite tab.</span><span class="sxs-lookup"><span data-stu-id="857e4-203">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
