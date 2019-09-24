---
title: Ereditarietà (database relazionale)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: a7fb19f9c86d1768967d172c006eb5d894254e0c
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196940"
---
# <a name="inheritance-relational-database"></a>Ereditarietà (database relazionale)

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

L'ereditarietà nel modello EF viene utilizzata per controllare il modo in cui l'ereditarietà nelle classi di entità viene rappresentata nel database.

> [!NOTE]  
> Attualmente, in EF Core viene implementato solo il modello tabella per gerarchia (TPH). Altri modelli comuni, ad esempio tabella per tipo (TPT) e tabella per tipo concreto (TPC), non sono ancora disponibili.

## <a name="conventions"></a>Convenzioni

Per convenzione, verrà eseguito il mapping dell'ereditarietà utilizzando il modello tabella per gerarchia (TPH). TPH usa una singola tabella per archiviare i dati per tutti i tipi nella gerarchia. Una colonna discriminatore viene utilizzata per identificare il tipo rappresentato da ogni riga.

EF Core configurerà l'ereditarietà solo se due o più tipi ereditati vengono inclusi in modo esplicito nel modello. per ulteriori informazioni, vedere [ereditarietà](../inheritance.md) .

Di seguito è riportato un esempio che mostra uno scenario di ereditarietà semplice e i dati archiviati in una tabella di database relazionale usando il modello TPH. La colonna *discriminatore* identifica il tipo di *Blog* archiviato in ogni riga.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/InheritanceDbSets.cs)] -->
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

>[!NOTE]
> Le colonne del database vengono rese automaticamente Nullable quando necessario quando si usa il mapping di TPH.

## <a name="data-annotations"></a>Annotazioni dei dati

Non è possibile utilizzare le annotazioni dei dati per configurare l'ereditarietà.

## <a name="fluent-api"></a>API Fluent

È possibile utilizzare l'API Fluent per configurare il nome e il tipo della colonna discriminatore e i valori utilizzati per identificare ogni tipo nella gerarchia.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
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

## <a name="configuring-the-discriminator-property"></a>Configurazione della proprietà Discriminator

Negli esempi precedenti, il discriminatore viene creato come [proprietà shadow](xref:core/modeling/shadow-properties) nell'entità di base della gerarchia. Poiché si tratta di una proprietà nel modello, può essere configurata esattamente come le altre proprietà. Ad esempio, per impostare la lunghezza massima quando viene usato il discriminatore predefinito per convenzione:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

È anche possibile eseguire il mapping del discriminatore a una proprietà CLR effettiva nell'entità. Ad esempio:
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

Combinando questi due elementi, è possibile eseguire il mapping del discriminatore a una proprietà reale e configurarlo:
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
