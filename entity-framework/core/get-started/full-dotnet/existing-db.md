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
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="24df2-102">Introduzione a EF Core in .NET Framework con un database esistente</span><span class="sxs-lookup"><span data-stu-id="24df2-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="24df2-103">In questa procedura dettagliata verrà compilata un'applicazione console che esegue l'accesso ai dati di base in un database Microsoft SQL Server usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="24df2-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="24df2-104">Si userà il reverse engineering per creare un modello di Entity Framework basato su un database esistente.</span><span class="sxs-lookup"><span data-stu-id="24df2-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="24df2-105">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="24df2-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="24df2-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="24df2-106">Prerequisites</span></span>

<span data-ttu-id="24df2-107">Per completare la procedura dettagliata, sono necessari i prerequisiti seguenti:</span><span class="sxs-lookup"><span data-stu-id="24df2-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* [<span data-ttu-id="24df2-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="24df2-108">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)

* [<span data-ttu-id="24df2-109">Versione più recente di Gestione pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="24df2-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="24df2-110">Versione più recente di Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="24df2-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [<span data-ttu-id="24df2-111">Database Blogging</span><span class="sxs-lookup"><span data-stu-id="24df2-111">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="24df2-112">Database Blogging</span><span class="sxs-lookup"><span data-stu-id="24df2-112">Blogging database</span></span>

<span data-ttu-id="24df2-113">Questa esercitazione usa un database **Blogging** nell'istanza di LocalDb come database esistente.</span><span class="sxs-lookup"><span data-stu-id="24df2-113">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="24df2-114">Se si è già creato il database **Blogging** durante un'altra esercitazione, è possibile ignorare questi passaggi.</span><span class="sxs-lookup"><span data-stu-id="24df2-114">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="24df2-115">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24df2-115">Open Visual Studio</span></span>

* <span data-ttu-id="24df2-116">Strumenti > Connetti al database</span><span class="sxs-lookup"><span data-stu-id="24df2-116">Tools > Connect to Database...</span></span>

* <span data-ttu-id="24df2-117">Selezionare **Microsoft SQL Server** e fare clic su **Continua**</span><span class="sxs-lookup"><span data-stu-id="24df2-117">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="24df2-118">Immettere **(localdb)\mssqllocaldb** in **Nome server**</span><span class="sxs-lookup"><span data-stu-id="24df2-118">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="24df2-119">Immettere **master** in **Nome database** e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="24df2-119">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="24df2-120">Il database master viene ora visualizzato sotto **Connessioni dati** in **Esplora server**</span><span class="sxs-lookup"><span data-stu-id="24df2-120">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="24df2-121">Fare clic con il pulsante destro del mouse sul database in **Esplora server** e scegliere **Nuova query**</span><span class="sxs-lookup"><span data-stu-id="24df2-121">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="24df2-122">Copiare lo script, riportato sotto, nell'editor di query</span><span class="sxs-lookup"><span data-stu-id="24df2-122">Copy the script, listed below, into the query editor</span></span>

* <span data-ttu-id="24df2-123">Fare clic con il pulsante destro del mouse sull'editor di query e scegliere **Esegui**</span><span class="sxs-lookup"><span data-stu-id="24df2-123">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="24df2-124">Creare un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="24df2-124">Create a new project</span></span>

* <span data-ttu-id="24df2-125">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24df2-125">Open Visual Studio</span></span>

* <span data-ttu-id="24df2-126">File > Nuovo > Progetto</span><span class="sxs-lookup"><span data-stu-id="24df2-126">File > New > Project...</span></span>

* <span data-ttu-id="24df2-127">Scegliere Modelli > Visual C# > Windows dal menu a sinistra</span><span class="sxs-lookup"><span data-stu-id="24df2-127">From the left menu select Templates > Visual C# > Windows</span></span>

* <span data-ttu-id="24df2-128">Selezionare il modello di progetto **Applicazione console**</span><span class="sxs-lookup"><span data-stu-id="24df2-128">Select the **Console Application** project template</span></span>

* <span data-ttu-id="24df2-129">Assicurarsi di specificare come destinazione **.NET Framework 4.5.1** o versione successiva</span><span class="sxs-lookup"><span data-stu-id="24df2-129">Ensure you are targeting **.NET Framework 4.5.1** or later</span></span>

* <span data-ttu-id="24df2-130">Specificare un nome per il progetto e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="24df2-130">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="24df2-131">Installare Entity Framework</span><span class="sxs-lookup"><span data-stu-id="24df2-131">Install Entity Framework</span></span>

<span data-ttu-id="24df2-132">Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione.</span><span class="sxs-lookup"><span data-stu-id="24df2-132">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="24df2-133">Questa procedura dettagliata usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="24df2-133">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="24df2-134">Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="24df2-134">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="24df2-135">Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti</span><span class="sxs-lookup"><span data-stu-id="24df2-135">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="24df2-136">Eseguire `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="24df2-136">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="24df2-137">Per abilitare il reverse engineering da un database esistente, è necessario installare altri due pacchetti.</span><span class="sxs-lookup"><span data-stu-id="24df2-137">To enable reverse engineering from an existing database we need to install a couple of other packages too.</span></span>

* <span data-ttu-id="24df2-138">Eseguire `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="24df2-138">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="24df2-139">Decompilare il modello</span><span class="sxs-lookup"><span data-stu-id="24df2-139">Reverse engineer your model</span></span>

<span data-ttu-id="24df2-140">È ora possibile creare il modello di EF basato sul database esistente.</span><span class="sxs-lookup"><span data-stu-id="24df2-140">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="24df2-141">Strumenti –> Gestione pacchetti NuGet –> Console di Gestione pacchetti</span><span class="sxs-lookup"><span data-stu-id="24df2-141">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="24df2-142">Eseguire il comando seguente per creare un modello dal database esistente</span><span class="sxs-lookup"><span data-stu-id="24df2-142">Run the following command to create a model from the existing database</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="24df2-143">Il processo di reverse engineering ha creato le classi di entità e un contesto derivato in base allo schema del database esistente.</span><span class="sxs-lookup"><span data-stu-id="24df2-143">The reverse engineer process created entity classes and a derived context based on the schema of the existing database.</span></span> <span data-ttu-id="24df2-144">Le classi di entità sono semplici oggetti C# che rappresentano i dati di cui verranno eseguite query e che verranno salvati.</span><span class="sxs-lookup"><span data-stu-id="24df2-144">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

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

<span data-ttu-id="24df2-145">Il contesto rappresenta una sessione con il database e consente di eseguire query delle istanze delle classi di entità e di salvarle.</span><span class="sxs-lookup"><span data-stu-id="24df2-145">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="use-your-model"></a><span data-ttu-id="24df2-146">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="24df2-146">Use your model</span></span>

<span data-ttu-id="24df2-147">È ora possibile usare il modello per eseguire l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="24df2-147">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="24df2-148">Aprire *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="24df2-148">Open *Program.cs*</span></span>

* <span data-ttu-id="24df2-149">Sostituire tutti i contenuti del file con il codice seguente</span><span class="sxs-lookup"><span data-stu-id="24df2-149">Replace the contents of the file with the following code</span></span>

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

* <span data-ttu-id="24df2-150">Debug > Avvia senza eseguire debug</span><span class="sxs-lookup"><span data-stu-id="24df2-150">Debug > Start Without Debugging</span></span>

<span data-ttu-id="24df2-151">Si noterà che un blog è salvato nel database e quindi che i dettagli di tutti i blog vengono visualizzati nella console.</span><span class="sxs-lookup"><span data-stu-id="24df2-151">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![immagine](_static/output-existing-db.png)
