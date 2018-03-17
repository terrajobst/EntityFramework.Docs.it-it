---
title: Tipi di query - Core a Entity Framework
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: dfd08cd1c30debddc79740bbf05c39c22e973855
ms.sourcegitcommit: 01b5cf3b7c983bcced91e7cc4c78391ced2d2caa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/17/2018
---
# <a name="query-types"></a><span data-ttu-id="e3185-102">Tipi di query</span><span class="sxs-lookup"><span data-stu-id="e3185-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="e3185-103">Questa funzionalità è nuova in Entity Framework Core 2.1</span><span class="sxs-lookup"><span data-stu-id="e3185-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="e3185-104">Tipi di query sono tipi di risultati di query di sola lettura che possono essere aggiunto al modello di base di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e3185-104">Query Types are read-only query result types that can be added to the EF Core model.</span></span> <span data-ttu-id="e3185-105">Tipi di query abilitare query ad hoc (ad esempio, tipi anonimi), ma sono più flessibili perché possono avere configurazione del mapping specificata.</span><span class="sxs-lookup"><span data-stu-id="e3185-105">Query Types enable ad-hoc querying (like anonymous types), but are more flexible because they can have mapping configuration specified.</span></span>

<span data-ttu-id="e3185-106">Sono concettualmente simili ai tipi di entità in cui:</span><span class="sxs-lookup"><span data-stu-id="e3185-106">They are conceptually similar to Entity Types in that:</span></span>

- <span data-ttu-id="e3185-107">Sono tipi POCO c# che vengono aggiunti al modello, in ```OnModelCreating``` utilizzando il ```ModelBuilder.Query``` metodo, o tramite una proprietà DbContext "set" (per i tipi di query tale proprietà è digitata come ```DbQuery<T>``` anziché ```DbSet<T>```).</span><span class="sxs-lookup"><span data-stu-id="e3185-107">They are POCO C# types that are added to the model, either in ```OnModelCreating``` using the ```ModelBuilder.Query``` method, or via a DbContext "set" property (for query types such a property is typed as ```DbQuery<T>``` rather than ```DbSet<T>```).</span></span>
- <span data-ttu-id="e3185-108">Supportano la maggior parte delle stesse funzionalità di mapping come tipi di entità normale.</span><span class="sxs-lookup"><span data-stu-id="e3185-108">They support much of the same mapping capabilities as regular entity types.</span></span> <span data-ttu-id="e3185-109">Ad esempio, il mapping di ereditarietà, spostamenti (vedere limitiations riportato di seguito) e, in archivi relazionali, la possibilità di configurare gli oggetti dello schema di database di destinazione tramite ```ToTable```, ```HasColumn``` metodi api fluent (o le annotazioni dei dati).</span><span class="sxs-lookup"><span data-stu-id="e3185-109">For example, inheritance mapping, navigations (see limitiations below) and, on relational stores, the ability to configure the target database schema objects via ```ToTable```, ```HasColumn``` fluent-api methods (or data annotations).</span></span>

<span data-ttu-id="e3185-110">Tipi di query sono diversi dall'entità tipi in cui:</span><span class="sxs-lookup"><span data-stu-id="e3185-110">Query Types are different from entity types in that they:</span></span>

- <span data-ttu-id="e3185-111">Non richiedono una chiave da definire.</span><span class="sxs-lookup"><span data-stu-id="e3185-111">Do not require a key to be defined.</span></span>
- <span data-ttu-id="e3185-112">Non sono mai monitorati dal rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="e3185-112">Are never tracked by the Change Tracker.</span></span>
- <span data-ttu-id="e3185-113">Non vengono individuati per convenzione.</span><span class="sxs-lookup"><span data-stu-id="e3185-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="e3185-114">Supportano solo un sottoinsieme di funzionalità di mapping di spostamento, in particolare, non possono agire come l'entità finale principale di una relazione.</span><span class="sxs-lookup"><span data-stu-id="e3185-114">Only support a subset of navigation mapping capabilities - Specifically, they may never act as the principal end of a relationship.</span></span>
- <span data-ttu-id="e3185-115">Può essere mappato a un _definizione query_ -una definizione di Query è secondario che funge da un'origine dati per un tipo di Query.</span><span class="sxs-lookup"><span data-stu-id="e3185-115">May be mapped to a _defining query_ - A Defining Query is a secondary query that acts a data source for a Query Type.</span></span>

<span data-ttu-id="e3185-116">Alcuni degli scenari di utilizzo principali per i tipi di query sono:</span><span class="sxs-lookup"><span data-stu-id="e3185-116">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="e3185-117">Mapping a viste di database.</span><span class="sxs-lookup"><span data-stu-id="e3185-117">Mapping to database views.</span></span>
- <span data-ttu-id="e3185-118">Mapping a tabelle che non dispone di una chiave primaria definita.</span><span class="sxs-lookup"><span data-stu-id="e3185-118">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="e3185-119">Serve come tipo restituito per hoc ```FromSql()``` query.</span><span class="sxs-lookup"><span data-stu-id="e3185-119">Serving as the return type for ad hoc ```FromSql()``` queries.</span></span>
- <span data-ttu-id="e3185-120">Mapping per le query definite nel modello.</span><span class="sxs-lookup"><span data-stu-id="e3185-120">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="e3185-121">Mapping di un tipo di query a una vista di database viene eseguito mediante il ```ToTable``` API fluent.</span><span class="sxs-lookup"><span data-stu-id="e3185-121">Mapping a query type to a database view is achieved using the ```ToTable``` fluent API.</span></span>

## <a name="example"></a><span data-ttu-id="e3185-122">Esempio</span><span class="sxs-lookup"><span data-stu-id="e3185-122">Example</span></span>

<span data-ttu-id="e3185-123">Nell'esempio seguente viene illustrato come utilizzare il tipo di Query eseguire query su una vista di database.</span><span class="sxs-lookup"><span data-stu-id="e3185-123">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="e3185-124">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="e3185-124">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="e3185-125">In primo luogo, è necessario definire un modello semplice di Blog e Post:</span><span class="sxs-lookup"><span data-stu-id="e3185-125">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="e3185-126">Successivamente, è possibile definire una vista di database semplice che consente di eseguire una query il numero di inserimenti associata a ogni blog:</span><span class="sxs-lookup"><span data-stu-id="e3185-126">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="e3185-127">Successivamente, è possibile definire una classe per contenere il risultato della vista di database:</span><span class="sxs-lookup"><span data-stu-id="e3185-127">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="e3185-128">Successivamente, è stato possibile configurare il tipo di query in _OnModelCreating_ utilizzando il ```modelBuilder.Query<T>``` API.</span><span class="sxs-lookup"><span data-stu-id="e3185-128">Next, we configure the query type in _OnModelCreating_ using the ```modelBuilder.Query<T>``` API.</span></span>
<span data-ttu-id="e3185-129">Si utilizza configurazione standard di Microsoft Office fluent API per configurare il mapping per il tipo di Query:</span><span class="sxs-lookup"><span data-stu-id="e3185-129">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="e3185-130">Infine, possiamo eseguire una query la visualizzazione del database nella modalità standard:</span><span class="sxs-lookup"><span data-stu-id="e3185-130">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="e3185-131">Si noti che vengono definite anche una proprietà di query di livello (DbQuery) di agire come radice per le query su questo tipo di contesto.</span><span class="sxs-lookup"><span data-stu-id="e3185-131">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
