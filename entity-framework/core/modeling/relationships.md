---
title: Relazioni - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 0e60ade605ffb73b02934ea32f1c757affce970d
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949218"
---
# <a name="relationships"></a>Relazioni

Una relazione definisce due entità correlate tra loro. In un database relazionale, ciò è rappresentato da un vincolo di chiave esterna.

> [!NOTE]  
> La maggior parte degli esempi in questo articolo utilizza una relazione uno-a-molti per illustrare i concetti. Per esempi di relazioni uno a uno e molti-a-molti, vedere la [altri modelli di relazione](#other-relationship-patterns) sezione alla fine dell'articolo.

## <a name="definition-of-terms"></a>Definizione dei termini

Esistono una serie di termini usati per descrivere le relazioni

* **Entità dipendente:** questo è l'entità che contiene le proprietà di chiave esterna. Talvolta definito come figlio della relazione.

* **Entità principale:** questo è l'entità che contiene le proprietà di chiave primaria o alternativa. Talvolta definito come 'parent' della relazione.

* **Chiave esterna:** le proprietà dell'entità dipendente che viene usato per archiviare i valori della proprietà della chiave dell'entità correlata all'entità.

* **Chiave dell'entità:** le proprietà che identifica in modo univoco l'entità principale. Può trattarsi di chiave primaria o una chiave alternativa.

* **Proprietà di navigazione:** una proprietà definita nell'entità dipendente e/o dell'entità che contiene un riferimenti per le entità correlate corrisponde o corrispondono.

  * **Proprietà di navigazione di raccolta:** una proprietà di navigazione che contiene riferimenti a molte entità correlate.

  * **Proprietà di navigazione di riferimento:** una proprietà di navigazione che contiene un riferimento a una sola entità correlata.

  * **Proprietà di navigazione inversa:** quando si parla di una particolare proprietà di navigazione, questo termine fa riferimento alla proprietà di navigazione su altra estremità della relazione.

Il listato di codice seguente viene illustrata una relazione uno-a-molti tra `Blog` e `Post`

* `Post` è l'entità dipendente

* `Blog` è l'entità principale

* `Post.BlogId` rappresenta la chiave esterna

* `Blog.BlogId` è la chiave dell'entità (in questo caso è una chiave primaria, anziché una chiave alternativa)

* `Post.Blog` è una proprietà di navigazione di riferimento

* `Blog.Posts` è una proprietà di navigazione della raccolta

* `Post.Blog` la proprietà di navigazione inversa di `Blog.Posts` (e viceversa)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Convenzioni

Per convenzione, quando è disponibile una proprietà di navigazione individuata in un tipo verrà creata una relazione. Una proprietà viene considerata una proprietà di navigazione se il tipo che cui punta può essere eseguito come un tipo scalare non dal provider del database corrente.

> [!NOTE]  
> Le relazioni individuate per convenzione verranno eseguita sempre la chiave primaria dell'entità principale. Per impostare come destinazione una chiave alternativa, un'ulteriore configurazione deve essere eseguita mediante l'API Fluent.

### <a name="fully-defined-relationships"></a>Relazioni completamente definite

Il modello più comune per le relazioni è di proprietà di navigazione definite su entrambe le estremità della relazione e una proprietà di chiave esterna definita nella classe di entità dipendente.

* Se viene trovata una coppia di proprietà di navigazione tra due tipi, quindi verranno configurati come proprietà di navigazione inversa della stessa relazione.

* Se l'entità dipendente contiene una proprietà denominata `<primary key property name>`, `<navigation property name><primary key property name>`, o `<principal entity name><primary key property name>` quindi verrà configurata come chiave esterna.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Se sono presenti più proprietà di navigazione definite tra due tipi (vale a dire, più di una coppia di distinct della navigazione che puntano a vicenda), quindi non verranno creata alcuna relazione per convenzione e sarà necessario configurarle manualmente per identificare il uniscono le proprietà di navigazione.

### <a name="no-foreign-key-property"></a>Nessuna proprietà di chiave esterna

Sebbene sia consigliabile disporre di una proprietà di chiave esterna definita nella classe di entità dipendente, non è obbligatorio. Se non viene trovata alcuna proprietà di chiave esterna, verranno introdotte con il nome di una proprietà di chiave esterna di shadow `<navigation property name><principal key property name>` (vedere [delle proprietà Shadow](shadow-properties.md) per altre informazioni).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Singola proprietà di navigazione

Tra cui una sola proprietà di navigazione (Nessuna navigazione inversa e nessuna proprietà di chiave esterna) è sufficiente avere una relazione definita per convenzione. Si può anche avere una singola proprietà di navigazione e una proprietà di chiave esterna.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Eliminazione a catena

Per convenzione, eliminazione a catena verrà impostato su *Cascade* per le relazioni necessarie e *ClientSetNull* per le relazioni facoltative. *CASCADE* significa che vengono eliminate anche le entità dipendente. *ClientSetNull* significa che rimangono entità dipendenti non vengono caricati in memoria non modificato e deve essere eliminato manualmente o aggiornato per puntare a un'entità principale valida. Per le entità che vengono caricate in memoria, EF Core tenterà di impostare la proprietà di chiave esterna su null.

Vedere le [relazioni obbligatorie e facoltative](#required-and-optional-relationships) sezione per la differenza tra le relazioni obbligatorie e facoltative.

Visualizzare [eliminazione a catena](../saving/cascade-delete.md) per altri dettagli sulle diverse eliminazione comportamenti e le impostazioni predefinite utilizzate per convenzione.

## <a name="data-annotations"></a>Annotazioni dei dati

Esistono due annotazioni dei dati che possono essere usate per configurare le relazioni, `[ForeignKey]` e `[InverseProperty]`.

### <a name="foreignkey"></a>[Perché ForeignKey]

È possibile usare le annotazioni dei dati per configurare quali proprietà deve essere utilizzata come proprietà della chiave esterna per una determinata relazione. Questa operazione viene in genere eseguita quando la proprietà di chiave esterna non è stata individuata dalla convenzione.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> Il `[ForeignKey]` annotazione può essere inserita su una proprietà di navigazione nella relazione. Non è necessario passare la proprietà di navigazione nella classe di entità dipendente.

### <a name="inverseproperty"></a>[InverseProperty]

È possibile usare le annotazioni dei dati per configurare come proprietà di navigazione per le entità dipendente e principale abbinare le. Questa operazione viene in genere eseguita quando è presente più di una coppia di proprietà di navigazione tra i due tipi di entità.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a>API Fluent

Per configurare una relazione nell'API Fluent, si inizia identificando le proprietà di navigazione che costituiscono la relazione. `HasOne` o `HasMany` identifica la proprietà di navigazione sul tipo di entità a cui si sta iniziando la configurazione su. È quindi concatenare una chiamata a `WithOne` o `WithMany` per identificare lo spostamento inverso. `HasOne`/`WithOne` vengono usati per le proprietà di navigazione di riferimento e `HasMany` / `WithMany` vengono usati per le proprietà di navigazione di raccolta.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a>Singola proprietà di navigazione

Se si ha solo una proprietà di navigazione, esistono overload senza parametri del `WithOne` e `WithMany`. Ciò indica che si verifica a livello concettuale un riferimento o insieme a altra estremità della relazione, ma è presente nessuna proprietà di navigazione inclusa nella classe di entità.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a>Chiave esterna

È possibile usare l'API Fluent per configurare quali proprietà deve essere utilizzata come proprietà della chiave esterna per una determinata relazione.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

Il listato di codice seguente viene illustrato come configurare una chiave esterna composta.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

È possibile usare l'overload dei valori della `HasForeignKey(...)` per configurare una proprietà shadow come chiave esterna (vedere [delle proprietà Shadow](shadow-properties.md) per altre informazioni). È consigliabile aggiungere in modo esplicito la proprietà shadow per il modello prima di usarlo come chiave esterna (come illustrato di seguito).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a>Chiave dell'entità

Se si desidera che la chiave esterna per fare riferimento a una proprietà diversa dalla chiave primaria, è possibile usare l'API Fluent per configurare la proprietà chiave dell'entità per la relazione. La proprietà che si configura automaticamente come verrà la chiave dell'entità da configurare come una chiave alternativa (vedere [le chiavi alternative](alternate-keys.md) per altre informazioni).

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

Il listato di codice seguente viene illustrato come configurare una chiave composta dell'entità.

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
> L'ordine in cui si specificano le proprietà di chiave dell'entità deve corrispondere all'ordine in cui vengono specificati per la chiave esterna.

### <a name="required-and-optional-relationships"></a>Relazioni obbligatorie e facoltative

È possibile utilizzare l'API Fluent per specificare se la relazione è obbligatoria o facoltativa. In definitiva consente di controllare se la proprietà di chiave esterna è obbligatorio o facoltativo. Ciò è particolarmente utile quando si usa una chiave esterna di ombreggiatura dello stato. Se si ha una proprietà di chiave esterna nella classe di entità, requiredness della relazione viene determinato in base indica se la proprietà di chiave esterna è obbligatorio o facoltativo (vedere [le proprietà obbligatorie e facoltative](required-optional.md) per altre informazioni informazioni).

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

È possibile usare l'API Fluent per configurare il comportamento di eliminazione a catena per una determinata relazione in modo esplicito.

Visualizzare [eliminazione a catena](../saving/cascade-delete.md) sulla sezione del salvataggio di dati per una descrizione dettagliata di ogni opzione.

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

Le relazioni uno a uno dispongono di una proprietà di navigazione di riferimento su entrambi i lati. Seguono le stesse convenzioni di relazioni uno-a-molti, ma è stato introdotto un indice univoco sulla proprietà della chiave esterna per assicurarsi che un solo dipendente è correlata a ogni entità.

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
> Entity Framework sceglie una delle entità per il tipo dipendente basato sulla relativa capacità di rilevare una proprietà di chiave esterna. Se l'entità non corretto viene scelto come dipendente, è possibile utilizzare l'API Fluent per risolvere il problema.

Quando si configura la relazione con l'API Fluent, usare il `HasOne` e `WithOne` metodi.

Quando si configura la chiave esterna, è necessario specificare il tipo di entità dipendente, si noti che il parametro generico fornito per `HasForeignKey` nell'elenco riportato di seguito. In una relazione uno-a-molti è evidente che l'entità con la navigazione di riferimento è dipendente e quello con la raccolta è l'entità. Ma questo non è quindi in una relazione uno a uno - di conseguenza la necessità di definirlo in modo esplicito.

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

Le relazioni molti-a-molti senza una classe di entità per rappresentare la tabella di join non sono ancora supportate. Tuttavia, è possibile rappresentare una relazione molti-a-molti, includendo una classe di entità per la tabella di join e due relazioni uno-a-molti separato mapping.

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
