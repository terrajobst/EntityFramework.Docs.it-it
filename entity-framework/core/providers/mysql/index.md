---
title: Provider di database MySQL - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: c151845c8b08ef6a668b352f15545752156b0a9d
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
# <a name="mysql-ef-core-database-provider"></a>Provider di database MySQL per Entity Framework Core

Questo provider di database consente l'uso di Entity Framework Core (EF Core) con MySQL. Il provider viene gestito nel quadro del [progetto MySQL](http://dev.mysql.com).

> [!WARNING]  
> Questo provider è una versione non definitiva.

> [!NOTE]  
> Il provider non viene gestito nell'ambito del progetto Entity Framework Core. Quando si prende in considerazione un provider di terze parti, valutarne con cura gli aspetti relativi a qualità, licenze, supporto e così via, per essere certi che soddisfi i requisiti correnti.

## <a name="install"></a>Installazione di

Installare il [pacchetto NuGet MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a>Introduzione

Vedere [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/) (Iniziare a usare il provider EF Core MySQL e Connector/Net 7.0.4).

## <a name="supported-database-engines"></a>Motori di database supportati

* MySQL

## <a name="supported-platforms"></a>Piattaforme supportate

* .NET Framework (4.5.1 e versioni successive)

* .NET Core
