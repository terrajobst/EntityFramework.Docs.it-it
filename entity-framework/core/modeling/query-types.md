---
title: Tipi di query - EF Core
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 89f5be356654dc02e353441a83e34c90fc727593
ms.sourcegitcommit: fd50ac53b93a03825dcbb42ed2e7ca95ca858d5f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900304"
---
# <a name="query-types"></a><span data-ttu-id="098e5-102">Tipi di query</span><span class="sxs-lookup"><span data-stu-id="098e5-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="098e5-103">Questa funzionalità è una novità di EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="098e5-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="098e5-104">Oltre ai tipi di entità, un modello di EF Core può contenere _tipi di query_, che può essere usato per eseguire query sui dati che non sono stato eseguito il mapping ai tipi di entità di database.</span><span class="sxs-lookup"><span data-stu-id="098e5-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

<span data-ttu-id="098e5-105">Tipi di query presentano molte analogie con tipi di entità:</span><span class="sxs-lookup"><span data-stu-id="098e5-105">Query types have many similarities with entity types:</span></span>

- <span data-ttu-id="098e5-106">Possono anche essere aggiunti al modello sia nel `OnModelCreating`, o tramite una proprietà "set" su un oggetto derivato _DbContext_.</span><span class="sxs-lookup"><span data-stu-id="098e5-106">They can also be added to the model either in `OnModelCreating`, or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="098e5-107">Supportano molte delle stesse funzionalità di mapping, come l'ereditarietà di mapping, le proprietà di navigazione (vedere le limitazioni seguenti) e, in archivi relazionali, la possibilità di configurare le oggetti di database di destinazione e le colonne tramite metodi dell'API fluent o annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="098e5-107">They support many of the same mapping capabilities, like inheritance mapping, navigation properties (see limitations below) and, on relational stores, the ability to configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="098e5-108">Tuttavia sono diversi da entità tipi che sono:</span><span class="sxs-lookup"><span data-stu-id="098e5-108">However they are different from entity types in that they:</span></span>

- <span data-ttu-id="098e5-109">Non richiedono una chiave da definire.</span><span class="sxs-lookup"><span data-stu-id="098e5-109">Do not require a key to be defined.</span></span>
- <span data-ttu-id="098e5-110">Non verranno mai rilevati per le modifiche nel _DbContext_ e pertanto mai inserite, aggiornate o eliminate nel database.</span><span class="sxs-lookup"><span data-stu-id="098e5-110">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="098e5-111">Non vengono mai individuati dalla convenzione.</span><span class="sxs-lookup"><span data-stu-id="098e5-111">Are never discovered by convention.</span></span>
- <span data-ttu-id="098e5-112">Supportano solo un subset di funzioni di mapping di spostamento, in particolare:</span><span class="sxs-lookup"><span data-stu-id="098e5-112">Only support a subset of navigation mapping capabilities - Specifically:</span></span>
  - <span data-ttu-id="098e5-113">Mai agiscono come entità finale principale di una relazione.</span><span class="sxs-lookup"><span data-stu-id="098e5-113">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="098e5-114">Possono contenere solo le proprietà di navigazione di riferimento che punta all'entità.</span><span class="sxs-lookup"><span data-stu-id="098e5-114">They can only contain reference navigation properties pointing to entities.</span></span>
  - <span data-ttu-id="098e5-115">Le entità non possono contenere le proprietà di navigazione ai tipi di query.</span><span class="sxs-lookup"><span data-stu-id="098e5-115">Entities cannot contain navigation properties to query types.</span></span>
- <span data-ttu-id="098e5-116">Vengano affrontati nel _ModelBuilder_ usando la `Query` metodo anziché la `Entity` (metodo).</span><span class="sxs-lookup"><span data-stu-id="098e5-116">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="098e5-117">Viene eseguito il mapping sul _DbContext_ tramite le proprietà di tipo `DbQuery<T>` anziché `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="098e5-117">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="098e5-118">Viene eseguito il mapping agli oggetti di database usando il `ToView` metodo, anziché `ToTable`.</span><span class="sxs-lookup"><span data-stu-id="098e5-118">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="098e5-119">Può eseguire il mapping a un _definizione di query_ : una definizione di query è una query secondaria dichiarata nel modello che funge da un'origine dati per un tipo di query.</span><span class="sxs-lookup"><span data-stu-id="098e5-119">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

<span data-ttu-id="098e5-120">Alcuni degli scenari di utilizzo principali per i tipi di query sono:</span><span class="sxs-lookup"><span data-stu-id="098e5-120">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="098e5-121">Che funge da tipo restituito per ad hoc `FromSql()` le query.</span><span class="sxs-lookup"><span data-stu-id="098e5-121">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="098e5-122">Mapping a viste di database.</span><span class="sxs-lookup"><span data-stu-id="098e5-122">Mapping to database views.</span></span>
- <span data-ttu-id="098e5-123">Mapping a tabelle che non è definita una chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="098e5-123">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="098e5-124">Mapping a query definite nel modello.</span><span class="sxs-lookup"><span data-stu-id="098e5-124">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="098e5-125">Mapping di un tipo di query a un oggetto di database viene eseguito mediante il `ToView` API fluent.</span><span class="sxs-lookup"><span data-stu-id="098e5-125">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="098e5-126">Dal punto di vista di EF Core, l'oggetto di database specificato in questo metodo è un _vista_, vale a dire che viene considerato come un'origine di query di sola lettura e non può essere la destinazione dell'aggiornamento, inserire o eliminare le operazioni.</span><span class="sxs-lookup"><span data-stu-id="098e5-126">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="098e5-127">Tuttavia, ciò non significa che l'oggetto di database è effettivamente necessario per essere una vista di database, in alternativa può essere una tabella di database che verrà considerata di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="098e5-127">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="098e5-128">Viceversa, per i tipi di entità, EF Core si presuppone che un oggetto di database specificato nella `ToTable` metodo può essere considerato come un _tabella_, vale a dire che può essere utilizzato come origine query ma anche di destinazione da aggiornare, eliminare e inserire operazioni.</span><span class="sxs-lookup"><span data-stu-id="098e5-128">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="098e5-129">In effetti, è possibile specificare il nome di una vista di database in `ToTable` e tutto dovrebbe funzionare senza problemi, purché la visualizzazione è configurata per essere aggiornabile nel database.</span><span class="sxs-lookup"><span data-stu-id="098e5-129">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="098e5-130">Esempio</span><span class="sxs-lookup"><span data-stu-id="098e5-130">Example</span></span>

<span data-ttu-id="098e5-131">Nell'esempio seguente viene illustrato come utilizzare il tipo di Query per eseguire query di una vista di database.</span><span class="sxs-lookup"><span data-stu-id="098e5-131">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="098e5-132">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="098e5-132">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="098e5-133">In primo luogo, definiamo un semplice modello di post di Blog e Post:</span><span class="sxs-lookup"><span data-stu-id="098e5-133">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="098e5-134">Successivamente, viene definita una vista di database semplici che ci consentirà di eseguire una query il numero di post associati a ciascun blog:</span><span class="sxs-lookup"><span data-stu-id="098e5-134">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="098e5-135">Successivamente, viene definita una classe che contenga il risultato della vista di database:</span><span class="sxs-lookup"><span data-stu-id="098e5-135">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="098e5-136">Successivamente, si configura il tipo di query nel _OnModelCreating_ usando il `modelBuilder.Query<T>` API.</span><span class="sxs-lookup"><span data-stu-id="098e5-136">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="098e5-137">Utilizziamo standard configurazione fluent API per configurare il mapping per il tipo di Query:</span><span class="sxs-lookup"><span data-stu-id="098e5-137">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="098e5-138">Infine, è possibile eseguire una query la visualizzazione del database nella modalità standard:</span><span class="sxs-lookup"><span data-stu-id="098e5-138">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="098e5-139">Si noti che inoltre abbiamo definito una proprietà di query a livello di contesto (DbQuery) di agire come un utente root per le query su questo tipo.</span><span class="sxs-lookup"><span data-stu-id="098e5-139">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
