---
title: API Fluent - relazioni - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
ms.openlocfilehash: e13a1370f46362e0f2ca3743ec5b063ce6f87cbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994762"
---
# <a name="fluent-api---relationships"></a><span data-ttu-id="bb50f-102">API Fluent - relazioni</span><span class="sxs-lookup"><span data-stu-id="bb50f-102">Fluent API - Relationships</span></span>
> [!NOTE]
> <span data-ttu-id="bb50f-103">Questa pagina fornisce informazioni sulla configurazione di relazioni nel modello Code First usando l'API fluent.</span><span class="sxs-lookup"><span data-stu-id="bb50f-103">This page provides information about setting up relationships in your Code First model using the fluent API.</span></span> <span data-ttu-id="bb50f-104">Per informazioni generali sulle relazioni in Entity Framework e su come accedere e modificare dati tramite relazioni, vedere [relazioni di & proprietà di navigazione](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="bb50f-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>  

<span data-ttu-id="bb50f-105">Quando si utilizza Code First, per definire il modello che definisce le classi CLR di dominio.</span><span class="sxs-lookup"><span data-stu-id="bb50f-105">When working with Code First, you define your model by defining your domain CLR classes.</span></span> <span data-ttu-id="bb50f-106">Per impostazione predefinita, Entity Framework Usa le convenzioni Code First per eseguire il mapping di classi per lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="bb50f-106">By default, Entity Framework uses the Code First conventions to map your classes to the database schema.</span></span> <span data-ttu-id="bb50f-107">Se si usano le convenzioni di denominazione di Code First, nella maggior parte dei casi è possibile basarsi su Code First per impostare le relazioni tra le tabelle di base chiavi esterne e le proprietà di navigazione definite nelle classi.</span><span class="sxs-lookup"><span data-stu-id="bb50f-107">If you use the Code First naming conventions, in most cases you can rely on Code First to set up relationships between your tables based on the foreign keys and navigation properties that you define on the classes.</span></span> <span data-ttu-id="bb50f-108">Se non si seguono le convenzioni di quando si definiscono le classi o se si desidera modificare il modo in cui funzionano le convenzioni, è possibile usare l'API fluent o annotazioni dei dati per configurare le classi in modo da Code First può mappare le relazioni tra le tabelle.</span><span class="sxs-lookup"><span data-stu-id="bb50f-108">If you do not follow the conventions when defining your classes, or if you want to change the way the conventions work, you can use the fluent API or data annotations to configure your classes so Code First can map the relationships between your tables.</span></span>  

## <a name="introduction"></a><span data-ttu-id="bb50f-109">Introduzione</span><span class="sxs-lookup"><span data-stu-id="bb50f-109">Introduction</span></span>  

<span data-ttu-id="bb50f-110">Quando si configura una relazione con l'API fluent, si inizia con l'istanza EntityTypeConfiguration e quindi Usa il metodo HasRequired, HasOptional o HasMany per specificare il tipo di relazione che fa parte di questa entità.</span><span class="sxs-lookup"><span data-stu-id="bb50f-110">When configuring a relationship with the fluent API, you start with the EntityTypeConfiguration instance and then use the HasRequired, HasOptional, or HasMany method to specify the type of relationship this entity participates in.</span></span> <span data-ttu-id="bb50f-111">I metodi HasRequired e HasOptional accettano un'espressione lambda che rappresenta una proprietà di navigazione di riferimento.</span><span class="sxs-lookup"><span data-stu-id="bb50f-111">The HasRequired and HasOptional methods take a lambda expression that represents a reference navigation property.</span></span> <span data-ttu-id="bb50f-112">Il metodo HasMany accetta un'espressione lambda che rappresenta una proprietà di navigazione della raccolta.</span><span class="sxs-lookup"><span data-stu-id="bb50f-112">The HasMany method takes a lambda expression that represents a collection navigation property.</span></span> <span data-ttu-id="bb50f-113">È quindi possibile configurare una proprietà di navigazione inversa utilizzando i metodi WithRequired WithOptional e WithMany.</span><span class="sxs-lookup"><span data-stu-id="bb50f-113">You can then configure an inverse navigation property by using the WithRequired, WithOptional, and WithMany methods.</span></span> <span data-ttu-id="bb50f-114">Questi metodi hanno overload che non accettano argomenti e può essere utilizzato per specificare la cardinalità con le esplorazioni unidirezionale.</span><span class="sxs-lookup"><span data-stu-id="bb50f-114">These methods have overloads that do not take arguments and can be used to specify cardinality with unidirectional navigations.</span></span>  

<span data-ttu-id="bb50f-115">È quindi possibile configurare le proprietà di chiave esterna tramite il metodo HasForeignKey.</span><span class="sxs-lookup"><span data-stu-id="bb50f-115">You can then configure foreign key properties by using the HasForeignKey method.</span></span> <span data-ttu-id="bb50f-116">Questo metodo accetta un'espressione lambda che rappresenta la proprietà da utilizzare come chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="bb50f-116">This method takes a lambda expression that represents the property to be used as the foreign key.</span></span>  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a><span data-ttu-id="bb50f-117">Configurazione di una relazione necessaria-a-facoltativo (uno-a-Zero-o-uno)</span><span class="sxs-lookup"><span data-stu-id="bb50f-117">Configuring a Required-to-Optional Relationship (One-to–Zero-or-One)</span></span>  

<span data-ttu-id="bb50f-118">Nell'esempio seguente configura una relazione uno-a-zero-o-uno.</span><span class="sxs-lookup"><span data-stu-id="bb50f-118">The following example configures a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="bb50f-119">Il OfficeAssignment dispone della proprietà InstructorID che è una chiave primaria e una chiave esterna, perché il nome della proprietà non segue la convenzione che il metodo HasKey viene usato per configurare la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="bb50f-119">The OfficeAssignment has the InstructorID property that is a primary key and a foreign key, because the name of the property does not follow the convention the HasKey method is used to configure the primary key.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a><span data-ttu-id="bb50f-120">Configurazione di una relazione in cui entrambe le estremità sono necessari (uno a uno)</span><span class="sxs-lookup"><span data-stu-id="bb50f-120">Configuring a Relationship Where Both Ends Are Required (One-to-One)</span></span>  

<span data-ttu-id="bb50f-121">Nella maggior parte dei casi Entity Framework in grado di dedurre il tipo è dipendente e che è l'entità in una relazione.</span><span class="sxs-lookup"><span data-stu-id="bb50f-121">In most cases Entity Framework can infer which type is the dependent and which is the principal in a relationship.</span></span> <span data-ttu-id="bb50f-122">Tuttavia, quando entrambe le estremità della relazione sono obbligatori o entrambi i lati sono facoltativi Entity Framework non è possibile identificare i dipendenti e un'entità.</span><span class="sxs-lookup"><span data-stu-id="bb50f-122">However, when both ends of the relationship are required or both sides are optional Entity Framework cannot identify the dependent and principal.</span></span> <span data-ttu-id="bb50f-123">Quando sono necessarie entrambe le estremità della relazione, utilizzare WithRequiredPrincipal o WithRequiredDependent dopo il metodo HasRequired.</span><span class="sxs-lookup"><span data-stu-id="bb50f-123">When both ends of the relationship are required, use WithRequiredPrincipal or WithRequiredDependent after the HasRequired method.</span></span> <span data-ttu-id="bb50f-124">Quando entrambe le estremità della relazione sono facoltative, usare WithOptionalPrincipal o WithOptionalDependent dopo il metodo HasOptional.</span><span class="sxs-lookup"><span data-stu-id="bb50f-124">When both ends of the relationship are optional, use WithOptionalPrincipal or WithOptionalDependent after the HasOptional method.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a><span data-ttu-id="bb50f-125">Configurazione di una relazione molti-a-molti</span><span class="sxs-lookup"><span data-stu-id="bb50f-125">Configuring a Many-to-Many Relationship</span></span>  

<span data-ttu-id="bb50f-126">Il codice seguente consente di configurare una relazione molti-a-molti tra i tipi Course e Instructor.</span><span class="sxs-lookup"><span data-stu-id="bb50f-126">The following code configures a many-to-many relationship between the Course and Instructor types.</span></span> <span data-ttu-id="bb50f-127">Nell'esempio seguente, le convenzioni Code First predefiniti vengono utilizzate per creare una tabella di join.</span><span class="sxs-lookup"><span data-stu-id="bb50f-127">In the following example, the default Code First conventions are used to create a join table.</span></span> <span data-ttu-id="bb50f-128">Di conseguenza la tabella CourseInstructor viene creata con colonne Course_CourseID e Instructor_InstructorID.</span><span class="sxs-lookup"><span data-stu-id="bb50f-128">As a result the CourseInstructor table is created with Course_CourseID and Instructor_InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

<span data-ttu-id="bb50f-129">Se si desidera specificare il nome della tabella join e i nomi delle colonne nella tabella è necessario eseguire una configurazione aggiuntiva tramite il metodo Map.</span><span class="sxs-lookup"><span data-stu-id="bb50f-129">If you want to specify the join table name and the names of the columns in the table you need to do additional configuration by using the Map method.</span></span> <span data-ttu-id="bb50f-130">Il codice seguente genera una tabella con colonne CourseID e InstructorID CourseInstructor.</span><span class="sxs-lookup"><span data-stu-id="bb50f-130">The following code generates the CourseInstructor table with CourseID and InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
    .Map(m =>
    {
        m.ToTable("CourseInstructor");
        m.MapLeftKey("CourseID");
        m.MapRightKey("InstructorID");
    });
```  

## <a name="configuring-a-relationship-with-one-navigation-property"></a><span data-ttu-id="bb50f-131">Configurazione di una relazione con una proprietà di navigazione</span><span class="sxs-lookup"><span data-stu-id="bb50f-131">Configuring a Relationship with One Navigation Property</span></span>  

<span data-ttu-id="bb50f-132">Un direzionale (detto anche unidirezionali) avviene quando una proprietà di navigazione viene definita in uno solo delle entità finali della relazione e non su entrambi.</span><span class="sxs-lookup"><span data-stu-id="bb50f-132">A one-directional (also called unidirectional) relationship is when a navigation property is defined on only one of the relationship ends and not on both.</span></span> <span data-ttu-id="bb50f-133">Per convenzione, Code First interpreta sempre una relazione unidirezionale come uno-a-molti.</span><span class="sxs-lookup"><span data-stu-id="bb50f-133">By convention, Code First always interprets a unidirectional relationship as one-to-many.</span></span> <span data-ttu-id="bb50f-134">Ad esempio, se si desidera che una relazione uno a uno tra Instructor e OfficeAssignment, in cui si dispone di una proprietà di navigazione in solo il tipo Instructor, è necessario usare l'API fluent per configurare questa relazione.</span><span class="sxs-lookup"><span data-stu-id="bb50f-134">For example, if you want a one-to-one relationship between Instructor and OfficeAssignment, where you have a navigation property on only the Instructor type, you need to use the fluent API to configure this relationship.</span></span>  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a><span data-ttu-id="bb50f-135">Abilitazione dell'eliminazione a catena</span><span class="sxs-lookup"><span data-stu-id="bb50f-135">Enabling Cascade Delete</span></span>  

<span data-ttu-id="bb50f-136">È possibile configurare cascade delete su una relazione usando il metodo WillCascadeOnDelete.</span><span class="sxs-lookup"><span data-stu-id="bb50f-136">You can configure cascade delete on a relationship by using the WillCascadeOnDelete method.</span></span> <span data-ttu-id="bb50f-137">Se una chiave esterna per l'entità dipendente non ammette valori null, il Code First imposta eliminazione a catena sulla relazione.</span><span class="sxs-lookup"><span data-stu-id="bb50f-137">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="bb50f-138">Se una chiave esterna per l'entità dipendente è nullable, Code First nenastaveno eliminazione a catena sulla relazione e quando l'entità viene eliminata la chiave esterna verrà impostata su null.</span><span class="sxs-lookup"><span data-stu-id="bb50f-138">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span>  

<span data-ttu-id="bb50f-139">È possibile rimuovere queste convenzioni di eliminazione a catena utilizzando:</span><span class="sxs-lookup"><span data-stu-id="bb50f-139">You can remove these cascade delete conventions by using:</span></span>  

<span data-ttu-id="bb50f-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>)</span><span class="sxs-lookup"><span data-stu-id="bb50f-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>()</span></span>  
<span data-ttu-id="bb50f-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>)</span><span class="sxs-lookup"><span data-stu-id="bb50f-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>()</span></span>  

<span data-ttu-id="bb50f-142">Il codice seguente configura la relazione in modo che sia obbligatoria e quindi Disabilita eliminazione a catena.</span><span class="sxs-lookup"><span data-stu-id="bb50f-142">The following code configures the relationship to be required and then disables cascade delete.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a><span data-ttu-id="bb50f-143">Configurazione di una chiave esterna composta</span><span class="sxs-lookup"><span data-stu-id="bb50f-143">Configuring a Composite Foreign Key</span></span>  

<span data-ttu-id="bb50f-144">Se la chiave primaria nel tipo di reparto è costituita da proprietà DepartmentID e Name, si verrà configurata la chiave primaria per il reparto e la chiave esterna sui tipi di corso come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="bb50f-144">If the primary key on the Department type consisted of DepartmentID and Name properties, you would configure the primary key for the Department and the foreign key on the Course types as follows:</span></span>  

``` csharp
// Composite primary key
modelBuilder.Entity<Department>()
.HasKey(d => new { d.DepartmentID, d.Name });

// Composite foreign key
modelBuilder.Entity<Course>()  
    .HasRequired(c => c.Department)  
    .WithMany(d => d.Courses)
    .HasForeignKey(d => new { d.DepartmentID, d.DepartmentName });
```  

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="bb50f-145">Ridenominazione di una chiave esterna che non è definita nel modello</span><span class="sxs-lookup"><span data-stu-id="bb50f-145">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="bb50f-146">Se si sceglie di non definire una chiave esterna nel tipo CLR, ma si desidera specificare il nome deve essere nel database, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="bb50f-146">If you choose not to define a foreign key on the CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a><span data-ttu-id="bb50f-147">Configurazione di un nome di chiave esterno che non segue la convenzione prima del codice</span><span class="sxs-lookup"><span data-stu-id="bb50f-147">Configuring a Foreign Key Name That Does Not Follow the Code First Convention</span></span>  

<span data-ttu-id="bb50f-148">Se la proprietà di chiave esterna nella classe del corso è stata chiamata SomeDepartmentID anziché DepartmentID è presente, è necessario eseguire il comando seguente per specificare che si desidera SomeDepartmentID da utilizzare come chiave esterna:</span><span class="sxs-lookup"><span data-stu-id="bb50f-148">If the foreign key property on the Course class was called SomeDepartmentID instead of DepartmentID, you would need to do the following to specify that you want SomeDepartmentID to be the foreign key:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a><span data-ttu-id="bb50f-149">Modello utilizzato negli esempi</span><span class="sxs-lookup"><span data-stu-id="bb50f-149">Model Used in Samples</span></span>  

<span data-ttu-id="bb50f-150">Il modello Code First seguente viene usato per gli esempi in questa pagina.</span><span class="sxs-lookup"><span data-stu-id="bb50f-150">The following Code First model is used for the samples on this page.</span></span>  

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
