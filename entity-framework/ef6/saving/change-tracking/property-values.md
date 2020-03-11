---
title: Uso dei valori delle proprietà-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
ms.openlocfilehash: d8a18182754980d79b71df3f227b30c4ce40366f
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416967"
---
# <a name="working-with-property-values"></a><span data-ttu-id="4ded0-102">Utilizzo dei valori delle proprietà</span><span class="sxs-lookup"><span data-stu-id="4ded0-102">Working with property values</span></span>
<span data-ttu-id="4ded0-103">Per la maggior parte Entity Framework si occuperà di tenere traccia dello stato, dei valori originali e dei valori correnti delle proprietà delle istanze dell'entità.</span><span class="sxs-lookup"><span data-stu-id="4ded0-103">For the most part Entity Framework will take care of tracking the state, original values, and current values of the properties of your entity instances.</span></span> <span data-ttu-id="4ded0-104">Tuttavia, potrebbero verificarsi alcuni casi, ad esempio gli scenari disconnessi, in cui si desidera visualizzare o modificare le informazioni di EF sulle proprietà.</span><span class="sxs-lookup"><span data-stu-id="4ded0-104">However, there may be some cases - such as disconnected scenarios - where you want to view or manipulate the information EF has about the properties.</span></span> <span data-ttu-id="4ded0-105">Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.</span><span class="sxs-lookup"><span data-stu-id="4ded0-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="4ded0-106">Entity Framework tiene traccia di due valori per ogni proprietà di un'entità rilevata.</span><span class="sxs-lookup"><span data-stu-id="4ded0-106">Entity Framework keeps track of two values for each property of a tracked entity.</span></span> <span data-ttu-id="4ded0-107">Il valore corrente è, come indica il nome, il valore corrente della proprietà nell'entità.</span><span class="sxs-lookup"><span data-stu-id="4ded0-107">The current value is, as the name indicates, the current value of the property in the entity.</span></span> <span data-ttu-id="4ded0-108">Il valore originale è il valore che la proprietà aveva quando è stata eseguita una query sul database o è stato associato al contesto.</span><span class="sxs-lookup"><span data-stu-id="4ded0-108">The original value is the value that the property had when the entity was queried from the database or attached to the context.</span></span>  

<span data-ttu-id="4ded0-109">Sono disponibili due meccanismi generali per l'utilizzo dei valori delle proprietà:</span><span class="sxs-lookup"><span data-stu-id="4ded0-109">There are two general mechanisms for working with property values:</span></span>  

- <span data-ttu-id="4ded0-110">Il valore di una singola proprietà può essere ottenuto in modo fortemente tipizzato usando il metodo Property.</span><span class="sxs-lookup"><span data-stu-id="4ded0-110">The value of a single property can be obtained in a strongly typed way using the Property method.</span></span>  
- <span data-ttu-id="4ded0-111">I valori per tutte le proprietà di un'entità possono essere letti in un oggetto elementi dbpropertyvalues.</span><span class="sxs-lookup"><span data-stu-id="4ded0-111">Values for all properties of an entity can be read into a DbPropertyValues object.</span></span> <span data-ttu-id="4ded0-112">Elementi dbpropertyvalues funge quindi da oggetto simile a un dizionario per consentire la lettura e l'impostazione dei valori delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="4ded0-112">DbPropertyValues then acts as a dictionary-like object to allow property values to be read and set.</span></span> <span data-ttu-id="4ded0-113">I valori in un oggetto elementi dbpropertyvalues possono essere impostati dai valori di un altro oggetto elementi dbpropertyvalues o dai valori in un altro oggetto, ad esempio un'altra copia dell'entità o un oggetto di trasferimento dati semplice (DTO).</span><span class="sxs-lookup"><span data-stu-id="4ded0-113">The values in a DbPropertyValues object can be set from values in another DbPropertyValues object or from values in some other object, such as another copy of the entity or a simple data transfer object (DTO).</span></span>  

<span data-ttu-id="4ded0-114">Le sezioni seguenti illustrano esempi di utilizzo di entrambi i meccanismi descritti in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4ded0-114">The sections below show examples of using both of the above mechanisms.</span></span>  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a><span data-ttu-id="4ded0-115">Recupero e impostazione del valore corrente o originale di una singola proprietà</span><span class="sxs-lookup"><span data-stu-id="4ded0-115">Getting and setting the current or original value of an individual property</span></span>  

<span data-ttu-id="4ded0-116">Nell'esempio seguente viene illustrato come è possibile leggere il valore corrente di una proprietà e quindi impostarla su un nuovo valore:</span><span class="sxs-lookup"><span data-stu-id="4ded0-116">The example below shows how the current value of a property can be read and then set to a new value:</span></span>  

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

<span data-ttu-id="4ded0-117">Usare la proprietà OriginalValue invece della proprietà CurrentValue per leggere o impostare il valore originale.</span><span class="sxs-lookup"><span data-stu-id="4ded0-117">Use the OriginalValue property instead of the CurrentValue property to read or set the original value.</span></span>  

<span data-ttu-id="4ded0-118">Si noti che il valore restituito viene digitato come "Object" quando viene utilizzata una stringa per specificare il nome della proprietà.</span><span class="sxs-lookup"><span data-stu-id="4ded0-118">Note that the returned value is typed as “object” when a string is used to specify the property name.</span></span> <span data-ttu-id="4ded0-119">D'altra parte, il valore restituito è fortemente tipizzato se viene utilizzata un'espressione lambda.</span><span class="sxs-lookup"><span data-stu-id="4ded0-119">On the other hand, the returned value is strongly typed if a lambda expression is used.</span></span>  

<span data-ttu-id="4ded0-120">Impostando il valore della proprietà come questo, la proprietà verrà contrassegnata come modificata solo se il nuovo valore è diverso dal valore precedente.</span><span class="sxs-lookup"><span data-stu-id="4ded0-120">Setting the property value like this will only mark the property as modified if the new value is different from the old value.</span></span>  

<span data-ttu-id="4ded0-121">Quando un valore di proprietà viene impostato in questo modo, la modifica viene rilevata automaticamente anche se AutoDetectChanges è disattivato.</span><span class="sxs-lookup"><span data-stu-id="4ded0-121">When a property value is set in this way the change is automatically detected even if AutoDetectChanges is turned off.</span></span>  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a><span data-ttu-id="4ded0-122">Recupero e impostazione del valore corrente di una proprietà non mappata</span><span class="sxs-lookup"><span data-stu-id="4ded0-122">Getting and setting the current value of an unmapped property</span></span>  

<span data-ttu-id="4ded0-123">È anche possibile leggere il valore corrente di una proprietà di cui non è stato eseguito il mapping al database.</span><span class="sxs-lookup"><span data-stu-id="4ded0-123">The current value of a property that is not mapped to the database can also be read.</span></span> <span data-ttu-id="4ded0-124">Un esempio di proprietà di cui non è stato eseguito il mapping può essere una proprietà RssLink nel Blog.</span><span class="sxs-lookup"><span data-stu-id="4ded0-124">An example of an unmapped property could be an RssLink property on Blog.</span></span> <span data-ttu-id="4ded0-125">Questo valore può essere calcolato in base a BlogId e pertanto non deve essere archiviato nel database.</span><span class="sxs-lookup"><span data-stu-id="4ded0-125">This value may be calculated based on the BlogId, and therefore doesn't need to be stored in the database.</span></span> <span data-ttu-id="4ded0-126">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4ded0-126">For example:</span></span>  

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

<span data-ttu-id="4ded0-127">Il valore corrente può essere impostato anche se la proprietà espone un setter.</span><span class="sxs-lookup"><span data-stu-id="4ded0-127">The current value can also be set if the property exposes a setter.</span></span>  

<span data-ttu-id="4ded0-128">La lettura dei valori delle proprietà di cui non è stato eseguito il mapping risulta utile quando si esegue Entity Framework la convalida delle proprietà senza mapping.</span><span class="sxs-lookup"><span data-stu-id="4ded0-128">Reading the values of unmapped properties is useful when performing Entity Framework validation of unmapped properties.</span></span> <span data-ttu-id="4ded0-129">Per lo stesso motivo è possibile leggere e impostare i valori correnti per le proprietà delle entità che non sono attualmente rilevate dal contesto.</span><span class="sxs-lookup"><span data-stu-id="4ded0-129">For the same reason current values can be read and set for properties of entities that are not currently being tracked by the context.</span></span> <span data-ttu-id="4ded0-130">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4ded0-130">For example:</span></span>  

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

<span data-ttu-id="4ded0-131">Si noti che i valori originali non sono disponibili per le proprietà di cui non è stato eseguito il mapping o per le proprietà di entità che non vengono rilevate dal contesto.</span><span class="sxs-lookup"><span data-stu-id="4ded0-131">Note that original values are not available for unmapped properties or for properties of entities that are not being tracked by the context.</span></span>  

## <a name="checking-whether-a-property-is-marked-as-modified"></a><span data-ttu-id="4ded0-132">Verifica per determinare se una proprietà è contrassegnata come modificata</span><span class="sxs-lookup"><span data-stu-id="4ded0-132">Checking whether a property is marked as modified</span></span>  

<span data-ttu-id="4ded0-133">Nell'esempio seguente viene illustrato come verificare se una singola proprietà è contrassegnata come modificata:</span><span class="sxs-lookup"><span data-stu-id="4ded0-133">The example below shows how to check whether or not an individual property is marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

<span data-ttu-id="4ded0-134">I valori delle proprietà modificate vengono inviati come aggiornamenti al database quando viene chiamato SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="4ded0-134">The values of modified properties are sent as updates to the database when SaveChanges is called.</span></span>  

##  <a name="marking-a-property-as-modified"></a><span data-ttu-id="4ded0-135">Contrassegno di una proprietà come modificata</span><span class="sxs-lookup"><span data-stu-id="4ded0-135">Marking a property as modified</span></span>  

<span data-ttu-id="4ded0-136">Nell'esempio seguente viene illustrato come forzare la contrassegnatura di una singola proprietà come modificata:</span><span class="sxs-lookup"><span data-stu-id="4ded0-136">The example below shows how to force an individual property to be marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

<span data-ttu-id="4ded0-137">Contrassegnando una proprietà come modificata, viene inviato un aggiornamento al database per la proprietà quando SaveChanges viene chiamato anche se il valore corrente della proprietà è uguale al valore originale.</span><span class="sxs-lookup"><span data-stu-id="4ded0-137">Marking a property as modified forces an update to be send to the database for the property when SaveChanges is called even if the current value of the property is the same as its original value.</span></span>  

<span data-ttu-id="4ded0-138">Non è attualmente possibile reimpostare una singola proprietà in modo che non venga modificata dopo che è stata contrassegnata come modificata.</span><span class="sxs-lookup"><span data-stu-id="4ded0-138">It is not currently possible to reset an individual property to be not modified after it has been marked as modified.</span></span> <span data-ttu-id="4ded0-139">Si tratta di un elemento che si prevede di supportare in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="4ded0-139">This is something we plan to support in a future release.</span></span>  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a><span data-ttu-id="4ded0-140">Lettura dei valori correnti, originali e del database per tutte le proprietà di un'entità</span><span class="sxs-lookup"><span data-stu-id="4ded0-140">Reading current, original, and database values for all properties of an entity</span></span>  

<span data-ttu-id="4ded0-141">Nell'esempio seguente viene illustrato come leggere i valori correnti, i valori originali e i valori effettivamente presenti nel database per tutte le proprietà di cui è stato eseguito il mapping di un'entità.</span><span class="sxs-lookup"><span data-stu-id="4ded0-141">The example below shows how to read the current values, the original values, and the values actually in the database for all mapped properties of an entity.</span></span>  

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

<span data-ttu-id="4ded0-142">I valori correnti sono i valori attualmente contenuti nelle proprietà dell'entità.</span><span class="sxs-lookup"><span data-stu-id="4ded0-142">The current values are the values that the properties of the entity currently contain.</span></span> <span data-ttu-id="4ded0-143">I valori originali sono i valori letti dal database durante la query dell'entità.</span><span class="sxs-lookup"><span data-stu-id="4ded0-143">The original values are the values that were read from the database when the entity was queried.</span></span> <span data-ttu-id="4ded0-144">I valori del database sono i valori attualmente archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="4ded0-144">The database values are the values as they are currently stored in the database.</span></span> <span data-ttu-id="4ded0-145">Il recupero dei valori del database è utile quando è possibile che i valori nel database siano stati modificati dopo l'esecuzione di query sull'entità, ad esempio quando è stata eseguita una modifica simultanea del database da un altro utente.</span><span class="sxs-lookup"><span data-stu-id="4ded0-145">Getting the database values is useful when the values in the database may have changed since the entity was queried such as when a concurrent edit to the database has been made by another user.</span></span>  

## <a name="setting-current-or-original-values-from-another-object"></a><span data-ttu-id="4ded0-146">Impostazione dei valori correnti o originali da un altro oggetto</span><span class="sxs-lookup"><span data-stu-id="4ded0-146">Setting current or original values from another object</span></span>  

<span data-ttu-id="4ded0-147">I valori correnti o originali di un'entità rilevata possono essere aggiornati copiando i valori da un altro oggetto.</span><span class="sxs-lookup"><span data-stu-id="4ded0-147">The current or original values of a tracked entity can be updated by copying values from another object.</span></span> <span data-ttu-id="4ded0-148">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4ded0-148">For example:</span></span>  

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

<span data-ttu-id="4ded0-149">L'esecuzione del codice precedente comporterà la stampa:</span><span class="sxs-lookup"><span data-stu-id="4ded0-149">Running the code above will print out:</span></span>  

```console
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

<span data-ttu-id="4ded0-150">Questa tecnica viene talvolta utilizzata quando si aggiorna un'entità con valori ottenuti da una chiamata al servizio o da un client in un'applicazione a più livelli.</span><span class="sxs-lookup"><span data-stu-id="4ded0-150">This technique is sometimes used when updating an entity with values obtained from a service call or a client in an n-tier application.</span></span> <span data-ttu-id="4ded0-151">Si noti che l'oggetto utilizzato non deve essere dello stesso tipo dell'entità purché disponga di proprietà i cui nomi corrispondono a quelli dell'entità.</span><span class="sxs-lookup"><span data-stu-id="4ded0-151">Note that the object used does not have to be of the same type as the entity so long as it has properties whose names match those of the entity.</span></span> <span data-ttu-id="4ded0-152">Nell'esempio precedente viene usata un'istanza di BlogDTO per aggiornare i valori originali.</span><span class="sxs-lookup"><span data-stu-id="4ded0-152">In the example above, an instance of BlogDTO is used to update the original values.</span></span>  

<span data-ttu-id="4ded0-153">Si noti che solo le proprietà impostate su valori diversi quando vengono copiati dall'altro oggetto verranno contrassegnate come modificate.</span><span class="sxs-lookup"><span data-stu-id="4ded0-153">Note that only properties that are set to different values when copied from the other object will be marked as modified.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary"></a><span data-ttu-id="4ded0-154">Impostazione dei valori correnti o originali da un dizionario</span><span class="sxs-lookup"><span data-stu-id="4ded0-154">Setting current or original values from a dictionary</span></span>  

<span data-ttu-id="4ded0-155">I valori correnti o originali di un'entità rilevata possono essere aggiornati copiando i valori da un dizionario o da un'altra struttura di dati.</span><span class="sxs-lookup"><span data-stu-id="4ded0-155">The current or original values of a tracked entity can be updated by copying values from a dictionary or some other data structure.</span></span> <span data-ttu-id="4ded0-156">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4ded0-156">For example:</span></span>  

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

<span data-ttu-id="4ded0-157">Usare la proprietà OriginalValues invece della proprietà CurrentValues per impostare i valori originali.</span><span class="sxs-lookup"><span data-stu-id="4ded0-157">Use the OriginalValues property instead of the CurrentValues property to set original values.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a><span data-ttu-id="4ded0-158">Impostazione dei valori correnti o originali da un dizionario usando la proprietà</span><span class="sxs-lookup"><span data-stu-id="4ded0-158">Setting current or original values from a dictionary using Property</span></span>  

<span data-ttu-id="4ded0-159">Un'alternativa all'uso di CurrentValues o OriginalValues, come illustrato in precedenza, consiste nell'usare il metodo Property per impostare il valore di ogni proprietà.</span><span class="sxs-lookup"><span data-stu-id="4ded0-159">An alternative to using CurrentValues or OriginalValues as shown above is to use the Property method to set the value of each property.</span></span> <span data-ttu-id="4ded0-160">Questa operazione può essere preferibile quando è necessario impostare i valori delle proprietà complesse.</span><span class="sxs-lookup"><span data-stu-id="4ded0-160">This can be preferable when you need to set the values of complex properties.</span></span> <span data-ttu-id="4ded0-161">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4ded0-161">For example:</span></span>  

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

<span data-ttu-id="4ded0-162">Nell'esempio precedente viene eseguito l'accesso alle proprietà complesse utilizzando nomi punteggiati.</span><span class="sxs-lookup"><span data-stu-id="4ded0-162">In the example above complex properties are accessed using dotted names.</span></span> <span data-ttu-id="4ded0-163">Per altre modalità di accesso alle proprietà complesse, vedere le due sezioni più avanti in questo argomento, in particolare sulle proprietà complesse.</span><span class="sxs-lookup"><span data-stu-id="4ded0-163">For other ways to access complex properties see the two sections later in this topic specifically about complex properties.</span></span>  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a><span data-ttu-id="4ded0-164">Creazione di un oggetto clonato contenente i valori correnti, originali o di database</span><span class="sxs-lookup"><span data-stu-id="4ded0-164">Creating a cloned object containing current, original, or database values</span></span>  

<span data-ttu-id="4ded0-165">Per creare un clone dell'entità, è possibile usare l'oggetto elementi dbpropertyvalues restituito da CurrentValues, OriginalValues o GetDatabaseValues.</span><span class="sxs-lookup"><span data-stu-id="4ded0-165">The DbPropertyValues object returned from CurrentValues, OriginalValues, or GetDatabaseValues can be used to create a clone of the entity.</span></span> <span data-ttu-id="4ded0-166">Questo clone conterrà i valori delle proprietà dall'oggetto elementi dbpropertyvalues usato per crearlo.</span><span class="sxs-lookup"><span data-stu-id="4ded0-166">This clone will contain the property values from the DbPropertyValues object used to create it.</span></span> <span data-ttu-id="4ded0-167">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4ded0-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

<span data-ttu-id="4ded0-168">Si noti che l'oggetto restituito non è l'entità e non viene rilevata dal contesto.</span><span class="sxs-lookup"><span data-stu-id="4ded0-168">Note that the object returned is not the entity and is not being tracked by the context.</span></span> <span data-ttu-id="4ded0-169">Anche l'oggetto restituito non dispone di relazioni impostate su altri oggetti.</span><span class="sxs-lookup"><span data-stu-id="4ded0-169">The returned object also does not have any relationships set to other objects.</span></span>  

<span data-ttu-id="4ded0-170">L'oggetto clonato può essere utile per risolvere i problemi relativi agli aggiornamenti simultanei al database, soprattutto in caso di utilizzo di un'interfaccia utente che include data binding a oggetti di un determinato tipo.</span><span class="sxs-lookup"><span data-stu-id="4ded0-170">The cloned object can be useful for resolving issues related to concurrent updates to the database, especially where a UI that involves data binding to objects of a certain type is being used.</span></span>  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a><span data-ttu-id="4ded0-171">Recupero e impostazione dei valori correnti o originali delle proprietà complesse</span><span class="sxs-lookup"><span data-stu-id="4ded0-171">Getting and setting the current or original values of complex properties</span></span>  

<span data-ttu-id="4ded0-172">Il valore di un intero oggetto complesso può essere letto e impostato usando il metodo Property, così come può essere per una proprietà primitiva.</span><span class="sxs-lookup"><span data-stu-id="4ded0-172">The value of an entire complex object can be read and set using the Property method just as it can be for a primitive property.</span></span> <span data-ttu-id="4ded0-173">Inoltre, è possibile eseguire il drill-down nell'oggetto complesso e leggere o impostare le proprietà di tale oggetto o persino un oggetto annidato.</span><span class="sxs-lookup"><span data-stu-id="4ded0-173">In addition you can drill down into the complex object and read or set properties of that object, or even a nested object.</span></span> <span data-ttu-id="4ded0-174">Di seguito sono riportati alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="4ded0-174">Here are some examples:</span></span>  

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

<span data-ttu-id="4ded0-175">Usare la proprietà OriginalValue invece della proprietà CurrentValue per ottenere o impostare un valore originale.</span><span class="sxs-lookup"><span data-stu-id="4ded0-175">Use the OriginalValue property instead of the CurrentValue property to get or set an original value.</span></span>  

<span data-ttu-id="4ded0-176">Si noti che è possibile usare la proprietà o il Metodo ComplexProperty per accedere a una proprietà complessa.</span><span class="sxs-lookup"><span data-stu-id="4ded0-176">Note that either the Property or the ComplexProperty method can be used to access a complex property.</span></span> <span data-ttu-id="4ded0-177">Tuttavia, il Metodo ComplexProperty deve essere usato se si vuole eseguire il drill-down nell'oggetto complesso con chiamate di proprietà o ComplexProperty aggiuntive.</span><span class="sxs-lookup"><span data-stu-id="4ded0-177">However, the ComplexProperty method must be used if you wish to drill down into the complex object with additional Property or ComplexProperty calls.</span></span>  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a><span data-ttu-id="4ded0-178">Uso di elementi dbpropertyvalues per accedere a proprietà complesse</span><span class="sxs-lookup"><span data-stu-id="4ded0-178">Using DbPropertyValues to access complex properties</span></span>  

<span data-ttu-id="4ded0-179">Quando si usa CurrentValues, OriginalValues o GetDatabaseValues per ottenere tutti i valori correnti, originali o di database per un'entità, i valori di tutte le proprietà complesse vengono restituiti come oggetti elementi dbpropertyvalues annidati.</span><span class="sxs-lookup"><span data-stu-id="4ded0-179">When you use CurrentValues, OriginalValues, or GetDatabaseValues to get all the current, original, or database values for an entity, the values of any complex properties are returned as nested DbPropertyValues objects.</span></span> <span data-ttu-id="4ded0-180">Questi oggetti annidati possono quindi essere utilizzati per ottenere i valori dell'oggetto complesso.</span><span class="sxs-lookup"><span data-stu-id="4ded0-180">These nested objects can then be used to get values of the complex object.</span></span> <span data-ttu-id="4ded0-181">Ad esempio, il metodo seguente consente di stampare i valori di tutte le proprietà, inclusi i valori di tutte le proprietà complesse e le proprietà complesse annidate.</span><span class="sxs-lookup"><span data-stu-id="4ded0-181">For example, the following method will print out the values of all properties, including values of any complex properties and nested complex properties.</span></span>  

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

<span data-ttu-id="4ded0-182">Per stampare tutti i valori di proprietà correnti, il metodo verrebbe chiamato come segue:</span><span class="sxs-lookup"><span data-stu-id="4ded0-182">To print out all current property values the method would be called like this:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
