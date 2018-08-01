---
title: Le relazioni, le proprietà di navigazione e le chiavi esterne - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
caps.latest.revision: 3
ms.openlocfilehash: 28dab25c7d19a117e594b1761f7bc745b684f7b3
ms.sourcegitcommit: 5c2634c546720902fe01935f4fc031d73aa3ebde
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39393763"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a><span data-ttu-id="864ee-102">Le relazioni, le proprietà di navigazione e le chiavi esterne</span><span class="sxs-lookup"><span data-stu-id="864ee-102">Relationships, navigation properties and foreign keys</span></span>
<span data-ttu-id="864ee-103">Questo argomento offre una panoramica del modo in cui Entity Framework gestisce le relazioni tra entità.</span><span class="sxs-lookup"><span data-stu-id="864ee-103">This topic gives an overview of how Entity Framework manages relationships between entities.</span></span> <span data-ttu-id="864ee-104">Offre inoltre alcune indicazioni su come eseguire il mapping e modificare le relazioni.</span><span class="sxs-lookup"><span data-stu-id="864ee-104">It also gives some guidance on how to map and manipulate relationships.</span></span>

## <a name="relationships-in-ef"></a><span data-ttu-id="864ee-105">Relazioni in Entity Framework</span><span class="sxs-lookup"><span data-stu-id="864ee-105">Relationships in EF</span></span>

<span data-ttu-id="864ee-106">Nei database relazionali, le relazioni (dette anche associazioni) tra le tabelle vengono definite tramite le chiavi esterne.</span><span class="sxs-lookup"><span data-stu-id="864ee-106">In relational databases, relationships (also called associations) between tables are defined through foreign keys.</span></span> <span data-ttu-id="864ee-107">Una chiave esterna (FK) è una colonna o combinazione di colonne utilizzato per stabilire e applicare un collegamento tra i dati in due tabelle.</span><span class="sxs-lookup"><span data-stu-id="864ee-107">A foreign key (FK) is a column or combination of columns that is used to establish and enforce a link between the data in two tables.</span></span> <span data-ttu-id="864ee-108">In genere esistono tre tipi di relazioni: uno a uno, uno-a-molti e molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="864ee-108">There are generally three types of relationships: one-to-one, one-to-many, and many-to-many.</span></span> <span data-ttu-id="864ee-109">In una relazione uno-a-molti, la chiave esterna viene definita sulla tabella che rappresenta il lato molti della relazione.</span><span class="sxs-lookup"><span data-stu-id="864ee-109">In a one-to-many relationship, the foreign key is defined on the table that represents the many end of the relationship.</span></span> <span data-ttu-id="864ee-110">La relazione molti-a-molti implica la definizione di una terza tabella (definita tabella giunzione o join), la cui chiave primaria è costituita da chiavi esterne da entrambe le tabelle correlate.</span><span class="sxs-lookup"><span data-stu-id="864ee-110">The many-to-many relationship involves defining a third table (called a junction or join table), whose primary key is composed of the foreign keys from both related tables.</span></span> <span data-ttu-id="864ee-111">In una relazione uno a uno, la chiave primaria agisce anche come chiave esterna e non esiste alcuna colonna chiave esterna separata per entrambe le tabelle.</span><span class="sxs-lookup"><span data-stu-id="864ee-111">In a one-to-one relationship, the primary key acts additionally as a foreign key and there is no separate foreign key column for either table.</span></span>

<span data-ttu-id="864ee-112">L'immagine seguente mostra due tabelle che fanno parte di relazione uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="864ee-112">The following image shows two tables that participate in one-to-many relationship.</span></span> <span data-ttu-id="864ee-113">Il **Course** tabella è la tabella dipendente perché contiene la **DepartmentID** colonna che si collega al **reparto** tabella.</span><span class="sxs-lookup"><span data-stu-id="864ee-113">The **Course** table is the dependent table because it contains the **DepartmentID** column that links it to the **Department** table.</span></span>

![Database2](~/ef6/media/database2.png)

<span data-ttu-id="864ee-115">Entity Framework, un'entità può essere associata ad altre entità tramite un'associazione o la relazione.</span><span class="sxs-lookup"><span data-stu-id="864ee-115">In Entity Framework, an entity can be related to other entities through an association or relationship.</span></span> <span data-ttu-id="864ee-116">Ogni relazione contiene due entità finali che descrivono il tipo di entità e la molteplicità del tipo (uno, zero-o-uno o molti) per le due entità nella relazione.</span><span class="sxs-lookup"><span data-stu-id="864ee-116">Each relationship contains two ends that describe the entity type and the multiplicity of the type (one, zero-or-one, or many) for the two entities in that relationship.</span></span> <span data-ttu-id="864ee-117">La relazione che potrebbero essere regolata da un vincolo referenziale, che descrive quale entità finale nella relazione riveste il ruolo principale e che è un ruolo dipendente.</span><span class="sxs-lookup"><span data-stu-id="864ee-117">The relationship may be governed by a referential constraint, which describes which end in the relationship is a principal role and which is a dependent role.</span></span>

<span data-ttu-id="864ee-118">Le proprietà di navigazione consentono di passare un'associazione tra due tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="864ee-118">Navigation properties provide a way to navigate an association between two entity types.</span></span> <span data-ttu-id="864ee-119">Ogni oggetto può disporre di una proprietà di navigazione per ogni relazione di cui fa parte.</span><span class="sxs-lookup"><span data-stu-id="864ee-119">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="864ee-120">Le proprietà di navigazione consentono di esplorare e gestire relazioni in entrambe le direzioni, restituendo un oggetto di riferimento (se la molteplicità è uno o zero-o-uno) o una raccolta (se la molteplicità è molti).</span><span class="sxs-lookup"><span data-stu-id="864ee-120">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="864ee-121">È anche possibile scegliere avere una navigazione unidirezionale; nel qual caso si definiscono le proprietà di navigazione su solo uno dei tipi che fa parte della relazione e non su entrambi.</span><span class="sxs-lookup"><span data-stu-id="864ee-121">You may also choose to have one-way navigation, in which case you define the navigation property on only one of the types that participates in the relationship and not on both.</span></span>

<span data-ttu-id="864ee-122">È consigliabile includere le proprietà nel modello di cui eseguire il mapping a chiavi esterne nel database.</span><span class="sxs-lookup"><span data-stu-id="864ee-122">It is recommended to include properties in the model that map to foreign keys in the database.</span></span> <span data-ttu-id="864ee-123">Con le proprietà di chiave esterna incluse, è possibile creare o modificare una relazione cambiando il valore della chiave esterna in un oggetto dipendente.</span><span class="sxs-lookup"><span data-stu-id="864ee-123">With foreign key properties included, you can create or change a relationship by modifying the foreign key value on a dependent object.</span></span> <span data-ttu-id="864ee-124">Questo tipo di associazione viene definito associazione di chiavi esterne.</span><span class="sxs-lookup"><span data-stu-id="864ee-124">This kind of association is called a foreign key association.</span></span> <span data-ttu-id="864ee-125">Usando le chiavi esterne è sempre più importanti quando si usano entità disconnesse.</span><span class="sxs-lookup"><span data-stu-id="864ee-125">Using foreign keys is even more essential when working with disconnected entities.</span></span> <span data-ttu-id="864ee-126">Si noti che, quando utilizzano 1-a-1 o 1 a 0... le relazioni di 1, non sono presenti colonne di chiave esterna separata, la proprietà di chiave primaria funge da chiave esterna e viene sempre incluso nel modello.</span><span class="sxs-lookup"><span data-stu-id="864ee-126">Note, that when working with 1-to-1 or 1-to-0..1 relationships, there is no separate foreign key column, the primary key property acts as the foreign key and is always included in the model.</span></span>

<span data-ttu-id="864ee-127">Quando le colonne chiave esterna non sono incluse nel modello, le informazioni di associazione viene gestite come un oggetto indipendente.</span><span class="sxs-lookup"><span data-stu-id="864ee-127">When foreign key columns are not included in the model, the association information is managed as an independent object.</span></span> <span data-ttu-id="864ee-128">Le relazioni vengono rilevate tramite riferimenti a oggetti anziché le proprietà di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="864ee-128">Relationships are tracked through object references instead of foreign key properties.</span></span> <span data-ttu-id="864ee-129">Questo tipo di associazione viene chiamato un *associazione indipendente*.</span><span class="sxs-lookup"><span data-stu-id="864ee-129">This type of association is called an *independent association*.</span></span> <span data-ttu-id="864ee-130">Il modo più comune per modificare un' *associazione indipendente* consiste nel modificare le proprietà di navigazione generate per ogni entità che partecipa all'associazione.</span><span class="sxs-lookup"><span data-stu-id="864ee-130">The most common way to modify an *independent association* is to modify the navigation properties that are generated for each entity that participates in the association.</span></span>

<span data-ttu-id="864ee-131">È possibile scegliere di utilizzare uno o entrambi i tipi di associazioni nel modello.</span><span class="sxs-lookup"><span data-stu-id="864ee-131">You can choose to use one or both types of associations in your model.</span></span> <span data-ttu-id="864ee-132">Tuttavia, se si dispone di una relazione molti-a-molti pura che è connesso da una tabella di join che contiene solo chiavi esterne, il EF utilizzerà un'associazione indipendente per gestire tale relazione molti-a-molti.</span><span class="sxs-lookup"><span data-stu-id="864ee-132">However, if you have a pure many-to-many relationship that is connected by a join table that contains only foreign keys, the EF will use an independent association to manage such many-to-many relationship.</span></span>   

<span data-ttu-id="864ee-133">L'immagine seguente illustra un modello concettuale con Entity Framework Designer è stato creato.</span><span class="sxs-lookup"><span data-stu-id="864ee-133">The following image shows a conceptual model that was created with the Entity Framework Designer.</span></span> <span data-ttu-id="864ee-134">Il modello contiene due entità che partecipano alla relazione uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="864ee-134">The model contains two entities that participate in one-to-many relationship.</span></span> <span data-ttu-id="864ee-135">Entrambe le entità dispongono di proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="864ee-135">Both entities have navigation properties.</span></span> <span data-ttu-id="864ee-136">**Corsi** è l'entità depend e ha il **DepartmentID** proprietà di chiave esterna definita.</span><span class="sxs-lookup"><span data-stu-id="864ee-136">**Course** is the depend entity and has the **DepartmentID** foreign key property defined.</span></span>

![RelationshipEFDesigner](~/ef6/media/relationshipefdesigner.png)

<span data-ttu-id="864ee-138">Il frammento di codice seguente viene illustrato lo stesso modello di cui è stato creato con Code First.</span><span class="sxs-lookup"><span data-stu-id="864ee-138">The following code snippet shows the same model that was created with Code First.</span></span>

``` csharp
public class Course
{
  public int CourseID { get; set; }
  public string Title { get; set; }
  public int Credits { get; set; }
  public int DepartmentID { get; set; }
  public virtual Department Department { get; set; }
}

public class Department
{
   public Department()
   {
     this.Course = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a><span data-ttu-id="864ee-139">La configurazione o il mapping di relazioni</span><span class="sxs-lookup"><span data-stu-id="864ee-139">Configuring or mapping relationships</span></span>

<span data-ttu-id="864ee-140">La parte restante di questa pagina illustra come accedere e modificare i dati di utilizzo di relazioni.</span><span class="sxs-lookup"><span data-stu-id="864ee-140">The rest of this page covers how to access and manipulate data using relationships.</span></span> <span data-ttu-id="864ee-141">Per informazioni sulla configurazione di relazioni nel modello, vedere le pagine seguenti.</span><span class="sxs-lookup"><span data-stu-id="864ee-141">For information on setting up relationships in your model, see the following pages.</span></span>

-   <span data-ttu-id="864ee-142">Per configurare le relazioni in Code First, vedere [annotazioni dei dati](~/ef6/modeling/code-first/data-annotations.md) e [API Fluent-relazioni](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="864ee-142">To configure relationships in Code First, see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API – Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>
-   <span data-ttu-id="864ee-143">Per configurare le relazioni mediante Entity Framework Designer, vedere [relazioni con Entity Framework Designer](~/ef6/modeling/designer/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="864ee-143">To configure relationships using the Entity Framework Designer, see [Relationships with the EF Designer](~/ef6/modeling/designer/relationships.md).</span></span>

## <a name="creating-and-modifying-relationships"></a><span data-ttu-id="864ee-144">Creazione e modifica delle relazioni</span><span class="sxs-lookup"><span data-stu-id="864ee-144">Creating and modifying relationships</span></span>

<span data-ttu-id="864ee-145">In un *associazione di chiavi esterne*, quando si modifica la relazione, lo stato di un oggetto dipendente con un EntityState.Unchanged lo stato passa ad EntityState.Modified.</span><span class="sxs-lookup"><span data-stu-id="864ee-145">In a *foreign key association*, when you change the relationship, the state of a dependent object with an EntityState.Unchanged state changes to EntityState.Modified.</span></span> <span data-ttu-id="864ee-146">In una relazione indipendente la modifica della relazione non aggiorna lo stato dell'oggetto dipendente.</span><span class="sxs-lookup"><span data-stu-id="864ee-146">In an independent relationship, changing the relationship does not update the state of the dependent object.</span></span>

<span data-ttu-id="864ee-147">Negli esempi seguenti mostrano come usare le proprietà di chiave esterna e le proprietà di navigazione per associare gli oggetti correlati.</span><span class="sxs-lookup"><span data-stu-id="864ee-147">The following examples show how to use the foreign key properties and navigation properties to associate the related objects.</span></span> <span data-ttu-id="864ee-148">Con associazioni di chiavi esterne, è possibile usare uno dei due metodi per modificare, creare o modificare le relazioni.</span><span class="sxs-lookup"><span data-stu-id="864ee-148">With foreign key associations, you can use either method to change, create, or modify relationships.</span></span> <span data-ttu-id="864ee-149">Con associazioni indipendenti, non è possibile utilizzare la proprietà di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="864ee-149">With independent associations, you cannot use the foreign key property.</span></span>

-   <span data-ttu-id="864ee-150">Assegnando un nuovo valore a una proprietà di chiave esterna, come nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="864ee-150">By assigning a new value to a foreign key property, as in the following example.</span></span>  
    ``` csharp
    course.DepartmentID = newCourse.DepartmentID;
    ```

-   <span data-ttu-id="864ee-151">Il codice seguente viene rimossa una relazione impostando la chiave esterna su **null**.</span><span class="sxs-lookup"><span data-stu-id="864ee-151">The following code removes a relationship by setting the foreign key to **null**.</span></span> <span data-ttu-id="864ee-152">Si noti che la proprietà di chiave esterna deve essere nullable.</span><span class="sxs-lookup"><span data-stu-id="864ee-152">Note, that the foreign key property must be nullable.</span></span>  
    ``` csharp
    course.DepartmentID = null;
    ```  
    >[!NOTE]
    > <span data-ttu-id="864ee-153">Se il riferimento è nello stato added (in questo esempio, l'oggetto course), la proprietà di navigazione di riferimento non essere sincronizzata con i valori di chiave di un nuovo oggetto fino a quando non viene chiamato SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="864ee-153">If the reference is in the added state (in this example, the course object), the reference navigation property will not be synchronized with the key values of a new object until SaveChanges is called.</span></span> <span data-ttu-id="864ee-154">La sincronizzazione non si verifica in quanto il contesto dell'oggetto non contiene chiavi permanenti per gli oggetti aggiunti fino quando questi non vengono salvati.</span><span class="sxs-lookup"><span data-stu-id="864ee-154">Synchronization does not occur because the object context does not contain permanent keys for added objects until they are saved.</span></span> <span data-ttu-id="864ee-155">Se è necessario disporre di nuovi oggetti completamente sincronizzati appena si imposta la relazione, utilizzare uno del methods.\* seguenti</span><span class="sxs-lookup"><span data-stu-id="864ee-155">If you must have new objects fully synchronized as soon as you set the relationship, use one of the following methods.\*</span></span>

-   <span data-ttu-id="864ee-156">Assegnando un nuovo oggetto a una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="864ee-156">By assigning a new object to a navigation property.</span></span> <span data-ttu-id="864ee-157">Il codice seguente crea una relazione tra un corso e una `department`.</span><span class="sxs-lookup"><span data-stu-id="864ee-157">The following code creates a relationship between a course and a `department`.</span></span> <span data-ttu-id="864ee-158">Se gli oggetti vengono connessi al contesto, il `course` viene anche aggiunto al `department.Courses` raccolta ed esterna corrispondente proprietà di chiave sul `course` oggetto viene impostato sul valore di proprietà della chiave del reparto.</span><span class="sxs-lookup"><span data-stu-id="864ee-158">If the objects are attached to the context, the `course` is also added to the `department.Courses` collection, and the corresponding foreign key property on the `course` object is set to the key property value of the department.</span></span>  
    ``` csharp
    course.Department = department;
    ```

 -   <span data-ttu-id="864ee-159">Per eliminare la relazione, impostare la proprietà di navigazione su `null`.</span><span class="sxs-lookup"><span data-stu-id="864ee-159">To delete the relationship, set the navigation property to `null`.</span></span> <span data-ttu-id="864ee-160">Se si lavora con Entity Framework basato su .NET 4.0, quindi l'entità finale correlata deve essere caricato prima di impostare su null.</span><span class="sxs-lookup"><span data-stu-id="864ee-160">If you are working with Entity Framework that is based on .NET 4.0, then the related end needs to be loaded before you set it to null.</span></span> <span data-ttu-id="864ee-161">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="864ee-161">For example:</span></span>  
    ``` chsarp
    context.Entry(course).Reference(c => c.Department).Load();  
    course.Department = null;
    ```  
    <span data-ttu-id="864ee-162">A partire da Entity Framework 5.0, che è basato su .NET 4.5, è possibile impostare la relazione NULL senza caricare l'entità finale correlata.</span><span class="sxs-lookup"><span data-stu-id="864ee-162">Starting with Entity Framework 5.0, that is based on .NET 4.5, you can set the relationship to null without loading the related end.</span></span> <span data-ttu-id="864ee-163">È anche possibile impostare il valore corrente su null utilizzando il metodo seguente.</span><span class="sxs-lookup"><span data-stu-id="864ee-163">You can also set the current value to null using the following method.</span></span>  
    ``` csharp
    context.Entry(course).Reference(c => c.Department).CurrentValue = null;
    ```

-   <span data-ttu-id="864ee-164">Eliminando o aggiungendo un oggetto in una raccolta di entità.</span><span class="sxs-lookup"><span data-stu-id="864ee-164">By deleting or adding an object in an entity collection.</span></span> <span data-ttu-id="864ee-165">Ad esempio, è possibile aggiungere un oggetto di tipo `Course` per il `department.Courses` raccolta.</span><span class="sxs-lookup"><span data-stu-id="864ee-165">For example, you can add an object of type `Course` to the `department.Courses` collection.</span></span> <span data-ttu-id="864ee-166">Questa operazione crea una relazione tra un determinato **course** e un particolare `department`.</span><span class="sxs-lookup"><span data-stu-id="864ee-166">This operation creates a relationship between a particular **course** and a particular `department`.</span></span> <span data-ttu-id="864ee-167">Se gli oggetti vengono connessi al contesto, il riferimento di reparto e la proprietà di chiave esterna sul **course** oggetto verrà impostato su appropriato `department`.</span><span class="sxs-lookup"><span data-stu-id="864ee-167">If the objects are attached to the context, the department reference and the foreign key property on the **course** object will be set to the appropriate `department`.</span></span>  
    ``` csharp
    department.Courses.Add(newCourse);
    ```

- <span data-ttu-id="864ee-168">Tramite il `ChangeRelationshipState` metodo per modificare lo stato della relazione specificata tra due oggetti entità.</span><span class="sxs-lookup"><span data-stu-id="864ee-168">By using the `ChangeRelationshipState` method to change the state of the specified relationship between two entity objects.</span></span> <span data-ttu-id="864ee-169">Questo metodo viene in genere utilizzato quando si lavora con applicazioni a più livelli e un *associazione indipendente* (non può essere utilizzato con un'associazione di chiavi esterne).</span><span class="sxs-lookup"><span data-stu-id="864ee-169">This method is most commonly used when working with N-Tier applications and an *independent association* (it cannot be used with a foreign key association).</span></span> <span data-ttu-id="864ee-170">Inoltre, per usare questo metodo, è necessario eliminare verso il basso per `ObjectContext`, come illustrato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="864ee-170">Also, to use this method you must drop down to `ObjectContext`, as shown in the example below.</span></span>  
<span data-ttu-id="864ee-171">Nell'esempio seguente, è presente una relazione molti-a-molti tra instructors (insegnanti) e i corsi.</span><span class="sxs-lookup"><span data-stu-id="864ee-171">In the following example, there is a many-to-many relationship between Instructors and Courses.</span></span> <span data-ttu-id="864ee-172">Chiama il `ChangeRelationshipState` metodo e passando il `EntityState.Added` parametro, consente la `SchoolContext` sapere che è stata aggiunta una relazione tra i due oggetti:</span><span class="sxs-lookup"><span data-stu-id="864ee-172">Calling the `ChangeRelationshipState` method and passing the `EntityState.Added` parameter, lets the `SchoolContext` know that a relationship has been added between the two objects:</span></span>

``` csharp

       ((IObjectContextAdapter)context).ObjectContext.
                 ObjectStateManager.
                  ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
```

    Note that if you are updating (not just adding) a relationship, you must delete the old relationship after adding the new one:

``` csharp
       ((IObjectContextAdapter)context).ObjectContext.
                  ObjectStateManager.
                  ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a><span data-ttu-id="864ee-173">La sincronizzazione le modifiche tra le chiavi esterne e le proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="864ee-173">Synchronizing the changes between the foreign keys and navigation properties</span></span>

<span data-ttu-id="864ee-174">Quando si modifica la relazione degli oggetti connessi al contesto in uno dei metodi descritti in precedenza, Entity Framework deve mantenere sincronizzati le chiavi esterne, i riferimenti e raccolte. Entity Framework gestisce automaticamente la sincronizzazione (noto anche come relazione correzione verticale) per il poco con proxy.</span><span class="sxs-lookup"><span data-stu-id="864ee-174">When you change the relationship of the objects attached to the context by using one of the methods described above, Entity Framework needs to keep foreign keys, references, and collections in sync. Entity Framework automatically manages this synchronization (also known as relationship fix-up) for the POCO entities with proxies.</span></span> <span data-ttu-id="864ee-175">Per altre informazioni, vedere [uso di proxy di](~/ef6/fundamentals/proxies.md).</span><span class="sxs-lookup"><span data-stu-id="864ee-175">For more information, see [Working with Proxies](~/ef6/fundamentals/proxies.md).</span></span>

<span data-ttu-id="864ee-176">Se si utilizzano entità POCO senza proxy, è necessario assicurarsi che il **DetectChanges** metodo viene chiamato per sincronizzare gli oggetti correlati nel contesto.</span><span class="sxs-lookup"><span data-stu-id="864ee-176">If you are using POCO entities without proxies, you must make sure that the **DetectChanges** method is called to synchronize the related objects in the context.</span></span> <span data-ttu-id="864ee-177">Si noti che le seguenti API attivano automaticamente una **DetectChanges** chiamare.</span><span class="sxs-lookup"><span data-stu-id="864ee-177">Note, that the following APIs automatically trigger a **DetectChanges** call.</span></span>

-   `DbSet.Add`
-   `DbSet.AddRange`
-   `DbSet.Remove`
-   `DbSet.RemoveRange`
-   `DbSet.Find`
-   `DbSet.Local`
-   `DbContext.SaveChanges`
-   `DbSet.Attach`
-   `DbContext.GetValidationErrors`
-   `DbContext.Entry`
-   `DbChangeTracker.Entries`
-   <span data-ttu-id="864ee-178">L'esecuzione di LINQ eseguire una query su un `DbSet`</span><span class="sxs-lookup"><span data-stu-id="864ee-178">Executing a LINQ query against a `DbSet`</span></span>

## <a name="loading-related-objects"></a><span data-ttu-id="864ee-179">Caricamento di oggetti correlati</span><span class="sxs-lookup"><span data-stu-id="864ee-179">Loading related objects</span></span>

<span data-ttu-id="864ee-180">In Entity Framework che si utilizza in genere usare le proprietà di navigazione per caricare entità correlate dall'associazione definita per l'entità restituita.</span><span class="sxs-lookup"><span data-stu-id="864ee-180">In Entity Framework you use most commonly use the navigation properties to load entities that are related to the returned entity by the defined association.</span></span> <span data-ttu-id="864ee-181">Per altre informazioni, vedere [caricamento di oggetti correlati](~/ef6/querying/related-data.md).</span><span class="sxs-lookup"><span data-stu-id="864ee-181">For more information, see [Loading Related Objects](~/ef6/querying/related-data.md).</span></span>

> [!NOTE]
> <span data-ttu-id="864ee-182">In un'associazione di chiavi esterne, quando si carica un'entità finale correlata di un oggetto dipendente, l'oggetto correlato sarà caricato in base al valore della chiave esterna dell'oggetto dipendente attualmente in memoria:</span><span class="sxs-lookup"><span data-stu-id="864ee-182">In a foreign key association, when you load a related end of a dependent object, the related object will be loaded based on the foreign key value of the dependent that is currently in memory:</span></span>

``` csharp
    // Get the course where currently DepartmentID = 1.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

<span data-ttu-id="864ee-183">In un'associazione indipendente viene eseguita una query sull'entità finale correlata di un oggetto dipendente in base al valore della chiave esterna che è attualmente nel database.</span><span class="sxs-lookup"><span data-stu-id="864ee-183">In an independent association, the related end of a dependent object is queried based on the foreign key value that is currently in the database.</span></span> <span data-ttu-id="864ee-184">Tuttavia, se la relazione è stata modificata e la proprietà di riferimento sull'oggetto dipendente punta a un oggetto principale diverso caricato nel contesto dell'oggetto, Entity Framework tenterà di creare una relazione come viene definito nel client.</span><span class="sxs-lookup"><span data-stu-id="864ee-184">However, if the relationship was modified, and the reference property on the dependent object points to a different principal object that is loaded in the object context, Entity Framework will try to create a relationship as it is defined on the client.</span></span>

## <a name="managing-concurrency"></a><span data-ttu-id="864ee-185">Gestione della concorrenza</span><span class="sxs-lookup"><span data-stu-id="864ee-185">Managing concurrency</span></span>

<span data-ttu-id="864ee-186">Nella chiave esterna e le associazioni indipendenti, i controlli di concorrenza si basano su altre proprietà delle entità definite nel modello e le chiavi di entità.</span><span class="sxs-lookup"><span data-stu-id="864ee-186">In both foreign key and independent associations, concurrency checks are based on the entity keys and other entity properties that are defined in the model.</span></span> <span data-ttu-id="864ee-187">Quando si utilizza Entity Framework Designer per creare un modello, impostare il `ConcurrencyMode` dell'attributo **fissa** per specificare che la proprietà deve essere verificata la concorrenza.</span><span class="sxs-lookup"><span data-stu-id="864ee-187">When using the EF Designer to create a model, set the `ConcurrencyMode` attribute to **fixed** to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="864ee-188">Quando si utilizza Code First per definire un modello, usare il `ConcurrencyCheck` annotazione sulle proprietà che si desidera controllare la concorrenza.</span><span class="sxs-lookup"><span data-stu-id="864ee-188">When using Code First to define a model, use the `ConcurrencyCheck` annotation on properties that you want to be checked for concurrency.</span></span> <span data-ttu-id="864ee-189">Quando si lavora con Code First è anche possibile usare il `TimeStamp` annotazione per specificare che la proprietà deve essere verificata la concorrenza.</span><span class="sxs-lookup"><span data-stu-id="864ee-189">When working with Code First you can also use the `TimeStamp` annotation to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="864ee-190">È possibile avere solo la proprietà timestamp in una determinata classe.</span><span class="sxs-lookup"><span data-stu-id="864ee-190">You can have only one timestamp property in a given class.</span></span> <span data-ttu-id="864ee-191">Questa proprietà le mappe codice prima di tutto a un campo che non ammette valori null nel database.</span><span class="sxs-lookup"><span data-stu-id="864ee-191">Code First maps this property to a non-nullable field in the database.</span></span>

<span data-ttu-id="864ee-192">È consigliabile usare sempre l'associazione di chiavi esterne quando si lavora con le entità che fanno parte di controllo della concorrenza e la risoluzione.</span><span class="sxs-lookup"><span data-stu-id="864ee-192">We recommend that you always use the foreign key association when working with entities that participate in concurrency checking and resolution.</span></span>

<span data-ttu-id="864ee-193">Per altre informazioni, vedere [alla gestione dei conflitti di concorrenza](~/ef6/saving/concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="864ee-193">For more information, see [Handling Concurrency Conflicts](~/ef6/saving/concurrency.md).</span></span>

## <a name="working-with-overlapping-keys"></a><span data-ttu-id="864ee-194">Utilizzo di chiavi sovrapposte</span><span class="sxs-lookup"><span data-stu-id="864ee-194">Working with overlapping Keys</span></span>

<span data-ttu-id="864ee-195">Le chiavi sovrapposte sono chiavi composte in cui alcune proprietà nella chiave fanno parte anche di un'altra chiave nell'entità.</span><span class="sxs-lookup"><span data-stu-id="864ee-195">Overlapping keys are composite keys where some properties in the key are also part of another key in the entity.</span></span> <span data-ttu-id="864ee-196">Non è possibile disporre di una chiave sovrapposta in un'associazione indipendente.</span><span class="sxs-lookup"><span data-stu-id="864ee-196">You cannot have an overlapping key in an independent association.</span></span> <span data-ttu-id="864ee-197">Per modificare un'associazione di chiavi esterne che include chiavi sovrapposte, si consiglia di modificare i valori della chiave esterna anziché utilizzare i riferimenti a un oggetto.</span><span class="sxs-lookup"><span data-stu-id="864ee-197">To change a foreign key association that includes overlapping keys, we recommend that you modify the foreign key values instead of using the object references.</span></span>
