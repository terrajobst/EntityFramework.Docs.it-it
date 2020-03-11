---
title: Alternanza tra più modelli con lo stesso tipo di DbContext-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: a160f0d382ee2a3ac7130ce1ac98eb24b3f79394
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416360"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Alternanza tra più modelli con lo stesso tipo di DbContext

Il modello incorporato `OnModelCreating` può utilizzare una proprietà nel contesto per modificare la modalità di compilazione del modello. Si supponga, ad esempio, di voler configurare un'entità in modo diverso in base a una proprietà:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

Sfortunatamente, questo codice non funziona così com'è, poiché EF compila il modello ed esegue `OnModelCreating` una sola volta, memorizzando nella cache il risultato per motivi di prestazioni. Tuttavia, è possibile associare il meccanismo di memorizzazione nella cache del modello per rendere EF consapevole della proprietà che produce modelli diversi.

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory

EF usa il `IModelCacheKeyFactory` per generare chiavi di cache per i modelli; per impostazione predefinita, EF presuppone che per un determinato tipo di contesto il modello sarà lo stesso, quindi l'implementazione predefinita di questo servizio restituisce una chiave che contiene solo il tipo di contesto. Per produrre modelli diversi dallo stesso tipo di contesto, è necessario sostituire il servizio `IModelCacheKeyFactory` con l'implementazione corretta; la chiave generata verrà confrontata con altre chiavi del modello usando il metodo `Equals`, prendendo in considerazione tutte le variabili che influiscono sul modello:

Nell'implementazione seguente viene preso in considerazione il `IgnoreIntProperty` quando si produce una chiave di cache del modello:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

Infine, registrare la nuova `IModelCacheKeyFactory` nel `OnConfiguring`del contesto:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

Per ulteriori informazioni sul contesto, vedere il [progetto di esempio completo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) .
