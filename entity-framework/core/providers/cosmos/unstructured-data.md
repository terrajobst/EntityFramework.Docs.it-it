---
title: Provider di Azure Cosmos DB-utilizzo di dati non strutturati-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: b47d41b6-984f-419a-ab10-2ed3b95e3919
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 86bb0f7915c8a2561e7d5cd5dffc27474218a112
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150816"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="1a232-102">Utilizzo di dati non strutturati nel provider EF Core Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="1a232-102">Working with Unstructured Data in EF Core Azure Cosmos DB Provider</span></span>

<span data-ttu-id="1a232-103">EF Core è stato progettato per semplificare l'utilizzo dei dati che seguono uno schema definito nel modello.</span><span class="sxs-lookup"><span data-stu-id="1a232-103">EF Core was designed to make it easy to work with data that follows a schema defined in the model.</span></span> <span data-ttu-id="1a232-104">Tuttavia uno dei punti di forza di Azure Cosmos DB è la flessibilità nella forma dei dati archiviati.</span><span class="sxs-lookup"><span data-stu-id="1a232-104">However one of the strengths of Azure Cosmos DB is the flexibility in the shape of the data stored.</span></span>

## <a name="accessing-the-raw-json"></a><span data-ttu-id="1a232-105">Accesso al file JSON non elaborato</span><span class="sxs-lookup"><span data-stu-id="1a232-105">Accessing the raw JSON</span></span>

<span data-ttu-id="1a232-106">È possibile accedere alle proprietà che non vengono rilevate da EF Core tramite una proprietà speciale nello [stato Shadow](../../modeling/shadow-properties.md) denominato `"__jObject"` contenente un oggetto `JObject` che rappresenta i dati ricevuti dall'archivio e i dati che verranno archiviati:</span><span class="sxs-lookup"><span data-stu-id="1a232-106">It is possible to access the properties that are not tracked by EF Core through a special property in [shadow-state](../../modeling/shadow-properties.md) named `"__jObject"` that contains a `JObject` representing the data recieved from the store and data that will be stored:</span></span>

[!code-csharp[Unmapped](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=21-23&name=Unmapped)]

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
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
> <span data-ttu-id="1a232-107">La `"__jObject"` proprietà fa parte dell'infrastruttura di EF core e deve essere usata solo come ultima risorsa, perché probabilmente avrà un comportamento diverso nelle versioni future.</span><span class="sxs-lookup"><span data-stu-id="1a232-107">The `"__jObject"` property is part of the EF Core infrastructure and should only be used as a last resort as it is likely to have different behavior in future releases.</span></span>

> [!NOTE]
> <span data-ttu-id="1a232-108">Le modifiche apportate all'entità sostituiranno i `"__jObject"` valori `SaveChanges`archiviati in durante.</span><span class="sxs-lookup"><span data-stu-id="1a232-108">Changes to the entity will override the values stored in `"__jObject"` during `SaveChanges`.</span></span>

## <a name="using-cosmosclient"></a><span data-ttu-id="1a232-109">Uso di CosmosClient</span><span class="sxs-lookup"><span data-stu-id="1a232-109">Using CosmosClient</span></span>

<span data-ttu-id="1a232-110">Per separare completamente da EF Core ottenere l' `CosmosClient` oggetto che fa [parte dell'SDK Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) da: `DbContext`</span><span class="sxs-lookup"><span data-stu-id="1a232-110">To decouple completely from EF Core get the `CosmosClient` object that is [part of the Azure Cosmos DB SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) from `DbContext`:</span></span>

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a><span data-ttu-id="1a232-111">Valori di proprietà mancanti</span><span class="sxs-lookup"><span data-stu-id="1a232-111">Missing property values</span></span>

<span data-ttu-id="1a232-112">Nell'esempio precedente la `"TrackingNumber"` proprietà è stata rimossa dall'ordine.</span><span class="sxs-lookup"><span data-stu-id="1a232-112">In the previous example we removed the `"TrackingNumber"` property from the order.</span></span> <span data-ttu-id="1a232-113">A causa del funzionamento dell'indicizzazione in Cosmos DB, le query che fanno riferimento alla proprietà mancante altrove rispetto alla proiezione potrebbero restituire risultati imprevisti.</span><span class="sxs-lookup"><span data-stu-id="1a232-113">Because of how indexing works in Cosmos DB, queries that reference the missing property somewhere else than in the projection could return unexpected results.</span></span> <span data-ttu-id="1a232-114">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="1a232-114">For example:</span></span>

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

<span data-ttu-id="1a232-115">La query ordinata non restituisce effettivamente alcun risultato.</span><span class="sxs-lookup"><span data-stu-id="1a232-115">The sorted query actually returns no results.</span></span> <span data-ttu-id="1a232-116">Ciò significa che è necessario prestare attenzione a popolare sempre le proprietà mappate da EF Core quando si utilizza direttamente l'archivio.</span><span class="sxs-lookup"><span data-stu-id="1a232-116">This means that one should take care to always populate properties mapped by EF Core when working with the store directly.</span></span>

> [!NOTE]
> <span data-ttu-id="1a232-117">Questo comportamento potrebbe cambiare nelle versioni future di Cosmos.</span><span class="sxs-lookup"><span data-stu-id="1a232-117">This behavior might change in future versions of Cosmos.</span></span> <span data-ttu-id="1a232-118">Attualmente, ad esempio, se i criteri di indicizzazione definiscono l'indice composito {ID/?</span><span class="sxs-lookup"><span data-stu-id="1a232-118">For instance, currently if the indexing policy defines the composite index {Id/?</span></span> <span data-ttu-id="1a232-119">ASC, TrackingNumber/?</span><span class="sxs-lookup"><span data-stu-id="1a232-119">ASC, TrackingNumber/?</span></span> <span data-ttu-id="1a232-120">ASC)}, una query con ' order by c.ID ASC, c. Discriminator ASC __' restituisce gli__ elementi che non dispongono della `"TrackingNumber"` proprietà.</span><span class="sxs-lookup"><span data-stu-id="1a232-120">ASC)}, then a query the has 'ORDER BY c.Id ASC, c.Discriminator ASC' __would__ return items that are missing the `"TrackingNumber"` property.</span></span>