---
title: Provider di database MySQL - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: 1500d017cb463c3f394131a79b9063ff90cce5e2
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/27/2018
---
# <a name="mysql-ef-core-database-provider"></a><span data-ttu-id="7a7d9-102">Provider di database MySQL per Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="7a7d9-102">MySQL EF Core Database Provider</span></span>

<span data-ttu-id="7a7d9-103">Questo provider di database consente l'uso di Entity Framework Core (EF Core) con MySQL.</span><span class="sxs-lookup"><span data-stu-id="7a7d9-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="7a7d9-104">Il provider viene gestito nel quadro del [progetto MySQL](http://dev.mysql.com).</span><span class="sxs-lookup"><span data-stu-id="7a7d9-104">The provider is maintained as part of the [MySQL project](http://dev.mysql.com).</span></span>

> [!WARNING]  
> <span data-ttu-id="7a7d9-105">Questo provider è una versione non definitiva.</span><span class="sxs-lookup"><span data-stu-id="7a7d9-105">This provider is pre-release.</span></span>

> [!NOTE]  
> <span data-ttu-id="7a7d9-106">Il provider non viene gestito nell'ambito del progetto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="7a7d9-106">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="7a7d9-107">Quando si prende in considerazione un provider di terze parti, valutarne con cura gli aspetti relativi a qualità, licenze, supporto e così via, per essere certi che soddisfi i requisiti correnti.</span><span class="sxs-lookup"><span data-stu-id="7a7d9-107">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="7a7d9-108">Installazione di</span><span class="sxs-lookup"><span data-stu-id="7a7d9-108">Install</span></span>

<span data-ttu-id="7a7d9-109">Installare il [pacchetto NuGet MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="7a7d9-109">Install the [MySql.Data.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).</span></span>

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a><span data-ttu-id="7a7d9-110">Introduzione</span><span class="sxs-lookup"><span data-stu-id="7a7d9-110">Get Started</span></span>

<span data-ttu-id="7a7d9-111">Vedere [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/) (Iniziare a usare il provider EF Core MySQL e Connector/Net 7.0.4).</span><span class="sxs-lookup"><span data-stu-id="7a7d9-111">See [Starting with MySQL EF Core provider and Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="7a7d9-112">Motori di database supportati</span><span class="sxs-lookup"><span data-stu-id="7a7d9-112">Supported Database Engines</span></span>

* <span data-ttu-id="7a7d9-113">MySQL</span><span class="sxs-lookup"><span data-stu-id="7a7d9-113">MySQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="7a7d9-114">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="7a7d9-114">Supported Platforms</span></span>

* <span data-ttu-id="7a7d9-115">.NET Framework (4.5.1 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="7a7d9-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="7a7d9-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="7a7d9-116">.NET Core</span></span>

<span data-ttu-id="7a7d9-117">Consultare la documentazione di MySQL per informazioni sulla compatibilità della versione [qui](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) e [qui](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)</span><span class="sxs-lookup"><span data-stu-id="7a7d9-117">Be sure to review the MySQL documentation for version compatibility information [here](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) and [here](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)</span></span>
