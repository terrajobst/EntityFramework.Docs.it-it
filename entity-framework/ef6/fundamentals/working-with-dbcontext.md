---
title: Utilizzo di DbContext - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
caps.latest.revision: 3
ms.openlocfilehash: 865c9883ce25f405a173791df4e46b98550cd41f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120801"
---
# <a name="working-with-dbcontext"></a><span data-ttu-id="9fa62-102">Utilizzo di DbContext</span><span class="sxs-lookup"><span data-stu-id="9fa62-102">Working with DbContext</span></span>

<span data-ttu-id="9fa62-103">Per usare Entity Framework per eseguire una query, inserire, aggiornare ed eliminare i dati con gli oggetti .NET, è necessario innanzitutto [creare un modello](~/ef6/modeling/index.md) che esegue il mapping di entità e relazioni definite nel modello per le tabelle in un database.</span><span class="sxs-lookup"><span data-stu-id="9fa62-103">In order to use Entity Framework to query, insert, update, and delete data using .NET objects, you first need to [Create a Model](~/ef6/modeling/index.md) which maps the entities and relationships that are defined in your model to tables in a database.</span></span>

<span data-ttu-id="9fa62-104">Dopo aver creato un modello, la classe primaria l'applicazione interagisca con è `System.Data.Entity.DbContext` (noto anche come la classe del contesto).</span><span class="sxs-lookup"><span data-stu-id="9fa62-104">Once you have a model, the primary class your application interacts with is `System.Data.Entity.DbContext` (often referred to as the context class).</span></span> <span data-ttu-id="9fa62-105">È possibile usare un oggetto DbContext associati a un modello per:</span><span class="sxs-lookup"><span data-stu-id="9fa62-105">You can use a DbContext associated to a model to:</span></span>
- <span data-ttu-id="9fa62-106">Scrivere ed eseguire query</span><span class="sxs-lookup"><span data-stu-id="9fa62-106">Write and execute queries</span></span>   
- <span data-ttu-id="9fa62-107">Materializzare i risultati della query come oggetti entità</span><span class="sxs-lookup"><span data-stu-id="9fa62-107">Materialize query results as entity objects</span></span>
- <span data-ttu-id="9fa62-108">Tenere traccia delle modifiche apportate agli oggetti</span><span class="sxs-lookup"><span data-stu-id="9fa62-108">Track changes that are made to those objects</span></span>
- <span data-ttu-id="9fa62-109">Salvare in modo permanente delle modifiche degli oggetti nel database</span><span class="sxs-lookup"><span data-stu-id="9fa62-109">Persist object changes back on the database</span></span>
- <span data-ttu-id="9fa62-110">Associare gli oggetti in memoria ai controlli dell'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="9fa62-110">Bind objects in memory to UI controls</span></span>

<span data-ttu-id="9fa62-111">Questa pagina offre alcune indicazioni su come gestire la classe del contesto.</span><span class="sxs-lookup"><span data-stu-id="9fa62-111">This page gives some guidance on how to manage the context class.</span></span>  

## <a name="defining-a-dbcontext-derived-class"></a><span data-ttu-id="9fa62-112">Definizione di una classe DbContext derivata</span><span class="sxs-lookup"><span data-stu-id="9fa62-112">Defining a DbContext derived class</span></span>  

<span data-ttu-id="9fa62-113">Il metodo consigliato per lavorare con contesto consiste nel definire una classe che deriva da DbContext e ne espone proprietà DbSet che rappresentano raccolte di entità nel contesto specificata.</span><span class="sxs-lookup"><span data-stu-id="9fa62-113">The recommended way to work with context is to define a class that derives from DbContext and exposes DbSet properties that represent collections of the specified entities in the context.</span></span> <span data-ttu-id="9fa62-114">Se si lavora con la finestra di progettazione di Entity Framework, il contesto verrà generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="9fa62-114">If you are working with the EF Designer, the context will be generated for you.</span></span> <span data-ttu-id="9fa62-115">Se si lavora con Code First, è in genere scriverà il contesto di se stessi.</span><span class="sxs-lookup"><span data-stu-id="9fa62-115">If you are working with Code First, you will typically write the context yourself.</span></span>  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

<span data-ttu-id="9fa62-116">Dopo aver creato un contesto, è necessario eseguire una query per, aggiungere (mediante `Add` oppure `Attach` metodi) o rimuovere (usando `Remove`) entità nel contesto tramite queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="9fa62-116">Once you have a context, you would query for, add (using `Add` or `Attach` methods ) or remove (using `Remove`) entities in the context through these properties.</span></span> <span data-ttu-id="9fa62-117">L'accesso a un `DbSet` proprietà su un oggetto di contesto rappresenta una query iniziale che restituisce tutte le entità del tipo specificato.</span><span class="sxs-lookup"><span data-stu-id="9fa62-117">Accessing a `DbSet` property on a context object represent a starting query that returns all entities of the specified type.</span></span> <span data-ttu-id="9fa62-118">Si noti che l'accesso solo a una proprietà non eseguirà la query.</span><span class="sxs-lookup"><span data-stu-id="9fa62-118">Note that just accessing a property will not execute the query.</span></span> <span data-ttu-id="9fa62-119">Una query viene eseguita quando:</span><span class="sxs-lookup"><span data-stu-id="9fa62-119">A query is executed when:</span></span>  

- <span data-ttu-id="9fa62-120">Enumerata da un'istruzione `foreach` (C#) o `For Each` (Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="9fa62-120">It is enumerated by a `foreach` (C#) or `For Each` (Visual Basic) statement.</span></span>  
- <span data-ttu-id="9fa62-121">Enumerata da un'operazione di raccolta, ad esempio `ToArray`, `ToDictionary`, o `ToList`.</span><span class="sxs-lookup"><span data-stu-id="9fa62-121">It is enumerated by a collection operation such as `ToArray`, `ToDictionary`, or `ToList`.</span></span>  
- <span data-ttu-id="9fa62-122">Gli operatori LINQ come `First` o `Any` vengono specificati nella parte più esterna della query.</span><span class="sxs-lookup"><span data-stu-id="9fa62-122">LINQ operators such as `First` or `Any` are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="9fa62-123">Viene chiamato uno dei metodi seguenti: il `Load` metodo di estensione `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`, e `DbSet<T>.Find`, se un'entità con la chiave specificata non è presente già caricate nel contesto.</span><span class="sxs-lookup"><span data-stu-id="9fa62-123">One of the following methods are called: the `Load` extension method, `DbEntityEntry.Reload`,  `Database.ExecuteSqlCommand`, and `DbSet<T>.Find`, if an entity with the specified key is not found already loaded in the context.</span></span>  

## <a name="lifetime"></a><span data-ttu-id="9fa62-124">Durata</span><span class="sxs-lookup"><span data-stu-id="9fa62-124">Lifetime</span></span>  

<span data-ttu-id="9fa62-125">La durata del contesto inizia quando l'istanza viene creata e termina quando l'istanza viene eliminata o sottoposto a garbage collection.</span><span class="sxs-lookup"><span data-stu-id="9fa62-125">The lifetime of the context begins when the instance is created and ends when the instance is either disposed or garbage-collected.</span></span> <span data-ttu-id="9fa62-126">Uso **usando** se si desidera che tutte le risorse controllate dal contesto siano eliminate alla fine del blocco.</span><span class="sxs-lookup"><span data-stu-id="9fa62-126">Use **using** if you want all the resources that the context controls to be disposed at the end of the block.</span></span> <span data-ttu-id="9fa62-127">Quando si usa **usando**, il compilatore crea automaticamente un blocco try/finally e chiama dispose nel **infine** blocco.</span><span class="sxs-lookup"><span data-stu-id="9fa62-127">When you use **using**, the compiler automatically creates a try/finally block and calls dispose in the **finally** block.</span></span>  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

<span data-ttu-id="9fa62-128">Di seguito sono riportate alcune linee guida generali prima di decidere sulla durata del contesto:</span><span class="sxs-lookup"><span data-stu-id="9fa62-128">Here are some general guidelines when deciding on the lifetime of the context:</span></span>  

- <span data-ttu-id="9fa62-129">Quando si lavora con le applicazioni Web, usare un'istanza del contesto per ogni richiesta.</span><span class="sxs-lookup"><span data-stu-id="9fa62-129">When working with Web applications, use a context instance per request.</span></span>  
- <span data-ttu-id="9fa62-130">Quando si lavora con Windows Presentation Foundation (WPF) o Windows Form, usare un'istanza del contesto per ogni modulo.</span><span class="sxs-lookup"><span data-stu-id="9fa62-130">When working with Windows Presentation Foundation (WPF) or Windows Forms, use a context instance per form.</span></span> <span data-ttu-id="9fa62-131">Ciò consente di usare la funzionalità di rilevamento delle modifiche fornisce tale contesto.</span><span class="sxs-lookup"><span data-stu-id="9fa62-131">This lets you use change-tracking functionality that context provides.</span></span>  
- <span data-ttu-id="9fa62-132">Se l'istanza del contesto viene creato da un contenitore di inserimento delle dipendenze, è in genere la responsabilità del contenitore da eliminare il contesto.</span><span class="sxs-lookup"><span data-stu-id="9fa62-132">If the context instance is created by a dependency injection container, it is usually the responsibility of the container to dispose the context.</span></span>
- <span data-ttu-id="9fa62-133">Se il contesto viene creato nel codice dell'applicazione, ricordarsi di eliminare il contesto quando non è più necessaria.</span><span class="sxs-lookup"><span data-stu-id="9fa62-133">If the context is created in application code, remember to dispose of the context when it is no longer required.</span></span>  
- <span data-ttu-id="9fa62-134">Quando si lavora sul contesto con esecuzione prolungata tenere presente quanto segue:</span><span class="sxs-lookup"><span data-stu-id="9fa62-134">When working with long-running context consider the following:</span></span>  
    - <span data-ttu-id="9fa62-135">Come si caricano più oggetti e i relativi riferimenti in memoria, il consumo di memoria del contesto può aumentare rapidamente.</span><span class="sxs-lookup"><span data-stu-id="9fa62-135">As you load more objects and their references into memory, the memory consumption of the context may increase rapidly.</span></span> <span data-ttu-id="9fa62-136">È possibile pertanto che si verifichino problemi di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="9fa62-136">This may cause performance issues.</span></span>  
    - <span data-ttu-id="9fa62-137">Il contesto non è thread-safe, pertanto non deve essere condivise tra più thread lavorare contemporaneamente su di esso.</span><span class="sxs-lookup"><span data-stu-id="9fa62-137">The context is not thread-safe, therefore it should not be shared across multiple threads doing work on it concurrently.</span></span>
    - <span data-ttu-id="9fa62-138">Se un'eccezione provoca uno stato irreversibile per il contesto, l'intera applicazione può terminare.</span><span class="sxs-lookup"><span data-stu-id="9fa62-138">If an exception causes the context to be in an unrecoverable state, the whole application may terminate.</span></span>  
    - <span data-ttu-id="9fa62-139">Le possibilità che si verifichino problemi correlati alla concorrenza aumentano quando l'intervallo tra il momento in cui viene eseguita una query sui dati e quello in cui i dati vengono aggiornati aumenta.</span><span class="sxs-lookup"><span data-stu-id="9fa62-139">The chances of running into concurrency-related issues increase as the gap between the time when the data is queried and updated grows.</span></span>  

## <a name="connections"></a><span data-ttu-id="9fa62-140">Connessioni</span><span class="sxs-lookup"><span data-stu-id="9fa62-140">Connections</span></span>  

<span data-ttu-id="9fa62-141">Per impostazione predefinita, il contesto gestisce le connessioni al database.</span><span class="sxs-lookup"><span data-stu-id="9fa62-141">By default, the context manages connections to the database.</span></span> <span data-ttu-id="9fa62-142">Il contesto apre e chiude le connessioni in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="9fa62-142">The context opens and closes connections as needed.</span></span> <span data-ttu-id="9fa62-143">Ad esempio, il contesto viene aperta una connessione per eseguire una query e quindi chiude la connessione quando sono stati elaborati tutti i set di risultati.</span><span class="sxs-lookup"><span data-stu-id="9fa62-143">For example, the context opens a connection to execute a query, and then closes the connection when all the result sets have been processed.</span></span>  

<span data-ttu-id="9fa62-144">In alcuni casi, tuttavia, può essere necessario disporre di maggiore controllo sull'apertura e la chiusura della connessione.</span><span class="sxs-lookup"><span data-stu-id="9fa62-144">There are cases when you want to have more control over when the connection opens and closes.</span></span> <span data-ttu-id="9fa62-145">Ad esempio, quando si usa SQL Server Compact, è spesso consigliabile mantenere una connessione aperta separata per il database per la durata dell'applicazione per migliorare le prestazioni.</span><span class="sxs-lookup"><span data-stu-id="9fa62-145">For example, when working with SQL Server Compact, it is often recommended to maintain a separate open connection to the database for the lifetime of the application to improve performance.</span></span> <span data-ttu-id="9fa62-146">È possibile gestire manualmente questo processo tramite la proprietà `Connection`.</span><span class="sxs-lookup"><span data-stu-id="9fa62-146">You can manage this process manually by using the `Connection` property.</span></span>  
