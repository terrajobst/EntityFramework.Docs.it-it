---
title: Proprietà di tipi di entità - EF Core
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: 768429b857b09c1974f4ade31b5bbb6b1c7e15c3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911875"
---
# <a name="owned-entity-types"></a><span data-ttu-id="40f21-102">Tipi di entità di proprietà</span><span class="sxs-lookup"><span data-stu-id="40f21-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="40f21-103">Questa funzionalità è una novità di EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="40f21-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="40f21-104">EF Core consente di accedere ai tipi di entità del modello che è possono visualizzare unicamente le proprietà di navigazione di altri tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="40f21-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="40f21-105">Questi sono denominati _tipi di entità di proprietà_.</span><span class="sxs-lookup"><span data-stu-id="40f21-105">These are called _owned entity types_.</span></span> <span data-ttu-id="40f21-106">L'entità che contiene un tipo di entità di proprietà è relativa _proprietario_.</span><span class="sxs-lookup"><span data-stu-id="40f21-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="40f21-107">Configurazione esplicita</span><span class="sxs-lookup"><span data-stu-id="40f21-107">Explicit configuration</span></span>

<span data-ttu-id="40f21-108">Proprietà dell'entità tipi non sono mai inclusi da EF Core nel modello per convenzione.</span><span class="sxs-lookup"><span data-stu-id="40f21-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="40f21-109">È possibile usare la `OwnsOne` nel metodo `OnModelCreating` o annotare il tipo con `OwnedAttribute` (novità di EF Core 2.1) per configurare il tipo come un tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="40f21-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="40f21-110">In questo esempio StreetAddress è un tipo con nessuna proprietà identity.</span><span class="sxs-lookup"><span data-stu-id="40f21-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="40f21-111">Viene usato come proprietà del tipo Order per specificare l'indirizzo di spedizione per uno specifico ordine.</span><span class="sxs-lookup"><span data-stu-id="40f21-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="40f21-112">Nelle `OnModelCreating`, viene usato il `OwnsOne` metodo per specificare che la proprietà ShippingAddress è un'entità di proprietà del tipo di ordine.</span><span class="sxs-lookup"><span data-stu-id="40f21-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

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

<span data-ttu-id="40f21-113">Se la proprietà ShippingAddress è privata nel tipo di ordine, è possibile usare la versione della stringa di `OwnsOne` metodo:</span><span class="sxs-lookup"><span data-stu-id="40f21-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="40f21-114">In questo esempio, utilizziamo il `OwnedAttribute` per raggiungere lo stesso scopo:</span><span class="sxs-lookup"><span data-stu-id="40f21-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

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

## <a name="implicit-keys"></a><span data-ttu-id="40f21-115">Chiavi implicite</span><span class="sxs-lookup"><span data-stu-id="40f21-115">Implicit keys</span></span>

<span data-ttu-id="40f21-116">In EF Core 2.0 e 2.1, solo le proprietà di navigazione di riferimento possono puntare a tipi di proprietà.</span><span class="sxs-lookup"><span data-stu-id="40f21-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="40f21-117">Raccolte di tipi di proprietà non sono supportate.</span><span class="sxs-lookup"><span data-stu-id="40f21-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="40f21-118">Questi riferimenti di proprietà tipi hanno sempre una relazione uno a uno con il proprietario, pertanto non necessitano i propri valori di chiave.</span><span class="sxs-lookup"><span data-stu-id="40f21-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="40f21-119">Nell'esempio precedente, il tipo StreetAddress non necessario definire una proprietà chiave.</span><span class="sxs-lookup"><span data-stu-id="40f21-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="40f21-120">Per comprendere come EF Core controlla questi oggetti, è utile sapere che una chiave primaria viene creata come un [proprietà shadow](xref:core/modeling/shadow-properties) per il tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="40f21-120">In order to understanding how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="40f21-121">Il valore della chiave di un'istanza del tipo di proprietà sarà identico al valore della chiave dell'istanza del proprietario.</span><span class="sxs-lookup"><span data-stu-id="40f21-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="40f21-122">Mapping dei tipi con la suddivisione di tabelle di proprietà</span><span class="sxs-lookup"><span data-stu-id="40f21-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="40f21-123">Quando si usano database relazionali, per convenzione, i tipi di proprietà vengono mappati alla stessa tabella come proprietario.</span><span class="sxs-lookup"><span data-stu-id="40f21-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="40f21-124">È necessario suddividere la tabella in due parti: alcune colonne da utilizzare per archiviare i dati del proprietario e alcune colonne da utilizzare per archiviare i dati dell'entità di proprietà.</span><span class="sxs-lookup"><span data-stu-id="40f21-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="40f21-125">Questa è una funzionalità comune nota come la suddivisione di tabelle.</span><span class="sxs-lookup"><span data-stu-id="40f21-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="40f21-126">Proprietà tipo archiviato con la suddivisione di tabelle possono essere usati in modo molto simile ai tipi complessi come vengono usati in EF6.</span><span class="sxs-lookup"><span data-stu-id="40f21-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="40f21-127">Per convenzione, EF Core sarà denominare le colonne del database per le proprietà del tipo di entità di proprietà segue il modello _EntityProperty_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="40f21-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="40f21-128">Di conseguenza la proprietà StreetAddress verranno visualizzate nella tabella Orders con i nomi ShippingAddress_Street e ShippingAddress_City.</span><span class="sxs-lookup"><span data-stu-id="40f21-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="40f21-129">È possibile aggiungere il `HasColumnName` metodo per rinominare le colonne.</span><span class="sxs-lookup"><span data-stu-id="40f21-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="40f21-130">Nel caso in cui StreetAddress è una proprietà pubblica, i mapping sono</span><span class="sxs-lookup"><span data-stu-id="40f21-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="40f21-131">Condividere lo stesso tipo di .NET tra più tipi di proprietà</span><span class="sxs-lookup"><span data-stu-id="40f21-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="40f21-132">Un tipo di entità di proprietà possa essere dello stesso tipo .NET come un altro tipo di entità di proprietà, pertanto che potrebbe non essere sufficiente per identificare un tipo di proprietà del tipo .NET.</span><span class="sxs-lookup"><span data-stu-id="40f21-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="40f21-133">In questi casi, la proprietà che punta da parte del proprietario per l'entità di proprietà diventa il _definizione di spostamento_ le proprietà del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="40f21-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="40f21-134">Dal punto di vista di EF Core, il navigazione che lo definisce è parte dell'identità del tipo con il tipo .NET.</span><span class="sxs-lookup"><span data-stu-id="40f21-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="40f21-135">Ad esempio, nella classe seguente, ShippingAddress e BillingAddress sono entrambi dello stesso tipo .NET, StreetAddress:</span><span class="sxs-lookup"><span data-stu-id="40f21-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="40f21-136">Per comprendere come EF Core sarà possibile distinguere le istanze rilevate di questi oggetti, può essere utile pensare che il navigazione che lo definisce è diventata parte della chiave dell'istanza con il valore della chiave del proprietario e il tipo .NET del tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="40f21-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="40f21-137">Tipi di proprietà annidati</span><span class="sxs-lookup"><span data-stu-id="40f21-137">Nested owned types</span></span>

<span data-ttu-id="40f21-138">In questo esempio OrderDetails possiede BillingAddress e ShippingAddress, che sono entrambi tipi StreetAddress.</span><span class="sxs-lookup"><span data-stu-id="40f21-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="40f21-139">Quindi OrderDetails appartiene il tipo di ordine.</span><span class="sxs-lookup"><span data-stu-id="40f21-139">Then OrderDetails is owned by the Order type.</span></span>

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

<span data-ttu-id="40f21-140">È possibile concatenare il `OwnsOne` metodo in un mapping fluent per configurare questo modello:</span><span class="sxs-lookup"><span data-stu-id="40f21-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="40f21-141">È possibile ottenere la stessa cosa utilizzando `OwnedAttribute` OrderDetails sia StreetAdress.</span><span class="sxs-lookup"><span data-stu-id="40f21-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="40f21-142">Oltre ai tipi di proprietà annidati, un tipo di proprietà può fare riferimento a un'entità normale.</span><span class="sxs-lookup"><span data-stu-id="40f21-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="40f21-143">Nell'esempio seguente, il paese è un'entità normale (ovvero non di proprietà):</span><span class="sxs-lookup"><span data-stu-id="40f21-143">In the following example, Country is a regular (i.e. non-owned) entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="40f21-144">Questa funzionalità imposta i tipi di entità di proprietà oltre ai tipi complessi in EF6.</span><span class="sxs-lookup"><span data-stu-id="40f21-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="40f21-145">L'archiviazione dei tipi di proprietà in tabelle distinte</span><span class="sxs-lookup"><span data-stu-id="40f21-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="40f21-146">A differenza dei tipi complessi di EF6, anche i tipi di proprietà possono essere archiviati in una tabella separata dal proprietario.</span><span class="sxs-lookup"><span data-stu-id="40f21-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="40f21-147">Per eseguire l'override la convenzione che esegue il mapping di un tipo di proprietà alla stessa tabella come proprietario, è possibile chiamare semplicemente `ToTable` e specificare un nome di tabella diverso.</span><span class="sxs-lookup"><span data-stu-id="40f21-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="40f21-148">Nell'esempio seguente eseguirà il mapping OrderDetails e i relativi indirizzi di due a una tabella separata dall'ordine:</span><span class="sxs-lookup"><span data-stu-id="40f21-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a><span data-ttu-id="40f21-149">Esecuzione di query su tipi di proprietà</span><span class="sxs-lookup"><span data-stu-id="40f21-149">Querying owned types</span></span>

<span data-ttu-id="40f21-150">Quando si esegue una query sul proprietario, i tipi di proprietà saranno inclusi per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="40f21-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="40f21-151">Non è necessario usare il `Include` (metodo), anche se i tipi di proprietà vengono archiviati in una tabella separata.</span><span class="sxs-lookup"><span data-stu-id="40f21-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="40f21-152">Basati sul modello descritto in precedenza, la query seguente effettuerà il pull dell'ordine, OrderDetails e le due proprietà StreeAddresses per tutti gli ordini in sospeso dal database:</span><span class="sxs-lookup"><span data-stu-id="40f21-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreeAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="40f21-153">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="40f21-153">Limitations</span></span>

<span data-ttu-id="40f21-154">Alcune di queste limitazioni sono fondamentali per l'uso di tipi di entità come proprietà, ma alcuni altri sono restrizioni che potrebbe essere in grado di rimuovere nelle future versioni:</span><span class="sxs-lookup"><span data-stu-id="40f21-154">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="40f21-155">Limitazioni nelle versioni precedenti</span><span class="sxs-lookup"><span data-stu-id="40f21-155">Shortcomings in previous versions</span></span>
- <span data-ttu-id="40f21-156">In EF Core 2.0, le esplorazioni di proprietà di tipi di entità non possono essere dichiarati in tipi di entità derivati, a meno che l'entità di proprietà vengono mappate in modo esplicito a una tabella separata dalla gerarchia di proprietario.</span><span class="sxs-lookup"><span data-stu-id="40f21-156">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="40f21-157">Questa limitazione è stata rimossa in EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="40f21-157">This limitation has been removed in EF Core 2.1</span></span>
 
### <a name="current-shortcomings"></a><span data-ttu-id="40f21-158">Limitazioni correnti</span><span class="sxs-lookup"><span data-stu-id="40f21-158">Current shortcomings</span></span>
- <span data-ttu-id="40f21-159">Gerarchie di ereditarietà che include proprietà non sono supportati i tipi di entità</span><span class="sxs-lookup"><span data-stu-id="40f21-159">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="40f21-160">Tipi di entità di proprietà non possono essere a cui fa riferimento una proprietà di navigazione della raccolta (solo riferimenti navigazioni sono attualmente supportati)</span><span class="sxs-lookup"><span data-stu-id="40f21-160">Owned entity types cannot be pointed at by a collection navigation property (only reference navigations are currently supported)</span></span>
- <span data-ttu-id="40f21-161">Spostamenti a tipi di entità non possono essere null, a meno che queste vengono mappate a una tabella separata in modo esplicito da parte del proprietario di proprietà</span><span class="sxs-lookup"><span data-stu-id="40f21-161">Navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span> 
- <span data-ttu-id="40f21-162">Le istanze di tipi di entità di proprietà non possono essere condiviso da più proprietari (si tratta di uno scenario noto per gli oggetti valore non può essere implementato usando tipi di entità di proprietà)</span><span class="sxs-lookup"><span data-stu-id="40f21-162">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="40f21-163">Restrizioni da Progettazione</span><span class="sxs-lookup"><span data-stu-id="40f21-163">By-design restrictions</span></span>
- <span data-ttu-id="40f21-164">Non è possibile creare un `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="40f21-164">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="40f21-165">Non è possibile chiamare `Entity<T>()` con un tipo di proprietà in `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="40f21-165">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
