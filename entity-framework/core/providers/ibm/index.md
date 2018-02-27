---
title: Provider di database IBM Data Server (DB2) - EF Core
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a>Provider di database IBM Server (DB2) EF Core

Questo provider di database consente l'uso di Entity Framework Core (EF Core) con IBM. Il provider è gestito da IBM.

> [!NOTE]  
> Il provider non viene gestito nell'ambito del progetto Entity Framework Core. Quando si prende in considerazione un provider di terze parti, valutarne con cura gli aspetti relativi a qualità, licenze, supporto e così via, per essere certi che soddisfi i requisiti correnti.

## <a name="install"></a>Installazione di

Per lavorare con IBM Data Server in Windows, installare il [pacchetto NuGet IBM EntityFrameworkCore ](https://www.nuget.org/packages/IBM.EntityFrameworkCore).

``` powershell
Install-Package IBM.EntityFrameworkCore
```

Per lavorare con IBM Data Server in Linux, installare il [pacchetto NuGet IBM EntityFrameworkCore ](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a>Introduzione

[Guida introduttiva al provider IBM .NET per .NET Core](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en)

## <a name="supported-database-engines"></a>Motori di database supportati

* zOS
* LUW tra cui IBM dashDB
* IBM I
* Informix

## <a name="supported-platforms"></a>Piattaforme supportate

* .NET Framework (4.6 e versioni successive)
* .NET Core
