---
title: Connessione tentativi e la resilienza per la logica - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
caps.latest.revision: 3
ms.openlocfilehash: 4c6e296ecb8b43468408d359f44fd1d1a6edf864
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121340"
---
# <a name="connection-resiliency-and-retry-logic"></a><span data-ttu-id="65a65-102">Logica di tentativi e la resilienza di connessione</span><span class="sxs-lookup"><span data-stu-id="65a65-102">Connection resiliency and retry logic</span></span>
> [!NOTE]
> <span data-ttu-id="65a65-103">**Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="65a65-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="65a65-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="65a65-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="65a65-105">Applicazioni che si connettono a un server di database sono sempre state vulnerabili alle interruzioni di connessione a causa di errori di back-end e dall'instabilità della rete.</span><span class="sxs-lookup"><span data-stu-id="65a65-105">Applications connecting to a database server have always been vulnerable to connection breaks due to back-end failures and network instability.</span></span> <span data-ttu-id="65a65-106">Tuttavia, in un ambiente basato su LAN funziona con i server di database dedicato questi errori sono abbastanza rari che per la logica aggiuntiva per gestire tali errori non è spesso necessaria.</span><span class="sxs-lookup"><span data-stu-id="65a65-106">However, in a LAN based environment working against dedicated database servers these errors are rare enough that extra logic to handle those failures is not often required.</span></span> <span data-ttu-id="65a65-107">Con l'avvento del cloud basati su server di database, ad esempio Database SQL di Azure e le connessioni su reti meno affidabile che è ora più comune per le interruzioni di connessione si verifichi.</span><span class="sxs-lookup"><span data-stu-id="65a65-107">With the rise of cloud based database servers such as Windows Azure SQL Database and connections over less reliable networks it is now more common for connection breaks to occur.</span></span> <span data-ttu-id="65a65-108">Probabilmente a causa di tecniche difensive che Usa database per garantire l'equità del servizio, ad esempio la limitazione delle richieste di connessione, o di instabilità della rete causando i timeout intermittenti e altri errori temporanei di cloud.</span><span class="sxs-lookup"><span data-stu-id="65a65-108">This could be due to defensive techniques that cloud databases use to ensure fairness of service, such as connection throttling, or to instability in the network causing intermittent timeouts and other transient errors.</span></span>  

<span data-ttu-id="65a65-109">Resilienza della connessione si intende la possibilità di ritentare automaticamente i comandi che non riescono a causa di queste interruzioni di connessione a Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="65a65-109">Connection Resiliency refers to the ability for EF to automatically retry any commands that fail due to these connection breaks.</span></span>  

## <a name="execution-strategies"></a><span data-ttu-id="65a65-110">Strategie di esecuzione</span><span class="sxs-lookup"><span data-stu-id="65a65-110">Execution Strategies</span></span>  

<span data-ttu-id="65a65-111">Nuovi tentativi di connessione viene preso in considerazione da un'implementazione dell'interfaccia IDbExecutionStrategy.</span><span class="sxs-lookup"><span data-stu-id="65a65-111">Connection retry is taken care of by an implementation of the IDbExecutionStrategy interface.</span></span> <span data-ttu-id="65a65-112">Le implementazioni del IDbExecutionStrategy saranno ritenuto responsabile per l'accettazione di un'operazione e, se si verifica un'eccezione, che determina se un nuovo tentativo è appropriato e nuovo tentativo in caso.</span><span class="sxs-lookup"><span data-stu-id="65a65-112">Implementations of the IDbExecutionStrategy will be responsible for accepting an operation and, if an exception occurs, determining if a retry is appropriate and retrying if it is.</span></span> <span data-ttu-id="65a65-113">Esistono quattro strategie di esecuzione forniti con Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="65a65-113">There are four execution strategies that ship with EF:</span></span>  

1. <span data-ttu-id="65a65-114">**DefaultExecutionStrategy**: questa strategia di esecuzione non ripeterà tutte le operazioni, è il valore predefinito per i database diversi da sql server.</span><span class="sxs-lookup"><span data-stu-id="65a65-114">**DefaultExecutionStrategy**: this execution strategy does not retry any operations, it is the default for databases other than sql server.</span></span>  
2. <span data-ttu-id="65a65-115">**DefaultSqlExecutionStrategy**: si tratta di una strategia di esecuzione interna che viene usata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="65a65-115">**DefaultSqlExecutionStrategy**: this is an internal execution strategy that is used by default.</span></span> <span data-ttu-id="65a65-116">Questa strategia non tenterà affatto, tuttavia, eseguirà il wrapping di tutte le eccezioni che potrebbero essere temporanee per informare gli utenti che desiderano abilitare la resilienza di connessione.</span><span class="sxs-lookup"><span data-stu-id="65a65-116">This strategy does not retry at all, however, it will wrap any exceptions that could be transient to inform users that they might want to enable connection resiliency.</span></span>  
3. <span data-ttu-id="65a65-117">**DbExecutionStrategy**: questa classe può fungere da una classe di base per altre strategie di esecuzione, tra cui i proprio quelle personalizzate.</span><span class="sxs-lookup"><span data-stu-id="65a65-117">**DbExecutionStrategy**: this class is suitable as a base class for other execution strategies, including your own custom ones.</span></span> <span data-ttu-id="65a65-118">Implementa un criterio di ripetizione esponenziale, in cui il numero di tentativi iniziale viene eseguita con zero ritardo e ritardo aumenta in misura esponenziale fino a quando non viene raggiunto il numero massimo di tentativi.</span><span class="sxs-lookup"><span data-stu-id="65a65-118">It implements an exponential retry policy, where the initial retry happens with zero delay and the delay increases exponentially until the maximum retry count is hit.</span></span> <span data-ttu-id="65a65-119">Questa classe ha un metodo ShouldRetryOn astratto che può essere implementato nelle strategie di esecuzione derivata per controllare le eccezioni che devono essere riprovate.</span><span class="sxs-lookup"><span data-stu-id="65a65-119">This class has an abstract ShouldRetryOn method that can be implemented in derived execution strategies to control which exceptions should be retried.</span></span>  
4. <span data-ttu-id="65a65-120">**SqlAzureExecutionStrategy**: questa strategia di esecuzione eredita da DbExecutionStrategy e ritenterà in caso di eccezioni noti per essere eventualmente temporaneo quando si lavora con Database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="65a65-120">**SqlAzureExecutionStrategy**: this execution strategy inherits from DbExecutionStrategy and will retry on exceptions that are known to be possibly transient when working with Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="65a65-121">Strategie di esecuzione 2 e 4 sono inclusi nel provider di Sql Server fornito con Entity Framework, ovvero nell'assembly EntityFramework. SqlServer e sono progettate per funzionare con SQL Server.</span><span class="sxs-lookup"><span data-stu-id="65a65-121">Execution strategies 2 and 4 are included in the Sql Server provider that ships with EF, which is in the EntityFramework.SqlServer assembly and are designed to work with SQL Server.</span></span>  

## <a name="enabling-an-execution-strategy"></a><span data-ttu-id="65a65-122">Abilitazione di una strategia di esecuzione</span><span class="sxs-lookup"><span data-stu-id="65a65-122">Enabling an Execution Strategy</span></span>  

<span data-ttu-id="65a65-123">È il modo più semplice per indicare a Entity Framework da usare una strategia di esecuzione con il metodo SetExecutionStrategy il [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) classe:</span><span class="sxs-lookup"><span data-stu-id="65a65-123">The easiest way to tell EF to use an execution strategy is with the SetExecutionStrategy method of the [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) class:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

<span data-ttu-id="65a65-124">Questo codice indica a Entity Framework da usare il SqlAzureExecutionStrategy quando ci si connette a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="65a65-124">This code tells EF to use the SqlAzureExecutionStrategy when connecting to SQL Server.</span></span>  

## <a name="configuring-the-execution-strategy"></a><span data-ttu-id="65a65-125">Configurare la strategia di esecuzione</span><span class="sxs-lookup"><span data-stu-id="65a65-125">Configuring the Execution Strategy</span></span>  

<span data-ttu-id="65a65-126">Il costruttore di SqlAzureExecutionStrategy può accettare due parametri, MaxRetryCount e MaxDelay.</span><span class="sxs-lookup"><span data-stu-id="65a65-126">The constructor of SqlAzureExecutionStrategy can accept two parameters, MaxRetryCount and MaxDelay.</span></span> <span data-ttu-id="65a65-127">Conteggio MaxRetry è il numero massimo di volte in cui la strategia eseguirà un nuovo tentativo.</span><span class="sxs-lookup"><span data-stu-id="65a65-127">MaxRetry count is the maximum number of times that the strategy will retry.</span></span> <span data-ttu-id="65a65-128">Il MaxDelay è un oggetto TimeSpan che rappresenta il ritardo massimo tra i tentativi che utilizzerà la strategia di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="65a65-128">The MaxDelay is a TimeSpan representing the maximum delay between retries that the execution strategy will use.</span></span>  

<span data-ttu-id="65a65-129">Per impostare il numero massimo di tentativi per il ritardo massimo a 30 secondi e 1 si farebbe execue quanto segue:</span><span class="sxs-lookup"><span data-stu-id="65a65-129">To set the maximum number of retries to 1 and the maximum delay to 30 seconds you would execue the following:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy(
            "System.Data.SqlClient",
            () => new SqlAzureExecutionStrategy(1, TimeSpan.FromSeconds(30)));
    }
}
```  

<span data-ttu-id="65a65-130">Il SqlAzureExecutionStrategy verranno effettuati tentativi immediatamente la prima volta che si verifica un errore temporaneo, ma verrà ritardata più tra i singoli tentativi fino a quando non un il numero massimo di tentativi limite viene superata o il tempo totale raggiunge il ritardo massimo.</span><span class="sxs-lookup"><span data-stu-id="65a65-130">The SqlAzureExecutionStrategy will retry instantly the first time a transient failure occurs, but will delay longer between each retry until either the max retry limit is exceeded or the total time hits the max delay.</span></span>  

<span data-ttu-id="65a65-131">Le strategie di esecuzione riproverà solo un numero limitato di eccezioni che sono in genere tansient, sarà comunque necessario gestire gli altri errori, oltre a rilevare l'eccezione RetryLimitExceeded nel caso in cui un errore non è temporaneo o richiede troppo tempo risolvere se stessa.</span><span class="sxs-lookup"><span data-stu-id="65a65-131">The execution strategies will only retry a limited number of exceptions that are usually tansient, you will still need to handle other errors as well as catching the RetryLimitExceeded exception for the case where an error is not transient or takes too long to resolve itself.</span></span>  

## <a name="limitations"></a><span data-ttu-id="65a65-132">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="65a65-132">Limitations</span></span>  

<span data-ttu-id="65a65-133">Esistono alcune note delle limitazioni quando si usa una strategia di esecuzione di nuovo tentativo:</span><span class="sxs-lookup"><span data-stu-id="65a65-133">There are some known of limitations when using a retrying execution strategy:</span></span>  

### <a name="streaming-queries-are-not-supported"></a><span data-ttu-id="65a65-134">Non sono supportate le query di streaming</span><span class="sxs-lookup"><span data-stu-id="65a65-134">Streaming queries are not supported</span></span>  

<span data-ttu-id="65a65-135">Per impostazione predefinita, Entity Framework 6 e versioni successive nel buffer i risultati della query anziché streaming li.</span><span class="sxs-lookup"><span data-stu-id="65a65-135">By default, EF6 and later version will buffer query results rather than streaming them.</span></span> <span data-ttu-id="65a65-136">Se si desidera avere risultati trasmessi è possibile usare il metodo AsStreaming per modificare un LINQ in query di entità in streaming.</span><span class="sxs-lookup"><span data-stu-id="65a65-136">If you want to have results streamed you can use the AsStreaming method to change a LINQ to Entities query to streaming.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

<span data-ttu-id="65a65-137">Flusso non è supportato quando viene registrata una nuovo tentativo strategia di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="65a65-137">Streaming is not supported when a retrying execution strategy is registered.</span></span> <span data-ttu-id="65a65-138">Questa limitazione esiste perché la connessione è stato possibile eliminare più tramite i risultati restituiti.</span><span class="sxs-lookup"><span data-stu-id="65a65-138">This limitation exists because the connection could drop part way through the results being returned.</span></span> <span data-ttu-id="65a65-139">In questo caso, EF deve eseguire di nuovo l'intera query ma non dispone di alcun modo affidabile di sapere quali risultati già restituiti (dati potrebbero essere cambiato dall'invio della query iniziale, i risultati potrebbero tornare in un ordine diverso, i risultati potrebbero non avere un identificatore univoco e così via.).</span><span class="sxs-lookup"><span data-stu-id="65a65-139">When this occurs, EF needs to re-run the entire query but has no reliable way of knowing which results have already been returned (data may have changed since the initial query was sent, results may come back in a different order, results may not have a unique identifier, etc.).</span></span>  

### <a name="user-initiated-transactions-not-supported"></a><span data-ttu-id="65a65-140">Le transazioni non supportate avviata dall'utente</span><span class="sxs-lookup"><span data-stu-id="65a65-140">User initiated transactions not supported</span></span>  

<span data-ttu-id="65a65-141">Dopo aver configurato una strategia di esecuzione che genera nuovi tentativi, esistono alcune limitazioni per l'utilizzo di transazioni.</span><span class="sxs-lookup"><span data-stu-id="65a65-141">When you have configured an execution strategy that results in retries, there are some limitations around the use of transactions.</span></span>  

#### <a name="whats-supported-efs-default-transaction-behavior"></a><span data-ttu-id="65a65-142">Che cos'è supportato: il comportamento della transazione di predefinito di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="65a65-142">What's Supported: EF's default transaction behavior</span></span>  

<span data-ttu-id="65a65-143">Per impostazione predefinita, Entity Framework eseguirà gli aggiornamenti di database all'interno di una transazione.</span><span class="sxs-lookup"><span data-stu-id="65a65-143">By default, EF will perform any database updates within a transaction.</span></span> <span data-ttu-id="65a65-144">Non è necessario eseguire alcuna operazione per abilitare questa opzione, EF sempre viene eseguito automaticamente.</span><span class="sxs-lookup"><span data-stu-id="65a65-144">You don’t need to do anything to enable this, EF always does this automatically.</span></span>  

<span data-ttu-id="65a65-145">Ad esempio, nel codice seguente SaveChanges viene eseguita automaticamente all'interno di una transazione.</span><span class="sxs-lookup"><span data-stu-id="65a65-145">For example, in the following code SaveChanges is automatically performed within a transaction.</span></span> <span data-ttu-id="65a65-146">Se SaveChanges dovesse avere esito negativo dopo aver inserito tra il nuovo sito, quindi potrebbe essere eseguito il rollback della transazione e nessuna modifica applicata al database.</span><span class="sxs-lookup"><span data-stu-id="65a65-146">If SaveChanges were to fail after inserting one of the new Site’s then the transaction would be rolled back and no changes applied to the database.</span></span> <span data-ttu-id="65a65-147">Il contesto è anche disponibile in uno stato che consente a SaveChanges venga nuovamente chiamata per eseguire l'applicazione delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="65a65-147">The context is also left in a state that allows SaveChanges to be called again to retry applying the changes.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

#### <a name="whats-not-supported-user-initiated-transactions"></a><span data-ttu-id="65a65-148">Che cosa non è supportata: le transazioni avviata dall'utente</span><span class="sxs-lookup"><span data-stu-id="65a65-148">What’s not supported: User initiated transactions</span></span>  

<span data-ttu-id="65a65-149">Se non si usa una strategia di esecuzione di nuovo tentativo è possibile eseguire il wrapping di più operazioni in una singola transazione.</span><span class="sxs-lookup"><span data-stu-id="65a65-149">When not using a retrying execution strategy you can wrap multiple operations in a single transaction.</span></span> <span data-ttu-id="65a65-150">Ad esempio, il codice seguente esegue il wrapping di due chiamate a SaveChanges in un'unica transazione.</span><span class="sxs-lookup"><span data-stu-id="65a65-150">For example, the following code wraps two SaveChanges calls in a single transaction.</span></span> <span data-ttu-id="65a65-151">Se qualsiasi parte di questa operazione ha esito negativo, nessuna delle modifiche vengono applicate.</span><span class="sxs-lookup"><span data-stu-id="65a65-151">If any part of either operation fails then none of the changes are applied.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Site { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }
}
```  

<span data-ttu-id="65a65-152">Ciò non è supportato quando si usa una strategia di esecuzione di nuovo tentativo perché EF non tiene conto di qualsiasi operazione precedente e su come eseguire un nuovo tentativo.</span><span class="sxs-lookup"><span data-stu-id="65a65-152">This is not supported when using a retrying execution strategy because EF isn’t aware of any previous operations and how to retry them.</span></span> <span data-ttu-id="65a65-153">Ad esempio, se il secondo metodo SaveChanges, l'errore EF non è più ha le informazioni necessarie per ripetere la prima chiamata a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="65a65-153">For example, if the second SaveChanges failed then EF no longer has the required information to retry the first SaveChanges call.</span></span>  

#### <a name="possible-workarounds"></a><span data-ttu-id="65a65-154">Possibili soluzioni alternative</span><span class="sxs-lookup"><span data-stu-id="65a65-154">Possible workarounds</span></span>  

##### <a name="suspend-execution-strategy"></a><span data-ttu-id="65a65-155">Sospendere una strategia di esecuzione</span><span class="sxs-lookup"><span data-stu-id="65a65-155">Suspend Execution Strategy</span></span>  

<span data-ttu-id="65a65-156">Una possibile soluzione alternativa consiste nel sospendere la strategia di esecuzione nuovo tentativo per il frammento di codice che deve usare un utente avviate delle transazioni.</span><span class="sxs-lookup"><span data-stu-id="65a65-156">One possible workaround is to suspend the retrying execution strategy for the piece of code that needs to use a user initiated transaction.</span></span> <span data-ttu-id="65a65-157">Il modo più semplice per eseguire questa operazione consiste nell'aggiungere un flag SuspendExecutionStrategy al codice in base a classe di configurazione e modifica l'espressione lambda strategia di esecuzione per restituire la strategia di esecuzione (non-retying) predefinito quando il flag è impostato.</span><span class="sxs-lookup"><span data-stu-id="65a65-157">The easiest way to do this is to add a SuspendExecutionStrategy flag to your code based configuration class and change the execution strategy lambda to return the default (non-retying) execution strategy when the flag is set.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;
using System.Runtime.Remoting.Messaging;

namespace Demo
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            this.SetExecutionStrategy("System.Data.SqlClient", () => SuspendExecutionStrategy
              ? (IDbExecutionStrategy)new DefaultExecutionStrategy()
              : new SqlAzureExecutionStrategy());
        }

        public static bool SuspendExecutionStrategy
        {
            get
            {
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy")  false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

<span data-ttu-id="65a65-158">Si noti che si sta usando CallContext per archiviare il valore del flag.</span><span class="sxs-lookup"><span data-stu-id="65a65-158">Note that we are using CallContext to store the flag value.</span></span> <span data-ttu-id="65a65-159">Questo fornisce funzionalità simili all'archiviazione thread-local, ma è possibile usare con il codice asincrono - tra cui query asincrona e risparmiare con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="65a65-159">This provides similar functionality to thread local storage but is safe to use with asynchronous code - including async query and save with Entity Framework.</span></span>  

<span data-ttu-id="65a65-160">È ora possibile sospendere la strategia di esecuzione per la sezione di codice che utilizza una transazione avviata dall'utente.</span><span class="sxs-lookup"><span data-stu-id="65a65-160">We can now suspend the execution strategy for the section of code that uses a user initiated transaction.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    MyConfiguration.SuspendExecutionStrategy = true;

    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }

    MyConfiguration.SuspendExecutionStrategy = false;
}
```  

##### <a name="manually-call-execution-strategy"></a><span data-ttu-id="65a65-161">Chiamare manualmente strategia di esecuzione</span><span class="sxs-lookup"><span data-stu-id="65a65-161">Manually Call Execution Strategy</span></span>  

<span data-ttu-id="65a65-162">Un'altra possibilità è usare la strategia di esecuzione e assegnargli l'intero set di logica da eseguire, in modo che è possibile riprovare a eseguire tutto ciò che in caso di una delle operazioni manualmente.</span><span class="sxs-lookup"><span data-stu-id="65a65-162">Another option is to manually use the execution strategy and give it the entire set of logic to be run, so that it can retry everything if one of the operations fails.</span></span> <span data-ttu-id="65a65-163">Tuttavia è necessario sospendere la strategia di esecuzione - uso della tecnica illustrato in precedenza, in modo che i contesti utilizzati all'interno del blocco di codice non irreversibile non tentano di ripetere.</span><span class="sxs-lookup"><span data-stu-id="65a65-163">We still need to suspend the execution strategy - using the technique shown above - so that any contexts used inside the retryable code block do not attempt to retry.</span></span>  

<span data-ttu-id="65a65-164">Si noti che i contesti devono essere creati all'interno del blocco di codice per effettuare altri tentativi.</span><span class="sxs-lookup"><span data-stu-id="65a65-164">Note that any contexts should be constructed within the code block to be retried.</span></span> <span data-ttu-id="65a65-165">Ciò garantisce che sta iniziando con uno stato pulito per ogni nuovo tentativo.</span><span class="sxs-lookup"><span data-stu-id="65a65-165">This ensures that we are starting with a clean state for each retry.</span></span>  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

MyConfiguration.SuspendExecutionStrategy = true;

executionStrategy.Execute(
    () =>
    {
        using (var db = new BloggingContext())
        {
            using (var trn = db.Database.BeginTransaction())
            {
                db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                db.SaveChanges();

                db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
                db.SaveChanges();

                trn.Commit();
            }
        }
    });

MyConfiguration.SuspendExecutionStrategy = false;
```  
