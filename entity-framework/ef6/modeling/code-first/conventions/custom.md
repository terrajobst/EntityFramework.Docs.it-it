---
title: Codice personalizzato prima convenzioni - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: cfd7f7cad532dca5227793c04d7d91e977ea5e4e
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489844"
---
# <a name="custom-code-first-conventions"></a><span data-ttu-id="3e21a-102">Convenzioni del primo codice personalizzato</span><span class="sxs-lookup"><span data-stu-id="3e21a-102">Custom Code First Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="3e21a-103">**Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="3e21a-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="3e21a-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="3e21a-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="3e21a-105">Quando si usa il modello viene calcolato da classi usando un set di convenzioni Code First.</span><span class="sxs-lookup"><span data-stu-id="3e21a-105">When using Code First your model is calculated from your classes using a set of conventions.</span></span> <span data-ttu-id="3e21a-106">Il valore predefinito [convenzioni del primo codice](~/ef6/modeling/code-first/conventions/built-in.md) determinare aspetti, ad esempio quale proprietà diventa la chiave primaria di un'entità, il nome della tabella esegue il mapping a un'entità, e quali precisione e scala per impostazione predefinita contiene una colonna decimale.</span><span class="sxs-lookup"><span data-stu-id="3e21a-106">The default [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) determine things like which property becomes the primary key of an entity, the name of the table an entity maps to, and what precision and scale a decimal column has by default.</span></span>

<span data-ttu-id="3e21a-107">A volte queste convenzioni predefinite non sono idonee per il modello e si hanno per colmarli configurando molte entità individuali usando le annotazioni dei dati o l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="3e21a-107">Sometimes these default conventions are not ideal for your model, and you have to work around them by configuring many individual entities using Data Annotations or the Fluent API.</span></span> <span data-ttu-id="3e21a-108">Convenzioni del primo codice personalizzati consentono di definire le convenzioni che forniscono i valori predefiniti della configurazione per il modello.</span><span class="sxs-lookup"><span data-stu-id="3e21a-108">Custom Code First Conventions let you define your own conventions that provide configuration defaults for your model.</span></span> <span data-ttu-id="3e21a-109">In questa procedura dettagliata, verranno analizzati i diversi tipi di convenzioni personalizzate e come creare ciascuno di essi.</span><span class="sxs-lookup"><span data-stu-id="3e21a-109">In this walkthrough, we will explore the different types of custom conventions and how to create each of them.</span></span>


## <a name="model-based-conventions"></a><span data-ttu-id="3e21a-110">Convenzioni basato su modello</span><span class="sxs-lookup"><span data-stu-id="3e21a-110">Model-Based Conventions</span></span>

<span data-ttu-id="3e21a-111">Questa pagina illustra l'API DbModelBuilder per le convenzioni personalizzate.</span><span class="sxs-lookup"><span data-stu-id="3e21a-111">This page covers the DbModelBuilder API for custom conventions.</span></span> <span data-ttu-id="3e21a-112">Questa API dovrebbero essere sufficiente per la maggior parte delle convenzioni personalizzate di creazione.</span><span class="sxs-lookup"><span data-stu-id="3e21a-112">This API should be sufficient for authoring most custom conventions.</span></span> <span data-ttu-id="3e21a-113">Tuttavia, è inoltre disponibile la possibilità di creare le convenzioni basato su modello - convenzioni che consentono di modificare il modello finale dopo averla creata, per gestire scenari avanzati.</span><span class="sxs-lookup"><span data-stu-id="3e21a-113">However, there is also the ability to author model-based conventions - conventions that manipulate the final model once it is created - to handle advanced scenarios.</span></span> <span data-ttu-id="3e21a-114">Per altre informazioni, vedere [basato su modello convenzioni](~/ef6/modeling/code-first/conventions/model.md).</span><span class="sxs-lookup"><span data-stu-id="3e21a-114">For more information, see [Model-Based Conventions](~/ef6/modeling/code-first/conventions/model.md).</span></span>

 

## <a name="our-model"></a><span data-ttu-id="3e21a-115">Il nostro modello</span><span class="sxs-lookup"><span data-stu-id="3e21a-115">Our Model</span></span>

<span data-ttu-id="3e21a-116">Iniziamo con la definizione di un modello semplice che è possibile usare con la convenzioni.</span><span class="sxs-lookup"><span data-stu-id="3e21a-116">Let's start by defining a simple model that we can use with our conventions.</span></span> <span data-ttu-id="3e21a-117">Aggiungere le classi seguenti al progetto.</span><span class="sxs-lookup"><span data-stu-id="3e21a-117">Add the following classes to your project.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;

    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }
    }

    public class Product
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public decimal? Price { get; set; }
        public DateTime? ReleaseDate { get; set; }
        public ProductCategory Category { get; set; }
    }

    public class ProductCategory
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public List<Product> Products { get; set; }
    }
```

 

## <a name="introducing-custom-conventions"></a><span data-ttu-id="3e21a-118">Introduzione a convenzioni personalizzate</span><span class="sxs-lookup"><span data-stu-id="3e21a-118">Introducing Custom Conventions</span></span>

<span data-ttu-id="3e21a-119">Scriviamo una convenzione che consente di configurare qualsiasi proprietà di chiave da utilizzare come chiave primaria per il tipo di entità denominata.</span><span class="sxs-lookup"><span data-stu-id="3e21a-119">Let’s write a convention that configures any property named Key to be the primary key for its entity type.</span></span>

<span data-ttu-id="3e21a-120">Le convenzioni sono abilitate nel generatore di modelli, è possibile accedere eseguendo l'override OnModelCreating nel contesto.</span><span class="sxs-lookup"><span data-stu-id="3e21a-120">Conventions are enabled on the model builder, which can be accessed by overriding OnModelCreating in the context.</span></span> <span data-ttu-id="3e21a-121">Aggiornare la classe ProductContext come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="3e21a-121">Update the ProductContext class as follows:</span></span>

``` csharp
    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Properties()
                        .Where(p => p.Name == "Key")
                        .Configure(p => p.IsKey());
        }
    }
```

<span data-ttu-id="3e21a-122">A questo punto, qualsiasi proprietà nel modello denominato chiave sarà configurata come chiave primaria di qualsiasi entità propria parte di.</span><span class="sxs-lookup"><span data-stu-id="3e21a-122">Now, any property in our model named Key will be configured as the primary key of whatever entity its part of.</span></span>

<span data-ttu-id="3e21a-123">Possiamo inoltre fare la convenzioni più specifica filtrando in base al tipo di proprietà che si intende configurare:</span><span class="sxs-lookup"><span data-stu-id="3e21a-123">We could also make our conventions more specific by filtering on the type of property that we are going to configure:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

<span data-ttu-id="3e21a-124">Si configurerà tutte le proprietà denominate chiave primaria chiave relative entità, ma solo se sono un numero intero.</span><span class="sxs-lookup"><span data-stu-id="3e21a-124">This will configure all properties called Key to be the primary key of their entity, but only if they are an integer.</span></span>

<span data-ttu-id="3e21a-125">Una funzionalità interessante del metodo IsKey è costituito da sommano tra loro.</span><span class="sxs-lookup"><span data-stu-id="3e21a-125">An interesting feature of the IsKey method is that it is additive.</span></span> <span data-ttu-id="3e21a-126">Ciò significa che se si chiama IsKey su più proprietà e tutti diventano parte di una chiave composta.</span><span class="sxs-lookup"><span data-stu-id="3e21a-126">Which means that if you call IsKey on multiple properties and they will all become part of a composite key.</span></span> <span data-ttu-id="3e21a-127">Un'avvertenza per questo è che, quando si specificano più proprietà per una chiave è necessario specificare anche un ordine di tali proprietà.</span><span class="sxs-lookup"><span data-stu-id="3e21a-127">The one caveat for this is that when you specify multiple properties for a key you must also specify an order for those properties.</span></span> <span data-ttu-id="3e21a-128">È possibile farlo tramite una chiamata di HasColumnOrder metodo simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="3e21a-128">You can do this by calling the HasColumnOrder method like below:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

<span data-ttu-id="3e21a-129">Questo codice configurerà i tipi nel modello a dispone di una chiave composta costituita la colonna chiave int e la colonna di nome di stringa.</span><span class="sxs-lookup"><span data-stu-id="3e21a-129">This code will configure the types in our model to have a composite key consisting of the int Key column and the string Name column.</span></span> <span data-ttu-id="3e21a-130">Se si visualizza il modello nella finestra di progettazione si presenta come segue:</span><span class="sxs-lookup"><span data-stu-id="3e21a-130">If we view the model in the designer it would look like this:</span></span>

![Chiave composta](~/ef6/media/compositekey.png)

<span data-ttu-id="3e21a-132">Un altro esempio di convenzioni proprietà consiste nel configurare tutte le proprietà di data/ora nel mio modello per eseguire il mapping a un tipo datetime2 in SQL Server invece di data/ora.</span><span class="sxs-lookup"><span data-stu-id="3e21a-132">Another example of property conventions is to configure all DateTime properties in my model to map to the datetime2 type in SQL Server instead of datetime.</span></span> <span data-ttu-id="3e21a-133">È possibile ottenere questo risultato con gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="3e21a-133">You can achieve this with the following:</span></span>

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a><span data-ttu-id="3e21a-134">Classi di convenzione</span><span class="sxs-lookup"><span data-stu-id="3e21a-134">Convention Classes</span></span>

<span data-ttu-id="3e21a-135">Un altro modo per definire le convenzioni consiste nell'utilizzare una convenzione di classe per incapsulare la convenzione.</span><span class="sxs-lookup"><span data-stu-id="3e21a-135">Another way of defining conventions is to use a Convention Class to encapsulate your convention.</span></span> <span data-ttu-id="3e21a-136">Quando si usa una classe convenzione è creare un tipo che eredita dalla classe convenzione System.Data.Entity.ModelConfiguration.Conventions nello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="3e21a-136">When using a Convention Class then you create a type that inherits from the Convention class in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>

<span data-ttu-id="3e21a-137">È possibile creare una classe convenzione in base alla convenzione datetime2 che è stato illustrato in precedenza eseguendo queste operazioni:</span><span class="sxs-lookup"><span data-stu-id="3e21a-137">We can create a Convention Class with the datetime2 convention that we showed earlier by doing the following:</span></span>

``` csharp
    public class DateTime2Convention : Convention
    {
        public DateTime2Convention()
        {
            this.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));        
        }
    }
```

<span data-ttu-id="3e21a-138">Per indicare a Entity Framework per utilizzare questa convenzione che è aggiungerlo alla raccolta di convenzioni in OnModelCreating, che se sono state eseguite con la procedura dettagliata avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="3e21a-138">To tell EF to use this convention you add it to the Conventions collection in OnModelCreating, which if you’ve been following along with the walkthrough will look like this:</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

<span data-ttu-id="3e21a-139">Come si può notare è aggiungere un'istanza di convenzione per l'insieme di convenzioni.</span><span class="sxs-lookup"><span data-stu-id="3e21a-139">As you can see we add an instance of our convention to the conventions collection.</span></span> <span data-ttu-id="3e21a-140">Eredità dalla convenzione fornisce un modo pratico di raggruppamento e le convenzioni di condivisione tra progetti o team.</span><span class="sxs-lookup"><span data-stu-id="3e21a-140">Inheriting from Convention provides a convenient way of grouping and sharing conventions across teams or projects.</span></span> <span data-ttu-id="3e21a-141">Potrebbe, ad esempio, hai una libreria di classi con un set di convenzioni che tutte le organizzazioni che proietta di uso comune.</span><span class="sxs-lookup"><span data-stu-id="3e21a-141">You could, for example, have a class library with a common set of conventions that all of your organizations projects use.</span></span>

 

## <a name="custom-attributes"></a><span data-ttu-id="3e21a-142">Attributi personalizzati</span><span class="sxs-lookup"><span data-stu-id="3e21a-142">Custom Attributes</span></span>

<span data-ttu-id="3e21a-143">Un altro ottimo uso convenzioni consiste nell'abilitare nuovi attributi da utilizzare quando si configura un modello.</span><span class="sxs-lookup"><span data-stu-id="3e21a-143">Another great use of conventions is to enable new attributes to be used when configuring a model.</span></span> <span data-ttu-id="3e21a-144">Per illustrare questo concetto, è possibile creare un attributo che è possibile usare per contrassegnare le proprietà della stringa come non Unicode.</span><span class="sxs-lookup"><span data-stu-id="3e21a-144">To illustrate this, let’s create an attribute that we can use to mark String properties as non-Unicode.</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

<span data-ttu-id="3e21a-145">A questo punto, passiamo alla creazione di una convenzione per applicare questo attributo per il nostro modello:</span><span class="sxs-lookup"><span data-stu-id="3e21a-145">Now, let’s create a convention to apply this attribute to our model:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

<span data-ttu-id="3e21a-146">Questa convenzione è possibile aggiungere l'attributo non Unicode a uno dei nostri proprietà stringa che indica che la colonna nel database verrà archiviata come varchar invece di nvarchar.</span><span class="sxs-lookup"><span data-stu-id="3e21a-146">With this convention we can add the NonUnicode attribute to any of our string properties, which means the column in the database will be stored as varchar instead of nvarchar.</span></span>

<span data-ttu-id="3e21a-147">Un aspetto da notare circa questa convenzione è che se si inserisce l'attributo non Unicode in qualsiasi elemento diverso da una proprietà stringa, quindi verrà generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="3e21a-147">One thing to note about this convention is that if you put the NonUnicode attribute on anything other than a string property then it will throw an exception.</span></span> <span data-ttu-id="3e21a-148">Ciò avviene perché non è possibile configurare IsUnicode su qualsiasi tipo diverso da una stringa.</span><span class="sxs-lookup"><span data-stu-id="3e21a-148">It does this because you cannot configure IsUnicode on any type other than a string.</span></span> <span data-ttu-id="3e21a-149">In questo caso, successivamente sarà possibile effettuare la convenzione più specifica, in modo che filtri i dati che non è una stringa.</span><span class="sxs-lookup"><span data-stu-id="3e21a-149">If this happens, then you can make your convention more specific, so that it filters out anything that isn’t a string.</span></span>

<span data-ttu-id="3e21a-150">Quando la convenzione precedente funziona per la definizione di attributi personalizzati è disponibile un'altra API che possono essere molto più semplice da utilizzare, soprattutto quando si desidera utilizzare le proprietà dalla classe di attributi.</span><span class="sxs-lookup"><span data-stu-id="3e21a-150">While the above convention works for defining custom attributes there is another API that can be much easier to use, especially when you want to use properties from the attribute class.</span></span>

<span data-ttu-id="3e21a-151">Per questo esempio si userà per aggiornare l'attributo e modificarlo in un attributo IsUnicode, in modo che risulti simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="3e21a-151">For this example we are going to update our attribute and change it to an IsUnicode attribute, so it looks like this:</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    internal class IsUnicode : Attribute
    {
        public bool Unicode { get; set; }

        public IsUnicode(bool isUnicode)
        {
            Unicode = isUnicode;
        }
    }
```

<span data-ttu-id="3e21a-152">Dopo aver ottenuto ciò, è possibile impostare un valore booleano nell'attributo per indicare la convenzione o meno una proprietà deve essere Unicode.</span><span class="sxs-lookup"><span data-stu-id="3e21a-152">Once we have this, we can set a bool on our attribute to tell the convention whether or not a property should be Unicode.</span></span> <span data-ttu-id="3e21a-153">Possiamo creare nella convenzione di che abbiamo già accedendo ClrProperty della classe di configurazione simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="3e21a-153">We could do this in the convention we have already by accessing the ClrProperty of the configuration class like this:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

<span data-ttu-id="3e21a-154">Ciò è abbastanza semplice, ma c'è un modo più conciso di raggiungere questo obiettivo tramite la presenza di un metodo per le convenzioni di API.</span><span class="sxs-lookup"><span data-stu-id="3e21a-154">This is easy enough, but there is a more succinct way of achieving this by using the Having method of the conventions API.</span></span> <span data-ttu-id="3e21a-155">La presenza di metodo ha un parametro di tipo Func&lt;PropertyInfo, T&gt; PropertyInfo che accetta lo stesso come Where (metodo), ma è previsto per restituire un oggetto.</span><span class="sxs-lookup"><span data-stu-id="3e21a-155">The Having method has a parameter of type Func&lt;PropertyInfo, T&gt; which accepts the PropertyInfo the same as the Where method, but is expected to return an object.</span></span> <span data-ttu-id="3e21a-156">Se l'oggetto restituito è null, quindi non verrà configurata la proprietà, ovvero è possibile filtrare le proprietà con esso come Where, ma è diverso in quanto verrà inoltre acquisire l'oggetto restituito e passarlo al metodo Configure.</span><span class="sxs-lookup"><span data-stu-id="3e21a-156">If the returned object is null then the property will not be configured, which means you can filter out properties with it just like Where, but it is different in that it will also capture the returned object and pass it to the Configure method.</span></span> <span data-ttu-id="3e21a-157">Questo codice funziona come segue:</span><span class="sxs-lookup"><span data-stu-id="3e21a-157">This works like the following:</span></span>

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

<span data-ttu-id="3e21a-158">Gli attributi personalizzati non sono l'unico motivo per usare la presenza di un metodo, è utile qualsiasi luogo in cui è necessario prendere in considerazione un elemento che si sta filtrando quando si configurano i tipi o la proprietà.</span><span class="sxs-lookup"><span data-stu-id="3e21a-158">Custom attributes are not the only reason to use the Having method, it is useful anywhere that you need to reason about something that you are filtering on when configuring your types or properties.</span></span>

 

## <a name="configuring-types"></a><span data-ttu-id="3e21a-159">Configurazione dei tipi</span><span class="sxs-lookup"><span data-stu-id="3e21a-159">Configuring Types</span></span>

<span data-ttu-id="3e21a-160">Tutti i nostri convenzioni finora sono stati per le proprietà, ma è presente un'altra area delle convenzioni di API per la configurazione dei tipi nel modello.</span><span class="sxs-lookup"><span data-stu-id="3e21a-160">So far all of our conventions have been for properties, but there is another area of the conventions API for configuring the types in your model.</span></span> <span data-ttu-id="3e21a-161">L'esperienza è simile alle convenzioni che abbiamo visto finora, ma le opzioni di configurazione all'interno dovrà essere eseguita all'entità anziché proprietà livello.</span><span class="sxs-lookup"><span data-stu-id="3e21a-161">The experience is similar to the conventions we have seen so far, but the options inside configure will be at the entity instead of property level.</span></span>

<span data-ttu-id="3e21a-162">Una delle operazioni che possono essere molto utile per le convenzioni a livello di tipo in fase di modifica la tabella convenzione di denominazione, eseguire il mapping a uno schema esistente che è diverso da quello predefinito di Entity Framework o per creare un nuovo database con una convenzione di denominazione diversa.</span><span class="sxs-lookup"><span data-stu-id="3e21a-162">One of the things that Type level conventions can be really useful for is changing the table naming convention, either to map to an existing schema that differs from the EF default or to create a new database with a different naming convention.</span></span> <span data-ttu-id="3e21a-163">A tale scopo è necessario prima di tutto un metodo che può accettare le informazioni sul tipo per un tipo nel modello e restituire cosa deve essere il nome della tabella per quel tipo:</span><span class="sxs-lookup"><span data-stu-id="3e21a-163">To do this we first need a method that can accept the TypeInfo for a type in our model and return what the table name for that type should be:</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

<span data-ttu-id="3e21a-164">Questo metodo accetta un tipo e restituisce una stringa che usa lettere minuscole con caratteri di sottolineatura anziché CamelCase.</span><span class="sxs-lookup"><span data-stu-id="3e21a-164">This method takes a type and returns a string that uses lower case with underscores instead of CamelCase.</span></span> <span data-ttu-id="3e21a-165">Nel nostro modello ciò significa che la classe ProductCategory verrà mappata a una tabella denominata product\_categoria anziché ProductCategories.</span><span class="sxs-lookup"><span data-stu-id="3e21a-165">In our model this means that the ProductCategory class will be mapped to a table called product\_category instead of ProductCategories.</span></span>

<span data-ttu-id="3e21a-166">Dopo aver ottenuto tale metodo poterlo chiamare una convenzione simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="3e21a-166">Once we have that method we can call it in a convention like this:</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

<span data-ttu-id="3e21a-167">Questa convenzione consente di configurare tutti i tipi nel modello per eseguire il mapping al nome della tabella che viene restituito dal nostro metodo GetTableName.</span><span class="sxs-lookup"><span data-stu-id="3e21a-167">This convention configures every type in our model to map to the table name that is returned from our GetTableName method.</span></span> <span data-ttu-id="3e21a-168">Questa convenzione è l'equivalente alla chiamata al metodo ToTable per ogni entità nel modello tramite l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="3e21a-168">This convention is the equivalent to calling the ToTable method for each entity in the model using the Fluent API.</span></span>

<span data-ttu-id="3e21a-169">Un aspetto da sottolineare a proposito è che quando si chiama EF ToTable richiederà la stringa specificata dall'utente come nome della tabella esatto, senza la pluralizzazione che farebbe normalmente quando si determina i nomi di tabella.</span><span class="sxs-lookup"><span data-stu-id="3e21a-169">One thing to note about this is that when you call ToTable EF will take the string that you provide as the exact table name, without any of the pluralization that it would normally do when determining table names.</span></span> <span data-ttu-id="3e21a-170">Ecco perché il nome della tabella dalla convenzione viene prodotto\_categoria anziché prodotto\_categorie.</span><span class="sxs-lookup"><span data-stu-id="3e21a-170">This is why the table name from our convention is product\_category instead of product\_categories.</span></span> <span data-ttu-id="3e21a-171">Afferma che la convenzione effettuando una chiamata al servizio di pluralizzazione noi stessi.</span><span class="sxs-lookup"><span data-stu-id="3e21a-171">We can resolve that in our convention by making a call to the pluralization service ourselves.</span></span>

<span data-ttu-id="3e21a-172">Nel codice seguente si userà il [risoluzione delle dipendenze](~/ef6/fundamentals/configuring/dependency-resolution.md) funzionalità aggiunta in EF6 per recuperare il servizio di pluralizzazione che EF sarebbe utilizzato e rendere plurali i nome della tabella.</span><span class="sxs-lookup"><span data-stu-id="3e21a-172">In the following code we will use the [Dependency Resolution](~/ef6/fundamentals/configuring/dependency-resolution.md) feature added in EF6 to retrieve the pluralization service that EF would have used and pluralize our table name.</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var pluralizationService = DbConfiguration.DependencyResolver.GetService<IPluralizationService>();

        var result = pluralizationService.Pluralize(type.Name);

        result = Regex.Replace(result, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

> [!NOTE]
> <span data-ttu-id="3e21a-173">La versione generica di GetService è un metodo di estensione nello spazio dei nomi System.Data.Entity.Infrastructure.DependencyResolution, sarà necessario aggiungere un utilizzo dell'istruzione il contesto per usarlo.</span><span class="sxs-lookup"><span data-stu-id="3e21a-173">The generic version of GetService is an extension method in the System.Data.Entity.Infrastructure.DependencyResolution namespace, you will need to add a using statement to your context in order to use it.</span></span>

### <a name="totable-and-inheritance"></a><span data-ttu-id="3e21a-174">ToTable ed ereditarietà</span><span class="sxs-lookup"><span data-stu-id="3e21a-174">ToTable and Inheritance</span></span>

<span data-ttu-id="3e21a-175">Un altro aspetto importante di ToTable è che se si esegue il mapping in modo esplicito un tipo per una determinata tabella, quindi è possibile modificare la strategia di mapping che useranno Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3e21a-175">Another important aspect of ToTable is that if you explicitly map a type to a given table, then you can alter the mapping strategy that EF will use.</span></span> <span data-ttu-id="3e21a-176">Se si chiama ToTable per ogni tipo in una gerarchia di ereditarietà, passando il nome del tipo come il nome della tabella come in precedenza, quindi si modificherà la strategia di mapping tabella Per gerarchia (TPH) predefinita alla tabella Per tipo TPT ().</span><span class="sxs-lookup"><span data-stu-id="3e21a-176">If you call ToTable for every type in an inheritance hierarchy, passing the type name as the name of the table like we did above, then you will change the default Table-Per-Hierarchy (TPH) mapping strategy to Table-Per-Type (TPT).</span></span> <span data-ttu-id="3e21a-177">Il modo migliore per descrivere questo è un esempio concreto con:</span><span class="sxs-lookup"><span data-stu-id="3e21a-177">The best way to describe this is whith a concrete example:</span></span>

``` csharp
    public class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Manager : Employee
    {
        public string SectionManaged { get; set; }
    }
```

<span data-ttu-id="3e21a-178">Per impostazione predefinita i dipendenti e manager viene eseguito il mapping alla stessa tabella (dipendenti) nel database.</span><span class="sxs-lookup"><span data-stu-id="3e21a-178">By default both employee and manager are mapped to the same table (Employees) in the database.</span></span> <span data-ttu-id="3e21a-179">La tabella conterrà sia i dipendenti e ai Manager con una colonna discriminatore che indicherà il tipo di istanza viene memorizzato in ogni riga.</span><span class="sxs-lookup"><span data-stu-id="3e21a-179">The table will contain both employees and managers with a discriminator column that will tell you what type of instance is stored in each row.</span></span> <span data-ttu-id="3e21a-180">Si tratta di mapping tabella per gerarchia in quanto non esiste una sola tabella per gerarchia.</span><span class="sxs-lookup"><span data-stu-id="3e21a-180">This is TPH mapping as there is a single table for the hierarchy.</span></span> <span data-ttu-id="3e21a-181">Tuttavia, se si chiama ToTable su entrambi classe quindi ogni tipo verrà invece eseguire il mapping alla relativa tabella, nota anche come TPT poiché ogni tipo ha un proprio tabella.</span><span class="sxs-lookup"><span data-stu-id="3e21a-181">However, if you call ToTable on both classe then each type will instead be mapped to its own table, also known as TPT since each type has its own table.</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

<span data-ttu-id="3e21a-182">Il codice precedente eseguirà il mapping a una struttura di tabella che è simile a quanto segue:</span><span class="sxs-lookup"><span data-stu-id="3e21a-182">The code above will map to a table structure that looks like the following:</span></span>

![Esempio di tabella per tipo](~/ef6/media/tptexample.jpg)

<span data-ttu-id="3e21a-184">È possibile evitare questo problema e gestire il mapping di tabella per gerarchia predefinita, in due modi:</span><span class="sxs-lookup"><span data-stu-id="3e21a-184">You can avoid this, and maintain the default TPH mapping, in a couple ways:</span></span>

1.  <span data-ttu-id="3e21a-185">Chiamare ToTable con lo stesso nome di tabella per ogni tipo nella gerarchia.</span><span class="sxs-lookup"><span data-stu-id="3e21a-185">Call ToTable with the same table name for each type in the hierarchy.</span></span>
2.  <span data-ttu-id="3e21a-186">Chiamare ToTable solo sulla classe di base della gerarchia, nel nostro esempio che potrebbe essere dipendente.</span><span class="sxs-lookup"><span data-stu-id="3e21a-186">Call ToTable only on the base class of the hierarchy, in our example that would be employee.</span></span>

 

## <a name="execution-order"></a><span data-ttu-id="3e21a-187">Ordine di esecuzione</span><span class="sxs-lookup"><span data-stu-id="3e21a-187">Execution Order</span></span>

<span data-ttu-id="3e21a-188">Convenzioni di operano in modo wins ultimo quello utilizzato per l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="3e21a-188">Conventions operate in a last wins manner, the same as the Fluent API.</span></span> <span data-ttu-id="3e21a-189">Ciò significa che se si scrive due convenzioni che consentono di configurare la stessa opzione della stessa proprietà, quindi quello più recente per l'esecuzione wins.</span><span class="sxs-lookup"><span data-stu-id="3e21a-189">What this means is that if you write two conventions that configure the same option of the same property, then the last one to execute wins.</span></span> <span data-ttu-id="3e21a-190">Ad esempio, nel codice seguente è impostata la lunghezza massima di tutte le stringhe fino a 500 ma configuriamo quindi tutte le proprietà denominate Name nel modello di avere una lunghezza massima pari a 250.</span><span class="sxs-lookup"><span data-stu-id="3e21a-190">As an example, in the code below the max length of all strings is set to 500 but we then configure all properties called Name in the model to have a max length of 250.</span></span>

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

<span data-ttu-id="3e21a-191">Poiché la convenzione per impostare della lunghezza massima di 250 è successiva a quella che consente di impostare tutte le stringhe fino a 500, tutte le proprietà denominate Name nel modello avrà una lunghezza massima di 250 durante qualsiasi altra stringa, ad esempio le descrizioni, è 500.</span><span class="sxs-lookup"><span data-stu-id="3e21a-191">Because the convention to set max length to 250 is after the one that sets all strings to 500, all the properties called Name in our model will have a MaxLength of 250 while any other strings, such as descriptions, would be 500.</span></span> <span data-ttu-id="3e21a-192">L'uso di convenzioni in questo modo significa che è possibile fornire una convenzione generale per i tipi o le proprietà nel modello e quindi overide per subset diversi.</span><span class="sxs-lookup"><span data-stu-id="3e21a-192">Using conventions in this way means that you can provide a general convention for types or properties in your model and then overide them for subsets that are different.</span></span>

<span data-ttu-id="3e21a-193">L'API Fluent e le annotazioni dei dati è anche utilizzabile per eseguire l'override di una convenzione in casi specifici.</span><span class="sxs-lookup"><span data-stu-id="3e21a-193">The Fluent API and Data Annotations can also be used to override a convention in specific cases.</span></span> <span data-ttu-id="3e21a-194">Nell'esempio riportato sopra se avessimo usato l'API Fluent per impostare la lunghezza massima di una proprietà quindi, sarebbe possibile avere inserirlo prima o dopo la convenzione, perché l'API Fluent più specifica avrà sempre la precedenza tramite la convenzione di configurazione più generale.</span><span class="sxs-lookup"><span data-stu-id="3e21a-194">In our example above if we had used the Fluent API to set the max length of a property then we could have put it before or after the convention, because the more specific Fluent API will win over the more general Configuration Convention.</span></span>

 

## <a name="built-in-conventions"></a><span data-ttu-id="3e21a-195">Convenzioni predefinite</span><span class="sxs-lookup"><span data-stu-id="3e21a-195">Built-in Conventions</span></span>

<span data-ttu-id="3e21a-196">Poiché le convenzioni personalizzate può esserne influenzate dalle convenzioni Code First predefiniti, può essere utile aggiungere convenzioni da eseguire prima o dopo un'altra convenzione.</span><span class="sxs-lookup"><span data-stu-id="3e21a-196">Because custom conventions could be affected by the default Code First conventions, it can be useful to add conventions to run before or after another convention.</span></span> <span data-ttu-id="3e21a-197">A tale scopo è possibile utilizzare i metodi AddBefore e AddAfter della raccolta convenzioni in DbContext derivata.</span><span class="sxs-lookup"><span data-stu-id="3e21a-197">To do this you can use the AddBefore and AddAfter methods of the Conventions collection on your derived DbContext.</span></span> <span data-ttu-id="3e21a-198">Il codice seguente sarebbe aggiungere la classe convenzione creato in precedenza, in modo che venga eseguito prima incorporato nella convenzione di individuazione.</span><span class="sxs-lookup"><span data-stu-id="3e21a-198">The following code would add the convention class we created earlier so that it will run before the built in key discovery convention.</span></span>

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

<span data-ttu-id="3e21a-199">Si sta per essere più utile quando si aggiungono le convenzioni che dovranno essere eseguiti prima o dopo le convenzioni predefinite, un elenco delle convenzioni predefinite è disponibili qui: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .</span><span class="sxs-lookup"><span data-stu-id="3e21a-199">This is going to be of the most use when adding conventions that need to run before or after the built in conventions, a list of the built in conventions can be found here: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>

<span data-ttu-id="3e21a-200">È anche possibile rimuovere le convenzioni che non si desidera applicate al modello.</span><span class="sxs-lookup"><span data-stu-id="3e21a-200">You can also remove conventions that you do not want applied to your model.</span></span> <span data-ttu-id="3e21a-201">Per rimuovere una convenzione, usare il metodo Remove.</span><span class="sxs-lookup"><span data-stu-id="3e21a-201">To remove a convention, use the Remove method.</span></span> <span data-ttu-id="3e21a-202">Ecco un esempio di rimozione di PluralizingTableNameConvention.</span><span class="sxs-lookup"><span data-stu-id="3e21a-202">Here is an example of removing the PluralizingTableNameConvention.</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
