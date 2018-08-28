---
title: Tutte le query SQL non elaborate, Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 99893ca1c634ce6f2e4cf9dcb70b1a1e43532c60
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995734"
---
# <a name="raw-sql-queries"></a>Query SQL non elaborate
Entity Framework consente di eseguire una query usando LINQ con le classi di entità. Tuttavia, potrebbero esserci delle volte in cui si desidera eseguire query usando SQL non elaborate direttamente nel database. Ciò include la chiamata di stored procedure che possono essere utili per i modelli di Code First che attualmente non supportano il mapping a stored procedure. Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.  

## <a name="writing-sql-queries-for-entities"></a>Scrittura di query SQL per le entità  

Il metodo SqlQuery su DbSet consente a una query SQL non elaborata deve essere scritto che restituirà le istanze di entità. Gli oggetti restituiti saranno rilevati dal contesto esattamente come verrebbero usati se questi sono stati restituiti da una query LINQ. Ad esempio:  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

Si noti che, come per le query LINQ, la query non viene eseguita fino a quando non vengono enumerati i risultati, nell'esempio precedente questa operazione viene eseguita con la chiamata a ToList.  

Attenzione ogni volta che vengono scritte query SQL non elaborate per due motivi. In primo luogo, la query deve essere scritta per garantire che restituisce solo le entità che sono in realtà del tipo richiesto. Ad esempio, quando si usano funzionalità, ad esempio ereditarietà è facile da scrivere una query che verrà creata l'entità che sono di tipo CLR non corretto.  

In secondo luogo, alcuni tipi di query SQL non elaborata espongano potenziali rischi di sicurezza, soprattutto per quanto riguarda gli attacchi SQL injection. Assicurarsi che i parametri utilizzati nelle query in modo corretto per evitare questi attacchi.  

### <a name="loading-entities-from-stored-procedures"></a>Caricamento delle entità dalle stored procedure  

È possibile usare DbSet.SqlQuery per caricare le entità dai risultati di una stored procedure. Ad esempio, il codice seguente chiama l'utente dbo. GetBlogs procedure nel database:  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

È anche possibile passare parametri a una stored procedure utilizzando la sintassi seguente:  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a>Scrittura di query SQL per i tipi non entità  

Una query SQL che restituisce istanze di qualsiasi tipo, inclusi i tipi primitivi, possono essere create usando il metodo SqlQuery nella classe di Database. Ad esempio:  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

I risultati restituiti da SqlQuery nel Database non verranno rilevati dal contesto mai anche se gli oggetti sono istanze di un tipo di entità.  

## <a name="sending-raw-commands-to-the-database"></a>L'invio di comandi non elaborato nel database  

I comandi di query non possono essere inviati al database usando il metodo ExecuteSqlCommand nel Database. Ad esempio:  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

Si noti che tutte le modifiche apportate ai dati nel database di utilizzando ExecuteSqlCommand sono opache al contesto fino a quando non le entità vengono caricate o ricaricate dal database.  

### <a name="output-parameters"></a>Parametri di output  

Se si utilizzano parametri di output, i relativi valori non sarà disponibili fino a quando i risultati sono stati letti completamente. Ciò è dovuto il comportamento sottostante di DbDataReader, vedere [recupero di dati mediante DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) per altri dettagli.  
