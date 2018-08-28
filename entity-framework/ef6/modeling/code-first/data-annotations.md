---
title: Annotazioni dei dati First - EF6 del codice
author: divega
ms.date: 2016-10-23
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: 0ab66afa3babafe657b3ddb32c02c3fba0ae310e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994586"
---
# <a name="code-first-data-annotations"></a><span data-ttu-id="a2f0d-102">Annotazioni dei dati per Code First</span><span class="sxs-lookup"><span data-stu-id="a2f0d-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="a2f0d-103">**EF4.1 e versioni successive solo** -le funzionalità, le API, e così via illustrati in questa pagina sono stati introdotti in Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="a2f0d-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="a2f0d-105">Il contenuto in questa pagina è stato adattato dalla e l'articolo è stato scritto originariamente da Julie Lerman (\<http://thedatafarm.com>).</span><span class="sxs-lookup"><span data-stu-id="a2f0d-105">The content on this page is adapted from and article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="a2f0d-106">Code First di Entity Framework consente di usare le proprie classi di dominio per rappresentare il modello di Entity Framework si basa su per eseguire una query, modifica di rilevamento e l'aggiornamento di funzioni.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-106">Entity Framework Code First allows you to use your own domain classes to represent the model which EF relies on to perform querying, change tracking and updating functions.</span></span> <span data-ttu-id="a2f0d-107">Codice prima di tutto si basa su un modello di programmazione definito convenzione rispetto alle configurazioni.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-107">Code first leverages a programming pattern referred to as convention over configuration.</span></span> <span data-ttu-id="a2f0d-108">Ciò significa che il codice prima di tutto presuppone che le classi seguono le convenzioni che usa Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-108">What this means is that code first will assume that your classes follow the conventions that EF uses.</span></span> <span data-ttu-id="a2f0d-109">In tal caso, Entity Framework sarà in grado di elaborare i dettagli che necessarie per svolgere il proprio lavoro.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-109">In that case, EF will be able to work out the details it needs to do its job.</span></span> <span data-ttu-id="a2f0d-110">Tuttavia, se le classi non si seguono tali convenzioni, hai la possibilità di aggiungere le configurazioni alle classi per fornire Entity Framework con le informazioni che necessarie.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-110">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the information it needs.</span></span>

<span data-ttu-id="a2f0d-111">Codice prima di tutto offre due modi per aggiungere queste configurazioni alle classi.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-111">Code first gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="a2f0d-112">Una è l'uso semplici attributi DataAnnotations chiamati e l'altra è l'uso di codice prima di tutto è API Fluent, che offre un modo per descrivere le configurazioni in modo imperativo nel codice.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-112">One is using simple attributes called DataAnnotations and the other is using code first’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="a2f0d-113">Questo articolo è incentrato sull'uso di DataAnnotations (nello spazio dei nomi System.ComponentModel.DataAnnotations) per configurare classi, evidenziando le configurazioni più comunemente necessari.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-113">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="a2f0d-114">DataAnnotations vengono riconosciuti anche da un numero di applicazioni .NET, ad esempio ASP.NET MVC che consente a queste applicazioni di sfruttare le stesse annotazioni per le convalide sul lato client.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-114">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="a2f0d-115">Il modello</span><span class="sxs-lookup"><span data-stu-id="a2f0d-115">The model</span></span>

<span data-ttu-id="a2f0d-116">Viene illustrato prima DataAnnotations con una semplice coppia di classi di codice: post di Blog e Post.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-116">I’ll demonstrate code first DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
    public class Blog
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
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

<span data-ttu-id="a2f0d-117">Così come sono, le classi di Blog e Post seguono la convenzione prima di codice in modo facile e non necessaria alcuna piccole modifiche per aiutare a EF di lavorare con loro.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-117">As they are, the Blog and Post classes conveniently follow code first convention and required no tweaks to help EF work with them.</span></span> <span data-ttu-id="a2f0d-118">Ma è anche possibile usare le annotazioni per fornire informazioni aggiuntive per Entity Framework sulle classi e il database che eseguono il mapping a.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-118">But you can also use the annotations to provide more information to EF about the classes and the database that they map to.</span></span>

 

## <a name="key"></a><span data-ttu-id="a2f0d-119">Chiave</span><span class="sxs-lookup"><span data-stu-id="a2f0d-119">Key</span></span>

<span data-ttu-id="a2f0d-120">Entity Framework si basa su tutte le entità con un valore di chiave che viene utilizzata per le entità con rilevamento.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-120">Entity Framework relies on every entity having a key value that it uses for tracking entities.</span></span> <span data-ttu-id="a2f0d-121">Una delle convenzioni di codice dipendente prima di tutto è come implica la proprietà è la chiave in ognuna delle classi di primo codice.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-121">One of the conventions that code first depends on is how it implies which property is the key in each of the code first classes.</span></span> <span data-ttu-id="a2f0d-122">Tale convenzione consiste nel cercare una proprietà denominata "Id" o che combina il nome della classe e il "Id", ad esempio "BlogId".</span><span class="sxs-lookup"><span data-stu-id="a2f0d-122">That convention is to look for a property named “Id” or one that combines the class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="a2f0d-123">La proprietà verrà eseguito il mapping a una colonna chiave primaria nel database.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-123">The property will map to a primary key column in the database.</span></span>

<span data-ttu-id="a2f0d-124">Le classi post di Blog e Post seguono questa convenzione.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-124">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="a2f0d-125">Ma cosa succede se essi non?</span><span class="sxs-lookup"><span data-stu-id="a2f0d-125">But what if they didn’t?</span></span> <span data-ttu-id="a2f0d-126">Cosa accade se Blog usato il nome *PrimaryTrackingKey* invece o addirittura *foo*?</span><span class="sxs-lookup"><span data-stu-id="a2f0d-126">What if Blog used the name *PrimaryTrackingKey* instead or even *foo*?</span></span> <span data-ttu-id="a2f0d-127">Se il codice prima di tutto non trova una proprietà che corrisponde a questa convenzione genererà un'eccezione a causa di requisiti di Entity Framework è necessario che una proprietà chiave.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-127">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="a2f0d-128">È possibile utilizzare l'annotazione chiave per specificare quale proprietà deve essere usata come EntityKey.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-128">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

``` csharp
    public class Blog
    {
        [Key]
        public int PrimaryTrackingKey { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }
```

<span data-ttu-id="a2f0d-129">Se si è l'uso di codice prima di tutto è la funzionalità di generazione del database, la tabella Blog avrà una colonna chiave primaria denominata PrimaryTrackingKey che viene definito anche come identità per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-129">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey which is also defined as Identity by default.</span></span>

![jj591583_figure01](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="a2f0d-131">Chiavi composte</span><span class="sxs-lookup"><span data-stu-id="a2f0d-131">Composite keys</span></span>

<span data-ttu-id="a2f0d-132">Entity Framework supporta le chiavi composte - chiavi primarie costituite da più di una proprietà.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-132">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="a2f0d-133">Ad esempio, le è stato possibile disporre di una classe Passport la cui chiave primaria è una combinazione di PassportNumber e IssuingCountry.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-133">For example, your could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        public int PassportNumber { get; set; }
        [Key]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="a2f0d-134">Se si prova a usare la classe precedente nel modello di EF, si otterrebbe un indicante le eccezioni InvalidOperationException;</span><span class="sxs-lookup"><span data-stu-id="a2f0d-134">If you were to try and use the above class in your EF model you wuld get an InvalidOperationExceptions stating;</span></span>

<span data-ttu-id="a2f0d-135">*Impossibile determinare di composite primario ordinamento delle chiavi per il tipo 'Passport'. Usare il ColumnAttribute o il metodo HasKey per specificare un ordine per le chiavi primarie composte.*</span><span class="sxs-lookup"><span data-stu-id="a2f0d-135">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="a2f0d-136">Quando si dispongono di una chiave composta, Entity Framework è necessario definire un ordine delle proprietà della chiave.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-136">When you have composite keys, Entity Framework requires you to define an order of the key properties.</span></span> <span data-ttu-id="a2f0d-137">È possibile farlo mediante l'annotazione di colonna per specificare un ordine.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-137">You can do this using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="a2f0d-138">Il valore dell'ordine è relativo (anziché l'indice in base) in modo che eventuali valori possono essere usati.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-138">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="a2f0d-139">Ad esempio, 100 e 200 sarebbe accettabile al posto di 1 e 2.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-139">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        [Column(Order=1)]
        public int PassportNumber { get; set; }
        [Key]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="a2f0d-140">Se sono presenti entità con una chiave esterna composta è necessario specificare la stessa colonna di ordinamento utilizzato per la proprietà di chiave primaria corrispondente.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-140">If you have entities with composite foreign keys then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="a2f0d-141">Solo l'ordinamento relativo all'interno di proprietà della chiave esterna deve essere lo stesso, i valori esatti assegnati a **ordine** non è necessario per la corrispondenza.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-141">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="a2f0d-142">Ad esempio, nella classe seguente, 3 e 4 può essere usato al posto di 1 e 2.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-142">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

``` csharp
    public class PassportStamp
    {
        [Key]
        public int StampId { get; set; }
        public DateTime Stamped { get; set; }
        public string StampingCountry { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 1)]
        public int PassportNumber { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }

        public Passport Passport { get; set; }
    }
```

## <a name="required"></a><span data-ttu-id="a2f0d-143">Obbligatorio</span><span class="sxs-lookup"><span data-stu-id="a2f0d-143">Required</span></span>

<span data-ttu-id="a2f0d-144">L'annotazione obbligatorio indica a EF che una particolare proprietà è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-144">The Required annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="a2f0d-145">Aggiunta di necessarie per la proprietà Title forzerà (Entity Framework e MVC) per garantire che la proprietà contiene i dati di.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-145">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="a2f0d-146">Nessun senza ulteriori modifiche di codice o markup dell'applicazione, un'applicazione MVC eseguirà la convalida lato client, anche in modo dinamico la creazione di un messaggio utilizzando i nomi di proprietà e l'annotazione.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-146">With no additional no code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![jj591583_figure02](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="a2f0d-148">L'attributo obbligatorio influirà anche database generato, rendendo la proprietà mappata che non ammette valori null.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-148">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="a2f0d-149">Si noti che il campo del titolo è stato modificato in "not null".</span><span class="sxs-lookup"><span data-stu-id="a2f0d-149">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="a2f0d-150">In alcuni casi potrebbe non essere possibile per la colonna nel database sia non nullable, anche se la proprietà è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-150">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="a2f0d-151">Ad esempio, quando tramite una data di strategia ereditarietà tabella per gerarchia per i tipi più viene archiviato in una singola tabella.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-151">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="a2f0d-152">Se un tipo derivato include una proprietà obbligatoria la colonna può essere reso non nullable poiché non tutti i tipi nella gerarchia disporrà di questa proprietà.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-152">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![jj591583_figure03](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="a2f0d-154">MaxLength e MinLength</span><span class="sxs-lookup"><span data-stu-id="a2f0d-154">MaxLength and MinLength</span></span>

<span data-ttu-id="a2f0d-155">Gli attributi MaxLength e MinLength consentono di specificare le convalide di proprietà aggiuntive, come è stato fatto con obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-155">The MaxLength and MinLength attributes allow you to specify additional property validations, just as you did with Required.</span></span>

<span data-ttu-id="a2f0d-156">Ecco il BloggerName con i requisiti di lunghezza.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-156">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="a2f0d-157">L'esempio illustra anche come combinare gli attributi.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-157">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="a2f0d-158">L'annotazione di proprietà MaxLength influirà il database impostando la lunghezza della proprietà a 10.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-158">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![jj591583_figure04](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="a2f0d-160">Annotazione sul lato client, MVC ed Entity Framework 4.1 lato server annotazione entrambi rispetterà la convalida, nuovamente in modo dinamico la creazione di un messaggio di errore: "il campo BloggerName deve essere un tipo stringa o matrice con una lunghezza massima di '10'." Tale messaggio è un po' lungo.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-160">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="a2f0d-161">Molte annotazioni consentono di specificare un messaggio di errore con l'attributo di messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-161">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="a2f0d-162">È anche possibile specificare ErrorMessage nell'annotazione necessaria.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-162">You can also specify ErrorMessage in the Required annotation.</span></span>

![jj591583_figure05](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="a2f0d-164">NotMapped</span><span class="sxs-lookup"><span data-stu-id="a2f0d-164">NotMapped</span></span>

<span data-ttu-id="a2f0d-165">Convenzione primo codice determina che tutte le proprietà di un tipo di dati supportati sono rappresentata nel database.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-165">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="a2f0d-166">Ma ciò non sempre avveniva nelle applicazioni.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-166">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="a2f0d-167">Ad esempio si potrebbe disporre di una proprietà nella classe del Blog che crea un codice in base ai campi titolo e BloggerName.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-167">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="a2f0d-168">Tale proprietà può essere creata in modo dinamico e non devono essere archiviati.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-168">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="a2f0d-169">È possibile contrassegnare le proprietà che non eseguono il mapping al database con l'annotazione NotMapped, ad esempio questa proprietà BlogCode.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-169">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

``` csharp
    [NotMapped]
    public string BlogCode
    {
        get
        {
            return Title.Substring(0, 1) + ":" + BloggerName.Substring(0, 1);
        }
    }
```

 

## <a name="complextype"></a><span data-ttu-id="a2f0d-170">ComplexType</span><span class="sxs-lookup"><span data-stu-id="a2f0d-170">ComplexType</span></span>

<span data-ttu-id="a2f0d-171">Non è insolito per descrivere le entità di dominio in un set di classi e quindi di livello tali classi per descrivere un'entità completa.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-171">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="a2f0d-172">Ad esempio, si potrebbe aggiungere una classe denominata BlogDetails al modello.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-172">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="a2f0d-173">Si noti che BlogDetails non dispone di qualsiasi tipo di proprietà di chiave.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-173">Notice that BlogDetails does not have any type of key property.</span></span> <span data-ttu-id="a2f0d-174">Nella progettazione basata su domini, BlogDetails si intende un oggetto valore.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-174">In domain driven design, BlogDetails is referred to as a value object.</span></span> <span data-ttu-id="a2f0d-175">Entity Framework fa riferimento a oggetti valore come tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-175">Entity Framework refers to value objects as complex types.</span></span>  <span data-ttu-id="a2f0d-176">I tipi complessi non possono essere rilevati in modo indipendente.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-176">Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="a2f0d-177">Tuttavia come proprietà nella classe Blog BlogDetails verrà rilevato come parte di un oggetto di Blog.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-177">However as a property in the Blog class, BlogDetails it will be tracked as part of a Blog object.</span></span> <span data-ttu-id="a2f0d-178">Affinché code first per riconoscere questo, è necessario contrassegnare la classe BlogDetails come ComplexType.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-178">In order for code first to recognize this, you must mark the BlogDetails class as a ComplexType.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="a2f0d-179">A questo punto è possibile aggiungere una proprietà nella classe Blog per rappresentare il BlogDetails per tale blog.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-179">Now you can add a property in the Blog class to represent the BlogDetails for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="a2f0d-180">Nel database, la tabella Blog conterrà tutte le proprietà del blog incluse le proprietà contenute nella relativa proprietà BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-180">In the database, the Blog table will contain all of the properties of the blog including the properties contained in its BlogDetail property.</span></span> <span data-ttu-id="a2f0d-181">Per impostazione predefinita, ognuno di essi è preceduto con il nome del tipo complesso, BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-181">By default, each one is preceded with the name of the complex type, BlogDetail.</span></span>

![jj591583_figure06](~/ef6/media/jj591583-figure06.png)

<span data-ttu-id="a2f0d-183">Un'altra interessante ricordare è che anche se la proprietà DateCreated è stata definita come un valore DateTime che non ammette valori null nella classe, il campo di database corrispondente è nullable.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-183">Another interesting note is that although the DateCreated property was defined as a non-nullable DateTime in the class, the relevant database field is nullable.</span></span> <span data-ttu-id="a2f0d-184">È necessario usare l'annotazione necessaria se si vuole interessano lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-184">You must use the Required annotation if you wish to affect the database schema.</span></span>

 

## <a name="concurrencycheck"></a><span data-ttu-id="a2f0d-185">ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="a2f0d-185">ConcurrencyCheck</span></span>

<span data-ttu-id="a2f0d-186">L'annotazione ConcurrencyCheck consente di contrassegnare una o più proprietà da utilizzare per controllo della concorrenza nel database quando un utente modifica o elimina un'entità.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-186">The ConcurrencyCheck annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="a2f0d-187">Se Usa già con la finestra di progettazione di Entity Framework, questo viene allineato con l'impostazione ConcurrencyMode della proprietà su Fixed.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-187">If you've been working with the EF Designer, this aligns with setting a property's ConcurrencyMode to Fixed.</span></span>

<span data-ttu-id="a2f0d-188">Di seguito viene illustrato il funzionamento ConcurrencyCheck aggiungendola alla proprietà BloggerName.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-188">Let’s see how ConcurrencyCheck works by adding it to the BloggerName property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="a2f0d-189">Quando viene chiamato SaveChanges, a causa di annotazione ConcurrencyCheck sul campo BloggerName, il valore originale di tale proprietà verrà utilizzato nell'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-189">When SaveChanges is called, because of the ConcurrencyCheck annotation on the BloggerName field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="a2f0d-190">Il comando tenterà di individuare la riga corretta da un filtro non solo sul valore della chiave, ma anche sul valore originale di BloggerName.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-190">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of BloggerName.</span></span>  <span data-ttu-id="a2f0d-191">Ecco le parti critiche del comando di aggiornamento inviate al database, in cui è possibile visualizzare il comando verrà aggiornata la riga contenente un PrimaryTrackingKey è 1 e un BloggerName di "Julie", ovvero il valore originale quando questo blog è stato recuperato dal database.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-191">Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a PrimaryTrackingKey is 1 and a BloggerName of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="a2f0d-192">Se un utente è stato modificato il nome blogger per tale blog nel frattempo, questo aggiornamento avrà esito negativo e si otterrà un DbUpdateConcurrencyException che sarà necessario gestire.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-192">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a DbUpdateConcurrencyException that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="a2f0d-193">TimeStamp</span><span class="sxs-lookup"><span data-stu-id="a2f0d-193">TimeStamp</span></span>

<span data-ttu-id="a2f0d-194">È più comune da usare i campi timestamp o rowversion per controllo della concorrenza.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-194">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="a2f0d-195">Ma invece di usare l'annotazione ConcurrencyCheck, è possibile usare l'annotazione TimeStamp più specifico, purché il tipo della proprietà è una matrice di byte.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-195">But rather than using the ConcurrencyCheck annotation, you can use the more specific TimeStamp annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="a2f0d-196">Codice prima di tutto considererà le proprietà di Timestamp uguale come proprietà ConcurrencyCheck, ma si assicurerà che il campo di database generata da codice prima di tutto è non nullable.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-196">Code first will treat Timestamp properties the same as ConcurrencyCheck properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="a2f0d-197">È possibile avere solo una proprietà di timestamp in una determinata classe.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-197">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="a2f0d-198">Aggiunta della proprietà seguente alla classe di Blog:</span><span class="sxs-lookup"><span data-stu-id="a2f0d-198">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="a2f0d-199">risultati nel codice creando innanzitutto una colonna timestamp non ammettono valori null nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-199">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![jj591583_figure07](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="a2f0d-201">Tabelle e colonne</span><span class="sxs-lookup"><span data-stu-id="a2f0d-201">Table and Column</span></span>

<span data-ttu-id="a2f0d-202">Se si consente codice prima creare il database, è possibile modificare il nome delle tabelle e colonne che si sta creando.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-202">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="a2f0d-203">È inoltre possibile utilizzare Code First con un database esistente.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-203">You can also use Code First with an existing database.</span></span> <span data-ttu-id="a2f0d-204">Ma non sempre è il caso che i nomi delle classi e proprietà del dominio corrispondano ai nomi delle tabelle e colonne nel database.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-204">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="a2f0d-205">La classe è denominata Blog e per convenzione, code presuppone prima di tutto che ciò verrà eseguito il mapping a una tabella denominata blog.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-205">My class is named Blog and by convention, code first presumes this will map to a table named Blogs.</span></span> <span data-ttu-id="a2f0d-206">Se non è questo il caso è possibile specificare il nome della tabella con l'attributo di tabella.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-206">If that's not the case you can specify the name of the table with the Table attribute.</span></span> <span data-ttu-id="a2f0d-207">In questo caso, ad esempio, l'annotazione è specificando che il nome della tabella è InternalBlogs.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-207">Here for example, the annotation is specifying that the table name is InternalBlogs.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="a2f0d-208">L'annotazione di colonna è una TEPSA ulteriori nello specificare gli attributi di una colonna mappata.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-208">The Column annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="a2f0d-209">È possibile stabilire un nome, tipo di dati o anche l'ordine in cui viene visualizzata una colonna nella tabella.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-209">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="a2f0d-210">Ecco un esempio di attributo di colonna.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-210">Here is an example of the Column attribute.</span></span>

``` csharp
    [Column(“BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="a2f0d-211">Non confondere TypeName (attributo) della colonna con il DataType DataAnnotation.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-211">Don’t confuse Column’s TypeName attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="a2f0d-212">Tipo di dati è un'annotazione utilizzata per l'interfaccia utente e pertanto ignorata dalle Code First.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-212">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="a2f0d-213">Ecco la tabella dopo che è stato rigenerato.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-213">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="a2f0d-214">Il nome della tabella è stato modificato per InternalBlogs e colonna Descrizione dal tipo complesso è ora BlogDescription.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-214">The table name has changed to InternalBlogs and Description column from the complex type is now BlogDescription.</span></span> <span data-ttu-id="a2f0d-215">Il nome specificato nell'annotazione, code first non utilizzerà la convenzione del nome della colonna che iniziano con il nome del tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-215">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![jj591583_figure08](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="a2f0d-217">DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="a2f0d-217">DatabaseGenerated</span></span>

<span data-ttu-id="a2f0d-218">Una funzionalità di database più importanti è la possibilità di disporre di proprietà calcolate.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-218">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="a2f0d-219">Se si esegue il mapping di Code First alle classi di tabelle contenenti colonne calcolate, non si desidera che Entity Framework per tentare di aggiornare le colonne.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-219">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="a2f0d-220">Ma si desidera restituire questi valori dal database dopo aver inserito o i dati aggiornati a Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-220">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="a2f0d-221">È possibile utilizzare l'annotazione DatabaseGenerated per contrassegnare tutte le proprietà nella classe con l'enumerazione calcolata.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-221">You can use the DatabaseGenerated annotation to flag those properties in your class along with the Computed enum.</span></span> <span data-ttu-id="a2f0d-222">Ne sono altre enumerazioni e identità.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-222">Other enums are None and Identity.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGenerationOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="a2f0d-223">È possibile usare database generati nelle colonne timestamp o byte quando codice genera prima di tutto il database, in caso contrario, è consigliabile utilizzare solo questo posizionando i database esistenti perché codice prima di tutto sarà in grado di determinare la formula per la colonna calcolata.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-223">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="a2f0d-224">Leggere sopra che per impostazione predefinita, una proprietà chiave che è un numero intero diventerà una chiave di identità nel database.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-224">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="a2f0d-225">Sarebbe lo stesso come impostazione DatabaseGenerated su DatabaseGenerationOption.Identity.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-225">That would be the same as setting DatabaseGenerated to DatabaseGenerationOption.Identity.</span></span> <span data-ttu-id="a2f0d-226">Se si desidera che non sia una chiave di identità, è possibile impostare il valore per DatabaseGenerationOption.None.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-226">If you do not want it to be an identity key, you can set the value to DatabaseGenerationOption.None.</span></span>

 

## <a name="index"></a><span data-ttu-id="a2f0d-227">Indice</span><span class="sxs-lookup"><span data-stu-id="a2f0d-227">Index</span></span>

> [!NOTE]
> <span data-ttu-id="a2f0d-228">**EF6.1 e versioni successive solo** -attributo l'indice è stata introdotta in Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-228">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="a2f0d-229">Se si usa una versione precedente le informazioni contenute in questa sezione non si applicano.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-229">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="a2f0d-230">È possibile creare un indice su una o più colonne usando il **IndexAttribute**.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-230">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="a2f0d-231">Aggiunta dell'attributo a una o più proprietà verrà causare Entity Framework creare l'indice corrispondente nel database durante la creazione del database o eseguire lo scaffolding corrispondente **CreateIndex** chiama se si usa migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-231">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="a2f0d-232">Ad esempio, il codice seguente comporterà un indice viene creato per il **Rating** della colonna della **post** tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-232">For example, the following code will result in an index being created on the **Rating** column of the **Posts** table in the database.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index]
        public int Rating { get; set; }
        public int BlogId { get; set; }
    }
```

<span data-ttu-id="a2f0d-233">Per impostazione predefinita, l'indice verrà denominato **IX\_&lt;NomeProprietà&gt;**  (IX\_Rating nell'esempio precedente).</span><span class="sxs-lookup"><span data-stu-id="a2f0d-233">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="a2f0d-234">È anche possibile specificare un nome per l'indice tuttavia.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-234">You can also specify a name for the index though.</span></span> <span data-ttu-id="a2f0d-235">L'esempio seguente specifica che l'indice deve essere denominata **PostRatingIndex**.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-235">The following example specifies that the index should be named **PostRatingIndex**.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="a2f0d-236">Per impostazione predefinita, gli indici non sono univoche, ma è possibile usare la **IsUnique** denominato parametro per specificare che un indice deve essere univoco.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-236">By default, indexes are non-unique, but you can use the **IsUnique** named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="a2f0d-237">Nell'esempio seguente illustra un indice univoco su un **utente**del nome di accesso.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-237">The following example introduces a unique index on a **User**'s login name.</span></span>

``` csharp
    public class User
    {
        public int UserId { get; set; }

        [Index(IsUnique = true)]
        [StringLength(200)]
        public string Username { get; set; }

        public string DisplayName { get; set; }
    }
```

### <a name="multiple-column-indexes"></a><span data-ttu-id="a2f0d-238">Indici più colonne</span><span class="sxs-lookup"><span data-stu-id="a2f0d-238">Multiple-Column Indexes</span></span>

<span data-ttu-id="a2f0d-239">Gli indici che si estendono su più colonne vengono specificati usando lo stesso nome in più annotazioni di indice per una determinata tabella.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-239">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="a2f0d-240">Quando si creano indici su più colonne, è necessario specificare un ordine per le colonne nell'indice.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-240">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="a2f0d-241">Ad esempio, il codice seguente crea un indice multicolonna sul **Rating** e **BlogId** chiamato **IX\_BlogAndRating**.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-241">For example, the following code creates a multi-column index on **Rating** and **BlogId** called **IX\_BlogAndRating**.</span></span> <span data-ttu-id="a2f0d-242">**BlogId** è la prima colonna nell'indice e **Rating** è il secondo.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-242">**BlogId** is the first column in the index and **Rating** is the second.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index("IX_BlogIdAndRating", 2)]
        public int Rating { get; set; }
        [Index("IX_BlogIdAndRating", 1)]
        public int BlogId { get; set; }
    }
```

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="a2f0d-243">Gli attributi di relazione: InverseProperty e perché ForeignKey</span><span class="sxs-lookup"><span data-stu-id="a2f0d-243">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="a2f0d-244">Questa pagina fornisce informazioni sulla configurazione di relazioni nel modello Code First con annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-244">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="a2f0d-245">Per informazioni generali sulle relazioni in Entity Framework e su come accedere e modificare dati tramite relazioni, vedere [relazioni di & proprietà di navigazione](~/ef6/fundamentals/relationships.md). \*</span><span class="sxs-lookup"><span data-stu-id="a2f0d-245">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="a2f0d-246">Convenzione primo codice si occuperà delle relazioni nel modello più comune, ma vi sono casi in cui è necessario supporto.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-246">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="a2f0d-247">Modifica del nome della proprietà della chiave nella classe Blog creata un problema con le relazioni con i Post.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-247">Changing the name of the key property in the Blog class created a problem with its relationship to Post.</span></span> 

<span data-ttu-id="a2f0d-248">Durante la generazione del database, codice innanzitutto rileva la proprietà BlogId nella classe Post e lo riconosce, per la convenzione che corrisponde a un nome e il "Id", come una chiave esterna per la classe di Blog.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-248">When generating the database, code first sees the BlogId property in the Post class and recognizes it, by the convention that it matches a class name plus “Id”, as a foreign key to the Blog class.</span></span> <span data-ttu-id="a2f0d-249">Ma è presente nessuna proprietà BlogId nella classe blog.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-249">But there is no BlogId property in the blog class.</span></span> <span data-ttu-id="a2f0d-250">La soluzione al problema consiste nel creare una proprietà di navigazione nel Post e usare il DataAnnotation esterna per consentire codice innanzitutto comprendere come creare la relazione tra le due classi, usando la proprietà Post.BlogId, nonché come specificare i vincoli nel database.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-250">The solution for this is to create a navigation property in the Post and use the Foreign DataAnnotation to help code first understand how to build the relationship between the two classes —using the Post.BlogId property — as well as how to specify constraints in the database.</span></span>

``` csharp
    public class Post
    {
            public int Id { get; set; }
            public string Title { get; set; }
            public DateTime DateCreated { get; set; }
            public string Content { get; set; }
            public int BlogId { get; set; }
            [ForeignKey("BlogId")]
            public Blog Blog { get; set; }
            public ICollection<Comment> Comments { get; set; }
    }
```

<span data-ttu-id="a2f0d-251">Il vincolo nel database Mostra una relazione tra InternalBlogs.PrimaryTrackingKey e Posts.BlogId.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-251">The constraint in the database shows a relationship between InternalBlogs.PrimaryTrackingKey and Posts.BlogId.</span></span> 

![jj591583_figure09](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="a2f0d-253">Il InverseProperty viene usato quando si dispone di più relazioni tra classi.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-253">The InverseProperty is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="a2f0d-254">Nella classe Post, è possibile tenere traccia di chi ha scritto un post di blog, oltre che ha modificata per ultimo.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-254">In the Post class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="a2f0d-255">Ecco due nuove proprietà di navigazione per la classe Post.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-255">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="a2f0d-256">È anche necessario aggiungere nella classe di persona a cui fanno riferimento tali proprietà.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-256">You’ll also need to add in the Person class referenced by these properties.</span></span> <span data-ttu-id="a2f0d-257">La classe Person presenta le proprietà di navigazione indietro al Post, uno per tutti i post scritti dall'utente e una per tutti i post aggiornati da questa persona.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-257">The Person class has navigation properties back to the Post, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="a2f0d-258">Prima di tutto codice non è in grado di associare le proprietà, le due classi di per sé.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-258">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="a2f0d-259">La tabella di database per i post deve avere una chiave esterna per la persona del CreatedBy e uno per la persona UpdatedBy ma il codice innanzitutto creare quattro proprietà di chiave esterna verrà: persona\_Id, Person\_Id1, CreatedBy\_Id e UpdatedBy\_ID.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-259">The database table for Posts should have one foreign key for the CreatedBy person and one for the UpdatedBy person but code first will create four will foreign key properties: Person\_Id, Person\_Id1, CreatedBy\_Id and UpdatedBy\_Id.</span></span>

![jj591583_figure10](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="a2f0d-261">Per risolvere questi problemi, è possibile utilizzare l'annotazione InverseProperty per specificare l'allineamento delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-261">To fix these problems, you can use the InverseProperty annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="a2f0d-262">Poiché la proprietà PostsWritten di persona riconosce che si riferisce al tipo di Post, compilerà la relazione di Post.CreatedBy.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-262">Because the PostsWritten property in Person knows that this refers to the Post type, it will build the relationship to Post.CreatedBy.</span></span> <span data-ttu-id="a2f0d-263">Analogamente, verrà connessa a Post.UpdatedBy PostsUpdated.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-263">Similarly, PostsUpdated will be connected to Post.UpdatedBy.</span></span> <span data-ttu-id="a2f0d-264">E il codice prima di tutto non crea le chiavi esterne aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-264">And code first will not create the extra foreign keys.</span></span>

![jj591583_figure11](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="a2f0d-266">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="a2f0d-266">Summary</span></span>

<span data-ttu-id="a2f0d-267">DataAnnotations consentono non solo di descrivere la convalida sul lato client e server nelle classi prima del codice, ma consentono anche di migliorare e anche correggere le ipotesi che codice prima di tutto renderà sulle classi base la convenzioni.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-267">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="a2f0d-268">Con DataAnnotations non è possibile richiedere solo la generazione dello schema di database, ma è anche possibile mappare le classi di code first per un database preesistente.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-268">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="a2f0d-269">Mentre sono molto flessibile, tenere presente che DataAnnotations forniscono solo più comunemente necessarie modifiche alla configurazione che è possibile apportare le classi di code first.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-269">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="a2f0d-270">Per configurare le classi per alcune dei casi limite, dovrebbe essere al meccanismo di configurazione alternativa API Fluent di Code First.</span><span class="sxs-lookup"><span data-stu-id="a2f0d-270">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>
