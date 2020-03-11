---
title: Uso delle transazioni-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 7030dc675993339f72c935f6b430cead85fecb7f
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419685"
---
# <a name="working-with-transactions"></a><span data-ttu-id="dd30b-102">Utilizzo delle transazioni</span><span class="sxs-lookup"><span data-stu-id="dd30b-102">Working with Transactions</span></span>
> [!NOTE]
> <span data-ttu-id="dd30b-103">**Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="dd30b-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="dd30b-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="dd30b-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="dd30b-105">Questo documento descrive l'uso delle transazioni in EF6, inclusi i miglioramenti aggiunti da EF5 per semplificare l'utilizzo delle transazioni.</span><span class="sxs-lookup"><span data-stu-id="dd30b-105">This document will describe using transactions in EF6 including the enhancements we have added since EF5 to make working with transactions easy.</span></span>  

## <a name="what-ef-does-by-default"></a><span data-ttu-id="dd30b-106">Che cosa EF esegue per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="dd30b-106">What EF does by default</span></span>  

<span data-ttu-id="dd30b-107">In tutte le versioni di Entity Framework, ogni volta che si esegue **SaveChanges ()** per inserire, aggiornare o eliminare nel database, il Framework eseguirà il wrapping dell'operazione in una transazione.</span><span class="sxs-lookup"><span data-stu-id="dd30b-107">In all versions of Entity Framework, whenever you execute **SaveChanges()** to insert, update or delete on the database the framework will wrap that operation in a transaction.</span></span> <span data-ttu-id="dd30b-108">Questa transazione dura solo un tempo sufficiente per eseguire l'operazione e quindi viene completata.</span><span class="sxs-lookup"><span data-stu-id="dd30b-108">This transaction lasts only long enough to execute the operation and then completes.</span></span> <span data-ttu-id="dd30b-109">Quando si esegue un'altra operazione di questo tipo, viene avviata una nuova transazione.</span><span class="sxs-lookup"><span data-stu-id="dd30b-109">When you execute another such operation a new transaction is started.</span></span>  

<span data-ttu-id="dd30b-110">A partire da EF6 **database. ExecuteSqlCommand ()** per impostazione predefinita esegue il wrapping del comando in una transazione se non ne è già presente uno.</span><span class="sxs-lookup"><span data-stu-id="dd30b-110">Starting with EF6 **Database.ExecuteSqlCommand()** by default will wrap the command in a transaction if one was not already present.</span></span> <span data-ttu-id="dd30b-111">Sono disponibili overload di questo metodo che consentono di eseguire l'override di questo comportamento, se lo si desidera.</span><span class="sxs-lookup"><span data-stu-id="dd30b-111">There are overloads of this method that allow you to override this behavior if you wish.</span></span> <span data-ttu-id="dd30b-112">Inoltre, in EF6 l'esecuzione di stored procedure incluse nel modello tramite API come **ObjectContext. ExecuteFunction ()** esegue la stessa operazione (ad eccezione del fatto che al momento non è possibile eseguire l'override del comportamento predefinito).</span><span class="sxs-lookup"><span data-stu-id="dd30b-112">Also in EF6 execution of stored procedures included in the model through APIs such as **ObjectContext.ExecuteFunction()** does the same (except that the default behavior cannot at the moment be overridden).</span></span>  

<span data-ttu-id="dd30b-113">In entrambi i casi, il livello di isolamento della transazione corrisponde al livello di isolamento che il provider di database considera l'impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="dd30b-113">In either case, the isolation level of the transaction is whatever isolation level the database provider considers its default setting.</span></span> <span data-ttu-id="dd30b-114">Per impostazione predefinita, ad esempio, in SQL Server si tratta di READ COMMITTED.</span><span class="sxs-lookup"><span data-stu-id="dd30b-114">By default, for instance, on SQL Server this is READ COMMITTED.</span></span>  

<span data-ttu-id="dd30b-115">Entity Framework non esegue il wrapping delle query in una transazione.</span><span class="sxs-lookup"><span data-stu-id="dd30b-115">Entity Framework does not wrap queries in a transaction.</span></span>  

<span data-ttu-id="dd30b-116">Questa funzionalità predefinita è adatta per un numero elevato di utenti e, in caso affermativo, non è necessario eseguire alcuna operazione diversa in EF6; è sufficiente scrivere il codice come è sempre stato fatto.</span><span class="sxs-lookup"><span data-stu-id="dd30b-116">This default functionality is suitable for a lot of users and if so there is no need to do anything different in EF6; just write the code as you always did.</span></span>  

<span data-ttu-id="dd30b-117">Tuttavia, alcuni utenti richiedono un maggiore controllo sulle transazioni. questa procedura è illustrata nelle sezioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="dd30b-117">However some users require greater control over their transactions – this is covered in the following sections.</span></span>  

## <a name="how-the-apis-work"></a><span data-ttu-id="dd30b-118">Funzionamento delle API</span><span class="sxs-lookup"><span data-stu-id="dd30b-118">How the APIs work</span></span>  

<span data-ttu-id="dd30b-119">Prima di EF6 Entity Framework insistito sull'apertura della connessione al database stessa (generava un'eccezione se era stata passata una connessione già aperta).</span><span class="sxs-lookup"><span data-stu-id="dd30b-119">Prior to EF6 Entity Framework insisted on opening the database connection itself (it threw an exception if it was passed a connection that was already open).</span></span> <span data-ttu-id="dd30b-120">Poiché una transazione può essere avviata solo su una connessione aperta, questo significa che l'unico modo in cui un utente può eseguire il wrapping di più operazioni in un'unica transazione è stato usare un [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) o usare la proprietà **ObjectContext. Connection** e iniziare a chiamare **Open ()** e **BeginTransaction ()** direttamente nell'oggetto **EntityConnection** restituito.</span><span class="sxs-lookup"><span data-stu-id="dd30b-120">Since a transaction can only be started on an open connection, this meant that the only way a user could wrap several operations into one transaction was either to use a [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) or use the **ObjectContext.Connection** property and start calling **Open()** and **BeginTransaction()** directly on the returned **EntityConnection** object.</span></span> <span data-ttu-id="dd30b-121">Inoltre, le chiamate API che hanno contattato il database avrebbero esito negativo se era stata avviata una transazione sulla connessione al database sottostante.</span><span class="sxs-lookup"><span data-stu-id="dd30b-121">In addition, API calls which contacted the database would fail if you had started a transaction on the underlying database connection on your own.</span></span>  

> [!NOTE]
> <span data-ttu-id="dd30b-122">La limitazione dell'accettazione solo delle connessioni chiuse è stata rimossa in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="dd30b-122">The limitation of only accepting closed connections was removed in Entity Framework 6.</span></span> <span data-ttu-id="dd30b-123">Per informazioni dettagliate, vedere [gestione della connessione](~/ef6/fundamentals/connection-management.md).</span><span class="sxs-lookup"><span data-stu-id="dd30b-123">For details, see [Connection Management](~/ef6/fundamentals/connection-management.md).</span></span>  

<span data-ttu-id="dd30b-124">A partire da EF6, il Framework offre ora:</span><span class="sxs-lookup"><span data-stu-id="dd30b-124">Starting with EF6 the framework now provides:</span></span>  

1. <span data-ttu-id="dd30b-125">**Database. BeginTransaction ()** : un metodo più semplice per l'avvio e il completamento delle transazioni all'interno di un DbContext esistente, che consente di combinare diverse operazioni all'interno della stessa transazione e quindi di eseguire il commit o il rollback di tutte.</span><span class="sxs-lookup"><span data-stu-id="dd30b-125">**Database.BeginTransaction()** : An easier method for a user to start and complete transactions themselves within an existing DbContext – allowing several operations to be combined within the same transaction and hence either all committed or all rolled back as one.</span></span> <span data-ttu-id="dd30b-126">Consente inoltre all'utente di specificare più facilmente il livello di isolamento per la transazione.</span><span class="sxs-lookup"><span data-stu-id="dd30b-126">It also allows the user to more easily specify the isolation level for the transaction.</span></span>  
2. <span data-ttu-id="dd30b-127">**Database. UseTransaction ()** : che consente al DbContext di usare una transazione che è stata avviata all'esterno del Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="dd30b-127">**Database.UseTransaction()** : which allows the DbContext to use a transaction which was started outside of the Entity Framework.</span></span>  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a><span data-ttu-id="dd30b-128">Combinazione di più operazioni in un'unica transazione nello stesso contesto</span><span class="sxs-lookup"><span data-stu-id="dd30b-128">Combining several operations into one transaction within the same context</span></span>  

<span data-ttu-id="dd30b-129">**Database. BeginTransaction ()** dispone di due sostituzioni: una che accetta un valore [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) esplicito e uno che non accetta argomenti e usa l'oggetto IsolationLevel predefinito del provider di database sottostante.</span><span class="sxs-lookup"><span data-stu-id="dd30b-129">**Database.BeginTransaction()** has two overrides – one which takes an explicit [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) and one which takes no arguments and uses the default IsolationLevel from the underlying database provider.</span></span> <span data-ttu-id="dd30b-130">Entrambe le sostituzioni restituiscono un oggetto **DbContextTransaction** che fornisce metodi **commit ()** e **rollback ()** che eseguono il commit e il rollback della transazione di archivio sottostante.</span><span class="sxs-lookup"><span data-stu-id="dd30b-130">Both overrides return a **DbContextTransaction** object which provides **Commit()** and **Rollback()** methods which perform commit and rollback on the underlying store transaction.</span></span>  

<span data-ttu-id="dd30b-131">La **DbContextTransaction** deve essere eliminata una volta eseguito il commit o il rollback.</span><span class="sxs-lookup"><span data-stu-id="dd30b-131">The **DbContextTransaction** is meant to be disposed once it has been committed or rolled back.</span></span> <span data-ttu-id="dd30b-132">Un modo semplice per eseguire questa operazione è l' **uso di (...) {...}**</span><span class="sxs-lookup"><span data-stu-id="dd30b-132">One easy way to accomplish this is the **using(…) {…}**</span></span> <span data-ttu-id="dd30b-133">sintassi che chiamerà automaticamente **Dispose ()** al completamento del blocco using:</span><span class="sxs-lookup"><span data-stu-id="dd30b-133">syntax which will automatically call **Dispose()** when the using block completes:</span></span>  

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
            }
        }
    }
}
```  

> [!NOTE]
> <span data-ttu-id="dd30b-134">Per iniziare una transazione è necessario che sia aperta la connessione all'archivio sottostante.</span><span class="sxs-lookup"><span data-stu-id="dd30b-134">Beginning a transaction requires that the underlying store connection is open.</span></span> <span data-ttu-id="dd30b-135">Quindi, chiamando database. BeginTransaction (), la connessione verrà aperta se non è già aperta.</span><span class="sxs-lookup"><span data-stu-id="dd30b-135">So calling Database.BeginTransaction() will open the connection  if it is not already opened.</span></span> <span data-ttu-id="dd30b-136">Se DbContextTransaction ha aperto la connessione, verrà chiusa quando viene chiamato il metodo Dispose ().</span><span class="sxs-lookup"><span data-stu-id="dd30b-136">If DbContextTransaction opened the connection then it will close it when Dispose() is called.</span></span>  

### <a name="passing-an-existing-transaction-to-the-context"></a><span data-ttu-id="dd30b-137">Passaggio di una transazione esistente al contesto</span><span class="sxs-lookup"><span data-stu-id="dd30b-137">Passing an existing transaction to the context</span></span>  

<span data-ttu-id="dd30b-138">A volte si desidera una transazione che sia ancora più ampia nell'ambito e che includa le operazioni sullo stesso database ma al di fuori di EF completamente.</span><span class="sxs-lookup"><span data-stu-id="dd30b-138">Sometimes you would like a transaction which is even broader in scope and which includes operations on the same database but outside of EF completely.</span></span> <span data-ttu-id="dd30b-139">A tale scopo, è necessario aprire la connessione e avviare la transazione manualmente, quindi indicare a EF a) di utilizzare la connessione al database già aperta e b) per utilizzare la transazione esistente sulla connessione.</span><span class="sxs-lookup"><span data-stu-id="dd30b-139">To accomplish this you must open the connection and start the transaction yourself and then tell EF a) to use the already-opened database connection, and b) to use the existing transaction on that connection.</span></span>  

<span data-ttu-id="dd30b-140">A tale scopo, è necessario definire e usare un costruttore nella classe Context che eredita da uno dei costruttori DbContext che accettano i, un parametro di connessione esistente e II) il valore booleano contextOwnsConnection.</span><span class="sxs-lookup"><span data-stu-id="dd30b-140">To do this you must define and use a constructor on your context class which inherits from one of the DbContext constructors which take i) an existing connection parameter and ii) the contextOwnsConnection boolean.</span></span>  

> [!NOTE]
> <span data-ttu-id="dd30b-141">Quando viene chiamato in questo scenario, il flag contextOwnsConnection deve essere impostato su false.</span><span class="sxs-lookup"><span data-stu-id="dd30b-141">The contextOwnsConnection flag must be set to false when called in this scenario.</span></span> <span data-ttu-id="dd30b-142">Questo è importante in quanto informa Entity Framework che non dovrebbe chiudere la connessione al termine dell'operazione (ad esempio, vedere la riga 4 riportata di seguito):</span><span class="sxs-lookup"><span data-stu-id="dd30b-142">This is important as it informs Entity Framework that it should not close the connection when it is done with it (for example, see line 4 below):</span></span>  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

<span data-ttu-id="dd30b-143">Inoltre, è necessario avviare la transazione manualmente, incluso il valore IsolationLevel se si desidera evitare l'impostazione predefinita, e consentire Entity Framework sapere che è già stata avviata una transazione nella connessione (vedere la riga 33 di seguito).</span><span class="sxs-lookup"><span data-stu-id="dd30b-143">Furthermore, you must start the transaction yourself (including the IsolationLevel if you want to avoid the default setting) and let Entity Framework know that there is an existing transaction already started on the connection (see line 33 below).</span></span>  

<span data-ttu-id="dd30b-144">È quindi possibile eseguire le operazioni di database direttamente in SqlConnection o in DbContext.</span><span class="sxs-lookup"><span data-stu-id="dd30b-144">Then you are free to execute database operations either directly on the SqlConnection itself, or on the DbContext.</span></span> <span data-ttu-id="dd30b-145">Tutte queste operazioni vengono eseguite all'interno di una transazione.</span><span class="sxs-lookup"><span data-stu-id="dd30b-145">All such operations are executed within one transaction.</span></span> <span data-ttu-id="dd30b-146">Si assume la responsabilità di eseguire il commit o il rollback della transazione e di chiamare Dispose () su di essa, nonché di chiudere ed eliminare la connessione al database.</span><span class="sxs-lookup"><span data-stu-id="dd30b-146">You take responsibility for committing or rolling back the transaction and for calling Dispose() on it, as well as for closing and disposing the database connection.</span></span> <span data-ttu-id="dd30b-147">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="dd30b-147">For example:</span></span>  

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
        static void UsingExternalTransaction()
        {
            using (var conn = new SqlConnection("..."))
            {
               conn.Open();

               using (var sqlTxn = conn.BeginTransaction(System.Data.IsolationLevel.Snapshot))
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
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a><span data-ttu-id="dd30b-148">Cancellazione della transazione</span><span class="sxs-lookup"><span data-stu-id="dd30b-148">Clearing up the transaction</span></span>

<span data-ttu-id="dd30b-149">È possibile passare null a database. UseTransaction () per cancellare la conoscenza Entity Framework della transazione corrente.</span><span class="sxs-lookup"><span data-stu-id="dd30b-149">You can pass null to Database.UseTransaction() to clear Entity Framework’s knowledge of the current transaction.</span></span> <span data-ttu-id="dd30b-150">Se non si esegue questa operazione, non verrà eseguito il commit né il rollback della transazione esistente. utilizzare quindi con cautela e solo se si è certi di cosa si desidera eseguire. Entity Framework</span><span class="sxs-lookup"><span data-stu-id="dd30b-150">Entity Framework will neither commit nor rollback the existing transaction when you do this, so use with care and only if you’re sure this is what you want to do.</span></span>  

### <a name="errors-in-usetransaction"></a><span data-ttu-id="dd30b-151">Errori in UseTransaction</span><span class="sxs-lookup"><span data-stu-id="dd30b-151">Errors in UseTransaction</span></span>

<span data-ttu-id="dd30b-152">Si noterà un'eccezione da database. UseTransaction () se si passa una transazione nei casi seguenti:</span><span class="sxs-lookup"><span data-stu-id="dd30b-152">You will see an exception from Database.UseTransaction() if you pass a transaction when:</span></span>  
- <span data-ttu-id="dd30b-153">Entity Framework dispone già di una transazione esistente</span><span class="sxs-lookup"><span data-stu-id="dd30b-153">Entity Framework already has an existing transaction</span></span>  
- <span data-ttu-id="dd30b-154">Entity Framework è già in uso in un TransactionScope</span><span class="sxs-lookup"><span data-stu-id="dd30b-154">Entity Framework is already operating within a TransactionScope</span></span>  
- <span data-ttu-id="dd30b-155">L'oggetto connessione nella transazione passata è null.</span><span class="sxs-lookup"><span data-stu-id="dd30b-155">The connection object in the transaction passed is null.</span></span> <span data-ttu-id="dd30b-156">Ovvero, la transazione non è associata a una connessione, in genere si tratta di un segno che la transazione è già stata completata</span><span class="sxs-lookup"><span data-stu-id="dd30b-156">That is, the transaction is not associated with a connection – usually this is a sign that that transaction has already completed</span></span>  
- <span data-ttu-id="dd30b-157">L'oggetto connessione nella transazione passata non corrisponde alla connessione del Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="dd30b-157">The connection object in the transaction passed does not match the Entity Framework’s connection.</span></span>  

## <a name="using-transactions-with-other-features"></a><span data-ttu-id="dd30b-158">Utilizzo di transazioni con altre funzionalità</span><span class="sxs-lookup"><span data-stu-id="dd30b-158">Using transactions with other features</span></span>  

<span data-ttu-id="dd30b-159">Questa sezione illustra in dettaglio il modo in cui le transazioni precedenti interagiscono con:</span><span class="sxs-lookup"><span data-stu-id="dd30b-159">This section details how the above transactions interact with:</span></span>  

- <span data-ttu-id="dd30b-160">Resilienza delle connessioni</span><span class="sxs-lookup"><span data-stu-id="dd30b-160">Connection resiliency</span></span>  
- <span data-ttu-id="dd30b-161">Metodi asincroni</span><span class="sxs-lookup"><span data-stu-id="dd30b-161">Asynchronous methods</span></span>  
- <span data-ttu-id="dd30b-162">Transazioni TransactionScope</span><span class="sxs-lookup"><span data-stu-id="dd30b-162">TransactionScope transactions</span></span>  

### <a name="connection-resiliency"></a><span data-ttu-id="dd30b-163">Resilienza della connessione</span><span class="sxs-lookup"><span data-stu-id="dd30b-163">Connection Resiliency</span></span>  

<span data-ttu-id="dd30b-164">La nuova funzionalità di resilienza della connessione non funziona con le transazioni avviate dall'utente.</span><span class="sxs-lookup"><span data-stu-id="dd30b-164">The new Connection Resiliency feature does not work with user-initiated transactions.</span></span> <span data-ttu-id="dd30b-165">Per informazioni dettagliate, vedere ripetizione delle [strategie di esecuzione](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span><span class="sxs-lookup"><span data-stu-id="dd30b-165">For details, see [Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).</span></span>  

### <a name="asynchronous-programming"></a><span data-ttu-id="dd30b-166">Programmazione asincrona</span><span class="sxs-lookup"><span data-stu-id="dd30b-166">Asynchronous Programming</span></span>  

<span data-ttu-id="dd30b-167">L'approccio descritto nelle sezioni precedenti non necessita di altre opzioni o impostazioni per lavorare con i metodi di [query e salvataggio asincroni](~/ef6/fundamentals/async.md
).</span><span class="sxs-lookup"><span data-stu-id="dd30b-167">The approach outlined in the previous sections needs no further options or settings to work with the [asynchronous query and save methods](~/ef6/fundamentals/async.md
).</span></span> <span data-ttu-id="dd30b-168">Tenere tuttavia presente che, a seconda delle operazioni eseguite all'interno dei metodi asincroni, è possibile che si verifichino transazioni con esecuzione prolungata, che possono a sua volta causare deadlock o un blocco negativo per le prestazioni dell'applicazione complessiva.</span><span class="sxs-lookup"><span data-stu-id="dd30b-168">But be aware that, depending on what you do within the asynchronous methods, this may result in long-running transactions – which can in turn cause deadlocks or blocking which is bad for the performance of the overall application.</span></span>  

### <a name="transactionscope-transactions"></a><span data-ttu-id="dd30b-169">Transazioni TransactionScope</span><span class="sxs-lookup"><span data-stu-id="dd30b-169">TransactionScope Transactions</span></span>  

<span data-ttu-id="dd30b-170">Prima di EF6 il metodo consigliato per fornire transazioni con ambito più ampio consisteva nell'usare un oggetto TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="dd30b-170">Prior to EF6 the recommended way of providing larger scope transactions was to use a TransactionScope object:</span></span>  

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

<span data-ttu-id="dd30b-171">SqlConnection e Entity Framework utilizzeranno entrambe la transazione TransactionScope di ambiente e pertanto verranno sottoposte a commit insieme.</span><span class="sxs-lookup"><span data-stu-id="dd30b-171">The SqlConnection and Entity Framework would both use the ambient TransactionScope transaction and hence be committed together.</span></span>  

<span data-ttu-id="dd30b-172">A partire da .NET 4.5.1 TransactionScope è stato aggiornato in modo da funzionare anche con i metodi asincroni tramite l'uso dell'enumerazione [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) :</span><span class="sxs-lookup"><span data-stu-id="dd30b-172">Starting with .NET 4.5.1 TransactionScope has been updated to also work with asynchronous methods via the use of the [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeration:</span></span>  

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

<span data-ttu-id="dd30b-173">Esistono tuttavia alcune limitazioni all'approccio TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="dd30b-173">There are still some limitations to the TransactionScope approach:</span></span>  

- <span data-ttu-id="dd30b-174">Per usare i metodi asincroni, è necessario .NET 4.5.1 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="dd30b-174">Requires .NET 4.5.1 or greater to work with asynchronous methods.</span></span>  
- <span data-ttu-id="dd30b-175">Non può essere usato in scenari cloud, a meno che non si sia certi di avere una sola connessione (gli scenari cloud non supportano le transazioni distribuite).</span><span class="sxs-lookup"><span data-stu-id="dd30b-175">It cannot be used in cloud scenarios unless you are sure you have one and only one connection (cloud scenarios do not support distributed transactions).</span></span>  
- <span data-ttu-id="dd30b-176">Non può essere combinato con l'approccio database. UseTransaction () delle sezioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="dd30b-176">It cannot be combined with the Database.UseTransaction() approach of the previous sections.</span></span>  
- <span data-ttu-id="dd30b-177">Le eccezioni vengono generate se si inviano istruzioni DDL e non sono state abilitate transazioni distribuite tramite il servizio MSDTC.</span><span class="sxs-lookup"><span data-stu-id="dd30b-177">It will throw exceptions if you issue any DDL and have not enabled distributed transactions through the MSDTC Service.</span></span>  

<span data-ttu-id="dd30b-178">Vantaggi dell'approccio TransactionScope:</span><span class="sxs-lookup"><span data-stu-id="dd30b-178">Advantages of the TransactionScope approach:</span></span>  

- <span data-ttu-id="dd30b-179">Verrà automaticamente aggiornata una transazione locale a una transazione distribuita se si esegue più di una connessione a un database specifico o si combina una connessione a un database con una connessione a un database diverso all'interno della stessa transazione (Nota: è necessario avere il servizio MSDTC configurato per consentire il funzionamento delle transazioni distribuite.</span><span class="sxs-lookup"><span data-stu-id="dd30b-179">It will automatically upgrade a local transaction to a distributed transaction if you make more than one connection to a given database or combine a connection to one database with a connection to a different database within the same transaction (note: you must have the MSDTC service configured to allow distributed transactions for this to work).</span></span>  
- <span data-ttu-id="dd30b-180">Semplicità di codifica.</span><span class="sxs-lookup"><span data-stu-id="dd30b-180">Ease of coding.</span></span> <span data-ttu-id="dd30b-181">Se si preferisce che la transazione sia in ambiente e gestita in modo implicito in background, anziché in modo esplicito sotto il controllo, l'approccio TransactionScope può essere più adatto.</span><span class="sxs-lookup"><span data-stu-id="dd30b-181">If you prefer the transaction to be ambient and dealt with implicitly in the background rather than explicitly under you control then the TransactionScope approach may suit you better.</span></span>  

<span data-ttu-id="dd30b-182">In breve, con le nuove API database. BeginTransaction () e database. UseTransaction (), l'approccio TransactionScope non è più necessario per la maggior parte degli utenti.</span><span class="sxs-lookup"><span data-stu-id="dd30b-182">In summary, with the new Database.BeginTransaction() and Database.UseTransaction() APIs above, the TransactionScope approach is no longer necessary for most users.</span></span> <span data-ttu-id="dd30b-183">Se si continua a usare TransactionScope, tenere presente le limitazioni sopra riportate.</span><span class="sxs-lookup"><span data-stu-id="dd30b-183">If you do continue to use TransactionScope then be aware of the above limitations.</span></span> <span data-ttu-id="dd30b-184">È consigliabile usare invece l'approccio descritto nelle sezioni precedenti, laddove possibile.</span><span class="sxs-lookup"><span data-stu-id="dd30b-184">We recommend using the approach outlined in the previous sections instead where possible.</span></span>  
