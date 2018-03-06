---
title: Breve panoramica - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: c76b4cd318151b502c549fa0a82800f9987ed94c
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2018
---
# <a name="entity-framework-core-quick-overview"></a><span data-ttu-id="11459-102">Breve panoramica di Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="11459-102">Entity Framework Core Quick Overview</span></span>

<span data-ttu-id="11459-103">Entity Framework (EF) Core è una versione semplice, estendibile e multipiattaforma della tecnologia di accesso dati Entity Framework più diffusa.</span><span class="sxs-lookup"><span data-stu-id="11459-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="11459-104">EF Core è un mapper relazionale a oggetti che consente agli sviluppatori di .NET di usare un database con gli oggetti .NET.</span><span class="sxs-lookup"><span data-stu-id="11459-104">EF Core is an object-relational mapper (O/RM) that enables .NET developers to work with a database using .NET objects.</span></span> <span data-ttu-id="11459-105">In questo modo la maggior parte del codice di accesso ai dati che in genere gli sviluppatori devono scrivere non è più necessaria.</span><span class="sxs-lookup"><span data-stu-id="11459-105">It eliminates the need for most of the data-access code that developers usually need to write.</span></span> <span data-ttu-id="11459-106">EF Core supporta molti motori di database. Per informazioni dettagliate, vedere [Provider di Database](providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="11459-106">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

<span data-ttu-id="11459-107">Per un apprendimento per scrittura di codice, vedere una delle guide [Introduzione](get-started/index.md) per iniziare con Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="11459-107">If you like to learn by writing code, we'd recommend one of our [Getting Started](get-started/index.md) guides to get you started with EF Core.</span></span>

## <a name="what-is-new-in-ef-core"></a><span data-ttu-id="11459-108">Novità di EF Core</span><span class="sxs-lookup"><span data-stu-id="11459-108">What is new in EF Core</span></span>

<span data-ttu-id="11459-109">Se si ha familiarità con EF Core e si vuole passare direttamente ai dettagli delle versioni più recenti:</span><span class="sxs-lookup"><span data-stu-id="11459-109">If you are familiar with EF Core and want to jump straight into the details of the latest releases:</span></span>

- <span data-ttu-id="11459-110">**[Novità di EF Core 2.1 (attualmente in anteprima)](xref:core/what-is-new/ef-core-2.1)**</span><span class="sxs-lookup"><span data-stu-id="11459-110">**[What is new in EF Core 2.1 (currently in preview)](xref:core/what-is-new/ef-core-2.1)**</span></span>
- <span data-ttu-id="11459-111">**[Novità di EF Core 2.0 (ultima versione rilasciata)](xref:core/what-is-new/ef-core-2.0)**</span><span class="sxs-lookup"><span data-stu-id="11459-111">**[What is new in EF Core 2.0 (the latest released version)](xref:core/what-is-new/ef-core-2.0)**</span></span>
- <span data-ttu-id="11459-112">**[aggiornamento delle applicazioni esistenti di EF Core 2.0](xref:core/miscellaneous/1x-2x-upgrade)**</span><span class="sxs-lookup"><span data-stu-id="11459-112">**[Upgrading existing applications to EF Core 2.0](xref:core/miscellaneous/1x-2x-upgrade)**</span></span>


## <a name="get-entity-framework-core"></a><span data-ttu-id="11459-113">Ottenere Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="11459-113">Get Entity Framework Core</span></span>

<span data-ttu-id="11459-114">[Installare il pacchetto NuGet](https://docs.nuget.org/ndocs/quickstart/use-a-package) per il provider di database che si vuole usare.</span><span class="sxs-lookup"><span data-stu-id="11459-114">[Install the NuGet package](https://docs.nuget.org/ndocs/quickstart/use-a-package) for the database provider you want to use.</span></span> <span data-ttu-id="11459-115">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="11459-115">E.g.</span></span> <span data-ttu-id="11459-116">per installare il provider SQL Server nello sviluppo multipiattaforma tramite lo strumento `dotnet` nella riga di comando:</span><span class="sxs-lookup"><span data-stu-id="11459-116">to install the SQL Server provider in cross-platform development using `dotnet` tool in the command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="11459-117">o, in Visual Studio, usando la Console di gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="11459-117">Or in Visual Studio, using the Package Manager Console:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
<span data-ttu-id="11459-118">Vedere [i provider di database](providers/index.md) per informazioni sui provider disponibili e [installazione Core EF](get-started/install/index.md) per altri passaggi di installazione.</span><span class="sxs-lookup"><span data-stu-id="11459-118">See [Database Providers](providers/index.md) for information on available providers and [Installing EF Core](get-started/install/index.md) for more detailed installation steps.</span></span>

## <a name="the-model"></a><span data-ttu-id="11459-119">Il modello</span><span class="sxs-lookup"><span data-stu-id="11459-119">The Model</span></span>

<span data-ttu-id="11459-120">Con Entity Framework Core, l'accesso ai dati viene eseguito tramite un modello.</span><span class="sxs-lookup"><span data-stu-id="11459-120">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="11459-121">Un modello è costituito da classi di entità e da un contesto derivato che rappresenta una sessione con il database che consente di eseguire una query e salvare i dati.</span><span class="sxs-lookup"><span data-stu-id="11459-121">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="11459-122">Per altre informazioni, vedere [Creazione di un modello](modeling/index.md).</span><span class="sxs-lookup"><span data-stu-id="11459-122">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="11459-123">È possibile generare un modello da un database esistente, gestire il codice di un modello in modo che corrisponda al database o usare migrazioni di Entity Framework per creare un database a partire dal modello (e svilupparlo, come le modifiche del modello nel tempo).</span><span class="sxs-lookup"><span data-stu-id="11459-123">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

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

## <a name="querying"></a><span data-ttu-id="11459-124">Query</span><span class="sxs-lookup"><span data-stu-id="11459-124">Querying</span></span>

<span data-ttu-id="11459-125">Le istanze di classi di entità vengono recuperate dal database tramite LINQ (Language Integrated Query).</span><span class="sxs-lookup"><span data-stu-id="11459-125">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="11459-126">Per altre informazioni, vedere [Esecuzione di query su dati](querying/index.md).</span><span class="sxs-lookup"><span data-stu-id="11459-126">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="11459-127">Salvataggio di dati</span><span class="sxs-lookup"><span data-stu-id="11459-127">Saving Data</span></span>

<span data-ttu-id="11459-128">I dati vengano creati, eliminati e modificati nel database tramite le istanze di classi di entità.</span><span class="sxs-lookup"><span data-stu-id="11459-128">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="11459-129">Per altre informazioni, vedere [Salvataggio di dati](saving/index.md).</span><span class="sxs-lookup"><span data-stu-id="11459-129">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
