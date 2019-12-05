---
title: Provider di database Microsoft SQL Server - EF Core
description: Documentazione per il provider di database che consente l'uso di Entity Framework Core con Microsoft SQL Server
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: 18a69789ff4ae013c1d60bb6d34ca5c27ee285c2
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824779"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Provider di database Microsoft SQL Server per EF Core

Questo provider di database consente l'uso di Entity Framework Core con Microsoft SQL Server (incluso il database SQL di Azure). Il provider viene gestito nell'ambito del [progetto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Installazione di

Installare il [pacchetto NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

## <a name="net-core-clitabdotnet-core-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> A partire dalla versione 3.0.0, il provider fa riferimento a Microsoft.Data.SqlClient (le versioni precedenti dipendono da System.Data.SqlClient). Se il progetto assume una dipendenza diretta da SqlClient, assicurarsi che faccia riferimento al pacchetto Microsoft.Data.SqlClient.

## <a name="supported-database-engines"></a>Motori di database supportati

* Microsoft SQL Server (2012 e versioni successive)
