---
title: Ereditarietà-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: abc1caa4d3839b7cdb52b316bcfc8f648b609b70
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655680"
---
# <a name="inheritance"></a>Ereditarietà

L'ereditarietà nel modello EF viene utilizzata per controllare il modo in cui l'ereditarietà nelle classi di entità viene rappresentata nel database.

## <a name="conventions"></a>Convenzioni

Per convenzione, spetta al provider di database determinare il modo in cui l'ereditarietà verrà rappresentata nel database. Vedere [ereditarietà (database relazionale)](relational/inheritance.md) per il modo in cui viene gestita con un provider di database relazionale.

EF imposterà l'ereditarietà solo se due o più tipi ereditati vengono inclusi in modo esplicito nel modello. EF non analizza i tipi di base o derivati che non sono stati altrimenti inclusi nel modello. È possibile includere tipi nel modello esponendo un *DbSet\<tentity >* per ogni tipo nella gerarchia di ereditarietà.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Se non si vuole esporre un *DbSet\<tentity >* per una o più entità nella gerarchia, è possibile usare l'API Fluent per assicurarsi che siano incluse nel modello.
Se non si basano sulle convenzioni, è possibile specificare il tipo di base in modo esplicito utilizzando `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> È possibile utilizzare `.HasBaseType((Type)null)` per rimuovere un tipo di entità dalla gerarchia.

## <a name="data-annotations"></a>Annotazioni dei dati

Non è possibile utilizzare le annotazioni dei dati per configurare l'ereditarietà.

## <a name="fluent-api"></a>API Fluent

L'API Fluent per l'ereditarietà dipende dal provider di database in uso. Vedere [ereditarietà (database relazionale)](relational/inheritance.md) per la configurazione che è possibile eseguire per un provider di database relazionale.
