---
title: API Fluent - configurazione e Mapping dei tipi e le proprietà - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: 031376d2fc4778e6f0fa2434ab7ccfd45d436c4a
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490196"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a>API Fluent - configurazione e di Mapping dei tipi e proprietà
Quando si lavora con Code First di Entity Framework per eseguire il mapping alle tabelle mediante un set di convenzioni incorporato in Entity Framework le classi POCO è il comportamento predefinito. In alcuni casi, tuttavia, non è possibile o non si desidera seguire queste convenzioni ed è necessario eseguire il mapping di entità a un valore diverso da ciò che determinano le convenzioni.  

Esistono due modi principali, è possibile configurare Entity Framework per utilizzare un valore diverso da convenzioni, vale a dire [annotazioni](~/ef6/modeling/code-first/data-annotations.md) o l'API fluent EFs. Le annotazioni coprono solo un sottoinsieme delle funzionalità API fluent, pertanto non esistono gli scenari di mapping che non possono essere ottenuti utilizzando le annotazioni. Questo articolo è progettato per illustrare come usare l'API fluent per configurare le proprietà.  

L'API Office fluent code first in genere avviene eseguendo l'override di [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) metodo nella classe derivata [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx). Gli esempi seguenti sono progettati per mostrare come eseguire diverse attività con l'api fluent e consentono di copiare il codice di e personalizzarlo in base alle proprie del modello, se si vuole visualizzare il modello che può essere utilizzate con come-viene quindi viene fornito alla fine di questo articolo.  

## <a name="model-wide-settings"></a>Impostazioni a livello di modello  

### <a name="default-schema-ef6-onwards"></a>Schema predefinito (6 e versioni successive)  

A partire da Entity Framework 6 è possibile utilizzare il metodo HasDefaultSchema su DbModelBuilder per specificare lo schema del database da utilizzare per tutte le tabelle, stored procedure e così via. Verrà ignorata per tutti gli oggetti che è configurato in modo esplicito uno schema diverso per questa impostazione predefinita.  

``` csharp
modelBuilder.HasDefaultSchema(“sales”);
```  

### <a name="custom-conventions-ef6-onwards"></a>Convenzioni personalizzate (6 e versioni successive)  

A partire da Entity Framework 6 è possibile creare proprie convenzioni che integrano quelli inclusi in Code First. Per altre informazioni, vedere [convenzioni del primo codice personalizzato](~/ef6/modeling/code-first/conventions/custom.md).  

## <a name="property-mapping"></a>Mapping di proprietà  

Il [proprietà](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) metodo viene usato per configurare gli attributi per ogni proprietà che appartengono a un'entità o un tipo complesso. Il metodo di proprietà viene utilizzato per ottenere un oggetto di configurazione per una determinata proprietà. Le opzioni per l'oggetto di configurazione sono specifiche per il tipo di configurato; Ad esempio, è disponibile solo per le proprietà della stringa IsUnicode.  

### <a name="configuring-a-primary-key"></a>Configurazione di una chiave primaria  

La convenzione di Entity Framework per le chiavi primarie è:  

1. La classe definisce una proprietà il cui nome è "ID" o "Id"  
2. o un nome di classe seguito da "ID" o "Id"  

Per impostare in modo esplicito una proprietà sia una chiave primaria, è possibile usare il metodo HasKey. Nell'esempio seguente, il metodo HasKey consente di configurare la chiave primaria InstructorID sul tipo OfficeAssignment.  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a>Configurazione di una chiave primaria composta  

L'esempio seguente configura le proprietà DepartmentID e il nome da chiave primaria composta del tipo di reparto.  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a>Se si disattiva l'identità per le chiavi primarie numerico  

Nell'esempio seguente imposta le proprietà DepartmentID System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None per indicare che il valore non viene generato dal database.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a>Specifica la lunghezza massima consentita in una proprietà  

Nell'esempio seguente, la proprietà Name deve essere non più di 50 caratteri. Se si imposta il valore più di 50 caratteri, si otterrà un [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) eccezione. Se il primo codice crea un database da questo modello verrà impostato anche la lunghezza massima della colonna nome fino a 50 caratteri.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a>Configurare le proprietà come obbligatoria  

Nell'esempio seguente, la proprietà Name è obbligatoria. Se non si specifica il nome, si otterrà un'eccezione DbEntityValidationException. Se il primo codice crea un database da questo modello la colonna utilizzata per archiviare questa proprietà in genere sarà non nullable.  

> [!NOTE]
> In alcuni casi potrebbe non essere possibile per la colonna nel database sia non nullable, anche se la proprietà è obbligatoria. Ad esempio, quando tramite una data di strategia ereditarietà tabella per gerarchia per i tipi più viene archiviato in una singola tabella. Se un tipo derivato include una proprietà obbligatoria la colonna può essere reso non nullable poiché non tutti i tipi nella gerarchia disporrà di questa proprietà.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a>La configurazione di un indice su una o più proprietà  

> [!NOTE]
> **EF6.1 e versioni successive solo** -attributo l'indice è stata introdotta in Entity Framework 6.1. Se si usa una versione precedente le informazioni contenute in questa sezione non si applicano.  

Creazione di indici non è supportata in modo nativo dall'API Fluent, ma è possibile fare uso del supporto per **IndexAttribute** tramite l'API Fluent. Gli attributi dell'indice vengono elaborati tramite l'inclusione di un'annotazione di modello del modello che viene quindi convertito in un indice nel database in un secondo momento nella pipeline. È possibile aggiungere manualmente questi stessi annotazioni tramite l'API Fluent.  

Il modo più semplice per eseguire questa operazione consiste nel creare un'istanza di **IndexAttribute** che contiene tutte le impostazioni per il nuovo indice. È quindi possibile creare un'istanza di **IndexAnnotation** che è un tipo specifico di Entity Framework che convertirà il **IndexAttribute** impostazioni in un'annotazione di modello che possono essere archiviati nel modello di Entity Framework. Si può quindi essere passati per il **HasColumnAnnotation** metodo nell'API Fluent, che specifica il nome **indice** per l'annotazione.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

Per un elenco completo delle impostazioni disponibili nel **IndexAttribute**, vedere la *indice* sezione [annotazioni di dati Code First](~/ef6/modeling/code-first/data-annotations.md). Ciò include il nome dell'indice di personalizzazione, creazione di indici univoci e la creazione degli indici a più colonne.  

È possibile specificare più annotazioni di indice in una singola proprietà, passando una matrice di **IndexAttribute** al costruttore della **IndexAnnotation**.  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a>Specifica di non eseguire il mapping di una proprietà CLR a una colonna nel Database  

Nell'esempio seguente viene illustrato come specificare che una proprietà su un tipo CLR non è mappata a una colonna nel database.  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a>Mapping di una proprietà CLR a una colonna specifica nel Database  

Nell'esempio seguente esegue il mapping alla colonna del database DepartmentName la proprietà nome CLR.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Ridenominazione di una chiave esterna che non è definita nel modello  

Se si sceglie di non definire una chiave esterna su un tipo CLR, ma si desidera specificare il nome deve essere nel database, eseguire le operazioni seguenti:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a>La configurazione se una proprietà stringa supporta il contenuto Unicode  

Per impostazione predefinita le stringhe sono Unicode (nvarchar in SQL Server). È possibile usare il metodo IsUnicode per specificare che deve essere una stringa di tipo varchar.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a>Configurazione del tipo di dati di una colonna di Database  

Il [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) metodo attiva il mapping alle diverse rappresentazioni dello stesso tipo di base. Con questo metodo non consente di eseguire alcuna conversione dei dati in fase di esecuzione. Si noti che IsUnicode è il modo migliore di colonne di impostazione per varchar, perché è indipendente dal database.  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a>Configurazione delle proprietà per un tipo complesso  

Esistono due modi per configurare le proprietà scalari in un tipo complesso.  

È possibile chiamare proprietà su ComplexTypeConfiguration.  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

È anche possibile usare la notazione del punto per accedere a una proprietà di un tipo complesso.  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a>Configurazione di una proprietà da utilizzare come Token di concorrenza ottimistica  

Per specificare che una proprietà in un'entità rappresenta un token di concorrenza, è possibile usare l'attributo ConcurrencyCheck o il metodo IsConcurrencyToken.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

È inoltre possibile utilizzare il metodo IsRowVersion per configurare le proprietà in modo che sia una versione di riga nel database. Impostazione della proprietà sia che una versione di riga viene configurato automaticamente in modo che sia un token di concorrenza ottimistica.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a>Mapping dei tipi  

### <a name="specifying-that-a-class-is-a-complex-type"></a>Specifica che una classe è un tipo complesso  

Per convenzione, un tipo che non dispone di alcuna chiave primaria specificato viene considerato un tipo complesso. Esistono alcuni scenari in cui Code First non rileverà un tipo complesso (ad esempio, se è presente una proprietà denominata ID, ma non significa che per poter essere una chiave primaria). In questi casi, è necessario usare l'API fluent specificare in modo esplicito che un tipo è un tipo complesso.  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a>Specifica di non eseguire il mapping di un tipo di entità di Common Language Runtime a una tabella nel Database  

Nell'esempio seguente viene illustrato come impedire che un tipo CLR mappato a una tabella nel database.  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a>Mapping di un tipo di entità a una tabella specifica nel Database  

Tutte le proprietà del reparto verranno mappate a colonne in una tabella denominata t_ Department.  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

È anche possibile specificare il nome dello schema simile al seguente:  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a>Mapping dell'ereditarietà tabella Per gerarchia (TPH)  

Nello scenario di mapping tabella per gerarchia, tutti i tipi in una gerarchia di ereditarietà sono mappati a una singola tabella. Una colonna discriminatore viene utilizzata per identificare il tipo di ogni riga. Quando si crea il modello Code First, della tabella per gerarchia è la strategia predefinita per i tipi che fanno parte della gerarchia di ereditarietà. Per impostazione predefinita, la colonna discriminatore viene aggiunta alla tabella con il nome "Discriminator" e il nome del tipo CLR di ogni tipo nella gerarchia viene usato per i valori del discriminatore. È possibile modificare il comportamento predefinito tramite l'API fluent.  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a>Mapping dell'ereditarietà tabella Per tipo TPT)  

Nello scenario di mapping tabella per tipo, tutti i tipi sono mappati a singole tabelle. Le proprietà che appartengono esclusivamente a un tipo di base o derivato sono archiviate in una tabella che viene mappata a quel tipo. Tabelle in cui eseguire il mapping a tipi derivati anche archiviano una chiave esterna che unisce in join la tabella derivata con la tabella di base.  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a>Mapping di ereditarietà della classe tabella Per tipo concreto (TP)  

Nello scenario di mapping TPC, tutti i tipi non astratti nella gerarchia vengono mappati a singole tabelle. Le tabelle in cui eseguire il mapping a classi derivate hanno nessuna relazione con la tabella che esegue il mapping alla classe di base nel database. Tutte le proprietà di una classe, incluse le proprietà ereditate, vengono mappate alle colonne della tabella corrispondente.  

Il metodo MapInheritedProperties consente di configurare ogni tipo derivato. Il metodo MapInheritedProperties esegue un nuovo mapping di tutte le proprietà che sono state ereditate dalla classe di base a nuove colonne nella tabella per la classe derivata.  

> [!NOTE]
> Si noti che poiché non condividono le tabelle che fanno parte di una gerarchia di ereditarietà TPC esiste una chiave primaria saranno le chiavi di entità duplicate durante l'inserimento in tabelle in cui vengono eseguito il mapping alle sottoclassi, in presenza di valori del database generato con il valore di inizializzazione identity stessa. Per risolvere questo problema è possibile specificare un valore di inizializzazione iniziale diverso per ogni tabella o disattivare l'identità sulla proprietà della chiave primaria. Identità è il valore predefinito per le proprietà di integer chiave quando si lavora con Code First.  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a>Mapping delle proprietà di un tipo di entità a più tabelle nel Database (suddivisione di entità)  

Separazione delle entità consente le proprietà di un tipo di entità siano distribuiti in più tabelle. Nell'esempio seguente, l'entità Department è suddiviso in due tabelle: reparto e DepartmentDetails. Separazione delle entità Usa più chiamate al metodo Map per eseguire il mapping di un subset delle proprietà a una tabella specifica.  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a>Mapping di più tipi di entità a una tabella nel Database (suddivisione di tabelle)  

Nell'esempio seguente esegue il mapping di due tipi di entità che condividono una chiave primaria a una tabella.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a>Mapping di un tipo di entità alle Stored procedure Insert/Update/Delete (6 e versioni successive)  

A partire da Entity Framework 6 è possibile eseguire il mapping di un'entità per l'uso di stored procedure per insert, update e delete. Per altre informazioni, vedere [primo Insert/Update/Delete. Stored procedure codice](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).  

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
