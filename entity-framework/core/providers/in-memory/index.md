---
title: Provider di database InMemory - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: fd31c8ef2dc2e35e69f9845933a5578a5ff84c9c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413006"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="80f5f-102">Provider di database InMemory per Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="80f5f-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="80f5f-103">Questo provider di database consente l'uso di Entity Framework Core con un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="80f5f-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="80f5f-104">Ciò può essere utile per il testing, anche se il provider SQLite in modalità in memoria può essere un sostituto per i test più appropriato per i database relazionali.</span><span class="sxs-lookup"><span data-stu-id="80f5f-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="80f5f-105">Il provider viene gestito nell'ambito del [progetto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="80f5f-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="80f5f-106">Installazione di</span><span class="sxs-lookup"><span data-stu-id="80f5f-106">Install</span></span>

<span data-ttu-id="80f5f-107">Installare il [pacchetto NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="80f5f-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="80f5f-108">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="80f5f-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studio"></a>[<span data-ttu-id="80f5f-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="80f5f-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a><span data-ttu-id="80f5f-110">Introduzione</span><span class="sxs-lookup"><span data-stu-id="80f5f-110">Get Started</span></span>

<span data-ttu-id="80f5f-111">Per acquisire familiarità con il provider, usare le risorse seguenti.</span><span class="sxs-lookup"><span data-stu-id="80f5f-111">The following resources will help you get started with this provider.</span></span>

* <span data-ttu-id="80f5f-112">[Testing with InMemory](../../miscellaneous/testing/in-memory.md) (Test con InMemory)</span><span class="sxs-lookup"><span data-stu-id="80f5f-112">[Testing with InMemory](../../miscellaneous/testing/in-memory.md)</span></span>
* <span data-ttu-id="80f5f-113">[UnicornStore Sample Application Tests](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs) (Test dell'applicazione di esempio UnicornStore)</span><span class="sxs-lookup"><span data-stu-id="80f5f-113">[UnicornStore Sample Application Tests](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="80f5f-114">Motori di database supportati</span><span class="sxs-lookup"><span data-stu-id="80f5f-114">Supported Database Engines</span></span>

<span data-ttu-id="80f5f-115">Database con memoria In-Process (progettato solo a scopo di test)</span><span class="sxs-lookup"><span data-stu-id="80f5f-115">In-process memory database (designed for testing purposes only)</span></span>
