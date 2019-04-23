---
title: Inclusione ed esclusione di proprietà - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 022534091bb48e491c8808791a401216a339d7b0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929824"
---
# <a name="including--excluding-properties"></a>Inclusione ed esclusione di proprietà

Tra cui una proprietà nel modello significa che Entity Framework include metadati sulla proprietà e tenterà di leggere e scrivere i valori da e verso il database.

## <a name="conventions"></a>Convenzioni

Per convenzione, le proprietà pubbliche con metodi get e un setter verranno inclusi nel modello.

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile utilizzare le annotazioni dei dati per escludere una proprietà dal modello.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per escludere una proprietà dal modello.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
