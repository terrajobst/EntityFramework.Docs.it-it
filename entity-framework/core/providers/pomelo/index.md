---
title: Provider di database MySQL Pomelo - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d0198c04-d30d-4419-98f8-a54690cea3c8
ms.technology: entity-framework-core
uid: core/providers/pomelo/index
ms.openlocfilehash: 9ddcda1ae7b058c01937867817e2666b97e7f37a
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
# <a name="pomelo-ef-core-database-provider-for-mysql"></a><span data-ttu-id="44a83-102">Pomelo EF Core provider di database per MySQL</span><span class="sxs-lookup"><span data-stu-id="44a83-102">Pomelo EF Core Database Provider for MySQL</span></span>

<span data-ttu-id="44a83-103">Questo provider di database consente l'uso di Entity Framework Core (EF Core) con MySQL.</span><span class="sxs-lookup"><span data-stu-id="44a83-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="44a83-104">Il provider viene gestito nel quadro del [progetto Pomelo Foundation](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span><span class="sxs-lookup"><span data-stu-id="44a83-104">The provider is maintained as part of the [Pomelo Foundation Project](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="44a83-105">Il provider non viene gestito nell'ambito del progetto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="44a83-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="44a83-106">Quando si prende in considerazione un provider di terze parti, valutarne con cura gli aspetti relativi a qualità, licenze, supporto e così via, per essere certi che soddisfi i requisiti correnti.</span><span class="sxs-lookup"><span data-stu-id="44a83-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="44a83-107">Installazione di</span><span class="sxs-lookup"><span data-stu-id="44a83-107">Install</span></span>

<span data-ttu-id="44a83-108">Installare il [pacchetto NuGet Pomelo.MySqlEntityFrameworkCore](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span><span class="sxs-lookup"><span data-stu-id="44a83-108">Install the [Pomelo.EntityFrameworkCore.MySql NuGet package](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span></span>

``` powershell
Install-Package Pomelo.EntityFrameworkCore.MySql
```

## <a name="get-started"></a><span data-ttu-id="44a83-109">Introduzione</span><span class="sxs-lookup"><span data-stu-id="44a83-109">Get Started</span></span>

<span data-ttu-id="44a83-110">Per acquisire familiarità con il provider, usare le risorse seguenti.</span><span class="sxs-lookup"><span data-stu-id="44a83-110">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="44a83-111">Documentazione di introduzione</span><span class="sxs-lookup"><span data-stu-id="44a83-111">Getting started documentation</span></span>](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md#getting-started)

* [<span data-ttu-id="44a83-112">Applicazione di esempio Yuuko Blog</span><span class="sxs-lookup"><span data-stu-id="44a83-112">Yuuko Blog sample application</span></span>](https://github.com/PomeloFoundation/YuukoBlog)

## <a name="supported-database-engines"></a><span data-ttu-id="44a83-113">Motori di database supportati</span><span class="sxs-lookup"><span data-stu-id="44a83-113">Supported Database Engines</span></span>

* <span data-ttu-id="44a83-114">MySQL</span><span class="sxs-lookup"><span data-stu-id="44a83-114">MySQL</span></span>
* <span data-ttu-id="44a83-115">MariaDB</span><span class="sxs-lookup"><span data-stu-id="44a83-115">MariaDB</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="44a83-116">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="44a83-116">Supported Platforms</span></span>

* <span data-ttu-id="44a83-117">.NET Framework (4.5.1 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="44a83-117">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="44a83-118">.NET Core</span><span class="sxs-lookup"><span data-stu-id="44a83-118">.NET Core</span></span>

* <span data-ttu-id="44a83-119">Mono (4.2.0 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="44a83-119">Mono (4.2.0 onwards)</span></span>
