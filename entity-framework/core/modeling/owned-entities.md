---
title: Tipi di entità di proprietà-EF Core
description: Come configurare i tipi di entità o le aggregazioni di proprietà quando si usa Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/owned-entities
ms.openlocfilehash: da4a459fbc40010fc14190204c8ed66fe0495b84
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416460"
---
# <a name="owned-entity-types"></a>Tipi di entità di proprietà

EF Core consente di modellare i tipi di entità che possono essere visualizzati solo nelle proprietà di navigazione di altri tipi di entità. Questi sono denominati _tipi di entità di proprietà_. L'entità che contiene un tipo di entità di proprietà è il _proprietario_.

Le entità di proprietà sono essenzialmente parte del proprietario e non possono esistere senza di essa, sono concettualmente simili a quelle delle [aggregazioni](https://martinfowler.com/bliki/DDD_Aggregate.html). Ciò significa che l'entità di proprietà è per definizione sul lato dipendente della relazione con il proprietario.

## <a name="explicit-configuration"></a>Configurazione esplicita

I tipi di entità di proprietà non vengono mai inclusi da EF Core nel modello per convenzione. È possibile usare il metodo `OwnsOne` in `OnModelCreating` o annotare il tipo con `OwnedAttribute` (nuovo in EF Core 2,1) per configurare il tipo come tipo di proprietà.

In questo esempio `StreetAddress` è un tipo senza proprietà Identity. Viene usato come proprietà del tipo Order per specificare l'indirizzo di spedizione per uno specifico ordine.

È possibile utilizzare il `OwnedAttribute` per considerarlo come un'entità di proprietà quando viene fatto riferimento da un altro tipo di entità:

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

È anche possibile usare il metodo `OwnsOne` in `OnModelCreating` per specificare che la proprietà `ShippingAddress` è un'entità di proprietà del tipo di entità `Order` e per configurare i facet aggiuntivi, se necessario.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

Se la proprietà `ShippingAddress` è privata nel tipo di `Order`, è possibile usare la versione in formato stringa del metodo `OwnsOne`:

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

Per ulteriori informazioni sul contesto, vedere il [progetto di esempio completo](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) .

## <a name="implicit-keys"></a>Chiavi implicite

I tipi di proprietà configurati con `OwnsOne` o individuati tramite un'esplorazione dei riferimenti hanno sempre una relazione uno-a-uno con il proprietario, pertanto non necessitano di valori di chiave personalizzati perché i valori di chiave esterna sono univoci. Nell'esempio precedente, non è necessario che il tipo di `StreetAddress` definisca una proprietà chiave.  

Per comprendere il modo in cui EF Core tiene traccia di questi oggetti, è utile sapere che una chiave primaria viene creata come [proprietà shadow](xref:core/modeling/shadow-properties) per il tipo di proprietà. Il valore della chiave di un'istanza del tipo di proprietà sarà uguale al valore della chiave dell'istanza del proprietario.

## <a name="collections-of-owned-types"></a>Raccolte di tipi di proprietà

> [!NOTE]
> Questa funzionalità è stata introdotta in EF Core 2.2.

Per configurare una raccolta di tipi di proprietà, utilizzare `OwnsMany` in `OnModelCreating`.

I tipi di proprietà necessitano di una chiave primaria. Se non sono presenti proprietà valide dei candidati sul tipo .NET, EF Core possibile provare a crearne una. Tuttavia, quando i tipi di proprietà vengono definiti tramite una raccolta, non è sufficiente creare una proprietà shadow da utilizzare come chiave esterna nel proprietario e la chiave primaria dell'istanza di proprietà, come per `OwnsOne`: possono essere presenti più istanze di tipo di proprietà per ogni proprietario e, di conseguenza, la chiave del proprietario non è sufficiente per fornire un'identità univoca per ogni istanza di proprietà.

Le due soluzioni più semplici sono:

- Definizione di una chiave primaria surrogata in una nuova proprietà indipendente dalla chiave esterna che punta al proprietario. I valori contenuti devono essere univoci in tutti i proprietari, ad esempio se la {1} padre ha {1}figlio, quindi il {2} padre non può avere {1}figlio, quindi il valore non ha alcun significato intrinseco. Poiché la chiave esterna non fa parte della chiave primaria, i relativi valori possono essere modificati, quindi è possibile spostare un elemento figlio da un elemento padre a un altro, ma in genere viene eseguita la semantica di aggregazione.
- Uso della chiave esterna e di una proprietà aggiuntiva come chiave composta. Il valore aggiuntivo della proprietà deve ora essere univoco solo per un determinato elemento padre (pertanto, se {1} padre ha {1,1} figlio, il {2} padre può ancora avere {2,1}figlio). Rendendo la parte chiave esterna della chiave primaria, la relazione tra il proprietario e l'entità di proprietà diventa non modificabile e riflette meglio la semantica di aggregazione. Per impostazione predefinita, EF Core esegue questa operazione.

In questo esempio verrà usata la classe `Distributor`:

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

Per impostazione predefinita, la chiave primaria utilizzata per il tipo di proprietà a cui si fa riferimento tramite la proprietà di navigazione `ShippingCenters` sarà `("DistributorId", "Id")` dove `"DistributorId"` è l'FK e `"Id"` è un valore di `int` univoco.

Per configurare un'altra chiamata a PK `HasKey`:

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> Prima di EF Core 3,0 metodo `WithOwner()` non esisteva, quindi la chiamata deve essere rimossa. Inoltre, la chiave primaria non è stata individuata automaticamente, quindi è sempre stata specificata.

## <a name="mapping-owned-types-with-table-splitting"></a>Mapping dei tipi di proprietà con suddivisione della tabella

Quando si usano database relazionali, per impostazione predefinita i tipi di proprietà di riferimento vengono mappati alla stessa tabella del proprietario. Questa operazione richiede la suddivisione della tabella in due: alcune colonne verranno utilizzate per archiviare i dati del proprietario e alcune colonne verranno utilizzate per archiviare i dati dell'entità di proprietà. Si tratta di una funzionalità comune nota come [suddivisione della tabella](table-splitting.md).

Per impostazione predefinita, EF Core assegna un nome alle colonne del database per le proprietà del tipo di entità di proprietà che segue il modello _Navigation_OwnedEntityProperty_. Le proprietà `StreetAddress` verranno pertanto visualizzate nella tabella ' Orders ' con i nomi ' ShippingAddress_Street ' è ShippingAddress_City '.

È possibile usare il metodo `HasColumnName` per rinominare le colonne:

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

> [!NOTE]
> La maggior parte dei normali metodi di configurazione del tipo di entità come [Ignore](/dotnet/api/microsoft.entityframeworkcore.metadata.builders.ownednavigationbuilder.ignore) può essere chiamata nello stesso modo.

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Condivisione dello stesso tipo .NET tra più tipi di proprietà

Un tipo di entità di proprietà può essere dello stesso tipo .NET di un altro tipo di entità di proprietà, pertanto il tipo .NET potrebbe non essere sufficiente per identificare un tipo di proprietà.

In questi casi, la proprietà che punta dal proprietario all'entità di proprietà diventa la _navigazione che definisce_ il tipo di entità di proprietà. Dal punto di vista del EF Core, la navigazione di definizione fa parte dell'identità del tipo insieme al tipo .NET.

Ad esempio, nella classe seguente `ShippingAddress` e `BillingAddress` sono entrambi dello stesso tipo .NET, `StreetAddress`:

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Per comprendere in che modo EF Core distinguere le istanze rilevate di questi oggetti, può essere utile pensare che la navigazione di definizione sia diventata parte della chiave dell'istanza insieme al valore della chiave del proprietario e al tipo .NET del tipo di proprietà.

## <a name="nested-owned-types"></a>Tipi di proprietà annidati

In questo esempio `OrderDetails` possiede `BillingAddress` e `ShippingAddress`, che sono entrambi `StreetAddress` tipi. Quindi `OrderDetails` è di proprietà del tipo `DetailedOrder`.

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

Ogni navigazione a un tipo di proprietà definisce un tipo di entità separato con configurazione completamente indipendente.

Oltre ai tipi di proprietà annidati, un tipo di proprietà può fare riferimento a un'entità regolare, può essere il proprietario o un'altra entità purché l'entità di proprietà si trovi sul lato dipendente. Questa funzionalità imposta i tipi di entità di proprietà oltre ai tipi complessi in EF6.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

È possibile concatenare il metodo `OwnsOne` in una chiamata Fluent per configurare questo modello:

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

Si noti la chiamata `WithOwner` utilizzata per configurare la proprietà di navigazione che punta al proprietario. Per configurare una navigazione al tipo di entità Owner che non fa parte della relazione di proprietà `WithOwner()` necessario chiamare senza argomenti.

È possibile ottenere il risultato utilizzando `OwnedAttribute` sia `OrderDetails` che `StreetAddress`.

## <a name="storing-owned-types-in-separate-tables"></a>Archiviazione di tipi di proprietà in tabelle separate

A differenza dei tipi complessi EF6, inoltre, i tipi di proprietà possono essere archiviati in una tabella separata dal proprietario. Per eseguire l'override della convenzione che esegue il mapping di un tipo di proprietà alla stessa tabella del proprietario, è possibile chiamare semplicemente `ToTable` e fornire un nome di tabella diverso. Nell'esempio seguente viene mappato `OrderDetails` e i relativi due indirizzi a una tabella separata da `DetailedOrder`:

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

Per eseguire questa operazione, è anche possibile usare la `TableAttribute`, ma si noti che questa operazione avrà esito negativo se sono presenti più spostamenti al tipo di proprietà, poiché in questo caso più tipi di entità verrebbero mappati alla stessa tabella.

## <a name="querying-owned-types"></a>Esecuzione di query sui tipi di proprietà

Quando si esegue una query sul proprietario, i tipi di proprietà saranno inclusi per impostazione predefinita. Non è necessario utilizzare il metodo `Include`, anche se i tipi di proprietà vengono archiviati in una tabella separata. In base al modello descritto in precedenza, la query seguente otterrà `Order`, `OrderDetails` e le due `StreetAddresses` di proprietà del database:

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>Limitazioni

Alcune di queste limitazioni sono fondamentali per il funzionamento dei tipi di entità di proprietà, ma altre sono restrizioni che potrebbero essere rimosse nelle versioni future:

### <a name="by-design-restrictions"></a>Limitazioni di progettazione

- Non è possibile creare un `DbSet<T>` per un tipo di proprietà
- Non è possibile chiamare `Entity<T>()` con un tipo di proprietà in `ModelBuilder`

### <a name="current-shortcomings"></a>Carenze correnti

- I tipi di entità di proprietà non possono avere gerarchie di ereditarietà
- Le esplorazioni dei riferimenti ai tipi di entità di proprietà non possono essere null a meno che non ne venga eseguito il mapping esplicito a una tabella separata dal proprietario
- Le istanze dei tipi di entità di proprietà non possono essere condivise da più proprietari (si tratta di uno scenario noto per gli oggetti valore che non possono essere implementati con i tipi di entità di proprietà)

### <a name="shortcomings-in-previous-versions"></a>Difetti nelle versioni precedenti

- In EF Core 2,0, le navigazioni nei tipi di entità di proprietà non possono essere dichiarate in tipi di entità derivate, a meno che non sia stato eseguito il mapping esplicito delle entità di proprietà a una tabella separata dalla gerarchia Questa limitazione è stata rimossa in EF Core 2,1
- In EF Core 2,0 e 2,1 sono supportati solo gli spostamenti di riferimento ai tipi di proprietà. Questa limitazione è stata rimossa in EF Core 2,2
