---
title: Query SQL non elaborato - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: ddf3a841800684688d50dcf9323f4d83c851222f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="raw-sql-queries"></a><span data-ttu-id="1a1b9-102">Query SQL non elaborato</span><span class="sxs-lookup"><span data-stu-id="1a1b9-102">Raw SQL Queries</span></span>

<span data-ttu-id="1a1b9-103">Entity Framework Core consente di elenco a discesa di query SQL non elaborate quando si lavora con un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="1a1b9-104">Può essere utile se la query da eseguire non può essere espresse utilizzando LINQ, o se tramite una query LINQ è risultante in inefficiente SQL inviati al database.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL being sent to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="1a1b9-105">È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) su GitHub.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="1a1b9-106">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="1a1b9-106">Limitations</span></span>

<span data-ttu-id="1a1b9-107">Esistono alcune limitazioni da tenere presenti quando si usano query SQL non elaborate:</span><span class="sxs-lookup"><span data-stu-id="1a1b9-107">There are a couple of limitations to be aware of when using raw SQL queries:</span></span>
* <span data-ttu-id="1a1b9-108">Query SQL è utilizzabile solo per restituire i tipi di entità che fanno parte del modello.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-108">SQL queries can only be used to return entity types that are part of your model.</span></span> <span data-ttu-id="1a1b9-109">Un aumento nel nostro backlog per [enable restituzione di tipi ad hoc da una query SQL non elaborate](https://github.com/aspnet/EntityFramework/issues/1862).</span><span class="sxs-lookup"><span data-stu-id="1a1b9-109">There is an enhancement on our backlog to [enable returning ad-hoc types from raw SQL queries](https://github.com/aspnet/EntityFramework/issues/1862).</span></span>

* <span data-ttu-id="1a1b9-110">La query SQL deve restituire dati per tutte le proprietà del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-110">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="1a1b9-111">I nomi delle colonne nel set di risultati deve corrispondere ai nomi di proprietà sono mappate a colonna.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-111">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="1a1b9-112">Questo è diverso da EF6 in cui il mapping di proprietà o la colonna è stato ignorato per le query SQL non elaborate e nomi devono corrispondere ai nomi di proprietà di colonna del set di risultati.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-112">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="1a1b9-113">La query SQL non può contenere i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-113">The SQL query cannot contain related data.</span></span> <span data-ttu-id="1a1b9-114">Tuttavia, in molti casi è possibile comporre sopra la query utilizzando il `Include` operatore per restituire i dati correlati (vedere [inclusi i dati correlati](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="1a1b9-114">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="1a1b9-115">Query SQL non elaborate base</span><span class="sxs-lookup"><span data-stu-id="1a1b9-115">Basic raw SQL queries</span></span>

<span data-ttu-id="1a1b9-116">È possibile utilizzare il *FromSql* il metodo di estensione per iniziare una query LINQ in base a una query SQL non elaborata.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-116">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="1a1b9-117">Per eseguire una stored procedure, è possibile utilizzare query SQL non elaborata.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-117">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="1a1b9-118">Passaggio di parametri</span><span class="sxs-lookup"><span data-stu-id="1a1b9-118">Passing parameters</span></span>

<span data-ttu-id="1a1b9-119">Come con qualsiasi API che accetta SQL, è importante aggiungere parametri di input per proteggersi da un attacco SQL injection qualsiasi utente.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-119">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="1a1b9-120">È possibile includere i segnaposto dei parametri nella stringa di query SQL e quindi fornire i valori dei parametri come argomenti aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-120">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="1a1b9-121">I valori dei parametri è fornire verranno automaticamente convertiti in un `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-121">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="1a1b9-122">Nell'esempio seguente viene passato un parametro singolo a una stored procedure.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-122">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="1a1b9-123">Mentre il risultato potrebbe essere ad esempio `String.Format` sintassi, il valore fornito è racchiuso in cui vengono inseriti un parametro e il nome di parametro generato il `{0}` segnaposto è stato specificato.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-123">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="1a1b9-124">Questa è la stessa query ma utilizzando la sintassi di interpolazione di stringa, che è supportata in EF Core 2.0 e versioni successive:</span><span class="sxs-lookup"><span data-stu-id="1a1b9-124">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="1a1b9-125">È inoltre possibile costruire un DbParameter e fornirlo come valore del parametro.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="1a1b9-126">In questo modo è possibile utilizzare parametri denominati nella stringa di query SQL</span><span class="sxs-lookup"><span data-stu-id="1a1b9-126">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="1a1b9-127">Composizione con LINQ</span><span class="sxs-lookup"><span data-stu-id="1a1b9-127">Composing with LINQ</span></span>

<span data-ttu-id="1a1b9-128">Se la query SQL può essere composte nel database, è possibile comporre sopra la query SQL non elaborata iniziale utilizzando operatori LINQ.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-128">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="1a1b9-129">Query SQL che possono essere composte in corso con il `SELECT` (parola chiave).</span><span class="sxs-lookup"><span data-stu-id="1a1b9-129">SQL queries that can be composed on being with the `SELECT` keyword.</span></span>

<span data-ttu-id="1a1b9-130">L'esempio seguente usa una query SQL non elaborata che consente di selezionare da una funzione con valori di tabella (TVF) e quindi compone su di esso utilizzando LINQ per eseguire il filtro e ordinamento.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-130">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="1a1b9-131">Inclusi i dati correlati</span><span class="sxs-lookup"><span data-stu-id="1a1b9-131">Including related data</span></span>

<span data-ttu-id="1a1b9-132">Interagire con gli operatori LINQ consente di includere dati correlati nella query.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-132">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="1a1b9-133">**Utilizzare sempre la parametrizzazione per query SQL non elaborata:** di stringa, ad esempio le API che accettano un database SQL non elaborato `FromSql` e `ExecuteSqlCommand` consentire facilmente essere passati come parametri di valori.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-133">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="1a1b9-134">Oltre a convalidare l'input dell'utente, utilizzare sempre la parametrizzazione per tutti i valori utilizzati in non elaborato o comando di query SQL.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-134">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="1a1b9-135">Se si utilizza la concatenazione di stringhe per compilare in modo dinamico qualsiasi parte della stringa di query, quindi l'utente è responsabile per la convalida di input per proteggersi da attacchi SQL injection.</span><span class="sxs-lookup"><span data-stu-id="1a1b9-135">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
