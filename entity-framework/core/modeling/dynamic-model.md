---
title: Alternanza tra più modelli con lo stesso tipo di DbContext-EF Core
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 034076b1595894e80b98467354f6c9f139bd7426
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655721"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="15341-102">Alternanza tra più modelli con lo stesso tipo di DbContext</span><span class="sxs-lookup"><span data-stu-id="15341-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="15341-103">Il modello incorporato `OnModelCreating` possibile utilizzare una proprietà nel contesto per modificare la modalità di compilazione del modello.</span><span class="sxs-lookup"><span data-stu-id="15341-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="15341-104">Ad esempio, può essere usata per escludere una determinata proprietà:</span><span class="sxs-lookup"><span data-stu-id="15341-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="15341-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="15341-105">IModelCacheKeyFactory</span></span>

<span data-ttu-id="15341-106">Tuttavia, se si è provato a eseguire l'operazione precedente senza modifiche aggiuntive, si otterrebbe lo stesso modello ogni volta che viene creato un nuovo contesto per qualsiasi valore di `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="15341-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="15341-107">Questo è dovuto al meccanismo di memorizzazione nella cache del modello utilizzato da Entity Framework per migliorare le prestazioni richiamando `OnModelCreating` una sola volta e memorizzando nella cache il modello.</span><span class="sxs-lookup"><span data-stu-id="15341-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="15341-108">Per impostazione predefinita, EF presuppone che per un determinato tipo di contesto il modello sarà lo stesso.</span><span class="sxs-lookup"><span data-stu-id="15341-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="15341-109">Per eseguire questa operazione, l'implementazione predefinita di `IModelCacheKeyFactory` restituisce una chiave che contiene solo il tipo di contesto.</span><span class="sxs-lookup"><span data-stu-id="15341-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="15341-110">Per modificare questa operazione, è necessario sostituire il servizio `IModelCacheKeyFactory`.</span><span class="sxs-lookup"><span data-stu-id="15341-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="15341-111">La nuova implementazione deve restituire un oggetto che può essere confrontato con altre chiavi del modello usando il metodo `Equals` che prende in considerazione tutte le variabili che interessano il modello:</span><span class="sxs-lookup"><span data-stu-id="15341-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
