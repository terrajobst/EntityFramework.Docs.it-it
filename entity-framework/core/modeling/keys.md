---
title: Chiavi (primario)-EF Core
description: Come configurare le chiavi per i tipi di entità quando si usa Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: fdaccb42259ba9dad97a05c626edd0291ca96cb0
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824616"
---
# <a name="keys-primary"></a>Chiavi (primarie)

Una chiave funge da identificatore univoco primario per ogni istanza di entità. Quando si utilizza un database relazionale, viene eseguito il mapping al concetto di *chiave primaria*. È anche possibile configurare un identificatore univoco che non sia la chiave primaria. per ulteriori informazioni, vedere [chiavi alternative](alternate-keys.md) .

Per configurare o creare una chiave primaria, è possibile utilizzare uno dei metodi seguenti.

## <a name="conventions"></a>Convenzioni

Per impostazione predefinita, una proprietà denominata `Id` o `<type name>Id` verrà configurata come chiave di un'entità.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyId&highlight=3)]

> [!NOTE]
> I [tipi di entità di proprietà](xref:core/modeling/owned-entities) utilizzano regole diverse per definire le chiavi.

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile utilizzare le annotazioni dei dati per configurare una singola proprietà come chiave di un'entità.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare una singola proprietà come chiave di un'entità.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

È anche possibile usare l'API Fluent per configurare più proprietà come chiave di un'entità, nota come chiave composta. Le chiavi composite possono essere configurate solo usando le convenzioni API Fluent non installeranno mai una chiave composta e non è possibile usare le annotazioni dei dati per configurarne una.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]

## <a name="key-types-and-values"></a>Tipi di chiave e valori

EF Core supporta l'utilizzo di proprietà di qualsiasi tipo primitivo come chiave primaria, inclusi `string`, `Guid`, `byte[]` e altri. Ma non tutti i database li supportano. In alcuni casi i valori di chiave possono essere convertiti automaticamente in un tipo supportato. in caso contrario, la conversione deve essere [specificata manualmente](xref:core/modeling/value-conversions).

Quando si aggiunge una nuova entità al contesto, le proprietà chiave devono sempre avere un valore non predefinito, ma alcuni tipi verranno [generati dal database](xref:core/modeling/generated-properties). In tal caso, EF tenterà di generare un valore temporaneo quando l'entità viene aggiunta per finalità di rilevamento. Dopo la chiamata a [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) , il valore temporaneo verrà sostituito dal valore generato dal database.

> [!Important]
> Se una proprietà chiave ha un valore generato dal database e viene specificato un valore non predefinito quando viene aggiunta un'entità, EF presuppone che l'entità esista già nel database e tenterà di aggiornarla invece di inserirne una nuova. Per evitare questo problema, disabilitare la generazione di valori o vedere [come specificare valori espliciti per le proprietà generate](../saving/explicit-values-generated-properties.md).