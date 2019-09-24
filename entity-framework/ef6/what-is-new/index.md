---
title: Novità - EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 568790d9c9bb7dd2213907bef8fa090710cd3ba0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149121"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="b7700-102">Novità di EF6</span><span class="sxs-lookup"><span data-stu-id="b7700-102">What's New in EF6</span></span>

<span data-ttu-id="b7700-103">Per ottenere le funzionalità più recenti e il livello di stabilità più elevato è consigliabile usare l'ultima versione rilasciata di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b7700-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="b7700-104">È possibile, tuttavia, usare una versione precedente o provare i miglioramenti resi disponibili nella versione provvisoria più recente.</span><span class="sxs-lookup"><span data-stu-id="b7700-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="b7700-105">Per installare versioni specifiche di Entity Framework, vedere [Get Entity Framework](~/ef6/fundamentals/install.md) (Ottenere Entity Framework).</span><span class="sxs-lookup"><span data-stu-id="b7700-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="b7700-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="b7700-106">EF 6.3.0</span></span>

<span data-ttu-id="b7700-107">Il runtime di EF 6.3.0 è stato rilasciato in NuGet nel mese di settembre 2019.</span><span class="sxs-lookup"><span data-stu-id="b7700-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="b7700-108">L'obiettivo principale di questa versione è quello di semplificare la migrazione delle applicazioni esistenti che usano EF 6 a .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="b7700-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="b7700-109">La community ha anche contribuito a diverse correzioni di bug e miglioramenti.</span><span class="sxs-lookup"><span data-stu-id="b7700-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="b7700-110">Per informazioni dettagliate, vedere i problemi chiusi in ogni [fase cardine](https://github.com/aspnet/EntityFramework6/milestones?state=closed) della versione 6.3.0.</span><span class="sxs-lookup"><span data-stu-id="b7700-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="b7700-111">Ecco alcuni dei più importanti:</span><span class="sxs-lookup"><span data-stu-id="b7700-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="b7700-112">Supporto per .NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="b7700-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="b7700-113">Il pacchetto EntityFramework ora ha come destinazione .NET Standard 2.1 oltre a .NET Framework 4.x</span><span class="sxs-lookup"><span data-stu-id="b7700-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x</span></span>
  - <span data-ttu-id="b7700-114">I comandi per le migrazioni sono stati riscritti per l'esecuzione out-of-process e supportano i progetti in stile SDK</span><span class="sxs-lookup"><span data-stu-id="b7700-114">The migrations commands have been rewritten to execute out of process and work with SDK-style projects</span></span>
- <span data-ttu-id="b7700-115">Supporto di HierarchyId di SQL Server</span><span class="sxs-lookup"><span data-stu-id="b7700-115">Support for SQL Server HierarchyId</span></span>
- <span data-ttu-id="b7700-116">Compatibilità migliorata con Roslyn e NuGet PackageReference</span><span class="sxs-lookup"><span data-stu-id="b7700-116">Improved compatibility with Roslyn and NuGet PackageReference</span></span>
- <span data-ttu-id="b7700-117">Aggiunta di ef6.exe per l'abilitazione, l'aggiunta, l'esecuzione di script e l'applicazione di migrazioni da assembly.</span><span class="sxs-lookup"><span data-stu-id="b7700-117">Added ef6.exe for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="b7700-118">Sostituisce migrate.exe</span><span class="sxs-lookup"><span data-stu-id="b7700-118">This replaces migrate.exe</span></span>

## <a name="past-releases"></a><span data-ttu-id="b7700-119">Versioni precedenti</span><span class="sxs-lookup"><span data-stu-id="b7700-119">Past Releases</span></span>

<span data-ttu-id="b7700-120">La pagina [Versioni precedenti](past-releases.md) contiene un archivio di tutte le versioni precedenti di Entity Framework e delle funzionalità principali introdotte in ogni versione.</span><span class="sxs-lookup"><span data-stu-id="b7700-120">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
