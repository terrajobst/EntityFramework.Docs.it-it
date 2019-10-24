---
title: Provider di database Microsoft SQL Server - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: 1e75bc4bf334b1a60d13a2ec9ef314e3afcf0273
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812098"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Provider di database Microsoft SQL Server per EF Core

Questo provider di database consente l'uso di Entity Framework Core con Microsoft SQL Server (incluso SQL Azure). Il provider viene gestito nell'ambito del [progetto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Installazione di

Installare il [pacchetto NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

# <a name="net-core-clitabdotnet-core-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

# <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> A partire dalla versione 3.0.0, il provider fa riferimento a Microsoft.Data.SqlClient (le versioni precedenti dipendono da System.Data.SqlClient). Se il progetto assume una dipendenza diretta da SqlClient, verificare che faccia riferimento al pacchetto corretto.

## <a name="supported-database-engines"></a>Motori di database supportati

* Microsoft SQL Server (2012 e versioni successive)
