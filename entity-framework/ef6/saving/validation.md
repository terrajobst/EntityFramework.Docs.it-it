---
title: Convalida - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: eec834888e2e3efaadc8acf9d4f64307f394ea4a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994445"
---
# <a name="data-validation"></a>Convalida dei dati
> [!NOTE]
> **EF4.1 e versioni successive solo** -le funzionalità, le API, e così via illustrati in questa pagina sono stati introdotti in Entity Framework 4.1. Se si usa una versione precedente, alcune o tutte le informazioni non è applicabile

Il contenuto in questa pagina è stato adattato dalla e l'articolo è stato scritto originariamente da Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).

Entity Framework fornisce una vasta gamma di funzionalità di convalida che può alimentare tramite a un'interfaccia utente per la convalida lato client o essere utilizzato per la convalida sul lato server. Quando si usa codice prima di tutto, è possibile specificare le convalide mediante annotazione o configurazioni di API fluent. Le convalide aggiuntive e più complesso, può essere specificate nel codice e funzionerà se il modello vanta oltre dal codice in primo luogo, a partire dal modello o prima di tutto il database.

## <a name="the-model"></a>Il modello

Illustrerò le convalide con una semplice coppia di classi: post di Blog e Post.

``` csharp
    public class Blog
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public string BloggerName { get; set; }
          public DateTime DateCreated { get; set; }
          public virtual ICollection<Post> Posts { get; set; }
          }
      }

      public class Post
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public DateTime DateCreated { get; set; }
          public string Content { get; set; }
          public int BlogId { get; set; }
          public ICollection<Comment> Comments { get; set; }
      }
```
## <a name="data-annotations"></a>Annotazioni dei dati

Codice Usa prima di tutto le annotazioni dall'assembly System.ComponentModel.DataAnnotations come un metodo di configurazione di classi di code first. Tra queste annotazioni sono quelli che forniscono regole, ad esempio obbligatori, MinLength o MaxLength. Un numero di applicazioni client .NET supporta anche queste annotazioni, ad esempio, ASP.NET MVC. È possibile ottenere entrambi server e lato la convalida lato client con queste annotazioni. Ad esempio, è possibile forzare la proprietà del titolo del Blog su una proprietà obbligatoria.

``` csharp
    [Required]
    public string Title { get; set; }
```

Senza codice aggiuntivo o modifiche di markup dell'applicazione, un'applicazione MVC esistente eseguirà la convalida lato client, anche in modo dinamico la creazione di un messaggio utilizzando i nomi di proprietà e l'annotazione.

![figure01](~/ef6/media/figure01.png)

Nel post di eseguire il metodo di questa visualizzazione di creazione, Entity Framework consente di salvare il nuovo post di blog nel database, ma la convalida lato client di MVC viene attivata prima che l'applicazione raggiunga tale codice.

La convalida lato client non è tuttavia valide. Gli utenti possono influire sulle funzionalità del browser o peggio ancora, un hacker potrebbe usare avvengono per evitare le convalide dell'interfaccia utente. Ma anche riconoscerà l'annotazione necessaria e convalidarlo in Entity Framework.

Un modo semplice per eseguire il test è per disabilitare la funzionalità di convalida lato client di MVC. È possibile eseguire questa operazione nel file Web. config dell'applicazione MVC. La sezione appSettings dispone di una chiave per ClientValidationEnabled. Impostazione di questa chiave su false, l'interfaccia utente, potrà eseguire convalide.

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

Anche con la convalida lato client è disabilitata, si otterrà la stessa risposta nell'applicazione. Come verrà visualizzato il messaggio di errore "il campo del titolo è obbligatorio". La differenza sarà ora un risultato di convalida sul lato server. Entity Framework eseguirà la convalida sull'annotazione richiesto (prima anche esserci di compilazione e il comando di inserimento per inviare al database) e restituire l'errore per MVC che verrà visualizzato il messaggio.

## <a name="fluent-api"></a>API Fluent

È possibile usare codice di convalida sul lato di API fluent del primo anziché le annotazioni per ottenere lo stesso client side & server. Invece di utilizzo richiesto, vi mostrerò questa operazione utilizzando una convalida di MaxLength.

Applicazione delle configurazioni di API Fluent come codice prima di tutto è la compilazione del modello dalle classi. È possibile inserire le configurazioni di eseguendo l'override del metodo OnModelCreating della classe DbContext. Ecco una configurazione specifica che la proprietà BloggerName può essere non più di 10 caratteri.

``` csharp
    public class BlogContext : DbContext
      {
          public DbSet<Blog> Blogs { get; set; }
          public DbSet<Post> Posts { get; set; }
          public DbSet<Comment> Comments { get; set; }

          protected override void OnModelCreating(DbModelBuilder modelBuilder)
          {
              modelBuilder.Entity<Blog>().Property(p => p.BloggerName).HasMaxLength(10);
          }
        }
```

Gli errori di convalida generati sulla base delle configurazioni API Fluent non reach dell'interfaccia utente, ma è può acquisisce automaticamente nel codice e quindi rispondere di conseguenza.

Alcuni gestione Ecco delle eccezioni codice di errore nella classe BlogController dell'applicazione che consente di acquisire tale errore di convalida quando Entity Framework tenta di salvare un blog con una BloggerName che supera il limite massimo di 10 caratteri.

``` csharp
    [HttpPost]
    public ActionResult Edit(int id, Blog blog)
    {
        try
        {
            db.Entry(blog).State = EntityState.Modified;
            db.SaveChanges();
            return RedirectToAction("Index");
        }
        catch(DbEntityValidationException ex)
        {
            var error = ex.EntityValidationErrors.First().ValidationErrors.First();
            this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
            return View();
        }
    }
```

La convalida non vengono passata automaticamente in visualizzazione per questo motivo il codice aggiuntivo che usa AddModelError utilizzato. Ciò garantisce che i dettagli dell'errore rendono alla vista che userà quindi il ValidationMessageFor Htmlhelper per visualizzare l'errore.

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a>IValidatableObject

IValidatableObject è un'interfaccia che si trova in System.ComponentModel.DataAnnotations. Sebbene non sia parte dell'API Entity Framework, è possibile comunque sfruttare, per la convalida sul lato server nelle classi di Entity Framework. IValidatableObject fornisce un metodo di convalida che Entity Framework chiamerà durante SaveChanges oppure è possibile chiamare manualmente ogni volta che si desidera convalidare le classi.

Configurazioni necessarie, ad esempio e MaxLength eseguono validaton su un singolo campo. Nel metodo di convalida è possibile avere anche una logica più complessa, ad esempio, il confronto di due campi.

Nell'esempio seguente, la classe Blog è stato esteso per implementare IValidatableObject e quindi specificare una regola che il titolo e BloggerName non può essere uguale.

``` csharp
    public class Blog : IValidatableObject
     {
         public int Id { get; set; }
         [Required]
         public string Title { get; set; }
         public string BloggerName { get; set; }
         public DateTime DateCreated { get; set; }
         public virtual ICollection<Post> Posts { get; set; }

         public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
         {
             if (Title == BloggerName)
             {
                 yield return new ValidationResult
                  ("Blog Title cannot match Blogger Name", new[] { "Title", “BloggerName” });
             }
         }
     }
```

Il costruttore ValidationResult accetta una stringa che rappresenta il messaggio di errore e una matrice di stringhe che rappresentano i nomi dei membri che sono associati con la convalida. Poiché questa convalida controlla il titolo e il BloggerName, vengono restituiti entrambi i nomi delle proprietà.

A differenza di convalida fornita dall'API Fluent, il risultato della convalida verrà riconosciuto dalla visualizzazione e il gestore di eccezioni che ho utilizzato in precedenza per aggiungere l'errore in ModelState non è necessario. Perché ValidationResult impostare entrambi i nomi delle proprietà, il HtmlHelpers MVC visualizzare il messaggio di errore per entrambe le proprietà.

![figure02](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a>DbContext.ValidateEntity

DbContext è un metodo sostituibile denominato ValidateEntity. Quando si chiama SaveChanges, Entity Framework chiamerà il metodo per ogni entità nella propria cache il cui stato non è invariato. È possibile inserire la logica di convalida direttamente in uso qui o anche questo metodo per chiamare, ad esempio, il metodo Blog.Validate aggiunto nella sezione precedente.

Di seguito è riportato un esempio di un override ValidateEntity che convalida ai nuovi post per assicurarsi che il titolo del post non è già stato utilizzato. Innanzitutto verifica se l'entità è una richiesta post e che sia stato aggiunto il relativo stato. Se è questo il caso, la ricerca nel database per vedere se è già presente un post con lo stesso titolo. Se è già presente un post esistente, viene creato un nuovo DbEntityValidationResult.

DbEntityValidationResult ospita un DbEntityEntry e un ICollection di DbValidationErrors per una singola entità. All'inizio di questo metodo, viene creata un'istanza di un DbEntityValidationResult e quindi gli eventuali errori che vengono individuati vengono aggiunti nella sua raccolta ValidationErrors.

``` csharp
    protected override DbEntityValidationResult ValidateEntity (
        System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
        IDictionary\<object, object> items)
    {
        var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());
        if (entityEntry.Entity is Post && entityEntry.State == EntityState.Added)
        {
            Post post = entityEntry.Entity as Post;
            //check for uniqueness of post title
            if (Posts.Where(p => p.Title == post.Title).Count() > 0)
            {
                result.ValidationErrors.Add(
                        new System.Data.Entity.Validation.DbValidationError("Title",
                        "Post title must be unique."));
            }
        }

        if (result.ValidationErrors.Count > 0)
        {
            return result;
        }
        else
        {
         return base.ValidateEntity(entityEntry, items);
        }
    }
```

## <a name="explicitly-triggering-validation"></a>Attivare in modo esplicito la convalida

Una chiamata a SaveChanges attiva tutte le convalide contenute in questo articolo. Ma non è necessario affidarsi a SaveChanges. È possibile anche convalidare in un' posizione nell'applicazione.

Attiva tutte le convalide, quelli definiti per le annotazioni o l'API Fluent, la convalida creata in IValidatableObject (ad esempio, Blog.Validate) e le convalide eseguite nel DbContext.ValidateEntity DbContext.GetValidationErrors metodo.

Il codice seguente chiama GetValidationErrors nell'istanza corrente di un oggetto DbContext. ValidationErrors sono raggruppate per tipo di entità in DbValidationRestuls. Il codice esegue l'iterazione prima di tutto tramite il DbValidationResults restituito dal metodo e quindi ogni ValidationError all'interno.

``` csharp
    foreach (var validationResults in db.GetValidationErrors())
        {
            foreach (var error in validationResults.ValidationErrors)
            {
                Debug.WriteLine(
                                  "Entity Property: {0}, Error {1}",
                                  error.PropertyName,
                                  error.ErrorMessage);
            }
        }
```

## <a name="other-considerations-when-using-validation"></a>Altre considerazioni relative all'uso della convalida

Ecco alcuni altri punti da considerare quando si usa la convalida di Entity Framework:

-   Il caricamento lazy è disabilitato durante la convalida.
-   Entity Framework convaliderà annotazioni dei dati in una proprietà non mappate (proprietà che non sono mappate a una colonna nel database).
-   Dopo che le modifiche vengono rilevate durante il metodo SaveChanges, viene eseguita la convalida. Se si apportano modifiche durante la convalida è responsabilità dell'utente per notificare il rilevamento delle modifiche.
-   DbUnexpectedValidationException viene generata se si verificano errori durante la convalida.
-   Facet di Entity Framework è incluso nel modello (lunghezza massima, obbligatoria, e così via) provocherà la convalida, anche se non sono presenti annotazioni dei dati nelle classi e/o è utilizzata la finestra di progettazione di Entity Framework per creare il modello.
-   Regole di precedenza:
    -   Chiamate API Fluent esegue l'override di annotazioni dei dati corrispondente
-   Ordine di esecuzione:
    -   Convalida delle proprietà si verifica prima della convalida di tipo
    -   Tipo di convalida si verifica solo se ha esito positivo della convalida della proprietà
-   Se una proprietà è complessa, la convalida si includerà anche:
    -   Convalida a livello di proprietà nelle proprietà del tipo complesso
    -   Tipo di convalida a livello del tipo complesso, compresa la convalida IValidatableObject del tipo complesso

## <a name="summary"></a>Riepilogo

La convalida API in Entity Framework funziona molto bene con la convalida lato client in MVC, ma non è necessario basarsi sulla convalida sul lato client. Entity Framework si occuperà della convalida sul lato server per DataAnnotations o le configurazioni che è stato applicato con l'API Office Fluent code first.

È stato anche illustrato un numero di punti di estendibilità per personalizzare il comportamento se si usa l'interfaccia IValidatableObject o si tocca DbContet.ValidateEntity nel metodo. E questi ultimi due metodi di convalida sono disponibili tramite l'oggetto DbContext, se si utilizza il flusso di lavoro Database First o Model First Code First per descrivere il modello concettuale.
