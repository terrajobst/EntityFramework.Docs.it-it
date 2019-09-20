---
title: Provider di Azure Cosmos DB-limitazioni-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 9d02a2cd-484e-4687-b8a8-3748ba46dbc9
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 8dcc82a68c89e21ad1902a0bbbce8ebbc3535801
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150809"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a><span data-ttu-id="f19fb-102">Limitazioni del provider di Azure Cosmos DB EF Core</span><span class="sxs-lookup"><span data-stu-id="f19fb-102">EF Core Azure Cosmos DB Provider Limitations</span></span>

<span data-ttu-id="f19fb-103">Il provider Cosmos presenta alcune limitazioni.</span><span class="sxs-lookup"><span data-stu-id="f19fb-103">The Cosmos provider has a number of limitations.</span></span> <span data-ttu-id="f19fb-104">Molte di queste limitazioni sono dovute a limitazioni nel motore di database di Cosmos sottostante e non sono specifiche di EF.</span><span class="sxs-lookup"><span data-stu-id="f19fb-104">Many of these limitations are a result of limitations in the underlying Cosmos database engine and are not specific to EF.</span></span> <span data-ttu-id="f19fb-105">Ma la cosa più semplice [non è stata ancora implementata](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="f19fb-105">But most simply [haven't been implemented yet](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span></span>

## <a name="temporary-limitations"></a><span data-ttu-id="f19fb-106">Limitazioni temporanee</span><span class="sxs-lookup"><span data-stu-id="f19fb-106">Temporary limitations</span></span>

- <span data-ttu-id="f19fb-107">Anche se è presente un solo tipo di entità senza ereditarietà mappata a un contenitore, è ancora presente una proprietà Discriminator.</span><span class="sxs-lookup"><span data-stu-id="f19fb-107">Even if there is only one entity type without inheritance mapped to a container it still has a discriminator property.</span></span>
- <span data-ttu-id="f19fb-108">I tipi di entità con chiavi di partizione non funzionano correttamente in alcuni scenari</span><span class="sxs-lookup"><span data-stu-id="f19fb-108">Entity types with partition keys don't work correctly in some scenarios</span></span>
- <span data-ttu-id="f19fb-109">`Include`le chiamate non sono supportate</span><span class="sxs-lookup"><span data-stu-id="f19fb-109">`Include` calls are not supported</span></span>
- <span data-ttu-id="f19fb-110">`Join`le chiamate non sono supportate</span><span class="sxs-lookup"><span data-stu-id="f19fb-110">`Join` calls are not supported</span></span>

## <a name="azure-cosmos-db-sdk-limitations"></a><span data-ttu-id="f19fb-111">Limitazioni di Azure Cosmos DB SDK</span><span class="sxs-lookup"><span data-stu-id="f19fb-111">Azure Cosmos DB SDK limitations</span></span>

- <span data-ttu-id="f19fb-112">Sono disponibili solo metodi asincroni</span><span class="sxs-lookup"><span data-stu-id="f19fb-112">Only async methods are provided</span></span>

> [!WARNING]
> <span data-ttu-id="f19fb-113">Poiché non sono presenti versioni di sincronizzazione dei metodi di basso livello EF core si basa su, la funzionalità corrispondente è attualmente implementata `.Wait()` chiamando sull'oggetto `Task`restituito.</span><span class="sxs-lookup"><span data-stu-id="f19fb-113">Since there are no sync versions of the low level methods EF Core relies on, the corresponding functionality is currently implemented by calling `.Wait()` on the returned `Task`.</span></span> <span data-ttu-id="f19fb-114">Ciò significa che l'uso di `SaveChanges`metodi come `ToList` o anziché le relative controparti asincrone potrebbe causare un deadlock nell'applicazione</span><span class="sxs-lookup"><span data-stu-id="f19fb-114">This means that using methods like `SaveChanges`, or `ToList` instead of their async counterparts could lead to a deadlock in your application</span></span>

## <a name="azure-cosmos-db-limitations"></a><span data-ttu-id="f19fb-115">Limitazioni Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="f19fb-115">Azure Cosmos DB limitations</span></span>

<span data-ttu-id="f19fb-116">È possibile visualizzare la panoramica completa delle [funzionalità supportate da Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data), che rappresentano le differenze più importanti rispetto a un database relazionale:</span><span class="sxs-lookup"><span data-stu-id="f19fb-116">You can see the full overview of [Azure Cosmos DB supported features](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data), these are the most notable differences compared to a relational database:</span></span>

- <span data-ttu-id="f19fb-117">Le transazioni avviate dal client non sono supportate</span><span class="sxs-lookup"><span data-stu-id="f19fb-117">Client-initiated transactions are not supported</span></span>
- <span data-ttu-id="f19fb-118">Alcune query tra partizioni non sono supportate o molto più lente a seconda degli operatori interessati</span><span class="sxs-lookup"><span data-stu-id="f19fb-118">Some cross-partition queries are either not supported or much slower depending on the operators involved</span></span>