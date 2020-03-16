---
title: Resilienza della connessione e logica di ripetizione dei tentativi-EF6
author: AndriySvyryd
ms.date: 11/20/2019
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 50e65bed32d0cfcf42746da0d632f9e990424b97
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/15/2020
ms.locfileid: "79402159"
---
# <a name="connection-resiliency-and-retry-logic"></a><span data-ttu-id="0b28d-102">Resilienza della connessione e logica di ripetizione dei tentativi</span><span class="sxs-lookup"><span data-stu-id="0b28d-102">Connection resiliency and retry logic</span></span>
> [!NOTE]
> <span data-ttu-id="0b28d-103">**Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="0b28d-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="0b28d-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="0b28d-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="0b28d-105">Le applicazioni che si connettono a un server di database sono sempre state vulnerabili alle interruzioni di connessione a causa di errori di back-end e di instabilità della rete</span><span class="sxs-lookup"><span data-stu-id="0b28d-105">Applications connecting to a database server have always been vulnerable to connection breaks due to back-end failures and network instability.</span></span> <span data-ttu-id="0b28d-106">Tuttavia, in un ambiente basato su LAN che utilizza server di database dedicati questi errori sono abbastanza rari che la logica aggiuntiva per gestire tali errori non è spesso necessaria.</span><span class="sxs-lookup"><span data-stu-id="0b28d-106">However, in a LAN based environment working against dedicated database servers these errors are rare enough that extra logic to handle those failures is not often required.</span></span> <span data-ttu-id="0b28d-107">Con l'incremento dei server di database basati su cloud, ad esempio il database SQL di Windows Azure e le connessioni su reti meno affidabili, è ora più comune che si verifichino interruzioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="0b28d-107">With the rise of cloud based database servers such as Windows Azure SQL Database and connections over less reliable networks it is now more common for connection breaks to occur.</span></span> <span data-ttu-id="0b28d-108">Questo problema può essere dovuto a tecniche difensive che i database cloud utilizzano per garantire la correttezza del servizio, ad esempio la limitazione della connessione, o l'instabilità della rete, causando timeout intermittenti e altri errori temporanei.</span><span class="sxs-lookup"><span data-stu-id="0b28d-108">This could be due to defensive techniques that cloud databases use to ensure fairness of service, such as connection throttling, or to instability in the network causing intermittent timeouts and other transient errors.</span></span>  

<span data-ttu-id="0b28d-109">La resilienza delle connessioni si riferisce alla possibilità per EF di ritentare automaticamente i comandi che non riescono a causa di questi interruzioni di connessione</span><span class="sxs-lookup"><span data-stu-id="0b28d-109">Connection Resiliency refers to the ability for EF to automatically retry any commands that fail due to these connection breaks.</span></span>  

## <a name="execution-strategies"></a><span data-ttu-id="0b28d-110">Strategie di esecuzione</span><span class="sxs-lookup"><span data-stu-id="0b28d-110">Execution Strategies</span></span>  

<span data-ttu-id="0b28d-111">Il tentativo di connessione viene gestito da un'implementazione dell'interfaccia IDbExecutionStrategy.</span><span class="sxs-lookup"><span data-stu-id="0b28d-111">Connection retry is taken care of by an implementation of the IDbExecutionStrategy interface.</span></span> <span data-ttu-id="0b28d-112">Le implementazioni di IDbExecutionStrategy saranno responsabili dell'accettazione di un'operazione e, se si verifica un'eccezione, che determina se un nuovo tentativo è appropriato e se è in corso un nuovo tentativo.</span><span class="sxs-lookup"><span data-stu-id="0b28d-112">Implementations of the IDbExecutionStrategy will be responsible for accepting an operation and, if an exception occurs, determining if a retry is appropriate and retrying if it is.</span></span> <span data-ttu-id="0b28d-113">Con EF sono disponibili quattro strategie di esecuzione:</span><span class="sxs-lookup"><span data-stu-id="0b28d-113">There are four execution strategies that ship with EF:</span></span>  

1. <span data-ttu-id="0b28d-114">**DefaultExecutionStrategy**: questa strategia di esecuzione non tenta di eseguire alcuna operazione. si tratta dell'impostazione predefinita per i database diversi da SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0b28d-114">**DefaultExecutionStrategy**: this execution strategy does not retry any operations, it is the default for databases other than sql server.</span></span>  
2. <span data-ttu-id="0b28d-115">**DefaultSqlExecutionStrategy**: si tratta di una strategia di esecuzione interna utilizzata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="0b28d-115">**DefaultSqlExecutionStrategy**: this is an internal execution strategy that is used by default.</span></span> <span data-ttu-id="0b28d-116">Questa strategia non esegue alcun tentativo, tuttavia, eseguirà il wrapping di tutte le eccezioni che potrebbero essere temporanee per informare gli utenti che potrebbero voler abilitare la resilienza della connessione.</span><span class="sxs-lookup"><span data-stu-id="0b28d-116">This strategy does not retry at all, however, it will wrap any exceptions that could be transient to inform users that they might want to enable connection resiliency.</span></span>  
3. <span data-ttu-id="0b28d-117">**DbExecutionStrategy**: questa classe è adatta come classe base per altre strategie di esecuzione, incluse quelle personalizzate.</span><span class="sxs-lookup"><span data-stu-id="0b28d-117">**DbExecutionStrategy**: this class is suitable as a base class for other execution strategies, including your own custom ones.</span></span> <span data-ttu-id="0b28d-118">Implementa un criterio di ripetizione esponenziale, dove il tentativo iniziale si verifica con un ritardo zero e il ritardo aumenta in modo esponenziale fino a quando non viene raggiunto il numero massimo di tentativi.</span><span class="sxs-lookup"><span data-stu-id="0b28d-118">It implements an exponential retry policy, where the initial retry happens with zero delay and the delay increases exponentially until the maximum retry count is hit.</span></span> <span data-ttu-id="0b28d-119">Questa classe dispone di un metodo astratto ShouldRetryOn che può essere implementato in strategie di esecuzione derivate per controllare quali eccezioni devono essere ritentate.</span><span class="sxs-lookup"><span data-stu-id="0b28d-119">This class has an abstract ShouldRetryOn method that can be implemented in derived execution strategies to control which exceptions should be retried.</span></span>  
4. <span data-ttu-id="0b28d-120">**SqlAzureExecutionStrategy**: questa strategia di esecuzione eredita da DbExecutionStrategy e tenterà di ritentare le eccezioni che potrebbero essere transitorie quando si lavora con il database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="0b28d-120">**SqlAzureExecutionStrategy**: this execution strategy inherits from DbExecutionStrategy and will retry on exceptions that are known to be possibly transient when working with Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="0b28d-121">Le strategie di esecuzione 2 e 4 sono incluse nel provider SQL Server fornito con EF, che si trova nell'assembly EntityFramework. SqlServer e sono progettate per funzionare con SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0b28d-121">Execution strategies 2 and 4 are included in the Sql Server provider that ships with EF, which is in the EntityFramework.SqlServer assembly and are designed to work with SQL Server.</span></span>  

## <a name="enabling-an-execution-strategy"></a><span data-ttu-id="0b28d-122">Abilitazione di una strategia di esecuzione</span><span class="sxs-lookup"><span data-stu-id="0b28d-122">Enabling an Execution Strategy</span></span>  

<span data-ttu-id="0b28d-123">Il modo più semplice per indicare a EF di usare una strategia di esecuzione è con il metodo SetExecutionStrategy della classe [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) :</span><span class="sxs-lookup"><span data-stu-id="0b28d-123">The easiest way to tell EF to use an execution strategy is with the SetExecutionStrategy method of the [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) class:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

<span data-ttu-id="0b28d-124">Questo codice indica a EF di usare SqlAzureExecutionStrategy durante la connessione a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0b28d-124">This code tells EF to use the SqlAzureExecutionStrategy when connecting to SQL Server.</span></span>  

## <a name="configuring-the-execution-strategy"></a><span data-ttu-id="0b28d-125">Configurazione della strategia di esecuzione</span><span class="sxs-lookup"><span data-stu-id="0b28d-125">Configuring the Execution Strategy</span></span>  

<span data-ttu-id="0b28d-126">Il costruttore di SqlAzureExecutionStrategy può accettare due parametri, MaxRetryCount e MaxDelay.</span><span class="sxs-lookup"><span data-stu-id="0b28d-126">The constructor of SqlAzureExecutionStrategy can accept two parameters, MaxRetryCount and MaxDelay.</span></span> <span data-ttu-id="0b28d-127">MaxRetry count è il numero massimo di volte in cui la strategia tenterà di riprovare.</span><span class="sxs-lookup"><span data-stu-id="0b28d-127">MaxRetry count is the maximum number of times that the strategy will retry.</span></span> <span data-ttu-id="0b28d-128">MaxDelay è un intervallo di tempo che rappresenta il ritardo massimo tra i tentativi che la strategia di esecuzione utilizzerà.</span><span class="sxs-lookup"><span data-stu-id="0b28d-128">The MaxDelay is a TimeSpan representing the maximum delay between retries that the execution strategy will use.</span></span>  

<span data-ttu-id="0b28d-129">Per impostare il numero massimo di tentativi su 1 e il ritardo massimo su 30 secondi, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="0b28d-129">To set the maximum number of retries to 1 and the maximum delay to 30 seconds you would execute the following:</span></span>  

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

<span data-ttu-id="0b28d-130">Il SqlAzureExecutionStrategy tenterà immediatamente la prima volta che si verifica un errore temporaneo, ma ritarderà tra un tentativo e l'altro fino a quando non viene superato il limite massimo di tentativi o il tempo totale raggiunge il ritardo massimo.</span><span class="sxs-lookup"><span data-stu-id="0b28d-130">The SqlAzureExecutionStrategy will retry instantly the first time a transient failure occurs, but will delay longer between each retry until either the max retry limit is exceeded or the total time hits the max delay.</span></span>  

<span data-ttu-id="0b28d-131">Le strategie di esecuzione ripeteranno solo un numero limitato di eccezioni generalmente temporanee, sarà comunque necessario gestire altri errori, oltre a intercettare l'eccezione RetryLimitExceeded per il caso in cui un errore non è temporaneo o impiega troppo tempo per la risoluzione stesso.</span><span class="sxs-lookup"><span data-stu-id="0b28d-131">The execution strategies will only retry a limited number of exceptions that are usually transient, you will still need to handle other errors as well as catching the RetryLimitExceeded exception for the case where an error is not transient or takes too long to resolve itself.</span></span>  

<span data-ttu-id="0b28d-132">Esistono alcune limitazioni quando si usa una strategia di esecuzione di ripetizione dei tentativi:</span><span class="sxs-lookup"><span data-stu-id="0b28d-132">There are some known of limitations when using a retrying execution strategy:</span></span>  

## <a name="streaming-queries-are-not-supported"></a><span data-ttu-id="0b28d-133">Le query di streaming non sono supportate</span><span class="sxs-lookup"><span data-stu-id="0b28d-133">Streaming queries are not supported</span></span>  

<span data-ttu-id="0b28d-134">Per impostazione predefinita, EF6 e versioni successive registreranno i risultati della query anziché eseguirne il flusso.</span><span class="sxs-lookup"><span data-stu-id="0b28d-134">By default, EF6 and later version will buffer query results rather than streaming them.</span></span> <span data-ttu-id="0b28d-135">Se si desidera che i risultati vengano trasmessi, è possibile utilizzare il metodo AsStreaming per modificare una query LINQ to Entities per la trasmissione.</span><span class="sxs-lookup"><span data-stu-id="0b28d-135">If you want to have results streamed you can use the AsStreaming method to change a LINQ to Entities query to streaming.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

<span data-ttu-id="0b28d-136">Il flusso non è supportato quando viene registrata una strategia di esecuzione ritentata.</span><span class="sxs-lookup"><span data-stu-id="0b28d-136">Streaming is not supported when a retrying execution strategy is registered.</span></span> <span data-ttu-id="0b28d-137">Questa limitazione è dovuta al fatto che la connessione può essere trasformata in modo parziale attraverso i risultati restituiti.</span><span class="sxs-lookup"><span data-stu-id="0b28d-137">This limitation exists because the connection could drop part way through the results being returned.</span></span> <span data-ttu-id="0b28d-138">Quando si verifica questa situazione, EF deve eseguire di nuovo l'intera query, ma non dispone di un modo affidabile per sapere quali risultati sono già stati restituiti (i dati potrebbero essere stati modificati dopo l'invio della query iniziale, i risultati potrebbero essere restituiti in un ordine diverso, i risultati potrebbero non avere un identificatore univoco e così via).</span><span class="sxs-lookup"><span data-stu-id="0b28d-138">When this occurs, EF needs to re-run the entire query but has no reliable way of knowing which results have already been returned (data may have changed since the initial query was sent, results may come back in a different order, results may not have a unique identifier, etc.).</span></span>  

## <a name="user-initiated-transactions-are-not-supported"></a><span data-ttu-id="0b28d-139">Le transazioni avviate dall'utente non sono supportate</span><span class="sxs-lookup"><span data-stu-id="0b28d-139">User initiated transactions are not supported</span></span>  

<span data-ttu-id="0b28d-140">Una volta configurata una strategia di esecuzione che comporta nuovi tentativi, esistono alcune limitazioni relative all'utilizzo delle transazioni.</span><span class="sxs-lookup"><span data-stu-id="0b28d-140">When you have configured an execution strategy that results in retries, there are some limitations around the use of transactions.</span></span>  

<span data-ttu-id="0b28d-141">Per impostazione predefinita, EF eseguirà tutti gli aggiornamenti del database all'interno di una transazione.</span><span class="sxs-lookup"><span data-stu-id="0b28d-141">By default, EF will perform any database updates within a transaction.</span></span> <span data-ttu-id="0b28d-142">Non è necessario eseguire alcuna operazione per abilitare questa operazione, EF esegue sempre questa operazione automaticamente.</span><span class="sxs-lookup"><span data-stu-id="0b28d-142">You don’t need to do anything to enable this, EF always does this automatically.</span></span>  

<span data-ttu-id="0b28d-143">Ad esempio, nel codice SaveChanges seguente viene eseguito automaticamente all'interno di una transazione.</span><span class="sxs-lookup"><span data-stu-id="0b28d-143">For example, in the following code SaveChanges is automatically performed within a transaction.</span></span> <span data-ttu-id="0b28d-144">Se il metodo SaveChanges ha esito negativo dopo l'inserimento di uno dei nuovi siti, viene eseguito il rollback della transazione e non viene applicata alcuna modifica al database.</span><span class="sxs-lookup"><span data-stu-id="0b28d-144">If SaveChanges were to fail after inserting one of the new Site’s then the transaction would be rolled back and no changes applied to the database.</span></span> <span data-ttu-id="0b28d-145">Il contesto viene inoltre lasciato in uno stato che consente di richiamare SaveChanges per ritentare l'applicazione delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="0b28d-145">The context is also left in a state that allows SaveChanges to be called again to retry applying the changes.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

<span data-ttu-id="0b28d-146">Quando non si utilizza una strategia di esecuzione di ripetizione di tentativi, è possibile eseguire il wrapping di più operazioni in una singola transazione.</span><span class="sxs-lookup"><span data-stu-id="0b28d-146">When not using a retrying execution strategy you can wrap multiple operations in a single transaction.</span></span> <span data-ttu-id="0b28d-147">Il codice seguente, ad esempio, esegue il wrapping di due chiamate SaveChanges in un'unica transazione.</span><span class="sxs-lookup"><span data-stu-id="0b28d-147">For example, the following code wraps two SaveChanges calls in a single transaction.</span></span> <span data-ttu-id="0b28d-148">Se una parte di entrambe le operazioni ha esito negativo, non viene applicata nessuna delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="0b28d-148">If any part of either operation fails then none of the changes are applied.</span></span>  

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

<span data-ttu-id="0b28d-149">Questa operazione non è supportata quando si usa una strategia di esecuzione di ripetizione dei tentativi perché EF non è a conoscenza di alcuna operazione precedente e come riprovare.</span><span class="sxs-lookup"><span data-stu-id="0b28d-149">This is not supported when using a retrying execution strategy because EF isn’t aware of any previous operations and how to retry them.</span></span> <span data-ttu-id="0b28d-150">Se ad esempio il secondo SaveChanges ha avuto esito negativo, EF non dispone più delle informazioni necessarie per ripetere la prima chiamata a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="0b28d-150">For example, if the second SaveChanges failed then EF no longer has the required information to retry the first SaveChanges call.</span></span>  

### <a name="solution-manually-call-execution-strategy"></a><span data-ttu-id="0b28d-151">Soluzione: chiamare manualmente la strategia di esecuzione</span><span class="sxs-lookup"><span data-stu-id="0b28d-151">Solution: Manually Call Execution Strategy</span></span>  

<span data-ttu-id="0b28d-152">La soluzione consiste nell'usare manualmente la strategia di esecuzione e fornire l'intero set di logica da eseguire, in modo da poter ritentare tutti i tentativi se una delle operazioni ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="0b28d-152">The solution is to manually use the execution strategy and give it the entire set of logic to be run, so that it can retry everything if one of the operations fails.</span></span> <span data-ttu-id="0b28d-153">Quando viene eseguita una strategia di esecuzione derivata da DbExecutionStrategy, la strategia di esecuzione implicita utilizzata in SaveChanges verrà sospesa.</span><span class="sxs-lookup"><span data-stu-id="0b28d-153">When an execution strategy derived from DbExecutionStrategy is running it will suspend the implicit execution strategy used in SaveChanges.</span></span>  

<span data-ttu-id="0b28d-154">Si noti che tutti i contesti devono essere costruiti all'interno del blocco di codice per poter essere ripetuti.</span><span class="sxs-lookup"><span data-stu-id="0b28d-154">Note that any contexts should be constructed within the code block to be retried.</span></span> <span data-ttu-id="0b28d-155">In questo modo si inizia con uno stato pulito per ogni nuovo tentativo.</span><span class="sxs-lookup"><span data-stu-id="0b28d-155">This ensures that we are starting with a clean state for each retry.</span></span>  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

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
```  
