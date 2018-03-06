---
title: Provider di database Npgsql per PostgreSQL - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/27/2018
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a>Provider di database Npgsql EF Core per PostgreSQL

Questo provider di database consente l'uso di Entity Framework Core con PostgreSQL. Il provider viene gestito nel quadro del [progetto Npgsql](http://www.npgsql.org).

> [!NOTE]  
> Il provider non viene gestito nell'ambito del progetto Entity Framework Core. Quando si prende in considerazione un provider di terze parti, valutarne con cura gli aspetti relativi a qualità, licenze, supporto e così via, per essere certi che soddisfi i requisiti correnti.

## <a name="install"></a>Installazione di

Installare il [pacchetto NuGet Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a>Introduzione

Vedere la [documentazione di Npgsql](http://www.npgsql.org/efcore/index.html) per iniziare.

## <a name="supported-database-engines"></a>Motori di database supportati

* PostgreSQL

## <a name="supported-platforms"></a>Piattaforme supportate

* .NET Framework (4.5.1 e versioni successive)

* .NET Core

* Mono (4.2.0 e versioni successive)
