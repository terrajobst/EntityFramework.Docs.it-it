---
title: Provider di database InMemory - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: 28f5f262b41cbc1f196e41d75c8b88ca60e678fe
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149223"
---
# <a name="ef-core-in-memory-database-provider"></a><span data-ttu-id="d28ce-102">Provider di database InMemory per Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d28ce-102">EF Core In-Memory Database Provider</span></span>

<span data-ttu-id="d28ce-103">Questo provider di database consente l'uso di Entity Framework Core con un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="d28ce-103">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span> <span data-ttu-id="d28ce-104">Ciò può essere utile per il testing, anche se il provider SQLite in modalità in memoria può essere un sostituto per i test più appropriato per i database relazionali.</span><span class="sxs-lookup"><span data-stu-id="d28ce-104">This can be useful for testing, although the SQLite provider in in-memory mode may be a more appropriate test replacement for relational databases.</span></span> <span data-ttu-id="d28ce-105">Il provider viene gestito nell'ambito del [progetto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="d28ce-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="d28ce-106">Installazione di</span><span class="sxs-lookup"><span data-stu-id="d28ce-106">Install</span></span>

<span data-ttu-id="d28ce-107">Installare il [pacchetto NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="d28ce-107">Install the [Microsoft.EntityFrameworkCore.InMemory NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a><span data-ttu-id="d28ce-108">Introduzione</span><span class="sxs-lookup"><span data-stu-id="d28ce-108">Get Started</span></span>

<span data-ttu-id="d28ce-109">Per acquisire familiarità con il provider, usare le risorse seguenti.</span><span class="sxs-lookup"><span data-stu-id="d28ce-109">The following resources will help you get started with this provider.</span></span>
* <span data-ttu-id="d28ce-110">[Testing with InMemory](../../miscellaneous/testing/in-memory.md) (Test con InMemory)</span><span class="sxs-lookup"><span data-stu-id="d28ce-110">[Testing with InMemory](../../miscellaneous/testing/in-memory.md)</span></span>

* <span data-ttu-id="d28ce-111">[UnicornStore Sample Application Tests](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs) (Test dell'applicazione di esempio UnicornStore)</span><span class="sxs-lookup"><span data-stu-id="d28ce-111">[UnicornStore Sample Application Tests](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="d28ce-112">Motori di database supportati</span><span class="sxs-lookup"><span data-stu-id="d28ce-112">Supported Database Engines</span></span>

* <span data-ttu-id="d28ce-113">Database in memoria incorporato (solo a scopo di test)</span><span class="sxs-lookup"><span data-stu-id="d28ce-113">Built-in in-memory database (designed for testing purposes only)</span></span>
