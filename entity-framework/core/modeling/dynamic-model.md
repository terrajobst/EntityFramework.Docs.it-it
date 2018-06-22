---
title: Alternando tra più modelli con lo stesso tipo DbContext - Core a Entity Framework
author: AndriySvyryd
ms.author: divega
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/dynamic-model
ms.openlocfilehash: 57136802001124ebf9ae7682e33f8dc7826fc2b0
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678722"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="c7c0d-102">Alternando tra più modelli con lo stesso tipo DbContext</span><span class="sxs-lookup"><span data-stu-id="c7c0d-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="c7c0d-103">Il modello compilato `OnModelCreating` possibile utilizzare una proprietà nel contesto del database per modificare la modalità di compilazione del modello.</span><span class="sxs-lookup"><span data-stu-id="c7c0d-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="c7c0d-104">Ad esempio potrebbe essere utilizzata per escludere una determinata proprietà:</span><span class="sxs-lookup"><span data-stu-id="c7c0d-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="c7c0d-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="c7c0d-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="c7c0d-106">Tuttavia se si è tentato di eseguire precedenti senza modifiche aggiuntive si otterrebbe lo stesso modello ogni volta che viene creato un nuovo contesto per qualsiasi valore di `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="c7c0d-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="c7c0d-107">Ciò è causato dal modello di memorizzazione nella cache meccanismo EF utilizza per migliorare le prestazioni solo richiamando `OnModelCreating` una sola volta e la memorizzazione nella cache del modello.</span><span class="sxs-lookup"><span data-stu-id="c7c0d-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="c7c0d-108">Per impostazione predefinita EF si presuppone che per qualsiasi contesto specifico tipo di modello sarà lo stesso.</span><span class="sxs-lookup"><span data-stu-id="c7c0d-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="c7c0d-109">A tale scopo l'implementazione predefinita di `IModelCacheKeyFactory` restituisce una chiave che contiene solo il tipo di contesto.</span><span class="sxs-lookup"><span data-stu-id="c7c0d-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="c7c0d-110">Per modificare questa impostazione è necessario sostituire il `IModelCacheKeyFactory` servizio.</span><span class="sxs-lookup"><span data-stu-id="c7c0d-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="c7c0d-111">La nuova implementazione deve restituire un oggetto che può essere confrontato alle altre chiavi modello utilizzando il `Equals` metodo che prende in considerazione tutte le variabili che influiscono sul modello:</span><span class="sxs-lookup"><span data-stu-id="c7c0d-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
