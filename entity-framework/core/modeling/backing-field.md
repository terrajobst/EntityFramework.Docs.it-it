---
title: Campi sottoposti a backup-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 20cf9dc9b0d556f29680bce588bcbdc4ea48fa74
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417309"
---
# <a name="backing-fields"></a><span data-ttu-id="4bda2-102">Campi sottostanti</span><span class="sxs-lookup"><span data-stu-id="4bda2-102">Backing Fields</span></span>

<span data-ttu-id="4bda2-103">I campi di backup consentono a EF di leggere e/o scrivere in un campo anziché in una proprietà.</span><span class="sxs-lookup"><span data-stu-id="4bda2-103">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="4bda2-104">Questa operazione può essere utile quando si utilizza l'incapsulamento della classe per limitare l'utilizzo di e/o migliorare la semantica di accesso ai dati da parte del codice dell'applicazione, ma il valore deve essere letto e/o scritto nel database senza utilizzare tali restrizioni/miglioramenti.</span><span class="sxs-lookup"><span data-stu-id="4bda2-104">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="basic-configuration"></a><span data-ttu-id="4bda2-105">Configurazione di base</span><span class="sxs-lookup"><span data-stu-id="4bda2-105">Basic configuration</span></span>

<span data-ttu-id="4bda2-106">Per convenzione, i campi seguenti vengono individuati come campi di backup per una determinata proprietà (elencati in ordine di precedenza).</span><span class="sxs-lookup"><span data-stu-id="4bda2-106">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> 

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

<span data-ttu-id="4bda2-107">Nell'esempio seguente, la proprietà `Url` è configurata in modo da avere `_url` come campo di supporto:</span><span class="sxs-lookup"><span data-stu-id="4bda2-107">In the following sample, the `Url` property is configured to have `_url` as its backing field:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

<span data-ttu-id="4bda2-108">Si noti che i campi sottoposti a backup vengono individuati solo per le proprietà incluse nel modello.</span><span class="sxs-lookup"><span data-stu-id="4bda2-108">Note that backing fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="4bda2-109">Per ulteriori informazioni su quali proprietà sono incluse nel modello, vedere [inclusione & escluse le proprietà](included-properties.md).</span><span class="sxs-lookup"><span data-stu-id="4bda2-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

<span data-ttu-id="4bda2-110">È anche possibile configurare i campi di supporto in modo esplicito, ad esempio se il nome del campo non corrisponde alle convenzioni precedenti:</span><span class="sxs-lookup"><span data-stu-id="4bda2-110">You can also configure backing fields explicitly, e.g. if the field name doesn't correspond to the above conventions:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs?name=BackingField&highlight=5)]

## <a name="field-and-property-access"></a><span data-ttu-id="4bda2-111">Accesso ai campi e alle proprietà</span><span class="sxs-lookup"><span data-stu-id="4bda2-111">Field and property access</span></span>

<span data-ttu-id="4bda2-112">Per impostazione predefinita, EF leggerà e scriverà sempre nel campo sottostante, presumendo che sia stato configurato correttamente, e non userà mai la proprietà.</span><span class="sxs-lookup"><span data-stu-id="4bda2-112">By default, EF will always read and write to the backing field - assuming one has been properly configured - and will never use the property.</span></span> <span data-ttu-id="4bda2-113">Tuttavia, EF supporta anche altri modelli di accesso.</span><span class="sxs-lookup"><span data-stu-id="4bda2-113">However, EF also supports other access patterns.</span></span> <span data-ttu-id="4bda2-114">Ad esempio, l'esempio seguente indica a EF di scrivere nel campo sottostante solo quando materializzazione e di usare la proprietà in tutti gli altri casi:</span><span class="sxs-lookup"><span data-stu-id="4bda2-114">For example, the following sample instructs EF to write to the backing field only while materializing, and to use the property in all other cases:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs?name=BackingFieldAccessMode&highlight=6)]

<span data-ttu-id="4bda2-115">Per il set completo di opzioni supportate, vedere l' [enumerazione PropertyAccessMode](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) .</span><span class="sxs-lookup"><span data-stu-id="4bda2-115">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the complete set of supported options.</span></span>

> [!NOTE]
> <span data-ttu-id="4bda2-116">Con EF Core 3,0, la modalità di accesso alla proprietà predefinita è cambiata da `PreferFieldDuringConstruction` a `PreferField`.</span><span class="sxs-lookup"><span data-stu-id="4bda2-116">With EF Core 3.0, the default property access mode changed from `PreferFieldDuringConstruction` to `PreferField`.</span></span>

## <a name="field-only-properties"></a><span data-ttu-id="4bda2-117">Proprietà solo campo</span><span class="sxs-lookup"><span data-stu-id="4bda2-117">Field-only properties</span></span>

<span data-ttu-id="4bda2-118">È anche possibile creare una proprietà concettuale nel modello che non disponga di una proprietà CLR corrispondente nella classe di entità, ma usa invece un campo per archiviare i dati nell'entità.</span><span class="sxs-lookup"><span data-stu-id="4bda2-118">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="4bda2-119">Questo è diverso dalle [proprietà shadow](shadow-properties.md), in cui i dati vengono archiviati nello strumento di rilevamento delle modifiche, anziché nel tipo CLR dell'entità.</span><span class="sxs-lookup"><span data-stu-id="4bda2-119">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker, rather than in the entity's CLR type.</span></span> <span data-ttu-id="4bda2-120">Le proprietà di solo campo vengono in genere utilizzate quando la classe di entità utilizza metodi anziché proprietà per ottenere o impostare valori oppure nei casi in cui i campi non devono essere esposti nel modello di dominio, ad esempio le chiavi primarie.</span><span class="sxs-lookup"><span data-stu-id="4bda2-120">Field-only properties are commonly used when the entity class uses methods instead of properties to get/set values, or in cases where fields shouldn't be exposed at all in the domain model (e.g. primary keys).</span></span>

<span data-ttu-id="4bda2-121">È possibile configurare una proprietà di solo campo fornendo un nome nell'API `Property(...)`:</span><span class="sxs-lookup"><span data-stu-id="4bda2-121">You can configure a field-only property by providing a name in the `Property(...)` API:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="4bda2-122">EF tenterà di trovare una proprietà CLR con il nome specificato o un campo se non viene trovata una proprietà.</span><span class="sxs-lookup"><span data-stu-id="4bda2-122">EF will attempt to find a CLR property with the given name, or a field if a property isn't found.</span></span> <span data-ttu-id="4bda2-123">Se non viene trovata alcuna proprietà né un campo, verrà invece configurata una proprietà shadow.</span><span class="sxs-lookup"><span data-stu-id="4bda2-123">If neither a property nor a field are found, a shadow property will be set up instead.</span></span>

<span data-ttu-id="4bda2-124">Potrebbe essere necessario fare riferimento a una proprietà solo campo dalle query LINQ, ma tali campi sono in genere privati.</span><span class="sxs-lookup"><span data-stu-id="4bda2-124">You may need to refer to a field-only property from LINQ queries, but such fields are typically private.</span></span> <span data-ttu-id="4bda2-125">È possibile usare il metodo `EF.Property(...)` in una query LINQ per fare riferimento al campo:</span><span class="sxs-lookup"><span data-stu-id="4bda2-125">You can use the `EF.Property(...)` method in a LINQ query to refer to the field:</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
