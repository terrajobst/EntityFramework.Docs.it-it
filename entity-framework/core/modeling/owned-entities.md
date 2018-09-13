---
title: Proprietà di tipi di entità - EF Core
author: julielerman
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: 1104a8a9a4540e33624fad69c47f2f950c6669bf
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489414"
---
# <a name="owned-entity-types"></a>Tipi di entità di proprietà

>[!NOTE]
> Questa funzionalità è una novità di EF Core 2.0.

EF Core consente di accedere ai tipi di entità del modello che è possono visualizzare unicamente le proprietà di navigazione di altri tipi di entità. Questi sono denominati _tipi di entità di proprietà_. L'entità che contiene un tipo di entità di proprietà è relativa _proprietario_.

## <a name="explicit-configuration"></a>Configurazione esplicita

Proprietà dell'entità tipi non sono mai inclusi da EF Core nel modello per convenzione. È possibile usare la `OwnsOne` nel metodo `OnModelCreating` o annotare il tipo con `OwnedAttribute` (novità di EF Core 2.1) per configurare il tipo come un tipo di proprietà.

In questo esempio StreetAddress è un tipo con nessuna proprietà identity. Viene usato come proprietà del tipo Order per specificare l'indirizzo di spedizione per uno specifico ordine. Nelle `OnModelCreating`, viene usato il `OwnsOne` metodo per specificare che la proprietà ShippingAddress è un'entità di proprietà del tipo di ordine.

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

Se la proprietà ShippingAddress è privata nel tipo di ordine, è possibile usare la versione della stringa di `OwnsOne` metodo:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

In questo esempio, utilizziamo il `OwnedAttribute` per raggiungere lo stesso scopo:

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

In EF Core 2.0 e 2.1, solo le proprietà di navigazione di riferimento possono puntare a tipi di proprietà. Raccolte di tipi di proprietà non sono supportate. Questi riferimenti di proprietà tipi hanno sempre una relazione uno a uno con il proprietario, pertanto non necessitano i propri valori di chiave. Nell'esempio precedente, il tipo StreetAddress non necessario definire una proprietà chiave.  

Per comprendere come EF Core controlla questi oggetti, è utile pensare che una chiave primaria viene creata come un [proprietà shadow](xref:core/modeling/shadow-properties) per il tipo di proprietà. Il valore della chiave di un'istanza del tipo di proprietà sarà identico al valore della chiave dell'istanza del proprietario.      

## <a name="mapping-owned-types-with-table-splitting"></a>Mapping dei tipi con la suddivisione di tabelle di proprietà

Quando si usano database relazionali, per convenzione, i tipi di proprietà vengono mappati alla stessa tabella come proprietario. È necessario suddividere la tabella in due parti: alcune colonne da utilizzare per archiviare i dati del proprietario e alcune colonne da utilizzare per archiviare i dati dell'entità di proprietà. Questa è una funzionalità comune nota come la suddivisione di tabelle.

> [!TIP]
> Proprietà tipo archiviato con la suddivisione di tabelle possono essere usati in modo molto simile ai tipi complessi come vengono usati in EF6.

Per convenzione, EF Core sarà denominare le colonne del database per le proprietà del tipo di entità di proprietà segue il modello _EntityProperty_OwnedEntityProperty_. Di conseguenza la proprietà StreetAddress verranno visualizzate nella tabella Orders con i nomi ShippingAddress_Street e ShippingAddress_City.

È possibile aggiungere il `HasColumnName` metodo per rinominare le colonne. Nel caso in cui StreetAddress è una proprietà pubblica, i mapping sono

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Condividere lo stesso tipo di .NET tra più tipi di proprietà

Un tipo di entità di proprietà possa essere dello stesso tipo .NET come un altro tipo di entità di proprietà, pertanto che potrebbe non essere sufficiente per identificare un tipo di proprietà del tipo .NET.

In questi casi, la proprietà che punta da parte del proprietario per l'entità di proprietà diventa il _definizione di spostamento_ le proprietà del tipo di entità. Dal punto di vista di EF Core, il navigazione che lo definisce è parte dell'identità del tipo con il tipo .NET.   

Ad esempio, nella classe seguente, ShippingAddress e BillingAddress sono entrambi dello stesso tipo .NET, StreetAddress:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

Per comprendere come EF Core sarà possibile distinguere le istanze rilevate di questi oggetti, può essere utile pensare che il navigazione che lo definisce è diventata parte della chiave dell'istanza con il valore della chiave del proprietario e il tipo .NET del tipo di proprietà.

## <a name="nested-owned-types"></a>Tipi di proprietà annidati

In questo esempio OrderDetails possiede BillingAddress e ShippingAddress, che sono entrambi tipi StreetAddress. Quindi OrderDetails appartiene il tipo di ordine.

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

public enum OrderStatus
{
    Pending,
    Shipped
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

È possibile concatenare il `OwnsOne` metodo in un mapping fluent per configurare questo modello:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

È possibile ottenere la stessa cosa utilizzando `OwnedAttribute` OrderDetails sia StreetAdress.

Oltre ai tipi di proprietà annidati, un tipo di proprietà può fare riferimento a un'entità normale. Nell'esempio seguente, il paese è un'entità normale non di proprietà:

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

Questa funzionalità imposta i tipi di entità di proprietà oltre ai tipi complessi in EF6.

## <a name="storing-owned-types-in-separate-tables"></a>L'archiviazione dei tipi di proprietà in tabelle distinte

A differenza dei tipi complessi di EF6, anche i tipi di proprietà possono essere archiviati in una tabella separata dal proprietario. Per eseguire l'override la convenzione che esegue il mapping di un tipo di proprietà alla stessa tabella come proprietario, è possibile chiamare semplicemente `ToTable` e specificare un nome di tabella diverso. Nell'esempio seguente eseguirà il mapping OrderDetails e i relativi indirizzi di due a una tabella separata dall'ordine:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
        od.ToTable("OrderDetails");
    });
```

## <a name="querying-owned-types"></a>Esecuzione di query su tipi di proprietà

Quando si esegue una query sul proprietario, i tipi di proprietà saranno inclusi per impostazione predefinita. Non è necessario usare il `Include` (metodo), anche se i tipi di proprietà vengono archiviati in una tabella separata. Basati sul modello descritto in precedenza, la query seguente effettuerà il pull dell'ordine, OrderDetails e le due proprietà StreetAddresses per tutti gli ordini in sospeso dal database:

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>Limitazioni

Alcune di queste limitazioni sono fondamentali per l'uso di tipi di entità come proprietà, ma alcuni altri sono restrizioni che potrebbe essere in grado di rimuovere nelle future versioni:

### <a name="shortcomings-in-previous-versions"></a>Limitazioni nelle versioni precedenti
- In EF Core 2.0, le esplorazioni di proprietà di tipi di entità non possono essere dichiarati in tipi di entità derivati, a meno che l'entità di proprietà vengono mappate in modo esplicito a una tabella separata dalla gerarchia di proprietario. Questa limitazione è stata rimossa in EF Core 2.1

### <a name="current-shortcomings"></a>Limitazioni correnti
- Gerarchie di ereditarietà che include proprietà non sono supportati i tipi di entità
- Tipi di entità di proprietà non possono essere a cui fa riferimento una proprietà di navigazione della raccolta (solo riferimenti navigazioni sono attualmente supportati)
- Spostamenti a tipi di entità non possono essere null, a meno che queste vengono mappate a una tabella separata in modo esplicito da parte del proprietario di proprietà
- Le istanze di tipi di entità di proprietà non possono essere condiviso da più proprietari (si tratta di uno scenario noto per gli oggetti valore non può essere implementato usando tipi di entità di proprietà)

### <a name="by-design-restrictions"></a>Restrizioni da Progettazione
- Non è possibile creare un `DbSet<T>`
- Non è possibile chiamare `Entity<T>()` con un tipo di proprietà in `ModelBuilder`
