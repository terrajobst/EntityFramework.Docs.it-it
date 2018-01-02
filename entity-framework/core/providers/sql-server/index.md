---
title: Provider di database Microsoft SQL Server - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: b2faf932e0484da4df0c1774afa7ba7ae2d077a5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="5f6ad-102">Provider di database Microsoft SQL Server per EF Core</span><span class="sxs-lookup"><span data-stu-id="5f6ad-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="5f6ad-103">Questo provider di database consente l'uso di Entity Framework Core con Microsoft SQL Server (incluso SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="5f6ad-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="5f6ad-104">Il provider viene gestito nel quadro del [progetto GitHub EntityFramework](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="5f6ad-104">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="5f6ad-105">Installazione di</span><span class="sxs-lookup"><span data-stu-id="5f6ad-105">Install</span></span>

<span data-ttu-id="5f6ad-106">Installare il [pacchetto NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="5f6ad-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="5f6ad-107">Introduzione</span><span class="sxs-lookup"><span data-stu-id="5f6ad-107">Get Started</span></span>

<span data-ttu-id="5f6ad-108">Per acquisire familiarità con il provider, usare le risorse seguenti.</span><span class="sxs-lookup"><span data-stu-id="5f6ad-108">The following resources will help you get started with this provider.</span></span>
* <span data-ttu-id="5f6ad-109">[Getting Started on .NET Framework (Console, WinForms, WPF, etc.)](../../get-started/full-dotnet/index.md) (Introduzione a .NET Framework - Console, WinForms, WPF e così via)</span><span class="sxs-lookup"><span data-stu-id="5f6ad-109">[Getting Started on .NET Framework (Console, WinForms, WPF, etc.)](../../get-started/full-dotnet/index.md)</span></span>

* <span data-ttu-id="5f6ad-110">[Getting Started on ASP.NET Core](../../get-started/aspnetcore/index.md) (Introduzione ad ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="5f6ad-110">[Getting Started on ASP.NET Core](../../get-started/aspnetcore/index.md)</span></span>

* <span data-ttu-id="5f6ad-111">[UnicornStore Sample Application](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore) (Applicazione di esempio UnicornStore)</span><span class="sxs-lookup"><span data-stu-id="5f6ad-111">[UnicornStore Sample Application](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="5f6ad-112">Motori di database supportati</span><span class="sxs-lookup"><span data-stu-id="5f6ad-112">Supported Database Engines</span></span>

* <span data-ttu-id="5f6ad-113">Microsoft SQL Server (2008 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="5f6ad-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="5f6ad-114">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="5f6ad-114">Supported Platforms</span></span>

* <span data-ttu-id="5f6ad-115">.NET Framework (4.5.1 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="5f6ad-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="5f6ad-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="5f6ad-116">.NET Core</span></span>

* <span data-ttu-id="5f6ad-117">Mono (4.2.0 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="5f6ad-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
