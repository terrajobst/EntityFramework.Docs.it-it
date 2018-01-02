---
title: Breve panoramica - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: 13de9cf98111b8e253e073c591fcec04206b4c4f
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
# <a name="entity-framework-core-quick-overview"></a><span data-ttu-id="c0d02-102">Breve panoramica di Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c0d02-102">Entity Framework Core Quick Overview</span></span>

<span data-ttu-id="c0d02-103">Entity Framework (EF) Core è una versione semplice, estendibile e multipiattaforma della tecnologia di accesso dati Entity Framework più diffusa.</span><span class="sxs-lookup"><span data-stu-id="c0d02-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="c0d02-104">EF Core è un mapper relazionale a oggetti che consente agli sviluppatori di .NET di usare un database con gli oggetti .NET.</span><span class="sxs-lookup"><span data-stu-id="c0d02-104">EF Core is an object-relational mapper (O/RM) that enables .NET developers to work with a database using .NET objects.</span></span> <span data-ttu-id="c0d02-105">In questo modo la maggior parte del codice di accesso ai dati che in genere gli sviluppatori devono scrivere non è più necessaria.</span><span class="sxs-lookup"><span data-stu-id="c0d02-105">It eliminates the need for most of the data-access code that developers usually need to write.</span></span> <span data-ttu-id="c0d02-106">EF Core supporta molti motori di database. Per informazioni dettagliate, vedere [Provider di Database](providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="c0d02-106">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

<span data-ttu-id="c0d02-107">Per un apprendimento per scrittura di codice, vedere una delle guide [Introduzione](get-started/index.md) per iniziare con Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c0d02-107">If you like to learn by writing code, we'd recommend one of our [Getting Started](get-started/index.md) guides to get you started with EF Core.</span></span>

## <a name="latest-version-ef-core-20"></a><span data-ttu-id="c0d02-108">Ultima versione: EF Core 2.0</span><span class="sxs-lookup"><span data-stu-id="c0d02-108">Latest version: EF Core 2.0</span></span>

<span data-ttu-id="c0d02-109">Se si ha familiarità con Entity Framework Core e si vuole passare direttamente ai dettagli della nuova versione:</span><span class="sxs-lookup"><span data-stu-id="c0d02-109">If you are familiar with EF Core and want to jump straight into the details of the new version:</span></span>

- <span data-ttu-id="c0d02-110">**[nuove funzionalità di EF Core 2.0](what-is-new/index.md)**</span><span class="sxs-lookup"><span data-stu-id="c0d02-110">**[New features in EF Core 2.0](what-is-new/index.md)**</span></span>
- <span data-ttu-id="c0d02-111">**[aggiornamento delle applicazioni esistenti di EF Core 2.0](miscellaneous/1x-2x-upgrade.md)**</span><span class="sxs-lookup"><span data-stu-id="c0d02-111">**[Upgrading existing applications to EF Core 2.0](miscellaneous/1x-2x-upgrade.md)**</span></span>

## <a name="get-entity-framework-core"></a><span data-ttu-id="c0d02-112">Ottenere Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c0d02-112">Get Entity Framework Core</span></span>

<span data-ttu-id="c0d02-113">[Installare il pacchetto NuGet](https://docs.nuget.org/ndocs/quickstart/use-a-package) per il provider di database che si vuole usare.</span><span class="sxs-lookup"><span data-stu-id="c0d02-113">[Install the NuGet package](https://docs.nuget.org/ndocs/quickstart/use-a-package) for the database provider you want to use.</span></span> <span data-ttu-id="c0d02-114">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="c0d02-114">E.g.</span></span> <span data-ttu-id="c0d02-115">per installare il provider SQL Server nello sviluppo multipiattaforma tramite lo strumento `dotnet` nella riga di comando:</span><span class="sxs-lookup"><span data-stu-id="c0d02-115">to install the SQL Server provider in cross-platform development using `dotnet` tool in the command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="c0d02-116">o, in Visual Studio, usando la Console di gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="c0d02-116">Or in Visual Studio, using the Package Manager Console:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
<span data-ttu-id="c0d02-117">Vedere [i provider di database](providers/index.md) per informazioni sui provider disponibili e [installazione Core EF](get-started/install/index.md) per altri passaggi di installazione.</span><span class="sxs-lookup"><span data-stu-id="c0d02-117">See [Database Providers](providers/index.md) for information on available providers and [Installing EF Core](get-started/install/index.md) for more detailed installation steps.</span></span>

## <a name="the-model"></a><span data-ttu-id="c0d02-118">Il modello</span><span class="sxs-lookup"><span data-stu-id="c0d02-118">The Model</span></span>

<span data-ttu-id="c0d02-119">Con Entity Framework Core, l'accesso ai dati viene eseguito tramite un modello.</span><span class="sxs-lookup"><span data-stu-id="c0d02-119">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="c0d02-120">Un modello è costituito da classi di entità e da un contesto derivato che rappresenta una sessione con il database che consente di eseguire una query e salvare i dati.</span><span class="sxs-lookup"><span data-stu-id="c0d02-120">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="c0d02-121">Per altre informazioni, vedere [Creazione di un modello](modeling/index.md).</span><span class="sxs-lookup"><span data-stu-id="c0d02-121">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="c0d02-122">È possibile generare un modello da un database esistente, gestire il codice di un modello in modo che corrisponda al database o usare migrazioni di Entity Framework per creare un database a partire dal modello (e svilupparlo, come le modifiche del modello nel tempo).</span><span class="sxs-lookup"><span data-stu-id="c0d02-122">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace Intro
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }
        public int Rating { get; set; }
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

## <a name="querying"></a><span data-ttu-id="c0d02-123">Query</span><span class="sxs-lookup"><span data-stu-id="c0d02-123">Querying</span></span>

<span data-ttu-id="c0d02-124">Le istanze di classi di entità vengono recuperate dal database tramite LINQ (Language Integrated Query).</span><span class="sxs-lookup"><span data-stu-id="c0d02-124">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="c0d02-125">Per altre informazioni, vedere [Esecuzione di query su dati](querying/index.md).</span><span class="sxs-lookup"><span data-stu-id="c0d02-125">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="c0d02-126">Salvataggio di dati</span><span class="sxs-lookup"><span data-stu-id="c0d02-126">Saving Data</span></span>

<span data-ttu-id="c0d02-127">I dati vengano creati, eliminati e modificati nel database tramite le istanze di classi di entità.</span><span class="sxs-lookup"><span data-stu-id="c0d02-127">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="c0d02-128">Per altre informazioni, vedere [Salvataggio di dati](saving/index.md).</span><span class="sxs-lookup"><span data-stu-id="c0d02-128">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
