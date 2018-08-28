---
title: Provider di database SQLite - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 31de8449a12a10d4f98ebb4bb6125389606e9bbd
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994002"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="b5b51-102">Provider di database SQLite per Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="b5b51-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="b5b51-103">Questo provider di database consente l'uso di Entity Framework Core (EF Core) con SQLite.</span><span class="sxs-lookup"><span data-stu-id="b5b51-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="b5b51-104">Il provider viene gestito nell'ambito del [progetto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="b5b51-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="b5b51-105">Installazione di</span><span class="sxs-lookup"><span data-stu-id="b5b51-105">Install</span></span>

<span data-ttu-id="b5b51-106">Installare il [pacchetto NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="b5b51-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="b5b51-107">Introduzione</span><span class="sxs-lookup"><span data-stu-id="b5b51-107">Get Started</span></span>

<span data-ttu-id="b5b51-108">Per acquisire familiarità con il provider, usare le risorse seguenti.</span><span class="sxs-lookup"><span data-stu-id="b5b51-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="b5b51-109">SQLite locale in piattaforma UWP</span><span class="sxs-lookup"><span data-stu-id="b5b51-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="b5b51-110">Applicazione di .NET Core al nuovo Database SQLite</span><span class="sxs-lookup"><span data-stu-id="b5b51-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="b5b51-111">Applicazione di esempio Unicorn Clicker</span><span class="sxs-lookup"><span data-stu-id="b5b51-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="b5b51-112">Applicazione di esempio Unicorn Packer</span><span class="sxs-lookup"><span data-stu-id="b5b51-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="b5b51-113">Motori di database supportati</span><span class="sxs-lookup"><span data-stu-id="b5b51-113">Supported Database Engines</span></span>

* <span data-ttu-id="b5b51-114">SQLite (3.7 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="b5b51-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="b5b51-115">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="b5b51-115">Supported Platforms</span></span>

* <span data-ttu-id="b5b51-116">.NET Framework (4.5.1 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="b5b51-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="b5b51-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5b51-117">.NET Core</span></span>

* <span data-ttu-id="b5b51-118">Mono (4.2.0 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="b5b51-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="b5b51-119">Piattaforma UWP (Universal Windows Platform)</span><span class="sxs-lookup"><span data-stu-id="b5b51-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="b5b51-120">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="b5b51-120">Limitations</span></span>

<span data-ttu-id="b5b51-121">Vedere [Limitazioni SQLite](limitations.md) per alcune importanti limitazioni del provider di SQLite.</span><span class="sxs-lookup"><span data-stu-id="b5b51-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
