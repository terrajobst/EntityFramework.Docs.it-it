---
title: Installazione di EF Core
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 31b96ebd0ae282b88be98988eff6263084dc5dd5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
ms.locfileid: "26049243"
---
# <a name="installing-ef-core"></a><span data-ttu-id="80b11-102">Installazione di EF Core</span><span class="sxs-lookup"><span data-stu-id="80b11-102">Installing EF Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="80b11-103">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="80b11-103">Prerequisites</span></span>

<span data-ttu-id="80b11-104">Per sviluppare applicazioni .NET Core 2.0 (incluse le applicazioni ASP.NET Core 2.0 associate a .NET Core) è necessario scaricare e installare una versione di [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) appropriata per la piattaforma in uso.</span><span class="sxs-lookup"><span data-stu-id="80b11-104">In order to develop .NET Core 2.0 applications (including ASP.NET Core 2.0 applications that target .NET Core) you will need to download and install a version of the [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core) that is appropriate to your platform.</span></span> <span data-ttu-id="80b11-105">**Questo vale anche se è stato già installato Visual Studio 2017 versione 15.3.**</span><span class="sxs-lookup"><span data-stu-id="80b11-105">**This is true even if you have installed Visual Studio 2017 version 15.3.**</span></span>

<span data-ttu-id="80b11-106">Per usare EF Core 2.0 o qualsiasi altra libreria .NET Standard 2.0 con una piattaforma .NET diversa da .NET 2.0 (ad esempio con .NET Framework 4.6.1 o versione successiva) è necessaria una versione di NuGet che supporta .NET Standard 2.0 e i framework compatibili.</span><span class="sxs-lookup"><span data-stu-id="80b11-106">In order to use EF Core 2.0 or any other .NET Standard 2.0 library with a .NET platforms besides .NET Core 2.0 (e.g. with .NET Framework 4.6.1 or greater) you will need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="80b11-107">Ecco alcuni metodi per ottenere quanto sopra:</span><span class="sxs-lookup"><span data-stu-id="80b11-107">Here are a few ways you can obtain this:</span></span>

* <span data-ttu-id="80b11-108">Installare Visual Studio 2017 versione 15.3</span><span class="sxs-lookup"><span data-stu-id="80b11-108">Install Visual Studio 2017 version 15.3</span></span>
* <span data-ttu-id="80b11-109">Se si usa Visual Studio 2015, [scaricare il client NuGet e aggiornarlo alla versione 3.6.0](https://www.nuget.org/downloads)</span><span class="sxs-lookup"><span data-stu-id="80b11-109">If you are using Visual Studio 2015, [download and upgrade NuGet client to version 3.6.0](https://www.nuget.org/downloads)</span></span>

<span data-ttu-id="80b11-110">Per essere compatibili con le librerie .NET Standard 2.0, i progetti creati con versioni precedenti di Visual Studio e destinati a .NET Framework potrebbero richiedere modifiche aggiuntive:</span><span class="sxs-lookup"><span data-stu-id="80b11-110">Projects created with previous versions of Visual Studio and targeting .NET Framework may need additional modifications in order to be compatible with .NET Standard 2.0 libraries:</span></span>

* <span data-ttu-id="80b11-111">Modificare il file di progetto e assicurarsi che la voce seguente sia visualizzata nel gruppo di proprietà iniziale:</span><span class="sxs-lookup"><span data-stu-id="80b11-111">Edit the project file and make sure the following entry appears in the initial property group:</span></span>
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* <span data-ttu-id="80b11-112">Per i progetti di test, assicurarsi anche che sia presente la voce seguente:</span><span class="sxs-lookup"><span data-stu-id="80b11-112">For test projects, also make sure the following entry is present:</span></span>
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a><span data-ttu-id="80b11-113">Ottenimento dei componenti</span><span class="sxs-lookup"><span data-stu-id="80b11-113">Getting the bits</span></span>
<span data-ttu-id="80b11-114">Il metodo consigliato per l'aggiunta delle librerie di runtime di Entity Framework Core a un'applicazione è l'installazione di un provider di database EF Core da NuGet.</span><span class="sxs-lookup"><span data-stu-id="80b11-114">The recommended way to add EF Core runtime libraries into an application is to install an EF Core database provider from NuGet.</span></span>

<span data-ttu-id="80b11-115">Oltre alle librerie di runtime è possibile installare strumenti che semplificano l'esecuzione di varie attività associate a EF Core nel progetto in fase di elaborazione, quali la creazione e l'applicazione di migrazioni e la creazione di un modello a partire da un database esistente.</span><span class="sxs-lookup"><span data-stu-id="80b11-115">Besides the runtime libraries, you can install tools which make it easier to perform several EF Core-related tasks in your project at design time, such as creating and applying migrations, and creating a model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="80b11-116">Se si deve aggiornare un'applicazione che usa un provider di database di terze parti, verificare sempre se è disponibile un aggiornamento del provider compatibile con la versione di Entity Framework Core che si vuole usare.</span><span class="sxs-lookup"><span data-stu-id="80b11-116">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="80b11-117">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="80b11-117">E.g.</span></span> <span data-ttu-id="80b11-118">i provider di database per le versioni precedenti non sono compatibili con la versione 2.0 del runtime di Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="80b11-118">database providers for previous versions are not compatible with version 2.0 of the EF Core runtime.</span></span>  

> [!TIP]  
> <span data-ttu-id="80b11-119">Le applicazioni associate ad ASP.NET Core 2.0 possono usare EF Core 2.0 senza dipendenze aggiuntive oltre ai provider di database di terze parti.</span><span class="sxs-lookup"><span data-stu-id="80b11-119">Applications targeting ASP.NET Core 2.0 can use EF Core 2.0 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="80b11-120">Le applicazioni associate a versioni precedenti di ASP.NET Core possono usare EF Core 2.0 dopo l'aggiornamento ad ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="80b11-120">Applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.0 in order to use EF Core 2.0.</span></span>

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a><span data-ttu-id="80b11-121">Sviluppo multipiattaforma con l'interfaccia della riga di comando (CLI) .NET Core</span><span class="sxs-lookup"><span data-stu-id="80b11-121">Cross-platform development using the .NET Core Command Line Interface (CLI)</span></span>

<span data-ttu-id="80b11-122">Per sviluppare applicazioni associate a [.NET Core](https://www.microsoft.com/net/download/core) è possibile scegliere di usare i [comandi CLI](https://docs.microsoft.com/dotnet/core/tools/) `dotnet` in combinazione con l'editor di testo di preferenza oppure un ambiente di sviluppo integrato (IDE) come Visual Studio, Visual Studio per Mac o Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="80b11-122">To develop applications that target [.NET Core](https://www.microsoft.com/net/download/core) you can choose to use the [`dotnet` CLI commands](https://docs.microsoft.com/dotnet/core/tools/) in combination with your favorite text editor, or an Integrated Development Environment (IDE) such as Visual Studio, Visual Studio for Mac or Visual Studio Code.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="80b11-123">Le applicazioni associate a .NET Core richiedono versioni specifiche di Visual Studio. Ad esempio per lo sviluppo .NET Core 1.x è necessario Visual Studio 2017, mentre per lo sviluppo .NET Core 2.0 è necessario Visual Studio 2017 versione 15.3.</span><span class="sxs-lookup"><span data-stu-id="80b11-123">Applications that target .NET Core require specific versions of Visual Studio, e.g. .NET Core 1.x development requires Visual Studio 2017, while .NET Core 2.0 development requires Visual Studio 2017 version 15.3.</span></span>

<span data-ttu-id="80b11-124">Per installare o aggiornare il provider SQL Server in un'applicazione .NET Core multipiattaforma, passare alla directory dell'applicazione ed eseguire le operazioni seguenti dalla riga di comando:</span><span class="sxs-lookup"><span data-stu-id="80b11-124">To install or upgrade the SQL Server provider in a cross-platform .NET Core application, switch to the application's directory and run the following in a command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="80b11-125">È possibile indicare l'installazione di una versione specifica nel comando `dotnet add package` mediante il modificatore `-v`.</span><span class="sxs-lookup"><span data-stu-id="80b11-125">You can indicate a specific version install in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="80b11-126">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="80b11-126">E.g.</span></span> <span data-ttu-id="80b11-127">per installare i pacchetti di EF Core 2.0 aggiungere `-v 2.0.0` al comando.</span><span class="sxs-lookup"><span data-stu-id="80b11-127">to install EF Core 2.0 packages, append `-v 2.0.0` to the command.</span></span>

<span data-ttu-id="80b11-128">EF Core include un set di [comandi aggiuntivi per l'infrastruttura CLI `dotnet`](../../miscellaneous/cli/dotnet.md), a partire da `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="80b11-128">EF Core includes a set of [additional commands for the `dotnet` CLI](../../miscellaneous/cli/dotnet.md), starting with `dotnet ef`.</span></span> <span data-ttu-id="80b11-129">Per usare i comandi CLI `dotnet ef`, il file `.csproj` dell'applicazione deve contenere la voce seguente:</span><span class="sxs-lookup"><span data-stu-id="80b11-129">In order to use the `dotnet ef` CLI commands, your application’s `.csproj` file needs to contain the following entry:</span></span>

``` xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
</ItemGroup>
```

<span data-ttu-id="80b11-130">Gli strumenti CLI di .NET Core per Entity Framework Core richiedono anche un pacchetto distinto denominato Microsoft.EntityFrameworkCore.Design.</span><span class="sxs-lookup"><span data-stu-id="80b11-130">The .NET Core CLI tools for EF Core also require a separate package called Microsoft.EntityFrameworkCore.Design.</span></span> <span data-ttu-id="80b11-131">È possibile aggiungerlo direttamente al progetto usando:</span><span class="sxs-lookup"><span data-stu-id="80b11-131">You can simply add it to the project using:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]  
> <span data-ttu-id="80b11-132">Usare sempre versioni dei pacchetti degli strumenti corrispondenti alla versione principale dei pacchetti di runtime.</span><span class="sxs-lookup"><span data-stu-id="80b11-132">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a><span data-ttu-id="80b11-133">Sviluppo in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="80b11-133">Visual Studio development</span></span>

<span data-ttu-id="80b11-134">In Visual Studio è possibile sviluppare molti tipi diversi di applicazioni destinate a .NET Core, .NET Framework o altre piattaforme supportate da EF Core.</span><span class="sxs-lookup"><span data-stu-id="80b11-134">You can develop many different types of applications that target .NET Core, .NET Framework, or other platforms supported by EF Core using Visual Studio.</span></span>

<span data-ttu-id="80b11-135">Esistono due modi per installare un provider di database EF Core nell'applicazione da Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="80b11-135">There are two ways you can install an EF Core database provider in your application from Visual Studio:</span></span>

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a><span data-ttu-id="80b11-136">Mediante [l'interfaccia Gestione pacchetti NuGet](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span><span class="sxs-lookup"><span data-stu-id="80b11-136">Using NuGet's [Package Manager User Interface](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span></span>

* <span data-ttu-id="80b11-137">Selezionare il menu **Progetto > Gestisci pacchetti NuGet**</span><span class="sxs-lookup"><span data-stu-id="80b11-137">Select on the menu **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="80b11-138">Fare clic sulla scheda **Sfoglia** o sulla scheda **Aggiornamenti**</span><span class="sxs-lookup"><span data-stu-id="80b11-138">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="80b11-139">Selezionare il pacchetto `Microsoft.EntityFrameworkCore.SqlServer` e la versione desiderata e confermare</span><span class="sxs-lookup"><span data-stu-id="80b11-139">Select the `Microsoft.EntityFrameworkCore.SqlServer` package and the desired version and confirm</span></span>

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a><span data-ttu-id="80b11-140">Mediante la [Console di Gestione pacchetti (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console) di NuGet</span><span class="sxs-lookup"><span data-stu-id="80b11-140">Using NuGet's [Package Manager Console (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span></span>

* <span data-ttu-id="80b11-141">Selezionare il menu **Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="80b11-141">Select on the menu **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="80b11-142">Nella Console di Gestione pacchetti, digitare ed eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="80b11-142">Type and run the following command in the PMC:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="80b11-143">In alternativa è possibile usare il comando `Update-Package` per aggiornare un pacchetto già installato a una versione più recente</span><span class="sxs-lookup"><span data-stu-id="80b11-143">You can use the `Update-Package` command instead to update a package that is already installed to a more recent  version</span></span>

* <span data-ttu-id="80b11-144">Per indicare una versione specifica, usare il modificatore `-Version`. Ad esempio, per installare i pacchetti di EF Core 2.0, accodare `-Version 2.0.0` ai comandi</span><span class="sxs-lookup"><span data-stu-id="80b11-144">To specify a specific version, you can use the `-Version` modifier, e.g. to install EF Core 2.0 packages, append `-Version 2.0.0` to the commands</span></span>

#### <a name="tools"></a><span data-ttu-id="80b11-145">Strumenti</span><span class="sxs-lookup"><span data-stu-id="80b11-145">Tools</span></span>

<span data-ttu-id="80b11-146">È anche disponibile una versione PowerShell dei [comandi EF Core eseguiti nella Console di Gestione pacchetti](../../miscellaneous/cli/powershell.md) in Visual Studio, con funzionalità simili a quelle dei comandi `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="80b11-146">There is also a PowerShell version of the [EF Core commands which run inside the PMC](../../miscellaneous/cli/powershell.md) in Visual Studio, with similar capabilities to the `dotnet ef` commands.</span></span> <span data-ttu-id="80b11-147">Per usare questi comandi installare il pacchetto `Microsoft.EntityFrameworkCore.Tools` usando l'interfaccia utente di Gestione pacchetti o la Console di Gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="80b11-147">In order to use these, install the `Microsoft.EntityFrameworkCore.Tools` package using either the Package Manager UI or the PMC.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="80b11-148">Usare sempre versioni dei pacchetti degli strumenti corrispondenti alla versione principale dei pacchetti di runtime.</span><span class="sxs-lookup"><span data-stu-id="80b11-148">Always use versions of the tools packages that match the major version of the runtime packages.</span></span>

> [!TIP]  
> <span data-ttu-id="80b11-149">Sebbene sia possibile usare i comandi `dotnet ef` della Console di Gestione pacchetti in Visual Studio, è molto più comodo usare la versione PowerShell:</span><span class="sxs-lookup"><span data-stu-id="80b11-149">Although it is possible to use the `dotnet ef` commands from the PMC in Visual Studio, it is far more convenient to use the PowerShell version:</span></span>
> * <span data-ttu-id="80b11-150">I comandi funzionano automaticamente con il progetto corrente selezionato nella Console di Gestione pacchetti, senza richiedere il passaggio manuale da una directory all'altra.</span><span class="sxs-lookup"><span data-stu-id="80b11-150">They automatically work with the current project selected in the PMC without requiring manually switching directories.</span></span>  
> * <span data-ttu-id="80b11-151">Dopo il completamento del comando, i file generati vengono aperti automaticamente in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="80b11-151">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="80b11-152">**Pacchetti deprecati in EF Core 2.0**: se si aggiorna un'applicazione esistente a EF Core 2.0, può essere necessario rimuovere manualmente riferimenti a pacchetti EF Core meno recenti.</span><span class="sxs-lookup"><span data-stu-id="80b11-152">**Deprecated packages in EF Core 2.0:** If you are upgrading an existing application to EF Core 2.0, some references to older EF Core packages may need to be removed manually.</span></span> <span data-ttu-id="80b11-153">In particolare, i pacchetti della fase di progettazione per provider di database quali `Microsoft.EntityFrameworkCore.SqlServer.Design` non sono più necessari né supportati in EF Core 2.0, ma non vengono eliminati automaticamente quando si aggiornano gli altri pacchetti.</span><span class="sxs-lookup"><span data-stu-id="80b11-153">In particular, database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.0, but will not be automatically removed when upgrading the other packages.</span></span>
