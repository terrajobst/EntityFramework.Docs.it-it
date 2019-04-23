---
title: Chiavi (primario) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 51d163b867085f42f415dbd7afa9e311ab1781a0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929836"
---
# <a name="keys-primary"></a><span data-ttu-id="d4073-102">Chiavi (primarie)</span><span class="sxs-lookup"><span data-stu-id="d4073-102">Keys (primary)</span></span>

<span data-ttu-id="d4073-103">Una chiave funge da identificatore univoco primario per ogni istanza di entità.</span><span class="sxs-lookup"><span data-stu-id="d4073-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="d4073-104">Quando si usa un database relazionale viene eseguito il mapping al concetto di una *chiave primaria*.</span><span class="sxs-lookup"><span data-stu-id="d4073-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="d4073-105">È anche possibile configurare un identificatore univoco che non corrisponde alla chiave primaria (vedere [le chiavi alternative](alternate-keys.md) per altre informazioni).</span><span class="sxs-lookup"><span data-stu-id="d4073-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span> 

<span data-ttu-id="d4073-106">Uno dei metodi descritti di seguito è utilizzabile per il programma di installazione/creare una chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="d4073-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="d4073-107">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="d4073-107">Conventions</span></span>

<span data-ttu-id="d4073-108">Per convenzione, una proprietà denominata `Id` o `<type name>Id` saranno configurati come la chiave di un'entità.</span><span class="sxs-lookup"><span data-stu-id="d4073-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="d4073-109">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="d4073-109">Data Annotations</span></span>

<span data-ttu-id="d4073-110">È possibile usare le annotazioni dei dati per configurare una singola proprietà per la chiave di un'entità.</span><span class="sxs-lookup"><span data-stu-id="d4073-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="d4073-111">API Fluent</span><span class="sxs-lookup"><span data-stu-id="d4073-111">Fluent API</span></span>

<span data-ttu-id="d4073-112">È possibile usare l'API Fluent per configurare una singola proprietà per la chiave di un'entità.</span><span class="sxs-lookup"><span data-stu-id="d4073-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="d4073-113">È anche possibile usare l'API Fluent per configurare più proprietà per la chiave di un'entità (nota come una chiave composta).</span><span class="sxs-lookup"><span data-stu-id="d4073-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="d4073-114">Le chiavi composte possono essere configurate solo usando l'API Fluent: convenzioni non installerà mai una chiave composta e non è possibile usare le annotazioni dei dati per configurarne uno.</span><span class="sxs-lookup"><span data-stu-id="d4073-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=11,12)]
