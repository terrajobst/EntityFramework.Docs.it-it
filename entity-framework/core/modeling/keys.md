---
title: Chiavi (primario)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 66c64c389294e8e109a614a2bea8311932660dea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655939"
---
# <a name="keys-primary"></a>Chiavi (primarie)

Una chiave funge da identificatore univoco primario per ogni istanza di entità. Quando si utilizza un database relazionale, viene eseguito il mapping al concetto di *chiave primaria*. È anche possibile configurare un identificatore univoco che non sia la chiave primaria. per ulteriori informazioni, vedere [chiavi alternative](alternate-keys.md) .

Per configurare o creare una chiave primaria, è possibile utilizzare uno dei metodi seguenti.

## <a name="conventions"></a>Convenzioni

Per convenzione, una proprietà denominata `Id` o `<type name>Id` verrà configurata come chiave di un'entità.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyIdhighlight=3)]

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile utilizzare le annotazioni dei dati per configurare una singola proprietà come chiave di un'entità.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare una singola proprietà come chiave di un'entità.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

È anche possibile usare l'API Fluent per configurare più proprietà come chiave di un'entità, nota come chiave composta. Le chiavi composite possono essere configurate solo usando le convenzioni API Fluent non installeranno mai una chiave composta e non è possibile usare le annotazioni dei dati per configurarne una.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
