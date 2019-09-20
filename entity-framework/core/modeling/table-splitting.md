---
title: Suddivisione di tabelle-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 684fcfbb66debfd1b89e23c8aaf0a32909378c6b
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149193"
---
# <a name="table-splitting"></a>Suddivisione di tabelle

>[!NOTE]
> Questa funzionalità è una novità di EF Core 2,0.

EF Core consente di eseguire il mapping di due o più entità a una singola riga. Questa operazione è denominata _suddivisione tabelle_ o _condivisione tabella_.

## <a name="configuration"></a>Configurazione

Per utilizzare la suddivisione della tabella è necessario eseguire il mapping dei tipi di entità alla stessa tabella, disporre delle chiavi primarie mappate alle stesse colonne e almeno una relazione configurata tra la chiave primaria di un tipo di entità e un'altra nella stessa tabella.

Uno scenario comune per la suddivisione delle tabelle consiste nell'utilizzare solo un subset delle colonne della tabella per ottenere un livello superiore di prestazioni o incapsulamento.

In questo esempio `Order` rappresenta un subset di `DetailedOrder`.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Oltre alla configurazione richiesta, `Property(o => o.Status).HasColumnName("Status")` `Order.Status`viene chiamato per eseguire il `DetailedOrder.Status` mapping alla stessa colonna di.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> Per ulteriori informazioni sul contesto, vedere il [progetto di esempio completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) .

## <a name="usage"></a>Utilizzo

Il salvataggio e l'esecuzione di query sulle entità tramite la suddivisione delle tabelle vengono eseguite in modo analogo alle altre entità. A partire da EF Core 3,0, il riferimento all'entità dipendente `null`può essere. Se tutte le colonne utilizzate dall'entità dipendente sono `NULL` rappresentate dal database, non verrà creata alcuna istanza per la query. Questa situazione si verifica anche quando tutte le proprietà sono facoltative e `null`impostate su, che potrebbe non essere previsto.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Token di concorrenza

Se uno dei tipi di entità che condividono una tabella include un token di concorrenza, è necessario che sia incluso in tutti gli altri tipi di entità per evitare un valore del token di concorrenza non aggiornato quando viene aggiornata solo una delle entità mappate alla stessa tabella.

Per evitare di esporlo al codice consumer, è possibile crearne uno in stato di ombreggiatura.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]