---
title: Indici-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: d1b5cd6853cd24f7e1aa3aace2f0a3455c657cc1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655703"
---
# <a name="indexes"></a>Indexes

Gli indici sono un concetto comune in molti archivi dati. Mentre la loro implementazione nell'archivio dati può variare, vengono usati per rendere più efficienti le ricerche basate su una colonna o un set di colonne.

## <a name="conventions"></a>Convenzioni

Per convenzione, viene creato un indice in ogni proprietà o set di proprietà utilizzate come chiave esterna.

## <a name="data-annotations"></a>Annotazioni dei dati

Non è possibile creare indici utilizzando le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per specificare un indice in una singola proprietà. Per impostazione predefinita, gli indici non sono univoci.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=7,8)]

È inoltre possibile specificare che un indice deve essere univoco, pertanto due entità non possono avere gli stessi valori per le proprietà specificate.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=ModelBuilder&highlight=3)]

È inoltre possibile specificare un indice per più di una colonna.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=7,8)]

> [!TIP]  
> È disponibile un solo indice per set di proprietà distinti. Se si usa l'API Fluent per configurare un indice in un set di proprietà in cui è già definito un indice, per convenzione o per la configurazione precedente, si modificherà la definizione di tale indice. Questa opzione è utile se si desidera configurare ulteriormente un indice creato per convenzione.
