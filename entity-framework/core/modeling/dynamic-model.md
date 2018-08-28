---
title: Alternando tra più modelli con lo stesso tipo DbContext - EF Core
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 1d87efb668c12a2862583fba16a6c444b3cda9de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994986"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="8f1ec-102">Alternando tra più modelli con lo stesso tipo DbContext</span><span class="sxs-lookup"><span data-stu-id="8f1ec-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="8f1ec-103">Il modello compilato `OnModelCreating` potrebbe usare una proprietà nel contesto per modificare la modalità di compilazione del modello.</span><span class="sxs-lookup"><span data-stu-id="8f1ec-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="8f1ec-104">Ad esempio può essere usata per escludere una determinata proprietà:</span><span class="sxs-lookup"><span data-stu-id="8f1ec-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="8f1ec-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="8f1ec-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="8f1ec-106">Tuttavia se si è provato a eseguire il codice precedente senza modifiche aggiuntive si otterrebbe lo stesso modello ogni volta che viene creato un nuovo contesto per qualsiasi valore di `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="8f1ec-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="8f1ec-107">Ciò è dovuto il modello di Entity Framework utilizza per migliorare le prestazioni eseguendo solo meccanismo di memorizzazione nella cache `OnModelCreating` una sola volta e la memorizzazione nella cache il modello.</span><span class="sxs-lookup"><span data-stu-id="8f1ec-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="8f1ec-108">Per impostazione predefinita Entity Framework presuppone che per qualsiasi contesto specificato tipo di modello sia lo stesso.</span><span class="sxs-lookup"><span data-stu-id="8f1ec-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="8f1ec-109">Per eseguire questa operazione l'implementazione predefinita di `IModelCacheKeyFactory` restituisce una chiave che contiene solo il tipo di contesto.</span><span class="sxs-lookup"><span data-stu-id="8f1ec-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="8f1ec-110">Modificare questa impostazione è necessario sostituire il `IModelCacheKeyFactory` servizio.</span><span class="sxs-lookup"><span data-stu-id="8f1ec-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="8f1ec-111">La nuova implementazione deve restituire un oggetto che può essere confrontato a altre chiavi di modello utilizzando il `Equals` metodo che prende in considerazione tutte le variabili che influiscono sul modello:</span><span class="sxs-lookup"><span data-stu-id="8f1ec-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
