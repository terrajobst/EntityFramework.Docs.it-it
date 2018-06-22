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
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="ab70f-102">Introduzione a EF Core in .NET Framework con un nuovo database</span><span class="sxs-lookup"><span data-stu-id="ab70f-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="ab70f-103">In questa procedura dettagliata verrà compilata un'applicazione console che esegue l'accesso ai dati di base in un database Microsoft SQL Server usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ab70f-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="ab70f-104">Si useranno le migrazioni per creare il database dal modello.</span><span class="sxs-lookup"><span data-stu-id="ab70f-104">You will use migrations to create the database from your model.</span></span>

> [!TIP]  
> <span data-ttu-id="ab70f-105">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="ab70f-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ab70f-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="ab70f-106">Prerequisites</span></span>

<span data-ttu-id="ab70f-107">Per completare la procedura dettagliata, sono necessari i prerequisiti seguenti:</span><span class="sxs-lookup"><span data-stu-id="ab70f-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* [<span data-ttu-id="ab70f-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="ab70f-108">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)

* [<span data-ttu-id="ab70f-109">Versione più recente di Gestione pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="ab70f-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="ab70f-110">Versione più recente di Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="ab70f-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a><span data-ttu-id="ab70f-111">Creare un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="ab70f-111">Create a new project</span></span>

* <span data-ttu-id="ab70f-112">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ab70f-112">Open Visual Studio</span></span>

* <span data-ttu-id="ab70f-113">File > Nuovo > Progetto</span><span class="sxs-lookup"><span data-stu-id="ab70f-113">File > New > Project...</span></span>

* <span data-ttu-id="ab70f-114">Scegliere Modelli > Visual C# > Desktop classico di Windows dal menu a sinistra</span><span class="sxs-lookup"><span data-stu-id="ab70f-114">From the left menu select Templates > Visual C# > Windows Classic Desktop</span></span>

* <span data-ttu-id="ab70f-115">Selezionare il modello di progetto **App console (.NET Framework)**</span><span class="sxs-lookup"><span data-stu-id="ab70f-115">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="ab70f-116">Assicurarsi di specificare come destinazione **.NET Framework 4.5.1** o versione successiva</span><span class="sxs-lookup"><span data-stu-id="ab70f-116">Ensure you are targeting **.NET Framework 4.5.1** or later</span></span>

* <span data-ttu-id="ab70f-117">Specificare un nome per il progetto e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="ab70f-117">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="ab70f-118">Installare Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ab70f-118">Install Entity Framework</span></span>

<span data-ttu-id="ab70f-119">Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione.</span><span class="sxs-lookup"><span data-stu-id="ab70f-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="ab70f-120">Questa procedura dettagliata usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ab70f-120">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="ab70f-121">Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="ab70f-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="ab70f-122">Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti</span><span class="sxs-lookup"><span data-stu-id="ab70f-122">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="ab70f-123">Eseguire `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="ab70f-123">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="ab70f-124">Più avanti in questa procedura dettagliata verrà anche usato Entity Framework Tools per gestire il database,</span><span class="sxs-lookup"><span data-stu-id="ab70f-124">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="ab70f-125">quindi verrà anche installato il pacchetto di strumenti.</span><span class="sxs-lookup"><span data-stu-id="ab70f-125">So we will install the tools package as well.</span></span>

* <span data-ttu-id="ab70f-126">Eseguire `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="ab70f-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="ab70f-127">Creare il modello</span><span class="sxs-lookup"><span data-stu-id="ab70f-127">Create your model</span></span>

<span data-ttu-id="ab70f-128">È ora possibile definire un contesto e le classi di entità che costituiscono il modello.</span><span class="sxs-lookup"><span data-stu-id="ab70f-128">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="ab70f-129">Progetto > Aggiungi classe</span><span class="sxs-lookup"><span data-stu-id="ab70f-129">Project > Add Class...</span></span>

* <span data-ttu-id="ab70f-130">Immettere *Model.cs* come nome e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="ab70f-130">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="ab70f-131">Sostituire tutti i contenuti del file con il codice seguente</span><span class="sxs-lookup"><span data-stu-id="ab70f-131">Replace the contents of the file with the following code</span></span>

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
> <span data-ttu-id="ab70f-132">In una vera applicazione si inserisce ogni classe in un file separato e la stringa di connessione nel file `App.Config` e la si legge usando `ConfigurationManager`.</span><span class="sxs-lookup"><span data-stu-id="ab70f-132">In a real application you would put each class in a separate file and put the connection string in the `App.Config` file and read it out using `ConfigurationManager`.</span></span> <span data-ttu-id="ab70f-133">Per ragioni di semplicità, in questa esercitazione viene inserito tutto in un solo file.</span><span class="sxs-lookup"><span data-stu-id="ab70f-133">For the sake of simplicity, we are putting everything in a single code file for this tutorial.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="ab70f-134">Creare il database</span><span class="sxs-lookup"><span data-stu-id="ab70f-134">Create your database</span></span>

<span data-ttu-id="ab70f-135">Dopo avere creato un modello, è possibile usare le migrazioni per creare un database.</span><span class="sxs-lookup"><span data-stu-id="ab70f-135">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="ab70f-136">Strumenti –> Gestione pacchetti NuGet –> Console di Gestione pacchetti</span><span class="sxs-lookup"><span data-stu-id="ab70f-136">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="ab70f-137">Eseguire `Add-Migration MyFirstMigration` per effettuare lo scaffolding di una migrazione per poter creare il set iniziale di tabelle per il modello.</span><span class="sxs-lookup"><span data-stu-id="ab70f-137">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

* <span data-ttu-id="ab70f-138">Eseguire `Update-Database` per applicare la nuova migrazione al database.</span><span class="sxs-lookup"><span data-stu-id="ab70f-138">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="ab70f-139">Poiché il database non esiste ancora, verrà creato automaticamente prima di applicare la migrazione.</span><span class="sxs-lookup"><span data-stu-id="ab70f-139">Because your database doesn't exist yet, it will be created for you before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="ab70f-140">Se si apporteranno modifiche al modello in futuro, sarà possibile usare il comando `Add-Migration` per eseguire lo scaffolding di una nuova migrazione e poter apportare le corrispondenti modifiche dello schema al database.</span><span class="sxs-lookup"><span data-stu-id="ab70f-140">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="ab70f-141">Dopo avere controllato il codice di scaffolding e avere apportato eventuali modifiche necessarie, è possibile usare il comando `Update-Database` per applicare le modifiche al database.</span><span class="sxs-lookup"><span data-stu-id="ab70f-141">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
><span data-ttu-id="ab70f-142">EF usa una tabella `__EFMigrationsHistory` nel database per tenere traccia delle migrazioni che sono già state applicate al database.</span><span class="sxs-lookup"><span data-stu-id="ab70f-142">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="ab70f-143">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="ab70f-143">Use your model</span></span>

<span data-ttu-id="ab70f-144">È ora possibile usare il modello per eseguire l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="ab70f-144">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="ab70f-145">Aprire *Program.cs*</span><span class="sxs-lookup"><span data-stu-id="ab70f-145">Open *Program.cs*</span></span>

* <span data-ttu-id="ab70f-146">Sostituire tutti i contenuti del file con il codice seguente</span><span class="sxs-lookup"><span data-stu-id="ab70f-146">Replace the contents of the file with the following code</span></span>

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

* <span data-ttu-id="ab70f-147">Debug > Avvia senza eseguire debug</span><span class="sxs-lookup"><span data-stu-id="ab70f-147">Debug > Start Without Debugging</span></span>

<span data-ttu-id="ab70f-148">Si noterà che un blog è salvato nel database e quindi che i dettagli di tutti i blog vengono visualizzati nella console.</span><span class="sxs-lookup"><span data-stu-id="ab70f-148">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![immagine](_static/output-new-db.png)
