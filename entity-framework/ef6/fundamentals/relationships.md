---
title: Le relazioni, le proprietà di navigazione e le chiavi esterne - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: 8292ae7af8d760240715854611d92ab340bf1ca7
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319192"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a>Le relazioni, le proprietà di navigazione e le chiavi esterne
Questo argomento offre una panoramica del modo in cui Entity Framework gestisce le relazioni tra entità. Offre inoltre alcune indicazioni su come eseguire il mapping e modificare le relazioni.

## <a name="relationships-in-ef"></a>Relazioni in Entity Framework

Nei database relazionali, le relazioni (dette anche associazioni) tra le tabelle vengono definite tramite le chiavi esterne. Una chiave esterna (FK) è una colonna o combinazione di colonne utilizzato per stabilire e applicare un collegamento tra i dati in due tabelle. In genere esistono tre tipi di relazioni: uno a uno, uno-a-molti e molti-a-molti. In una relazione uno-a-molti, la chiave esterna viene definita sulla tabella che rappresenta il lato molti della relazione. La relazione molti-a-molti implica la definizione di una terza tabella (definita tabella giunzione o join), la cui chiave primaria è costituita da chiavi esterne da entrambe le tabelle correlate. In una relazione uno a uno, la chiave primaria agisce anche come chiave esterna e non esiste alcuna colonna chiave esterna separata per entrambe le tabelle.

L'immagine seguente mostra due tabelle che fanno parte di relazione uno-a-molti. Il **Course** tabella è la tabella dipendente perché contiene la **DepartmentID** colonna che si collega al **reparto** tabella.

![Tabelle di reparto e Course](~/ef6/media/database2.png)

Entity Framework, un'entità può essere associata ad altre entità tramite un'associazione o la relazione. Ogni relazione contiene due entità finali che descrivono il tipo di entità e la molteplicità del tipo (uno, zero-o-uno o molti) per le due entità nella relazione. La relazione che potrebbero essere regolata da un vincolo referenziale, che descrive quale entità finale nella relazione riveste il ruolo principale e che è un ruolo dipendente.

Le proprietà di navigazione consentono di passare un'associazione tra due tipi di entità. Ogni oggetto può disporre di una proprietà di navigazione per ogni relazione di cui fa parte. Le proprietà di navigazione consentono di esplorare e gestire relazioni in entrambe le direzioni, restituendo un oggetto di riferimento (se la molteplicità è uno o zero-o-uno) o una raccolta (se la molteplicità è molti). È anche possibile scegliere avere una navigazione unidirezionale; nel qual caso si definiscono le proprietà di navigazione su solo uno dei tipi che fa parte della relazione e non su entrambi.

È consigliabile includere le proprietà nel modello di cui eseguire il mapping a chiavi esterne nel database. Con le proprietà di chiave esterna incluse, è possibile creare o modificare una relazione cambiando il valore della chiave esterna in un oggetto dipendente. Questo tipo di associazione viene definito associazione di chiavi esterne. Usando le chiavi esterne è sempre più importanti quando si usano entità disconnesse. Si noti che, quando utilizzano 1-a-1 o 1 a 0... le relazioni di 1, non sono presenti colonne di chiave esterna separata, la proprietà di chiave primaria funge da chiave esterna e viene sempre incluso nel modello.

Quando le colonne chiave esterna non sono incluse nel modello, le informazioni di associazione viene gestite come un oggetto indipendente. Le relazioni vengono rilevate tramite riferimenti a oggetti anziché le proprietà di chiave esterna. Questo tipo di associazione viene chiamato un *associazione indipendente*. Il modo più comune per modificare un' *associazione indipendente* consiste nel modificare le proprietà di navigazione generate per ogni entità che partecipa all'associazione.

È possibile scegliere di utilizzare uno o entrambi i tipi di associazioni nel modello. Tuttavia, se si dispone di una relazione molti-a-molti pura che è connesso da una tabella di join che contiene solo chiavi esterne, il EF utilizzerà un'associazione indipendente per gestire tale relazione molti-a-molti.   

L'immagine seguente illustra un modello concettuale con Entity Framework Designer è stato creato. Il modello contiene due entità che partecipano alla relazione uno-a-molti. Entrambe le entità dispongono di proprietà di navigazione. **Corsi** è l'entità depend e ha il **DepartmentID** proprietà di chiave esterna definita.

![Tabelle di reparto e Course con le proprietà di navigazione](~/ef6/media/relationshipefdesigner.png)

Il frammento di codice seguente viene illustrato lo stesso modello di cui è stato creato con Code First.

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
     this.Courses = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a>La configurazione o il mapping di relazioni

La parte restante di questa pagina illustra come accedere e modificare i dati di utilizzo di relazioni. Per informazioni sulla configurazione di relazioni nel modello, vedere le pagine seguenti.

-   Per configurare le relazioni in Code First, vedere [annotazioni dei dati](~/ef6/modeling/code-first/data-annotations.md) e [API Fluent-relazioni](~/ef6/modeling/code-first/fluent/relationships.md).
-   Per configurare le relazioni mediante Entity Framework Designer, vedere [relazioni con Entity Framework Designer](~/ef6/modeling/designer/relationships.md).

## <a name="creating-and-modifying-relationships"></a>Creazione e modifica delle relazioni

In un *associazione di chiavi esterne*, quando si modifica la relazione, lo stato di un oggetto dipendente con un `EntityState.Unchanged` lo stato passa ad `EntityState.Modified`. In una relazione indipendente la modifica della relazione non aggiorna lo stato dell'oggetto dipendente.

Negli esempi seguenti mostrano come usare le proprietà di chiave esterna e le proprietà di navigazione per associare gli oggetti correlati. Con associazioni di chiavi esterne, è possibile usare uno dei due metodi per modificare, creare o modificare le relazioni. Con associazioni indipendenti, non è possibile utilizzare la proprietà di chiave esterna.

- Assegnando un nuovo valore a una proprietà di chiave esterna, come nell'esempio seguente.  
  ``` csharp
  course.DepartmentID = newCourse.DepartmentID;
  ```

- Il codice seguente viene rimossa una relazione impostando la chiave esterna su **null**. Si noti che la proprietà di chiave esterna deve essere nullable.  
  ``` csharp
  course.DepartmentID = null;
  ```

  >[!NOTE]
  > Se il riferimento è nello stato added (in questo esempio, l'oggetto course), la proprietà di navigazione di riferimento non essere sincronizzata con i valori di chiave di un nuovo oggetto fino a quando non viene chiamato SaveChanges. La sincronizzazione non si verifica in quanto il contesto dell'oggetto non contiene chiavi permanenti per gli oggetti aggiunti fino quando questi non vengono salvati. Se è necessario disporre di nuovi oggetti completamente sincronizzati appena si imposta la relazione, utilizzare uno del methods.* seguenti

- Assegnando un nuovo oggetto a una proprietà di navigazione. Il codice seguente crea una relazione tra un corso e una `department`. Se gli oggetti vengono connessi al contesto, il `course` viene anche aggiunto al `department.Courses` raccolta ed esterna corrispondente proprietà di chiave sul `course` oggetto viene impostato sul valore di proprietà della chiave del reparto.  
  ``` csharp
  course.Department = department;
  ```

- Per eliminare la relazione, impostare la proprietà di navigazione su `null`. Se si lavora con Entity Framework basato su .NET 4.0, quindi l'entità finale correlata deve essere caricato prima di impostare su null. Ad esempio:   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).Load();
  course.Department = null;
  ```

  A partire da Entity Framework 5.0, che è basato su .NET 4.5, è possibile impostare la relazione NULL senza caricare l'entità finale correlata. È anche possibile impostare il valore corrente su null utilizzando il metodo seguente.   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).CurrentValue = null;
  ```

- Eliminando o aggiungendo un oggetto in una raccolta di entità. Ad esempio, è possibile aggiungere un oggetto di tipo `Course` per il `department.Courses` raccolta. Questa operazione crea una relazione tra un determinato **course** e un particolare `department`. Se gli oggetti vengono connessi al contesto, il riferimento di reparto e la proprietà di chiave esterna sul **course** oggetto verrà impostato su appropriato `department`.  
  ``` csharp
  department.Courses.Add(newCourse);
  ```

- Tramite il `ChangeRelationshipState` metodo per modificare lo stato della relazione specificata tra due oggetti entità. Questo metodo viene in genere utilizzato quando si lavora con applicazioni a più livelli e un *associazione indipendente* (non può essere utilizzato con un'associazione di chiavi esterne). Inoltre, per usare questo metodo, è necessario eliminare verso il basso per `ObjectContext`, come illustrato nell'esempio seguente.  
Nell'esempio seguente, è presente una relazione molti-a-molti tra instructors (insegnanti) e i corsi. Chiama il `ChangeRelationshipState` metodo e passando il `EntityState.Added` parametro, consente la `SchoolContext` sapere che è stata aggiunta una relazione tra i due oggetti:
  ``` csharp

  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
  ```

  Si noti che se si sta aggiornando (non solo aggiungendo) una relazione, è necessario eliminare la relazione precedente dopo l'aggiunta di una nuova:

  ``` csharp
  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
  ```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a>La sincronizzazione le modifiche tra le chiavi esterne e le proprietà di navigazione

Quando si modifica la relazione degli oggetti connessi al contesto in uno dei metodi descritti in precedenza, Entity Framework deve mantenere sincronizzati le chiavi esterne, i riferimenti e raccolte. Entity Framework gestisce automaticamente la sincronizzazione (noto anche come relazione correzione verticale) per il poco con proxy. Per altre informazioni, vedere [uso di proxy di](~/ef6/fundamentals/proxies.md).

Se si utilizzano entità POCO senza proxy, è necessario assicurarsi che il **DetectChanges** metodo viene chiamato per sincronizzare gli oggetti correlati nel contesto. Si noti che le seguenti API attivano automaticamente una **DetectChanges** chiamare.

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
-   L'esecuzione di LINQ eseguire una query su un `DbSet`

## <a name="loading-related-objects"></a>Caricamento di oggetti correlati

In Entity Framework è in genere usare le proprietà di navigazione per caricare entità correlate dall'associazione definita per l'entità restituita. Per altre informazioni, vedere [caricamento di oggetti correlati](~/ef6/querying/related-data.md).

> [!NOTE]
> In un'associazione di chiavi esterne, quando si carica un'entità finale correlata di un oggetto dipendente, l'oggetto correlato sarà caricato in base al valore della chiave esterna dell'oggetto dipendente attualmente in memoria:

``` csharp
    // Get the course where currently DepartmentID = 2.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

In un'associazione indipendente viene eseguita una query sull'entità finale correlata di un oggetto dipendente in base al valore della chiave esterna che è attualmente nel database. Tuttavia, se la relazione è stata modificata e la proprietà di riferimento sull'oggetto dipendente punta a un oggetto principale diverso caricato nel contesto dell'oggetto, Entity Framework tenterà di creare una relazione come viene definito nel client.

## <a name="managing-concurrency"></a>Gestione della concorrenza

Nella chiave esterna e le associazioni indipendenti, i controlli di concorrenza si basano su altre proprietà delle entità definite nel modello e le chiavi di entità. Quando si utilizza Entity Framework Designer per creare un modello, impostare il `ConcurrencyMode` dell'attributo **fissa** per specificare che la proprietà deve essere verificata la concorrenza. Quando si utilizza Code First per definire un modello, usare il `ConcurrencyCheck` annotazione sulle proprietà che si desidera controllare la concorrenza. Quando si lavora con Code First è anche possibile usare il `TimeStamp` annotazione per specificare che la proprietà deve essere verificata la concorrenza. È possibile avere solo la proprietà timestamp in una determinata classe. Questa proprietà le mappe codice prima di tutto a un campo che non ammette valori null nel database.

È consigliabile usare sempre l'associazione di chiavi esterne quando si lavora con le entità che fanno parte di controllo della concorrenza e la risoluzione.

Per altre informazioni, vedere [alla gestione dei conflitti di concorrenza](~/ef6/saving/concurrency.md).

## <a name="working-with-overlapping-keys"></a>Utilizzo di chiavi sovrapposte

Le chiavi sovrapposte sono chiavi composte in cui alcune proprietà nella chiave fanno parte anche di un'altra chiave nell'entità. Non è possibile disporre di una chiave sovrapposta in un'associazione indipendente. Per modificare un'associazione di chiavi esterne che include chiavi sovrapposte, si consiglia di modificare i valori della chiave esterna anziché utilizzare i riferimenti a un oggetto.
