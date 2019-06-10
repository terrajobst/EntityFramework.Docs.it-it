---
title: Relazioni - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 793401362788e865c89ce01b6246b1ba14c36c8a
ms.sourcegitcommit: 8b9568211d37a1c36da9533fa1ac2ef063b0bf8c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/08/2019
ms.locfileid: "66815005"
---
# <a name="relationships"></a><span data-ttu-id="a59cd-102">Relazioni</span><span class="sxs-lookup"><span data-stu-id="a59cd-102">Relationships</span></span>

<span data-ttu-id="a59cd-103">Una relazione definisce due entità correlate tra loro.</span><span class="sxs-lookup"><span data-stu-id="a59cd-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="a59cd-104">In un database relazionale, ciò è rappresentato da un vincolo di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="a59cd-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="a59cd-105">La maggior parte degli esempi in questo articolo utilizza una relazione uno-a-molti per illustrare i concetti.</span><span class="sxs-lookup"><span data-stu-id="a59cd-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="a59cd-106">Per esempi di relazioni uno a uno e molti-a-molti, vedere la [altri modelli di relazione](#other-relationship-patterns) sezione alla fine dell'articolo.</span><span class="sxs-lookup"><span data-stu-id="a59cd-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="a59cd-107">Definizione dei termini</span><span class="sxs-lookup"><span data-stu-id="a59cd-107">Definition of Terms</span></span>

<span data-ttu-id="a59cd-108">Esistono una serie di termini usati per descrivere le relazioni</span><span class="sxs-lookup"><span data-stu-id="a59cd-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="a59cd-109">**Entità dipendente:** Questo è l'entità che contiene le proprietà di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="a59cd-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="a59cd-110">Talvolta definito come figlio della relazione.</span><span class="sxs-lookup"><span data-stu-id="a59cd-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="a59cd-111">**Entità principale:** Questo è l'entità che contiene le proprietà di chiave primaria o alternativa.</span><span class="sxs-lookup"><span data-stu-id="a59cd-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="a59cd-112">Talvolta definito come 'parent' della relazione.</span><span class="sxs-lookup"><span data-stu-id="a59cd-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="a59cd-113">**Chiave esterna:** Le proprietà dell'entità dipendente che viene usato per archiviare i valori della proprietà della chiave dell'entità correlata all'entità.</span><span class="sxs-lookup"><span data-stu-id="a59cd-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="a59cd-114">**Chiave dell'entità:** Le proprietà che identifica in modo univoco l'entità principale.</span><span class="sxs-lookup"><span data-stu-id="a59cd-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="a59cd-115">Può trattarsi di chiave primaria o una chiave alternativa.</span><span class="sxs-lookup"><span data-stu-id="a59cd-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="a59cd-116">**Proprietà di navigazione:** Una proprietà definita nell'entità dipendente e/o dell'entità che contiene un riferimenti per le entità correlate corrisponde o corrispondono.</span><span class="sxs-lookup"><span data-stu-id="a59cd-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="a59cd-117">**Proprietà di navigazione di raccolta:** Proprietà di navigazione che contiene riferimenti a molte entità correlate.</span><span class="sxs-lookup"><span data-stu-id="a59cd-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="a59cd-118">**Proprietà di navigazione di riferimento:** Proprietà di navigazione che contiene un riferimento a una sola entità correlata.</span><span class="sxs-lookup"><span data-stu-id="a59cd-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="a59cd-119">**Proprietà di navigazione inverso:** Quando si parla di una particolare proprietà di navigazione, questo termine fa riferimento alla proprietà di navigazione su altra estremità della relazione.</span><span class="sxs-lookup"><span data-stu-id="a59cd-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="a59cd-120">Il listato di codice seguente viene illustrata una relazione uno-a-molti tra `Blog` e `Post`</span><span class="sxs-lookup"><span data-stu-id="a59cd-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="a59cd-121">`Post` è l'entità dipendente</span><span class="sxs-lookup"><span data-stu-id="a59cd-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="a59cd-122">`Blog` è l'entità principale</span><span class="sxs-lookup"><span data-stu-id="a59cd-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="a59cd-123">`Post.BlogId` rappresenta la chiave esterna</span><span class="sxs-lookup"><span data-stu-id="a59cd-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="a59cd-124">`Blog.BlogId` è la chiave dell'entità (in questo caso è una chiave primaria, anziché una chiave alternativa)</span><span class="sxs-lookup"><span data-stu-id="a59cd-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="a59cd-125">`Post.Blog` è una proprietà di navigazione di riferimento</span><span class="sxs-lookup"><span data-stu-id="a59cd-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="a59cd-126">`Blog.Posts` è una proprietà di navigazione della raccolta</span><span class="sxs-lookup"><span data-stu-id="a59cd-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="a59cd-127">`Post.Blog` la proprietà di navigazione inversa di `Blog.Posts` (e viceversa)</span><span class="sxs-lookup"><span data-stu-id="a59cd-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="a59cd-128">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="a59cd-128">Conventions</span></span>

<span data-ttu-id="a59cd-129">Per convenzione, quando è disponibile una proprietà di navigazione individuata in un tipo verrà creata una relazione.</span><span class="sxs-lookup"><span data-stu-id="a59cd-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="a59cd-130">Una proprietà viene considerata una proprietà di navigazione se il tipo che cui punta può essere eseguito come un tipo scalare non dal provider del database corrente.</span><span class="sxs-lookup"><span data-stu-id="a59cd-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="a59cd-131">Le relazioni individuate per convenzione verranno eseguita sempre la chiave primaria dell'entità principale.</span><span class="sxs-lookup"><span data-stu-id="a59cd-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="a59cd-132">Per impostare come destinazione una chiave alternativa, un'ulteriore configurazione deve essere eseguita mediante l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="a59cd-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="a59cd-133">Relazioni completamente definite</span><span class="sxs-lookup"><span data-stu-id="a59cd-133">Fully Defined Relationships</span></span>

<span data-ttu-id="a59cd-134">Il modello più comune per le relazioni è di proprietà di navigazione definite su entrambe le estremità della relazione e una proprietà di chiave esterna definita nella classe di entità dipendente.</span><span class="sxs-lookup"><span data-stu-id="a59cd-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="a59cd-135">Se viene trovata una coppia di proprietà di navigazione tra due tipi, quindi verranno configurati come proprietà di navigazione inversa della stessa relazione.</span><span class="sxs-lookup"><span data-stu-id="a59cd-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="a59cd-136">Se l'entità dipendente contiene una proprietà denominata `<primary key property name>`, `<navigation property name><primary key property name>`, o `<principal entity name><primary key property name>` quindi verrà configurata come chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="a59cd-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="a59cd-137">Se sono presenti più proprietà di navigazione definite tra due tipi (vale a dire, più di una coppia di distinct della navigazione che puntano a vicenda), quindi non verranno creata alcuna relazione per convenzione e sarà necessario configurarle manualmente per identificare il uniscono le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a59cd-137">If there are multiple navigation properties defined between two types (that is, more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="a59cd-138">Nessuna proprietà di chiave esterna</span><span class="sxs-lookup"><span data-stu-id="a59cd-138">No Foreign Key Property</span></span>

<span data-ttu-id="a59cd-139">Sebbene sia consigliabile disporre di una proprietà di chiave esterna definita nella classe di entità dipendente, non è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="a59cd-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="a59cd-140">Se non viene trovata alcuna proprietà di chiave esterna, verranno introdotte con il nome di una proprietà di chiave esterna di shadow `<navigation property name><principal key property name>` (vedere [delle proprietà Shadow](shadow-properties.md) per altre informazioni).</span><span class="sxs-lookup"><span data-stu-id="a59cd-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="a59cd-141">Singola proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="a59cd-141">Single Navigation Property</span></span>

<span data-ttu-id="a59cd-142">Tra cui una sola proprietà di navigazione (Nessuna navigazione inversa e nessuna proprietà di chiave esterna) è sufficiente avere una relazione definita per convenzione.</span><span class="sxs-lookup"><span data-stu-id="a59cd-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="a59cd-143">Si può anche avere una singola proprietà di navigazione e una proprietà di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="a59cd-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="a59cd-144">Eliminazione a catena</span><span class="sxs-lookup"><span data-stu-id="a59cd-144">Cascade Delete</span></span>

<span data-ttu-id="a59cd-145">Per convenzione, eliminazione a catena verrà impostato su *Cascade* per le relazioni necessarie e *ClientSetNull* per le relazioni facoltative.</span><span class="sxs-lookup"><span data-stu-id="a59cd-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="a59cd-146">*CASCADE* significa che vengono eliminate anche le entità dipendente.</span><span class="sxs-lookup"><span data-stu-id="a59cd-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="a59cd-147">*ClientSetNull* significa che rimangono entità dipendenti non vengono caricati in memoria non modificato e deve essere eliminato manualmente o aggiornato per puntare a un'entità principale valida.</span><span class="sxs-lookup"><span data-stu-id="a59cd-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="a59cd-148">Per le entità che vengono caricate in memoria, EF Core tenterà di impostare la proprietà di chiave esterna su null.</span><span class="sxs-lookup"><span data-stu-id="a59cd-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="a59cd-149">Vedere le [relazioni obbligatorie e facoltative](#required-and-optional-relationships) sezione per la differenza tra le relazioni obbligatorie e facoltative.</span><span class="sxs-lookup"><span data-stu-id="a59cd-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="a59cd-150">Visualizzare [eliminazione a catena](../saving/cascade-delete.md) per altri dettagli sulle diverse eliminazione comportamenti e le impostazioni predefinite utilizzate per convenzione.</span><span class="sxs-lookup"><span data-stu-id="a59cd-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a59cd-151">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="a59cd-151">Data Annotations</span></span>

<span data-ttu-id="a59cd-152">Esistono due annotazioni dei dati che possono essere usate per configurare le relazioni, `[ForeignKey]` e `[InverseProperty]`.</span><span class="sxs-lookup"><span data-stu-id="a59cd-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span> <span data-ttu-id="a59cd-153">Questi sono disponibili nel `System.ComponentModel.DataAnnotations.Schema` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="a59cd-153">These are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="a59cd-154">[Perché ForeignKey]</span><span class="sxs-lookup"><span data-stu-id="a59cd-154">[ForeignKey]</span></span>

<span data-ttu-id="a59cd-155">È possibile usare le annotazioni dei dati per configurare quali proprietà deve essere utilizzata come proprietà della chiave esterna per una determinata relazione.</span><span class="sxs-lookup"><span data-stu-id="a59cd-155">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="a59cd-156">Questa operazione viene in genere eseguita quando la proprietà di chiave esterna non è stata individuata dalla convenzione.</span><span class="sxs-lookup"><span data-stu-id="a59cd-156">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> <span data-ttu-id="a59cd-157">Il `[ForeignKey]` annotazione può essere inserita su una proprietà di navigazione nella relazione.</span><span class="sxs-lookup"><span data-stu-id="a59cd-157">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="a59cd-158">Non è necessario passare la proprietà di navigazione nella classe di entità dipendente.</span><span class="sxs-lookup"><span data-stu-id="a59cd-158">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="a59cd-159">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="a59cd-159">[InverseProperty]</span></span>

<span data-ttu-id="a59cd-160">È possibile usare le annotazioni dei dati per configurare come proprietà di navigazione per le entità dipendente e principale abbinare le.</span><span class="sxs-lookup"><span data-stu-id="a59cd-160">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="a59cd-161">Questa operazione viene in genere eseguita quando è presente più di una coppia di proprietà di navigazione tra i due tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="a59cd-161">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a><span data-ttu-id="a59cd-162">API Fluent</span><span class="sxs-lookup"><span data-stu-id="a59cd-162">Fluent API</span></span>

<span data-ttu-id="a59cd-163">Per configurare una relazione nell'API Fluent, si inizia identificando le proprietà di navigazione che costituiscono la relazione.</span><span class="sxs-lookup"><span data-stu-id="a59cd-163">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="a59cd-164">`HasOne` o `HasMany` identifica la proprietà di navigazione sul tipo di entità a cui si sta iniziando la configurazione su.</span><span class="sxs-lookup"><span data-stu-id="a59cd-164">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="a59cd-165">È quindi concatenare una chiamata a `WithOne` o `WithMany` per identificare lo spostamento inverso.</span><span class="sxs-lookup"><span data-stu-id="a59cd-165">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="a59cd-166">`HasOne`/`WithOne` vengono usati per le proprietà di navigazione di riferimento e `HasMany` / `WithMany` vengono usati per le proprietà di navigazione di raccolta.</span><span class="sxs-lookup"><span data-stu-id="a59cd-166">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a><span data-ttu-id="a59cd-167">Singola proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="a59cd-167">Single Navigation Property</span></span>

<span data-ttu-id="a59cd-168">Se si ha solo una proprietà di navigazione, esistono overload senza parametri del `WithOne` e `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="a59cd-168">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="a59cd-169">Ciò indica che si verifica a livello concettuale un riferimento o insieme a altra estremità della relazione, ma è presente nessuna proprietà di navigazione inclusa nella classe di entità.</span><span class="sxs-lookup"><span data-stu-id="a59cd-169">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a><span data-ttu-id="a59cd-170">Chiave esterna</span><span class="sxs-lookup"><span data-stu-id="a59cd-170">Foreign Key</span></span>

<span data-ttu-id="a59cd-171">È possibile usare l'API Fluent per configurare quali proprietà deve essere utilizzata come proprietà della chiave esterna per una determinata relazione.</span><span class="sxs-lookup"><span data-stu-id="a59cd-171">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?highlight=17)]

<span data-ttu-id="a59cd-172">Il listato di codice seguente viene illustrato come configurare una chiave esterna composta.</span><span class="sxs-lookup"><span data-stu-id="a59cd-172">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?highlight=20)]

<span data-ttu-id="a59cd-173">È possibile usare l'overload dei valori della `HasForeignKey(...)` per configurare una proprietà shadow come chiave esterna (vedere [delle proprietà Shadow](shadow-properties.md) per altre informazioni).</span><span class="sxs-lookup"><span data-stu-id="a59cd-173">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="a59cd-174">È consigliabile aggiungere in modo esplicito la proprietà shadow per il modello prima di usarlo come chiave esterna (come illustrato di seguito).</span><span class="sxs-lookup"><span data-stu-id="a59cd-174">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a><span data-ttu-id="a59cd-175">Senza proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="a59cd-175">Without Navigation Property</span></span>

<span data-ttu-id="a59cd-176">Non è necessario fornire una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a59cd-176">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="a59cd-177">È possibile fornire semplicemente una chiave esterna su un lato della relazione.</span><span class="sxs-lookup"><span data-stu-id="a59cd-177">You can simply provide a Foreign Key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a><span data-ttu-id="a59cd-178">Chiave dell'entità</span><span class="sxs-lookup"><span data-stu-id="a59cd-178">Principal Key</span></span>

<span data-ttu-id="a59cd-179">Se si desidera che la chiave esterna per fare riferimento a una proprietà diversa dalla chiave primaria, è possibile usare l'API Fluent per configurare la proprietà chiave dell'entità per la relazione.</span><span class="sxs-lookup"><span data-stu-id="a59cd-179">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="a59cd-180">La proprietà che si configura automaticamente come verrà la chiave dell'entità da configurare come una chiave alternativa (vedere [le chiavi alternative](alternate-keys.md) per altre informazioni).</span><span class="sxs-lookup"><span data-stu-id="a59cd-180">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

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

<span data-ttu-id="a59cd-181">Il listato di codice seguente viene illustrato come configurare una chiave composta dell'entità.</span><span class="sxs-lookup"><span data-stu-id="a59cd-181">The following code listing shows how to configure a composite principal key.</span></span>

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
> <span data-ttu-id="a59cd-182">L'ordine in cui si specificano le proprietà di chiave dell'entità deve corrispondere all'ordine in cui vengono specificati per la chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="a59cd-182">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="a59cd-183">Relazioni obbligatorie e facoltative</span><span class="sxs-lookup"><span data-stu-id="a59cd-183">Required and Optional Relationships</span></span>

<span data-ttu-id="a59cd-184">È possibile utilizzare l'API Fluent per specificare se la relazione è obbligatoria o facoltativa.</span><span class="sxs-lookup"><span data-stu-id="a59cd-184">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="a59cd-185">In definitiva consente di controllare se la proprietà di chiave esterna è obbligatorio o facoltativo.</span><span class="sxs-lookup"><span data-stu-id="a59cd-185">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="a59cd-186">Ciò è particolarmente utile quando si usa una chiave esterna di ombreggiatura dello stato.</span><span class="sxs-lookup"><span data-stu-id="a59cd-186">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="a59cd-187">Se si ha una proprietà di chiave esterna nella classe di entità, requiredness della relazione viene determinato in base indica se la proprietà di chiave esterna è obbligatorio o facoltativo (vedere [le proprietà obbligatorie e facoltative](required-optional.md) per altre informazioni informazioni).</span><span class="sxs-lookup"><span data-stu-id="a59cd-187">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

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

### <a name="cascade-delete"></a><span data-ttu-id="a59cd-188">Eliminazione a catena</span><span class="sxs-lookup"><span data-stu-id="a59cd-188">Cascade Delete</span></span>

<span data-ttu-id="a59cd-189">È possibile usare l'API Fluent per configurare il comportamento di eliminazione a catena per una determinata relazione in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="a59cd-189">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="a59cd-190">Visualizzare [eliminazione a catena](../saving/cascade-delete.md) sulla sezione del salvataggio di dati per una descrizione dettagliata di ogni opzione.</span><span class="sxs-lookup"><span data-stu-id="a59cd-190">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

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

## <a name="other-relationship-patterns"></a><span data-ttu-id="a59cd-191">Altri modelli di relazione</span><span class="sxs-lookup"><span data-stu-id="a59cd-191">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="a59cd-192">Uno a uno</span><span class="sxs-lookup"><span data-stu-id="a59cd-192">One-to-one</span></span>

<span data-ttu-id="a59cd-193">Le relazioni uno a uno dispongono di una proprietà di navigazione di riferimento su entrambi i lati.</span><span class="sxs-lookup"><span data-stu-id="a59cd-193">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="a59cd-194">Seguono le stesse convenzioni di relazioni uno-a-molti, ma è stato introdotto un indice univoco sulla proprietà della chiave esterna per assicurarsi che un solo dipendente è correlata a ogni entità.</span><span class="sxs-lookup"><span data-stu-id="a59cd-194">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

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
> <span data-ttu-id="a59cd-195">Entity Framework sceglie una delle entità per il tipo dipendente basato sulla relativa capacità di rilevare una proprietà di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="a59cd-195">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="a59cd-196">Se l'entità non corretto viene scelto come dipendente, è possibile utilizzare l'API Fluent per risolvere il problema.</span><span class="sxs-lookup"><span data-stu-id="a59cd-196">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="a59cd-197">Quando si configura la relazione con l'API Fluent, usare il `HasOne` e `WithOne` metodi.</span><span class="sxs-lookup"><span data-stu-id="a59cd-197">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="a59cd-198">Quando si configura la chiave esterna, è necessario specificare il tipo di entità dipendente, si noti che il parametro generico fornito per `HasForeignKey` nell'elenco riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="a59cd-198">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="a59cd-199">In una relazione uno-a-molti è evidente che l'entità con la navigazione di riferimento è dipendente e quello con la raccolta è l'entità.</span><span class="sxs-lookup"><span data-stu-id="a59cd-199">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="a59cd-200">Ma questo non è quindi in una relazione uno a uno - di conseguenza la necessità di definirlo in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="a59cd-200">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

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

### <a name="many-to-many"></a><span data-ttu-id="a59cd-201">Molti-a-molti</span><span class="sxs-lookup"><span data-stu-id="a59cd-201">Many-to-many</span></span>

<span data-ttu-id="a59cd-202">Le relazioni molti-a-molti senza una classe di entità per rappresentare la tabella di join non sono ancora supportate.</span><span class="sxs-lookup"><span data-stu-id="a59cd-202">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="a59cd-203">Tuttavia, è possibile rappresentare una relazione molti-a-molti, includendo una classe di entità per la tabella di join e due relazioni uno-a-molti separato mapping.</span><span class="sxs-lookup"><span data-stu-id="a59cd-203">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

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
