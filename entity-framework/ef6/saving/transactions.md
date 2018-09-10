---
title: Utilizzo di transazioni - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 26473e1e52a6044babc717d5b158ad73aac5c738
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250609"
---
# <a name="working-with-transactions"></a>Utilizzo di transazioni
> [!NOTE]
> **Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.  

Questo documento viene descritto con transazioni in EF6, inclusi i miglioramenti che sono stati aggiunti dall'EF5 per facilitare l'utilizzo di transazioni.  

## <a name="what-ef-does-by-default"></a>Che cosa avviene per impostazione predefinita Entity Framework  

In tutte le versioni di Entity Framework, ogni volta che si esegue **SaveChanges ()** per inserire, aggiornare o eliminare il database il framework eseguirà il wrapping dell'operazione in una transazione. Questa operazione dura solo il tempo sufficiente per eseguire l'operazione e quindi viene completato. Quando si esegue un'altra operazione di questo tipo viene avviata una nuova transazione.  

A partire da EF6 **Database.ExecuteSqlCommand()** per impostazione predefinita va a capo il comando in una transazione se uno non era già presente. Esistono overload del metodo che consentono di eseguire l'override di questo comportamento se si desidera. Anche in EF6 l'esecuzione di stored procedure incluse nel modello tramite le API, ad esempio **ObjectContext.ExecuteFunction()** esegue la stessa (ad eccezione del fatto che il comportamento predefinito non è al momento eseguire l'override).  

In entrambi i casi, il livello di isolamento della transazione è qualsiasi livello di isolamento il provider di database prende in considerazione l'impostazione predefinita. Per impostazione predefinita, ad esempio, in SQL Server questo è READ COMMITTED.  

Entity Framework non va a capo le query in una transazione.  

Questa funzionalità predefinita è adatta per molti utenti e se così non è necessario eseguire alcuna operazione diversa in EF6; basta scrivere il codice come sempre.  

Tuttavia, alcuni utenti richiedono un maggiore controllo alle transazioni: questo argomento viene trattato nelle sezioni seguenti.  

## <a name="how-the-apis-work"></a>Funzionamento delle API  

Prima di EF6 Entity Framework scoperto sull'apertura della connessione al database stesso (ha generato un'eccezione se è stata passata una connessione che era già aperta). Poiché una transazione può essere avviata solo su una connessione aperta, ciò significava che l'unico modo un utente è stato possibile eseguire il wrapping di diverse operazioni in un'unica transazione era di usare una [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) oppure usare il  **ObjectContext.Connection** proprietà e chiamare il metodo start **Open ()** e **BeginTransaction ()** direttamente sull'oggetto restituito **EntityConnection** oggetto. Inoltre, le chiamate API che ha contattato il database avrebbe esito negativo se si ha avviato una transazione sulla connessione al database sottostante in modo autonomo.  

> [!NOTE]
> In Entity Framework 6 è stata rimossa la limitazione di accettare solo connessioni chiuse. Per informazioni dettagliate, vedere [gestione connessione](~/ef6/fundamentals/connection-management.md).  

A partire da Entity Framework 6 il framework ora fornisce:  

1. **Database.BeginTransaction()** : un metodo più semplice per un utente di avviare e completare le transazioni stesse all'interno di un DbContext esistenti, consentendo alle diverse operazioni di essere combinati all'interno della stessa transazione e pertanto eseguito il commit oppure tutti il rollback di uno. Consente inoltre all'utente di specificare più facilmente il livello di isolamento della transazione.  
2. **Database.UseTransaction()** : che consente di utilizzare una transazione di cui è stata avviata all'esterno di Entity Framework DbContext.  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a>La combinazione di diverse operazioni in un'unica transazione nello stesso contesto  

**Database.BeginTransaction()** include due sostituzioni: uno che accetta l'impostazione esplicita [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) e uno che non accetta argomenti e utilizza il valore predefinito IsolationLevel dal provider di database sottostante. In entrambi gli override restituiscono una **DbContextTransaction** oggetto che fornisce **commit ()** e **rollback ()** metodi che consentono di eseguire commit e rollback nell'archivio sottostante transazione.  

Il **DbContextTransaction** è progettato per essere eliminato dopo che è stato eseguito il commit o rollback. Un semplice metodo per ottenere questo risultato è il **using(...) {...}** sintassi che chiamerà automaticamente **Dispose ()** blocco using quando viene completata:  

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
                    try
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
                    catch (Exception)
                    {
                        dbContextTransaction.Rollback();
                    }
                }
            }
        }
    }
}
```  

> [!NOTE]
> Iniziare una transazione richiede che sia aperta la connessione all'archivio sottostante. Quindi la chiamata Database.BeginTransaction() verrà aperta la connessione se non è già aperta. Se DbContextTransaction aperta la connessione quindi chiuderà quando viene chiamato Dispose ().  

### <a name="passing-an-existing-transaction-to-the-context"></a>Il passaggio di una transazione esistente nel contesto  

A volte si vuole che una transazione di cui è ancora più ampio nell'ambito e include operazioni nello stesso database, ma all'esterno di EF completamente. A tale scopo è necessario aprire la connessione e avviare manualmente la transazione e quindi indicare a Entity Framework) per usare la connessione al database già aperto e b) per utilizzare la transazione esistente su tale connessione.  

A tale scopo è necessario definire e usare un costruttore nella classe del contesto che eredita da uno dei costruttori che accettano booleani i) un parametro di connessione esistente e ii) i contextOwnsConnection DbContext.  

> [!NOTE]
> Il flag contextOwnsConnection deve essere impostato su false quando viene chiamato in questo scenario. Questo è importante in quanto indica a Entity Framework non deve chiudere la connessione dopo che è stata eseguita con esso (ad esempio, vedere la riga 4 di seguito):  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

Inoltre, è necessario avviare la transazione se stessi (incluso il IsolationLevel se si desidera evitare l'impostazione predefinita) e informare Entity Framework che vi sia una transazione esistente già avviata per la connessione (vedere la riga 33 riportato di seguito).  

Quindi si possono eseguire operazioni di database direttamente in SqlConnection stesso o in DbContext. Tutte queste operazioni vengono eseguite all'interno di una transazione. Assumersi la responsabilità per eseguire il commit o rollback della transazione e per la chiamata a Dispose () su di esso, nonché per la chiusura ed eliminazione della connessione di database. Ad esempio:  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
sing System.Transactions;

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
                   try
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
                    catch (Exception)
                    {
                        sqlTxn.Rollback();
                    }
                }
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a>La cancellazione del contenuto della transazione

È possibile passare null per Database.UseTransaction() per cancellare la conoscenza di Entity Framework della transazione corrente. Entity Framework verrà né commit né il rollback della transazione esistente quando si esegue questa operazione, pertanto usare con cautela e solo se si conosce questo è ciò che si desidera eseguire.  

### <a name="errors-in-usetransaction"></a>Errori in UseTransaction

Si noterà un'eccezione da Database.UseTransaction() se si passa una transazione quando:  
- Entity Framework è già presente una transazione esistente  
- Entity Framework è già operativo all'interno di un ambito di transazione  
- L'oggetto di connessione nella transazione passato è null. Vale a dire, la transazione non è associata a una connessione – in genere questo è un segno di tale transazione già completata  
- L'oggetto di connessione nella transazione passato non corrisponde la connessione di Entity Framework.  

## <a name="using-transactions-with-other-features"></a>Utilizzo delle transazioni con altre funzionalità  

In questa sezione descrive come interagiscono le transazioni precedenti:  

- Resilienza della connessione  
- Metodi asincroni  
- Transazioni di TransactionScope  

### <a name="connection-resiliency"></a>Resilienza della connessione  

La nuova funzionalità resilienza della connessione non funziona con le transazioni avviate dall'utente. Per informazioni dettagliate, vedere [ripetono strategie di esecuzione](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).  

### <a name="asynchronous-programming"></a>Programmazione asincrona  

L'approccio descritto nelle sezioni precedenti non richiede ulteriori opzioni o le impostazioni per lavorare con i [asincrono di query e salvare i metodi](~/ef6/fundamentals/async.md
). Tuttavia, tenere presente che, a seconda delle operazioni eseguite all'interno di metodi asincroni, questo può comportare transazioni a esecuzione prolungata, che a sua volta possono causare deadlock o il blocco che non è valido per le prestazioni dell'applicazione complessiva.  

### <a name="transactionscope-transactions"></a>Transazioni di TransactionScope  

Prima di EF6 il modo consigliato di fornire le transazioni di ambito più grande era per usare un oggetto TransactionScope:  

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

Il SqlConnection Entity Framework potrebbe usare la transazione di ambiente TransactionScope e quindi eseguire il commit.  

A partire da .NET 4.5.1 TransactionScope è stata aggiornata per funzionare anche con i metodi asincroni tramite l'utilizzo dei [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumerazione:  

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

Esistono comunque alcune limitazioni all'approccio TransactionScope:  

- Richiede .NET 4.5.1 o versioni successive lavorare con i metodi asincroni.  
- Non può essere usato negli scenari cloud, a meno che non è certi di utilizzare un'unica connessione (scenari basati su cloud non supportano le transazioni distribuite).  
- Non può essere combinato con l'approccio Database.UseTransaction() delle sezioni precedenti.  
- Genererà eccezioni se si rilascia qualsiasi DDL e le transazioni distribuite tramite il servizio MSDTC non sono stati abilitati.  

Vantaggi dell'approccio TransactionScope:  

- Aggiorneranno automaticamente una transazione locale a una transazione distribuita se si imposta più di una connessione a un determinato database o combina una connessione a un database con una connessione a un database diverso nella stessa transazione (Nota: è necessario disporre di il servizio MSDTC è configurato per consentire le transazioni distribuite per funzionare).  
- Semplicità di codifica. Se si preferisce la transazione di ambiente e trattate in modo implicito in background invece di controllare in modo esplicito in quindi l'approccio di TransactionScope potrebbe essere più appropriato è migliore.  

In breve, con la nuova Database.BeginTransaction() Database.UseTransaction() API precedente, l'approccio di TransactionScope è più necessario per la maggior parte degli utenti. Se si continua a usare TransactionScope tenere presente le limitazioni precedenti. È consigliabile usare l'approccio descritto nelle sezioni precedenti invece laddove possibile.  
