---
title: Query SQL non elaborate - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 21cb688d6775039def3b0be12768da71b5d96531
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997144"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="567ff-102">Query SQL non elaborate</span><span class="sxs-lookup"><span data-stu-id="567ff-102">Raw SQL Queries</span></span>

<span data-ttu-id="567ff-103">Entity Framework Core consente di ricorrere a query SQL non elaborate quando si lavora con un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="567ff-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="567ff-104">Ciò può essere utile se la query da eseguire non può essere espressa usando LINQ oppure se l'uso di una query LINQ comporta l'invio di codice SQL non efficiente al database.</span><span class="sxs-lookup"><span data-stu-id="567ff-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL being sent to the database.</span></span> <span data-ttu-id="567ff-105">Le query SQL non elaborate possono restituire tipi di entità o, a partire da EF Core 2.1, [tipi di query](xref:core/modeling/query-types) che fanno parte del modello.</span><span class="sxs-lookup"><span data-stu-id="567ff-105">Raw SQL queries can return entity types or, starting with EF Core 2.1, [query types](xref:core/modeling/query-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="567ff-106">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="567ff-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="567ff-107">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="567ff-107">Limitations</span></span>

<span data-ttu-id="567ff-108">Esistono alcune limitazioni da tenere presenti quando si usano query SQL non elaborate:</span><span class="sxs-lookup"><span data-stu-id="567ff-108">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="567ff-109">La query SQL deve restituire i dati per tutte le proprietà del tipo di entità o query.</span><span class="sxs-lookup"><span data-stu-id="567ff-109">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="567ff-110">I nomi delle colonne nel set di risultati devono corrispondere ai nomi di colonna a cui sono mappate le proprietà.</span><span class="sxs-lookup"><span data-stu-id="567ff-110">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="567ff-111">Si noti che questo comportamento è diverso da EF6 in cui il mapping di proprietà/colonne viene ignorato per le query SQL non elaborate e i nomi di colonna del set di risultati devono corrispondere ai nomi di proprietà.</span><span class="sxs-lookup"><span data-stu-id="567ff-111">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="567ff-112">La query SQL non può contenere dati correlati.</span><span class="sxs-lookup"><span data-stu-id="567ff-112">The SQL query cannot contain related data.</span></span> <span data-ttu-id="567ff-113">Tuttavia, in molti casi è possibile estendere la query usando l'operatore `Include` per restituire i dati correlati (vedere [Inclusione di dati correlati](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="567ff-113">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="567ff-114">Le istruzioni `SELECT` passate a questo metodo devono essere in genere componibili. Se EF Core deve valutare operatori di query aggiuntivi nel server (ad esempio, per convertire gli operatori LINQ applicati dopo `FromSql`), le istruzioni SQL fornite verranno considerate una sottoquery.</span><span class="sxs-lookup"><span data-stu-id="567ff-114">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="567ff-115">Questo significa che le istruzioni SQL passate non devono contenere caratteri o opzioni non validi in una sottoquery, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="567ff-115">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="567ff-116">Un punto e virgola finale</span><span class="sxs-lookup"><span data-stu-id="567ff-116">a trailing semicolon</span></span>
  * <span data-ttu-id="567ff-117">In SQL Server, un hint a livello di query finale, ad esempio `OPTION (HASH JOIN)`</span><span class="sxs-lookup"><span data-stu-id="567ff-117">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="567ff-118">In SQL Server, una clausola `ORDER BY` non accompagnata da `TOP 100 PERCENT` nella clausola `SELECT`</span><span class="sxs-lookup"><span data-stu-id="567ff-118">On SQL Server, an `ORDER BY` clause that is not accompanied of `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="567ff-119">Le istruzioni SQL diverse da `SELECT` vengono riconosciute automaticamente come non componibili.</span><span class="sxs-lookup"><span data-stu-id="567ff-119">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="567ff-120">Di conseguenza, i risultati completi delle stored procedure vengono sempre restituiti al client e tutti gli operatori LINQ applicati dopo `FromSql` vengono valutati in memoria.</span><span class="sxs-lookup"><span data-stu-id="567ff-120">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="567ff-121">Query SQL non elaborate di base</span><span class="sxs-lookup"><span data-stu-id="567ff-121">Basic raw SQL queries</span></span>

<span data-ttu-id="567ff-122">È possibile usare il metodo di estensione *FromSql* per avviare una query LINQ in base a una query SQL non elaborata.</span><span class="sxs-lookup"><span data-stu-id="567ff-122">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="567ff-123">Per eseguire una stored procedure, è possibile usare query SQL non elaborate.</span><span class="sxs-lookup"><span data-stu-id="567ff-123">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="567ff-124">Passaggio di parametri</span><span class="sxs-lookup"><span data-stu-id="567ff-124">Passing parameters</span></span>

<span data-ttu-id="567ff-125">Come con qualsiasi API che accetta SQL, è importante parametrizzare qualsiasi input utente per la protezione da attacchi SQL injection.</span><span class="sxs-lookup"><span data-stu-id="567ff-125">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="567ff-126">Si possono includere segnaposto dei parametri nella stringa di query SQL e quindi fornire i valori dei parametri come argomenti aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="567ff-126">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="567ff-127">I valori dei parametri forniti verranno convertiti automaticamente in `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="567ff-127">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="567ff-128">L'esempio seguente passa un parametro singolo a una stored procedure.</span><span class="sxs-lookup"><span data-stu-id="567ff-128">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="567ff-129">Anche se apparentemente sembra sintassi `String.Format`, per il valore fornito viene eseguito il wrapping in un parametro e il nome di parametro generato viene inserito nella posizione in cui è stato specificato il segnaposto `{0}`.</span><span class="sxs-lookup"><span data-stu-id="567ff-129">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="567ff-130">Questa è la stessa query ma usa la sintassi di interpolazione di stringa, supportata in EF Core 2.0 e versioni successive:</span><span class="sxs-lookup"><span data-stu-id="567ff-130">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="567ff-131">È anche possibile costruire un DbParameter e fornirlo come valore del parametro.</span><span class="sxs-lookup"><span data-stu-id="567ff-131">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="567ff-132">In questo modo è possibile usare parametri denominati nella stringa di query SQL</span><span class="sxs-lookup"><span data-stu-id="567ff-132">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="567ff-133">Composizione con LINQ</span><span class="sxs-lookup"><span data-stu-id="567ff-133">Composing with LINQ</span></span>

<span data-ttu-id="567ff-134">Se la query SQL può essere composta nel database, è possibile estendere la query SQL non elaborata iniziale usando operatori LINQ.</span><span class="sxs-lookup"><span data-stu-id="567ff-134">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="567ff-135">Nelle query SQL componibili è possibile usare la parola chiave `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="567ff-135">SQL queries that can be composed on being with the `SELECT` keyword.</span></span>

<span data-ttu-id="567ff-136">L'esempio seguente usa una query SQL non elaborata che consente di effettuare una selezione da una funzione con valori di tabella e quindi di usare la composizione con LINQ per eseguire operazioni di filtro e ordinamento.</span><span class="sxs-lookup"><span data-stu-id="567ff-136">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="567ff-137">Inclusione di dati correlati</span><span class="sxs-lookup"><span data-stu-id="567ff-137">Including related data</span></span>

<span data-ttu-id="567ff-138">La composizione con operatori LINQ può essere usata per includere dati correlati nella query.</span><span class="sxs-lookup"><span data-stu-id="567ff-138">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="567ff-139">**Usare sempre la parametrizzazione per query SQL non elaborate:** le API che accettano una stringa SQL non elaborata come `FromSql` e `ExecuteSqlCommand` consentono di passare facilmente i valori come parametri.</span><span class="sxs-lookup"><span data-stu-id="567ff-139">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="567ff-140">Oltre a convalidare l'input dell'utente, usare sempre la parametrizzazione per qualsiasi valore usato in query/comandi SQL non elaborati.</span><span class="sxs-lookup"><span data-stu-id="567ff-140">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="567ff-141">Se si usa la concatenazione di stringhe per compilare in modo dinamico qualsiasi parte della stringa di query, si è responsabili della convalida di qualsiasi input per la protezione da attacchi SQL injection.</span><span class="sxs-lookup"><span data-stu-id="567ff-141">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
