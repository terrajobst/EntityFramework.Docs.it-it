---
title: Informazioni di riferimento sulla riga di comando - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: d43b01fc61bb1c9b678e12e41c27d7efe9a59fa5
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490363"
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="17e07-102">Strumenti di Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="17e07-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="17e07-103">Gli strumenti di Entity Framework Core offrono un supporto durante lo sviluppo delle applicazioni EF Core.</span><span class="sxs-lookup"><span data-stu-id="17e07-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="17e07-104">Vengono usati principalmente per eseguire lo scaffolding di un elemento DbContext e dei tipi di entità decompilando lo schema di un database e per gestire le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="17e07-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="17e07-105">Gli [strumenti della Console di Gestione pacchetti di EF Core][1] offrono una migliore esperienza all'interno di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="17e07-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="17e07-106">Per eseguirli, usare la [Console di Gestione pacchetti][2] di NuGet.</span><span class="sxs-lookup"><span data-stu-id="17e07-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="17e07-107">Questi strumenti funzionano sia con i progetti .NET Framework che con i progetti .NET Core.</span><span class="sxs-lookup"><span data-stu-id="17e07-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="17e07-108">Gli [strumenti da riga di comando .NET di EF Core][3] sono un'estensione degli [strumenti dell'interfaccia della riga di comando di .NET Core][4] che sono multipiattaforma e possono essere eseguiti all'esterno di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="17e07-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="17e07-109">Tali strumenti richiedono un progetto .NET Core SDK (uno con `Sdk="Microsoft.NET.Sdk"` o simili nel file di progetto).</span><span class="sxs-lookup"><span data-stu-id="17e07-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="17e07-110">Entrambi gli strumenti espongono la stessa funzionalità.</span><span class="sxs-lookup"><span data-stu-id="17e07-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="17e07-111">Se sviluppa in Visual Studio, è consigliabile usare gli strumenti della Console di Gestione pacchetti poiché offrono un'esperienza più integrata.</span><span class="sxs-lookup"><span data-stu-id="17e07-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="17e07-112">Framework</span><span class="sxs-lookup"><span data-stu-id="17e07-112">Frameworks</span></span>
----------
<span data-ttu-id="17e07-113">Gli strumenti supportano i progetti destinati a .NET Framework o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="17e07-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="17e07-114">Se si vuole usare una libreria di classi, prendere in considerazione la possibilità di usare una libreria di classi .NET Framework o .NET Core, se possibile.</span><span class="sxs-lookup"><span data-stu-id="17e07-114">If you want to use a class library, then consider using a .NET Core or .NET Framework class library if possible.</span></span> <span data-ttu-id="17e07-115">Ciò consentirà di ridurre al minimo i problemi con gli strumenti .NET.</span><span class="sxs-lookup"><span data-stu-id="17e07-115">This will result in the least issues with .NET tooling.</span></span> <span data-ttu-id="17e07-116">Se invece si vuole usare una libreria di classi .NET Standard, è necessario usare un progetto di avvio destinato a .NET Framework o a .NET Core, in modo che gli strumenti abbiano una piattaforma di destinazione concreta in cui caricare la libreria di classi.</span><span class="sxs-lookup"><span data-stu-id="17e07-116">If instead you wish to use a .NET Standard class library, then you will need to use a startup project that targets .NET Framework or .NET Core so that the tooling has a conrete target platform into which it can load your class library.</span></span> <span data-ttu-id="17e07-117">Il progetto di avvio può essere un progetto fittizio senza vero codice. È infatti necessario solo per specificare una destinazione per gli strumenti.</span><span class="sxs-lookup"><span data-stu-id="17e07-117">This startup project can be a dummy project with no real code--it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="17e07-118">Se il progetto è destinato a un altro framework (ad esempio, Windows universale o Xamarin), sarà necessario creare una libreria di classi .NET Standard separata.</span><span class="sxs-lookup"><span data-stu-id="17e07-118">If your project targets another framework (for example, Universal Windows or Xamarin), then you will need to create a separate .NET Standard class library.</span></span> <span data-ttu-id="17e07-119">In questo caso, seguire le linee guida precedenti per creare anche un progetto di avvio che possa essere usato dagli strumenti.</span><span class="sxs-lookup"><span data-stu-id="17e07-119">In this case, follow the guidance above to also create a startup project that can be used by the tooling.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="17e07-120">Progetti di avvio e destinazione</span><span class="sxs-lookup"><span data-stu-id="17e07-120">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="17e07-121">Ogni volta che si richiama un comando, sono coinvolti due progetti: il progetto di destinazione e il progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="17e07-121">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="17e07-122">Il progetto di destinazione è dove vengono aggiunti, in alcuni casi rimossi, tutti i file.</span><span class="sxs-lookup"><span data-stu-id="17e07-122">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="17e07-123">Il progetto di avvio è quello emulato dagli strumenti quando si esegue il codice del progetto.</span><span class="sxs-lookup"><span data-stu-id="17e07-123">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="17e07-124">Il progetto di destinazione e il progetto di avvio possono coincidere.</span><span class="sxs-lookup"><span data-stu-id="17e07-124">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
