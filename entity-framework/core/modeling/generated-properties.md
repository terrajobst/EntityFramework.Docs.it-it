---
title: Valori generati - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
ms.technology: entity-framework-core
uid: core/modeling/generated-properties
ms.openlocfilehash: 2d79bf1339ebe522c39fe8971d908c30e1f4dca0
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="generated-values"></a>Valori generati

## <a name="value-generation-patterns"></a>Valore di generazione dei modelli

Esistono tre modelli di generazione di valore che possono essere usati per le proprietà.

### <a name="no-value-generation"></a>Nessuna generazione di valore

La generazione di valori non significa che si fornirà sempre un valore valido per essere salvato nel database. Questo valore valido deve essere assegnato alle nuove entità prima di essere aggiunti al contesto.

### <a name="value-generated-on-add"></a>Aggiungere valore generato in

Valore generato in add significa che viene generato un valore per le nuove entità.

A seconda del provider di database in uso, i valori possono essere generate sul lato client da Entity Framework o nel database. Se il valore viene generato dal database, Entity Framework può assegnare un valore temporaneo quando si aggiunge l'entità al contesto. Verrà sostituito dal valore generato dal database durante questo tipo di valore `SaveChanges()`.

Se si aggiunge un'entità al contesto con un valore assegnato alla proprietà, Entity Framework tenterà di inserire tale valore anziché generarne uno nuovo. Una proprietà viene considerata come un valore assegnato se non viene assegnato il valore predefinito CLR (`null` per `string`, `0` per `int`, `Guid.Empty` per `Guid`, ecc.). Per ulteriori informazioni, vedere [valori espliciti per le proprietà generate](..\saving\explicit-values-generated-properties.md).

> [!WARNING]  
> Come il valore viene generato per le entità aggiunte dipenderà dal provider di database in uso. Provider di database possono configurare automaticamente la generazione di valori per alcuni tipi di proprietà, ma altri potrebbe essere necessario configurare manualmente la modalità con cui il valore viene generato.
>
> Ad esempio, quando si utilizza SQL Server, sarà possibile generare automaticamente valori per `GUID` proprietà (mediante l'algoritmo GUID sequenza di SQL Server). Tuttavia, se si specifica che un `DateTime` proprietà viene generata all'aggiunta, quindi è necessario impostare un modo per i valori da generare. Per a tale scopo, è possibile configurare un valore predefinito di `GETDATE()`, vedere [i valori predefiniti](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Valore generato in aggiunta o l'aggiornamento

Valore generato all'aggiunta o aggiornamento indica che un nuovo valore viene generato ogni volta che il record viene salvato (insert o update).

Ad esempio `value generated on add`, se si specifica un valore per la proprietà in un'istanza di un'entità, che verrà inserito anziché un valore in corso la generazione valore appena aggiunta. È inoltre possibile impostare un valore esplicito durante l'aggiornamento. Per ulteriori informazioni, vedere [valori espliciti per le proprietà generate](..\saving\explicit-values-generated-properties.md).

> [!WARNING]  
> Come il valore viene generato per le entità aggiunte e aggiornate dipenderà dal provider di database in uso. Provider di database possono automaticamente del programma di installazione la generazione di valori per alcuni tipi di proprietà, mentre altri sarà necessario configurare manualmente la modalità con cui il valore viene generato.
>
> Ad esempio, quando si utilizza SQL Server, `byte[]` le proprietà impostate come generato nell'aggiungere o aggiornare e contrassegnati come token di concorrenza, verrà configurato con il `rowversion` il tipo di dati, in modo che i valori verranno generati nel database. Tuttavia, se si specifica che un `DateTime` proprietà viene generata in aggiunta o l'aggiornamento, quindi è necessario impostare un modo per i valori da generare. Per a tale scopo, è possibile configurare un valore predefinito di `GETDATE()` (vedere [i valori predefiniti](relational/default-values.md)) per generare i valori per le nuove righe. È quindi possibile utilizzare un trigger di database per generare i valori durante gli aggiornamenti (ad esempio, il trigger di esempio seguente).
>
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Convenzioni

Per convenzione, le chiavi primarie di un intero o un tipo di dati GUID verrà configurato i valori vengono generati su Aggiungi. Tutte le altre proprietà verrà configurato con la generazione di alcun valore.

## <a name="data-annotations"></a>Annotazioni dei dati

### <a name="no-value-generation-data-annotations"></a>Nessuna generazione di valore (le annotazioni dei dati)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Valore generato in aggiungere (le annotazioni dei dati)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> In questo modo solo di EF che vengono generati valori per le entità aggiunte, non garantisce che EF installerà il meccanismo effettivo per generare valori. Vedere [aggiungere valore generato in](#value-generated-on-add) sezione per ulteriori dettagli.

### <a name="value-generated-on-add-or-update-data-annotations"></a>Valore generato in aggiunta o aggiornamento (le annotazioni dei dati)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Ciò consente solo di EF che vengono generati valori per le entità aggiunte o aggiornate, non garantisce che EF installerà il meccanismo effettivo per generare valori. Vedere [valore generato in aggiunta o aggiornamento](#value-generated-on-add-or-update) sezione per ulteriori dettagli.

## <a name="fluent-api"></a>Microsoft Office Fluent API

È possibile utilizzare l'API Fluent per cambiare il modello di generazione del valore per una determinata proprietà.

### <a name="no-value-generation-fluent-api"></a>Nessuna generazione di valore (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Valore generato in aggiungere (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()`consente solo EF che vengono generati valori per le entità aggiunte, non garantisce che EF installerà il meccanismo effettivo per generare valori.  Vedere [aggiungere valore generato in](#value-generated-on-add) sezione per ulteriori dettagli.

### <a name="value-generated-on-add-or-update-fluent-api"></a>Valore generato in aggiunta o aggiornamento (Fluent API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Ciò consente solo di EF che vengono generati valori per le entità aggiunte o aggiornate, non garantisce che EF installerà il meccanismo effettivo per generare valori. Vedere [valore generato in aggiunta o aggiornamento](#value-generated-on-add-or-update) sezione per ulteriori dettagli.
