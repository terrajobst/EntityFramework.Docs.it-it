---
title: Gestione della connessione - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
ms.openlocfilehash: dc405e1876edc850ae6e4ce4649da52635d316ae
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997905"
---
# <a name="connection-management"></a><span data-ttu-id="bb6d0-102">Gestione delle connessioni</span><span class="sxs-lookup"><span data-stu-id="bb6d0-102">Connection management</span></span>
<span data-ttu-id="bb6d0-103">Questa pagina descrive il comportamento di Entity Framework per quanto riguarda il passaggio di connessioni per il contesto e le funzionalità dei **Database.Connection.Open()** API.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-103">This page describes the behavior of Entity Framework with regard to passing connections to the context and the functionality of the **Database.Connection.Open()** API.</span></span>  

## <a name="passing-connections-to-the-context"></a><span data-ttu-id="bb6d0-104">Connessioni passando al contesto</span><span class="sxs-lookup"><span data-stu-id="bb6d0-104">Passing Connections to the Context</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="bb6d0-105">Comportamento per EF5 e versioni precedenti</span><span class="sxs-lookup"><span data-stu-id="bb6d0-105">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="bb6d0-106">Esistono due costruttori che accettano connessioni:</span><span class="sxs-lookup"><span data-stu-id="bb6d0-106">There are two constructors which accept connections:</span></span>  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

<span data-ttu-id="bb6d0-107">È possibile usare queste ma è necessario risolvere un paio di limitazioni:</span><span class="sxs-lookup"><span data-stu-id="bb6d0-107">It is possible to use these but you have to work around a couple of limitations:</span></span>  

1. <span data-ttu-id="bb6d0-108">Se si passa una connessione aperta a uno di questi quindi la prima volta che il framework tenta di utilizzarla che viene generata un'eccezione InvalidOperationException che indica che non è possibile riaprire una connessione già aperta.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-108">If you pass an open connection to either of these then the first time the framework attempts to use it an InvalidOperationException is thrown saying it cannot re-open an already open connection.</span></span>  
2. <span data-ttu-id="bb6d0-109">Il flag contextOwnsConnection è interpretato in modo da indicare se la connessione all'archivio sottostante deve essere eliminata quando viene eliminato il contesto.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-109">The contextOwnsConnection flag is interpreted to mean whether or not the underlying store connection should be disposed when the context is disposed.</span></span> <span data-ttu-id="bb6d0-110">Tuttavia, indipendentemente dal fatto che l'impostazione, la connessione all'archivio viene sempre chiusa quando viene eliminato il contesto.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-110">But, regardless of that setting, the store connection is always closed when the context is disposed.</span></span> <span data-ttu-id="bb6d0-111">In modo che se si dispone di più di un oggetto DbContext con la stessa connessione qualunque sia il contesto viene eliminato prima di tutto la connessione verrà chiusa (allo stesso modo se si dispone di una connessione ADO.NET esistente con un oggetto DbContext, DbContext sempre la connessione verrà chiusa quando viene eliminata) .</span><span class="sxs-lookup"><span data-stu-id="bb6d0-111">So if you have more than one DbContext with the same connection whichever context is disposed first will close the connection (similarly if you have mixed an existing ADO.NET connection with a DbContext, DbContext will always close the connection when it is disposed).</span></span>  

<span data-ttu-id="bb6d0-112">È possibile aggirare la prima limitazione sopra il passaggio di una connessione chiusa e solo l'esecuzione di codice che viene aperto dopo avere creati tutti i contesti:</span><span class="sxs-lookup"><span data-stu-id="bb6d0-112">It is possible to work around the first limitation above by passing a closed connection and only executing code that would open it once all contexts have been created:</span></span>  

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

<span data-ttu-id="bb6d0-113">La seconda limitazione significa semplicemente che devi fintato disposing uno qualsiasi degli oggetti DbContext fino a quando non si è pronti per la connessione da chiudere.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-113">The second limitation just means you need to refrain from disposing any of your DbContext objects until you are ready for the connection to be closed.</span></span>  

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="bb6d0-114">Comportamento in EF6 e le versioni future</span><span class="sxs-lookup"><span data-stu-id="bb6d0-114">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="bb6d0-115">Nelle versioni future ed EF6 DbContext ha gli stessi due costruttori, ma non è più necessario che la connessione passata al costruttore chiusa quando viene ricevuto.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-115">In EF6 and future versions the DbContext has the same two constructors but no longer requires that the connection passed to the constructor be closed when it is received.</span></span> <span data-ttu-id="bb6d0-116">Pertanto, questa funzionalità è ora:</span><span class="sxs-lookup"><span data-stu-id="bb6d0-116">So this is now possible:</span></span>  

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

<span data-ttu-id="bb6d0-117">Il flag contextOwnsConnection ora controlla anche se la connessione viene chiusa ed eliminata quando viene eliminato l'oggetto DbContext.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-117">Also the contextOwnsConnection flag now controls whether or not the connection is both closed and disposed when the DbContext is disposed.</span></span> <span data-ttu-id="bb6d0-118">Pertanto nell'esempio precedente la connessione non viene chiusa quando il contesto viene eliminato (riga 32) come avveniva nelle versioni precedenti di Entity Framework, ma piuttosto quando viene eliminata la connessione stessa (riga 40).</span><span class="sxs-lookup"><span data-stu-id="bb6d0-118">So in the above example the connection is not closed when the context is disposed (line 32) as it would have been in previous versions of EF, but rather when the connection itself is disposed (line 40).</span></span>  

<span data-ttu-id="bb6d0-119">Naturalmente è comunque possibile per DbContext assumere il controllo della connessione (solo set contextOwnsConnection su true oppure utilizzare uno degli altri costruttori) se si vuole quindi.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-119">Of course it is still possible for the DbContext to take control of the connection (just set contextOwnsConnection to true or use one of the other constructors) if you so wish.</span></span>  

> [!NOTE]
> <span data-ttu-id="bb6d0-120">Esistono alcune considerazioni aggiuntive quando si utilizzano transazioni con questo nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-120">There are some additional considerations when using transactions with this new model.</span></span> <span data-ttu-id="bb6d0-121">Per informazioni dettagliate, vedere [utilizzo di transazioni](~/ef6/saving/transactions.md).</span><span class="sxs-lookup"><span data-stu-id="bb6d0-121">For details see [Working with Transactions](~/ef6/saving/transactions.md).</span></span>  

## <a name="databaseconnectionopen"></a><span data-ttu-id="bb6d0-122">Database.Connection.Open()</span><span class="sxs-lookup"><span data-stu-id="bb6d0-122">Database.Connection.Open()</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="bb6d0-123">Comportamento per EF5 e versioni precedenti</span><span class="sxs-lookup"><span data-stu-id="bb6d0-123">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="bb6d0-124">Nelle versioni precedenti ed EF5 c'è un bug in modo che il **ObjectContext.Connection.State** non è stato aggiornato per riflettere lo stato reale di connessione all'archivio sottostante.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-124">In EF5 and earlier versions there is a bug such that the **ObjectContext.Connection.State** was not updated to reflect the true state of the underlying store connection.</span></span> <span data-ttu-id="bb6d0-125">Ad esempio, se è stato eseguito il codice seguente si può essere restituito lo stato **Closed** anche se in realtà sottostante della connessione dell'archivio **Open**.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-125">For example, if you executed the following code you can be returned the status **Closed** even though in fact the underlying store connection is **Open**.</span></span>  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

<span data-ttu-id="bb6d0-126">Separatamente, se si apre la connessione al database tramite la chiamata Database.Connection.Open() sarà aperto fino a quando non la volta successiva che si esegue una query o chiama alcuna operazione che richiede una connessione al database (ad esempio, SaveChanges()) mentre dopo che l'oggetto sottostante archivio connessione verrà chiusa.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-126">Separately, if you open the database connection by calling Database.Connection.Open() it will be open until the next time you execute a query or call anything which requires a database connection (for example, SaveChanges()) but after that the underlying store connection will be closed.</span></span> <span data-ttu-id="bb6d0-127">Il contesto verrà riaprire e chiudere nuovamente la connessione ogni volta che un'altra operazione di database è necessaria:</span><span class="sxs-lookup"><span data-stu-id="bb6d0-127">The context will then re-open and re-close the connection any time another database operation is required:</span></span>  

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

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="bb6d0-128">Comportamento in EF6 e le versioni future</span><span class="sxs-lookup"><span data-stu-id="bb6d0-128">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="bb6d0-129">Per Entity Framework 6 e versioni successive sono state prese l'approccio che se il codice chiamante sceglie di aprire la connessione dal contesto di chiamata. Il framework presuppone che vuole controllare di apertura e chiusura della connessione e non chiuderà la connessione automaticamente Database.Connection.Open(), allora ha un motivo valido per questa operazione.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-129">For EF6 and future versions we have taken the approach that if the calling code chooses to open the connection by calling context.Database.Connection.Open() then it has a good reason for doing so and the framework will assume that it wants control over opening and closing of the connection and will no longer close the connection automatically.</span></span>  

> [!NOTE]
> <span data-ttu-id="bb6d0-130">Questo può causare potenzialmente le connessioni che sono aperte per molto tempo così usare con cautela.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-130">This can potentially lead to connections which are open for a long time so use with care.</span></span>  

<span data-ttu-id="bb6d0-131">Il codice è inoltre aggiornato in modo che ObjectContext.Connection.State ora tiene traccia dello stato della connessione sottostante in modo corretto.</span><span class="sxs-lookup"><span data-stu-id="bb6d0-131">We also updated the code so that ObjectContext.Connection.State now keeps track of the state of the underlying connection correctly.</span></span>  

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
