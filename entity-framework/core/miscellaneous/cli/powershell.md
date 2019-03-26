---
title: EF Core riferimenti per gli strumenti (Console di gestione pacchetti) - EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: cb05e3fb66adf96f8a6778711a76520d0be24c71
ms.sourcegitcommit: 645785187ae23ddf7d7b0642c7a4da5ffb0c7f30
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419770"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="7faf4-102">Riferimenti - Console di gestione pacchetti in Visual Studio agli strumenti di Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="7faf4-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="7faf4-103">Gli strumenti di Package Manager Console (console di gestione pacchetti) per Entity Framework Core eseguono attività di sviluppo in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="7faf4-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="7faf4-104">Ad esempio, creano [migrazioni](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), applicare le migrazioni e generare il codice per un modello basato su un database esistente.</span><span class="sxs-lookup"><span data-stu-id="7faf4-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="7faf4-105">I comandi vengono eseguiti all'interno di Visual Studio usando il [Console di gestione pacchetti](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="7faf4-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="7faf4-106">Questi strumenti funzionano sia con i progetti .NET Framework che con i progetti .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7faf4-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="7faf4-107">Se non si usa Visual Studio, è consigliabile la [degli strumenti della riga di comando di EF Core](dotnet.md) invece.</span><span class="sxs-lookup"><span data-stu-id="7faf4-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="7faf4-108">Gli strumenti dell'interfaccia della riga sono multipiattaforma e vengono eseguiti all'interno di un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="7faf4-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="7faf4-109">Installazione degli strumenti</span><span class="sxs-lookup"><span data-stu-id="7faf4-109">Installing the tools</span></span>

<span data-ttu-id="7faf4-110">Le procedure per l'installazione e gli strumenti di aggiornamento differiscono tra ASP.NET Core 2.1 e versioni precedenti o altri tipi di progetto.</span><span class="sxs-lookup"><span data-stu-id="7faf4-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="7faf4-111">ASP.NET Core 2.1 e versioni successive</span><span class="sxs-lookup"><span data-stu-id="7faf4-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="7faf4-112">Gli strumenti sono inclusi automaticamente in un progetto ASP.NET Core 2.1 + in quanto il `Microsoft.EntityFrameworkCore.Tools` incluso nel pacchetto la [Microsoft.AspNetCore.App metapacchetto](/aspnet/core/fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="7faf4-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="7faf4-113">Pertanto, non devi eseguire alcuna operazione per installare gli strumenti, ma è necessario:</span><span class="sxs-lookup"><span data-stu-id="7faf4-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>
* <span data-ttu-id="7faf4-114">Ripristinare i pacchetti prima di usare gli strumenti in un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="7faf4-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="7faf4-115">Installare un pacchetto per aggiornare gli strumenti a una versione più recente.</span><span class="sxs-lookup"><span data-stu-id="7faf4-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="7faf4-116">Per essere certi di ottenere la versione più recente degli strumenti, è consigliabile eseguire anche il passaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="7faf4-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="7faf4-117">Modificare il *csproj* del file e aggiungere una riga che specifica la versione più recente del [entityframeworkcore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) pacchetto.</span><span class="sxs-lookup"><span data-stu-id="7faf4-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="7faf4-118">Ad esempio, il *csproj* può includere file un `ItemGroup` simile alla seguente:</span><span class="sxs-lookup"><span data-stu-id="7faf4-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="7faf4-119">Aggiornare gli strumenti quando viene visualizzato un messaggio simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="7faf4-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="7faf4-120">La versione degli strumenti di Entity Framework Core '2.1.1-rtm-30846' è precedente a quella del runtime di '2.1.3-rtm-32065'.</span><span class="sxs-lookup"><span data-stu-id="7faf4-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="7faf4-121">Aggiornare gli strumenti per le ultime funzionalità e correzioni di bug.</span><span class="sxs-lookup"><span data-stu-id="7faf4-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="7faf4-122">Per aggiornare gli strumenti:</span><span class="sxs-lookup"><span data-stu-id="7faf4-122">To update the tools:</span></span>
* <span data-ttu-id="7faf4-123">Installare la versione più recente di .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="7faf4-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="7faf4-124">Aggiornare Visual Studio alla versione più recente.</span><span class="sxs-lookup"><span data-stu-id="7faf4-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="7faf4-125">Modificare il *file con estensione csproj* file in modo che includa un riferimento al pacchetto per il pacchetto di strumenti più recente, come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="7faf4-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="7faf4-126">Le altre versioni e i tipi di progetto</span><span class="sxs-lookup"><span data-stu-id="7faf4-126">Other versions and project types</span></span>

<span data-ttu-id="7faf4-127">Installare gli strumenti della Console di gestione pacchetti eseguendo il comando seguente nella **Console di gestione pacchetti**:</span><span class="sxs-lookup"><span data-stu-id="7faf4-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="7faf4-128">Aggiornare gli strumenti eseguendo il comando seguente nella **Console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="7faf4-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="7faf4-129">Verificare l'installazione</span><span class="sxs-lookup"><span data-stu-id="7faf4-129">Verify the installation</span></span>

<span data-ttu-id="7faf4-130">Verificare che gli strumenti vengono installati eseguendo questo comando:</span><span class="sxs-lookup"><span data-stu-id="7faf4-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="7faf4-131">L'output è simile al seguente (non consente di capire quale versione degli strumenti in uso):</span><span class="sxs-lookup"><span data-stu-id="7faf4-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

TOPIC
    about_EntityFrameworkCore

SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

<A list of available commands follows, omitted here.>
```

## <a name="using-the-tools"></a><span data-ttu-id="7faf4-132">Usando gli strumenti</span><span class="sxs-lookup"><span data-stu-id="7faf4-132">Using the tools</span></span>

<span data-ttu-id="7faf4-133">Prima di usare gli strumenti:</span><span class="sxs-lookup"><span data-stu-id="7faf4-133">Before using the tools:</span></span>
* <span data-ttu-id="7faf4-134">Comprendere la differenza tra progetti di avvio e di destinazione.</span><span class="sxs-lookup"><span data-stu-id="7faf4-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="7faf4-135">Informazioni su come usare gli strumenti con le librerie di classi .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="7faf4-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="7faf4-136">Per i progetti ASP.NET Core, impostare l'ambiente.</span><span class="sxs-lookup"><span data-stu-id="7faf4-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="7faf4-137">Progetto di avvio e di destinazione</span><span class="sxs-lookup"><span data-stu-id="7faf4-137">Target and startup project</span></span>

<span data-ttu-id="7faf4-138">I comandi di fare riferimento a un *project* e una *progetto di avvio*.</span><span class="sxs-lookup"><span data-stu-id="7faf4-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="7faf4-139">Il *project* è noto anche come il *progetto di destinazione* perché è in cui i comandi aggiungono o rimuovono file.</span><span class="sxs-lookup"><span data-stu-id="7faf4-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="7faf4-140">Per impostazione predefinita, il **progetto predefinito** selezionato in **Console di gestione pacchetti** è il progetto di destinazione.</span><span class="sxs-lookup"><span data-stu-id="7faf4-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="7faf4-141">È possibile specificare un altro progetto come progetto di destinazione usando il <nobr> `--project` </nobr> opzione.</span><span class="sxs-lookup"><span data-stu-id="7faf4-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="7faf4-142">Il *progetto di avvio* è quello che gli strumenti di compilare ed eseguire.</span><span class="sxs-lookup"><span data-stu-id="7faf4-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="7faf4-143">Gli strumenti richiede l'esecuzione di codice dell'applicazione in fase di progettazione per ottenere informazioni sul progetto, ad esempio la stringa di connessione di database e la configurazione del modello.</span><span class="sxs-lookup"><span data-stu-id="7faf4-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="7faf4-144">Per impostazione predefinita, il **progetto di avvio** nelle **Esplora soluzioni** è il progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="7faf4-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="7faf4-145">È possibile specificare un altro progetto come progetto di avvio usando il <nobr> `--startup-project` </nobr> opzione.</span><span class="sxs-lookup"><span data-stu-id="7faf4-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="7faf4-146">Il progetto di avvio e di un progetto di destinazione sono spesso dello stesso progetto.</span><span class="sxs-lookup"><span data-stu-id="7faf4-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="7faf4-147">Uno scenario tipico in cui sono progetti separati è quando:</span><span class="sxs-lookup"><span data-stu-id="7faf4-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="7faf4-148">Le classi di contesto ed entità di Entity Framework Core sono in una libreria di classi .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7faf4-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="7faf4-149">Un'app console .NET Core o un'app web fa riferimento alla libreria di classi.</span><span class="sxs-lookup"><span data-stu-id="7faf4-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="7faf4-150">È anche possibile [inserire il codice di migrazioni in una libreria di classi separata dal contesto di Entity Framework Core](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="7faf4-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="7faf4-151">Altri framework di destinazione</span><span class="sxs-lookup"><span data-stu-id="7faf4-151">Other target frameworks</span></span>

<span data-ttu-id="7faf4-152">Gli strumenti della Console di gestione pacchetti funzionano con i progetti .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7faf4-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="7faf4-153">Le app con il modello di EF Core in una libreria di classi .NET Standard potrebbero non avere un progetto di .NET Framework o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7faf4-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="7faf4-154">Ad esempio, questo vale delle App Xamarin e (Universal Windows Platform).</span><span class="sxs-lookup"><span data-stu-id="7faf4-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="7faf4-155">In questi casi, è possibile creare un progetto di app console .NET Core o .NET Framework il cui unico scopo è agire come progetto di avvio per gli strumenti.</span><span class="sxs-lookup"><span data-stu-id="7faf4-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="7faf4-156">Il progetto può essere un progetto fittizio senza vero codice &mdash; , è necessario soltanto per fornire una destinazione per gli strumenti.</span><span class="sxs-lookup"><span data-stu-id="7faf4-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="7faf4-157">Perché è un progetto fittizio necessario?</span><span class="sxs-lookup"><span data-stu-id="7faf4-157">Why is a dummy project required?</span></span> <span data-ttu-id="7faf4-158">Come accennato in precedenza, gli strumenti di dovranno eseguire il codice dell'applicazione in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="7faf4-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="7faf4-159">A tale scopo, è necessario usare il runtime di .NET Core o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="7faf4-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="7faf4-160">Quando il modello di EF Core è in un progetto destinato a .NET Core o .NET Framework, gli strumenti di Entity Framework Core presi in prestito il runtime dal progetto.</span><span class="sxs-lookup"><span data-stu-id="7faf4-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="7faf4-161">Che non possono eseguire se il modello di EF Core è in una libreria di classi .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="7faf4-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="7faf4-162">.NET Standard non è un'implementazione .NET effettiva; si tratta di una specifica di un set di API che le implementazioni di .NET devono supportare.</span><span class="sxs-lookup"><span data-stu-id="7faf4-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="7faf4-163">.NET Standard non è pertanto sufficiente per gli strumenti di Entity Framework Core eseguire il codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7faf4-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="7faf4-164">Il progetto fittizio creato per essere utilizzato come progetto di avvio fornisce una piattaforma di destinazione concreta in cui gli strumenti possono caricare la libreria di classi .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="7faf4-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="7faf4-165">Ambiente ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7faf4-165">ASP.NET Core environment</span></span>

<span data-ttu-id="7faf4-166">Per specificare l'ambiente per i progetti ASP.NET Core, impostare **env:ASPNETCORE_ENVIRONMENT** prima di eseguire comandi.</span><span class="sxs-lookup"><span data-stu-id="7faf4-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="7faf4-167">Parametri comuni</span><span class="sxs-lookup"><span data-stu-id="7faf4-167">Common parameters</span></span>

<span data-ttu-id="7faf4-168">Nella tabella seguente vengono illustrati i parametri che sono comuni a tutti i comandi di EF Core:</span><span class="sxs-lookup"><span data-stu-id="7faf4-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="7faf4-169">Parametro</span><span class="sxs-lookup"><span data-stu-id="7faf4-169">Parameter</span></span>                 | <span data-ttu-id="7faf4-170">Descrizione</span><span class="sxs-lookup"><span data-stu-id="7faf4-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7faf4-171">-Contesto \<stringa ></span><span class="sxs-lookup"><span data-stu-id="7faf4-171">-Context \<String></span></span>        | <span data-ttu-id="7faf4-172">Classe `DbContext` da usare.</span><span class="sxs-lookup"><span data-stu-id="7faf4-172">The `DbContext` class to use.</span></span> <span data-ttu-id="7faf4-173">Nome della classe solo o nome completo con gli spazi dei nomi.</span><span class="sxs-lookup"><span data-stu-id="7faf4-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="7faf4-174">Se questo parametro viene omesso, EF Core consente di trovare la classe del contesto.</span><span class="sxs-lookup"><span data-stu-id="7faf4-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="7faf4-175">Se sono presenti più classi di contesto, questo parametro è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="7faf4-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="7faf4-176">-Progetto \<stringa ></span><span class="sxs-lookup"><span data-stu-id="7faf4-176">-Project \<String></span></span>        | <span data-ttu-id="7faf4-177">Il progetto di destinazione.</span><span class="sxs-lookup"><span data-stu-id="7faf4-177">The target project.</span></span> <span data-ttu-id="7faf4-178">Se questo parametro viene omesso, il **progetto predefinito** per **Console di gestione pacchetti** viene usato come progetto di destinazione.</span><span class="sxs-lookup"><span data-stu-id="7faf4-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="7faf4-179">-StartupProject \<stringa ></span><span class="sxs-lookup"><span data-stu-id="7faf4-179">-StartupProject \<String></span></span> | <span data-ttu-id="7faf4-180">Il progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="7faf4-180">The startup project.</span></span> <span data-ttu-id="7faf4-181">Se questo parametro viene omesso, il **progetto di avvio** nelle **proprietà della soluzione** viene usato come progetto di destinazione.</span><span class="sxs-lookup"><span data-stu-id="7faf4-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="7faf4-182">-Verbose</span><span class="sxs-lookup"><span data-stu-id="7faf4-182">-Verbose</span></span>                  | <span data-ttu-id="7faf4-183">Visualizzare output dettagliato.</span><span class="sxs-lookup"><span data-stu-id="7faf4-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="7faf4-184">Per visualizzare la Guida informazioni su un comando, usare PowerShell `Get-Help` comando.</span><span class="sxs-lookup"><span data-stu-id="7faf4-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="7faf4-185">I parametri di contesto, progetto e oggetto StartupProject supportano l'espansione tramite tab.</span><span class="sxs-lookup"><span data-stu-id="7faf4-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="7faf4-186">Add-Migration</span><span class="sxs-lookup"><span data-stu-id="7faf4-186">Add-Migration</span></span>

<span data-ttu-id="7faf4-187">Aggiunge una nuova migrazione.</span><span class="sxs-lookup"><span data-stu-id="7faf4-187">Adds a new migration.</span></span>

<span data-ttu-id="7faf4-188">Parametri:</span><span class="sxs-lookup"><span data-stu-id="7faf4-188">Parameters:</span></span>

| <span data-ttu-id="7faf4-189">Parametro</span><span class="sxs-lookup"><span data-stu-id="7faf4-189">Parameter</span></span>                         | <span data-ttu-id="7faf4-190">Descrizione</span><span class="sxs-lookup"><span data-stu-id="7faf4-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7faf4-191"><nobr>-Nome \<stringa ><nobr></span><span class="sxs-lookup"><span data-stu-id="7faf4-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="7faf4-192">Il nome della migrazione.</span><span class="sxs-lookup"><span data-stu-id="7faf4-192">The name of the migration.</span></span> <span data-ttu-id="7faf4-193">Si tratta di un parametro posizionale ed è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="7faf4-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="7faf4-194"><nobr>-OutputDir \<stringa ></nobr></span><span class="sxs-lookup"><span data-stu-id="7faf4-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="7faf4-195">La directory e sub-spazio dei nomi da usare.</span><span class="sxs-lookup"><span data-stu-id="7faf4-195">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="7faf4-196">I percorsi sono relativi alla directory del progetto di destinazione.</span><span class="sxs-lookup"><span data-stu-id="7faf4-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="7faf4-197">Il valore predefinito è "Migrazione".</span><span class="sxs-lookup"><span data-stu-id="7faf4-197">Defaults to "Migrations".</span></span> |

## <a name="drop-database"></a><span data-ttu-id="7faf4-198">Eliminazione di Database</span><span class="sxs-lookup"><span data-stu-id="7faf4-198">Drop-Database</span></span>

<span data-ttu-id="7faf4-199">Elimina il database.</span><span class="sxs-lookup"><span data-stu-id="7faf4-199">Drops the database.</span></span>

<span data-ttu-id="7faf4-200">Parametri:</span><span class="sxs-lookup"><span data-stu-id="7faf4-200">Parameters:</span></span>

| <span data-ttu-id="7faf4-201">Parametro</span><span class="sxs-lookup"><span data-stu-id="7faf4-201">Parameter</span></span> | <span data-ttu-id="7faf4-202">Descrizione</span><span class="sxs-lookup"><span data-stu-id="7faf4-202">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="7faf4-203">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="7faf4-203">-WhatIf</span></span>   | <span data-ttu-id="7faf4-204">Mostra in quale database verrà rimossa, ma non eliminarlo.</span><span class="sxs-lookup"><span data-stu-id="7faf4-204">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="7faf4-205">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="7faf4-205">Get-DbContext</span></span>

<span data-ttu-id="7faf4-206">Ottiene informazioni su un `DbContext` tipo.</span><span class="sxs-lookup"><span data-stu-id="7faf4-206">Gets information about a `DbContext` type.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="7faf4-207">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="7faf4-207">Remove-Migration</span></span>

<span data-ttu-id="7faf4-208">Rimuove l'ultima migrazione (rollback le modifiche al codice che sono state eseguite per la migrazione).</span><span class="sxs-lookup"><span data-stu-id="7faf4-208">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="7faf4-209">Parametri:</span><span class="sxs-lookup"><span data-stu-id="7faf4-209">Parameters:</span></span>

| <span data-ttu-id="7faf4-210">Parametro</span><span class="sxs-lookup"><span data-stu-id="7faf4-210">Parameter</span></span> | <span data-ttu-id="7faf4-211">Descrizione</span><span class="sxs-lookup"><span data-stu-id="7faf4-211">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="7faf4-212">-Force</span><span class="sxs-lookup"><span data-stu-id="7faf4-212">-Force</span></span>    | <span data-ttu-id="7faf4-213">Ripristinare la migrazione (rollback delle modifiche che sono state applicate al database).</span><span class="sxs-lookup"><span data-stu-id="7faf4-213">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="7faf4-214">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="7faf4-214">Scaffold-DbContext</span></span>

<span data-ttu-id="7faf4-215">Genera il codice per un `DbContext` e tipi di entità per un database.</span><span class="sxs-lookup"><span data-stu-id="7faf4-215">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="7faf4-216">Affinché `Scaffold-DbContext` per generare un tipo di entità, la tabella di database deve avere una chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="7faf4-216">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="7faf4-217">Parametri:</span><span class="sxs-lookup"><span data-stu-id="7faf4-217">Parameters:</span></span>

| <span data-ttu-id="7faf4-218">Parametro</span><span class="sxs-lookup"><span data-stu-id="7faf4-218">Parameter</span></span>                          | <span data-ttu-id="7faf4-219">Descrizione</span><span class="sxs-lookup"><span data-stu-id="7faf4-219">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7faf4-220"><nobr>-Connection \<stringa ></nobr></span><span class="sxs-lookup"><span data-stu-id="7faf4-220"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="7faf4-221">La stringa di connessione al database.</span><span class="sxs-lookup"><span data-stu-id="7faf4-221">The connection string to the database.</span></span> <span data-ttu-id="7faf4-222">Per progetti ASP.NET Core 2.x, il valore può essere *nome =\<nome della stringa di connessione >*.</span><span class="sxs-lookup"><span data-stu-id="7faf4-222">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="7faf4-223">In tal caso il nome deriva dalle origini configurazione che sono configurati per il progetto.</span><span class="sxs-lookup"><span data-stu-id="7faf4-223">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="7faf4-224">Si tratta di un parametro posizionale ed è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="7faf4-224">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="7faf4-225"><nobr>-Provider \<stringa ></nobr></span><span class="sxs-lookup"><span data-stu-id="7faf4-225"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="7faf4-226">Il provider da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="7faf4-226">The provider to use.</span></span> <span data-ttu-id="7faf4-227">In genere si tratta del nome del pacchetto NuGet, ad esempio: `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="7faf4-227">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="7faf4-228">Si tratta di un parametro posizionale ed è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="7faf4-228">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="7faf4-229">-OutputDir \<stringa ></span><span class="sxs-lookup"><span data-stu-id="7faf4-229">-OutputDir \<String></span></span>               | <span data-ttu-id="7faf4-230">La directory in cui inserire file nella.</span><span class="sxs-lookup"><span data-stu-id="7faf4-230">The directory to put files in.</span></span> <span data-ttu-id="7faf4-231">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="7faf4-231">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="7faf4-232">-ContextDir \<stringa ></span><span class="sxs-lookup"><span data-stu-id="7faf4-232">-ContextDir \<String></span></span>              | <span data-ttu-id="7faf4-233">La directory di inserire il `DbContext` del file in.</span><span class="sxs-lookup"><span data-stu-id="7faf4-233">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="7faf4-234">I percorsi sono relativi alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="7faf4-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="7faf4-235">-Contesto \<stringa ></span><span class="sxs-lookup"><span data-stu-id="7faf4-235">-Context \<String></span></span>                 | <span data-ttu-id="7faf4-236">Il nome del `DbContext` classe da generare.</span><span class="sxs-lookup"><span data-stu-id="7faf4-236">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="7faf4-237">-Schemas \<String[]></span><span class="sxs-lookup"><span data-stu-id="7faf4-237">-Schemas \<String[]></span></span>               | <span data-ttu-id="7faf4-238">Gli schemi delle tabelle per generare i tipi di entità per.</span><span class="sxs-lookup"><span data-stu-id="7faf4-238">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="7faf4-239">Se questo parametro viene omesso, vengono inclusi tutti gli schemi.</span><span class="sxs-lookup"><span data-stu-id="7faf4-239">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="7faf4-240">-Tables \<String[]></span><span class="sxs-lookup"><span data-stu-id="7faf4-240">-Tables \<String[]></span></span>                | <span data-ttu-id="7faf4-241">Generare tipi di entità per le tabelle.</span><span class="sxs-lookup"><span data-stu-id="7faf4-241">The tables to generate entity types for.</span></span> <span data-ttu-id="7faf4-242">Se questo parametro viene omesso, vengono incluse tutte le tabelle.</span><span class="sxs-lookup"><span data-stu-id="7faf4-242">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="7faf4-243">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="7faf4-243">-DataAnnotations</span></span>                   | <span data-ttu-id="7faf4-244">Usare gli attributi per configurare il modello (se possibile).</span><span class="sxs-lookup"><span data-stu-id="7faf4-244">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="7faf4-245">Se questo parametro viene omesso, viene utilizzato solo l'API fluent.</span><span class="sxs-lookup"><span data-stu-id="7faf4-245">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="7faf4-246">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="7faf4-246">-UseDatabaseNames</span></span>                  | <span data-ttu-id="7faf4-247">Usare nomi di tabella e colonna esattamente come appaiono nel database.</span><span class="sxs-lookup"><span data-stu-id="7faf4-247">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="7faf4-248">Se questo parametro viene omesso, vengono modificati i nomi di database per più da vicino è conforme alle convenzioni di stile nome c#.</span><span class="sxs-lookup"><span data-stu-id="7faf4-248">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="7faf4-249">-Force</span><span class="sxs-lookup"><span data-stu-id="7faf4-249">-Force</span></span>                             | <span data-ttu-id="7faf4-250">Sovrascrivi file esistenti.</span><span class="sxs-lookup"><span data-stu-id="7faf4-250">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="7faf4-251">Esempio:</span><span class="sxs-lookup"><span data-stu-id="7faf4-251">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="7faf4-252">Esempio che esegue lo scaffolding delle solo le tabelle selezionate e crea il contesto in una cartella separata con il nome specificato:</span><span class="sxs-lookup"><span data-stu-id="7faf4-252">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a><span data-ttu-id="7faf4-253">Migrazione degli script</span><span class="sxs-lookup"><span data-stu-id="7faf4-253">Script-Migration</span></span>

<span data-ttu-id="7faf4-254">Genera uno script SQL che tutte le modifiche da una migrazione selezionata si applica a un'altra migrazione selezionata.</span><span class="sxs-lookup"><span data-stu-id="7faf4-254">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="7faf4-255">Parametri:</span><span class="sxs-lookup"><span data-stu-id="7faf4-255">Parameters:</span></span>

| <span data-ttu-id="7faf4-256">Parametro</span><span class="sxs-lookup"><span data-stu-id="7faf4-256">Parameter</span></span>                | <span data-ttu-id="7faf4-257">Descrizione</span><span class="sxs-lookup"><span data-stu-id="7faf4-257">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7faf4-258">*-From* \<String></span><span class="sxs-lookup"><span data-stu-id="7faf4-258">*-From* \<String></span></span>        | <span data-ttu-id="7faf4-259">La migrazione inizia.</span><span class="sxs-lookup"><span data-stu-id="7faf4-259">The starting migration.</span></span> <span data-ttu-id="7faf4-260">Le migrazioni possono essere identificate in base al nome o ID.</span><span class="sxs-lookup"><span data-stu-id="7faf4-260">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="7faf4-261">Il numero 0 rappresenta un caso speciale che si intende *prima della migrazione prima*.</span><span class="sxs-lookup"><span data-stu-id="7faf4-261">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="7faf4-262">Il valore predefinito è 0.</span><span class="sxs-lookup"><span data-stu-id="7faf4-262">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="7faf4-263">*-To* \<String></span><span class="sxs-lookup"><span data-stu-id="7faf4-263">*-To* \<String></span></span>          | <span data-ttu-id="7faf4-264">La migrazione finale.</span><span class="sxs-lookup"><span data-stu-id="7faf4-264">The ending migration.</span></span> <span data-ttu-id="7faf4-265">Per impostazione predefinita all'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="7faf4-265">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="7faf4-266"><nobr>-Idempotent</nobr></span><span class="sxs-lookup"><span data-stu-id="7faf4-266"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="7faf4-267">Generare uno script che può essere utilizzato in un database in ogni operazione di migrazione.</span><span class="sxs-lookup"><span data-stu-id="7faf4-267">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="7faf4-268">-Output \<stringa ></span><span class="sxs-lookup"><span data-stu-id="7faf4-268">-Output \<String></span></span>        | <span data-ttu-id="7faf4-269">File in cui scrivere il risultato.</span><span class="sxs-lookup"><span data-stu-id="7faf4-269">The file to write the result to.</span></span> <span data-ttu-id="7faf4-270">Se questo parametro viene omesso, il file viene creato con un nome generato nella stessa cartella quando vengono creati i file di runtime dell'app, ad esempio: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span><span class="sxs-lookup"><span data-stu-id="7faf4-270">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="7faf4-271">To, From, e i parametri di Output supportano espansione tramite tab.</span><span class="sxs-lookup"><span data-stu-id="7faf4-271">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="7faf4-272">Nell'esempio seguente crea uno script per la migrazione InitialCreate, usando il nome della migrazione.</span><span class="sxs-lookup"><span data-stu-id="7faf4-272">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="7faf4-273">L'esempio seguente crea uno script per tutte le migrazioni dopo la migrazione InitialCreate, usando l'ID di migrazione.</span><span class="sxs-lookup"><span data-stu-id="7faf4-273">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="7faf4-274">Update-Database</span><span class="sxs-lookup"><span data-stu-id="7faf4-274">Update-Database</span></span>

<span data-ttu-id="7faf4-275">Aggiorna il database per l'ultima migrazione o per una migrazione specificata.</span><span class="sxs-lookup"><span data-stu-id="7faf4-275">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="7faf4-276">Parametro</span><span class="sxs-lookup"><span data-stu-id="7faf4-276">Parameter</span></span>                           | <span data-ttu-id="7faf4-277">Descrizione</span><span class="sxs-lookup"><span data-stu-id="7faf4-277">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7faf4-278"><nobr>*-Migrazione* \<stringa ></nobr></span><span class="sxs-lookup"><span data-stu-id="7faf4-278"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="7faf4-279">La migrazione di destinazione.</span><span class="sxs-lookup"><span data-stu-id="7faf4-279">The target migration.</span></span> <span data-ttu-id="7faf4-280">Le migrazioni possono essere identificate in base al nome o ID.</span><span class="sxs-lookup"><span data-stu-id="7faf4-280">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="7faf4-281">Il numero 0 rappresenta un caso speciale che si intende *prima della migrazione prima* e fa sì che tutte le migrazioni da ripristinare.</span><span class="sxs-lookup"><span data-stu-id="7faf4-281">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="7faf4-282">Se non viene specificata alcuna migrazione, il comando predefinito all'ultima migrazione.</span><span class="sxs-lookup"><span data-stu-id="7faf4-282">If no migration is specified, the command defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="7faf4-283">Il parametro di migrazione supporta l'espansione tramite tab.</span><span class="sxs-lookup"><span data-stu-id="7faf4-283">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="7faf4-284">Nell'esempio seguente ripristina tutte le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="7faf4-284">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="7faf4-285">Negli esempi seguenti aggiornano il database per una migrazione specificata.</span><span class="sxs-lookup"><span data-stu-id="7faf4-285">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="7faf4-286">Il primo Usa il nome della migrazione e il secondo Usa l'ID di migrazione:</span><span class="sxs-lookup"><span data-stu-id="7faf4-286">The first uses the migration name and the second uses the migration ID:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a><span data-ttu-id="7faf4-287">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="7faf4-287">Additional resources</span></span>

* [<span data-ttu-id="7faf4-288">Migrazioni</span><span class="sxs-lookup"><span data-stu-id="7faf4-288">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="7faf4-289">Reverse engineering</span><span class="sxs-lookup"><span data-stu-id="7faf4-289">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
