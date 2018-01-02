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
# <a name="sqlite-ef-core-database-provider"></a>Provider di database SQLite per Entity Framework Core

Questo provider di database consente l'uso di Entity Framework Core (EF Core) con SQLite. Il provider viene gestito nel quadro del [progetto GitHub EntityFramework](https://github.com/aspnet/EntityFramework).

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
