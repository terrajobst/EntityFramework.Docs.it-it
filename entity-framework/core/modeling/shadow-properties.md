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
# <a name="shadow-properties"></a><span data-ttu-id="a6226-102">Proprietà shadow</span><span class="sxs-lookup"><span data-stu-id="a6226-102">Shadow Properties</span></span>

<span data-ttu-id="a6226-103">Proprietà shadow sono proprietà che non sono definite nella classe di entità .NET, ma sono definite per tale tipo di entità nel modello di EF Core.</span><span class="sxs-lookup"><span data-stu-id="a6226-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="a6226-104">Il valore e lo stato di queste proprietà sono mantenute esclusivamente nel rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="a6226-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="a6226-105">Proprietà shadow sono utili quando sono presenti dati nel database che non deve essere esposte sui tipi di entità con mapping.</span><span class="sxs-lookup"><span data-stu-id="a6226-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="a6226-106">Vengono spesso utilizzati per la proprietà di chiave esterna, dove la relazione tra due entità è rappresentata da un valore di chiave esterna nel database, ma la relazione viene eseguita sui tipi di entità usando le proprietà di navigazione tra i tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="a6226-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="a6226-107">I valori delle proprietà shadow possono essere ottenuti e modificati tramite la `ChangeTracker` API.</span><span class="sxs-lookup"><span data-stu-id="a6226-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="a6226-108">Proprietà shadow sono reperibile in query LINQ tramite le `EF.Property` metodo statico.</span><span class="sxs-lookup"><span data-stu-id="a6226-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="a6226-109">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="a6226-109">Conventions</span></span>

<span data-ttu-id="a6226-110">È possibile creare proprietà shadow per convenzione quando viene individuata una relazione ma nessuna proprietà di chiave esterna è stato trovato nella classe di entità dipendente.</span><span class="sxs-lookup"><span data-stu-id="a6226-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="a6226-111">In questo caso, verrà introdotta una proprietà di chiave esterna di ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="a6226-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="a6226-112">La proprietà di chiave esterna di shadow verrà denominata `<navigation property name><principal key property name>` (riquadro di spostamento dell'entità dipendente, che fa riferimento all'entità principale, viene usato per la denominazione).</span><span class="sxs-lookup"><span data-stu-id="a6226-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="a6226-113">Se il nome della proprietà chiave dell'entità include il nome della proprietà di navigazione, quindi il nome sarà semplicemente `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="a6226-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="a6226-114">Se è presente nessuna proprietà di navigazione nell'entità dipendente, il nome del tipo dell'entità viene utilizzato al suo posto.</span><span class="sxs-lookup"><span data-stu-id="a6226-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="a6226-115">Ad esempio, il listato di codice seguente genererà un `BlogId` introdotta per la proprietà shadow il `Post` entità.</span><span class="sxs-lookup"><span data-stu-id="a6226-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="a6226-116">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="a6226-116">Data Annotations</span></span>

<span data-ttu-id="a6226-117">Nascondere le proprietà non è possibile creare con le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="a6226-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a6226-118">API Fluent</span><span class="sxs-lookup"><span data-stu-id="a6226-118">Fluent API</span></span>

<span data-ttu-id="a6226-119">È possibile usare l'API Fluent per configurare le proprietà shadow.</span><span class="sxs-lookup"><span data-stu-id="a6226-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="a6226-120">Dopo aver chiamato l'overload dei valori di `Property` è possibile concatenare qualsiasi delle chiamate di configurazione si farebbe per le altre proprietà.</span><span class="sxs-lookup"><span data-stu-id="a6226-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="a6226-121">Se il nome fornito per il `Property` metodo corrisponde al nome di una proprietà esistente (una proprietà shadow o quello definito nella classe di entità), quindi verrà configurato il codice di tale proprietà esistente anziché introdurre una nuova proprietà shadow.</span><span class="sxs-lookup"><span data-stu-id="a6226-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

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
