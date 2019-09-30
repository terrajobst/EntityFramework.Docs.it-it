---
title: Provider di Azure Cosmos DB - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 28264681-4486-4891-888c-be5e4ade24f1
uid: core/providers/cosmos/index
ms.openlocfilehash: 683436aa485d2fef9aa8bf6c6ff02b00dfeb28cf
ms.sourcegitcommit: 2caec1e63f2ce1d9439ef6193df5a77da2fedd0f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317568"
---
# <a name="ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="61d7b-102">Provider di Azure Cosmos DB per EF Core</span><span class="sxs-lookup"><span data-stu-id="61d7b-102">EF Core Azure Cosmos DB Provider</span></span>

>[!NOTE]
> <span data-ttu-id="61d7b-103">Questo provider è una novità di EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="61d7b-103">This provider is new in EF Core 3.0.</span></span>

<span data-ttu-id="61d7b-104">Questo provider di database consente l'uso di Entity Framework Core con Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="61d7b-104">This database provider allows Entity Framework Core to be used with Azure Cosmos DB.</span></span> <span data-ttu-id="61d7b-105">Il provider viene gestito nell'ambito del [progetto Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="61d7b-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

<span data-ttu-id="61d7b-106">Prima di leggere questa sezione, è consigliabile acquisire familiarità con la [documentazione di Azure Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction).</span><span class="sxs-lookup"><span data-stu-id="61d7b-106">It is strongly recommended to familiarize yourself with the [Azure Cosmos DB documentation](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction) before reading this section.</span></span>

## <a name="install"></a><span data-ttu-id="61d7b-107">Installazione di</span><span class="sxs-lookup"><span data-stu-id="61d7b-107">Install</span></span>

<span data-ttu-id="61d7b-108">Installare il [pacchetto NuGet Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span><span class="sxs-lookup"><span data-stu-id="61d7b-108">Install the [Microsoft.EntityFrameworkCore.Cosmos NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Cosmos
```

## <a name="get-started"></a><span data-ttu-id="61d7b-109">Introduzione</span><span class="sxs-lookup"><span data-stu-id="61d7b-109">Get Started</span></span>

> [!TIP]  
> <span data-ttu-id="61d7b-110">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="61d7b-110">You can view this article's [sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Cosmos).</span></span>

<span data-ttu-id="61d7b-111">Come per gli altri provider, il primo passaggio consiste nel chiamare `UseCosmos`:</span><span class="sxs-lookup"><span data-stu-id="61d7b-111">Like for other providers the first step is to call `UseCosmos`:</span></span>

[!code-csharp[Configuration](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Configuration)]

> [!WARNING]
> <span data-ttu-id="61d7b-112">L'endpoint e la chiave sono hardcoded qui per semplicità, ma in un'app di produzione devono essere [archiviati in modo sicuro](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)</span><span class="sxs-lookup"><span data-stu-id="61d7b-112">The endpoint and key are hardcoded here for simplicity, but in a production app these should be [stored securily](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)</span></span>

<span data-ttu-id="61d7b-113">In questo esempio `Order` è un'entità semplice con un riferimento al [tipo i proprietà](../../modeling/owned-entities.md) `StreetAddress`.</span><span class="sxs-lookup"><span data-stu-id="61d7b-113">In this example `Order` is a simple entity with a reference to the [owned type](../../modeling/owned-entities.md) `StreetAddress`.</span></span>

[!code-csharp[Order](../../../../samples/core/Cosmos/ModelBuilding/Order.cs?name=Order)]

[!code-csharp[StreetAddress](../../../../samples/core/Cosmos/ModelBuilding/StreetAddress.cs?name=StreetAddress)]

<span data-ttu-id="61d7b-114">Per il salvataggio e l'esecuzione di query sui dati si segue il modello EF normale:</span><span class="sxs-lookup"><span data-stu-id="61d7b-114">Saving and quering data follows the normal EF pattern:</span></span>

[!code-csharp[HelloCosmos](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=HelloCosmos)]

> [!IMPORTANT]
> <span data-ttu-id="61d7b-115">La chiamata di `EnsureCreated` è necessaria per creare le raccolte richieste e inserire i [dati di inizializzazione](../../modeling/data-seeding.md) se presenti nel modello.</span><span class="sxs-lookup"><span data-stu-id="61d7b-115">Calling `EnsureCreated` is necessary to create the required collections and insert the [seed data](../../modeling/data-seeding.md) if present in the model.</span></span> <span data-ttu-id="61d7b-116">Tuttavia, `EnsureCreated` deve essere chiamato solo durante la distribuzione, e non durante il normale funzionamento, perché potrebbe causare problemi di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="61d7b-116">However `EnsureCreated` should only be called during deployment, not normal operation, as it may cause performance issues.</span></span>

## <a name="cosmos-specific-model-customization"></a><span data-ttu-id="61d7b-117">Personalizzazione del modello specifico di Cosmos</span><span class="sxs-lookup"><span data-stu-id="61d7b-117">Cosmos-specific Model Customization</span></span>

<span data-ttu-id="61d7b-118">Per impostazione predefinita, viene eseguito il mapping di tutti i tipi di entità allo stesso contenitore, denominato in base al contesto derivato (`"OrderContext"` in questo caso).</span><span class="sxs-lookup"><span data-stu-id="61d7b-118">By default all entity types are mapped to the same container, named after the derived context (`"OrderContext"` in this case).</span></span> <span data-ttu-id="61d7b-119">Per modificare il nome del contenitore predefinito, usare `HasDefaultContainer`:</span><span class="sxs-lookup"><span data-stu-id="61d7b-119">To change the default container name use `HasDefaultContainer`:</span></span>

[!code-csharp[DefaultContainer](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=DefaultContainer)]

<span data-ttu-id="61d7b-120">Per eseguire il mapping di un tipo di entità a un contenitore diverso, usare `ToContainer`:</span><span class="sxs-lookup"><span data-stu-id="61d7b-120">To map an entity type to a different container use `ToContainer`:</span></span>

[!code-csharp[Container](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=Container)]

<span data-ttu-id="61d7b-121">Per identificare il tipo di entità rappresentato da un determinato elemento, EF Core aggiunge un valore discriminatore anche se non sono presenti tipi di entità derivati.</span><span class="sxs-lookup"><span data-stu-id="61d7b-121">To identify the entity type that a given item represent EF Core adds a discriminator value even if there are no derived entity types.</span></span> <span data-ttu-id="61d7b-122">Il nome e il valore del discriminatore [possono essere modificati](../../modeling/inheritance.md).</span><span class="sxs-lookup"><span data-stu-id="61d7b-122">The name and value of the discriminator [can be changed](../../modeling/inheritance.md).</span></span>

## <a name="embedded-entities"></a><span data-ttu-id="61d7b-123">Entità incorporate</span><span class="sxs-lookup"><span data-stu-id="61d7b-123">Embedded Entities</span></span>

<span data-ttu-id="61d7b-124">Per Cosmos, le entità di proprietà sono incorporate nello stesso elemento del proprietario.</span><span class="sxs-lookup"><span data-stu-id="61d7b-124">For Cosmos owned entities are embedded in the same item as the owner.</span></span> <span data-ttu-id="61d7b-125">Per modificare il nome di una proprietà, usare `ToJsonProperty`:</span><span class="sxs-lookup"><span data-stu-id="61d7b-125">To change a property name use `ToJsonProperty`:</span></span>

[!code-csharp[PropertyNames](../../../../samples/core/Cosmos/ModelBuilding/OrderContext.cs?name=PropertyNames)]

<span data-ttu-id="61d7b-126">Con questa configurazione, l'ordine dell'esempio precedente viene archiviato come segue:</span><span class="sxs-lookup"><span data-stu-id="61d7b-126">With this configuration the order from the example above is stored like this:</span></span>

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

<span data-ttu-id="61d7b-127">Anche le raccolte di entità di proprietà sono incorporate.</span><span class="sxs-lookup"><span data-stu-id="61d7b-127">Collections of owned entities are embedded as well.</span></span> <span data-ttu-id="61d7b-128">Per l'esempio successivo verrà usata la classe `Distributor` con una raccolta di `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="61d7b-128">For the next example we'll use the `Distributor` class with a collection of `StreetAddress`:</span></span>

[!code-csharp[Distributor](../../../../samples/core/Cosmos/ModelBuilding/Distributor.cs?name=Distributor)]

<span data-ttu-id="61d7b-129">Non è necessario che le entità di proprietà forniscano valori di chiave espliciti da archiviare:</span><span class="sxs-lookup"><span data-stu-id="61d7b-129">The owned entities don't need to provide explicit key values to be stored:</span></span>

[!code-csharp[OwnedCollection](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=OwnedCollection)]

<span data-ttu-id="61d7b-130">Verranno salvati in modo persistente in questo modo:</span><span class="sxs-lookup"><span data-stu-id="61d7b-130">They will be persisted in this way:</span></span>

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

<span data-ttu-id="61d7b-131">Internamente, EF Core deve avere sempre valori di chiave univoca per tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="61d7b-131">Internally EF Core always needs to have unique key values for all tracked entities.</span></span> <span data-ttu-id="61d7b-132">La chiave primaria creata per impostazione predefinita per le raccolte di tipi di proprietà è costituita dalle proprietà di chiave esterna che puntano al proprietario e da una proprietà `int` corrispondente all'indice nella matrice JSON.</span><span class="sxs-lookup"><span data-stu-id="61d7b-132">The primary key created by default for collections of owned types consists of the foreign key properties pointing to the owner and an `int` property corresponding to the index in the JSON array.</span></span> <span data-ttu-id="61d7b-133">Per recuperare questi valori, è possibile usare l'API Entry:</span><span class="sxs-lookup"><span data-stu-id="61d7b-133">To retrieve these values entry API could be used:</span></span>

[!code-csharp[ImpliedProperties](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?name=ImpliedProperties)]

> [!TIP]
> <span data-ttu-id="61d7b-134">Quando necessario, è possibile modificare la chiave primaria predefinita per i tipi di entità di proprietà, ma i valori di chiave devono poi essere specificati in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="61d7b-134">When necessary the default primary key for the owned entity types can be changed, but then key values should be provided explicitly.</span></span>

## <a name="working-with-disconnected-entities"></a><span data-ttu-id="61d7b-135">Uso delle entità disconnesse</span><span class="sxs-lookup"><span data-stu-id="61d7b-135">Working with Disconnected Entities</span></span>

<span data-ttu-id="61d7b-136">Ogni elemento deve avere un valore `id` univoco per la chiave di partizione specificata.</span><span class="sxs-lookup"><span data-stu-id="61d7b-136">Every item needs to have an `id` value that is unique for the given partition key.</span></span> <span data-ttu-id="61d7b-137">Per impostazione predefinita, EF Core genera il valore concatenando il discriminatore e i valori di chiave primaria usando '|' come delimitatore.</span><span class="sxs-lookup"><span data-stu-id="61d7b-137">By default EF Core generates the value by concatenating the discriminator and the primary key values, using '|' as a delimiter.</span></span> <span data-ttu-id="61d7b-138">I valori delle chiavi vengono generati solo quando un'entità entra nello stato `Added`.</span><span class="sxs-lookup"><span data-stu-id="61d7b-138">The key values are only generated when an entity enters the `Added` state.</span></span> <span data-ttu-id="61d7b-139">Questo potrebbe rappresentare un problema quando si [collegano le entità](../../saving/disconnected-entities.md) se non hanno una proprietà `id` per il tipo .NET per archiviare il valore.</span><span class="sxs-lookup"><span data-stu-id="61d7b-139">This might pose a problem when [attaching entities](../../saving/disconnected-entities.md) if they don't have an `id` property on the .NET type to store the value.</span></span>

<span data-ttu-id="61d7b-140">Per ovviare a questa limitazione, è possibile creare e impostare il valore `id` manualmente oppure contrassegnare prima l'entità come aggiunta, per poi modificarla nello stato desiderato:</span><span class="sxs-lookup"><span data-stu-id="61d7b-140">To work around this limitation one could create and set the `id` value manually or mark the entity as added first, then changing it to the desired state:</span></span>

[!code-csharp[Attach](../../../../samples/core/Cosmos/ModelBuilding/Sample.cs?highlight=4&name=Attach)]

<span data-ttu-id="61d7b-141">Questo è il codice JSON risultante:</span><span class="sxs-lookup"><span data-stu-id="61d7b-141">This is the resulting JSON:</span></span>

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
