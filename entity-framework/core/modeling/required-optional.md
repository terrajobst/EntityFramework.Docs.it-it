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
# <a name="required-and-optional-properties"></a><span data-ttu-id="c0ca0-102">Proprietà obbligatorie e facoltative</span><span class="sxs-lookup"><span data-stu-id="c0ca0-102">Required and Optional Properties</span></span>

<span data-ttu-id="c0ca0-103">Una proprietà è considerata facoltativa se è valido per contenere `null`.</span><span class="sxs-lookup"><span data-stu-id="c0ca0-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="c0ca0-104">Se `null` non è un valore valido da assegnare a una proprietà, quindi viene considerato come una proprietà obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="c0ca0-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="c0ca0-105">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="c0ca0-105">Conventions</span></span>

<span data-ttu-id="c0ca0-106">Per convenzione, una proprietà il cui tipo CLR può contenere null verrà configurata come facoltativi (`string`, `int?`, `byte[]`e così via.).</span><span class="sxs-lookup"><span data-stu-id="c0ca0-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="c0ca0-107">Le proprietà il cui tipo CLR non può contenere null verranno configurate come richiesto (`int`, `decimal`, `bool`e così via.).</span><span class="sxs-lookup"><span data-stu-id="c0ca0-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="c0ca0-108">Una proprietà il cui tipo CLR non può contenere null non possa essere configurata come facoltativi.</span><span class="sxs-lookup"><span data-stu-id="c0ca0-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="c0ca0-109">La proprietà verrà considerata sempre richiesti da Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c0ca0-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c0ca0-110">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="c0ca0-110">Data Annotations</span></span>

<span data-ttu-id="c0ca0-111">È possibile usare le annotazioni dei dati per indicare che la proprietà è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="c0ca0-111">You can use Data Annotations to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="c0ca0-112">API Fluent</span><span class="sxs-lookup"><span data-stu-id="c0ca0-112">Fluent API</span></span>

<span data-ttu-id="c0ca0-113">È possibile usare l'API Fluent per indicare che la proprietà è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="c0ca0-113">You can use the Fluent API to indicate that a property is required.</span></span>

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
