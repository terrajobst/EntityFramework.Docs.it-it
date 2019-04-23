---
title: Proprietà obbligatorie o facoltative - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 564d9e62e2ed4f1a52b569630ed4994529e31dc1
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929810"
---
# <a name="required-and-optional-properties"></a>Proprietà obbligatorie e facoltative

Una proprietà è considerata facoltativa se è valido per contenere `null`. Se `null` non è un valore valido da assegnare a una proprietà, quindi viene considerato come una proprietà obbligatoria.

## <a name="conventions"></a>Convenzioni

Per convenzione, una proprietà il cui tipo CLR può contenere null verrà configurata come facoltativi (`string`, `int?`, `byte[]`e così via.). Le proprietà il cui tipo CLR non può contenere null verranno configurate come richiesto (`int`, `decimal`, `bool`e così via.).

> [!NOTE]  
> Una proprietà il cui tipo CLR non può contenere null non possa essere configurata come facoltativi. La proprietà verrà considerata sempre richiesti da Entity Framework.

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile usare le annotazioni dei dati per indicare che la proprietà è obbligatoria.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per indicare che la proprietà è obbligatoria.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

