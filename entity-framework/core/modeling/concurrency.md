---
title: Token di concorrenza-EF Core
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: db768c1de99000be91d33764ccd3c3924237f8bb
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197460"
---
# <a name="concurrency-tokens"></a>Token di concorrenza

> [!NOTE]
> Questa pagina illustra come configurare i token di concorrenza. Vedere [gestione dei conflitti di concorrenza](../saving/concurrency.md) per una spiegazione dettagliata del funzionamento del controllo della concorrenza in EF core ed esempi di come gestire i conflitti di concorrenza nell'applicazione.

Le proprietà configurate come token di concorrenza vengono usate per implementare il controllo della concorrenza ottimistica.

## <a name="conventions"></a>Convenzioni

Per convenzione, le proprietà non vengono mai configurate come token di concorrenza.

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile usare le annotazioni dei dati per configurare una proprietà come token di concorrenza.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare una proprietà come token di concorrenza.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Versione timestamp/riga

Un timestamp è una proprietà in cui un nuovo valore viene generato dal database ogni volta che viene inserita o aggiornata una riga. Anche la proprietà viene considerata come un token di concorrenza. In questo modo si otterrà un'eccezione se altri utenti hanno modificato una riga che si sta tentando di aggiornare poiché è stata eseguita una query per i dati.

Il modo in cui questa operazione viene eseguita dipende dal provider di database in uso. Per SQL Server, timestamp viene in genere usato in una proprietà *byte []* , che verrà impostata come colonna *rowversion* nel database.

### <a name="conventions"></a>Convenzioni

Per convenzione, le proprietà non vengono mai configurate come timestamp.

### <a name="data-annotations"></a>Annotazioni dei dati

È possibile utilizzare le annotazioni dei dati per configurare una proprietà come timestamp.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare una proprietà come timestamp.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Timestamp.cs#ConfigureTimestampFluent)]
