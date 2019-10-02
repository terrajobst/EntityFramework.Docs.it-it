---
title: Provider di Azure Cosmos DB - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 28264681-4486-4891-888c-be5e4ade24f1
uid: core/providers/cosmos/index
ms.openlocfilehash: 96686256bb93f5828bb21fed167eb57812806390
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813539"
---
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="c3904-102">Provider di Azure Cosmos DB per EF Core</span><span class="sxs-lookup"><span data-stu-id="c3904-102">EF Core Azure Cosmos DB Provider</span></span>

>[!NOTE]
> <span data-ttu-id="c3904-103">Questo provider è una novità di EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="c3904-103">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="c3904-104">Questo provider di database consente l'uso di Entity Framework Core con Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c3904-104">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="c3904-105">Il provider viene gestito nell'ambito del [progetto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="c3904-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="c3904-106">Prima di leggere questa sezione, è consigliabile acquisire familiarità con la [documentazione di Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction).</span><span class="sxs-lookup"><span data-stu-id="c3904-106">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) before reading this section.</span></span>

## <a name="install"></a><span data-ttu-id="c3904-107">Installazione di</span><span class="sxs-lookup"><span data-stu-id="c3904-107">Install</span></span>

<span data-ttu-id="c3904-108">Installare il [pacchetto NuGet Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span><span class="sxs-lookup"><span data-stu-id="c3904-108">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

# <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="c3904-109">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="c3904-109">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.Cosmos
```

# <a name="visual-studiotabvs"></a>[<span data-ttu-id="c3904-110">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c3904-110">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

***

## <a name="get-started"></a><span data-ttu-id="c3904-111">Introduzione</span><span class="sxs-lookup"><span data-stu-id="c3904-111">Get Started</span></span>

> [!TIP]  
> <span data-ttu-id="c3904-112">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="c3904-112">You can view this article's [sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="c3904-113">Come per gli altri provider, il primo passaggio consiste nel chiamare `UseCosmos`:</span><span class="sxs-lookup"><span data-stu-id="c3904-113">Like for other providers the first step is to call `UseCosmos`:</span></span>

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> <span data-ttu-id="c3904-114">L'endpoint e la chiave sono hardcoded qui per semplicità, ma in un'app di produzione devono essere [archiviati in modo sicuro](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)</span><span class="sxs-lookup"><span data-stu-id="c3904-114">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securily](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)</span></span>

<span data-ttu-id="c3904-115">In questo esempio `Order` è un'entità semplice con un riferimento al [tipo i proprietà](../../modeling/owned-entities.md) `StreetAddress`.</span><span class="sxs-lookup"><span data-stu-id="c3904-115">In this example `Order` is a simple entity with a reference to the [owned type](../../modeling/owned-entities.md) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="c3904-116">Per il salvataggio e l'esecuzione di query sui dati si segue il modello EF normale:</span><span class="sxs-lookup"><span data-stu-id="c3904-116">Saving and quering data follows the normal EF pattern:</span></span>

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> <span data-ttu-id="c3904-117">La chiamata di `EnsureCreated` è necessaria per creare le raccolte richieste e inserire i [dati di inizializzazione](../../modeling/data-seeding.md) se presenti nel modello.</span><span class="sxs-lookup"><span data-stu-id="c3904-117">Calling `EnsureCreated` is necessary to create the required collections and insert the [seed data](../../modeling/data-seeding.md) if present in the model.</span></span> <span data-ttu-id="c3904-118">Tuttavia, `EnsureCreated` deve essere chiamato solo durante la distribuzione, e non durante il normale funzionamento, perché potrebbe causare problemi di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="c3904-118">However `EnsureCreated` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="c3904-119">Personalizzazione del modello specifico di Cosmos</span><span class="sxs-lookup"><span data-stu-id="c3904-119">Cosmos-specific Model Customization</span></span>

<span data-ttu-id="c3904-120">Per impostazione predefinita, viene eseguito il mapping di tutti i tipi di entità allo stesso contenitore, denominato in base al contesto derivato (`"OrderContext"` in questo caso).</span><span class="sxs-lookup"><span data-stu-id="c3904-120">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="c3904-121">Per modificare il nome del contenitore predefinito, usare `HasDefaultContainer`:</span><span class="sxs-lookup"><span data-stu-id="c3904-121">To change the default container name use `HasDefaultContainer`:</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="c3904-122">Per eseguire il mapping di un tipo di entità a un contenitore diverso, usare `ToContainer`:</span><span class="sxs-lookup"><span data-stu-id="c3904-122">To map an entity type to a different container use `ToContainer`:</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="c3904-123">Per identificare il tipo di entità rappresentato da un determinato elemento, EF Core aggiunge un valore discriminatore anche se non sono presenti tipi di entità derivati.</span><span class="sxs-lookup"><span data-stu-id="c3904-123">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="c3904-124">Il nome e il valore del discriminatore [possono essere modificati](../../modeling/inheritance.md).</span><span class="sxs-lookup"><span data-stu-id="c3904-124">The name and value of the discriminator [can be changed](../../modeling/inheritance.md).</span></span>

## <a name="embedded-entities"></a><span data-ttu-id="c3904-125">Entità incorporate</span><span class="sxs-lookup"><span data-stu-id="c3904-125">Embedded Entities</span></span>

<span data-ttu-id="c3904-126">Per Cosmos, le entità di proprietà sono incorporate nello stesso elemento del proprietario.</span><span class="sxs-lookup"><span data-stu-id="c3904-126">For Cosmos owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="c3904-127">Per modificare il nome di una proprietà, usare `ToJsonProperty`:</span><span class="sxs-lookup"><span data-stu-id="c3904-127">To change a property name use `ToJsonProperty`:</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="c3904-128">Con questa configurazione, l'ordine dell'esempio precedente viene archiviato come segue:</span><span class="sxs-lookup"><span data-stu-id="c3904-128">With this configuration the order from the example above is stored like this:</span></span>

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

<span data-ttu-id="c3904-129">Anche le raccolte di entità di proprietà sono incorporate.</span><span class="sxs-lookup"><span data-stu-id="c3904-129">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="c3904-130">Per l'esempio successivo verrà usata la classe `Distributor` con una raccolta di `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="c3904-130">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="c3904-131">Non è necessario che le entità di proprietà forniscano valori di chiave espliciti da archiviare:</span><span class="sxs-lookup"><span data-stu-id="c3904-131">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="c3904-132">Verranno salvati in modo persistente in questo modo:</span><span class="sxs-lookup"><span data-stu-id="c3904-132">They will be persisted in this way:</span></span>

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

<span data-ttu-id="c3904-133">Internamente, EF Core deve avere sempre valori di chiave univoca per tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="c3904-133">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="c3904-134">La chiave primaria creata per impostazione predefinita per le raccolte di tipi di proprietà è costituita dalle proprietà di chiave esterna che puntano al proprietario e da una proprietà `int` corrispondente all'indice nella matrice JSON.</span><span class="sxs-lookup"><span data-stu-id="c3904-134">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="c3904-135">Per recuperare questi valori, è possibile usare l'API Entry:</span><span class="sxs-lookup"><span data-stu-id="c3904-135">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="c3904-136">Quando necessario, è possibile modificare la chiave primaria predefinita per i tipi di entità di proprietà, ma i valori di chiave devono poi essere specificati in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="c3904-136">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="c3904-137">Uso delle entità disconnesse</span><span class="sxs-lookup"><span data-stu-id="c3904-137">Working with Disconnected Entities</span></span>

<span data-ttu-id="c3904-138">Ogni elemento deve avere un valore `id` univoco per la chiave di partizione specificata.</span><span class="sxs-lookup"><span data-stu-id="c3904-138">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="c3904-139">Per impostazione predefinita, EF Core genera il valore concatenando il discriminatore e i valori di chiave primaria usando '|' come delimitatore.</span><span class="sxs-lookup"><span data-stu-id="c3904-139">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="c3904-140">I valori delle chiavi vengono generati solo quando un'entità entra nello stato `Added`.</span><span class="sxs-lookup"><span data-stu-id="c3904-140">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="c3904-141">Questo potrebbe rappresentare un problema quando si [collegano le entità](../../saving/disconnected-entities.md) se non hanno una proprietà `id` per il tipo .NET per archiviare il valore.</span><span class="sxs-lookup"><span data-stu-id="c3904-141">This might pose a problem when [attaching entities](../../saving/disconnected-entities.md) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="c3904-142">Per ovviare a questa limitazione, è possibile creare e impostare il valore `id` manualmente oppure contrassegnare prima l'entità come aggiunta, per poi modificarla nello stato desiderato:</span><span class="sxs-lookup"><span data-stu-id="c3904-142">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="c3904-143">Questo è il codice JSON risultante:</span><span class="sxs-lookup"><span data-stu-id="c3904-143">This is the resulting JSON:</span></span>

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
