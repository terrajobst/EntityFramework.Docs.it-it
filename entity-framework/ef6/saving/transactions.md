---
title: Utilizzo di transazioni - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 26473e1e52a6044babc717d5b158ad73aac5c738
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250609"
---
# <a name="working-with-transactions"></a><span data-ttu-id="156f4-102">Utilizzo di transazioni</span><span class="sxs-lookup"><span data-stu-id="156f4-102">Working with Transactions</span></span>
> [!NOTE]
> <span data-ttu-id="156f4-103">**Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="156f4-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="156f4-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="156f4-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="156f4-105">Questo documento viene descritto con transazioni in EF6, inclusi i miglioramenti che sono stati aggiunti dall'EF5 per facilitare l'utilizzo di transazioni.</span><span class="sxs-lookup"><span data-stu-id="156f4-105">This document will describe using transactions in EF6 including the enhancements we have added since EF5 to make working with transactions easy.</span></span>  

## <a name="what-ef-does-by-default"></a><span data-ttu-id="156f4-106">Che cosa avviene per impostazione predefinita Entity Framework</span><span class="sxs-lookup"><span data-stu-id="156f4-106">What EF does by default</span></span>  

<span data-ttu-id="156f4-107">In tutte le versioni di Entity Framework, ogni volta che si esegue **SaveChanges ()** per inserire, aggiornare o eliminare il database il framework eseguirà il wrapping dell'operazione in una transazione.</span><span class="sxs-lookup"><span data-stu-id="156f4-107">In all versions of Entity Framework, whenever you execute **SaveChanges()** to insert, update or delete on the database the framework will wrap that operation in a transaction.</span></span> <span data-ttu-id="156f4-108">Questa operazione dura solo il tempo sufficiente per eseguire l'operazione e quindi viene completato.</span><span class="sxs-lookup"><span data-stu-id="156f4-108">This transaction lasts only long enough to execute the operation and then completes.</span></span> <span data-ttu-id="156f4-109">Quando si esegue un'altra operazione di questo tipo viene avviata una nuova transazione.</span><span class="sxs-lookup"><span data-stu-id="156f4-109">When you execute another such operation a new transaction is started.</span></span>  

<span data-ttu-id="156f4-110">A partire da EF6 **Database.ExecuteSqlCommand()** per impostazione predefinita va a capo il comando in una transazione se uno non era già presente.</span><span class="sxs-lookup"><span data-stu-id="156f4-110">Starting with EF6 **Database.ExecuteSqlCommand()** by default will wrap the command in a transaction if one was not already present.</span></span> <span data-ttu-id="156f4-111">Esistono overload del metodo che consentono di eseguire l'override di questo comportamento se si desidera.</span><span class="sxs-lookup"><span data-stu-id="156f4-111">There are overloads of this method that allow you to override this behavior if you wish.</span></span> <span data-ttu-id="156f4-112">Anche in EF6 l'esecuzione di stored procedure incluse nel modello tramite le API, ad esempio **ObjectContext.ExecuteFunction()** esegue la stessa (ad eccezione del fatto che il comportamento predefinito non è al momento eseguire l'override).</span><span class="sxs-lookup"><span data-stu-id="156f4-112">Also in EF6 execution of stored procedures included in the model through APIs such as **ObjectContext.ExecuteFunction()** does the same (except that the default behavior cannot at the moment be overridden).</span></span>  

<span data-ttu-id="156f4-113">In entrambi i casi, il livello di isolamento della transazione è qualsiasi livello di isolamento il provider di database prende in considerazione l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="156f4-113">In either case, the isolation level of the transaction is whatever isolation level the database provider considers its default setting.</span></span> <span data-ttu-id="156f4-114">Per impostazione predefinita, ad esempio, in SQL Server questo è READ COMMITTED.</span><span class="sxs-lookup"><span data-stu-id="156f4-114">By default, for instance, on SQL Server this is READ COMMITTED.</span></span>  

<span data-ttu-id="156f4-115">Entity Framework non va a capo le query in una transazione.</span><span class="sxs-lookup"><span data-stu-id="156f4-115">Entity Framework does not wrap queries in a transaction.</span></span>  

<span data-ttu-id="156f4-116">Questa funzionalità predefinita è adatta per molti utenti e se così non è necessario eseguire alcuna operazione diversa in EF6; basta scrivere il codice come sempre.</span><span class="sxs-lookup"><span data-stu-id="156f4-116">This default functionality is suitable for a lot of users and if so there is no need to do anything different in EF6; just write the code as you always did.</span></span>  

<span data-ttu-id="156f4-117">Tuttavia, alcuni utenti richiedono un maggiore controllo alle transazioni: questo argomento viene trattato nelle sezioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="156f4-117">However some users require greater control over their transactions – this is covered in the following sections.</span></span>  

## <a name="how-the-apis-work"></a><span data-ttu-id="156f4-118">Funzionamento delle API</span><span class="sxs-lookup"><span data-stu-id="156f4-118">How the APIs work</span></span>  

<span data-ttu-id="156f4-119">Prima di EF6 Entity Framework scoperto sull'apertura della connessione al database stesso (ha generato un'eccezione se è stata passata una connessione che era già aperta).</span><span class="sxs-lookup"><span data-stu-id="156f4-119">Prior to EF6 Entity Framework insisted on opening the database connection itself (it threw an exception if it was passed a connection that was already open).</span></span> <span data-ttu-id="156f4-120">Poiché una transazione può essere avviata solo su una connessione aperta, ciò significava che l'unico modo un utente è stato possibile eseguire il wrapping di diverse operazioni in un'unica transazione era di usare una [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) oppure usare il  **ObjectContext.Connection** proprietà e chiamare il metodo start **Open ()** e **BeginTransaction ()** direttamente sull'oggetto restituito **EntityConnection** oggetto.</span><span class="sxs-lookup"><span data-stu-id="156f4-120">Since a transaction can only be started on an open connection, this meant that the only way a user could wrap several operations into one transaction was either to use a [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) or use the **ObjectContext.Connection** property and start calling **Open()** and **BeginTransaction()** directly on the returned **EntityConnection** object.</span></span> <span data-ttu-id="156f4-121">Inoltre, le chiamate API che ha contattato il database avrebbe esito negativo se si ha avviato una transazione sulla connessione al database sottostante in modo autonomo.</span><span class="sxs-lookup"><span data-stu-id="156f4-121">In addition, API calls which contacted the database would fail if you had started a transaction on the underlying database connection on your own.</span></span>  

> [!NOTE]
> <span data-ttu-id="156f4-122">In Entity Framework 6 è stata rimossa la limitazione di accettare solo connessioni chiuse.</span><span class="sxs-lookup"><span data-stu-id="156f4-122">The limitation of only accepting closed connections was removed in Entity Framework 6.</span></span> <span data-ttu-id="156f4-123">Per informazioni dettagliate, vedere [gestione connessione](~/ef6/fundamentals/connection-management.md).</span><span class="sxs-lookup"><span data-stu-id="156f4-123">For details, see [Connection Management](~/ef6/fundamentals/connection-management.md).</span></span>  

<span data-ttu-id="156f4-124">A partire da Entity Framework 6 il framework ora fornisce:</span><span class="sxs-lookup"><span data-stu-id="156f4-124">Starting with EF6 the framework now provides:</span></span>  

1. <span data-ttu-id="156f4-125">**Database.BeginTransaction()** : un metodo più semplice per un utente di avviare e completare le transazioni stesse all'interno di un DbContext esistenti, consentendo alle diverse operazioni di essere combinati all'interno della stessa transazione e pertanto eseguito il commit oppure tutti il rollback di uno.</span><span class="sxs-lookup"><span data-stu-id="156f4-125">**Database.BeginTransaction()** : An easier method for a user to start and complete transactions themselves within an existing DbContext – allowing several operations to be combined within the same transaction and hence either all committed or all rolled back as one.</span></span> <span data-ttu-id="156f4-126">Consente inoltre all'utente di specificare più facilmente il livello di isolamento della transazione.</span><span class="sxs-lookup"><span data-stu-id="156f4-126">It also allows the user to more easily specify the isolation level for the transaction.</span></span>  
2. <span data-ttu-id="156f4-127">**Database.UseTransaction()** : che consente di utilizzare una transazione di cui è stata avviata all'esterno di Entity Framework DbContext.</span><span class="sxs-lookup"><span data-stu-id="156f4-127">**Database.UseTransaction()** : which allows the DbContext to use a transaction which was started outside of the Entity Framework.</span></span>  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a><span data-ttu-id="156f4-128">La combinazione di diverse operazioni in un'unica transazione nello stesso contesto</span><span class="sxs-lookup"><span data-stu-id="156f4-128">Combining several operations into one transaction within the same context</span></span>  

<span data-ttu-id="156f4-129">**Database.BeginTransaction()** include due sostituzioni: uno che accetta l'impostazione esplicita [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) e uno che non accetta argomenti e utilizza il valore predefinito IsolationLevel dal provider di database sottostante.</span><span class="sxs-lookup"><span data-stu-id="156f4-129">**Database.BeginTransaction()** has two overrides – one which takes an explicit [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) and one which takes no arguments and uses the default IsolationLevel from the underlying database provider.</span></span> <span data-ttu-id="156f4-130">In entrambi gli override restituiscono una **DbContextTransaction** oggetto che fornisce **commit ()** e **rollback ()** metodi che consentono di eseguire commit e rollback nell'archivio sottostante transazione.</span><span class="sxs-lookup"><span data-stu-id="156f4-130">Both overrides return a **DbContextTransaction** object which provides **Commit()** and **Rollback()** methods which perform commit and rollback on the underlying store transaction.</span></span>  

<span data-ttu-id="156f4-131">Il **DbContextTransaction** è progettato per essere eliminato dopo che è stato eseguito il commit o rollback.</span><span class="sxs-lookup"><span data-stu-id="156f4-131">The **DbContextTransaction** is meant to be disposed once it has been committed or rolled back.</span></span> <span data-ttu-id="156f4-132">Un semplice metodo per ottenere questo risultato è il **using(...) {...}**</span><span class="sxs-lookup"><span data-stu-id="156f4-132">One easy way to accomplish this is the **using(…) {…}**</span></span> <span data-ttu-id="156f4-133">sintassi che chiamerà automaticamente **Dispose ()** blocco using quando viene completata:</span><span class="sxs-lookup"><span data-stu-id="156f4-133">syntax which will automatically call **Dispose()** when the using block completes:</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void StartOwnTransactionWithinContext()
        {
            using (var context = new BloggingContext())
            {
                using (var dbContextTransaction = context.Database.BeginTransaction())
                {
                    try
                    {
                        context.Database.ExecuteSqlCommand(
                            @"UPDATE Blogs SET Rating = 5" +
                                " WHERE Name LIKE '%Entity Framework%'"
                            );

                        var query = context.Posts.Where(p => p.Blog.Rating >= 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }

                        context.SaveChanges();

                        dbContextTransaction.Commit();
                    }
                    catch (Exception)
                    {
                        dbContextTransaction.Rollback();
                    }
                }
            }
        }
    }
}
```  

> [!NOTE]
> <span data-ttu-id="156f4-134">Iniziare una transazione richiede che sia aperta la connessione all'archivio sottostante.</span><span class="sxs-lookup"><span data-stu-id="156f4-134">Beginning a transaction requires that the underlying store connection is open.</span></span> <span data-ttu-id="156f4-135">Quindi la chiamata Database.BeginTransaction() verrà aperta la connessione se non è già aperta.</span><span class="sxs-lookup"><span data-stu-id="156f4-135">So calling Database.BeginTransaction() will open the connection  if it is not already opened.</span></span> <span data-ttu-id="156f4-136">Se DbContextTransaction aperta la connessione quindi chiuderà quando viene chiamato Dispose ().</span><span class="sxs-lookup"><span data-stu-id="156f4-136">If DbContextTransaction opened the connection then it will close it when Dispose() is called.</span></span>  

### <a name="passing-an-existing-transaction-to-the-context"></a><span data-ttu-id="156f4-137">Il passaggio di una transazione esistente nel contesto</span><span class="sxs-lookup"><span data-stu-id="156f4-137">Passing an existing transaction to the context</span></span>  

<span data-ttu-id="156f4-138">A volte si vuole che una transazione di cui è ancora più ampio nell'ambito e include operazioni nello stesso database, ma all'esterno di EF completamente.</span><span class="sxs-lookup"><span data-stu-id="156f4-138">Sometimes you would like a transaction which is even broader in scope and which includes operations on the same database but outside of EF completely.</span></span> <span data-ttu-id="156f4-139">A tale scopo è necessario aprire la connessione e avviare manualmente la transazione e quindi indicare a Entity Framework) per usare la connessione al database già aperto e b) per utilizzare la transazione esistente su tale connessione.</span><span class="sxs-lookup"><span data-stu-id="156f4-139">To accomplish this you must open the connection and start the transaction yourself and then tell EF a) to use the already-opened database connection, and b) to use the existing transaction on that connection.</span></span>  

<span data-ttu-id="156f4-140">A tale scopo è necessario definire e usare un costruttore nella classe del contesto che eredita da uno dei costruttori che accettano booleani i) un parametro di connessione esistente e ii) i contextOwnsConnection DbContext.</span><span class="sxs-lookup"><span data-stu-id="156f4-140">To do this you must define and use a constructor on your context class which inherits from one of the DbContext constructors which take i) an existing connection parameter and ii) the contextOwnsConnection boolean.</span></span>  

> [!NOTE]
> <span data-ttu-id="156f4-141">Il flag contextOwnsConnection deve essere impostato su false quando viene chiamato in questo scenario.</span><span class="sxs-lookup"><span data-stu-id="156f4-141">The contextOwnsConnection flag must be set to false when called in this scenario.</span></span> <span data-ttu-id="156f4-142">Questo è importante in quanto indica a Entity Framework non deve chiudere la connessione dopo che è stata eseguita con esso (ad esempio, vedere la riga 4 di seguito):</span><span class="sxs-lookup"><span data-stu-id="156f4-142">This is important as it informs Entity Framework that it should not close the connection when it is done with it (for example, see line 4 below):</span></span>  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

<span data-ttu-id="156f4-143">Inoltre, è necessario avviare la transazione se stessi (incluso il IsolationLevel se si desidera evitare l'impostazione predefinita) e informare Entity Framework che vi sia una transazione esistente già avviata per la connessione (vedere la riga 33 riportato di seguito).</span><span class="sxs-lookup"><span data-stu-id="156f4-143">Furthermore, you must start the transaction yourself (including the IsolationLevel if you want to avoid the default setting) and let Entity Framework know that there is an existing transaction already started on the connection (see line 33 below).</span></span>  

<span data-ttu-id="156f4-144">Quindi si possono eseguire operazioni di database direttamente in SqlConnection stesso o in DbContext.</span><span class="sxs-lookup"><span data-stu-id="156f4-144">Then you are free to execute database operations either directly on the SqlConnection itself, or on the DbContext.</span></span> <span data-ttu-id="156f4-145">Tutte queste operazioni vengono eseguite all'interno di una transazione.</span><span class="sxs-lookup"><span data-stu-id="156f4-145">All such operations are executed within one transaction.</span></span> <span data-ttu-id="156f4-146">Assumersi la responsabilità per eseguire il commit o rollback della transazione e per la chiamata a Dispose () su di esso, nonché per la chiusura ed eliminazione della connessione di database.</span><span class="sxs-lookup"><span data-stu-id="156f4-146">You take responsibility for committing or rolling back the transaction and for calling Dispose() on it, as well as for closing and disposing the database connection.</span></span> <span data-ttu-id="156f4-147">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="156f4-147">For example:</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
sing System.Transactions;

namespace TransactionsExamples
{
     class TransactionsExample
     {
        static void UsingExternalTransaction()
        {
            using (var conn = new SqlConnection("..."))
            {
               conn.Open();

               using (var sqlTxn = conn.BeginTransaction(System.Data.IsolationLevel.Snapshot))
               {
                   try
                   {
                       var sqlCommand = new SqlCommand();
                       sqlCommand.Connection = conn;
                       sqlCommand.Transaction = sqlTxn;
                       sqlCommand.CommandText =
                           @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                       sqlCommand.ExecuteNonQuery();

                       using (var context =  
                         new BloggingContext(conn, contextOwnsConnection: false))
                        {
                            context.Database.UseTransaction(sqlTxn);

                            var query =  context.Posts.Where(p => p.Blog.Rating >= 5);
                            foreach (var post in query)
                            {
                                post.Title += "[Cool Blog]";
                            }
                           context.SaveChanges();
                        }

                        sqlTxn.Commit();
                    }
                    catch (Exception)
                    {
                        sqlTxn.Rollback();
                    }
                }
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a><span data-ttu-id="156f4-148">La cancellazione del contenuto della transazione</span><span class="sxs-lookup"><span data-stu-id="156f4-148">Clearing up the transaction</span></span>

<span data-ttu-id="156f4-149">È possibile passare null per Database.UseTransaction() per cancellare la conoscenza di Entity Framework della transazione corrente.</span><span class="sxs-lookup"><span data-stu-id="156f4-149">You can pass null to Database.UseTransaction() to clear Entity Framework’s knowledge of the current transaction.</span></span> <span data-ttu-id="156f4-150">Entity Framework verrà né commit né il rollback della transazione esistente quando si esegue questa operazione, pertanto usare con cautela e solo se si conosce questo è ciò che si desidera eseguire.</span><span class="sxs-lookup"><span data-stu-id="156f4-150">Entity Framework will neither commit nor rollback the existing transaction when you do this, so use with care and only if you’re sure this is what you want to do.</span></span>  

### <a name="errors-in-usetransaction"></a><span data-ttu-id="156f4-151">Errori in UseTransaction</span><span class="sxs-lookup"><span data-stu-id="156f4-151">Errors in UseTransaction</span></span>

<span data-ttu-id="156f4-152">Si noterà un'eccezione da Database.UseTransaction() se si passa una transazione quando:</span><span class="sxs-lookup"><span data-stu-id="156f4-152">You will see an exception from Database.UseTransaction() if you pass a transaction when:</span></span>  
- <span data-ttu-id="156f4-153">Entity Framework è già presente una transazione esistente</span><span class="sxs-lookup"><span data-stu-id="156f4-153">Entity Framework already has an existing transaction</span></span>  
- <span data-ttu-id="156f4-154">Entity Framework è già operativo all'interno di un ambito di transazione</span><span class="sxs-lookup"><span data-stu-id="156f4-154">Entity Framework is already operating within a TransactionScope</span></span>  
- <span data-ttu-id="156f4-155">L'oggetto di connessione nella transazione passato è null.</span><span class="sxs-lookup"><span data-stu-id="156f4-155">The connection object in the transaction passed is null.</span></span> <span data-ttu-id="156f4-156">Vale a dire, la transazione non è associata a una connessione – in genere questo è un segno di tale transazione già completata</span><span class="sxs-lookup"><span data-stu-id="156f4-156">That is, the transaction is not associated with a connection – usually this is a sign that that transaction has already completed</span></span>  
- <span data-ttu-id="156f4-157">L'oggetto di connessione nella transazione passato non corrisponde la connessione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="156f4-157">The connection object in the transaction passed does not match the Entity Framework’s connection.</span></span>  

## <a name="using-transactions-with-other-features"></a><span data-ttu-id="156f4-158">Utilizzo delle transazioni con altre funzionalità</span><span class="sxs-lookup"><span data-stu-id="156f4-158">Using transactions with other features</span></span>  

<span data-ttu-id="156f4-159">In questa sezione descrive come interagiscono le transazioni precedenti:</span><span class="sxs-lookup"><span data-stu-id="156f4-159">This section details how the above transactions interact with:</span></span>  

- <span data-ttu-id="156f4-160">Resilienza della connessione</span><span class="sxs-lookup"><span data-stu-id="156f4-160">Connection resiliency</span></span>  
- <span data-ttu-id="156f4-161">Metodi asincroni</span><span class="sxs-lookup"><span data-stu-id="156f4-161">Asynchronous methods</span></span>  
- <span data-ttu-id="156f4-162">Transazioni di TransactionScope</span><span class="sxs-lookup"><span data-stu-id="156f4-162">TransactionScope transactions</span></span>  

### <a name="connection-resiliency"></a><span data-ttu-id="156f4-163">Resilienza della connessione</span><span class="sxs-lookup"><span data-stu-id="156f4-163">Connection Resiliency</span></span>  

<span data-ttu-id="156f4-164">La nuova funzionalità resilienza della connessione non funziona con le transazioni avviate dall'utente.</span><span class="sxs-lookup"><span data-stu-id="156f4-164">The new Connection Resiliency feature does not work with user-initiated transactions.</span></span> <span data-ttu-id="156f4-165">Per informazioni dettagliate, vedere [ripetono strategie di esecuzione](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span><span class="sxs-lookup"><span data-stu-id="156f4-165">For details, see [Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span></span>  

### <a name="asynchronous-programming"></a><span data-ttu-id="156f4-166">Programmazione asincrona</span><span class="sxs-lookup"><span data-stu-id="156f4-166">Asynchronous Programming</span></span>  

<span data-ttu-id="156f4-167">L'approccio descritto nelle sezioni precedenti non richiede ulteriori opzioni o le impostazioni per lavorare con i [asincrono di query e salvare i metodi](~/ef6/fundamentals/async.md
).</span><span class="sxs-lookup"><span data-stu-id="156f4-167">The approach outlined in the previous sections needs no further options or settings to work with the [asynchronous query and save methods](~/ef6/fundamentals/async.md
).</span></span> <span data-ttu-id="156f4-168">Tuttavia, tenere presente che, a seconda delle operazioni eseguite all'interno di metodi asincroni, questo può comportare transazioni a esecuzione prolungata, che a sua volta possono causare deadlock o il blocco che non è valido per le prestazioni dell'applicazione complessiva.</span><span class="sxs-lookup"><span data-stu-id="156f4-168">But be aware that, depending on what you do within the asynchronous methods, this may result in long-running transactions – which can in turn cause deadlocks or blocking which is bad for the performance of the overall application.</span></span>  

### <a name="transactionscope-transactions"></a><span data-ttu-id="156f4-169">Transazioni di TransactionScope</span><span class="sxs-lookup"><span data-stu-id="156f4-169">TransactionScope Transactions</span></span>  

<span data-ttu-id="156f4-170">Prima di EF6 il modo consigliato di fornire le transazioni di ambito più grande era per usare un oggetto TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="156f4-170">Prior to EF6 the recommended way of providing larger scope transactions was to use a TransactionScope object:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void UsingTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeOption.Required))
            {
                using (var conn = new SqlConnection("..."))
                {
                    conn.Open();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    sqlCommand.ExecuteNonQuery();

                    using (var context =
                        new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }
                        context.SaveChanges();
                    }
                }

                scope.Complete();
            }
        }
    }
}
```  

<span data-ttu-id="156f4-171">Il SqlConnection Entity Framework potrebbe usare la transazione di ambiente TransactionScope e quindi eseguire il commit.</span><span class="sxs-lookup"><span data-stu-id="156f4-171">The SqlConnection and Entity Framework would both use the ambient TransactionScope transaction and hence be committed together.</span></span>  

<span data-ttu-id="156f4-172">A partire da .NET 4.5.1 TransactionScope è stata aggiornata per funzionare anche con i metodi asincroni tramite l'utilizzo dei [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumerazione:</span><span class="sxs-lookup"><span data-stu-id="156f4-172">Starting with .NET 4.5.1 TransactionScope has been updated to also work with asynchronous methods via the use of the [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeration:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        public static void AsyncTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeAsyncFlowOption.Enabled))
            {
                using (var conn = new SqlConnection("..."))
                {
                    await conn.OpenAsync();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    await sqlCommand.ExecuteNonQueryAsync();

                    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }

                        await context.SaveChangesAsync();
                    }
                }
            }
        }
    }
}
```  

<span data-ttu-id="156f4-173">Esistono comunque alcune limitazioni all'approccio TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="156f4-173">There are still some limitations to the TransactionScope approach:</span></span>  

- <span data-ttu-id="156f4-174">Richiede .NET 4.5.1 o versioni successive lavorare con i metodi asincroni.</span><span class="sxs-lookup"><span data-stu-id="156f4-174">Requires .NET 4.5.1 or greater to work with asynchronous methods.</span></span>  
- <span data-ttu-id="156f4-175">Non può essere usato negli scenari cloud, a meno che non è certi di utilizzare un'unica connessione (scenari basati su cloud non supportano le transazioni distribuite).</span><span class="sxs-lookup"><span data-stu-id="156f4-175">It cannot be used in cloud scenarios unless you are sure you have one and only one connection (cloud scenarios do not support distributed transactions).</span></span>  
- <span data-ttu-id="156f4-176">Non può essere combinato con l'approccio Database.UseTransaction() delle sezioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="156f4-176">It cannot be combined with the Database.UseTransaction() approach of the previous sections.</span></span>  
- <span data-ttu-id="156f4-177">Genererà eccezioni se si rilascia qualsiasi DDL e le transazioni distribuite tramite il servizio MSDTC non sono stati abilitati.</span><span class="sxs-lookup"><span data-stu-id="156f4-177">It will throw exceptions if you issue any DDL and have not enabled distributed transactions through the MSDTC Service.</span></span>  

<span data-ttu-id="156f4-178">Vantaggi dell'approccio TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="156f4-178">Advantages of the TransactionScope approach:</span></span>  

- <span data-ttu-id="156f4-179">Aggiorneranno automaticamente una transazione locale a una transazione distribuita se si imposta più di una connessione a un determinato database o combina una connessione a un database con una connessione a un database diverso nella stessa transazione (Nota: è necessario disporre di il servizio MSDTC è configurato per consentire le transazioni distribuite per funzionare).</span><span class="sxs-lookup"><span data-stu-id="156f4-179">It will automatically upgrade a local transaction to a distributed transaction if you make more than one connection to a given database or combine a connection to one database with a connection to a different database within the same transaction (note: you must have the MSDTC service configured to allow distributed transactions for this to work).</span></span>  
- <span data-ttu-id="156f4-180">Semplicità di codifica.</span><span class="sxs-lookup"><span data-stu-id="156f4-180">Ease of coding.</span></span> <span data-ttu-id="156f4-181">Se si preferisce la transazione di ambiente e trattate in modo implicito in background invece di controllare in modo esplicito in quindi l'approccio di TransactionScope potrebbe essere più appropriato è migliore.</span><span class="sxs-lookup"><span data-stu-id="156f4-181">If you prefer the transaction to be ambient and dealt with implicitly in the background rather than explicitly under you control then the TransactionScope approach may suit you better.</span></span>  

<span data-ttu-id="156f4-182">In breve, con la nuova Database.BeginTransaction() Database.UseTransaction() API precedente, l'approccio di TransactionScope è più necessario per la maggior parte degli utenti.</span><span class="sxs-lookup"><span data-stu-id="156f4-182">In summary, with the new Database.BeginTransaction() and Database.UseTransaction() APIs above, the TransactionScope approach is no longer necessary for most users.</span></span> <span data-ttu-id="156f4-183">Se si continua a usare TransactionScope tenere presente le limitazioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="156f4-183">If you do continue to use TransactionScope then be aware of the above limitations.</span></span> <span data-ttu-id="156f4-184">È consigliabile usare l'approccio descritto nelle sezioni precedenti invece laddove possibile.</span><span class="sxs-lookup"><span data-stu-id="156f4-184">We recommend using the approach outlined in the previous sections instead where possible.</span></span>  
