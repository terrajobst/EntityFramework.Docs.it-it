---
title: Campi sottoposti a backup-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 288440a4494117fe59d27187e24424c4d2fd44ab
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811882"
---
# <a name="backing-fields"></a><span data-ttu-id="4e335-102">Campi sottostanti</span><span class="sxs-lookup"><span data-stu-id="4e335-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="4e335-103">Questa funzionalità è una novità di EF Core 1,1.</span><span class="sxs-lookup"><span data-stu-id="4e335-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="4e335-104">I campi di backup consentono a EF di leggere e/o scrivere in un campo anziché in una proprietà.</span><span class="sxs-lookup"><span data-stu-id="4e335-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="4e335-105">Questa operazione può essere utile quando si utilizza l'incapsulamento nella classe per limitare l'utilizzo di e/o migliorare la semantica di accesso ai dati da parte del codice dell'applicazione, ma il valore deve essere letto e/o scritto nel database senza utilizzare tali restrizioni/ miglioramenti.</span><span class="sxs-lookup"><span data-stu-id="4e335-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="4e335-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="4e335-106">Conventions</span></span>

<span data-ttu-id="4e335-107">Per convenzione, i campi seguenti vengono individuati come campi di backup per una determinata proprietà (elencati in ordine di precedenza).</span><span class="sxs-lookup"><span data-stu-id="4e335-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="4e335-108">I campi vengono individuati solo per le proprietà incluse nel modello.</span><span class="sxs-lookup"><span data-stu-id="4e335-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="4e335-109">Per ulteriori informazioni su quali proprietà sono incluse nel modello, vedere [inclusione & escluse le proprietà](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="4e335-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

<span data-ttu-id="4e335-110">Quando viene configurato un campo sottostante, EF scrive direttamente in quel campo quando materializzazione le istanze dell'entità dal database, invece di usare il metodo di impostazione della proprietà.</span><span class="sxs-lookup"><span data-stu-id="4e335-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="4e335-111">Se EF deve leggere o scrivere il valore in altri momenti, utilizzerà la proprietà, se possibile.</span><span class="sxs-lookup"><span data-stu-id="4e335-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="4e335-112">Se, ad esempio, EF deve aggiornare il valore di una proprietà, utilizzerà il metodo di impostazione della proprietà, se ne è stato definito uno.</span><span class="sxs-lookup"><span data-stu-id="4e335-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="4e335-113">Se la proprietà è di sola lettura, verrà scritta nel campo.</span><span class="sxs-lookup"><span data-stu-id="4e335-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="4e335-114">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="4e335-114">Data Annotations</span></span>

<span data-ttu-id="4e335-115">Non è possibile configurare i campi di backup con le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="4e335-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="4e335-116">API Fluent</span><span class="sxs-lookup"><span data-stu-id="4e335-116">Fluent API</span></span>

<span data-ttu-id="4e335-117">È possibile usare l'API Fluent per configurare un campo sottostante per una proprietà.</span><span class="sxs-lookup"><span data-stu-id="4e335-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="4e335-118">Controllo quando viene utilizzato il campo</span><span class="sxs-lookup"><span data-stu-id="4e335-118">Controlling when the field is used</span></span>

<span data-ttu-id="4e335-119">È possibile configurare quando EF usa il campo o la proprietà.</span><span class="sxs-lookup"><span data-stu-id="4e335-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="4e335-120">Per le opzioni supportate, vedere l' [enumerazione PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) .</span><span class="sxs-lookup"><span data-stu-id="4e335-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="4e335-121">Campi senza una proprietà</span><span class="sxs-lookup"><span data-stu-id="4e335-121">Fields without a property</span></span>

<span data-ttu-id="4e335-122">È anche possibile creare una proprietà concettuale nel modello che non disponga di una proprietà CLR corrispondente nella classe di entità, ma usa invece un campo per archiviare i dati nell'entità.</span><span class="sxs-lookup"><span data-stu-id="4e335-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="4e335-123">Questo è diverso dalle [proprietà shadow](shadow-properties.md), in cui i dati vengono archiviati nello strumento di rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="4e335-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="4e335-124">Questa operazione viene in genere utilizzata se la classe di entità utilizza metodi per ottenere o impostare i valori.</span><span class="sxs-lookup"><span data-stu-id="4e335-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="4e335-125">È possibile assegnare a EF il nome del campo nell'API `Property(...)`.</span><span class="sxs-lookup"><span data-stu-id="4e335-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="4e335-126">Se non è presente alcuna proprietà con il nome specificato, Entity Framework cercherà un campo.</span><span class="sxs-lookup"><span data-stu-id="4e335-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="4e335-127">Quando non è presente alcuna proprietà nella classe di entità, è possibile usare il metodo `EF.Property(...)` in una query LINQ per fare riferimento alla proprietà che è concettualmente parte del modello.</span><span class="sxs-lookup"><span data-stu-id="4e335-127">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
