---
title: "Ereditarietà (Database relazionale) - Core a Entity Framework"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 55286adf08a6a1c3286b7059d747a62e1feffd22
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/22/2017
---
# <a name="inheritance-relational-database"></a>Ereditarietà (Database relazionale)

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Ereditarietà nel modello di Entity Framework viene utilizzato per controllare la modalità di rappresentazione di ereditarietà nelle classi di entità nel database.

> [!NOTE]  
> Attualmente, solo il criterio di tabella per gerarchia (Implementerà) viene implementato in EF Core. Altri modelli comuni come tabella per tipo (tabella per tipo) e -per-concreto-tipo di tabella (TPC) non sono ancora disponibili.

## <a name="conventions"></a>Convenzioni

Per convenzione, ereditarietà verrà mappato usando il modello di tabella per ogni-gerarchia (TPH). Implementerà Usa una singola tabella per archiviare i dati per tutti i tipi nella gerarchia. Una colonna discriminatore viene utilizzata per identificare il tipo di ogni riga rappresenta.

Core EF installerà ereditarietà solo se due o più tipi ereditati sono inclusi in modo esplicito nel modello (vedere [ereditarietà](../inheritance.md) per altri dettagli).

Di seguito è riportato un esempio che illustra uno scenario semplice di ereditarietà e i dati archiviati in una tabella di database relazionale utilizzando il modello della tabella per gerarchia. Il *discriminatore* colonna identifica il tipo di *Blog* viene archiviato in ogni riga.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<RssBlog> RssBlogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

![immagine](_static/inheritance-tph-data.png)

## <a name="data-annotations"></a>Annotazioni dei dati

Per configurare l'ereditarietà, è possibile utilizzare le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

Per configurare il nome e il tipo di colonna discriminatore e i valori utilizzati per identificare ogni tipo nella gerarchia, è possibile utilizzare l'API Fluent.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("blog_type")
            .HasValue<Blog>("blog_base")
            .HasValue<RssBlog>("blog_rss");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```
