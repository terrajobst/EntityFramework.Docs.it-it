---
title: Provider di database SQLite - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 0f8b0044e11ba665b610ac583bf3a79ed7174630
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813485"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="0d3c0-102">Provider di database SQLite per Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="0d3c0-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="0d3c0-103">Questo provider di database consente l'uso di Entity Framework Core (EF Core) con SQLite.</span><span class="sxs-lookup"><span data-stu-id="0d3c0-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="0d3c0-104">Il provider viene gestito nell'ambito del [progetto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="0d3c0-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="0d3c0-105">Installazione di</span><span class="sxs-lookup"><span data-stu-id="0d3c0-105">Install</span></span>

<span data-ttu-id="0d3c0-106">Installare il [pacchetto NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="0d3c0-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

# <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="0d3c0-107">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="0d3c0-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

# <a name="visual-studiotabvs"></a>[<span data-ttu-id="0d3c0-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0d3c0-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="0d3c0-109">Motori di database supportati</span><span class="sxs-lookup"><span data-stu-id="0d3c0-109">Supported Database Engines</span></span>

* <span data-ttu-id="0d3c0-110">SQLite (3.7 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="0d3c0-110">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="0d3c0-111">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="0d3c0-111">Limitations</span></span>

<span data-ttu-id="0d3c0-112">Vedere [Limitazioni SQLite](limitations.md) per alcune importanti limitazioni del provider di SQLite.</span><span class="sxs-lookup"><span data-stu-id="0d3c0-112">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
