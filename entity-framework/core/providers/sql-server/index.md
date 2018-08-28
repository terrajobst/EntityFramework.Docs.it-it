---
title: Provider di database Microsoft SQL Server - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: a524794a61a9f5078998aea04b45c31c19357f2b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995669"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="91fec-102">Provider di database Microsoft SQL Server per EF Core</span><span class="sxs-lookup"><span data-stu-id="91fec-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="91fec-103">Questo provider di database consente l'uso di Entity Framework Core con Microsoft SQL Server (incluso SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="91fec-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="91fec-104">Il provider viene gestito nell'ambito del [progetto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="91fec-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="91fec-105">Installazione di</span><span class="sxs-lookup"><span data-stu-id="91fec-105">Install</span></span>

<span data-ttu-id="91fec-106">Installare il [pacchetto NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="91fec-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="91fec-107">Introduzione</span><span class="sxs-lookup"><span data-stu-id="91fec-107">Get Started</span></span>

<span data-ttu-id="91fec-108">Per acquisire familiarità con il provider, usare le risorse seguenti.</span><span class="sxs-lookup"><span data-stu-id="91fec-108">The following resources will help you get started with this provider.</span></span>
* <span data-ttu-id="91fec-109">[Getting Started on .NET Framework (Console, WinForms, WPF, etc.)](../../get-started/full-dotnet/index.md) (Introduzione a .NET Framework - Console, WinForms, WPF e così via)</span><span class="sxs-lookup"><span data-stu-id="91fec-109">[Getting Started on .NET Framework (Console, WinForms, WPF, etc.)](../../get-started/full-dotnet/index.md)</span></span>

* <span data-ttu-id="91fec-110">[Getting Started on ASP.NET Core](../../get-started/aspnetcore/index.md) (Introduzione ad ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="91fec-110">[Getting Started on ASP.NET Core](../../get-started/aspnetcore/index.md)</span></span>

* <span data-ttu-id="91fec-111">[UnicornStore Sample Application](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore) (Applicazione di esempio UnicornStore)</span><span class="sxs-lookup"><span data-stu-id="91fec-111">[UnicornStore Sample Application](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="91fec-112">Motori di database supportati</span><span class="sxs-lookup"><span data-stu-id="91fec-112">Supported Database Engines</span></span>

* <span data-ttu-id="91fec-113">Microsoft SQL Server (2008 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="91fec-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="91fec-114">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="91fec-114">Supported Platforms</span></span>

* <span data-ttu-id="91fec-115">.NET Framework (4.5.1 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="91fec-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="91fec-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="91fec-116">.NET Core</span></span>

* <span data-ttu-id="91fec-117">Mono (4.2.0 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="91fec-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
