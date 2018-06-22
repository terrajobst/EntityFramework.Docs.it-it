---
title: Ereditarietà (Database relazionale) - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 22eed0002b5903d3cfd18a7e4af0fcd2d46a5c4c
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152363"
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

## <a name="configuring-the-discriminator-property"></a>Configurazione proprietà discriminator

Negli esempi precedenti, viene creato il discriminatore come un [nascondere proprietà](xref:core/modeling/shadow-properties) su entità di base della gerarchia. Poiché si tratta di una proprietà nel modello, può essere configurato come altre proprietà. Ad esempio, per impostare la lunghezza massima, quando viene utilizzato il valore predefinito, del discriminatore per convenzione:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

Il discriminatore può anche essere mappato a una proprietà CLR effettiva nell'entità. Ad esempio:
```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

Combinando queste due operazioni è possibile eseguire il mapping del discriminatore per una proprietà reale sia configurarlo:
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
