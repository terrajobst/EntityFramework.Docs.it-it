---
title: Convalida - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
caps.latest.revision: 3
ms.openlocfilehash: 758865255d7868337dc1d7801bd9ff77f0bb57a9
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122780"
---
# <a name="data-validation"></a><span data-ttu-id="e5367-102">Convalida dei dati</span><span class="sxs-lookup"><span data-stu-id="e5367-102">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="e5367-103">**EF4.1 e versioni successive solo** -le funzionalità, le API, e così via illustrati in questa pagina sono stati introdotti in Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="e5367-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="e5367-104">Se si usa una versione precedente, alcune o tutte le informazioni non è applicabile</span><span class="sxs-lookup"><span data-stu-id="e5367-104">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="e5367-105">Il contenuto in questa pagina è stato adattato dalla e l'articolo è stato scritto originariamente da Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span><span class="sxs-lookup"><span data-stu-id="e5367-105">The content on this page is adapted from and article originally written by Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span></span>

<span data-ttu-id="e5367-106">Entity Framework fornisce una vasta gamma di funzionalità di convalida che può alimentare tramite a un'interfaccia utente per la convalida lato client o essere utilizzato per la convalida sul lato server.</span><span class="sxs-lookup"><span data-stu-id="e5367-106">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="e5367-107">Quando si usa codice prima di tutto, è possibile specificare le convalide mediante annotazione o configurazioni di API fluent.</span><span class="sxs-lookup"><span data-stu-id="e5367-107">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="e5367-108">Le convalide aggiuntive e più complesso, può essere specificate nel codice e funzionerà se il modello vanta oltre dal codice in primo luogo, a partire dal modello o prima di tutto il database.</span><span class="sxs-lookup"><span data-stu-id="e5367-108">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="e5367-109">Il modello</span><span class="sxs-lookup"><span data-stu-id="e5367-109">The model</span></span>

<span data-ttu-id="e5367-110">Illustrerò le convalide con una semplice coppia di classi: post di Blog e Post.</span><span class="sxs-lookup"><span data-stu-id="e5367-110">I’ll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

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
## <a name="data-annotations"></a><span data-ttu-id="e5367-111">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="e5367-111">Data Annotations</span></span>

<span data-ttu-id="e5367-112">Codice Usa prima di tutto le annotazioni dall'assembly System.ComponentModel.DataAnnotations come un metodo di configurazione di classi di code first.</span><span class="sxs-lookup"><span data-stu-id="e5367-112">Code First uses annotations from the System.ComponentModel.DataAnnotations assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="e5367-113">Tra queste annotazioni sono quelli che forniscono regole, ad esempio obbligatori, MinLength o MaxLength.</span><span class="sxs-lookup"><span data-stu-id="e5367-113">Among these annotations are those which provide rules such as the Required, MaxLength and MinLength.</span></span> <span data-ttu-id="e5367-114">Un numero di applicazioni client .NET supporta anche queste annotazioni, ad esempio, ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e5367-114">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="e5367-115">È possibile ottenere entrambi server e lato la convalida lato client con queste annotazioni.</span><span class="sxs-lookup"><span data-stu-id="e5367-115">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="e5367-116">Ad esempio, è possibile forzare la proprietà del titolo del Blog su una proprietà obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="e5367-116">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="e5367-117">Senza codice aggiuntivo o modifiche di markup dell'applicazione, un'applicazione MVC esistente eseguirà la convalida lato client, anche in modo dinamico la creazione di un messaggio utilizzando i nomi di proprietà e l'annotazione.</span><span class="sxs-lookup"><span data-stu-id="e5367-117">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![figure01](~/ef6/media/figure01.png)

<span data-ttu-id="e5367-119">Nel post di eseguire il metodo di questa visualizzazione di creazione, Entity Framework consente di salvare il nuovo post di blog nel database, ma la convalida lato client di MVC viene attivata prima che l'applicazione raggiunga tale codice.</span><span class="sxs-lookup"><span data-stu-id="e5367-119">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC’s client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="e5367-120">La convalida lato client non è tuttavia valide.</span><span class="sxs-lookup"><span data-stu-id="e5367-120">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="e5367-121">Gli utenti possono influire sulle funzionalità del browser o peggio ancora, un hacker potrebbe usare avvengono per evitare le convalide dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="e5367-121">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="e5367-122">Ma anche riconoscerà l'annotazione necessaria e convalidarlo in Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e5367-122">But Entity Framework will also recognize the Required annotation and validate it.</span></span>

<span data-ttu-id="e5367-123">Un modo semplice per eseguire il test è per disabilitare la funzionalità di convalida lato client di MVC.</span><span class="sxs-lookup"><span data-stu-id="e5367-123">A simple way to test this is to disable MVC’s client-side validation feature.</span></span> <span data-ttu-id="e5367-124">È possibile eseguire questa operazione nel file Web. config dell'applicazione MVC.</span><span class="sxs-lookup"><span data-stu-id="e5367-124">You can do this in the MVC application’s web.config file.</span></span> <span data-ttu-id="e5367-125">La sezione appSettings dispone di una chiave per ClientValidationEnabled.</span><span class="sxs-lookup"><span data-stu-id="e5367-125">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="e5367-126">Impostazione di questa chiave su false, l'interfaccia utente, potrà eseguire convalide.</span><span class="sxs-lookup"><span data-stu-id="e5367-126">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

<span data-ttu-id="e5367-127">Anche con la convalida lato client è disabilitata, si otterrà la stessa risposta nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e5367-127">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="e5367-128">Come verrà visualizzato il messaggio di errore "il campo del titolo è obbligatorio".</span><span class="sxs-lookup"><span data-stu-id="e5367-128">The error message “The Title field is required” will be displayed as.</span></span> <span data-ttu-id="e5367-129">La differenza sarà ora un risultato di convalida sul lato server.</span><span class="sxs-lookup"><span data-stu-id="e5367-129">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="e5367-130">Entity Framework eseguirà la convalida sull'annotazione richiesto (prima anche esserci di compilazione e il comando di inserimento per inviare al database) e restituire l'errore per MVC che verrà visualizzato il messaggio.</span><span class="sxs-lookup"><span data-stu-id="e5367-130">Entity Framework will perform the validation on the Required annotation (before it even bothers to build and INSERT command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="e5367-131">API Fluent</span><span class="sxs-lookup"><span data-stu-id="e5367-131">Fluent API</span></span>

<span data-ttu-id="e5367-132">È possibile usare codice di convalida sul lato di API fluent del primo anziché le annotazioni per ottenere lo stesso client side & server.</span><span class="sxs-lookup"><span data-stu-id="e5367-132">You can use code first’s fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="e5367-133">Invece di utilizzo richiesto, vi mostrerò questa operazione utilizzando una convalida di MaxLength.</span><span class="sxs-lookup"><span data-stu-id="e5367-133">Rather than use Required, I’ll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="e5367-134">Applicazione delle configurazioni di API Fluent come codice prima di tutto è la compilazione del modello dalle classi.</span><span class="sxs-lookup"><span data-stu-id="e5367-134">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="e5367-135">È possibile inserire le configurazioni di eseguendo l'override del metodo OnModelCreating della classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="e5367-135">You can inject the configurations by overriding the DbContext class’ OnModelCreating  method.</span></span> <span data-ttu-id="e5367-136">Ecco una configurazione specifica che la proprietà BloggerName può essere non più di 10 caratteri.</span><span class="sxs-lookup"><span data-stu-id="e5367-136">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

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

<span data-ttu-id="e5367-137">Gli errori di convalida generati sulla base delle configurazioni API Fluent non reach dell'interfaccia utente, ma è può acquisisce automaticamente nel codice e quindi rispondere di conseguenza.</span><span class="sxs-lookup"><span data-stu-id="e5367-137">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="e5367-138">Alcuni gestione Ecco delle eccezioni codice di errore nella classe BlogController dell'applicazione che consente di acquisire tale errore di convalida quando Entity Framework tenta di salvare un blog con una BloggerName che supera il limite massimo di 10 caratteri.</span><span class="sxs-lookup"><span data-stu-id="e5367-138">Here’s some exception handling error code in the application’s BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

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

<span data-ttu-id="e5367-139">La convalida non vengono passata automaticamente in visualizzazione per questo motivo il codice aggiuntivo che usa AddModelError utilizzato.</span><span class="sxs-lookup"><span data-stu-id="e5367-139">The validation doesn’t automatically get passed back into the view which is why the additional code that uses ModelState.AddModelError is being used.</span></span> <span data-ttu-id="e5367-140">Ciò garantisce che i dettagli dell'errore rendono alla vista che userà quindi il ValidationMessageFor Htmlhelper per visualizzare l'errore.</span><span class="sxs-lookup"><span data-stu-id="e5367-140">This ensures that the error details make it to the view which will then use the ValidationMessageFor Htmlhelper to display the error.</span></span>

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="e5367-141">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="e5367-141">IValidatableObject</span></span>

<span data-ttu-id="e5367-142">IValidatableObject è un'interfaccia che si trova in System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="e5367-142">IValidatableObject is an interface that lives in System.ComponentModel.DataAnnotations.</span></span> <span data-ttu-id="e5367-143">Sebbene non sia parte dell'API Entity Framework, è possibile comunque sfruttare, per la convalida sul lato server nelle classi di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e5367-143">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="e5367-144">IValidatableObject fornisce un metodo di convalida che Entity Framework chiamerà durante SaveChanges oppure è possibile chiamare manualmente ogni volta che si desidera convalidare le classi.</span><span class="sxs-lookup"><span data-stu-id="e5367-144">IValidatableObject provides a Validate method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="e5367-145">Configurazioni necessarie, ad esempio e MaxLength eseguono validaton su un singolo campo.</span><span class="sxs-lookup"><span data-stu-id="e5367-145">Configurations such as Required and MaxLength perform validaton on a single field.</span></span> <span data-ttu-id="e5367-146">Nel metodo di convalida è possibile avere anche una logica più complessa, ad esempio, il confronto di due campi.</span><span class="sxs-lookup"><span data-stu-id="e5367-146">In the Validate method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="e5367-147">Nell'esempio seguente, la classe Blog è stato esteso per implementare IValidatableObject e quindi specificare una regola che il titolo e BloggerName non può essere uguale.</span><span class="sxs-lookup"><span data-stu-id="e5367-147">In the following example, the Blog class has been extended to implement IValidatableObject and then provide a rule that the Title and BloggerName cannot match.</span></span>

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

<span data-ttu-id="e5367-148">Il costruttore ValidationResult accetta una stringa che rappresenta il messaggio di errore e una matrice di stringhe che rappresentano i nomi dei membri che sono associati con la convalida.</span><span class="sxs-lookup"><span data-stu-id="e5367-148">The ValidationResult constructor takes a string that represents the error message and an array of strings that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="e5367-149">Poiché questa convalida controlla il titolo e il BloggerName, vengono restituiti entrambi i nomi delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="e5367-149">Since this validation checks both the Title and the BloggerName, both property names are returned.</span></span>

<span data-ttu-id="e5367-150">A differenza di convalida fornita dall'API Fluent, il risultato della convalida verrà riconosciuto dalla visualizzazione e il gestore di eccezioni che ho utilizzato in precedenza per aggiungere l'errore in ModelState non è necessario.</span><span class="sxs-lookup"><span data-stu-id="e5367-150">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into ModelState is unnecessary.</span></span> <span data-ttu-id="e5367-151">Perché ValidationResult impostare entrambi i nomi delle proprietà, il HtmlHelpers MVC visualizzare il messaggio di errore per entrambe le proprietà.</span><span class="sxs-lookup"><span data-stu-id="e5367-151">Because I set both property names in the ValidationResult, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![figure02](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="e5367-153">DbContext.ValidateEntity</span><span class="sxs-lookup"><span data-stu-id="e5367-153">DbContext.ValidateEntity</span></span>

<span data-ttu-id="e5367-154">DbContext è un metodo sostituibile denominato ValidateEntity.</span><span class="sxs-lookup"><span data-stu-id="e5367-154">DbContext has an Overridable method called ValidateEntity.</span></span> <span data-ttu-id="e5367-155">Quando si chiama SaveChanges, Entity Framework chiamerà il metodo per ogni entità nella propria cache il cui stato non è invariato.</span><span class="sxs-lookup"><span data-stu-id="e5367-155">When you call SaveChanges, Entity Framework will call this method for each entity in its cache whose state is not Unchanged.</span></span> <span data-ttu-id="e5367-156">È possibile inserire la logica di convalida direttamente in uso qui o anche questo metodo per chiamare, ad esempio, il metodo Blog.Validate aggiunto nella sezione precedente.</span><span class="sxs-lookup"><span data-stu-id="e5367-156">You can put validation logic directly in here or even use this method to call, for example, the Blog.Validate method added in the previous section.</span></span>

<span data-ttu-id="e5367-157">Di seguito è riportato un esempio di un override ValidateEntity che convalida ai nuovi post per assicurarsi che il titolo del post non è già stato utilizzato.</span><span class="sxs-lookup"><span data-stu-id="e5367-157">Here’s an example of a ValidateEntity override that validates new Posts to ensure that the post title hasn’t been used already.</span></span> <span data-ttu-id="e5367-158">Innanzitutto verifica se l'entità è una richiesta post e che sia stato aggiunto il relativo stato.</span><span class="sxs-lookup"><span data-stu-id="e5367-158">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="e5367-159">Se è questo il caso, la ricerca nel database per vedere se è già presente un post con lo stesso titolo.</span><span class="sxs-lookup"><span data-stu-id="e5367-159">If that’s the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="e5367-160">Se è già presente un post esistente, viene creato un nuovo DbEntityValidationResult.</span><span class="sxs-lookup"><span data-stu-id="e5367-160">If there is an existing post already, then a new DbEntityValidationResult is created.</span></span>

<span data-ttu-id="e5367-161">DbEntityValidationResult ospita un DbEntityEntry e un ICollection di DbValidationErrors per una singola entità.</span><span class="sxs-lookup"><span data-stu-id="e5367-161">DbEntityValidationResult houses a DbEntityEntry and an ICollection of DbValidationErrors for a single entity.</span></span> <span data-ttu-id="e5367-162">All'inizio di questo metodo, viene creata un'istanza di un DbEntityValidationResult e quindi gli eventuali errori che vengono individuati vengono aggiunti nella sua raccolta ValidationErrors.</span><span class="sxs-lookup"><span data-stu-id="e5367-162">At the start of this method, a  DbEntityValidationResult is instantiated and then any errors that are discovered are added into its ValidationErrors collection.</span></span>

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

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="e5367-163">Attivare in modo esplicito la convalida</span><span class="sxs-lookup"><span data-stu-id="e5367-163">Explicitly triggering validation</span></span>

<span data-ttu-id="e5367-164">Una chiamata a SaveChanges attiva tutte le convalide contenute in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="e5367-164">A call to SaveChanges triggers all of the validations covered in this article.</span></span> <span data-ttu-id="e5367-165">Ma non è necessario affidarsi a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="e5367-165">But you don’t need to rely on SaveChanges.</span></span> <span data-ttu-id="e5367-166">È possibile anche convalidare in un' posizione nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e5367-166">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="e5367-167">Attiva tutte le convalide, quelli definiti per le annotazioni o l'API Fluent, la convalida creata in IValidatableObject (ad esempio, Blog.Validate) e le convalide eseguite nel DbContext.ValidateEntity DbContext.GetValidationErrors metodo.</span><span class="sxs-lookup"><span data-stu-id="e5367-167">DbContext.GetValidationErrors will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in IValidatableObject (for example, Blog.Validate), and the validations performed in the DbContext.ValidateEntity method.</span></span>

<span data-ttu-id="e5367-168">Il codice seguente chiama GetValidationErrors nell'istanza corrente di un oggetto DbContext.</span><span class="sxs-lookup"><span data-stu-id="e5367-168">The following code will call GetValidationErrors on the current instance of a DbContext.</span></span> <span data-ttu-id="e5367-169">ValidationErrors sono raggruppate per tipo di entità in DbValidationRestuls.</span><span class="sxs-lookup"><span data-stu-id="e5367-169">ValidationErrors are grouped by entity type into DbValidationRestuls.</span></span> <span data-ttu-id="e5367-170">Il codice esegue l'iterazione prima di tutto tramite il DbValidationResults restituito dal metodo e quindi ogni ValidationError all'interno.</span><span class="sxs-lookup"><span data-stu-id="e5367-170">The code iterates first through the DbValidationResults returned by the method and then through each ValidationError inside.</span></span>

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

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="e5367-171">Altre considerazioni relative all'uso della convalida</span><span class="sxs-lookup"><span data-stu-id="e5367-171">Other considerations when using validation</span></span>

<span data-ttu-id="e5367-172">Ecco alcuni altri punti da considerare quando si usa la convalida di Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="e5367-172">Here are a few other points to consider when using Entity Framework validation:</span></span>

-   <span data-ttu-id="e5367-173">Il caricamento lazy è disabilitato durante la convalida.</span><span class="sxs-lookup"><span data-stu-id="e5367-173">Lazy loading is disabled during validation.</span></span>
-   <span data-ttu-id="e5367-174">Entity Framework convaliderà annotazioni dei dati in una proprietà non mappate (proprietà che non sono mappate a una colonna nel database).</span><span class="sxs-lookup"><span data-stu-id="e5367-174">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database).</span></span>
-   <span data-ttu-id="e5367-175">Dopo che le modifiche vengono rilevate durante il metodo SaveChanges, viene eseguita la convalida.</span><span class="sxs-lookup"><span data-stu-id="e5367-175">Validation is performed after changes are detected during SaveChanges.</span></span> <span data-ttu-id="e5367-176">Se si apportano modifiche durante la convalida è responsabilità dell'utente per notificare il rilevamento delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="e5367-176">If you make changes during validation it is your responsibility to notify the change tracker.</span></span>
-   <span data-ttu-id="e5367-177">DbUnexpectedValidationException viene generata se si verificano errori durante la convalida.</span><span class="sxs-lookup"><span data-stu-id="e5367-177">DbUnexpectedValidationException is thrown if errors occur during validation.</span></span>
-   <span data-ttu-id="e5367-178">Facet di Entity Framework è incluso nel modello (lunghezza massima, obbligatoria, e così via) provocherà la convalida, anche se non sono presenti annotazioni dei dati nelle classi e/o è utilizzata la finestra di progettazione di Entity Framework per creare il modello.</span><span class="sxs-lookup"><span data-stu-id="e5367-178">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are not data annotations on your classes and/or you used the EF Designer to create your model.</span></span>
-   <span data-ttu-id="e5367-179">Regole di precedenza:</span><span class="sxs-lookup"><span data-stu-id="e5367-179">Precedence rules:</span></span>
    -   <span data-ttu-id="e5367-180">Chiamate API Fluent esegue l'override di annotazioni dei dati corrispondente</span><span class="sxs-lookup"><span data-stu-id="e5367-180">Fluent API calls override the corresponding data annotations</span></span>
-   <span data-ttu-id="e5367-181">Ordine di esecuzione:</span><span class="sxs-lookup"><span data-stu-id="e5367-181">Execution order:</span></span>
    -   <span data-ttu-id="e5367-182">Convalida delle proprietà si verifica prima della convalida di tipo</span><span class="sxs-lookup"><span data-stu-id="e5367-182">Property validation occurs before type validation</span></span>
    -   <span data-ttu-id="e5367-183">Tipo di convalida si verifica solo se ha esito positivo della convalida della proprietà</span><span class="sxs-lookup"><span data-stu-id="e5367-183">Type validation only occurs if property validation succeeds</span></span>
-   <span data-ttu-id="e5367-184">Se una proprietà è complessa, la convalida si includerà anche:</span><span class="sxs-lookup"><span data-stu-id="e5367-184">If a property is complex its validation will also include:</span></span>
    -   <span data-ttu-id="e5367-185">Convalida a livello di proprietà nelle proprietà del tipo complesso</span><span class="sxs-lookup"><span data-stu-id="e5367-185">Property-level validation on the complex type properties</span></span>
    -   <span data-ttu-id="e5367-186">Tipo di convalida a livello del tipo complesso, compresa la convalida IValidatableObject del tipo complesso</span><span class="sxs-lookup"><span data-stu-id="e5367-186">Type level validation on the complex type, including IValidatableObject validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="e5367-187">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="e5367-187">Summary</span></span>

<span data-ttu-id="e5367-188">La convalida API in Entity Framework funziona molto bene con la convalida lato client in MVC, ma non è necessario basarsi sulla convalida sul lato client.</span><span class="sxs-lookup"><span data-stu-id="e5367-188">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="e5367-189">Entity Framework si occuperà della convalida sul lato server per DataAnnotations o le configurazioni che è stato applicato con l'API Office Fluent code first.</span><span class="sxs-lookup"><span data-stu-id="e5367-189">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="e5367-190">È stato anche illustrato un numero di punti di estendibilità per personalizzare il comportamento se si usa l'interfaccia IValidatableObject o si tocca DbContet.ValidateEntity nel metodo.</span><span class="sxs-lookup"><span data-stu-id="e5367-190">You also saw a number of extensibility points for customizing the behavior whether you use the IValidatableObject interface or tap into the DbContet.ValidateEntity method.</span></span> <span data-ttu-id="e5367-191">E questi ultimi due metodi di convalida sono disponibili tramite l'oggetto DbContext, se si utilizza il flusso di lavoro Database First o Model First Code First per descrivere il modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="e5367-191">And these last two means of validation are available through the DbContext, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
