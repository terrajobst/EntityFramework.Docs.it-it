---
title: "Proprietà di tipi di entità: EF Core"
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: a6823377eb626ca92263c31351e1aef61db5a787
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2018
---
# <a name="owned-entity-types"></a>Tipi di proprietà di entità

>[!NOTE]
> Questa funzionalità è nuova in Entity Framework Core 2.0.

Componenti di base di Entity Framework consente ai tipi di entità di modello che è possono visualizzare solo le proprietà di navigazione di altri tipi di entità. Questi sono denominati _tipi di entità di proprietà_. L'entità che contiene un tipo di proprietà di entità è relativa _proprietario_.

## <a name="explicit-configuration"></a>Configurazione esplicita

Proprietà entità tipi non sono mai inclusi EF core nel modello per convenzione. È possibile utilizzare il `OwnsOne` metodo `OnModelCreating` o annotare il tipo con `OwnedAttrbibute` (nuova in Entity Framework Core 2.1) per configurare il tipo come tipo di proprietà.

In questo esempio StreetAddress è un tipo con nessuna proprietà identity. Viene usato come proprietà del tipo Order per specificare l'indirizzo di spedizione per uno specifico ordine. In `OnModelCreating`, utilizziamo la `OwnsOne` per specificare che la proprietà ShippingAddress è un'entità di proprietà del tipo di ordine.

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

// OnModelCreating
modelBuilder.Entity<Order>().OwnsOne(p => p.ShippingAddress);
```

Se la proprietà ShippingAddress è privata nel tipo di ordine, è possibile utilizzare la versione della stringa di `OwnsOne` metodo:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

In questo esempio, si usa il `OwnedAttribute` per ottenere lo stesso risultato:

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="implicit-keys"></a>Chiavi implicite

In Entity Framework Core 2.0 e 2.1, solo le proprietà di navigazione di riferimento possono puntare a tipi di proprietà. Le raccolte di tipi di proprietà non sono supportate. Questi riferimenti di proprietà tipi hanno sempre una relazione uno a uno con il proprietario, di conseguenza, non è necessario i propri valori di chiave. Nell'esempio precedente, il tipo StreetAddress non è necessario definire una proprietà chiave.  

Per comprendere la modalità di traccia di questi oggetti EF Core, è utile sapere che una chiave primaria viene creata come un [nascondere proprietà](xref:core/modeling/shadow-properties) per il tipo di proprietà. Il valore della chiave di un'istanza del tipo di proprietà sarà identico al valore della chiave dell'istanza del proprietario.      

## <a name="mapping-owned-types-with-table-splitting"></a>Mapping di tipi con la suddivisione di tabella di proprietà

Quando si utilizzano database relazionali, per convenzione, i tipi di proprietà vengono mappate alla stessa tabella come proprietario. È necessario suddividere la tabella in due parti: alcune colonne da utilizzare per archiviare i dati del proprietario e alcune colonne da utilizzare per archiviare i dati dell'entità di proprietà. Questa è una caratteristica comune nota come divisione di tabella.

> [!TIP]
> Proprietà tipi archiviati con la suddivisione di tabella possono essere molto simili ai tipi complessi come vengono usati in EF6.

Per convenzione, Core EF denominerà le colonne del database per le proprietà del tipo di entità proprietà modello di _EntityProperty_OwnedEntityProperty_. Pertanto, le proprietà di StreetAddress verranno visualizzate nella tabella Orders con i nomi ShippingAddress_Street e ShippingAddress_City.

È possibile aggiungere il `HasColumnName` metodo rinominare le colonne. Nel caso in cui StreetAddress è una proprietà pubblica, sarebbe i mapping

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>La condivisione tra più tipi di proprietà lo stesso tipo di .NET

Un tipo di proprietà di entità può essere dello stesso tipo .NET come un altro tipo di proprietà di entità, pertanto che il tipo .NET potrebbe non essere sufficiente per identificare un tipo di proprietà.

In questi casi, la proprietà scegliendo dal proprietario dell'entità di proprietà diventa il _definizione spostamento_ del tipo di proprietà di entità. Dal punto di vista di EF Core, la definizione di spostamento è parte dell'identità del tipo con il tipo .NET.   

Ad esempio, nella classe seguente, ShippingAddress e BillingAddress sono entrambi dello stesso tipo .NET, StreetAddress:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

Per capire come EF Core sarà possibile distinguere rilevate le istanze di questi oggetti, può essere utile pensare che la definizione di spostamento è diventata parte della chiave dell'istanza con il valore della chiave del proprietario e il tipo .NET del tipo di proprietà.

## <a name="nested-owned-types"></a>Proprietà di tipi annidati

In questo esempio OrderDetails proprietario BillingAddress e ShippingAddress, che sono entrambi tipi StreetAddress. Quindi OrderDetails appartiene il tipo di ordine.

``` csharp
public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
    public OrderStatus Status { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

È possibile concatenare il `OwnsOne` metodo in un mapping di Microsoft Office fluent per configurare questo modello:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

È possibile ottenere la stessa cosa utilizzando `OwnedAttribute` OrderDetails sia StreetAdress.

Oltre ai tipi di proprietà annidati, un tipo di proprietà può fare riferimento a un'entità normale. Nell'esempio seguente, Country è un'entità normale (vale a dire non proprietà):

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

Questa funzionalità imposta i tipi di proprietà di entità oltre ai tipi complessi in EF6.

## <a name="storing-owned-types-in-separate-tables"></a>L'archiviazione dei tipi di proprietà in tabelle separate

A differenza dei tipi complessi EF6, tipi di proprietà possono essere archiviati anche in una tabella separata dal proprietario. Per ignorare la convenzione che esegue il mapping alla stessa tabella come proprietario di un tipo di proprietà, è sufficiente chiamare `ToTable` e fornire un nome di tabella diverso. Nell'esempio seguente verrà eseguito il mapping OrderDetails e i relativi due indirizzi in una tabella separata dall'ordine:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a>Esecuzione di query su tipi di proprietà

Quando si esegue una query sul proprietario, i tipi di proprietà saranno inclusi per impostazione predefinita. Non è necessario utilizzare il `Include` (metodo), anche se i tipi di proprietà vengono archiviati in una tabella separata. Basato sul modello descritto in precedenza, la query seguente eseguirà il pull dell'ordine, OrderDetails e le due proprietà StreeAddresses per tutti gli ordini in sospeso dal database:

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>Limitazioni

Ecco alcune limitazioni dei tipi di proprietà di entità. Alcune di queste limitazioni sono fondamentali per l'utilizzo di tipi come proprietà, ma alcuni altri sono restrizioni punto nel tempo che si desidera rimuovere nelle future versioni:

### <a name="current-shortcomings"></a>Molti punti deboli correnti
- Ereditarietà dei tipi di proprietà non è supportata.
- Tipi di proprietà non possono essere fa riferimento una proprietà di navigazione della raccolta
- Poiché utilizzano la suddivisione di tabella per impostazione predefinita di proprietà tipi presentano anche le restrizioni seguenti, a meno che in modo esplicito il mapping a una tabella diversa:
   - Un tipo derivato non può essere proprietario
   - La proprietà di navigazione definizione non può essere impostata su null (ad esempio di proprietà sono sempre necessari tipi nella stessa tabella)

### <a name="by-design-restrictions"></a>Le restrizioni da Progettazione
- Non è possibile creare un `DbSet<T>`
- Non è possibile chiamare `Entity<T>()` con un tipo di proprietà in `ModelBuilder`
