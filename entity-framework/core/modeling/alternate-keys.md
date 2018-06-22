---
title: Chiavi alternative - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
ms.technology: entity-framework-core
uid: core/modeling/alternate-keys
ms.openlocfilehash: 09f86a8932b71ec8f30ee90a088091a00233c20f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052471"
---
# <a name="alternate-keys"></a>Chiavi alternative

Una chiave alternativa viene utilizzato come un identificatore univoco alternativo per ogni istanza di entità oltre alla chiave primaria. Le chiavi alternative utilizzabile come destinazione di una relazione. Quando si utilizza un database relazionale esegue il mapping al concetto di un indice o vincolo unique per le colonne di chiave alternative e uno o più vincoli di chiave esterna che fanno riferimento le colonne.

> [!TIP]  
> Se si desidera applicare l'univocità di una colonna, è necessario un indice univoco anziché di una chiave alternativa, vedere [indici](indexes.md). In Entity Framework, le chiavi alternative offrono maggiori funzionalità rispetto agli indici univoci perché possono essere usati come destinazione di una chiave esterna.

Le chiavi alternative vengono in genere introdotti automaticamente quando necessario e non è necessario configurarle manualmente. Vedere [convenzioni](#conventions) per altri dettagli.

## <a name="conventions"></a>Convenzioni

Per convenzione, una chiave alternativa è stato introdotto per l'utente quando si rileva una proprietà, che non è la chiave primaria, come destinazione di una relazione.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/AlternateKey.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogUrl)
            .HasPrincipalKey(b => b.Url);
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

    public string BlogUrl { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a>Annotazioni dei dati

Le chiavi alternative non possono essere configurate utilizzando le annotazioni dei dati.

## <a name="fluent-api"></a>Microsoft Office Fluent API

Per configurare una singola proprietà di una chiave alternativa, è possibile utilizzare l'API Fluent.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => c.LicensePlate);
    }
}

class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```

È inoltre possibile utilizzare l'API Fluent per configurare le proprietà più di una chiave alternativa (nota come una chiave composta alternativa).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```
