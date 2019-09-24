---
title: Ereditarietà-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: 1f20c455176b5922364584f8c7688c15a4c3f0f9
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197294"
---
# <a name="inheritance"></a>Ereditarietà

L'ereditarietà nel modello EF viene utilizzata per controllare il modo in cui l'ereditarietà nelle classi di entità viene rappresentata nel database.

## <a name="conventions"></a>Convenzioni

Per convenzione, spetta al provider di database determinare il modo in cui l'ereditarietà verrà rappresentata nel database. Vedere [ereditarietà (database relazionale)](relational/inheritance.md) per il modo in cui viene gestita con un provider di database relazionale.

EF imposterà l'ereditarietà solo se due o più tipi ereditati vengono inclusi in modo esplicito nel modello. EF non analizza i tipi di base o derivati che non sono stati altrimenti inclusi nel modello. È possibile includere tipi nel modello esponendo un *DbSet<TEntity>*  per ogni tipo nella gerarchia di ereditarietà.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Se non si vuole esporre un *DbSet<TEntity>*  per una o più entità nella gerarchia, è possibile usare l'API Fluent per assicurarsi che siano incluse nel modello.
Se non si basano sulle convenzioni, è possibile specificare il tipo di base in `HasBaseType`modo esplicito utilizzando.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> È possibile utilizzare `.HasBaseType((Type)null)` per rimuovere un tipo di entità dalla gerarchia.

## <a name="data-annotations"></a>Annotazioni dei dati

Non è possibile utilizzare le annotazioni dei dati per configurare l'ereditarietà.

## <a name="fluent-api"></a>API Fluent

L'API Fluent per l'ereditarietà dipende dal provider di database in uso. Vedere [ereditarietà (database relazionale)](relational/inheritance.md) per la configurazione che è possibile eseguire per un provider di database relazionale.
