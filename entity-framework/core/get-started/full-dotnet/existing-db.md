---
title: Introduzione a .NET Framework - Database esistente - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 3cd69109e3cf8dbc103f9eea6e2553df17f29a98
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812625"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>Introduzione a EF Core in .NET Framework con un database esistente

In questa procedura dettagliata verrà compilata un'applicazione console che esegue l'accesso ai dati di base in un database Microsoft SQL Server usando Entity Framework. Si userà il reverse engineering per creare un modello di Entity Framework basato su un database esistente.

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) di questo articolo in GitHub.

## <a name="prerequisites"></a>Prerequisiti

Per completare la procedura dettagliata, sono necessari i prerequisiti seguenti:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)

* [Versione più recente di Gestione pacchetti NuGet](https://dist.nuget.org/index.html)

* [Versione più recente di Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [Database Blogging](#blogging-database)

### <a name="blogging-database"></a>Database Blogging

Questa esercitazione usa un database **Blogging** nell'istanza di LocalDb come database esistente.

> [!TIP]  
> Se si è già creato il database **Blogging** durante un'altra esercitazione, è possibile ignorare questi passaggi.

* Aprire Visual Studio

* Strumenti > Connetti al database

* Selezionare **Microsoft SQL Server** e fare clic su **Continua**

* Immettere **(localdb)\mssqllocaldb** in **Nome server**

* Immettere **master** in **Nome database** e fare clic su **OK**

* Il database master viene ora visualizzato sotto **Connessioni dati** in **Esplora server**

* Fare clic con il pulsante destro del mouse sul database in **Esplora server** e scegliere **Nuova query**

* Copiare lo script, riportato sotto, nell'editor di query

* Fare clic con il pulsante destro del mouse sull'editor di query e scegliere **Esegui**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Creare un nuovo progetto

* Aprire Visual Studio

* File > Nuovo > Progetto

* Scegliere Modelli > Visual C# > Windows dal menu a sinistra

* Selezionare il modello di progetto **Applicazione console**

* Assicurarsi di specificare come destinazione **.NET Framework 4.5.1** o versione successiva

* Specificare un nome per il progetto e fare clic su **OK**

## <a name="install-entity-framework"></a>Installare Entity Framework

Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione. Questa procedura dettagliata usa SQL Server. Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).

* Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti

* Eseguire `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Per abilitare il reverse engineering da un database esistente, è necessario installare altri due pacchetti.

* Eseguire `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="reverse-engineer-your-model"></a>Decompilare il modello

È ora possibile creare il modello di EF basato sul database esistente.

* Strumenti –> Gestione pacchetti NuGet –> Console di Gestione pacchetti

* Eseguire il comando seguente per creare un modello dal database esistente

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

Il processo di reverse engineering ha creato le classi di entità e un contesto derivato in base allo schema del database esistente. Le classi di entità sono semplici oggetti C# che rappresentano i dati di cui verranno eseguite query e che verranno salvati.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)] -->
``` csharp
using System;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class Blog
    {
        public Blog()
        {
            Post = new HashSet<Post>();
        }

        public int BlogId { get; set; }
        public string Url { get; set; }

        public virtual ICollection<Post> Post { get; set; }
    }
}
```

Il contesto rappresenta una sessione con il database e consente di eseguire query delle istanze delle classi di entità e di salvarle.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class BloggingContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>(entity =>
            {
                entity.Property(e => e.Url).IsRequired();
            });

            modelBuilder.Entity<Post>(entity =>
            {
                entity.HasOne(d => d.Blog)
                    .WithMany(p => p.Post)
                    .HasForeignKey(d => d.BlogId);
            });
        }

        public virtual DbSet<Blog> Blog { get; set; }
        public virtual DbSet<Post> Post { get; set; }
    }
}
```

## <a name="use-your-model"></a>Usare il modello

È ora possibile usare il modello per eseguire l'accesso ai dati.

* Aprire *Program.cs*

* Sostituire tutti i contenuti del file con il codice seguente

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blog.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blog)
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

![immagine](_static/output-existing-db.png)
