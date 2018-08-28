---
title: Chiavi alternative - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: b26d8bc1630af9e811d9c4e7da850a618bc8042e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996971"
---
# <a name="alternate-keys"></a>Chiavi alternative

Una chiave alternativa viene utilizzato come un identificatore univoco alternativo per ogni istanza di entità oltre alla chiave primaria. Le chiavi alternative utilizzabili come destinazione di una relazione. Quando si usa un database relazionale viene eseguito il mapping al concetto di un indice o vincolo unique su una o più vincoli di chiave esterna che fanno riferimento le colonne e le colonne di chiavi alternative.

> [!TIP]  
> Se si desidera semplicemente garantire l'univocità di una colonna, quindi preferibile un indice univoco anziché una chiave alternativa, vedere [indici](indexes.md). In Entity Framework, le chiavi alternative offrono maggiori funzionalità rispetto agli indici univoci perché possono essere usati come destinazione di una chiave esterna.

Le chiavi alternative vengono in genere introdotte automaticamente all'occorrenza e non è necessario configurarle manualmente. Visualizzare [convenzioni](#conventions) per altri dettagli.

## <a name="conventions"></a>Convenzioni

Per convenzione, una chiave alternativa viene introdotto per l'utente quando si identifica una proprietà, non la chiave primaria, come la destinazione di una relazione.

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

## <a name="fluent-api"></a>API Fluent

Per configurare una singola proprietà da una chiave alternativa, è possibile utilizzare l'API Fluent.

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

È anche possibile usare l'API Fluent per configurare più proprietà da una chiave alternativa (nota come una chiave composta alternativa).

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
