---
title: Provider di database InMemory - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: 356af9390a8aafa5afe35f333cd1e6ac1988390d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678986"
---
# <a name="ef-core-in-memory-database-provider"></a>Provider di database InMemory per Entity Framework Core

Questo provider di database consente l'uso di Entity Framework Core con un database in memoria. Ciò può essere utile per il testing, anche se il provider SQLite in modalità in memoria può essere un sostituto per i test più appropriato per i database relazionali. Il provider viene gestito nell'ambito del [progetto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Installazione di

Installare il [pacchetto NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a>Introduzione

Per acquisire familiarità con il provider, usare le risorse seguenti.
* [Testing with InMemory](../../miscellaneous/testing/in-memory.md) (Test con InMemory)

* [UnicornStore Sample Application Tests](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs) (Test dell'applicazione di esempio UnicornStore)

## <a name="supported-database-engines"></a>Motori di database supportati

* Database in memoria incorporato (solo a scopo di test)

## <a name="supported-platforms"></a>Piattaforme supportate

* .NET Framework (4.5.1 e versioni successive)

* .NET Core

* Mono (4.2.0 e versioni successive)

* Piattaforma UWP (Universal Windows Platform)
