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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Alternando tra più modelli con lo stesso tipo DbContext

Il modello compilato `OnModelCreating` potrebbe usare una proprietà nel contesto per modificare la modalità di compilazione del modello. Ad esempio può essere usata per escludere una determinata proprietà:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Tuttavia se si è provato a eseguire il codice precedente senza modifiche aggiuntive si otterrebbe lo stesso modello ogni volta che viene creato un nuovo contesto per qualsiasi valore di `IgnoreIntProperty`. Ciò è dovuto il modello di Entity Framework utilizza per migliorare le prestazioni eseguendo solo meccanismo di memorizzazione nella cache `OnModelCreating` una sola volta e la memorizzazione nella cache il modello.

Per impostazione predefinita Entity Framework presuppone che per qualsiasi contesto specificato tipo di modello sia lo stesso. Per eseguire questa operazione l'implementazione predefinita di `IModelCacheKeyFactory` restituisce una chiave che contiene solo il tipo di contesto. Modificare questa impostazione è necessario sostituire il `IModelCacheKeyFactory` servizio. La nuova implementazione deve restituire un oggetto che può essere confrontato a altre chiavi di modello utilizzando il `Equals` metodo che prende in considerazione tutte le variabili che influiscono sul modello:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
