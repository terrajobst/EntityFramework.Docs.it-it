---
title: Provider di database SQLite - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 2e392f382f0e6f4d092a362c44f2149eb336db17
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678748"
---
# <a name="sqlite-ef-core-database-provider"></a>Provider di database SQLite per Entity Framework Core

Questo provider di database consente l'uso di Entity Framework Core (EF Core) con SQLite. Il provider viene gestito nell'ambito del [progetto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Installazione di

Installare il [pacchetto NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a>Introduzione

Per acquisire familiarità con il provider, usare le risorse seguenti.
* [SQLite locale in piattaforma UWP](../../get-started/uwp/getting-started.md)

* [Applicazione di .NET Core al nuovo Database SQLite](../../get-started/netcore/new-db-sqlite.md)

* [Applicazione di esempio Unicorn Clicker](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [Applicazione di esempio Unicorn Packer](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a>Motori di database supportati

* SQLite (3.7 e versioni successive)

## <a name="supported-platforms"></a>Piattaforme supportate

* .NET Framework (4.5.1 e versioni successive)

* .NET Core

* Mono (4.2.0 e versioni successive)

* Piattaforma UWP (Universal Windows Platform)

## <a name="limitations"></a>Limitazioni

Vedere [Limitazioni SQLite](limitations.md) per alcune importanti limitazioni del provider di SQLite.
