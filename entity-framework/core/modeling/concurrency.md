---
title: Token di concorrenza - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a>Token di concorrenza

Se una proprietà è configurata come un token di concorrenza EF verificherà che nessun altro utente ha modificato il valore nel database durante il salvataggio delle modifiche al record specifico. Entity Framework Usa un modello di concorrenza ottimistica, ovvero verrà si supponga che il valore non è stato modificato e tenta di salvare i dati, ma genera se rileva che è stato modificato il valore.

Ad esempio potrebbe essere necessario configurare `LastName` su `Person` da un token di concorrenza. Ciò significa che se un utente tenta di salvare alcune modifiche apportate a un `Person`, ma un altro utente ha modificato il `LastName` quindi verrà generata un'eccezione. Potrebbe essere auspicabile in modo che l'applicazione può richiedere all'utente di verificare che il record rappresenta comunque la stessa persona prima di salvare le modifiche.

> [!NOTE]
> Questa pagina viene illustrato come configurare i token di concorrenza. Vedere [la gestione della concorrenza](../saving/concurrency.md) per esempi di come usare concorrenza ottimistica nell'applicazione.

## <a name="how-concurrency-tokens-work-in-ef"></a>Modalità di funzionamento di token di concorrenza in Entity Framework

Archivi dati è possono applicare i token di concorrenza verificando che tutti i record da aggiornare o eliminare ancora ha lo stesso valore per il token di concorrenza assegnato al momento il contesto di caricamento iniziale dei dati dal database.

Ad esempio, database relazionali di ottenere questo risultato, includendo il token di concorrenza nel `WHERE` clausola di qualsiasi `UPDATE` o `DELETE` comandi e il numero di righe che sono stati interessati. Se il token di concorrenza corrisponde ancora verrà aggiornata una riga. Se il valore nel database è stato modificato, non le righe vengono aggiornate.

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="conventions"></a>Convenzioni

Per convenzione, le proprietà non sono configurate come token di concorrenza.

## <a name="data-annotations"></a>Annotazioni dei dati

Per configurare una proprietà come un token di concorrenza, è possibile utilizzare le annotazioni dei dati.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Microsoft Office Fluent API

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

### <a name="fluent-api"></a>Microsoft Office Fluent API

Per configurare una proprietà come un timestamp, è possibile utilizzare l'API Fluent.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
