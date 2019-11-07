---
title: Indici-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: d1b5cd6853cd24f7e1aa3aace2f0a3455c657cc1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655703"
---
# <a name="indexes"></a><span data-ttu-id="8947f-102">Indexes</span><span class="sxs-lookup"><span data-stu-id="8947f-102">Indexes</span></span>

<span data-ttu-id="8947f-103">Gli indici sono un concetto comune in molti archivi dati.</span><span class="sxs-lookup"><span data-stu-id="8947f-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="8947f-104">Mentre la loro implementazione nell'archivio dati può variare, vengono usati per rendere più efficienti le ricerche basate su una colonna o un set di colonne.</span><span class="sxs-lookup"><span data-stu-id="8947f-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="8947f-105">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="8947f-105">Conventions</span></span>

<span data-ttu-id="8947f-106">Per convenzione, viene creato un indice in ogni proprietà o set di proprietà utilizzate come chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="8947f-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="8947f-107">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="8947f-107">Data Annotations</span></span>

<span data-ttu-id="8947f-108">Non è possibile creare indici utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="8947f-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="8947f-109">API Fluent</span><span class="sxs-lookup"><span data-stu-id="8947f-109">Fluent API</span></span>

<span data-ttu-id="8947f-110">È possibile usare l'API Fluent per specificare un indice in una singola proprietà.</span><span class="sxs-lookup"><span data-stu-id="8947f-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="8947f-111">Per impostazione predefinita, gli indici non sono univoci.</span><span class="sxs-lookup"><span data-stu-id="8947f-111">By default, indexes are non-unique.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=7,8)]

<span data-ttu-id="8947f-112">È inoltre possibile specificare che un indice deve essere univoco, pertanto due entità non possono avere gli stessi valori per le proprietà specificate.</span><span class="sxs-lookup"><span data-stu-id="8947f-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=ModelBuilder&highlight=3)]

<span data-ttu-id="8947f-113">È inoltre possibile specificare un indice per più di una colonna.</span><span class="sxs-lookup"><span data-stu-id="8947f-113">You can also specify an index over more than one column.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=7,8)]

> [!TIP]  
> <span data-ttu-id="8947f-114">È disponibile un solo indice per set di proprietà distinti.</span><span class="sxs-lookup"><span data-stu-id="8947f-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="8947f-115">Se si usa l'API Fluent per configurare un indice in un set di proprietà in cui è già definito un indice, per convenzione o per la configurazione precedente, si modificherà la definizione di tale indice.</span><span class="sxs-lookup"><span data-stu-id="8947f-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="8947f-116">Questa opzione è utile se si desidera configurare ulteriormente un indice creato per convenzione.</span><span class="sxs-lookup"><span data-stu-id="8947f-116">This is useful if you want to further configure an index that was created by convention.</span></span>
