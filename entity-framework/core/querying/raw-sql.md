---
title: Query SQL non elaborato - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: 29b7e20e875bf791a88a92636c1df4bc4e31656b
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2018
---
# <a name="raw-sql-queries"></a>Query SQL non elaborate

Entity Framework Core consente di elenco a discesa di query SQL non elaborate quando si lavora con un database relazionale. Può essere utile se la query da eseguire non può essere espresse utilizzando LINQ, o se tramite una query LINQ è risultante in inefficiente SQL inviati al database.

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) di questo articolo in GitHub.

## <a name="limitations"></a>Limitazioni

Esistono alcune limitazioni da tenere presenti quando si usano query SQL non elaborate:
* Query SQL è utilizzabile solo per restituire i tipi di entità che fanno parte del modello. Un aumento nel nostro backlog per [enable restituzione di tipi ad hoc da una query SQL non elaborate](https://github.com/aspnet/EntityFramework/issues/1862).

* La query SQL deve restituire i dati per tutte le proprietà del tipo di entità o query.

* I nomi delle colonne nel set di risultati deve corrispondere ai nomi di proprietà sono mappate a colonna. Questo è diverso da EF6 in cui il mapping di proprietà o la colonna è stato ignorato per le query SQL non elaborate e nomi devono corrispondere ai nomi di proprietà di colonna del set di risultati.

* La query SQL non può contenere i dati correlati. Tuttavia, in molti casi è possibile comporre sopra la query utilizzando il `Include` operatore per restituire i dati correlati (vedere [inclusi i dati correlati](#including-related-data)).

* `SELECT` istruzioni passate a questo metodo in genere devono essere componibile: Core EF se è necessario valutare gli operatori di query aggiuntive nel server (ad esempio, per convertire gli operatori LINQ applicati dopo `FromSql`), SQL fornito verrà considerato come una sottoquery. Ciò significa che l'istruzione SQL passata non può contenere caratteri o le opzioni non valide in una sottoquery, ad esempio:
  * un punto e virgola finale
  * In SQL Server, a livello di query finale hint, ad esempio `OPTION (HASH JOIN)`
  * In SQL Server, un `ORDER BY` clausola che non è disponibile di `TOP 100 PERCENT` nel `SELECT` clausola

* Istruzioni SQL diverso `SELECT` vengono riconosciute automaticamente come non componibile. Di conseguenza, i risultati completi della stored procedure vengono sempre restituiti al client e tutti gli operatori LINQ applicato dopo `FromSql` vengono valutate in memoria. 

## <a name="basic-raw-sql-queries"></a>Query SQL non elaborate base

È possibile utilizzare il *FromSql* il metodo di estensione per iniziare una query LINQ in base a una query SQL non elaborata.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

Per eseguire una stored procedure, è possibile utilizzare query SQL non elaborata.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Passaggio di parametri

Come con qualsiasi API che accetta SQL, è importante aggiungere parametri di input per proteggersi da un attacco SQL injection qualsiasi utente. È possibile includere i segnaposto dei parametri nella stringa di query SQL e quindi fornire i valori dei parametri come argomenti aggiuntivi. I valori dei parametri è fornire verranno automaticamente convertiti in un `DbParameter`.

Nell'esempio seguente viene passato un parametro singolo a una stored procedure. Mentre il risultato potrebbe essere ad esempio `String.Format` sintassi, il valore fornito è racchiuso in cui vengono inseriti un parametro e il nome di parametro generato il `{0}` segnaposto è stato specificato.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Questa è la stessa query ma utilizzando la sintassi di interpolazione di stringa, che è supportata in EF Core 2.0 e versioni successive:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

È inoltre possibile costruire un DbParameter e fornirlo come valore del parametro. In questo modo è possibile utilizzare parametri denominati nella stringa di query SQL

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Composizione con LINQ

Se la query SQL può essere composte nel database, è possibile comporre sopra la query SQL non elaborata iniziale utilizzando operatori LINQ. Query SQL che possono essere composte in corso con il `SELECT` (parola chiave).

L'esempio seguente usa una query SQL non elaborata che consente di selezionare da una funzione con valori di tabella (TVF) e quindi compone su di esso utilizzando LINQ per eseguire il filtro e ordinamento.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a>Inclusi i dati correlati

Interagire con gli operatori LINQ consente di includere dati correlati nella query.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> **Utilizzare sempre la parametrizzazione per query SQL non elaborata:** di stringa, ad esempio le API che accettano un database SQL non elaborato `FromSql` e `ExecuteSqlCommand` consentono i valori essere facilmente passati come parametri. Oltre a convalidare l'input dell'utente, utilizzare sempre la parametrizzazione per tutti i valori utilizzati in non elaborato o comando di query SQL. Se si utilizza la concatenazione di stringhe per compilare in modo dinamico qualsiasi parte della stringa di query, quindi l'utente è responsabile per la convalida di input per proteggersi da attacchi SQL injection.
