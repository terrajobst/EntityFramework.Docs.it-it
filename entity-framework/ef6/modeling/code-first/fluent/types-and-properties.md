---
title: API Fluent - configurazione e Mapping dei tipi e le proprietà - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
caps.latest.revision: 3
ms.openlocfilehash: ec8b484433d13899a88f44e37823dd1a4bed6530
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121370"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a><span data-ttu-id="12e27-102">API Fluent - configurazione e di Mapping dei tipi e proprietà</span><span class="sxs-lookup"><span data-stu-id="12e27-102">Fluent API - Configuring and Mapping Properties and Types</span></span>
<span data-ttu-id="12e27-103">Quando si lavora con Code First di Entity Framework per eseguire il mapping alle tabelle mediante un set di convenzioni incorporato in Entity Framework le classi POCO è il comportamento predefinito.</span><span class="sxs-lookup"><span data-stu-id="12e27-103">When working with Entity Framework Code First the default behavior is to map your POCO classes to tables using a set of conventions baked into EF.</span></span> <span data-ttu-id="12e27-104">In alcuni casi, tuttavia, non è possibile o non si desidera seguire queste convenzioni ed è necessario eseguire il mapping di entità a un valore diverso da ciò che determinano le convenzioni.</span><span class="sxs-lookup"><span data-stu-id="12e27-104">Sometimes, however, you cannot or do not want to follow those conventions and need to map entities to something other than what the conventions dictate.</span></span>  

<span data-ttu-id="12e27-105">Esistono due modi principali, è possibile configurare Entity Framework per utilizzare un valore diverso da convenzioni, vale a dire [annotazioni](~/ef6/modeling/code-first/data-annotations.md) o l'API fluent EFs.</span><span class="sxs-lookup"><span data-stu-id="12e27-105">There are two main ways you can configure EF to use something other than conventions, namely [annotations](~/ef6/modeling/code-first/data-annotations.md) or EFs fluent API.</span></span> <span data-ttu-id="12e27-106">Le annotazioni coprono solo un sottoinsieme delle funzionalità API fluent, pertanto non esistono gli scenari di mapping che non possono essere ottenuti utilizzando le annotazioni.</span><span class="sxs-lookup"><span data-stu-id="12e27-106">The annotations only cover a subset of the fluent API functionality, so there are mapping scenarios that cannot be achieved using annotations.</span></span> <span data-ttu-id="12e27-107">Questo articolo è progettato per illustrare come usare l'API fluent per configurare le proprietà.</span><span class="sxs-lookup"><span data-stu-id="12e27-107">This article is designed to demonstrate how to use the fluent API to configure properties.</span></span>  

<span data-ttu-id="12e27-108">L'API Office fluent code first in genere avviene eseguendo l'override di [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) metodo nella classe derivata [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="12e27-108">The code first fluent API is most commonly accessed by overriding the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) method on your derived [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span></span> <span data-ttu-id="12e27-109">Gli esempi seguenti sono progettati per mostrare come eseguire diverse attività con l'api fluent e consentono di copiare il codice di e personalizzarlo in base alle proprie del modello, se si vuole visualizzare il modello che può essere utilizzate con come-viene quindi viene fornito alla fine di questo articolo.</span><span class="sxs-lookup"><span data-stu-id="12e27-109">The following samples are designed to show how to do various tasks with the fluent api and allow you to copy the code out and customize it to suit your model, if you wish to see the model that they can be used with as-is then it is provided at the end of this article.</span></span>  

## <a name="model-wide-settings"></a><span data-ttu-id="12e27-110">Impostazioni a livello di modello</span><span class="sxs-lookup"><span data-stu-id="12e27-110">Model-Wide Settings</span></span>  

### <a name="default-schema-ef6-onwards"></a><span data-ttu-id="12e27-111">Schema predefinito (6 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="12e27-111">Default Schema (EF6 onwards)</span></span>  

<span data-ttu-id="12e27-112">A partire da Entity Framework 6 è possibile utilizzare il metodo HasDefaultSchema su DbModelBuilder per specificare lo schema del database da utilizzare per tutte le tabelle, stored procedure e così via. Verrà ignorata per tutti gli oggetti che è configurato in modo esplicito uno schema diverso per questa impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="12e27-112">Starting with EF6 you can use the HasDefaultSchema method on DbModelBuilder to specify the database schema to use for all tables, stored procedures, etc. This default setting will be overridden for any objects that you explicitly configure a different schema for.</span></span>  

``` csharp
modelBuilder.HasDefaultSchema(“sales”);
```  

### <a name="custom-conventions-ef6-onwards"></a><span data-ttu-id="12e27-113">Convenzioni personalizzate (6 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="12e27-113">Custom Conventions (EF6 onwards)</span></span>  

<span data-ttu-id="12e27-114">A partire da Entity Framework 6 è possibile creare proprie convenzioni che integrano quelli inclusi in Code First.</span><span class="sxs-lookup"><span data-stu-id="12e27-114">Starting with EF6 you can create your own conventions to supplement the ones included in Code First.</span></span> <span data-ttu-id="12e27-115">Per altre informazioni, vedere [convenzioni del primo codice personalizzato](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="12e27-115">For more details, see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>  

## <a name="property-mapping"></a><span data-ttu-id="12e27-116">Mapping di proprietà</span><span class="sxs-lookup"><span data-stu-id="12e27-116">Property Mapping</span></span>  

<span data-ttu-id="12e27-117">Il [proprietà](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) metodo viene usato per configurare gli attributi per ogni proprietà che appartengono a un'entità o un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="12e27-117">The [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) method is used to configure attributes for each property belonging to an entity or complex type.</span></span> <span data-ttu-id="12e27-118">Il metodo di proprietà viene utilizzato per ottenere un oggetto di configurazione per una determinata proprietà.</span><span class="sxs-lookup"><span data-stu-id="12e27-118">The Property method is used to obtain a configuration object for a given property.</span></span> <span data-ttu-id="12e27-119">Le opzioni per l'oggetto di configurazione sono specifiche per il tipo di configurato; Ad esempio, è disponibile solo per le proprietà della stringa IsUnicode.</span><span class="sxs-lookup"><span data-stu-id="12e27-119">The options on the configuration object are specific to the type being configured; IsUnicode is available only on string properties for example.</span></span>  

### <a name="configuring-a-primary-key"></a><span data-ttu-id="12e27-120">Configurazione di una chiave primaria</span><span class="sxs-lookup"><span data-stu-id="12e27-120">Configuring a Primary Key</span></span>  

<span data-ttu-id="12e27-121">La convenzione di Entity Framework per le chiavi primarie è:</span><span class="sxs-lookup"><span data-stu-id="12e27-121">The Entity Framework convention for primary keys is:</span></span>  

1. <span data-ttu-id="12e27-122">La classe definisce una proprietà il cui nome è "ID" o "Id"</span><span class="sxs-lookup"><span data-stu-id="12e27-122">Your class defines a property whose name is “ID” or “Id”</span></span>  
2. <span data-ttu-id="12e27-123">o un nome di classe seguito da "ID" o "Id"</span><span class="sxs-lookup"><span data-stu-id="12e27-123">or a class name followed by “ID” or “Id”</span></span>  

<span data-ttu-id="12e27-124">Per impostare in modo esplicito una proprietà sia una chiave primaria, è possibile usare il metodo HasKey.</span><span class="sxs-lookup"><span data-stu-id="12e27-124">To explicitly set a property to be a primary key, you can use the HasKey method.</span></span> <span data-ttu-id="12e27-125">Nell'esempio seguente, il metodo HasKey consente di configurare la chiave primaria InstructorID sul tipo OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="12e27-125">In the following example, the HasKey method is used to configure the InstructorID primary key on the OfficeAssignment type.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a><span data-ttu-id="12e27-126">Configurazione di una chiave primaria composta</span><span class="sxs-lookup"><span data-stu-id="12e27-126">Configuring a Composite Primary Key</span></span>  

<span data-ttu-id="12e27-127">L'esempio seguente configura le proprietà DepartmentID e il nome da chiave primaria composta del tipo di reparto.</span><span class="sxs-lookup"><span data-stu-id="12e27-127">The following example configures the DepartmentID and Name properties to be the composite primary key of the Department type.</span></span>  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a><span data-ttu-id="12e27-128">Se si disattiva l'identità per le chiavi primarie numerico</span><span class="sxs-lookup"><span data-stu-id="12e27-128">Switching off Identity for Numeric Primary Keys</span></span>  

<span data-ttu-id="12e27-129">Nell'esempio seguente imposta le proprietà DepartmentID System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None per indicare che il valore non viene generato dal database.</span><span class="sxs-lookup"><span data-stu-id="12e27-129">The following example sets the DepartmentID property to System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None to indicate that the value will not be generated by the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a><span data-ttu-id="12e27-130">Specifica la lunghezza massima consentita in una proprietà</span><span class="sxs-lookup"><span data-stu-id="12e27-130">Specifying the Maximum Length on a Property</span></span>  

<span data-ttu-id="12e27-131">Nell'esempio seguente, la proprietà Name deve essere non più di 50 caratteri.</span><span class="sxs-lookup"><span data-stu-id="12e27-131">In the following example, the Name property should be no longer than 50 characters.</span></span> <span data-ttu-id="12e27-132">Se si imposta il valore più di 50 caratteri, si otterrà un [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) eccezione.</span><span class="sxs-lookup"><span data-stu-id="12e27-132">If you make the value longer than 50 characters, you will get a [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exception.</span></span> <span data-ttu-id="12e27-133">Se il primo codice crea un database da questo modello verrà impostato anche la lunghezza massima della colonna nome fino a 50 caratteri.</span><span class="sxs-lookup"><span data-stu-id="12e27-133">If Code First creates a database from this model it will also set the maximum length of the Name column to 50 characters.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a><span data-ttu-id="12e27-134">Configurare le proprietà come obbligatoria</span><span class="sxs-lookup"><span data-stu-id="12e27-134">Configuring the Property to be Required</span></span>  

<span data-ttu-id="12e27-135">Nell'esempio seguente, la proprietà Name è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="12e27-135">In the following example, the Name property is required.</span></span> <span data-ttu-id="12e27-136">Se non si specifica il nome, si otterrà un'eccezione DbEntityValidationException.</span><span class="sxs-lookup"><span data-stu-id="12e27-136">If you do not specify the Name, you will get a DbEntityValidationException exception.</span></span> <span data-ttu-id="12e27-137">Se il primo codice crea un database da questo modello la colonna utilizzata per archiviare questa proprietà in genere sarà non nullable.</span><span class="sxs-lookup"><span data-stu-id="12e27-137">If Code First creates a database from this model then the column used to store this property will usually be non-nullable.</span></span>  

> [!NOTE]
> <span data-ttu-id="12e27-138">In alcuni casi potrebbe non essere possibile per la colonna nel database sia non nullable, anche se la proprietà è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="12e27-138">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="12e27-139">Ad esempio, quando tramite una data di strategia ereditarietà tabella per gerarchia per i tipi più viene archiviato in una singola tabella.</span><span class="sxs-lookup"><span data-stu-id="12e27-139">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="12e27-140">Se un tipo derivato include una proprietà obbligatoria la colonna può essere reso non nullable poiché non tutti i tipi nella gerarchia disporrà di questa proprietà.</span><span class="sxs-lookup"><span data-stu-id="12e27-140">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a><span data-ttu-id="12e27-141">La configurazione di un indice su una o più proprietà</span><span class="sxs-lookup"><span data-stu-id="12e27-141">Configuring an Index on one or more properties</span></span>  

> [!NOTE]
> <span data-ttu-id="12e27-142">**EF6.1 e versioni successive solo** -attributo l'indice è stata introdotta in Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="12e27-142">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="12e27-143">Se si usa una versione precedente le informazioni contenute in questa sezione non si applicano.</span><span class="sxs-lookup"><span data-stu-id="12e27-143">If you are using an earlier version the information in this section does not apply.</span></span>  

<span data-ttu-id="12e27-144">Creazione di indici non è supportata in modo nativo dall'API Fluent, ma è possibile fare uso del supporto per **IndexAttribute** tramite l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="12e27-144">Creating indexes isn't natively supported by the Fluent API, but you can make use of the support for **IndexAttribute** via the Fluent API.</span></span> <span data-ttu-id="12e27-145">Gli attributi dell'indice vengono elaborati tramite l'inclusione di un'annotazione di modello del modello che viene quindi convertito in un indice nel database in un secondo momento nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="12e27-145">Index attributes are processed by including a model annotation on the model that is then turned into an Index in the database later in the pipeline.</span></span> <span data-ttu-id="12e27-146">È possibile aggiungere manualmente questi stessi annotazioni tramite l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="12e27-146">You can manually add these same annotations using the Fluent API.</span></span>  

<span data-ttu-id="12e27-147">Il modo più semplice per eseguire questa operazione consiste nel creare un'istanza di **IndexAttribute** che contiene tutte le impostazioni per il nuovo indice.</span><span class="sxs-lookup"><span data-stu-id="12e27-147">The easiest way to do this is to create an instance of **IndexAttribute** that contains all the settings for the new index.</span></span> <span data-ttu-id="12e27-148">È quindi possibile creare un'istanza di **IndexAnnotation** che è un tipo specifico di Entity Framework che convertirà il **IndexAttribute** impostazioni in un'annotazione di modello che possono essere archiviati nel modello di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="12e27-148">You can then create an instance of **IndexAnnotation** which is an EF specific type that will convert the **IndexAttribute** settings into a model annotation that can be stored on the EF model.</span></span> <span data-ttu-id="12e27-149">Si può quindi essere passati per il **HasColumnAnnotation** metodo nell'API Fluent, che specifica il nome **indice** per l'annotazione.</span><span class="sxs-lookup"><span data-stu-id="12e27-149">These can then be passed to the **HasColumnAnnotation** method on the Fluent API, specifying the name **Index** for the annotation.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

<span data-ttu-id="12e27-150">Per un elenco completo delle impostazioni disponibili nel **IndexAttribute**, vedere la *indice* sezione [annotazioni di dati Code First](~/ef6/modeling/code-first/data-annotations.md).</span><span class="sxs-lookup"><span data-stu-id="12e27-150">For a complete list of the settings available in **IndexAttribute**, see the *Index* section of [Code First Data Annotations](~/ef6/modeling/code-first/data-annotations.md).</span></span> <span data-ttu-id="12e27-151">Ciò include il nome dell'indice di personalizzazione, creazione di indici univoci e la creazione degli indici a più colonne.</span><span class="sxs-lookup"><span data-stu-id="12e27-151">This includes customizing the index name, creating unique indexes, and creating multi-column indexes.</span></span>  

<span data-ttu-id="12e27-152">È possibile specificare più annotazioni di indice in una singola proprietà, passando una matrice di **IndexAttribute** al costruttore della **IndexAnnotation**.</span><span class="sxs-lookup"><span data-stu-id="12e27-152">You can specify multiple index annotations on a single property by passing an array of **IndexAttribute** to the constructor of **IndexAnnotation**.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation(
        "Index",  
        new IndexAnnotation(new[]
            {
                new IndexAttribute("Index1"),
                new IndexAttribute("Index2") { IsUnique = true }
            })));
```  

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a><span data-ttu-id="12e27-153">Specifica di non eseguire il mapping di una proprietà CLR a una colonna nel Database</span><span class="sxs-lookup"><span data-stu-id="12e27-153">Specifying Not to Map a CLR Property to a Column in the Database</span></span>  

<span data-ttu-id="12e27-154">Nell'esempio seguente viene illustrato come specificare che una proprietà su un tipo CLR non è mappata a una colonna nel database.</span><span class="sxs-lookup"><span data-stu-id="12e27-154">The following example shows how to specify that a property on a CLR type is not mapped to a column in the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a><span data-ttu-id="12e27-155">Mapping di una proprietà CLR a una colonna specifica nel Database</span><span class="sxs-lookup"><span data-stu-id="12e27-155">Mapping a CLR Property to a Specific Column in the Database</span></span>  

<span data-ttu-id="12e27-156">Nell'esempio seguente esegue il mapping alla colonna del database DepartmentName la proprietà nome CLR.</span><span class="sxs-lookup"><span data-stu-id="12e27-156">The following example maps the Name CLR property to the DepartmentName database column.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="12e27-157">Ridenominazione di una chiave esterna che non è definita nel modello</span><span class="sxs-lookup"><span data-stu-id="12e27-157">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="12e27-158">Se si sceglie di non definire una chiave esterna su un tipo CLR, ma si desidera specificare il nome deve essere nel database, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="12e27-158">If you choose not to define a foreign key on a CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a><span data-ttu-id="12e27-159">La configurazione se una proprietà stringa supporta il contenuto Unicode</span><span class="sxs-lookup"><span data-stu-id="12e27-159">Configuring whether a String Property Supports Unicode Content</span></span>  

<span data-ttu-id="12e27-160">Per impostazione predefinita le stringhe sono Unicode (nvarchar in SQL Server).</span><span class="sxs-lookup"><span data-stu-id="12e27-160">By default strings are Unicode (nvarchar in SQL Server).</span></span> <span data-ttu-id="12e27-161">È possibile usare il metodo IsUnicode per specificare che deve essere una stringa di tipo varchar.</span><span class="sxs-lookup"><span data-stu-id="12e27-161">You can use the IsUnicode method to specify that a string should be of varchar type.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a><span data-ttu-id="12e27-162">Configurazione del tipo di dati di una colonna di Database</span><span class="sxs-lookup"><span data-stu-id="12e27-162">Configuring the Data Type of a Database Column</span></span>  

<span data-ttu-id="12e27-163">Il [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) metodo attiva il mapping alle diverse rappresentazioni dello stesso tipo di base.</span><span class="sxs-lookup"><span data-stu-id="12e27-163">The [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) method enables mapping to different representations of the same basic type.</span></span> <span data-ttu-id="12e27-164">Con questo metodo non consente di eseguire alcuna conversione dei dati in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="12e27-164">Using this method does not enable you to perform any conversion of the data at run time.</span></span> <span data-ttu-id="12e27-165">Si noti che IsUnicode è il modo migliore di colonne di impostazione per varchar, perché è indipendente dal database.</span><span class="sxs-lookup"><span data-stu-id="12e27-165">Note that IsUnicode is the preferred way of setting columns to varchar, as it is database agnostic.</span></span>  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a><span data-ttu-id="12e27-166">Configurazione delle proprietà per un tipo complesso</span><span class="sxs-lookup"><span data-stu-id="12e27-166">Configuring Properties on a Complex Type</span></span>  

<span data-ttu-id="12e27-167">Esistono due modi per configurare le proprietà scalari in un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="12e27-167">There are two ways to configure scalar properties on a complex type.</span></span>  

<span data-ttu-id="12e27-168">È possibile chiamare proprietà su ComplexTypeConfiguration.</span><span class="sxs-lookup"><span data-stu-id="12e27-168">You can call Property on ComplexTypeConfiguration.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

<span data-ttu-id="12e27-169">È anche possibile usare la notazione del punto per accedere a una proprietà di un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="12e27-169">You can also use the dot notation to access a property of a complex type.</span></span>  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a><span data-ttu-id="12e27-170">Configurazione di una proprietà da utilizzare come Token di concorrenza ottimistica</span><span class="sxs-lookup"><span data-stu-id="12e27-170">Configuring a Property to Be Used as an Optimistic Concurrency Token</span></span>  

<span data-ttu-id="12e27-171">Per specificare che una proprietà in un'entità rappresenta un token di concorrenza, è possibile usare l'attributo ConcurrencyCheck o il metodo IsConcurrencyToken.</span><span class="sxs-lookup"><span data-stu-id="12e27-171">To specify that a property in an entity represents a concurrency token, you can use either the ConcurrencyCheck attribute or the IsConcurrencyToken method.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

<span data-ttu-id="12e27-172">È inoltre possibile utilizzare il metodo IsRowVersion per configurare le proprietà in modo che sia una versione di riga nel database.</span><span class="sxs-lookup"><span data-stu-id="12e27-172">You can also use the IsRowVersion method to configure the property to be a row version in the database.</span></span> <span data-ttu-id="12e27-173">Impostazione della proprietà sia che una versione di riga viene configurato automaticamente in modo che sia un token di concorrenza ottimistica.</span><span class="sxs-lookup"><span data-stu-id="12e27-173">Setting the property to be a row version automatically configures it to be an optimistic concurrency token.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a><span data-ttu-id="12e27-174">Mapping dei tipi</span><span class="sxs-lookup"><span data-stu-id="12e27-174">Type Mapping</span></span>  

### <a name="specifying-that-a-class-is-a-complex-type"></a><span data-ttu-id="12e27-175">Specifica che una classe è un tipo complesso</span><span class="sxs-lookup"><span data-stu-id="12e27-175">Specifying That a Class Is a Complex Type</span></span>  

<span data-ttu-id="12e27-176">Per convenzione, un tipo che non dispone di alcuna chiave primaria specificato viene considerato un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="12e27-176">By convention, a type that has no primary key specified is treated as a complex type.</span></span> <span data-ttu-id="12e27-177">Esistono alcuni scenari in cui Code First non rileverà un tipo complesso (ad esempio, se è presente una proprietà denominata ID, ma non significa che per poter essere una chiave primaria).</span><span class="sxs-lookup"><span data-stu-id="12e27-177">There are some scenarios where Code First will not detect a complex type (for example, if you do have a property called ID, but you do not mean for it to be a primary key).</span></span> <span data-ttu-id="12e27-178">In questi casi, è necessario usare l'API fluent specificare in modo esplicito che un tipo è un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="12e27-178">In such cases, you would use the fluent API to explicitly specify that a type is a complex type.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a><span data-ttu-id="12e27-179">Specifica di non eseguire il mapping di un tipo di entità di Common Language Runtime a una tabella nel Database</span><span class="sxs-lookup"><span data-stu-id="12e27-179">Specifying Not to Map a CLR Entity Type to a Table in the Database</span></span>  

<span data-ttu-id="12e27-180">Nell'esempio seguente viene illustrato come impedire che un tipo CLR mappato a una tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="12e27-180">The following example shows how to exclude a CLR type from being mapped to a table in the database.</span></span>  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a><span data-ttu-id="12e27-181">Mapping di un tipo di entità a una tabella specifica nel Database</span><span class="sxs-lookup"><span data-stu-id="12e27-181">Mapping an Entity Type to a Specific Table in the Database</span></span>  

<span data-ttu-id="12e27-182">Tutte le proprietà del reparto verranno mappate a colonne in una tabella denominata t_ Department.</span><span class="sxs-lookup"><span data-stu-id="12e27-182">All properties of Department will be mapped to columns in a table called t_ Department.</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

<span data-ttu-id="12e27-183">È anche possibile specificare il nome dello schema simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="12e27-183">You can also specify the schema name like this:</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a><span data-ttu-id="12e27-184">Mapping dell'ereditarietà tabella Per gerarchia (TPH)</span><span class="sxs-lookup"><span data-stu-id="12e27-184">Mapping the Table-Per-Hierarchy (TPH) Inheritance</span></span>  

<span data-ttu-id="12e27-185">Nello scenario di mapping tabella per gerarchia, tutti i tipi in una gerarchia di ereditarietà sono mappati a una singola tabella.</span><span class="sxs-lookup"><span data-stu-id="12e27-185">In the TPH mapping scenario, all types in an inheritance hierarchy are mapped to a single table.</span></span> <span data-ttu-id="12e27-186">Una colonna discriminatore viene utilizzata per identificare il tipo di ogni riga.</span><span class="sxs-lookup"><span data-stu-id="12e27-186">A discriminator column is used to identify the type of each row.</span></span> <span data-ttu-id="12e27-187">Quando si crea il modello Code First, della tabella per gerarchia è la strategia predefinita per i tipi che fanno parte della gerarchia di ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="12e27-187">When creating your model with Code First, TPH is the default strategy for the types that participate in the inheritance hierarchy.</span></span> <span data-ttu-id="12e27-188">Per impostazione predefinita, la colonna discriminatore viene aggiunta alla tabella con il nome "Discriminator" e il nome del tipo CLR di ogni tipo nella gerarchia viene usato per i valori del discriminatore.</span><span class="sxs-lookup"><span data-stu-id="12e27-188">By default, the discriminator column is added to the table with the name “Discriminator” and the CLR type name of each type in the hierarchy is used for the discriminator values.</span></span> <span data-ttu-id="12e27-189">È possibile modificare il comportamento predefinito tramite l'API fluent.</span><span class="sxs-lookup"><span data-stu-id="12e27-189">You can modify the default behavior by using the fluent API.</span></span>  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a><span data-ttu-id="12e27-190">Mapping dell'ereditarietà tabella Per tipo TPT)</span><span class="sxs-lookup"><span data-stu-id="12e27-190">Mapping the Table-Per-Type (TPT) Inheritance</span></span>  

<span data-ttu-id="12e27-191">Nello scenario di mapping tabella per tipo, tutti i tipi sono mappati a singole tabelle.</span><span class="sxs-lookup"><span data-stu-id="12e27-191">In the TPT mapping scenario, all types are mapped to individual tables.</span></span> <span data-ttu-id="12e27-192">Le proprietà che appartengono esclusivamente a un tipo di base o derivato sono archiviate in una tabella che viene mappata a quel tipo.</span><span class="sxs-lookup"><span data-stu-id="12e27-192">Properties that belong solely to a base type or derived type are stored in a table that maps to that type.</span></span> <span data-ttu-id="12e27-193">Tabelle in cui eseguire il mapping a tipi derivati anche archiviano una chiave esterna che unisce in join la tabella derivata con la tabella di base.</span><span class="sxs-lookup"><span data-stu-id="12e27-193">Tables that map to derived types also store a foreign key that joins the derived table with the base table.</span></span>  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a><span data-ttu-id="12e27-194">Mapping di ereditarietà della classe tabella Per tipo concreto (TP)</span><span class="sxs-lookup"><span data-stu-id="12e27-194">Mapping the Table-Per-Concrete Class (TPC) Inheritance</span></span>  

<span data-ttu-id="12e27-195">Nello scenario di mapping TPC, tutti i tipi non astratti nella gerarchia vengono mappati a singole tabelle.</span><span class="sxs-lookup"><span data-stu-id="12e27-195">In the TPC mapping scenario, all non-abstract types in the hierarchy are mapped to individual tables.</span></span> <span data-ttu-id="12e27-196">Le tabelle in cui eseguire il mapping a classi derivate hanno nessuna relazione con la tabella che esegue il mapping alla classe di base nel database.</span><span class="sxs-lookup"><span data-stu-id="12e27-196">The tables that map to the derived classes have no relationship to the table that maps to the base class in the database.</span></span> <span data-ttu-id="12e27-197">Tutte le proprietà di una classe, incluse le proprietà ereditate, vengono mappate alle colonne della tabella corrispondente.</span><span class="sxs-lookup"><span data-stu-id="12e27-197">All properties of a class, including inherited properties, are mapped to columns of the corresponding table.</span></span>  

<span data-ttu-id="12e27-198">Il metodo MapInheritedProperties consente di configurare ogni tipo derivato.</span><span class="sxs-lookup"><span data-stu-id="12e27-198">Call the MapInheritedProperties method to configure each derived type.</span></span> <span data-ttu-id="12e27-199">Il metodo MapInheritedProperties esegue un nuovo mapping di tutte le proprietà che sono state ereditate dalla classe di base a nuove colonne nella tabella per la classe derivata.</span><span class="sxs-lookup"><span data-stu-id="12e27-199">MapInheritedProperties remaps all properties that were inherited from the base class to new columns in the table for the derived class.</span></span>  

> [!NOTE]
> <span data-ttu-id="12e27-200">Si noti che poiché non condividono le tabelle che fanno parte di una gerarchia di ereditarietà TPC esiste una chiave primaria saranno le chiavi di entità duplicate durante l'inserimento in tabelle in cui vengono eseguito il mapping alle sottoclassi, in presenza di valori del database generato con il valore di inizializzazione identity stessa.</span><span class="sxs-lookup"><span data-stu-id="12e27-200">Note that because the tables participating in TPC inheritance hierarchy do not share a primary key there will be duplicate entity keys when inserting in tables that are mapped to subclasses if you have database generated values with the same identity seed.</span></span> <span data-ttu-id="12e27-201">Per risolvere questo problema è possibile specificare un valore di inizializzazione iniziale diverso per ogni tabella o disattivare l'identità sulla proprietà della chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="12e27-201">To solve this problem you can either specify a different initial seed value for each table or switch off identity on the primary key property.</span></span> <span data-ttu-id="12e27-202">Identità è il valore predefinito per le proprietà di integer chiave quando si lavora con Code First.</span><span class="sxs-lookup"><span data-stu-id="12e27-202">Identity is the default value for integer key properties when working with Code First.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .Property(c => c.CourseID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);

modelBuilder.Entity<OnsiteCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnsiteCourse");
});

modelBuilder.Entity<OnlineCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnlineCourse");
});
```  

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a><span data-ttu-id="12e27-203">Mapping delle proprietà di un tipo di entità a più tabelle nel Database (suddivisione di entità)</span><span class="sxs-lookup"><span data-stu-id="12e27-203">Mapping Properties of an Entity Type to Multiple Tables in the Database (Entity Splitting)</span></span>  

<span data-ttu-id="12e27-204">Separazione delle entità consente le proprietà di un tipo di entità siano distribuiti in più tabelle.</span><span class="sxs-lookup"><span data-stu-id="12e27-204">Entity splitting allows the properties of an entity type to be spread across multiple tables.</span></span> <span data-ttu-id="12e27-205">Nell'esempio seguente, l'entità Department è suddiviso in due tabelle: reparto e DepartmentDetails.</span><span class="sxs-lookup"><span data-stu-id="12e27-205">In the following example, the Department entity is split into two tables: Department and DepartmentDetails.</span></span> <span data-ttu-id="12e27-206">Separazione delle entità Usa più chiamate al metodo Map per eseguire il mapping di un subset delle proprietà a una tabella specifica.</span><span class="sxs-lookup"><span data-stu-id="12e27-206">Entity splitting uses multiple calls to the Map method to map a subset of properties to a specific table.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Name });
        m.ToTable("Department");
    })
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Administrator, t.StartDate, t.Budget });
        m.ToTable("DepartmentDetails");
    });
```  

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a><span data-ttu-id="12e27-207">Mapping di più tipi di entità a una tabella nel Database (suddivisione di tabelle)</span><span class="sxs-lookup"><span data-stu-id="12e27-207">Mapping Multiple Entity Types to One Table in the Database (Table Splitting)</span></span>  

<span data-ttu-id="12e27-208">Nell'esempio seguente esegue il mapping di due tipi di entità che condividono una chiave primaria a una tabella.</span><span class="sxs-lookup"><span data-stu-id="12e27-208">The following example maps two entity types that share a primary key to one table.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a><span data-ttu-id="12e27-209">Mapping di un tipo di entità alle Stored procedure Insert/Update/Delete (6 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="12e27-209">Mapping an Entity Type to Insert/Update/Delete Stored Procedures (EF6 onwards)</span></span>  

<span data-ttu-id="12e27-210">A partire da Entity Framework 6 è possibile eseguire il mapping di un'entità per l'uso di stored procedure per insert, update e delete.</span><span class="sxs-lookup"><span data-stu-id="12e27-210">Starting with EF6 you can map an entity to use stored procedures for insert update and delete.</span></span> <span data-ttu-id="12e27-211">Per altre informazioni, vedere [primo Insert/Update/Delete. Stored procedure codice](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span><span class="sxs-lookup"><span data-stu-id="12e27-211">For more details see, [Code First Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span></span>  

## <a name="model-used-in-samples"></a><span data-ttu-id="12e27-212">Modello utilizzato negli esempi</span><span class="sxs-lookup"><span data-stu-id="12e27-212">Model Used in Samples</span></span>  

<span data-ttu-id="12e27-213">Il modello Code First seguente viene usato per gli esempi in questa pagina.</span><span class="sxs-lookup"><span data-stu-id="12e27-213">The following Code First model is used for the samples on this page.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.ModelConfiguration.Conventions;
// add a reference to System.ComponentModel.DataAnnotations DLL
using System.ComponentModel.DataAnnotations;
using System.Collections.Generic;
using System;

public class SchoolEntities : DbContext
{
    public DbSet<Course> Courses { get; set; }
    public DbSet<Department> Departments { get; set; }
    public DbSet<Instructor> Instructors { get; set; }
    public DbSet<OfficeAssignment> OfficeAssignments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention then the generated tables will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}

public class Department
{
    public Department()
    {
        this.Courses = new HashSet<Course>();
    }
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }
    public decimal Budget { get; set; }
    public System.DateTime StartDate { get; set; }
    public int? Administrator { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; private set; }
}

public class Course
{
    public Course()
    {
        this.Instructors = new HashSet<Instructor>();
    }
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
    public virtual ICollection<Instructor> Instructors { get; private set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public OnsiteCourse()
    {
        Details = new Details();
    }

    public Details Details { get; set; }
}

public class Details
{
    public System.DateTime Time { get; set; }
    public string Location { get; set; }
    public string Days { get; set; }
}

public class Instructor
{
    public Instructor()
    {
        this.Courses = new List<Course>();
    }

    // Primary key
    public int InstructorID { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
    public System.DateTime HireDate { get; set; }

    // Navigation properties
    public virtual ICollection<Course> Courses { get; private set; }
}

public class OfficeAssignment
{
    // Specifying InstructorID as a primary
    [Key()]
    public Int32 InstructorID { get; set; }

    public string Location { get; set; }

    // When Entity Framework sees Timestamp attribute
    // it configures ConcurrencyCheck and DatabaseGeneratedPattern=Computed.
    [Timestamp]
    public Byte[] Timestamp { get; set; }

    // Navigation property
    public virtual Instructor Instructor { get; set; }
}
```  
