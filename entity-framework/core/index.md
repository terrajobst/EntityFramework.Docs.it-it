---
title: Panoramica di Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: 0107a520e5a698eaf76426b63c6f784392559167
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196980"
---
# <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) Core è una versione semplice, estendibile, [open source](https://github.com/aspnet/EntityFrameworkCore) e multipiattaforma della tecnologia di accesso ai dati di grande diffusione Entity Framework.

Entity Framework Core può essere usato come mapper relazionale a oggetti (O/RM), consentendo agli sviluppatori .NET di utilizzare un database con oggetti .NET ed eliminando la necessità della maggior parte del codice di accesso ai dati che è in genere necessario scrivere.

EF Core supporta molti motori di database. Per informazioni dettagliate, vedere [Provider di Database](providers/index.md).

## <a name="the-model"></a>Il modello

Con Entity Framework Core, l'accesso ai dati viene eseguito tramite un modello. Un modello è costituito da classi di entità e da un contesto dell'oggetto che rappresenta una sessione con il database che consente di eseguire una query e salvare i dati. Per altre informazioni, vedere [Creazione di un modello](modeling/index.md).

È possibile generare un modello da un database esistente, scrivere manualmente il codice di un modello in modo che corrisponda al database o usare [migrazioni di Entity Framework](managing-schemas/migrations/index.md) per creare un database a partire dal modello e quindi svilupparlo man mano che il modello cambia nel tempo.

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
            optionsBuilder.UseSqlServer(
                @"Server=(localdb)\mssqllocaldb;Database=Blogging;Integrated Security=True");
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

## <a name="next-steps"></a>Passaggi successivi

Per le esercitazioni introduttive, vedere [Introduzione a Entity Framework Core](get-started/index.md).

