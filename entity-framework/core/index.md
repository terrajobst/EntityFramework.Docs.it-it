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
# <a name="entity-framework-core-quick-overview"></a>Breve panoramica di Entity Framework Core

Entity Framework (EF) Core è una versione semplice, estendibile e multipiattaforma della tecnologia di accesso dati Entity Framework più diffusa.

EF Core è un mapper relazionale a oggetti che consente agli sviluppatori di .NET di usare un database con gli oggetti .NET. In questo modo la maggior parte del codice di accesso ai dati che in genere gli sviluppatori devono scrivere non è più necessaria. EF Core supporta molti motori di database. Per informazioni dettagliate, vedere [Provider di Database](providers/index.md).

Per un apprendimento per scrittura di codice, vedere una delle guide [Introduzione](get-started/index.md) per iniziare con Entity Framework Core.

## <a name="latest-version-ef-core-20"></a>Ultima versione: EF Core 2.0

Se si ha familiarità con Entity Framework Core e si vuole passare direttamente ai dettagli della nuova versione:

- **[nuove funzionalità di EF Core 2.0](what-is-new/index.md)**
- **[aggiornamento delle applicazioni esistenti di EF Core 2.0](miscellaneous/1x-2x-upgrade.md)**

## <a name="get-entity-framework-core"></a>Ottenere Entity Framework Core

[Installare il pacchetto NuGet](https://docs.nuget.org/ndocs/quickstart/use-a-package) per il provider di database che si vuole usare. Ad esempio, per installare il provider SQL Server nello sviluppo multipiattaforma tramite lo strumento `dotnet` nella riga di comando:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

o, in Visual Studio, usando la Console di gestione pacchetti:

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
Vedere [i provider di database](providers/index.md) per informazioni sui provider disponibili e [installazione Core EF](get-started/install/index.md) per altri passaggi di installazione.

## <a name="the-model"></a>Il modello

Con Entity Framework Core, l'accesso ai dati viene eseguito tramite un modello. Un modello è costituito da classi di entità e da un contesto derivato che rappresenta una sessione con il database che consente di eseguire una query e salvare i dati. Per altre informazioni, vedere [Creazione di un modello](modeling/index.md).

È possibile generare un modello da un database esistente, gestire il codice di un modello in modo che corrisponda al database o usare migrazioni di Entity Framework per creare un database a partire dal modello (e svilupparlo, come le modifiche del modello nel tempo).

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

## <a name="querying"></a>Query

Le istanze di classi di entità vengono recuperate dal database tramite LINQ (Language Integrated Query). Per altre informazioni, vedere [Esecuzione di query su dati](querying/index.md).

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a>Salvataggio di dati

I dati vengano creati, eliminati e modificati nel database tramite le istanze di classi di entità. Per altre informazioni, vedere [Salvataggio di dati](saving/index.md).

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
