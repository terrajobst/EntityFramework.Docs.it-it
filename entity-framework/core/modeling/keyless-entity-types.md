---
title: Tipi di entità autofirmati-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: b968ac9602b9aa1f1c1e3181b6b76a64394d70f0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150843"
---
# <a name="keyless-entity-types"></a><span data-ttu-id="46bfe-102">Tipi di entità autochiave</span><span class="sxs-lookup"><span data-stu-id="46bfe-102">Keyless Entity Types</span></span>
> [!NOTE]
> <span data-ttu-id="46bfe-103">Questa funzionalità è stata introdotta in EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="46bfe-103">This feature is new in EF Core 2.1.</span></span> <span data-ttu-id="46bfe-104">Prima del 3,0 erano noti come tipi di query</span><span class="sxs-lookup"><span data-stu-id="46bfe-104">Prior to 3.0 they were known as Query Types</span></span>

<span data-ttu-id="46bfe-105">Oltre ai normali tipi di entità, un modello di EF Core può contenere _tipi di entità_senza chiave, che possono essere usati per eseguire query di database su dati che non contengono valori di chiave.</span><span class="sxs-lookup"><span data-stu-id="46bfe-105">In addition to regular entity types, an EF Core model can contain _keyless entity types_, which can be used to carry out database queries against data that doesn't contain key values.</span></span>

## <a name="keyless-entity-types-characteristics"></a><span data-ttu-id="46bfe-106">Caratteristiche di tipi di entità autochiave</span><span class="sxs-lookup"><span data-stu-id="46bfe-106">Keyless entity types characteristics</span></span>

<span data-ttu-id="46bfe-107">I tipi di entità autonome supportano molte delle stesse funzionalità di mapping dei normali tipi di entità, ad esempio il mapping di ereditarietà e le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="46bfe-107">Keyless entity types support many of the same mapping capabilities as regular entity types, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="46bfe-108">In archivi relazionali, è possibile configurare le oggetti di database di destinazione e le colonne tramite metodi dell'API fluent o annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="46bfe-108">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="46bfe-109">Tuttavia, sono diversi dai normali tipi di entità in quanto:</span><span class="sxs-lookup"><span data-stu-id="46bfe-109">However, they are different from regular entity types in that they:</span></span>

- <span data-ttu-id="46bfe-110">Non è possibile definire una chiave.</span><span class="sxs-lookup"><span data-stu-id="46bfe-110">Cannot have a key defined.</span></span>
- <span data-ttu-id="46bfe-111">Non vengono mai rilevate per le modifiche apportate in _DbContext_ e pertanto non vengono mai inserite, aggiornate o eliminate nel database.</span><span class="sxs-lookup"><span data-stu-id="46bfe-111">Are never tracked for changes in the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="46bfe-112">Non vengono mai individuati dalla convenzione.</span><span class="sxs-lookup"><span data-stu-id="46bfe-112">Are never discovered by convention.</span></span>
- <span data-ttu-id="46bfe-113">Supporta solo un subset di funzionalità di mapping di navigazione, in particolare:</span><span class="sxs-lookup"><span data-stu-id="46bfe-113">Only support a subset of navigation mapping capabilities, specifically:</span></span>
  - <span data-ttu-id="46bfe-114">Mai agiscono come entità finale principale di una relazione.</span><span class="sxs-lookup"><span data-stu-id="46bfe-114">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="46bfe-115">Potrebbero non avere spostamenti sulle entità di proprietà</span><span class="sxs-lookup"><span data-stu-id="46bfe-115">They may not have navigations to owned entities</span></span>
  - <span data-ttu-id="46bfe-116">Possono contenere solo proprietà di navigazione di riferimento che puntano a entità regolari.</span><span class="sxs-lookup"><span data-stu-id="46bfe-116">They can only contain reference navigation properties pointing to regular entities.</span></span>
  - <span data-ttu-id="46bfe-117">Le entità non possono contenere proprietà di navigazione per i tipi di entità autochiave.</span><span class="sxs-lookup"><span data-stu-id="46bfe-117">Entities cannot contain navigation properties to keyless entity types.</span></span>
- <span data-ttu-id="46bfe-118">Deve essere configurato con la `.HasNoKey()` chiamata al metodo.</span><span class="sxs-lookup"><span data-stu-id="46bfe-118">Need to be configured with `.HasNoKey()` method call.</span></span>
- <span data-ttu-id="46bfe-119">Può essere mappato a una _query di definizione_.</span><span class="sxs-lookup"><span data-stu-id="46bfe-119">May be mapped to a _defining query_.</span></span> <span data-ttu-id="46bfe-120">Una query di definizione è una query dichiarata nel modello che funge da origine dati per un tipo di entità autonome.</span><span class="sxs-lookup"><span data-stu-id="46bfe-120">A defining query is a query declared in the model that acts as a data source for a keyless entity type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="46bfe-121">Scenari di utilizzo</span><span class="sxs-lookup"><span data-stu-id="46bfe-121">Usage scenarios</span></span>

<span data-ttu-id="46bfe-122">Di seguito sono riportati alcuni degli scenari di utilizzo principali per i tipi di entità autochiave:</span><span class="sxs-lookup"><span data-stu-id="46bfe-122">Some of the main usage scenarios for keyless entity types are:</span></span>

- <span data-ttu-id="46bfe-123">Fungendo da tipo restituito per le [query SQL non elaborate](xref:core/querying/raw-sql).</span><span class="sxs-lookup"><span data-stu-id="46bfe-123">Serving as the return type for [raw SQL queries](xref:core/querying/raw-sql).</span></span>
- <span data-ttu-id="46bfe-124">Mapping a viste di database che non contengono una chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="46bfe-124">Mapping to database views that do not contain a primary key.</span></span>
- <span data-ttu-id="46bfe-125">Mapping a tabelle che non è definita una chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="46bfe-125">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="46bfe-126">Mapping a query definite nel modello.</span><span class="sxs-lookup"><span data-stu-id="46bfe-126">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="46bfe-127">Mapping agli oggetti di database</span><span class="sxs-lookup"><span data-stu-id="46bfe-127">Mapping to database objects</span></span>

<span data-ttu-id="46bfe-128">Il mapping di un tipo di entità autochiave a un oggetto di `ToTable` database `ToView` viene eseguito tramite l'API o Fluent.</span><span class="sxs-lookup"><span data-stu-id="46bfe-128">Mapping a keyless entity type to a database object is achieved using the `ToTable` or `ToView` fluent API.</span></span> <span data-ttu-id="46bfe-129">Dal punto di vista di EF Core, l'oggetto di database specificato in questo metodo è un _vista_, vale a dire che viene considerato come un'origine di query di sola lettura e non può essere la destinazione dell'aggiornamento, inserire o eliminare le operazioni.</span><span class="sxs-lookup"><span data-stu-id="46bfe-129">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="46bfe-130">Tuttavia, ciò non significa che l'oggetto di database debba essere effettivamente una vista di database.</span><span class="sxs-lookup"><span data-stu-id="46bfe-130">However, this does not mean that the database object is actually required to be a database view.</span></span> <span data-ttu-id="46bfe-131">In alternativa, può essere una tabella di database che verrà considerata di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="46bfe-131">It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="46bfe-132">Viceversa, per i tipi di entità regolari, EF Core presuppone che un oggetto di database specificato nel `ToTable` metodo possa essere considerato come una _tabella_, ovvero può essere utilizzato come origine della query, ma anche come destinazione da operazioni di aggiornamento, eliminazione e inserimento.</span><span class="sxs-lookup"><span data-stu-id="46bfe-132">Conversely, for regular entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="46bfe-133">In effetti, è possibile specificare il nome di una vista di database in `ToTable` e tutto dovrebbe funzionare senza problemi, purché la visualizzazione è configurata per essere aggiornabile nel database.</span><span class="sxs-lookup"><span data-stu-id="46bfe-133">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

> [!NOTE]
> <span data-ttu-id="46bfe-134">`ToView`presuppone che l'oggetto sia già presente nel database e non venga creato dalle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="46bfe-134">`ToView` assumes that the object already exists in the database and it won't be created by migrations.</span></span>

## <a name="example"></a><span data-ttu-id="46bfe-135">Esempio</span><span class="sxs-lookup"><span data-stu-id="46bfe-135">Example</span></span>

<span data-ttu-id="46bfe-136">Nell'esempio seguente viene illustrato come utilizzare i tipi di entità autochiave per eseguire una query su una vista di database.</span><span class="sxs-lookup"><span data-stu-id="46bfe-136">The following example shows how to use keyless entity types to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="46bfe-137">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="46bfe-137">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="46bfe-138">In primo luogo, definiamo un semplice modello di post di Blog e Post:</span><span class="sxs-lookup"><span data-stu-id="46bfe-138">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

<span data-ttu-id="46bfe-139">Successivamente, viene definita una vista di database semplici che ci consentirà di eseguire una query il numero di post associati a ciascun blog:</span><span class="sxs-lookup"><span data-stu-id="46bfe-139">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

<span data-ttu-id="46bfe-140">Successivamente, viene definita una classe che contenga il risultato della vista di database:</span><span class="sxs-lookup"><span data-stu-id="46bfe-140">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

<span data-ttu-id="46bfe-141">Successivamente, si configura il tipo di entità autochiave in _OnModelCreating_ usando l' `HasNoKey` API.</span><span class="sxs-lookup"><span data-stu-id="46bfe-141">Next, we configure the keyless entity type in _OnModelCreating_ using the `HasNoKey` API.</span></span>
<span data-ttu-id="46bfe-142">Si usa l'API di configurazione Fluent per configurare il mapping per il tipo di entità autochiave:</span><span class="sxs-lookup"><span data-stu-id="46bfe-142">We use fluent configuration API to configure the mapping for the keyless entity type:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

<span data-ttu-id="46bfe-143">Successivamente, viene configurato `DbContext` per `DbSet<T>`includere:</span><span class="sxs-lookup"><span data-stu-id="46bfe-143">Next, we configure the `DbContext` to include the `DbSet<T>`:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

<span data-ttu-id="46bfe-144">Infine, è possibile eseguire una query la visualizzazione del database nella modalità standard:</span><span class="sxs-lookup"><span data-stu-id="46bfe-144">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="46bfe-145">Si noti che è stata definita anche una proprietà di query a livello di contesto (DbSet) che funge da radice per le query su questo tipo.</span><span class="sxs-lookup"><span data-stu-id="46bfe-145">Note we have also defined a context level query property (DbSet) to act as a root for queries against this type.</span></span>