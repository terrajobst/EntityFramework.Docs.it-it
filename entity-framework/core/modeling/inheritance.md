---
title: Ereditarietà-EF Core
description: Come configurare l'ereditarietà del tipo di entità usando Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 507854e3acc0347adee612e516b3e2e0b10f55cf
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502168"
---
# <a name="inheritance"></a><span data-ttu-id="a7acc-103">Ereditarietà</span><span class="sxs-lookup"><span data-stu-id="a7acc-103">Inheritance</span></span>

<span data-ttu-id="a7acc-104">EF è in grado di eseguire il mapping di una gerarchia di tipi .NET a un database.</span><span class="sxs-lookup"><span data-stu-id="a7acc-104">EF can map a .NET type hierarchy to a database.</span></span> <span data-ttu-id="a7acc-105">In questo modo è possibile scrivere le entità .NET nel codice come di consueto, usando i tipi base e derivati e fare in modo che EF crei facilmente lo schema del database appropriato, emette query e così via. I dettagli effettivi su come viene eseguito il mapping di una gerarchia dei tipi sono dipendenti dal provider. in questa pagina viene descritto il supporto dell'ereditarietà nel contesto di un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="a7acc-105">This allows you to write your .NET entities in code as usual, using base and derived types, and have EF seamlessly create the appropriate database schema, issue queries, etc. The actual details of how a type hierarchy is mapped are provider-dependent; this page describes inheritance support in the context of a relational database.</span></span>

<span data-ttu-id="a7acc-106">Al momento, EF Core supporta solo il modello tabella per gerarchia (TPH).</span><span class="sxs-lookup"><span data-stu-id="a7acc-106">At the moment, EF Core only supports the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="a7acc-107">TPH usa una singola tabella per archiviare i dati per tutti i tipi nella gerarchia e viene usata una colonna discriminatore per identificare il tipo rappresentato da ogni riga.</span><span class="sxs-lookup"><span data-stu-id="a7acc-107">TPH uses a single table to store the data for all types in the hierarchy, and a discriminator column is used to identify which type each row represents.</span></span>

> [!NOTE]
> <span data-ttu-id="a7acc-108">La tabella per tipo (TPT) e la tabella per tipo concreto (TPC), supportati da EF6, non sono ancora supportate da EF Core.</span><span class="sxs-lookup"><span data-stu-id="a7acc-108">The table-per-type (TPT) and table-per-concrete-type (TPC), which are supported by EF6, are not yet supported by EF Core.</span></span> <span data-ttu-id="a7acc-109">TPT è una funzionalità principale progettata per EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="a7acc-109">TPT is a major feature planned for EF Core 5.0.</span></span>

## <a name="entity-type-hierarchy-mapping"></a><span data-ttu-id="a7acc-110">Mapping della gerarchia dei tipi di entità</span><span class="sxs-lookup"><span data-stu-id="a7acc-110">Entity type hierarchy mapping</span></span>

<span data-ttu-id="a7acc-111">Per convenzione, EF configurerà l'ereditarietà solo se due o più tipi ereditati vengono inclusi in modo esplicito nel modello.</span><span class="sxs-lookup"><span data-stu-id="a7acc-111">By convention, EF will only set up inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="a7acc-112">EF non analizzerà automaticamente i tipi di base o derivati che non sono inclusi nel modello.</span><span class="sxs-lookup"><span data-stu-id="a7acc-112">EF will not automatically scan for base or derived types that are not otherwise included in the model.</span></span>

<span data-ttu-id="a7acc-113">È possibile includere tipi nel modello esponendo un DbSet per ogni tipo nella gerarchia di ereditarietà:</span><span class="sxs-lookup"><span data-stu-id="a7acc-113">You can include types in the model by exposing a DbSet for each type in the inheritance hierarchy:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?name=InheritanceDbSets&highlight=3-4)]

<span data-ttu-id="a7acc-114">È necessario eseguire il mapping di questo modello allo schema di database seguente (si noti la colonna *Discriminator* creata in modo implicito, che identifica il tipo di *Blog* archiviato in ogni riga):</span><span class="sxs-lookup"><span data-stu-id="a7acc-114">This model be mapped to the following database schema (note the implicitly-created *Discriminator* column, which identifies which type of *Blog* is stored in each row):</span></span>

![immagine](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="a7acc-116">Le colonne del database vengono rese automaticamente Nullable quando necessario quando si usa il mapping di TPH.</span><span class="sxs-lookup"><span data-stu-id="a7acc-116">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span> <span data-ttu-id="a7acc-117">Ad esempio, la colonna *RssUrl* ammette i valori null perché le istanze di *Blog* normali non hanno tale proprietà.</span><span class="sxs-lookup"><span data-stu-id="a7acc-117">For example, the *RssUrl* column is nullable because regular *Blog* instances do not have that property.</span></span>

<span data-ttu-id="a7acc-118">Se non si vuole esporre un DbSet per una o più entità nella gerarchia, è anche possibile usare l'API Fluent per assicurarsi che siano incluse nel modello.</span><span class="sxs-lookup"><span data-stu-id="a7acc-118">If you don't want to expose a DbSet for one or more entities in the hierarchy, you can also use the Fluent API to ensure they are included in the model.</span></span>

> [!TIP]
> <span data-ttu-id="a7acc-119">Se non si basano sulle convenzioni, è possibile specificare il tipo di base in modo esplicito utilizzando `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="a7acc-119">If you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span> <span data-ttu-id="a7acc-120">È inoltre possibile utilizzare `.HasBaseType((Type)null)` per rimuovere un tipo di entità dalla gerarchia.</span><span class="sxs-lookup"><span data-stu-id="a7acc-120">You can also use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="discriminator-configuration"></a><span data-ttu-id="a7acc-121">Configurazione discriminatore</span><span class="sxs-lookup"><span data-stu-id="a7acc-121">Discriminator configuration</span></span>

<span data-ttu-id="a7acc-122">È possibile configurare il nome e il tipo della colonna discriminatore e i valori utilizzati per identificare ogni tipo nella gerarchia:</span><span class="sxs-lookup"><span data-stu-id="a7acc-122">You can configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorConfiguration.cs?name=DiscriminatorConfiguration&highlight=4-6)]

<span data-ttu-id="a7acc-123">Negli esempi precedenti, EF ha aggiunto il discriminatore in modo implicito come [proprietà shadow](xref:core/modeling/shadow-properties) nell'entità di base della gerarchia.</span><span class="sxs-lookup"><span data-stu-id="a7acc-123">In the examples above, EF added the discriminator implicitly as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="a7acc-124">Questa proprietà può essere configurata come qualsiasi altra:</span><span class="sxs-lookup"><span data-stu-id="a7acc-124">This property can be configured like any other:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorPropertyConfiguration.cs?name=DiscriminatorPropertyConfiguration&highlight=4-5)]

<span data-ttu-id="a7acc-125">Infine, è anche possibile eseguire il mapping del discriminatore a una normale proprietà .NET nell'entità:</span><span class="sxs-lookup"><span data-stu-id="a7acc-125">Finally, the discriminator can also be mapped to a regular .NET property in your entity:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs?name=NonShadowDiscriminator&highlight=4)]

## <a name="shared-columns"></a><span data-ttu-id="a7acc-126">Colonne condivise</span><span class="sxs-lookup"><span data-stu-id="a7acc-126">Shared columns</span></span>

<span data-ttu-id="a7acc-127">Per impostazione predefinita, quando due tipi di entità di pari livello nella gerarchia hanno una proprietà con lo stesso nome, verrà eseguito il mapping a due colonne separate.</span><span class="sxs-lookup"><span data-stu-id="a7acc-127">By default, when two sibling entity types in the hierarchy have a property with the same name, they will be mapped to two separate columns.</span></span> <span data-ttu-id="a7acc-128">Tuttavia, se il tipo è identico, è possibile eseguirne il mapping alla stessa colonna di database:</span><span class="sxs-lookup"><span data-stu-id="a7acc-128">However, if their type is identical they can be mapped to the same database column:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs?name=SharedTPHColumns&highlight=9,13)]
