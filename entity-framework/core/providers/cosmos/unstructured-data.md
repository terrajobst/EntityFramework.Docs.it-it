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
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a>Utilizzo di dati non strutturati nel provider EF Core Azure Cosmos DB

EF Core è stato progettato per semplificare l'utilizzo dei dati che seguono uno schema definito nel modello. Tuttavia uno dei punti di forza di Azure Cosmos DB è la flessibilità nella forma dei dati archiviati.

## <a name="accessing-the-raw-json"></a>Accesso al file JSON non elaborato

È possibile accedere alle proprietà che non vengono rilevate da EF Core tramite una proprietà speciale nello [stato Shadow](../../modeling/shadow-properties.md) denominato `"__jObject"` contenente un oggetto `JObject` che rappresenta i dati ricevuti dall'archivio e i dati che verranno archiviati:

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
> La `"__jObject"` proprietà fa parte dell'infrastruttura di EF core e deve essere usata solo come ultima risorsa, perché probabilmente avrà un comportamento diverso nelle versioni future.

> [!NOTE]
> Le modifiche apportate all'entità sostituiranno i `"__jObject"` valori `SaveChanges`archiviati in durante.

## <a name="using-cosmosclient"></a>Uso di CosmosClient

Per separare completamente da EF Core ottenere l' `CosmosClient` oggetto che fa [parte dell'SDK Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) da: `DbContext`

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a>Valori di proprietà mancanti

Nell'esempio precedente la `"TrackingNumber"` proprietà è stata rimossa dall'ordine. A causa del funzionamento dell'indicizzazione in Cosmos DB, le query che fanno riferimento alla proprietà mancante altrove rispetto alla proiezione potrebbero restituire risultati imprevisti. Ad esempio:

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

La query ordinata non restituisce effettivamente alcun risultato. Ciò significa che è necessario prestare attenzione a popolare sempre le proprietà mappate da EF Core quando si utilizza direttamente l'archivio.

> [!NOTE]
> Questo comportamento potrebbe cambiare nelle versioni future di Cosmos. Attualmente, ad esempio, se i criteri di indicizzazione definiscono l'indice composito {ID/? ASC, TrackingNumber/? ASC)}, una query con ' order by c.ID ASC, c. Discriminator ASC __' restituisce gli__ elementi che non dispongono della `"TrackingNumber"` proprietà.