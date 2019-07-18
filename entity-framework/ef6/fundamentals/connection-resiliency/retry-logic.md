---
title: Resilienza della connessione e logica di ripetizione dei tentativi-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: a01216c3399ca4a04943563435eacd0047337a5f
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306571"
---
# <a name="connection-resiliency-and-retry-logic"></a><span data-ttu-id="a1715-102">Resilienza della connessione e logica di ripetizione dei tentativi</span><span class="sxs-lookup"><span data-stu-id="a1715-102">Connection resiliency and retry logic</span></span>
> [!NOTE]
> <span data-ttu-id="a1715-103">**Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="a1715-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="a1715-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="a1715-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="a1715-105">Le applicazioni che si connettono a un server di database sono sempre state vulnerabili alle interruzioni di connessione a causa di errori di back-end e di instabilità della rete</span><span class="sxs-lookup"><span data-stu-id="a1715-105">Applications connecting to a database server have always been vulnerable to connection breaks due to back-end failures and network instability.</span></span> <span data-ttu-id="a1715-106">Tuttavia, in un ambiente basato su LAN che utilizza server di database dedicati questi errori sono abbastanza rari che la logica aggiuntiva per gestire tali errori non è spesso necessaria.</span><span class="sxs-lookup"><span data-stu-id="a1715-106">However, in a LAN based environment working against dedicated database servers these errors are rare enough that extra logic to handle those failures is not often required.</span></span> <span data-ttu-id="a1715-107">Con l'incremento dei server di database basati su cloud, ad esempio il database SQL di Windows Azure e le connessioni su reti meno affidabili, è ora più comune che si verifichino interruzioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="a1715-107">With the rise of cloud based database servers such as Windows Azure SQL Database and connections over less reliable networks it is now more common for connection breaks to occur.</span></span> <span data-ttu-id="a1715-108">Questo problema può essere dovuto a tecniche difensive che i database cloud utilizzano per garantire la correttezza del servizio, ad esempio la limitazione della connessione, o l'instabilità della rete, causando timeout intermittenti e altri errori temporanei.</span><span class="sxs-lookup"><span data-stu-id="a1715-108">This could be due to defensive techniques that cloud databases use to ensure fairness of service, such as connection throttling, or to instability in the network causing intermittent timeouts and other transient errors.</span></span>  

<span data-ttu-id="a1715-109">La resilienza delle connessioni si riferisce alla possibilità per EF di ritentare automaticamente i comandi che non riescono a causa di questi interruzioni di connessione</span><span class="sxs-lookup"><span data-stu-id="a1715-109">Connection Resiliency refers to the ability for EF to automatically retry any commands that fail due to these connection breaks.</span></span>  

## <a name="execution-strategies"></a><span data-ttu-id="a1715-110">Strategie di esecuzione</span><span class="sxs-lookup"><span data-stu-id="a1715-110">Execution Strategies</span></span>  

<span data-ttu-id="a1715-111">Il tentativo di connessione viene gestito da un'implementazione dell'interfaccia IDbExecutionStrategy.</span><span class="sxs-lookup"><span data-stu-id="a1715-111">Connection retry is taken care of by an implementation of the IDbExecutionStrategy interface.</span></span> <span data-ttu-id="a1715-112">Le implementazioni di IDbExecutionStrategy saranno responsabili dell'accettazione di un'operazione e, se si verifica un'eccezione, che determina se un nuovo tentativo è appropriato e se è in corso un nuovo tentativo.</span><span class="sxs-lookup"><span data-stu-id="a1715-112">Implementations of the IDbExecutionStrategy will be responsible for accepting an operation and, if an exception occurs, determining if a retry is appropriate and retrying if it is.</span></span> <span data-ttu-id="a1715-113">Con EF sono disponibili quattro strategie di esecuzione:</span><span class="sxs-lookup"><span data-stu-id="a1715-113">There are four execution strategies that ship with EF:</span></span>  

1. <span data-ttu-id="a1715-114">**DefaultExecutionStrategy**: questa strategia di esecuzione non tenta di eseguire alcuna operazione. si tratta dell'impostazione predefinita per i database diversi da SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a1715-114">**DefaultExecutionStrategy**: this execution strategy does not retry any operations, it is the default for databases other than sql server.</span></span>  
2. <span data-ttu-id="a1715-115">**DefaultSqlExecutionStrategy**: si tratta di una strategia di esecuzione interna utilizzata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="a1715-115">**DefaultSqlExecutionStrategy**: this is an internal execution strategy that is used by default.</span></span> <span data-ttu-id="a1715-116">Questa strategia non esegue alcun tentativo, tuttavia, eseguirà il wrapping di tutte le eccezioni che potrebbero essere temporanee per informare gli utenti che potrebbero voler abilitare la resilienza della connessione.</span><span class="sxs-lookup"><span data-stu-id="a1715-116">This strategy does not retry at all, however, it will wrap any exceptions that could be transient to inform users that they might want to enable connection resiliency.</span></span>  
3. <span data-ttu-id="a1715-117">**DbExecutionStrategy**: questa classe è adatta come classe base per altre strategie di esecuzione, incluse quelle personalizzate.</span><span class="sxs-lookup"><span data-stu-id="a1715-117">**DbExecutionStrategy**: this class is suitable as a base class for other execution strategies, including your own custom ones.</span></span> <span data-ttu-id="a1715-118">Implementa un criterio di ripetizione esponenziale, dove il tentativo iniziale si verifica con un ritardo zero e il ritardo aumenta in modo esponenziale fino a quando non viene raggiunto il numero massimo di tentativi.</span><span class="sxs-lookup"><span data-stu-id="a1715-118">It implements an exponential retry policy, where the initial retry happens with zero delay and the delay increases exponentially until the maximum retry count is hit.</span></span> <span data-ttu-id="a1715-119">Questa classe dispone di un metodo astratto ShouldRetryOn che può essere implementato in strategie di esecuzione derivate per controllare quali eccezioni devono essere ritentate.</span><span class="sxs-lookup"><span data-stu-id="a1715-119">This class has an abstract ShouldRetryOn method that can be implemented in derived execution strategies to control which exceptions should be retried.</span></span>  
4. <span data-ttu-id="a1715-120">**SqlAzureExecutionStrategy**: questa strategia di esecuzione eredita da DbExecutionStrategy e tenterà di ritentare le eccezioni che potrebbero essere transitorie quando si lavora con il database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="a1715-120">**SqlAzureExecutionStrategy**: this execution strategy inherits from DbExecutionStrategy and will retry on exceptions that are known to be possibly transient when working with Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="a1715-121">Le strategie di esecuzione 2 e 4 sono incluse nel provider SQL Server fornito con EF, che si trova nell'assembly EntityFramework. SqlServer e sono progettate per funzionare con SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a1715-121">Execution strategies 2 and 4 are included in the Sql Server provider that ships with EF, which is in the EntityFramework.SqlServer assembly and are designed to work with SQL Server.</span></span>  

## <a name="enabling-an-execution-strategy"></a><span data-ttu-id="a1715-122">Abilitazione di una strategia di esecuzione</span><span class="sxs-lookup"><span data-stu-id="a1715-122">Enabling an Execution Strategy</span></span>  

<span data-ttu-id="a1715-123">Il modo più semplice per indicare a EF di usare una strategia di esecuzione è con il metodo SetExecutionStrategy della classe [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) :</span><span class="sxs-lookup"><span data-stu-id="a1715-123">The easiest way to tell EF to use an execution strategy is with the SetExecutionStrategy method of the [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) class:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

<span data-ttu-id="a1715-124">Questo codice indica a EF di usare SqlAzureExecutionStrategy durante la connessione a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a1715-124">This code tells EF to use the SqlAzureExecutionStrategy when connecting to SQL Server.</span></span>  

## <a name="configuring-the-execution-strategy"></a><span data-ttu-id="a1715-125">Configurazione della strategia di esecuzione</span><span class="sxs-lookup"><span data-stu-id="a1715-125">Configuring the Execution Strategy</span></span>  

<span data-ttu-id="a1715-126">Il costruttore di SqlAzureExecutionStrategy può accettare due parametri, MaxRetryCount e MaxDelay.</span><span class="sxs-lookup"><span data-stu-id="a1715-126">The constructor of SqlAzureExecutionStrategy can accept two parameters, MaxRetryCount and MaxDelay.</span></span> <span data-ttu-id="a1715-127">MaxRetry count è il numero massimo di volte in cui la strategia tenterà di riprovare.</span><span class="sxs-lookup"><span data-stu-id="a1715-127">MaxRetry count is the maximum number of times that the strategy will retry.</span></span> <span data-ttu-id="a1715-128">MaxDelay è un intervallo di tempo che rappresenta il ritardo massimo tra i tentativi che la strategia di esecuzione utilizzerà.</span><span class="sxs-lookup"><span data-stu-id="a1715-128">The MaxDelay is a TimeSpan representing the maximum delay between retries that the execution strategy will use.</span></span>  

<span data-ttu-id="a1715-129">Per impostare il numero massimo di tentativi su 1 e il ritardo massimo su 30 secondi, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="a1715-129">To set the maximum number of retries to 1 and the maximum delay to 30 seconds you would execute the following:</span></span>  

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

<span data-ttu-id="a1715-130">Il SqlAzureExecutionStrategy tenterà immediatamente la prima volta che si verifica un errore temporaneo, ma ritarderà tra un tentativo e l'altro fino a quando non viene superato il limite massimo di tentativi o il tempo totale raggiunge il ritardo massimo.</span><span class="sxs-lookup"><span data-stu-id="a1715-130">The SqlAzureExecutionStrategy will retry instantly the first time a transient failure occurs, but will delay longer between each retry until either the max retry limit is exceeded or the total time hits the max delay.</span></span>  

<span data-ttu-id="a1715-131">Le strategie di esecuzione ripeteranno solo un numero limitato di eccezioni generalmente temporanee, sarà comunque necessario gestire altri errori, oltre a intercettare l'eccezione RetryLimitExceeded per il caso in cui un errore non è temporaneo o impiega troppo tempo per la risoluzione stesso.</span><span class="sxs-lookup"><span data-stu-id="a1715-131">The execution strategies will only retry a limited number of exceptions that are usually transient, you will still need to handle other errors as well as catching the RetryLimitExceeded exception for the case where an error is not transient or takes too long to resolve itself.</span></span>  

<span data-ttu-id="a1715-132">Esistono alcune limitazioni quando si usa una strategia di esecuzione di ripetizione dei tentativi:</span><span class="sxs-lookup"><span data-stu-id="a1715-132">There are some known of limitations when using a retrying execution strategy:</span></span>  

## <a name="streaming-queries-are-not-supported"></a><span data-ttu-id="a1715-133">Le query di streaming non sono supportate</span><span class="sxs-lookup"><span data-stu-id="a1715-133">Streaming queries are not supported</span></span>  

<span data-ttu-id="a1715-134">Per impostazione predefinita, EF6 e versioni successive registreranno i risultati della query anziché eseguirne il flusso.</span><span class="sxs-lookup"><span data-stu-id="a1715-134">By default, EF6 and later version will buffer query results rather than streaming them.</span></span> <span data-ttu-id="a1715-135">Se si desidera che i risultati vengano trasmessi, è possibile utilizzare il metodo AsStreaming per modificare una query LINQ to Entities per la trasmissione.</span><span class="sxs-lookup"><span data-stu-id="a1715-135">If you want to have results streamed you can use the AsStreaming method to change a LINQ to Entities query to streaming.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

<span data-ttu-id="a1715-136">Il flusso non è supportato quando viene registrata una strategia di esecuzione ritentata.</span><span class="sxs-lookup"><span data-stu-id="a1715-136">Streaming is not supported when a retrying execution strategy is registered.</span></span> <span data-ttu-id="a1715-137">Questa limitazione è dovuta al fatto che la connessione può essere trasformata in modo parziale attraverso i risultati restituiti.</span><span class="sxs-lookup"><span data-stu-id="a1715-137">This limitation exists because the connection could drop part way through the results being returned.</span></span> <span data-ttu-id="a1715-138">Quando si verifica questa situazione, EF deve eseguire di nuovo l'intera query, ma non dispone di un modo affidabile per sapere quali risultati sono già stati restituiti (i dati potrebbero essere stati modificati dopo l'invio della query iniziale, i risultati potrebbero essere restituiti in un ordine diverso, i risultati potrebbero non avere un identificatore univoco e così via).</span><span class="sxs-lookup"><span data-stu-id="a1715-138">When this occurs, EF needs to re-run the entire query but has no reliable way of knowing which results have already been returned (data may have changed since the initial query was sent, results may come back in a different order, results may not have a unique identifier, etc.).</span></span>  

## <a name="user-initiated-transactions-are-not-supported"></a><span data-ttu-id="a1715-139">Le transazioni avviate dall'utente non sono supportate</span><span class="sxs-lookup"><span data-stu-id="a1715-139">User initiated transactions are not supported</span></span>  

<span data-ttu-id="a1715-140">Una volta configurata una strategia di esecuzione che comporta nuovi tentativi, esistono alcune limitazioni relative all'utilizzo delle transazioni.</span><span class="sxs-lookup"><span data-stu-id="a1715-140">When you have configured an execution strategy that results in retries, there are some limitations around the use of transactions.</span></span>  

<span data-ttu-id="a1715-141">Per impostazione predefinita, EF eseguirà tutti gli aggiornamenti del database all'interno di una transazione.</span><span class="sxs-lookup"><span data-stu-id="a1715-141">By default, EF will perform any database updates within a transaction.</span></span> <span data-ttu-id="a1715-142">Non è necessario eseguire alcuna operazione per abilitare questa operazione, EF esegue sempre questa operazione automaticamente.</span><span class="sxs-lookup"><span data-stu-id="a1715-142">You don’t need to do anything to enable this, EF always does this automatically.</span></span>  

<span data-ttu-id="a1715-143">Ad esempio, nel codice SaveChanges seguente viene eseguito automaticamente all'interno di una transazione.</span><span class="sxs-lookup"><span data-stu-id="a1715-143">For example, in the following code SaveChanges is automatically performed within a transaction.</span></span> <span data-ttu-id="a1715-144">Se il metodo SaveChanges ha esito negativo dopo l'inserimento di uno dei nuovi siti, viene eseguito il rollback della transazione e non viene applicata alcuna modifica al database.</span><span class="sxs-lookup"><span data-stu-id="a1715-144">If SaveChanges were to fail after inserting one of the new Site’s then the transaction would be rolled back and no changes applied to the database.</span></span> <span data-ttu-id="a1715-145">Il contesto viene inoltre lasciato in uno stato che consente di richiamare SaveChanges per ritentare l'applicazione delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="a1715-145">The context is also left in a state that allows SaveChanges to be called again to retry applying the changes.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

<span data-ttu-id="a1715-146">Quando non si utilizza una strategia di esecuzione di ripetizione di tentativi, è possibile eseguire il wrapping di più operazioni in una singola transazione.</span><span class="sxs-lookup"><span data-stu-id="a1715-146">When not using a retrying execution strategy you can wrap multiple operations in a single transaction.</span></span> <span data-ttu-id="a1715-147">Il codice seguente, ad esempio, esegue il wrapping di due chiamate SaveChanges in un'unica transazione.</span><span class="sxs-lookup"><span data-stu-id="a1715-147">For example, the following code wraps two SaveChanges calls in a single transaction.</span></span> <span data-ttu-id="a1715-148">Se una parte di entrambe le operazioni ha esito negativo, non viene applicata nessuna delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="a1715-148">If any part of either operation fails then none of the changes are applied.</span></span>  

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

<span data-ttu-id="a1715-149">Questa operazione non è supportata quando si usa una strategia di esecuzione di ripetizione dei tentativi perché EF non è a conoscenza di alcuna operazione precedente e come riprovare.</span><span class="sxs-lookup"><span data-stu-id="a1715-149">This is not supported when using a retrying execution strategy because EF isn’t aware of any previous operations and how to retry them.</span></span> <span data-ttu-id="a1715-150">Se ad esempio il secondo SaveChanges ha avuto esito negativo, EF non dispone più delle informazioni necessarie per ripetere la prima chiamata a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="a1715-150">For example, if the second SaveChanges failed then EF no longer has the required information to retry the first SaveChanges call.</span></span>  

### <a name="workaround-suspend-execution-strategy"></a><span data-ttu-id="a1715-151">Soluzione alternativa: Sospendere la strategia di esecuzione</span><span class="sxs-lookup"><span data-stu-id="a1715-151">Workaround: Suspend Execution Strategy</span></span>  

<span data-ttu-id="a1715-152">Una possibile soluzione alternativa consiste nel sospendere la strategia di esecuzione del nuovo tentativo per il frammento di codice che deve usare una transazione avviata dall'utente.</span><span class="sxs-lookup"><span data-stu-id="a1715-152">One possible workaround is to suspend the retrying execution strategy for the piece of code that needs to use a user initiated transaction.</span></span> <span data-ttu-id="a1715-153">Il modo più semplice per eseguire questa operazione consiste nell'aggiungere un flag SuspendExecutionStrategy alla classe di configurazione basata su codice e modificare l'espressione lambda della strategia di esecuzione per restituire la strategia di esecuzione predefinita (non di rivendita) quando il flag è impostato.</span><span class="sxs-lookup"><span data-stu-id="a1715-153">The easiest way to do this is to add a SuspendExecutionStrategy flag to your code based configuration class and change the execution strategy lambda to return the default (non-retying) execution strategy when the flag is set.</span></span>  

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
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy") ?? false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

<span data-ttu-id="a1715-154">Si noti che si sta usando CallContext per archiviare il valore del flag.</span><span class="sxs-lookup"><span data-stu-id="a1715-154">Note that we are using CallContext to store the flag value.</span></span> <span data-ttu-id="a1715-155">Questo fornisce funzionalità simili all'archiviazione locale di thread, ma è sicuro da usare con codice asincrono, inclusa la query asincrona e Salva con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a1715-155">This provides similar functionality to thread local storage but is safe to use with asynchronous code - including async query and save with Entity Framework.</span></span>  

<span data-ttu-id="a1715-156">È ora possibile sospendere la strategia di esecuzione per la sezione di codice che usa una transazione avviata dall'utente.</span><span class="sxs-lookup"><span data-stu-id="a1715-156">We can now suspend the execution strategy for the section of code that uses a user initiated transaction.</span></span>  

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

### <a name="workaround-manually-call-execution-strategy"></a><span data-ttu-id="a1715-157">Soluzione alternativa: Chiamare manualmente la strategia di esecuzione</span><span class="sxs-lookup"><span data-stu-id="a1715-157">Workaround: Manually Call Execution Strategy</span></span>  

<span data-ttu-id="a1715-158">Un'altra opzione consiste nel usare manualmente la strategia di esecuzione e assegnarle l'intero set di logica da eseguire, in modo da poter ritentare tutti i tentativi se una delle operazioni ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="a1715-158">Another option is to manually use the execution strategy and give it the entire set of logic to be run, so that it can retry everything if one of the operations fails.</span></span> <span data-ttu-id="a1715-159">È ancora necessario sospendere la strategia di esecuzione, usando la tecnica illustrata in precedenza, in modo che tutti i contesti usati all'interno del blocco di codice irreversibile non tentino di riprovare.</span><span class="sxs-lookup"><span data-stu-id="a1715-159">We still need to suspend the execution strategy - using the technique shown above - so that any contexts used inside the retryable code block do not attempt to retry.</span></span>  

<span data-ttu-id="a1715-160">Si noti che tutti i contesti devono essere costruiti all'interno del blocco di codice per poter essere ripetuti.</span><span class="sxs-lookup"><span data-stu-id="a1715-160">Note that any contexts should be constructed within the code block to be retried.</span></span> <span data-ttu-id="a1715-161">In questo modo si inizia con uno stato pulito per ogni nuovo tentativo.</span><span class="sxs-lookup"><span data-stu-id="a1715-161">This ensures that we are starting with a clean state for each retry.</span></span>  

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
