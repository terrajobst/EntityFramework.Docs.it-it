---
title: Console di gestione pacchetti (Visual Studio) - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: 3d57a1665da2c94c55981d17e041b0ef74282496
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490850"
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="0b675-102">Strumenti della Console di gestione pacchetti di EF Core</span><span class="sxs-lookup"><span data-stu-id="0b675-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="0b675-103">Gli strumenti di Entity Framework Core Package Manager Console (console di gestione pacchetti) vengono eseguiti all'interno di Visual Studio usando NuGet [Console di gestione pacchetti][2].</span><span class="sxs-lookup"><span data-stu-id="0b675-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="0b675-104">Questi strumenti funzionano sia con i progetti .NET Framework che con i progetti .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0b675-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="0b675-105">Non si utilizza Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="0b675-105">Not using Visual Studio?</span></span> <span data-ttu-id="0b675-106">Il [degli strumenti della riga di comando di Entity Framework Core] [ 1] sono multipiattaforma e vengono eseguiti all'interno di un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="0b675-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="0b675-107">Installazione degli strumenti</span><span class="sxs-lookup"><span data-stu-id="0b675-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="0b675-108">Installare gli strumenti di Entity Framework Core Package Manager Console installando il pacchetto NuGet entityframeworkcore.</span><span class="sxs-lookup"><span data-stu-id="0b675-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="0b675-109">È possibile installarlo eseguendo il comando seguente all'interno [Console di gestione pacchetti][2].</span><span class="sxs-lookup"><span data-stu-id="0b675-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="0b675-110">Se tutto funziona correttamente, è necessario essere in grado di eseguire questo comando:</span><span class="sxs-lookup"><span data-stu-id="0b675-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="0b675-111">Se il progetto di avvio è destinata a .NET Standard [cross-destinazione un framework supportato] [ 3] prima di usare gli strumenti.</span><span class="sxs-lookup"><span data-stu-id="0b675-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0b675-112">Se si usa **Windows Universal** oppure **Xamarin**, spostare il codice di Entity Framework in una libreria di classi .NET Standard e [destinazione trasversale un framework supportato] [ 3] prima di usare gli strumenti.</span><span class="sxs-lookup"><span data-stu-id="0b675-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="0b675-113">Specificare la libreria di classi come progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="0b675-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="0b675-114">Usando gli strumenti</span><span class="sxs-lookup"><span data-stu-id="0b675-114">Using the tools</span></span>
---------------
<span data-ttu-id="0b675-115">Ogni volta che si richiama un comando, sono disponibili due progetti:</span><span class="sxs-lookup"><span data-stu-id="0b675-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="0b675-116">Il progetto di destinazione è dove vengono aggiunti, in alcuni casi rimossi, tutti i file.</span><span class="sxs-lookup"><span data-stu-id="0b675-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="0b675-117">Per impostazione predefinita il progetto di destinazione il **progetto predefinito** selezionato nella Console di gestione pacchetti, ma può anche essere specificata utilizzando-parametro del progetto.</span><span class="sxs-lookup"><span data-stu-id="0b675-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="0b675-118">Il progetto di avvio è quello emulato dagli strumenti quando si esegue il codice del progetto.</span><span class="sxs-lookup"><span data-stu-id="0b675-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="0b675-119">Per impostazione predefinita uno **imposta come progetto di avvio** in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="0b675-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="0b675-120">Può anche essere specificato usando il parametro - StartupProject.</span><span class="sxs-lookup"><span data-stu-id="0b675-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="0b675-121">Parametri comuni:</span><span class="sxs-lookup"><span data-stu-id="0b675-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="0b675-122">-Contesto \<stringa ></span><span class="sxs-lookup"><span data-stu-id="0b675-122">-Context \<String></span></span>        | <span data-ttu-id="0b675-123">DbContext da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="0b675-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="0b675-124">-Progetto \<stringa ></span><span class="sxs-lookup"><span data-stu-id="0b675-124">-Project \<String></span></span>        | <span data-ttu-id="0b675-125">Il progetto da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="0b675-125">The project to use.</span></span>         |
| <span data-ttu-id="0b675-126">-StartupProject \<stringa ></span><span class="sxs-lookup"><span data-stu-id="0b675-126">-StartupProject \<String></span></span> | <span data-ttu-id="0b675-127">Il progetto di avvio da usare.</span><span class="sxs-lookup"><span data-stu-id="0b675-127">The startup project to use.</span></span> |
| <span data-ttu-id="0b675-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="0b675-128">-Verbose</span></span>                  | <span data-ttu-id="0b675-129">Visualizzare output dettagliato.</span><span class="sxs-lookup"><span data-stu-id="0b675-129">Show verbose output.</span></span>        |

<span data-ttu-id="0b675-130">Per visualizzare la Guida informazioni su un comando, usare PowerShell `Get-Help` comando.</span><span class="sxs-lookup"><span data-stu-id="0b675-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="0b675-131">I parametri di contesto, progetto e oggetto StartupProject supportano l'espansione tramite tab.</span><span class="sxs-lookup"><span data-stu-id="0b675-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="0b675-132">Impostare **env:ASPNETCORE_ENVIRONMENT** prima per specificare l'ambiente di ASP.NET Core in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="0b675-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="0b675-133">Comandi:</span><span class="sxs-lookup"><span data-stu-id="0b675-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="0b675-134">Add-Migration</span><span class="sxs-lookup"><span data-stu-id="0b675-134">Add-Migration</span></span>

<span data-ttu-id="0b675-135">Aggiunge una nuova migrazione.</span><span class="sxs-lookup"><span data-stu-id="0b675-135">Adds a new migration.</span></span>

<span data-ttu-id="0b675-136">Parametri:</span><span class="sxs-lookup"><span data-stu-id="0b675-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0b675-137">***-Name*** \<stringa ></span><span class="sxs-lookup"><span data-stu-id="0b675-137">***-Name*** \<String></span></span>             | <span data-ttu-id="0b675-138">Il nome della migrazione.</span><span class="sxs-lookup"><span data-stu-id="0b675-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="0b675-139"><nobr>-OutputDir \<stringa ></nobr></span><span class="sxs-lookup"><span data-stu-id="0b675-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="0b675-140">La directory e sub-spazio dei nomi da usare.</span><span class="sxs-lookup"><span data-stu-id="0b675-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="0b675-141">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="0b675-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="0b675-142">Il valore predefinito è "Migrazione".</span><span class="sxs-lookup"><span data-stu-id="0b675-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="0b675-143">I parametri in **bold** sono obbligatori e quelle nella *corsivo* sono posizionali.</span><span class="sxs-lookup"><span data-stu-id="0b675-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="0b675-144">Eliminazione di Database</span><span class="sxs-lookup"><span data-stu-id="0b675-144">Drop-Database</span></span>

<span data-ttu-id="0b675-145">Elimina il database.</span><span class="sxs-lookup"><span data-stu-id="0b675-145">Drops the database.</span></span>

<span data-ttu-id="0b675-146">Parametri:</span><span class="sxs-lookup"><span data-stu-id="0b675-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="0b675-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="0b675-147">-WhatIf</span></span> | <span data-ttu-id="0b675-148">Mostra in quale database verrà rimossa, ma non eliminarlo.</span><span class="sxs-lookup"><span data-stu-id="0b675-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="0b675-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="0b675-149">Get-DbContext</span></span>

<span data-ttu-id="0b675-150">Ottiene informazioni su un tipo DbContext.</span><span class="sxs-lookup"><span data-stu-id="0b675-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="0b675-151">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="0b675-151">Remove-Migration</span></span>

<span data-ttu-id="0b675-152">Rimuove l'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="0b675-152">Removes the last migration.</span></span>

<span data-ttu-id="0b675-153">Parametri:</span><span class="sxs-lookup"><span data-stu-id="0b675-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="0b675-154">-Force</span><span class="sxs-lookup"><span data-stu-id="0b675-154">-Force</span></span> | <span data-ttu-id="0b675-155">Se è stato applicato al database, ripristinare la migrazione.</span><span class="sxs-lookup"><span data-stu-id="0b675-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="0b675-156">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="0b675-156">Scaffold-DbContext</span></span>

<span data-ttu-id="0b675-157">Esegue lo scaffolding di un tipi DbContext ed entità per un database.</span><span class="sxs-lookup"><span data-stu-id="0b675-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="0b675-158">Parametri:</span><span class="sxs-lookup"><span data-stu-id="0b675-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0b675-159"><nobr>***-Connection*** \<stringa ></nobr></span><span class="sxs-lookup"><span data-stu-id="0b675-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="0b675-160">La stringa di connessione al database.</span><span class="sxs-lookup"><span data-stu-id="0b675-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="0b675-161">***-Provider*** \<stringa ></span><span class="sxs-lookup"><span data-stu-id="0b675-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="0b675-162">Il provider da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="0b675-162">The provider to use.</span></span> <span data-ttu-id="0b675-163">(ad esempio, l'entityframeworkcore)</span><span class="sxs-lookup"><span data-stu-id="0b675-163">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span>                      |
| <span data-ttu-id="0b675-164">-OutputDir \<stringa ></span><span class="sxs-lookup"><span data-stu-id="0b675-164">-OutputDir \<String></span></span>                     | <span data-ttu-id="0b675-165">La directory in cui inserire file nella.</span><span class="sxs-lookup"><span data-stu-id="0b675-165">The directory to put files in.</span></span> <span data-ttu-id="0b675-166">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="0b675-166">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="0b675-167">-ContextDir \<stringa ></span><span class="sxs-lookup"><span data-stu-id="0b675-167">-ContextDir \<String></span></span>                    | <span data-ttu-id="0b675-168">La directory in cui inserire file DbContext in.</span><span class="sxs-lookup"><span data-stu-id="0b675-168">The directory to put DbContext file in.</span></span> <span data-ttu-id="0b675-169">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="0b675-169">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="0b675-170">-Contesto \<stringa ></span><span class="sxs-lookup"><span data-stu-id="0b675-170">-Context \<String></span></span>                       | <span data-ttu-id="0b675-171">Il nome di DbContext da generare.</span><span class="sxs-lookup"><span data-stu-id="0b675-171">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="0b675-172">-Schemi \<String [] ></span><span class="sxs-lookup"><span data-stu-id="0b675-172">-Schemas \<String[]></span></span>                     | <span data-ttu-id="0b675-173">Gli schemi delle tabelle per generare i tipi di entità per.</span><span class="sxs-lookup"><span data-stu-id="0b675-173">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="0b675-174">-Tabelle \<String [] ></span><span class="sxs-lookup"><span data-stu-id="0b675-174">-Tables \<String[]></span></span>                      | <span data-ttu-id="0b675-175">Generare tipi di entità per le tabelle.</span><span class="sxs-lookup"><span data-stu-id="0b675-175">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="0b675-176">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="0b675-176">-DataAnnotations</span></span>                         | <span data-ttu-id="0b675-177">Usare gli attributi per configurare il modello (se possibile).</span><span class="sxs-lookup"><span data-stu-id="0b675-177">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="0b675-178">Se omesso, viene utilizzato solo l'API fluent.</span><span class="sxs-lookup"><span data-stu-id="0b675-178">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="0b675-179">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="0b675-179">-UseDatabaseNames</span></span>                        | <span data-ttu-id="0b675-180">Usare nomi di tabella e colonna direttamente dal database.</span><span class="sxs-lookup"><span data-stu-id="0b675-180">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="0b675-181">-Force</span><span class="sxs-lookup"><span data-stu-id="0b675-181">-Force</span></span>                                   | <span data-ttu-id="0b675-182">Sovrascrivi file esistenti.</span><span class="sxs-lookup"><span data-stu-id="0b675-182">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="0b675-183">Migrazione degli script</span><span class="sxs-lookup"><span data-stu-id="0b675-183">Script-Migration</span></span>

<span data-ttu-id="0b675-184">Genera uno script SQL dalle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="0b675-184">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="0b675-185">Parametri:</span><span class="sxs-lookup"><span data-stu-id="0b675-185">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="0b675-186">*-From* \<stringa ></span><span class="sxs-lookup"><span data-stu-id="0b675-186">*-From* \<String></span></span> | <span data-ttu-id="0b675-187">La migrazione inizia.</span><span class="sxs-lookup"><span data-stu-id="0b675-187">The starting migration.</span></span> <span data-ttu-id="0b675-188">Il valore predefinito è 0 (il database iniziale).</span><span class="sxs-lookup"><span data-stu-id="0b675-188">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="0b675-189">*-A* \<stringa ></span><span class="sxs-lookup"><span data-stu-id="0b675-189">*-To* \<String></span></span>   | <span data-ttu-id="0b675-190">La migrazione finale.</span><span class="sxs-lookup"><span data-stu-id="0b675-190">The ending migration.</span></span> <span data-ttu-id="0b675-191">Per impostazione predefinita all'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="0b675-191">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="0b675-192">-Idempotenti</span><span class="sxs-lookup"><span data-stu-id="0b675-192">-Idempotent</span></span>       | <span data-ttu-id="0b675-193">Generare uno script che può essere utilizzato in un database in ogni operazione di migrazione.</span><span class="sxs-lookup"><span data-stu-id="0b675-193">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="0b675-194">-Output \<stringa ></span><span class="sxs-lookup"><span data-stu-id="0b675-194">-Output \<String></span></span> | <span data-ttu-id="0b675-195">File in cui scrivere il risultato.</span><span class="sxs-lookup"><span data-stu-id="0b675-195">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="0b675-196">To, From, e i parametri di Output supportano espansione tramite tab.</span><span class="sxs-lookup"><span data-stu-id="0b675-196">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="0b675-197">Update-Database</span><span class="sxs-lookup"><span data-stu-id="0b675-197">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="0b675-198"><nobr>*-Migrazione* \<stringa ></nobr></span><span class="sxs-lookup"><span data-stu-id="0b675-198"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="0b675-199">La migrazione di destinazione.</span><span class="sxs-lookup"><span data-stu-id="0b675-199">The target migration.</span></span> <span data-ttu-id="0b675-200">Se è '0', verranno ripristinate tutte le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="0b675-200">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="0b675-201">Per impostazione predefinita all'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="0b675-201">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="0b675-202">Il parametro di migrazione supporta l'espansione tramite tab.</span><span class="sxs-lookup"><span data-stu-id="0b675-202">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
