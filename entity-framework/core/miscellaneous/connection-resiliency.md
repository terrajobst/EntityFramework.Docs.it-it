---
title: Resilienza di connessione - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: aec69577cd4b19fdebedb33ed6fd8f2665b0a032
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2017
---
# <a name="connection-resiliency"></a><span data-ttu-id="1f989-102">Resilienza della connessione</span><span class="sxs-lookup"><span data-stu-id="1f989-102">Connection Resiliency</span></span>

<span data-ttu-id="1f989-103">Resilienza di connessione ritenta automaticamente i comandi di database non riuscita.</span><span class="sxs-lookup"><span data-stu-id="1f989-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="1f989-104">La funzionalità è utilizzabile con qualsiasi database specificando una "strategia di esecuzione", che incapsula la logica necessaria per rilevare errori e ripetere i comandi.</span><span class="sxs-lookup"><span data-stu-id="1f989-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="1f989-105">Il provider di sistema di Entity Framework può fornire le strategie di esecuzione personalizzata in base a criteri di tentativi ottimali e le condizioni di errore di database specifico.</span><span class="sxs-lookup"><span data-stu-id="1f989-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="1f989-106">Ad esempio, il provider SQL Server include una strategia di esecuzione che è adatto a SQL Server (incluso SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="1f989-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="1f989-107">È compatibile con i tipi di eccezione che è possibile ritentare e si dispone di impostazioni predefinite ragionevoli per numero massimo di tentativi, intervallo tra tentativi e così via.</span><span class="sxs-lookup"><span data-stu-id="1f989-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="1f989-108">Una strategia di esecuzione viene specificata quando si configurano le opzioni per il contesto.</span><span class="sxs-lookup"><span data-stu-id="1f989-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="1f989-109">Si tratta in genere nel `OnConfiguring` metodo del contesto derivato o in `Startup.cs` per un'applicazione ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1f989-109">This is typically in the `OnConfiguring` method of your derived context, or in `Startup.cs` for an ASP.NET Core application.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a><span data-ttu-id="1f989-110">Strategia di esecuzione personalizzato</span><span class="sxs-lookup"><span data-stu-id="1f989-110">Custom execution strategy</span></span>

<span data-ttu-id="1f989-111">È un meccanismo per registrare una strategia di esecuzione personalizzata personalizzati se si desidera modificare le impostazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="1f989-111">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="1f989-112">Le transazioni e le strategie di esecuzione</span><span class="sxs-lookup"><span data-stu-id="1f989-112">Execution strategies and transactions</span></span>

<span data-ttu-id="1f989-113">Una strategia di esecuzione che ritenta automaticamente in caso di errore deve essere in grado di riprodurre ogni operazione in un blocco di ripetizione che ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="1f989-113">An execution strategy that automatically retries on failures needs to be able to play back each operation in an retry block that fails.</span></span> <span data-ttu-id="1f989-114">Quando sono abilitati i tentativi, ogni operazione effettuata tramite EF Core diventa il proprio riproducibile, vale a dire ogni query e ogni chiamata a `SaveChanges()` verrà ritentata come unità se si verifica un errore temporaneo.</span><span class="sxs-lookup"><span data-stu-id="1f989-114">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation, i.e. each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="1f989-115">Tuttavia, se il codice avvia una transazione tramite `BeginTransaction()` si sta definendo un proprio gruppo di operazioni che devono essere trattati come un'unità, ad esempio tutti gli elementi all'interno della transazione dovrà essere riprodotti deve verificarsi un errore.</span><span class="sxs-lookup"><span data-stu-id="1f989-115">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, i.e. everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="1f989-116">Se si tenta di eseguire questa operazione quando si utilizza una strategia di esecuzione, si riceverà un'eccezione simile al seguente.</span><span class="sxs-lookup"><span data-stu-id="1f989-116">You will receive an exception like the following if you attempt to do this when using an execution strategy.</span></span>

> <span data-ttu-id="1f989-117">InvalidOperationException: La strategia di esecuzione configurata 'SqlServerRetryingExecutionStrategy' non supporta le transazioni avviate dall'utente.</span><span class="sxs-lookup"><span data-stu-id="1f989-117">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="1f989-118">Utilizzare la strategia di esecuzione restituita da 'DbContext.Database.CreateExecutionStrategy()' per eseguire tutte le operazioni nella transazione come unità possibilità di ritentare.</span><span class="sxs-lookup"><span data-stu-id="1f989-118">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="1f989-119">La soluzione consiste nel richiamare manualmente la strategia di esecuzione con un delegato che rappresenta gli elementi che deve essere eseguito.</span><span class="sxs-lookup"><span data-stu-id="1f989-119">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="1f989-120">Se si verifica un errore temporaneo, la strategia di esecuzione verrà nuovamente richiamare il delegato.</span><span class="sxs-lookup"><span data-stu-id="1f989-120">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="1f989-121">Errore commit della transazione e il problema idempotenza</span><span class="sxs-lookup"><span data-stu-id="1f989-121">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="1f989-122">In generale, quando si verifica un errore di connessione è eseguire il rollback della transazione corrente.</span><span class="sxs-lookup"><span data-stu-id="1f989-122">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="1f989-123">Tuttavia, se la connessione viene eliminata mentre la transazione è stata eseguito il commit risultante lo stato della transazione è sconosciuto.</span><span class="sxs-lookup"><span data-stu-id="1f989-123">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> <span data-ttu-id="1f989-124">Vedere questo [post di blog](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="1f989-124">See this [blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) for more details.</span></span>

<span data-ttu-id="1f989-125">Per impostazione predefinita, la strategia di esecuzione verrà ripetere l'operazione come se è stato eseguito il rollback della transazione, ma se non è il caso questa genererà un'eccezione se il nuovo stato del database non è compatibile o potrebbe causare **il danneggiamento dei dati** se la operazione non si basa su uno stato specifico, ad esempio quando si inserisce una nuova riga con valori di chiave generato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="1f989-125">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="1f989-126">Esistono diversi modi per risolvere questo problema.</span><span class="sxs-lookup"><span data-stu-id="1f989-126">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="1f989-127">Opzione 1: nothing (quasi)</span><span class="sxs-lookup"><span data-stu-id="1f989-127">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="1f989-128">La probabilità di un errore di connessione durante il commit delle transazioni è bassa, pertanto potrebbe essere accettabile per l'applicazione viene eseguita solo se questa condizione si verifica effettivamente.</span><span class="sxs-lookup"><span data-stu-id="1f989-128">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="1f989-129">Tuttavia, è necessario evitare le chiavi generate dall'archivio per garantire che viene generata un'eccezione anziché aggiungere una riga duplicata.</span><span class="sxs-lookup"><span data-stu-id="1f989-129">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="1f989-130">È consigliabile utilizzare un valore GUID generato dal client o un generatore di valore sul lato client.</span><span class="sxs-lookup"><span data-stu-id="1f989-130">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="1f989-131">Opzione 2: lo stato dell'applicazione di ricompilazione</span><span class="sxs-lookup"><span data-stu-id="1f989-131">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="1f989-132">Eliminare l'oggetto corrente `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1f989-132">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="1f989-133">Creare un nuovo `DbContext` e ripristinare lo stato dell'applicazione dal database.</span><span class="sxs-lookup"><span data-stu-id="1f989-133">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="1f989-134">Informare l'utente che l'ultima operazione potrebbe non sia stata completata correttamente.</span><span class="sxs-lookup"><span data-stu-id="1f989-134">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="1f989-135">Opzione 3: aggiungere la verifica dello stato</span><span class="sxs-lookup"><span data-stu-id="1f989-135">Option 3 - Add state verification</span></span>

<span data-ttu-id="1f989-136">Per la maggior parte delle operazioni che modificano lo stato del database è possibile aggiungere codice che verifica se ha avuto esito positivo.</span><span class="sxs-lookup"><span data-stu-id="1f989-136">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="1f989-137">Entity Framework fornisce un metodo di estensione per semplificare questa operazione - `IExecutionStrategy.ExecuteInTransaction`.</span><span class="sxs-lookup"><span data-stu-id="1f989-137">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="1f989-138">Questo metodo inizia e viene eseguito il commit di una transazione e accetta inoltre una funzione di `verifySucceeded` parametro che viene richiamato quando si verifica un errore temporaneo durante il commit della transazione.</span><span class="sxs-lookup"><span data-stu-id="1f989-138">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="1f989-139">Qui `SaveChanges` viene richiamato con `acceptAllChangesOnSuccess` impostato su `false` per evitare di modificare lo stato del `Blog` entità `Unchanged` se `SaveChanges` ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="1f989-139">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="1f989-140">Questo consente di ripetere la stessa operazione, se il commit non riesce e viene eseguito il rollback della transazione.</span><span class="sxs-lookup"><span data-stu-id="1f989-140">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="1f989-141">Opzione 4: registrare manualmente la transazione</span><span class="sxs-lookup"><span data-stu-id="1f989-141">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="1f989-142">Se è necessario utilizzare le chiavi generate dall'archivio o una modalità generica di gestione degli errori di commit che non dipende l'operazione eseguita ogni transazione è possibile assegnare un ID che viene controllato durante il commit ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="1f989-142">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="1f989-143">Aggiungere una tabella per il database utilizzato per tenere traccia dello stato delle transazioni.</span><span class="sxs-lookup"><span data-stu-id="1f989-143">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="1f989-144">Inserire una riga nella tabella all'inizio di ogni transazione.</span><span class="sxs-lookup"><span data-stu-id="1f989-144">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="1f989-145">Se la connessione non riesce durante il commit, verificare la presenza della riga corrispondente nel database.</span><span class="sxs-lookup"><span data-stu-id="1f989-145">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="1f989-146">Se il commit ha esito positivo, è possibile eliminare la riga corrispondente per evitare la crescita della tabella.</span><span class="sxs-lookup"><span data-stu-id="1f989-146">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="1f989-147">Assicurarsi che il contesto utilizzato per la verifica ha una strategia di esecuzione definita come la connessione è probabile che non viene eseguita nuovamente durante la verifica se non è riuscito durante il commit della transazione.</span><span class="sxs-lookup"><span data-stu-id="1f989-147">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
