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
# <a name="raw-sql-queries"></a>Query SQL non elaborate

Entity Framework Core consente di ricorrere a query SQL non elaborate quando si lavora con un database relazionale. Le query SQL non elaborate sono utili se la query desiderata non può essere espressa tramite LINQ. Le query SQL non elaborate vengono utilizzate anche se l'utilizzo di una query LINQ genera una query SQL inefficiente. Le query SQL non elaborate possono restituire tipi di entità regolari o tipi di [entità senza chiave](xref:core/modeling/keyless-entity-types) che fanno parte del modello.

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/) di questo articolo in GitHub.

## <a name="basic-raw-sql-queries"></a>Query SQL non elaborate di base

È possibile `FromSqlRaw` utilizzare il metodo di estensione per avviare una query LINQ basata su una query SQL non elaborata. `FromSqlRaw`può essere utilizzato solo sulle radici della `DbSet<>`query, che si trova direttamente sul file .

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

Per eseguire una stored procedure, è possibile usare query SQL non elaborate.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a>Passaggio di parametri

> [!WARNING]
> **Usa sempre la parametrizzazione per le query SQL non elaborate**
>
> Quando si introducono i valori forniti dall'utente in una query SQL non elaborata, è necessario prestare attenzione per evitare attacchi SQL injection. Oltre a verificare che tali valori non contengano caratteri non validi, utilizzare sempre la parametrizzazione che invia i valori separati dal testo SQL.
>
> In particolare, non passare mai una stringa`$""`concatenata o interpolata `FromSqlRaw` ( `ExecuteSqlRaw`) con valori forniti dall'utente non convalidati in o . I `FromSqlInterpolated` `ExecuteSqlInterpolated` metodi e consentono di utilizzare la sintassi di interpolazione di stringhe in modo da proteggersi dagli attacchi SQL injection.

Nell'esempio seguente viene passato un singolo parametro a una stored procedure includendo un segnaposto di parametro nella stringa di query SQL e fornendo un argomento aggiuntivo. Anche se questa `String.Format` sintassi può essere simile, `DbParameter` il valore fornito viene `{0}` racchiuso in un e il nome del parametro generato viene inserito nel punto in cui è stato specificato il segnaposto.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

`FromSqlInterpolated`è simile `FromSqlRaw` ma consente di utilizzare la sintassi di interpolazione delle stringhe. Proprio `FromSqlRaw`come `FromSqlInterpolated` , può essere utilizzato solo sulle radici della query. Come nell'esempio precedente, il valore `DbParameter` viene convertito in un e non è vulnerabile a SQL injection.

> [!NOTE]
> Prima della versione 3.0 `FromSqlRaw` e `FromSqlInterpolated` erano `FromSql`due overload denominati . Per ulteriori informazioni, vedere la [sezione delle versioni precedenti.](#previous-versions)

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

È anche possibile costruire un DbParameter e fornirlo come valore del parametro. Poiché viene utilizzato un segnaposto di parametro `FromSqlRaw` SQL normale, anziché un segnaposto di stringa, può essere utilizzato in modo sicuro:As a regular SQL parameter placeholder is used, rather than a string placeholder, can be safely used:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

`FromSqlRaw`consente di utilizzare parametri denominati nella stringa di query SQL, utile quando una stored procedure dispone di parametri facoltativi:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a>Composizione con LINQ

È possibile comporre sopra la query SQL non elaborata iniziale utilizzando gli operatori LINQ. EF Core verrà trattato come sottoquery e comporre su di esso nel database. Nell'esempio seguente viene utilizzata una query SQL non elaborata che seleziona da una funzione con valori di tabella (TVF). E poi compone su di esso utilizzando LINQ per eseguire il filtro e l'ordinamento.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

La query precedente genera il codice SQL seguente:

```sql
SELECT [b].[BlogId], [b].[OwnerId], [b].[Rating], [b].[Url]
FROM (
    SELECT * FROM dbo.SearchBlogs(@p0)
) AS [b]
WHERE [b].[Rating] > 3
ORDER BY [b].[Rating] DESC
```

### <a name="including-related-data"></a>Inclusione di dati correlati

Il metodo `Include` può essere usato per includere dati correlati, come in qualsiasi altra query LINQ:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedInclude)]

La composizione con LINQ richiede che la query SQL non elaborata sia componibile poiché Entity Framework Core considererà l'SQL fornito come una sottoquery. Le query SQL componibili iniziano con la parola chiave `SELECT`. Inoltre, SQL passato non deve contenere caratteri o opzioni non validi in una sottoquery, ad esempio:

- Un punto e virgola finale
- In SQL Server, un hint a livello di query finale, ad esempio `OPTION (HASH JOIN)`
- In SQL `ORDER BY` Server, una clausola `OFFSET 0` che `TOP 100 PERCENT` non `SELECT` viene utilizzata con OR nella clausola

SQL ServerSQL Server non consente la composizione su chiamate di stored procedure, pertanto qualsiasi tentativo di applicare operatori di query aggiuntivi a una chiamata di questo tipo comporterà codice SQL non valido. Utilizzare `AsEnumerable` `AsAsyncEnumerable` o metodo `FromSqlRaw` `FromSqlInterpolated` a destra dopo o metodi per assicurarsi che EF Core non tenta di comporre su una stored procedure.

## <a name="change-tracking"></a>Rilevamento modifiche

Le query `FromSqlRaw` che `FromSqlInterpolated` utilizzano i metodi o seguono esattamente le stesse regole di rilevamento delle modifiche di qualsiasi altra query LINQ in Entity Framework Core. Se ad esempio la query proietta tipi di entità, i risultati vengono rilevati per impostazione predefinita.

Nell'esempio seguente viene utilizzata una query SQL non elaborata che seleziona da una funzione con `AsNoTracking`valori di tabella (TVF), quindi viene disabilitato il rilevamento delle modifiche con la chiamata a :

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a>Limitazioni

Esistono alcune limitazioni da tenere presenti quando si usano query SQL non elaborate:

- La query SQL deve restituire dati per tutte le proprietà del tipo di entità.
- I nomi delle colonne nel set di risultati devono corrispondere ai nomi di colonna a cui sono mappate le proprietà. Si noti che questo comportamento è diverso da EF6. EF6 proprietà ignorata per il mapping di colonna per le query SQL non elaborate e i nomi di colonna del set di risultati deve corrispondere ai nomi delle proprietà.
- La query SQL non può contenere dati correlati. Tuttavia, in molti casi è possibile estendere la query usando l'operatore `Include` per restituire i dati correlati (vedere [Inclusione di dati correlati](#including-related-data)).

## <a name="previous-versions"></a>Versioni precedenti

EF Core versione 2.2 e precedenti aveva `FromSql`due overload del metodo denominato , `FromSqlRaw` che `FromSqlInterpolated`si comportava nello stesso modo di più recente e . Era facile chiamare accidentalmente il metodo stringa non elaborata quando l'intento era quello di chiamare il metodo stringa interpolata e viceversa. La chiamata accidentale di overload errato potrebbe causare che le query non vengano parametrizzate quando avrebbero dovuto essere.
