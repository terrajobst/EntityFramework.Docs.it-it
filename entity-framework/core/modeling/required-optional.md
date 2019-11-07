---
title: Proprietà obbligatorie e facoltative-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 62b2b3f5a761c0aacece986ecd0b2dd2f958d048
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655655"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="cb998-102">Proprietà obbligatorie e facoltative</span><span class="sxs-lookup"><span data-stu-id="cb998-102">Required and Optional Properties</span></span>

<span data-ttu-id="cb998-103">Una proprietà è considerata facoltativa se è valida affinché contenga `null`.</span><span class="sxs-lookup"><span data-stu-id="cb998-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="cb998-104">Se `null` non è un valore valido da assegnare a una proprietà, viene considerata una proprietà obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="cb998-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

<span data-ttu-id="cb998-105">Quando si esegue il mapping a uno schema di database relazionale, le proprietà obbligatorie vengono create come colonne che non ammettono i valori null e le proprietà facoltative vengono create come colonne nullable.</span><span class="sxs-lookup"><span data-stu-id="cb998-105">When mapping to a relational database schema, required properties are created as non-nullable columns, and optional properties are created as nullable columns.</span></span>

## <a name="conventions"></a><span data-ttu-id="cb998-106">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="cb998-106">Conventions</span></span>

<span data-ttu-id="cb998-107">Per convenzione, una proprietà il cui tipo .NET può contenere null verrà configurato come facoltativo, mentre le proprietà il cui tipo .NET non può contenere valori null verranno configurate in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="cb998-107">By convention, a property whose .NET type can contain null will be configured as optional, whereas properties whose .NET type cannot contain null will be configured as required.</span></span> <span data-ttu-id="cb998-108">Tutte le proprietà con tipi di valore .NET (`int`, `decimal`, `bool`e così via), ad esempio, sono configurate come obbligatorie e tutte le proprietà con tipi di valore .NET Nullable (`int?`, `decimal?`, `bool?`e così via) sono configurate come facoltative.</span><span class="sxs-lookup"><span data-stu-id="cb998-108">For example, all properties with .NET value types (`int`, `decimal`, `bool`, etc.) are configured as required, and all properties with nullable .NET value types (`int?`, `decimal?`, `bool?`, etc.) are configured as optional.</span></span>

<span data-ttu-id="cb998-109">C#8 è stata introdotta una nuova funzionalità denominata [tipi di riferimento Nullable](/dotnet/csharp/tutorials/nullable-reference-types), che consente di aggiungere annotazioni ai tipi di riferimento, indicando se è valido per consentirne l'inclusione null.</span><span class="sxs-lookup"><span data-stu-id="cb998-109">C# 8 introduced a new feature called [nullable reference types](/dotnet/csharp/tutorials/nullable-reference-types), which allows reference types to be annotated, indicating whether it is valid for them to contain null or not.</span></span> <span data-ttu-id="cb998-110">Questa funzionalità è disabilitata per impostazione predefinita e, se abilitata, modifica il comportamento del EF Core nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="cb998-110">This feature is disabled by default, and if enabled, it modifies EF Core's behavior in the following way:</span></span>

* <span data-ttu-id="cb998-111">Se i tipi di riferimento nullable sono disabilitati (impostazione predefinita), tutte le proprietà con i tipi di riferimento .NET vengono configurate come facoltative per convenzione, ad esempio `string`.</span><span class="sxs-lookup"><span data-stu-id="cb998-111">If nullable reference types are disabled (the default), all properties with .NET reference types are configured as optional by convention (e.g. `string`).</span></span>
* <span data-ttu-id="cb998-112">Se sono abilitati i tipi di riferimento Nullable, le proprietà verranno configurate in base al supporto di valori null del tipo .NET: `string?` verranno configurati come facoltativi, mentre `string` verranno configurati in base alle C# esigenze.</span><span class="sxs-lookup"><span data-stu-id="cb998-112">If nullable reference types are enabled, properties will be configured based on the C# nullability of their .NET type: `string?` will be configured as optional, whereas `string` will be configured as required.</span></span>

<span data-ttu-id="cb998-113">Nell'esempio seguente viene illustrato un tipo di entità con proprietà obbligatorie e facoltative, con la funzionalità di riferimento Nullable disabilitata (impostazione predefinita) e abilitata:</span><span class="sxs-lookup"><span data-stu-id="cb998-113">The following example shows an entity type with required and optional properties, with the nullable reference feature disabled (the default) and enabled:</span></span>

# <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[<span data-ttu-id="cb998-114">Senza tipi di riferimento Nullable (impostazione predefinita)</span><span class="sxs-lookup"><span data-stu-id="cb998-114">Without nullable reference types (default)</span></span>](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

# <a name="with-nullable-reference-typestabwith-nrt"></a>[<span data-ttu-id="cb998-115">Con tipi di riferimento Nullable</span><span class="sxs-lookup"><span data-stu-id="cb998-115">With nullable reference types</span></span>](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

<span data-ttu-id="cb998-116">L'uso di tipi di riferimento Nullable è consigliato perché trasmette il supporto di C# valori null espresso nel codice al modello di EF core e al database e evita l'uso dell'API Fluent o delle annotazioni dei dati per esprimere lo stesso concetto due volte.</span><span class="sxs-lookup"><span data-stu-id="cb998-116">Using nullable reference types is recommended since it flows the nullability expressed in C# code to EF Core's model and to the database, and obviates the use of the Fluent API or Data Annotations to express the same concept twice.</span></span>

> [!NOTE]
> <span data-ttu-id="cb998-117">Prestare attenzione quando si abilitano i tipi di riferimento nullable in un progetto esistente: le proprietà del tipo di riferimento che in precedenza erano configurate come facoltative ora verranno configurate come richieste, a meno che non vengano annotate in modo esplicito come Nullable.</span><span class="sxs-lookup"><span data-stu-id="cb998-117">Exercise caution when enabling nullable reference types on an existing project: reference type properties which were previously configured as optional will now be configured as required, unless they are explicitly annotated to be nullable.</span></span> <span data-ttu-id="cb998-118">Quando si gestisce uno schema di database relazionale, è possibile che vengano generate migrazioni che modificano il supporto di valori null per la colonna di database.</span><span class="sxs-lookup"><span data-stu-id="cb998-118">When managing a relational database schema, this may cause migrations to be generated which alter the database column's nullability.</span></span>

<span data-ttu-id="cb998-119">Per ulteriori informazioni sui tipi di riferimento nullable e su come utilizzarli con EF Core, [vedere la pagina della documentazione dedicata per questa funzionalità](xref:core/miscellaneous/nullable-reference-types).</span><span class="sxs-lookup"><span data-stu-id="cb998-119">For more information on nullable reference types and how to use them with EF Core, [see the dedicated documentation page for this feature](xref:core/miscellaneous/nullable-reference-types).</span></span>

## <a name="configuration"></a><span data-ttu-id="cb998-120">Configurazione</span><span class="sxs-lookup"><span data-stu-id="cb998-120">Configuration</span></span>

<span data-ttu-id="cb998-121">Una proprietà che sarebbe facoltativa per convenzione può essere configurata in modo da essere obbligatoria come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="cb998-121">A property that would be optional by convention can be configured to be required as follows:</span></span>

# <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="cb998-122">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="cb998-122">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]

# <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="cb998-123">API Fluent</span><span class="sxs-lookup"><span data-stu-id="cb998-123">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

***
