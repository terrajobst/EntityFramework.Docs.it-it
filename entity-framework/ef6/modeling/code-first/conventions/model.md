---
title: Convenzioni basato su modello - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: fb79164f71cb3afff705a83f5078a13d043abca8
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490935"
---
# <a name="model-based-conventions"></a><span data-ttu-id="8e31b-102">Convenzioni basato su modello</span><span class="sxs-lookup"><span data-stu-id="8e31b-102">Model-Based Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="8e31b-103">**Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="8e31b-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="8e31b-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="8e31b-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="8e31b-105">Modello basato su convenzioni sono un metodo avanzato di configurazione del modello basato sulle convenzioni.</span><span class="sxs-lookup"><span data-stu-id="8e31b-105">Model based conventions are an advanced method of convention based model configuration.</span></span> <span data-ttu-id="8e31b-106">Per la maggior parte degli scenari le [Custom API Code First convenzione su DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) deve essere utilizzato.</span><span class="sxs-lookup"><span data-stu-id="8e31b-106">For most scenarios the [Custom Code First Convention API on DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) should be used.</span></span> <span data-ttu-id="8e31b-107">È consigliabile l'API DbModelBuilder per le convenzioni di capire prima di usare le convenzioni del modello basato su.</span><span class="sxs-lookup"><span data-stu-id="8e31b-107">An understanding of the DbModelBuilder API for conventions is recommended before using model based conventions.</span></span>  

<span data-ttu-id="8e31b-108">Convenzioni di modello basati su consentono la creazione di convenzioni che influiscono sulla proprietà e le tabelle che non possono essere configurate tramite le convenzioni standard.</span><span class="sxs-lookup"><span data-stu-id="8e31b-108">Model based conventions allow the creation of conventions that affect properties and tables which are not configurable through standard conventions.</span></span> <span data-ttu-id="8e31b-109">Esempi di queste sono colonne discriminatore nella tabella per i modelli di gerarchia e le colonne di associazione indipendente.</span><span class="sxs-lookup"><span data-stu-id="8e31b-109">Examples of these are discriminator columns in table per hierarchy models and Independent Association columns.</span></span>  

## <a name="creating-a-convention"></a><span data-ttu-id="8e31b-110">Creazione di una convenzione</span><span class="sxs-lookup"><span data-stu-id="8e31b-110">Creating a Convention</span></span>   

<span data-ttu-id="8e31b-111">Il primo passaggio nella creazione di una convenzione del modello basato su è la scelta quando nella pipeline deve essere applicato al modello la convenzione.</span><span class="sxs-lookup"><span data-stu-id="8e31b-111">The first step in creating a model based convention is choosing when in the pipeline the convention needs to be applied to the model.</span></span> <span data-ttu-id="8e31b-112">Esistono due tipi di convenzioni di modello, concettuale (C-Space) e Store (S-Space).</span><span class="sxs-lookup"><span data-stu-id="8e31b-112">There are two types of model conventions, Conceptual (C-Space) and Store (S-Space).</span></span> <span data-ttu-id="8e31b-113">Una convenzione C-Space viene applicata al modello di cui l'applicazione viene compilata, mentre una convenzione S-Space viene applicata alla versione del modello che rappresenta il database e sono denominati controlli elementi quali colonne come lettura generati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="8e31b-113">A C-Space convention is applied to the model that the application builds, whereas an S-Space convention is applied to the version of the model that represents the database and controls things such as how automatically-generated columns are named.</span></span>  

<span data-ttu-id="8e31b-114">Una convenzione del modello è una classe che si estende dal IConceptualModelConvention o IStoreModelConvention.</span><span class="sxs-lookup"><span data-stu-id="8e31b-114">A model convention is a class that extends from either IConceptualModelConvention or IStoreModelConvention.</span></span>  <span data-ttu-id="8e31b-115">Queste interfacce di che entrambi accettare un tipo generico che può essere tipo MetadataItem che consente di filtrare il tipo di dati cui si applica la convenzione.</span><span class="sxs-lookup"><span data-stu-id="8e31b-115">These interfaces both accept a generic type that can be of type MetadataItem which is used to filter the data type that the convention applies to.</span></span>  

## <a name="adding-a-convention"></a><span data-ttu-id="8e31b-116">Aggiunta di una convenzione</span><span class="sxs-lookup"><span data-stu-id="8e31b-116">Adding a Convention</span></span>   

<span data-ttu-id="8e31b-117">Convenzioni di modello vengono aggiunti nello stesso modo come classi convenzioni normali.</span><span class="sxs-lookup"><span data-stu-id="8e31b-117">Model conventions are added in the same way as regular conventions classes.</span></span> <span data-ttu-id="8e31b-118">Nel **OnModelCreating** metodo, aggiungere la convenzione all'elenco di convenzioni per un modello.</span><span class="sxs-lookup"><span data-stu-id="8e31b-118">In the **OnModelCreating** method, add the convention to the list of conventions for a model.</span></span>  

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

<span data-ttu-id="8e31b-119">È possibile aggiungere una convenzione anche in relazione a un'altra convenzione utilizzando il Conventions.AddBefore\< \> o Conventions.AddAfter\< \> metodi.</span><span class="sxs-lookup"><span data-stu-id="8e31b-119">A convention can also be added in relation to another convention using the Conventions.AddBefore\<\> or Conventions.AddAfter\<\> methods.</span></span> <span data-ttu-id="8e31b-120">Per altre informazioni sulle convenzioni di Entity Framework applica vedere la sezione Note.</span><span class="sxs-lookup"><span data-stu-id="8e31b-120">For more information about the conventions that Entity Framework applies see the notes section.</span></span>  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a><span data-ttu-id="8e31b-121">Esempio: Convenzione per il modello Discriminator</span><span class="sxs-lookup"><span data-stu-id="8e31b-121">Example: Discriminator Model Convention</span></span>  

<span data-ttu-id="8e31b-122">La ridenominazione di colonne generate da Entity Framework è un esempio di un elemento che è possibile eseguire con le altre convenzioni di API.</span><span class="sxs-lookup"><span data-stu-id="8e31b-122">Renaming columns generated by EF is an example of something that you can’t do with the other conventions APIs.</span></span>  <span data-ttu-id="8e31b-123">Si tratta di una situazione in cui usando le convenzioni del modello è l'unica opzione.</span><span class="sxs-lookup"><span data-stu-id="8e31b-123">This is a situation where using model conventions is your only option.</span></span>  

<span data-ttu-id="8e31b-124">Un esempio di come usare una convenzione del modello basato su come per configurare le colonne generate è la personalizzazione della modalità discriminatore colonne sono denominate.</span><span class="sxs-lookup"><span data-stu-id="8e31b-124">An example of how to use a model based convention to configure generated columns is customizing the way discriminator columns are named.</span></span>  <span data-ttu-id="8e31b-125">Di seguito è riportato un esempio di una convenzione di modello semplice basato su che consente di rinominare ogni colonna del modello denominato "Discriminator" in "EntityType".</span><span class="sxs-lookup"><span data-stu-id="8e31b-125">Below is an example of a simple model based convention that renames every column in the model named “Discriminator” to “EntityType”.</span></span>  <span data-ttu-id="8e31b-126">Ciò include le colonne che lo sviluppatore denominato semplicemente "Discriminator".</span><span class="sxs-lookup"><span data-stu-id="8e31b-126">This includes columns that the developer simply named “Discriminator”.</span></span> <span data-ttu-id="8e31b-127">Poiché la colonna "Discriminator" è una colonna generata, che questa deve essere eseguita nello spazio S.</span><span class="sxs-lookup"><span data-stu-id="8e31b-127">Since the “Discriminator” column is a generated column this needs to run in S-Space.</span></span>  

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

## <a name="example-general-ia-renaming-convention"></a><span data-ttu-id="8e31b-128">Esempio: IA generale ridenominazione di convenzione</span><span class="sxs-lookup"><span data-stu-id="8e31b-128">Example: General IA Renaming Convention</span></span>   

<span data-ttu-id="8e31b-129">Un altro esempio più complesso di convenzioni di modello basato in azione consiste nel configurare il modo che le associazioni indipendenti (IAs) sono denominate.</span><span class="sxs-lookup"><span data-stu-id="8e31b-129">Another more complicated example of model based conventions in action is to configure the way that Independent Associations (IAs) are named.</span></span>  <span data-ttu-id="8e31b-130">Si tratta di una situazione in cui le convenzioni del modello sono applicabili perché IAs vengono generati da Entity Framework e non sono presenti nel modello di cui l'API DbModelBuilder possono accedere.</span><span class="sxs-lookup"><span data-stu-id="8e31b-130">This is a situation where Model conventions are applicable because IAs are generated by EF and aren’t present in the model that the DbModelBuilder API can access.</span></span>  

<span data-ttu-id="8e31b-131">Quando Entity Framework genera un accordo, viene creata una colonna denominata EntityType_KeyName.</span><span class="sxs-lookup"><span data-stu-id="8e31b-131">When EF generates an IA, it creates a column named EntityType_KeyName.</span></span> <span data-ttu-id="8e31b-132">Ad esempio, per un'associazione denominata Customer, con una colonna chiave denominato genererà una colonna denominata Customer_CustomerId CustomerId.</span><span class="sxs-lookup"><span data-stu-id="8e31b-132">For example, for an association named Customer with a key column named CustomerId it would generate a column named Customer_CustomerId.</span></span> <span data-ttu-id="8e31b-133">Le strisce di convenzione seguente il '\_' carattere il nome della colonna che viene generato l'IA.</span><span class="sxs-lookup"><span data-stu-id="8e31b-133">The following convention strips the ‘\_’ character out of the column name that is generated for the IA.</span></span>  

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

## <a name="extending-existing-conventions"></a><span data-ttu-id="8e31b-134">Estensione convenzioni esistente</span><span class="sxs-lookup"><span data-stu-id="8e31b-134">Extending Existing Conventions</span></span>   

<span data-ttu-id="8e31b-135">Se è necessario scrivere una convenzione simile a una delle convenzioni di Entity Framework già si applica al modello è sempre possibile estendere tale convenzione per evitare la necessità di riscriverlo completamente.</span><span class="sxs-lookup"><span data-stu-id="8e31b-135">If you need to write a convention that is similar to one of the conventions that Entity Framework already applies to your model you can always extend that convention to avoid having to rewrite it from scratch.</span></span>  <span data-ttu-id="8e31b-136">Un esempio di ciò consiste nel sostituire l'Id esistente corrispondente convenzione con uno personalizzato.</span><span class="sxs-lookup"><span data-stu-id="8e31b-136">An example of this is to replace the existing Id matching convention with a custom one.</span></span>   <span data-ttu-id="8e31b-137">Un ulteriore vantaggio di eseguire l'override della convenzione chiave è che verrà chiamato il metodo sottoposto a override solo se è presente alcuna chiave già rilevata o configurata in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="8e31b-137">An added benefit to overriding the key convention is that the overridden method will get called only if there is no key already detected or explicitly configured.</span></span> <span data-ttu-id="8e31b-138">Un elenco di convenzioni utilizzate da Entity Framework è disponibile qui: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="8e31b-138">A list of conventions that used by Entity Framework is available here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  

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

<span data-ttu-id="8e31b-139">È quindi necessario aggiungere la nuova convenzione prima la convenzione di chiave esistente.</span><span class="sxs-lookup"><span data-stu-id="8e31b-139">We then need to add our new convention before the existing key convention.</span></span> <span data-ttu-id="8e31b-140">Dopo che si aggiunge il CustomKeyDiscoveryConvention, è possibile rimuovere il IdKeyDiscoveryConvention.</span><span class="sxs-lookup"><span data-stu-id="8e31b-140">After we add the CustomKeyDiscoveryConvention, we can remove the IdKeyDiscoveryConvention.</span></span>  <span data-ttu-id="8e31b-141">Se non è possibile rimuovere il IdKeyDiscoveryConvention esistenti questa convenzione sarebbe ancora hanno la precedenza sulla convenzione di individuazione Id poiché viene eseguito prima di tutto, ma nel caso in cui viene trovata alcuna proprietà di "chiave", verrà eseguita la convenzione "id".</span><span class="sxs-lookup"><span data-stu-id="8e31b-141">If we didn’t remove the existing IdKeyDiscoveryConvention this convention would still take precedence over the Id discovery convention since it is run first, but in the case where no “key” property is found, the “id” convention will run.</span></span>  <span data-ttu-id="8e31b-142">Si può notare questo comportamento perché ogni convenzione vede il modello come aggiornato per la convenzione precedente (anziché sul funzionamento su di esso in modo indipendente e tutti combinati insieme) in modo che se, ad esempio, una convenzione precedente aggiornato un nome di colonna in modo che corrisponda a un elemento di interesse per la convenzione personalizzata (quando prima che il nome non di interesse), allora verrà applicate a tale colonna.</span><span class="sxs-lookup"><span data-stu-id="8e31b-142">We see this behavior because each convention sees the model as updated by the previous convention (rather than operating on it independently and all being combined together) so that if for example, a previous convention updated a column name to match something of interest to your custom convention (when before that the name was not of interest) then it will apply to that column.</span></span>  

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

## <a name="notes"></a><span data-ttu-id="8e31b-143">Note</span><span class="sxs-lookup"><span data-stu-id="8e31b-143">Notes</span></span>  

<span data-ttu-id="8e31b-144">Un elenco di convenzioni applicate da Entity Framework è disponibile nella documentazione MSDN qui: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="8e31b-144">A list of conventions that are currently applied by Entity Framework is available in the MSDN documentation here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  <span data-ttu-id="8e31b-145">Questo elenco viene effettuato il pull direttamente dal codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="8e31b-145">This list is pulled directly from our source code.</span></span>  <span data-ttu-id="8e31b-146">Il codice sorgente per Entity Framework 6 è disponibile nel [GitHub](https://github.com/aspnet/entityframework6/) e molte delle convenzioni utilizzate da Entity Framework sono buon punto di partenza per modelli personalizzato basato su convenzioni.</span><span class="sxs-lookup"><span data-stu-id="8e31b-146">The source code for Entity Framework 6 is available on [GitHub](https://github.com/aspnet/entityframework6/) and many of the conventions used by Entity Framework are good starting points for custom model based conventions.</span></span>  
