---
title: Chiavi (primario)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 8b32bf6417890a954c933a5973a2c90c609beeca
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197278"
---
# <a name="keys-primary"></a><span data-ttu-id="5efc8-102">Chiavi (primario)</span><span class="sxs-lookup"><span data-stu-id="5efc8-102">Keys (primary)</span></span>

<span data-ttu-id="5efc8-103">Una chiave funge da identificatore univoco primario per ogni istanza di entità.</span><span class="sxs-lookup"><span data-stu-id="5efc8-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="5efc8-104">Quando si utilizza un database relazionale, viene eseguito il mapping al concetto di *chiave primaria*.</span><span class="sxs-lookup"><span data-stu-id="5efc8-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="5efc8-105">È anche possibile configurare un identificatore univoco che non sia la chiave primaria. per ulteriori informazioni, vedere [chiavi alternative](alternate-keys.md) .</span><span class="sxs-lookup"><span data-stu-id="5efc8-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span> 

<span data-ttu-id="5efc8-106">Per configurare o creare una chiave primaria, è possibile utilizzare uno dei metodi seguenti.</span><span class="sxs-lookup"><span data-stu-id="5efc8-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="5efc8-107">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="5efc8-107">Conventions</span></span>

<span data-ttu-id="5efc8-108">Per convenzione, una proprietà denominata `Id` o `<type name>Id` verrà configurata come chiave di un'entità.</span><span class="sxs-lookup"><span data-stu-id="5efc8-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="5efc8-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="5efc8-109">Data Annotations</span></span>

<span data-ttu-id="5efc8-110">È possibile utilizzare le annotazioni dei dati per configurare una singola proprietà come chiave di un'entità.</span><span class="sxs-lookup"><span data-stu-id="5efc8-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="5efc8-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="5efc8-111">Fluent API</span></span>

<span data-ttu-id="5efc8-112">È possibile usare l'API Fluent per configurare una singola proprietà come chiave di un'entità.</span><span class="sxs-lookup"><span data-stu-id="5efc8-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="5efc8-113">È anche possibile usare l'API Fluent per configurare più proprietà come chiave di un'entità, nota come chiave composta.</span><span class="sxs-lookup"><span data-stu-id="5efc8-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="5efc8-114">Le chiavi composite possono essere configurate solo usando le convenzioni API Fluent non installeranno mai una chiave composta e non è possibile usare le annotazioni dei dati per configurarne una.</span><span class="sxs-lookup"><span data-stu-id="5efc8-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
