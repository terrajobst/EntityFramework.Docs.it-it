---
title: Relazioni-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 1e59ce9e19c12aa5564bc8467dcfcb3be8ee8996
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655667"
---
# <a name="relationships"></a><span data-ttu-id="5ced0-102">Relazioni</span><span class="sxs-lookup"><span data-stu-id="5ced0-102">Relationships</span></span>

<span data-ttu-id="5ced0-103">Una relazione definisce il modo in cui due entità sono correlate tra loro.</span><span class="sxs-lookup"><span data-stu-id="5ced0-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="5ced0-104">In un database relazionale, questo è rappresentato da un vincolo FOREIGN KEY.</span><span class="sxs-lookup"><span data-stu-id="5ced0-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="5ced0-105">La maggior parte degli esempi in questo articolo usa una relazione uno-a-molti per illustrare i concetti.</span><span class="sxs-lookup"><span data-stu-id="5ced0-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="5ced0-106">Per esempi di relazioni uno-a-uno e molti-a-molti, vedere la sezione [altri modelli di relazione](#other-relationship-patterns) alla fine dell'articolo.</span><span class="sxs-lookup"><span data-stu-id="5ced0-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="5ced0-107">Definizione dei termini</span><span class="sxs-lookup"><span data-stu-id="5ced0-107">Definition of Terms</span></span>

<span data-ttu-id="5ced0-108">Esistono diversi termini usati per descrivere le relazioni</span><span class="sxs-lookup"><span data-stu-id="5ced0-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="5ced0-109">**Entità dipendente:** Si tratta dell'entità che contiene le proprietà di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="5ced0-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="5ced0-110">Noto anche come ' Child ' della relazione.</span><span class="sxs-lookup"><span data-stu-id="5ced0-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="5ced0-111">**Entità principale:** Si tratta dell'entità che contiene le proprietà della chiave primaria/alternativa.</span><span class="sxs-lookup"><span data-stu-id="5ced0-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="5ced0-112">Noto anche come ' Parent ' della relazione.</span><span class="sxs-lookup"><span data-stu-id="5ced0-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="5ced0-113">**Chiave esterna:** Proprietà nell'entità dipendente utilizzata per archiviare i valori della proprietà della chiave principale a cui è correlata l'entità.</span><span class="sxs-lookup"><span data-stu-id="5ced0-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="5ced0-114">**Chiave principale:** Proprietà che identifica in modo univoco l'entità principale.</span><span class="sxs-lookup"><span data-stu-id="5ced0-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="5ced0-115">Può trattarsi della chiave primaria o di una chiave alternativa.</span><span class="sxs-lookup"><span data-stu-id="5ced0-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="5ced0-116">**Proprietà di navigazione:** Proprietà definita nell'entità dipendente e/o dipendente che contiene uno o più riferimenti alle entità correlate.</span><span class="sxs-lookup"><span data-stu-id="5ced0-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="5ced0-117">**Proprietà di navigazione della raccolta:** Proprietà di navigazione che contiene riferimenti a molte entità correlate.</span><span class="sxs-lookup"><span data-stu-id="5ced0-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="5ced0-118">**Proprietà di navigazione riferimento:** Proprietà di navigazione che include un riferimento a una singola entità correlata.</span><span class="sxs-lookup"><span data-stu-id="5ced0-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="5ced0-119">**Proprietà di navigazione inversa:** Quando si discute una particolare proprietà di navigazione, questo termine fa riferimento alla proprietà di navigazione nell'altra entità finale della relazione.</span><span class="sxs-lookup"><span data-stu-id="5ced0-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="5ced0-120">Nel listato di codice seguente viene illustrata una relazione uno-a-molti tra `Blog` e `Post`</span><span class="sxs-lookup"><span data-stu-id="5ced0-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="5ced0-121">`Post` è l'entità dipendente</span><span class="sxs-lookup"><span data-stu-id="5ced0-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="5ced0-122">`Blog` è l'entità principale</span><span class="sxs-lookup"><span data-stu-id="5ced0-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="5ced0-123">`Post.BlogId` è la chiave esterna</span><span class="sxs-lookup"><span data-stu-id="5ced0-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="5ced0-124">`Blog.BlogId` è la chiave principale (in questo caso è una chiave primaria anziché una chiave alternativa)</span><span class="sxs-lookup"><span data-stu-id="5ced0-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="5ced0-125">`Post.Blog` è una proprietà di navigazione di riferimento</span><span class="sxs-lookup"><span data-stu-id="5ced0-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="5ced0-126">`Blog.Posts` è una proprietà di navigazione della raccolta</span><span class="sxs-lookup"><span data-stu-id="5ced0-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="5ced0-127">`Post.Blog` è la proprietà di navigazione inversa di `Blog.Posts` (e viceversa)</span><span class="sxs-lookup"><span data-stu-id="5ced0-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="5ced0-128">Convenzioni</span><span class="sxs-lookup"><span data-stu-id="5ced0-128">Conventions</span></span>

<span data-ttu-id="5ced0-129">Per convenzione, viene creata una relazione quando viene individuata una proprietà di navigazione su un tipo.</span><span class="sxs-lookup"><span data-stu-id="5ced0-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="5ced0-130">Una proprietà viene considerata una proprietà di navigazione se il tipo a cui punta non può essere mappato come tipo scalare dal provider di database corrente.</span><span class="sxs-lookup"><span data-stu-id="5ced0-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="5ced0-131">Le relazioni individuate per convenzione saranno sempre destinate alla chiave primaria dell'entità principale.</span><span class="sxs-lookup"><span data-stu-id="5ced0-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="5ced0-132">Per fare riferimento a una chiave alternativa, è necessario eseguire una configurazione aggiuntiva usando l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="5ced0-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="5ced0-133">Relazioni completamente definite</span><span class="sxs-lookup"><span data-stu-id="5ced0-133">Fully Defined Relationships</span></span>

<span data-ttu-id="5ced0-134">Il modello più comune per le relazioni prevede che le proprietà di navigazione siano definite in entrambe le estremità della relazione e in una proprietà di chiave esterna definita nella classe di entità dipendente.</span><span class="sxs-lookup"><span data-stu-id="5ced0-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="5ced0-135">Se tra due tipi viene trovata una coppia di proprietà di navigazione, queste verranno configurate come proprietà di navigazione inversa della stessa relazione.</span><span class="sxs-lookup"><span data-stu-id="5ced0-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="5ced0-136">Se l'entità dipendente contiene una proprietà denominata `<primary key property name>`, `<navigation property name><primary key property name>`o `<principal entity name><primary key property name>`, sarà configurata come chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="5ced0-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="5ced0-137">Se sono presenti più proprietà di navigazione definite tra due tipi, ovvero più di una coppia distinta di spostamenti che puntano l'uno all'altro, non verrà creata alcuna relazione per convenzione e sarà necessario configurarle manualmente per identificare il modo in cui coppia proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="5ced0-137">If there are multiple navigation properties defined between two types (that is, more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="5ced0-138">Nessuna proprietà di chiave esterna</span><span class="sxs-lookup"><span data-stu-id="5ced0-138">No Foreign Key Property</span></span>

<span data-ttu-id="5ced0-139">Sebbene sia consigliabile disporre di una proprietà di chiave esterna definita nella classe di entità dipendente, non è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="5ced0-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="5ced0-140">Se non viene trovata alcuna proprietà di chiave esterna, verrà introdotta una proprietà di chiave esterna Shadow con il nome `<navigation property name><principal key property name>`. per ulteriori informazioni, vedere [proprietà shadow](shadow-properties.md) .</span><span class="sxs-lookup"><span data-stu-id="5ced0-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="5ced0-141">Proprietà di navigazione singola</span><span class="sxs-lookup"><span data-stu-id="5ced0-141">Single Navigation Property</span></span>

<span data-ttu-id="5ced0-142">Per includere una relazione definita per convenzione, è sufficiente includere una sola proprietà di navigazione (nessuna navigazione inversa e nessuna proprietà di chiave esterna).</span><span class="sxs-lookup"><span data-stu-id="5ced0-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="5ced0-143">È inoltre possibile disporre di una sola proprietà di navigazione e di una proprietà di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="5ced0-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="5ced0-144">Eliminazione a catena</span><span class="sxs-lookup"><span data-stu-id="5ced0-144">Cascade Delete</span></span>

<span data-ttu-id="5ced0-145">Per convenzione, CASCADE DELETE verrà impostato su *Cascade* per le relazioni obbligatorie e *ClientSetNull* per le relazioni facoltative.</span><span class="sxs-lookup"><span data-stu-id="5ced0-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="5ced0-146">*Cascade* significa che vengono eliminate anche le entità dipendenti.</span><span class="sxs-lookup"><span data-stu-id="5ced0-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="5ced0-147">*ClientSetNull* indica che le entità dipendenti che non vengono caricate in memoria rimarranno invariate e dovranno essere eliminate manualmente o aggiornate in modo da puntare a un'entità principale valida.</span><span class="sxs-lookup"><span data-stu-id="5ced0-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="5ced0-148">Per le entità caricate in memoria, EF Core tenterà di impostare le proprietà di chiave esterna su null.</span><span class="sxs-lookup"><span data-stu-id="5ced0-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="5ced0-149">Per la differenza tra le relazioni obbligatorie e facoltative, vedere la sezione [relazioni obbligatorie e facoltative](#required-and-optional-relationships) .</span><span class="sxs-lookup"><span data-stu-id="5ced0-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="5ced0-150">Per ulteriori informazioni sui diversi comportamenti di eliminazione e sulle impostazioni predefinite utilizzate dalla convenzione, vedere [CASCADE DELETE](../saving/cascade-delete.md) .</span><span class="sxs-lookup"><span data-stu-id="5ced0-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5ced0-151">Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="5ced0-151">Data Annotations</span></span>

<span data-ttu-id="5ced0-152">Sono disponibili due annotazioni di dati che possono essere utilizzate per configurare relazioni, `[ForeignKey]` e `[InverseProperty]`.</span><span class="sxs-lookup"><span data-stu-id="5ced0-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span> <span data-ttu-id="5ced0-153">Sono disponibili nello spazio dei nomi `System.ComponentModel.DataAnnotations.Schema`.</span><span class="sxs-lookup"><span data-stu-id="5ced0-153">These are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="5ced0-154">ForeignKey</span><span class="sxs-lookup"><span data-stu-id="5ced0-154">[ForeignKey]</span></span>

<span data-ttu-id="5ced0-155">È possibile utilizzare le annotazioni dei dati per configurare quale proprietà deve essere utilizzata come proprietà di chiave esterna per una determinata relazione.</span><span class="sxs-lookup"><span data-stu-id="5ced0-155">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="5ced0-156">Questa operazione viene in genere eseguita quando la proprietà di chiave esterna non viene individuata per convenzione.</span><span class="sxs-lookup"><span data-stu-id="5ced0-156">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> <span data-ttu-id="5ced0-157">L'annotazione `[ForeignKey]` può essere posizionata su una proprietà di navigazione nella relazione.</span><span class="sxs-lookup"><span data-stu-id="5ced0-157">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="5ced0-158">Non è necessario che venga eseguita la proprietà di navigazione nella classe di entità dipendente.</span><span class="sxs-lookup"><span data-stu-id="5ced0-158">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="5ced0-159">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="5ced0-159">[InverseProperty]</span></span>

<span data-ttu-id="5ced0-160">È possibile utilizzare le annotazioni dei dati per configurare la modalità di associazione delle proprietà di navigazione nelle entità dipendenti e entità.</span><span class="sxs-lookup"><span data-stu-id="5ced0-160">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="5ced0-161">Questa operazione viene in genere eseguita quando è presente più di una coppia di proprietà di navigazione tra due tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="5ced0-161">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a><span data-ttu-id="5ced0-162">API Fluent</span><span class="sxs-lookup"><span data-stu-id="5ced0-162">Fluent API</span></span>

<span data-ttu-id="5ced0-163">Per configurare una relazione nell'API Fluent, è necessario innanzitutto identificare le proprietà di navigazione che compongono la relazione.</span><span class="sxs-lookup"><span data-stu-id="5ced0-163">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="5ced0-164">`HasOne` o `HasMany` identifica la proprietà di navigazione nel tipo di entità in cui si sta iniziando la configurazione.</span><span class="sxs-lookup"><span data-stu-id="5ced0-164">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="5ced0-165">È quindi possibile concatenare una chiamata a `WithOne` o `WithMany` per identificare la navigazione inversa.</span><span class="sxs-lookup"><span data-stu-id="5ced0-165">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="5ced0-166">`HasOne`/`WithOne` vengono utilizzate per le proprietà di navigazione di riferimento e `HasMany`/`WithMany` vengono utilizzate per le proprietà di navigazione della raccolta.</span><span class="sxs-lookup"><span data-stu-id="5ced0-166">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a><span data-ttu-id="5ced0-167">Proprietà di navigazione singola</span><span class="sxs-lookup"><span data-stu-id="5ced0-167">Single Navigation Property</span></span>

<span data-ttu-id="5ced0-168">Se è presente una sola proprietà di navigazione, sono presenti overload senza parametri di `WithOne` e `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="5ced0-168">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="5ced0-169">Ciò indica che è concettualmente presente un riferimento o una raccolta nell'altra entità finale della relazione, ma nella classe di entità non è inclusa alcuna proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="5ced0-169">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a><span data-ttu-id="5ced0-170">Chiave esterna</span><span class="sxs-lookup"><span data-stu-id="5ced0-170">Foreign Key</span></span>

<span data-ttu-id="5ced0-171">È possibile utilizzare l'API Fluent per configurare quale proprietà deve essere utilizzata come proprietà di chiave esterna per una determinata relazione.</span><span class="sxs-lookup"><span data-stu-id="5ced0-171">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

<span data-ttu-id="5ced0-172">Nel listato di codice seguente viene illustrato come configurare una chiave esterna composta.</span><span class="sxs-lookup"><span data-stu-id="5ced0-172">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

<span data-ttu-id="5ced0-173">È possibile utilizzare l'overload di stringa di `HasForeignKey(...)` per configurare una proprietà shadow come chiave esterna. per ulteriori informazioni, vedere [proprietà shadow](shadow-properties.md) .</span><span class="sxs-lookup"><span data-stu-id="5ced0-173">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="5ced0-174">È consigliabile aggiungere esplicitamente la proprietà shadow al modello prima di utilizzarla come chiave esterna, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="5ced0-174">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a><span data-ttu-id="5ced0-175">Senza proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="5ced0-175">Without Navigation Property</span></span>

<span data-ttu-id="5ced0-176">Non è necessario necessariamente fornire una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="5ced0-176">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="5ced0-177">È possibile specificare semplicemente una chiave esterna su un lato della relazione.</span><span class="sxs-lookup"><span data-stu-id="5ced0-177">You can simply provide a Foreign Key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a><span data-ttu-id="5ced0-178">Chiave principale</span><span class="sxs-lookup"><span data-stu-id="5ced0-178">Principal Key</span></span>

<span data-ttu-id="5ced0-179">Se si desidera che la chiave esterna faccia riferimento a una proprietà diversa dalla chiave primaria, è possibile utilizzare l'API Fluent per configurare la proprietà chiave principale per la relazione.</span><span class="sxs-lookup"><span data-stu-id="5ced0-179">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="5ced0-180">La proprietà configurata come chiave principale verrà automaticamente impostata come chiave alternativa. per ulteriori informazioni, vedere [chiavi alternative](alternate-keys.md) .</span><span class="sxs-lookup"><span data-stu-id="5ced0-180">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

<span data-ttu-id="5ced0-181">Nel listato di codice seguente viene illustrato come configurare una chiave principale composta.</span><span class="sxs-lookup"><span data-stu-id="5ced0-181">The following code listing shows how to configure a composite principal key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=Composite&highlight=11)]

> [!WARNING]  
> <span data-ttu-id="5ced0-182">L'ordine in cui si specificano le proprietà della chiave principale deve corrispondere all'ordine in cui sono specificate per la chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="5ced0-182">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="5ced0-183">Relazioni obbligatorie e facoltative</span><span class="sxs-lookup"><span data-stu-id="5ced0-183">Required and Optional Relationships</span></span>

<span data-ttu-id="5ced0-184">È possibile usare l'API Fluent per configurare se la relazione è obbligatoria o facoltativa.</span><span class="sxs-lookup"><span data-stu-id="5ced0-184">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="5ced0-185">In definitiva, controlla se la proprietà della chiave esterna è obbligatoria o facoltativa.</span><span class="sxs-lookup"><span data-stu-id="5ced0-185">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="5ced0-186">Questa operazione è particolarmente utile quando si usa una chiave esterna dello stato di ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="5ced0-186">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="5ced0-187">Se nella classe di entità è presente una proprietà di chiave esterna, la richiesta della relazione viene determinata a seconda che la proprietà della chiave esterna sia obbligatoria o facoltativa (per ulteriori informazioni, vedere [proprietà obbligatorie e facoltative](required-optional.md) ).</span><span class="sxs-lookup"><span data-stu-id="5ced0-187">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=11)]

### <a name="cascade-delete"></a><span data-ttu-id="5ced0-188">Eliminazione a catena</span><span class="sxs-lookup"><span data-stu-id="5ced0-188">Cascade Delete</span></span>

<span data-ttu-id="5ced0-189">È possibile usare l'API Fluent per configurare in modo esplicito il comportamento di eliminazione a catena per una determinata relazione.</span><span class="sxs-lookup"><span data-stu-id="5ced0-189">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="5ced0-190">Vedere [CASCADE DELETE](../saving/cascade-delete.md) nella sezione Saving data per una descrizione dettagliata di ogni opzione.</span><span class="sxs-lookup"><span data-stu-id="5ced0-190">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=11)]

## <a name="other-relationship-patterns"></a><span data-ttu-id="5ced0-191">Altri modelli di relazione</span><span class="sxs-lookup"><span data-stu-id="5ced0-191">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="5ced0-192">Uno-a-uno</span><span class="sxs-lookup"><span data-stu-id="5ced0-192">One-to-one</span></span>

<span data-ttu-id="5ced0-193">Le relazioni uno-a-uno hanno una proprietà di navigazione di riferimento su entrambi i lati.</span><span class="sxs-lookup"><span data-stu-id="5ced0-193">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="5ced0-194">Seguono le stesse convenzioni delle relazioni uno-a-molti, ma un indice univoco viene introdotto sulla proprietà della chiave esterna per garantire che solo un dipendente sia correlato a ogni entità.</span><span class="sxs-lookup"><span data-stu-id="5ced0-194">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=Property&highlight=6,15,16)]

> [!NOTE]  
> <span data-ttu-id="5ced0-195">EF sceglierà una delle entità come dipendente in base alla capacità di rilevare una proprietà di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="5ced0-195">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="5ced0-196">Se l'entità sbagliata viene scelta come dipendente, è possibile usare l'API Fluent per correggere questa operazione.</span><span class="sxs-lookup"><span data-stu-id="5ced0-196">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="5ced0-197">Quando si configura la relazione con l'API Fluent, si usano i metodi `HasOne` e `WithOne`.</span><span class="sxs-lookup"><span data-stu-id="5ced0-197">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="5ced0-198">Quando si configura la chiave esterna è necessario specificare il tipo di entità dipendente. si noti il parametro generico fornito a `HasForeignKey` nell'elenco seguente.</span><span class="sxs-lookup"><span data-stu-id="5ced0-198">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="5ced0-199">In una relazione uno-a-molti è evidente che l'entità con l'esplorazione dei riferimenti è il dipendente e quello con la raccolta è l'entità.</span><span class="sxs-lookup"><span data-stu-id="5ced0-199">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="5ced0-200">Ma non si tratta di una relazione uno-a-uno, di conseguenza è necessario definirla in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="5ced0-200">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a><span data-ttu-id="5ced0-201">Molti-a-molti</span><span class="sxs-lookup"><span data-stu-id="5ced0-201">Many-to-many</span></span>

<span data-ttu-id="5ced0-202">Le relazioni many-to-many senza una classe di entità per rappresentare la tabella di join non sono ancora supportate.</span><span class="sxs-lookup"><span data-stu-id="5ced0-202">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="5ced0-203">È tuttavia possibile rappresentare una relazione molti-a-molti includendo una classe di entità per la tabella di join ed eseguendo il mapping di due relazioni uno-a-molti separate.</span><span class="sxs-lookup"><span data-stu-id="5ced0-203">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)]
