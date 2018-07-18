---
title: Gestione della connessione - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
caps.latest.revision: 3
ms.openlocfilehash: 9588ef85435d3c0218defdc098f1e7150fb7ef72
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122761"
---
# <a name="connection-management"></a>Gestione delle connessioni
Questa pagina descrive il comportamento di Entity Framework per quanto riguarda il passaggio di connessioni per il contesto e le funzionalità dei **Database.Connection.Open()** API.  

## <a name="passing-connections-to-the-context"></a>Connessioni passando al contesto  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Comportamento per EF5 e versioni precedenti  

Esistono due costruttori che accettano connessioni:  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

È possibile usare queste ma è necessario risolvere un paio di limitazioni:  

1. Se si passa una connessione aperta a uno di questi quindi la prima volta che il framework tenta di utilizzarla che viene generata un'eccezione InvalidOperationException che indica che non è possibile riaprire una connessione già aperta.  
2. Il flag contextOwnsConnection è interpretato in modo da indicare se la connessione all'archivio sottostante deve essere eliminata quando viene eliminato il contesto. Tuttavia, indipendentemente dal fatto che l'impostazione, la connessione all'archivio viene sempre chiusa quando viene eliminato il contesto. In modo che se si dispone di più di un oggetto DbContext con la stessa connessione qualunque sia il contesto viene eliminato prima di tutto la connessione verrà chiusa (allo stesso modo se si dispone di una connessione ADO.NET esistente con un oggetto DbContext, DbContext sempre la connessione verrà chiusa quando viene eliminata) .  

È possibile aggirare la prima limitazione sopra il passaggio di una connessione chiusa e solo l'esecuzione di codice che viene aperto dopo avere creati tutti i contesti:  

``` csharp
using System.Collections.Generic;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Linq;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExampleEF5
    {         
        public static void TwoDbContextsOneConnection()
        {
            using (var context1 = new BloggingContext())
            {
                var conn =
                    ((EntityConnection)  
                        ((IObjectContextAdapter)context1).ObjectContext.Connection)  
                            .StoreConnection;

                using (var context2 = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    context2.Database.ExecuteSqlCommand(
                        @"UPDATE Blogs SET Rating = 5" +
                        " WHERE Name LIKE '%Entity Framework%'");

                    var query = context1.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context1.SaveChanges();
                }
            }
        }
    }
}
```  

La seconda limitazione significa semplicemente che devi fintato disposing uno qualsiasi degli oggetti DbContext fino a quando non si è pronti per la connessione da chiudere.  

### <a name="behavior-in-ef6-and-future-versions"></a>Comportamento in EF6 e le versioni future  

Nelle versioni future ed EF6 DbContext ha gli stessi due costruttori, ma non è più necessario che la connessione passata al costruttore chiusa quando viene ricevuto. Pertanto, questa funzionalità è ora:  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExample
    {
        public static void PassingAnOpenConnection()
        {
            using (var conn = new SqlConnection("{connectionString}"))
            {
                conn.Open();

                var sqlCommand = new SqlCommand();
                sqlCommand.Connection = conn;
                sqlCommand.CommandText =
                    @"UPDATE Blogs SET Rating = 5" +
                     " WHERE Name LIKE '%Entity Framework%'";
                sqlCommand.ExecuteNonQuery();

                using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    var query = context.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context.SaveChanges();
                }

                var sqlCommand2 = new SqlCommand();
                sqlCommand2.Connection = conn;
                sqlCommand2.CommandText =
                    @"UPDATE Blogs SET Rating = 7" +
                     " WHERE Name LIKE '%Entity Framework Rocks%'";
                sqlCommand2.ExecuteNonQuery();
            }
        }
    }
}
```  

Il flag contextOwnsConnection ora controlla anche se la connessione viene chiusa ed eliminata quando viene eliminato l'oggetto DbContext. Pertanto nell'esempio precedente la connessione non viene chiusa quando il contesto viene eliminato (riga 32) come avveniva nelle versioni precedenti di Entity Framework, ma piuttosto quando viene eliminata la connessione stessa (riga 40).  

Naturalmente è comunque possibile per DbContext assumere il controllo della connessione (solo set contextOwnsConnection su true oppure utilizzare uno degli altri costruttori) se si vuole quindi.  

> [!NOTE]
> Esistono alcune considerazioni aggiuntive quando si utilizzano transazioni con questo nuovo modello. Per informazioni dettagliate, vedere [utilizzo di transazioni](~/ef6/saving/transactions.md).  

## <a name="databaseconnectionopen"></a>Database.Connection.Open()  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Comportamento per EF5 e versioni precedenti  

Nelle versioni precedenti ed EF5 c'è un bug in modo che il **ObjectContext.Connection.State** non è stato aggiornato per riflettere lo stato reale di connessione all'archivio sottostante. Ad esempio, se è stato eseguito il codice seguente si può essere restituito lo stato **Closed** anche se in realtà sottostante della connessione dell'archivio **Open**.  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

Separatamente, se si apre la connessione al database tramite la chiamata Database.Connection.Open() sarà aperto fino a quando non la volta successiva che si esegue una query o chiama alcuna operazione che richiede una connessione al database (ad esempio, SaveChanges()) mentre dopo che l'oggetto sottostante archivio connessione verrà chiusa. Il contesto verrà riaprire e chiudere nuovamente la connessione ogni volta che un'altra operazione di database è necessaria:  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;

namespace ConnectionManagementExamples
{
    public class DatabaseOpenConnectionBehaviorEF5
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open  
                // (though ObjectContext.Connection.State will report closed)

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);

                // The underlying store connection is still open  

                context.SaveChanges();

                // After SaveChanges() the underlying store connection is closed  
                // Each SaveChanges() / query etc now opens and immediately closes
                // the underlying store connection

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();
            }
        }
    }
}
```  

### <a name="behavior-in-ef6-and-future-versions"></a>Comportamento in EF6 e le versioni future  

Per Entity Framework 6 e versioni successive sono state prese l'approccio che se il codice chiamante sceglie di aprire la connessione dal contesto di chiamata. Il framework presuppone che vuole controllare di apertura e chiusura della connessione e non chiuderà la connessione automaticamente Database.Connection.Open(), allora ha un motivo valido per questa operazione.  

> [!NOTE]
> Questo può causare potenzialmente le connessioni che sono aperte per molto tempo così usare con cautela.  

Il codice è inoltre aggiornato in modo che ObjectContext.Connection.State ora tiene traccia dello stato della connessione sottostante in modo corretto.  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Core.EntityClient;
using System.Data.Entity.Infrastructure;

namespace ConnectionManagementExamples
{
    internal class DatabaseOpenConnectionBehaviorEF6
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open and the
                // ObjectContext.Connection.State correctly reports open too

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection remains open for the next operation  

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection is still open

           } // The context is disposed – so now the underlying store connection is closed
        }
    }
}
```  
