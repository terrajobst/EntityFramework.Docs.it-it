---
title: Provider di database IBM Data Server (DB2) - EF Core
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/27/2018
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a><span data-ttu-id="c389c-102">Provider di database IBM Server (DB2) EF Core</span><span class="sxs-lookup"><span data-stu-id="c389c-102">IBM Data Server (DB2) EF Core Database Providers</span></span>

<span data-ttu-id="c389c-103">Questo provider di database consente l'uso di Entity Framework Core (EF Core) con IBM.</span><span class="sxs-lookup"><span data-stu-id="c389c-103">This database provider allows Entity Framework Core to be used with IBM Data Server.</span></span> <span data-ttu-id="c389c-104">Il provider è gestito da IBM.</span><span class="sxs-lookup"><span data-stu-id="c389c-104">The provider is maintained by IBM.</span></span>

> [!NOTE]  
> <span data-ttu-id="c389c-105">Il provider non viene gestito nell'ambito del progetto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c389c-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="c389c-106">Quando si prende in considerazione un provider di terze parti, valutarne con cura gli aspetti relativi a qualità, licenze, supporto e così via, per essere certi che soddisfi i requisiti correnti.</span><span class="sxs-lookup"><span data-stu-id="c389c-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="c389c-107">Installazione di</span><span class="sxs-lookup"><span data-stu-id="c389c-107">Install</span></span>

<span data-ttu-id="c389c-108">Per lavorare con IBM Data Server in Windows, installare il [pacchetto NuGet IBM EntityFrameworkCore ](https://www.nuget.org/packages/IBM.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="c389c-108">To work with IBM Data Server on Windows, install the [IBM.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore
```

<span data-ttu-id="c389c-109">Per lavorare con IBM Data Server in Linux, installare il [pacchetto NuGet IBM EntityFrameworkCore ](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).</span><span class="sxs-lookup"><span data-stu-id="c389c-109">To work with IBM Data Server on Linux, install the [IBM.EntityFrameworkCore-lnx NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a><span data-ttu-id="c389c-110">Introduzione</span><span class="sxs-lookup"><span data-stu-id="c389c-110">Get Started</span></span>

[<span data-ttu-id="c389c-111">Guida introduttiva al provider IBM .NET per .NET Core</span><span class="sxs-lookup"><span data-stu-id="c389c-111">Getting started with IBM .NET Provider for .NET Core</span></span>](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en)

## <a name="supported-database-engines"></a><span data-ttu-id="c389c-112">Motori di database supportati</span><span class="sxs-lookup"><span data-stu-id="c389c-112">Supported Database Engines</span></span>

* <span data-ttu-id="c389c-113">zOS</span><span class="sxs-lookup"><span data-stu-id="c389c-113">zOS</span></span>
* <span data-ttu-id="c389c-114">LUW tra cui IBM dashDB</span><span class="sxs-lookup"><span data-stu-id="c389c-114">LUW including IBM dashDB</span></span>
* <span data-ttu-id="c389c-115">IBM I</span><span class="sxs-lookup"><span data-stu-id="c389c-115">IBM I</span></span>
* <span data-ttu-id="c389c-116">Informix</span><span class="sxs-lookup"><span data-stu-id="c389c-116">Informix</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="c389c-117">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="c389c-117">Supported Platforms</span></span>

* <span data-ttu-id="c389c-118">.NET Framework (4.6 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="c389c-118">.NET Framework (4.6 onwards)</span></span>
* <span data-ttu-id="c389c-119">.NET Core</span><span class="sxs-lookup"><span data-stu-id="c389c-119">.NET Core</span></span>
