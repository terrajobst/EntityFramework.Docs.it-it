---
title: Supporto del provider per i tipi spaziali-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 863f1b4551bd62160915eba90fee7ba6c49c169c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416339"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="b6256-102">Supporto del provider per i tipi spaziali</span><span class="sxs-lookup"><span data-stu-id="b6256-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="b6256-103">Entity Framework supporta l'utilizzo di dati spaziali tramite le classi DbGeography o DbGeometry.</span><span class="sxs-lookup"><span data-stu-id="b6256-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="b6256-104">Queste classi si basano sulle funzionalità specifiche del database offerte dal provider Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b6256-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="b6256-105">Non tutti i provider supportano i dati spaziali e quelli che possono avere prerequisiti aggiuntivi, ad esempio l'installazione di assembly di tipo spaziale.</span><span class="sxs-lookup"><span data-stu-id="b6256-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="b6256-106">Altre informazioni sul supporto dei provider per i tipi spaziali sono disponibili di seguito.</span><span class="sxs-lookup"><span data-stu-id="b6256-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="b6256-107">Altre informazioni su come usare i tipi spaziali in un'applicazione sono disponibili in due procedure dettagliate, una per Code First, l'altra per Database First o Model First:</span><span class="sxs-lookup"><span data-stu-id="b6256-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="b6256-108">Tipi di dati spaziali in Code First</span><span class="sxs-lookup"><span data-stu-id="b6256-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="b6256-109">Tipi di dati spaziali in EF designer</span><span class="sxs-lookup"><span data-stu-id="b6256-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="b6256-110">Versioni EF che supportano i tipi spaziali</span><span class="sxs-lookup"><span data-stu-id="b6256-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="b6256-111">Il supporto per i tipi spaziali è stato introdotto in EF5.</span><span class="sxs-lookup"><span data-stu-id="b6256-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="b6256-112">Tuttavia, nei tipi spaziali EF5 sono supportati solo quando l'applicazione è destinata ed è in esecuzione in .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="b6256-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="b6256-113">A partire da i tipi spaziali EF6 sono supportati per le applicazioni destinate sia a .NET 4 che a .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="b6256-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="b6256-114">Provider EF che supportano i tipi spaziali</span><span class="sxs-lookup"><span data-stu-id="b6256-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="b6256-115">EF5</span><span class="sxs-lookup"><span data-stu-id="b6256-115">EF5</span></span>  

<span data-ttu-id="b6256-116">I provider Entity Framework per EF5 che sono in grado di supportare i tipi spaziali sono:</span><span class="sxs-lookup"><span data-stu-id="b6256-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="b6256-117">Provider di Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="b6256-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="b6256-118">Questo provider viene fornito come parte di EF5.</span><span class="sxs-lookup"><span data-stu-id="b6256-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="b6256-119">Questo provider dipende da alcune librerie di basso livello aggiuntive che potrebbero dover essere installate. per informazioni dettagliate, vedere di seguito.</span><span class="sxs-lookup"><span data-stu-id="b6256-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="b6256-120">Devart dotConnect per Oracle</span><span class="sxs-lookup"><span data-stu-id="b6256-120">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="b6256-121">Si tratta di un provider di terze parti di Devart.</span><span class="sxs-lookup"><span data-stu-id="b6256-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="b6256-122">Se si è a conoscenza di un provider EF5 che supporta i tipi spaziali, contattare il contatto e sarà possibile aggiungerlo a questo elenco.</span><span class="sxs-lookup"><span data-stu-id="b6256-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="b6256-123">EF6</span><span class="sxs-lookup"><span data-stu-id="b6256-123">EF6</span></span>  

<span data-ttu-id="b6256-124">I provider Entity Framework per EF6 che sono in grado di supportare i tipi spaziali sono:</span><span class="sxs-lookup"><span data-stu-id="b6256-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="b6256-125">Provider di Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="b6256-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="b6256-126">Questo provider viene fornito come parte di EF6.</span><span class="sxs-lookup"><span data-stu-id="b6256-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="b6256-127">Questo provider dipende da alcune librerie di basso livello aggiuntive che potrebbero dover essere installate. per informazioni dettagliate, vedere di seguito.</span><span class="sxs-lookup"><span data-stu-id="b6256-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="b6256-128">Devart dotConnect per Oracle</span><span class="sxs-lookup"><span data-stu-id="b6256-128">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="b6256-129">Si tratta di un provider di terze parti di Devart.</span><span class="sxs-lookup"><span data-stu-id="b6256-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="b6256-130">Se si è a conoscenza di un provider EF6 che supporta i tipi spaziali, contattare il contatto e sarà possibile aggiungerlo a questo elenco.</span><span class="sxs-lookup"><span data-stu-id="b6256-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="b6256-131">Prerequisiti per i tipi spaziali con Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="b6256-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="b6256-132">SQL Server supporto spaziale dipende dai tipi SqlGeography e SqlGeometry di basso livello SQL Server specifici.</span><span class="sxs-lookup"><span data-stu-id="b6256-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="b6256-133">Questi tipi risiedono nell'assembly Microsoft. SqlServer. Types. dll e questo assembly non viene fornito come parte di EF o come parte del .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b6256-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="b6256-134">Quando si installa Visual Studio, viene spesso installata anche una versione di SQL Server, che includerà l'installazione di Microsoft. SqlServer. Types. dll.</span><span class="sxs-lookup"><span data-stu-id="b6256-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="b6256-135">Se SQL Server non è installato nel computer in cui si desidera utilizzare tipi spaziali o se i tipi spaziali sono stati esclusi dall'installazione SQL Server, sarà necessario installarli manualmente.</span><span class="sxs-lookup"><span data-stu-id="b6256-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="b6256-136">I tipi possono essere installati utilizzando `SQLSysClrTypes.msi`, che fa parte di Microsoft SQL Server Feature Pack.</span><span class="sxs-lookup"><span data-stu-id="b6256-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="b6256-137">I tipi spaziali sono SQL Server specifici della versione, quindi è consigliabile [cercare "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) nell'area download Microsoft, quindi selezionare e scaricare l'opzione che corrisponde alla versione di SQL Server che verrà usata.</span><span class="sxs-lookup"><span data-stu-id="b6256-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
