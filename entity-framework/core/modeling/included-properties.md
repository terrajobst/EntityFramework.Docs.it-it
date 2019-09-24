---
title: Inclusione & escluse le proprietà-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: cd111af891ef0bbaccf515eed0c1991f105bd362
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197417"
---
# <a name="including--excluding-properties"></a>Inclusione & escluse le proprietà

L'inclusione di una proprietà nel modello significa che EF dispone di metadati relativi a tale proprietà e tenterà di leggere e scrivere valori da/nel database.

## <a name="conventions"></a>Convenzioni

Per convenzione, le proprietà pubbliche con un getter e un setter verranno incluse nel modello.

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile utilizzare le annotazioni dei dati per escludere una proprietà dal modello.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>API Fluent

È possibile utilizzare l'API Fluent per escludere una proprietà dal modello.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?highlight=12,13)]
