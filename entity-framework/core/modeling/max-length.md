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
---
# <a name="maximum-length"></a><span data-ttu-id="5a477-102">Lunghezza massima</span><span class="sxs-lookup"><span data-stu-id="5a477-102">Maximum Length</span></span>

<span data-ttu-id="5a477-103">Configurazione di una lunghezza massima fornisce un hint per l'archivio dati sul tipo di dati appropriato da utilizzare per una determinata proprietà.</span><span class="sxs-lookup"><span data-stu-id="5a477-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="5a477-104">Lunghezza massima si applica solo ai tipi di dati matrice, ad esempio `string` e `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="5a477-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="5a477-105">Entity Framework non esegue alcuna convalida della lunghezza massima consentita prima di passare dati al provider.</span><span class="sxs-lookup"><span data-stu-id="5a477-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="5a477-106">È responsabilità dell'archivio del provider o dati da convalidare se appropriato.</span><span class="sxs-lookup"><span data-stu-id="5a477-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="5a477-107">Ad esempio, quando SQL Server, che supera la lunghezza massima di destinazione verrà generata un'eccezione come tipo di dati della colonna sottostante non consentirà di archiviare i dati in eccesso.</span><span class="sxs-lookup"><span data-stu-id="5a477-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="5a477-108">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="5a477-108">Conventions</span></span>

<span data-ttu-id="5a477-109">Per convenzione, viene lasciata al provider di database di scegliere un tipo di dati appropriato per le proprietà.</span><span class="sxs-lookup"><span data-stu-id="5a477-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="5a477-110">Per le proprietà che hanno una lunghezza, il provider di database verrà in genere scegliere un tipo di dati che consente la lunghezza massima dei dati.</span><span class="sxs-lookup"><span data-stu-id="5a477-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="5a477-111">Ad esempio, Microsoft SQL Server utilizzerà `nvarchar(max)` per `string` proprietà (o `nvarchar(450)` se la colonna viene utilizzata come chiave).</span><span class="sxs-lookup"><span data-stu-id="5a477-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5a477-112">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="5a477-112">Data Annotations</span></span>

<span data-ttu-id="5a477-113">Per configurare una lunghezza massima per una proprietà, è possibile utilizzare le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="5a477-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="5a477-114">In questo esempio, destinato a SQL Server in questo modo, il `nvarchar(500)` il tipo di dati in uso.</span><span class="sxs-lookup"><span data-stu-id="5a477-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [MaxLength(500)]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="5a477-115">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="5a477-115">Fluent API</span></span>

<span data-ttu-id="5a477-116">Per configurare una lunghezza massima per una proprietà, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="5a477-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="5a477-117">In questo esempio, destinato a SQL Server in questo modo, il `nvarchar(500)` il tipo di dati in uso.</span><span class="sxs-lookup"><span data-stu-id="5a477-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

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
