---
title: Query SQL non elaborate - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 7a0df6fb656be58103971f45b9e12e9f1383311f
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921711"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="ba1e8-102">Query SQL non elaborate</span><span class="sxs-lookup"><span data-stu-id="ba1e8-102">Raw SQL Queries</span></span>

<span data-ttu-id="ba1e8-103">Entity Framework Core consente di ricorrere a query SQL non elaborate quando si lavora con un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="ba1e8-104">Ciò può essere utile se la query da eseguire non può essere espressa usando LINQ oppure se l'uso di una query LINQ comporta l'invio di query SQL non efficienti.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL queries.</span></span> <span data-ttu-id="ba1e8-105">Le query SQL non elaborate possono restituire tipi di entità o, a partire da EF Core 2.1, [tipi di query](xref:core/modeling/query-types) che fanno parte del modello.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-105">Raw SQL queries can return entity types or, starting with EF Core 2.1, [query types](xref:core/modeling/query-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="ba1e8-106">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="ba1e8-107">Query SQL non elaborate di base</span><span class="sxs-lookup"><span data-stu-id="ba1e8-107">Basic raw SQL queries</span></span>

<span data-ttu-id="ba1e8-108">È possibile usare il metodo di estensione *FromSql* per avviare una query LINQ in base a una query SQL non elaborata.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-108">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="ba1e8-109">Per eseguire una stored procedure, è possibile usare query SQL non elaborate.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-109">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="ba1e8-110">Passaggio di parametri</span><span class="sxs-lookup"><span data-stu-id="ba1e8-110">Passing parameters</span></span>

<span data-ttu-id="ba1e8-111">Come con qualsiasi API che accetta SQL, è importante parametrizzare qualsiasi input utente per la protezione da attacchi SQL injection.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-111">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="ba1e8-112">Si possono includere segnaposto dei parametri nella stringa di query SQL e quindi fornire i valori dei parametri come argomenti aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-112">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="ba1e8-113">I valori dei parametri forniti verranno convertiti automaticamente in `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-113">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="ba1e8-114">L'esempio seguente passa un parametro singolo a una stored procedure.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-114">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="ba1e8-115">Anche se apparentemente sembra sintassi `String.Format`, per il valore fornito viene eseguito il wrapping in un parametro e il nome di parametro generato viene inserito nella posizione in cui è stato specificato il segnaposto `{0}`.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-115">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="ba1e8-116">Questa è la stessa query ma usa la sintassi di interpolazione di stringa, supportata in EF Core 2.0 e versioni successive:</span><span class="sxs-lookup"><span data-stu-id="ba1e8-116">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="ba1e8-117">È anche possibile costruire un DbParameter e fornirlo come valore del parametro:</span><span class="sxs-lookup"><span data-stu-id="ba1e8-117">You can also construct a DbParameter and supply it as a parameter value:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

<span data-ttu-id="ba1e8-118">In questo modo è possibile usare parametri denominati nella stringa di query SQL, operazione che risulta utile quando una stored procedure include parametri facoltativi:</span><span class="sxs-lookup"><span data-stu-id="ba1e8-118">This allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="ba1e8-119">Composizione con LINQ</span><span class="sxs-lookup"><span data-stu-id="ba1e8-119">Composing with LINQ</span></span>

<span data-ttu-id="ba1e8-120">Se la query SQL può essere composta nel database, è possibile estendere la query SQL non elaborata iniziale usando operatori LINQ.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-120">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="ba1e8-121">Le query SQL componibili iniziano con la parola chiave `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-121">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="ba1e8-122">L'esempio seguente usa una query SQL non elaborata che consente di effettuare una selezione da una funzione con valori di tabella e quindi di usare la composizione con LINQ per eseguire operazioni di filtro e ordinamento.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-122">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

## <a name="change-tracking"></a><span data-ttu-id="ba1e8-123">Change Tracking</span><span class="sxs-lookup"><span data-stu-id="ba1e8-123">Change Tracking</span></span>

<span data-ttu-id="ba1e8-124">Le query che usano `FromSql()` osservano le stesse regole di rilevamento modifiche di qualsiasi altra query LINQ in EF Core.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-124">Queries that use the `FromSql()` follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="ba1e8-125">Se ad esempio la query proietta tipi di entità, i risultati vengono rilevati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-125">For example, if the query projects entity types, the results will be tracked by default.</span></span>  

<span data-ttu-id="ba1e8-126">L'esempio seguente usa una query SQL non elaborata che effettua selezioni da una funzione con valori di tabella (TVF), quindi disabilita il rilevamento delle modifiche mediante la chiamata di .AsNoTracking():</span><span class="sxs-lookup"><span data-stu-id="ba1e8-126">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to .AsNoTracking():</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a><span data-ttu-id="ba1e8-127">Inclusione di dati correlati</span><span class="sxs-lookup"><span data-stu-id="ba1e8-127">Including related data</span></span>

<span data-ttu-id="ba1e8-128">Il metodo `Include()` può essere usato per includere dati correlati, come in qualsiasi altra query LINQ:</span><span class="sxs-lookup"><span data-stu-id="ba1e8-128">The `Include()` method can be used to include related data, just like with any other LINQ query:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

## <a name="limitations"></a><span data-ttu-id="ba1e8-129">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="ba1e8-129">Limitations</span></span>

<span data-ttu-id="ba1e8-130">Esistono alcune limitazioni da tenere presenti quando si usano query SQL non elaborate:</span><span class="sxs-lookup"><span data-stu-id="ba1e8-130">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="ba1e8-131">La query SQL deve restituire i dati per tutte le proprietà del tipo di entità o query.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-131">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="ba1e8-132">I nomi delle colonne nel set di risultati devono corrispondere ai nomi di colonna a cui sono mappate le proprietà.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-132">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="ba1e8-133">Si noti che questo comportamento è diverso da EF6 in cui il mapping di proprietà/colonne viene ignorato per le query SQL non elaborate e i nomi di colonna del set di risultati devono corrispondere ai nomi di proprietà.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-133">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="ba1e8-134">La query SQL non può contenere dati correlati.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-134">The SQL query cannot contain related data.</span></span> <span data-ttu-id="ba1e8-135">Tuttavia, in molti casi è possibile estendere la query usando l'operatore `Include` per restituire i dati correlati (vedere [Inclusione di dati correlati](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="ba1e8-135">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="ba1e8-136">Le istruzioni `SELECT` passate a questo metodo devono essere in genere componibili: se EF Core deve valutare operatori di query aggiuntivi nel server (ad esempio, per convertire gli operatori LINQ applicati dopo `FromSql`), le istruzioni SQL fornite verranno considerate una sottoquery.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-136">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="ba1e8-137">Questo significa che le istruzioni SQL passate non devono contenere caratteri o opzioni non validi in una sottoquery, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ba1e8-137">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="ba1e8-138">Un punto e virgola finale</span><span class="sxs-lookup"><span data-stu-id="ba1e8-138">a trailing semicolon</span></span>
  * <span data-ttu-id="ba1e8-139">In SQL Server, un hint a livello di query finale, ad esempio `OPTION (HASH JOIN)`</span><span class="sxs-lookup"><span data-stu-id="ba1e8-139">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="ba1e8-140">In SQL Server, una clausola `ORDER BY` non accompagnata da `OFFSET 0` o `TOP 100 PERCENT` nella clausola `SELECT`</span><span class="sxs-lookup"><span data-stu-id="ba1e8-140">On SQL Server, an `ORDER BY` clause that is not accompanied of `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="ba1e8-141">Le istruzioni SQL diverse da `SELECT` vengono riconosciute automaticamente come non componibili.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-141">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="ba1e8-142">Di conseguenza, i risultati completi delle stored procedure vengono sempre restituiti al client e tutti gli operatori LINQ applicati dopo `FromSql` vengono valutati in memoria.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-142">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span>

> [!WARNING]  
> <span data-ttu-id="ba1e8-143">**Usare sempre la parametrizzazione per le query SQL non elaborate:** Oltre a convalidare l'input dell'utente, usare sempre la parametrizzazione per qualsiasi valore usato in query/comandi SQL non elaborati.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-143">**Always use parameterization for raw SQL queries:** In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="ba1e8-144">le API che accettano una stringa SQL non elaborata come `FromSql` e `ExecuteSqlCommand` consentono di passare facilmente i valori come parametri.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-144">APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="ba1e8-145">Gli overload di `FromSql` e `ExecuteSqlCommand` che accettano FormattableString consentono anche di usare la sintassi dell'interpolazione di stringhe in modo da proteggersi dagli attacchi SQL injection.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-145">Overloads of `FromSql` and `ExecuteSqlCommand` that accept FormattableString also allow using string interpolation syntax in a way that helps protect against SQL injection attacks.</span></span> 
> 
> <span data-ttu-id="ba1e8-146">Se si usa la concatenazione o l'interpolazione di stringhe per creare dinamicamente qualsiasi parte della stringa di query o si passa l'input utente a istruzioni o stored procedure in grado di eseguire tali input come SQL dinamico, è responsabilità dell'utente convalidare l'input per proteggersi dagli attacchi SQL injection.</span><span class="sxs-lookup"><span data-stu-id="ba1e8-146">If you are using string concatenation or interpolation to dynamically build any part of the query string, or passing user input to statements or stored procedures that can execute those inputs as dynamic SQL, then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
