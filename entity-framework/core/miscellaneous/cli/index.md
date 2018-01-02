---
title: Informazioni di riferimento sulla riga di comando - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 076e9251850ba10df323cd25922aa8b95b3a5491
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="aff43-102">Strumenti di Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="aff43-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="aff43-103">Gli strumenti di Entity Framework Core offrono un supporto durante lo sviluppo delle applicazioni EF Core.</span><span class="sxs-lookup"><span data-stu-id="aff43-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="aff43-104">Vengono usati principalmente per eseguire lo scaffolding di un elemento DbContext e dei tipi di entità decompilando lo schema di un database e per gestire le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="aff43-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="aff43-105">Gli [strumenti della Console di Gestione pacchetti di EF Core][1] offrono una migliore esperienza all'interno di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aff43-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="aff43-106">Per eseguirli, usare la [Console di Gestione pacchetti][2] di NuGet.</span><span class="sxs-lookup"><span data-stu-id="aff43-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="aff43-107">Questi strumenti funzionano sia con i progetti .NET Framework che con i progetti .NET Core.</span><span class="sxs-lookup"><span data-stu-id="aff43-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="aff43-108">Gli [strumenti da riga di comando .NET di EF Core][3] sono un'estensione degli [strumenti dell'interfaccia della riga di comando di .NET Core][4] che sono multipiattaforma e possono essere eseguiti all'esterno di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aff43-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="aff43-109">Tali strumenti richiedono un progetto .NET Core SDK (uno con `Sdk="Microsoft.NET.Sdk"` o simili nel file di progetto).</span><span class="sxs-lookup"><span data-stu-id="aff43-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="aff43-110">Entrambi gli strumenti espongono la stessa funzionalità.</span><span class="sxs-lookup"><span data-stu-id="aff43-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="aff43-111">Se sviluppa in Visual Studio, è consigliabile usare gli strumenti della Console di Gestione pacchetti poiché offrono un'esperienza più integrata.</span><span class="sxs-lookup"><span data-stu-id="aff43-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="aff43-112">Framework</span><span class="sxs-lookup"><span data-stu-id="aff43-112">Frameworks</span></span>
----------
<span data-ttu-id="aff43-113">Gli strumenti supportano i progetti destinati a .NET Framework o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="aff43-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="aff43-114">Se il progetto è destinato a un altro framework, ad esempio Windows universale o Xamarin, si consiglia di creare un progetto .NET Standard separato e destinarlo in modo trasversale a uno dei framework supportati.</span><span class="sxs-lookup"><span data-stu-id="aff43-114">If your project targets another framework (for example, Universal Windows or Xamarin), we recommend creating a separate .NET Standard project and cross-targeting one of the supported frameworks.</span></span>

<span data-ttu-id="aff43-115">Per specificare .NET Core come destinazione trasversale, ad esempio, fare clic sul progetto con il pulsante destro del mouse e selezionare **Modifica \*.csproj**.</span><span class="sxs-lookup"><span data-stu-id="aff43-115">To cross-target .NET Core, for example, right-click on the project and select **Edit \*.csproj**.</span></span> <span data-ttu-id="aff43-116">Aggiornare la proprietà `TargetFramework` come indicato di seguito</span><span class="sxs-lookup"><span data-stu-id="aff43-116">Update the `TargetFramework` property as follows.</span></span> <span data-ttu-id="aff43-117">(si noti che il nome della proprietà diventa plurale).</span><span class="sxs-lookup"><span data-stu-id="aff43-117">(Note, the property name becomes plural.)</span></span>

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

<span data-ttu-id="aff43-118">Se si usa una libreria di classi .NET Standard, la destinazione trasversale non è necessaria se il progetto di avvio ha come destinazione .NET Framework o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="aff43-118">If you're using a .NET Standard class library, you don't need to cross-target if your startup project targets .NET Framework or .NET Core.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="aff43-119">Progetti di avvio e destinazione</span><span class="sxs-lookup"><span data-stu-id="aff43-119">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="aff43-120">Ogni volta che si richiama un comando, sono coinvolti due progetti: il progetto di destinazione e il progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="aff43-120">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="aff43-121">Il progetto di destinazione è dove vengono aggiunti, in alcuni casi rimossi, tutti i file.</span><span class="sxs-lookup"><span data-stu-id="aff43-121">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="aff43-122">Il progetto di avvio è quello emulato dagli strumenti quando si esegue il codice del progetto.</span><span class="sxs-lookup"><span data-stu-id="aff43-122">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="aff43-123">Il progetto di destinazione e il progetto di avvio possono coincidere.</span><span class="sxs-lookup"><span data-stu-id="aff43-123">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
