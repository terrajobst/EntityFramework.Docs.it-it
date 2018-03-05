---
title: Token di concorrenza - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: f3cf28d5c54e63aa76058e9fe1d9f3de5b37d579
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2018
---
# <a name="concurrency-tokens"></a>Token di concorrenza

> [!NOTE]
> Questa pagina viene illustrato come configurare i token di concorrenza. Vedere [gestione dei conflitti di concorrenza](../saving/concurrency.md) per una spiegazione dettagliata del funzionamento di controllo della concorrenza in EF Core ed esempi di come gestire i conflitti di concorrenza nell'applicazione.

Proprietà configurata come token di concorrenza vengono utilizzate per implementare il controllo della concorrenza ottimistica.

## <a name="conventions"></a>Convenzioni

Per convenzione, le proprietà non sono configurate come token di concorrenza.

## <a name="data-annotations"></a>Annotazioni dei dati

Per configurare una proprietà come un token di concorrenza, è possibile utilizzare le annotazioni dei dati.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>API Fluent

Per configurare una proprietà come un token di concorrenza, è possibile utilizzare l'API Fluent.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Versione di timestamp o la riga

Un timestamp è una proprietà in cui viene generato un nuovo valore dal database ogni volta che una riga viene inserita o aggiornata. La proprietà viene anche considerata come un token di concorrenza. In questo modo che si otterrà un'eccezione se qualsiasi altro utente ha modificato una riga che si sta tentando di aggiornare poiché si esegue una query per i dati.

La procedura è compito del provider di database usato. Per SQL Server, i timestamp in genere è utilizzato in un *byte []* programma di installazione come proprietà, che sarà un *ROWVERSION* colonna nel database.

### <a name="conventions"></a>Convenzioni

Per convenzione, le proprietà non sono configurate come timestamp.

### <a name="data-annotations"></a>Annotazioni dei dati

Per configurare una proprietà come un timestamp, è possibile utilizzare le annotazioni dei dati.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>API Fluent

Per configurare una proprietà come un timestamp, è possibile utilizzare l'API Fluent.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
