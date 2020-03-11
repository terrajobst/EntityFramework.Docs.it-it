---
title: 'Convenzioni basate su modelli: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: c873e9a216bd9bd1934f2149ae6af602072f3608
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419171"
---
# <a name="model-based-conventions"></a><span data-ttu-id="00205-102">Convenzioni basate su modelli</span><span class="sxs-lookup"><span data-stu-id="00205-102">Model-Based Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="00205-103">**Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="00205-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="00205-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="00205-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="00205-105">Le convenzioni basate su modelli sono un metodo avanzato per la configurazione del modello basato sulle convenzioni.</span><span class="sxs-lookup"><span data-stu-id="00205-105">Model based conventions are an advanced method of convention based model configuration.</span></span> <span data-ttu-id="00205-106">Per la maggior parte degli scenari è consigliabile usare l' [API della convenzione di Code First personalizzata in DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) .</span><span class="sxs-lookup"><span data-stu-id="00205-106">For most scenarios the [Custom Code First Convention API on DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) should be used.</span></span> <span data-ttu-id="00205-107">Prima di usare le convenzioni basate su modelli, è consigliabile conoscere l'API DbModelBuilder per le convenzioni.</span><span class="sxs-lookup"><span data-stu-id="00205-107">An understanding of the DbModelBuilder API for conventions is recommended before using model based conventions.</span></span>  

<span data-ttu-id="00205-108">Le convenzioni basate su modelli consentono la creazione di convenzioni che interessano proprietà e tabelle che non possono essere configurate tramite le convenzioni standard.</span><span class="sxs-lookup"><span data-stu-id="00205-108">Model based conventions allow the creation of conventions that affect properties and tables which are not configurable through standard conventions.</span></span> <span data-ttu-id="00205-109">Esempi di queste sono le colonne del discriminatore nella tabella per i modelli di gerarchia e le colonne di associazione indipendenti.</span><span class="sxs-lookup"><span data-stu-id="00205-109">Examples of these are discriminator columns in table per hierarchy models and Independent Association columns.</span></span>  

## <a name="creating-a-convention"></a><span data-ttu-id="00205-110">Creazione di una convenzione</span><span class="sxs-lookup"><span data-stu-id="00205-110">Creating a Convention</span></span>   

<span data-ttu-id="00205-111">Il primo passaggio per la creazione di una convenzione basata su modello è la scelta di quando nella pipeline è necessario applicare la convenzione al modello.</span><span class="sxs-lookup"><span data-stu-id="00205-111">The first step in creating a model based convention is choosing when in the pipeline the convention needs to be applied to the model.</span></span> <span data-ttu-id="00205-112">Esistono due tipi di convenzioni del modello, concettuale (C-Space) e Store (S-space).</span><span class="sxs-lookup"><span data-stu-id="00205-112">There are two types of model conventions, Conceptual (C-Space) and Store (S-Space).</span></span> <span data-ttu-id="00205-113">Una convenzione C-Space viene applicata al modello compilato dall'applicazione, mentre una convenzione S-space viene applicata alla versione del modello che rappresenta il database e controlla elementi quali la modalità di denominazione delle colonne generate automaticamente.</span><span class="sxs-lookup"><span data-stu-id="00205-113">A C-Space convention is applied to the model that the application builds, whereas an S-Space convention is applied to the version of the model that represents the database and controls things such as how automatically-generated columns are named.</span></span>  

<span data-ttu-id="00205-114">Una convenzione del modello è una classe che si estende da IConceptualModelConvention o IStoreModelConvention.</span><span class="sxs-lookup"><span data-stu-id="00205-114">A model convention is a class that extends from either IConceptualModelConvention or IStoreModelConvention.</span></span>  <span data-ttu-id="00205-115">Queste interfacce accettano entrambi un tipo generico che può essere di tipo MetadataItem usato per filtrare il tipo di dati a cui si applica la convenzione.</span><span class="sxs-lookup"><span data-stu-id="00205-115">These interfaces both accept a generic type that can be of type MetadataItem which is used to filter the data type that the convention applies to.</span></span>  

## <a name="adding-a-convention"></a><span data-ttu-id="00205-116">Aggiunta di una convenzione</span><span class="sxs-lookup"><span data-stu-id="00205-116">Adding a Convention</span></span>   

<span data-ttu-id="00205-117">Le convenzioni del modello vengono aggiunte allo stesso modo delle classi di convenzioni regolari.</span><span class="sxs-lookup"><span data-stu-id="00205-117">Model conventions are added in the same way as regular conventions classes.</span></span> <span data-ttu-id="00205-118">Nel metodo **OnModelCreating** aggiungere la convenzione all'elenco di convenzioni per un modello.</span><span class="sxs-lookup"><span data-stu-id="00205-118">In the **OnModelCreating** method, add the convention to the list of conventions for a model.</span></span>  

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

<span data-ttu-id="00205-119">È anche possibile aggiungere una convenzione in relazione a un'altra convenzione usando i metodi convenzioni. AddBefore\<\> o conventions. AddAfter\<\>.</span><span class="sxs-lookup"><span data-stu-id="00205-119">A convention can also be added in relation to another convention using the Conventions.AddBefore\<\> or Conventions.AddAfter\<\> methods.</span></span> <span data-ttu-id="00205-120">Per ulteriori informazioni sulle convenzioni che Entity Framework applica, vedere la sezione Note.</span><span class="sxs-lookup"><span data-stu-id="00205-120">For more information about the conventions that Entity Framework applies see the notes section.</span></span>  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a><span data-ttu-id="00205-121">Esempio: Convenzione del modello discriminatore</span><span class="sxs-lookup"><span data-stu-id="00205-121">Example: Discriminator Model Convention</span></span>  

<span data-ttu-id="00205-122">Rinominare le colonne generate da EF è un esempio di qualcosa che non è possibile eseguire con le altre API delle convenzioni.</span><span class="sxs-lookup"><span data-stu-id="00205-122">Renaming columns generated by EF is an example of something that you can’t do with the other conventions APIs.</span></span>  <span data-ttu-id="00205-123">Si tratta di una situazione in cui l'uso delle convenzioni del modello è l'unica opzione.</span><span class="sxs-lookup"><span data-stu-id="00205-123">This is a situation where using model conventions is your only option.</span></span>  

<span data-ttu-id="00205-124">Un esempio di come utilizzare una convenzione basata su modello per configurare colonne generate consiste nel personalizzare la modalità con cui vengono denominate le colonne del discriminatore.</span><span class="sxs-lookup"><span data-stu-id="00205-124">An example of how to use a model based convention to configure generated columns is customizing the way discriminator columns are named.</span></span>  <span data-ttu-id="00205-125">Di seguito è riportato un esempio di una semplice convenzione basata su modello che Rinomina ogni colonna del modello denominato "discriminatore" in "EntityType".</span><span class="sxs-lookup"><span data-stu-id="00205-125">Below is an example of a simple model based convention that renames every column in the model named “Discriminator” to “EntityType”.</span></span>  <span data-ttu-id="00205-126">Sono incluse le colonne che lo sviluppatore ha semplicemente denominato "discriminatore".</span><span class="sxs-lookup"><span data-stu-id="00205-126">This includes columns that the developer simply named “Discriminator”.</span></span> <span data-ttu-id="00205-127">Poiché la colonna "discriminatore" è una colonna generata, questa deve essere eseguita nello spazio S.</span><span class="sxs-lookup"><span data-stu-id="00205-127">Since the “Discriminator” column is a generated column this needs to run in S-Space.</span></span>  

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

## <a name="example-general-ia-renaming-convention"></a><span data-ttu-id="00205-128">Esempio: Convenzione di ridenominazione generale IA</span><span class="sxs-lookup"><span data-stu-id="00205-128">Example: General IA Renaming Convention</span></span>   

<span data-ttu-id="00205-129">Un altro esempio più complesso di convenzioni basate su modelli in azione consiste nel configurare il modo in cui vengono denominate le associazioni indipendenti (IAs).</span><span class="sxs-lookup"><span data-stu-id="00205-129">Another more complicated example of model based conventions in action is to configure the way that Independent Associations (IAs) are named.</span></span>  <span data-ttu-id="00205-130">Si tratta di una situazione in cui le convenzioni del modello sono applicabili perché gli IAs vengono generati da EF e non sono presenti nel modello a cui l'API DbModelBuilder può accedere.</span><span class="sxs-lookup"><span data-stu-id="00205-130">This is a situation where Model conventions are applicable because IAs are generated by EF and aren’t present in the model that the DbModelBuilder API can access.</span></span>  

<span data-ttu-id="00205-131">Quando EF genera un'IA, viene creata una colonna denominata EntityType_KeyName.</span><span class="sxs-lookup"><span data-stu-id="00205-131">When EF generates an IA, it creates a column named EntityType_KeyName.</span></span> <span data-ttu-id="00205-132">Per un'associazione denominata Customer con una colonna chiave denominata CustomerId, ad esempio, verrebbe generata una colonna denominata Customer_CustomerId.</span><span class="sxs-lookup"><span data-stu-id="00205-132">For example, for an association named Customer with a key column named CustomerId it would generate a column named Customer_CustomerId.</span></span> <span data-ttu-id="00205-133">La convenzione seguente rimuove il carattere '\_' dal nome della colonna generato per IA.</span><span class="sxs-lookup"><span data-stu-id="00205-133">The following convention strips the ‘\_’ character out of the column name that is generated for the IA.</span></span>  

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
        for (int i = 0; i < properties.Count; ++i)
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

## <a name="extending-existing-conventions"></a><span data-ttu-id="00205-134">Estensione delle convenzioni esistenti</span><span class="sxs-lookup"><span data-stu-id="00205-134">Extending Existing Conventions</span></span>   

<span data-ttu-id="00205-135">Se è necessario scrivere una convenzione simile a una delle convenzioni che Entity Framework si applica già al modello, è sempre possibile estendere tale convenzione per evitare che sia necessario riscriverla da zero.</span><span class="sxs-lookup"><span data-stu-id="00205-135">If you need to write a convention that is similar to one of the conventions that Entity Framework already applies to your model you can always extend that convention to avoid having to rewrite it from scratch.</span></span>  <span data-ttu-id="00205-136">Un esempio è la sostituzione della convenzione di corrispondenza ID esistente con una personalizzata.</span><span class="sxs-lookup"><span data-stu-id="00205-136">An example of this is to replace the existing Id matching convention with a custom one.</span></span>   <span data-ttu-id="00205-137">Un ulteriore vantaggio dell'override della convenzione di chiave è che il metodo sottoposto a override verrà chiamato solo se non è presente alcuna chiave già rilevata o configurata in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="00205-137">An added benefit to overriding the key convention is that the overridden method will get called only if there is no key already detected or explicitly configured.</span></span> <span data-ttu-id="00205-138">Un elenco delle convenzioni usate da Entity Framework è disponibile qui: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="00205-138">A list of conventions that used by Entity Framework is available here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  

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

<span data-ttu-id="00205-139">È quindi necessario aggiungere la nuova Convenzione prima della convenzione di chiave esistente.</span><span class="sxs-lookup"><span data-stu-id="00205-139">We then need to add our new convention before the existing key convention.</span></span> <span data-ttu-id="00205-140">Dopo aver aggiunto il CustomKeyDiscoveryConvention, è possibile rimuovere IdKeyDiscoveryConvention.</span><span class="sxs-lookup"><span data-stu-id="00205-140">After we add the CustomKeyDiscoveryConvention, we can remove the IdKeyDiscoveryConvention.</span></span>  <span data-ttu-id="00205-141">Se non è stato rimosso il IdKeyDiscoveryConvention esistente, questa convenzione avrebbe ancora la precedenza sulla convenzione di individuazione ID poiché viene eseguita per prima, ma nel caso in cui non venga trovata alcuna proprietà "Key", verrà eseguita la convenzione "ID".</span><span class="sxs-lookup"><span data-stu-id="00205-141">If we didn’t remove the existing IdKeyDiscoveryConvention this convention would still take precedence over the Id discovery convention since it is run first, but in the case where no “key” property is found, the “id” convention will run.</span></span>  <span data-ttu-id="00205-142">Questo comportamento si verifica perché ogni convenzione considera il modello aggiornato dalla convenzione precedente (anziché operare in modo indipendente e tutti gli elementi combinati) in modo che, se ad esempio una convenzione precedente abbia aggiornato un nome di colonna per trovare una corrispondenza tra interesse per la convenzione personalizzata (quando prima che il nome non fosse di interesse) verrà applicato a tale colonna.</span><span class="sxs-lookup"><span data-stu-id="00205-142">We see this behavior because each convention sees the model as updated by the previous convention (rather than operating on it independently and all being combined together) so that if for example, a previous convention updated a column name to match something of interest to your custom convention (when before that the name was not of interest) then it will apply to that column.</span></span>  

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

## <a name="notes"></a><span data-ttu-id="00205-143">Note</span><span class="sxs-lookup"><span data-stu-id="00205-143">Notes</span></span>  

<span data-ttu-id="00205-144">Un elenco delle convenzioni attualmente applicate da Entity Framework è disponibile nella documentazione di MSDN qui: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="00205-144">A list of conventions that are currently applied by Entity Framework is available in the MSDN documentation here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  <span data-ttu-id="00205-145">Questo elenco viene estratto direttamente dal codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="00205-145">This list is pulled directly from our source code.</span></span>  <span data-ttu-id="00205-146">Il codice sorgente per Entity Framework 6 è disponibile su [GitHub](https://github.com/aspnet/entityframework6/) e molte delle convenzioni usate da Entity Framework sono ottimi punti di partenza per le convenzioni basate su modelli personalizzati.</span><span class="sxs-lookup"><span data-stu-id="00205-146">The source code for Entity Framework 6 is available on [GitHub](https://github.com/aspnet/entityframework6/) and many of the conventions used by Entity Framework are good starting points for custom model based conventions.</span></span>  
