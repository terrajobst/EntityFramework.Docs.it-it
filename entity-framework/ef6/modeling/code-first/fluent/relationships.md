---
title: API Fluent - relazioni - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
ms.openlocfilehash: 05f282c02699f8bf3c71197ac5e01000f1855917
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490467"
---
# <a name="fluent-api---relationships"></a>API Fluent - relazioni
> [!NOTE]
> Questa pagina fornisce informazioni sulla configurazione di relazioni nel modello Code First usando l'API fluent. Per informazioni generali sulle relazioni in Entity Framework e su come accedere e modificare dati tramite relazioni, vedere [relazioni di & proprietà di navigazione](~/ef6/fundamentals/relationships.md).  

Quando si utilizza Code First, per definire il modello che definisce le classi CLR di dominio. Per impostazione predefinita, Entity Framework Usa le convenzioni Code First per eseguire il mapping di classi per lo schema del database. Se si usano le convenzioni di denominazione di Code First, nella maggior parte dei casi è possibile basarsi su Code First per impostare le relazioni tra le tabelle di base chiavi esterne e le proprietà di navigazione definite nelle classi. Se non si seguono le convenzioni di quando si definiscono le classi o se si desidera modificare il modo in cui funzionano le convenzioni, è possibile usare l'API fluent o annotazioni dei dati per configurare le classi in modo da Code First può mappare le relazioni tra le tabelle.  

## <a name="introduction"></a>Introduzione  

Quando si configura una relazione con l'API fluent, si inizia con l'istanza EntityTypeConfiguration e quindi Usa il metodo HasRequired, HasOptional o HasMany per specificare il tipo di relazione che fa parte di questa entità. I metodi HasRequired e HasOptional accettano un'espressione lambda che rappresenta una proprietà di navigazione di riferimento. Il metodo HasMany accetta un'espressione lambda che rappresenta una proprietà di navigazione della raccolta. È quindi possibile configurare una proprietà di navigazione inversa utilizzando i metodi WithRequired WithOptional e WithMany. Questi metodi hanno overload che non accettano argomenti e può essere utilizzato per specificare la cardinalità con le esplorazioni unidirezionale.  

È quindi possibile configurare le proprietà di chiave esterna tramite il metodo HasForeignKey. Questo metodo accetta un'espressione lambda che rappresenta la proprietà da utilizzare come chiave esterna.  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a>Configurazione di una relazione necessaria-a-facoltativo (uno-a-Zero-o-uno)  

Nell'esempio seguente configura una relazione uno-a-zero-o-uno. Il OfficeAssignment dispone della proprietà InstructorID che è una chiave primaria e una chiave esterna, perché il nome della proprietà non segue la convenzione che il metodo HasKey viene usato per configurare la chiave primaria.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a>Configurazione di una relazione in cui entrambe le estremità sono necessari (uno a uno)  

Nella maggior parte dei casi Entity Framework in grado di dedurre il tipo è dipendente e che è l'entità in una relazione. Tuttavia, quando entrambe le estremità della relazione sono obbligatori o entrambi i lati sono facoltativi Entity Framework non è possibile identificare i dipendenti e un'entità. Quando sono necessarie entrambe le estremità della relazione, utilizzare WithRequiredPrincipal o WithRequiredDependent dopo il metodo HasRequired. Quando entrambe le estremità della relazione sono facoltative, usare WithOptionalPrincipal o WithOptionalDependent dopo il metodo HasOptional.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a>Configurazione di una relazione molti-a-molti  

Il codice seguente consente di configurare una relazione molti-a-molti tra i tipi Course e Instructor. Nell'esempio seguente, le convenzioni Code First predefiniti vengono utilizzate per creare una tabella di join. Di conseguenza la tabella CourseInstructor viene creata con colonne Course_CourseID e Instructor_InstructorID.  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

Se si desidera specificare il nome della tabella join e i nomi delle colonne nella tabella è necessario eseguire una configurazione aggiuntiva tramite il metodo Map. Il codice seguente genera una tabella con colonne CourseID e InstructorID CourseInstructor.  

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

## <a name="configuring-a-relationship-with-one-navigation-property"></a>Configurazione di una relazione con una proprietà di navigazione  

Un direzionale (detto anche unidirezionali) avviene quando una proprietà di navigazione viene definita in uno solo delle entità finali della relazione e non su entrambi. Per convenzione, Code First interpreta sempre una relazione unidirezionale come uno-a-molti. Ad esempio, se si desidera che una relazione uno a uno tra Instructor e OfficeAssignment, in cui si dispone di una proprietà di navigazione in solo il tipo Instructor, è necessario usare l'API fluent per configurare questa relazione.  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a>Abilitazione dell'eliminazione a catena  

È possibile configurare cascade delete su una relazione usando il metodo WillCascadeOnDelete. Se una chiave esterna per l'entità dipendente non ammette valori null, il Code First imposta eliminazione a catena sulla relazione. Se una chiave esterna per l'entità dipendente è nullable, Code First nenastaveno eliminazione a catena sulla relazione e quando l'entità viene eliminata la chiave esterna verrà impostata su null.  

È possibile rimuovere queste convenzioni di eliminazione a catena utilizzando:  

modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>)  
modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>)  

Il codice seguente configura la relazione in modo che sia obbligatoria e quindi Disabilita eliminazione a catena.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a>Configurazione di una chiave esterna composta  

Se la chiave primaria nel tipo di reparto è costituita da proprietà DepartmentID e Name, si verrà configurata la chiave primaria per il reparto e la chiave esterna sui tipi di corso come indicato di seguito:  

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

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Ridenominazione di una chiave esterna che non è definita nel modello  

Se si sceglie di non definire una chiave esterna nel tipo CLR, ma si desidera specificare il nome deve essere nel database, eseguire le operazioni seguenti:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a>Configurazione di un nome di chiave esterno che non segue la convenzione prima del codice  

Se la proprietà di chiave esterna nella classe del corso è stata chiamata SomeDepartmentID anziché DepartmentID è presente, è necessario eseguire il comando seguente per specificare che si desidera SomeDepartmentID da utilizzare come chiave esterna:  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a>Modello utilizzato negli esempi  

Il modello Code First seguente viene usato per gli esempi in questa pagina.  

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
