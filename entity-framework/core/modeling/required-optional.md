---
title: Proprietà obbligatorie/facoltative-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 7200cd2eeeba2f22365ef09b1f50edd077240130
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149151"
---
# <a name="required-and-optional-properties"></a>Proprietà obbligatorie e facoltative

Una proprietà è considerata facoltativa se è valida per `null`contenerla. Se `null` non è un valore valido da assegnare a una proprietà, viene considerata una proprietà obbligatoria.

## <a name="conventions"></a>Convenzioni

Per convenzione, una proprietà il cui tipo .NET può contenere null verrà configurata come`string`facoltativa `byte[]`(, `int?`, e così via). Le proprietà il cui tipo CLR non può contenere valori null verranno configurate `bool`come richieste (`int`, `decimal`, e così via).

> [!NOTE]  
> Una proprietà il cui tipo .NET non può contenere null non può essere configurata come facoltativa. La proprietà sarà sempre considerata obbligatoria dal Entity Framework.

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile utilizzare le annotazioni dei dati per indicare che una proprietà è obbligatoria.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per indicare che una proprietà è obbligatoria.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

