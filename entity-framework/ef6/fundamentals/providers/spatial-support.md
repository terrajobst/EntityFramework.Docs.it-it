---
title: Supporto del provider per i tipi spaziali - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 07eeecb5f5e3e3eab8548c4c7c0ed55c5ffb4f31
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998287"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="e5699-102">Supporto del provider per i tipi spaziali</span><span class="sxs-lookup"><span data-stu-id="e5699-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="e5699-103">Entity Framework supporta l'utilizzo di dati spaziali tramite le classi di DbGeography o DbGeometry.</span><span class="sxs-lookup"><span data-stu-id="e5699-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="e5699-104">Queste classi si basano su funzionalità specifiche del database offerte dal provider di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e5699-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="e5699-105">Non tutti i provider supportano i dati spaziali e quelli che si possono avere i prerequisiti aggiuntivi, ad esempio l'installazione degli assembly di tipo spaziale.</span><span class="sxs-lookup"><span data-stu-id="e5699-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="e5699-106">Altre informazioni sul supporto del provider per i tipi spaziali sono disponibili sotto.</span><span class="sxs-lookup"><span data-stu-id="e5699-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="e5699-107">Sono disponibili informazioni aggiuntive su come usare i tipi spaziali in un'applicazione in due procedure dettagliate, uno per Code First, l'altra per i Database First o Model First:</span><span class="sxs-lookup"><span data-stu-id="e5699-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="e5699-108">Tipi di dati spaziali nel codice prima di tutto</span><span class="sxs-lookup"><span data-stu-id="e5699-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="e5699-109">Tipi di dati spaziali nella finestra di progettazione di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e5699-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="e5699-110">Versioni di Entity Framework che supportano i tipi spaziali</span><span class="sxs-lookup"><span data-stu-id="e5699-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="e5699-111">Supporto per i tipi spaziali è stato introdotto in EF5.</span><span class="sxs-lookup"><span data-stu-id="e5699-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="e5699-112">Tuttavia, in EF5 tipi spaziali sono supportati solo quando l'applicazione ha come destinazione e viene eseguita in .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="e5699-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="e5699-113">A partire da Entity Framework 6 tipi di dati spaziali sono supportati per le applicazioni destinate a .NET 4 sia .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="e5699-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="e5699-114">Provider di Entity Framework che supportano i tipi spaziali</span><span class="sxs-lookup"><span data-stu-id="e5699-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="e5699-115">EF5</span><span class="sxs-lookup"><span data-stu-id="e5699-115">EF5</span></span>  

<span data-ttu-id="e5699-116">I provider di Entity Framework per EF5 che siamo a conoscenza di che supportano i tipi spaziali sono:</span><span class="sxs-lookup"><span data-stu-id="e5699-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="e5699-117">Provider di Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="e5699-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="e5699-118">Questo provider viene fornito come parte di EF5.</span><span class="sxs-lookup"><span data-stu-id="e5699-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="e5699-119">Questo provider dipende da alcune librerie aggiuntive di basso livello che potrebbero dover essere installata, ovvero per i dettagli vedere di seguito.</span><span class="sxs-lookup"><span data-stu-id="e5699-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="e5699-120">Devart dotConnect per Oracle</span><span class="sxs-lookup"><span data-stu-id="e5699-120">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="e5699-121">Si tratta di un provider di terze parti da Devart.</span><span class="sxs-lookup"><span data-stu-id="e5699-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="e5699-122">Se si conosce di provider che supporta i tipi spaziali, quindi EF5 contattarci e saremo lieti di aggiungerlo a questo elenco.</span><span class="sxs-lookup"><span data-stu-id="e5699-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="e5699-123">EF6</span><span class="sxs-lookup"><span data-stu-id="e5699-123">EF6</span></span>  

<span data-ttu-id="e5699-124">I provider di Entity Framework per Entity Framework 6 che siamo a conoscenza di che supportano i tipi spaziali sono:</span><span class="sxs-lookup"><span data-stu-id="e5699-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="e5699-125">Provider di Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="e5699-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="e5699-126">Questo provider viene fornito come parte di Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="e5699-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="e5699-127">Questo provider dipende da alcune librerie aggiuntive di basso livello che potrebbero dover essere installata, ovvero per i dettagli vedere di seguito.</span><span class="sxs-lookup"><span data-stu-id="e5699-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="e5699-128">Devart dotConnect per Oracle</span><span class="sxs-lookup"><span data-stu-id="e5699-128">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="e5699-129">Si tratta di un provider di terze parti da Devart.</span><span class="sxs-lookup"><span data-stu-id="e5699-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="e5699-130">Se si sa di un provider di Entity Framework 6 che supporta i tipi spaziali quindi, contatto e di Microsoft sarà lieta di aggiungerlo a questo elenco.</span><span class="sxs-lookup"><span data-stu-id="e5699-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="e5699-131">Prerequisiti per i tipi spaziali con Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="e5699-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="e5699-132">Supporto spaziale di SQL Server dipende dai tipi specifici di SQL Server a basso livello, SqlGeometry e SqlGeography.</span><span class="sxs-lookup"><span data-stu-id="e5699-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="e5699-133">In tempo reale di questi tipi in assembly Microsoft.SqlServer.Types.dll, e questo assembly non viene fornito come parte di Entity Framework o come parte di .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="e5699-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="e5699-134">Quando è installato Visual Studio viene installato spesso anche una versione di SQL Server e installazione del Microsoft.SqlServer.Types.dll includerà.</span><span class="sxs-lookup"><span data-stu-id="e5699-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="e5699-135">Se SQL Server non è installato nel computer in cui si desidera utilizzare i tipi spaziali o tipi di dati spaziali sono stati esclusi dall'installazione di SQL Server, sarà necessario installarli manualmente.</span><span class="sxs-lookup"><span data-stu-id="e5699-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="e5699-136">I tipi possono essere installati usando `SQLSysClrTypes.msi`, che fa parte del Feature Pack di Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e5699-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="e5699-137">Tipi di dati spaziali sono SQL Server specifiche della versione, pertanto si consiglia [cercare "SQL Server Feature Pack"](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) nel Microsoft Download Center, quindi selezionare e scaricare l'opzione che corrisponde alla versione di SQL Server che verrà utilizzato.</span><span class="sxs-lookup"><span data-stu-id="e5699-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
