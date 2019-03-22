---
title: Ereditarietà - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: f6b5c8f5a398ac1e28e29bc17f0674c5b76d7b20
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319127"
---
# <a name="inheritance"></a>Ereditarietà

Ereditarietà nel modello di Entity Framework viene utilizzato per controllare la modalità di rappresentazione di ereditarietà nelle classi di entità nel database.

## <a name="conventions"></a>Convenzioni

Per convenzione, spetta il provider di database per determinare come ereditarietà verrà rappresentata nel database. Visualizzare [ereditarietà (Database relazionale)](relational/inheritance.md) per come questa operazione viene gestita con un provider di database relazionale.

EF installerà solo l'ereditarietà se due o più tipi ereditati sono inclusi in modo esplicito nel modello. EF non analizza per i tipi di base o derivati in caso contrario, non incluse nel modello. È possibile includere i tipi nel modello tramite l'esposizione di un *DbSet<TEntity>*  per ogni tipo nella gerarchia di ereditarietà.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Se non si vuole esporre una *DbSet<TEntity>*  per una o più entità nella gerarchia, è possibile usare l'API Fluent per assicurare che siano incluse nel modello.
E se non si basano sulle convenzioni, è possibile specificare il tipo di base in modo esplicito usando `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> È possibile usare `.HasBaseType((Type)null)` per rimuovere un tipo di entità dalla gerarchia.

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile usare le annotazioni dei dati per configurare l'ereditarietà.

## <a name="fluent-api"></a>API Fluent

L'API Fluent per ereditarietà dipende il provider di database in che uso. Visualizzare [ereditarietà (Database relazionale)](relational/inheritance.md) per la configurazione è possibile eseguire per un provider di database relazionale.
