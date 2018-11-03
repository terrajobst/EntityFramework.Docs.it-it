---
title: Proprietà di tipi di entità - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: 58da3b6b951b3fa4aa04ec75f5759555c1f0cde5
ms.sourcegitcommit: 39080d38e1adea90db741257e60dc0e7ed08aa82
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/03/2018
ms.locfileid: "50980028"
---
# <a name="owned-entity-types"></a><span data-ttu-id="5870e-102">Tipi di entità di proprietà</span><span class="sxs-lookup"><span data-stu-id="5870e-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="5870e-103">Questa funzionalità è una novità di EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="5870e-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="5870e-104">EF Core consente di accedere ai tipi di entità del modello che è possono visualizzare unicamente le proprietà di navigazione di altri tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="5870e-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="5870e-105">Questi sono denominati _tipi di entità di proprietà_.</span><span class="sxs-lookup"><span data-stu-id="5870e-105">These are called _owned entity types_.</span></span> <span data-ttu-id="5870e-106">L'entità che contiene un tipo di entità di proprietà è relativa _proprietario_.</span><span class="sxs-lookup"><span data-stu-id="5870e-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="5870e-107">Configurazione esplicita</span><span class="sxs-lookup"><span data-stu-id="5870e-107">Explicit configuration</span></span>

<span data-ttu-id="5870e-108">Proprietà dell'entità tipi non sono mai inclusi da EF Core nel modello per convenzione.</span><span class="sxs-lookup"><span data-stu-id="5870e-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="5870e-109">È possibile usare la `OwnsOne` nel metodo `OnModelCreating` o annotare il tipo con `OwnedAttribute` (novità di EF Core 2.1) per configurare il tipo come un tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="5870e-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="5870e-110">In questo esempio `StreetAddress` è un tipo con nessuna proprietà identity.</span><span class="sxs-lookup"><span data-stu-id="5870e-110">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="5870e-111">Viene usato come proprietà del tipo Order per specificare l'indirizzo di spedizione per uno specifico ordine.</span><span class="sxs-lookup"><span data-stu-id="5870e-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="5870e-112">È possibile usare il `OwnedAttribute` da considerare come un'entità di proprietà quando viene fatto riferimento da un altro tipo di entità:</span><span class="sxs-lookup"><span data-stu-id="5870e-112">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="5870e-113">È anche possibile usare la `OwnsOne` metodo nella `OnModelCreating` per specificare che le `ShippingAddress` proprietà è un'entità di proprietà del `Order` tipo di entità e configurare i facet aggiuntivi se necessario.</span><span class="sxs-lookup"><span data-stu-id="5870e-113">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="5870e-114">Se il `ShippingAddress` tratta di una proprietà privata il `Order` tipo, è possibile usare la versione della stringa il `OwnsOne` metodo:</span><span class="sxs-lookup"><span data-stu-id="5870e-114">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="5870e-115">Vedere le [progetto di esempio completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) per altre informazioni sul contesto.</span><span class="sxs-lookup"><span data-stu-id="5870e-115">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span> 

## <a name="implicit-keys"></a><span data-ttu-id="5870e-116">Chiavi implicite</span><span class="sxs-lookup"><span data-stu-id="5870e-116">Implicit keys</span></span>

<span data-ttu-id="5870e-117">Configurato con i tipi di proprietà `OwnsOne` o individuati mediante una navigazione di riferimento hanno sempre una relazione uno a uno con il proprietario, pertanto non devono i propri valori di chiave come i valori di chiave esterni siano univoci.</span><span class="sxs-lookup"><span data-stu-id="5870e-117">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="5870e-118">Nell'esempio precedente, il `StreetAddress` tipo non è necessario definire una proprietà chiave.</span><span class="sxs-lookup"><span data-stu-id="5870e-118">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="5870e-119">Per comprendere come EF Core controlla questi oggetti, è utile pensare che una chiave primaria viene creata come un [proprietà shadow](xref:core/modeling/shadow-properties) per il tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="5870e-119">In order to understand how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="5870e-120">Il valore della chiave di un'istanza del tipo di proprietà sarà identico al valore della chiave dell'istanza del proprietario.</span><span class="sxs-lookup"><span data-stu-id="5870e-120">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="5870e-121">Raccolte di tipi di proprietà</span><span class="sxs-lookup"><span data-stu-id="5870e-121">Collections of owned types</span></span>

>[!NOTE]
> <span data-ttu-id="5870e-122">Questa funzionalità è una novità di EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="5870e-122">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="5870e-123">Per configurare una raccolta di tipi di proprietà `OwnsMany` deve essere usato in `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="5870e-123">To configure a collection of owned types `OwnsMany` should be used in `OnModelCreating`.</span></span> <span data-ttu-id="5870e-124">Tuttavia la chiave primaria non verrà configurata per convenzione, in modo che sia possibile specificare in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="5870e-124">However the primary key will not be configured by convention, so it need to be specified explicitly.</span></span> <span data-ttu-id="5870e-125">Accade spesso di usare una chiave complessa per questi tipi di entità che includa la chiave esterna per il proprietario e una proprietà aggiuntiva univoca che può anche essere nello stato shadow:</span><span class="sxs-lookup"><span data-stu-id="5870e-125">It is common to use a complex key for these type of entities incorporating the foreign key to the owner and an additional unique property that can also be in shadow state:</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="5870e-126">Mapping dei tipi con la suddivisione di tabelle di proprietà</span><span class="sxs-lookup"><span data-stu-id="5870e-126">Mapping owned types with table splitting</span></span>

<span data-ttu-id="5870e-127">Quando si utilizza relazionale i database, per riferimento di convenzione di proprietà vengono eseguito il mapping di tipi alla stessa tabella come proprietario.</span><span class="sxs-lookup"><span data-stu-id="5870e-127">When using relational databases, by convention reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="5870e-128">È necessario suddividere la tabella in due parti: alcune colonne da utilizzare per archiviare i dati del proprietario e alcune colonne da utilizzare per archiviare i dati dell'entità di proprietà.</span><span class="sxs-lookup"><span data-stu-id="5870e-128">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="5870e-129">Questa è una funzionalità comune nota come la suddivisione di tabelle.</span><span class="sxs-lookup"><span data-stu-id="5870e-129">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="5870e-130">Proprietà tipo archiviato con la suddivisione di tabelle possono essere usati in modo analogo ai tipi complessi come vengono usati in EF6.</span><span class="sxs-lookup"><span data-stu-id="5870e-130">Owned types stored with table splitting can be used similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="5870e-131">Per convenzione, EF Core sarà denominare le colonne del database per le proprietà del tipo di entità di proprietà segue il modello _Navigation_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="5870e-131">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="5870e-132">Di conseguenza il `StreetAddress` proprietà verranno visualizzate nella tabella "Ordini" con i nomi 'ShippingAddress_Street' e 'ShippingAddress_City'.</span><span class="sxs-lookup"><span data-stu-id="5870e-132">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="5870e-133">È possibile aggiungere il `HasColumnName` metodo per rinominare le colonne:</span><span class="sxs-lookup"><span data-stu-id="5870e-133">You can append the `HasColumnName` method to rename those columns:</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="5870e-134">Condividere lo stesso tipo di .NET tra più tipi di proprietà</span><span class="sxs-lookup"><span data-stu-id="5870e-134">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="5870e-135">Un tipo di entità di proprietà possa essere dello stesso tipo .NET come un altro tipo di entità di proprietà, pertanto che potrebbe non essere sufficiente per identificare un tipo di proprietà del tipo .NET.</span><span class="sxs-lookup"><span data-stu-id="5870e-135">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="5870e-136">In questi casi, la proprietà che punta da parte del proprietario per l'entità di proprietà diventa il _definizione di spostamento_ le proprietà del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="5870e-136">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="5870e-137">Dal punto di vista di EF Core, il navigazione che lo definisce è parte dell'identità del tipo con il tipo .NET.</span><span class="sxs-lookup"><span data-stu-id="5870e-137">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="5870e-138">Ad esempio, nella classe seguente `ShippingAddress` e `BillingAddress` sono entrambi dello stesso tipo, .NET `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="5870e-138">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`:</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="5870e-139">Per comprendere come EF Core sarà possibile distinguere le istanze rilevate di questi oggetti, può essere utile pensare che il navigazione che lo definisce è diventata parte della chiave dell'istanza con il valore della chiave del proprietario e il tipo .NET del tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="5870e-139">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="5870e-140">Tipi di proprietà annidati</span><span class="sxs-lookup"><span data-stu-id="5870e-140">Nested owned types</span></span>

<span data-ttu-id="5870e-141">In questo esempio `OrderDetails` possiede `BillingAddress` e `ShippingAddress`, che sono entrambi `StreetAddress` tipi.</span><span class="sxs-lookup"><span data-stu-id="5870e-141">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="5870e-142">Quindi `OrderDetails` è di proprietà del tipo `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="5870e-142">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="5870e-143">Oltre ai tipi di proprietà annidati, un tipo di proprietà può fare riferimento a un'entità normale, può essere il proprietario o un'entità diversa, purché l'entità di proprietà sia sul lato dipendente.</span><span class="sxs-lookup"><span data-stu-id="5870e-143">In addition to nested owned types, an owned type can reference a regular entity, it can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="5870e-144">Questa funzionalità imposta i tipi di entità di proprietà oltre ai tipi complessi in EF6.</span><span class="sxs-lookup"><span data-stu-id="5870e-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="5870e-145">È possibile concatenare il `OwnsOne` metodo in una chiamata fluent per configurare questo modello:</span><span class="sxs-lookup"><span data-stu-id="5870e-145">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="5870e-146">È anche possibile ottenere la stessa operazione usando `OwnedAttribute` su entrambi `OrderDetails` e `StreetAdress`.</span><span class="sxs-lookup"><span data-stu-id="5870e-146">It is also possible to achieve the same thing using `OwnedAttribute` on both `OrderDetails` and `StreetAdress`.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="5870e-147">L'archiviazione dei tipi di proprietà in tabelle distinte</span><span class="sxs-lookup"><span data-stu-id="5870e-147">Storing owned types in separate tables</span></span>

<span data-ttu-id="5870e-148">A differenza dei tipi complessi di EF6, anche i tipi di proprietà possono essere archiviati in una tabella separata dal proprietario.</span><span class="sxs-lookup"><span data-stu-id="5870e-148">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="5870e-149">Per eseguire l'override la convenzione che esegue il mapping di un tipo di proprietà alla stessa tabella come proprietario, è possibile chiamare semplicemente `ToTable` e specificare un nome di tabella diverso.</span><span class="sxs-lookup"><span data-stu-id="5870e-149">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="5870e-150">Nell'esempio seguente verrà eseguito il mapping `OrderDetails` e i relativi indirizzi in una tabella separata da due `DetailedOrder`:</span><span class="sxs-lookup"><span data-stu-id="5870e-150">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a><span data-ttu-id="5870e-151">Esecuzione di query su tipi di proprietà</span><span class="sxs-lookup"><span data-stu-id="5870e-151">Querying owned types</span></span>

<span data-ttu-id="5870e-152">Quando si esegue una query sul proprietario, i tipi di proprietà saranno inclusi per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="5870e-152">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="5870e-153">Non è necessario usare il `Include` (metodo), anche se i tipi di proprietà vengono archiviati in una tabella separata.</span><span class="sxs-lookup"><span data-stu-id="5870e-153">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="5870e-154">Basati sul modello descritto in precedenza, la query seguente otterrà `Order`, `OrderDetails` e le due proprietà `StreetAddresses` dal database:</span><span class="sxs-lookup"><span data-stu-id="5870e-154">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="5870e-155">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="5870e-155">Limitations</span></span>

<span data-ttu-id="5870e-156">Alcune di queste limitazioni sono fondamentali per l'uso di tipi di entità come proprietà, ma alcuni altri sono restrizioni che potrebbe essere in grado di rimuovere nelle future versioni:</span><span class="sxs-lookup"><span data-stu-id="5870e-156">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="5870e-157">Restrizioni da Progettazione</span><span class="sxs-lookup"><span data-stu-id="5870e-157">By-design restrictions</span></span>
- <span data-ttu-id="5870e-158">Non è possibile creare un `DbSet<T>` per un tipo di proprietà</span><span class="sxs-lookup"><span data-stu-id="5870e-158">You cannot create a `DbSet<T>` for an owned type</span></span>
- <span data-ttu-id="5870e-159">Non è possibile chiamare `Entity<T>()` con un tipo di proprietà in `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="5870e-159">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="5870e-160">Limitazioni correnti</span><span class="sxs-lookup"><span data-stu-id="5870e-160">Current shortcomings</span></span>
- <span data-ttu-id="5870e-161">Gerarchie di ereditarietà che include proprietà non sono supportati i tipi di entità</span><span class="sxs-lookup"><span data-stu-id="5870e-161">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="5870e-162">Le esplorazioni di riferimento di proprietà i tipi di entità non possono essere null, a meno che queste vengono mappate a una tabella separata in modo esplicito dal proprietario</span><span class="sxs-lookup"><span data-stu-id="5870e-162">Reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="5870e-163">Le istanze di tipi di entità di proprietà non possono essere condiviso da più proprietari (si tratta di uno scenario noto per gli oggetti valore non può essere implementato usando tipi di entità di proprietà)</span><span class="sxs-lookup"><span data-stu-id="5870e-163">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="5870e-164">Limitazioni nelle versioni precedenti</span><span class="sxs-lookup"><span data-stu-id="5870e-164">Shortcomings in previous versions</span></span>
- <span data-ttu-id="5870e-165">In EF Core 2.0, le esplorazioni di proprietà di tipi di entità non possono essere dichiarati in tipi di entità derivati, a meno che l'entità di proprietà vengono mappate in modo esplicito a una tabella separata dalla gerarchia di proprietario.</span><span class="sxs-lookup"><span data-stu-id="5870e-165">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="5870e-166">Questa limitazione è stata rimossa in EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="5870e-166">This limitation has been removed in EF Core 2.1</span></span>
- <span data-ttu-id="5870e-167">En EF Core 2.0 e 2.1 unico riferimento le esplorazioni di tipi di proprietà erano supportate.</span><span class="sxs-lookup"><span data-stu-id="5870e-167">En EF Core 2.0 and 2.1 only reference navigations to owned types were supported.</span></span> <span data-ttu-id="5870e-168">Questa limitazione è stata rimossa in EF Core 2.2</span><span class="sxs-lookup"><span data-stu-id="5870e-168">This limitation has been removed in EF Core 2.2</span></span>