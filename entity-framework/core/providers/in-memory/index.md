---
title: Provider di database InMemory - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: a8e05f50837f3da554b338475d24215706dfa2ec
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="49698-102">Provider di database InMemory per Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="49698-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="49698-103">Questo provider di database consente l'uso di Entity Framework Core con un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="49698-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="49698-104">Ciò è utile per il test di codice che usa Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="49698-104">This is useful when testing code that uses Entity Framework Core.</span></span> <span data-ttu-id="49698-105">Il provider viene gestito nel quadro del [progetto GitHub EntityFramework](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="49698-105">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="49698-106">Installazione di</span><span class="sxs-lookup"><span data-stu-id="49698-106">Install</span></span>

<span data-ttu-id="49698-107">Installare il [pacchetto NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="49698-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="49698-108">Introduzione</span><span class="sxs-lookup"><span data-stu-id="49698-108">Get Started</span></span>

<span data-ttu-id="49698-109">Per acquisire familiarità con il provider, usare le risorse seguenti.</span><span class="sxs-lookup"><span data-stu-id="49698-109">The following resources will help you get started with this provider.</span></span>
* <span data-ttu-id="49698-110">[Testing with InMemory](../../miscellaneous/testing/in-memory.md) (Test con InMemory)</span><span class="sxs-lookup"><span data-stu-id="49698-110">[Testing with InMemory](../../miscellaneous/testing/in-memory.md)</span></span>

* <span data-ttu-id="49698-111">[UnicornStore Sample Application Tests](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs) (Test dell'applicazione di esempio UnicornStore)</span><span class="sxs-lookup"><span data-stu-id="49698-111">[UnicornStore Sample Application Tests](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="49698-112">Motori di database supportati</span><span class="sxs-lookup"><span data-stu-id="49698-112">Supported Database Engines</span></span>

* <span data-ttu-id="49698-113">Database in memoria incorporato (solo a scopo di test)</span><span class="sxs-lookup"><span data-stu-id="49698-113">Built-in in-memory database (designed for testing purposes only)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="49698-114">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="49698-114">Supported Platforms</span></span>

* <span data-ttu-id="49698-115">.NET Framework (4.5.1 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="49698-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="49698-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="49698-116">.NET Core</span></span>

* <span data-ttu-id="49698-117">Mono (4.2.0 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="49698-117">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="49698-118">Piattaforma UWP (Universal Windows Platform)</span><span class="sxs-lookup"><span data-stu-id="49698-118">Universal Windows Platform</span></span>
