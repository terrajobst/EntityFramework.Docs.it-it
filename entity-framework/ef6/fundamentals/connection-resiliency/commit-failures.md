---
title: La gestione degli errori di commit transaction - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: f912777104c2e925122c05046d4d65660de8b8a8
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250861"
---
# <a name="handling-transaction-commit-failures"></a><span data-ttu-id="c6f73-102">Gestione degli errori di commit delle transazioni</span><span class="sxs-lookup"><span data-stu-id="c6f73-102">Handling transaction commit failures</span></span>
> [!NOTE]
> <span data-ttu-id="c6f73-103">**EF6.1 e versioni successive solo** -le funzionalità, le API, e così via illustrati in questa pagina sono stati introdotti in Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="c6f73-103">**EF6.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="c6f73-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="c6f73-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="c6f73-105">Come parte di 6.1 viene introdotto una nuova funzionalità di resilienza di connessione per Entity Framework: la possibilità di rilevare e correggere automaticamente quando gli errori di connessione temporanei interessano l'acknowledgement del commit della transazione.</span><span class="sxs-lookup"><span data-stu-id="c6f73-105">As part of 6.1 we are introducing a new connection resiliency feature for EF: the ability to detect and recover automatically when transient connection failures affect the acknowledgement of transaction commits.</span></span> <span data-ttu-id="c6f73-106">I dettagli completi dello scenario sono meglio descritti nel post di blog [connettività del Database SQL e il problema di idempotenza](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span><span class="sxs-lookup"><span data-stu-id="c6f73-106">The full details of the scenario are best described in the blog post [SQL Database Connectivity and the Idempotency Issue](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span></span>  <span data-ttu-id="c6f73-107">In breve, lo scenario è che, quando viene generata un'eccezione durante il commit di una transazione, ci sono due le cause possibili:</span><span class="sxs-lookup"><span data-stu-id="c6f73-107">In summary, the scenario is that when an exception is raised during a transaction commit there are two possible causes:</span></span>  

1. <span data-ttu-id="c6f73-108">Il commit della transazione non riuscita nel server</span><span class="sxs-lookup"><span data-stu-id="c6f73-108">The transaction commit failed on the server</span></span>
2. <span data-ttu-id="c6f73-109">Il commit della transazione ha avuto esito positivo nel server, ma un problema di connettività ha impedito la notifica di esito positivo di raggiungere il client</span><span class="sxs-lookup"><span data-stu-id="c6f73-109">The transaction commit succeeded on the server but a connectivity issue prevented the success notification from reaching the client</span></span>  

<span data-ttu-id="c6f73-110">Quando la prima situazione si verifica l'applicazione o l'utente può ripetere l'operazione, ma quando si verifica la situazione secondo i tentativi devono essere evitati e l'applicazione è stato possibile recuperare automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c6f73-110">When the first situation happens the application or the user can retry the operation, but when the second situation occurs retries should be avoided and the application could recover automatically.</span></span> <span data-ttu-id="c6f73-111">Il problema è che senza la possibilità di rilevare Qual era il motivo effettivo è stato segnalato un'eccezione durante il commit, l'applicazione non è possibile scegliere la scelta giusta dell'azione.</span><span class="sxs-lookup"><span data-stu-id="c6f73-111">The challenge is that without the ability to detect what was the actual reason an exception was reported during commit, the application cannot choose the right course of action.</span></span> <span data-ttu-id="c6f73-112">La nuova funzionalità in EF 6.1 consente di controllare con il database se la transazione ha avuto esito positivo e Segui il corso a destra dell'azione in modo trasparente a Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c6f73-112">The new feature in EF 6.1 allows EF to double-check with the database if the transaction succeeded and take the right course of action transparently.</span></span>  

## <a name="using-the-feature"></a><span data-ttu-id="c6f73-113">Uso della funzionalità</span><span class="sxs-lookup"><span data-stu-id="c6f73-113">Using the feature</span></span>  

<span data-ttu-id="c6f73-114">Per abilitare la funzionalità è necessario includere una chiamata a [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) nel costruttore delle **DbConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="c6f73-114">In order to enable the feature you need include a call to [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) in the constructor of your **DbConfiguration**.</span></span> <span data-ttu-id="c6f73-115">Se non si ha familiarità con **DbConfiguration**, vedere [configurazione basata su codice](~/ef6/fundamentals/configuring/code-based.md).</span><span class="sxs-lookup"><span data-stu-id="c6f73-115">If you are unfamiliar with **DbConfiguration**, see [Code Based Configuration](~/ef6/fundamentals/configuring/code-based.md).</span></span> <span data-ttu-id="c6f73-116">Questa funzionalità può essere utilizzata in combinazione con la ripetizione automatica dei tentativi abbiamo introdotto in EF6, che consentono la situazione in cui la transazione effettivamente non è stato possibile eseguire il commit nel server a causa di un errore temporaneo:</span><span class="sxs-lookup"><span data-stu-id="c6f73-116">This feature can be used in combination with the automatic retries we introduced in EF6, which help in the situation in which the transaction actually failed to commit on the server due to a transient failure:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

public class MyConfiguration : DbConfiguration  
{
  public MyConfiguration()  
  {  
    SetTransactionHandler(SqlProviderServices.ProviderInvariantName, () => new CommitFailureHandler());  
    SetExecutionStrategy(SqlProviderServices.ProviderInvariantName, () => new SqlAzureExecutionStrategy());  
  }  
}
```  

## <a name="how-transactions-are-tracked"></a><span data-ttu-id="c6f73-117">Modalità di rilevamento delle transazioni</span><span class="sxs-lookup"><span data-stu-id="c6f73-117">How transactions are tracked</span></span>  

<span data-ttu-id="c6f73-118">Quando è abilitata la funzionalità, Entity Framework aggiungerà automaticamente una nuova tabella al database denominato **__Transactions**.</span><span class="sxs-lookup"><span data-stu-id="c6f73-118">When the feature is enabled, EF will automatically add a new table to the database called **__Transactions**.</span></span> <span data-ttu-id="c6f73-119">In questa tabella viene inserita una nuova riga ogni volta che viene creata una transazione da Entity Framework e che la riga viene verificata l'esistenza se si verifica un errore della transazione durante il commit.</span><span class="sxs-lookup"><span data-stu-id="c6f73-119">A new row is inserted in this table every time a transaction is created by EF and that row is checked for existence if a transaction failure occurs during commit.</span></span>  

<span data-ttu-id="c6f73-120">Benché EF eseguirà sforzo possibile escludere le righe della tabella quando non sono più necessari, la tabella può aumentare se l'applicazione viene chiusa in modo anomalo e per tale motivo, che potrebbe essere necessario eliminare la tabella manualmente in alcuni casi.</span><span class="sxs-lookup"><span data-stu-id="c6f73-120">Although EF will do a best effort to prune rows from the table when they aren’t needed anymore, the table can grow if the application exits prematurely and for that reason you may need to purge the table manually in some cases.</span></span>  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a><span data-ttu-id="c6f73-121">Come gestire gli errori di commit con le versioni precedenti</span><span class="sxs-lookup"><span data-stu-id="c6f73-121">How to handle commit failures with previous Versions</span></span>

<span data-ttu-id="c6f73-122">Prima di 6.1 di Entity Framework non era meccanismo per gestire gli errori di commit all'interno del prodotto di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c6f73-122">Before EF 6.1 there was not mechanism to handle commit failures in the EF product.</span></span> <span data-ttu-id="c6f73-123">Esistono diversi modi per gestire questa situazione che può essere applicata alle versioni precedenti di Entity Framework 6:</span><span class="sxs-lookup"><span data-stu-id="c6f73-123">There are several ways to dealing with this situation that can be applied to previous versions of EF6:</span></span>  

* <span data-ttu-id="c6f73-124">Opzione 1: non eseguire alcuna operazione</span><span class="sxs-lookup"><span data-stu-id="c6f73-124">Option 1 - Do nothing</span></span>  

  <span data-ttu-id="c6f73-125">La probabilità di un errore di connessione durante il commit della transazione è bassa, pertanto potrebbe essere accettabile per l'applicazione viene eseguita solo se questa condizione si verifica effettivamente.</span><span class="sxs-lookup"><span data-stu-id="c6f73-125">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>  

* <span data-ttu-id="c6f73-126">Opzione 2: usare il database per reimpostare lo stato</span><span class="sxs-lookup"><span data-stu-id="c6f73-126">Option 2 - Use the database to reset state</span></span>  

  1. <span data-ttu-id="c6f73-127">Eliminare DbContext corrente</span><span class="sxs-lookup"><span data-stu-id="c6f73-127">Discard the current DbContext</span></span>  
  2. <span data-ttu-id="c6f73-128">Creare un nuovo DbContext e ripristinare lo stato dell'applicazione dal database</span><span class="sxs-lookup"><span data-stu-id="c6f73-128">Create a new DbContext and restore the state of your application from the database</span></span>  
  3. <span data-ttu-id="c6f73-129">Informare l'utente che l'ultima operazione potrebbe non siano stata completata correttamente</span><span class="sxs-lookup"><span data-stu-id="c6f73-129">Inform the user that the last operation might not have been completed successfully</span></span>  

* <span data-ttu-id="c6f73-130">Opzione 3: tenere manualmente traccia della transazione</span><span class="sxs-lookup"><span data-stu-id="c6f73-130">Option 3 - Manually track the transaction</span></span>  

  1. <span data-ttu-id="c6f73-131">Aggiungere una tabella con rilevamento nel database usato per tenere traccia dello stato delle transazioni.</span><span class="sxs-lookup"><span data-stu-id="c6f73-131">Add a non-tracked table to the database used to track the status of the transactions.</span></span>  
  2. <span data-ttu-id="c6f73-132">Inserire una riga nella tabella all'inizio di ogni transazione.</span><span class="sxs-lookup"><span data-stu-id="c6f73-132">Insert a row into the table at the beginning of each transaction.</span></span>  
  3. <span data-ttu-id="c6f73-133">Se la connessione non riesce durante il commit, verificare la presenza della riga corrispondente nel database.</span><span class="sxs-lookup"><span data-stu-id="c6f73-133">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>  
     - <span data-ttu-id="c6f73-134">Se la riga è presente, continua a funzionare normalmente, come la transazione è stato eseguito il commit</span><span class="sxs-lookup"><span data-stu-id="c6f73-134">If the row is present, continue normally, as the transaction was committed successfully</span></span>  
     - <span data-ttu-id="c6f73-135">Se la riga è assente, è possibile usare una strategia di esecuzione per ripetere l'operazione corrente.</span><span class="sxs-lookup"><span data-stu-id="c6f73-135">If the row is absent, use an execution strategy to retry the current operation.</span></span>  
  4. <span data-ttu-id="c6f73-136">Se il commit ha esito positivo, eliminare la riga corrispondente per evitare la crescita della tabella.</span><span class="sxs-lookup"><span data-stu-id="c6f73-136">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>  

<span data-ttu-id="c6f73-137">[Questo post di blog](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contiene codice di esempio per questa operazione in SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="c6f73-137">[This blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contains sample code for accomplishing this on SQL Azure.</span></span>  
