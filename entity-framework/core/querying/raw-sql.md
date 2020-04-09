---
title: Query SQL non elaborate - EF Core
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: a54bb67c0fce9d621382f6372e70fe4cdca48a20
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417670"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="146cd-102">Query SQL non elaborate</span><span class="sxs-lookup"><span data-stu-id="146cd-102">Raw SQL Queries</span></span>

<span data-ttu-id="146cd-103">Entity Framework Core consente di ricorrere a query SQL non elaborate quando si lavora con un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="146cd-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="146cd-104">Le query SQL non elaborate sono utili se la query desiderata non può essere espressa tramite LINQ.</span><span class="sxs-lookup"><span data-stu-id="146cd-104">Raw SQL queries are useful if the query you want can't be expressed using LINQ.</span></span> <span data-ttu-id="146cd-105">Le query SQL non elaborate vengono utilizzate anche se l'utilizzo di una query LINQ genera una query SQL inefficiente.</span><span class="sxs-lookup"><span data-stu-id="146cd-105">Raw SQL queries are also used if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="146cd-106">Le query SQL non elaborate possono restituire tipi di entità regolari o tipi di [entità senza chiave](xref:core/modeling/keyless-entity-types) che fanno parte del modello.</span><span class="sxs-lookup"><span data-stu-id="146cd-106">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="146cd-107">È possibile visualizzare l'[esempio](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="146cd-107">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="146cd-108">Query SQL non elaborate di base</span><span class="sxs-lookup"><span data-stu-id="146cd-108">Basic raw SQL queries</span></span>

<span data-ttu-id="146cd-109">È possibile `FromSqlRaw` utilizzare il metodo di estensione per avviare una query LINQ basata su una query SQL non elaborata.</span><span class="sxs-lookup"><span data-stu-id="146cd-109">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span> <span data-ttu-id="146cd-110">`FromSqlRaw`può essere utilizzato solo sulle radici della `DbSet<>`query, che si trova direttamente sul file .</span><span class="sxs-lookup"><span data-stu-id="146cd-110">`FromSqlRaw` can only be used on query roots, that is directly on the `DbSet<>`.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

<span data-ttu-id="146cd-111">Per eseguire una stored procedure, è possibile usare query SQL non elaborate.</span><span class="sxs-lookup"><span data-stu-id="146cd-111">Raw SQL queries can be used to execute a stored procedure.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a><span data-ttu-id="146cd-112">Passaggio di parametri</span><span class="sxs-lookup"><span data-stu-id="146cd-112">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="146cd-113">**Usa sempre la parametrizzazione per le query SQL non elaborate**</span><span class="sxs-lookup"><span data-stu-id="146cd-113">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="146cd-114">Quando si introducono i valori forniti dall'utente in una query SQL non elaborata, è necessario prestare attenzione per evitare attacchi SQL injection.</span><span class="sxs-lookup"><span data-stu-id="146cd-114">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="146cd-115">Oltre a verificare che tali valori non contengano caratteri non validi, utilizzare sempre la parametrizzazione che invia i valori separati dal testo SQL.</span><span class="sxs-lookup"><span data-stu-id="146cd-115">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="146cd-116">In particolare, non passare mai una stringa`$""`concatenata o interpolata `FromSqlRaw` ( `ExecuteSqlRaw`) con valori forniti dall'utente non convalidati in o .</span><span class="sxs-lookup"><span data-stu-id="146cd-116">In particular, never pass a concatenated or interpolated string (`$""`) with non-validated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="146cd-117">I `FromSqlInterpolated` `ExecuteSqlInterpolated` metodi e consentono di utilizzare la sintassi di interpolazione di stringhe in modo da proteggersi dagli attacchi SQL injection.</span><span class="sxs-lookup"><span data-stu-id="146cd-117">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="146cd-118">Nell'esempio seguente viene passato un singolo parametro a una stored procedure includendo un segnaposto di parametro nella stringa di query SQL e fornendo un argomento aggiuntivo.</span><span class="sxs-lookup"><span data-stu-id="146cd-118">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="146cd-119">Anche se questa `String.Format` sintassi può essere simile, `DbParameter` il valore fornito viene `{0}` racchiuso in un e il nome del parametro generato viene inserito nel punto in cui è stato specificato il segnaposto.</span><span class="sxs-lookup"><span data-stu-id="146cd-119">While this syntax may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

<span data-ttu-id="146cd-120">`FromSqlInterpolated`è simile `FromSqlRaw` ma consente di utilizzare la sintassi di interpolazione delle stringhe.</span><span class="sxs-lookup"><span data-stu-id="146cd-120">`FromSqlInterpolated` is similar to `FromSqlRaw` but allows you to use string interpolation syntax.</span></span> <span data-ttu-id="146cd-121">Proprio `FromSqlRaw`come `FromSqlInterpolated` , può essere utilizzato solo sulle radici della query.</span><span class="sxs-lookup"><span data-stu-id="146cd-121">Just like `FromSqlRaw`, `FromSqlInterpolated` can only be used on query roots.</span></span> <span data-ttu-id="146cd-122">Come nell'esempio precedente, il valore `DbParameter` viene convertito in un e non è vulnerabile a SQL injection.</span><span class="sxs-lookup"><span data-stu-id="146cd-122">As with the previous example, the value is converted to a `DbParameter` and isn't vulnerable to SQL injection.</span></span>

> [!NOTE]
> <span data-ttu-id="146cd-123">Prima della versione 3.0 `FromSqlRaw` e `FromSqlInterpolated` erano `FromSql`due overload denominati .</span><span class="sxs-lookup"><span data-stu-id="146cd-123">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="146cd-124">Per ulteriori informazioni, vedere la [sezione delle versioni precedenti.](#previous-versions)</span><span class="sxs-lookup"><span data-stu-id="146cd-124">For more information, see the [previous versions section](#previous-versions).</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

<span data-ttu-id="146cd-125">È anche possibile costruire un DbParameter e fornirlo come valore del parametro.</span><span class="sxs-lookup"><span data-stu-id="146cd-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="146cd-126">Poiché viene utilizzato un segnaposto di parametro `FromSqlRaw` SQL normale, anziché un segnaposto di stringa, può essere utilizzato in modo sicuro:As a regular SQL parameter placeholder is used, rather than a string placeholder, can be safely used:</span><span class="sxs-lookup"><span data-stu-id="146cd-126">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

<span data-ttu-id="146cd-127">`FromSqlRaw`consente di utilizzare parametri denominati nella stringa di query SQL, utile quando una stored procedure dispone di parametri facoltativi:</span><span class="sxs-lookup"><span data-stu-id="146cd-127">`FromSqlRaw` allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a><span data-ttu-id="146cd-128">Composizione con LINQ</span><span class="sxs-lookup"><span data-stu-id="146cd-128">Composing with LINQ</span></span>

<span data-ttu-id="146cd-129">È possibile comporre sopra la query SQL non elaborata iniziale utilizzando gli operatori LINQ.</span><span class="sxs-lookup"><span data-stu-id="146cd-129">You can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="146cd-130">EF Core verrà trattato come sottoquery e comporre su di esso nel database.</span><span class="sxs-lookup"><span data-stu-id="146cd-130">EF Core will treat it as subquery and compose over it in the database.</span></span> <span data-ttu-id="146cd-131">Nell'esempio seguente viene utilizzata una query SQL non elaborata che seleziona da una funzione con valori di tabella (TVF).</span><span class="sxs-lookup"><span data-stu-id="146cd-131">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF).</span></span> <span data-ttu-id="146cd-132">E poi compone su di esso utilizzando LINQ per eseguire il filtro e l'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="146cd-132">And then composes on it using LINQ to do filtering and sorting.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

<span data-ttu-id="146cd-133">La query precedente genera il codice SQL seguente:</span><span class="sxs-lookup"><span data-stu-id="146cd-133">Above query generates following SQL:</span></span>

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a><span data-ttu-id="146cd-134">Inclusione di dati correlati</span><span class="sxs-lookup"><span data-stu-id="146cd-134">Including related data</span></span>

<span data-ttu-id="146cd-135">Il metodo `Include` può essere usato per includere dati correlati, come in qualsiasi altra query LINQ:</span><span class="sxs-lookup"><span data-stu-id="146cd-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

<span data-ttu-id="146cd-136">La composizione con LINQ richiede che la query SQL non elaborata sia componibile poiché Entity Framework Core considererà l'SQL fornito come una sottoquery.</span><span class="sxs-lookup"><span data-stu-id="146cd-136">Composing with LINQ requires your raw SQL query to be composable since EF Core will treat the supplied SQL as a subquery.</span></span> <span data-ttu-id="146cd-137">Le query SQL componibili iniziano con la parola chiave `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="146cd-137">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span> <span data-ttu-id="146cd-138">Inoltre, SQL passato non deve contenere caratteri o opzioni non validi in una sottoquery, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="146cd-138">Further, SQL passed shouldn't contain any characters or options that aren't valid on a subquery, such as:</span></span>

- <span data-ttu-id="146cd-139">Un punto e virgola finale</span><span class="sxs-lookup"><span data-stu-id="146cd-139">A trailing semicolon</span></span>
- <span data-ttu-id="146cd-140">In SQL Server, un hint a livello di query finale, ad esempio `OPTION (HASH JOIN)`</span><span class="sxs-lookup"><span data-stu-id="146cd-140">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
- <span data-ttu-id="146cd-141">In SQL `ORDER BY` Server, una clausola `OFFSET 0` che `TOP 100 PERCENT` non `SELECT` viene utilizzata con OR nella clausola</span><span class="sxs-lookup"><span data-stu-id="146cd-141">On SQL Server, an `ORDER BY` clause that isn't used with `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

<span data-ttu-id="146cd-142">SQL ServerSQL Server non consente la composizione su chiamate di stored procedure, pertanto qualsiasi tentativo di applicare operatori di query aggiuntivi a una chiamata di questo tipo comporterà codice SQL non valido.</span><span class="sxs-lookup"><span data-stu-id="146cd-142">SQL Server doesn't allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="146cd-143">Utilizzare `AsEnumerable` `AsAsyncEnumerable` o metodo `FromSqlRaw` `FromSqlInterpolated` a destra dopo o metodi per assicurarsi che EF Core non tenta di comporre su una stored procedure.</span><span class="sxs-lookup"><span data-stu-id="146cd-143">Use `AsEnumerable` or `AsAsyncEnumerable` method right after `FromSqlRaw` or `FromSqlInterpolated` methods to make sure that EF Core doesn't try to compose over a stored procedure.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="146cd-144">Rilevamento modifiche</span><span class="sxs-lookup"><span data-stu-id="146cd-144">Change Tracking</span></span>

<span data-ttu-id="146cd-145">Le query `FromSqlRaw` che `FromSqlInterpolated` utilizzano i metodi o seguono esattamente le stesse regole di rilevamento delle modifiche di qualsiasi altra query LINQ in Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="146cd-145">Queries that use the `FromSqlRaw` or `FromSqlInterpolated` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="146cd-146">Se ad esempio la query proietta tipi di entità, i risultati vengono rilevati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="146cd-146">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="146cd-147">Nell'esempio seguente viene utilizzata una query SQL non elaborata che seleziona da una funzione con `AsNoTracking`valori di tabella (TVF), quindi viene disabilitato il rilevamento delle modifiche con la chiamata a :</span><span class="sxs-lookup"><span data-stu-id="146cd-147">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a><span data-ttu-id="146cd-148">Limitazioni</span><span class="sxs-lookup"><span data-stu-id="146cd-148">Limitations</span></span>

<span data-ttu-id="146cd-149">Esistono alcune limitazioni da tenere presenti quando si usano query SQL non elaborate:</span><span class="sxs-lookup"><span data-stu-id="146cd-149">There are a few limitations to be aware of when using raw SQL queries:</span></span>

- <span data-ttu-id="146cd-150">La query SQL deve restituire dati per tutte le proprietà del tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="146cd-150">The SQL query must return data for all properties of the entity type.</span></span>
- <span data-ttu-id="146cd-151">I nomi delle colonne nel set di risultati devono corrispondere ai nomi di colonna a cui sono mappate le proprietà.</span><span class="sxs-lookup"><span data-stu-id="146cd-151">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="146cd-152">Si noti che questo comportamento è diverso da EF6.</span><span class="sxs-lookup"><span data-stu-id="146cd-152">Note this behavior is different from EF6.</span></span> <span data-ttu-id="146cd-153">EF6 proprietà ignorata per il mapping di colonna per le query SQL non elaborate e i nomi di colonna del set di risultati deve corrispondere ai nomi delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="146cd-153">EF6 ignored property to column mapping for raw SQL queries and result set column names had to match the property names.</span></span>
- <span data-ttu-id="146cd-154">La query SQL non può contenere dati correlati.</span><span class="sxs-lookup"><span data-stu-id="146cd-154">The SQL query can't contain related data.</span></span> <span data-ttu-id="146cd-155">Tuttavia, in molti casi è possibile estendere la query usando l'operatore `Include` per restituire i dati correlati (vedere [Inclusione di dati correlati](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="146cd-155">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="previous-versions"></a><span data-ttu-id="146cd-156">Versioni precedenti</span><span class="sxs-lookup"><span data-stu-id="146cd-156">Previous versions</span></span>

<span data-ttu-id="146cd-157">EF Core versione 2.2 e precedenti aveva `FromSql`due overload del metodo denominato , `FromSqlRaw` che `FromSqlInterpolated`si comportava nello stesso modo di più recente e .</span><span class="sxs-lookup"><span data-stu-id="146cd-157">EF Core version 2.2 and earlier had two overloads of method named `FromSql`, which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="146cd-158">Era facile chiamare accidentalmente il metodo stringa non elaborata quando l'intento era quello di chiamare il metodo stringa interpolata e viceversa.</span><span class="sxs-lookup"><span data-stu-id="146cd-158">It was easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="146cd-159">La chiamata accidentale di overload errato potrebbe causare che le query non vengano parametrizzate quando avrebbero dovuto essere.</span><span class="sxs-lookup"><span data-stu-id="146cd-159">Calling wrong overload accidentally could result in queries not being parameterized when they should have been.</span></span>
