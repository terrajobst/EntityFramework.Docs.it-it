---
title: Provider di database Npgsql per PostgreSQL - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a><span data-ttu-id="c9a4b-102">Provider di database Npgsql EF Core per PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="c9a4b-102">Npgsql EF Core Database Provider for PostgreSQL</span></span>

<span data-ttu-id="c9a4b-103">Questo provider di database consente l'uso di Entity Framework Core con PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="c9a4b-103">This database provider allows Entity Framework Core to be used with PostgreSQL.</span></span> <span data-ttu-id="c9a4b-104">Il provider viene gestito nel quadro del [progetto Npgsql](http://www.npgsql.org).</span><span class="sxs-lookup"><span data-stu-id="c9a4b-104">The provider is maintained as part of the [Npgsql project](http://www.npgsql.org).</span></span>

> [!NOTE]  
> <span data-ttu-id="c9a4b-105">Il provider non viene gestito nell'ambito del progetto Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c9a4b-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="c9a4b-106">Quando si prende in considerazione un provider di terze parti, valutarne con cura gli aspetti relativi a qualità, licenze, supporto e così via, per essere certi che soddisfi i requisiti correnti.</span><span class="sxs-lookup"><span data-stu-id="c9a4b-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="c9a4b-107">Installazione di</span><span class="sxs-lookup"><span data-stu-id="c9a4b-107">Install</span></span>

<span data-ttu-id="c9a4b-108">Installare il [pacchetto NuGet Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span><span class="sxs-lookup"><span data-stu-id="c9a4b-108">Install the [Npgsql.EntityFrameworkCore.PostgreSQL NuGet package](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span></span>

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a><span data-ttu-id="c9a4b-109">Introduzione</span><span class="sxs-lookup"><span data-stu-id="c9a4b-109">Get Started</span></span>

<span data-ttu-id="c9a4b-110">Vedere la [documentazione di Npgsql](http://www.npgsql.org/efcore/index.html) per iniziare.</span><span class="sxs-lookup"><span data-stu-id="c9a4b-110">See the [Npgsql documentation](http://www.npgsql.org/efcore/index.html) to get started.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="c9a4b-111">Motori di database supportati</span><span class="sxs-lookup"><span data-stu-id="c9a4b-111">Supported Database Engines</span></span>

* <span data-ttu-id="c9a4b-112">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="c9a4b-112">PostgreSQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="c9a4b-113">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="c9a4b-113">Supported Platforms</span></span>

* <span data-ttu-id="c9a4b-114">.NET Framework (4.5.1 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="c9a4b-114">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="c9a4b-115">.NET Core</span><span class="sxs-lookup"><span data-stu-id="c9a4b-115">.NET Core</span></span>

* <span data-ttu-id="c9a4b-116">Mono (4.2.0 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="c9a4b-116">Mono (4.2.0 onwards)</span></span>
