---
title: Transazioni - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: 2dda7b7d58ae058fc2aa89fe16fbf46adc8c6bdc
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2018
---
# <a name="using-transactions"></a><span data-ttu-id="c5c49-102">Utilizzo delle transazioni</span><span class="sxs-lookup"><span data-stu-id="c5c49-102">Using Transactions</span></span>

<span data-ttu-id="c5c49-103">Le transazioni consentono di varie operazioni di database possono essere elaborati in modo atomico.</span><span class="sxs-lookup"><span data-stu-id="c5c49-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="c5c49-104">Se viene eseguito il commit della transazione, tutte le operazioni vengono applicate correttamente nel database.</span><span class="sxs-lookup"><span data-stu-id="c5c49-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="c5c49-105">Se viene eseguito il rollback della transazione, nessuna delle operazioni vengono applicata al database.</span><span class="sxs-lookup"><span data-stu-id="c5c49-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="c5c49-106">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="c5c49-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="c5c49-107">Comportamento predefinito delle transazioni</span><span class="sxs-lookup"><span data-stu-id="c5c49-107">Default transaction behavior</span></span>

<span data-ttu-id="c5c49-108">Per impostazione predefinita, se il provider di database supporta le transazioni, tutte le modifiche in una singola chiamata a `SaveChanges()` vengono applicate in una transazione.</span><span class="sxs-lookup"><span data-stu-id="c5c49-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="c5c49-109">Se le modifiche hanno esito negativo, viene eseguito il rollback della transazione e le modifiche vengono applicate al database.</span><span class="sxs-lookup"><span data-stu-id="c5c49-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="c5c49-110">Ciò significa che `SaveChanges()` è considerata completamente esito positivo, o lasciare il database non modificato se si verifica un errore.</span><span class="sxs-lookup"><span data-stu-id="c5c49-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="c5c49-111">Per la maggior parte delle applicazioni, questo comportamento predefinito è sufficiente.</span><span class="sxs-lookup"><span data-stu-id="c5c49-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="c5c49-112">Controllare le transazioni manualmente solo se i requisiti dell'applicazione sono necessario.</span><span class="sxs-lookup"><span data-stu-id="c5c49-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="c5c49-113">Controllo delle transazioni</span><span class="sxs-lookup"><span data-stu-id="c5c49-113">Controlling transactions</span></span>

<span data-ttu-id="c5c49-114">È possibile utilizzare il `DbContext.Database` API per iniziare, eseguire il commit e il rollback di transazioni.</span><span class="sxs-lookup"><span data-stu-id="c5c49-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="c5c49-115">Nell'esempio seguente vengono illustrati due `SaveChanges()` LINQ e operazioni di query in esecuzione in una singola transazione.</span><span class="sxs-lookup"><span data-stu-id="c5c49-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="c5c49-116">Non tutti i provider di database supportano le transazioni.</span><span class="sxs-lookup"><span data-stu-id="c5c49-116">Not all database providers support transactions.</span></span> <span data-ttu-id="c5c49-117">Alcuni provider possono generare o viene eseguita alcuna operazione quando transazione vengono chiamate le API.</span><span class="sxs-lookup"><span data-stu-id="c5c49-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?highlight=3,17,18,19)] -->
``` csharp
        using (var context = new BloggingContext())
        {
            using (var transaction = context.Database.BeginTransaction())
            {
                try
                {
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();

                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/visualstudio" });
                    context.SaveChanges();

                    var blogs = context.Blogs
                        .OrderBy(b => b.Url)
                        .ToList();

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="c5c49-118">Transazione di contesto tra (solo database relazionali)</span><span class="sxs-lookup"><span data-stu-id="c5c49-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="c5c49-119">È anche possibile condividere una transazione tra più istanze di contesto.</span><span class="sxs-lookup"><span data-stu-id="c5c49-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="c5c49-120">Questa funzionalità è disponibile solo quando si utilizza un provider di database relazionale, perché richiede l'utilizzo di `DbTransaction` e `DbConnection`, che sono specifiche per i database relazionali.</span><span class="sxs-lookup"><span data-stu-id="c5c49-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="c5c49-121">Per condividere una transazione, è necessario che i contesti di condividere entrambi un `DbConnection` e `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="c5c49-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="c5c49-122">Consenti connessione devono essere fornite esternamente</span><span class="sxs-lookup"><span data-stu-id="c5c49-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="c5c49-123">La condivisione di un `DbConnection` richiede la possibilità di passare una connessione in un contesto durante la costruzione.</span><span class="sxs-lookup"><span data-stu-id="c5c49-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="c5c49-124">Il modo più semplice per consentire `DbConnection` specificare esternamente consiste nell'interrompere l'utilizzo di `DbContext.OnConfiguring` per configurare il contesto e creare esternamente `DbContextOptions` e passarli al costruttore di contesto.</span><span class="sxs-lookup"><span data-stu-id="c5c49-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="c5c49-125">`DbContextOptionsBuilder` è l'API utilizzata nel `DbContext.OnConfiguring` per configurare il contesto, ora si desidera utilizzarlo per creare esternamente `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="c5c49-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="c5c49-126">In alternativa è possibile continuare a usare `DbContext.OnConfiguring`, ma accetta un `DbConnection` che viene salvato e quindi utilizzato `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="c5c49-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    private DbConnection _connection;

    public BloggingContext(DbConnection connection)
    {
      _connection = connection;
    }

    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(_connection);
    }
}
```

### <a name="share-connection-and-transaction"></a><span data-ttu-id="c5c49-127">Condivisione connessione e transazione</span><span class="sxs-lookup"><span data-stu-id="c5c49-127">Share connection and transaction</span></span>

<span data-ttu-id="c5c49-128">È ora possibile creare più istanze di contesto che condividono la stessa connessione.</span><span class="sxs-lookup"><span data-stu-id="c5c49-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="c5c49-129">Utilizzare quindi la `DbContext.Database.UseTransaction(DbTransaction)` API per integrare entrambi i contesti nella stessa transazione.</span><span class="sxs-lookup"><span data-stu-id="c5c49-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="c5c49-130">Utilizzando DbTransactions esterno (solo database relazionali)</span><span class="sxs-lookup"><span data-stu-id="c5c49-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="c5c49-131">Se si utilizza tecnologie di accesso ai dati più per accedere a un database relazionale, si desidera condividere una transazione tra le operazioni eseguite da queste tecnologie diverse.</span><span class="sxs-lookup"><span data-stu-id="c5c49-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="c5c49-132">L'esempio seguente viene illustrato come eseguire un'operazione di ADO.NET SqlClient e un'operazione di Entity Framework Core nella stessa transazione.</span><span class="sxs-lookup"><span data-stu-id="c5c49-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="c5c49-133">Using System. Transactions</span><span class="sxs-lookup"><span data-stu-id="c5c49-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="c5c49-134">Questa funzionalità è nuova in Entity Framework Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="c5c49-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="c5c49-135">È possibile utilizzare le transazioni di ambiente, se è necessario coordinare un ambito più ampio.</span><span class="sxs-lookup"><span data-stu-id="c5c49-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,24,25,26)]

<span data-ttu-id="c5c49-136">È inoltre possibile eseguire l'inserimento in una transazione esplicita.</span><span class="sxs-lookup"><span data-stu-id="c5c49-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,13,26,27,28)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="c5c49-137">Limitazioni di System. Transactions</span><span class="sxs-lookup"><span data-stu-id="c5c49-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="c5c49-138">Core EF si basa sui provider di database per implementare il supporto per System. Transactions.</span><span class="sxs-lookup"><span data-stu-id="c5c49-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="c5c49-139">Sebbene il supporto è piuttosto comune tra i provider ADO.NET per .NET Framework, l'API solo aggiunti di recente a .NET Core e il supporto di conseguenza non è essere molto diffuso.</span><span class="sxs-lookup"><span data-stu-id="c5c49-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not be as widespread.</span></span> <span data-ttu-id="c5c49-140">Se un provider non implementa il supporto per System. Transactions, è possibile che le chiamate a queste API verranno ignorate completamente.</span><span class="sxs-lookup"><span data-stu-id="c5c49-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="c5c49-141">SqlClient per .NET Core supporti da 2.1 in poi.</span><span class="sxs-lookup"><span data-stu-id="c5c49-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="c5c49-142">SqlClient per .NET 2.0 Core genererà un'eccezione di si tenta di utilizzare la funzionalità.</span><span class="sxs-lookup"><span data-stu-id="c5c49-142">SqlClient for .NET Core 2.0 will throw an exception of you attempt to use the feature.</span></span> 

   > [!IMPORTANT]  
   > <span data-ttu-id="c5c49-143">È consigliabile verificare che l'API si comporti correttamente con il provider prima basarsi su di essa per la gestione delle transazioni.</span><span class="sxs-lookup"><span data-stu-id="c5c49-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="c5c49-144">Sono invitati a contattare il gestore del provider del database in caso contrario.</span><span class="sxs-lookup"><span data-stu-id="c5c49-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span> 

2. <span data-ttu-id="c5c49-145">A partire dalla versione 2.1, l'implementazione di System. Transactions in .NET Core non include il supporto per le transazioni distribuite, pertanto è possibile utilizzare `TransactionScope` o `CommitableTransaction`per coordinare le transazioni tra più gestori di risorse.</span><span class="sxs-lookup"><span data-stu-id="c5c49-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommitableTransaction`to coordinate transactions across multiple resource managers.</span></span> 
