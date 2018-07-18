---
title: Convenzioni del primo codice - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
caps.latest.revision: 4
ms.openlocfilehash: b124a034ba900780cc4d7e1354408cd3995e874e
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120813"
---
# <a name="code-first-conventions"></a><span data-ttu-id="cde37-102">Convenzioni del primo codice</span><span class="sxs-lookup"><span data-stu-id="cde37-102">Code First Conventions</span></span>
<span data-ttu-id="cde37-103">Code First consente di descrivere un modello utilizzando le classi di c# o Visual Basic .NET.</span><span class="sxs-lookup"><span data-stu-id="cde37-103">Code First enables you to describe a model by using C# or Visual Basic .NET classes.</span></span> <span data-ttu-id="cde37-104">La forma di base del modello viene rilevata usando convenzioni.</span><span class="sxs-lookup"><span data-stu-id="cde37-104">The basic shape of the model is detected by using conventions.</span></span> <span data-ttu-id="cde37-105">Le convenzioni sono set di regole che consentono di configurare automaticamente un modello concettuale basato sulle definizioni di classe quando si lavora con Code First.</span><span class="sxs-lookup"><span data-stu-id="cde37-105">Conventions are sets of rules that are used to automatically configure a conceptual model based on class definitions when working with Code First.</span></span> <span data-ttu-id="cde37-106">Le convenzioni sono definite nello spazio dei nomi System.Data.Entity.ModelConfiguration.Conventions.</span><span class="sxs-lookup"><span data-stu-id="cde37-106">The conventions are defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>  

<span data-ttu-id="cde37-107">È possibile configurare ulteriormente il modello utilizzando le annotazioni dei dati o l'API fluent.</span><span class="sxs-lookup"><span data-stu-id="cde37-107">You can further configure your model by using data annotations or the fluent API.</span></span> <span data-ttu-id="cde37-108">La precedenza viene assegnata alla configurazione tramite l'API fluent, seguito da convenzioni e le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="cde37-108">Precedence is given to configuration through the fluent API followed by data annotations and then conventions.</span></span> <span data-ttu-id="cde37-109">Per altre informazioni, vedere [annotazioni dei dati](~/ef6/modeling/code-first/data-annotations.md), [API Fluent - relazioni](~/ef6/modeling/code-first/fluent/relationships.md), [API Fluent - tipi di & proprietà](~/ef6/modeling/code-first/fluent/types-and-properties.md) e [API Fluent con VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span><span class="sxs-lookup"><span data-stu-id="cde37-109">For more information see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API - Types & Properties](~/ef6/modeling/code-first/fluent/types-and-properties.md) and [Fluent API with VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span></span>  

<span data-ttu-id="cde37-110">È disponibile in un elenco dettagliato delle convenzioni Code First la [documentazione dell'API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="cde37-110">A detailed list of Code First conventions is available in the [API Documentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span> <span data-ttu-id="cde37-111">In questo argomento fornisce una panoramica delle convenzioni utilizzate da Code First.</span><span class="sxs-lookup"><span data-stu-id="cde37-111">This topic provides an overview of the conventions used by Code First.</span></span>  

## <a name="type-discovery"></a><span data-ttu-id="cde37-112">Individuazione del tipo</span><span class="sxs-lookup"><span data-stu-id="cde37-112">Type Discovery</span></span>  

<span data-ttu-id="cde37-113">Quando si usa lo sviluppo Code First è in genere iniziare la scrittura di classi .NET Framework che definiscono il modello concettuale (dominio).</span><span class="sxs-lookup"><span data-stu-id="cde37-113">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="cde37-114">Oltre a definire le classi, è anche necessario consentire **DbContext** sapere quali tipi di cui si desidera includere nel modello.</span><span class="sxs-lookup"><span data-stu-id="cde37-114">In addition to defining the classes, you also need to let **DbContext** know which types you want to include in the model.</span></span> <span data-ttu-id="cde37-115">A tale scopo, si definisce una classe del contesto da cui deriva **DbContext** ed espone **DbSet** le proprietà per i tipi che si desidera far parte del modello.</span><span class="sxs-lookup"><span data-stu-id="cde37-115">To do this, you define a context class that derives from **DbContext** and exposes **DbSet** properties for the types that you want to be part of the model.</span></span> <span data-ttu-id="cde37-116">Codice prima di tutto includerà questi tipi e anche effettuerà il pull in qualsiasi tipo riferimento, anche se i tipi di riferimento sono definiti in un assembly diverso.</span><span class="sxs-lookup"><span data-stu-id="cde37-116">Code First will include these types and also will pull in any referenced types, even if the referenced types are defined in a different assembly.</span></span>  

<span data-ttu-id="cde37-117">Se i tipi di partecipano a una gerarchia di ereditarietà, è sufficiente definire un **DbSet** proprietà per la classe di base e i tipi derivati sarà incluso automaticamente, se sono dello stesso assembly come classe di base.</span><span class="sxs-lookup"><span data-stu-id="cde37-117">If your types participate in an inheritance hierarchy, it is enough to define a **DbSet** property for the base class, and the derived types will be automatically included, if they are in the same assembly as the base class.</span></span>  

<span data-ttu-id="cde37-118">Nell'esempio seguente, è presente un solo **DbSet** definita nella proprietà di **SchoolEntities** classe (**reparti**).</span><span class="sxs-lookup"><span data-stu-id="cde37-118">In the following example, there is only one **DbSet** property defined on the **SchoolEntities** class (**Departments**).</span></span> <span data-ttu-id="cde37-119">Codice prima di tutto utilizza questa proprietà per individuare e il pull di tutti i tipi di riferimento.</span><span class="sxs-lookup"><span data-stu-id="cde37-119">Code First uses this property to discover and pull in any referenced types.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
    public DbSet<Department> Departments { get; set; }
}

public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public string Location { get; set; }
    public string Days { get; set; }
    public System.DateTime Time { get; set; }
}
```  

<span data-ttu-id="cde37-120">Se si desidera escludere un tipo dal modello, usare il **NotMapped** attributo o il **DbModelBuilder.Ignore** API fluent.</span><span class="sxs-lookup"><span data-stu-id="cde37-120">If you want to exclude a type from the model, use the **NotMapped** attribute or the **DbModelBuilder.Ignore** fluent API.</span></span>  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a><span data-ttu-id="cde37-121">Convenzione di chiave primaria</span><span class="sxs-lookup"><span data-stu-id="cde37-121">Primary Key Convention</span></span>  

<span data-ttu-id="cde37-122">Codice prima di tutto deduce che una proprietà è una chiave primaria se una proprietà in una classe è denominata "ID" (non maiuscole / minuscole) o il nome della classe seguito da "ID".</span><span class="sxs-lookup"><span data-stu-id="cde37-122">Code First infers that a property is a primary key if a property on a class is named “ID” (not case sensitive), or the class name followed by "ID".</span></span> <span data-ttu-id="cde37-123">Se il tipo della proprietà della chiave primaria è numerico o GUID e verrà configurato come una colonna identity.</span><span class="sxs-lookup"><span data-stu-id="cde37-123">If the type of the primary key property is numeric or GUID it will be configured as an identity column.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a><span data-ttu-id="cde37-124">Convenzione di relazione</span><span class="sxs-lookup"><span data-stu-id="cde37-124">Relationship Convention</span></span>  

<span data-ttu-id="cde37-125">In Entity Framework, le proprietà di navigazione offrono un modo per spostarsi in una relazione tra due tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="cde37-125">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span> <span data-ttu-id="cde37-126">Ogni oggetto può disporre di una proprietà di navigazione per ogni relazione di cui fa parte.</span><span class="sxs-lookup"><span data-stu-id="cde37-126">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="cde37-127">Le proprietà di navigazione consentono di esplorare e gestire relazioni in entrambe le direzioni, restituendo un oggetto di riferimento (se la molteplicità è uno o zero-o-uno) o una raccolta (se la molteplicità è molti).</span><span class="sxs-lookup"><span data-stu-id="cde37-127">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="cde37-128">Codice deduce prima di tutto le relazioni basate sulle proprietà di navigazione definite per i tipi.</span><span class="sxs-lookup"><span data-stu-id="cde37-128">Code First infers relationships based on the navigation properties defined on your types.</span></span>  

<span data-ttu-id="cde37-129">Oltre alle proprietà di navigazione, è consigliabile includere le proprietà di chiave esterna sui tipi che rappresentano gli oggetti dipendenti.</span><span class="sxs-lookup"><span data-stu-id="cde37-129">In addition to navigation properties, we recommend that you include foreign key properties on the types that represent dependent objects.</span></span> <span data-ttu-id="cde37-130">Qualsiasi proprietà con lo stesso tipo di dati della proprietà di chiave primaria dell'entità e con un nome che segue uno dei seguenti formati rappresenta una chiave esterna per la relazione: '\<nome di proprietà di navigazione\>\<dell'entità nome della proprietà di chiave primaria\>','\<nome classe principale\>\<il nome di proprietà di chiave primaria\>', o '\<nome proprietà della chiave primaria dell'entità\>'.</span><span class="sxs-lookup"><span data-stu-id="cde37-130">Any property with the same data type as the principal primary key property and with a name that follows one of the following formats represents a foreign key for the relationship: '\<navigation property name\>\<principal primary key property name\>', '\<principal class name\>\<primary key property name\>', or '\<principal primary key property name\>'.</span></span> <span data-ttu-id="cde37-131">Se sono state trovate più corrispondenze quindi ha la precedenza nell'ordine indicato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="cde37-131">If multiple matches are found then precedence is given in the order listed above.</span></span> <span data-ttu-id="cde37-132">Il rilevamento di chiavi esterno non distinzione maiuscole / minuscole.</span><span class="sxs-lookup"><span data-stu-id="cde37-132">Foreign key detection is not case sensitive.</span></span> <span data-ttu-id="cde37-133">Quando viene rilevata una proprietà di chiave esterna, Code First deduce la molteplicità della relazione basata su valori null della chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="cde37-133">When a foreign key property is detected, Code First infers the multiplicity of the relationship based on the nullability of the foreign key.</span></span> <span data-ttu-id="cde37-134">Se la proprietà è nullable, quindi la relazione viene registrata come facoltativa. in caso contrario, viene registrata la relazione in base alle necessità.</span><span class="sxs-lookup"><span data-stu-id="cde37-134">If the property is nullable then the relationship is registered as optional; otherwise the relationship is registered as required.</span></span>  

<span data-ttu-id="cde37-135">Se una chiave esterna per l'entità dipendente non ammette valori null, il Code First imposta eliminazione a catena sulla relazione.</span><span class="sxs-lookup"><span data-stu-id="cde37-135">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="cde37-136">Se una chiave esterna per l'entità dipendente è nullable, Code First nenastaveno eliminazione a catena sulla relazione e quando l'entità viene eliminata la chiave esterna verrà impostata su null.</span><span class="sxs-lookup"><span data-stu-id="cde37-136">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span> <span data-ttu-id="cde37-137">La molteplicità e cascade delete comportamento rilevato da convenzione può essere sottoposto a override tramite l'API fluent.</span><span class="sxs-lookup"><span data-stu-id="cde37-137">The multiplicity and cascade delete behavior detected by convention can be overridden by using the fluent API.</span></span>  

<span data-ttu-id="cde37-138">Nell'esempio seguente le proprietà di navigazione e la chiave esterna vengono usate per definire la relazione tra le classi di reparto e lo stesso corso.</span><span class="sxs-lookup"><span data-stu-id="cde37-138">In the following example the navigation properties and a foreign key are used to define the relationship between the Department and Course classes.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}
```  

> [!NOTE]
> <span data-ttu-id="cde37-139">Se si dispone di più relazioni tra gli stessi tipi (ad esempio, si supponga di definire il **persona** e **libro** classi, in cui il **persona** classe contiene il  **ReviewedBooks** e **AuthoredBooks** le proprietà di navigazione e il **libro** classe contiene la **autore** e  **Revisore** le proprietà di navigazione) è necessario configurare manualmente le relazioni tramite le annotazioni dei dati o l'API fluent.</span><span class="sxs-lookup"><span data-stu-id="cde37-139">If you have multiple relationships between the same types (for example, suppose you define the **Person** and **Book** classes, where the **Person** class contains the **ReviewedBooks** and **AuthoredBooks** navigation properties and the **Book** class contains the **Author** and **Reviewer** navigation properties) you need to manually configure the relationships by using Data Annotations or the fluent API.</span></span> <span data-ttu-id="cde37-140">Per altre informazioni, vedere [annotazioni dei dati - relazioni](~/ef6/modeling/code-first/data-annotations.md) e [API Fluent - relazioni](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="cde37-140">For more information, see [Data Annotations - Relationships](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>  

## <a name="complex-types-convention"></a><span data-ttu-id="cde37-141">Convenzione di tipi complessi</span><span class="sxs-lookup"><span data-stu-id="cde37-141">Complex Types Convention</span></span>  

<span data-ttu-id="cde37-142">Quando Code First rileva una definizione di classe in cui non è possibile dedurre una chiave primaria e in nessuna chiave primaria viene registrata tramite le annotazioni dei dati o l'API fluent, il tipo viene registrato automaticamente come un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="cde37-142">When Code First discovers a class definition where a primary key cannot be inferred, and no primary key is registered through data annotations or the fluent API, then the type is automatically registered as a complex type.</span></span> <span data-ttu-id="cde37-143">Rilevamento del tipo complesso richiede anche che il tipo non dispone di proprietà che fanno riferimento ai tipi di entità e non viene fatto riferimento da una proprietà di raccolta in un altro tipo.</span><span class="sxs-lookup"><span data-stu-id="cde37-143">Complex type detection also requires that the type does not have properties that reference entity types and is not referenced from a collection property on another type.</span></span> <span data-ttu-id="cde37-144">Ha le seguenti definizioni di classe Code First sarebbe in grado di dedurre che **dettagli** è un tipo complesso perché non contiene alcuna chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="cde37-144">Given the following class definitions Code First would infer that **Details** is a complex type because it has no primary key.</span></span>  

``` csharp
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
```  

## <a name="connection-string-convention"></a><span data-ttu-id="cde37-145">Convenzione di stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="cde37-145">Connection String Convention</span></span>  

<span data-ttu-id="cde37-146">Per informazioni sulle convenzioni di che DbContext utilizza per individuare la connessione da usare, vedere [modelli e le connessioni](~/ef6/fundamentals/configuring/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="cde37-146">To learn about the conventions that DbContext uses to discover the connection to use see [Connections and Models](~/ef6/fundamentals/configuring/connection-strings.md).</span></span>  

## <a name="removing-conventions"></a><span data-ttu-id="cde37-147">Rimozione di convenzioni</span><span class="sxs-lookup"><span data-stu-id="cde37-147">Removing Conventions</span></span>  

<span data-ttu-id="cde37-148">È possibile rimuovere una delle convenzioni definite nello spazio dei nomi System.Data.Entity.ModelConfiguration.Conventions.</span><span class="sxs-lookup"><span data-stu-id="cde37-148">You can remove any of the conventions defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span> <span data-ttu-id="cde37-149">L'esempio seguente rimuove **PluralizingTableNameConvention**.</span><span class="sxs-lookup"><span data-stu-id="cde37-149">The following example removes **PluralizingTableNameConvention**.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
     . . .

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention, the generated tables  
        // will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}
```  

## <a name="custom-conventions"></a><span data-ttu-id="cde37-150">Convenzioni personalizzate</span><span class="sxs-lookup"><span data-stu-id="cde37-150">Custom Conventions</span></span>  

<span data-ttu-id="cde37-151">Convenzioni personalizzate sono supportate in Entity Framework 6 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="cde37-151">Custom conventions are supported in EF6 onwards.</span></span> <span data-ttu-id="cde37-152">Per altre informazioni, vedere [convenzioni del primo codice personalizzato](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="cde37-152">For more information see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>
