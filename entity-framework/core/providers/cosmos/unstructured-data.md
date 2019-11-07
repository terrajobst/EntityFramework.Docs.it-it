---
title: Provider di Azure Cosmos DB-utilizzo di dati non strutturati-EF Core
description: Come usare Azure Cosmos DB dati non strutturati con Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 0bfccbfd3af6e209967004752b5a3947d644544b
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655521"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="3c864-103">Utilizzo di dati non strutturati nel provider EF Core Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3c864-103">Working with Unstructured Data in EF Core Azure Cosmos DB Provider</span></span>

<span data-ttu-id="3c864-104">EF Core è stato progettato per semplificare l'utilizzo dei dati che seguono uno schema definito nel modello.</span><span class="sxs-lookup"><span data-stu-id="3c864-104">EF Core was designed to make it easy to work with data that follows a schema defined in the model.</span></span> <span data-ttu-id="3c864-105">Tuttavia uno dei punti di forza di Azure Cosmos DB è la flessibilità nella forma dei dati archiviati.</span><span class="sxs-lookup"><span data-stu-id="3c864-105">However one of the strengths of Azure Cosmos DB is the flexibility in the shape of the data stored.</span></span>

## <a name="accessing-the-raw-json"></a><span data-ttu-id="3c864-106">Accesso al file JSON non elaborato</span><span class="sxs-lookup"><span data-stu-id="3c864-106">Accessing the raw JSON</span></span>

<span data-ttu-id="3c864-107">È possibile accedere alle proprietà che non vengono rilevate da EF Core tramite una proprietà speciale nello [stato di ombreggiatura](../../modeling/shadow-properties.md) denominato `"__jObject"` contenente una `JObject` che rappresenta i dati ricevuti dall'archivio e i dati che verranno archiviati:</span><span class="sxs-lookup"><span data-stu-id="3c864-107">It is possible to access the properties that are not tracked by EF Core through a special property in [shadow-state](../../modeling/shadow-properties.md) named `"__jObject"` that contains a `JObject` representing the data recieved from the store and data that will be stored:</span></span>

[!code-csharp[Unmapped](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=23,24&name=Unmapped)]

``` json
{
    "Id": 1,
    "PartitionKey": "1",
    "TrackingNumber": null,
    "id": "1",
    "Address": {
        "ShipsToCity": "London",
        "ShipsToStreet": "221 B Baker St"
    },
    "_rid": "eLMaAK8TzkIBAAAAAAAAAA==",
    "_self": "dbs/eLMaAA==/colls/eLMaAK8TzkI=/docs/eLMaAK8TzkIBAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683e-0a12bf8d01d5\"",
    "_attachments": "attachments/",
    "BillingAddress": "Clarence House",
    "_ts": 1568164374
}
```

> [!WARNING]
> <span data-ttu-id="3c864-108">La proprietà `"__jObject"` fa parte dell'infrastruttura EF Core e deve essere usata solo come ultima risorsa, poiché è probabile che abbia un comportamento diverso nelle versioni future.</span><span class="sxs-lookup"><span data-stu-id="3c864-108">The `"__jObject"` property is part of the EF Core infrastructure and should only be used as a last resort as it is likely to have different behavior in future releases.</span></span>

> [!NOTE]
> <span data-ttu-id="3c864-109">Le modifiche apportate all'entità sostituiranno i valori archiviati in `"__jObject"` durante `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="3c864-109">Changes to the entity will override the values stored in `"__jObject"` during `SaveChanges`.</span></span>

## <a name="using-cosmosclient"></a><span data-ttu-id="3c864-110">Uso di CosmosClient</span><span class="sxs-lookup"><span data-stu-id="3c864-110">Using CosmosClient</span></span>

<span data-ttu-id="3c864-111">Per separare completamente da EF Core ottenere l'oggetto [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) che fa [parte di Azure Cosmos DB SDK](/azure/cosmos-db/sql-api-get-started) da `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="3c864-111">To decouple completely from EF Core get the [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) object that is [part of the Azure Cosmos DB SDK](/azure/cosmos-db/sql-api-get-started) from `DbContext`:</span></span>

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a><span data-ttu-id="3c864-112">Valori di proprietà mancanti</span><span class="sxs-lookup"><span data-stu-id="3c864-112">Missing property values</span></span>

<span data-ttu-id="3c864-113">Nell'esempio precedente è stata rimossa la proprietà `"TrackingNumber"` dall'ordine.</span><span class="sxs-lookup"><span data-stu-id="3c864-113">In the previous example we removed the `"TrackingNumber"` property from the order.</span></span> <span data-ttu-id="3c864-114">A causa del funzionamento dell'indicizzazione in Cosmos DB, le query che fanno riferimento alla proprietà mancante altrove rispetto alla proiezione potrebbero restituire risultati imprevisti.</span><span class="sxs-lookup"><span data-stu-id="3c864-114">Because of how indexing works in Cosmos DB, queries that reference the missing property somewhere else than in the projection could return unexpected results.</span></span> <span data-ttu-id="3c864-115">Esempio:</span><span class="sxs-lookup"><span data-stu-id="3c864-115">For example:</span></span>

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

<span data-ttu-id="3c864-116">La query ordinata non restituisce effettivamente alcun risultato.</span><span class="sxs-lookup"><span data-stu-id="3c864-116">The sorted query actually returns no results.</span></span> <span data-ttu-id="3c864-117">Ciò significa che è necessario prestare attenzione a popolare sempre le proprietà mappate da EF Core quando si utilizza direttamente l'archivio.</span><span class="sxs-lookup"><span data-stu-id="3c864-117">This means that one should take care to always populate properties mapped by EF Core when working with the store directly.</span></span>

> [!NOTE]
> <span data-ttu-id="3c864-118">Questo comportamento potrebbe cambiare nelle versioni future di Cosmos.</span><span class="sxs-lookup"><span data-stu-id="3c864-118">This behavior might change in future versions of Cosmos.</span></span> <span data-ttu-id="3c864-119">Attualmente, ad esempio, se i criteri di indicizzazione definiscono l'indice composito {ID/?</span><span class="sxs-lookup"><span data-stu-id="3c864-119">For instance, currently if the indexing policy defines the composite index {Id/?</span></span> <span data-ttu-id="3c864-120">ASC, TrackingNumber/?</span><span class="sxs-lookup"><span data-stu-id="3c864-120">ASC, TrackingNumber/?</span></span> <span data-ttu-id="3c864-121">ASC)}, una query che ha ' ORDER BY c.Id ASC, c. Discriminator ASC __' restituisce gli__ elementi che non hanno la proprietà `"TrackingNumber"`.</span><span class="sxs-lookup"><span data-stu-id="3c864-121">ASC)}, then a query the has 'ORDER BY c.Id ASC, c.Discriminator ASC' __would__ return items that are missing the `"TrackingNumber"` property.</span></span>
