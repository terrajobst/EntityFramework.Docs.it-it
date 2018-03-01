---
title: "Alternando tra più modelli con lo stesso tipo DbContext - Core a Entity Framework"
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
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Alternando tra più modelli con lo stesso tipo DbContext

Il modello compilato `OnModelCreating` possibile utilizzare una proprietà nel contesto del database per modificare la modalità di compilazione del modello. Ad esempio potrebbe essere utilizzata per escludere una determinata proprietà:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Tuttavia se si è tentato di eseguire precedenti senza modifiche aggiuntive si otterrebbe lo stesso modello ogni volta che viene creato un nuovo contesto per qualsiasi valore di `IgnoreIntProperty`. Ciò è causato dal modello di memorizzazione nella cache meccanismo EF utilizza per migliorare le prestazioni solo richiamando `OnModelCreating` una sola volta e la memorizzazione nella cache del modello.

Per impostazione predefinita EF si presuppone che per qualsiasi contesto specifico tipo di modello sarà lo stesso. A tale scopo l'implementazione predefinita di `IModelCacheKeyFactory` restituisce una chiave che contiene solo il tipo di contesto. Per modificare questa impostazione è necessario sostituire il `IModelCacheKeyFactory` servizio. La nuova implementazione deve restituire un oggetto che può essere confrontato alle altre chiavi modello utilizzando il `Equals` metodo che prende in considerazione tutte le variabili che influiscono sul modello:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
