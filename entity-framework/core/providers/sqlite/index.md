---
title: Provider di database SQLite - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 0f3905a491fb5f7a657ce9037d166771e1f326d8
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="82ad3-102">Provider di database SQLite per Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="82ad3-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="82ad3-103">Questo provider di database consente l'uso di Entity Framework Core (EF Core) con SQLite.</span><span class="sxs-lookup"><span data-stu-id="82ad3-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="82ad3-104">Il provider viene gestito nel quadro del [progetto GitHub EntityFramework](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="82ad3-104">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="82ad3-105">Installazione di</span><span class="sxs-lookup"><span data-stu-id="82ad3-105">Install</span></span>

<span data-ttu-id="82ad3-106">Installare il [pacchetto NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="82ad3-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="82ad3-107">Introduzione</span><span class="sxs-lookup"><span data-stu-id="82ad3-107">Get Started</span></span>

<span data-ttu-id="82ad3-108">Per acquisire familiarità con il provider, usare le risorse seguenti.</span><span class="sxs-lookup"><span data-stu-id="82ad3-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="82ad3-109">SQLite locale in piattaforma UWP</span><span class="sxs-lookup"><span data-stu-id="82ad3-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="82ad3-110">Applicazione di .NET Core al nuovo Database SQLite</span><span class="sxs-lookup"><span data-stu-id="82ad3-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="82ad3-111">Applicazione di esempio Unicorn Clicker</span><span class="sxs-lookup"><span data-stu-id="82ad3-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="82ad3-112">Applicazione di esempio Unicorn Packer</span><span class="sxs-lookup"><span data-stu-id="82ad3-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="82ad3-113">Motori di database supportati</span><span class="sxs-lookup"><span data-stu-id="82ad3-113">Supported Database Engines</span></span>

* <span data-ttu-id="82ad3-114">SQLite (3.7 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="82ad3-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="82ad3-115">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="82ad3-115">Supported Platforms</span></span>

* <span data-ttu-id="82ad3-116">.NET Framework (4.5.1 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="82ad3-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="82ad3-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="82ad3-117">.NET Core</span></span>

* <span data-ttu-id="82ad3-118">Mono (4.2.0 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="82ad3-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="82ad3-119">Piattaforma UWP (Universal Windows Platform)</span><span class="sxs-lookup"><span data-stu-id="82ad3-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="82ad3-120">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="82ad3-120">Limitations</span></span>

<span data-ttu-id="82ad3-121">Vedere [Limitazioni SQLite](limitations.md) per alcune importanti limitazioni del provider di SQLite.</span><span class="sxs-lookup"><span data-stu-id="82ad3-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
