---
title: "Proprietà shadow - Core a Entity Framework"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
ms.technology: entity-framework-core
uid: core/modeling/shadow-properties
ms.openlocfilehash: 8c7f70789ddc6ebd24f9bfae931069834db593c2
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2017
---
# <a name="shadow-properties"></a>Nascondere le proprietà

Nascondere le proprietà sono proprietà che non sono definite nella classe di entità .NET, ma sono definite per quel tipo di entità nel modello di base di Entity Framework. Il valore e lo stato di queste proprietà vengono gestiti esclusivamente nel rilevamento delle modifiche,

Nascondere le proprietà sono utili quando sono presenti dati nel database che non deve essere esposto ai tipi di entità mappata. Vengono spesso utilizzati per la proprietà di chiave esterna, in cui la relazione tra due entità è rappresentata da un valore di chiave esterna nel database, ma la relazione è gestita in tipi di entità tramite le proprietà di navigazione tra i tipi di entità.

I valori delle proprietà shadow possono essere ottenuti e può essere modificati tramite il `ChangeTracker` API.

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Nascondere le proprietà possono essere a cui fa riferimento nelle query LINQ tramite il `EF.Property` metodo statico.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Convenzioni

È possibile creare proprietà shadow per convenzione quando viene individuata una relazione ma nessuna proprietà di chiave esterna viene trovata nella classe di entità dipendente. In questo caso, verrà introdotte una proprietà di chiave esterna di ombreggiatura. Proprietà della chiave esterna shadow verrà denominata `<navigation property name><principal key property name>` (riquadro di spostamento dell'entità dipendente, che fa riferimento all'entità principale, viene utilizzato per la denominazione). Se il nome della proprietà chiave principale include il nome della proprietà di navigazione, quindi il nome sarà sufficiente `<principal key property name>`. Se è presente alcuna proprietà di navigazione sull'entità dipendenti, al suo posto viene utilizzato il nome del tipo di entità.

Ad esempio, il listato di codice seguente comporterà un `BlogId` proprietà shadow viene introdotta per la `Post` entità.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/ShadowForeignKey.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
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

    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a>Annotazioni dei dati

Nascondere le proprietà non possono essere create con le annotazioni dei dati.

## <a name="fluent-api"></a>Microsoft Office Fluent API

Per configurare le proprietà di ombreggiatura, è possibile utilizzare l'API Fluent. Dopo aver richiamato il metodo di overload stringa `Property` è possibile concatenare qualsiasi delle chiamate di configurazione si farebbe per le altre proprietà.

Se il nome specificato per il `Property` metodo corrisponde al nome di una proprietà esistente (una proprietà di ombreggiatura o quello definito nella classe di entità), quindi il codice verrà configurare tale proprietà esistenti, anziché introdurre una nuova proprietà di ombreggiatura.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/ShadowProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property<DateTime>("LastUpdated");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
