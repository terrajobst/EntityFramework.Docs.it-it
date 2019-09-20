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
