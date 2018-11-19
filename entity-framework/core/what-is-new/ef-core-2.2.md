---
title: Nuove funzionalità di EF Core 2.2 - EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: 79b4efc3aee23e19a9ea1deb6373b9984b77f886
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688759"
---
# <a name="new-features-in-ef-core-22"></a><span data-ttu-id="2740d-102">Nuove funzionalità di EF Core 2.2</span><span class="sxs-lookup"><span data-stu-id="2740d-102">New features in EF Core 2.2</span></span>

## <a name="spatial-data-support"></a><span data-ttu-id="2740d-103">Supporto di dati spaziali</span><span class="sxs-lookup"><span data-stu-id="2740d-103">Spatial data support</span></span>

<span data-ttu-id="2740d-104">Si possono usare dati spaziali per rappresentare la posizione fisica e la forma degli oggetti.</span><span class="sxs-lookup"><span data-stu-id="2740d-104">Spatial data can be used to represent the physical location and shape of objects.</span></span>
<span data-ttu-id="2740d-105">Molti database possono archiviare, indicizzare ed eseguire query sui dati spaziali in modo nativo.</span><span class="sxs-lookup"><span data-stu-id="2740d-105">Many databases can natively store, index, and query spatial data.</span></span> <span data-ttu-id="2740d-106">Sono esempi di scenari comuni l'esecuzione di query per recuperare gli oggetti entro una distanza specificata e il test della presenza di una posizione specifica in un poligono.</span><span class="sxs-lookup"><span data-stu-id="2740d-106">Common scenarios include querying for objects within a given distance, and testing if a polygon contains a given location.</span></span>
<span data-ttu-id="2740d-107">EF Core 2.2 supporta ora l'utilizzo dei dati spaziali da vari database usando i tipi della libreria [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS).</span><span class="sxs-lookup"><span data-stu-id="2740d-107">EF Core 2.2 now supports working with spatial data from various databases using types from the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS) library.</span></span>

<span data-ttu-id="2740d-108">Il supporto dei dati spaziali viene implementato come una serie di pacchetti di estensione specifici del provider.</span><span class="sxs-lookup"><span data-stu-id="2740d-108">Spatial data support is implemented as a series of provider-specific extension packages.</span></span>
<span data-ttu-id="2740d-109">Ognuno di questi pacchetti fornisce i mapping per tipi e i metodi NTS, nonché i tipi e le funzioni spaziali corrispondenti nel database.</span><span class="sxs-lookup"><span data-stu-id="2740d-109">Each of these packages contributes mappings for NTS types and methods, and the corresponding spatial types and functions in the database.</span></span>
<span data-ttu-id="2740d-110">Queste estensioni del provider sono ora disponibili per [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/) e [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (dal [progetto Npgsql](http://www.npgsql.org/)).</span><span class="sxs-lookup"><span data-stu-id="2740d-110">Such provider extensions are now available for [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), and [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (from the [Npgsql project](http://www.npgsql.org/)).</span></span>
<span data-ttu-id="2740d-111">I tipi spaziali possono essere usati direttamente con il [provider in memoria EF Core](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) senza estensioni aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="2740d-111">Spatial types can be used directly with the [EF Core in-memory provider](https://docs.microsoft.com/en-us/ef/core/providers/in-memory/) without additional extensions.</span></span>

<span data-ttu-id="2740d-112">Dopo aver installato l'estensione del provider, è possibile aggiungere proprietà dei tipi supportati alle entità.</span><span class="sxs-lookup"><span data-stu-id="2740d-112">Once the provider extension is installed, you can add properties of supported types to your entities.</span></span> <span data-ttu-id="2740d-113">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="2740d-113">For example:</span></span>

``` csharp
using NetTopologySuite.Geometries;

namespace MyApp
{
  public class Friend
  {
    [Key]
    public string Name { get; set; }
  
    [Required]
    public Point Location { get; set; }
  }
}
``` 

<span data-ttu-id="2740d-114">È quindi possibile rendere persistenti le entità con dati spaziali:</span><span class="sxs-lookup"><span data-stu-id="2740d-114">You can then persist entities with spatial data:</span></span>

``` csharp
using (var context = new MyDbContext())
{
    context.Add(
        new Friend
        {
            Name = "Bill",
            Location = new Point(-122.34877, 47.6233355) {SRID = 4326 }
        });
    context.SaveChanges();
}
```
<span data-ttu-id="2740d-115">Ed è possibile eseguire query di database basate su operazioni e dati spaziali:</span><span class="sxs-lookup"><span data-stu-id="2740d-115">And you can execute database queries based on spatial data and operations:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="2740d-116">Per altre informazioni su questa funzionalità, vedere la [documentazione sui tipi spaziali](xref:core/modeling/spatial).</span><span class="sxs-lookup"><span data-stu-id="2740d-116">For more information on this feature, see the [spatial types documentation](xref:core/modeling/spatial).</span></span> 

## <a name="collections-of-owned-entities"></a><span data-ttu-id="2740d-117">Raccolte di entità di proprietà</span><span class="sxs-lookup"><span data-stu-id="2740d-117">Collections of owned entities</span></span>

<span data-ttu-id="2740d-118">In EF Core 2.0 è stata aggiunta la possibilità di modellare la proprietà nelle associazioni uno-a-uno.</span><span class="sxs-lookup"><span data-stu-id="2740d-118">EF Core 2.0 added the ability to model ownership in one-to-one associations.</span></span>
<span data-ttu-id="2740d-119">EF Core 2.2 estende la possibilità di esprimere la proprietà alle associazioni uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="2740d-119">EF Core 2.2 extends the ability to express ownership to one-to-many associations.</span></span>
<span data-ttu-id="2740d-120">La proprietà consente di vincolare l'uso delle entità.</span><span class="sxs-lookup"><span data-stu-id="2740d-120">Ownership helps constrain how entities are used.</span></span>

<span data-ttu-id="2740d-121">Ad esempio, le entità di proprietà:</span><span class="sxs-lookup"><span data-stu-id="2740d-121">For example, owned entities:</span></span>
- <span data-ttu-id="2740d-122">Possono comparire solo nelle proprietà di navigazione di altri tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="2740d-122">Can only ever appear on navigation properties of other entity types.</span></span> 
- <span data-ttu-id="2740d-123">Vengono caricate automaticamente e possono essere gestite solo da un DbContext insieme al proprietario.</span><span class="sxs-lookup"><span data-stu-id="2740d-123">Are automatically loaded, and can only be tracked by a DbContext alongside their owner.</span></span>

<span data-ttu-id="2740d-124">Nei database relazionali, le raccolte di proprietà vengono mappate a tabelle separate dal proprietario, proprio come le normali associazioni uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="2740d-124">In relational databases, owned collections are mapped to separate tables from the owner, just like regular one-to-many associations.</span></span>
<span data-ttu-id="2740d-125">Tuttavia, nei database orientati ai documenti è previsto l'annidamento delle entità di proprietà (in raccolte di proprietà o riferimenti) all'interno dello stesso documento del proprietario.</span><span class="sxs-lookup"><span data-stu-id="2740d-125">But in document-oriented databases, we plan to nest owned entities (in owned collections or references) within the same document as the owner.</span></span>

<span data-ttu-id="2740d-126">È possibile usare la funzionalità chiamando la nuova API OwnsMany():</span><span class="sxs-lookup"><span data-stu-id="2740d-126">You can use the feature by calling the new OwnsMany() API:</span></span>

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

<span data-ttu-id="2740d-127">Per altre informazioni, vedere la [documentazione aggiornata sulle entità di proprietà](xref:core/modeling/owned-entities#collections-of-owned-types).</span><span class="sxs-lookup"><span data-stu-id="2740d-127">For more information, see the [updated owned entities documentation](xref:core/modeling/owned-entities#collections-of-owned-types).</span></span>

## <a name="query-tags"></a><span data-ttu-id="2740d-128">Tag delle query</span><span class="sxs-lookup"><span data-stu-id="2740d-128">Query tags</span></span>

<span data-ttu-id="2740d-129">Questa funzionalità semplifica la correlazione di query LINQ nel codice con query SQL generate acquisite nei log.</span><span class="sxs-lookup"><span data-stu-id="2740d-129">This feature simplifies the correlation of LINQ queries in code with generated SQL queries captured in logs.</span></span>

<span data-ttu-id="2740d-130">Per sfruttare i vantaggi dei tag delle query, è possibile annotare una query LINQ con il nuovo metodo TagWith().</span><span class="sxs-lookup"><span data-stu-id="2740d-130">To take advantage of query tags, you annotate a LINQ query using the new TagWith() method.</span></span>
<span data-ttu-id="2740d-131">Se si usa la query spaziale da un esempio precedente:</span><span class="sxs-lookup"><span data-stu-id="2740d-131">Using the spatial query from a previous example:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="2740d-132">Questa query LINQ produrrà l'output SQL seguente:</span><span class="sxs-lookup"><span data-stu-id="2740d-132">This LINQ query will produce the following SQL output:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="2740d-133">Per altre informazioni, vedere la [documentazione sui tag delle query](xref:core/querying/tags).</span><span class="sxs-lookup"><span data-stu-id="2740d-133">For more information, see the [query tags documentation](xref:core/querying/tags).</span></span> 