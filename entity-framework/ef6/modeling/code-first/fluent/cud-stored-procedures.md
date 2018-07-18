---
title: Codice primo inserimento, aggiornamento ed eliminazione di Stored procedure - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
caps.latest.revision: 3
ms.openlocfilehash: 1f100ed888abd98df83c80d0de2086cfb1ba7b4f
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122750"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a>Codice primo inserimento, aggiornamento ed eliminazione di Stored procedure
> [!NOTE]
> **Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.  

Per impostazione predefinita, Code First configurerà tutte le entità per eseguire insert, aggiornare ed eliminare i comandi usando l'accesso diretto alle tabelle. A partire da Entity Framework 6 è possibile configurare il modello Code First per utilizzare le stored procedure per alcune o tutte le entità nel modello.  

## <a name="basic-entity-mapping"></a>Mapping di entità di base  

È possibile acconsentire esplicitamente tramite le stored procedure per l'inserimento, aggiornare ed eliminare tramite l'API Fluent.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

Questa operazione causerà Code First per usare alcune convenzioni per compilare la struttura prevista delle stored procedure nel database.  

- Tre stored procedure denominate  **\<type_name\>Inserisci**,  **\<type_name\>Aggiorna** e  **\<type_ nome\>Elimina** (ad esempio, Blog_Insert Blog_Update e Blog_Delete).  
- I nomi dei parametri corrispondono ai nomi delle proprietà.  
  > [!NOTE]
  > Se si usa HasColumnName() o l'attributo di colonna per rinominare la colonna per una determinata proprietà questo nome viene usato per i parametri anziché il nome della proprietà.  
- **Stored procedure insert** disporrà di un parametro per ogni proprietà, ad eccezione di quelli contrassegnati come archivio generato (identity o calcolate). La stored procedure deve restituire un set di risultati con una colonna per ogni proprietà dell'archivio generato.  
- **Stored procedure di aggiornamento** disporrà di un parametro per ogni proprietà, ad eccezione di quelli contrassegnati con un modello di archivio generato di 'Computed'. Alcuni token di concorrenza che richiedono un parametro per il valore originale, vedere la *token di concorrenza* sezione per informazioni dettagliate. La stored procedure deve restituire un set di risultati con una colonna per ogni proprietà calcolata.  
- **L'eliminazione di stored procedure** deve contenere un parametro per il valore della chiave di entità (o più parametri se l'entità dispone di una chiave composta). Inoltre, la procedura di eliminazione deve avere anche parametri per chiavi esterne associazione indipendente nella tabella di destinazione (relazioni che non dispongono di proprietà chiave esterna corrispondenti dichiarate nell'entità). Alcuni token di concorrenza che richiedono un parametro per il valore originale, vedere la *token di concorrenza* sezione per informazioni dettagliate.  

Usando la classe seguente come esempio:  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

Il valore predefinito sarebbero stored procedure:  

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS BlogId
END
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId;
CREATE PROCEDURE [dbo].[Blog_Delete]  
  @BlogId int  
AS  
  DELETE FROM [dbo].[Blogs]
  WHERE BlogId = @BlogId
```  

### <a name="overriding-the-defaults"></a>Viene sottoposto a override le impostazioni predefinite  

È possibile eseguire l'override di parte o tutto ciò che è stato configurato per impostazione predefinita.  

È possibile modificare il nome di uno o più stored procedure. Questo esempio Rinomina la stored procedure update solo.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

Questo esempio Rinomina tutte le tre stored procedure.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

In questi esempi le chiamate vengono concatenate tra loro, ma è anche possibile usare la sintassi di blocco di espressione lambda.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    {  
      s.Update(u => u.HasName("modify_blog"));  
      s.Delete(d => d.HasName("delete_blog"));  
      s.Insert(i => i.HasName("insert_blog"));  
    });
```  

Questo esempio Rinomina il parametro per la proprietà BlogId in stored procedure update.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

Queste chiamate sono tutti concatenabile e componibile. Ecco un esempio che consente di ridenominare tutte le tre stored procedure e i relativi parametri.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")  
                   .Parameter(b => b.BlogId, "blog_id")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url"))  
     .Delete(d => d.HasName("delete_blog")  
                   .Parameter(b => b.BlogId, "blog_id"))  
     .Insert(i => i.HasName("insert_blog")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url")));
```  

È anche possibile modificare il nome delle colonne nel set di risultati che contiene i valori del database generato.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures(s =>
    s.Insert(i => i.Result(b => b.BlogId, "generated_blog_identity")));
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS generated_blog_id
END
```  

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a>Relazioni senza una chiave esterna nella classe (associazioni indipendenti)  

Quando una proprietà di chiave esterna viene inclusa nella definizione della classe, il parametro corrispondente può essere rinominato in esattamente come qualsiasi altra proprietà. Se non esiste una relazione senza una proprietà di chiave esterna nella classe, il nome di parametro predefinito è  **\<navigation_property_name\>_\<primary_key_name\>**.  

Ad esempio, le definizioni di classe seguente comporta un parametro Blog_BlogId viene previsto nelle stored procedure per inserire e aggiornare gli invii.  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }

  public List<Post> Posts { get; set; }  
}  

public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public Blog Blog { get; set; }  
}
```  

### <a name="overriding-the-defaults"></a>Viene sottoposto a override le impostazioni predefinite  

È possibile modificare i parametri per le chiavi esterne che non sono inclusi nella classe specificando il percorso per la proprietà di chiave primaria per il metodo con parametri.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

Se non si dispone di una proprietà di navigazione nell'entità dipendente (ad es. Nessuna proprietà Post.Blog) è possibile usare il metodo di associazione per identificare l'altra estremità della relazione e quindi configurare i parametri che corrispondono a ogni le proprietà di chiave.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a>Token di concorrenza  

Aggiornamento ed eliminazione di stored procedure potrebbe essere necessario anche gestire la concorrenza:  

- Se l'entità contiene i token di concorrenza, la stored procedure può facoltativamente includere un parametro di output che restituisce il numero di righe aggiornate/eliminate (righe interessate). Tale parametro deve essere configurato utilizzando il metodo RowsAffectedParameter.  
Per impostazione predefinita Entity Framework Usa il valore restituito da ExecuteNonQuery per determinare il numero di righe interessato. Specificare un parametro di output delle righe interessate è utile se si esegue qualsiasi logica nella stored procedure che comporterebbe il valore restituito di ExecuteNonQuery risulti errato (dal punto di vista di Entity Framework) alla fine dell'esecuzione.  
- Per ogni concorrenza di token vi sarà un parametro denominato  **\<property_name\>_Original** (ad esempio, Timestamp_Original). Questo verrà passato il valore originale di questa proprietà: il valore quando sottoposto a query dal database.  
    - I token di concorrenza che vengono calcolati dal database, ad esempio i timestamp: disporrà solo un parametro di valore originale.  
    - Le proprietà calcolate non sono impostate come token di concorrenza avrà inoltre un parametro per il nuovo valore nella procedura di aggiornamento. Usa le convenzioni di denominazione già illustrate per i nuovi valori. Un esempio di un token di questo tipo potrebbe essere utilizzando l'URL del Blog come un token di concorrenza, il nuovo valore è obbligatorio perché ciò può essere aggiornata a un nuovo valore da codice (a differenza di un token di Timestamp che viene aggiornato solo dal database).  

Si tratta di un esempio di classe e aggiornare stored procedure con un token di concorrenza di timestamp.  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
  [Timestamp]
  public byte[] Timestamp { get; set; }
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Timestamp_Original rowversion  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Timestamp] = @Timestamp_Original
```  

Ecco un esempio di classe e aggiornare stored procedure con token di concorrenza non calcolata.  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  [ConcurrencyCheck]
  public string Url { get; set; }  
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Url_Original nvarchar(max),
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Url] = @Url_Original
```  

### <a name="overriding-the-defaults"></a>Viene sottoposto a override le impostazioni predefinite  

È facoltativamente possibile introdurre un parametro delle righe interessate.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

Per i token di concorrenza di database calcolato, in cui viene passato solo il valore originale: è possibile utilizzare solo il parametro standard ridenominazione meccanismo per rinominare il parametro per il valore originale.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

Per i token di concorrenza non calcolata, in cui sia il valore originale e quello nuovo viene passato – è possibile usare un overload di parametro che consente di specificare un nome per ogni parametro.  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a>Relazioni many-to-many  

Le classi seguenti si userà come esempio in questa sezione.  

``` csharp
public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public List<Tag> Tags { get; set; }  
}  

public class Tag  
{  
  public int TagId { get; set; }  
  public string TagName { get; set; }  

  public List<Post> Posts { get; set; }  
}
```  

Molti-a-molti relazioni possono essere mappate alle stored procedure con la sintassi seguente.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

Se viene fornita alcuna altra configurazione la forma di stored procedure seguente viene utilizzata per impostazione predefinita.  

- Due stored procedure denominate  **\<type_one\>\<type_two\>Inserisci** e  **\<type_one\>\<type_two \>Elimina** (ad esempio, PostTag_Insert e PostTag_Delete).  
- I parametri saranno i valori di chiave per ogni tipo. Il nome di ogni parametro in corso **\<type_name\>_\<property_name\>** (ad esempio, Post_PostId e Tag_TagId).

Di seguito sono esempio inseriscono e aggiornano le stored procedure.  

``` SQL
CREATE PROCEDURE [dbo].[PostTag_Insert]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  INSERT INTO [dbo].[Post_Tags] (Post_PostId, Tag_TagId)   
  VALUES (@Post_PostId, @Tag_TagId)
CREATE PROCEDURE [dbo].[PostTag_Delete]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  DELETE FROM [dbo].[Post_Tags]    
  WHERE Post_PostId = @Post_PostId AND Tag_TagId = @Tag_TagId
```  

### <a name="overriding-the-defaults"></a>Viene sottoposto a override le impostazioni predefinite  

Il procedura e nomi di parametro possono essere configurati in modo analogo alle procedure entità archiviata.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.HasName("add_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id"))  
     .Delete(d => d.HasName("remove_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id")));
```  
