---
title: Proprietà obbligatorie o facoltative - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: b6716a5b03e1afc2933e317d606ef50f986c22c7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995497"
---
# <a name="required-and-optional-properties"></a>Proprietà obbligatorie e facoltative

Una proprietà è considerata facoltativa se è valido per contenere `null`. Se `null` non è un valore valido da assegnare a una proprietà, quindi viene considerato come una proprietà obbligatoria.

## <a name="conventions"></a>Convenzioni

Per convenzione, una proprietà il cui tipo CLR può contenere null verrà configurata come facoltativi (`string`, `int?`, `byte[]`e così via.). Le proprietà il cui tipo CLR non può contenere null verranno configurate come richiesto (`int`, `decimal`, `bool`e così via.).

> [!NOTE]  
> Una proprietà il cui tipo CLR non può contenere null non possa essere configurata come facoltativi. La proprietà verrà considerata sempre richiesti da Entity Framework.

## <a name="data-annotations"></a>Annotazioni dei dati

È possibile usare le annotazioni dei dati per indicare che la proprietà è obbligatoria.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent per indicare che la proprietà è obbligatoria.

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
