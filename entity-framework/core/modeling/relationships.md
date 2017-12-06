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
---
# <a name="relationships"></a><span data-ttu-id="223a5-102">Relazioni</span><span class="sxs-lookup"><span data-stu-id="223a5-102">Relationships</span></span>

<span data-ttu-id="223a5-103">Una relazione definisce come due entità interagiscono tra di loro.</span><span class="sxs-lookup"><span data-stu-id="223a5-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="223a5-104">In un database relazionale, questo è rappresentato da un vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="223a5-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="223a5-105">La maggior parte degli esempi in questo articolo usa una relazione uno-a-molti per illustrare i concetti.</span><span class="sxs-lookup"><span data-stu-id="223a5-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="223a5-106">Per esempi di relazioni uno a uno e molti-a-molti, vedere il [altri modelli di relazione](#other-relationship-patterns) sezione alla fine dell'articolo.</span><span class="sxs-lookup"><span data-stu-id="223a5-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="223a5-107">Definizione dei termini</span><span class="sxs-lookup"><span data-stu-id="223a5-107">Definition of Terms</span></span>

<span data-ttu-id="223a5-108">Sono presenti vari termini utilizzati per descrivere le relazioni</span><span class="sxs-lookup"><span data-stu-id="223a5-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="223a5-109">**Entità dipendente:** questo è l'entità che contiene le proprietà di chiave esterne.</span><span class="sxs-lookup"><span data-stu-id="223a5-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="223a5-110">Talvolta definito come figlio della relazione.</span><span class="sxs-lookup"><span data-stu-id="223a5-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="223a5-111">**Entità principale:** questo è l'entità che contiene le proprietà di chiave primaria/alternativo.</span><span class="sxs-lookup"><span data-stu-id="223a5-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="223a5-112">Talvolta definito come 'parent' della relazione.</span><span class="sxs-lookup"><span data-stu-id="223a5-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="223a5-113">**Chiave esterna:** delle proprietà dell'entità dipendente che viene utilizzato per archiviare i valori della proprietà della chiave dell'entità dell'entità correlata.</span><span class="sxs-lookup"><span data-stu-id="223a5-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="223a5-114">**Chiave dell'entità:** delle proprietà che identificano in modo univoco l'entità principale.</span><span class="sxs-lookup"><span data-stu-id="223a5-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="223a5-115">Può trattarsi di chiave primaria o una chiave alternativa.</span><span class="sxs-lookup"><span data-stu-id="223a5-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="223a5-116">**Proprietà di navigazione:** una proprietà definita nell'entità dipendenti e/o dell'entità che contiene un riferimento per l'entità corrisponde o corrispondono correlati.</span><span class="sxs-lookup"><span data-stu-id="223a5-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="223a5-117">**Proprietà di navigazione della raccolta:** una proprietà di navigazione che contiene riferimenti a tutte le entità correlate.</span><span class="sxs-lookup"><span data-stu-id="223a5-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="223a5-118">**Fare riferimento a proprietà di navigazione:** una proprietà di navigazione che contiene un riferimento a una singola entità correlate.</span><span class="sxs-lookup"><span data-stu-id="223a5-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="223a5-119">**Proprietà di navigazione inversa:** quando si parla di una proprietà di navigazione particolare, questo termine si riferisce alla proprietà di navigazione su altra estremità della relazione.</span><span class="sxs-lookup"><span data-stu-id="223a5-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="223a5-120">Listato di codice seguente mostra una relazione uno-a-molti tra `Blog` e`Post`</span><span class="sxs-lookup"><span data-stu-id="223a5-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="223a5-121">`Post`è l'entità dipendente</span><span class="sxs-lookup"><span data-stu-id="223a5-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="223a5-122">`Blog`è l'entità principale</span><span class="sxs-lookup"><span data-stu-id="223a5-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="223a5-123">`Post.BlogId`rappresenta la chiave esterna</span><span class="sxs-lookup"><span data-stu-id="223a5-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="223a5-124">`Blog.BlogId`è la chiave dell'entità (in questo caso è una chiave primaria, anziché una chiave alternativa)</span><span class="sxs-lookup"><span data-stu-id="223a5-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="223a5-125">`Post.Blog`è una proprietà di navigazione di riferimento</span><span class="sxs-lookup"><span data-stu-id="223a5-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="223a5-126">`Blog.Posts`è una proprietà di navigazione della raccolta</span><span class="sxs-lookup"><span data-stu-id="223a5-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="223a5-127">`Post.Blog`la proprietà di navigazione inversa di `Blog.Posts` (e viceversa)</span><span class="sxs-lookup"><span data-stu-id="223a5-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="223a5-128">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="223a5-128">Conventions</span></span>

<span data-ttu-id="223a5-129">Per convenzione, quando è disponibile una proprietà di navigazione individuata in un tipo verrà creata una relazione.</span><span class="sxs-lookup"><span data-stu-id="223a5-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="223a5-130">Una proprietà viene considerata una proprietà di navigazione, se il tipo di a che punti non è possibile mappare come un tipo scalare dal provider di database corrente.</span><span class="sxs-lookup"><span data-stu-id="223a5-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="223a5-131">Le relazioni che vengono individuate per convenzione utilizzerà sempre la chiave primaria dell'entità principale.</span><span class="sxs-lookup"><span data-stu-id="223a5-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="223a5-132">Per indirizzare una chiave alternativa, è necessario effettuare un'ulteriore configurazione utilizzando l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="223a5-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="223a5-133">Relazioni completamente definite</span><span class="sxs-lookup"><span data-stu-id="223a5-133">Fully Defined Relationships</span></span>

<span data-ttu-id="223a5-134">Il modello più comune per le relazioni è di proprietà di navigazione definita in entrambe le estremità della relazione e una proprietà di chiave esterna definita nella classe di entità dipendente.</span><span class="sxs-lookup"><span data-stu-id="223a5-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="223a5-135">Se tra due tipi, viene trovata una coppia di proprietà di navigazione, quindi verranno configurati come proprietà di navigazione inversa della stessa relazione.</span><span class="sxs-lookup"><span data-stu-id="223a5-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="223a5-136">Se l'entità dipendente contiene una proprietà denominata `<primary key property name>`, `<navigation property name><primary key property name>`, o `<principal entity name><primary key property name>` quindi verrà configurato come chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="223a5-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="223a5-137">Se sono presenti più proprietà di navigazione definita tra due tipi (ad esempio più di una coppia distinta di spostamenti che puntano a altro), quindi verranno creata alcuna relazione per convenzione, e sarà necessario configurarle manualmente per identificare il modo in le proprietà di navigazione della coppia.</span><span class="sxs-lookup"><span data-stu-id="223a5-137">If there are multiple navigation properties defined between two types (i.e. more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="223a5-138">Nessuna proprietà di chiave esterna</span><span class="sxs-lookup"><span data-stu-id="223a5-138">No Foreign Key Property</span></span>

<span data-ttu-id="223a5-139">Sebbene sia consigliabile disporre di una proprietà di chiave esterna definita nella classe di entità dipendenti, non è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="223a5-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="223a5-140">Se non viene trovata alcuna proprietà di chiave esterna, verrà introdotte con il nome di una proprietà di chiave esterna di ombreggiatura `<navigation property name><principal key property name>` (vedere [proprietà Shadow](shadow-properties.md) per altre informazioni).</span><span class="sxs-lookup"><span data-stu-id="223a5-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="223a5-141">Singola proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="223a5-141">Single Navigation Property</span></span>

<span data-ttu-id="223a5-142">Inclusi solo da una proprietà di navigazione (nessun navigazione inversa e nessuna proprietà di chiave esterna) è sufficiente disporre di una relazione definita per convenzione.</span><span class="sxs-lookup"><span data-stu-id="223a5-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="223a5-143">È anche possibile una singola proprietà di navigazione e una proprietà di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="223a5-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="223a5-144">Eliminazione a catena</span><span class="sxs-lookup"><span data-stu-id="223a5-144">Cascade Delete</span></span>

<span data-ttu-id="223a5-145">Per convenzione, eliminazione a catena verrà impostato su *Cascade* per le relazioni necessarie e *ClientSetNull* per le relazioni facoltative.</span><span class="sxs-lookup"><span data-stu-id="223a5-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="223a5-146">*CASCADE* entità dipendenti verranno eliminati anche.</span><span class="sxs-lookup"><span data-stu-id="223a5-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="223a5-147">*ClientSetNull* significa che le entità dipendenti che non vengono caricate in memoria rimarrà invariata e deve essere eliminato manualmente o aggiornato per puntare a un'entità principale valida.</span><span class="sxs-lookup"><span data-stu-id="223a5-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="223a5-148">Per le entità che vengono caricate in memoria, Core di Entity Framework tenterà di impostare le proprietà di chiave esterne su null.</span><span class="sxs-lookup"><span data-stu-id="223a5-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="223a5-149">Vedere il [relazioni obbligatori e facoltative](#required-and-optional-relationships) sezione per la differenza tra le relazioni obbligatori e facoltative.</span><span class="sxs-lookup"><span data-stu-id="223a5-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="223a5-150">Vedere [eliminazione a catena](../saving/cascade-delete.md) per ulteriori informazioni sui diversi comportamenti e le impostazioni predefinite utilizzate dalla convenzione di eliminazione.</span><span class="sxs-lookup"><span data-stu-id="223a5-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="223a5-151">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="223a5-151">Data Annotations</span></span>

<span data-ttu-id="223a5-152">Esistono due le annotazioni dei dati che possono essere usate per configurare le relazioni, `[ForeignKey]` e `[InverseProperty]`.</span><span class="sxs-lookup"><span data-stu-id="223a5-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="223a5-153">[ForeignKey]</span><span class="sxs-lookup"><span data-stu-id="223a5-153">[ForeignKey]</span></span>

<span data-ttu-id="223a5-154">È possibile utilizzare le annotazioni dei dati per configurare la proprietà deve essere utilizzata come proprietà della chiave esterna per una determinata relazione.</span><span class="sxs-lookup"><span data-stu-id="223a5-154">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="223a5-155">Questa operazione viene in genere eseguita quando la proprietà di chiave esterna non viene individuata per convenzione.</span><span class="sxs-lookup"><span data-stu-id="223a5-155">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> <span data-ttu-id="223a5-156">Il `[ForeignKey]` annotazione può essere inserita su una proprietà di navigazione nella relazione.</span><span class="sxs-lookup"><span data-stu-id="223a5-156">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="223a5-157">Non è necessario passare la proprietà di navigazione della classe di entità dipendente.</span><span class="sxs-lookup"><span data-stu-id="223a5-157">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="223a5-158">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="223a5-158">[InverseProperty]</span></span>

<span data-ttu-id="223a5-159">Per configurare come coppia di proprietà di navigazione per le entità dipendente e principale, è possibile utilizzare le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="223a5-159">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="223a5-160">Questa operazione viene in genere eseguita quando è presente più di una coppia di proprietà di navigazione tra due tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="223a5-160">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a><span data-ttu-id="223a5-161">Microsoft Office Fluent API</span><span class="sxs-lookup"><span data-stu-id="223a5-161">Fluent API</span></span>

<span data-ttu-id="223a5-162">Per configurare una relazione nell'API Fluent, iniziare identificando le proprietà di navigazione che costituiscono la relazione.</span><span class="sxs-lookup"><span data-stu-id="223a5-162">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="223a5-163">`HasOne`o `HasMany` identifica la proprietà di navigazione sul tipo di entità a cui si sta iniziando la configurazione in.</span><span class="sxs-lookup"><span data-stu-id="223a5-163">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="223a5-164">È quindi concatenare una chiamata a `WithOne` o `WithMany` per identificare la navigazione inversa.</span><span class="sxs-lookup"><span data-stu-id="223a5-164">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="223a5-165">`HasOne`/`WithOne`vengono utilizzati per le proprietà di navigazione di riferimento e `HasMany` / `WithMany` vengono utilizzati per le proprietà di navigazione di raccolta.</span><span class="sxs-lookup"><span data-stu-id="223a5-165">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a><span data-ttu-id="223a5-166">Singola proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="223a5-166">Single Navigation Property</span></span>

<span data-ttu-id="223a5-167">Se si dispone solo di una proprietà di navigazione, esistono overload senza parametri di `WithOne` e `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="223a5-167">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="223a5-168">Indica che è concettualmente un riferimento o una raccolta a altra estremità della relazione, ma nessuna proprietà di navigazione incluso nella classe di entità.</span><span class="sxs-lookup"><span data-stu-id="223a5-168">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a><span data-ttu-id="223a5-169">Chiave esterna</span><span class="sxs-lookup"><span data-stu-id="223a5-169">Foreign Key</span></span>

<span data-ttu-id="223a5-170">Per configurare la proprietà deve essere utilizzata come proprietà della chiave esterna per una determinata relazione, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="223a5-170">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

<span data-ttu-id="223a5-171">Listato di codice seguente viene illustrato come configurare una chiave esterna composta.</span><span class="sxs-lookup"><span data-stu-id="223a5-171">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

<span data-ttu-id="223a5-172">È possibile utilizzare l'overload della stringa di `HasForeignKey(...)` per configurare una proprietà shadow come chiave esterna (vedere [proprietà Shadow](shadow-properties.md) per altre informazioni).</span><span class="sxs-lookup"><span data-stu-id="223a5-172">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="223a5-173">È consigliabile aggiungere in modo esplicito la proprietà shadow per il modello prima di utilizzare come chiave esterna (come illustrato di seguito).</span><span class="sxs-lookup"><span data-stu-id="223a5-173">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a><span data-ttu-id="223a5-174">Chiave di entità</span><span class="sxs-lookup"><span data-stu-id="223a5-174">Principal Key</span></span>

<span data-ttu-id="223a5-175">Se si desidera che la chiave esterna per fare riferimento a una proprietà diversa dalla chiave primaria, è possibile utilizzare l'API Fluent per configurare la proprietà chiave principale per la relazione.</span><span class="sxs-lookup"><span data-stu-id="223a5-175">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="223a5-176">La proprietà che consentono di configurare automaticamente come la chiave dell'entità sia configurato come una chiave alternativa (vedere [chiavi alternative](alternate-keys.md) per altre informazioni).</span><span class="sxs-lookup"><span data-stu-id="223a5-176">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

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

<span data-ttu-id="223a5-177">Listato di codice seguente viene illustrato come configurare una chiave composta principale.</span><span class="sxs-lookup"><span data-stu-id="223a5-177">The following code listing shows how to configure a composite principal key.</span></span>

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
> <span data-ttu-id="223a5-178">L'ordine in cui si specificano le proprietà chiave principale deve corrispondere all'ordine in cui sono specificati per la chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="223a5-178">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="223a5-179">Relazioni obbligatori e facoltative</span><span class="sxs-lookup"><span data-stu-id="223a5-179">Required and Optional Relationships</span></span>

<span data-ttu-id="223a5-180">È possibile utilizzare l'API Fluent per specificare se la relazione è obbligatorio o facoltativo.</span><span class="sxs-lookup"><span data-stu-id="223a5-180">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="223a5-181">In definitiva controlla se la proprietà di chiave esterna è obbligatorio o facoltativo.</span><span class="sxs-lookup"><span data-stu-id="223a5-181">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="223a5-182">Questo è particolarmente utile quando si utilizza una chiave esterna di stato di ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="223a5-182">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="223a5-183">Se si dispone di una proprietà di chiave esterna nella classe di entità, requiredness della relazione viene determinato in base che proprietà della chiave esterna è obbligatorio o facoltativo (vedere [proprietà obbligatorie e facoltative](required-optional.md) per ulteriori informazioni informazioni).</span><span class="sxs-lookup"><span data-stu-id="223a5-183">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

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

### <a name="cascade-delete"></a><span data-ttu-id="223a5-184">Eliminazione a catena</span><span class="sxs-lookup"><span data-stu-id="223a5-184">Cascade Delete</span></span>

<span data-ttu-id="223a5-185">Per configurare in modo esplicito il comportamento di delete cascade per una determinata relazione, è possibile utilizzare l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="223a5-185">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="223a5-186">Vedere [eliminazione a catena](../saving/cascade-delete.md) nella sezione di salvataggio dei dati per una descrizione dettagliata di ogni opzione.</span><span class="sxs-lookup"><span data-stu-id="223a5-186">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

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

## <a name="other-relationship-patterns"></a><span data-ttu-id="223a5-187">Altri modelli di relazione</span><span class="sxs-lookup"><span data-stu-id="223a5-187">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="223a5-188">Uno a uno</span><span class="sxs-lookup"><span data-stu-id="223a5-188">One-to-one</span></span>

<span data-ttu-id="223a5-189">Le relazioni uno-a-uno dispongono di una proprietà di navigazione di riferimento su entrambi i lati.</span><span class="sxs-lookup"><span data-stu-id="223a5-189">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="223a5-190">Seguono le stesse convenzioni relazioni uno-a-molti, ma è stato introdotto un indice univoco sulla proprietà della chiave esterna per garantire un solo dipendente è correlata a ogni entità.</span><span class="sxs-lookup"><span data-stu-id="223a5-190">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

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
> <span data-ttu-id="223a5-191">Entity Framework sceglierà un'entità per il dipendente in base alle capacità di rilevare una proprietà di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="223a5-191">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="223a5-192">Se l'entità non corretto viene scelto come dipendente, è possibile utilizzare l'API Fluent per risolvere il problema.</span><span class="sxs-lookup"><span data-stu-id="223a5-192">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="223a5-193">Quando si configura la relazione con l'API Fluent, utilizzare il `HasOne` e `WithOne` metodi.</span><span class="sxs-lookup"><span data-stu-id="223a5-193">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="223a5-194">Quando si configura la chiave esterna, è necessario specificare il tipo di entità dipendente, si noti il parametro generico fornito a `HasForeignKey` nell'elenco riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="223a5-194">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="223a5-195">In una relazione uno-a-molti è evidente che l'entità con lo spostamento di riferimento è dipendente e quello con la raccolta è l'entità.</span><span class="sxs-lookup"><span data-stu-id="223a5-195">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="223a5-196">Ma non è pertanto in una relazione uno - pertanto la necessità di definirlo in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="223a5-196">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

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

### <a name="many-to-many"></a><span data-ttu-id="223a5-197">Molti-a-molti</span><span class="sxs-lookup"><span data-stu-id="223a5-197">Many-to-many</span></span>

<span data-ttu-id="223a5-198">Le relazioni molti-a-molti senza una classe di entità per rappresentare la tabella di join non sono ancora supportate.</span><span class="sxs-lookup"><span data-stu-id="223a5-198">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="223a5-199">Tuttavia, è possibile rappresentare una relazione molti-a-molti con l'inclusione di una classe di entità per la tabella di join e due relazioni uno-a-molti separato mapping.</span><span class="sxs-lookup"><span data-stu-id="223a5-199">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

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
