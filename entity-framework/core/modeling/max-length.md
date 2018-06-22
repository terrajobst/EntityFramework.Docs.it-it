---
title: Lunghezza massima - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
ms.technology: entity-framework-core
uid: core/modeling/max-length
ms.openlocfilehash: 7325c0c3328477473392bf9e7c82f1696bb4f424
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052671"
---
# <a name="maximum-length"></a>Lunghezza massima

Configurazione di una lunghezza massima fornisce un hint per l'archivio dati sul tipo di dati appropriato da utilizzare per una determinata proprietà. Lunghezza massima si applica solo ai tipi di dati matrice, ad esempio `string` e `byte[]`.

> [!NOTE]  
> Entity Framework non esegue alcuna convalida della lunghezza massima consentita prima di passare dati al provider. È responsabilità dell'archivio del provider o dati da convalidare se appropriato. Ad esempio, quando SQL Server, che supera la lunghezza massima di destinazione verrà generata un'eccezione come tipo di dati della colonna sottostante non consentirà di archiviare i dati in eccesso.

## <a name="conventions"></a>Convenzioni

Per convenzione, viene lasciata al provider di database di scegliere un tipo di dati appropriato per le proprietà. Per le proprietà che hanno una lunghezza, il provider di database verrà in genere scegliere un tipo di dati che consente la lunghezza massima dei dati. Ad esempio, Microsoft SQL Server utilizzerà `nvarchar(max)` per `string` proprietà (o `nvarchar(450)` se la colonna viene utilizzata come chiave).

## <a name="data-annotations"></a>Annotazioni dei dati

Per configurare una lunghezza massima per una proprietà, è possibile utilizzare le annotazioni dei dati. In questo esempio, destinato a SQL Server in questo modo, il `nvarchar(500)` il tipo di dati in uso.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Microsoft Office Fluent API

Per configurare una lunghezza massima per una proprietà, è possibile utilizzare l'API Fluent. In questo esempio, destinato a SQL Server in questo modo, il `nvarchar(500)` il tipo di dati in uso.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .HasMaxLength(500);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
