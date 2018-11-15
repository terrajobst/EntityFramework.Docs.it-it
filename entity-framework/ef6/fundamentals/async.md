---
title: Asincrono di query e save - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 4ed4f5c13341f33ccff8325a5ddacd8f7b195a76
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283823"
---
# <a name="async-query-and-save"></a><span data-ttu-id="d4bf3-102">Asincrono di query e salvare</span><span class="sxs-lookup"><span data-stu-id="d4bf3-102">Async query and save</span></span>
> [!NOTE]
> <span data-ttu-id="d4bf3-103">**Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="d4bf3-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="d4bf3-105">Entity Framework 6 ha introdotto il supporto per query asincrona e il salvataggio con il [async e await parole chiave](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) che sono state introdotte in .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-105">EF6 introduced support for asynchronous query and save using the [async and await keywords](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) that were introduced in .NET 4.5.</span></span> <span data-ttu-id="d4bf3-106">Sebbene non tutte le applicazioni possono trarre vantaggio dalla modalità asincrona, può essere utilizzato per migliorare la scalabilità della velocità di risposta e server client durante la gestione con esecuzione prolungata, rete o I/O-limite attività.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-106">While not all applications may benefit from asynchrony, it can be used to improve client responsiveness and server scalability when handling long-running, network or I/O-bound tasks.</span></span>

## <a name="when-to-really-use-async"></a><span data-ttu-id="d4bf3-107">Quando utilizzare davvero async</span><span class="sxs-lookup"><span data-stu-id="d4bf3-107">When to really use async</span></span>

<span data-ttu-id="d4bf3-108">Lo scopo di questa procedura dettagliata è presentare i concetti di async in modo che rende più semplice osservare la differenza tra l'esecuzione del programma sincrone e asincrone.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-108">The purpose of this walkthrough is to introduce the async concepts in a way that makes it easy to observe the difference between asynchronous and synchronous program execution.</span></span> <span data-ttu-id="d4bf3-109">Questa procedura dettagliata non può illustrare gli scenari principali in cui la programmazione asincrona fornisce vantaggi.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-109">This walkthrough is not intended to illustrate any of the key scenarios where async programming provides benefits.</span></span>

<span data-ttu-id="d4bf3-110">Programmazione asincrona è incentrata principalmente sugli liberare il thread gestito corrente (codice .NET in esecuzione thread) di eseguire altre attività durante l'attesa di un'operazione che non richiede alcun tempo di calcolo da un thread gestito.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-110">Async programming is primarily focused on freeing up the current managed thread (thread running .NET code) to do other work while it waits for an operation that does not require any compute time from a managed thread.</span></span> <span data-ttu-id="d4bf3-111">Ad esempio, durante il motore di database è l'elaborazione di una query non esegue alcuna operazione da eseguire dal codice .NET.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-111">For example, whilst the database engine is processing a query there is nothing to be done by .NET code.</span></span>

<span data-ttu-id="d4bf3-112">Nelle applicazioni client (WinForms, WPF e così via), il thread corrente è utilizzabile per mantenere reattiva l'interfaccia utente mentre viene eseguita l'operazione asincrona.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-112">In client applications (WinForms, WPF, etc.) the current thread can be used to keep the UI responsive while the async operation is performed.</span></span> <span data-ttu-id="d4bf3-113">Nelle applicazioni server (ASP.NET e così via) al thread può essere utilizzato per elaborare altre richieste in ingresso - ciò può ridurre l'utilizzo della memoria e/o aumentare la velocità effettiva del server.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-113">In server applications (ASP.NET etc.) the thread can be used to process other incoming requests - this can reduce memory usage and/or increase throughput of the server.</span></span>

<span data-ttu-id="d4bf3-114">Nella maggior parte delle applicazioni di uso della funzionalità async non avranno alcun vantaggio evidente e potrebbe anche essere negativo.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-114">In most applications using async will have no noticeable benefits and even could be detrimental.</span></span> <span data-ttu-id="d4bf3-115">Usare i test, profilatura e buon senso per misurare l'impatto delle operazioni asincrone in un determinato scenario prima di eseguire il commit a esso.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-115">Use tests, profiling and common sense to measure the impact of async in your particular scenario before committing to it.</span></span>

<span data-ttu-id="d4bf3-116">Ecco alcune altre risorse per apprendere async:</span><span class="sxs-lookup"><span data-stu-id="d4bf3-116">Here are some more resources to learn about async:</span></span>

-   [<span data-ttu-id="d4bf3-117">Panoramica del Brandon Bray di async/await in .NET 4.5</span><span class="sxs-lookup"><span data-stu-id="d4bf3-117">Brandon Bray’s overview of async/await in .NET 4.5</span></span>](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   <span data-ttu-id="d4bf3-118">[Programmazione asincrona](https://msdn.microsoft.com/library/hh191443.aspx) pagine in MSDN Library</span><span class="sxs-lookup"><span data-stu-id="d4bf3-118">[Asynchronous Programming](https://msdn.microsoft.com/library/hh191443.aspx) pages in the MSDN Library</span></span>
-   <span data-ttu-id="d4bf3-119">[Come compilare ASP.NET Web Applications tramite Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (che include una dimostrazione di velocità effettiva maggiore server)</span><span class="sxs-lookup"><span data-stu-id="d4bf3-119">[How to Build ASP.NET Web Applications Using Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (includes a demo of increased server throughput)</span></span>

## <a name="create-the-model"></a><span data-ttu-id="d4bf3-120">Creare il modello</span><span class="sxs-lookup"><span data-stu-id="d4bf3-120">Create the model</span></span>

<span data-ttu-id="d4bf3-121">Verrà usato il [flusso di lavoro di Code First](~/ef6/modeling/code-first/workflows/new-database.md) per creare il nostro modello e generare il database, tuttavia, la funzionalità asincrona funzionerà con tutti i modelli di Entity Framework, inclusi quelli creati con la finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-121">We’ll be using the [Code First workflow](~/ef6/modeling/code-first/workflows/new-database.md) to create our model and generate the database, however the asynchronous functionality will work with all EF models including those created with the EF Designer.</span></span>

-   <span data-ttu-id="d4bf3-122">Creare un'applicazione Console e chiamarlo **AsyncDemo**</span><span class="sxs-lookup"><span data-stu-id="d4bf3-122">Create a Console Application and call it **AsyncDemo**</span></span>
-   <span data-ttu-id="d4bf3-123">Aggiungere il pacchetto EntityFramework NuGet</span><span class="sxs-lookup"><span data-stu-id="d4bf3-123">Add the EntityFramework NuGet package</span></span>
    -   <span data-ttu-id="d4bf3-124">In Esplora soluzioni fare clic sui **AsyncDemo** progetto</span><span class="sxs-lookup"><span data-stu-id="d4bf3-124">In Solution Explorer, right-click on the **AsyncDemo** project</span></span>
    -   <span data-ttu-id="d4bf3-125">Selezionare **Gestisci pacchetti NuGet...**</span><span class="sxs-lookup"><span data-stu-id="d4bf3-125">Select **Manage NuGet Packages…**</span></span>
    -   <span data-ttu-id="d4bf3-126">Nella finestra di dialogo Gestisci pacchetti NuGet, selezionare la **Online** scheda e scegliere il **EntityFramework** pacchetto</span><span class="sxs-lookup"><span data-stu-id="d4bf3-126">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
    -   <span data-ttu-id="d4bf3-127">Fare clic su **installare**</span><span class="sxs-lookup"><span data-stu-id="d4bf3-127">Click **Install**</span></span>
-   <span data-ttu-id="d4bf3-128">Aggiungere un **Model.cs** classe con l'implementazione seguente</span><span class="sxs-lookup"><span data-stu-id="d4bf3-128">Add a **Model.cs** class with the following implementation</span></span>

``` csharp
    using System.Collections.Generic;
    using System.Data.Entity;

    namespace AsyncDemo
    {
        public class BloggingContext : DbContext
        {
            public DbSet<Blog> Blogs { get; set; }
            public DbSet<Post> Posts { get; set; }
        }

        public class Blog
        {
            public int BlogId { get; set; }
            public string Name { get; set; }

            public virtual List<Post> Posts { get; set; }
        }

        public class Post
        {
            public int PostId { get; set; }
            public string Title { get; set; }
            public string Content { get; set; }

            public int BlogId { get; set; }
            public virtual Blog Blog { get; set; }
        }
    }
```

 

## <a name="create-a-synchronous-program"></a><span data-ttu-id="d4bf3-129">Creare un programma sincrono</span><span class="sxs-lookup"><span data-stu-id="d4bf3-129">Create a synchronous program</span></span>

<span data-ttu-id="d4bf3-130">Ora che abbiamo un modello di EF, è possibile scrivere codice che lo usa per eseguire alcune l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-130">Now that we have an EF model, let's write some code that uses it to perform some data access.</span></span>

-   <span data-ttu-id="d4bf3-131">Sostituire il contenuto del **Program.cs** con il codice seguente</span><span class="sxs-lookup"><span data-stu-id="d4bf3-131">Replace the contents of **Program.cs** with the following code</span></span>

``` csharp
    using System;
    using System.Linq;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                PerformDatabaseOperations();

                Console.WriteLine();
                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static void PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    db.SaveChanges();

                    // Query for all blogs ordered by name
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine();
                    Console.WriteLine("All blogs:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="d4bf3-132">Questo codice chiama il **PerformDatabaseOperations** metodo che consente di salvare un nuovo **Blog** per il database, quindi recupera tutti i **blog** dal database e li visualizza per il **Console**.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-132">This code calls the **PerformDatabaseOperations** method which saves a new **Blog** to the database and then retrieves all **Blogs** from the database and prints them to the **Console**.</span></span> <span data-ttu-id="d4bf3-133">Al termine, il programma scrive un'offerta del giorno per il **Console**.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-133">After this, the program writes a quote of the day to the **Console**.</span></span>

<span data-ttu-id="d4bf3-134">Poiché il codice sincrono, è possibile osservare il seguente flusso di esecuzione quando si esegue il programma:</span><span class="sxs-lookup"><span data-stu-id="d4bf3-134">Since the code is synchronous, we can observe the following execution flow when we run the program:</span></span>

1.  <span data-ttu-id="d4bf3-135">**SaveChanges** inizia a eseguire il push di nuove **Blog** al database</span><span class="sxs-lookup"><span data-stu-id="d4bf3-135">**SaveChanges** begins to push the new **Blog** to the database</span></span>
2.  <span data-ttu-id="d4bf3-136">**SaveChanges** completa</span><span class="sxs-lookup"><span data-stu-id="d4bf3-136">**SaveChanges** completes</span></span>
3.  <span data-ttu-id="d4bf3-137">Query per tutti i **blog** viene inviato al database</span><span class="sxs-lookup"><span data-stu-id="d4bf3-137">Query for all **Blogs** is sent to the database</span></span>
4.  <span data-ttu-id="d4bf3-138">Query restituisce e i risultati vengono scritti **Console**</span><span class="sxs-lookup"><span data-stu-id="d4bf3-138">Query returns and results are written to **Console**</span></span>
5.  <span data-ttu-id="d4bf3-139">Offerta del giorno verrà scritti in **Console**</span><span class="sxs-lookup"><span data-stu-id="d4bf3-139">Quote of the day is written to **Console**</span></span>

![Uscita sincronizzazione](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a><span data-ttu-id="d4bf3-141">Rendendo asincrona</span><span class="sxs-lookup"><span data-stu-id="d4bf3-141">Making it asynchronous</span></span>

<span data-ttu-id="d4bf3-142">Ora che abbiamo il nostro programma sia operativa, è possibile iniziare ad apportarvi usare di nuovo async e await parole chiave.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-142">Now that we have our program up and running, we can begin making use of the new async and await keywords.</span></span> <span data-ttu-id="d4bf3-143">Sono state apportate le modifiche seguenti al file Program.cs</span><span class="sxs-lookup"><span data-stu-id="d4bf3-143">We've made the following changes to Program.cs</span></span>

1.  <span data-ttu-id="d4bf3-144">La riga 2: L'uso istruzione per il **Data. Entity** dello spazio dei nomi fornisce l'accesso ai metodi di estensione asincrono Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-144">Line 2: The using statement for the **System.Data.Entity** namespace gives us access to the EF async extension methods.</span></span>
2.  <span data-ttu-id="d4bf3-145">Riga 4: L'uso istruzione per il **Tasks** dello spazio dei nomi consente di utilizzare il **attività** tipo.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-145">Line 4: The using statement for the **System.Threading.Tasks** namespace allows us to use the **Task** type.</span></span>
3.  <span data-ttu-id="d4bf3-146">Riga 12 & 18: vengono acquisiti come attività che monitori l'avanzamento della **PerformSomeDatabaseOperations** (riga 12) e quindi blocca l'esecuzione del programma per questa attività a una volta completata tutto il lavoro per il programma viene creato (riga 18).</span><span class="sxs-lookup"><span data-stu-id="d4bf3-146">Line 12 & 18: We are capturing as task that monitors the progress of **PerformSomeDatabaseOperations** (line 12) and then blocking program execution for this task to complete once all the work for the program is done (line 18).</span></span>
4.  <span data-ttu-id="d4bf3-147">Riga 25: Abbiamo update **PerformSomeDatabaseOperations** essere contrassegnato come **async** e restituire un **attività**.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-147">Line 25: We've update **PerformSomeDatabaseOperations** to be marked as **async** and return a **Task**.</span></span>
5.  <span data-ttu-id="d4bf3-148">Riga 35: È ora stiamo chiamando la versione asincrona di SaveChanges e in attesa del relativo completamento.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-148">Line 35: We're now calling the Async version of SaveChanges and awaiting it's completion.</span></span>
6.  <span data-ttu-id="d4bf3-149">Riga 42: È ora stiamo chiamando la versione asincrona di ToList e in attesa del risultato.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-149">Line 42: We're now calling hte Async version of ToList and awaiting on the result.</span></span>

<span data-ttu-id="d4bf3-150">Per un elenco completo dei metodi di estensione disponibile nello spazio dei nomi di Data. Entity, fare riferimento alla classe QueryableExtensions.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-150">For a comprehensive list of available extension methods in the System.Data.Entity namespace, refer to the QueryableExtensions class.</span></span> <span data-ttu-id="d4bf3-151">*È anche necessario aggiungere "using Data. Entity" al usando le istruzioni.*</span><span class="sxs-lookup"><span data-stu-id="d4bf3-151">*You’ll also need to add “using System.Data.Entity” to your using statements.*</span></span>

``` csharp
    using System;
    using System.Data.Entity;
    using System.Linq;
    using System.Threading.Tasks;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                var task = PerformDatabaseOperations();

                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                task.Wait();

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static async Task PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    Console.WriteLine("Calling SaveChanges.");
                    await db.SaveChangesAsync();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = await (from b in db.Blogs
                                orderby b.Name
                                select b).ToListAsync();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" - " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="d4bf3-152">Ora che il codice è asincrono, è possibile osservare un flusso di esecuzione diverso quando si esegue il programma:</span><span class="sxs-lookup"><span data-stu-id="d4bf3-152">Now that the code is asyncronous, we can observe a different execution flow when we run the program:</span></span>

1.  <span data-ttu-id="d4bf3-153">**SaveChanges** inizia a eseguire il push di nuove **Blog** al database *dopo il comando viene inviato al database non più di calcolo è necessario tempo sul thread gestito corrente. Il **PerformDatabaseOperations** metodo viene restituito (anche se non è stata completata l'esecuzione) e continua il flusso di programma nel metodo Main.*</span><span class="sxs-lookup"><span data-stu-id="d4bf3-153">**SaveChanges** begins to push the new **Blog** to the database *Once the command is sent to the database no more compute time is needed on the current managed thread. The **PerformDatabaseOperations** method returns (even though it hasn't finished executing) and program flow in the Main method continues.*</span></span>
2.  <span data-ttu-id="d4bf3-154">**Offerta del giorno Console verrà scritti**
    \*perché è presente non altre operazioni da eseguire nel metodo Main, il thread gestito è bloccato in attesa chiamare fino al completamento dell'operazione sul database. Al termine, la parte restante del nostro **PerformDatabaseOperations** \* verrà eseguito.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-154">**Quote of the day is written to Console**
*Since there is no more work to do in the Main method, the managed thread is blocked on the Wait call until the database operation completes. Once it completes, the remainder of our **PerformDatabaseOperations*** will be executed.</span></span>
3.  <span data-ttu-id="d4bf3-155">**SaveChanges** completa</span><span class="sxs-lookup"><span data-stu-id="d4bf3-155">**SaveChanges** completes</span></span>
4.  <span data-ttu-id="d4bf3-156">Query per tutti i **blog** viene inviato al database *anche in questo caso, il thread gestito è libero di eseguire altre operazioni mentre viene elaborata la query nel database. Poiché tutte le altre esecuzione è stata completata, il thread appena arresterà alla chiamata di attesa tuttavia.*</span><span class="sxs-lookup"><span data-stu-id="d4bf3-156">Query for all **Blogs** is sent to the database *Again, the managed thread is free to do other work while the query is processed in the database. Since all other execution has completed, the thread will just halt on the Wait call though.*</span></span>
5.  <span data-ttu-id="d4bf3-157">Query restituisce e i risultati vengono scritti **Console**</span><span class="sxs-lookup"><span data-stu-id="d4bf3-157">Query returns and results are written to **Console**</span></span>

![Output asincroni](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a><span data-ttu-id="d4bf3-159">Le informazioni</span><span class="sxs-lookup"><span data-stu-id="d4bf3-159">The takeaway</span></span>

<span data-ttu-id="d4bf3-160">A questo punto, abbiamo visto come è facile apportare usare metodi asincroni di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-160">We now saw how easy it is to make use of EF’s asynchronous methods.</span></span> <span data-ttu-id="d4bf3-161">Sebbene i vantaggi delle operazioni asincrone potrebbero non essere molto evidenti con un'app console semplice, queste stesse strategie possono essere applicati in situazioni in cui le attività di lunga durata o associate alla rete potrebbe essere in caso contrario, blocco dell'applicazione, o causare un numero elevato di thread da aumentare il footprint di memoria.</span><span class="sxs-lookup"><span data-stu-id="d4bf3-161">Although the advantages of async may not be very apparent with a simple console app, these same strategies can be applied in situations where long-running or network-bound activities might otherwise block the application, or cause a large number of threads to increase the memory footprint.</span></span>
