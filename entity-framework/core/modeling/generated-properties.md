---
title: Valori generati - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
uid: core/modeling/generated-properties
ms.openlocfilehash: a3656eb1d2dc79ceead04e3a142a58e8afb3cbce
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996777"
---
# <a name="generated-values"></a>Valori generati

## <a name="value-generation-patterns"></a>Modelli di generazione di valore

Sono disponibili tre modelli di generazione di valore che possono essere utilizzati per le proprietà.

### <a name="no-value-generation"></a>Nessuna generazione di valori

Nessuna generazione di valori significa che verrà sempre fornire un valore valido per essere salvati nel database. Questo valore valido deve essere assegnato a nuove entità prima di essere aggiunti al contesto.

### <a name="value-generated-on-add"></a>Aggiungi valore generato in

Valore generato in aggiungere significa che viene generato un valore per le nuove entità.

A seconda del provider di database in uso, i valori possono essere generati sul lato client da Entity Framework o nel database. Se il valore viene generato dal database, Entity Framework può assegnare un valore temporaneo quando si aggiunge l'entità al contesto. Questo valore temporaneo verrà sostituito dal valore generato dal database durante `SaveChanges()`.

Se si aggiunge un'entità al contesto con un valore assegnato alla proprietà, quindi Entity Framework tenterà di inserire tale valore anziché generarne uno nuovo. Una proprietà viene considerata come un valore assegnato se non viene assegnato il valore predefinito CLR (`null` per `string`, `0` per `int`, `Guid.Empty` per `Guid`e così via.). Per altre informazioni, vedere [valori espliciti per le proprietà generate](../saving/explicit-values-generated-properties.md).

> [!WARNING]  
> Come il valore viene generato per le entità aggiunte dipenderà dal provider del database in uso. I provider di database possono configurare automaticamente la generazione di valori per alcuni tipi di proprietà, ma altri utenti potrebbe essere necessario configurare manualmente il modo in cui il valore viene generato.
>
> Ad esempio, quando si usa SQL Server, i valori verranno automaticamente generati per `GUID` proprietà (tramite l'algoritmo GUID sequenza di SQL Server). Tuttavia, se si specifica che un `DateTime` proprietà viene generata all'aggiunta, quindi è necessario configurare un modo per i valori da generare. Un modo per eseguire questa operazione, consiste nel configurare un valore predefinito pari `GETDATE()`, vedere [valori predefiniti](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Valore generato in aggiunta o l'aggiornamento

Valore generato all'aggiunta o aggiornamento indica che un nuovo valore viene generato ogni volta che viene salvato il record (insert o update).

Ad esempio `value generated on add`, se si specifica un valore per la proprietà in un'istanza di un'entità, che verrà inserito anziché un valore generato valore appena aggiunta. È anche possibile impostare un valore esplicito durante l'aggiornamento. Per altre informazioni, vedere [valori espliciti per le proprietà generate](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> Come il valore viene generato per le entità aggiunte e aggiornate dipenderà dal provider del database in uso. I provider di database possono configurare automaticamente la generazione di valori per alcuni tipi di proprietà, mentre altri sarà necessario configurare manualmente il modo in cui il valore viene generato.
> 
> Ad esempio, quando si usa SQL Server, `byte[]` le proprietà impostate, generata in aggiungere o aggiornare e contrassegnati come token di concorrenza, verrà configurato con il `rowversion` il tipo di dati, in modo che i valori verranno generati nel database. Tuttavia, se si specifica che un `DateTime` proprietà viene generata in aggiunta o l'aggiornamento, quindi è necessario configurare un modo per i valori da generare. Un modo per eseguire questa operazione, consiste nel configurare un valore predefinito pari `GETDATE()` (vedere [valori predefiniti](relational/default-values.md)) per generare valori per le nuove righe. È quindi possibile utilizzare un trigger di database per generare i valori durante gli aggiornamenti (ad esempio, il trigger di esempio seguente).
> 
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Convenzioni

Per convenzione, non composte chiavi primarie del tipo short, int, long o Guid verrà configurato per avere i valori generati su Aggiungi. Tutte le altre proprietà verrà configurato con la generazione di alcun valore.

## <a name="data-annotations"></a>Annotazioni dei dati

### <a name="no-value-generation-data-annotations"></a>Nessuna generazione di valori (annotazioni dei dati)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Valore generato in aggiungere (annotazioni dei dati)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> In questo modo solo di Entity Framework sapere che i valori vengono generati per le entità aggiunte, ciò non garantisce che Entity Framework consente di configurare il meccanismo effettivo per generare valori. Visualizzare [generati sul valore aggiunto da](#value-generated-on-add) sezione per altri dettagli.

### <a name="value-generated-on-add-or-update-data-annotations"></a>Valore generato in aggiunta o aggiornamento (annotazioni dei dati)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> In questo modo solo di Entity Framework sapere che i valori vengono generati per le entità aggiunte o aggiornate, ciò non garantisce che Entity Framework consente di configurare il meccanismo effettivo per generare valori. Visualizzare [valore generato in aggiunta o l'aggiornamento](#value-generated-on-add-or-update) sezione per altri dettagli.

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per modificare il modello di generazione del valore per una determinata proprietà.

### <a name="no-value-generation-fluent-api"></a>Nessuna generazione di valori (API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Valore generato in aggiungere (API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()` semplicemente consente a Entity Framework sapere che i valori vengono generati per le entità aggiunte, ciò non garantisce che Entity Framework consente di configurare il meccanismo effettivo per generare valori.  Visualizzare [generati sul valore aggiunto da](#value-generated-on-add) sezione per altri dettagli.

### <a name="value-generated-on-add-or-update-fluent-api"></a>Valore generato in aggiunta o aggiornamento (API Fluent)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> In questo modo solo di Entity Framework sapere che i valori vengono generati per le entità aggiunte o aggiornate, ciò non garantisce che Entity Framework consente di configurare il meccanismo effettivo per generare valori. Visualizzare [valore generato in aggiunta o l'aggiornamento](#value-generated-on-add-or-update) sezione per altri dettagli.
