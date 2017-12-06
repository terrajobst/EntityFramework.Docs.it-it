---
title: Il backup di base EF campi-
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
ms.technology: entity-framework-core
uid: core/modeling/backing-field
ms.openlocfilehash: e95417b3368d09a64f9ec02ae40c38dc770c2e6b
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2017
---
# <a name="backing-fields"></a><span data-ttu-id="f9e59-102">Campi sottostanti</span><span class="sxs-lookup"><span data-stu-id="f9e59-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="f9e59-103">Questa funzionalità è nuova in Entity Framework Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="f9e59-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="f9e59-104">Campi sottostanti consentono EF leggere e/o scrittura a un campo anziché a una proprietà.</span><span class="sxs-lookup"><span data-stu-id="f9e59-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="f9e59-105">Ciò può rivelarsi utile incapsulamento nella classe viene utilizzata per limitare l'utilizzo di e/o migliorare la semantica dell'accesso ai dati dal codice dell'applicazione, ma il valore deve essere letto da e/o scritto nel database senza l'utilizzo di tali restrizioni / miglioramenti.</span><span class="sxs-lookup"><span data-stu-id="f9e59-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="f9e59-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="f9e59-106">Conventions</span></span>

<span data-ttu-id="f9e59-107">Per convenzione, i campi seguenti verranno individuati come il backup di campi per una determinata proprietà (elencati in ordine di precedenza).</span><span class="sxs-lookup"><span data-stu-id="f9e59-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="f9e59-108">I campi vengono individuati solo per le proprietà che vengono incluse nel modello.</span><span class="sxs-lookup"><span data-stu-id="f9e59-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="f9e59-109">Per ulteriori informazioni, in cui sono incluse le proprietà nel modello, vedere [inclusi & esclusione proprietà](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="f9e59-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

<span data-ttu-id="f9e59-110">Quando un campo sottostante è configurato, EF scriverà direttamente a tale campo durante la materializzazione di istanze di entità dal database (anziché il setter di proprietà).</span><span class="sxs-lookup"><span data-stu-id="f9e59-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="f9e59-111">Se Entity Framework deve leggere o scrivere il valore in altri momenti, utilizzerà la proprietà se possibile.</span><span class="sxs-lookup"><span data-stu-id="f9e59-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="f9e59-112">Ad esempio, se è necessario aggiornare il valore per una proprietà EF, utilizzerà il setter della proprietà se definita.</span><span class="sxs-lookup"><span data-stu-id="f9e59-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="f9e59-113">Se la proprietà è di sola lettura, scriverà nel campo.</span><span class="sxs-lookup"><span data-stu-id="f9e59-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f9e59-114">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="f9e59-114">Data Annotations</span></span>

<span data-ttu-id="f9e59-115">Il backup di campi non può essere configurato con le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="f9e59-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="f9e59-116">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="f9e59-116">Fluent API</span></span>

<span data-ttu-id="f9e59-117">Per configurare un campo sottostante per una proprietà, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="f9e59-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="f9e59-118">Quando viene usato il campo di controllo</span><span class="sxs-lookup"><span data-stu-id="f9e59-118">Controlling when the field is used</span></span>

<span data-ttu-id="f9e59-119">È possibile configurare quando Entity Framework Usa il campo o proprietà.</span><span class="sxs-lookup"><span data-stu-id="f9e59-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="f9e59-120">Vedere il [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) per le opzioni supportate.</span><span class="sxs-lookup"><span data-stu-id="f9e59-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="f9e59-121">Campi senza una proprietà</span><span class="sxs-lookup"><span data-stu-id="f9e59-121">Fields without a property</span></span>

<span data-ttu-id="f9e59-122">È anche possibile creare una proprietà concettuale del modello che non dispone di una proprietà CLR corrispondente nella classe di entità, ma utilizza invece un campo per archiviare i dati nell'entità.</span><span class="sxs-lookup"><span data-stu-id="f9e59-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="f9e59-123">Questo comportamento è diverso da [proprietà Shadow](shadow-properties.md), in cui i dati vengono archiviati nel rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="f9e59-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="f9e59-124">Questo viene in genere utilizzato se la classe di entità vengono utilizzati metodi per ottenere o impostare i valori.</span><span class="sxs-lookup"><span data-stu-id="f9e59-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="f9e59-125">È possibile assegnare il nome del campo di EF il `Property(...)` API.</span><span class="sxs-lookup"><span data-stu-id="f9e59-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="f9e59-126">Se è presente alcuna proprietà con il nome specificato, Entity Framework cercherà un campo.</span><span class="sxs-lookup"><span data-stu-id="f9e59-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="f9e59-127">È inoltre possibile assegnare la proprietà di un nome diverso dal nome del campo.</span><span class="sxs-lookup"><span data-stu-id="f9e59-127">You can also choose to give the property a name, other than the field name.</span></span> <span data-ttu-id="f9e59-128">Questo nome viene quindi utilizzato durante la creazione del modello, in particolare verrà utilizzato per il nome della colonna che viene eseguito il mapping per il database.</span><span class="sxs-lookup"><span data-stu-id="f9e59-128">This name is then used when creating the model, most notably it will be used for the column name that is mapped to in the database.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

<span data-ttu-id="f9e59-129">Quando è presente alcuna proprietà nella classe di entità, è possibile utilizzare il `EF.Property(...)` metodo in una query LINQ per fare riferimento alla proprietà a livello concettuale che fa parte del modello.</span><span class="sxs-lookup"><span data-stu-id="f9e59-129">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
