---
title: Annotazioni dei dati First - EF6 del codice
author: divega
ms.date: 10/23/2016
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: 38ae52543ed99e5a1c1da7d19a2e15d168e3a1bd
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490115"
---
# <a name="code-first-data-annotations"></a><span data-ttu-id="ee55b-102">Annotazioni dei dati per Code First</span><span class="sxs-lookup"><span data-stu-id="ee55b-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="ee55b-103">**EF4.1 e versioni successive solo** -le funzionalità, le API, e così via illustrati in questa pagina sono stati introdotti in Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="ee55b-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="ee55b-104">Se si usa una versione precedente, alcune o tutte queste informazioni non è applicabile.</span><span class="sxs-lookup"><span data-stu-id="ee55b-104">If you are using an earlier version, some or all of this information does not apply.</span></span>

<span data-ttu-id="ee55b-105">Il contenuto in questa pagina è stato adattato da un articolo scritto originariamente di Julie Lerman (\<http://thedatafarm.com>).</span><span class="sxs-lookup"><span data-stu-id="ee55b-105">The content on this page is adapted from an article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="ee55b-106">Code First di Entity Framework consente di usare le proprie classi di dominio per rappresentare il modello di Entity Framework si basa su per eseguire una query, modifica di rilevamento e l'aggiornamento di funzioni.</span><span class="sxs-lookup"><span data-stu-id="ee55b-106">Entity Framework Code First allows you to use your own domain classes to represent the model that EF relies on to perform querying, change tracking, and updating functions.</span></span> <span data-ttu-id="ee55b-107">Codice prima di tutto si basa su un modello di programmazione definito come 'convention over configuration'.</span><span class="sxs-lookup"><span data-stu-id="ee55b-107">Code First leverages a programming pattern referred to as 'convention over configuration.'</span></span> <span data-ttu-id="ee55b-108">Codice prima di tutto presupporrà che seguono le convenzioni di Entity Framework le classi e in tal caso, verranno attivati automaticamente su come eseguire il processo.</span><span class="sxs-lookup"><span data-stu-id="ee55b-108">Code First will assume that your classes follow the conventions of Entity Framework, and in that case, will automatically work out how to perform it's job.</span></span> <span data-ttu-id="ee55b-109">Tuttavia, se le classi non si seguono tali convenzioni, hai la possibilità di aggiungere le configurazioni alle classi per fornire Entity Framework con le informazioni necessarie.</span><span class="sxs-lookup"><span data-stu-id="ee55b-109">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the requisite information.</span></span>

<span data-ttu-id="ee55b-110">Codice prima di tutto offre due modi per aggiungere queste configurazioni alle classi.</span><span class="sxs-lookup"><span data-stu-id="ee55b-110">Code First gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="ee55b-111">Uno è con attributi semplici chiamati DataAnnotations e il secondo Usa API del Code First Fluent, che offre un modo per descrivere le configurazioni in modo imperativo nel codice.</span><span class="sxs-lookup"><span data-stu-id="ee55b-111">One is using simple attributes called DataAnnotations, and the second is using Code First’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="ee55b-112">Questo articolo è incentrato sull'uso di DataAnnotations (nello spazio dei nomi System.ComponentModel.DataAnnotations) per configurare classi, evidenziando le configurazioni più comunemente necessari.</span><span class="sxs-lookup"><span data-stu-id="ee55b-112">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="ee55b-113">DataAnnotations vengono riconosciuti anche da un numero di applicazioni .NET, ad esempio ASP.NET MVC che consente a queste applicazioni di sfruttare le stesse annotazioni per le convalide sul lato client.</span><span class="sxs-lookup"><span data-stu-id="ee55b-113">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="ee55b-114">Il modello</span><span class="sxs-lookup"><span data-stu-id="ee55b-114">The model</span></span>

<span data-ttu-id="ee55b-115">Illustrerò DataAnnotations prima del codice con una semplice coppia di classi: post di Blog e Post.</span><span class="sxs-lookup"><span data-stu-id="ee55b-115">I’ll demonstrate Code First DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

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

<span data-ttu-id="ee55b-116">Così come sono, le classi di Blog e Post seguono la convenzione prima di codice in modo facile e non richiedono modifiche specifiche per abilitare la compatibilità di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ee55b-116">As they are, the Blog and Post classes conveniently follow code first convention and require no tweaks to enable EF compatability.</span></span> <span data-ttu-id="ee55b-117">Tuttavia, è possibile utilizzare anche le annotazioni per a Entity Framework vengono fornite ulteriori informazioni sulle classi e il database a cui eseguono il mapping.</span><span class="sxs-lookup"><span data-stu-id="ee55b-117">However, you can also use the annotations to provide more information to EF about the classes and the database to which they map.</span></span>

 

## <a name="key"></a><span data-ttu-id="ee55b-118">Chiave</span><span class="sxs-lookup"><span data-stu-id="ee55b-118">Key</span></span>

<span data-ttu-id="ee55b-119">Entity Framework si basa su tutte le entità con un valore di chiave utilizzata per l'entità di rilevamento.</span><span class="sxs-lookup"><span data-stu-id="ee55b-119">Entity Framework relies on every entity having a key value that is used for entity tracking.</span></span> <span data-ttu-id="ee55b-120">Una convenzione di Code First è proprietà chiave implicita. Codice cerca innanzitutto una proprietà denominata "Id", o una combinazione di nome di classe e "Id", ad esempio "BlogId".</span><span class="sxs-lookup"><span data-stu-id="ee55b-120">One convention of Code First is implicit key properties; Code First will look for a property named “Id”, or a combination of class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="ee55b-121">Questa proprietà verrà eseguito il mapping a una colonna chiave primaria nel database.</span><span class="sxs-lookup"><span data-stu-id="ee55b-121">This property will map to a primary key column in the database.</span></span>

<span data-ttu-id="ee55b-122">Le classi post di Blog e Post seguono questa convenzione.</span><span class="sxs-lookup"><span data-stu-id="ee55b-122">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="ee55b-123">Cosa accade se non si ha?</span><span class="sxs-lookup"><span data-stu-id="ee55b-123">What if they didn’t?</span></span> <span data-ttu-id="ee55b-124">Cosa accade se Blog usato il nome *PrimaryTrackingKey* invece, o addirittura *foo*?</span><span class="sxs-lookup"><span data-stu-id="ee55b-124">What if Blog used the name *PrimaryTrackingKey* instead, or even *foo*?</span></span> <span data-ttu-id="ee55b-125">Se il codice prima di tutto non trova una proprietà che corrisponde a questa convenzione genererà un'eccezione a causa di requisiti di Entity Framework è necessario che una proprietà chiave.</span><span class="sxs-lookup"><span data-stu-id="ee55b-125">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="ee55b-126">È possibile utilizzare l'annotazione chiave per specificare quale proprietà deve essere usata come EntityKey.</span><span class="sxs-lookup"><span data-stu-id="ee55b-126">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

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

<span data-ttu-id="ee55b-127">Se si è l'uso di codice prima di tutto è la funzionalità di generazione del database, la tabella Blog avrà una colonna chiave primaria denominata PrimaryTrackingKey, che è definito anche come identità per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="ee55b-127">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey, which is also defined as Identity by default.</span></span>

![Blog di tabella con la chiave primaria](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="ee55b-129">Chiavi composte</span><span class="sxs-lookup"><span data-stu-id="ee55b-129">Composite keys</span></span>

<span data-ttu-id="ee55b-130">Entity Framework supporta le chiavi composte - chiavi primarie costituite da più di una proprietà.</span><span class="sxs-lookup"><span data-stu-id="ee55b-130">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="ee55b-131">Ad esempio, si potrebbe avere una classe Passport la cui chiave primaria è una combinazione di PassportNumber e IssuingCountry.</span><span class="sxs-lookup"><span data-stu-id="ee55b-131">For example, you could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

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

<span data-ttu-id="ee55b-132">Tentativo di utilizzare la classe precedente nel modello di EF comporterebbe un `InvalidOperationException`:</span><span class="sxs-lookup"><span data-stu-id="ee55b-132">Attempting to use the above class in your EF model would result in an `InvalidOperationException`:</span></span>

<span data-ttu-id="ee55b-133">*Impossibile determinare di composite primario ordinamento delle chiavi per il tipo 'Passport'. Usare il ColumnAttribute o il metodo HasKey per specificare un ordine per le chiavi primarie composte.*</span><span class="sxs-lookup"><span data-stu-id="ee55b-133">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="ee55b-134">Per usare le chiavi composte, Entity Framework è necessario definire un ordine per le proprietà della chiave.</span><span class="sxs-lookup"><span data-stu-id="ee55b-134">In order to use composite keys, Entity Framework requires you to define an order for the key properties.</span></span> <span data-ttu-id="ee55b-135">È possibile farlo utilizzando l'annotazione di colonna per specificare un ordine.</span><span class="sxs-lookup"><span data-stu-id="ee55b-135">You can do this by using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="ee55b-136">Il valore dell'ordine è relativo (anziché l'indice in base) in modo che eventuali valori possono essere usati.</span><span class="sxs-lookup"><span data-stu-id="ee55b-136">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="ee55b-137">Ad esempio, 100 e 200 sarebbe accettabile al posto di 1 e 2.</span><span class="sxs-lookup"><span data-stu-id="ee55b-137">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

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

<span data-ttu-id="ee55b-138">Se sono presenti entità con una chiave esterna composta, è necessario specificare la stessa colonna di ordinamento utilizzato per la proprietà di chiave primaria corrispondente.</span><span class="sxs-lookup"><span data-stu-id="ee55b-138">If you have entities with composite foreign keys, then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="ee55b-139">Solo l'ordinamento relativo all'interno di proprietà della chiave esterna deve essere lo stesso, i valori esatti assegnati a **ordine** non è necessario per la corrispondenza.</span><span class="sxs-lookup"><span data-stu-id="ee55b-139">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="ee55b-140">Ad esempio, nella classe seguente, 3 e 4 può essere usato al posto di 1 e 2.</span><span class="sxs-lookup"><span data-stu-id="ee55b-140">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

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

## <a name="required"></a><span data-ttu-id="ee55b-141">Obbligatorio</span><span class="sxs-lookup"><span data-stu-id="ee55b-141">Required</span></span>

<span data-ttu-id="ee55b-142">L'annotazione obbligatorio indica a EF che una particolare proprietà è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="ee55b-142">The Required annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="ee55b-143">Aggiunta di necessarie per la proprietà Title forzerà (Entity Framework e MVC) per garantire che la proprietà contiene i dati di.</span><span class="sxs-lookup"><span data-stu-id="ee55b-143">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="ee55b-144">Nessun senza ulteriori modifiche di codice o markup dell'applicazione, un'applicazione MVC eseguirà la convalida lato client, anche in modo dinamico la creazione di un messaggio utilizzando i nomi di proprietà e l'annotazione.</span><span class="sxs-lookup"><span data-stu-id="ee55b-144">With no additional no code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Creare una pagina con titolo è necessaria](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="ee55b-146">L'attributo obbligatorio influirà anche database generato, rendendo la proprietà mappata che non ammette valori null.</span><span class="sxs-lookup"><span data-stu-id="ee55b-146">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="ee55b-147">Si noti che il campo del titolo è stato modificato in "not null".</span><span class="sxs-lookup"><span data-stu-id="ee55b-147">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="ee55b-148">In alcuni casi potrebbe non essere possibile per la colonna nel database sia non nullable, anche se la proprietà è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="ee55b-148">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="ee55b-149">Ad esempio, quando tramite una data di strategia ereditarietà tabella per gerarchia per i tipi più viene archiviato in una singola tabella.</span><span class="sxs-lookup"><span data-stu-id="ee55b-149">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="ee55b-150">Se un tipo derivato include una proprietà obbligatoria la colonna può essere reso non nullable poiché non tutti i tipi nella gerarchia disporrà di questa proprietà.</span><span class="sxs-lookup"><span data-stu-id="ee55b-150">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![Blog su tabella](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="ee55b-152">MaxLength e MinLength</span><span class="sxs-lookup"><span data-stu-id="ee55b-152">MaxLength and MinLength</span></span>

<span data-ttu-id="ee55b-153">Gli attributi MaxLength e MinLength consentono di specificare le convalide di proprietà aggiuntive, come è stato fatto con obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="ee55b-153">The MaxLength and MinLength attributes allow you to specify additional property validations, just as you did with Required.</span></span>

<span data-ttu-id="ee55b-154">Ecco il BloggerName con i requisiti di lunghezza.</span><span class="sxs-lookup"><span data-stu-id="ee55b-154">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="ee55b-155">L'esempio illustra anche come combinare gli attributi.</span><span class="sxs-lookup"><span data-stu-id="ee55b-155">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="ee55b-156">L'annotazione di proprietà MaxLength influirà il database impostando la lunghezza della proprietà a 10.</span><span class="sxs-lookup"><span data-stu-id="ee55b-156">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![Tabella di blog con lunghezza massima in BloggerName colonna](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="ee55b-158">Annotazione sul lato client, MVC ed Entity Framework 4.1 lato server annotazione entrambi rispetterà la convalida, nuovamente in modo dinamico la creazione di un messaggio di errore: "il campo BloggerName deve essere un tipo stringa o matrice con una lunghezza massima di '10'." Tale messaggio è un po' lungo.</span><span class="sxs-lookup"><span data-stu-id="ee55b-158">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="ee55b-159">Molte annotazioni consentono di specificare un messaggio di errore con l'attributo di messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="ee55b-159">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="ee55b-160">È anche possibile specificare ErrorMessage nell'annotazione necessaria.</span><span class="sxs-lookup"><span data-stu-id="ee55b-160">You can also specify ErrorMessage in the Required annotation.</span></span>

![Creare una pagina con messaggio di errore personalizzato](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="ee55b-162">NotMapped</span><span class="sxs-lookup"><span data-stu-id="ee55b-162">NotMapped</span></span>

<span data-ttu-id="ee55b-163">Convenzione primo codice determina che tutte le proprietà di un tipo di dati supportati sono rappresentata nel database.</span><span class="sxs-lookup"><span data-stu-id="ee55b-163">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="ee55b-164">Ma ciò non sempre avveniva nelle applicazioni.</span><span class="sxs-lookup"><span data-stu-id="ee55b-164">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="ee55b-165">Ad esempio si potrebbe disporre di una proprietà nella classe del Blog che crea un codice in base ai campi titolo e BloggerName.</span><span class="sxs-lookup"><span data-stu-id="ee55b-165">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="ee55b-166">Tale proprietà può essere creata in modo dinamico e non devono essere archiviati.</span><span class="sxs-lookup"><span data-stu-id="ee55b-166">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="ee55b-167">È possibile contrassegnare le proprietà che non eseguono il mapping al database con l'annotazione NotMapped, ad esempio questa proprietà BlogCode.</span><span class="sxs-lookup"><span data-stu-id="ee55b-167">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

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

 

## <a name="complextype"></a><span data-ttu-id="ee55b-168">ComplexType</span><span class="sxs-lookup"><span data-stu-id="ee55b-168">ComplexType</span></span>

<span data-ttu-id="ee55b-169">Non è insolito per descrivere le entità di dominio in un set di classi e quindi di livello tali classi per descrivere un'entità completa.</span><span class="sxs-lookup"><span data-stu-id="ee55b-169">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="ee55b-170">Ad esempio, si potrebbe aggiungere una classe denominata BlogDetails al modello.</span><span class="sxs-lookup"><span data-stu-id="ee55b-170">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="ee55b-171">Si noti che BlogDetails non dispone di qualsiasi tipo di proprietà di chiave.</span><span class="sxs-lookup"><span data-stu-id="ee55b-171">Notice that BlogDetails does not have any type of key property.</span></span> <span data-ttu-id="ee55b-172">Nella progettazione basata su domini, BlogDetails si intende un oggetto valore.</span><span class="sxs-lookup"><span data-stu-id="ee55b-172">In domain driven design, BlogDetails is referred to as a value object.</span></span> <span data-ttu-id="ee55b-173">Entity Framework fa riferimento a oggetti valore come tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="ee55b-173">Entity Framework refers to value objects as complex types.</span></span>  <span data-ttu-id="ee55b-174">I tipi complessi non possono essere rilevati in modo indipendente.</span><span class="sxs-lookup"><span data-stu-id="ee55b-174">Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="ee55b-175">Tuttavia come proprietà nella classe Blog BlogDetails verrà rilevato come parte di un oggetto di Blog.</span><span class="sxs-lookup"><span data-stu-id="ee55b-175">However as a property in the Blog class, BlogDetails it will be tracked as part of a Blog object.</span></span> <span data-ttu-id="ee55b-176">Affinché code first per riconoscere questo, è necessario contrassegnare la classe BlogDetails come ComplexType.</span><span class="sxs-lookup"><span data-stu-id="ee55b-176">In order for code first to recognize this, you must mark the BlogDetails class as a ComplexType.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="ee55b-177">A questo punto è possibile aggiungere una proprietà nella classe Blog per rappresentare il BlogDetails per tale blog.</span><span class="sxs-lookup"><span data-stu-id="ee55b-177">Now you can add a property in the Blog class to represent the BlogDetails for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="ee55b-178">Nel database, la tabella Blog conterrà tutte le proprietà del blog incluse le proprietà contenute nella relativa proprietà BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="ee55b-178">In the database, the Blog table will contain all of the properties of the blog including the properties contained in its BlogDetail property.</span></span> <span data-ttu-id="ee55b-179">Per impostazione predefinita, ognuno di essi è preceduto con il nome del tipo complesso, BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="ee55b-179">By default, each one is preceded with the name of the complex type, BlogDetail.</span></span>

![Blog di tabella con tipi complessi](~/ef6/media/jj591583-figure06.png)

<span data-ttu-id="ee55b-181">Un'altra interessante ricordare è che anche se la proprietà DateCreated è stata definita come un valore DateTime che non ammette valori null nella classe, il campo di database corrispondente è nullable.</span><span class="sxs-lookup"><span data-stu-id="ee55b-181">Another interesting note is that although the DateCreated property was defined as a non-nullable DateTime in the class, the relevant database field is nullable.</span></span> <span data-ttu-id="ee55b-182">È necessario usare l'annotazione necessaria se si vuole interessano lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="ee55b-182">You must use the Required annotation if you wish to affect the database schema.</span></span>

 

## <a name="concurrencycheck"></a><span data-ttu-id="ee55b-183">ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="ee55b-183">ConcurrencyCheck</span></span>

<span data-ttu-id="ee55b-184">L'annotazione ConcurrencyCheck consente di contrassegnare una o più proprietà da utilizzare per controllo della concorrenza nel database quando un utente modifica o elimina un'entità.</span><span class="sxs-lookup"><span data-stu-id="ee55b-184">The ConcurrencyCheck annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="ee55b-185">Se Usa già con la finestra di progettazione di Entity Framework, questo viene allineato con l'impostazione ConcurrencyMode della proprietà su Fixed.</span><span class="sxs-lookup"><span data-stu-id="ee55b-185">If you've been working with the EF Designer, this aligns with setting a property's ConcurrencyMode to Fixed.</span></span>

<span data-ttu-id="ee55b-186">Di seguito viene illustrato il funzionamento ConcurrencyCheck aggiungendola alla proprietà BloggerName.</span><span class="sxs-lookup"><span data-stu-id="ee55b-186">Let’s see how ConcurrencyCheck works by adding it to the BloggerName property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="ee55b-187">Quando viene chiamato SaveChanges, a causa di annotazione ConcurrencyCheck sul campo BloggerName, il valore originale di tale proprietà verrà utilizzato nell'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="ee55b-187">When SaveChanges is called, because of the ConcurrencyCheck annotation on the BloggerName field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="ee55b-188">Il comando tenterà di individuare la riga corretta da un filtro non solo sul valore della chiave, ma anche sul valore originale di BloggerName.</span><span class="sxs-lookup"><span data-stu-id="ee55b-188">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of BloggerName.</span></span>  <span data-ttu-id="ee55b-189">Ecco le parti critiche del comando di aggiornamento inviate al database, in cui è possibile visualizzare il comando verrà aggiornata la riga contenente un PrimaryTrackingKey è 1 e un BloggerName di "Julie", ovvero il valore originale quando questo blog è stato recuperato dal database.</span><span class="sxs-lookup"><span data-stu-id="ee55b-189">Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a PrimaryTrackingKey is 1 and a BloggerName of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="ee55b-190">Se un utente è stato modificato il nome blogger per tale blog nel frattempo, questo aggiornamento avrà esito negativo e si otterrà un DbUpdateConcurrencyException che sarà necessario gestire.</span><span class="sxs-lookup"><span data-stu-id="ee55b-190">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a DbUpdateConcurrencyException that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="ee55b-191">TimeStamp</span><span class="sxs-lookup"><span data-stu-id="ee55b-191">TimeStamp</span></span>

<span data-ttu-id="ee55b-192">È più comune da usare i campi timestamp o rowversion per controllo della concorrenza.</span><span class="sxs-lookup"><span data-stu-id="ee55b-192">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="ee55b-193">Ma invece di usare l'annotazione ConcurrencyCheck, è possibile usare l'annotazione TimeStamp più specifico, purché il tipo della proprietà è una matrice di byte.</span><span class="sxs-lookup"><span data-stu-id="ee55b-193">But rather than using the ConcurrencyCheck annotation, you can use the more specific TimeStamp annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="ee55b-194">Codice prima di tutto considererà le proprietà di Timestamp uguale come proprietà ConcurrencyCheck, ma si assicurerà che il campo di database generata da codice prima di tutto è non nullable.</span><span class="sxs-lookup"><span data-stu-id="ee55b-194">Code first will treat Timestamp properties the same as ConcurrencyCheck properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="ee55b-195">È possibile avere solo una proprietà di timestamp in una determinata classe.</span><span class="sxs-lookup"><span data-stu-id="ee55b-195">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="ee55b-196">Aggiunta della proprietà seguente alla classe di Blog:</span><span class="sxs-lookup"><span data-stu-id="ee55b-196">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="ee55b-197">risultati nel codice creando innanzitutto una colonna timestamp non ammettono valori null nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="ee55b-197">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![Blog di tabella con colonna timestamp](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="ee55b-199">Tabelle e colonne</span><span class="sxs-lookup"><span data-stu-id="ee55b-199">Table and Column</span></span>

<span data-ttu-id="ee55b-200">Se si consente codice prima creare il database, è possibile modificare il nome delle tabelle e colonne che si sta creando.</span><span class="sxs-lookup"><span data-stu-id="ee55b-200">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="ee55b-201">È inoltre possibile utilizzare Code First con un database esistente.</span><span class="sxs-lookup"><span data-stu-id="ee55b-201">You can also use Code First with an existing database.</span></span> <span data-ttu-id="ee55b-202">Ma non sempre è il caso che i nomi delle classi e proprietà del dominio corrispondano ai nomi delle tabelle e colonne nel database.</span><span class="sxs-lookup"><span data-stu-id="ee55b-202">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="ee55b-203">La classe è denominata Blog e per convenzione, code presuppone prima di tutto che ciò verrà eseguito il mapping a una tabella denominata blog.</span><span class="sxs-lookup"><span data-stu-id="ee55b-203">My class is named Blog and by convention, code first presumes this will map to a table named Blogs.</span></span> <span data-ttu-id="ee55b-204">Se non è questo il caso è possibile specificare il nome della tabella con l'attributo di tabella.</span><span class="sxs-lookup"><span data-stu-id="ee55b-204">If that's not the case you can specify the name of the table with the Table attribute.</span></span> <span data-ttu-id="ee55b-205">In questo caso, ad esempio, l'annotazione è specificando che il nome della tabella è InternalBlogs.</span><span class="sxs-lookup"><span data-stu-id="ee55b-205">Here for example, the annotation is specifying that the table name is InternalBlogs.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="ee55b-206">L'annotazione di colonna è una TEPSA ulteriori nello specificare gli attributi di una colonna mappata.</span><span class="sxs-lookup"><span data-stu-id="ee55b-206">The Column annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="ee55b-207">È possibile stabilire un nome, tipo di dati o anche l'ordine in cui viene visualizzata una colonna nella tabella.</span><span class="sxs-lookup"><span data-stu-id="ee55b-207">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="ee55b-208">Ecco un esempio di attributo di colonna.</span><span class="sxs-lookup"><span data-stu-id="ee55b-208">Here is an example of the Column attribute.</span></span>

``` csharp
    [Column(“BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="ee55b-209">Non confondere TypeName (attributo) della colonna con il DataType DataAnnotation.</span><span class="sxs-lookup"><span data-stu-id="ee55b-209">Don’t confuse Column’s TypeName attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="ee55b-210">Tipo di dati è un'annotazione utilizzata per l'interfaccia utente e pertanto ignorata dalle Code First.</span><span class="sxs-lookup"><span data-stu-id="ee55b-210">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="ee55b-211">Ecco la tabella dopo che è stato rigenerato.</span><span class="sxs-lookup"><span data-stu-id="ee55b-211">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="ee55b-212">Il nome della tabella è stato modificato per InternalBlogs e colonna Descrizione dal tipo complesso è ora BlogDescription.</span><span class="sxs-lookup"><span data-stu-id="ee55b-212">The table name has changed to InternalBlogs and Description column from the complex type is now BlogDescription.</span></span> <span data-ttu-id="ee55b-213">Il nome specificato nell'annotazione, code first non utilizzerà la convenzione del nome della colonna che iniziano con il nome del tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="ee55b-213">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![Blog su tabelle e colonne rinominate](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="ee55b-215">DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="ee55b-215">DatabaseGenerated</span></span>

<span data-ttu-id="ee55b-216">Una funzionalità di database più importanti è la possibilità di disporre di proprietà calcolate.</span><span class="sxs-lookup"><span data-stu-id="ee55b-216">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="ee55b-217">Se si esegue il mapping di Code First alle classi di tabelle contenenti colonne calcolate, non si desidera che Entity Framework per tentare di aggiornare le colonne.</span><span class="sxs-lookup"><span data-stu-id="ee55b-217">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="ee55b-218">Ma si desidera restituire questi valori dal database dopo aver inserito o i dati aggiornati a Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ee55b-218">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="ee55b-219">È possibile utilizzare l'annotazione DatabaseGenerated per contrassegnare tutte le proprietà nella classe con l'enumerazione calcolata.</span><span class="sxs-lookup"><span data-stu-id="ee55b-219">You can use the DatabaseGenerated annotation to flag those properties in your class along with the Computed enum.</span></span> <span data-ttu-id="ee55b-220">Ne sono altre enumerazioni e identità.</span><span class="sxs-lookup"><span data-stu-id="ee55b-220">Other enums are None and Identity.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGenerationOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="ee55b-221">È possibile usare database generati nelle colonne timestamp o byte quando codice genera prima di tutto il database, in caso contrario, è consigliabile utilizzare solo questo posizionando i database esistenti perché codice prima di tutto sarà in grado di determinare la formula per la colonna calcolata.</span><span class="sxs-lookup"><span data-stu-id="ee55b-221">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="ee55b-222">Leggere sopra che per impostazione predefinita, una proprietà chiave che è un numero intero diventerà una chiave di identità nel database.</span><span class="sxs-lookup"><span data-stu-id="ee55b-222">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="ee55b-223">Sarebbe lo stesso come impostazione DatabaseGenerated su DatabaseGenerationOption.Identity.</span><span class="sxs-lookup"><span data-stu-id="ee55b-223">That would be the same as setting DatabaseGenerated to DatabaseGenerationOption.Identity.</span></span> <span data-ttu-id="ee55b-224">Se si desidera che non sia una chiave di identità, è possibile impostare il valore per DatabaseGenerationOption.None.</span><span class="sxs-lookup"><span data-stu-id="ee55b-224">If you do not want it to be an identity key, you can set the value to DatabaseGenerationOption.None.</span></span>

 

## <a name="index"></a><span data-ttu-id="ee55b-225">Indice</span><span class="sxs-lookup"><span data-stu-id="ee55b-225">Index</span></span>

> [!NOTE]
> <span data-ttu-id="ee55b-226">**EF6.1 e versioni successive solo** -attributo l'indice è stata introdotta in Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="ee55b-226">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="ee55b-227">Se si usa una versione precedente le informazioni contenute in questa sezione non si applicano.</span><span class="sxs-lookup"><span data-stu-id="ee55b-227">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="ee55b-228">È possibile creare un indice su una o più colonne usando il **IndexAttribute**.</span><span class="sxs-lookup"><span data-stu-id="ee55b-228">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="ee55b-229">Aggiunta dell'attributo a una o più proprietà verrà causare Entity Framework creare l'indice corrispondente nel database durante la creazione del database o eseguire lo scaffolding corrispondente **CreateIndex** chiama se si usa migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="ee55b-229">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="ee55b-230">Ad esempio, il codice seguente comporterà un indice viene creato per il **Rating** della colonna della **post** tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="ee55b-230">For example, the following code will result in an index being created on the **Rating** column of the **Posts** table in the database.</span></span>

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

<span data-ttu-id="ee55b-231">Per impostazione predefinita, l'indice verrà denominato **IX\_&lt;NomeProprietà&gt;**  (IX\_Rating nell'esempio precedente).</span><span class="sxs-lookup"><span data-stu-id="ee55b-231">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="ee55b-232">È anche possibile specificare un nome per l'indice tuttavia.</span><span class="sxs-lookup"><span data-stu-id="ee55b-232">You can also specify a name for the index though.</span></span> <span data-ttu-id="ee55b-233">L'esempio seguente specifica che l'indice deve essere denominata **PostRatingIndex**.</span><span class="sxs-lookup"><span data-stu-id="ee55b-233">The following example specifies that the index should be named **PostRatingIndex**.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="ee55b-234">Per impostazione predefinita, gli indici non sono univoche, ma è possibile usare la **IsUnique** denominato parametro per specificare che un indice deve essere univoco.</span><span class="sxs-lookup"><span data-stu-id="ee55b-234">By default, indexes are non-unique, but you can use the **IsUnique** named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="ee55b-235">Nell'esempio seguente illustra un indice univoco su un **utente**del nome di accesso.</span><span class="sxs-lookup"><span data-stu-id="ee55b-235">The following example introduces a unique index on a **User**'s login name.</span></span>

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

### <a name="multiple-column-indexes"></a><span data-ttu-id="ee55b-236">Indici più colonne</span><span class="sxs-lookup"><span data-stu-id="ee55b-236">Multiple-Column Indexes</span></span>

<span data-ttu-id="ee55b-237">Gli indici che si estendono su più colonne vengono specificati usando lo stesso nome in più annotazioni di indice per una determinata tabella.</span><span class="sxs-lookup"><span data-stu-id="ee55b-237">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="ee55b-238">Quando si creano indici su più colonne, è necessario specificare un ordine per le colonne nell'indice.</span><span class="sxs-lookup"><span data-stu-id="ee55b-238">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="ee55b-239">Ad esempio, il codice seguente crea un indice multicolonna sul **Rating** e **BlogId** chiamato **IX\_BlogAndRating**.</span><span class="sxs-lookup"><span data-stu-id="ee55b-239">For example, the following code creates a multi-column index on **Rating** and **BlogId** called **IX\_BlogAndRating**.</span></span> <span data-ttu-id="ee55b-240">**BlogId** è la prima colonna nell'indice e **Rating** è il secondo.</span><span class="sxs-lookup"><span data-stu-id="ee55b-240">**BlogId** is the first column in the index and **Rating** is the second.</span></span>

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

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="ee55b-241">Gli attributi di relazione: InverseProperty e perché ForeignKey</span><span class="sxs-lookup"><span data-stu-id="ee55b-241">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="ee55b-242">Questa pagina fornisce informazioni sulla configurazione di relazioni nel modello Code First con annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="ee55b-242">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="ee55b-243">Per informazioni generali sulle relazioni in Entity Framework e su come accedere e modificare dati tramite relazioni, vedere [relazioni di & proprietà di navigazione](~/ef6/fundamentals/relationships.md). \*</span><span class="sxs-lookup"><span data-stu-id="ee55b-243">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="ee55b-244">Convenzione primo codice si occuperà delle relazioni nel modello più comune, ma vi sono casi in cui è necessario supporto.</span><span class="sxs-lookup"><span data-stu-id="ee55b-244">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="ee55b-245">Modifica del nome della proprietà della chiave nella classe Blog creata un problema con le relazioni con i Post.</span><span class="sxs-lookup"><span data-stu-id="ee55b-245">Changing the name of the key property in the Blog class created a problem with its relationship to Post.</span></span> 

<span data-ttu-id="ee55b-246">Durante la generazione del database, codice innanzitutto rileva la proprietà BlogId nella classe Post e lo riconosce, per la convenzione che corrisponde a un nome e il "Id", come una chiave esterna per la classe di Blog.</span><span class="sxs-lookup"><span data-stu-id="ee55b-246">When generating the database, code first sees the BlogId property in the Post class and recognizes it, by the convention that it matches a class name plus “Id”, as a foreign key to the Blog class.</span></span> <span data-ttu-id="ee55b-247">Ma è presente nessuna proprietà BlogId nella classe blog.</span><span class="sxs-lookup"><span data-stu-id="ee55b-247">But there is no BlogId property in the blog class.</span></span> <span data-ttu-id="ee55b-248">La soluzione al problema consiste nel creare una proprietà di navigazione nel Post e usare il DataAnnotation esterna per consentire codice innanzitutto comprendere come creare la relazione tra le due classi, usando la proprietà Post.BlogId, nonché come specificare i vincoli nel database.</span><span class="sxs-lookup"><span data-stu-id="ee55b-248">The solution for this is to create a navigation property in the Post and use the Foreign DataAnnotation to help code first understand how to build the relationship between the two classes —using the Post.BlogId property — as well as how to specify constraints in the database.</span></span>

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

<span data-ttu-id="ee55b-249">Il vincolo nel database Mostra una relazione tra InternalBlogs.PrimaryTrackingKey e Posts.BlogId.</span><span class="sxs-lookup"><span data-stu-id="ee55b-249">The constraint in the database shows a relationship between InternalBlogs.PrimaryTrackingKey and Posts.BlogId.</span></span> 

![relazione tra InternalBlogs.PrimaryTrackingKey e Posts.BlogId](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="ee55b-251">Il InverseProperty viene usato quando si dispone di più relazioni tra classi.</span><span class="sxs-lookup"><span data-stu-id="ee55b-251">The InverseProperty is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="ee55b-252">Nella classe Post, è possibile tenere traccia di chi ha scritto un post di blog, oltre che ha modificata per ultimo.</span><span class="sxs-lookup"><span data-stu-id="ee55b-252">In the Post class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="ee55b-253">Ecco due nuove proprietà di navigazione per la classe Post.</span><span class="sxs-lookup"><span data-stu-id="ee55b-253">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="ee55b-254">È anche necessario aggiungere nella classe di persona a cui fanno riferimento tali proprietà.</span><span class="sxs-lookup"><span data-stu-id="ee55b-254">You’ll also need to add in the Person class referenced by these properties.</span></span> <span data-ttu-id="ee55b-255">La classe Person presenta le proprietà di navigazione indietro al Post, uno per tutti i post scritti dall'utente e una per tutti i post aggiornati da questa persona.</span><span class="sxs-lookup"><span data-stu-id="ee55b-255">The Person class has navigation properties back to the Post, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="ee55b-256">Prima di tutto codice non è in grado di associare le proprietà, le due classi di per sé.</span><span class="sxs-lookup"><span data-stu-id="ee55b-256">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="ee55b-257">La tabella di database per i post deve avere una chiave esterna per la persona del CreatedBy e uno per la persona UpdatedBy ma il codice innanzitutto creare quattro proprietà di chiave esterna verrà: persona\_Id, Person\_Id1, CreatedBy\_Id e UpdatedBy\_ID.</span><span class="sxs-lookup"><span data-stu-id="ee55b-257">The database table for Posts should have one foreign key for the CreatedBy person and one for the UpdatedBy person but code first will create four will foreign key properties: Person\_Id, Person\_Id1, CreatedBy\_Id and UpdatedBy\_Id.</span></span>

![Post di tabella con le chiavi esterne aggiuntive](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="ee55b-259">Per risolvere questi problemi, è possibile utilizzare l'annotazione InverseProperty per specificare l'allineamento delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="ee55b-259">To fix these problems, you can use the InverseProperty annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="ee55b-260">Poiché la proprietà PostsWritten di persona riconosce che si riferisce al tipo di Post, compilerà la relazione di Post.CreatedBy.</span><span class="sxs-lookup"><span data-stu-id="ee55b-260">Because the PostsWritten property in Person knows that this refers to the Post type, it will build the relationship to Post.CreatedBy.</span></span> <span data-ttu-id="ee55b-261">Analogamente, verrà connessa a Post.UpdatedBy PostsUpdated.</span><span class="sxs-lookup"><span data-stu-id="ee55b-261">Similarly, PostsUpdated will be connected to Post.UpdatedBy.</span></span> <span data-ttu-id="ee55b-262">E il codice prima di tutto non crea le chiavi esterne aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="ee55b-262">And code first will not create the extra foreign keys.</span></span>

![Post di tabelle senza chiavi esterne aggiuntive](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="ee55b-264">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="ee55b-264">Summary</span></span>

<span data-ttu-id="ee55b-265">DataAnnotations consentono non solo di descrivere la convalida sul lato client e server nelle classi prima del codice, ma consentono anche di migliorare e anche correggere le ipotesi che codice prima di tutto renderà sulle classi base la convenzioni.</span><span class="sxs-lookup"><span data-stu-id="ee55b-265">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="ee55b-266">Con DataAnnotations non è possibile richiedere solo la generazione dello schema di database, ma è anche possibile mappare le classi di code first per un database preesistente.</span><span class="sxs-lookup"><span data-stu-id="ee55b-266">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="ee55b-267">Mentre sono molto flessibile, tenere presente che DataAnnotations forniscono solo più comunemente necessarie modifiche alla configurazione che è possibile apportare le classi di code first.</span><span class="sxs-lookup"><span data-stu-id="ee55b-267">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="ee55b-268">Per configurare le classi per alcune dei casi limite, dovrebbe essere al meccanismo di configurazione alternativa API Fluent di Code First.</span><span class="sxs-lookup"><span data-stu-id="ee55b-268">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>
