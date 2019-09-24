---
title: Valori generati-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
uid: core/modeling/generated-properties
ms.openlocfilehash: 6b38fd2e540ec29674f1116e7c204052d06ca1bc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197433"
---
# <a name="generated-values"></a>Valori generati

## <a name="value-generation-patterns"></a>Modelli di generazione di valori

Sono disponibili tre modelli di generazione dei valori che possono essere usati per le proprietà:
* Nessuna generazione di valori
* Valore generato in aggiunta
* Valore generato durante l'aggiunta o l'aggiornamento

### <a name="no-value-generation"></a>Nessuna generazione di valori

Nessuna generazione di valori indica che verrà sempre fornito un valore valido da salvare nel database. Questo valore valido deve essere assegnato a nuove entità prima che vengano aggiunte al contesto.

### <a name="value-generated-on-add"></a>Valore generato in aggiunta

Il valore generato durante l'aggiunta indica che viene generato un valore per le nuove entità.

A seconda del provider di database in uso, è possibile che i valori vengano generati lato client da EF o nel database. Se il valore viene generato dal database, EF può assegnare un valore temporaneo quando si aggiunge l'entità al contesto. Questo valore temporaneo verrà quindi sostituito dal valore generato dal database durante `SaveChanges()`.

Se si aggiunge un'entità al contesto a cui è assegnato un valore assegnato alla proprietà, EF tenterà di inserire tale valore anziché generarne uno nuovo. A una proprietà viene assegnato un valore se non viene assegnato il valore predefinito CLR (`null` `0` per `Guid.Empty` `string` `int`, per, per `Guid`e così via). Per ulteriori informazioni, vedere [valori espliciti per le proprietà generate](../saving/explicit-values-generated-properties.md).

> [!WARNING]  
> Il modo in cui il valore viene generato per le entità aggiunte dipenderà dal provider di database in uso. I provider di database possono impostare automaticamente la generazione del valore per alcuni tipi di proprietà, mentre altri potrebbero richiedere di configurare manualmente il modo in cui viene generato il valore.
>
> Quando si utilizza SQL Server, ad esempio, i valori vengono generati automaticamente `GUID` per le proprietà (usando l'algoritmo GUID sequenziale SQL Server). Tuttavia, se si specifica che una `DateTime` proprietà viene generata in aggiunta, è necessario configurare una modalità per la generazione dei valori. A tale scopo, è possibile configurare un valore predefinito di `GETDATE()`, vedere [valori predefiniti](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Valore generato durante l'aggiunta o l'aggiornamento

Il valore generato durante l'aggiunta o l'aggiornamento indica che viene generato un nuovo valore ogni volta che il record viene salvato (inserimento o aggiornamento).

Analogamente `value generated on add`, se si specifica un valore per la proprietà in un'istanza appena aggiunta di un'entità, tale valore verrà inserito anziché un valore generato. È anche possibile impostare un valore esplicito durante l'aggiornamento. Per ulteriori informazioni, vedere [valori espliciti per le proprietà generate](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> Il modo in cui il valore viene generato per le entità aggiunte e aggiornate dipenderà dal provider di database in uso. I provider di database possono impostare automaticamente la generazione del valore per alcuni tipi di proprietà, mentre altri richiederanno di configurare manualmente il modo in cui viene generato il valore.
> 
> Quando si utilizza SQL Server, ad esempio `byte[]` , le proprietà impostate come generate in Aggiungi o aggiorna e contrassegnate come token di concorrenza verranno impostate con il `rowversion` tipo di dati, in modo che i valori vengano generati nel database. Tuttavia, se si specifica che una `DateTime` proprietà viene generata durante l'aggiunta o l'aggiornamento, è necessario configurare una modalità per la generazione dei valori. A tale scopo, è possibile configurare un valore predefinito di `GETDATE()` (vedere [valori predefiniti](relational/default-values.md)) per generare valori per le nuove righe. È quindi possibile usare un trigger di database per generare valori durante gli aggiornamenti, ad esempio il trigger di esempio seguente.
> 
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Convenzioni

Per convenzione, le chiavi primarie non composite di tipo short, int, Long o GUID verranno impostate in modo da generare valori in Aggiungi. Tutte le altre proprietà verranno impostate senza generazione di valori.

## <a name="data-annotations"></a>Annotazioni dei dati

### <a name="no-value-generation-data-annotations"></a>Nessuna generazione di valori (annotazioni dati)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Valore generato in aggiunta (annotazioni dati)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> In questo modo EF sa che i valori vengono generati per le entità aggiunte, non garantisce che EF configurerà il meccanismo effettivo per generare valori. Per altri dettagli, vedere [valore generato nella sezione Aggiungi](#value-generated-on-add) .

### <a name="value-generated-on-add-or-update-data-annotations"></a>Valore generato durante l'aggiunta o l'aggiornamento (annotazioni dei dati)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> In questo modo, EF sa che i valori vengono generati per le entità aggiunte o aggiornate, ma non garantisce che EF configurerà il meccanismo effettivo per generare valori. Per ulteriori informazioni, vedere la sezione [valore generato nella sezione Aggiungi o aggiorna](#value-generated-on-add-or-update) .

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per modificare il modello di generazione del valore per una determinata proprietà.

### <a name="no-value-generation-fluent-api"></a>Nessuna generazione di valori (API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Valore generato all'aggiunta (API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()`è sufficiente che EF sappia che i valori vengono generati per le entità aggiunte, non garantisce che EF configurerà il meccanismo effettivo per generare valori.  Per altri dettagli, vedere [valore generato nella sezione Aggiungi](#value-generated-on-add) .

### <a name="value-generated-on-add-or-update-fluent-api"></a>Valore generato durante l'aggiunta o l'aggiornamento (API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> In questo modo, EF sa che i valori vengono generati per le entità aggiunte o aggiornate, ma non garantisce che EF configurerà il meccanismo effettivo per generare valori. Per ulteriori informazioni, vedere la sezione [valore generato nella sezione Aggiungi o aggiorna](#value-generated-on-add-or-update) .
