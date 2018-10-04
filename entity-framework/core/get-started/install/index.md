---
title: Installazione di Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 455eccbb149f0980cefa250ef5db443c73e66603
ms.sourcegitcommit: ae399f9f3d1bae2c446b552247bd3af3ca5a2cf9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48575639"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="c5052-102">Installazione di Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c5052-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c5052-103">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c5052-103">Prerequisites</span></span>

* <span data-ttu-id="c5052-104">Per sviluppare app destinate a .NET Core 2.1, installare [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="c5052-104">To develop apps that target .NET Core 2.1, install [the .NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span> <span data-ttu-id="c5052-105">Il SDK deve essere installato anche se è presente la versione più recente di Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="c5052-105">The SDK has to be installed even if you have the latest version of Visual Studio 2017.</span></span>

* <span data-ttu-id="c5052-106">Per usare Visual Studio per lo sviluppo di app destinate a .NET Core 2.1, installare Visual Studio 2017 versione 15.7 o successiva.</span><span class="sxs-lookup"><span data-stu-id="c5052-106">To use Visual Studio for development of apps that target .NET Core 2.1, install Visual Studio 2017 version 15.7 or later.</span></span>

* <span data-ttu-id="c5052-107">Per usare Entity Framework 2.1 nelle applicazioni ASP.NET Core, usare ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="c5052-107">To use Entity Framework 2.1 in ASP.NET Core applications, use ASP.NET Core 2.1.</span></span> <span data-ttu-id="c5052-108">Le applicazioni che usano versioni precedenti di ASP.NET Core devono essere aggiornate alla versione 2.1.</span><span class="sxs-lookup"><span data-stu-id="c5052-108">Applications that use earlier versions of ASP.NET Core must be updated to 2.1.</span></span>

* <span data-ttu-id="c5052-109">È possibile usare Visual Studio 2015 per le app destinate a .NET Framework 4.6.1 o versioni successive.</span><span class="sxs-lookup"><span data-stu-id="c5052-109">You can use Visual Studio 2015 for apps that target the .NET Framework 4.6.1 or later.</span></span> <span data-ttu-id="c5052-110">Tuttavia è necessaria una versione di NuGet che supporta .NET Standard 2.0 e i framework compatibili.</span><span class="sxs-lookup"><span data-stu-id="c5052-110">But you need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="c5052-111">Per ottenere questa configurazione in Visual Studio 2015 [aggiornare il client NuGet alla versione 3.6.0](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="c5052-111">To get that in Visual Studio 2015, [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads).</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="c5052-112">Ottenere il runtime Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c5052-112">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="c5052-113">Per aggiungere le librerie di runtime di Entity Framework Core a un'applicazione, installare il pacchetto NuGet del provider di database da usare.</span><span class="sxs-lookup"><span data-stu-id="c5052-113">To add EF Core runtime libraries to an application, install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="c5052-114">Per l'elenco dei provider supportati e dei nomi dei pacchetti NuGet corrispondenti, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="c5052-114">For a list of supported providers and their NuGet package names, see [Database providers](../../providers/index.md).</span></span>

<span data-ttu-id="c5052-115">Per installare o aggiornare i pacchetti NuGet usare l'interfaccia della riga di comando di .NET Core, la finestra di dialogo Gestione pacchetti di Visual Studio o la console di gestione pacchetti di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c5052-115">To install or update NuGet packages, use the .NET Core CLI, the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

<span data-ttu-id="c5052-116">Per le applicazioni ASP.NET Core 2.1 i provider di SQL Server e in memoria vengono inclusi automaticamente, per cui non è necessario installarli separatamente.</span><span class="sxs-lookup"><span data-stu-id="c5052-116">For ASP.NET Core 2.1 applications, the in-memory and SQL Server providers are automatically included, so there's no need to install them separately.</span></span>

> [!TIP]  
> <span data-ttu-id="c5052-117">Se si deve aggiornare un'applicazione che usa un provider di database di terze parti, verificare sempre se è disponibile un aggiornamento del provider compatibile con la versione di Entity Framework Core che si vuole usare.</span><span class="sxs-lookup"><span data-stu-id="c5052-117">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="c5052-118">Ad esempio, i provider di database per le versioni precedenti non sono compatibili con la versione 2.1 del runtime di Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c5052-118">For example, database providers for previous versions are not compatible with version 2.1 of the EF Core runtime.</span></span>  

### <a name="net-core-cli"></a><span data-ttu-id="c5052-119">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="c5052-119">.NET Core CLI</span></span>

<span data-ttu-id="c5052-120">Il seguente comando dell'interfaccia della riga di comando di .NET Core installa o aggiorna il provider SQL Server:</span><span class="sxs-lookup"><span data-stu-id="c5052-120">The following .NET Core CLI command installs or updates the SQL Server provider:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="c5052-121">È possibile indicare una versione specifica nel comando `dotnet add package` mediante il modificatore `-v`.</span><span class="sxs-lookup"><span data-stu-id="c5052-121">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="c5052-122">Ad esempio, per installare i pacchetti di EF Core 2.1.0 aggiungere `-v 2.1.0` al comando.</span><span class="sxs-lookup"><span data-stu-id="c5052-122">For example, to install EF Core 2.1.0 packages, append `-v 2.1.0` to the command.</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="c5052-123">Finestra di dialogo Gestione pacchetti NuGet in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c5052-123">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="c5052-124">Nel menu selezionare **Progetto > Gestisci pacchetti NuGet**</span><span class="sxs-lookup"><span data-stu-id="c5052-124">From the menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="c5052-125">Fare clic sulla scheda **Sfoglia** o sulla scheda **Aggiornamenti**</span><span class="sxs-lookup"><span data-stu-id="c5052-125">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="c5052-126">Per installare o aggiornare il provider SQL Server selezionare il pacchetto `Microsoft.EntityFrameworkCore.SqlServer` e confermare.</span><span class="sxs-lookup"><span data-stu-id="c5052-126">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="c5052-127">Per altre informazioni, vedere [Finestra di dialogo Gestione pacchetti NuGet](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span><span class="sxs-lookup"><span data-stu-id="c5052-127">For more information, see [NuGet Package Manager Dialog](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="c5052-128">Console di Gestione pacchetti NuGet di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c5052-128">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="c5052-129">Nel menu selezionare **Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="c5052-129">From the menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="c5052-130">Per installare il provider SQL Server eseguire il comando seguente nella console di Gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="c5052-130">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="c5052-131">Per aggiornare il provider, usare il comando `Update-Package`.</span><span class="sxs-lookup"><span data-stu-id="c5052-131">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="c5052-132">Per indicare una versione specifica usare il modificatore `-Version`.</span><span class="sxs-lookup"><span data-stu-id="c5052-132">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="c5052-133">Ad esempio, per installare i pacchetti di EF Core 2.1.0 aggiungere `-Version 2.1.0` ai comandi</span><span class="sxs-lookup"><span data-stu-id="c5052-133">For example, to install EF Core 2.1.0 packages, append `-Version 2.1.0` to the commands</span></span>

<span data-ttu-id="c5052-134">Per altre informazioni, vedere [Console di Gestione pacchetti](https://docs.microsoft.com/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="c5052-134">For more information, see [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console).</span></span>

## <a name="get-entity-framework-core-tools"></a><span data-ttu-id="c5052-135">Ottenere strumenti di Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c5052-135">Get Entity Framework Core tools</span></span>

<span data-ttu-id="c5052-136">Oltre alle librerie di runtime, è possibile installare nel progetto strumenti che svolgono attività associate a EF Core in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="c5052-136">Besides the runtime libraries, you can install tools that can perform some EF Core-related tasks in your project at design time.</span></span> <span data-ttu-id="c5052-137">È ad esempio possibile creare migrazioni, applicare migrazioni e creare un modello basato su un database esistente.</span><span class="sxs-lookup"><span data-stu-id="c5052-137">For example, you can create migrations, apply migrations, and create a model based on an existing database.</span></span>

<span data-ttu-id="c5052-138">Sono disponibili due set di strumenti:</span><span class="sxs-lookup"><span data-stu-id="c5052-138">Two sets of tools are available:</span></span>
* <span data-ttu-id="c5052-139">Gli [strumenti dell'interfaccia della riga di comando (CLI)](../../miscellaneous/cli/dotnet.md) di .NET Core possono essere usati in Windows, Linux o macOS.</span><span class="sxs-lookup"><span data-stu-id="c5052-139">The .NET Core [command-line interface (CLI) tools](../../miscellaneous/cli/dotnet.md) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="c5052-140">Questi comandi iniziano con `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="c5052-140">These commands begin with `dotnet ef`.</span></span> 
* <span data-ttu-id="c5052-141">Gli [strumenti della console di Gestione pacchetti](../../miscellaneous/cli/powershell.md) vengono eseguiti in Visual Studio 2017 su Windows.</span><span class="sxs-lookup"><span data-stu-id="c5052-141">The [Package Manager Console tools](../../miscellaneous/cli/powershell.md)  run in Visual Studio 2017 on Windows.</span></span> <span data-ttu-id="c5052-142">Questi comandi iniziano con un verbo, ad esempio `Add-Migration`, `Update-Database`.</span><span class="sxs-lookup"><span data-stu-id="c5052-142">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="c5052-143">Sebbene sia possibile usare i comandi `dotnet ef` della console di Gestione pacchetti, quando si usa Visual Studio risulta più pratico usare gli strumenti della console di Gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="c5052-143">Although you can use the `dotnet ef` commands from the Package Manager Console, it's more convenient to use the Package Manager Console tools when you're using Visual Studio:</span></span>
* <span data-ttu-id="c5052-144">Tali strumenti funzionano automaticamente con il progetto corrente selezionato nella console di Gestione pacchetti, senza richiedere il passaggio manuale da una directory all'altra.</span><span class="sxs-lookup"><span data-stu-id="c5052-144">They automatically work with the current project selected in the Package Manager Console without requiring manually switching directories.</span></span>  
* <span data-ttu-id="c5052-145">Dopo il completamento del comando, i file generati vengono aperti automaticamente in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c5052-145">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-cli-tools"></a><span data-ttu-id="c5052-146">Ottenere gli strumenti dell'interfaccia della riga di comando (CLI)</span><span class="sxs-lookup"><span data-stu-id="c5052-146">Get the CLI tools</span></span>

<span data-ttu-id="c5052-147">I comandi `dotnet ef` sono inclusi in .NET Core SDK, ma per abilitarli è necessario installare il pacchetto `Microsoft.EntityFrameworkCore.Design`:</span><span class="sxs-lookup"><span data-stu-id="c5052-147">The `dotnet ef` commands are included in the .NET Core SDK, but to enable the commands you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

<span data-ttu-id="c5052-148">Per le app ASP.NET Core 2.1 questo pacchetto è incluso automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c5052-148">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

<span data-ttu-id="c5052-149">Come descritto in precedenza in [Prerequisiti](#prerequisites), è necessario installare anche .NET Core 2.1 SDK.</span><span class="sxs-lookup"><span data-stu-id="c5052-149">As explained earlier in [Prerequisites](#prerequisites), you also need to install the .NET Core 2.1 SDK.</span></span>

> [!IMPORTANT]      
> <span data-ttu-id="c5052-150">Usare sempre le versioni dei pacchetti degli strumenti corrispondenti alla versione principale dei pacchetti di runtime.</span><span class="sxs-lookup"><span data-stu-id="c5052-150">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="c5052-151">Ottenere gli strumenti della console di Gestione pacchetti</span><span class="sxs-lookup"><span data-stu-id="c5052-151">Get the Package Manager Console tools</span></span>

<span data-ttu-id="c5052-152">Per ottenere gli strumenti della console di Gestione pacchetti per EF Core, installare il pacchetto `Microsoft.EntityFrameworkCore.Tools`:</span><span class="sxs-lookup"><span data-stu-id="c5052-152">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

<span data-ttu-id="c5052-153">Per le app ASP.NET Core 2.1 questo pacchetto è incluso automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c5052-153">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

## <a name="upgrading-to-ef-core-21"></a><span data-ttu-id="c5052-154">Aggiornamento a EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="c5052-154">Upgrading to EF Core 2.1</span></span>

<span data-ttu-id="c5052-155">Se si aggiorna un'applicazione esistente a EF Core 2.1, potrebbe essere necessario rimuovere manualmente alcuni riferimenti a pacchetti EF Core meno recenti:</span><span class="sxs-lookup"><span data-stu-id="c5052-155">If you're upgrading an existing application to EF Core 2.1, some references to older EF Core packages may need to be removed manually:</span></span>

* <span data-ttu-id="c5052-156">I pacchetti della fase di progettazione per provider di database quali `Microsoft.EntityFrameworkCore.SqlServer.Design` non sono più necessari né supportati in EF Core 2.1, ma non vengono rimossi automaticamente quando si aggiornano gli altri pacchetti.</span><span class="sxs-lookup"><span data-stu-id="c5052-156">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.1, but aren't automatically removed when upgrading the other packages.</span></span>

* <span data-ttu-id="c5052-157">Gli strumenti dell'interfaccia della riga di comando di .NET sono ora inclusi in .NET SDK, in modo che il riferimento a tale pacchetto possa essere rimosso dal file *csproj*:</span><span class="sxs-lookup"><span data-stu-id="c5052-157">The .NET CLI tools are now included in the .NET SDK, so the reference to that package can be removed from the *.csproj* file:</span></span>

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

<span data-ttu-id="c5052-158">Nel caso di applicazioni destinate a .NET Framework e create in versioni precedenti di Visual Studio, assicurarsi che siano compatibili con le librerie .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="c5052-158">For applications that target the .NET Framework and were created by earlier versions of Visual Studio, make sure that they are compatible with .NET Standard 2.0 libraries:</span></span>

  * <span data-ttu-id="c5052-159">Modificare il file di progetto e assicurarsi che la voce seguente sia visualizzata nel gruppo di proprietà iniziale:</span><span class="sxs-lookup"><span data-stu-id="c5052-159">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * <span data-ttu-id="c5052-160">Per i progetti di test, assicurarsi anche che sia presente la voce seguente:</span><span class="sxs-lookup"><span data-stu-id="c5052-160">For test projects, also make sure the following entry is present:</span></span>

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
