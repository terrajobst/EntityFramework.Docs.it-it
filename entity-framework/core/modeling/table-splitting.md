---
title: Table Splitting - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 4a0bfaf017106a0bfdff084b1c472bdc17459a89
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562587"
---
# <a name="table-splitting"></a>Suddivisione di tabelle

>[!NOTE]
> Questa funzionalità è una novità di EF Core 2.0.

EF Core consente di eseguire il mapping di due o più entità a una singola riga. Questa operazione viene definita _tabella suddivisione_ oppure _tabella condivisione_.

## <a name="configuration"></a>Configurazione

Per usare la suddivisione di tabelle, che i tipi di entità devono essere mappati alla stessa tabella, sono le chiavi primarie mappate alle stesse colonne e almeno una relazione tra chiave primaria di un tipo di entità e un'altra nella stessa tabella configurata.

Uno scenario comune per la suddivisione di tabelle utilizza solo un subset delle colonne nella tabella per maggiore sulle prestazioni o l'incapsulamento.

In questo esempio `Order` rappresenta un subset di `DetailedOrder`.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Oltre alla configurazione necessaria chiamiamo `HasBaseType((string)null)` per evitare di mapping `DetailedOrder` nella stessa gerarchia `Order`.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

Vedere le [progetto di esempio completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) per altre informazioni sul contesto.

## <a name="usage"></a>Utilizzo

Salvataggio e l'esecuzione di query su entità tramite la suddivisione di tabelle viene eseguita nello stesso modo di altre entità, l'unica differenza è che è necessario tenere traccia di tutte le entità la condivisione di una riga per l'inserimento.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Token di concorrenza

Se uno dei tipi di entità la condivisione di una tabella contiene un token di concorrenza quindi deve essere incluso in tutti gli altri tipi di entità per evitare un valore del token di concorrenza non aggiornato quando viene aggiornata solo una delle entità mappata alla stessa tabella.

Per evitare di esporli al codice consumer è possibile crearne uno in stato di ombreggiatura.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]