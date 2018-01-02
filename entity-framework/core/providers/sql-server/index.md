---
title: Provider di database Microsoft SQL Server - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: b2faf932e0484da4df0c1774afa7ba7ae2d077a5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Provider di database Microsoft SQL Server per EF Core

Questo provider di database consente l'uso di Entity Framework Core con Microsoft SQL Server (incluso SQL Azure). Il provider viene gestito nel quadro del [progetto GitHub EntityFramework](https://github.com/aspnet/EntityFramework).

## <a name="install"></a>Installazione di

Installare il [pacchetto NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a>Introduzione

Per acquisire familiarità con il provider, usare le risorse seguenti.
* [Getting Started on .NET Framework (Console, WinForms, WPF, etc.)](../../get-started/full-dotnet/index.md) (Introduzione a .NET Framework - Console, WinForms, WPF e così via)

* [Getting Started on ASP.NET Core](../../get-started/aspnetcore/index.md) (Introduzione ad ASP.NET Core)

* [UnicornStore Sample Application](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore) (Applicazione di esempio UnicornStore)

## <a name="supported-database-engines"></a>Motori di database supportati

* Microsoft SQL Server (2008 e versioni successive)

## <a name="supported-platforms"></a>Piattaforme supportate

* .NET Framework (4.5.1 e versioni successive)

* .NET Core

* Mono (4.2.0 e versioni successive)

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
