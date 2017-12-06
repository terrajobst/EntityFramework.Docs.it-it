---
title: "Proprietà obbligatorie o facoltative - Core a Entity Framework"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
ms.technology: entity-framework-core
uid: core/modeling/required-optional
ms.openlocfilehash: 2af1d49e12ef980f81cb9c00556dee471673ccae
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="required-and-optional-properties"></a>Proprietà obbligatorie e facoltative

Una proprietà è considerata facoltativa se è valido per contenere `null`. Se `null` non è un valore valido per poter essere assegnati a una proprietà, quindi è considerato una proprietà obbligatoria.

## <a name="conventions"></a>Convenzioni

Per convenzione, una proprietà il cui tipo CLR può contenere null verrà configurata come facoltativi (`string`, `int?`, `byte[]`, ecc.). Le proprietà il cui tipo CLR non può contenere null verranno configurate come richiesto (`int`, `decimal`, `bool`, ecc.).

> [!NOTE]  
> Una proprietà il cui tipo CLR non può contenere null non possa essere configurata come facoltativi. La proprietà verrà considerata sempre richiesto da Entity Framework.

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile utilizzare le annotazioni dei dati per indicare che una proprietà è obbligatoria.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Microsoft Office Fluent API

È possibile utilizzare l'API Fluent per indicare che una proprietà è obbligatoria.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
