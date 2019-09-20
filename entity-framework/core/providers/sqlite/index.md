---
title: Provider di database SQLite - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: e4cbdba46f901831892192a343db2920a5760042
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149272"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="1992d-102">Provider di database SQLite per Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="1992d-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="1992d-103">Questo provider di database consente l'uso di Entity Framework Core (EF Core) con SQLite.</span><span class="sxs-lookup"><span data-stu-id="1992d-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="1992d-104">Il provider viene gestito nell'ambito del [progetto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="1992d-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="1992d-105">Installazione di</span><span class="sxs-lookup"><span data-stu-id="1992d-105">Install</span></span>

<span data-ttu-id="1992d-106">Installare il [pacchetto NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="1992d-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="supported-database-engines"></a><span data-ttu-id="1992d-107">Motori di database supportati</span><span class="sxs-lookup"><span data-stu-id="1992d-107">Supported Database Engines</span></span>

* <span data-ttu-id="1992d-108">SQLite (3.7 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="1992d-108">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="1992d-109">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="1992d-109">Limitations</span></span>

<span data-ttu-id="1992d-110">Vedere [Limitazioni SQLite](limitations.md) per alcune importanti limitazioni del provider di SQLite.</span><span class="sxs-lookup"><span data-stu-id="1992d-110">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
