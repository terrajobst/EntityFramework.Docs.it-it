---
title: Proprietà obbligatorie o facoltative - Core a Entity Framework
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
ms.locfileid: "26052851"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="cc9d4-102">Proprietà obbligatorie e facoltative</span><span class="sxs-lookup"><span data-stu-id="cc9d4-102">Required and Optional Properties</span></span>

<span data-ttu-id="cc9d4-103">Una proprietà è considerata facoltativa se è valido per contenere `null`.</span><span class="sxs-lookup"><span data-stu-id="cc9d4-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="cc9d4-104">Se `null` non è un valore valido per poter essere assegnati a una proprietà, quindi è considerato una proprietà obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="cc9d4-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="cc9d4-105">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="cc9d4-105">Conventions</span></span>

<span data-ttu-id="cc9d4-106">Per convenzione, una proprietà il cui tipo CLR può contenere null verrà configurata come facoltativi (`string`, `int?`, `byte[]`, ecc.).</span><span class="sxs-lookup"><span data-stu-id="cc9d4-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="cc9d4-107">Le proprietà il cui tipo CLR non può contenere null verranno configurate come richiesto (`int`, `decimal`, `bool`, ecc.).</span><span class="sxs-lookup"><span data-stu-id="cc9d4-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="cc9d4-108">Una proprietà il cui tipo CLR non può contenere null non possa essere configurata come facoltativi.</span><span class="sxs-lookup"><span data-stu-id="cc9d4-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="cc9d4-109">La proprietà verrà considerata sempre richiesto da Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cc9d4-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="cc9d4-110">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="cc9d4-110">Data Annotations</span></span>

<span data-ttu-id="cc9d4-111">È possibile utilizzare le annotazioni dei dati per indicare che una proprietà è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="cc9d4-111">You can use Data Annotations to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="cc9d4-112">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="cc9d4-112">Fluent API</span></span>

<span data-ttu-id="cc9d4-113">È possibile utilizzare l'API Fluent per indicare che una proprietà è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="cc9d4-113">You can use the Fluent API to indicate that a property is required.</span></span>

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
