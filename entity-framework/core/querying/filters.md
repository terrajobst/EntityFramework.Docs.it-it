---
title: Filtri di query globali - EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: c9bbb8a5889834ea078ddb7e432863b3d0cf2ffe
ms.sourcegitcommit: 0cc9578fd49802789a00c0044b4e57325476ca2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/04/2019
ms.locfileid: "70271451"
---
# <a name="global-query-filters"></a><span data-ttu-id="b2ae4-102">Filtri di query globali</span><span class="sxs-lookup"><span data-stu-id="b2ae4-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="b2ae4-103">Questa funzionalità è stata introdotta in EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="b2ae4-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="b2ae4-104">I filtri di query globali sono predicati di query LINQ (un'espressione booleana in genere passata all'operatore di query *Where* di LINQ) applicati ai tipi di entità nel modello di metadati (solitamente in *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="b2ae4-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="b2ae4-105">Questi filtri vengono automaticamente applicati a qualsiasi query LINQ che interessa questi tipi di entità, inclusi quelli a cui si fa riferimento in modo indiretto, ad esempio tramite l'uso di riferimenti alle proprietà Include o di navigazione diretta.</span><span class="sxs-lookup"><span data-stu-id="b2ae4-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="b2ae4-106">Alcune applicazioni comuni di questa funzionalità sono:</span><span class="sxs-lookup"><span data-stu-id="b2ae4-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="b2ae4-107">**Eliminazione temporanea**, un tipo di entità definisce una proprietà *IsDeleted*.</span><span class="sxs-lookup"><span data-stu-id="b2ae4-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="b2ae4-108">**Multi-tenancy**, un tipo di entità definisce una proprietà *TenantId*.</span><span class="sxs-lookup"><span data-stu-id="b2ae4-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="b2ae4-109">Esempio</span><span class="sxs-lookup"><span data-stu-id="b2ae4-109">Example</span></span>

<span data-ttu-id="b2ae4-110">L'esempio seguente mostra come usare i filtri di query globali per implementare i comportamenti di query per eliminazione temporanea e multi-tenancy in un modello di blog semplice.</span><span class="sxs-lookup"><span data-stu-id="b2ae4-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="b2ae4-111">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="b2ae4-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="b2ae4-112">Prima di tutto, definire le entità:</span><span class="sxs-lookup"><span data-stu-id="b2ae4-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="b2ae4-113">Si noti la dichiarazione di un campo _tenantId_ per l'entità _Blog_.</span><span class="sxs-lookup"><span data-stu-id="b2ae4-113">Note the declaration of a _tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="b2ae4-114">Questo verrà usato per associare ogni istanza di Blog a un tenant specifico.</span><span class="sxs-lookup"><span data-stu-id="b2ae4-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="b2ae4-115">Viene anche definita una proprietà _IsDeleted_ per il tipo di entità _Post_.</span><span class="sxs-lookup"><span data-stu-id="b2ae4-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="b2ae4-116">Questa proprietà viene usata per tenere traccia dell'eliminazione temporanea di un'istanza di _Post_.</span><span class="sxs-lookup"><span data-stu-id="b2ae4-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="b2ae4-117">L'istanza è contrassegnata come eliminata senza rimuovere fisicamente i dati sottostanti.</span><span class="sxs-lookup"><span data-stu-id="b2ae4-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="b2ae4-118">A questo punto, configurare i filtri di query in _OnModelCreating_ usando l'API `HasQueryFilter`.</span><span class="sxs-lookup"><span data-stu-id="b2ae4-118">Next, configure the query filters in _OnModelCreating_ using the `HasQueryFilter` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="b2ae4-119">Le espressioni del predicato passate alle chiamate di _HasQueryFilter_ verranno ora applicate automaticamente a tutte le query LINQ per tali tipi.</span><span class="sxs-lookup"><span data-stu-id="b2ae4-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="b2ae4-120">Si noti l'uso del campo a livello di istanza di DbContext `_tenantId`, usato per impostare il tenant corrente.</span><span class="sxs-lookup"><span data-stu-id="b2ae4-120">Note the use of a DbContext instance level field: `_tenantId` used to set the current tenant.</span></span> <span data-ttu-id="b2ae4-121">I filtri a livello di modello useranno il valore dell'istanza del contesto corretta, ovvero l'istanza che esegue la query.</span><span class="sxs-lookup"><span data-stu-id="b2ae4-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

> [!NOTE]
> <span data-ttu-id="b2ae4-122">Attualmente non è possibile definire più filtri query sulla stessa entità. verrà applicato solo l'ultimo.</span><span class="sxs-lookup"><span data-stu-id="b2ae4-122">It is currently not possible to define multiple query filters on the same entity - only the last one will be applied.</span></span> <span data-ttu-id="b2ae4-123">Tuttavia, è possibile definire un singolo filtro con più condizioni usando l'operatore _and_ logico ([ `&&` in C# ](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span><span class="sxs-lookup"><span data-stu-id="b2ae4-123">However, you can define a single filter with multiple conditions using the logical _AND_ operator ([`&&` in C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="b2ae4-124">Disabilitazione dei filtri</span><span class="sxs-lookup"><span data-stu-id="b2ae4-124">Disabling Filters</span></span>

<span data-ttu-id="b2ae4-125">È possibile disabilitare i filtri per singole query LINQ usando l'operatore `IgnoreQueryFilters()`.</span><span class="sxs-lookup"><span data-stu-id="b2ae4-125">Filters may be disabled for individual LINQ queries by using the `IgnoreQueryFilters()` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="b2ae4-126">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="b2ae4-126">Limitations</span></span>

<span data-ttu-id="b2ae4-127">I filtri di query globali presentano le limitazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="b2ae4-127">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="b2ae4-128">I filtri non possono contenere riferimenti a proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="b2ae4-128">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="b2ae4-129">I filtri possono essere definiti solo per il tipo di entità radice di una gerarchia di ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="b2ae4-129">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
