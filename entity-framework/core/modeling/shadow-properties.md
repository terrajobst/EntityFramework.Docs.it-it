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
# <a name="shadow-properties"></a><span data-ttu-id="ea15b-102">Nascondere le proprietà</span><span class="sxs-lookup"><span data-stu-id="ea15b-102">Shadow Properties</span></span>

<span data-ttu-id="ea15b-103">Nascondere le proprietà sono proprietà che non sono definite nella classe di entità .NET, ma sono definite per quel tipo di entità nel modello di base di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ea15b-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="ea15b-104">Il valore e lo stato di queste proprietà vengono gestiti esclusivamente nel rilevamento delle modifiche,</span><span class="sxs-lookup"><span data-stu-id="ea15b-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="ea15b-105">Nascondere le proprietà sono utili quando sono presenti dati nel database che non deve essere esposto ai tipi di entità mappata.</span><span class="sxs-lookup"><span data-stu-id="ea15b-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="ea15b-106">Vengono spesso utilizzati per la proprietà di chiave esterna, in cui la relazione tra due entità è rappresentata da un valore di chiave esterna nel database, ma la relazione è gestita in tipi di entità tramite le proprietà di navigazione tra i tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="ea15b-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="ea15b-107">I valori delle proprietà shadow possono essere ottenuti e può essere modificati tramite il `ChangeTracker` API.</span><span class="sxs-lookup"><span data-stu-id="ea15b-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="ea15b-108">Nascondere le proprietà possono essere a cui fa riferimento nelle query LINQ tramite il `EF.Property` metodo statico.</span><span class="sxs-lookup"><span data-stu-id="ea15b-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="ea15b-109">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="ea15b-109">Conventions</span></span>

<span data-ttu-id="ea15b-110">È possibile creare proprietà shadow per convenzione quando viene individuata una relazione ma nessuna proprietà di chiave esterna viene trovata nella classe di entità dipendente.</span><span class="sxs-lookup"><span data-stu-id="ea15b-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="ea15b-111">In questo caso, verrà introdotte una proprietà di chiave esterna di ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="ea15b-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="ea15b-112">Proprietà della chiave esterna shadow verrà denominata `<navigation property name><principal key property name>` (riquadro di spostamento dell'entità dipendente, che fa riferimento all'entità principale, viene utilizzato per la denominazione).</span><span class="sxs-lookup"><span data-stu-id="ea15b-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="ea15b-113">Se il nome della proprietà chiave principale include il nome della proprietà di navigazione, quindi il nome sarà sufficiente `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="ea15b-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="ea15b-114">Se è presente alcuna proprietà di navigazione sull'entità dipendenti, al suo posto viene utilizzato il nome del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="ea15b-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="ea15b-115">Ad esempio, il listato di codice seguente comporterà un `BlogId` proprietà shadow viene introdotta per la `Post` entità.</span><span class="sxs-lookup"><span data-stu-id="ea15b-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="ea15b-116">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="ea15b-116">Data Annotations</span></span>

<span data-ttu-id="ea15b-117">Nascondere le proprietà non possono essere create con le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="ea15b-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ea15b-118">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="ea15b-118">Fluent API</span></span>

<span data-ttu-id="ea15b-119">Per configurare le proprietà di ombreggiatura, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="ea15b-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="ea15b-120">Dopo aver richiamato il metodo di overload stringa `Property` è possibile concatenare qualsiasi delle chiamate di configurazione si farebbe per le altre proprietà.</span><span class="sxs-lookup"><span data-stu-id="ea15b-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="ea15b-121">Se il nome specificato per il `Property` metodo corrisponde al nome di una proprietà esistente (una proprietà di ombreggiatura o quello definito nella classe di entità), quindi il codice verrà configurare tale proprietà esistenti, anziché introdurre una nuova proprietà di ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="ea15b-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

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
