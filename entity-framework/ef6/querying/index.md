---
title: Query e ricerca di entità - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
caps.latest.revision: 3
ms.openlocfilehash: 92467e1a93f576eca627cf7b7d2351054a882c2c
ms.sourcegitcommit: 00cb52625b57c1ea339ded1454179fe89b6bcfea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/16/2018
ms.locfileid: "39067547"
---
# <a name="querying-and-finding-entities"></a><span data-ttu-id="e2da4-102">Query e ricerca di entità</span><span class="sxs-lookup"><span data-stu-id="e2da4-102">Querying and Finding Entities</span></span>
<span data-ttu-id="e2da4-103">Questo argomento illustra i vari modi che è possibile usare per la ricerca di dati tramite Entity Framework, inclusi LINQ e il metodo Find.</span><span class="sxs-lookup"><span data-stu-id="e2da4-103">This topic covers the various ways you can query for data using Entity Framework, including LINQ and the Find method.</span></span> <span data-ttu-id="e2da4-104">Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.</span><span class="sxs-lookup"><span data-stu-id="e2da4-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="finding-entities-using-a-query"></a><span data-ttu-id="e2da4-105">Ricerca di entità tramite query</span><span class="sxs-lookup"><span data-stu-id="e2da4-105">Finding entities using a query</span></span>  

<span data-ttu-id="e2da4-106">DbSet e IDbSet implementano IQueryable e possono quindi essere usati come punto di partenza per scrivere una query LINQ sul database.</span><span class="sxs-lookup"><span data-stu-id="e2da4-106">DbSet and IDbSet implement IQueryable and so can be used as the starting point for writing a LINQ query against the database.</span></span> <span data-ttu-id="e2da4-107">In questo articolo non si affronterà una discussione approfondita su LINQ, ma vengono comunque riportati due semplici esempi:</span><span class="sxs-lookup"><span data-stu-id="e2da4-107">This is not the appropriate place for an in-depth discussion of LINQ, but here are a couple of simple examples:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs with names starting with B
    var blogs = from b in context.Blogs
                   where b.Name.StartsWith("B")
                   select b;

    // Query for the Blog named ADO.NET Blog
    var blog = context.Blogs
                    .Where(b => b.Name == "ADO.NET Blog")
                    .FirstOrDefault();
}
```  

<span data-ttu-id="e2da4-108">Si noti che DbSet e IDbSet creano sempre query sul database e determineranno sempre un round trip al database anche se le entità restituite esistono già nel contesto.</span><span class="sxs-lookup"><span data-stu-id="e2da4-108">Note that DbSet and IDbSet always create queries against the database and will always involve a round trip to the database even if the entities returned already exist in the context.</span></span> <span data-ttu-id="e2da4-109">Viene eseguita una query sul database quando:</span><span class="sxs-lookup"><span data-stu-id="e2da4-109">A query is executed against the database when:</span></span>  

- <span data-ttu-id="e2da4-110">È stata enumerata da un'istruzione **foreach** (C#) o **For Each** (Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="e2da4-110">It is enumerated by a **foreach** (C#) or **For Each** (Visual Basic) statement.</span></span>  
- <span data-ttu-id="e2da4-111">È stata enumerata da un'operazione di raccolta, ad esempio [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary) o [ToList](https://msdn.microsoft.com/library/bb342261).</span><span class="sxs-lookup"><span data-stu-id="e2da4-111">It is enumerated by a collection operation such as [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary), or [ToList](https://msdn.microsoft.com/library/bb342261).</span></span>  
- <span data-ttu-id="e2da4-112">Operatori LINQ quali [First](https://msdn.microsoft.com/library/bb291976) o [Any](https://msdn.microsoft.com/library/bb337697) vengono specificati nella parte più esterna della query.</span><span class="sxs-lookup"><span data-stu-id="e2da4-112">LINQ operators such as [First](https://msdn.microsoft.com/library/bb291976) or [Any](https://msdn.microsoft.com/library/bb337697) are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="e2da4-113">Vengono chiamati i metodi seguenti: il metodo di estensione [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) su DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx) e Database.ExecuteSqlCommand.</span><span class="sxs-lookup"><span data-stu-id="e2da4-113">The following methods are called: the [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) extension method on a DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx), and Database.ExecuteSqlCommand.</span></span>  

<span data-ttu-id="e2da4-114">Quando vengono restituiti i risultati dal database, gli oggetti che non esistono nel contesto vengono collegati al contesto.</span><span class="sxs-lookup"><span data-stu-id="e2da4-114">When results are returned from the database, objects that do not exist in the context are attached to the context.</span></span> <span data-ttu-id="e2da4-115">Se un oggetto è già presente nel contesto, viene restituito l'oggetto esistente (i valori corrente e originale delle relative proprietà nella voce **non** vengono sovrascritti con i valore del database).</span><span class="sxs-lookup"><span data-stu-id="e2da4-115">If an object is already in the context, the existing object is returned (the current and original values of the object's properties in the entry are **not** overwritten with database values).</span></span>  

<span data-ttu-id="e2da4-116">Quando si esegue una query, le entità aggiunte al contesto ma non ancora salvate nel database non vengono restituite nel set di risultati.</span><span class="sxs-lookup"><span data-stu-id="e2da4-116">When you perform a query, entities that have been added to the context but have not yet been saved to the database are not returned as part of the result set.</span></span> <span data-ttu-id="e2da4-117">Per ottenere i dati presenti nel contesto, vedere [Local Data](~/ef6/querying/local-data.md) (Dati locali).</span><span class="sxs-lookup"><span data-stu-id="e2da4-117">To get the data that is in the context, see [Local Data](~/ef6/querying/local-data.md).</span></span>  

<span data-ttu-id="e2da4-118">Se una query non restituisce righe dal database, il risultato sarà una raccolta vuota, anziché **Null**.</span><span class="sxs-lookup"><span data-stu-id="e2da4-118">If a query returns no rows from the database, the result will be an empty collection, rather than **null**.</span></span>  

## <a name="finding-entities-using-primary-keys"></a><span data-ttu-id="e2da4-119">Ricerca di entità tramite chiavi primarie</span><span class="sxs-lookup"><span data-stu-id="e2da4-119">Finding entities using primary keys</span></span>  

<span data-ttu-id="e2da4-120">Il metodo Find su DbSet usa il valore della chiave primaria per tentare di individuare un'entità rilevata dal contesto.</span><span class="sxs-lookup"><span data-stu-id="e2da4-120">The Find method on DbSet uses the primary key value to attempt to find an entity tracked by the context.</span></span> <span data-ttu-id="e2da4-121">Se l'entità non viene individuata nel contesto, viene inviata una query al database per individuarla al suo interno.</span><span class="sxs-lookup"><span data-stu-id="e2da4-121">If the entity is not found in the context then a query will be sent to the database to find the entity there.</span></span> <span data-ttu-id="e2da4-122">Se l'entità non è presente né nel contesto, né nel database, viene restituito Null.</span><span class="sxs-lookup"><span data-stu-id="e2da4-122">Null is returned if the entity is not found in the context or in the database.</span></span>  

<span data-ttu-id="e2da4-123">Il metodo Find si differenzia dall'uso di una query in due modi:</span><span class="sxs-lookup"><span data-stu-id="e2da4-123">Find is different from using a query in two significant ways:</span></span>  

- <span data-ttu-id="e2da4-124">Un round trip al database viene eseguito solo se l'entità con la chiave specificata non è stata individuata nel contesto.</span><span class="sxs-lookup"><span data-stu-id="e2da4-124">A round-trip to the database will only be made if the entity with the given key is not found in the context.</span></span>  
- <span data-ttu-id="e2da4-125">Find restituisce le entità con lo stato Added,</span><span class="sxs-lookup"><span data-stu-id="e2da4-125">Find will return entities that are in the Added state.</span></span> <span data-ttu-id="e2da4-126">ovvero le entità che sono state aggiunte al contesto, ma che non sono ancora state salvate nel database.</span><span class="sxs-lookup"><span data-stu-id="e2da4-126">That is, Find will return entities that have been added to the context but have not yet been saved to the database.</span></span>  
### <a name="finding-an-entity-by-primary-key"></a><span data-ttu-id="e2da4-127">Ricerca di un'entità in base alla chiave primaria</span><span class="sxs-lookup"><span data-stu-id="e2da4-127">Finding an entity by primary key</span></span>  

<span data-ttu-id="e2da4-128">Il codice seguente illustra alcuni usi del metodo Find:</span><span class="sxs-lookup"><span data-stu-id="e2da4-128">The following code shows some uses of Find:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Will hit the database
    var blog = context.Blogs.Find(3);

    // Will return the same instance without hitting the database
    var blogAgain = context.Blogs.Find(3);

    context.Blogs.Add(new Blog { Id = -1 });

    // Will find the new blog even though it does not exist in the database
    var newBlog = context.Blogs.Find(-1);

    // Will find a User which has a string primary key
    var user = context.Users.Find("johndoe1987");
}
```  

### <a name="finding-an-entity-by-composite-primary-key"></a><span data-ttu-id="e2da4-129">Ricerca di un'entità in base alla chiave primaria composta</span><span class="sxs-lookup"><span data-stu-id="e2da4-129">Finding an entity by composite primary key</span></span>  

<span data-ttu-id="e2da4-130">In Entity Framework le entità possono avere chiavi composte, ovvero chiavi costituite da più di una proprietà.</span><span class="sxs-lookup"><span data-stu-id="e2da4-130">Entity Framework allows your entities to have composite keys - that's a key that is made up of more than one property.</span></span> <span data-ttu-id="e2da4-131">È possibile, ad esempio, avere un'entità BlogSettings che rappresenta le impostazioni di un utente per un particolare blog.</span><span class="sxs-lookup"><span data-stu-id="e2da4-131">For example, you could have a BlogSettings entity that represents a users settings for a particular blog.</span></span> <span data-ttu-id="e2da4-132">Poiché un utente avrà sempre una sola entità BlogSettings per ogni blog, la chiave primaria di BlogSettings può essere una combinazione di ID blog e nome utente.</span><span class="sxs-lookup"><span data-stu-id="e2da4-132">Because a user would only ever have one BlogSettings for each blog you could chose to make the primary key of BlogSettings a combination of BlogId and Username.</span></span> <span data-ttu-id="e2da4-133">Il codice seguente prova a cercare l'entità BlogSettings con ID blog = 3 e nome utente = "johndoe1987":</span><span class="sxs-lookup"><span data-stu-id="e2da4-133">The following code attempts to find the BlogSettings with BlogId = 3 and Username = "johndoe1987":</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

<span data-ttu-id="e2da4-134">Si noti che quando si usano chiavi composte è necessario usare ColumnAttribute o l'API Fluent per specificare l'ordinamento delle proprietà della chiave composta.</span><span class="sxs-lookup"><span data-stu-id="e2da4-134">Note that when you have composite keys you need to use ColumnAttribute or the fluent API to specify an ordering for the properties of the composite key.</span></span> <span data-ttu-id="e2da4-135">La chiamata al metodo Find deve usare quest'ordine quando vengono specificati i valori che compongono la chiave.</span><span class="sxs-lookup"><span data-stu-id="e2da4-135">The call to Find must use this order when specifying the values that form the key.</span></span>  
