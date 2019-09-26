---
title: Novità - EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: c49f4cba0066d1e218f11c3959d96f9cafa913f4
ms.sourcegitcommit: 7bc43f21e7bdd64926314ea949aae689f1911956
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266788"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="6963d-102">Novità di EF6</span><span class="sxs-lookup"><span data-stu-id="6963d-102">What's new in EF6</span></span>

<span data-ttu-id="6963d-103">Per ottenere le funzionalità più recenti e il livello di stabilità più elevato è consigliabile usare l'ultima versione rilasciata di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6963d-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="6963d-104">È possibile, tuttavia, usare una versione precedente o provare i miglioramenti resi disponibili nella versione provvisoria più recente.</span><span class="sxs-lookup"><span data-stu-id="6963d-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="6963d-105">Per installare versioni specifiche di Entity Framework, vedere [Get Entity Framework](~/ef6/fundamentals/install.md) (Ottenere Entity Framework).</span><span class="sxs-lookup"><span data-stu-id="6963d-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="6963d-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="6963d-106">EF 6.3.0</span></span>

<span data-ttu-id="6963d-107">Il runtime di EF 6.3.0 è stato rilasciato in NuGet nel mese di settembre 2019.</span><span class="sxs-lookup"><span data-stu-id="6963d-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="6963d-108">L'obiettivo principale di questa versione è quello di semplificare la migrazione delle applicazioni esistenti che usano EF 6 a .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="6963d-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="6963d-109">La community ha anche contribuito a diverse correzioni di bug e miglioramenti.</span><span class="sxs-lookup"><span data-stu-id="6963d-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="6963d-110">Per informazioni dettagliate, vedere i problemi chiusi in ogni [fase cardine](https://github.com/aspnet/EntityFramework6/milestones?state=closed) della versione 6.3.0.</span><span class="sxs-lookup"><span data-stu-id="6963d-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="6963d-111">Ecco alcuni dei più importanti:</span><span class="sxs-lookup"><span data-stu-id="6963d-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="6963d-112">Supporto per .NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="6963d-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="6963d-113">Il pacchetto EntityFramework ora ha come destinazione .NET Standard 2.1 oltre a .NET Framework 4.x.</span><span class="sxs-lookup"><span data-stu-id="6963d-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x.</span></span>
  - <span data-ttu-id="6963d-114">Questo significa che EF 6.3 è multipiattaforma e supportato in altri sistemi operativi oltre a Windows, come Linux e macOS.</span><span class="sxs-lookup"><span data-stu-id="6963d-114">This means that EF 6.3 is cross-platform and supported on other operating systems besides Windows, like Linux and macOS.</span></span>
  - <span data-ttu-id="6963d-115">I comandi per le migrazioni sono stati riscritti per l'esecuzione out-of-process e supportano i progetti in stile SDK.</span><span class="sxs-lookup"><span data-stu-id="6963d-115">The migrations commands have been rewritten to execute out of process and work with SDK-style projects.</span></span>
- <span data-ttu-id="6963d-116">Supporto di HierarchyId di SQL Server</span><span class="sxs-lookup"><span data-stu-id="6963d-116">Support for SQL Server HierarchyId</span></span>
- <span data-ttu-id="6963d-117">Compatibilità migliorata con Roslyn e NuGet PackageReference</span><span class="sxs-lookup"><span data-stu-id="6963d-117">Improved compatibility with Roslyn and NuGet PackageReference</span></span>
- <span data-ttu-id="6963d-118">Aggiunta dell'utilità `ef6.exe` per l'abilitazione, l'aggiunta, l'esecuzione di script e l'applicazione di migrazioni da assembly.</span><span class="sxs-lookup"><span data-stu-id="6963d-118">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="6963d-119">Sostituisce `migrate.exe`</span><span class="sxs-lookup"><span data-stu-id="6963d-119">This replaces `migrate.exe`</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="6963d-120">Supporto di EF Designer</span><span class="sxs-lookup"><span data-stu-id="6963d-120">EF designer support</span></span>

<span data-ttu-id="6963d-121">Attualmente non è disponibile alcun supporto per l'uso di EF Designer direttamente nei progetti .NET Core o .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="6963d-121">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects.</span></span> 

<span data-ttu-id="6963d-122">Per ovviare a questa limitazione, è possibile aggiungere il file EDMX e le classi generate per le entità e DbContext come file collegati a un progetto .NET Core 3.0 o .NET Standard 2.1 nella stessa soluzione.</span><span class="sxs-lookup"><span data-stu-id="6963d-122">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="6963d-123">Nel file di progetto i file collegati appariranno come segue:</span><span class="sxs-lookup"><span data-stu-id="6963d-123">The linked files will look like this in the project file:</span></span>

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

<span data-ttu-id="6963d-124">Si noti che il file EDMX è collegato all'azione di compilazione EntityDeploy.</span><span class="sxs-lookup"><span data-stu-id="6963d-124">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="6963d-125">Si tratta di un'attività MSBuild speciale (ora inclusa nel pacchetto EF 6.3) che gestisce l'aggiunta del modello EF nell'assembly di destinazione come risorse incorporate (o la copia come file nella cartella di output, a seconda dell'impostazione di elaborazione degli artefatti dei metadati in EDMX).</span><span class="sxs-lookup"><span data-stu-id="6963d-125">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="6963d-126">Per altri dettagli su come ottenere questa configurazione, vedere l'[esempio EDMX .NET Core](https://aka.ms/EdmxDotNetCoreSample).</span><span class="sxs-lookup"><span data-stu-id="6963d-126">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

## <a name="past-releases"></a><span data-ttu-id="6963d-127">Versioni precedenti</span><span class="sxs-lookup"><span data-stu-id="6963d-127">Past Releases</span></span>

<span data-ttu-id="6963d-128">La pagina [Versioni precedenti](past-releases.md) contiene un archivio di tutte le versioni precedenti di Entity Framework e delle funzionalità principali introdotte in ogni versione.</span><span class="sxs-lookup"><span data-stu-id="6963d-128">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
