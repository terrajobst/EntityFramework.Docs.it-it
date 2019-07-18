---
title: Query asincrona e Save-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: bf2039110962e8dd114242dcd0b9454963750774
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306589"
---
# <a name="async-query-and-save"></a><span data-ttu-id="49d00-102">Query asincrona e salvataggio</span><span class="sxs-lookup"><span data-stu-id="49d00-102">Async query and save</span></span>
> [!NOTE]
> <span data-ttu-id="49d00-103">**Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="49d00-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="49d00-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="49d00-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="49d00-105">EF6 ha introdotto il supporto per la query asincrona e il salvataggio usando le [parole chiave async e await](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) introdotte in .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="49d00-105">EF6 introduced support for asynchronous query and save using the [async and await keywords](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) that were introduced in .NET 4.5.</span></span> <span data-ttu-id="49d00-106">Sebbene non tutte le applicazioni possano trarre vantaggio da modalità asincrona, possono essere usate per migliorare la velocità di risposta dei client e la scalabilità del server quando si gestiscono attività a esecuzione prolungata, di rete o di I/O.</span><span class="sxs-lookup"><span data-stu-id="49d00-106">While not all applications may benefit from asynchrony, it can be used to improve client responsiveness and server scalability when handling long-running, network or I/O-bound tasks.</span></span>

## <a name="when-to-really-use-async"></a><span data-ttu-id="49d00-107">Quando usare realmente Async</span><span class="sxs-lookup"><span data-stu-id="49d00-107">When to really use async</span></span>

<span data-ttu-id="49d00-108">Lo scopo di questa procedura dettagliata è quello di introdurre i concetti asincroni in modo da semplificare l'osservazione della differenza tra l'esecuzione asincrona e sincrona del programma.</span><span class="sxs-lookup"><span data-stu-id="49d00-108">The purpose of this walkthrough is to introduce the async concepts in a way that makes it easy to observe the difference between asynchronous and synchronous program execution.</span></span> <span data-ttu-id="49d00-109">Questa procedura dettagliata non è stata progettata per illustrare gli scenari principali in cui la programmazione asincrona offre vantaggi.</span><span class="sxs-lookup"><span data-stu-id="49d00-109">This walkthrough is not intended to illustrate any of the key scenarios where async programming provides benefits.</span></span>

<span data-ttu-id="49d00-110">La programmazione asincrona si concentra principalmente sulla liberazione del thread gestito corrente (thread che esegue il codice .NET) per eseguire altre operazioni durante l'attesa di un'operazione che non richiede alcun tempo di calcolo da un thread gestito.</span><span class="sxs-lookup"><span data-stu-id="49d00-110">Async programming is primarily focused on freeing up the current managed thread (thread running .NET code) to do other work while it waits for an operation that does not require any compute time from a managed thread.</span></span> <span data-ttu-id="49d00-111">Ad esempio, mentre il motore di database elabora una query, non è necessario eseguire alcuna operazione dal codice .NET.</span><span class="sxs-lookup"><span data-stu-id="49d00-111">For example, whilst the database engine is processing a query there is nothing to be done by .NET code.</span></span>

<span data-ttu-id="49d00-112">Nelle applicazioni client (WinForms, WPF e così via) il thread corrente può essere usato per assicurare la velocità di risposta dell'interfaccia utente mentre viene eseguita l'operazione asincrona.</span><span class="sxs-lookup"><span data-stu-id="49d00-112">In client applications (WinForms, WPF, etc.) the current thread can be used to keep the UI responsive while the async operation is performed.</span></span> <span data-ttu-id="49d00-113">Nelle applicazioni server (ASP.NET e così via) il thread può essere usato per elaborare altre richieste in ingresso. in questo modo è possibile ridurre l'utilizzo della memoria e/o aumentare la velocità effettiva del server.</span><span class="sxs-lookup"><span data-stu-id="49d00-113">In server applications (ASP.NET etc.) the thread can be used to process other incoming requests - this can reduce memory usage and/or increase throughput of the server.</span></span>

<span data-ttu-id="49d00-114">Nella maggior parte delle applicazioni, l'uso di Async non avrà vantaggi evidenti e anche potrebbe essere dannoso.</span><span class="sxs-lookup"><span data-stu-id="49d00-114">In most applications using async will have no noticeable benefits and even could be detrimental.</span></span> <span data-ttu-id="49d00-115">Usare i test, la profilatura e il senso comune per misurare l'effetto di Async in uno scenario specifico prima di eseguire il commit.</span><span class="sxs-lookup"><span data-stu-id="49d00-115">Use tests, profiling and common sense to measure the impact of async in your particular scenario before committing to it.</span></span>

<span data-ttu-id="49d00-116">Di seguito sono riportate alcune risorse per ottenere informazioni su Async:</span><span class="sxs-lookup"><span data-stu-id="49d00-116">Here are some more resources to learn about async:</span></span>

-   [<span data-ttu-id="49d00-117">Panoramica di Brandon Bray su async/await in .NET 4,5</span><span class="sxs-lookup"><span data-stu-id="49d00-117">Brandon Bray’s overview of async/await in .NET 4.5</span></span>](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   <span data-ttu-id="49d00-118">Pagine di [programmazione asincrone](https://msdn.microsoft.com/library/hh191443.aspx) in MSDN Library</span><span class="sxs-lookup"><span data-stu-id="49d00-118">[Asynchronous Programming](https://msdn.microsoft.com/library/hh191443.aspx) pages in the MSDN Library</span></span>
-   <span data-ttu-id="49d00-119">[Come compilare applicazioni Web ASP.NET con Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (include una demo sull'aumento della velocità effettiva del server)</span><span class="sxs-lookup"><span data-stu-id="49d00-119">[How to Build ASP.NET Web Applications Using Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (includes a demo of increased server throughput)</span></span>

## <a name="create-the-model"></a><span data-ttu-id="49d00-120">Creare il modello</span><span class="sxs-lookup"><span data-stu-id="49d00-120">Create the model</span></span>

<span data-ttu-id="49d00-121">Il flusso di lavoro [Code First](~/ef6/modeling/code-first/workflows/new-database.md) verrà usato per creare il modello e generare il database. la funzionalità asincrona, tuttavia, funzionerà con tutti i modelli EF, inclusi quelli creati con la finestra di progettazione EF.</span><span class="sxs-lookup"><span data-stu-id="49d00-121">We’ll be using the [Code First workflow](~/ef6/modeling/code-first/workflows/new-database.md) to create our model and generate the database, however the asynchronous functionality will work with all EF models including those created with the EF Designer.</span></span>

-   <span data-ttu-id="49d00-122">Creare un'applicazione console e chiamarla **AsyncDemo**</span><span class="sxs-lookup"><span data-stu-id="49d00-122">Create a Console Application and call it **AsyncDemo**</span></span>
-   <span data-ttu-id="49d00-123">Aggiungere il pacchetto NuGet EntityFramework</span><span class="sxs-lookup"><span data-stu-id="49d00-123">Add the EntityFramework NuGet package</span></span>
    -   <span data-ttu-id="49d00-124">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **AsyncDemo**</span><span class="sxs-lookup"><span data-stu-id="49d00-124">In Solution Explorer, right-click on the **AsyncDemo** project</span></span>
    -   <span data-ttu-id="49d00-125">Selezionare **Gestisci pacchetti NuGet...**</span><span class="sxs-lookup"><span data-stu-id="49d00-125">Select **Manage NuGet Packages…**</span></span>
    -   <span data-ttu-id="49d00-126">Nella finestra di dialogo Gestisci pacchetti NuGet selezionare la scheda **online** e scegliere il pacchetto **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="49d00-126">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
    -   <span data-ttu-id="49d00-127">Fare clic su **Installa**</span><span class="sxs-lookup"><span data-stu-id="49d00-127">Click **Install**</span></span>
-   <span data-ttu-id="49d00-128">Aggiungere una classe **Model.cs** con l'implementazione seguente</span><span class="sxs-lookup"><span data-stu-id="49d00-128">Add a **Model.cs** class with the following implementation</span></span>

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

 

## <a name="create-a-synchronous-program"></a><span data-ttu-id="49d00-129">Creazione di un programma sincrono</span><span class="sxs-lookup"><span data-stu-id="49d00-129">Create a synchronous program</span></span>

<span data-ttu-id="49d00-130">Ora che è disponibile un modello EF, è possibile scrivere il codice che lo usa per eseguire l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="49d00-130">Now that we have an EF model, let's write some code that uses it to perform some data access.</span></span>

-   <span data-ttu-id="49d00-131">Sostituire il contenuto di **Program.cs** con il codice seguente</span><span class="sxs-lookup"><span data-stu-id="49d00-131">Replace the contents of **Program.cs** with the following code</span></span>

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
                    Console.WriteLine("Calling SaveChanges.");
                    db.SaveChanges();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="49d00-132">Questo codice chiama il metodo **PerformDatabaseOperations** che salva un nuovo **Blog** nel database e quindi recupera tutti i **Blog** dal database e li stampa nella **console**.</span><span class="sxs-lookup"><span data-stu-id="49d00-132">This code calls the **PerformDatabaseOperations** method which saves a new **Blog** to the database and then retrieves all **Blogs** from the database and prints them to the **Console**.</span></span> <span data-ttu-id="49d00-133">Al termine di questa operazione, il programma scrive un'offerta del giorno nella **console**.</span><span class="sxs-lookup"><span data-stu-id="49d00-133">After this, the program writes a quote of the day to the **Console**.</span></span>

<span data-ttu-id="49d00-134">Poiché il codice è sincrono, è possibile osservare il flusso di esecuzione seguente quando si esegue il programma:</span><span class="sxs-lookup"><span data-stu-id="49d00-134">Since the code is synchronous, we can observe the following execution flow when we run the program:</span></span>

1.  <span data-ttu-id="49d00-135">**SaveChanges** inizia a eseguire il push del nuovo **Blog** nel database</span><span class="sxs-lookup"><span data-stu-id="49d00-135">**SaveChanges** begins to push the new **Blog** to the database</span></span>
2.  <span data-ttu-id="49d00-136">**SaveChanges** completato</span><span class="sxs-lookup"><span data-stu-id="49d00-136">**SaveChanges** completes</span></span>
3.  <span data-ttu-id="49d00-137">La query per tutti i **Blog** viene inviata al database</span><span class="sxs-lookup"><span data-stu-id="49d00-137">Query for all **Blogs** is sent to the database</span></span>
4.  <span data-ttu-id="49d00-138">La query restituisce e i risultati vengono scritti nella **console**</span><span class="sxs-lookup"><span data-stu-id="49d00-138">Query returns and results are written to **Console**</span></span>
5.  <span data-ttu-id="49d00-139">La citazione del giorno viene scritta nella **console**</span><span class="sxs-lookup"><span data-stu-id="49d00-139">Quote of the day is written to **Console**</span></span>

![Output di sincronizzazione](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a><span data-ttu-id="49d00-141">Esecuzione asincrona</span><span class="sxs-lookup"><span data-stu-id="49d00-141">Making it asynchronous</span></span>

<span data-ttu-id="49d00-142">Ora che il programma è in esecuzione, è possibile iniziare a usare le nuove parole chiave async e await.</span><span class="sxs-lookup"><span data-stu-id="49d00-142">Now that we have our program up and running, we can begin making use of the new async and await keywords.</span></span> <span data-ttu-id="49d00-143">Sono state apportate le modifiche seguenti a Program.cs</span><span class="sxs-lookup"><span data-stu-id="49d00-143">We've made the following changes to Program.cs</span></span>

1.  <span data-ttu-id="49d00-144">Riga 2: L'istruzione using per lo spazio dei nomi **System. Data. Entity** fornisce l'accesso ai metodi di estensione EF Async.</span><span class="sxs-lookup"><span data-stu-id="49d00-144">Line 2: The using statement for the **System.Data.Entity** namespace gives us access to the EF async extension methods.</span></span>
2.  <span data-ttu-id="49d00-145">Riga 4: L'istruzione using per lo spazio dei nomi **System. Threading. Tasks** consente di usare il tipo di **attività** .</span><span class="sxs-lookup"><span data-stu-id="49d00-145">Line 4: The using statement for the **System.Threading.Tasks** namespace allows us to use the **Task** type.</span></span>
3.  <span data-ttu-id="49d00-146">Riga 12 & 18: Si sta acquisendo come attività che monitora lo stato di avanzamento di **PerformSomeDatabaseOperations** (riga 12) e quindi blocca l'esecuzione del programma per completare questa attività dopo che tutto il lavoro per il programma è stato eseguito (riga 18).</span><span class="sxs-lookup"><span data-stu-id="49d00-146">Line 12 & 18: We are capturing as task that monitors the progress of **PerformSomeDatabaseOperations** (line 12) and then blocking program execution for this task to complete once all the work for the program is done (line 18).</span></span>
4.  <span data-ttu-id="49d00-147">Riga 25: È stato aggiornato **PerformSomeDatabaseOperations** in modo che sia contrassegnato come **Async** e venga restituita un' **attività**.</span><span class="sxs-lookup"><span data-stu-id="49d00-147">Line 25: We've update **PerformSomeDatabaseOperations** to be marked as **async** and return a **Task**.</span></span>
5.  <span data-ttu-id="49d00-148">Riga 35: Ora viene chiamata la versione asincrona di SaveChanges e in attesa del completamento.</span><span class="sxs-lookup"><span data-stu-id="49d00-148">Line 35: We're now calling the Async version of SaveChanges and awaiting it's completion.</span></span>
6.  <span data-ttu-id="49d00-149">Riga 42: Ora viene chiamata la versione asincrona di ToList e in attesa del risultato.</span><span class="sxs-lookup"><span data-stu-id="49d00-149">Line 42: We're now calling the Async version of ToList and awaiting on the result.</span></span>

<span data-ttu-id="49d00-150">Per un elenco completo dei metodi di estensione disponibili nello spazio dei nomi System. Data. Entity, vedere la classe QueryableExtensions.</span><span class="sxs-lookup"><span data-stu-id="49d00-150">For a comprehensive list of available extension methods in the System.Data.Entity namespace, refer to the QueryableExtensions class.</span></span> <span data-ttu-id="49d00-151">*Sarà inoltre necessario aggiungere "using System. Data. Entity" alle istruzioni using.*</span><span class="sxs-lookup"><span data-stu-id="49d00-151">*You’ll also need to add “using System.Data.Entity” to your using statements.*</span></span>

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

<span data-ttu-id="49d00-152">Ora che il codice è asincrono, è possibile osservare un flusso di esecuzione diverso durante l'esecuzione del programma:</span><span class="sxs-lookup"><span data-stu-id="49d00-152">Now that the code is asynchronous, we can observe a different execution flow when we run the program:</span></span>

1.  <span data-ttu-id="49d00-153">**SaveChanges** inizia a eseguire il push del nuovo **Blog** nel *database una volta che il comando viene inviato al database, non è necessario più tempo di calcolo nel thread gestito corrente. Il metodo **PerformDatabaseOperations** restituisce (anche se l'esecuzione non è terminata) e il flusso di programma nel metodo Main continua.*</span><span class="sxs-lookup"><span data-stu-id="49d00-153">**SaveChanges** begins to push the new **Blog** to the database *Once the command is sent to the database no more compute time is needed on the current managed thread. The **PerformDatabaseOperations** method returns (even though it hasn't finished executing) and program flow in the Main method continues.*</span></span>
2.  <span data-ttu-id="49d00-154">**La citazione del giorno viene scritta nella console**
    *perché non ci sono altre operazioni da eseguire nel metodo Main, il thread gestito viene bloccato nella chiamata wait fino al completamento dell'operazione del database. Al termine, verrà eseguito il resto del **PerformDatabaseOperations** .*</span><span class="sxs-lookup"><span data-stu-id="49d00-154">**Quote of the day is written to Console**
*Since there is no more work to do in the Main method, the managed thread is blocked on the Wait call until the database operation completes. Once it completes, the remainder of our **PerformDatabaseOperations** will be executed.*</span></span>
3.  <span data-ttu-id="49d00-155">**SaveChanges** completato</span><span class="sxs-lookup"><span data-stu-id="49d00-155">**SaveChanges** completes</span></span>
4.  <span data-ttu-id="49d00-156">La query per tutti i **Blog** viene inviata nuovamente *al database, mentre il thread gestito è libero di eseguire altre operazioni mentre la query viene elaborata nel database. Poiché tutte le altre esecuzioni sono state completate, il thread si interromperà solo sulla chiamata di attesa.*</span><span class="sxs-lookup"><span data-stu-id="49d00-156">Query for all **Blogs** is sent to the database *Again, the managed thread is free to do other work while the query is processed in the database. Since all other execution has completed, the thread will just halt on the Wait call though.*</span></span>
5.  <span data-ttu-id="49d00-157">La query restituisce e i risultati vengono scritti nella **console**</span><span class="sxs-lookup"><span data-stu-id="49d00-157">Query returns and results are written to **Console**</span></span>

![Output asincrono](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a><span data-ttu-id="49d00-159">Da asporto</span><span class="sxs-lookup"><span data-stu-id="49d00-159">The takeaway</span></span>

<span data-ttu-id="49d00-160">Ora abbiamo visto quanto sia facile usare i metodi asincroni di EF.</span><span class="sxs-lookup"><span data-stu-id="49d00-160">We now saw how easy it is to make use of EF’s asynchronous methods.</span></span> <span data-ttu-id="49d00-161">Sebbene i vantaggi di Async potrebbero non essere molto evidenti con una semplice app console, è possibile applicare queste stesse strategie in situazioni in cui le attività a esecuzione prolungata o in rete potrebbero bloccare l'applicazione o causare un numero elevato di thread aumentare il footprint di memoria.</span><span class="sxs-lookup"><span data-stu-id="49d00-161">Although the advantages of async may not be very apparent with a simple console app, these same strategies can be applied in situations where long-running or network-bound activities might otherwise block the application, or cause a large number of threads to increase the memory footprint.</span></span>
