---
title: Suddivisione di tabelle-EF Core
description: Come configurare la suddivisione delle tabelle con Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 01/03/2020
uid: core/modeling/table-splitting
ms.openlocfilehash: c38d3ee0efa82f84a1051017ae40c9f3fdd57f1f
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781170"
---
# <a name="table-splitting"></a>Suddivisione di tabelle

EF Core consente di eseguire il mapping di due o più entità a una singola riga. Questa operazione è denominata _suddivisione tabelle_ o _condivisione tabella_.

## <a name="configuration"></a>Configurazione di

Per utilizzare la suddivisione della tabella è necessario eseguire il mapping dei tipi di entità alla stessa tabella, disporre delle chiavi primarie mappate alle stesse colonne e almeno una relazione configurata tra la chiave primaria di un tipo di entità e un'altra nella stessa tabella.

Uno scenario comune per la suddivisione delle tabelle consiste nell'utilizzare solo un subset delle colonne della tabella per ottenere un livello superiore di prestazioni o incapsulamento.

In questo esempio `Order` rappresenta un subset di `DetailedOrder`.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Oltre alla configurazione richiesta, viene chiamato `Property(o => o.Status).HasColumnName("Status")` per eseguire il mapping `DetailedOrder.Status` alla stessa colonna `Order.Status`.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting)]

> [!TIP]
> Per ulteriori informazioni sul contesto, vedere il [progetto di esempio completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) .

## <a name="usage"></a>Usage

Il salvataggio e l'esecuzione di query sulle entità tramite la suddivisione delle tabelle vengono eseguite in modo analogo alle altre entità:

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="optional-dependent-entity"></a>Entità dipendente facoltativa

> [!NOTE]
> Questa funzionalità è stata introdotta in EF Core 3,0.

Se tutte le colonne utilizzate da un'entità dipendente sono `NULL` nel database, non verrà creata alcuna istanza per la query. In questo modo è possibile modellare un'entità dipendente facoltativa, in cui la proprietà relationship nell'entità è null. Si noti che questa situazione si verificherà anche che tutte le proprietà del dipendente sono facoltative e impostate su `null`, che potrebbe non essere previsto.

## <a name="concurrency-tokens"></a>Token di concorrenza

Se uno dei tipi di entità che condividono una tabella include un token di concorrenza, è necessario includerlo anche in tutti gli altri tipi di entità. Questa operazione è necessaria per evitare un valore del token di concorrenza non aggiornato quando viene aggiornata solo una delle entità mappate alla stessa tabella.

Per evitare che il token di concorrenza venga esposto al codice consumer, è possibile crearne uno come [proprietà shadow](xref:core/modeling/shadow-properties):

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
