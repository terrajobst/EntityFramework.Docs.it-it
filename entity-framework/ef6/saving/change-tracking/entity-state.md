---
title: Utilizzo di Entity Framework 6 - stati di entità
author: divega
ms.date: 10/23/2016
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: c1dde7810d1dfa8a73e6bd2cf091b24be6b865d8
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490661"
---
# <a name="working-with-entity-states"></a>Utilizzo di stati di entità
In questo argomento illustra come aggiungere e associare le entità a un contesto e le modalità di elaborazione durante SaveChanges Entity Framework.
Entity Framework si occupa di rilevamento lo stato delle entità mentre sono connessi a un contesto, ma negli scenari disconnessi o a più livelli è possibile informare EF allo stato delle entità deve essere in.
Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.  

## <a name="entity-states-and-savechanges"></a>Stati di entità e salvataggio di modifiche

Un'entità può essere in uno dei cinque stati di base a quanto definito dall'enumerazione EntityState. Questi stati sono:  

- Aggiunto: l'entità è rilevato dal contesto, ma non esiste ancora nel database  
- Non modificato: l'entità è rilevato dal contesto ed esiste nel database e i valori delle proprietà non modificati dai valori nel database  
- Modificato: l'entità è rilevato dal contesto e nel database esiste, e sono stati modificati alcuni o tutti i valori delle proprietà  
- Eliminato: l'entità è rilevato dal contesto e nel database esiste, ma è stato contrassegnato per l'eliminazione dal database la volta successiva che viene chiamato SaveChanges  
- Scollegato: l'entità non viene rilevato dal contesto  

SaveChanges esegue diverse operazioni per le entità nei diversi stati:  

- Entità invariate vengono ignorate da SaveChanges. Gli aggiornamenti non vengono inviati al database per le entità nello stato Unchanged.  
- Aggiungere le entità vengono inserite nel database e quindi diventano invariate quando restituisce SaveChanges.  
- Entità modificate vengono aggiornate nel database e quindi diventare invariate quando restituisce SaveChanges.  
- Le entità eliminate vengono eliminate dal database e sono quindi disconnesso dal contesto del.  

Gli esempi seguenti illustrano i modi in cui è possibile modificare lo stato di un'entità o un grafico di entità.  

## <a name="adding-a-new-entity-to-the-context"></a>Aggiunta di una nuova entità al contesto  

Una nuova entità possono essere aggiunti al contesto chiamando il metodo Add sul DbSet.
Questo, l'entità passa allo stato Added, vale a dire che verrà inserito nel database la prossima volta che viene chiamato SaveChanges.
Ad esempio:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

Un altro modo per aggiungere una nuova entità al contesto consiste nel modificare lo stato su Added. Ad esempio:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

Infine, è possibile aggiungere una nuova entità al contesto per eseguire l'hook fino a un'altra entità che è già registrata.
Potrebbe trattarsi di aggiungendo la nuova entità alla proprietà di navigazione dell'insieme di un'altra entità o impostando una proprietà di navigazione di riferimento di un'altra entità in modo che punti alla nuova entità. Ad esempio:  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    var blog = context.Blogs.Find(2);
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

Si noti che per tutti questi esempi, se l'entità aggiunta dispone di riferimenti ad altre entità che non sono ancora rilevate quindi queste nuove entità verranno aggiunti al contesto e verranno inserito nel database la prossima volta che viene chiamato SaveChanges.  

## <a name="attaching-an-existing-entity-to-the-context"></a>Collegamento di un'entità esistente nel contesto  

Se si dispone di un'entità che già conosci già esiste nel database ma che non attualmente viene rilevato dal contesto è possibile impostare il contesto per tenere traccia delle entità usando il metodo Attach DbSet. L'entità sarà nello stato Unchanged nel contesto. Ad esempio:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

Si noti che non sarà possibile apportare modifiche al database se viene chiamato SaveChanges senza eseguire eventuali altre modifiche dell'entità associata. Questo avviene perché l'entità è nello stato Unchanged.  

Un altro modo per collegare un'entità esistente nel contesto consiste nel modificare lo stato su Unchanged. Ad esempio:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

Nota che per entrambi questi esempi se l'entità che viene connesso contiene riferimenti ad altre entità che non vengono ancora rilevate quindi queste nuove entità verrà inoltre collegato nel contesto nello stato Unchanged.  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a>Collegamento di un oggetto esistente ma entità modificata al contesto  

Se si dispone di un'entità che già conosci già esiste nel database ma quali modifiche sono state rese è possibile impostare il contesto per associare l'entità e il relativo stato verrà impostato su Modified.
Ad esempio:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

Quando si modifica lo stato su Modified tutte le proprietà dell'entità verranno contrassegnate come modificata e i valori delle proprietà verranno inviati al database quando viene chiamato SaveChanges.  

Si noti che se l'entità che viene connesso contiene riferimenti ad altre entità che non vengono ancora rilevate, queste nuove entità verrà collegato al contesto nello stato Unchanged, essi non diventano automaticamente Modified.
Se si dispone di più entità che devono essere contrassegnati come modificati è necessario impostare lo stato singolarmente per ognuna di queste entità.  

## <a name="changing-the-state-of-a-tracked-entity"></a>Modifica dello stato di un'entità rilevata  

È possibile modificare lo stato di un'entità che è già rilevato impostando la proprietà State nella voce corrispondente. Ad esempio:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

Si noti che la chiamata a Add o Connetti per un'entità che è già registrata è anche utilizzabile per modificare lo stato dell'entità. Ad esempio, la chiamata di connessione per un'entità che si trova attualmente nello stato Added lo stato passerà su Unchanged.  

## <a name="insert-or-update-pattern"></a>Inserire o aggiornare modello  

Un modello comune per alcune applicazioni è aggiungere un'entità come nuovo (risultante in un database di inserimento) o collegare un'entità esistente e contrassegnarla come modificata (risultante in un aggiornamento del database) in base al valore della chiave primaria.
Ad esempio, quando si usano chiavi primarie integer generato dal database è comune a trattare un'entità con una chiave di nuovo e un'entità con una chiave diversa da zero come esistente.
Questo modello può essere ottenuto impostando lo stato dell'entità basato su un controllo del valore della chiave primario. Ad esempio:  

``` csharp
public void InsertOrUpdate(Blog blog)
{
    using (var context = new BloggingContext())
    {
        context.Entry(blog).State = blog.BlogId == 0 ?
                                   EntityState.Added :
                                   EntityState.Modified;

        context.SaveChanges();
    }
}
```  

Si noti che quando si modifica lo stato su Modified tutte le proprietà dell'entità verranno contrassegnate come modificata e i valori delle proprietà verranno inviati al database quando viene chiamato SaveChanges.  
