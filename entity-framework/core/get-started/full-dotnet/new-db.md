---
title: Introduzione a .NET Framework - Nuovo database - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: bd7054c6834ae11bfdc352d63654e4304771e432
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812521"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>Introduzione a EF Core in .NET Framework con un nuovo database

In questa procedura dettagliata verrà compilata un'applicazione console che esegue l'accesso ai dati di base in un database Microsoft SQL Server usando Entity Framework. Si useranno le migrazioni per creare il database dal modello.

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) di questo articolo in GitHub.

## <a name="prerequisites"></a>Prerequisiti

Per completare la procedura dettagliata, sono necessari i prerequisiti seguenti:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)

* [Versione più recente di Gestione pacchetti NuGet](https://dist.nuget.org/index.html)

* [Versione più recente di Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a>Creare un nuovo progetto

* Aprire Visual Studio

* File > Nuovo > Progetto

* Scegliere Modelli > Visual C# > Desktop classico di Windows dal menu a sinistra

* Selezionare il modello di progetto **App console (.NET Framework)**

* Assicurarsi di specificare come destinazione **.NET Framework 4.5.1** o versione successiva

* Specificare un nome per il progetto e fare clic su **OK**

## <a name="install-entity-framework"></a>Installare Entity Framework

Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione. Questa procedura dettagliata usa SQL Server. Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).

* Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti

* Eseguire `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Più avanti in questa procedura dettagliata verrà anche usato Entity Framework Tools per gestire il database, quindi verrà anche installato il pacchetto di strumenti.

* Eseguire `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="create-your-model"></a>Creare il modello

È ora possibile definire un contesto e le classi di entità che costituiscono il modello.

* Progetto > Aggiungi classe

* Immettere *Model.cs* come nome e fare clic su **OK**

* Sostituire tutti i contenuti del file con il codice seguente

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }

        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

> [!TIP]  
> In una vera applicazione si inserisce ogni classe in un file separato e la stringa di connessione nel file `App.Config` e la si legge usando `ConfigurationManager`. Per ragioni di semplicità, in questa esercitazione viene inserito tutto in un solo file.

## <a name="create-your-database"></a>Creare il database

Dopo avere creato un modello, è possibile usare le migrazioni per creare un database.

* Strumenti –> Gestione pacchetti NuGet –> Console di Gestione pacchetti

* Eseguire `Add-Migration MyFirstMigration` per effettuare lo scaffolding di una migrazione per poter creare il set iniziale di tabelle per il modello.

* Eseguire `Update-Database` per applicare la nuova migrazione al database. Poiché il database non esiste ancora, verrà creato automaticamente prima di applicare la migrazione.

> [!TIP]  
> Se si apporteranno modifiche al modello in futuro, sarà possibile usare il comando `Add-Migration` per eseguire lo scaffolding di una nuova migrazione e poter apportare le corrispondenti modifiche dello schema al database. Dopo avere controllato il codice di scaffolding e avere apportato eventuali modifiche necessarie, è possibile usare il comando `Update-Database` per applicare le modifiche al database.
>
>EF usa una tabella `__EFMigrationsHistory` nel database per tenere traccia delle migrazioni che sono già state applicate al database.

## <a name="use-your-model"></a>Usare il modello

È ora possibile usare il modello per eseguire l'accesso ai dati.

* Aprire *Program.cs*

* Sostituire tutti i contenuti del file con il codice seguente

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blogs)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* Debug > Avvia senza eseguire debug

Si noterà che un blog è salvato nel database e quindi che i dettagli di tutti i blog vengono visualizzati nella console.

![immagine](_static/output-new-db.png)
