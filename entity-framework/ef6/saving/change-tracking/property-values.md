---
title: Utilizzo dei valori delle proprietà - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
ms.openlocfilehash: afde503bb4ed15fcf83a57053541cd5da8c89835
ms.sourcegitcommit: 50521b4a2f71139e6a7210a69ac73da582ef46cf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/27/2019
ms.locfileid: "67416667"
---
# <a name="working-with-property-values"></a><span data-ttu-id="75166-102">Utilizzo di valori di proprietà</span><span class="sxs-lookup"><span data-stu-id="75166-102">Working with property values</span></span>
<span data-ttu-id="75166-103">Per la maggior parte Entity Framework si occuperà di stato, i valori originali e i valori correnti delle proprietà delle istanze di entità con rilevamento.</span><span class="sxs-lookup"><span data-stu-id="75166-103">For the most part Entity Framework will take care of tracking the state, original values, and current values of the properties of your entity instances.</span></span> <span data-ttu-id="75166-104">Tuttavia, potrebbero esserci alcuni casi, ad esempio gli scenari disconnessi, in cui si desidera visualizzare o modificare le informazioni che EF ha le proprietà.</span><span class="sxs-lookup"><span data-stu-id="75166-104">However, there may be some cases - such as disconnected scenarios - where you want to view or manipulate the information EF has about the properties.</span></span> <span data-ttu-id="75166-105">Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.</span><span class="sxs-lookup"><span data-stu-id="75166-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="75166-106">Entity Framework tiene traccia di due valori per ogni proprietà di un'entità rilevata.</span><span class="sxs-lookup"><span data-stu-id="75166-106">Entity Framework keeps track of two values for each property of a tracked entity.</span></span> <span data-ttu-id="75166-107">Il valore corrente è, come indica il nome, il valore corrente della proprietà nell'entità.</span><span class="sxs-lookup"><span data-stu-id="75166-107">The current value is, as the name indicates, the current value of the property in the entity.</span></span> <span data-ttu-id="75166-108">Il valore originale è il valore che la proprietà conteneva quando l'entità è stata sottoposti a query dal database o connesso al contesto.</span><span class="sxs-lookup"><span data-stu-id="75166-108">The original value is the value that the property had when the entity was queried from the database or attached to the context.</span></span>  

<span data-ttu-id="75166-109">Esistono due meccanismi generali per l'utilizzo con valori di proprietà:</span><span class="sxs-lookup"><span data-stu-id="75166-109">There are two general mechanisms for working with property values:</span></span>  

- <span data-ttu-id="75166-110">Il valore di una singola proprietà può essere ottenuto in modo fortemente tipizzato utilizzando il metodo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="75166-110">The value of a single property can be obtained in a strongly typed way using the Property method.</span></span>  
- <span data-ttu-id="75166-111">I valori per tutte le proprietà di un'entità possono essere letti in un oggetto DbPropertyValues.</span><span class="sxs-lookup"><span data-stu-id="75166-111">Values for all properties of an entity can be read into a DbPropertyValues object.</span></span> <span data-ttu-id="75166-112">DbPropertyValues funge quindi da un oggetto di tipo dizionario per i valori delle proprietà da leggere e impostare.</span><span class="sxs-lookup"><span data-stu-id="75166-112">DbPropertyValues then acts as a dictionary-like object to allow property values to be read and set.</span></span> <span data-ttu-id="75166-113">I valori in un oggetto DbPropertyValues possono essere impostati dai valori in un altro oggetto DbPropertyValues o dai valori in un altro oggetto, ad esempio un'altra copia dell'entità o a un oggetto di trasferimento di dati semplici (DTO).</span><span class="sxs-lookup"><span data-stu-id="75166-113">The values in a DbPropertyValues object can be set from values in another DbPropertyValues object or from values in some other object, such as another copy of the entity or a simple data transfer object (DTO).</span></span>  

<span data-ttu-id="75166-114">Le sezioni seguenti mostrano esempi di utilizzo di entrambi i meccanismi sopra.</span><span class="sxs-lookup"><span data-stu-id="75166-114">The sections below show examples of using both of the above mechanisms.</span></span>  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a><span data-ttu-id="75166-115">Ottenere e impostare il valore originale o corrente di una singola proprietà</span><span class="sxs-lookup"><span data-stu-id="75166-115">Getting and setting the current or original value of an individual property</span></span>  

<span data-ttu-id="75166-116">L'esempio seguente mostra come il valore corrente di una proprietà può essere letti e quindi impostato su un nuovo valore:</span><span class="sxs-lookup"><span data-stu-id="75166-116">The example below shows how the current value of a property can be read and then set to a new value:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(3);

    // Read the current value of the Name property
    string currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;

    // Set the Name property to a new value
    context.Entry(blog).Property(u => u.Name).CurrentValue = "My Fancy Blog";

    // Read the current value of the Name property using a string for the property name
    object currentName2 = context.Entry(blog).Property("Name").CurrentValue;

    // Set the Name property to a new value using a string for the property name
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

<span data-ttu-id="75166-117">Usare la proprietà OriginalValue anziché la proprietà CurrentValue leggerne o impostarne il valore originale.</span><span class="sxs-lookup"><span data-stu-id="75166-117">Use the OriginalValue property instead of the CurrentValue property to read or set the original value.</span></span>  

<span data-ttu-id="75166-118">Si noti che il valore restituito è tipizzato come "object" quando una stringa viene utilizzata per specificare il nome della proprietà.</span><span class="sxs-lookup"><span data-stu-id="75166-118">Note that the returned value is typed as “object” when a string is used to specify the property name.</span></span> <span data-ttu-id="75166-119">D'altra parte, il valore restituito è fortemente tipizzato se viene usata un'espressione lambda.</span><span class="sxs-lookup"><span data-stu-id="75166-119">On the other hand, the returned value is strongly typed if a lambda expression is used.</span></span>  

<span data-ttu-id="75166-120">Impostare il valore della proprietà simile al seguente solo contrassegnerà la proprietà come modificata se il nuovo valore è diverso dal valore precedente.</span><span class="sxs-lookup"><span data-stu-id="75166-120">Setting the property value like this will only mark the property as modified if the new value is different from the old value.</span></span>  

<span data-ttu-id="75166-121">Quando un valore della proprietà è impostato in questo modo la modifica viene rilevata automaticamente anche se AutoDetectChanges è disattivato.</span><span class="sxs-lookup"><span data-stu-id="75166-121">When a property value is set in this way the change is automatically detected even if AutoDetectChanges is turned off.</span></span>  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a><span data-ttu-id="75166-122">Ottenere e impostare il valore corrente di una proprietà non mappata</span><span class="sxs-lookup"><span data-stu-id="75166-122">Getting and setting the current value of an unmapped property</span></span>  

<span data-ttu-id="75166-123">Il valore corrente di una proprietà che non è mappato al database può anche essere letti.</span><span class="sxs-lookup"><span data-stu-id="75166-123">The current value of a property that is not mapped to the database can also be read.</span></span> <span data-ttu-id="75166-124">Un esempio di una proprietà non mappata potrebbe essere una proprietà RssLink nel Blog.</span><span class="sxs-lookup"><span data-stu-id="75166-124">An example of an unmapped property could be an RssLink property on Blog.</span></span> <span data-ttu-id="75166-125">Questo valore può essere calcolato in base il BlogId e pertanto non desidera essere archiviate nel database.</span><span class="sxs-lookup"><span data-stu-id="75166-125">This value may be calculated based on the BlogId, and therefore doesn't need to be stored in the database.</span></span> <span data-ttu-id="75166-126">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="75166-126">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    // Read the current value of an unmapped property
    var rssLink = context.Entry(blog).Property(p => p.RssLink).CurrentValue;

    // Use a string to specify the property name
    var rssLinkAgain = context.Entry(blog).Property("RssLink").CurrentValue;
}
```  

<span data-ttu-id="75166-127">Il valore corrente può essere impostato anche se la proprietà espone un setter.</span><span class="sxs-lookup"><span data-stu-id="75166-127">The current value can also be set if the property exposes a setter.</span></span>  

<span data-ttu-id="75166-128">Leggendo i valori delle proprietà non mappata è utile quando si esegue la convalida di Entity Framework di proprietà non mappata.</span><span class="sxs-lookup"><span data-stu-id="75166-128">Reading the values of unmapped properties is useful when performing Entity Framework validation of unmapped properties.</span></span> <span data-ttu-id="75166-129">Per lo stesso motivo i valori correnti possono leggere e impostare per le proprietà di entità che non sono attualmente controllata dal contesto.</span><span class="sxs-lookup"><span data-stu-id="75166-129">For the same reason current values can be read and set for properties of entities that are not currently being tracked by the context.</span></span> <span data-ttu-id="75166-130">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="75166-130">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Create an entity that is not being tracked
    var blog = new Blog { Name = "ADO.NET Blog" };

    // Read and set the current value of Name as before
    var currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;
    context.Entry(blog).Property(u => u.Name).CurrentValue = "My Fancy Blog";
    var currentName2 = context.Entry(blog).Property("Name").CurrentValue;
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

<span data-ttu-id="75166-131">Si noti che i valori originali non sono disponibili per le proprietà non mappate o per le proprietà delle entità che non vengono rilevate dal contesto.</span><span class="sxs-lookup"><span data-stu-id="75166-131">Note that original values are not available for unmapped properties or for properties of entities that are not being tracked by the context.</span></span>  

## <a name="checking-whether-a-property-is-marked-as-modified"></a><span data-ttu-id="75166-132">Verifica se una proprietà è contrassegnata come modificata</span><span class="sxs-lookup"><span data-stu-id="75166-132">Checking whether a property is marked as modified</span></span>  

<span data-ttu-id="75166-133">L'esempio seguente viene illustrato come controllare se una singola proprietà è contrassegnata come modificata:</span><span class="sxs-lookup"><span data-stu-id="75166-133">The example below shows how to check whether or not an individual property is marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

<span data-ttu-id="75166-134">I valori delle proprietà modificate vengono inviati come gli aggiornamenti al database quando viene chiamato SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="75166-134">The values of modified properties are sent as updates to the database when SaveChanges is called.</span></span>  

##  <a name="marking-a-property-as-modified"></a><span data-ttu-id="75166-135">Contrassegno di una proprietà come modificata</span><span class="sxs-lookup"><span data-stu-id="75166-135">Marking a property as modified</span></span>  

<span data-ttu-id="75166-136">L'esempio seguente viene illustrato come forzare una singola proprietà da contrassegnare come modificato:</span><span class="sxs-lookup"><span data-stu-id="75166-136">The example below shows how to force an individual property to be marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

<span data-ttu-id="75166-137">Contrassegno di una proprietà come modificata forza un aggiornamento da inviare al database per la proprietà quando viene chiamato SaveChanges anche se il valore corrente della proprietà è uguale a quello originale.</span><span class="sxs-lookup"><span data-stu-id="75166-137">Marking a property as modified forces an update to be send to the database for the property when SaveChanges is called even if the current value of the property is the same as its original value.</span></span>  

<span data-ttu-id="75166-138">Non è attualmente possibile reimpostare una singola proprietà da modificare non dopo che è stato contrassegnato come modificato.</span><span class="sxs-lookup"><span data-stu-id="75166-138">It is not currently possible to reset an individual property to be not modified after it has been marked as modified.</span></span> <span data-ttu-id="75166-139">Si tratta di qualcosa che si prevede di supportare in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="75166-139">This is something we plan to support in a future release.</span></span>  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a><span data-ttu-id="75166-140">La lettura correnti, originali e i valori del database per tutte le proprietà di un'entità</span><span class="sxs-lookup"><span data-stu-id="75166-140">Reading current, original, and database values for all properties of an entity</span></span>  

<span data-ttu-id="75166-141">L'esempio seguente viene illustrato come leggere i valori correnti, i valori originali e i valori effettivamente nel database per tutte le proprietà con mapping di un'entità.</span><span class="sxs-lookup"><span data-stu-id="75166-141">The example below shows how to read the current values, the original values, and the values actually in the database for all mapped properties of an entity.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Make a modification to Name in the tracked entity
    blog.Name = "My Cool Blog";

    // Make a modification to Name in the database
    context.Database.SqlCommand("update dbo.Blogs set Name = 'My Boring Blog' where Id = 1");

    // Print out current, original, and database values
    Console.WriteLine("Current values:");
    PrintValues(context.Entry(blog).CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(context.Entry(blog).OriginalValues);

    Console.WriteLine("\nDatabase values:");
    PrintValues(context.Entry(blog).GetDatabaseValues());
}

public static void PrintValues(DbPropertyValues values)
{
    foreach (var propertyName in values.PropertyNames)
    {
        Console.WriteLine("Property {0} has value {1}",
                          propertyName, values[propertyName]);
    }
}
```  

<span data-ttu-id="75166-142">I valori correnti sono i valori attualmente contenenti le proprietà dell'entità.</span><span class="sxs-lookup"><span data-stu-id="75166-142">The current values are the values that the properties of the entity currently contain.</span></span> <span data-ttu-id="75166-143">I valori originali sono i valori letti dal database quando l'entità è stata eseguita una query.</span><span class="sxs-lookup"><span data-stu-id="75166-143">The original values are the values that were read from the database when the entity was queried.</span></span> <span data-ttu-id="75166-144">I valori del database sono i valori attualmente archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="75166-144">The database values are the values as they are currently stored in the database.</span></span> <span data-ttu-id="75166-145">Ottenere i valori del database è utile quando i valori del database possono essere modificata perché l'entità è stata eseguita una query, ad esempio quando un simultanea modifica per il database è diventato da un altro utente.</span><span class="sxs-lookup"><span data-stu-id="75166-145">Getting the database values is useful when the values in the database may have changed since the entity was queried such as when a concurrent edit to the database has been made by another user.</span></span>  

## <a name="setting-current-or-original-values-from-another-object"></a><span data-ttu-id="75166-146">Impostazione dei valori correnti o originali da un altro oggetto</span><span class="sxs-lookup"><span data-stu-id="75166-146">Setting current or original values from another object</span></span>  

<span data-ttu-id="75166-147">I valori correnti o originali di un'entità rilevata possono essere aggiornati tramite la copia di valori da un altro oggetto.</span><span class="sxs-lookup"><span data-stu-id="75166-147">The current or original values of a tracked entity can be updated by copying values from another object.</span></span> <span data-ttu-id="75166-148">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="75166-148">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var coolBlog = new Blog { Id = 1, Name = "My Cool Blog" };
    var boringBlog = new BlogDto { Id = 1, Name = "My Boring Blog" };

    // Change the current and original values by copying the values from other objects
    var entry = context.Entry(blog);
    entry.CurrentValues.SetValues(coolBlog);
    entry.OriginalValues.SetValues(boringBlog);

    // Print out current and original values
    Console.WriteLine("Current values:");
    PrintValues(entry.CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(entry.OriginalValues);
}

public class BlogDto
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```  

<span data-ttu-id="75166-149">Esecuzione di codice precedente consente di stampare:</span><span class="sxs-lookup"><span data-stu-id="75166-149">Running the code above will print out:</span></span>  

```  
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

<span data-ttu-id="75166-150">Questa tecnica viene usata in alcuni casi quando si aggiorna un'entità con valori ottenuti da una chiamata al servizio o un client in un'applicazione a più livelli.</span><span class="sxs-lookup"><span data-stu-id="75166-150">This technique is sometimes used when updating an entity with values obtained from a service call or a client in an n-tier application.</span></span> <span data-ttu-id="75166-151">Si noti che l'oggetto usato non deve essere dello stesso tipo dell'entità, purché tale classe presenta proprietà i cui nomi corrispondono a quelle dell'entità.</span><span class="sxs-lookup"><span data-stu-id="75166-151">Note that the object used does not have to be of the same type as the entity so long as it has properties whose names match those of the entity.</span></span> <span data-ttu-id="75166-152">Nell'esempio precedente, un'istanza di BlogDTO consente di aggiornare i valori originali.</span><span class="sxs-lookup"><span data-stu-id="75166-152">In the example above, an instance of BlogDTO is used to update the original values.</span></span>  

<span data-ttu-id="75166-153">Si noti che solo le proprietà impostate su valori diversi quando copiati da altro oggetto verranno contrassegnate come modificata.</span><span class="sxs-lookup"><span data-stu-id="75166-153">Note that only properties that are set to different values when copied from the other object will be marked as modified.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary"></a><span data-ttu-id="75166-154">Impostazione dei valori correnti o originali da un dizionario</span><span class="sxs-lookup"><span data-stu-id="75166-154">Setting current or original values from a dictionary</span></span>  

<span data-ttu-id="75166-155">I valori correnti o originali di un'entità rilevata possono essere aggiornati tramite la copia di valori da un dizionario o altre strutture di dati.</span><span class="sxs-lookup"><span data-stu-id="75166-155">The current or original values of a tracked entity can be updated by copying values from a dictionary or some other data structure.</span></span> <span data-ttu-id="75166-156">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="75166-156">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "The New ADO.NET Blog" },
        { "Url", "blogs.msdn.com/adonet" },
    };

    var currentValues = context.Entry(blog).CurrentValues;

    foreach (var propertyName in newValues.Keys)
    {
        currentValues[propertyName] = newValues[propertyName];
    }

    PrintValues(currentValues);
}
```  

<span data-ttu-id="75166-157">Usare la proprietà OriginalValues anziché la proprietà CurrentValues per impostare i valori originali.</span><span class="sxs-lookup"><span data-stu-id="75166-157">Use the OriginalValues property instead of the CurrentValues property to set original values.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a><span data-ttu-id="75166-158">Impostazione dei valori correnti o originali da un dizionario con proprietà</span><span class="sxs-lookup"><span data-stu-id="75166-158">Setting current or original values from a dictionary using Property</span></span>  

<span data-ttu-id="75166-159">Un'alternativa all'utilizzo CurrentValues o OriginalValues come illustrato in precedenza consiste nell'usare il metodo di proprietà per impostare il valore di ogni proprietà.</span><span class="sxs-lookup"><span data-stu-id="75166-159">An alternative to using CurrentValues or OriginalValues as shown above is to use the Property method to set the value of each property.</span></span> <span data-ttu-id="75166-160">Ciò può essere preferibile quando è necessario impostare i valori di proprietà complesse.</span><span class="sxs-lookup"><span data-stu-id="75166-160">This can be preferable when you need to set the values of complex properties.</span></span> <span data-ttu-id="75166-161">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="75166-161">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "John Doe" },
        { "Location.City", "Redmond" },
        { "Location.State.Name", "Washington" },
        { "Location.State.Code", "WA" },
    };

    var entry = context.Entry(user);

    foreach (var propertyName in newValues.Keys)
    {
        entry.Property(propertyName).CurrentValue = newValues[propertyName];
    }
}
```  

<span data-ttu-id="75166-162">Nell'esempio precedente le proprietà complesse sono accessibili tramite nomi punteggiati.</span><span class="sxs-lookup"><span data-stu-id="75166-162">In the example above complex properties are accessed using dotted names.</span></span> <span data-ttu-id="75166-163">Per altri modi per accedere alle proprietà complesse vedere due sezioni più avanti in questo argomento in particolare sulle proprietà complesse.</span><span class="sxs-lookup"><span data-stu-id="75166-163">For other ways to access complex properties see the two sections later in this topic specifically about complex properties.</span></span>  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a><span data-ttu-id="75166-164">Creazione di un oggetto clonato contenente i valori del database, originale o corrente</span><span class="sxs-lookup"><span data-stu-id="75166-164">Creating a cloned object containing current, original, or database values</span></span>  

<span data-ttu-id="75166-165">L'oggetto DbPropertyValues restituito dal CurrentValues, OriginalValues, o GetDatabaseValues può essere utilizzato per creare un clone dell'entità.</span><span class="sxs-lookup"><span data-stu-id="75166-165">The DbPropertyValues object returned from CurrentValues, OriginalValues, or GetDatabaseValues can be used to create a clone of the entity.</span></span> <span data-ttu-id="75166-166">Questo clone conterrà i valori della proprietà dall'oggetto DbPropertyValues usata per crearlo.</span><span class="sxs-lookup"><span data-stu-id="75166-166">This clone will contain the property values from the DbPropertyValues object used to create it.</span></span> <span data-ttu-id="75166-167">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="75166-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

<span data-ttu-id="75166-168">Si noti che l'oggetto restituito non sono l'entità e non viene rilevato dal contesto.</span><span class="sxs-lookup"><span data-stu-id="75166-168">Note that the object returned is not the entity and is not being tracked by the context.</span></span> <span data-ttu-id="75166-169">L'oggetto restituito inoltre non ha alcuna relazione impostati su altri oggetti.</span><span class="sxs-lookup"><span data-stu-id="75166-169">The returned object also does not have any relationships set to other objects.</span></span>  

<span data-ttu-id="75166-170">Oggetto clonato può essere utile per la risoluzione dei problemi relativi agli aggiornamenti simultanei al database, in particolare in cui è in uso un'interfaccia utente che coinvolge data binding a oggetti di un determinato tipo.</span><span class="sxs-lookup"><span data-stu-id="75166-170">The cloned object can be useful for resolving issues related to concurrent updates to the database, especially where a UI that involves data binding to objects of a certain type is being used.</span></span>  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a><span data-ttu-id="75166-171">Ottenere e impostare i valori correnti o originali delle proprietà complessa</span><span class="sxs-lookup"><span data-stu-id="75166-171">Getting and setting the current or original values of complex properties</span></span>  

<span data-ttu-id="75166-172">Il valore di un intero oggetto complesso può essere letto e impostato utilizzando il metodo di proprietà, come è possibile per una proprietà primitiva.</span><span class="sxs-lookup"><span data-stu-id="75166-172">The value of an entire complex object can be read and set using the Property method just as it can be for a primitive property.</span></span> <span data-ttu-id="75166-173">Inoltre è possibile risalire alle oggetto complesso e lettura o impostare le proprietà di quell'oggetto, o anche un oggetto nidificato.</span><span class="sxs-lookup"><span data-stu-id="75166-173">In addition you can drill down into the complex object and read or set properties of that object, or even a nested object.</span></span> <span data-ttu-id="75166-174">Ecco alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="75166-174">Here are some examples:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    // Get the Location complex object
    var location = context.Entry(user)
                       .Property(u => u.Location)
                       .CurrentValue;

    // Get the nested State complex object using chained calls
    var state1 = context.Entry(user)
                     .ComplexProperty(u => u.Location)
                     .Property(l => l.State)
                     .CurrentValue;

    // Get the nested State complex object using a single lambda expression
    var state2 = context.Entry(user)
                     .Property(u => u.Location.State)
                     .CurrentValue;

    // Get the nested State complex object using a dotted string
    var state3 = context.Entry(user)
                     .Property("Location.State")
                     .CurrentValue;

    // Get the value of the Name property on the nested State complex object using chained calls
    var name1 = context.Entry(user)
                       .ComplexProperty(u => u.Location)
                       .ComplexProperty(l => l.State)
                       .Property(s => s.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a single lambda expression
    var name2 = context.Entry(user)
                       .Property(u => u.Location.State.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a dotted string
    var name3 = context.Entry(user)
                       .Property("Location.State.Name")
                       .CurrentValue;
}
```  

<span data-ttu-id="75166-175">Usare la proprietà OriginalValue anziché la proprietà CurrentValue da ottenere o impostare un valore originale.</span><span class="sxs-lookup"><span data-stu-id="75166-175">Use the OriginalValue property instead of the CurrentValue property to get or set an original value.</span></span>  

<span data-ttu-id="75166-176">Si noti che la proprietà o metodo ComplexProperty utilizzabile per accedere a una proprietà complessa.</span><span class="sxs-lookup"><span data-stu-id="75166-176">Note that either the Property or the ComplexProperty method can be used to access a complex property.</span></span> <span data-ttu-id="75166-177">Tuttavia, il metodo ComplexProperty deve essere usato se si vuole eseguire il drill-nell'oggetto complesso con proprietà aggiuntive o ComplexProperty chiamate.</span><span class="sxs-lookup"><span data-stu-id="75166-177">However, the ComplexProperty method must be used if you wish to drill down into the complex object with additional Property or ComplexProperty calls.</span></span>  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a><span data-ttu-id="75166-178">Uso di DbPropertyValues per accedere alle proprietà complesse</span><span class="sxs-lookup"><span data-stu-id="75166-178">Using DbPropertyValues to access complex properties</span></span>  

<span data-ttu-id="75166-179">Quando si usa CurrentValues, OriginalValues o GetDatabaseValues per ottenere l'oggetto corrente, originale, o i valori del database per un'entità, i valori di qualsiasi proprietà complesse vengono restituiti come oggetti DbPropertyValues annidati.</span><span class="sxs-lookup"><span data-stu-id="75166-179">When you use CurrentValues, OriginalValues, or GetDatabaseValues to get all the current, original, or database values for an entity, the values of any complex properties are returned as nested DbPropertyValues objects.</span></span> <span data-ttu-id="75166-180">Questi annidati gli oggetti possono quindi essere usati per ottenere i valori dell'oggetto complesso.</span><span class="sxs-lookup"><span data-stu-id="75166-180">These nested objects can then be used to get values of the complex object.</span></span> <span data-ttu-id="75166-181">Ad esempio, il metodo seguente verrà stampato i valori di tutte le proprietà, inclusi i valori di qualsiasi proprietà complesse e proprietà complesse nidificate.</span><span class="sxs-lookup"><span data-stu-id="75166-181">For example, the following method will print out the values of all properties, including values of any complex properties and nested complex properties.</span></span>  

``` csharp
public static void WritePropertyValues(string parentPropertyName, DbPropertyValues propertyValues)
{
    foreach (var propertyName in propertyValues.PropertyNames)
    {
        var nestedValues = propertyValues[propertyName] as DbPropertyValues;
        if (nestedValues != null)
        {
            WritePropertyValues(parentPropertyName + propertyName + ".", nestedValues);
        }
        else
        {
            Console.WriteLine("Property {0}{1} has value {2}",
                              parentPropertyName, propertyName,
                              propertyValues[propertyName]);
        }
    }
}
```  

<span data-ttu-id="75166-182">Per stampare una copia tutti i valori correnti delle proprietà del metodo viene chiamato come segue:</span><span class="sxs-lookup"><span data-stu-id="75166-182">To print out all current property values the method would be called like this:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
