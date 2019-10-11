---
title: Query SQL non elaborate - EF Core
author: smitpatel
ms.date: 10/08/2019
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 33601d570fa0b7a1fcada1705843da3798c00094
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181976"
---
# <a name="raw-sql-queries"></a>Query SQL non elaborate

Entity Framework Core consente di ricorrere a query SQL non elaborate quando si lavora con un database relazionale. Le query SQL non elaborate sono utili se la query desiderata non può essere espressa mediante LINQ. Le query SQL non elaborate vengono usate anche se l'uso di una query LINQ produce una query SQL inefficiente. Le query SQL non elaborate possono restituire tipi di entità regolari o [tipi di entità autochiave](xref:core/modeling/keyless-entity-types) che fanno parte del modello.

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/RawSQL/Sample.cs) di questo articolo in GitHub.

## <a name="basic-raw-sql-queries"></a>Query SQL non elaborate di base

È possibile usare il metodo di estensione `FromSqlRaw` per iniziare una query LINQ basata su una query SQL non elaborata. `FromSqlRaw` può essere usato solo nelle radici di query, direttamente nel `DbSet<>`.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRaw)]

Per eseguire una stored procedure, è possibile usare query SQL non elaborate.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedure)]

## <a name="passing-parameters"></a>Passaggio di parametri

> [!WARNING]
> **Usa sempre la parametrizzazione per query SQL non elaborate**
>
> Quando si introducono valori forniti dall'utente in una query SQL non elaborata, è necessario prestare attenzione per evitare attacchi intrusivi nel codice SQL. Oltre a convalidare che tali valori non contengono caratteri non validi, utilizzare sempre la parametrizzazione che invia i valori separati dal testo SQL.
>
> In particolare, non passare mai una stringa concatenata o interpolata (`$""`) con valori specificati dall'utente non convalidati in `FromSqlRaw` o `ExecuteSqlRaw`. I metodi `FromSqlInterpolated` e `ExecuteSqlInterpolated` consentono l'uso della sintassi di interpolazione delle stringhe in modo da proteggere gli attacchi intrusivi nel codice SQL.

Nell'esempio seguente viene passato un singolo parametro a un stored procedure includendo un segnaposto di parametro nella stringa di query SQL e fornendo un argomento aggiuntivo. Sebbene questa sintassi possa apparire come la sintassi `String.Format`, il valore fornito viene racchiuso in un `DbParameter` e il nome del parametro generato viene inserito nel punto in cui è stato specificato il segnaposto `{0}`.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureParameter)]

`FromSqlInterpolated` è simile a `FromSqlRaw`, ma consente di usare la sintassi di interpolazione delle stringhe. Analogamente a `FromSqlRaw`, `FromSqlInterpolated` può essere utilizzato solo nelle radici di query. Come nell'esempio precedente, il valore viene convertito in un `DbParameter` e non è vulnerabile a SQL injection.

> [!NOTE]
> Prima della versione 3,0, `FromSqlRaw` e `FromSqlInterpolated` erano due overload denominati `FromSql`. Per ulteriori informazioni, vedere la [sezione versioni precedenti](#previous-versions).

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedStoredProcedureParameter)]

È anche possibile costruire un DbParameter e fornirlo come valore del parametro. Poiché viene usato un segnaposto di parametro SQL normale, anziché un segnaposto di stringa, `FromSqlRaw` può essere usato in modo sicuro:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureSqlParameter)]

`FromSqlRaw` consente di utilizzare parametri denominati nella stringa di query SQL, utile quando un stored procedure dispone di parametri facoltativi:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlRawStoredProcedureNamedSqlParameter)]

## <a name="composing-with-linq"></a>Composizione con LINQ

È possibile comporre la query SQL RAW iniziale usando gli operatori LINQ. EF Core lo considererà come sottoquery e compose su di esso nel database. Nell'esempio seguente viene utilizzata una query SQL non elaborata che consente di selezionare da una funzione con valori di tabella (TVF). E quindi lo compone usando LINQ per eseguire operazioni di filtro e ordinamento.

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedComposed)]

La query precedente genera SQL seguente:

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

Per la composizione con LINQ è necessario che la query SQL non elaborata sia componibile perché EF Core considererà il SQL fornito come una sottoquery. Le query SQL componibili iniziano con la parola chiave `SELECT`. Inoltre, SQL passato non deve contenere caratteri o opzioni non valide per una sottoquery, ad esempio:

- Un punto e virgola finale
- In SQL Server, un hint a livello di query finale, ad esempio `OPTION (HASH JOIN)`
- In SQL Server, una clausola `ORDER BY` non usata con `OFFSET 0` o `TOP 100 PERCENT` nella clausola `SELECT`

SQL Server non consente la composizione su chiamate stored procedure, quindi qualsiasi tentativo di applicare operatori di query aggiuntivi a tale chiamata genererà SQL non valido. Usare il metodo `AsEnumerable` o `AsAsyncEnumerable` subito dopo i metodi `FromSqlRaw` o `FromSqlInterpolated` per assicurarsi che EF Core non tenti di comporre su una stored procedure.

## <a name="change-tracking"></a>Rilevamento modifiche

Le query che utilizzano i metodi `FromSqlRaw` o `FromSqlInterpolated` seguono esattamente le stesse regole di rilevamento delle modifiche di qualsiasi altra query LINQ in EF Core. Se ad esempio la query proietta tipi di entità, i risultati vengono rilevati per impostazione predefinita.

Nell'esempio seguente viene utilizzata una query SQL non elaborata che seleziona da una funzione con valori di tabella (TVF), quindi Disabilita il rilevamento delle modifiche con la chiamata a `AsNoTracking`:

[!code-csharp[Main](../../../samples/core/Querying/RawSQL/Sample.cs#FromSqlInterpolatedAsNoTracking)]

## <a name="limitations"></a>Limitazioni

Esistono alcune limitazioni da tenere presenti quando si usano query SQL non elaborate:

- La query SQL deve restituire i dati per tutte le proprietà del tipo di entità.
- I nomi delle colonne nel set di risultati devono corrispondere ai nomi di colonna a cui sono mappate le proprietà. Si noti che questo comportamento è diverso da EF6. EF6 ignorata la proprietà nel mapping delle colonne per le query SQL non elaborate e i nomi delle colonne del set di risultati devono corrispondere ai nomi delle proprietà.
- La query SQL non può contenere dati correlati. Tuttavia, in molti casi è possibile estendere la query usando l'operatore `Include` per restituire i dati correlati (vedere [Inclusione di dati correlati](#including-related-data)).

## <a name="previous-versions"></a>Versioni precedenti

EF Core versione 2,2 e versioni precedenti hanno due overload del metodo denominato `FromSql`, che si comportavano nello stesso modo dei `FromSqlRaw` e `FromSqlInterpolated` più recenti. È stato facile chiamare accidentalmente il metodo stringa non elaborata quando lo scopo era quello di chiamare il metodo della stringa interpolata e viceversa. La chiamata accidentale di un overload errato potrebbe causare la mancata esecuzione di query con parametri.
