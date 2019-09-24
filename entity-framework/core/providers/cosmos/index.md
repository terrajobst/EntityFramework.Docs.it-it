---
title: Provider di Azure Cosmos DB - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 28264681-4486-4891-888c-be5e4ade24f1
uid: core/providers/cosmos/index
ms.openlocfilehash: c753bb71089c91cbb26b970cddd118645fb18d56
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150728"
---
# <a name="ef-core-azure-cosmos-db-provider"></a>Provider di Azure Cosmos DB per EF Core

>[!NOTE]
> Questo provider è una novità di EF Core 3.0.

Questo provider di database consente l'uso di Entity Framework Core con Azure Cosmos DB. Il provider viene gestito nell'ambito del [progetto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

Prima di leggere questa sezione, è consigliabile acquisire familiarità con la [documentazione di Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction).

## <a name="install"></a>Installazione di

Installare il [pacchetto NuGet Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

## <a name="get-started"></a>Introduzione

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos) di questo articolo in GitHub.

Come per gli altri provider, il primo passaggio consiste nel chiamare `UseCosmos`: [!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> L'endpoint e la chiave sono hardcoded qui per semplicità, ma in un'app di produzione devono essere [archiviati in modo sicuro](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)

In questo esempio `Order` è un'entità semplice con un riferimento al [tipo i proprietà](../../modeling/owned-entities.md) `StreetAddress`.

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

Per il salvataggio e l'esecuzione di query sui dati si segue il modello EF normale: [!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> La chiamata di `EnsureCreated` è necessaria per creare le raccolte richieste e inserire i [dati di inizializzazione](../../modeling/data-seeding.md) se presenti nel modello. Tuttavia, `EnsureCreated` deve essere chiamato solo durante la distribuzione, e non durante il normale funzionamento, perché potrebbe causare problemi di prestazioni.

## <a name="cosmos-specific-model-customization"></a>Personalizzazione del modello specifico di Cosmos

Per impostazione predefinita, viene eseguito il mapping di tutti i tipi di entità allo stesso contenitore, denominato in base al contesto derivato (`"OrderContext"` in questo caso). Per modificare il nome del contenitore predefinito, usare `HasDefaultContainer`:

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

Per eseguire il mapping di un tipo di entità a un contenitore diverso, usare `ToContainer`:

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

Per identificare il tipo di entità rappresentato da un determinato elemento, EF Core aggiunge un valore discriminatore anche se non sono presenti tipi di entità derivati. Il nome e il valore del discriminatore [possono essere modificati](../../modeling/inheritance.md).

## <a name="embedded-entities"></a>Entità incorporate

Per Cosmos, le entità di proprietà sono incorporate nello stesso elemento del proprietario. Per modificare il nome di una proprietà, usare `ToJsonProperty`:

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

Con questa configurazione, l'ordine dell'esempio precedente viene archiviato come segue:

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
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-692e763901d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163674
}
```

Anche le raccolte di entità di proprietà sono incorporate. Per l'esempio successivo verrà usata la classe `Distributor` con una raccolta di `StreetAddress`:

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

Non è necessario che le entità di proprietà forniscano valori di chiave espliciti da archiviare:

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

Verranno salvati in modo persistente in questo modo:

``` json
{
    "Id": 1,
    "Discriminator": "Distributor",
    "id": "Distributor|1",
    "ShippingCenters": [
        {
            "City": "Phoenix",
            "Discriminator": "StreetAddress",
            "Street": "500 S 48th Street"
        },
        {
            "City": "Anaheim",
            "Discriminator": "StreetAddress",
            "Street": "5650 Dolly Ave"
        }
    ],
    "_rid": "6QEKANzISj0BAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKANzISj0=/docs/6QEKANzISj0BAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-7b2b439701d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163705
}
```

Internamente, EF Core deve avere sempre valori di chiave univoca per tutte le entità rilevate. La chiave primaria creata per impostazione predefinita per le raccolte di tipi di proprietà è costituita dalle proprietà di chiave esterna che puntano al proprietario e da una proprietà `int` corrispondente all'indice nella matrice JSON. Per recuperare questi valori, è possibile usare l'API Entry:

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> Quando necessario, è possibile modificare la chiave primaria predefinita per i tipi di entità di proprietà, ma i valori di chiave devono poi essere specificati in modo esplicito.

## <a name="working-with-disconnected-entities"></a>Uso delle entità disconnesse

Ogni elemento deve avere un valore `id` univoco per la chiave di partizione specificata. Per impostazione predefinita, EF Core genera il valore concatenando il discriminatore e i valori di chiave primaria usando '|' come delimitatore. I valori delle chiavi vengono generati solo quando un'entità entra nello stato `Added`. Questo potrebbe rappresentare un problema quando si [collegano le entità](../../saving/disconnected-entities.md) se non hanno una proprietà `id` per il tipo .NET per archiviare il valore.

Per ovviare a questa limitazione, è possibile creare e impostare il valore `id` manualmente oppure contrassegnare prima l'entità come aggiunta, per poi modificarla nello stato desiderato:

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

Questo è il codice JSON risultante:

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
        "ShipsToStreet": "3 Abbey Road"
    },
    "_rid": "6QEKAM+BOOABAAAAAAAAAA==",
    "_self": "dbs/6QEKAA==/colls/6QEKAM+BOOA=/docs/6QEKAM+BOOABAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683c-8f7ac48f01d5\"",
    "_attachments": "attachments/",
    "_ts": 1568163739
}
```