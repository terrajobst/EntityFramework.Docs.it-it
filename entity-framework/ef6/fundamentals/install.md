---
title: Ottenere Entity Framework-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 2bdec6a9be228fbe934d0f46aa1bfafdfb2c971c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419468"
---
# <a name="get-entity-framework"></a><span data-ttu-id="51005-102">Ottenere Entity Framework</span><span class="sxs-lookup"><span data-stu-id="51005-102">Get Entity Framework</span></span>
<span data-ttu-id="51005-103">Entity Framework è costituito da EF Tools per Visual Studio e dal runtime di EF.</span><span class="sxs-lookup"><span data-stu-id="51005-103">Entity Framework is made up of the EF Tools for Visual Studio and the EF Runtime.</span></span>

## <a name="ef-tools-for-visual-studio"></a><span data-ttu-id="51005-104">EF Tools per Visual Studio</span><span class="sxs-lookup"><span data-stu-id="51005-104">EF Tools for Visual Studio</span></span>

<span data-ttu-id="51005-105">I Entity Framework Tools per Visual Studio includono EF designer e la procedura guidata modello EF e sono necessari per i flussi di lavoro First e Model First del database.</span><span class="sxs-lookup"><span data-stu-id="51005-105">The Entity Framework Tools for Visual Studio include the EF Designer and the EF Model Wizard and are required for the database first and model first workflows.</span></span> <span data-ttu-id="51005-106">Gli strumenti EF sono inclusi in tutte le versioni recenti di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="51005-106">EF Tools are included in all recent versions of Visual Studio.</span></span> <span data-ttu-id="51005-107">Se si esegue un'installazione personalizzata di Visual Studio, è necessario assicurarsi che sia selezionato l'elemento "Entity Framework 6 Tools" scegliendo un carico di lavoro che lo includa o selezionandolo come singolo componente.</span><span class="sxs-lookup"><span data-stu-id="51005-107">If you perform a custom install of Visual Studio you will need to ensure that the item "Entity Framework 6 Tools" is selected by either choosing a workload that includes it or by selecting it as an individual component.</span></span>

<span data-ttu-id="51005-108">Per alcune versioni precedenti di Visual Studio, gli strumenti EF aggiornati sono disponibili come download.</span><span class="sxs-lookup"><span data-stu-id="51005-108">For some past versions of Visual Studio, updated EF Tools are available as a download.</span></span> <span data-ttu-id="51005-109">Vedere [versioni di Visual Studio](~/ef6/what-is-new/visual-studio.md) per istruzioni su come ottenere la versione più recente degli strumenti EF per la versione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="51005-109">See [Visual Studio Versions](~/ef6/what-is-new/visual-studio.md) for guidance on how to get the latest version of EF Tools available for your version of Visual Studio.</span></span>

## <a name="ef-runtime"></a><span data-ttu-id="51005-110">Runtime EF</span><span class="sxs-lookup"><span data-stu-id="51005-110">EF Runtime</span></span>

<span data-ttu-id="51005-111">La versione più recente di Entity Framework è disponibile come [pacchetto NuGet EntityFramework](https://nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="51005-111">The latest version of Entity Framework is available as the [EntityFramework NuGet package](https://nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="51005-112">Se non si ha familiarità con gestione pacchetti NuGet, si consiglia di leggere la [Panoramica di NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span><span class="sxs-lookup"><span data-stu-id="51005-112">If you are not familiar with the NuGet Package Manager, we encourage you to read the [NuGet Overview](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).</span></span>

### <a name="installing-the-ef-nuget-package"></a><span data-ttu-id="51005-113">Installazione del pacchetto NuGet EF</span><span class="sxs-lookup"><span data-stu-id="51005-113">Installing the EF NuGet Package</span></span>

<span data-ttu-id="51005-114">È possibile installare il pacchetto EntityFramework facendo clic con il pulsante destro del mouse sulla cartella **riferimenti** del progetto e scegliendo **Gestisci pacchetti NuGet...**</span><span class="sxs-lookup"><span data-stu-id="51005-114">You can install the EntityFramework package by right-clicking on the **References** folder of your project and selecting **Manage NuGet Packages…**</span></span>

![Manage NuGet Packages](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a><span data-ttu-id="51005-116">Installazione dalla console di gestione pacchetti</span><span class="sxs-lookup"><span data-stu-id="51005-116">Installing from Package Manager Console</span></span>

<span data-ttu-id="51005-117">In alternativa, è possibile installare EntityFramework eseguendo il comando seguente nella console di [Gestione pacchetti](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="51005-117">Alternatively, you can install EntityFramework by running the following command in the [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a><span data-ttu-id="51005-118">Installazione di una versione specifica di EF</span><span class="sxs-lookup"><span data-stu-id="51005-118">Installing a specific version of EF</span></span>

<span data-ttu-id="51005-119">Da EF 4,1 in poi, le nuove versioni di EF Runtime sono state rilasciate come [pacchetto NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="51005-119">From EF 4.1 onwards, new versions of the EF runtime have been released as the [EntityFramework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="51005-120">È possibile aggiungere una qualsiasi di queste versioni a un progetto basato su .NET Framework eseguendo il comando seguente nella [console di gestione pacchetti](https://docs.nuget.org/docs/start-here/using-the-package-manager-console)di Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="51005-120">Any of those versions can be added to a .NET Framework-based project by running the following command in Visual Studio's [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console):</span></span>

``` powershell
Install-Package EntityFramework -Version <number>
```

<span data-ttu-id="51005-121">Si noti che `<number>` rappresenta la versione specifica di EF da installare.</span><span class="sxs-lookup"><span data-stu-id="51005-121">Note that `<number>` represents the specific version of EF to install.</span></span> <span data-ttu-id="51005-122">Ad esempio, 6.2.0 è la versione di numero per EF 6,2.</span><span class="sxs-lookup"><span data-stu-id="51005-122">For example, 6.2.0 is the version of number for EF 6.2.</span></span>   

<span data-ttu-id="51005-123">I runtime EF prima del 4,1 facevano parte di .NET Framework e non possono essere installati separatamente.</span><span class="sxs-lookup"><span data-stu-id="51005-123">EF runtimes before 4.1 were part of .NET Framework and cannot be installed separately.</span></span>

### <a name="installing-the-latest-preview"></a><span data-ttu-id="51005-124">Installazione dell'anteprima più recente</span><span class="sxs-lookup"><span data-stu-id="51005-124">Installing the Latest Preview</span></span>

<span data-ttu-id="51005-125">I metodi precedenti forniranno la versione più recente di Entity Framework supportata.</span><span class="sxs-lookup"><span data-stu-id="51005-125">The above methods will give you the latest fully supported release of Entity Framework.</span></span> <span data-ttu-id="51005-126">Spesso sono disponibili versioni non definitive di Entity Framework che ci piacerebbero provare e inviare commenti e suggerimenti su.</span><span class="sxs-lookup"><span data-stu-id="51005-126">There are often prerelease versions of Entity Framework available that we would love you to try out and give us feedback on.</span></span>

<span data-ttu-id="51005-127">Per installare l'anteprima più recente di EntityFramework, è possibile selezionare **Includi versione preliminare** nella finestra Gestisci pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="51005-127">To install the latest preview of EntityFramework you can select **Include Prerelease** in the Manage NuGet Packages window.</span></span> <span data-ttu-id="51005-128">Se non sono disponibili versioni non definitive, verrà automaticamente scaricata la versione di Entity Framework più recente supportata.</span><span class="sxs-lookup"><span data-stu-id="51005-128">If no prerelease versions are available you will automatically get the latest fully supported version of Entity Framework.</span></span>

![Includi versione preliminare](~/ef6/media/includeprerelease.png)

<span data-ttu-id="51005-130">In alternativa, è possibile eseguire il comando seguente nella [console di gestione pacchetti](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="51005-130">Alternatively, you can run the following command in the [Package Manager Console](https://docs.nuget.org/docs/start-here/using-the-package-manager-console).</span></span>

``` powershell
Install-Package EntityFramework -Pre
```
