---
title: Resilienza della connessione - EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: 6d8cf117dfd94524a53e10bb4a23c2a44c4c8e7b
ms.sourcegitcommit: 33b2e84dae96040f60a613186a24ff3c7b00b6db
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/21/2019
ms.locfileid: "56459172"
---
# <a name="connection-resiliency"></a><span data-ttu-id="4af1b-102">Resilienza della connessione</span><span class="sxs-lookup"><span data-stu-id="4af1b-102">Connection Resiliency</span></span>

<span data-ttu-id="4af1b-103">Resilienza della connessione ritenta automaticamente i comandi di database non riuscita.</span><span class="sxs-lookup"><span data-stu-id="4af1b-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="4af1b-104">La funzionalità è utilizzabile con qualsiasi database specificando una "strategia di esecuzione", che incapsula la logica necessaria per rilevare gli errori e ripetere i comandi.</span><span class="sxs-lookup"><span data-stu-id="4af1b-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="4af1b-105">Provider di EF Core può fornire le strategie di esecuzione personalizzate in base a condizioni di errore specifico del database e i criteri di ripetizione dei tentativi ottimale.</span><span class="sxs-lookup"><span data-stu-id="4af1b-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="4af1b-106">Ad esempio, il provider SQL Server include una strategia di esecuzione che viene personalizzata in modo specifico per SQL Server (incluso SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="4af1b-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="4af1b-107">Si è a conoscenza dei tipi di eccezioni che possono essere ripetuti e offre impostazioni predefinite ragionevoli per numero massimo di tentativi, intervallo tra tentativi e così via.</span><span class="sxs-lookup"><span data-stu-id="4af1b-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="4af1b-108">Una strategia di esecuzione viene specificata quando si configurano le opzioni per il contesto.</span><span class="sxs-lookup"><span data-stu-id="4af1b-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="4af1b-109">Si tratta in genere nel `OnConfiguring` metodo del contesto derivato:</span><span class="sxs-lookup"><span data-stu-id="4af1b-109">This is typically in the `OnConfiguring` method of your derived context:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

<span data-ttu-id="4af1b-110">o in `Startup.cs` per un'applicazione ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="4af1b-110">or in `Startup.cs` for an ASP.NET Core application:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<PicnicContext>(
        options => options.UseSqlServer(
            "<connection string>",
            providerOptions => providerOptions.EnableRetryOnFailure()));
}
```

## <a name="custom-execution-strategy"></a><span data-ttu-id="4af1b-111">Strategia di esecuzione personalizzata</span><span class="sxs-lookup"><span data-stu-id="4af1b-111">Custom execution strategy</span></span>

<span data-ttu-id="4af1b-112">C'è un meccanismo per registrare una strategia di esecuzione personalizzata personalizzato se si desidera modificare le impostazioni predefinite.</span><span class="sxs-lookup"><span data-stu-id="4af1b-112">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="4af1b-113">Le transazioni e strategie di esecuzione</span><span class="sxs-lookup"><span data-stu-id="4af1b-113">Execution strategies and transactions</span></span>

<span data-ttu-id="4af1b-114">Una strategia di esecuzione che esegue automaticamente nuovi tentativi in caso di errori deve essere in grado di riprodurre ogni operazione in un blocco di ripetizione dei tentativi che ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="4af1b-114">An execution strategy that automatically retries on failures needs to be able to play back each operation in a retry block that fails.</span></span> <span data-ttu-id="4af1b-115">Quando sono abilitati i tentativi, ogni operazione che viene eseguita tramite EF Core diventa la propria operazione con possibilità di ritentare.</span><span class="sxs-lookup"><span data-stu-id="4af1b-115">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation.</span></span> <span data-ttu-id="4af1b-116">Vale a dire, ogni query e ogni chiamata a `SaveChanges()` verrà ritentata come unità se si verifica un errore temporaneo.</span><span class="sxs-lookup"><span data-stu-id="4af1b-116">That is, each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="4af1b-117">Tuttavia, se il codice avvia una transazione tramite `BeginTransaction()` si sta definendo il proprio gruppo di operazioni che devono essere considerati come un'unità e tutti gli elementi all'interno della transazione dovrà essere riprodotti deve verificarsi un errore.</span><span class="sxs-lookup"><span data-stu-id="4af1b-117">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, and everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="4af1b-118">Se si prova a eseguire questa operazione quando si usa una strategia di esecuzione, si riceverà un'eccezione simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="4af1b-118">You will receive an exception like the following if you attempt to do this when using an execution strategy:</span></span>

> <span data-ttu-id="4af1b-119">InvalidOperationException: La strategia di esecuzione configurata 'SqlServerRetryingExecutionStrategy' non supporta le transazioni avviate dall'utente.</span><span class="sxs-lookup"><span data-stu-id="4af1b-119">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="4af1b-120">Usare la strategia di esecuzione restituita da 'DbContext.Database.CreateExecutionStrategy()' per eseguire tutte le operazioni nella transazione come un'unità con possibilità di ritentare.</span><span class="sxs-lookup"><span data-stu-id="4af1b-120">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="4af1b-121">La soluzione consiste nel richiamare manualmente la strategia di esecuzione con un delegato che rappresenta tutti gli elementi che deve essere eseguito.</span><span class="sxs-lookup"><span data-stu-id="4af1b-121">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="4af1b-122">Se si verifica un errore temporaneo, la strategia di esecuzione richiamerà nuovamente il delegato.</span><span class="sxs-lookup"><span data-stu-id="4af1b-122">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

<span data-ttu-id="4af1b-123">Questo approccio può essere utilizzato anche con transazioni di ambiente.</span><span class="sxs-lookup"><span data-stu-id="4af1b-123">This approach can also be used with ambient transactions.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#AmbientTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="4af1b-124">Errore commit della transazione e il problema di idempotenza</span><span class="sxs-lookup"><span data-stu-id="4af1b-124">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="4af1b-125">In generale, quando si verifica un errore di connessione corrente è rollback della transazione.</span><span class="sxs-lookup"><span data-stu-id="4af1b-125">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="4af1b-126">Tuttavia, se la connessione viene eliminata mentre la transazione è in corso il commit risultante lo stato della transazione è sconosciuto.</span><span class="sxs-lookup"><span data-stu-id="4af1b-126">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> <span data-ttu-id="4af1b-127">Vedere questo [post di blog](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="4af1b-127">See this [blog post](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) for more details.</span></span>

<span data-ttu-id="4af1b-128">Per impostazione predefinita, la strategia di esecuzione ritenterà l'operazione come se è stato eseguito il rollback della transazione, ma se non è il caso ciò comporterà un'eccezione se il nuovo stato del database non è compatibile o potrebbe causare **danneggiamento dei dati** se la operazione non si basa su un determinato stato, ad esempio quando si inserisce una nuova riga con valori di chiave generati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="4af1b-128">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="4af1b-129">Esistono diversi modi per risolvere questo problema.</span><span class="sxs-lookup"><span data-stu-id="4af1b-129">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="4af1b-130">Opzione 1: nothing (quasi)</span><span class="sxs-lookup"><span data-stu-id="4af1b-130">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="4af1b-131">La probabilità di un errore di connessione durante il commit della transazione è bassa, pertanto potrebbe essere accettabile per l'applicazione viene eseguita solo se questa condizione si verifica effettivamente.</span><span class="sxs-lookup"><span data-stu-id="4af1b-131">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="4af1b-132">Tuttavia, è necessario evitare di usare le chiavi generate dall'archivio per garantire che invece di aggiungere una riga duplicata viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="4af1b-132">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="4af1b-133">È consigliabile usare un valore GUID generato dal client o un generatore di valore lato client.</span><span class="sxs-lookup"><span data-stu-id="4af1b-133">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="4af1b-134">Opzione 2: lo stato dell'applicazione di ricompilazione</span><span class="sxs-lookup"><span data-stu-id="4af1b-134">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="4af1b-135">Rimuovere l'oggetto corrente `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="4af1b-135">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="4af1b-136">Creare un nuovo `DbContext` e ripristinare lo stato dell'applicazione dal database.</span><span class="sxs-lookup"><span data-stu-id="4af1b-136">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="4af1b-137">Informare l'utente che l'ultima operazione potrebbe non siano stata completata correttamente.</span><span class="sxs-lookup"><span data-stu-id="4af1b-137">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="4af1b-138">Opzione 3: aggiungere la verifica dello stato</span><span class="sxs-lookup"><span data-stu-id="4af1b-138">Option 3 - Add state verification</span></span>

<span data-ttu-id="4af1b-139">Per la maggior parte delle operazioni che modificano lo stato del database è possibile aggiungere il codice che verifica se ha avuto esito positivo.</span><span class="sxs-lookup"><span data-stu-id="4af1b-139">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="4af1b-140">Entity Framework fornisce un metodo di estensione per semplificare questa operazione - `IExecutionStrategy.ExecuteInTransaction`.</span><span class="sxs-lookup"><span data-stu-id="4af1b-140">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="4af1b-141">Questo metodo avvia ed esegue il commit di una transazione e accetta anche una funzione nel `verifySucceeded` parametro che viene richiamato quando si verifica un errore temporaneo durante il commit della transazione.</span><span class="sxs-lookup"><span data-stu-id="4af1b-141">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="4af1b-142">Qui `SaveChanges` viene richiamato con `acceptAllChangesOnSuccess` impostata su `false` per evitare la modifica dello stato del `Blog` entità `Unchanged` se `SaveChanges` ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="4af1b-142">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="4af1b-143">Ciò consente di ripetere l'operazione stessa se il commit ha esito negativo e viene eseguito il rollback della transazione.</span><span class="sxs-lookup"><span data-stu-id="4af1b-143">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="4af1b-144">Opzione 4: registrare manualmente la transazione</span><span class="sxs-lookup"><span data-stu-id="4af1b-144">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="4af1b-145">Se è necessario usare le chiavi generate dall'archivio o necessitano di un modo generico di gestione degli errori di commit che non variano a seconda dell'operazione eseguita ogni transazione è stato possibile assegnare un ID che viene controllato durante il commit ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="4af1b-145">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="4af1b-146">Aggiungere una tabella nel database usato per tenere traccia dello stato delle transazioni.</span><span class="sxs-lookup"><span data-stu-id="4af1b-146">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="4af1b-147">Inserire una riga nella tabella all'inizio di ogni transazione.</span><span class="sxs-lookup"><span data-stu-id="4af1b-147">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="4af1b-148">Se la connessione non riesce durante il commit, verificare la presenza della riga corrispondente nel database.</span><span class="sxs-lookup"><span data-stu-id="4af1b-148">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="4af1b-149">Se il commit ha esito positivo, eliminare la riga corrispondente per evitare la crescita della tabella.</span><span class="sxs-lookup"><span data-stu-id="4af1b-149">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="4af1b-150">Assicurarsi che il contesto utilizzato per la verifica abbia una strategia di esecuzione definita come la connessione è probabile che non viene eseguita nuovamente durante la verifica se non è riuscito durante il commit della transazione.</span><span class="sxs-lookup"><span data-stu-id="4af1b-150">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
