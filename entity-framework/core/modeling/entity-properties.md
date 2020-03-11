---
title: Proprietà entità-EF Core
description: Come configurare e mappare le proprietà di un'entità usando Entity Framework Core
author: roji
ms.date: 12/10/2019
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/entity-properties
ms.openlocfilehash: b67603fbffd1f1c8506bc21f8972c851eb8eef29
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417216"
---
# <a name="entity-properties"></a>Proprietà delle entità

Ogni tipo di entità nel modello dispone di un set di proprietà, che EF Core leggerà e scriverà dal database. Se si usa un database relazionale, le proprietà dell'entità vengono mappate alle colonne della tabella.

## <a name="included-and-excluded-properties"></a>Proprietà incluse ed escluse

Per convenzione, tutte le proprietà pubbliche con un getter e un setter verranno incluse nel modello.

Le proprietà specifiche possono essere escluse come segue:

### <a name="data-annotations"></a>[Annotazioni dei dati](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?name=IgnoreProperty&highlight=6)]

### <a name="fluent-api"></a>[API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?name=IgnoreProperty&highlight=3,4)]

***

## <a name="column-names"></a>Nome colonna

Per convenzione, quando si utilizza un database relazionale, viene eseguito il mapping delle proprietà delle entità alle colonne della tabella con lo stesso nome della proprietà.

Se si preferisce configurare le colonne con nomi diversi, è possibile procedere come segue:

### <a name="data-annotations"></a>[Annotazioni dei dati](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnName.cs?Name=ColumnName&highlight=3)]

### <a name="fluent-api"></a>[API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnName.cs?Name=ColumnName&highlight=3-5)]

***

## <a name="column-data-types"></a>Tipi di dati delle colonne

Quando si utilizza un database relazionale, il provider di database seleziona un tipo di dati basato sul tipo .NET della proprietà. Prende in considerazione anche altri metadati, ad esempio la [lunghezza massima](#maximum-length)configurata, se la proprietà fa parte di una chiave primaria e così via.

Ad esempio, SQL Server esegue il mapping di `DateTime` proprietà alle colonne `datetime2(7)` e `string` proprietà a `nvarchar(max)` colonne (o a `nvarchar(450)` per le proprietà utilizzate come chiave).

È inoltre possibile configurare le colonne in modo da specificare un tipo di dati esatto per una colonna. Il codice seguente, ad esempio, consente di configurare `Url` come stringa non Unicode con lunghezza massima `200` e `Rating` come decimale con precisione `5` e scala di `2`:

### <a name="data-annotations"></a>[Annotazioni dei dati](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnDataType.cs?name=ColumnDataType&highlight=4,6)]

### <a name="fluent-api"></a>[API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnDataType.cs?name=ColumnDataType&highlight=5-6)]

***

### <a name="maximum-length"></a>Lunghezza massima

La configurazione di una lunghezza massima fornisce un suggerimento al provider di database sul tipo di dati della colonna appropriato da scegliere per una determinata proprietà. La lunghezza massima si applica solo ai tipi di dati della matrice, ad esempio `string` e `byte[]`.

> [!NOTE]
> Entity Framework non esegue alcuna convalida della lunghezza massima prima di passare i dati al provider. È necessario che il provider o l'archivio dati venga convalidato se appropriato. Se, ad esempio, la destinazione è SQL Server, il superamento della lunghezza massima provocherà un'eccezione poiché il tipo di dati della colonna sottostante non consentirà l'archiviazione di dati in eccesso.

Nell'esempio seguente, la configurazione di una lunghezza massima di 500 causerà la creazione di una colonna di tipo `nvarchar(500)` in SQL Server:

#### <a name="data-annotations"></a>[Annotazioni dei dati](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?name=MaxLength&highlight=4)]

#### <a name="fluent-api"></a>[API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?name=MaxLength&highlight=3-5)]

***

## <a name="required-and-optional-properties"></a>Proprietà obbligatorie e facoltative

Una proprietà è considerata facoltativa se è valida affinché contenga `null`. Se `null` non è un valore valido da assegnare a una proprietà, viene considerata una proprietà obbligatoria. Quando si esegue il mapping a uno schema di database relazionale, le proprietà obbligatorie vengono create come colonne che non ammettono i valori null e le proprietà facoltative vengono create come colonne nullable.

### <a name="conventions"></a>Convenzioni

Per convenzione, una proprietà il cui tipo .NET può contenere null verrà configurato come facoltativo, mentre le proprietà il cui tipo .NET non può contenere valori null verranno configurate in base alle esigenze. Tutte le proprietà con tipi di valore .NET (`int`, `decimal`, `bool`e così via), ad esempio, sono configurate come obbligatorie e tutte le proprietà con tipi di valore .NET Nullable (`int?`, `decimal?`, `bool?`e così via) sono configurate come facoltative.

C#8 è stata introdotta una nuova funzionalità denominata [tipi di riferimento Nullable](/dotnet/csharp/tutorials/nullable-reference-types), che consente di aggiungere annotazioni ai tipi di riferimento, indicando se è valido per consentirne l'inclusione null. Questa funzionalità è disabilitata per impostazione predefinita e, se abilitata, modifica il comportamento del EF Core nel modo seguente:

* Se i tipi di riferimento nullable sono disabilitati (impostazione predefinita), tutte le proprietà con i tipi di riferimento .NET vengono configurate come facoltative per convenzione, ad esempio `string`.
* Se sono abilitati i tipi di riferimento Nullable, le proprietà verranno configurate in base al supporto di valori null del tipo .NET: `string?` verranno configurati come facoltativi, mentre `string` verranno configurati in base alle C# esigenze.

Nell'esempio seguente viene illustrato un tipo di entità con proprietà obbligatorie e facoltative, con la funzionalità di riferimento Nullable disabilitata (impostazione predefinita) e abilitata:

#### <a name="without-nullable-reference-types-default"></a>[Senza tipi di riferimento Nullable (impostazione predefinita)](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

#### <a name="with-nullable-reference-types"></a>[Con tipi di riferimento Nullable](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

L'uso di tipi di riferimento Nullable è consigliato perché trasmette il supporto di C# valori null espresso nel codice al modello di EF core e al database e evita l'uso dell'API Fluent o delle annotazioni dei dati per esprimere lo stesso concetto due volte.

> [!NOTE]
> Prestare attenzione quando si abilitano i tipi di riferimento nullable in un progetto esistente: le proprietà del tipo di riferimento che in precedenza erano configurate come facoltative ora verranno configurate come richieste, a meno che non vengano annotate in modo esplicito come Nullable. Quando si gestisce uno schema di database relazionale, è possibile che vengano generate migrazioni che modificano il supporto di valori null per la colonna di database.

Per ulteriori informazioni sui tipi di riferimento nullable e su come utilizzarli con EF Core, [vedere la pagina della documentazione dedicata per questa funzionalità](xref:core/miscellaneous/nullable-reference-types).

### <a name="explicit-configuration"></a>Configurazione esplicita

Una proprietà che sarebbe facoltativa per convenzione può essere configurata in modo da essere obbligatoria come indicato di seguito:

#### <a name="data-annotations"></a>[Annotazioni dei dati](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?name=Required&highlight=4)]

#### <a name="fluent-api"></a>[API Fluent](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?name=Required&highlight=3-5)]

***
