---
title: Query SQL non elaborate - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: d8f52edfdf4bd7776ab8d81185c867cbfd7bcf44
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813601"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="b2295-102">Query SQL non elaborate</span><span class="sxs-lookup"><span data-stu-id="b2295-102">Raw SQL Queries</span></span>

<span data-ttu-id="b2295-103">Entity Framework Core consente di ricorrere a query SQL non elaborate quando si lavora con un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="b2295-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="b2295-104">Questa operazione può essere utile se la query che si desidera eseguire non può essere espressa mediante LINQ o se l'utilizzo di una query LINQ produce una query SQL inefficiente.</span><span class="sxs-lookup"><span data-stu-id="b2295-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="b2295-105">Le query SQL non elaborate possono restituire tipi di entità regolari o [tipi di entità autochiave](xref:core/modeling/keyless-entity-types) che fanno parte del modello.</span><span class="sxs-lookup"><span data-stu-id="b2295-105">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="b2295-106">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="b2295-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="b2295-107">Query SQL non elaborate di base</span><span class="sxs-lookup"><span data-stu-id="b2295-107">Basic raw SQL queries</span></span>

<span data-ttu-id="b2295-108">È possibile utilizzare il `FromSqlRaw` metodo di estensione per iniziare una query LINQ basata su una query SQL non elaborata.</span><span class="sxs-lookup"><span data-stu-id="b2295-108">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="b2295-109">Per eseguire una stored procedure, è possibile usare query SQL non elaborate.</span><span class="sxs-lookup"><span data-stu-id="b2295-109">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="b2295-110">Passaggio di parametri</span><span class="sxs-lookup"><span data-stu-id="b2295-110">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="b2295-111">**Usa sempre la parametrizzazione per query SQL non elaborate**</span><span class="sxs-lookup"><span data-stu-id="b2295-111">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="b2295-112">Quando si introducono valori forniti dall'utente in una query SQL non elaborata, è necessario prestare attenzione per evitare attacchi intrusivi nel codice SQL.</span><span class="sxs-lookup"><span data-stu-id="b2295-112">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="b2295-113">Oltre a convalidare che tali valori non contengono caratteri non validi, utilizzare sempre la parametrizzazione che invia i valori separati dal testo SQL.</span><span class="sxs-lookup"><span data-stu-id="b2295-113">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="b2295-114">In particolare, non passare mai una stringa concatenata o interpolata (`$""`) con valori specificati dall'utente non convalidati in `FromSqlRaw` o `ExecuteSqlRaw`.</span><span class="sxs-lookup"><span data-stu-id="b2295-114">In particular, never pass a concatenated or interpolated string (`$""`) with unvalidated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="b2295-115">I `FromSqlInterpolated` metodi `ExecuteSqlInterpolated` e consentono di utilizzare la sintassi di interpolazione delle stringhe in modo da proteggere gli attacchi intrusivi nel codice SQL.</span><span class="sxs-lookup"><span data-stu-id="b2295-115">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="b2295-116">Nell'esempio seguente viene passato un singolo parametro a un stored procedure includendo un segnaposto di parametro nella stringa di query SQL e fornendo un argomento aggiuntivo.</span><span class="sxs-lookup"><span data-stu-id="b2295-116">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="b2295-117">Sebbene possa sembrare una sintassi `String.Format` simile, il valore fornito viene racchiuso in `DbParameter` un oggetto e il nome del parametro generato `{0}` viene inserito nel punto in cui è stato specificato il segnaposto.</span><span class="sxs-lookup"><span data-stu-id="b2295-117">While this may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="b2295-118">In alternativa a `FromSqlRaw`, è possibile usare `FromSqlInterpolated` che consente l'uso sicuro dell'interpolazione di stringhe.</span><span class="sxs-lookup"><span data-stu-id="b2295-118">As an alternative to `FromSqlRaw`, you can use `FromSqlInterpolated` which allows the safe use of string interpolation.</span></span> <span data-ttu-id="b2295-119">Come nell'esempio precedente, il valore viene convertito in un oggetto `DbParameter` e pertanto non è vulnerabile a SQL injection:</span><span class="sxs-lookup"><span data-stu-id="b2295-119">As with the previous example, the value is converted to a `DbParameter` and is therefore not vulnerable to SQL injection:</span></span>

> [!NOTE]
> <span data-ttu-id="b2295-120">Prima della versione 3,0 `FromSqlRaw` ed `FromSqlInterpolated` erano due overload denominati `FromSql`.</span><span class="sxs-lookup"><span data-stu-id="b2295-120">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="b2295-121">Per ulteriori informazioni, vedere la [sezione versioni precedenti](#previous-versions) .</span><span class="sxs-lookup"><span data-stu-id="b2295-121">See the [previous versions section](#previous-versions) for more details.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlInterpolated($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="b2295-122">È anche possibile costruire un DbParameter e fornirlo come valore del parametro.</span><span class="sxs-lookup"><span data-stu-id="b2295-122">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="b2295-123">Poiché viene usato un segnaposto di parametro SQL normale, anziché un segnaposto `FromSqlRaw` di stringa, può essere usato in modo sicuro:</span><span class="sxs-lookup"><span data-stu-id="b2295-123">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

<span data-ttu-id="b2295-124">In questo modo è possibile usare parametri denominati nella stringa di query SQL, operazione che risulta utile quando una stored procedure include parametri facoltativi:</span><span class="sxs-lookup"><span data-stu-id="b2295-124">This allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="b2295-125">Composizione con LINQ</span><span class="sxs-lookup"><span data-stu-id="b2295-125">Composing with LINQ</span></span>

<span data-ttu-id="b2295-126">Se la query SQL può essere composta nel database, è possibile estendere la query SQL non elaborata iniziale usando operatori LINQ.</span><span class="sxs-lookup"><span data-stu-id="b2295-126">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="b2295-127">Le query SQL componibili iniziano con la parola chiave `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="b2295-127">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="b2295-128">L'esempio seguente usa una query SQL non elaborata che consente di effettuare una selezione da una funzione con valori di tabella e quindi di usare la composizione con LINQ per eseguire operazioni di filtro e ordinamento.</span><span class="sxs-lookup"><span data-stu-id="b2295-128">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

<span data-ttu-id="b2295-129">Questa operazione produrrà la query SQL seguente:</span><span class="sxs-lookup"><span data-stu-id="b2295-129">This will produce the following SQL query:</span></span>

``` sql
SELECT [b].[Id], [b].[Name], [b].[Rating]
        FROM (
            SELECT * FROM dbo.SearchBlogs(@p0)
        ) AS b
        WHERE b."Rating" > 3
        ORDER BY b."Rating" DESC
```

## <a name="change-tracking"></a><span data-ttu-id="b2295-130">Rilevamento modifiche</span><span class="sxs-lookup"><span data-stu-id="b2295-130">Change Tracking</span></span>

<span data-ttu-id="b2295-131">Le query che utilizzano `FromSql` i metodi seguono esattamente le stesse regole di rilevamento delle modifiche di qualsiasi altra query LINQ in EF core.</span><span class="sxs-lookup"><span data-stu-id="b2295-131">Queries that use the `FromSql` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="b2295-132">Se ad esempio la query proietta tipi di entità, i risultati vengono rilevati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="b2295-132">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="b2295-133">Nell'esempio seguente viene utilizzata una query SQL non elaborata che seleziona da una funzione con valori di tabella (TVF), quindi Disabilita il rilevamento delle modifiche `AsNoTracking`con la chiamata a:</span><span class="sxs-lookup"><span data-stu-id="b2295-133">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a><span data-ttu-id="b2295-134">Inclusione di dati correlati</span><span class="sxs-lookup"><span data-stu-id="b2295-134">Including related data</span></span>

<span data-ttu-id="b2295-135">Il metodo `Include` può essere usato per includere dati correlati, come in qualsiasi altra query LINQ:</span><span class="sxs-lookup"><span data-stu-id="b2295-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

<span data-ttu-id="b2295-136">Si noti che questa operazione richiede che la query SQL non elaborata sia componibile; in particolare, non funzionerà con le chiamate di stored procedure.</span><span class="sxs-lookup"><span data-stu-id="b2295-136">Note that this requires your raw SQL query to be composable; it will notably not work with stored procedure calls.</span></span> <span data-ttu-id="b2295-137">Vedere note sulla composizione in [limitazioni](#limitations).</span><span class="sxs-lookup"><span data-stu-id="b2295-137">See notes on composability under [Limitations](#limitations)).</span></span>

## <a name="limitations"></a><span data-ttu-id="b2295-138">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="b2295-138">Limitations</span></span>

<span data-ttu-id="b2295-139">Esistono alcune limitazioni da tenere presenti quando si usano query SQL non elaborate:</span><span class="sxs-lookup"><span data-stu-id="b2295-139">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="b2295-140">La query SQL deve restituire i dati per tutte le proprietà del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="b2295-140">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="b2295-141">I nomi delle colonne nel set di risultati devono corrispondere ai nomi di colonna a cui sono mappate le proprietà.</span><span class="sxs-lookup"><span data-stu-id="b2295-141">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="b2295-142">Si noti che questo comportamento è diverso da EF6 in cui il mapping di proprietà/colonne viene ignorato per le query SQL non elaborate e i nomi di colonna del set di risultati devono corrispondere ai nomi di proprietà.</span><span class="sxs-lookup"><span data-stu-id="b2295-142">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="b2295-143">La query SQL non può contenere dati correlati.</span><span class="sxs-lookup"><span data-stu-id="b2295-143">The SQL query cannot contain related data.</span></span> <span data-ttu-id="b2295-144">Tuttavia, in molti casi è possibile estendere la query usando l'operatore `Include` per restituire i dati correlati (vedere [Inclusione di dati correlati](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="b2295-144">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="b2295-145">Le istruzioni `SELECT` passate a questo metodo devono essere in genere componibili: Se EF Core necessario valutare gli operatori di query aggiuntivi sul server (ad esempio, per tradurre gli operatori LINQ applicati `FromSql` dopo i metodi), il SQL fornito verrà considerato come una sottoquery.</span><span class="sxs-lookup"><span data-stu-id="b2295-145">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql` methods), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="b2295-146">Questo significa che le istruzioni SQL passate non devono contenere caratteri o opzioni non validi in una sottoquery, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b2295-146">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="b2295-147">un punto e virgola finale</span><span class="sxs-lookup"><span data-stu-id="b2295-147">A trailing semicolon</span></span>
  * <span data-ttu-id="b2295-148">In SQL Server, un hint a livello di query finale, ad esempio `OPTION (HASH JOIN)`</span><span class="sxs-lookup"><span data-stu-id="b2295-148">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="b2295-149">In SQL Server, una clausola `ORDER BY` non accompagnata da `OFFSET 0` o `TOP 100 PERCENT` nella clausola `SELECT`</span><span class="sxs-lookup"><span data-stu-id="b2295-149">On SQL Server, an `ORDER BY` clause that is not accompanied of `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="b2295-150">Si noti che SQL Server non consente la composizione su chiamate stored procedure, quindi qualsiasi tentativo di applicare operatori di query aggiuntivi a tale chiamata genererà SQL non valido.</span><span class="sxs-lookup"><span data-stu-id="b2295-150">Note that SQL Server does not allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="b2295-151">Gli operatori di query possono essere `AsEnumerable()` introdotti dopo per la valutazione del client.</span><span class="sxs-lookup"><span data-stu-id="b2295-151">Query operators may be introduced after `AsEnumerable()` for client evaluation.</span></span>

## <a name="previous-versions"></a><span data-ttu-id="b2295-152">Versioni precedenti</span><span class="sxs-lookup"><span data-stu-id="b2295-152">Previous versions</span></span>

<span data-ttu-id="b2295-153">EF Core versione 2,2 e versioni precedenti hanno due overload denominati `FromSql` che si comportavano nello stesso modo dei più recenti `FromSqlInterpolated` `FromSqlRaw` e.</span><span class="sxs-lookup"><span data-stu-id="b2295-153">EF Core version 2.2 and earlier had two overloads named `FromSql` which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="b2295-154">In questo modo è molto semplice chiamare accidentalmente il metodo stringa non elaborata quando lo scopo era quello di chiamare il metodo della stringa interpolata e viceversa.</span><span class="sxs-lookup"><span data-stu-id="b2295-154">This made it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="b2295-155">Il risultato potrebbero essere query senza parametri, quando invece è prevista la parametrizzazione.</span><span class="sxs-lookup"><span data-stu-id="b2295-155">This could result in queries not being parameterized when they should have been.</span></span>
