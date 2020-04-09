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
# <a name="complex-query-operators"></a>Operatori di query complessi

Language Integrated Query (LINQ) contiene molti operatori complessi, che combinano più origini dati o eseguono elaborazioni complesse. Non tutti gli operatori LINQ dispongono di traduzioni adatte sul lato server. A volte, una query in un modulo viene convertita nel server, ma se scritta in una forma diversa non viene convertita anche se il risultato è lo stesso. Questa pagina descrive alcuni degli operatori complessi e le relative varianti supportate. Nelle versioni future, potremmo riconoscere più modelli e aggiungere le traduzioni corrispondenti. È anche importante tenere presente che il supporto della traduzione varia da un provider all'altro. Una particolare query, che viene tradotta in SqlServer, potrebbe non funzionare per i database SQLite.

> [!TIP]
> È possibile visualizzare l'[esempio](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) di questo articolo in GitHub.

## <a name="join"></a>Join

L'operatore Join LINQ consente di connettere due origini dati in base al selettore di chiave per ogni origine, generando una tupla di valori quando la chiave corrisponde. Si traduce naturalmente `INNER JOIN` in su database relazionali. Mentre il join LINQ dispone di selettori di chiave esterna e interna, il database richiede una singola condizione di join. In questo server EF Core viene generata una condizione di join confrontando il selettore di chiave esterna con il selettore della chiave interna per l'uguaglianza. Inoltre, se i selettori di chiave sono tipi anonimi, EF Core genera una condizione di join per confrontare il componente di uguaglianza in modo saggio.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#Join)]

```SQL
SELECT [p].[PersonId], [p].[Name], [p].[PhotoId], [p0].[PersonPhotoId], [p0].[Caption], [p0].[Photo]
FROM [PersonPhoto] AS [p0]
INNER JOIN [Person] AS [p] ON [p0].[PersonPhotoId] = [p].[PhotoId]
```

## <a name="groupjoin"></a>GroupJoin

L'operatore GroupJoin LINQ consente di connettere due origini dati simili a Join, ma crea un gruppo di valori interni per la corrispondenza degli elementi esterni. L'esecuzione di una query come `Blog`  &  `IEnumerable<Post>`nell'esempio seguente genera un risultato di . Poiché i database (in particolare i database relazionali) non hanno un modo per rappresentare una raccolta di oggetti lato client, GroupJoin non si traduce nel server in molti casi. È necessario ottenere tutti i dati dal server per eseguire GroupJoin senza un selettore speciale (prima query di seguito). Ma se il selettore limita la selezione dei dati, il recupero di tutti i dati dal server può causare problemi di prestazioni (seconda query di seguito). Ecco perché EF Core non traduce GroupJoin.That s why EF Core doesn't translate GroupJoin.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoin)]

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupJoinComposed)]

## <a name="selectmany"></a>SelectMany

L'operatore SelectMany LINQ consente di enumerare un selettore di raccolta per ogni elemento esterno e generare tuple di valori da ogni origine dati. In un certo senso, è un join ma senza alcuna condizione in modo che ogni elemento esterno è connesso con un elemento dall'origine della raccolta. A seconda del modo in cui il selettore di raccolta è correlato all'origine dati esterna, SelectMany può essere convertito in varie query diverse sul lato server.

### <a name="collection-selector-doesnt-reference-outer"></a>Il selettore di raccolta non fa riferimento all'esterno

Quando il selettore di raccolta non fa riferimento a nulla dall'origine esterna, il risultato è un prodotto cartesiano di entrambe le origini dati. Si traduce `CROSS JOIN` in in database relazionali.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToCrossJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
CROSS JOIN [Posts] AS [p]
```

### <a name="collection-selector-references-outer-in-a-where-clause"></a>Il selettore di raccolta fa riferimento all'esterno in una clausola where

Quando il selettore di raccolta ha una clausola where, che fa riferimento all'elemento esterno, quindi EF Core lo converte in un join di database e usa il predicato come condizione di join. In genere questo caso si verifica quando si usa lo spostamento di raccolta sull'elemento esterno come selettore di raccolta. Se la raccolta è vuota per un elemento esterno, non verrà generato alcun risultato per tale elemento esterno. Ma `DefaultIfEmpty` se viene applicato al selettore di raccolta, l'elemento esterno verrà connesso con un valore predefinito dell'elemento interno. A causa di questa distinzione, `INNER JOIN` questo tipo `DefaultIfEmpty` di `LEFT JOIN` `DefaultIfEmpty` query si traduce in assenza e quando viene applicato.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
INNER JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

### <a name="collection-selector-references-outer-in-a-non-where-case"></a>Il selettore di raccolta fa riferimento all'esterno in un caso non specifico

Quando il selettore di raccolta fa riferimento all'elemento esterno, che non si trova in una clausola where (come nel caso precedente), non si traduce in un join di database. Ecco perché è necessario valutare il selettore di raccolta per ogni elemento esterno. Si traduce `APPLY` in operazioni in molti database relazionali. Se la raccolta è vuota per un elemento esterno, non verrà generato alcun risultato per tale elemento esterno. Ma `DefaultIfEmpty` se viene applicato al selettore di raccolta, l'elemento esterno verrà connesso con un valore predefinito dell'elemento interno. A causa di questa distinzione, `CROSS APPLY` questo tipo `DefaultIfEmpty` di `OUTER APPLY` `DefaultIfEmpty` query si traduce in assenza e quando viene applicato. Alcuni database come SQLite `APPLY` non supportano gli operatori, quindi questo tipo di query potrebbe non essere tradotto.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#SelectManyConvertedToApply)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
CROSS APPLY [Posts] AS [p]

SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], ([b].[Url] + N'=>') + [p].[Title] AS [p]
FROM [Blogs] AS [b]
OUTER APPLY [Posts] AS [p]
```

## <a name="groupby"></a>GroupBy

Gli operatori LINQ GroupBy `IGrouping<TKey, TElement>` creano `TKey` `TElement` un risultato del tipo in cui e potrebbero essere qualsiasi tipo arbitrario. Inoltre, `IGrouping` `IEnumerable<TElement>`implementa , il che significa che è possibile comporre su di esso utilizzando qualsiasi operatore LINQ dopo il raggruppamento. Poiché nessuna struttura `IGrouping`di database può rappresentare un oggetto , nella maggior parte dei casi gli operatori GroupBy non hanno alcuna traduzione. Quando un operatore di aggregazione viene applicato a ogni gruppo, `GROUP BY` che restituisce uno scalare, può essere convertito in SQL nei database relazionali. Anche `GROUP BY` il codice SQL è restrittivo. È necessario eseguire il raggruppamento solo in base ai valori scalari. La proiezione può contenere solo colonne chiave di raggruppamento o qualsiasi aggregazione applicata a una colonna. EF Core identifica questo modello e lo converte nel server, come nell'esempio seguente:EF Core identifies this pattern and translates it to the server, as in the following example:

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupBy)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
```

Entity Framework Core converte anche le query in cui un operatore di aggregazione sul raggruppamento viene visualizzato in un Where o OrderBy (o altro operatore LINQ di ordinamento). Utilizza `HAVING` la clausola in SQL per la clausola where. La parte della query prima di applicare l'operatore GroupBy può essere qualsiasi query complessa, purché possa essere convertita in server. Inoltre, una volta applicati gli operatori di aggregazione a una query di raggruppamento per rimuovere i raggruppamenti dall'origine risultante, è possibile comporre su di esso come qualsiasi altra query.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#GroupByFilter)]

```SQL
SELECT [p].[AuthorId] AS [Key], COUNT(*) AS [Count]
FROM [Posts] AS [p]
GROUP BY [p].[AuthorId]
HAVING COUNT(*) > 0
ORDER BY [p].[AuthorId]
```

Gli operatori aggregati supportati da EF Core sono i seguenti:

- Media
- Conteggio
- LongCount
- Max
- Min
- SUM

## <a name="left-join"></a>Unione a sinistra

Mentre il left Join non è un operatore LINQ, i database relazionali hanno il concetto di Left Join che viene spesso utilizzato nelle query. Un modello particolare nelle query LINQLINQ `LEFT JOIN` fornisce lo stesso risultato di un nel server. EF Core identifica tali modelli `LEFT JOIN` e genera l'equivalente sul lato server. Il modello prevede la creazione di un GroupJoin tra entrambe le origini dati e quindi la conversione del raggruppamento utilizzando l'operatore SelectMany con DefaultIfEmpty nell'origine di raggruppamento per trovare una corrispondenza con null quando l'interno non dispone di un elemento correlato. Nell'esempio seguente viene illustrato l'aspetto di tale modello e il modello generato.

[!code-csharp[Main](../../../samples/core/Querying/ComplexQuery/Sample.cs#LeftJoin)]

```SQL
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url], [p].[PostId], [p].[AuthorId], [p].[BlogId], [p].[Content], [p].[Rating], [p].[Title]
FROM [Blogs] AS [b]
LEFT JOIN [Posts] AS [p] ON [b].[BlogId] = [p].[BlogId]
```

Il modello precedente crea una struttura complessa nell'albero delle espressioni. Per questo, Entity Framework Core richiede di appiattire i risultati di raggruppamento dell'operatore GroupJoin in un passaggio immediatamente successivo all'operatore. Anche se viene utilizzato groupJoin-DefaultIfEmpty-SelectMany ma in un modello diverso, è possibile che non venga identificato come join di sinistra.
