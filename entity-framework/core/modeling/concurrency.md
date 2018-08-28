---
title: Token di concorrenza - EF Core
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: 0051d416544a11385f99d36e45843c5b20725af7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994226"
---
# <a name="concurrency-tokens"></a>Token di concorrenza

> [!NOTE]
> Questa pagina illustra come configurare i token di concorrenza. Visualizzare [alla gestione dei conflitti di concorrenza](../saving/concurrency.md) per una spiegazione dettagliata del funzionamento di controllo della concorrenza in EF Core ed esempi su come gestire i conflitti di concorrenza nell'applicazione.

Proprietà configurata come token di concorrenza vengono usate per implementare il controllo della concorrenza ottimistica.

## <a name="conventions"></a>Convenzioni

Per convenzione, le proprietà mai sono configurate come token di concorrenza.

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile usare le annotazioni dei dati per configurare una proprietà come un token di concorrenza.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare una proprietà come un token di concorrenza.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Versione di riga/timestamp

Un timestamp è una proprietà in cui viene generato un nuovo valore dal database ogni volta che una riga viene inserita o aggiornata. La proprietà viene anche considerata come un token di concorrenza. In questo modo che si otterrà un'eccezione se altri utenti ha modificato una riga che si sta tentando di aggiornare dopo si esegue una query per i dati.

Come si ottiene questo risultato è stabilite dal provider di database in uso. Per SQL Server, timestamp viene in genere usato in un *byte[].{0* proprietà, che sarà di installazione come una *ROWVERSION* colonna nel database.

### <a name="conventions"></a>Convenzioni

Per convenzione, le proprietà non sono mai configurate come timestamp.

### <a name="data-annotations"></a>Annotazioni dei dati

È possibile usare le annotazioni dei dati per configurare una proprietà come un timestamp.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare una proprietà come un timestamp.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
