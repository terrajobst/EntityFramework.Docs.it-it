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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Alternanza tra più modelli con lo stesso tipo di DbContext

Il modello incorporato `OnModelCreating` possibile utilizzare una proprietà nel contesto per modificare la modalità di compilazione del modello. Ad esempio, può essere usata per escludere una determinata proprietà:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory

Tuttavia, se si è provato a eseguire l'operazione precedente senza modifiche aggiuntive, si otterrebbe lo stesso modello ogni volta che viene creato un nuovo contesto per qualsiasi valore di `IgnoreIntProperty`. Questo è dovuto al meccanismo di memorizzazione nella cache del modello utilizzato da Entity Framework per migliorare le prestazioni richiamando `OnModelCreating` una sola volta e memorizzando nella cache il modello.

Per impostazione predefinita, EF presuppone che per un determinato tipo di contesto il modello sarà lo stesso. Per eseguire questa operazione, l'implementazione predefinita di `IModelCacheKeyFactory` restituisce una chiave che contiene solo il tipo di contesto. Per modificare questa operazione, è necessario sostituire il servizio `IModelCacheKeyFactory`. La nuova implementazione deve restituire un oggetto che può essere confrontato con altre chiavi del modello usando il metodo `Equals` che prende in considerazione tutte le variabili che interessano il modello:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
