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
# <a name="owned-entity-types"></a><span data-ttu-id="4043a-102">Tipi di proprietà di entità</span><span class="sxs-lookup"><span data-stu-id="4043a-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="4043a-103">Questa funzionalità è nuova in Entity Framework Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="4043a-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="4043a-104">Componenti di base di Entity Framework consente ai tipi di entità di modello che è possono visualizzare solo le proprietà di navigazione di altri tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="4043a-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="4043a-105">Questi sono denominati _tipi di entità di proprietà_.</span><span class="sxs-lookup"><span data-stu-id="4043a-105">These are called _owned entity types_.</span></span> <span data-ttu-id="4043a-106">L'entità che contiene un tipo di proprietà di entità è relativa _proprietario_.</span><span class="sxs-lookup"><span data-stu-id="4043a-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="4043a-107">Configurazione esplicita</span><span class="sxs-lookup"><span data-stu-id="4043a-107">Explicit configuration</span></span>

<span data-ttu-id="4043a-108">Proprietà entità tipi non sono mai inclusi EF core nel modello per convenzione.</span><span class="sxs-lookup"><span data-stu-id="4043a-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="4043a-109">È possibile utilizzare il `OwnsOne` metodo `OnModelCreating` o annotare il tipo con `OwnedAttrbibute` (nuova in Entity Framework Core 2.1) per configurare il tipo come tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="4043a-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttrbibute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="4043a-110">In questo esempio StreetAddress è un tipo con nessuna proprietà identity.</span><span class="sxs-lookup"><span data-stu-id="4043a-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="4043a-111">Viene usato come proprietà del tipo Order per specificare l'indirizzo di spedizione per uno specifico ordine.</span><span class="sxs-lookup"><span data-stu-id="4043a-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="4043a-112">In `OnModelCreating`, utilizziamo la `OwnsOne` per specificare che la proprietà ShippingAddress è un'entità di proprietà del tipo di ordine.</span><span class="sxs-lookup"><span data-stu-id="4043a-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

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

<span data-ttu-id="4043a-113">Se la proprietà ShippingAddress è privata nel tipo di ordine, è possibile utilizzare la versione della stringa di `OwnsOne` metodo:</span><span class="sxs-lookup"><span data-stu-id="4043a-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="4043a-114">In questo esempio, si usa il `OwnedAttribute` per ottenere lo stesso risultato:</span><span class="sxs-lookup"><span data-stu-id="4043a-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

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

## <a name="implicit-keys"></a><span data-ttu-id="4043a-115">Chiavi implicite</span><span class="sxs-lookup"><span data-stu-id="4043a-115">Implicit keys</span></span>

<span data-ttu-id="4043a-116">In Entity Framework Core 2.0 e 2.1, solo le proprietà di navigazione di riferimento possono puntare a tipi di proprietà.</span><span class="sxs-lookup"><span data-stu-id="4043a-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="4043a-117">Le raccolte di tipi di proprietà non sono supportate.</span><span class="sxs-lookup"><span data-stu-id="4043a-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="4043a-118">Questi riferimenti di proprietà tipi hanno sempre una relazione uno a uno con il proprietario, di conseguenza, non è necessario i propri valori di chiave.</span><span class="sxs-lookup"><span data-stu-id="4043a-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="4043a-119">Nell'esempio precedente, il tipo StreetAddress non è necessario definire una proprietà chiave.</span><span class="sxs-lookup"><span data-stu-id="4043a-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="4043a-120">Per comprendere la modalità di traccia di questi oggetti EF Core, è utile sapere che una chiave primaria viene creata come un [nascondere proprietà](xref:core/modeling/shadow-properties) per il tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="4043a-120">In order to understanding how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="4043a-121">Il valore della chiave di un'istanza del tipo di proprietà sarà identico al valore della chiave dell'istanza del proprietario.</span><span class="sxs-lookup"><span data-stu-id="4043a-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="4043a-122">Mapping di tipi con la suddivisione di tabella di proprietà</span><span class="sxs-lookup"><span data-stu-id="4043a-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="4043a-123">Quando si utilizzano database relazionali, per convenzione, i tipi di proprietà vengono mappate alla stessa tabella come proprietario.</span><span class="sxs-lookup"><span data-stu-id="4043a-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="4043a-124">È necessario suddividere la tabella in due parti: alcune colonne da utilizzare per archiviare i dati del proprietario e alcune colonne da utilizzare per archiviare i dati dell'entità di proprietà.</span><span class="sxs-lookup"><span data-stu-id="4043a-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="4043a-125">Questa è una caratteristica comune nota come divisione di tabella.</span><span class="sxs-lookup"><span data-stu-id="4043a-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="4043a-126">Proprietà tipi archiviati con la suddivisione di tabella possono essere molto simili ai tipi complessi come vengono usati in EF6.</span><span class="sxs-lookup"><span data-stu-id="4043a-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="4043a-127">Per convenzione, Core EF denominerà le colonne del database per le proprietà del tipo di entità proprietà modello di _EntityProperty_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="4043a-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="4043a-128">Pertanto, le proprietà di StreetAddress verranno visualizzate nella tabella Orders con i nomi ShippingAddress_Street e ShippingAddress_City.</span><span class="sxs-lookup"><span data-stu-id="4043a-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="4043a-129">È possibile aggiungere il `HasColumnName` metodo rinominare le colonne.</span><span class="sxs-lookup"><span data-stu-id="4043a-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="4043a-130">Nel caso in cui StreetAddress è una proprietà pubblica, sarebbe i mapping</span><span class="sxs-lookup"><span data-stu-id="4043a-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="4043a-131">La condivisione tra più tipi di proprietà lo stesso tipo di .NET</span><span class="sxs-lookup"><span data-stu-id="4043a-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="4043a-132">Un tipo di proprietà di entità può essere dello stesso tipo .NET come un altro tipo di proprietà di entità, pertanto che il tipo .NET potrebbe non essere sufficiente per identificare un tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="4043a-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="4043a-133">In questi casi, la proprietà scegliendo dal proprietario dell'entità di proprietà diventa il _definizione spostamento_ del tipo di proprietà di entità.</span><span class="sxs-lookup"><span data-stu-id="4043a-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="4043a-134">Dal punto di vista di EF Core, la definizione di spostamento è parte dell'identità del tipo con il tipo .NET.</span><span class="sxs-lookup"><span data-stu-id="4043a-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="4043a-135">Ad esempio, nella classe seguente, ShippingAddress e BillingAddress sono entrambi dello stesso tipo .NET, StreetAddress:</span><span class="sxs-lookup"><span data-stu-id="4043a-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="4043a-136">Per capire come EF Core sarà possibile distinguere rilevate le istanze di questi oggetti, può essere utile pensare che la definizione di spostamento è diventata parte della chiave dell'istanza con il valore della chiave del proprietario e il tipo .NET del tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="4043a-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="4043a-137">Proprietà di tipi annidati</span><span class="sxs-lookup"><span data-stu-id="4043a-137">Nested owned types</span></span>

<span data-ttu-id="4043a-138">In questo esempio OrderDetails proprietario BillingAddress e ShippingAddress, che sono entrambi tipi StreetAddress.</span><span class="sxs-lookup"><span data-stu-id="4043a-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="4043a-139">Quindi OrderDetails appartiene il tipo di ordine.</span><span class="sxs-lookup"><span data-stu-id="4043a-139">Then OrderDetails is owned by the Order type.</span></span>

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

<span data-ttu-id="4043a-140">È possibile concatenare il `OwnsOne` metodo in un mapping di Microsoft Office fluent per configurare questo modello:</span><span class="sxs-lookup"><span data-stu-id="4043a-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="4043a-141">È possibile ottenere la stessa cosa utilizzando `OwnedAttribute` OrderDetails sia StreetAdress.</span><span class="sxs-lookup"><span data-stu-id="4043a-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="4043a-142">Oltre ai tipi di proprietà annidati, un tipo di proprietà può fare riferimento a un'entità normale.</span><span class="sxs-lookup"><span data-stu-id="4043a-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="4043a-143">Nell'esempio seguente, Country è un'entità normale (vale a dire non proprietà):</span><span class="sxs-lookup"><span data-stu-id="4043a-143">In the following example, Country is a regular (i.e. non-owned) entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="4043a-144">Questa funzionalità imposta i tipi di proprietà di entità oltre ai tipi complessi in EF6.</span><span class="sxs-lookup"><span data-stu-id="4043a-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="4043a-145">L'archiviazione dei tipi di proprietà in tabelle separate</span><span class="sxs-lookup"><span data-stu-id="4043a-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="4043a-146">A differenza dei tipi complessi EF6, tipi di proprietà possono essere archiviati anche in una tabella separata dal proprietario.</span><span class="sxs-lookup"><span data-stu-id="4043a-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="4043a-147">Per ignorare la convenzione che esegue il mapping alla stessa tabella come proprietario di un tipo di proprietà, è sufficiente chiamare `ToTable` e fornire un nome di tabella diverso.</span><span class="sxs-lookup"><span data-stu-id="4043a-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="4043a-148">Nell'esempio seguente verrà eseguito il mapping OrderDetails e i relativi due indirizzi in una tabella separata dall'ordine:</span><span class="sxs-lookup"><span data-stu-id="4043a-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a><span data-ttu-id="4043a-149">Esecuzione di query su tipi di proprietà</span><span class="sxs-lookup"><span data-stu-id="4043a-149">Querying owned types</span></span>

<span data-ttu-id="4043a-150">Quando si esegue una query sul proprietario, i tipi di proprietà saranno inclusi per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="4043a-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="4043a-151">Non è necessario utilizzare il `Include` (metodo), anche se i tipi di proprietà vengono archiviati in una tabella separata.</span><span class="sxs-lookup"><span data-stu-id="4043a-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="4043a-152">Basato sul modello descritto in precedenza, la query seguente eseguirà il pull dell'ordine, OrderDetails e le due proprietà StreeAddresses per tutti gli ordini in sospeso dal database:</span><span class="sxs-lookup"><span data-stu-id="4043a-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreeAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="4043a-153">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="4043a-153">Limitations</span></span>

<span data-ttu-id="4043a-154">Ecco alcune limitazioni dei tipi di proprietà di entità.</span><span class="sxs-lookup"><span data-stu-id="4043a-154">Here are some limitations of owned entity types.</span></span> <span data-ttu-id="4043a-155">Alcune di queste limitazioni sono fondamentali per l'utilizzo di tipi come proprietà, ma alcuni altri sono restrizioni punto nel tempo che si desidera rimuovere nelle future versioni:</span><span class="sxs-lookup"><span data-stu-id="4043a-155">Some of these limitations are fundamental to how owned types work, but some others are point-in-time restrictions that we would like to remove in future releases:</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="4043a-156">Molti punti deboli correnti</span><span class="sxs-lookup"><span data-stu-id="4043a-156">Current shortcomings</span></span>
- <span data-ttu-id="4043a-157">Ereditarietà dei tipi di proprietà non è supportata.</span><span class="sxs-lookup"><span data-stu-id="4043a-157">Inheritance of owned types is not supported</span></span>
- <span data-ttu-id="4043a-158">Tipi di proprietà non possono essere fa riferimento una proprietà di navigazione della raccolta</span><span class="sxs-lookup"><span data-stu-id="4043a-158">Owned types cannot be pointed at by a collection navigation property</span></span>
- <span data-ttu-id="4043a-159">Poiché utilizzano la suddivisione di tabella per impostazione predefinita di proprietà tipi presentano anche le restrizioni seguenti, a meno che in modo esplicito il mapping a una tabella diversa:</span><span class="sxs-lookup"><span data-stu-id="4043a-159">Since they use table splitting by default owned types also have the following restrictions unless explicitly mapped to a different table:</span></span>
   - <span data-ttu-id="4043a-160">Un tipo derivato non può essere proprietario</span><span class="sxs-lookup"><span data-stu-id="4043a-160">They cannot be owned by a derived type</span></span>
   - <span data-ttu-id="4043a-161">La proprietà di navigazione definizione non può essere impostata su null (ad esempio di proprietà sono sempre necessari tipi nella stessa tabella)</span><span class="sxs-lookup"><span data-stu-id="4043a-161">The defining navigation property cannot be set to null (i.e. owned types on the same table are always required)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="4043a-162">Le restrizioni da Progettazione</span><span class="sxs-lookup"><span data-stu-id="4043a-162">By-design restrictions</span></span>
- <span data-ttu-id="4043a-163">Non è possibile creare un `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="4043a-163">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="4043a-164">Non è possibile chiamare `Entity<T>()` con un tipo di proprietà in `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="4043a-164">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
