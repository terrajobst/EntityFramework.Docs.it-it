---
title: Ereditarietà-EF Core
description: Come configurare l'ereditarietà del tipo di entità usando Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 4d43a432174c92ab7f3f9d78a234aefb0a4a17e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824672"
---
# <a name="inheritance"></a>Ereditarietà

L'ereditarietà nel modello EF viene utilizzata per controllare il modo in cui l'ereditarietà nelle classi di entità viene rappresentata nel database.

## <a name="conventions"></a>Convenzioni

Per impostazione predefinita, spetta al provider di database determinare il modo in cui l'ereditarietà verrà rappresentata nel database. Vedere [ereditarietà (database relazionale)](relational/inheritance.md) per il modo in cui viene gestita con un provider di database relazionale.

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
