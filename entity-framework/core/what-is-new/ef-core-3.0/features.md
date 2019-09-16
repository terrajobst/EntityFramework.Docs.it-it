---
title: Nuove funzionalità di EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 528733d6eec33de2c9538541a6ed5be704b9d433
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005562"
---
# <a name="new-features-included-in-ef-core-30"></a><span data-ttu-id="37ec6-102">Nuove funzionalità incluse in EF Core 3,0</span><span class="sxs-lookup"><span data-stu-id="37ec6-102">New features included in EF Core 3.0</span></span>

<span data-ttu-id="37ec6-103">Nell'elenco seguente sono riportate le principali nuove funzionalità pianificate per EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="37ec6-103">The following list includes the major new features planned for EF Core 3.0.</span></span>

<span data-ttu-id="37ec6-104">EF Core 3,0 è una versione principale e contiene anche numerose [modifiche di rilievo](xref:core/what-is-new/ef-core-3.0/breaking-changes), ovvero miglioramenti dell'API che possono avere un impatto negativo sulle applicazioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="37ec6-104">EF Core 3.0 is a major release and also contains numerous [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), which are API improvements that may have negative impact on existing applications.</span></span>  

## <a name="linq-improvements"></a><span data-ttu-id="37ec6-105">Miglioramenti di LINQ</span><span class="sxs-lookup"><span data-stu-id="37ec6-105">LINQ improvements</span></span> 

<span data-ttu-id="37ec6-106">LINQ consente di scrivere query di database senza uscire dal linguaggio scelto, sfruttando i vantaggi delle informazioni dettagliate sui tipi per offrire IntelliSense e il controllo dei tipi in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="37ec6-106">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to offer IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="37ec6-107">LINQ consente inoltre di scrivere un numero illimitato di query complesse contenenti espressioni arbitrarie (chiamate al metodo o operazioni).</span><span class="sxs-lookup"><span data-stu-id="37ec6-107">But LINQ also enables you to write an unlimited number of complicated queries containing arbitrary expressions (method calls or operations).</span></span>
<span data-ttu-id="37ec6-108">La gestione di tutte queste combinazioni è sempre stata una sfida significativa per i provider LINQ.</span><span class="sxs-lookup"><span data-stu-id="37ec6-108">Handling all those combinations has always been a significant challenge for LINQ providers.</span></span>
<span data-ttu-id="37ec6-109">In EF Core 3,0, l'implementazione di LINQ è stata riscritta per consentire la traduzione di più espressioni in SQL, per generare query efficienti in più casi, per evitare che le query inefficienti non vengano rilevate e per semplificare l'introduzione graduale di una nuova query funzionalità e prestazioni improvementswithout le applicazioni e i provider di dati esistenti.</span><span class="sxs-lookup"><span data-stu-id="37ec6-109">In EF Core 3.0, we've rewritten our LINQ implementation to enable translating more expressions into SQL, to generate efficient queries in more cases, to prevent inefficient queries from going undetected, and to make it easier for us to gradually introduce new query capabilities and performance improvementswithout breaking existing applications and data providers.</span></span>

### <a name="client-evaluation"></a><span data-ttu-id="37ec6-110">Valutazione client</span><span class="sxs-lookup"><span data-stu-id="37ec6-110">Client evaluation</span></span>

<span data-ttu-id="37ec6-111">La modifica della progettazione principale in EF Core 3,0 ha a che fare con il modo in cui gestisce le espressioni LINQ che non è in grado di convertire in SQL o nei parametri:</span><span class="sxs-lookup"><span data-stu-id="37ec6-111">The main design change in EF Core 3.0 has to do with how it handles LINQ expressions that it cannot translate to SQL or parameters:</span></span>

<span data-ttu-id="37ec6-112">Nelle prime versioni EF Core semplicemente capito quali parti di una query possono essere convertite in SQL ed eseguito il resto della query sul client.</span><span class="sxs-lookup"><span data-stu-id="37ec6-112">In the first few versions, EF Core simply figured out what portions of a query could be translated to SQL, and executed the rest of the query on the client.</span></span>
<span data-ttu-id="37ec6-113">Questo tipo di esecuzione lato client può essere auspicabile in alcune situazioni, ma in molti altri casi può causare query inefficienti.</span><span class="sxs-lookup"><span data-stu-id="37ec6-113">This type of client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries.</span></span>
<span data-ttu-id="37ec6-114">Se ad esempio EF Core 2,2 non è stato in grado di convertire `Where()` un predicato in una chiamata, è stata eseguita un'istruzione SQL senza un filtro, vengono lette tutte le righe dal database e quindi filtrate in memoria.</span><span class="sxs-lookup"><span data-stu-id="37ec6-114">For example, if EF Core 2.2 couldn't translate a predicate in a `Where()` call, it executed a SQL statement without a filter, read all all the rows from the database, and then filtered them in-memory.</span></span>
<span data-ttu-id="37ec6-115">Questo può essere accettabile se il database contiene un numero ridotto di righe, ma può causare problemi di prestazioni significativi o persino un errore dell'applicazione se il database contiene un numero elevato di righe.</span><span class="sxs-lookup"><span data-stu-id="37ec6-115">That may be acceptable if the database contains a small number of rows, but can result in significant performance issues or even application failure if the database contains a large number or rows.</span></span>
<span data-ttu-id="37ec6-116">In EF Core 3,0 è stata limitata la valutazione client solo nella proiezione di primo livello (l'ultima chiamata a `Select()`).</span><span class="sxs-lookup"><span data-stu-id="37ec6-116">In EF Core 3.0 we have restricted client evaluation to only happen on the top-level projection (the last call to `Select()`).</span></span>
<span data-ttu-id="37ec6-117">Quando EF Core 3,0 rileva espressioni che non possono essere convertite altrove nella query, viene generata un'eccezione in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="37ec6-117">When EF Core 3.0 detects expressions that cannot be translated anywhere else in the query, it throws a runtime exception.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="37ec6-118">Supporto di Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="37ec6-118">Cosmos DB support</span></span> 

<span data-ttu-id="37ec6-119">Il provider di Cosmos DB per EF Core consente agli sviluppatori che hanno familiarità con il modello di programmazione EF di utilizzare facilmente Azure Cosmos DB come database dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="37ec6-119">The Cosmos DB provider for EF Core enables developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="37ec6-120">L'obiettivo è quello di rendere alcuni dei vantaggi di Cosmos DB, ad esempio la distribuzione globale, la disponibilità "AlwaysOn", la scalabilità elastica e la bassa latenza, ancora più accessibili per gli sviluppatori .NET.</span><span class="sxs-lookup"><span data-stu-id="37ec6-120">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="37ec6-121">Il provider abiliterà la maggior parte delle funzionalità di EF Core, come il rilevamento delle modifiche automatico, LINQ e le conversioni dei valori, con l'API SQL in Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="37ec6-121">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="37ec6-122">Supporto di C# 8.0</span><span class="sxs-lookup"><span data-stu-id="37ec6-122">C# 8.0 support</span></span>

<span data-ttu-id="37ec6-123">EF Core 3,0 sfrutta alcune delle nuove funzionalità di C# 8,0:</span><span class="sxs-lookup"><span data-stu-id="37ec6-123">EF Core 3.0 takes advantage of some of the new features in C# 8.0:</span></span>

### <a name="asynchronous-streams"></a><span data-ttu-id="37ec6-124">Flussi asincroni</span><span class="sxs-lookup"><span data-stu-id="37ec6-124">Asynchronous streams</span></span>

<span data-ttu-id="37ec6-125">I risultati della query asincrona vengono ora esposti usando la `IAsyncEnumerable<T>` nuova interfaccia standard e possono essere `await foreach`usati usando.</span><span class="sxs-lookup"><span data-stu-id="37ec6-125">Asynchronous query results are now exposed using the new standard `IAsyncEnumerable<T>` interface and can be consumed using `await foreach`.</span></span>

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Proccess(o);
} 
```

### <a name="nullable-reference-types"></a><span data-ttu-id="37ec6-126">Tipi riferimento nullable</span><span class="sxs-lookup"><span data-stu-id="37ec6-126">Nullable reference types</span></span> 

<span data-ttu-id="37ec6-127">Quando questa nuova funzionalità è abilitata nel codice, EF Core possibile determinare il supporto di valori Null delle proprietà dei tipi refrence (tipi primitivi come le proprietà di stringa o di navigazione) per stabilire il supporto di valori null per le colonne e le relazioni nel database.</span><span class="sxs-lookup"><span data-stu-id="37ec6-127">When this new feature is enabled in your code, EF Core can reason about the nullability of properties of refrence types (either of primitive types like string or navigation properties) to decide the nullability of columns and relationships in the database.</span></span>

## <a name="interception"></a><span data-ttu-id="37ec6-128">Intercettazione</span><span class="sxs-lookup"><span data-stu-id="37ec6-128">Interception</span></span>

<span data-ttu-id="37ec6-129">La nuova API di intercettazione in EF Core 3,0 consente a a livello di osservare e modificare il risultato delle operazioni di database di basso livello che si verificano nell'ambito della normale operazione di EF Core, ad esempio l'apertura di connessioni, le transazioni initating e l'esecuzione di comandi.</span><span class="sxs-lookup"><span data-stu-id="37ec6-129">The new interception API in EF Core 3.0 allows programatically observing and modifying the outcome of low-level database operations that occur as part of the normal operation of EF Core, such as opening connections, initating transactions, and executing commands.</span></span> 

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="37ec6-130">Decompilazione delle viste di database</span><span class="sxs-lookup"><span data-stu-id="37ec6-130">Reverse engineering of database views</span></span>

<span data-ttu-id="37ec6-131">I tipi di entità senza chiavi (noti in precedenza come [tipi di query](xref:core/modeling/query-types)) rappresentano i dati che possono essere letti dal database, ma non possono essere aggiornati.</span><span class="sxs-lookup"><span data-stu-id="37ec6-131">Entity types without keys (previously known as [query types](xref:core/modeling/query-types)) represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="37ec6-132">Questa caratteristica li rende un ottimo adattamento per il mapping delle viste di database nella maggior parte degli scenari, quindi è stata automatizzata la creazione di tipi di entità senza chiavi quando reverse engineering viste di database.</span><span class="sxs-lookup"><span data-stu-id="37ec6-132">This characteristic makes them an excellent fit for mapping database views in most scenarios, so we automated the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="37ec6-133">Le entità dipendenti che condividono la tabella con l'entità di sicurezza sono ora facoltative</span><span class="sxs-lookup"><span data-stu-id="37ec6-133">Dependent entities sharing the table with the principal are now optional</span></span>

<span data-ttu-id="37ec6-134">A partire da EF Core 3.0, se `OrderDetails` è di proprietà di `Order` o mappato in modo esplicito alla stessa tabella, sarà possibile aggiungere un `Order` senza `OrderDetails` e tutte le proprietà di `OrderDetails`, a eccezione della chiave primaria, verranno mappate a colonne che ammettono i valori Null.</span><span class="sxs-lookup"><span data-stu-id="37ec6-134">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="37ec6-135">In fase di query, EF Core imposterà `OrderDetails` su `null` se una delle relative proprietà obbligatorie non ha un valore o se non sono presenti proprietà obbligatorie oltre alla chiave primaria e tutte le proprietà sono `null`.</span><span class="sxs-lookup"><span data-stu-id="37ec6-135">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

## <a name="ef-63-on-net-core"></a><span data-ttu-id="37ec6-136">EF 6.3 in .NET Core</span><span class="sxs-lookup"><span data-stu-id="37ec6-136">EF 6.3 on .NET Core</span></span>

<span data-ttu-id="37ec6-137">siamo consapevoli che molte applicazioni esistenti usano versioni precedenti di EF e che la loro conversione per EF Core al solo scopo di sfruttare i vantaggi di .NET Core può talvolta richiedere notevoli sforzi.</span><span class="sxs-lookup"><span data-stu-id="37ec6-137">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="37ec6-138">Per questo motivo, è stata abilitata la versione newewst di EF 6 per l'esecuzione in .NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="37ec6-138">For that reason, we have enabled the newewst version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="37ec6-139">Esistono alcune limitazioni, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="37ec6-139">There are some limitations, for example:</span></span>
- <span data-ttu-id="37ec6-140">Per lavorare in .NET Core sono necessari nuovi provider</span><span class="sxs-lookup"><span data-stu-id="37ec6-140">New providers are required to work on .NET Core</span></span>
- <span data-ttu-id="37ec6-141">Il supporto spaziale con SQL Server non sarà attivato</span><span class="sxs-lookup"><span data-stu-id="37ec6-141">Spatial support with SQL Server won't be enabled</span></span>

## <a name="postponed-features"></a><span data-ttu-id="37ec6-142">Funzionalità posticipate</span><span class="sxs-lookup"><span data-stu-id="37ec6-142">Postponed features</span></span>

<span data-ttu-id="37ec6-143">Alcune funzionalità pianificate originariamente per EF Core 3,0 sono state rimandate alle versioni future:</span><span class="sxs-lookup"><span data-stu-id="37ec6-143">Some features originally planned for EF Core 3.0 were postponed to future releases:</span></span> 

- <span data-ttu-id="37ec6-144">Possibilità di ingore delle parti di un modello nelle migrazioni, registrate da [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span><span class="sxs-lookup"><span data-stu-id="37ec6-144">Ability to ingore parts of a model in migrations, tracked by [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span></span>
- <span data-ttu-id="37ec6-145">Entità contenitore delle proprietà, registrate da due problemi distinti: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) sulle entità di tipo condiviso e [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) sul supporto del mapping delle proprietà indicizzate.</span><span class="sxs-lookup"><span data-stu-id="37ec6-145">Property bag entities, tracked by two separate issues: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) about shared-type entities and [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) about indexed property mapping support.</span></span>
