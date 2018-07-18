---
title: Convenzioni basato su modello - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
caps.latest.revision: 3
ms.openlocfilehash: 135a51d93a06c0d64732438f067df4ce2675fbe2
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122770"
---
# <a name="model-based-conventions"></a>Convenzioni basato su modello
> [!NOTE]
> **Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.  

Modello basato su convenzioni sono un metodo avanzato di configurazione del modello basato sulle convenzioni. Per la maggior parte degli scenari le [Custom API Code First convenzione su DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) deve essere utilizzato. È consigliabile l'API DbModelBuilder per le convenzioni di capire prima di usare le convenzioni del modello basato su.  

Convenzioni di modello basati su consentono la creazione di convenzioni che influiscono sulla proprietà e le tabelle che non possono essere configurate tramite le convenzioni standard. Esempi di queste sono colonne discriminatore nella tabella per i modelli di gerarchia e le colonne di associazione indipendente.  

## <a name="creating-a-convention"></a>Creazione di una convenzione   

Il primo passaggio nella creazione di una convenzione del modello basato su è la scelta quando nella pipeline deve essere applicato al modello la convenzione. Esistono due tipi di convenzioni di modello, concettuale (C-Space) e Store (S-Space). Una convenzione C-Space viene applicata al modello di cui l'applicazione viene compilata, mentre una convenzione S-Space viene applicata alla versione del modello che rappresenta il database e sono denominati controlli elementi quali colonne come lettura generati automaticamente.  

Una convenzione del modello è una classe che si estende dal IConceptualModelConvention o IStoreModelConvention.  Queste interfacce di che entrambi accettare un tipo generico che può essere tipo MetadataItem che consente di filtrare il tipo di dati cui si applica la convenzione.  

## <a name="adding-a-convention"></a>Aggiunta di una convenzione   

Convenzioni di modello vengono aggiunti nello stesso modo come classi convenzioni normali. Nel **OnModelCreating** metodo, aggiungere la convenzione all'elenco di convenzioni per un modello.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

public class BlogContext : DbContext  
{  
    public DbSet<Post> Posts { get; set; }  
    public DbSet<Comment> Comments { get; set; }  

    protected override void OnModelCreating(DbModelBuilder modelBuilder)  
    {  
        modelBuilder.Conventions.Add<MyModelBasedConvention>();  
    }  
}
```  

È possibile aggiungere una convenzione anche in relazione a un'altra convenzione utilizzando il Conventions.AddBefore\< \> o Conventions.AddAfter\< \> metodi. Per altre informazioni sulle convenzioni di Entity Framework applica vedere la sezione Note.  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a>Esempio: Convenzione per il modello Discriminator  

La ridenominazione di colonne generate da Entity Framework è un esempio di un elemento che è possibile eseguire con le altre convenzioni di API.  Si tratta di una situazione in cui usando le convenzioni del modello è l'unica opzione.  

Un esempio di come usare una convenzione del modello basato su come per configurare le colonne generate è la personalizzazione della modalità discriminatore colonne sono denominate.  Di seguito è riportato un esempio di una convenzione di modello semplice basato su che consente di rinominare ogni colonna del modello denominato "Discriminator" in "EntityType".  Ciò include le colonne che lo sviluppatore denominato semplicemente "Discriminator". Poiché la colonna "Discriminator" è una colonna generata, che questa deve essere eseguita nello spazio S.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

class DiscriminatorRenamingConvention : IStoreModelConvention<EdmProperty>  
{  
    public void Apply(EdmProperty property, DbModel model)  
    {            
        if (property.Name == "Discriminator")  
        {  
            property.Name = "EntityType";  
        }  
    }  
}
```  

## <a name="example-general-ia-renaming-convention"></a>Esempio: IA generale ridenominazione di convenzione   

Un altro esempio più complesso di convenzioni di modello basato in azione consiste nel configurare il modo che le associazioni indipendenti (IAs) sono denominate.  Si tratta di una situazione in cui le convenzioni del modello sono applicabili perché IAs vengono generati da Entity Framework e non sono presenti nel modello di cui l'API DbModelBuilder possono accedere.  

Quando Entity Framework genera un accordo, viene creata una colonna denominata EntityType_KeyName. Ad esempio, per un'associazione denominata Customer, con una colonna chiave denominato genererà una colonna denominata Customer_CustomerId CustomerId. Le strisce di convenzione seguente il '\_' carattere il nome della colonna che viene generato l'IA.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

// Provides a convention for fixing the independent association (IA) foreign key column names.  
public class ForeignKeyNamingConvention : IStoreModelConvention<AssociationType>
{

    public void Apply(AssociationType association, DbModel model)
    {
        // Identify ForeignKey properties (including IAs)  
        if (association.IsForeignKey)
        {
            // rename FK columns  
            var constraint = association.Constraint;
            if (DoPropertiesHaveDefaultNames(constraint.FromProperties, constraint.ToRole.Name, constraint.ToProperties))
            {
                NormalizeForeignKeyProperties(constraint.FromProperties);
            }
            if (DoPropertiesHaveDefaultNames(constraint.ToProperties, constraint.FromRole.Name, constraint.FromProperties))
            {
                NormalizeForeignKeyProperties(constraint.ToProperties);
            }
        }
    }

    private bool DoPropertiesHaveDefaultNames(ReadOnlyMetadataCollection<EdmProperty> properties, string roleName, ReadOnlyMetadataCollection<EdmProperty> otherEndProperties)
    {
        if (properties.Count != otherEndProperties.Count)
        {
            return false;
        }

        for (int i = 0; i < properties.Count; ++i)
        {
            if (!properties[i].Name.EndsWith("_" + otherEndProperties[i].Name))
            {
                return false;
            }
        }
        return true;
    }

    private void NormalizeForeignKeyProperties(ReadOnlyMetadataCollection<EdmProperty> properties)
    {
        for (int i = 0; i \< properties.Count; ++i)
        {
            int underscoreIndex = properties[i].Name.IndexOf('_');
            if (underscoreIndex > 0)
            {
                properties[i].Name = properties[i].Name.Remove(underscoreIndex, 1);
            }                 
        }
    }
}
```  

## <a name="extending-existing-conventions"></a>Estensione convenzioni esistente   

Se è necessario scrivere una convenzione simile a una delle convenzioni di Entity Framework già si applica al modello è sempre possibile estendere tale convenzione per evitare la necessità di riscriverlo completamente.  Un esempio di ciò consiste nel sostituire l'Id esistente corrispondente convenzione con uno personalizzato.   Un ulteriore vantaggio di eseguire l'override della convenzione chiave è che verrà chiamato il metodo sottoposto a override solo se è presente alcuna chiave già rilevata o configurata in modo esplicito. Un elenco di convenzioni utilizzate da Entity Framework è disponibile qui: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;
using System.Linq;  

// Convention to detect primary key properties.
// Recognized naming patterns in order of precedence are:
// 1. 'Key'
// 2. [type name]Key
// Primary key detection is case insensitive.
public class CustomKeyDiscoveryConvention : KeyDiscoveryConvention
{
    private const string Id = "Key";

    protected override IEnumerable<EdmProperty> MatchKeyProperty(
        EntityType entityType, IEnumerable<EdmProperty> primitiveProperties)
    {
        Debug.Assert(entityType != null);
        Debug.Assert(primitiveProperties != null);

        var matches = primitiveProperties
            .Where(p => Id.Equals(p.Name, StringComparison.OrdinalIgnoreCase));

        if (!matches.Any())
       {
            matches = primitiveProperties
                .Where(p => (entityType.Name + Id).Equals(p.Name, StringComparison.OrdinalIgnoreCase));
        }

        // If the number of matches is more than one, then multiple properties matched differing only by
        // case--for example, "Key" and "key".  
        if (matches.Count() > 1)
        {
            throw new InvalidOperationException("Multiple properties match the key convention");
        }

        return matches;
    }
}
```  

È quindi necessario aggiungere la nuova convenzione prima la convenzione di chiave esistente. Dopo che si aggiunge il CustomKeyDiscoveryConvention, è possibile rimuovere il IdKeyDiscoveryConvention.  Se non è possibile rimuovere il IdKeyDiscoveryConvention esistenti questa convenzione sarebbe ancora hanno la precedenza sulla convenzione di individuazione Id poiché viene eseguito prima di tutto, ma nel caso in cui viene trovata alcuna proprietà di "chiave", verrà eseguita la convenzione "id".  Si può notare questo comportamento perché ogni convenzione vede il modello come aggiornato per la convenzione precedente (anziché sul funzionamento su di esso in modo indipendente e tutti combinati insieme) in modo che se, ad esempio, una convenzione precedente aggiornato un nome di colonna in modo che corrisponda a un elemento di interesse per la convenzione personalizzata (quando prima che il nome non di interesse), allora verrà applicate a tale colonna.  

``` csharp
public class BlogContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Comment> Comments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new CustomKeyDiscoveryConvention());
        modelBuilder.Conventions.Remove<IdKeyDiscoveryConvention>();
    }
}
```  

## <a name="notes"></a>Note  

Un elenco di convenzioni applicate da Entity Framework è disponibile nella documentazione MSDN qui: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  Questo elenco viene effettuato il pull direttamente dal codice sorgente.  Il codice sorgente per Entity Framework 6 è disponibile nel [GitHub](https://github.com/aspnet/entityframework6/) e molte delle convenzioni utilizzate da Entity Framework sono buon punto di partenza per modelli personalizzato basato su convenzioni.  
