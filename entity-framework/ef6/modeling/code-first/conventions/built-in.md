---
title: Convenzioni del primo codice - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
ms.openlocfilehash: c5fa580879a4b53fed34d94b737988875f38c62c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995526"
---
# <a name="code-first-conventions"></a>Convenzioni del primo codice
Code First consente di descrivere un modello utilizzando le classi di c# o Visual Basic .NET. La forma di base del modello viene rilevata usando convenzioni. Le convenzioni sono set di regole che consentono di configurare automaticamente un modello concettuale basato sulle definizioni di classe quando si lavora con Code First. Le convenzioni sono definite nello spazio dei nomi System.Data.Entity.ModelConfiguration.Conventions.  

È possibile configurare ulteriormente il modello utilizzando le annotazioni dei dati o l'API fluent. La precedenza viene assegnata alla configurazione tramite l'API fluent, seguito da convenzioni e le annotazioni dei dati. Per altre informazioni, vedere [annotazioni dei dati](~/ef6/modeling/code-first/data-annotations.md), [API Fluent - relazioni](~/ef6/modeling/code-first/fluent/relationships.md), [API Fluent - tipi di & proprietà](~/ef6/modeling/code-first/fluent/types-and-properties.md) e [API Fluent con VB.NET](~/ef6/modeling/code-first/fluent/vb.md).  

È disponibile in un elenco dettagliato delle convenzioni Code First la [documentazione dell'API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx). In questo argomento fornisce una panoramica delle convenzioni utilizzate da Code First.  

## <a name="type-discovery"></a>Individuazione del tipo  

Quando si usa lo sviluppo Code First è in genere iniziare la scrittura di classi .NET Framework che definiscono il modello concettuale (dominio). Oltre a definire le classi, è anche necessario consentire **DbContext** sapere quali tipi di cui si desidera includere nel modello. A tale scopo, si definisce una classe del contesto da cui deriva **DbContext** ed espone **DbSet** le proprietà per i tipi che si desidera far parte del modello. Codice prima di tutto includerà questi tipi e anche effettuerà il pull in qualsiasi tipo riferimento, anche se i tipi di riferimento sono definiti in un assembly diverso.  

Se i tipi di partecipano a una gerarchia di ereditarietà, è sufficiente definire un **DbSet** proprietà per la classe di base e i tipi derivati sarà incluso automaticamente, se sono dello stesso assembly come classe di base.  

Nell'esempio seguente, è presente un solo **DbSet** definita nella proprietà di **SchoolEntities** classe (**reparti**). Codice prima di tutto utilizza questa proprietà per individuare e il pull di tutti i tipi di riferimento.  

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

Se si desidera escludere un tipo dal modello, usare il **NotMapped** attributo o il **DbModelBuilder.Ignore** API fluent.  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a>Convenzione di chiave primaria  

Codice prima di tutto deduce che una proprietà è una chiave primaria se una proprietà in una classe è denominata "ID" (non maiuscole / minuscole) o il nome della classe seguito da "ID". Se il tipo della proprietà della chiave primaria è numerico o GUID e verrà configurato come una colonna identity.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a>Convenzione di relazione  

In Entity Framework, le proprietà di navigazione offrono un modo per spostarsi in una relazione tra due tipi di entità. Ogni oggetto può disporre di una proprietà di navigazione per ogni relazione di cui fa parte. Le proprietà di navigazione consentono di esplorare e gestire relazioni in entrambe le direzioni, restituendo un oggetto di riferimento (se la molteplicità è uno o zero-o-uno) o una raccolta (se la molteplicità è molti). Codice deduce prima di tutto le relazioni basate sulle proprietà di navigazione definite per i tipi.  

Oltre alle proprietà di navigazione, è consigliabile includere le proprietà di chiave esterna sui tipi che rappresentano gli oggetti dipendenti. Qualsiasi proprietà con lo stesso tipo di dati della proprietà di chiave primaria dell'entità e con un nome che segue uno dei seguenti formati rappresenta una chiave esterna per la relazione: '\<nome di proprietà di navigazione\>\<dell'entità nome della proprietà di chiave primaria\>','\<nome classe principale\>\<il nome di proprietà di chiave primaria\>', o '\<nome proprietà della chiave primaria dell'entità\>'. Se sono state trovate più corrispondenze quindi ha la precedenza nell'ordine indicato in precedenza. Il rilevamento di chiavi esterno non distinzione maiuscole / minuscole. Quando viene rilevata una proprietà di chiave esterna, Code First deduce la molteplicità della relazione basata su valori null della chiave esterna. Se la proprietà è nullable, quindi la relazione viene registrata come facoltativa. in caso contrario, viene registrata la relazione in base alle necessità.  

Se una chiave esterna per l'entità dipendente non ammette valori null, il Code First imposta eliminazione a catena sulla relazione. Se una chiave esterna per l'entità dipendente è nullable, Code First nenastaveno eliminazione a catena sulla relazione e quando l'entità viene eliminata la chiave esterna verrà impostata su null. La molteplicità e cascade delete comportamento rilevato da convenzione può essere sottoposto a override tramite l'API fluent.  

Nell'esempio seguente le proprietà di navigazione e la chiave esterna vengono usate per definire la relazione tra le classi di reparto e lo stesso corso.  

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
> Se si dispone di più relazioni tra gli stessi tipi (ad esempio, si supponga di definire il **persona** e **libro** classi, in cui il **persona** classe contiene il  **ReviewedBooks** e **AuthoredBooks** le proprietà di navigazione e il **libro** classe contiene la **autore** e  **Revisore** le proprietà di navigazione) è necessario configurare manualmente le relazioni tramite le annotazioni dei dati o l'API fluent. Per altre informazioni, vedere [annotazioni dei dati - relazioni](~/ef6/modeling/code-first/data-annotations.md) e [API Fluent - relazioni](~/ef6/modeling/code-first/fluent/relationships.md).  

## <a name="complex-types-convention"></a>Convenzione di tipi complessi  

Quando Code First rileva una definizione di classe in cui non è possibile dedurre una chiave primaria e in nessuna chiave primaria viene registrata tramite le annotazioni dei dati o l'API fluent, il tipo viene registrato automaticamente come un tipo complesso. Rilevamento del tipo complesso richiede anche che il tipo non dispone di proprietà che fanno riferimento ai tipi di entità e non viene fatto riferimento da una proprietà di raccolta in un altro tipo. Ha le seguenti definizioni di classe Code First sarebbe in grado di dedurre che **dettagli** è un tipo complesso perché non contiene alcuna chiave primaria.  

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

## <a name="connection-string-convention"></a>Convenzione di stringa di connessione  

Per informazioni sulle convenzioni di che DbContext utilizza per individuare la connessione da usare, vedere [modelli e le connessioni](~/ef6/fundamentals/configuring/connection-strings.md).  

## <a name="removing-conventions"></a>Rimozione di convenzioni  

È possibile rimuovere una delle convenzioni definite nello spazio dei nomi System.Data.Entity.ModelConfiguration.Conventions. L'esempio seguente rimuove **PluralizingTableNameConvention**.  

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

## <a name="custom-conventions"></a>Convenzioni personalizzate  

Convenzioni personalizzate sono supportate in Entity Framework 6 e versioni successive. Per altre informazioni, vedere [convenzioni del primo codice personalizzato](~/ef6/modeling/code-first/conventions/custom.md).
