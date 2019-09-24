---
title: Relazioni-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 1e9c62bec47263ef452c7ac425a0bb446f9371d8
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197645"
---
# <a name="relationships"></a>Relazioni

Una relazione definisce il modo in cui due entità sono correlate tra loro. In un database relazionale, questo è rappresentato da un vincolo FOREIGN KEY.

> [!NOTE]  
> La maggior parte degli esempi in questo articolo usa una relazione uno-a-molti per illustrare i concetti. Per esempi di relazioni uno-a-uno e molti-a-molti, vedere la sezione [altri modelli di relazione](#other-relationship-patterns) alla fine dell'articolo.

## <a name="definition-of-terms"></a>Definizione dei termini

Esistono diversi termini usati per descrivere le relazioni

* **Entità dipendente:** Si tratta dell'entità che contiene le proprietà di chiave esterna. Noto anche come ' Child ' della relazione.

* **Entità principale:** Si tratta dell'entità che contiene le proprietà della chiave primaria/alternativa. Noto anche come ' Parent ' della relazione.

* **Chiave esterna:** Proprietà nell'entità dipendente utilizzata per archiviare i valori della proprietà della chiave principale a cui è correlata l'entità.

* **Chiave principale:** Proprietà che identifica in modo univoco l'entità principale. Può trattarsi della chiave primaria o di una chiave alternativa.

* **Proprietà di navigazione:** Proprietà definita nell'entità dipendente e/o dipendente che contiene uno o più riferimenti alle entità correlate.

  * **Proprietà di navigazione della raccolta:** Proprietà di navigazione che contiene riferimenti a molte entità correlate.

  * **Proprietà di navigazione riferimento:** Proprietà di navigazione che include un riferimento a una singola entità correlata.

  * **Proprietà di navigazione inversa:** Quando si discute una particolare proprietà di navigazione, questo termine fa riferimento alla proprietà di navigazione nell'altra entità finale della relazione.

Nel listato di codice seguente viene illustrata una relazione uno- `Blog` a-molti tra e`Post`

* `Post`è l'entità dipendente

* `Blog`è l'entità principale

* `Post.BlogId`chiave esterna

* `Blog.BlogId`chiave principale (in questo caso è una chiave primaria anziché una chiave alternativa)

* `Post.Blog`è una proprietà di navigazione di riferimento

* `Blog.Posts`è una proprietà di navigazione della raccolta

* `Post.Blog`Proprietà di navigazione inversa di `Blog.Posts` (e viceversa)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Convenzioni

Per convenzione, viene creata una relazione quando viene individuata una proprietà di navigazione su un tipo. Una proprietà viene considerata una proprietà di navigazione se il tipo a cui punta non può essere mappato come tipo scalare dal provider di database corrente.

> [!NOTE]  
> Le relazioni individuate per convenzione saranno sempre destinate alla chiave primaria dell'entità principale. Per fare riferimento a una chiave alternativa, è necessario eseguire una configurazione aggiuntiva usando l'API Fluent.

### <a name="fully-defined-relationships"></a>Relazioni completamente definite

Il modello più comune per le relazioni prevede che le proprietà di navigazione siano definite in entrambe le estremità della relazione e in una proprietà di chiave esterna definita nella classe di entità dipendente.

* Se tra due tipi viene trovata una coppia di proprietà di navigazione, queste verranno configurate come proprietà di navigazione inversa della stessa relazione.

* Se l'entità dipendente contiene una proprietà denominata `<primary key property name>`, `<navigation property name><primary key property name>`, o `<principal entity name><primary key property name>` verrà configurata come chiave esterna.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Se sono presenti più proprietà di navigazione definite tra due tipi, ovvero più di una coppia distinta di spostamenti che puntano l'uno all'altro, non verrà creata alcuna relazione per convenzione e sarà necessario configurarle manualmente per identificare il modo in cui coppia proprietà di navigazione.

### <a name="no-foreign-key-property"></a>Nessuna proprietà di chiave esterna

Sebbene sia consigliabile disporre di una proprietà di chiave esterna definita nella classe di entità dipendente, non è obbligatorio. Se non viene trovata alcuna proprietà di chiave esterna, verrà introdotta una proprietà di chiave esterna Shadow `<navigation property name><principal key property name>` con il nome. per ulteriori informazioni, vedere [proprietà shadow](shadow-properties.md) .

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Proprietà di navigazione singola

Per includere una relazione definita per convenzione, è sufficiente includere una sola proprietà di navigazione (nessuna navigazione inversa e nessuna proprietà di chiave esterna). È inoltre possibile disporre di una sola proprietà di navigazione e di una proprietà di chiave esterna.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Eliminazione a catena

Per convenzione, CASCADE DELETE verrà impostato su *Cascade* per le relazioni obbligatorie e *ClientSetNull* per le relazioni facoltative. *Cascade* significa che vengono eliminate anche le entità dipendenti. *ClientSetNull* indica che le entità dipendenti che non vengono caricate in memoria rimarranno invariate e dovranno essere eliminate manualmente o aggiornate in modo da puntare a un'entità principale valida. Per le entità caricate in memoria, EF Core tenterà di impostare le proprietà di chiave esterna su null.

Per la differenza tra le relazioni obbligatorie e facoltative, vedere la sezione [relazioni obbligatorie e facoltative](#required-and-optional-relationships) .

Per ulteriori informazioni sui diversi comportamenti di eliminazione e sulle impostazioni predefinite utilizzate dalla convenzione, vedere [CASCADE DELETE](../saving/cascade-delete.md) .

## <a name="data-annotations"></a>Annotazioni dei dati

Sono disponibili due annotazioni dei dati che possono essere utilizzate per configurare `[ForeignKey]` le `[InverseProperty]`relazioni, e. Sono disponibili nello `System.ComponentModel.DataAnnotations.Schema` spazio dei nomi.

### <a name="foreignkey"></a>ForeignKey

È possibile utilizzare le annotazioni dei dati per configurare quale proprietà deve essere utilizzata come proprietà di chiave esterna per una determinata relazione. Questa operazione viene in genere eseguita quando la proprietà di chiave esterna non viene individuata per convenzione.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> L' `[ForeignKey]` annotazione può essere posizionata su una proprietà di navigazione nella relazione. Non è necessario che venga eseguita la proprietà di navigazione nella classe di entità dipendente.

### <a name="inverseproperty"></a>[InverseProperty]

È possibile utilizzare le annotazioni dei dati per configurare la modalità di associazione delle proprietà di navigazione nelle entità dipendenti e entità. Questa operazione viene in genere eseguita quando è presente più di una coppia di proprietà di navigazione tra due tipi di entità.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a>API Fluent

Per configurare una relazione nell'API Fluent, è necessario innanzitutto identificare le proprietà di navigazione che compongono la relazione. `HasOne`o `HasMany` identifica la proprietà di navigazione nel tipo di entità in cui si sta iniziando la configurazione. Quindi si concatenano una `WithOne` chiamata `WithMany` a o per identificare la navigazione inversa. `HasOne`/`WithOne`vengono utilizzati per le proprietà di navigazione `HasMany` di riferimento e vengono utilizzati per le / `WithMany` proprietà di navigazione della raccolta.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a>Proprietà di navigazione singola

Se è presente una sola proprietà di navigazione, sono presenti overload senza parametri di `WithOne` e. `WithMany` Ciò indica che è concettualmente presente un riferimento o una raccolta nell'altra entità finale della relazione, ma nella classe di entità non è inclusa alcuna proprietà di navigazione.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a>Chiave esterna

È possibile utilizzare l'API Fluent per configurare quale proprietà deve essere utilizzata come proprietà di chiave esterna per una determinata relazione.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

Nel listato di codice seguente viene illustrato come configurare una chiave esterna composta.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

È possibile utilizzare l'overload di stringa `HasForeignKey(...)` di per configurare una proprietà shadow come chiave esterna. per ulteriori informazioni, vedere [proprietà shadow](shadow-properties.md) . È consigliabile aggiungere esplicitamente la proprietà shadow al modello prima di utilizzarla come chiave esterna, come illustrato di seguito.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a>Senza proprietà di navigazione

Non è necessario necessariamente fornire una proprietà di navigazione. È possibile specificare semplicemente una chiave esterna su un lato della relazione.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a>Chiave principale

Se si desidera che la chiave esterna faccia riferimento a una proprietà diversa dalla chiave primaria, è possibile utilizzare l'API Fluent per configurare la proprietà chiave principale per la relazione. La proprietà configurata come chiave principale verrà automaticamente impostata come chiave alternativa. per ulteriori informazioni, vedere [chiavi alternative](alternate-keys.md) .

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

Nel listato di codice seguente viene illustrato come configurare una chiave principale composta.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> L'ordine in cui si specificano le proprietà della chiave principale deve corrispondere all'ordine in cui sono specificate per la chiave esterna.

### <a name="required-and-optional-relationships"></a>Relazioni obbligatorie e facoltative

È possibile usare l'API Fluent per configurare se la relazione è obbligatoria o facoltativa. In definitiva, controlla se la proprietà della chiave esterna è obbligatoria o facoltativa. Questa operazione è particolarmente utile quando si usa una chiave esterna dello stato di ombreggiatura. Se nella classe di entità è presente una proprietà di chiave esterna, la richiesta della relazione viene determinata a seconda che la proprietà della chiave esterna sia obbligatoria o facoltativa (per ulteriori informazioni, vedere [proprietà obbligatorie e facoltative](required-optional.md) ).

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
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

### <a name="cascade-delete"></a>Eliminazione a catena

È possibile usare l'API Fluent per configurare in modo esplicito il comportamento di eliminazione a catena per una determinata relazione.

Vedere [CASCADE DELETE](../saving/cascade-delete.md) nella sezione Saving data per una descrizione dettagliata di ogni opzione.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .OnDelete(DeleteBehavior.Cascade);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a>Altri modelli di relazione

### <a name="one-to-one"></a>Uno-a-uno

Le relazioni uno-a-uno hanno una proprietà di navigazione di riferimento su entrambi i lati. Seguono le stesse convenzioni delle relazioni uno-a-molti, ma un indice univoco viene introdotto sulla proprietà della chiave esterna per garantire che solo un dipendente sia correlato a ogni entità.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> EF sceglierà una delle entità come dipendente in base alla capacità di rilevare una proprietà di chiave esterna. Se l'entità sbagliata viene scelta come dipendente, è possibile usare l'API Fluent per correggere questa operazione.

Quando si configura la relazione con l'API Fluent, si usano `HasOne` i `WithOne` metodi e.

Quando si configura la chiave esterna è necessario specificare il tipo di entità dipendente. si noti il parametro generico `HasForeignKey` fornito a nell'elenco seguente. In una relazione uno-a-molti è evidente che l'entità con l'esplorazione dei riferimenti è il dipendente e quello con la raccolta è l'entità. Ma non si tratta di una relazione uno-a-uno, di conseguenza è necessario definirla in modo esplicito.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a>Molti-a-molti

Le relazioni many-to-many senza una classe di entità per rappresentare la tabella di join non sono ancora supportate. È tuttavia possibile rappresentare una relazione molti-a-molti includendo una classe di entità per la tabella di join ed eseguendo il mapping di due relazioni uno-a-molti separate.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(pt => new { pt.PostId, pt.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
