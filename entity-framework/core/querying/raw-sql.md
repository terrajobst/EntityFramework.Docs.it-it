---
title: Query SQL non elaborate - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: ebec5775770c0f1e297eaaf35bf644c605a69afc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197766"
---
# <a name="raw-sql-queries"></a>Query SQL non elaborate

Entity Framework Core consente di ricorrere a query SQL non elaborate quando si lavora con un database relazionale. Questa operazione può essere utile se la query che si desidera eseguire non può essere espressa mediante LINQ o se l'utilizzo di una query LINQ produce una query SQL inefficiente. Le query SQL non elaborate possono restituire tipi di entità regolari o [tipi di entità autochiave](xref:core/modeling/keyless-entity-types) che fanno parte del modello.

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) di questo articolo in GitHub.

## <a name="basic-raw-sql-queries"></a>Query SQL non elaborate di base

È possibile utilizzare il `FromSqlRaw` metodo di estensione per iniziare una query LINQ basata su una query SQL non elaborata.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
```

Per eseguire una stored procedure, è possibile usare query SQL non elaborate.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Passaggio di parametri

> [!WARNING]
> **Usa sempre la parametrizzazione per query SQL non elaborate**
>
> Quando si introducono valori forniti dall'utente in una query SQL non elaborata, è necessario prestare attenzione per evitare attacchi intrusivi nel codice SQL. Oltre a convalidare che tali valori non contengono caratteri non validi, utilizzare sempre la parametrizzazione che invia i valori separati dal testo SQL.
>
> In particolare, non passare mai una stringa concatenata o interpolata (`$""`) con valori specificati dall'utente non convalidati in `FromSqlRaw` o `ExecuteSqlRaw`. I `FromSqlInterpolated` metodi `ExecuteSqlInterpolated` e consentono di utilizzare la sintassi di interpolazione delle stringhe in modo da proteggere gli attacchi intrusivi nel codice SQL.

Nell'esempio seguente viene passato un singolo parametro a un stored procedure includendo un segnaposto di parametro nella stringa di query SQL e fornendo un argomento aggiuntivo. Sebbene possa sembrare una sintassi `String.Format` simile, il valore fornito viene racchiuso in `DbParameter` un oggetto e il nome del parametro generato `{0}` viene inserito nel punto in cui è stato specificato il segnaposto.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

In alternativa a `FromSqlRaw`, è possibile usare `FromSqlInterpolated` che consente l'uso sicuro dell'interpolazione di stringhe. Come nell'esempio precedente, il valore viene convertito in un oggetto `DbParameter` e pertanto non è vulnerabile a SQL injection:

> [!NOTE]
> Prima della versione 3,0 `FromSqlRaw` ed `FromSqlInterpolated` erano due overload denominati `FromSql`. Per ulteriori informazioni, vedere la [sezione versioni precedenti](#previous-versions) .


<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlInterpolated($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

È anche possibile costruire un DbParameter e fornirlo come valore del parametro. Poiché viene usato un segnaposto di parametro SQL normale, anziché un segnaposto `FromSqlRaw` di stringa, può essere usato in modo sicuro:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

In questo modo è possibile usare parametri denominati nella stringa di query SQL, operazione che risulta utile quando una stored procedure include parametri facoltativi:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Composizione con LINQ

Se la query SQL può essere composta nel database, è possibile estendere la query SQL non elaborata iniziale usando operatori LINQ. Le query SQL componibili iniziano con la parola chiave `SELECT`.

L'esempio seguente usa una query SQL non elaborata che consente di effettuare una selezione da una funzione con valori di tabella e quindi di usare la composizione con LINQ per eseguire operazioni di filtro e ordinamento.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

Questa operazione produrrà la query SQL seguente:

``` sql
SELECT [b].[Id], [b].[Name], [b].[Rating]
        FROM (
            SELECT * FROM dbo.SearchBlogs(@p0)
        ) AS b
        WHERE b."Rating" > 3
        ORDER BY b."Rating" DESC
```

## <a name="change-tracking"></a>Rilevamento modifiche

Le query che utilizzano `FromSql` i metodi seguono esattamente le stesse regole di rilevamento delle modifiche di qualsiasi altra query LINQ in EF core. Se ad esempio la query proietta tipi di entità, i risultati vengono rilevati per impostazione predefinita.

Nell'esempio seguente viene utilizzata una query SQL non elaborata che seleziona da una funzione con valori di tabella (TVF), quindi Disabilita il rilevamento delle modifiche `AsNoTracking`con la chiamata a:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a>Inclusione di dati correlati

Il metodo `Include` può essere usato per includere dati correlati, come in qualsiasi altra query LINQ:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

Si noti che questa operazione richiede che la query SQL non elaborata sia componibile; in particolare, non funzionerà con le chiamate di stored procedure. Vedere note sulla composizione in [limitazioni](#limitations).

## <a name="limitations"></a>Limitazioni

Esistono alcune limitazioni da tenere presenti quando si usano query SQL non elaborate:

* La query SQL deve restituire i dati per tutte le proprietà del tipo di entità.

* I nomi delle colonne nel set di risultati devono corrispondere ai nomi di colonna a cui sono mappate le proprietà. Si noti che questo comportamento è diverso da EF6 in cui il mapping di proprietà/colonne viene ignorato per le query SQL non elaborate e i nomi di colonna del set di risultati devono corrispondere ai nomi di proprietà.

* La query SQL non può contenere dati correlati. Tuttavia, in molti casi è possibile estendere la query usando l'operatore `Include` per restituire i dati correlati (vedere [Inclusione di dati correlati](#including-related-data)).

* Le istruzioni `SELECT` passate a questo metodo devono essere in genere componibili: Se EF Core necessario valutare gli operatori di query aggiuntivi sul server (ad esempio, per tradurre gli operatori LINQ applicati `FromSql` dopo i metodi), il SQL fornito verrà considerato come una sottoquery. Questo significa che le istruzioni SQL passate non devono contenere caratteri o opzioni non validi in una sottoquery, ad esempio:
  * un punto e virgola finale
  * In SQL Server, un hint a livello di query finale, ad esempio `OPTION (HASH JOIN)`
  * In SQL Server, una clausola `ORDER BY` non accompagnata da `OFFSET 0` o `TOP 100 PERCENT` nella clausola `SELECT`

* Si noti che SQL Server non consente la composizione su chiamate stored procedure, quindi qualsiasi tentativo di applicare operatori di query aggiuntivi a tale chiamata genererà SQL non valido. Gli operatori di query possono essere `AsEnumerable()` introdotti dopo per la valutazione del client.

# <a name="previous-versions"></a>Versioni precedenti

EF Core versione 2,2 e versioni precedenti hanno due overload denominati `FromSql` che si comportavano nello stesso modo dei più recenti `FromSqlInterpolated` `FromSqlRaw` e. In questo modo è molto semplice chiamare accidentalmente il metodo stringa non elaborata quando lo scopo era quello di chiamare il metodo della stringa interpolata e viceversa. Il risultato potrebbero essere query senza parametri, quando invece è prevista la parametrizzazione.
