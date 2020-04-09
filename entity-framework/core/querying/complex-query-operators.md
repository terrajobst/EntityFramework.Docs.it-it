---
title: Operatori di query complessi - Core di EFComplex Query Operators - EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 2e187a2a-4072-4198-9040-aaad68e424fd
uid: core/querying/complex-query-operators
ms.openlocfilehash: 44c2695ea003da043925740a52596fd27da638f8
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417743"
---
# <a name="complex-query-operators"></a><span data-ttu-id="23c98-102">Operatori di query complessi</span><span class="sxs-lookup"><span data-stu-id="23c98-102">Complex Query Operators</span></span>

<span data-ttu-id="23c98-103">Language Integrated Query (LINQ) contiene molti operatori complessi, che combinano più origini dati o eseguono elaborazioni complesse.</span><span class="sxs-lookup"><span data-stu-id="23c98-103">Language Integrated Query (LINQ) contains many complex operators, which combine multiple data sources or does complex processing.</span></span> <span data-ttu-id="23c98-104">Non tutti gli operatori LINQ dispongono di traduzioni adatte sul lato server.</span><span class="sxs-lookup"><span data-stu-id="23c98-104">Not all LINQ operators have suitable translations on the server side.</span></span> <span data-ttu-id="23c98-105">A volte, una query in un modulo viene convertita nel server, ma se scritta in una forma diversa non viene convertita anche se il risultato è lo stesso.</span><span class="sxs-lookup"><span data-stu-id="23c98-105">Sometimes, a query in one form translates to the server but if written in a different form doesn't translate even if the result is the same.</span></span> <span data-ttu-id="23c98-106">Questa pagina descrive alcuni degli operatori complessi e le relative varianti supportate.</span><span class="sxs-lookup"><span data-stu-id="23c98-106">This page describes some of the complex operators and their supported variations.</span></span> <span data-ttu-id="23c98-107">Nelle versioni future, potremmo riconoscere più modelli e aggiungere le traduzioni corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="23c98-107">In future releases, we may recognize more patterns and add their corresponding translations.</span></span> <span data-ttu-id="23c98-108">È anche importante tenere presente che il supporto della traduzione varia da un provider all'altro.</span><span class="sxs-lookup"><span data-stu-id="23c98-108">It's also important to keep in mind that translation support varies between providers.</span></span> <span data-ttu-id="23c98-109">Una particolare query, che viene tradotta in SqlServer, potrebbe non funzionare per i database SQLite.</span><span class="sxs-lookup"><span data-stu-id="23c98-109">A particular query, which is translated in SqlServer, may not work for SQLite databases.</span></span>

> [!TIP]
> <span data-ttu-id="23c98-110">È possibile visualizzare l'[esempio](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="23c98-110">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="join"></a><span data-ttu-id="23c98-111">Join</span><span class="sxs-lookup"><span data-stu-id="23c98-111">Join</span></span>

<span data-ttu-id="23c98-112">L'operatore Join LINQ consente di connettere due origini dati in base al selettore di chiave per ogni origine, generando una tupla di valori quando la chiave corrisponde.</span><span class="sxs-lookup"><span data-stu-id="23c98-112">The LINQ Join operator allows you to connect two data sources based on the key selector for each source, generating a tuple of values when the key matches.</span></span> <span data-ttu-id="23c98-113">Si traduce naturalmente `INNER JOIN` in su database relazionali.</span><span class="sxs-lookup"><span data-stu-id="23c98-113">It naturally translates to `INNER JOIN` on relational databases.</span></span> <span data-ttu-id="23c98-114">Mentre il join LINQ dispone di selettori di chiave esterna e interna, il database richiede una singola condizione di join.</span><span class="sxs-lookup"><span data-stu-id="23c98-114">While the LINQ Join has outer and inner key selectors, the database requires a single join condition.</span></span> <span data-ttu-id="23c98-115">In questo server EF Core viene generata una condizione di join confrontando il selettore di chiave esterna con il selettore della chiave interna per l'uguaglianza.</span><span class="sxs-lookup"><span data-stu-id="23c98-115">So EF Core generates a join condition by comparing the outer key selector to the inner key selector for equality.</span></span> <span data-ttu-id="23c98-116">Inoltre, se i selettori di chiave sono tipi anonimi, EF Core genera una condizione di join per confrontare il componente di uguaglianza in modo saggio.</span><span class="sxs-lookup"><span data-stu-id="23c98-116">Further, if the key selectors are anonymous types, EF Core generates a join condition to compare equality component wise.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a><span data-ttu-id="23c98-117">GroupJoin</span><span class="sxs-lookup"><span data-stu-id="23c98-117">GroupJoin</span></span>

<span data-ttu-id="23c98-118">L'operatore GroupJoin LINQ consente di connettere due origini dati simili a Join, ma crea un gruppo di valori interni per la corrispondenza degli elementi esterni.</span><span class="sxs-lookup"><span data-stu-id="23c98-118">The LINQ GroupJoin operator allows you to connect two data sources similar to Join, but it creates a group of inner values for matching outer elements.</span></span> <span data-ttu-id="23c98-119">L'esecuzione di una query come `Blog`  &  `IEnumerable<Post>`nell'esempio seguente genera un risultato di .</span><span class="sxs-lookup"><span data-stu-id="23c98-119">Executing a query like the following example generates a result of `Blog` & `IEnumerable<Post>`.</span></span> <span data-ttu-id="23c98-120">Poiché i database (in particolare i database relazionali) non hanno un modo per rappresentare una raccolta di oggetti lato client, GroupJoin non si traduce nel server in molti casi.</span><span class="sxs-lookup"><span data-stu-id="23c98-120">Since databases (especially relational databases) don't have a way to represent a collection of client-side objects, GroupJoin doesn't translate to the server in many cases.</span></span> <span data-ttu-id="23c98-121">È necessario ottenere tutti i dati dal server per eseguire GroupJoin senza un selettore speciale (prima query di seguito).</span><span class="sxs-lookup"><span data-stu-id="23c98-121">It requires you to get all of the data from the server to do GroupJoin without a special selector (first query below).</span></span> <span data-ttu-id="23c98-122">Ma se il selettore limita la selezione dei dati, il recupero di tutti i dati dal server può causare problemi di prestazioni (seconda query di seguito).</span><span class="sxs-lookup"><span data-stu-id="23c98-122">But if the selector is limiting data being selected then fetching all of the data from the server may cause performance issues (second query below).</span></span> <span data-ttu-id="23c98-123">Ecco perché EF Core non traduce GroupJoin.That s why EF Core doesn't translate GroupJoin.</span><span class="sxs-lookup"><span data-stu-id="23c98-123">That's why EF Core doesn't translate GroupJoin.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a><span data-ttu-id="23c98-124">SelectMany</span><span class="sxs-lookup"><span data-stu-id="23c98-124">SelectMany</span></span>

<span data-ttu-id="23c98-125">L'operatore SelectMany LINQ consente di enumerare un selettore di raccolta per ogni elemento esterno e generare tuple di valori da ogni origine dati.</span><span class="sxs-lookup"><span data-stu-id="23c98-125">The LINQ SelectMany operator allows you to enumerate over a collection selector for each outer element and generate tuples of values from each data source.</span></span> <span data-ttu-id="23c98-126">In un certo senso, è un join ma senza alcuna condizione in modo che ogni elemento esterno è connesso con un elemento dall'origine della raccolta.</span><span class="sxs-lookup"><span data-stu-id="23c98-126">In a way, it's a join but without any condition so every outer element is connected with an element from the collection source.</span></span> <span data-ttu-id="23c98-127">A seconda del modo in cui il selettore di raccolta è correlato all'origine dati esterna, SelectMany può essere convertito in varie query diverse sul lato server.</span><span class="sxs-lookup"><span data-stu-id="23c98-127">Depending on how the collection selector is related to the outer data source, SelectMany can translate into various different queries on the server side.</span></span>

### <a name="collection-selector-doesnt-reference-outer"></a><span data-ttu-id="23c98-128">Il selettore di raccolta non fa riferimento all'esterno</span><span class="sxs-lookup"><span data-stu-id="23c98-128">Collection selector doesn't reference outer</span></span>

<span data-ttu-id="23c98-129">Quando il selettore di raccolta non fa riferimento a nulla dall'origine esterna, il risultato è un prodotto cartesiano di entrambe le origini dati.</span><span class="sxs-lookup"><span data-stu-id="23c98-129">When the collection selector isn't referencing anything from the outer source, the result is a cartesian product of both data sources.</span></span> <span data-ttu-id="23c98-130">Si traduce `CROSS JOIN` in in database relazionali.</span><span class="sxs-lookup"><span data-stu-id="23c98-130">It translates to `CROSS JOIN` in relational databases.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a><span data-ttu-id="23c98-131">Il selettore di raccolta fa riferimento all'esterno in una clausola where</span><span class="sxs-lookup"><span data-stu-id="23c98-131">Collection selector references outer in a where clause</span></span>

<span data-ttu-id="23c98-132">Quando il selettore di raccolta ha una clausola where, che fa riferimento all'elemento esterno, quindi EF Core lo converte in un join di database e usa il predicato come condizione di join.</span><span class="sxs-lookup"><span data-stu-id="23c98-132">When the collection selector has a where clause, which references the outer element, then EF Core translates it to a database join and uses the predicate as the join condition.</span></span> <span data-ttu-id="23c98-133">In genere questo caso si verifica quando si usa lo spostamento di raccolta sull'elemento esterno come selettore di raccolta.</span><span class="sxs-lookup"><span data-stu-id="23c98-133">Normally this case arises when using collection navigation on the outer element as the collection selector.</span></span> <span data-ttu-id="23c98-134">Se la raccolta è vuota per un elemento esterno, non verrà generato alcun risultato per tale elemento esterno.</span><span class="sxs-lookup"><span data-stu-id="23c98-134">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="23c98-135">Ma `DefaultIfEmpty` se viene applicato al selettore di raccolta, l'elemento esterno verrà connesso con un valore predefinito dell'elemento interno.</span><span class="sxs-lookup"><span data-stu-id="23c98-135">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="23c98-136">A causa di questa distinzione, `INNER JOIN` questo tipo `DefaultIfEmpty` di `LEFT JOIN` `DefaultIfEmpty` query si traduce in assenza e quando viene applicato.</span><span class="sxs-lookup"><span data-stu-id="23c98-136">Because of this distinction, this kind of queries translates to `INNER JOIN` in the absence of `DefaultIfEmpty` and `LEFT JOIN` when `DefaultIfEmpty` is applied.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a><span data-ttu-id="23c98-137">Il selettore di raccolta fa riferimento all'esterno in un caso non specifico</span><span class="sxs-lookup"><span data-stu-id="23c98-137">Collection selector references outer in a non-where case</span></span>

<span data-ttu-id="23c98-138">Quando il selettore di raccolta fa riferimento all'elemento esterno, che non si trova in una clausola where (come nel caso precedente), non si traduce in un join di database.</span><span class="sxs-lookup"><span data-stu-id="23c98-138">When the collection selector references the outer element, which isn't in a where clause (as the case above), it doesn't translate to a database join.</span></span> <span data-ttu-id="23c98-139">Ecco perché è necessario valutare il selettore di raccolta per ogni elemento esterno.</span><span class="sxs-lookup"><span data-stu-id="23c98-139">That's why we need to evaluate the collection selector for each outer element.</span></span> <span data-ttu-id="23c98-140">Si traduce `APPLY` in operazioni in molti database relazionali.</span><span class="sxs-lookup"><span data-stu-id="23c98-140">It translates to `APPLY` operations in many relational databases.</span></span> <span data-ttu-id="23c98-141">Se la raccolta è vuota per un elemento esterno, non verrà generato alcun risultato per tale elemento esterno.</span><span class="sxs-lookup"><span data-stu-id="23c98-141">If the collection is empty for an outer element, then no results would be generated for that outer element.</span></span> <span data-ttu-id="23c98-142">Ma `DefaultIfEmpty` se viene applicato al selettore di raccolta, l'elemento esterno verrà connesso con un valore predefinito dell'elemento interno.</span><span class="sxs-lookup"><span data-stu-id="23c98-142">But if `DefaultIfEmpty` is applied on the collection selector then the outer element will be connected with a default value of the inner element.</span></span> <span data-ttu-id="23c98-143">A causa di questa distinzione, `CROSS APPLY` questo tipo `DefaultIfEmpty` di `OUTER APPLY` `DefaultIfEmpty` query si traduce in assenza e quando viene applicato.</span><span class="sxs-lookup"><span data-stu-id="23c98-143">Because of this distinction, this kind of queries translates to `CROSS APPLY` in the absence of `DefaultIfEmpty` and `OUTER APPLY` when `DefaultIfEmpty` is applied.</span></span> <span data-ttu-id="23c98-144">Alcuni database come SQLite `APPLY` non supportano gli operatori, quindi questo tipo di query potrebbe non essere tradotto.</span><span class="sxs-lookup"><span data-stu-id="23c98-144">Certain databases like SQLite don't support `APPLY` operators so this kind of query may not be translated.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a><span data-ttu-id="23c98-145">GroupBy</span><span class="sxs-lookup"><span data-stu-id="23c98-145">GroupBy</span></span>

<span data-ttu-id="23c98-146">Gli operatori LINQ GroupBy `IGrouping<TKey, TElement>` creano `TKey` `TElement` un risultato del tipo in cui e potrebbero essere qualsiasi tipo arbitrario.</span><span class="sxs-lookup"><span data-stu-id="23c98-146">LINQ GroupBy operators create a result of type `IGrouping<TKey, TElement>` where `TKey` and `TElement` could be any arbitrary type.</span></span> <span data-ttu-id="23c98-147">Inoltre, `IGrouping` `IEnumerable<TElement>`implementa , il che significa che è possibile comporre su di esso utilizzando qualsiasi operatore LINQ dopo il raggruppamento.</span><span class="sxs-lookup"><span data-stu-id="23c98-147">Furthermore, `IGrouping` implements `IEnumerable<TElement>`, which means you can compose over it using any LINQ operator after the grouping.</span></span> <span data-ttu-id="23c98-148">Poiché nessuna struttura `IGrouping`di database può rappresentare un oggetto , nella maggior parte dei casi gli operatori GroupBy non hanno alcuna traduzione.</span><span class="sxs-lookup"><span data-stu-id="23c98-148">Since no database structure can represent an `IGrouping`, GroupBy operators have no translation in most cases.</span></span> <span data-ttu-id="23c98-149">Quando un operatore di aggregazione viene applicato a ogni gruppo, `GROUP BY` che restituisce uno scalare, può essere convertito in SQL nei database relazionali.</span><span class="sxs-lookup"><span data-stu-id="23c98-149">When an aggregate operator is applied to each group, which returns a scalar, it can be translated to SQL `GROUP BY` in relational databases.</span></span> <span data-ttu-id="23c98-150">Anche `GROUP BY` il codice SQL è restrittivo.</span><span class="sxs-lookup"><span data-stu-id="23c98-150">The SQL `GROUP BY` is restrictive too.</span></span> <span data-ttu-id="23c98-151">È necessario eseguire il raggruppamento solo in base ai valori scalari.</span><span class="sxs-lookup"><span data-stu-id="23c98-151">It requires you to group only by scalar values.</span></span> <span data-ttu-id="23c98-152">La proiezione può contenere solo colonne chiave di raggruppamento o qualsiasi aggregazione applicata a una colonna.</span><span class="sxs-lookup"><span data-stu-id="23c98-152">The projection can only contain grouping key columns or any aggregate applied over a column.</span></span> <span data-ttu-id="23c98-153">EF Core identifica questo modello e lo converte nel server, come nell'esempio seguente:EF Core identifies this pattern and translates it to the server, as in the following example:</span><span class="sxs-lookup"><span data-stu-id="23c98-153">EF Core identifies this pattern and translates it to the server, as in the following example:</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

<span data-ttu-id="23c98-154">Entity Framework Core converte anche le query in cui un operatore di aggregazione sul raggruppamento viene visualizzato in un Where o OrderBy (o altro operatore LINQ di ordinamento).</span><span class="sxs-lookup"><span data-stu-id="23c98-154">EF Core also translates queries where an aggregate operator on the grouping appears in a Where or OrderBy (or other ordering) LINQ operator.</span></span> <span data-ttu-id="23c98-155">Utilizza `HAVING` la clausola in SQL per la clausola where.</span><span class="sxs-lookup"><span data-stu-id="23c98-155">It uses `HAVING` clause in SQL for the where clause.</span></span> <span data-ttu-id="23c98-156">La parte della query prima di applicare l'operatore GroupBy può essere qualsiasi query complessa, purché possa essere convertita in server.</span><span class="sxs-lookup"><span data-stu-id="23c98-156">The part of the query before applying the GroupBy operator can be any complex query as long as it can be translated to server.</span></span> <span data-ttu-id="23c98-157">Inoltre, una volta applicati gli operatori di aggregazione a una query di raggruppamento per rimuovere i raggruppamenti dall'origine risultante, è possibile comporre su di esso come qualsiasi altra query.</span><span class="sxs-lookup"><span data-stu-id="23c98-157">Furthermore, once you apply aggregate operators on a grouping query to remove groupings from the resulting source, you can compose on top of it like any other query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

<span data-ttu-id="23c98-158">Gli operatori aggregati supportati da EF Core sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="23c98-158">The aggregate operators EF Core supports are as follows</span></span>

- <span data-ttu-id="23c98-159">Media</span><span class="sxs-lookup"><span data-stu-id="23c98-159">Average</span></span>
- <span data-ttu-id="23c98-160">Conteggio</span><span class="sxs-lookup"><span data-stu-id="23c98-160">Count</span></span>
- <span data-ttu-id="23c98-161">LongCount</span><span class="sxs-lookup"><span data-stu-id="23c98-161">LongCount</span></span>
- <span data-ttu-id="23c98-162">Max</span><span class="sxs-lookup"><span data-stu-id="23c98-162">Max</span></span>
- <span data-ttu-id="23c98-163">Min</span><span class="sxs-lookup"><span data-stu-id="23c98-163">Min</span></span>
- <span data-ttu-id="23c98-164">SUM</span><span class="sxs-lookup"><span data-stu-id="23c98-164">Sum</span></span>

## <a name="left-join"></a><span data-ttu-id="23c98-165">Unione a sinistra</span><span class="sxs-lookup"><span data-stu-id="23c98-165">Left Join</span></span>

<span data-ttu-id="23c98-166">Mentre il left Join non è un operatore LINQ, i database relazionali hanno il concetto di Left Join che viene spesso utilizzato nelle query.</span><span class="sxs-lookup"><span data-stu-id="23c98-166">While Left Join isn't a LINQ operator, relational databases have the concept of a Left Join which is frequently used in queries.</span></span> <span data-ttu-id="23c98-167">Un modello particolare nelle query LINQLINQ `LEFT JOIN` fornisce lo stesso risultato di un nel server.</span><span class="sxs-lookup"><span data-stu-id="23c98-167">A particular pattern in LINQ queries gives the same result as a `LEFT JOIN` on the server.</span></span> <span data-ttu-id="23c98-168">EF Core identifica tali modelli `LEFT JOIN` e genera l'equivalente sul lato server.</span><span class="sxs-lookup"><span data-stu-id="23c98-168">EF Core identifies such patterns and generates the equivalent `LEFT JOIN` on the server side.</span></span> <span data-ttu-id="23c98-169">Il modello prevede la creazione di un GroupJoin tra entrambe le origini dati e quindi la conversione del raggruppamento utilizzando l'operatore SelectMany con DefaultIfEmpty nell'origine di raggruppamento per trovare una corrispondenza con null quando l'interno non dispone di un elemento correlato.</span><span class="sxs-lookup"><span data-stu-id="23c98-169">The pattern involves creating a GroupJoin between both the data sources and then flattening out the grouping by using the SelectMany operator with DefaultIfEmpty on the grouping source to match null when the inner doesn't have a related element.</span></span> <span data-ttu-id="23c98-170">Nell'esempio seguente viene illustrato l'aspetto di tale modello e il modello generato.</span><span class="sxs-lookup"><span data-stu-id="23c98-170">The following example shows what that pattern looks like and what it generates.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

<span data-ttu-id="23c98-171">Il modello precedente crea una struttura complessa nell'albero delle espressioni.</span><span class="sxs-lookup"><span data-stu-id="23c98-171">The above pattern creates a complex structure in the expression tree.</span></span> <span data-ttu-id="23c98-172">Per questo, Entity Framework Core richiede di appiattire i risultati di raggruppamento dell'operatore GroupJoin in un passaggio immediatamente successivo all'operatore.</span><span class="sxs-lookup"><span data-stu-id="23c98-172">Because of that, EF Core requires you to flatten out the grouping results of the GroupJoin operator in a step immediately following the operator.</span></span> <span data-ttu-id="23c98-173">Anche se viene utilizzato groupJoin-DefaultIfEmpty-SelectMany ma in un modello diverso, è possibile che non venga identificato come join di sinistra.</span><span class="sxs-lookup"><span data-stu-id="23c98-173">Even if the GroupJoin-DefaultIfEmpty-SelectMany is used but in a different pattern, we may not identify it as a Left Join.</span></span>
