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
# <a name="using-transactions"></a>Utilizzo delle transazioni

Le transazioni consentono di varie operazioni di database possono essere elaborati in modo atomico. Se viene eseguito il commit della transazione, tutte le operazioni vengono applicate correttamente nel database. Se viene eseguito il rollback della transazione, nessuna delle operazioni vengono applicata al database.

> [!TIP]  
> È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) su GitHub.

## <a name="default-transaction-behavior"></a>Comportamento predefinito delle transazioni

Per impostazione predefinita, se il provider di database supporta le transazioni, tutte le modifiche in una singola chiamata a `SaveChanges()` vengono applicate in una transazione. Se le modifiche hanno esito negativo, viene eseguito il rollback della transazione e le modifiche vengono applicate al database. Ciò significa che `SaveChanges()` è considerata completamente esito positivo, o lasciare il database non modificato se si verifica un errore.

Per la maggior parte delle applicazioni, questo comportamento predefinito è sufficiente. Controllare le transazioni manualmente solo se i requisiti dell'applicazione sono necessario.

## <a name="controlling-transactions"></a>Controllo delle transazioni

È possibile utilizzare il `DbContext.Database` API per iniziare, eseguire il commit e il rollback di transazioni. Nell'esempio seguente vengono illustrati due `SaveChanges()` LINQ e operazioni di query in esecuzione in una singola transazione.

Non tutti i provider di database supportano le transazioni. Alcuni provider possono generare o viene eseguita alcuna operazione quando transazione vengono chiamate le API.

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

## <a name="cross-context-transaction-relational-databases-only"></a>Transazione di contesto tra (solo database relazionali)

È anche possibile condividere una transazione tra più istanze di contesto. Questa funzionalità è disponibile solo quando si utilizza un provider di database relazionale, perché richiede l'utilizzo di `DbTransaction` e `DbConnection`, che sono specifiche per i database relazionali.

Per condividere una transazione, è necessario che i contesti di condividere entrambi un `DbConnection` e `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Consenti connessione devono essere fornite esternamente

La condivisione di un `DbConnection` richiede la possibilità di passare una connessione in un contesto durante la costruzione.

Il modo più semplice per consentire `DbConnection` specificare esternamente consiste nell'interrompere l'utilizzo di `DbContext.OnConfiguring` per configurare il contesto e creare esternamente `DbContextOptions` e passarli al costruttore di contesto.

> [!TIP]  
> `DbContextOptionsBuilder`è l'API utilizzata nel `DbContext.OnConfiguring` per configurare il contesto, ora si desidera utilizzarlo per creare esternamente `DbContextOptions`.

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

In alternativa è possibile continuare a usare `DbContext.OnConfiguring`, ma accetta un `DbConnection` che viene salvato e quindi utilizzato `DbContext.OnConfiguring`.

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

### <a name="share-connection-and-transaction"></a>Condivisione connessione e transazione

È ora possibile creare più istanze di contesto che condividono la stessa connessione. Utilizzare quindi la `DbContext.Database.UseTransaction(DbTransaction)` API per integrare entrambi i contesti nella stessa transazione.

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

## <a name="using-external-dbtransactions-relational-databases-only"></a>Utilizzando DbTransactions esterno (solo database relazionali)

Se si utilizza tecnologie di accesso ai dati più per accedere a un database relazionale, si desidera condividere una transazione tra le operazioni eseguite da queste tecnologie diverse.

L'esempio seguente viene illustrato come eseguire un'operazione di ADO.NET SqlClient e un'operazione di Entity Framework Core nella stessa transazione.

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
