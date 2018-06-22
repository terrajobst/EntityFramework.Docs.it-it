---
title: Relazioni - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 1732d32643effb0f12111191ed4ba3abb4182d93
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26053031"
---
# <a name="relationships"></a>Relazioni

Una relazione definisce come due entità interagiscono tra di loro. In un database relazionale, questo è rappresentato da un vincolo di chiave esterna.

> [!NOTE]  
> La maggior parte degli esempi in questo articolo usa una relazione uno-a-molti per illustrare i concetti. Per esempi di relazioni uno a uno e molti-a-molti, vedere il [altri modelli di relazione](#other-relationship-patterns) sezione alla fine dell'articolo.

## <a name="definition-of-terms"></a>Definizione dei termini

Sono presenti vari termini utilizzati per descrivere le relazioni

* **Entità dipendente:** questo è l'entità che contiene le proprietà di chiave esterne. Talvolta definito come figlio della relazione.

* **Entità principale:** questo è l'entità che contiene le proprietà di chiave primaria/alternativo. Talvolta definito come 'parent' della relazione.

* **Chiave esterna:** delle proprietà dell'entità dipendente che viene utilizzato per archiviare i valori della proprietà della chiave dell'entità dell'entità correlata.

* **Chiave dell'entità:** delle proprietà che identificano in modo univoco l'entità principale. Può trattarsi di chiave primaria o una chiave alternativa.

* **Proprietà di navigazione:** una proprietà definita nell'entità dipendenti e/o dell'entità che contiene un riferimento per l'entità corrisponde o corrispondono correlati.

  * **Proprietà di navigazione della raccolta:** una proprietà di navigazione che contiene riferimenti a tutte le entità correlate.

  * **Fare riferimento a proprietà di navigazione:** una proprietà di navigazione che contiene un riferimento a una singola entità correlate.

  * **Proprietà di navigazione inversa:** quando si parla di una proprietà di navigazione particolare, questo termine si riferisce alla proprietà di navigazione su altra estremità della relazione.

Listato di codice seguente mostra una relazione uno-a-molti tra `Blog` e`Post`

* `Post`è l'entità dipendente

* `Blog`è l'entità principale

* `Post.BlogId`rappresenta la chiave esterna

* `Blog.BlogId`è la chiave dell'entità (in questo caso è una chiave primaria, anziché una chiave alternativa)

* `Post.Blog`è una proprietà di navigazione di riferimento

* `Blog.Posts`è una proprietà di navigazione della raccolta

* `Post.Blog`la proprietà di navigazione inversa di `Blog.Posts` (e viceversa)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Convenzioni

Per convenzione, quando è disponibile una proprietà di navigazione individuata in un tipo verrà creata una relazione. Una proprietà viene considerata una proprietà di navigazione, se il tipo di a che punti non è possibile mappare come un tipo scalare dal provider di database corrente.

> [!NOTE]  
> Le relazioni che vengono individuate per convenzione utilizzerà sempre la chiave primaria dell'entità principale. Per indirizzare una chiave alternativa, è necessario effettuare un'ulteriore configurazione utilizzando l'API Fluent.

### <a name="fully-defined-relationships"></a>Relazioni completamente definite

Il modello più comune per le relazioni è di proprietà di navigazione definita in entrambe le estremità della relazione e una proprietà di chiave esterna definita nella classe di entità dipendente.

* Se tra due tipi, viene trovata una coppia di proprietà di navigazione, quindi verranno configurati come proprietà di navigazione inversa della stessa relazione.

* Se l'entità dipendente contiene una proprietà denominata `<primary key property name>`, `<navigation property name><primary key property name>`, o `<principal entity name><primary key property name>` quindi verrà configurato come chiave esterna.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Se sono presenti più proprietà di navigazione definita tra due tipi (ad esempio più di una coppia distinta di spostamenti che puntano a altro), quindi verranno creata alcuna relazione per convenzione, e sarà necessario configurarle manualmente per identificare il modo in le proprietà di navigazione della coppia.

### <a name="no-foreign-key-property"></a>Nessuna proprietà di chiave esterna

Sebbene sia consigliabile disporre di una proprietà di chiave esterna definita nella classe di entità dipendenti, non è obbligatorio. Se non viene trovata alcuna proprietà di chiave esterna, verrà introdotte con il nome di una proprietà di chiave esterna di ombreggiatura `<navigation property name><principal key property name>` (vedere [proprietà Shadow](shadow-properties.md) per altre informazioni).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Singola proprietà di navigazione

Inclusi solo da una proprietà di navigazione (nessun navigazione inversa e nessuna proprietà di chiave esterna) è sufficiente disporre di una relazione definita per convenzione. È anche possibile una singola proprietà di navigazione e una proprietà di chiave esterna.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Eliminazione a catena

Per convenzione, eliminazione a catena verrà impostato su *Cascade* per le relazioni necessarie e *ClientSetNull* per le relazioni facoltative. *CASCADE* entità dipendenti verranno eliminati anche. *ClientSetNull* significa che le entità dipendenti che non vengono caricate in memoria rimarrà invariata e deve essere eliminato manualmente o aggiornato per puntare a un'entità principale valida. Per le entità che vengono caricate in memoria, Core di Entity Framework tenterà di impostare le proprietà di chiave esterne su null.

Vedere il [relazioni obbligatori e facoltative](#required-and-optional-relationships) sezione per la differenza tra le relazioni obbligatori e facoltative.

Vedere [eliminazione a catena](../saving/cascade-delete.md) per ulteriori informazioni sui diversi comportamenti e le impostazioni predefinite utilizzate dalla convenzione di eliminazione.

## <a name="data-annotations"></a>Annotazioni dei dati

Esistono due le annotazioni dei dati che possono essere usate per configurare le relazioni, `[ForeignKey]` e `[InverseProperty]`.

### <a name="foreignkey"></a>[ForeignKey]

È possibile utilizzare le annotazioni dei dati per configurare la proprietà deve essere utilizzata come proprietà della chiave esterna per una determinata relazione. Questa operazione viene in genere eseguita quando la proprietà di chiave esterna non viene individuata per convenzione.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> Il `[ForeignKey]` annotazione può essere inserita su una proprietà di navigazione nella relazione. Non è necessario passare la proprietà di navigazione della classe di entità dipendente.

### <a name="inverseproperty"></a>[InverseProperty]

Per configurare come coppia di proprietà di navigazione per le entità dipendente e principale, è possibile utilizzare le annotazioni dei dati. Questa operazione viene in genere eseguita quando è presente più di una coppia di proprietà di navigazione tra due tipi di entità.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a>Microsoft Office Fluent API

Per configurare una relazione nell'API Fluent, iniziare identificando le proprietà di navigazione che costituiscono la relazione. `HasOne`o `HasMany` identifica la proprietà di navigazione sul tipo di entità a cui si sta iniziando la configurazione in. È quindi concatenare una chiamata a `WithOne` o `WithMany` per identificare la navigazione inversa. `HasOne`/`WithOne`vengono utilizzati per le proprietà di navigazione di riferimento e `HasMany` / `WithMany` vengono utilizzati per le proprietà di navigazione di raccolta.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a>Singola proprietà di navigazione

Se si dispone solo di una proprietà di navigazione, esistono overload senza parametri di `WithOne` e `WithMany`. Indica che è concettualmente un riferimento o una raccolta a altra estremità della relazione, ma nessuna proprietà di navigazione incluso nella classe di entità.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a>Chiave esterna

Per configurare la proprietà deve essere utilizzata come proprietà della chiave esterna per una determinata relazione, è possibile utilizzare l'API Fluent.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

Listato di codice seguente viene illustrato come configurare una chiave esterna composta.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

È possibile utilizzare l'overload della stringa di `HasForeignKey(...)` per configurare una proprietà shadow come chiave esterna (vedere [proprietà Shadow](shadow-properties.md) per altre informazioni). È consigliabile aggiungere in modo esplicito la proprietà shadow per il modello prima di utilizzare come chiave esterna (come illustrato di seguito).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a>Chiave di entità

Se si desidera che la chiave esterna per fare riferimento a una proprietà diversa dalla chiave primaria, è possibile utilizzare l'API Fluent per configurare la proprietà chiave principale per la relazione. La proprietà che consentono di configurare automaticamente come la chiave dell'entità sia configurato come una chiave alternativa (vedere [chiavi alternative](alternate-keys.md) per altre informazioni).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
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

Listato di codice seguente viene illustrato come configurare una chiave composta principale.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
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
> L'ordine in cui si specificano le proprietà chiave principale deve corrispondere all'ordine in cui sono specificati per la chiave esterna.

### <a name="required-and-optional-relationships"></a>Relazioni obbligatori e facoltative

È possibile utilizzare l'API Fluent per specificare se la relazione è obbligatorio o facoltativo. In definitiva controlla se la proprietà di chiave esterna è obbligatorio o facoltativo. Questo è particolarmente utile quando si utilizza una chiave esterna di stato di ombreggiatura. Se si dispone di una proprietà di chiave esterna nella classe di entità, requiredness della relazione viene determinato in base che proprietà della chiave esterna è obbligatorio o facoltativo (vedere [proprietà obbligatorie e facoltative](required-optional.md) per ulteriori informazioni informazioni).

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
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

Per configurare in modo esplicito il comportamento di delete cascade per una determinata relazione, è possibile utilizzare l'API Fluent.

Vedere [eliminazione a catena](../saving/cascade-delete.md) nella sezione di salvataggio dei dati per una descrizione dettagliata di ogni opzione.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
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

### <a name="one-to-one"></a>Uno a uno

Le relazioni uno-a-uno dispongono di una proprietà di navigazione di riferimento su entrambi i lati. Seguono le stesse convenzioni relazioni uno-a-molti, ma è stato introdotto un indice univoco sulla proprietà della chiave esterna per garantire un solo dipendente è correlata a ogni entità.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
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
> Entity Framework sceglierà un'entità per il dipendente in base alle capacità di rilevare una proprietà di chiave esterna. Se l'entità non corretto viene scelto come dipendente, è possibile utilizzare l'API Fluent per risolvere il problema.

Quando si configura la relazione con l'API Fluent, utilizzare il `HasOne` e `WithOne` metodi.

Quando si configura la chiave esterna, è necessario specificare il tipo di entità dipendente, si noti il parametro generico fornito a `HasForeignKey` nell'elenco riportato di seguito. In una relazione uno-a-molti è evidente che l'entità con lo spostamento di riferimento è dipendente e quello con la raccolta è l'entità. Ma non è pertanto in una relazione uno - pertanto la necessità di definirlo in modo esplicito.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
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

Le relazioni molti-a-molti senza una classe di entità per rappresentare la tabella di join non sono ancora supportate. Tuttavia, è possibile rappresentare una relazione molti-a-molti con l'inclusione di una classe di entità per la tabella di join e due relazioni uno-a-molti separato mapping.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(t => new { t.PostId, t.TagId });

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
