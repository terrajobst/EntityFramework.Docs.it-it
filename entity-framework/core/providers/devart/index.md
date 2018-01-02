---
title: Provider di database Devart - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: aad70a75-d04d-4d63-a489-7f9062a3c73d
ms.technology: entity-framework-core
uid: core/providers/devart/index
ms.openlocfilehash: 04de917b3327a6f544292781bca42a1170c199ad
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="devart-ef-core-database-providers"></a><span data-ttu-id="e2b6c-102">Provider di database EF Core Devart</span><span class="sxs-lookup"><span data-stu-id="e2b6c-102">Devart EF Core Database Providers</span></span>

<span data-ttu-id="e2b6c-103">Devart è un writer di provider di terze parti che offre provider di database per una vasta gamma di database.</span><span class="sxs-lookup"><span data-stu-id="e2b6c-103">Devart is a third party provider writer that offers database providers for a wide range of databases.</span></span> <span data-ttu-id="e2b6c-104">Per altre informazioni, vedere [devart.com/dotconnect](https://www.devart.com/dotconnect/).</span><span class="sxs-lookup"><span data-stu-id="e2b6c-104">Find out more at [devart.com/dotconnect](https://www.devart.com/dotconnect/).</span></span>

> [!NOTE]  
> <span data-ttu-id="e2b6c-105">Questo provider non viene mantenuto nell'ambito del progetto di Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="e2b6c-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="e2b6c-106">Quando si prende in considerazione un provider di terze parti, valutarne attentamente gli aspetti relativi a qualità, licenze, supporto, e così via per essere certi che le proprie esigenze vengano soddisfatte.</span><span class="sxs-lookup"><span data-stu-id="e2b6c-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="paid-versions-only"></a><span data-ttu-id="e2b6c-107">Solo versioni a pagamento</span><span class="sxs-lookup"><span data-stu-id="e2b6c-107">Paid Versions Only</span></span>

<span data-ttu-id="e2b6c-108">Devart dotConnect è un provider commerciale di terze parti.</span><span class="sxs-lookup"><span data-stu-id="e2b6c-108">Devart dotConnect is a commercial third party provider.</span></span> <span data-ttu-id="e2b6c-109">Il supporto di Entity Framework è disponibile solo nelle versioni a pagamento di dotConnect.</span><span class="sxs-lookup"><span data-stu-id="e2b6c-109">Entity Framework support is only available in paid versions of dotConnect.</span></span>

## <a name="install"></a><span data-ttu-id="e2b6c-110">Installazione di</span><span class="sxs-lookup"><span data-stu-id="e2b6c-110">Install</span></span>

<span data-ttu-id="e2b6c-111">Vedere la [documentazione di Devart dotConnect](https://www.devart.com/dotconnect/) per le istruzioni di installazione.</span><span class="sxs-lookup"><span data-stu-id="e2b6c-111">See the [Devart dotConnect documentation](https://www.devart.com/dotconnect/) for installation instructions.</span></span>

## <a name="get-started"></a><span data-ttu-id="e2b6c-112">Introduzione</span><span class="sxs-lookup"><span data-stu-id="e2b6c-112">Get Started</span></span>

<span data-ttu-id="e2b6c-113">Per iniziare, vedere la [documentazione di Entity Framework per Devart dotConnect](https://www.devart.com/dotconnect/entityframework.html) e l'[articolo del blog sul supporto di Entity Framework Core 1](http://blog.devart.com/entity-framework-core-1-entity-framework-7-support.html).</span><span class="sxs-lookup"><span data-stu-id="e2b6c-113">See the [Devart dotConnect Entity Framework documentation](https://www.devart.com/dotconnect/entityframework.html) and [blog article about Entity Framework Core 1 Support](http://blog.devart.com/entity-framework-core-1-entity-framework-7-support.html) to get started.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="e2b6c-114">Motori di database supportati</span><span class="sxs-lookup"><span data-stu-id="e2b6c-114">Supported Database Engines</span></span>

* <span data-ttu-id="e2b6c-115">MySQL</span><span class="sxs-lookup"><span data-stu-id="e2b6c-115">MySQL</span></span>

* <span data-ttu-id="e2b6c-116">Oracle</span><span class="sxs-lookup"><span data-stu-id="e2b6c-116">Oracle</span></span>

* <span data-ttu-id="e2b6c-117">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="e2b6c-117">PostgreSQL</span></span>

* <span data-ttu-id="e2b6c-118">SQLite</span><span class="sxs-lookup"><span data-stu-id="e2b6c-118">SQLite</span></span>

* <span data-ttu-id="e2b6c-119">DB2</span><span class="sxs-lookup"><span data-stu-id="e2b6c-119">DB2</span></span>

* [<span data-ttu-id="e2b6c-120">App cloud</span><span class="sxs-lookup"><span data-stu-id="e2b6c-120">Cloud apps</span></span>](https://www.devart.com/dotconnect/#cloud)

## <a name="supported-platforms"></a><span data-ttu-id="e2b6c-121">Piattaforme supportate</span><span class="sxs-lookup"><span data-stu-id="e2b6c-121">Supported Platforms</span></span>

* <span data-ttu-id="e2b6c-122">.NET Framework (a partire da 4.5.1)</span><span class="sxs-lookup"><span data-stu-id="e2b6c-122">.NET Framework (4.5.1 onwards)</span></span>
* <span data-ttu-id="e2b6c-123">.NET core 1.0 e versioni successive (provider per Oracle, MySQL, PostgreSQL, SQLite)</span><span class="sxs-lookup"><span data-stu-id="e2b6c-123">.NET Core 1.0 and higher (providers for Oracle, MySQL, PostgreSQL, SQLite)</span></span>
