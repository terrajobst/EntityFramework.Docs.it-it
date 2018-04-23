---
title: Tipi di query - Core a Entity Framework
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 4e02f106e086d243b23a60c02838f32555be210e
ms.sourcegitcommit: 26f33758c47399ae933f22fec8e1d19fa7d2c0b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/19/2018
---
# <a name="query-types"></a><span data-ttu-id="936c0-102">Tipi di query</span><span class="sxs-lookup"><span data-stu-id="936c0-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="936c0-103">Questa funzionalità è nuova in Entity Framework Core 2.1</span><span class="sxs-lookup"><span data-stu-id="936c0-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="936c0-104">Oltre ai tipi di entità, può contenere un modello di Entity Framework Core _tipi di query_, che può essere usato per eseguire query sui dati che non sono stato eseguito il mapping ai tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="936c0-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

<span data-ttu-id="936c0-105">Tipi di query presentano molte analogie con tipi di entità:</span><span class="sxs-lookup"><span data-stu-id="936c0-105">Query types have many similarities with entity types:</span></span>

- <span data-ttu-id="936c0-106">Possono inoltre essere aggiunti al modello oppure in `OnModelCreating`, o tramite una proprietà "set" su un oggetto derivato _DbContext_.</span><span class="sxs-lookup"><span data-stu-id="936c0-106">They can also be added to the model either in `OnModelCreating`, or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="936c0-107">Supportano molte delle stesse funzionalità di mapping, come l'ereditarietà di mapping, le proprietà di navigazione (vedere le limitazioni seguenti) e, negli archivi relazionali, la possibilità di configurare le oggetti di database di destinazione e le colonne tramite metodi dell'API fluent o le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="936c0-107">They support many of the same mapping capabilities, like inheritance mapping, navigation properties (see limitations below) and, on relational stores, the ability to configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="936c0-108">Tuttavia sono diversi dall'entità tipi che sono:</span><span class="sxs-lookup"><span data-stu-id="936c0-108">However they are different from entity types in that they:</span></span>

- <span data-ttu-id="936c0-109">Non richiedono una chiave da definire.</span><span class="sxs-lookup"><span data-stu-id="936c0-109">Do not require a key to be defined.</span></span>
- <span data-ttu-id="936c0-110">Non viene mai tenuta traccia delle modifiche la _DbContext_ e pertanto sono mai inseriti, aggiornati o eliminati nel database.</span><span class="sxs-lookup"><span data-stu-id="936c0-110">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="936c0-111">Non vengono individuati per convenzione.</span><span class="sxs-lookup"><span data-stu-id="936c0-111">Are never discovered by convention.</span></span>
- <span data-ttu-id="936c0-112">Supportano solo un sottoinsieme di funzionalità di mapping di spostamento, in particolare, non possono agire come l'entità finale principale di una relazione.</span><span class="sxs-lookup"><span data-stu-id="936c0-112">Only support a subset of navigation mapping capabilities - Specifically, they may never act as the principal end of a relationship.</span></span>
- <span data-ttu-id="936c0-113">Vengono indirizzate nel _ModelBuilder_ utilizzando il `Query` metodo invece che al `Entity` (metodo).</span><span class="sxs-lookup"><span data-stu-id="936c0-113">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="936c0-114">Viene eseguito il mapping sul _DbContext_ tramite le proprietà di tipo `DbQuery<T>` anziché `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="936c0-114">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="936c0-115">Viene eseguito il mapping agli oggetti di database utilizzando la `ToView` metodo, piuttosto che `ToTable`.</span><span class="sxs-lookup"><span data-stu-id="936c0-115">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="936c0-116">Può essere mappato a un _definizione di query_ - una definizione di query è una query secondaria dichiarata nel modello che funge da un'origine dati per un tipo di query.</span><span class="sxs-lookup"><span data-stu-id="936c0-116">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

<span data-ttu-id="936c0-117">Alcuni degli scenari di utilizzo principali per i tipi di query sono:</span><span class="sxs-lookup"><span data-stu-id="936c0-117">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="936c0-118">Serve come tipo restituito per hoc `FromSql()` query.</span><span class="sxs-lookup"><span data-stu-id="936c0-118">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="936c0-119">Mapping a viste di database.</span><span class="sxs-lookup"><span data-stu-id="936c0-119">Mapping to database views.</span></span>
- <span data-ttu-id="936c0-120">Mapping a tabelle che non dispone di una chiave primaria definita.</span><span class="sxs-lookup"><span data-stu-id="936c0-120">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="936c0-121">Mapping per le query definite nel modello.</span><span class="sxs-lookup"><span data-stu-id="936c0-121">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="936c0-122">Mapping di un tipo di query a un oggetto di database viene eseguito mediante il `ToView` API fluent.</span><span class="sxs-lookup"><span data-stu-id="936c0-122">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="936c0-123">Dal punto di vista di EF nell'elemento principale, l'oggetto di database specificato in questo metodo è un _vista_, vale a dire che viene considerato come un'origine di query di sola lettura e non può essere la destinazione dell'aggiornamento, inserimento o le operazioni di eliminazione.</span><span class="sxs-lookup"><span data-stu-id="936c0-123">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="936c0-124">Tuttavia, ciò non significa che l'oggetto di database risulta effettivamente necessario per essere una vista di database, in alternativa può essere una tabella di database che verrà trattata come di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="936c0-124">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="936c0-125">Al contrario, per i tipi di entità, Core EF si presuppone che un oggetto di database specificato nella `ToTable` metodo può essere considerato come un _tabella_, vale a dire che può essere utilizzato come origine di query ma anche assegnato dall'aggiornamento, eliminazione e inserimento operazioni.</span><span class="sxs-lookup"><span data-stu-id="936c0-125">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="936c0-126">In effetti, è possibile specificare il nome di una vista di database in `ToTable` e tutto dovrebbe funzionare, purché la vista è configurata per essere aggiornabile nel database.</span><span class="sxs-lookup"><span data-stu-id="936c0-126">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="936c0-127">Esempio</span><span class="sxs-lookup"><span data-stu-id="936c0-127">Example</span></span>

<span data-ttu-id="936c0-128">Nell'esempio seguente viene illustrato come utilizzare il tipo di Query eseguire query su una vista di database.</span><span class="sxs-lookup"><span data-stu-id="936c0-128">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="936c0-129">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="936c0-129">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="936c0-130">In primo luogo, è necessario definire un modello semplice di Blog e Post:</span><span class="sxs-lookup"><span data-stu-id="936c0-130">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="936c0-131">Successivamente, è possibile definire una vista di database semplice che consente di eseguire una query il numero di inserimenti associata a ogni blog:</span><span class="sxs-lookup"><span data-stu-id="936c0-131">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="936c0-132">Successivamente, è possibile definire una classe per contenere il risultato della vista di database:</span><span class="sxs-lookup"><span data-stu-id="936c0-132">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="936c0-133">Successivamente, è stato possibile configurare il tipo di query in _OnModelCreating_ utilizzando il `modelBuilder.Query<T>` API.</span><span class="sxs-lookup"><span data-stu-id="936c0-133">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="936c0-134">Si utilizza configurazione standard di Microsoft Office fluent API per configurare il mapping per il tipo di Query:</span><span class="sxs-lookup"><span data-stu-id="936c0-134">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="936c0-135">Infine, possiamo eseguire una query la visualizzazione del database nella modalità standard:</span><span class="sxs-lookup"><span data-stu-id="936c0-135">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="936c0-136">Si noti che vengono definite anche una proprietà di query di livello (DbQuery) di agire come radice per le query su questo tipo di contesto.</span><span class="sxs-lookup"><span data-stu-id="936c0-136">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
