---
title: Provider di database Microsoft SQL Server Compact Edition - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 073f0004-3eb5-4618-ab93-0674910e1819
ms.technology: entity-framework-core
uid: core/providers/sql-compact/index
ms.openlocfilehash: d8b73621bdd363efec5bb7728886e0a0f6bdcf76
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-compact-edition-ef-core-database-provider"></a>Provider di database Microsoft SQL Server Compact Edition EF Core

Questo provider di database consente l'uso di Entity Framework Core (EF Core) con SQL Server Compact Edition. Il provider viene gestito nel quadro del [progetto GitHub ErikEJ/EntityFramework.SqlServerCompact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).

> [!NOTE]  
> Il provider non viene gestito nell'ambito del progetto Entity Framework Core. Quando si prende in considerazione un provider di terze parti, valutarne con cura gli aspetti relativi a qualità, licenze, supporto e così via, per essere certi che soddisfi i requisiti correnti.

## <a name="install"></a>Installazione di

Per lavorare con SQL Server Compact Edition 4.0, installare il [pacchetto NuGet EntityFrameworkCore.SqlServerCompact40](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact40).

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact40
```

Per lavorare con SQL Server Compact Edition 3.5, installare il [pacchetto NuGet EntityFrameworkCore.SqlServerCompact35](https://www.nuget.org/packages/EntityFrameworkCore.SqlServerCompact35).

``` powershell
Install-Package EntityFrameworkCore.SqlServerCompact35
```

## <a name="get-started"></a>Introduzione

Vedere la [documentazione di introduzione nel sito del progetto](https://github.com/ErikEJ/EntityFramework.SqlServerCompact/wiki/Using-EF-Core-with-SQL-Server-Compact-in-Traditional-.NET-Applications)

## <a name="supported-database-engines"></a>Motori di database supportati

* SQL Server Compact Edition 3.5

* SQL Server Compact Edition 4.0

## <a name="supported-platforms"></a>Piattaforme supportate

* .NET Framework (4.5.1 e versioni successive)
