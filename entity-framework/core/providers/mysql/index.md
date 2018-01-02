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
# <a name="mysql-ef-core-database-provider"></a><span data-ttu-id="c8cdc-102">Provider di database MySQL per Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c8cdc-102">MySQL EF Core Database Provider</span></span>

<span data-ttu-id="c8cdc-103">Questo provider di database consente l'uso di Entity Framework Core (EF Core) con MySQL.</span><span class="sxs-lookup"><span data-stu-id="c8cdc-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="c8cdc-104">Il provider viene gestito nel quadro del [progetto MySQL](http://dev.mysql.com).</span><span class="sxs-lookup"><span data-stu-id="c8cdc-104">The provider is maintained as part of the [MySQL project](http://dev.mysql.com).</span></span>

> [!WARNING]  
> <span data-ttu-id="c8cdc-105">Questo provider è una versione non definitiva.</span><span class="sxs-lookup"><span data-stu-id="c8cdc-105">This provider is pre-release.</span></span>

> [!NOTE]  
> <span data-ttu-id="c8cdc-106">Il provider non viene gestito nell'ambito del progetto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c8cdc-106">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="c8cdc-107">Quando si prende in considerazione un provider di terze parti, valutarne con cura gli aspetti relativi a qualità, licenze, supporto e così via, per essere certi che soddisfi i requisiti correnti.</span><span class="sxs-lookup"><span data-stu-id="c8cdc-107">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="c8cdc-108">Installazione di</span><span class="sxs-lookup"><span data-stu-id="c8cdc-108">Install</span></span>

<span data-ttu-id="c8cdc-109">Installare il [pacchetto NuGet MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="c8cdc-109">Install the [MySql.Data.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span></span>

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a><span data-ttu-id="c8cdc-110">Introduzione</span><span class="sxs-lookup"><span data-stu-id="c8cdc-110">Get Started</span></span>

<span data-ttu-id="c8cdc-111">Vedere [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/) (Iniziare a usare il provider EF Core MySQL e Connector/Net 7.0.4).</span><span class="sxs-lookup"><span data-stu-id="c8cdc-111">See [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="c8cdc-112">Motori di database supportati</span><span class="sxs-lookup"><span data-stu-id="c8cdc-112">Supported Database Engines</span></span>

* <span data-ttu-id="c8cdc-113">MySQL</span><span class="sxs-lookup"><span data-stu-id="c8cdc-113">MySQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="c8cdc-114">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="c8cdc-114">Supported Platforms</span></span>

* <span data-ttu-id="c8cdc-115">.NET Framework (4.5.1 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="c8cdc-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="c8cdc-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="c8cdc-116">.NET Core</span></span>
