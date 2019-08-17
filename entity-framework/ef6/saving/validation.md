---
title: Convalida-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 457af0c32f0fe4804dbfe6e348664efb1af517c9
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565372"
---
# <a name="data-validation"></a>Convalida dei dati
> [!NOTE]
> **EF 4.1 solo e versioni successive** : le funzionalità, le API e così via descritte in questa pagina sono state introdotte in Entity Framework 4,1. Se si usa una versione precedente, alcune o tutte le informazioni non sono valide

Il contenuto di questa pagina è stato adattato da un articolo scritto in origine da Julie[http://thedatafarm.com](http://thedatafarm.com)Lerman ().

Entity Framework offre un'ampia gamma di funzionalità di convalida che consentono di passare a un'interfaccia utente per la convalida sul lato client o per la convalida sul lato server. Quando si usa Code First, è possibile specificare convalide usando un'annotazione o configurazioni API Fluent. Le convalide aggiuntive, e più complesse, possono essere specificate nel codice e funzionano indipendentemente dal fatto che il modello sia in primo luogo, prima del modello o del database.

## <a name="the-model"></a>Il modello

Illustrerò le convalide con una semplice coppia di classi: Blog e post.

``` csharp
public class Blog
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string BloggerName { get; set; }
    public DateTime DateCreated { get; set; }
    public virtual ICollection<Post> Posts { get; set; }
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

Code First utilizza le `System.ComponentModel.DataAnnotations` annotazioni dell'assembly come mezzo per la configurazione delle classi code first. Tra queste annotazioni sono disponibili regole quali `Required`, `MaxLength` e `MinLength`. Alcune applicazioni client .NET riconoscono anche queste annotazioni, ad esempio ASP.NET MVC. Con queste annotazioni è possibile ottenere la convalida lato client e lato server. Ad esempio, è possibile forzare la proprietà del titolo del Blog come una proprietà obbligatoria.

``` csharp
[Required]
public string Title { get; set; }
```

Senza modifiche al codice o al markup aggiuntive nell'applicazione, un'applicazione MVC esistente eseguirà la convalida lato client, anche creando dinamicamente un messaggio usando i nomi di proprietà e annotazione.

![Figura 1](~/ef6/media/figure01.png)

Nel metodo di postback di questa visualizzazione di creazione, Entity Framework viene usato per salvare il nuovo blog nel database, ma la convalida lato client di MVC viene attivata prima che l'applicazione raggiunga tale codice.

La convalida lato client non è tuttavia a prova di Bullet. Gli utenti possono indurre le funzionalità del browser o peggio ancora, un pirata informatico può usare alcuni espedienti per evitare le convalide dell'interfaccia utente. Tuttavia Entity Framework rileverà anche `Required` l'annotazione e la convaliderà.

Un modo semplice per verificare questo problema è disabilitare la funzionalità di convalida lato client di MVC. Questa operazione può essere eseguita nel file Web. config dell'applicazione MVC. La sezione appSettings contiene una chiave per ClientValidationEnabled. Se si imposta questa chiave su false, l'interfaccia utente non potrà eseguire convalide.

``` xml
<appSettings>
    <add key="ClientValidationEnabled"value="false"/>
    ...
</appSettings>
```

Anche se la convalida lato client è disabilitata, si otterrà la stessa risposta nell'applicazione. Il messaggio di errore "il campo titolo è obbligatorio" verrà visualizzato come prima. Eccetto ora, sarà il risultato della convalida sul lato server. Entity Framework eseguirà la convalida sull' `Required` annotazione (prima che venga creato un `INSERT` comando da inviare al database) e restituisca l'errore a MVC che visualizzerà il messaggio.

## <a name="fluent-api"></a>API Fluent

È possibile usare l'API Fluent Code First anziché le annotazioni per ottenere lo stesso lato client & convalida lato server. Invece di usare `Required`, vi mostrerò questa operazione usando una convalida MaxLength.

Le configurazioni API Fluent vengono applicate come Code First creando il modello dalle classi. È possibile inserire le configurazioni eseguendo l'override del metodo OnModelCreating della classe DbContext. Di seguito è riportato un esempio di configurazione che specifica che la proprietà BloggerName non può contenere più di 10 caratteri.

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

Gli errori di convalida generati in base alle configurazioni dell'API Fluent non raggiungeranno automaticamente l'interfaccia utente, ma è possibile acquisirli nel codice e quindi rispondere di conseguenza.

Di seguito è riportato un codice di errore di gestione delle eccezioni nella classe BlogController dell'applicazione che acquisisce l'errore di convalida quando Entity Framework tenta di salvare un Blog con un BloggerName che supera il limite massimo di 10 caratteri.

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
    catch (DbEntityValidationException ex)
    {
        var error = ex.EntityValidationErrors.First().ValidationErrors.First();
        this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
        return View();
    }
}
```

La convalida non viene restituita automaticamente nella visualizzazione ed è per questo motivo che viene usato il `ModelState.AddModelError` codice aggiuntivo che usa. In questo modo si garantisce che i dettagli dell'errore lo rendano nella visualizzazione che `ValidationMessageFor` utilizzerà quindi il HtmlHelper per visualizzare l'errore.

``` csharp
@Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a>IValidatableObject

`IValidatableObject`è un'interfaccia che risiede in `System.ComponentModel.DataAnnotations`. Sebbene non faccia parte dell'API Entity Framework, è comunque possibile usarla per la convalida lato server nelle classi Entity Framework. `IValidatableObject`fornisce un `Validate` metodo che Entity Framework chiamerà durante SaveChanges oppure è possibile chiamarsi in qualsiasi momento in cui si desidera convalidare le classi.

Configurazioni come `Required` ed `MaxLength` eseguono la convalida su un singolo campo. `Validate` Nel metodo è possibile avere una logica ancora più complessa, ad esempio confrontando due campi.

Nell' `Blog` esempio seguente, la classe è stata estesa per implementare `IValidatableObject` e quindi `Title` fornire una regola in base alla quale `BloggerName` e non possono corrispondere.

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
            yield return new ValidationResult(
                "Blog Title cannot match Blogger Name",
                new[] { nameof(Title), nameof(BloggerName) });
        }
    }
}
```

Il `ValidationResult` costruttore accetta un `string` oggetto che rappresenta il messaggio di errore e una `string`matrice di oggetti che rappresentano i nomi dei membri associati alla convalida. Poiché questa convalida controlla sia `Title` `BloggerName`che e, vengono restituiti entrambi i nomi di proprietà.

A differenza della convalida fornita dall'API Fluent, questo risultato di convalida verrà riconosciuto dalla vista e il gestore di eccezioni usato in precedenza per aggiungere l'errore `ModelState` non è necessario. Poiché i nomi delle proprietà sono stati impostati `ValidationResult`in, le HtmlHelper di MVC visualizzano il messaggio di errore per entrambe le proprietà.

![Figura 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a>DbContext.ValidateEntity

`DbContext`dispone di un metodo sottoponibile a override chiamato `ValidateEntity`. Quando si chiama `SaveChanges`, Entity Framework chiamerà questo metodo per ogni entità nella cache il cui stato non `Unchanged`è. È possibile inserire la logica di convalida direttamente qui o usare questo metodo per chiamare, ad esempio, il `Blog.Validate` metodo aggiunto nella sezione precedente.

Di seguito è riportato un esempio `ValidateEntity` di override che convalida i `Post`nuovi per assicurarsi che il titolo post non sia già stato utilizzato. Verifica prima di tutto se l'entità è un post e viene aggiunto il relativo stato. In tal caso, viene eseguita una ricerca nel database per verificare se è già presente un post con lo stesso titolo. Se è già presente un post esistente, viene creato un `DbEntityValidationResult` nuovo oggetto.

`DbEntityValidationResult`ospita un `DbEntityEntry` oggetto e `ICollection<DbValidationErrors>` un oggetto per una singola entità. All'inizio di questo metodo, viene creata `DbEntityValidationResult` un'istanza di un oggetto e quindi tutti gli errori individuati vengono aggiunti `ValidationErrors` alla relativa raccolta.

``` csharp
protected override DbEntityValidationResult ValidateEntity (
    System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
    IDictionary<object, object> items)
{
    var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());

    if (entityEntry.Entity is Post post && entityEntry.State == EntityState.Added)
    {
        // Check for uniqueness of post title
        if (Posts.Where(p => p.Title == post.Title).Any())
        {
            result.ValidationErrors.Add(
                    new System.Data.Entity.Validation.DbValidationError(
                        nameof(Title),
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

## <a name="explicitly-triggering-validation"></a>Attivazione esplicita della convalida

Una chiamata a `SaveChanges` attiva tutte le convalide descritte in questo articolo. Ma non è necessario basarsi su `SaveChanges`. È possibile che si preferisca convalidare un'altra posizione nell'applicazione.

`DbContext.GetValidationErrors`attiverà tutte le convalide, quelle definite dalle annotazioni o l'API Fluent, la convalida creata in `IValidatableObject` (ad esempio, `Blog.Validate`) e le `DbContext.ValidateEntity` convalide eseguite nel metodo.

Il codice `GetValidationErrors` seguente chiamerà sull'istanza corrente di un oggetto `DbContext`. `ValidationErrors`sono raggruppati in base al tipo `DbEntityValidationResult`di entità in. Il codice scorre prima di tutto l' `DbEntityValidationResult`oggetto restituito dal metodo e quindi attraverso ogni `DbValidationError` interno.

``` csharp
foreach (var validationResult in db.GetValidationErrors())
{
    foreach (var error in validationResult.ValidationErrors)
    {
        Debug.WriteLine(
            "Entity Property: {0}, Error {1}",
            error.PropertyName,
            error.ErrorMessage);
    }
}
```

## <a name="other-considerations-when-using-validation"></a>Altre considerazioni relative all'uso della convalida

Di seguito sono riportati alcuni altri punti da considerare quando si usa la convalida Entity Framework:

- Il caricamento lazy è disabilitato durante la convalida
- EF convaliderà le annotazioni dei dati in proprietà non mappate (proprietà di cui non è stato eseguito il mapping a una colonna nel database)
- La convalida viene eseguita dopo che le modifiche `SaveChanges`vengono rilevate durante. Se si apportano modifiche durante la convalida, è responsabilità dell'utente inviare una notifica a change tracker
- `DbUnexpectedValidationException`viene generata se si verificano errori durante la convalida
- I facet che Entity Framework include nel modello (lunghezza massima, obbligatoria e così via) provocheranno la convalida, anche se non sono presenti annotazioni di dati nelle classi e/o per la creazione del modello è stata usata la finestra di progettazione EF.
- Regole di precedenza:
  - Le chiamate API Fluent eseguono l'override delle annotazioni dei dati corrispondenti
- Ordine di esecuzione:
  - La convalida delle proprietà si verifica prima della convalida del tipo
  - La convalida del tipo si verifica solo se la convalida della proprietà ha esito positivo
- Se una proprietà è complessa, la relativa convalida includerà anche:
  - Convalida a livello di proprietà per le proprietà del tipo complesso
  - Convalida a livello di tipo per il tipo complesso `IValidatableObject` , inclusa la convalida sul tipo complesso

## <a name="summary"></a>Riepilogo

L'API di convalida in Entity Framework viene riprodotta molto bene con la convalida lato client in MVC, ma non è necessario basarsi sulla convalida lato client. Entity Framework si occuperà della convalida sul lato server per le annotazioni o le configurazioni applicate con l'API Code First Fluent.

È stato inoltre illustrato un numero di punti di estensibilità per personalizzare il comportamento se si `IValidatableObject` usa l'interfaccia o si `DbContext.ValidateEntity` tocca il metodo. Questi ultimi due metodi di convalida sono disponibili tramite `DbContext`, indipendentemente dal fatto che si utilizzi il flusso di lavoro Code First, Model First o database First per descrivere il modello concettuale.
