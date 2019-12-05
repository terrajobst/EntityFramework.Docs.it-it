---
title: Chiavi (primario)-EF Core
description: Come configurare le chiavi per i tipi di entità quando si usa Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: fdaccb42259ba9dad97a05c626edd0291ca96cb0
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824616"
---
# <a name="keys-primary"></a><span data-ttu-id="66e20-103">Chiavi (primarie)</span><span class="sxs-lookup"><span data-stu-id="66e20-103">Keys (primary)</span></span>

<span data-ttu-id="66e20-104">Una chiave funge da identificatore univoco primario per ogni istanza di entità.</span><span class="sxs-lookup"><span data-stu-id="66e20-104">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="66e20-105">Quando si utilizza un database relazionale, viene eseguito il mapping al concetto di *chiave primaria*.</span><span class="sxs-lookup"><span data-stu-id="66e20-105">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="66e20-106">È anche possibile configurare un identificatore univoco che non sia la chiave primaria. per ulteriori informazioni, vedere [chiavi alternative](alternate-keys.md) .</span><span class="sxs-lookup"><span data-stu-id="66e20-106">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<span data-ttu-id="66e20-107">Per configurare o creare una chiave primaria, è possibile utilizzare uno dei metodi seguenti.</span><span class="sxs-lookup"><span data-stu-id="66e20-107">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="66e20-108">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="66e20-108">Conventions</span></span>

<span data-ttu-id="66e20-109">Per impostazione predefinita, una proprietà denominata `Id` o `<type name>Id` verrà configurata come chiave di un'entità.</span><span class="sxs-lookup"><span data-stu-id="66e20-109">By default, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyId&highlight=3)]

> [!NOTE]
> <span data-ttu-id="66e20-110">I [tipi di entità di proprietà](xref:core/modeling/owned-entities) utilizzano regole diverse per definire le chiavi.</span><span class="sxs-lookup"><span data-stu-id="66e20-110">[Owned entity types](xref:core/modeling/owned-entities) use different rules to define keys.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="66e20-111">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="66e20-111">Data Annotations</span></span>

<span data-ttu-id="66e20-112">È possibile utilizzare le annotazioni dei dati per configurare una singola proprietà come chiave di un'entità.</span><span class="sxs-lookup"><span data-stu-id="66e20-112">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="66e20-113">API Fluent</span><span class="sxs-lookup"><span data-stu-id="66e20-113">Fluent API</span></span>

<span data-ttu-id="66e20-114">È possibile usare l'API Fluent per configurare una singola proprietà come chiave di un'entità.</span><span class="sxs-lookup"><span data-stu-id="66e20-114">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="66e20-115">È anche possibile usare l'API Fluent per configurare più proprietà come chiave di un'entità, nota come chiave composta.</span><span class="sxs-lookup"><span data-stu-id="66e20-115">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="66e20-116">Le chiavi composite possono essere configurate solo usando le convenzioni API Fluent non installeranno mai una chiave composta e non è possibile usare le annotazioni dei dati per configurarne una.</span><span class="sxs-lookup"><span data-stu-id="66e20-116">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]

## <a name="key-types-and-values"></a><span data-ttu-id="66e20-117">Tipi di chiave e valori</span><span class="sxs-lookup"><span data-stu-id="66e20-117">Key types and values</span></span>

<span data-ttu-id="66e20-118">EF Core supporta l'utilizzo di proprietà di qualsiasi tipo primitivo come chiave primaria, inclusi `string`, `Guid`, `byte[]` e altri.</span><span class="sxs-lookup"><span data-stu-id="66e20-118">EF Core supports using properties of any primitive type as the primary key, including `string`, `Guid`, `byte[]` and others.</span></span> <span data-ttu-id="66e20-119">Ma non tutti i database li supportano.</span><span class="sxs-lookup"><span data-stu-id="66e20-119">But not all databases support them.</span></span> <span data-ttu-id="66e20-120">In alcuni casi i valori di chiave possono essere convertiti automaticamente in un tipo supportato. in caso contrario, la conversione deve essere [specificata manualmente](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="66e20-120">In some cases the key values can be converted to a supported type automatically, otherwise the conversion should be [specified manually](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="66e20-121">Quando si aggiunge una nuova entità al contesto, le proprietà chiave devono sempre avere un valore non predefinito, ma alcuni tipi verranno [generati dal database](xref:core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="66e20-121">Key properties must always have a non-default value when adding a new entity to the context, but some types will be [generated by the database](xref:core/modeling/generated-properties).</span></span> <span data-ttu-id="66e20-122">In tal caso, EF tenterà di generare un valore temporaneo quando l'entità viene aggiunta per finalità di rilevamento.</span><span class="sxs-lookup"><span data-stu-id="66e20-122">In that case EF will try to generate a temporary value when the entity is added for tracking purposes.</span></span> <span data-ttu-id="66e20-123">Dopo la chiamata a [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) , il valore temporaneo verrà sostituito dal valore generato dal database.</span><span class="sxs-lookup"><span data-stu-id="66e20-123">After [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) is called the temporary value will be replaced by the value generated by the database.</span></span>

> [!Important]
> <span data-ttu-id="66e20-124">Se una proprietà chiave ha un valore generato dal database e viene specificato un valore non predefinito quando viene aggiunta un'entità, EF presuppone che l'entità esista già nel database e tenterà di aggiornarla invece di inserirne una nuova.</span><span class="sxs-lookup"><span data-stu-id="66e20-124">If a key property has value generated by the database and a non-default value is specified when an entity is added then EF will assume that the entity already exists in the database and will try to update it instead of inserting a new one.</span></span> <span data-ttu-id="66e20-125">Per evitare questo problema, disabilitare la generazione di valori o vedere [come specificare valori espliciti per le proprietà generate](../saving/explicit-values-generated-properties.md).</span><span class="sxs-lookup"><span data-stu-id="66e20-125">To avoid this turn off value generation or see [how to specify explicit values for generated properties](../saving/explicit-values-generated-properties.md).</span></span>