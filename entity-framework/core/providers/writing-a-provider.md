---
title: Scrittura di un Provider di Database - Core a Entity Framework
author: anmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
ms.technology: entity-framework-core
uid: core/providers/writing-a-provider
ms.openlocfilehash: 9d6d3748a9097b3b8eeee2a8a516c53f3b2afa58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="4925d-102">Scrittura di un Provider di Database</span><span class="sxs-lookup"><span data-stu-id="4925d-102">Writing a Database Provider</span></span>

<span data-ttu-id="4925d-103">Per informazioni sulla scrittura di un provider di database di Entity Framework Core, vedere [, pertanto si desidera scrivere un provider di Entity Framework Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) da [Arthur Vickers](https://github.com/ajcvickers).</span><span class="sxs-lookup"><span data-stu-id="4925d-103">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

<span data-ttu-id="4925d-104">La base di codice di Entity Framework Core open source e contiene diversi provider di database che può essere usato come riferimento.</span><span class="sxs-lookup"><span data-stu-id="4925d-104">The EF Core code base is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="4925d-105">È possibile trovare il codice sorgente in https://github.com/aspnet/EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="4925d-105">You can find the source code at https://github.com/aspnet/EntityFramework.</span></span>

## <a name="the-providers-beware-label"></a><span data-ttu-id="4925d-106">Il provider-beware etichetta</span><span class="sxs-lookup"><span data-stu-id="4925d-106">The providers-beware label</span></span>

<span data-ttu-id="4925d-107">Quando si inizia a lavorare su un provider, cercare il [ `providers-beware` ](https://github.com/aspnet/EntityFramework/labels/providers-beware) etichetta sui problemi di GitHub e richieste pull.</span><span class="sxs-lookup"><span data-stu-id="4925d-107">Once you begin work on a provider, watch for the [`providers-beware`](https://github.com/aspnet/EntityFramework/labels/providers-beware) label on our GitHub issues and pull requests.</span></span> <span data-ttu-id="4925d-108">Si può essere utilizzata per identificare le modifiche che potrebbero influire sulla writer di provider.</span><span class="sxs-lookup"><span data-stu-id="4925d-108">We use this label to identify changes that may impact provider writers.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="4925d-109">Suggerito di denominazione dei provider di terze parti</span><span class="sxs-lookup"><span data-stu-id="4925d-109">Suggested naming of third party providers</span></span>

<span data-ttu-id="4925d-110">È consigliabile utilizzare i seguenti nomi per i pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="4925d-110">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="4925d-111">Ciò è coerente con i nomi dei pacchetti recapitati dal team di EF Core.</span><span class="sxs-lookup"><span data-stu-id="4925d-111">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="4925d-112">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4925d-112">For example:</span></span>
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
