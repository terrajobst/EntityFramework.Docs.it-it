---
title: Indici-EF Core
author: roji
ms.date: 12/16/2019
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 810fccc0c6b035f515107601b245811f7b4118a6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416395"
---
# <a name="indexes"></a><span data-ttu-id="8a910-102">Indici</span><span class="sxs-lookup"><span data-stu-id="8a910-102">Indexes</span></span>

<span data-ttu-id="8a910-103">Gli indici sono un concetto comune in molti archivi dati.</span><span class="sxs-lookup"><span data-stu-id="8a910-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="8a910-104">Mentre la loro implementazione nell'archivio dati può variare, vengono usati per rendere più efficienti le ricerche basate su una colonna o un set di colonne.</span><span class="sxs-lookup"><span data-stu-id="8a910-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

<span data-ttu-id="8a910-105">Non è possibile creare indici utilizzando le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="8a910-105">Indexes cannot be created using data annotations.</span></span> <span data-ttu-id="8a910-106">È possibile usare l'API Fluent per specificare un indice in una singola colonna come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="8a910-106">You can use the Fluent API to specify an index on a single column as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=4)]

<span data-ttu-id="8a910-107">È anche possibile specificare un indice su più di una colonna:</span><span class="sxs-lookup"><span data-stu-id="8a910-107">You can also specify an index over more than one column:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=4)]

> [!NOTE]
> <span data-ttu-id="8a910-108">Per convenzione, viene creato un indice in ogni proprietà o set di proprietà utilizzate come chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="8a910-108">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>
>
> <span data-ttu-id="8a910-109">EF Core supporta solo un indice per set di proprietà distinti.</span><span class="sxs-lookup"><span data-stu-id="8a910-109">EF Core only supports one index per distinct set of properties.</span></span> <span data-ttu-id="8a910-110">Se si usa l'API Fluent per configurare un indice in un set di proprietà in cui è già definito un indice, per convenzione o per la configurazione precedente, si modificherà la definizione di tale indice.</span><span class="sxs-lookup"><span data-stu-id="8a910-110">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="8a910-111">Questa opzione è utile se si desidera configurare ulteriormente un indice creato per convenzione.</span><span class="sxs-lookup"><span data-stu-id="8a910-111">This is useful if you want to further configure an index that was created by convention.</span></span>

## <a name="index-uniqueness"></a><span data-ttu-id="8a910-112">Univocità indice</span><span class="sxs-lookup"><span data-stu-id="8a910-112">Index uniqueness</span></span>

<span data-ttu-id="8a910-113">Per impostazione predefinita, gli indici non sono univoci: più righe possono avere gli stessi valori per il set di colonne dell'indice.</span><span class="sxs-lookup"><span data-stu-id="8a910-113">By default, indexes aren't unique: multiple rows are allowed to have the same value(s) for the index's column set.</span></span> <span data-ttu-id="8a910-114">È possibile rendere univoco un indice nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="8a910-114">You can make an index unique as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=IndexUnique&highlight=5)]

<span data-ttu-id="8a910-115">Se si tenta di inserire più di un'entità con gli stessi valori per il set di colonne dell'indice, verrà generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="8a910-115">Attempting to insert more than one entity with the same values for the index's column set will cause an exception to be thrown.</span></span>

## <a name="index-name"></a><span data-ttu-id="8a910-116">Nome dell'indice</span><span class="sxs-lookup"><span data-stu-id="8a910-116">Index name</span></span>

<span data-ttu-id="8a910-117">Per convenzione, gli indici creati in un database relazionale vengono denominati `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="8a910-117">By convention, indexes created in a relational database are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="8a910-118">Per gli indici composti, `<property name>` diventa un elenco di nomi di proprietà separato da un carattere di sottolineatura.</span><span class="sxs-lookup"><span data-stu-id="8a910-118">For composite indexes, `<property name>` becomes an underscore separated list of property names.</span></span>

<span data-ttu-id="8a910-119">È possibile utilizzare l'API Fluent per impostare il nome dell'indice creato nel database:</span><span class="sxs-lookup"><span data-stu-id="8a910-119">You can use the Fluent API to set the name of the index created in the database:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexName.cs?name=IndexName&highlight=5)]

## <a name="index-filter"></a><span data-ttu-id="8a910-120">Filtro indice</span><span class="sxs-lookup"><span data-stu-id="8a910-120">Index filter</span></span>

<span data-ttu-id="8a910-121">Alcuni database relazionali consentono di specificare un indice filtrato o parziale.</span><span class="sxs-lookup"><span data-stu-id="8a910-121">Some relational databases allow you to specify a filtered or partial index.</span></span> <span data-ttu-id="8a910-122">Ciò consente di indicizzare solo un subset di valori di una colonna, riducendo le dimensioni dell'indice e migliorando le prestazioni e l'utilizzo dello spazio su disco.</span><span class="sxs-lookup"><span data-stu-id="8a910-122">This allows you to index only a subset of a column's values, reducing the index's size and improving both performance and disk space usage.</span></span> <span data-ttu-id="8a910-123">Per ulteriori informazioni su SQL Server indici filtrati, [vedere la documentazione](https://docs.microsoft.com/sql/relational-databases/indexes/create-filtered-indexes)di.</span><span class="sxs-lookup"><span data-stu-id="8a910-123">For more information on SQL Server filtered indexes, [see the documentation](https://docs.microsoft.com/sql/relational-databases/indexes/create-filtered-indexes).</span></span>

<span data-ttu-id="8a910-124">È possibile utilizzare l'API Fluent per specificare un filtro per un indice, fornito come espressione SQL:</span><span class="sxs-lookup"><span data-stu-id="8a910-124">You can use the Fluent API to specify a filter on an index, provided as a SQL expression:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexFilter.cs?name=IndexFilter&highlight=5)]

<span data-ttu-id="8a910-125">Quando si usa il provider di SQL Server EF aggiunge un filtro `'IS NOT NULL'` per tutte le colonne nullable che fanno parte di un indice univoco.</span><span class="sxs-lookup"><span data-stu-id="8a910-125">When using the SQL Server provider EF adds an `'IS NOT NULL'` filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="8a910-126">Per eseguire l'override di questa convenzione è possibile specificare un valore `null`.</span><span class="sxs-lookup"><span data-stu-id="8a910-126">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexNoFilter.cs?name=IndexNoFilter&highlight=6)]

## <a name="included-columns"></a><span data-ttu-id="8a910-127">included_columns</span><span class="sxs-lookup"><span data-stu-id="8a910-127">Included columns</span></span>

<span data-ttu-id="8a910-128">Alcuni database relazionali consentono di configurare un set di colonne che vengono incluse nell'indice, ma non fanno parte della relativa chiave.</span><span class="sxs-lookup"><span data-stu-id="8a910-128">Some relational databases allow you to configure a set of columns which get included in the index, but aren't part of its "key".</span></span> <span data-ttu-id="8a910-129">Questo può migliorare significativamente le prestazioni di esecuzione delle query quando tutte le colonne della query sono incluse nell'indice come colonne chiave o colonne, in quanto non è necessario accedere alla tabella stessa.</span><span class="sxs-lookup"><span data-stu-id="8a910-129">This can significantly improve query performance when all columns in the query are included in the index either as key or nonkey columns, as the table itself doesn't need to be accessed.</span></span> <span data-ttu-id="8a910-130">Per ulteriori informazioni su SQL Server colonne incluse, [vedere la documentazione](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns)di.</span><span class="sxs-lookup"><span data-stu-id="8a910-130">For more information on SQL Server included columns, [see the documentation](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns).</span></span>

<span data-ttu-id="8a910-131">Nell'esempio seguente, la colonna `Url` fa parte della chiave di indice, pertanto qualsiasi filtro di query su tale colonna può utilizzare l'indice.</span><span class="sxs-lookup"><span data-stu-id="8a910-131">In the following example, the `Url` column is part of the index key, so any query filtering on that column can use the index.</span></span> <span data-ttu-id="8a910-132">Inoltre, le query che accedono solo alle colonne `Title` e `PublishedOn` non dovranno accedere alla tabella e vengono eseguite in modo più efficiente:</span><span class="sxs-lookup"><span data-stu-id="8a910-132">But in addition, queries accessing only the `Title` and `PublishedOn` columns will not need to access the table and will run more efficiently:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexInclude.cs?name=IndexInclude&highlight=5-9)]
