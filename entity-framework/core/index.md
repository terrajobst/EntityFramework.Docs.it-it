---
title: Breve panoramica - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: f9aac91545b97e56686e3a8d2eb9e83c849587d9
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2018
---
# <a name="entity-framework-core-quick-overview"></a>Breve panoramica di Entity Framework Core

Entity Framework (EF) Core è una versione semplice, estendibile e multipiattaforma della tecnologia di accesso dati Entity Framework più diffusa.

Entity Framework Core può essere usato come mapper relazionale a oggetti (O/RM), consentendo agli sviluppatori .NET di utilizzare un database con oggetti .NET ed eliminando la necessità della maggior parte del codice di accesso ai dati che è in genere necessario scrivere. 

EF Core supporta molti motori di database. Per informazioni dettagliate, vedere [Provider di Database](providers/index.md).

Per un apprendimento per scrittura di codice, vedere una delle guide [Introduzione](get-started/index.md) per iniziare con Entity Framework Core.

## <a name="what-is-new-in-ef-core"></a>Novità di EF Core

Se si ha familiarità con EF Core e si vuole passare direttamente ai dettagli delle versioni più recenti:

- **[Novità di EF Core 2.1 (attualmente in anteprima)](xref:core/what-is-new/ef-core-2.1)**
- **[Novità di EF Core 2.0 (ultima versione rilasciata)](xref:core/what-is-new/ef-core-2.0)**
- **[aggiornamento delle applicazioni esistenti di EF Core 2.0](xref:core/miscellaneous/1x-2x-upgrade)**


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
