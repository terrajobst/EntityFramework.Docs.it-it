---
title: Proprietà di tipi di entità - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: b2d72b08de79939904bf4e726c695440c906a8aa
ms.sourcegitcommit: 7bde8e6ad3c4565a4638646ce04bcf5e66f7b5fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/07/2019
ms.locfileid: "54069204"
---
# <a name="owned-entity-types"></a>Tipi di entità di proprietà

>[!NOTE]
> Questa funzionalità è una novità di EF Core 2.0.

EF Core consente di accedere ai tipi di entità del modello che è possono visualizzare unicamente le proprietà di navigazione di altri tipi di entità. Questi sono denominati _tipi di entità di proprietà_. L'entità che contiene un tipo di entità di proprietà è relativa _proprietario_.

## <a name="explicit-configuration"></a>Configurazione esplicita

Proprietà dell'entità tipi non sono mai inclusi da EF Core nel modello per convenzione. È possibile usare la `OwnsOne` nel metodo `OnModelCreating` o annotare il tipo con `OwnedAttribute` (novità di EF Core 2.1) per configurare il tipo come un tipo di proprietà.

In questo esempio `StreetAddress` è un tipo con nessuna proprietà identity. Viene usato come proprietà del tipo Order per specificare l'indirizzo di spedizione per uno specifico ordine.

È possibile usare il `OwnedAttribute` da considerare come un'entità di proprietà quando viene fatto riferimento da un altro tipo di entità:

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

È anche possibile usare la `OwnsOne` metodo nella `OnModelCreating` per specificare che le `ShippingAddress` proprietà è un'entità di proprietà del `Order` tipo di entità e configurare i facet aggiuntivi se necessario.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

Se il `ShippingAddress` tratta di una proprietà privata il `Order` tipo, è possibile usare la versione della stringa il `OwnsOne` metodo:

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

Vedere le [progetto di esempio completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) per altre informazioni sul contesto. 

## <a name="implicit-keys"></a>Chiavi implicite

Configurato con i tipi di proprietà `OwnsOne` o individuati mediante una navigazione di riferimento hanno sempre una relazione uno a uno con il proprietario, pertanto non devono i propri valori di chiave come i valori di chiave esterni siano univoci. Nell'esempio precedente, il `StreetAddress` tipo non è necessario definire una proprietà chiave.  

Per comprendere come EF Core controlla questi oggetti, è utile pensare che una chiave primaria viene creata come un [proprietà shadow](xref:core/modeling/shadow-properties) per il tipo di proprietà. Il valore della chiave di un'istanza del tipo di proprietà sarà identico al valore della chiave dell'istanza del proprietario.

## <a name="collections-of-owned-types"></a>Raccolte di tipi di proprietà

>[!NOTE]
> Questa funzionalità è stata introdotta in EF Core 2.2.

Per configurare una raccolta di tipi di proprietà `OwnsMany` deve essere usato in `OnModelCreating`. Tuttavia la chiave primaria non verrà configurata per convenzione, quindi deve essere specificato in modo esplicito. Accade spesso di usare una chiave complessa per questi tipi di entità che includa la chiave esterna per il proprietario e una proprietà aggiuntiva univoca che può anche essere nello stato shadow:

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a>Mapping dei tipi con la suddivisione di tabelle di proprietà

Quando si utilizza relazionale i database, per riferimento di convenzione di proprietà vengono eseguito il mapping di tipi alla stessa tabella come proprietario. È necessario suddividere la tabella in due parti: alcune colonne da utilizzare per archiviare i dati del proprietario e alcune colonne da utilizzare per archiviare i dati dell'entità di proprietà. Questa è una funzionalità comune nota come la suddivisione di tabelle.

> [!TIP]
> Proprietà tipo archiviato con la suddivisione di tabelle possono essere usati in modo analogo ai tipi complessi come vengono usati in EF6.

Per convenzione, EF Core sarà denominare le colonne del database per le proprietà del tipo di entità di proprietà segue il modello _Navigation_OwnedEntityProperty_. Di conseguenza il `StreetAddress` proprietà verranno visualizzate nella tabella "Ordini" con i nomi 'ShippingAddress_Street' e 'ShippingAddress_City'.

È possibile aggiungere il `HasColumnName` metodo per rinominare le colonne:

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Condividere lo stesso tipo di .NET tra più tipi di proprietà

Un tipo di entità di proprietà possa essere dello stesso tipo .NET come un altro tipo di entità di proprietà, pertanto che potrebbe non essere sufficiente per identificare un tipo di proprietà del tipo .NET.

In questi casi, la proprietà che punta da parte del proprietario per l'entità di proprietà diventa il _definizione di spostamento_ le proprietà del tipo di entità. Dal punto di vista di EF Core, il navigazione che lo definisce è parte dell'identità del tipo con il tipo .NET.   

Ad esempio, nella classe seguente `ShippingAddress` e `BillingAddress` sono entrambi dello stesso tipo, .NET `StreetAddress`:

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Per comprendere come EF Core sarà possibile distinguere le istanze rilevate di questi oggetti, può essere utile pensare che il navigazione che lo definisce è diventata parte della chiave dell'istanza con il valore della chiave del proprietario e il tipo .NET del tipo di proprietà.

## <a name="nested-owned-types"></a>Tipi di proprietà annidati

In questo esempio `OrderDetails` possiede `BillingAddress` e `ShippingAddress`, che sono entrambi `StreetAddress` tipi. Quindi `OrderDetails` è di proprietà del tipo `DetailedOrder`.

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

Oltre ai tipi di proprietà annidati, un tipo di proprietà può fare riferimento a un'entità normale, può essere il proprietario o un'entità diversa, purché l'entità di proprietà sia sul lato dipendente. Questa funzionalità imposta i tipi di entità di proprietà oltre ai tipi complessi in EF6.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

È possibile concatenare il `OwnsOne` metodo in una chiamata fluent per configurare questo modello:

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

È anche possibile ottenere la stessa operazione usando `OwnedAttribute` su entrambi `OrderDetails` e `StreetAdress`.

## <a name="storing-owned-types-in-separate-tables"></a>L'archiviazione dei tipi di proprietà in tabelle distinte

A differenza dei tipi complessi di EF6, anche i tipi di proprietà possono essere archiviati in una tabella separata dal proprietario. Per eseguire l'override la convenzione che esegue il mapping di un tipo di proprietà alla stessa tabella come proprietario, è possibile chiamare semplicemente `ToTable` e specificare un nome di tabella diverso. Nell'esempio seguente verrà eseguito il mapping `OrderDetails` e i relativi indirizzi in una tabella separata da due `DetailedOrder`:

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a>Esecuzione di query su tipi di proprietà

Quando si esegue una query sul proprietario, i tipi di proprietà saranno inclusi per impostazione predefinita. Non è necessario usare il `Include` (metodo), anche se i tipi di proprietà vengono archiviati in una tabella separata. Basati sul modello descritto in precedenza, la query seguente otterrà `Order`, `OrderDetails` e le due proprietà `StreetAddresses` dal database:

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>Limitazioni

Alcune di queste limitazioni sono fondamentali per l'uso di tipi di entità come proprietà, ma alcuni altri sono restrizioni che potrebbe essere in grado di rimuovere nelle future versioni:

### <a name="by-design-restrictions"></a>Restrizioni da Progettazione
- Non è possibile creare un `DbSet<T>` per un tipo di proprietà
- Non è possibile chiamare `Entity<T>()` con un tipo di proprietà in `ModelBuilder`

### <a name="current-shortcomings"></a>Limitazioni correnti
- Gerarchie di ereditarietà che include proprietà non sono supportati i tipi di entità
- Le esplorazioni di riferimento di proprietà i tipi di entità non possono essere null, a meno che queste vengono mappate a una tabella separata in modo esplicito dal proprietario
- Le istanze di tipi di entità di proprietà non possono essere condiviso da più proprietari (si tratta di uno scenario noto per gli oggetti valore non può essere implementato usando tipi di entità di proprietà)

### <a name="shortcomings-in-previous-versions"></a>Limitazioni nelle versioni precedenti
- In EF Core 2.0, le esplorazioni di proprietà di tipi di entità non possono essere dichiarati in tipi di entità derivati, a meno che l'entità di proprietà vengono mappate in modo esplicito a una tabella separata dalla gerarchia di proprietario. Questa limitazione è stata rimossa in EF Core 2.1
- In EF Core 2.0 e 2.1 unico riferimento erano supportate le esplorazioni di tipi di proprietà. Questa limitazione è stata rimossa in EF Core 2.2
