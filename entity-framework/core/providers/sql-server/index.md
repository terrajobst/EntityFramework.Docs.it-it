---
title: Provider di database Microsoft SQL Server - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: dd352b81da05fa8ea8970495f20947bd109edf65
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655891"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="c36d9-102">Provider di database Microsoft SQL Server per EF Core</span><span class="sxs-lookup"><span data-stu-id="c36d9-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="c36d9-103">Questo provider di database consente l'uso di Entity Framework Core con Microsoft SQL Server (incluso SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="c36d9-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="c36d9-104">Il provider viene gestito nell'ambito del [progetto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="c36d9-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="c36d9-105">Installazione di</span><span class="sxs-lookup"><span data-stu-id="c36d9-105">Install</span></span>

<span data-ttu-id="c36d9-106">Installare il [pacchetto NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="c36d9-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="c36d9-107">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="c36d9-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="c36d9-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c36d9-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> <span data-ttu-id="c36d9-109">A partire dalla versione 3.0.0, il provider fa riferimento a Microsoft.Data.SqlClient (le versioni precedenti dipendono da System.Data.SqlClient).</span><span class="sxs-lookup"><span data-stu-id="c36d9-109">Since version 3.0.0, the provider references Microsoft.Data.SqlClient (previous versions depended on System.Data.SqlClient).</span></span> <span data-ttu-id="c36d9-110">Se il progetto assume una dipendenza diretta da SqlClient, verificare che faccia riferimento al pacchetto corretto.</span><span class="sxs-lookup"><span data-stu-id="c36d9-110">If your project takes a direct dependency on SqlClient, make sure it references the correct package.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="c36d9-111">Motori di database supportati</span><span class="sxs-lookup"><span data-stu-id="c36d9-111">Supported Database Engines</span></span>

* <span data-ttu-id="c36d9-112">Microsoft SQL Server (2012 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="c36d9-112">Microsoft SQL Server (2012 onwards)</span></span>
