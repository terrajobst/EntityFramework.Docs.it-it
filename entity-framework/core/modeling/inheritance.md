---
title: Ereditarietà - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
ms.technology: entity-framework-core
uid: core/modeling/inheritance
ms.openlocfilehash: f0394bc55dfbfea8277b1ddf898361165dd1fe51
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052781"
---
# <a name="inheritance"></a>Ereditarietà

Ereditarietà nel modello di Entity Framework viene utilizzato per controllare la modalità di rappresentazione di ereditarietà nelle classi di entità nel database.

## <a name="conventions"></a>Convenzioni

Per convenzione, spetta il provider del database per determinare come ereditarietà sarà rappresentato nel database. Vedere [ereditarietà (Database relazionale)](relational/inheritance.md) per questa modalità di gestione con un provider di database relazionale.

EF installerà solo l'ereditarietà se due o più tipi ereditati sono inclusi in modo esplicito nel modello. EF non verrà verificata per i tipi di base o derivati in caso contrario non incluse nel modello. È possibile includere i tipi nel modello esponendo un *DbSet<TEntity>*  per ogni tipo nella gerarchia di ereditarietà.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Se non si desidera esporre un *DbSet<TEntity>*  per una o più entità nella gerarchia, è possibile utilizzare l'API Fluent per verificare che siano incluse nel modello.
E se non si basano sulle convenzioni, è possibile specificare il tipo di base in modo esplicito utilizzando `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> È possibile utilizzare `.HasBaseType((Type)null)` per rimuovere un tipo di entità dalla gerarchia.

## <a name="data-annotations"></a>Annotazioni dei dati

Per configurare l'ereditarietà, è possibile utilizzare le annotazioni dei dati.

## <a name="fluent-api"></a>Microsoft Office Fluent API

L'API Fluent per l'ereditarietà dipende dal provider di database in uso. Vedere [ereditarietà (Database relazionale)](relational/inheritance.md) per la configurazione è possibile eseguire per un provider di database relazionale.
