---
title: Filtri di query globali - EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 73efe62262cf45cc1841d7a86cf59249cf07c5ea
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996665"
---
# <a name="global-query-filters"></a><span data-ttu-id="98041-102">Filtri di query globali</span><span class="sxs-lookup"><span data-stu-id="98041-102">Global Query Filters</span></span>

<span data-ttu-id="98041-103">I filtri di query globali sono predicati di query LINQ (un'espressione booleana in genere passata all'operatore di query *Where* di LINQ) applicati ai tipi di entità nel modello di metadati (solitamente in *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="98041-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="98041-104">Questi filtri vengono automaticamente applicati a qualsiasi query LINQ che interessa questi tipi di entità, inclusi quelli a cui si fa riferimento in modo indiretto, ad esempio tramite l'uso di riferimenti alle proprietà Include o di navigazione diretta.</span><span class="sxs-lookup"><span data-stu-id="98041-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="98041-105">Alcune applicazioni comuni di questa funzionalità sono:</span><span class="sxs-lookup"><span data-stu-id="98041-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="98041-106">**Eliminazione temporanea**, un tipo di entità definisce una proprietà *IsDeleted*.</span><span class="sxs-lookup"><span data-stu-id="98041-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="98041-107">**Multi-tenancy**, un tipo di entità definisce una proprietà *TenantId*.</span><span class="sxs-lookup"><span data-stu-id="98041-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="98041-108">Esempio</span><span class="sxs-lookup"><span data-stu-id="98041-108">Example</span></span>

<span data-ttu-id="98041-109">L'esempio seguente mostra come usare i filtri di query globali per implementare i comportamenti di query per eliminazione temporanea e multi-tenancy in un modello di blog semplice.</span><span class="sxs-lookup"><span data-stu-id="98041-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="98041-110">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryFilters) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="98041-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="98041-111">Prima di tutto, definire le entità:</span><span class="sxs-lookup"><span data-stu-id="98041-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="98041-112">Si noti la dichiarazione di un campo __tenantId_ per l'entità _Blog_.</span><span class="sxs-lookup"><span data-stu-id="98041-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="98041-113">Questo verrà usato per associare ogni istanza di Blog a un tenant specifico.</span><span class="sxs-lookup"><span data-stu-id="98041-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="98041-114">Viene anche definita una proprietà _IsDeleted_ per il tipo di entità _Post_.</span><span class="sxs-lookup"><span data-stu-id="98041-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="98041-115">Questa proprietà viene usata per tenere traccia dell'eliminazione temporanea di un'istanza di _Post_.</span><span class="sxs-lookup"><span data-stu-id="98041-115">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="98041-116">L'istanza è contrassegnata come eliminata senza rimuovere fisicamente i dati sottostanti.</span><span class="sxs-lookup"><span data-stu-id="98041-116">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="98041-117">A questo punto, configurare i filtri di query in _OnModelCreating_ usando l'API ```HasQueryFilter```.</span><span class="sxs-lookup"><span data-stu-id="98041-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="98041-118">Le espressioni del predicato passate alle chiamate di _HasQueryFilter_ verranno ora applicate automaticamente a tutte le query LINQ per tali tipi.</span><span class="sxs-lookup"><span data-stu-id="98041-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="98041-119">Si noti l'uso del campo a livello di istanza di DbContext ```_tenantId```, usato per impostare il tenant corrente.</span><span class="sxs-lookup"><span data-stu-id="98041-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="98041-120">I filtri a livello di modello useranno il valore dell'istanza del contesto corretta, ovvero l'istanza che esegue la query.</span><span class="sxs-lookup"><span data-stu-id="98041-120">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="98041-121">Disabilitazione dei filtri</span><span class="sxs-lookup"><span data-stu-id="98041-121">Disabling Filters</span></span>

<span data-ttu-id="98041-122">È possibile disabilitare i filtri per singole query LINQ usando l'operatore ```IgnoreQueryFilters()```.</span><span class="sxs-lookup"><span data-stu-id="98041-122">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="98041-123">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="98041-123">Limitations</span></span>

<span data-ttu-id="98041-124">I filtri di query globali presentano le limitazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="98041-124">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="98041-125">I filtri non possono contenere riferimenti a proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="98041-125">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="98041-126">I filtri possono essere definiti solo per il tipo di entità radice di una gerarchia di ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="98041-126">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
