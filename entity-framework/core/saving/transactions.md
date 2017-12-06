---
title: Transazioni - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: a2f890c0af1e83cbcc1d40d68540ff7132a9bafd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="using-transactions"></a><span data-ttu-id="34f64-102">Utilizzo delle transazioni</span><span class="sxs-lookup"><span data-stu-id="34f64-102">Using Transactions</span></span>

<span data-ttu-id="34f64-103">Le transazioni consentono di varie operazioni di database possono essere elaborati in modo atomico.</span><span class="sxs-lookup"><span data-stu-id="34f64-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="34f64-104">Se viene eseguito il commit della transazione, tutte le operazioni vengono applicate correttamente nel database.</span><span class="sxs-lookup"><span data-stu-id="34f64-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="34f64-105">Se viene eseguito il rollback della transazione, nessuna delle operazioni vengono applicata al database.</span><span class="sxs-lookup"><span data-stu-id="34f64-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="34f64-106">È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) su GitHub.</span><span class="sxs-lookup"><span data-stu-id="34f64-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="34f64-107">Comportamento predefinito delle transazioni</span><span class="sxs-lookup"><span data-stu-id="34f64-107">Default transaction behavior</span></span>

<span data-ttu-id="34f64-108">Per impostazione predefinita, se il provider di database supporta le transazioni, tutte le modifiche in una singola chiamata a `SaveChanges()` vengono applicate in una transazione.</span><span class="sxs-lookup"><span data-stu-id="34f64-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="34f64-109">Se le modifiche hanno esito negativo, viene eseguito il rollback della transazione e le modifiche vengono applicate al database.</span><span class="sxs-lookup"><span data-stu-id="34f64-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="34f64-110">Ciò significa che `SaveChanges()` è considerata completamente esito positivo, o lasciare il database non modificato se si verifica un errore.</span><span class="sxs-lookup"><span data-stu-id="34f64-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="34f64-111">Per la maggior parte delle applicazioni, questo comportamento predefinito è sufficiente.</span><span class="sxs-lookup"><span data-stu-id="34f64-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="34f64-112">Controllare le transazioni manualmente solo se i requisiti dell'applicazione sono necessario.</span><span class="sxs-lookup"><span data-stu-id="34f64-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="34f64-113">Controllo delle transazioni</span><span class="sxs-lookup"><span data-stu-id="34f64-113">Controlling transactions</span></span>

<span data-ttu-id="34f64-114">È possibile utilizzare il `DbContext.Database` API per iniziare, eseguire il commit e il rollback di transazioni.</span><span class="sxs-lookup"><span data-stu-id="34f64-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="34f64-115">Nell'esempio seguente vengono illustrati due `SaveChanges()` LINQ e operazioni di query in esecuzione in una singola transazione.</span><span class="sxs-lookup"><span data-stu-id="34f64-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="34f64-116">Non tutti i provider di database supportano le transazioni.</span><span class="sxs-lookup"><span data-stu-id="34f64-116">Not all database providers support transactions.</span></span> <span data-ttu-id="34f64-117">Alcuni provider possono generare o viene eseguita alcuna operazione quando transazione vengono chiamate le API.</span><span class="sxs-lookup"><span data-stu-id="34f64-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

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

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="34f64-118">Transazione di contesto tra (solo database relazionali)</span><span class="sxs-lookup"><span data-stu-id="34f64-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="34f64-119">È anche possibile condividere una transazione tra più istanze di contesto.</span><span class="sxs-lookup"><span data-stu-id="34f64-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="34f64-120">Questa funzionalità è disponibile solo quando si utilizza un provider di database relazionale, perché richiede l'utilizzo di `DbTransaction` e `DbConnection`, che sono specifiche per i database relazionali.</span><span class="sxs-lookup"><span data-stu-id="34f64-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="34f64-121">Per condividere una transazione, è necessario che i contesti di condividere entrambi un `DbConnection` e `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="34f64-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="34f64-122">Consenti connessione devono essere fornite esternamente</span><span class="sxs-lookup"><span data-stu-id="34f64-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="34f64-123">La condivisione di un `DbConnection` richiede la possibilità di passare una connessione in un contesto durante la costruzione.</span><span class="sxs-lookup"><span data-stu-id="34f64-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="34f64-124">Il modo più semplice per consentire `DbConnection` specificare esternamente consiste nell'interrompere l'utilizzo di `DbContext.OnConfiguring` per configurare il contesto e creare esternamente `DbContextOptions` e passarli al costruttore di contesto.</span><span class="sxs-lookup"><span data-stu-id="34f64-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="34f64-125">`DbContextOptionsBuilder`è l'API utilizzata nel `DbContext.OnConfiguring` per configurare il contesto, ora si desidera utilizzarlo per creare esternamente `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="34f64-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=3,4,5)] -->
``` csharp
    public class BloggingContext : DbContext
    {
        public BloggingContext(DbContextOptions<BloggingContext> options)
            : base(options)
        { }

        public DbSet<Blog> Blogs { get; set; }
    }
```

<span data-ttu-id="34f64-126">In alternativa è possibile continuare a usare `DbContext.OnConfiguring`, ma accetta un `DbConnection` che viene salvato e quindi utilizzato `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="34f64-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="34f64-127">Condivisione connessione e transazione</span><span class="sxs-lookup"><span data-stu-id="34f64-127">Share connection and transaction</span></span>

<span data-ttu-id="34f64-128">È ora possibile creare più istanze di contesto che condividono la stessa connessione.</span><span class="sxs-lookup"><span data-stu-id="34f64-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="34f64-129">Utilizzare quindi la `DbContext.Database.UseTransaction(DbTransaction)` API per integrare entrambi i contesti nella stessa transazione.</span><span class="sxs-lookup"><span data-stu-id="34f64-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=1,2,3,7,16,23,24,25)] -->
``` csharp
        var options = new DbContextOptionsBuilder<BloggingContext>()
            .UseSqlServer(new SqlConnection(connectionString))
            .Options;

        using (var context1 = new BloggingContext(options))
        {
            using (var transaction = context1.Database.BeginTransaction())
            {
                try
                {
                    context1.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context1.SaveChanges();

                    using (var context2 = new BloggingContext(options))
                    {
                        context2.Database.UseTransaction(transaction.GetDbTransaction());

                        var blogs = context2.Blogs
                            .OrderBy(b => b.Url)
                            .ToList();
                    }

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

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="34f64-130">Utilizzando DbTransactions esterno (solo database relazionali)</span><span class="sxs-lookup"><span data-stu-id="34f64-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="34f64-131">Se si utilizza tecnologie di accesso ai dati più per accedere a un database relazionale, si desidera condividere una transazione tra le operazioni eseguite da queste tecnologie diverse.</span><span class="sxs-lookup"><span data-stu-id="34f64-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="34f64-132">L'esempio seguente viene illustrato come eseguire un'operazione di ADO.NET SqlClient e un'operazione di Entity Framework Core nella stessa transazione.</span><span class="sxs-lookup"><span data-stu-id="34f64-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?highlight=4,10,21,26,27,28)] -->
``` csharp
        var connection = new SqlConnection(connectionString);
        connection.Open();

        using (var transaction = connection.BeginTransaction())
        {
            try
            {
                // Run raw ADO.NET command in the transaction
                var command = connection.CreateCommand();
                command.Transaction = transaction;
                command.CommandText = "DELETE FROM dbo.Blogs";
                command.ExecuteNonQuery();

                // Run an EF Core command in the transaction
                var options = new DbContextOptionsBuilder<BloggingContext>()
                    .UseSqlServer(connection)
                    .Options;

                using (var context = new BloggingContext(options))
                {
                    context.Database.UseTransaction(transaction);
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();
                }

                // Commit transaction if all commands succeed, transaction will auto-rollback
                // when disposed if either commands fails
                transaction.Commit();
            }
            catch (System.Exception)
            {
                // TODO: Handle failure
            }
```
