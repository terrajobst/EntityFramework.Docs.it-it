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
# <a name="async-query-and-save"></a>Asincrono di query e salvare
> [!NOTE]
> **Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.

Entity Framework 6 ha introdotto il supporto per query asincrona e il salvataggio con il [async e await parole chiave](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) che sono state introdotte in .NET 4.5. Sebbene non tutte le applicazioni possono trarre vantaggio dalla modalità asincrona, può essere utilizzato per migliorare la scalabilità della velocità di risposta e server client durante la gestione con esecuzione prolungata, rete o I/O-limite attività.

## <a name="when-to-really-use-async"></a>Quando utilizzare davvero async

Lo scopo di questa procedura dettagliata è presentare i concetti di async in modo che rende più semplice osservare la differenza tra l'esecuzione del programma sincrone e asincrone. Questa procedura dettagliata non può illustrare gli scenari principali in cui la programmazione asincrona fornisce vantaggi.

Programmazione asincrona è incentrata principalmente sugli liberare il thread gestito corrente (codice .NET in esecuzione thread) di eseguire altre attività durante l'attesa di un'operazione che non richiede alcun tempo di calcolo da un thread gestito. Ad esempio, durante il motore di database è l'elaborazione di una query non esegue alcuna operazione da eseguire dal codice .NET.

Nelle applicazioni client (WinForms, WPF e così via), il thread corrente è utilizzabile per mantenere reattiva l'interfaccia utente mentre viene eseguita l'operazione asincrona. Nelle applicazioni server (ASP.NET e così via) al thread può essere utilizzato per elaborare altre richieste in ingresso - ciò può ridurre l'utilizzo della memoria e/o aumentare la velocità effettiva del server.

Nella maggior parte delle applicazioni di uso della funzionalità async non avranno alcun vantaggio evidente e potrebbe anche essere negativo. Usare i test, profilatura e buon senso per misurare l'impatto delle operazioni asincrone in un determinato scenario prima di eseguire il commit a esso.

Ecco alcune altre risorse per apprendere async:

-   [Panoramica del Brandon Bray di async/await in .NET 4.5](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   [Programmazione asincrona](https://msdn.microsoft.com/library/hh191443.aspx) pagine in MSDN Library
-   [Come compilare ASP.NET Web Applications tramite Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (che include una dimostrazione di velocità effettiva maggiore server)

## <a name="create-the-model"></a>Creare il modello

Verrà usato il [flusso di lavoro di Code First](~/ef6/modeling/code-first/workflows/new-database.md) per creare il nostro modello e generare il database, tuttavia, la funzionalità asincrona funzionerà con tutti i modelli di Entity Framework, inclusi quelli creati con la finestra di progettazione di Entity Framework.

-   Creare un'applicazione Console e chiamarlo **AsyncDemo**
-   Aggiungere il pacchetto EntityFramework NuGet
    -   In Esplora soluzioni fare clic sui **AsyncDemo** progetto
    -   Selezionare **Gestisci pacchetti NuGet...**
    -   Nella finestra di dialogo Gestisci pacchetti NuGet, selezionare la **Online** scheda e scegliere il **EntityFramework** pacchetto
    -   Fare clic su **installare**
-   Aggiungere un **Model.cs** classe con l'implementazione seguente

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

 

## <a name="create-a-synchronous-program"></a>Creare un programma sincrono

Ora che abbiamo un modello di EF, è possibile scrivere codice che lo usa per eseguire alcune l'accesso ai dati.

-   Sostituire il contenuto del **Program.cs** con il codice seguente

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

Questo codice chiama il **PerformDatabaseOperations** metodo che consente di salvare un nuovo **Blog** per il database, quindi recupera tutti i **blog** dal database e li visualizza per il **Console**. Al termine, il programma scrive un'offerta del giorno per il **Console**.

Poiché il codice sincrono, è possibile osservare il seguente flusso di esecuzione quando si esegue il programma:

1.  **SaveChanges** inizia a eseguire il push di nuove **Blog** al database
2.  **SaveChanges** completa
3.  Query per tutti i **blog** viene inviato al database
4.  Query restituisce e i risultati vengono scritti **Console**
5.  Offerta del giorno verrà scritti in **Console**

![Uscita sincronizzazione](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a>Rendendo asincrona

Ora che abbiamo il nostro programma sia operativa, è possibile iniziare ad apportarvi usare di nuovo async e await parole chiave. Sono state apportate le modifiche seguenti al file Program.cs

1.  La riga 2: L'uso istruzione per il **Data. Entity** dello spazio dei nomi fornisce l'accesso ai metodi di estensione asincrono Entity Framework.
2.  Riga 4: L'uso istruzione per il **Tasks** dello spazio dei nomi consente di utilizzare il **attività** tipo.
3.  Riga 12 & 18: vengono acquisiti come attività che monitori l'avanzamento della **PerformSomeDatabaseOperations** (riga 12) e quindi blocca l'esecuzione del programma per questa attività a una volta completata tutto il lavoro per il programma viene creato (riga 18).
4.  Riga 25: Abbiamo update **PerformSomeDatabaseOperations** essere contrassegnato come **async** e restituire un **attività**.
5.  Riga 35: È ora stiamo chiamando la versione asincrona di SaveChanges e in attesa del relativo completamento.
6.  Riga 42: È ora stiamo chiamando la versione asincrona di ToList e in attesa del risultato.

Per un elenco completo dei metodi di estensione disponibile nello spazio dei nomi di Data. Entity, fare riferimento alla classe QueryableExtensions. *È anche necessario aggiungere "using Data. Entity" al usando le istruzioni.*

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

Ora che il codice è asincrono, è possibile osservare un flusso di esecuzione diverso quando si esegue il programma:

1.  **SaveChanges** inizia a eseguire il push di nuove **Blog** al database *dopo il comando viene inviato al database non più di calcolo è necessario tempo sul thread gestito corrente. Il **PerformDatabaseOperations** metodo viene restituito (anche se non è stata completata l'esecuzione) e continua il flusso di programma nel metodo Main.*
2.  **Offerta del giorno Console verrà scritti**
    *perché è presente non altre operazioni da eseguire nel metodo Main, il thread gestito è bloccato in attesa chiamare fino al completamento dell'operazione sul database. Al termine, la parte restante del nostro **PerformDatabaseOperations** * verrà eseguito.
3.  **SaveChanges** completa
4.  Query per tutti i **blog** viene inviato al database *anche in questo caso, il thread gestito è libero di eseguire altre operazioni mentre viene elaborata la query nel database. Poiché tutte le altre esecuzione è stata completata, il thread appena arresterà alla chiamata di attesa tuttavia.*
5.  Query restituisce e i risultati vengono scritti **Console**

![Output asincroni](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a>Le informazioni

A questo punto, abbiamo visto come è facile apportare usare metodi asincroni di Entity Framework. Sebbene i vantaggi delle operazioni asincrone potrebbero non essere molto evidenti con un'app console semplice, queste stesse strategie possono essere applicati in situazioni in cui le attività di lunga durata o associate alla rete potrebbe essere in caso contrario, blocco dell'applicazione, o causare un numero elevato di thread da aumentare il footprint di memoria.
