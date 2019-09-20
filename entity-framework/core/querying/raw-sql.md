---
title: Query SQL non elaborate - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: b0c9ba1bb452e47e8348d000e3f7b88cc2730d8e
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149305"
---
# <a name="raw-sql-queries"></a>Query SQL non elaborate

Entity Framework Core consente di ricorrere a query SQL non elaborate quando si lavora con un database relazionale. Ciò può essere utile se la query da eseguire non può essere espressa usando LINQ oppure se l'uso di una query LINQ comporta l'invio di query SQL non efficienti. Le query SQL non elaborate possono restituire i tipi di entità o, a partire da EF Core 2,1, i [tipi di entità senza chiave](xref:core/modeling/keyless-entity-types) che fanno parte del modello.

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) di questo articolo in GitHub.

## <a name="basic-raw-sql-queries"></a>Query SQL non elaborate di base

È possibile usare il metodo di estensione *FromSql* per avviare una query LINQ in base a una query SQL non elaborata.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

Per eseguire una stored procedure, è possibile usare query SQL non elaborate.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Passaggio di parametri

Come con qualsiasi API che accetta SQL, è importante parametrizzare qualsiasi input utente per la protezione da attacchi SQL injection. Si possono includere segnaposto dei parametri nella stringa di query SQL e quindi fornire i valori dei parametri come argomenti aggiuntivi. I valori dei parametri forniti verranno convertiti automaticamente in `DbParameter`.

L'esempio seguente passa un parametro singolo a una stored procedure. Anche se apparentemente sembra sintassi `String.Format`, per il valore fornito viene eseguito il wrapping in un parametro e il nome di parametro generato viene inserito nella posizione in cui è stato specificato il segnaposto `{0}`.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Questa è la stessa query ma usa la sintassi di interpolazione di stringa, supportata in EF Core 2.0 e versioni successive:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

È anche possibile costruire un DbParameter e fornirlo come valore del parametro:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

In questo modo è possibile usare parametri denominati nella stringa di query SQL, operazione che risulta utile quando una stored procedure include parametri facoltativi:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Composizione con LINQ

Se la query SQL può essere composta nel database, è possibile estendere la query SQL non elaborata iniziale usando operatori LINQ. Le query SQL componibili iniziano con la parola chiave `SELECT`.

L'esempio seguente usa una query SQL non elaborata che consente di effettuare una selezione da una funzione con valori di tabella e quindi di usare la composizione con LINQ per eseguire operazioni di filtro e ordinamento.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

## <a name="change-tracking"></a>Rilevamento modifiche

Le query che usano `FromSql()` osservano le stesse regole di rilevamento modifiche di qualsiasi altra query LINQ in EF Core. Se ad esempio la query proietta tipi di entità, i risultati vengono rilevati per impostazione predefinita.  

L'esempio seguente usa una query SQL non elaborata che effettua selezioni da una funzione con valori di tabella (TVF), quindi disabilita il rilevamento delle modifiche mediante la chiamata di .AsNoTracking():

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a>Inclusione di dati correlati

Il metodo `Include()` può essere usato per includere dati correlati, come in qualsiasi altra query LINQ:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

## <a name="limitations"></a>Limitazioni

Esistono alcune limitazioni da tenere presenti quando si usano query SQL non elaborate:

* La query SQL deve restituire i dati per tutte le proprietà del tipo di entità o query.

* I nomi delle colonne nel set di risultati devono corrispondere ai nomi di colonna a cui sono mappate le proprietà. Si noti che questo comportamento è diverso da EF6 in cui il mapping di proprietà/colonne viene ignorato per le query SQL non elaborate e i nomi di colonna del set di risultati devono corrispondere ai nomi di proprietà.

* La query SQL non può contenere dati correlati. Tuttavia, in molti casi è possibile estendere la query usando l'operatore `Include` per restituire i dati correlati (vedere [Inclusione di dati correlati](#including-related-data)).

* Le istruzioni `SELECT` passate a questo metodo devono essere in genere componibili: se EF Core deve valutare operatori di query aggiuntivi nel server (ad esempio, per convertire gli operatori LINQ applicati dopo `FromSql`), le istruzioni SQL fornite verranno considerate una sottoquery. Questo significa che le istruzioni SQL passate non devono contenere caratteri o opzioni non validi in una sottoquery, ad esempio:
  * Un punto e virgola finale
  * In SQL Server, un hint a livello di query finale, ad esempio `OPTION (HASH JOIN)`
  * In SQL Server, una clausola `ORDER BY` non accompagnata da `OFFSET 0` o `TOP 100 PERCENT` nella clausola `SELECT`

* Le istruzioni SQL diverse da `SELECT` vengono riconosciute automaticamente come non componibili. Di conseguenza, i risultati completi delle stored procedure vengono sempre restituiti al client e tutti gli operatori LINQ applicati dopo `FromSql` vengono valutati in memoria.

> [!WARNING]  
> **Usare sempre la parametrizzazione per le query SQL non elaborate:** Oltre a convalidare l'input dell'utente, usare sempre la parametrizzazione per qualsiasi valore usato in query/comandi SQL non elaborati. le API che accettano una stringa SQL non elaborata come `FromSql` e `ExecuteSqlCommand` consentono di passare facilmente i valori come parametri. Gli overload di `FromSql` e `ExecuteSqlCommand` che accettano FormattableString consentono anche di usare la sintassi dell'interpolazione di stringhe in modo da proteggersi dagli attacchi SQL injection. 
> 
> Se si usa la concatenazione o l'interpolazione di stringhe per creare dinamicamente qualsiasi parte della stringa di query o si passa l'input utente a istruzioni o stored procedure in grado di eseguire tali input come SQL dinamico, è responsabilità dell'utente convalidare l'input per proteggersi dagli attacchi SQL injection.
