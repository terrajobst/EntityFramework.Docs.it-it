---
title: Codice primo inserimento, aggiornamento ed eliminazione di Stored procedure - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
ms.openlocfilehash: bfc56671814aec1965ac054ff901297e5cdbbecb
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489622"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a><span data-ttu-id="7d690-102">Codice primo inserimento, aggiornamento ed eliminazione di Stored procedure</span><span class="sxs-lookup"><span data-stu-id="7d690-102">Code First Insert, Update, and Delete Stored Procedures</span></span>
> [!NOTE]
> <span data-ttu-id="7d690-103">**Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="7d690-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="7d690-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="7d690-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="7d690-105">Per impostazione predefinita, Code First configurerà tutte le entità per eseguire insert, aggiornare ed eliminare i comandi usando l'accesso diretto alle tabelle.</span><span class="sxs-lookup"><span data-stu-id="7d690-105">By default, Code First will configure all entities to perform insert, update and delete commands using direct table access.</span></span> <span data-ttu-id="7d690-106">A partire da Entity Framework 6 è possibile configurare il modello Code First per utilizzare le stored procedure per alcune o tutte le entità nel modello.</span><span class="sxs-lookup"><span data-stu-id="7d690-106">Starting in EF6 you can configure your Code First model to use stored procedures for some or all entities in your model.</span></span>  

## <a name="basic-entity-mapping"></a><span data-ttu-id="7d690-107">Mapping di entità di base</span><span class="sxs-lookup"><span data-stu-id="7d690-107">Basic Entity Mapping</span></span>  

<span data-ttu-id="7d690-108">È possibile acconsentire esplicitamente tramite le stored procedure per l'inserimento, aggiornare ed eliminare tramite l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="7d690-108">You can opt into using stored procedures for insert, update and delete using the Fluent API.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

<span data-ttu-id="7d690-109">Questa operazione causerà Code First per usare alcune convenzioni per compilare la struttura prevista delle stored procedure nel database.</span><span class="sxs-lookup"><span data-stu-id="7d690-109">Doing this will cause Code First to use some conventions to build the expected shape of the stored procedures in the database.</span></span>  

- <span data-ttu-id="7d690-110">Tre stored procedure denominate  **\<type_name\>Inserisci**,  **\<type_name\>Aggiorna** e  **\<type_ nome\>Elimina** (ad esempio, Blog_Insert Blog_Update e Blog_Delete).</span><span class="sxs-lookup"><span data-stu-id="7d690-110">Three stored procedures named **\<type_name\>_Insert**, **\<type_name\>_Update** and **\<type_name\>_Delete** (for example, Blog_Insert, Blog_Update and Blog_Delete).</span></span>  
- <span data-ttu-id="7d690-111">I nomi dei parametri corrispondono ai nomi delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="7d690-111">Parameter names correspond to the property names.</span></span>  
  > [!NOTE]
  > <span data-ttu-id="7d690-112">Se si usa HasColumnName() o l'attributo di colonna per rinominare la colonna per una determinata proprietà questo nome viene usato per i parametri anziché il nome della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7d690-112">If you use HasColumnName() or the Column attribute to rename the column for a given property then this name is used for parameters instead of the property name.</span></span>  
- <span data-ttu-id="7d690-113">**Stored procedure insert** disporrà di un parametro per ogni proprietà, ad eccezione di quelli contrassegnati come archivio generato (identity o calcolate).</span><span class="sxs-lookup"><span data-stu-id="7d690-113">**The insert stored procedure** will have a parameter for every property, except for those marked as store generated (identity or computed).</span></span> <span data-ttu-id="7d690-114">La stored procedure deve restituire un set di risultati con una colonna per ogni proprietà dell'archivio generato.</span><span class="sxs-lookup"><span data-stu-id="7d690-114">The stored procedure should return a result set with a column for each store generated property.</span></span>  
- <span data-ttu-id="7d690-115">**Stored procedure di aggiornamento** disporrà di un parametro per ogni proprietà, ad eccezione di quelli contrassegnati con un modello di archivio generato di 'Computed'.</span><span class="sxs-lookup"><span data-stu-id="7d690-115">**The update stored procedure** will have a parameter for every property, except for those marked with a store generated pattern of 'Computed'.</span></span> <span data-ttu-id="7d690-116">Alcuni token di concorrenza che richiedono un parametro per il valore originale, vedere la *token di concorrenza* sezione per informazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="7d690-116">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span> <span data-ttu-id="7d690-117">La stored procedure deve restituire un set di risultati con una colonna per ogni proprietà calcolata.</span><span class="sxs-lookup"><span data-stu-id="7d690-117">The stored procedure should return a result set with a column for each computed property.</span></span>  
- <span data-ttu-id="7d690-118">**L'eliminazione di stored procedure** deve contenere un parametro per il valore della chiave di entità (o più parametri se l'entità dispone di una chiave composta).</span><span class="sxs-lookup"><span data-stu-id="7d690-118">**The delete stored procedure** should have a parameter for the key value of the entity (or multiple parameters if the entity has a composite key).</span></span> <span data-ttu-id="7d690-119">Inoltre, la procedura di eliminazione deve avere anche parametri per chiavi esterne associazione indipendente nella tabella di destinazione (relazioni che non dispongono di proprietà chiave esterna corrispondenti dichiarate nell'entità).</span><span class="sxs-lookup"><span data-stu-id="7d690-119">Additionally, the delete procedure should also have parameters for any independent association foreign keys on the target table (relationships that do not have corresponding foreign key properties declared in the entity).</span></span> <span data-ttu-id="7d690-120">Alcuni token di concorrenza che richiedono un parametro per il valore originale, vedere la *token di concorrenza* sezione per informazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="7d690-120">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span>  

<span data-ttu-id="7d690-121">Usando la classe seguente come esempio:</span><span class="sxs-lookup"><span data-stu-id="7d690-121">Using the following class as an example:</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

<span data-ttu-id="7d690-122">Il valore predefinito sarebbero stored procedure:</span><span class="sxs-lookup"><span data-stu-id="7d690-122">The default stored procedures would be:</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="7d690-123">Viene sottoposto a override le impostazioni predefinite</span><span class="sxs-lookup"><span data-stu-id="7d690-123">Overriding the Defaults</span></span>  

<span data-ttu-id="7d690-124">È possibile eseguire l'override di parte o tutto ciò che è stato configurato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="7d690-124">You can override part or all of what was configured by default.</span></span>  

<span data-ttu-id="7d690-125">È possibile modificare il nome di uno o più stored procedure.</span><span class="sxs-lookup"><span data-stu-id="7d690-125">You can change the name of one or more stored procedures.</span></span> <span data-ttu-id="7d690-126">Questo esempio Rinomina la stored procedure update solo.</span><span class="sxs-lookup"><span data-stu-id="7d690-126">This example renames the update stored procedure only.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

<span data-ttu-id="7d690-127">Questo esempio Rinomina tutte le tre stored procedure.</span><span class="sxs-lookup"><span data-stu-id="7d690-127">This example renames all three stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

<span data-ttu-id="7d690-128">In questi esempi le chiamate vengono concatenate tra loro, ma è anche possibile usare la sintassi di blocco di espressione lambda.</span><span class="sxs-lookup"><span data-stu-id="7d690-128">In these examples the calls are chained together, but you can also use lambda block syntax.</span></span>  

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

<span data-ttu-id="7d690-129">Questo esempio Rinomina il parametro per la proprietà BlogId in stored procedure update.</span><span class="sxs-lookup"><span data-stu-id="7d690-129">This example renames the parameter for the BlogId property on the update stored procedure.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

<span data-ttu-id="7d690-130">Queste chiamate sono tutti concatenabile e componibile.</span><span class="sxs-lookup"><span data-stu-id="7d690-130">These calls are all chainable and composable.</span></span> <span data-ttu-id="7d690-131">Ecco un esempio che consente di ridenominare tutte le tre stored procedure e i relativi parametri.</span><span class="sxs-lookup"><span data-stu-id="7d690-131">Here is an example that renames all three stored procedures and their parameters.</span></span>  

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

<span data-ttu-id="7d690-132">È anche possibile modificare il nome delle colonne nel set di risultati che contiene i valori del database generato.</span><span class="sxs-lookup"><span data-stu-id="7d690-132">You can also change the name of the columns in the result set that contains database generated values.</span></span>  

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

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a><span data-ttu-id="7d690-133">Relazioni senza una chiave esterna nella classe (associazioni indipendenti)</span><span class="sxs-lookup"><span data-stu-id="7d690-133">Relationships Without a Foreign Key in the Class (Independent Associations)</span></span>  

<span data-ttu-id="7d690-134">Quando una proprietà di chiave esterna viene inclusa nella definizione della classe, il parametro corrispondente può essere rinominato in esattamente come qualsiasi altra proprietà.</span><span class="sxs-lookup"><span data-stu-id="7d690-134">When a foreign key property is included in the class definition, the corresponding parameter can be renamed in the same way as any other property.</span></span> <span data-ttu-id="7d690-135">Se non esiste una relazione senza una proprietà di chiave esterna nella classe, il nome di parametro predefinito è  **\<navigation_property_name\>_\<primary_key_name\>**.</span><span class="sxs-lookup"><span data-stu-id="7d690-135">When a relationship exists without a foreign key property in the class, the default parameter name is **\<navigation_property_name\>_\<primary_key_name\>**.</span></span>  

<span data-ttu-id="7d690-136">Ad esempio, le definizioni di classe seguente comporta un parametro Blog_BlogId viene previsto nelle stored procedure per inserire e aggiornare gli invii.</span><span class="sxs-lookup"><span data-stu-id="7d690-136">For example, the following class definitions would result in a Blog_BlogId parameter being expected in the stored procedures to insert and update Posts.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="7d690-137">Viene sottoposto a override le impostazioni predefinite</span><span class="sxs-lookup"><span data-stu-id="7d690-137">Overriding the Defaults</span></span>  

<span data-ttu-id="7d690-138">È possibile modificare i parametri per le chiavi esterne che non sono inclusi nella classe specificando il percorso per la proprietà di chiave primaria per il metodo con parametri.</span><span class="sxs-lookup"><span data-stu-id="7d690-138">You can change parameters for foreign keys that are not included in the class by supplying the path to the primary key property to the Parameter method.</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

<span data-ttu-id="7d690-139">Se non si dispone di una proprietà di navigazione nell'entità dipendente (ad es.</span><span class="sxs-lookup"><span data-stu-id="7d690-139">If you don’t have a navigation property on the dependent entity (i.e</span></span> <span data-ttu-id="7d690-140">Nessuna proprietà Post.Blog) è possibile usare il metodo di associazione per identificare l'altra estremità della relazione e quindi configurare i parametri che corrispondono a ogni le proprietà di chiave.</span><span class="sxs-lookup"><span data-stu-id="7d690-140">no Post.Blog property) then you can use the Association method to identify the other end of the relationship and then configure the parameters that correspond to each of the key property(s).</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a><span data-ttu-id="7d690-141">Token di concorrenza</span><span class="sxs-lookup"><span data-stu-id="7d690-141">Concurrency Tokens</span></span>  

<span data-ttu-id="7d690-142">Aggiornamento ed eliminazione di stored procedure potrebbe essere necessario anche gestire la concorrenza:</span><span class="sxs-lookup"><span data-stu-id="7d690-142">Update and delete stored procedures may also need to deal with concurrency:</span></span>  

- <span data-ttu-id="7d690-143">Se l'entità contiene i token di concorrenza, la stored procedure può facoltativamente includere un parametro di output che restituisce il numero di righe aggiornate/eliminate (righe interessate).</span><span class="sxs-lookup"><span data-stu-id="7d690-143">If the entity contains concurrency tokens, the stored procedure can optionally have an output parameter that returns the number of rows updated/deleted (rows affected).</span></span> <span data-ttu-id="7d690-144">Tale parametro deve essere configurato utilizzando il metodo RowsAffectedParameter.</span><span class="sxs-lookup"><span data-stu-id="7d690-144">Such a parameter must be configured using the RowsAffectedParameter method.</span></span>  
<span data-ttu-id="7d690-145">Per impostazione predefinita Entity Framework Usa il valore restituito da ExecuteNonQuery per determinare il numero di righe interessato.</span><span class="sxs-lookup"><span data-stu-id="7d690-145">By default EF uses the return value from ExecuteNonQuery to determine how many rows were affected.</span></span> <span data-ttu-id="7d690-146">Specificare un parametro di output delle righe interessate è utile se si esegue qualsiasi logica nella stored procedure che comporterebbe il valore restituito di ExecuteNonQuery risulti errato (dal punto di vista di Entity Framework) alla fine dell'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="7d690-146">Specifying a rows affected output parameter is useful if you perform any logic in your sproc that would result in the return value of ExecuteNonQuery being incorrect (from EF's perspective) at the end of execution.</span></span>  
- <span data-ttu-id="7d690-147">Per ogni concorrenza di token vi sarà un parametro denominato  **\<property_name\>_Original** (ad esempio, Timestamp_Original).</span><span class="sxs-lookup"><span data-stu-id="7d690-147">For each concurrency token there will be a parameter named **\<property_name\>_Original** (for example, Timestamp_Original ).</span></span> <span data-ttu-id="7d690-148">Questo verrà passato il valore originale di questa proprietà: il valore quando sottoposto a query dal database.</span><span class="sxs-lookup"><span data-stu-id="7d690-148">This will be passed the original value of this property – the value when queried from the database.</span></span>  
    - <span data-ttu-id="7d690-149">I token di concorrenza che vengono calcolati dal database, ad esempio i timestamp: disporrà solo un parametro di valore originale.</span><span class="sxs-lookup"><span data-stu-id="7d690-149">Concurrency tokens that are computed by the database – such as timestamps – will only have an original value parameter.</span></span>  
    - <span data-ttu-id="7d690-150">Le proprietà calcolate non sono impostate come token di concorrenza avrà inoltre un parametro per il nuovo valore nella procedura di aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="7d690-150">Non-computed properties that are set as concurrency tokens will also have a parameter for the new value in the update procedure.</span></span> <span data-ttu-id="7d690-151">Usa le convenzioni di denominazione già illustrate per i nuovi valori.</span><span class="sxs-lookup"><span data-stu-id="7d690-151">This uses the naming conventions already discussed for new values.</span></span> <span data-ttu-id="7d690-152">Un esempio di un token di questo tipo potrebbe essere utilizzando l'URL del Blog come un token di concorrenza, il nuovo valore è obbligatorio perché ciò può essere aggiornata a un nuovo valore da codice (a differenza di un token di Timestamp che viene aggiornato solo dal database).</span><span class="sxs-lookup"><span data-stu-id="7d690-152">An example of such a token would be using a Blog's URL as a concurrency token, the new value is required because this can be updated to a new value by your code (unlike a Timestamp token which is only updated by the database).</span></span>  

<span data-ttu-id="7d690-153">Si tratta di un esempio di classe e aggiornare stored procedure con un token di concorrenza di timestamp.</span><span class="sxs-lookup"><span data-stu-id="7d690-153">This is an example class and update stored procedure with a timestamp concurrency token.</span></span>  

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

<span data-ttu-id="7d690-154">Ecco un esempio di classe e aggiornare stored procedure con token di concorrenza non calcolata.</span><span class="sxs-lookup"><span data-stu-id="7d690-154">Here is an example class and update stored procedure with non-computed concurrency token.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="7d690-155">Viene sottoposto a override le impostazioni predefinite</span><span class="sxs-lookup"><span data-stu-id="7d690-155">Overriding the Defaults</span></span>  

<span data-ttu-id="7d690-156">È facoltativamente possibile introdurre un parametro delle righe interessate.</span><span class="sxs-lookup"><span data-stu-id="7d690-156">You can optionally introduce a rows affected parameter.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

<span data-ttu-id="7d690-157">Per i token di concorrenza di database calcolato, in cui viene passato solo il valore originale: è possibile utilizzare solo il parametro standard ridenominazione meccanismo per rinominare il parametro per il valore originale.</span><span class="sxs-lookup"><span data-stu-id="7d690-157">For database computed concurrency tokens – where only the original value is passed – you can just use the standard parameter renaming mechanism to rename the parameter for the original value.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

<span data-ttu-id="7d690-158">Per i token di concorrenza non calcolata, in cui sia il valore originale e quello nuovo viene passato – è possibile usare un overload di parametro che consente di specificare un nome per ogni parametro.</span><span class="sxs-lookup"><span data-stu-id="7d690-158">For non-computed concurrency tokens – where both the original and new value are passed – you can use an overload of Parameter that allows you to supply a name for each parameter.</span></span>  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a><span data-ttu-id="7d690-159">Relazioni many-to-many</span><span class="sxs-lookup"><span data-stu-id="7d690-159">Many to Many Relationships</span></span>  

<span data-ttu-id="7d690-160">Le classi seguenti si userà come esempio in questa sezione.</span><span class="sxs-lookup"><span data-stu-id="7d690-160">We’ll use the following classes as an example in this section.</span></span>  

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

<span data-ttu-id="7d690-161">Molti-a-molti relazioni possono essere mappate alle stored procedure con la sintassi seguente.</span><span class="sxs-lookup"><span data-stu-id="7d690-161">Many to many relationships can be mapped to stored procedures with the following syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

<span data-ttu-id="7d690-162">Se viene fornita alcuna altra configurazione la forma di stored procedure seguente viene utilizzata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="7d690-162">If no other configuration is supplied then the following stored procedure shape is used by default.</span></span>  

- <span data-ttu-id="7d690-163">Due stored procedure denominate  **\<type_one\>\<type_two\>Inserisci** e  **\<type_one\>\<type_two \>Elimina** (ad esempio, PostTag_Insert e PostTag_Delete).</span><span class="sxs-lookup"><span data-stu-id="7d690-163">Two stored procedures named **\<type_one\>\<type_two\>_Insert** and **\<type_one\>\<type_two\>_Delete** (for example, PostTag_Insert and PostTag_Delete).</span></span>  
- <span data-ttu-id="7d690-164">I parametri saranno i valori di chiave per ogni tipo.</span><span class="sxs-lookup"><span data-stu-id="7d690-164">The parameters will be the key value(s) for each type.</span></span> <span data-ttu-id="7d690-165">Il nome di ogni parametro in corso **\<type_name\>_\<property_name\>** (ad esempio, Post_PostId e Tag_TagId).</span><span class="sxs-lookup"><span data-stu-id="7d690-165">The name of each parameter being **\<type_name\>_\<property_name\>** (for example, Post_PostId and Tag_TagId).</span></span>

<span data-ttu-id="7d690-166">Di seguito sono esempio inseriscono e aggiornano le stored procedure.</span><span class="sxs-lookup"><span data-stu-id="7d690-166">Here are example insert and update stored procedures.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="7d690-167">Viene sottoposto a override le impostazioni predefinite</span><span class="sxs-lookup"><span data-stu-id="7d690-167">Overriding the Defaults</span></span>  

<span data-ttu-id="7d690-168">Il procedura e nomi di parametro possono essere configurati in modo analogo alle procedure entità archiviata.</span><span class="sxs-lookup"><span data-stu-id="7d690-168">The procedure and parameter names can be configured in a similar way to entity stored procedures.</span></span>  

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
