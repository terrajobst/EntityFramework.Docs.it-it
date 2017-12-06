---
title: "Alternando tra più modelli con lo stesso tipo DbContext - Core a Entity Framework"
author: AndriySvyryd
uid: core/modeling/dynamic-model
ms.openlocfilehash: e6828c62c55ae6f48d46a20528268264c3f22b26
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Alternando tra più modelli con lo stesso tipo DbContext

Il modello compilato `OnModelCreating` possibile utilizzare una proprietà nel contesto del database per modificare la modalità di compilazione del modello. Ad esempio potrebbe essere utilizzata per escludere una determinata proprietà:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Tuttavia se si è tentato di eseguire precedenti senza modifiche aggiuntive si otterrebbe lo stesso modello ogni volta che viene creato un nuovo contesto per qualsiasi valore di `IgnoreIntProperty`. Ciò è causato dal modello di memorizzazione nella cache meccanismo EF utilizza per migliorare le prestazioni solo richiamando `OnModelCreating` una sola volta e la memorizzazione nella cache del modello.

Per impostazione predefinita EF si presuppone che per qualsiasi contesto specifico tipo di modello sarà lo stesso. A tale scopo l'implementazione predefinita di `IModelCacheKeyFactory` restituisce una chiave che contiene solo il tipo di contesto. Per modificare questa impostazione è necessario sostituire il `IModelCacheKeyFactory` servizio. La nuova implementazione deve restituire un oggetto che può essere confrontato alle altre chiavi modello utilizzando il `Equals` metodo che prende in considerazione tutte le variabili che influiscono sul modello:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
