---
title: Ereditarietà (Database relazionale) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 2aaceb05bbc1b0eb5c116b3dc1fb33c90c115a70
ms.sourcegitcommit: 645785187ae23ddf7d7b0642c7a4da5ffb0c7f30
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419679"
---
# <a name="inheritance-relational-database"></a>Ereditarietà (Database relazionale)

> [!NOTE]  
> La configurazione di questa sezione è applicabile in generale ai database relazionali. I metodi di estensione descritti diventano disponibili quando si installa un provider di database relazionali (a causa del pacchetto *Microsoft.EntityFrameworkCore.Relational* condiviso).

Ereditarietà nel modello di Entity Framework viene utilizzato per controllare la modalità di rappresentazione di ereditarietà nelle classi di entità nel database.

> [!NOTE]  
> Attualmente, solo il criterio di tabella per gerarchia (TPH) viene implementato in EF Core. Altri modelli comuni, ad esempio tabella per tipo (TPT) e tabella-per-tipo concreto (TP) non sono ancora disponibili.

## <a name="conventions"></a>Convenzioni

Per convenzione, verrà mappata ereditarietà usando il modello di tabella per gerarchia (TPH). Tabella per gerarchia utilizza una singola tabella per archiviare i dati per tutti i tipi nella gerarchia. Consente di identificare il tipo di ogni riga rappresenta una colonna discriminatore.

EF Core configurerà ereditarietà solo se due o più tipi ereditati sono inclusi in modo esplicito nel modello (vedere [ereditarietà](../inheritance.md) per altri dettagli).

Di seguito è riportato un esempio che illustra uno scenario di ereditarietà semplice e i dati archiviati in una tabella di database relazionali adottando il modello di tabella per gerarchia. Il *discriminatore* colonna identifica il tipo di *Blog* viene archiviato in ogni riga.

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

>[!NOTE]
> Quando si usa il mapping di tabella per gerarchia colmmns database vengono automaticamente resi nullable in base alle esigenze.

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile usare le annotazioni dei dati per configurare l'ereditarietà.

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare il nome e il tipo della colonna discriminatore e i valori che vengono usati per identificare ogni tipo nella gerarchia.

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

## <a name="configuring-the-discriminator-property"></a>Configurazione della proprietà discriminator

Negli esempi precedenti, il discriminatore viene creato come un [proprietà shadow](xref:core/modeling/shadow-properties) nell'entità di base della gerarchia. Poiché si tratta di una proprietà nel modello, può essere configurato esattamente come le altre proprietà. Ad esempio, per impostare la lunghezza massima quando viene usato il valore predefinito, discriminatore da convenzione:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

Il discriminatore può anche eseguire il mapping a una proprietà CLR effettiva nell'entità. Ad esempio:
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

Combinando questi due elementi è possibile eseguire il mapping del discriminatore per una proprietà reale e configurarla:
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
