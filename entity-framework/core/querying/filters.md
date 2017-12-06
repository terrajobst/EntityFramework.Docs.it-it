---
title: Filtri di Query globale - Core EF
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
# <a name="global-query-filters"></a><span data-ttu-id="7dfde-102">Filtri di Query globale</span><span class="sxs-lookup"><span data-stu-id="7dfde-102">Global Query Filters</span></span>

<span data-ttu-id="7dfde-103">I filtri di query globale sono predicati di query LINQ (un'espressione booleana in genere passato a LINQ *in* operatore query) applicato ai tipi di entità nel modello di metadati (in genere in *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="7dfde-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="7dfde-104">Tali filtri vengono applicati automaticamente a tutte le query LINQ che interessa i tipi di entità, inclusi riferimenti a proprietà di navigazione a cui viene fatto riferimento indirettamente, ad esempio tramite l'utilizzo di inclusione o diretto di tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="7dfde-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="7dfde-105">Vengono elencate alcune applicazioni di questa funzionalità:</span><span class="sxs-lookup"><span data-stu-id="7dfde-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="7dfde-106">**Eliminare temporaneamente** -definisce un tipo di entità un *IsDeleted* proprietà.</span><span class="sxs-lookup"><span data-stu-id="7dfde-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="7dfde-107">**Multi-tenancy** -definisce un tipo di entità un *TenantId* proprietà.</span><span class="sxs-lookup"><span data-stu-id="7dfde-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="7dfde-108">Esempio</span><span class="sxs-lookup"><span data-stu-id="7dfde-108">Example</span></span>

<span data-ttu-id="7dfde-109">Nell'esempio seguente viene illustrato come utilizzare i filtri di Query globale per implementare i comportamenti di soft-delete e multi-tenancy query in un modello semplice blog.</span><span class="sxs-lookup"><span data-stu-id="7dfde-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="7dfde-110">È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) su GitHub.</span><span class="sxs-lookup"><span data-stu-id="7dfde-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="7dfde-111">Prima di definire le entità:</span><span class="sxs-lookup"><span data-stu-id="7dfde-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="7dfde-112">Si noti la dichiarazione di un __tenantId_ nel campo di _Blog_ entità.</span><span class="sxs-lookup"><span data-stu-id="7dfde-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="7dfde-113">Questo verrà utilizzato per associare ogni istanza di blog relativo a un tenant specifico.</span><span class="sxs-lookup"><span data-stu-id="7dfde-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="7dfde-114">Viene inoltre definito un _IsDeleted_ proprietà il _Post_ tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="7dfde-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="7dfde-115">Questa opzione viene utilizzata questa opzione per consentire di controllare se un _Post_ istanza è stato "eliminato".</span><span class="sxs-lookup"><span data-stu-id="7dfde-115">This is used this to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="7dfde-116">Ad esempio L'istanza è contrassegnata come eliminata withouth rimozione fisicamente i dati sottostanti.</span><span class="sxs-lookup"><span data-stu-id="7dfde-116">I.e. The instance is marked as deleted withouth physically removing the underlying data.</span></span>

<span data-ttu-id="7dfde-117">A questo punto, configurare i filtri di query in _OnModelCreating_ utilizzando il ```HasQueryFilter``` API.</span><span class="sxs-lookup"><span data-stu-id="7dfde-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="7dfde-118">Le espressioni del predicato passato per il _HasQueryFilter_ chiamate verranno ora applicate automaticamente a tutte le query LINQ per tali tipi.</span><span class="sxs-lookup"><span data-stu-id="7dfde-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="7dfde-119">Si noti l'utilizzo di un campo di livello di istanza di DbContext: ```_tenantId``` utilizzato per impostare il tenant corrente.</span><span class="sxs-lookup"><span data-stu-id="7dfde-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="7dfde-120">I filtri a livello di modello verranno utilizzato il valore dall'istanza del contesto corretto.</span><span class="sxs-lookup"><span data-stu-id="7dfde-120">Model-level filters will use the value from the correct context instance.</span></span> <span data-ttu-id="7dfde-121">Ad esempio L'istanza che esegue la query.</span><span class="sxs-lookup"><span data-stu-id="7dfde-121">I.e. The instance that is executing the query.</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="7dfde-122">La disabilitazione di filtri</span><span class="sxs-lookup"><span data-stu-id="7dfde-122">Disabling Filters</span></span>

<span data-ttu-id="7dfde-123">I filtri possono essere disabilitati per le singole query LINQ usando il ```IgnoreQueryFilters()``` operatore.</span><span class="sxs-lookup"><span data-stu-id="7dfde-123">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="7dfde-124">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="7dfde-124">Limitations</span></span>

<span data-ttu-id="7dfde-125">I filtri di query globale presentano le limitazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="7dfde-125">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="7dfde-126">I filtri non possono contenere riferimenti a proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="7dfde-126">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="7dfde-127">I filtri possono essere definiti solo per la radice tipo di entità di una gerarchia di ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="7dfde-127">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
