---
title: Alternanza tra più modelli con lo stesso tipo di DbContext-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 156d5666cbd9352b274ddc70c99704ca62aeb1fd
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781131"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="51e5c-102">Alternanza tra più modelli con lo stesso tipo di DbContext</span><span class="sxs-lookup"><span data-stu-id="51e5c-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="51e5c-103">Il modello incorporato `OnModelCreating` può utilizzare una proprietà nel contesto per modificare la modalità di compilazione del modello.</span><span class="sxs-lookup"><span data-stu-id="51e5c-103">The model built in `OnModelCreating` can use a property on the context to change how the model is built.</span></span> <span data-ttu-id="51e5c-104">Si supponga, ad esempio, di voler configurare un'entità in modo diverso in base a una proprietà:</span><span class="sxs-lookup"><span data-stu-id="51e5c-104">For example, suppose you wanted to configure an entity differently based on some property:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

<span data-ttu-id="51e5c-105">Sfortunatamente, questo codice non funziona così com'è, poiché EF compila il modello ed esegue `OnModelCreating` una sola volta, memorizzando nella cache il risultato per motivi di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="51e5c-105">Unfortunately, this code wouldn't work as-is, since EF builds the model and runs `OnModelCreating` only once, caching the result for performance reasons.</span></span> <span data-ttu-id="51e5c-106">Tuttavia, è possibile associare il meccanismo di memorizzazione nella cache del modello per rendere EF consapevole della proprietà che produce modelli diversi.</span><span class="sxs-lookup"><span data-stu-id="51e5c-106">However, you can hook into the model caching mechanism to make EF aware of the property producing different models.</span></span>

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="51e5c-107">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="51e5c-107">IModelCacheKeyFactory</span></span>

<span data-ttu-id="51e5c-108">EF usa il `IModelCacheKeyFactory` per generare chiavi di cache per i modelli; per impostazione predefinita, EF presuppone che per un determinato tipo di contesto il modello sarà lo stesso, quindi l'implementazione predefinita di questo servizio restituisce una chiave che contiene solo il tipo di contesto.</span><span class="sxs-lookup"><span data-stu-id="51e5c-108">EF uses the `IModelCacheKeyFactory` to generate cache keys for models; by default, EF assumes that for any given context type the model will be the same, so the default implementation of this service returns a key that just contains the context type.</span></span> <span data-ttu-id="51e5c-109">Per produrre modelli diversi dallo stesso tipo di contesto, è necessario sostituire il servizio `IModelCacheKeyFactory` con l'implementazione corretta; la chiave generata verrà confrontata con altre chiavi del modello usando il metodo `Equals`, prendendo in considerazione tutte le variabili che influiscono sul modello:</span><span class="sxs-lookup"><span data-stu-id="51e5c-109">To produce different models from the same context type, you need to replace the `IModelCacheKeyFactory` service with the correct  implementation; the generated key will be compared to other model keys using the `Equals` method, taking into account all the variables that affect the model:</span></span>

<span data-ttu-id="51e5c-110">Nell'implementazione seguente viene preso in considerazione il `IgnoreIntProperty` quando si produce una chiave di cache del modello:</span><span class="sxs-lookup"><span data-stu-id="51e5c-110">The following implementation takes the `IgnoreIntProperty` into account when producing a model cache key:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

<span data-ttu-id="51e5c-111">Infine, registrare la nuova `IModelCacheKeyFactory` nel `OnConfiguring`del contesto:</span><span class="sxs-lookup"><span data-stu-id="51e5c-111">Finally, register your new `IModelCacheKeyFactory` in your context's `OnConfiguring`:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

<span data-ttu-id="51e5c-112">Per ulteriori informazioni sul contesto, vedere il [progetto di esempio completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) .</span><span class="sxs-lookup"><span data-stu-id="51e5c-112">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) for more context.</span></span>
