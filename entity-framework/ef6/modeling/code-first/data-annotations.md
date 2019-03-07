---
title: Annotazioni dei dati First - EF6 del codice
author: divega
ms.date: 10/23/2016
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: e6b017306b4f66f5bac2a9964e11391da28ceb40
ms.sourcegitcommit: a013e243a14f384999ceccaf9c779b8c1ae3b936
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2019
ms.locfileid: "57463282"
---
# <a name="code-first-data-annotations"></a>Annotazioni dei dati per Code First
> [!NOTE]
> **EF4.1 e versioni successive solo** -le funzionalità, le API, e così via illustrati in questa pagina sono stati introdotti in Entity Framework 4.1. Se si usa una versione precedente, alcune o tutte queste informazioni non è applicabile.

Il contenuto in questa pagina è stato adattato da un articolo scritto originariamente di Julie Lerman (\<http://thedatafarm.com>).

Code First di Entity Framework consente di usare le proprie classi di dominio per rappresentare il modello di Entity Framework si basa su per eseguire una query, modifica di rilevamento e l'aggiornamento di funzioni. Codice prima di tutto si basa su un modello di programmazione definito come 'convention over configuration'. Codice prima di tutto presupporrà che seguono le convenzioni di Entity Framework le classi e in tal caso, verranno attivati automaticamente su come eseguire il processo. Tuttavia, se le classi non si seguono tali convenzioni, hai la possibilità di aggiungere le configurazioni alle classi per fornire Entity Framework con le informazioni necessarie.

Codice prima di tutto offre due modi per aggiungere queste configurazioni alle classi. Uno è con attributi semplici chiamati DataAnnotations e il secondo Usa API del Code First Fluent, che offre un modo per descrivere le configurazioni in modo imperativo nel codice.

Questo articolo è incentrato sull'uso di DataAnnotations (nello spazio dei nomi System.ComponentModel.DataAnnotations) per configurare classi, evidenziando le configurazioni più comunemente necessari. DataAnnotations vengono riconosciuti anche da un numero di applicazioni .NET, ad esempio ASP.NET MVC che consente a queste applicazioni di sfruttare le stesse annotazioni per le convalide sul lato client.


## <a name="the-model"></a>Il modello

Illustrerò DataAnnotations prima del codice con una semplice coppia di classi: Post di blog e Post.

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

Così come sono, le classi di Blog e Post seguono la convenzione prima di codice in modo facile e non richiedono modifiche specifiche per abilitare la compatibilità di Entity Framework. Tuttavia, è possibile utilizzare anche le annotazioni per a Entity Framework vengono fornite ulteriori informazioni sulle classi e il database a cui eseguono il mapping.

 

## <a name="key"></a>Chiave

Entity Framework si basa su tutte le entità con un valore di chiave utilizzata per l'entità di rilevamento. Una convenzione di Code First è proprietà chiave implicita. Codice cerca innanzitutto una proprietà denominata "Id", o una combinazione di nome di classe e "Id", ad esempio "BlogId". Questa proprietà verrà eseguito il mapping a una colonna chiave primaria nel database.

Le classi post di Blog e Post seguono questa convenzione. Cosa accade se non si ha? Cosa accade se Blog usato il nome *PrimaryTrackingKey* invece, o addirittura *foo*? Se il codice prima di tutto non trova una proprietà che corrisponde a questa convenzione genererà un'eccezione a causa di requisiti di Entity Framework è necessario che una proprietà chiave. È possibile utilizzare l'annotazione chiave per specificare quale proprietà deve essere usata come EntityKey.

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

Se si è l'uso di codice prima di tutto è la funzionalità di generazione del database, la tabella Blog avrà una colonna chiave primaria denominata PrimaryTrackingKey, che è definito anche come identità per impostazione predefinita.

![Blog di tabella con la chiave primaria](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a>Chiavi composte

Entity Framework supporta le chiavi composte - chiavi primarie costituite da più di una proprietà. Ad esempio, si potrebbe avere una classe Passport la cui chiave primaria è una combinazione di PassportNumber e IssuingCountry.

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

Tentativo di utilizzare la classe precedente nel modello di EF comporterebbe un `InvalidOperationException`:

*Impossibile determinare di composite primario ordinamento delle chiavi per il tipo 'Passport'. Usare il ColumnAttribute o il metodo HasKey per specificare un ordine per le chiavi primarie composte.*

Per usare le chiavi composte, Entity Framework è necessario definire un ordine per le proprietà della chiave. È possibile farlo utilizzando l'annotazione di colonna per specificare un ordine.

>[!NOTE]
> Il valore dell'ordine è relativo (anziché l'indice in base) in modo che eventuali valori possono essere usati. Ad esempio, 100 e 200 sarebbe accettabile al posto di 1 e 2.

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

Se sono presenti entità con una chiave esterna composta, è necessario specificare la stessa colonna di ordinamento utilizzato per la proprietà di chiave primaria corrispondente.

Solo l'ordinamento relativo all'interno di proprietà della chiave esterna deve essere lo stesso, i valori esatti assegnati a **ordine** non è necessario per la corrispondenza. Ad esempio, nella classe seguente, 3 e 4 può essere usato al posto di 1 e 2.

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

## <a name="required"></a>Obbligatorio

L'annotazione obbligatorio indica a EF che una particolare proprietà è obbligatoria.

Aggiunta di necessarie per la proprietà Title forzerà (Entity Framework e MVC) per garantire che la proprietà contiene i dati di.

``` csharp
    [Required]
    public string Title { get; set; }
```

Nessun senza ulteriori modifiche di codice o markup dell'applicazione, un'applicazione MVC eseguirà la convalida lato client, anche in modo dinamico la creazione di un messaggio utilizzando i nomi di proprietà e l'annotazione.

![Creare una pagina con titolo è necessaria](~/ef6/media/jj591583-figure02.png)

L'attributo obbligatorio influirà anche database generato, rendendo la proprietà mappata che non ammette valori null. Si noti che il campo del titolo è stato modificato in "not null".

>[!NOTE]
> In alcuni casi potrebbe non essere possibile per la colonna nel database sia non nullable, anche se la proprietà è obbligatoria. Ad esempio, quando tramite una data di strategia ereditarietà tabella per gerarchia per i tipi più viene archiviato in una singola tabella. Se un tipo derivato include una proprietà obbligatoria la colonna può essere reso non nullable poiché non tutti i tipi nella gerarchia disporrà di questa proprietà.

 

![Blog su tabella](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a>MaxLength e MinLength

Gli attributi MaxLength e MinLength consentono di specificare le convalide di proprietà aggiuntive, come è stato fatto con obbligatorio.

Ecco il BloggerName con i requisiti di lunghezza. L'esempio illustra anche come combinare gli attributi.

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

L'annotazione di proprietà MaxLength influirà il database impostando la lunghezza della proprietà a 10.

![Tabella di blog con lunghezza massima in BloggerName colonna](~/ef6/media/jj591583-figure04.png)

Annotazione sul lato client, MVC ed Entity Framework 4.1 lato server annotazione entrambi rispetta questa convalida, nuovamente in modo dinamico la creazione di un messaggio di errore: "Il campo BloggerName deve essere un tipo stringa o matrice con una lunghezza massima di '10'." Tale messaggio è un po' lungo. Molte annotazioni consentono di specificare un messaggio di errore con l'attributo di messaggio di errore.

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

È anche possibile specificare ErrorMessage nell'annotazione necessaria.

![Creare una pagina con messaggio di errore personalizzato](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a>NotMapped

Convenzione primo codice determina che tutte le proprietà di un tipo di dati supportati sono rappresentata nel database. Ma ciò non sempre avveniva nelle applicazioni. Ad esempio si potrebbe disporre di una proprietà nella classe del Blog che crea un codice in base ai campi titolo e BloggerName. Tale proprietà può essere creata in modo dinamico e non devono essere archiviati. È possibile contrassegnare le proprietà che non eseguono il mapping al database con l'annotazione NotMapped, ad esempio questa proprietà BlogCode.

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

 

## <a name="complextype"></a>ComplexType

Non è insolito per descrivere le entità di dominio in un set di classi e quindi di livello tali classi per descrivere un'entità completa. Ad esempio, si potrebbe aggiungere una classe denominata BlogDetails al modello.

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Si noti che BlogDetails non dispone di qualsiasi tipo di proprietà di chiave. Nella progettazione basata su domini, BlogDetails si intende un oggetto valore. Entity Framework fa riferimento a oggetti valore come tipi complessi.  I tipi complessi non possono essere rilevati in modo indipendente.

Tuttavia come proprietà nella classe Blog BlogDetails verrà rilevato come parte di un oggetto di Blog. Affinché code first per riconoscere questo, è necessario contrassegnare la classe BlogDetails come ComplexType.

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

A questo punto è possibile aggiungere una proprietà nella classe Blog per rappresentare il BlogDetails per tale blog.

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

Nel database, la tabella Blog conterrà tutte le proprietà del blog incluse le proprietà contenute nella relativa proprietà BlogDetail. Per impostazione predefinita, ognuno di essi è preceduto con il nome del tipo complesso, BlogDetail.

![Blog di tabella con tipi complessi](~/ef6/media/jj591583-figure06.png)


## <a name="concurrencycheck"></a>ConcurrencyCheck

L'annotazione ConcurrencyCheck consente di contrassegnare una o più proprietà da utilizzare per controllo della concorrenza nel database quando un utente modifica o elimina un'entità. Se Usa già con la finestra di progettazione di Entity Framework, questo viene allineato con l'impostazione ConcurrencyMode della proprietà su Fixed.

Di seguito viene illustrato il funzionamento ConcurrencyCheck aggiungendola alla proprietà BloggerName.

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

Quando viene chiamato SaveChanges, a causa di annotazione ConcurrencyCheck sul campo BloggerName, il valore originale di tale proprietà verrà utilizzato nell'aggiornamento. Il comando tenterà di individuare la riga corretta da un filtro non solo sul valore della chiave, ma anche sul valore originale di BloggerName.  Ecco le parti critiche del comando di aggiornamento inviate al database, in cui è possibile visualizzare il comando verrà aggiornata la riga contenente un PrimaryTrackingKey è 1 e un BloggerName di "Julie", ovvero il valore originale quando questo blog è stato recuperato dal database.

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

Se un utente è stato modificato il nome blogger per tale blog nel frattempo, questo aggiornamento avrà esito negativo e si otterrà un DbUpdateConcurrencyException che sarà necessario gestire.

 

## <a name="timestamp"></a>TimeStamp

È più comune da usare i campi timestamp o rowversion per controllo della concorrenza. Ma invece di usare l'annotazione ConcurrencyCheck, è possibile usare l'annotazione TimeStamp più specifico, purché il tipo della proprietà è una matrice di byte. Codice prima di tutto considererà le proprietà di Timestamp uguale come proprietà ConcurrencyCheck, ma si assicurerà che il campo di database generata da codice prima di tutto è non nullable. È possibile avere solo una proprietà di timestamp in una determinata classe.

Aggiunta della proprietà seguente alla classe di Blog:

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

risultati nel codice creando innanzitutto una colonna timestamp non ammettono valori null nella tabella di database.

![Blog di tabella con colonna timestamp](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a>Tabelle e colonne

Se si consente codice prima creare il database, è possibile modificare il nome delle tabelle e colonne che si sta creando. È inoltre possibile utilizzare Code First con un database esistente. Ma non sempre è il caso che i nomi delle classi e proprietà del dominio corrispondano ai nomi delle tabelle e colonne nel database.

La classe è denominata Blog e per convenzione, code presuppone prima di tutto che ciò verrà eseguito il mapping a una tabella denominata blog. Se non è questo il caso è possibile specificare il nome della tabella con l'attributo di tabella. In questo caso, ad esempio, l'annotazione è specificando che il nome della tabella è InternalBlogs.

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

L'annotazione di colonna è una TEPSA ulteriori nello specificare gli attributi di una colonna mappata. È possibile stabilire un nome, tipo di dati o anche l'ordine in cui viene visualizzata una colonna nella tabella. Ecco un esempio di attributo di colonna.

``` csharp
    [Column("BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

Non confondere TypeName (attributo) della colonna con il DataType DataAnnotation. Tipo di dati è un'annotazione utilizzata per l'interfaccia utente e pertanto ignorata dalle Code First.

Ecco la tabella dopo che è stato rigenerato. Il nome della tabella è stato modificato per InternalBlogs e colonna Descrizione dal tipo complesso è ora BlogDescription. Il nome specificato nell'annotazione, code first non utilizzerà la convenzione del nome della colonna che iniziano con il nome del tipo complesso.

![Blog su tabelle e colonne rinominate](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a>DatabaseGenerated

Una funzionalità di database più importanti è la possibilità di disporre di proprietà calcolate. Se si esegue il mapping di Code First alle classi di tabelle contenenti colonne calcolate, non si desidera che Entity Framework per tentare di aggiornare le colonne. Ma si desidera restituire questi valori dal database dopo aver inserito o i dati aggiornati a Entity Framework. È possibile utilizzare l'annotazione DatabaseGenerated per contrassegnare tutte le proprietà nella classe con l'enumerazione calcolata. Ne sono altre enumerazioni e identità.

``` csharp
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public DateTime DateCreated { get; set; }
```

È possibile usare database generati nelle colonne timestamp o byte quando codice genera prima di tutto il database, in caso contrario, è consigliabile utilizzare solo questo posizionando i database esistenti perché codice prima di tutto sarà in grado di determinare la formula per la colonna calcolata.

Leggere sopra che per impostazione predefinita, una proprietà chiave che è un numero intero diventerà una chiave di identità nel database. Sarebbe lo stesso come impostazione DatabaseGenerated su DatabaseGeneratedOption.Identity. Se si desidera che non sia una chiave di identità, è possibile impostare il valore per DatabaseGeneratedOption.None.

 

## <a name="index"></a>Indice

> [!NOTE]
> **EF6.1 e versioni successive solo** -attributo l'indice è stata introdotta in Entity Framework 6.1. Se si usa una versione precedente le informazioni contenute in questa sezione non si applicano.

È possibile creare un indice su una o più colonne usando il **IndexAttribute**. Aggiunta dell'attributo a una o più proprietà verrà causare Entity Framework creare l'indice corrispondente nel database durante la creazione del database o eseguire lo scaffolding corrispondente **CreateIndex** chiama se si usa migrazioni Code First.

Ad esempio, il codice seguente comporterà un indice viene creato per il **Rating** della colonna della **post** tabella nel database.

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

Per impostazione predefinita, l'indice verrà denominato **IX\_&lt;NomeProprietà&gt;**  (IX\_Rating nell'esempio precedente). È anche possibile specificare un nome per l'indice tuttavia. L'esempio seguente specifica che l'indice deve essere denominata **PostRatingIndex**.

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

Per impostazione predefinita, gli indici non sono univoche, ma è possibile usare la **IsUnique** denominato parametro per specificare che un indice deve essere univoco. Nell'esempio seguente illustra un indice univoco su un **utente**del nome di accesso.

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

### <a name="multiple-column-indexes"></a>Indici più colonne

Gli indici che si estendono su più colonne vengono specificati usando lo stesso nome in più annotazioni di indice per una determinata tabella. Quando si creano indici su più colonne, è necessario specificare un ordine per le colonne nell'indice. Ad esempio, il codice seguente crea un indice multicolonna sul **Rating** e **BlogId** chiamato **IX\_BlogIdAndRating**. **BlogId** è la prima colonna nell'indice e **Rating** è il secondo.

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

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a>Attributi di relazione: InverseProperty e perché ForeignKey

> [!NOTE]
> Questa pagina fornisce informazioni sulla configurazione di relazioni nel modello Code First con annotazioni dei dati. Per informazioni generali sulle relazioni in Entity Framework e su come accedere e modificare dati tramite relazioni, vedere [relazioni di & proprietà di navigazione](~/ef6/fundamentals/relationships.md). *

Convenzione primo codice si occuperà delle relazioni nel modello più comune, ma vi sono casi in cui è necessario supporto.

Modifica del nome della proprietà della chiave nella classe Blog creata un problema con le relazioni con i Post. 

Durante la generazione del database, codice innanzitutto rileva la proprietà BlogId nella classe Post e lo riconosce, per la convenzione che corrisponde a un nome e il "Id", come una chiave esterna per la classe di Blog. Ma è presente nessuna proprietà BlogId nella classe blog. La soluzione al problema consiste nel creare una proprietà di navigazione nel Post e usare il DataAnnotation esterna per consentire codice innanzitutto comprendere come creare la relazione tra le due classi, usando la proprietà Post.BlogId, nonché come specificare i vincoli nel database.

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

Il vincolo nel database Mostra una relazione tra InternalBlogs.PrimaryTrackingKey e Posts.BlogId. 

![relazione tra InternalBlogs.PrimaryTrackingKey e Posts.BlogId](~/ef6/media/jj591583-figure09.png)

Il InverseProperty viene usato quando si dispone di più relazioni tra classi.

Nella classe Post, è possibile tenere traccia di chi ha scritto un post di blog, oltre che ha modificata per ultimo. Ecco due nuove proprietà di navigazione per la classe Post.

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

È anche necessario aggiungere nella classe di persona a cui fanno riferimento tali proprietà. La classe Person presenta le proprietà di navigazione indietro al Post, uno per tutti i post scritti dall'utente e una per tutti i post aggiornati da questa persona.

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

Prima di tutto codice non è in grado di associare le proprietà, le due classi di per sé. La tabella di database per i post deve avere una chiave esterna per la persona del CreatedBy e una per persona UpdatedBy ma prima di tutto il codice creerà quattro proprietà di chiave esterna: Persona\_Id, Person\_Id1, CreatedBy\_Id e UpdatedBy\_ID.

![Post di tabella con le chiavi esterne aggiuntive](~/ef6/media/jj591583-figure10.png)

Per risolvere questi problemi, è possibile utilizzare l'annotazione InverseProperty per specificare l'allineamento delle proprietà.

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

Poiché la proprietà PostsWritten di persona riconosce che si riferisce al tipo di Post, compilerà la relazione di Post.CreatedBy. Analogamente, verrà connessa a Post.UpdatedBy PostsUpdated. E il codice prima di tutto non crea le chiavi esterne aggiuntive.

![Post di tabelle senza chiavi esterne aggiuntive](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a>Riepilogo

DataAnnotations consentono non solo di descrivere la convalida sul lato client e server nelle classi prima del codice, ma consentono anche di migliorare e anche correggere le ipotesi che codice prima di tutto renderà sulle classi base la convenzioni. Con DataAnnotations non è possibile richiedere solo la generazione dello schema di database, ma è anche possibile mappare le classi di code first per un database preesistente.

Mentre sono molto flessibile, tenere presente che DataAnnotations forniscono solo più comunemente necessarie modifiche alla configurazione che è possibile apportare le classi di code first. Per configurare le classi per alcune dei casi limite, dovrebbe essere al meccanismo di configurazione alternativa API Fluent di Code First.
