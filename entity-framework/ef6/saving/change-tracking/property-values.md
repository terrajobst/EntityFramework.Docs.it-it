---
title: Utilizzo dei valori delle proprietà - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
ms.openlocfilehash: a9b969950ec7dcfb86a2abc9c8bd6cc24899948c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998303"
---
# <a name="working-with-property-values"></a>Utilizzo di valori di proprietà
Per la maggior parte Entity Framework si occuperà di stato, i valori originali e i valori correnti delle proprietà delle istanze di entità con rilevamento. Tuttavia, potrebbero esserci alcuni casi, ad esempio gli scenari disconnessi, in cui si desidera visualizzare o modificare le informazioni che EF ha le proprietà. Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.  

Entity Framework tiene traccia di due valori per ogni proprietà di un'entità rilevata. Il valore corrente è, come indica il nome, il valore corrente della proprietà nell'entità. Il valore originale è il valore che la proprietà conteneva quando l'entità è stata sottoposti a query dal database o connesso al contesto.  

Esistono due meccanismi generali per l'utilizzo con valori di proprietà:  

- Il valore di una singola proprietà può essere ottenuto in modo fortemente tipizzato utilizzando il metodo di proprietà.  
- I valori per tutte le proprietà di un'entità possono essere letti in un oggetto DbPropertyValues. DbPropertyValues funge quindi da un oggetto di tipo dizionario per i valori delle proprietà da leggere e impostare. I valori in un oggetto DbPropertyValues possono essere impostati dai valori in un altro oggetto DbPropertyValues o dai valori in un altro oggetto, ad esempio un'altra copia dell'entità o a un oggetto di trasferimento di dati semplici (DTO).  

Le sezioni seguenti mostrano esempi di utilizzo di entrambi i meccanismi sopra.  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a>Ottenere e impostare il valore originale o corrente di una singola proprietà  

L'esempio seguente mostra come il valore corrente di una proprietà può essere letti e quindi impostato su un nuovo valore:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(3);

    // Read the current value of the Name property
    string currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;

    // Set the Name property to a new value
    context.Entry(name).Property(u => u.Name).CurrentValue = "My Fancy Blog";

    // Read the current value of the Name property using a string for the property name
    object currentName2 = context.Entry(blog).Property("Name").CurrentValue;

    // Set the Name property to a new value using a string for the property name
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

Usare la proprietà OriginalValue anziché la proprietà CurrentValue leggerne o impostarne il valore originale.  

Si noti che il valore restituito è tipizzato come "object" quando una stringa viene utilizzata per specificare il nome della proprietà. D'altra parte, il valore restituito è fortemente tipizzato se viene usata un'espressione lambda.  

Impostare il valore della proprietà simile al seguente solo contrassegnerà la proprietà come modificata se il nuovo valore è diverso dal valore precedente.  

Quando un valore della proprietà è impostato in questo modo la modifica viene rilevata automaticamente anche se AutoDetectChanges è disattivato.  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a>Ottenere e impostare il valore corrente di una proprietà non mappata  

Il valore corrente di una proprietà che non è mappato al database può anche essere letti. Un esempio di una proprietà non mappata potrebbe essere una proprietà RssLink nel Blog. Questo valore può essere calcolato in base il BlogId e pertanto non desidera essere archiviate nel database. Ad esempio:  

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

Il valore corrente può essere impostato anche se la proprietà espone un setter.  

Leggendo i valori delle proprietà non mappata è utile quando si esegue la convalida di Entity Framework di proprietà non mappata. Per lo stesso motivo i valori correnti possono leggere e impostare per le proprietà di entità che non sono attualmente controllata dal contesto. Ad esempio:  

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

Si noti che i valori originali non sono disponibili per le proprietà non mappate o per le proprietà delle entità che non vengono rilevate dal contesto.  

## <a name="checking-whether-a-property-is-marked-as-modified"></a>Verifica se una proprietà è contrassegnata come modificata  

L'esempio seguente viene illustrato come controllare se una singola proprietà è contrassegnata come modificata:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

I valori delle proprietà modificate vengono inviati come gli aggiornamenti al database quando viene chiamato SaveChanges.  

##  <a name="marking-a-property-as-modified"></a>Contrassegno di una proprietà come modificata  

L'esempio seguente viene illustrato come forzare una singola proprietà da contrassegnare come modificato:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

Contrassegno di una proprietà come modificata forza un aggiornamento da inviare al database per la proprietà quando viene chiamato SaveChanges anche se il valore corrente della proprietà è uguale a quello originale.  

Non è attualmente possibile reimpostare una singola proprietà da modificare non dopo che è stato contrassegnato come modificato. Si tratta di qualcosa che si prevede di supportare in una versione futura.  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a>La lettura correnti, originali e i valori del database per tutte le proprietà di un'entità  

L'esempio seguente viene illustrato come leggere i valori correnti, i valori originali e i valori effettivamente nel database per tutte le proprietà con mapping di un'entità.  

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

I valori correnti sono i valori attualmente contenenti le proprietà dell'entità. I valori originali sono i valori letti dal database quando l'entità è stata eseguita una query. I valori del database sono i valori attualmente archiviati nel database. Ottenere i valori del database è utile quando i valori del database possono essere modificata perché l'entità è stata eseguita una query, ad esempio quando un simultanea modifica per il database è diventato da un altro utente.  

## <a name="setting-current-or-original-values-from-another-object"></a>Impostazione dei valori correnti o originali da un altro oggetto  

I valori correnti o originali di un'entità rilevata possono essere aggiornati tramite la copia di valori da un altro oggetto. Ad esempio:  

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

Esecuzione di codice precedente consente di stampare:  

```  
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

Questa tecnica viene usata in alcuni casi quando si aggiorna un'entità con valori ottenuti da una chiamata al servizio o un client in un'applicazione a più livelli. Si noti che l'oggetto usato non deve essere dello stesso tipo dell'entità, purché tale classe presenta proprietà i cui nomi corrispondono a quelle dell'entità. Nell'esempio precedente, un'istanza di BlogDTO consente di aggiornare i valori originali.  

Si noti che solo le proprietà impostate su valori diversi quando copiati da altro oggetto verranno contrassegnate come modificata.  

## <a name="setting-current-or-original-values-from-a-dictionary"></a>Impostazione dei valori correnti o originali da un dizionario  

I valori correnti o originali di un'entità rilevata possono essere aggiornati tramite la copia di valori da un dizionario o altre strutture di dati. Ad esempio:  

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

Usare la proprietà OriginalValues anziché la proprietà CurrentValues per impostare i valori originali.  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a>Impostazione dei valori correnti o originali da un dizionario con proprietà  

Un'alternativa all'utilizzo CurrentValues o OriginalValues come illustrato in precedenza consiste nell'usare il metodo di proprietà per impostare il valore di ogni proprietà. Ciò può essere preferibile quando è necessario impostare i valori di proprietà complesse. Ad esempio:  

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

Nell'esempio precedente le proprietà complesse sono accessibili tramite nomi punteggiati. Per altri modi per accedere alle proprietà complesse vedere due sezioni più avanti in questo argomento in particolare sulle proprietà complesse.  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a>Creazione di un oggetto clonato contenente i valori del database, originale o corrente  

L'oggetto DbPropertyValues restituito dal CurrentValues, OriginalValues, o GetDatabaseValues può essere utilizzato per creare un clone dell'entità. Questo clone conterrà i valori della proprietà dall'oggetto DbPropertyValues usata per crearlo. Ad esempio:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

Si noti che l'oggetto restituito non sono l'entità e non viene rilevato dal contesto. L'oggetto restituito inoltre non ha alcuna relazione impostati su altri oggetti.  

Oggetto clonato può essere utile per la risoluzione dei problemi relativi agli aggiornamenti simultanei al database, in particolare in cui è in uso un'interfaccia utente che coinvolge data binding a oggetti di un determinato tipo.  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a>Ottenere e impostare i valori correnti o originali delle proprietà complessa  

Il valore di un intero oggetto complesso può essere letto e impostato utilizzando il metodo di proprietà, come è possibile per una proprietà primitiva. Inoltre è possibile risalire alle oggetto complesso e lettura o impostare le proprietà di quell'oggetto, o anche un oggetto nidificato. Ecco alcuni esempi:  

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

Usare la proprietà OriginalValue anziché la proprietà CurrentValue da ottenere o impostare un valore originale.  

Si noti che la proprietà o metodo ComplexProperty utilizzabile per accedere a una proprietà complessa. Tuttavia, il metodo ComplexProperty deve essere usato se si vuole eseguire il drill-nell'oggetto complesso con proprietà aggiuntive o ComplexProperty chiamate.  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a>Uso di DbPropertyValues per accedere alle proprietà complesse  

Quando si usa CurrentValues, OriginalValues o GetDatabaseValues per ottenere l'oggetto corrente, originale, o i valori del database per un'entità, i valori di qualsiasi proprietà complesse vengono restituiti come oggetti DbPropertyValues annidati. Questi annidati gli oggetti possono quindi essere usati per ottenere i valori dell'oggetto complesso. Ad esempio, il metodo seguente verrà stampato i valori di tutte le proprietà, inclusi i valori di qualsiasi proprietà complesse e proprietà complesse nidificate.  

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

Per stampare una copia tutti i valori correnti delle proprietà del metodo viene chiamato come segue:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
