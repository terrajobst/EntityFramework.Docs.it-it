---
title: Proprietà shadow - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: b7b7b10642564dfa3dbc05755188b5b5c63e0d03
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993802"
---
# <a name="shadow-properties"></a>Proprietà shadow

Proprietà shadow sono proprietà che non sono definite nella classe di entità .NET, ma sono definite per tale tipo di entità nel modello di EF Core. Il valore e lo stato di queste proprietà sono mantenute esclusivamente nel rilevamento delle modifiche.

Proprietà shadow sono utili quando sono presenti dati nel database che non deve essere esposte sui tipi di entità con mapping. Vengono spesso utilizzati per la proprietà di chiave esterna, dove la relazione tra due entità è rappresentata da un valore di chiave esterna nel database, ma la relazione viene eseguita sui tipi di entità usando le proprietà di navigazione tra i tipi di entità.

I valori delle proprietà shadow possono essere ottenuti e modificati tramite la `ChangeTracker` API.

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Proprietà shadow sono reperibile in query LINQ tramite le `EF.Property` metodo statico.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Convenzioni

È possibile creare proprietà shadow per convenzione quando viene individuata una relazione ma nessuna proprietà di chiave esterna è stato trovato nella classe di entità dipendente. In questo caso, verrà introdotta una proprietà di chiave esterna di ombreggiatura. La proprietà di chiave esterna di shadow verrà denominata `<navigation property name><principal key property name>` (riquadro di spostamento dell'entità dipendente, che fa riferimento all'entità principale, viene usato per la denominazione). Se il nome della proprietà chiave dell'entità include il nome della proprietà di navigazione, quindi il nome sarà semplicemente `<principal key property name>`. Se è presente nessuna proprietà di navigazione nell'entità dipendente, il nome del tipo dell'entità viene utilizzato al suo posto.

Ad esempio, il listato di codice seguente genererà un `BlogId` introdotta per la proprietà shadow il `Post` entità.

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

Nascondere le proprietà non è possibile creare con le annotazioni dei dati.

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per configurare le proprietà shadow. Dopo aver chiamato l'overload dei valori di `Property` è possibile concatenare qualsiasi delle chiamate di configurazione si farebbe per le altre proprietà.

Se il nome fornito per il `Property` metodo corrisponde al nome di una proprietà esistente (una proprietà shadow o quello definito nella classe di entità), quindi verrà configurato il codice di tale proprietà esistente anziché introdurre una nuova proprietà shadow.

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
