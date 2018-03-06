---
title: Provider di database Microsoft SQL Server Compact Edition - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/27/2018
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a><span data-ttu-id="1cde6-102">Provider di database Microsoft SQL Server Compact Edition EF Core</span><span class="sxs-lookup"><span data-stu-id="1cde6-102">Microsoft SQL Server Compact Edition EF Core Database Provider</span></span>

<span data-ttu-id="1cde6-103">Questo provider di database consente l'uso di Entity Framework Core (EF Core) con SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="1cde6-103">This database provider allows Entity Framework Core to be used with SQL Server Compact Edition.</span></span> <span data-ttu-id="1cde6-104">Il provider viene gestito nel quadro del [progetto GitHub ErikEJ/EntityFramework.SqlServerCompact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span><span class="sxs-lookup"><span data-stu-id="1cde6-104">The provider is maintained as part of the [ErikEJ/EntityFramework.SqlServerCompact GitHub Project](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span>

> [!NOTE]  
> <span data-ttu-id="1cde6-105">Il provider non viene gestito nell'ambito del progetto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="1cde6-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="1cde6-106">Quando si prende in considerazione un provider di terze parti, valutarne con cura gli aspetti relativi a qualità, licenze, supporto e così via, per essere certi che soddisfi i requisiti correnti.</span><span class="sxs-lookup"><span data-stu-id="1cde6-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="1cde6-107">Installazione di</span><span class="sxs-lookup"><span data-stu-id="1cde6-107">Install</span></span>

<span data-ttu-id="1cde6-108">Per lavorare con SQL Server Compact Edition 4.0, installare il [pacchetto NuGet EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).</span><span class="sxs-lookup"><span data-stu-id="1cde6-108">To work with SQL Server Compact Edition 4.0, install the [EntityFrameworkCore.SqlServerCompact40 NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

<span data-ttu-id="1cde6-109">Per lavorare con SQL Server Compact Edition 3.5, installare il [pacchetto NuGet EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).</span><span class="sxs-lookup"><span data-stu-id="1cde6-109">To work with SQL Server Compact Edition 3.5, install the [EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).</span></span>

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a><span data-ttu-id="1cde6-110">Introduzione</span><span class="sxs-lookup"><span data-stu-id="1cde6-110">Get Started</span></span>

<span data-ttu-id="1cde6-111">Vedere la [documentazione di introduzione nel sito del progetto](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)</span><span class="sxs-lookup"><span data-stu-id="1cde6-111">See the [getting started documentation on the project site](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="1cde6-112">Motori di database supportati</span><span class="sxs-lookup"><span data-stu-id="1cde6-112">Supported Database Engines</span></span>

* <span data-ttu-id="1cde6-113">SQL Server Compact Edition 3.5</span><span class="sxs-lookup"><span data-stu-id="1cde6-113">SQL Server Compact Edition 3.5</span></span>

* <span data-ttu-id="1cde6-114">SQL Server Compact Edition 4.0</span><span class="sxs-lookup"><span data-stu-id="1cde6-114">SQL Server Compact Edition 4.0</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="1cde6-115">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="1cde6-115">Supported Platforms</span></span>

* <span data-ttu-id="1cde6-116">.NET Framework (4.5.1 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="1cde6-116">.NET Framework (4.5.1 onwards)</span></span>
