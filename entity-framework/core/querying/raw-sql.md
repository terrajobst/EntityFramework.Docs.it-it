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
# <a name="raw-sql-queries"></a>Query SQL non elaborate

Entity Framework Core consente di ricorrere a query SQL non elaborate quando si lavora con un database relazionale. Ciò può essere utile se la query da eseguire non può essere espressa usando LINQ oppure se l'uso di una query LINQ comporta l'invio di codice SQL non efficiente al database. Le query SQL non elaborate possono restituire tipi di entità o, a partire da EF Core 2.1, [tipi di query](xref:core/modeling/query-types) che fanno parte del modello.

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) di questo articolo in GitHub.

## <a name="limitations"></a>Limitazioni

Esistono alcune limitazioni da tenere presenti quando si usano query SQL non elaborate:

* La query SQL deve restituire i dati per tutte le proprietà del tipo di entità o query.

* I nomi delle colonne nel set di risultati devono corrispondere ai nomi di colonna a cui sono mappate le proprietà. Si noti che questo comportamento è diverso da EF6 in cui il mapping di proprietà/colonne viene ignorato per le query SQL non elaborate e i nomi di colonna del set di risultati devono corrispondere ai nomi di proprietà.

* La query SQL non può contenere dati correlati. Tuttavia, in molti casi è possibile estendere la query usando l'operatore `Include` per restituire i dati correlati (vedere [Inclusione di dati correlati](#including-related-data)).

* Le istruzioni `SELECT` passate a questo metodo devono essere in genere componibili. Se EF Core deve valutare operatori di query aggiuntivi nel server (ad esempio, per convertire gli operatori LINQ applicati dopo `FromSql`), le istruzioni SQL fornite verranno considerate una sottoquery. Questo significa che le istruzioni SQL passate non devono contenere caratteri o opzioni non validi in una sottoquery, ad esempio:
  * Un punto e virgola finale
  * In SQL Server, un hint a livello di query finale, ad esempio `OPTION (HASH JOIN)`
  * In SQL Server, una clausola `ORDER BY` non accompagnata da `TOP 100 PERCENT` nella clausola `SELECT`

* Le istruzioni SQL diverse da `SELECT` vengono riconosciute automaticamente come non componibili. Di conseguenza, i risultati completi delle stored procedure vengono sempre restituiti al client e tutti gli operatori LINQ applicati dopo `FromSql` vengono valutati in memoria.

## <a name="basic-raw-sql-queries"></a>Query SQL non elaborate di base

È possibile usare il metodo di estensione *FromSql* per avviare una query LINQ in base a una query SQL non elaborata.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

Per eseguire una stored procedure, è possibile usare query SQL non elaborate.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Passaggio di parametri

Come con qualsiasi API che accetta SQL, è importante parametrizzare qualsiasi input utente per la protezione da attacchi SQL injection. Si possono includere segnaposto dei parametri nella stringa di query SQL e quindi fornire i valori dei parametri come argomenti aggiuntivi. I valori dei parametri forniti verranno convertiti automaticamente in `DbParameter`.

L'esempio seguente passa un parametro singolo a una stored procedure. Anche se apparentemente sembra sintassi `String.Format`, per il valore fornito viene eseguito il wrapping in un parametro e il nome di parametro generato viene inserito nella posizione in cui è stato specificato il segnaposto `{0}`.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Questa è la stessa query ma usa la sintassi di interpolazione di stringa, supportata in EF Core 2.0 e versioni successive:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

È anche possibile costruire un DbParameter e fornirlo come valore del parametro. In questo modo è possibile usare parametri denominati nella stringa di query SQL

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Composizione con LINQ

Se la query SQL può essere composta nel database, è possibile estendere la query SQL non elaborata iniziale usando operatori LINQ. Nelle query SQL componibili è possibile usare la parola chiave `SELECT`.

L'esempio seguente usa una query SQL non elaborata che consente di effettuare una selezione da una funzione con valori di tabella e quindi di usare la composizione con LINQ per eseguire operazioni di filtro e ordinamento.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a>Inclusione di dati correlati

La composizione con operatori LINQ può essere usata per includere dati correlati nella query.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> **Usare sempre la parametrizzazione per query SQL non elaborate:** le API che accettano una stringa SQL non elaborata come `FromSql` e `ExecuteSqlCommand` consentono di passare facilmente i valori come parametri. Oltre a convalidare l'input dell'utente, usare sempre la parametrizzazione per qualsiasi valore usato in query/comandi SQL non elaborati. Se si usa la concatenazione di stringhe per compilare in modo dinamico qualsiasi parte della stringa di query, si è responsabili della convalida di qualsiasi input per la protezione da attacchi SQL injection.
