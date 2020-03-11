---
title: Provider di Azure Cosmos DB-utilizzo di dati non strutturati-EF Core
description: Come usare Azure Cosmos DB dati non strutturati con Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 69f979d46174ff56310b334f28438ac271f45155
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417192"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a>Utilizzo di dati non strutturati nel provider EF Core Azure Cosmos DB

EF Core è stato progettato per semplificare l'utilizzo dei dati che seguono uno schema definito nel modello. Tuttavia uno dei punti di forza di Azure Cosmos DB è la flessibilità nella forma dei dati archiviati.

## <a name="accessing-the-raw-json"></a>Accesso al file JSON non elaborato

È possibile accedere alle proprietà che non vengono rilevate da EF Core tramite una proprietà speciale nello [stato di ombreggiatura](../../modeling/shadow-properties.md) denominato `"__jObject"` contenente una `JObject` che rappresenta i dati ricevuti dall'archivio e i dati che verranno archiviati:

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
> La proprietà `"__jObject"` fa parte dell'infrastruttura EF Core e deve essere usata solo come ultima risorsa, poiché è probabile che abbia un comportamento diverso nelle versioni future.

> [!NOTE]
> Le modifiche apportate all'entità sostituiranno i valori archiviati in `"__jObject"` durante `SaveChanges`.

## <a name="using-cosmosclient"></a>Uso di CosmosClient

Per separare completamente da EF Core ottenere l'oggetto [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) che fa [parte di Azure Cosmos DB SDK](/azure/cosmos-db/sql-api-get-started) da `DbContext`:

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a>Valori di proprietà mancanti

Nell'esempio precedente è stata rimossa la proprietà `"TrackingNumber"` dall'ordine. A causa del funzionamento dell'indicizzazione in Cosmos DB, le query che fanno riferimento alla proprietà mancante altrove rispetto alla proiezione potrebbero restituire risultati imprevisti. Ad esempio:

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

La query ordinata non restituisce effettivamente alcun risultato. Ciò significa che è necessario prestare attenzione a popolare sempre le proprietà mappate da EF Core quando si utilizza direttamente l'archivio.

> [!NOTE]
> Questo comportamento potrebbe cambiare nelle versioni future di Cosmos. Attualmente, ad esempio, se i criteri di indicizzazione definiscono l'indice composito {ID/? ASC, TrackingNumber/? ASC)}, una query con "ORDER BY c.Id ASC, c. Discriminator ASC __" restituisce gli__ elementi in cui manca la proprietà `"TrackingNumber"`.
