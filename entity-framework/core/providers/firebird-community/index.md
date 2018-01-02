---
title: Provider di database FirebirdSQL - EF Core
author: ralmsdeveloper
ms.author: ralmsdeveloper
ms.date: 11/22/2017
ms.assetid: d0168c04-d30d-4219-98f8-a54690cea3c6
ms.technology: entity-framework-core
uid: core/providers/firebird-community/index
ms.openlocfilehash: 682988a91ef04dbd552588a537f53124b931f17d
ms.sourcegitcommit: 1cbd3d3cd92bdaf8223b8821c58200bcfed10ede
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/23/2017
---
# <a name="firebird-ef-core-database-provider"></a><span data-ttu-id="2188a-102">Provider di database Firebird per Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="2188a-102">Firebird EF Core Database Provider</span></span>

<span data-ttu-id="2188a-103">Questo provider di database consente l'uso di Entity Framework Core (EF Core) con FirebirdSQL.</span><span class="sxs-lookup"><span data-stu-id="2188a-103">This database provider allows Entity Framework Core to be used with FirebirdSQL.</span></span> <span data-ttu-id="2188a-104">Il provider viene gestito nel quadro del [progetto GitHub ralmsdeveloper/EntityFrameworkCore.FirebirdSQL](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL).</span><span class="sxs-lookup"><span data-stu-id="2188a-104">The provider is maintained as part of the [ralmsdeveloper/EntityFrameworkCore.FirebirdSQL GitHub Project](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="2188a-105">Il provider non viene gestito nell'ambito del progetto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="2188a-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="2188a-106">Quando si prende in considerazione un provider di terze parti, valutarne con cura gli aspetti relativi a qualità, licenze, supporto e così via, per essere certi che soddisfi i requisiti correnti.</span><span class="sxs-lookup"><span data-stu-id="2188a-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="2188a-107">Installazione di</span><span class="sxs-lookup"><span data-stu-id="2188a-107">Install</span></span>

<span data-ttu-id="2188a-108">Installare il [pacchetto NuGet EntityFrameworkCore.FirebirdSQL](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span><span class="sxs-lookup"><span data-stu-id="2188a-108">Install the [EntityFrameworkCore.FirebirdSQL NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span></span>

``` powershell
Install-Package EntityFrameworkCore.FirebirdSQL
```

## <a name="get-started"></a><span data-ttu-id="2188a-109">Introduzione</span><span class="sxs-lookup"><span data-stu-id="2188a-109">Get Started</span></span>

<span data-ttu-id="2188a-110">Vedere la [documentazione di introduzione nel sito del progetto](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)</span><span class="sxs-lookup"><span data-stu-id="2188a-110">See the [getting started documentation on the project site](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="2188a-111">Motori di database supportati</span><span class="sxs-lookup"><span data-stu-id="2188a-111">Supported Database Engines</span></span>

* <span data-ttu-id="2188a-112">FirebirdSql 2.5</span><span class="sxs-lookup"><span data-stu-id="2188a-112">FirebirdSql 2.5</span></span>
* <span data-ttu-id="2188a-113">FirebirdSql 3.X</span><span class="sxs-lookup"><span data-stu-id="2188a-113">FirebirdSql 3.X</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="2188a-114">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="2188a-114">Supported Platforms</span></span>

* <span data-ttu-id="2188a-115">.NET Framework (4.5.1 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="2188a-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="2188a-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="2188a-116">.NET Core</span></span>

* <span data-ttu-id="2188a-117">Mono (4.2.0 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="2188a-117">Mono (4.2.0 onwards)</span></span>
