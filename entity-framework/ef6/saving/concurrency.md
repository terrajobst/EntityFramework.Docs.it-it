---
title: Gestione dei conflitti di concorrenza - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
ms.openlocfilehash: 81ae186201fdfac331b1d4e7836b222545fe78b5
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489154"
---
# <a name="handling-concurrency-conflicts"></a>Gestione dei conflitti di concorrenza
Ottimistica concorrenza implica ottimisticamente tenta di salvare l'entità al database nella speranza che i dati non è stata modificata l'entità è stata caricata. Se si scopre che i dati sono stati modificati, viene generata un'eccezione e il conflitto è necessario risolvere prima di provare a salvare di nuovo. Questo argomento illustra come gestire tali eccezioni in Entity Framework. Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.  

Questo post non è nella posizione appropriata per una spiegazione completa della concorrenza ottimistica. Le sezioni seguenti si presuppongono una conoscenza di risoluzione della concorrenza e visualizzazione dei modelli per attività comuni.  

Molti di questi modelli fanno uso degli argomenti illustrati in [utilizzano i valori di proprietà](~/ef6/saving/change-tracking/property-values.md).  

La risoluzione dei problemi di concorrenza quando si utilizzano associazioni indipendenti (in cui la chiave esterna non è mappata a una proprietà nell'entità) è molto più difficile che quando si utilizzano associazioni di chiavi esterne. Pertanto se si intende eseguire la risoluzione di concorrenza nell'applicazione è consigliabile che sempre eseguire il mapping di chiavi esterne nelle entità. Tutti gli esempi seguenti presuppongono che si siano utilizzando associazioni di chiavi esterne.  

Un DbUpdateConcurrencyException generata dal metodo SaveChanges quando viene rilevata un'eccezione di concorrenza ottimistica durante il tentativo di salvare un'entità che utilizza associazioni di chiavi esterne.  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a>Risoluzione delle eccezioni di concorrenza ottimistica con Ricarica database wins)  

Il metodo di ricaricamento è utilizzabile per sovrascrivere i valori correnti delle entità con i valori ora nel database. L'entità viene quindi in genere inviato nuovamente all'utente in un modulo e deve provare a effettuare nuovamente le modifiche e salvare di nuovo. Ad esempio:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;

        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update the values of the entity that failed to save from the store
            ex.Entries.Single().Reload();
        }

    } while (saveFailed);
}
```  

Un ottimo modo per simulare un'eccezione di concorrenza è impostare un punto di interruzione nella chiamata a SaveChanges e quindi modificare un'entità a cui viene salvata nel database di utilizzando un altro strumento come SQL Management Studio. È anche possibile inserire una riga prima SaveChanges per aggiornare il database usando direttamente SqlCommand. Ad esempio:  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

Il metodo di voci in DbUpdateConcurrencyException restituisce le istanze di DbEntityEntry per le entità che non è stato possibile aggiornare. (Questa proprietà attualmente restituisce sempre un solo valore per i problemi di concorrenza. Può restituire più valori per le eccezioni generali di aggiornamento.) Un'alternativa per alcune situazioni potrebbe essere di ottenere le voci per tutte le entità che potrebbero dover essere ricaricata dal database e ricaricare la chiamata per ciascuno di essi.  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a>Risoluzione delle eccezioni di concorrenza ottimistica come prevale il client  

L'esempio precedente che usa Ricarica viene talvolta chiamato wins database o store wins perché i valori dell'entità vengono sovrascritti dai valori dal database. A volte si desidera effettuare l'operazione inversa e sovrascrivere i valori nel database con i valori attualmente nell'entità. Ciò è talvolta chiamato prevale il client e può essere eseguita ottenendo i valori del database corrente e impostarli come valori originali per l'entità. (Vedere [utilizzano i valori di proprietà](~/ef6/saving/change-tracking/property-values.md) per informazioni sui valori correnti e originali.) Ad esempio:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update original values from the database
            var entry = ex.Entries.Single();
            entry.OriginalValues.SetValues(entry.GetDatabaseValues());
        }

    } while (saveFailed);
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a>Risoluzione personalizzata delle eccezioni di concorrenza ottimistica  

In alcuni casi è possibile combinare i valori presenti nel database con i valori attualmente nell'entità. Ciò richiede in genere l'interazione dell'utente o per la logica personalizzata. Ad esempio, si potrà presentare un form all'utente che contiene i valori correnti, i valori nel database, e imposta un valore predefinito di valori risolti. L'utente potrebbe quindi modificare i valori risolti in base alle esigenze e sarebbe questi valori risolti che vengono salvati nel database. Questa operazione può essere eseguita utilizzando gli oggetti DbPropertyValues restituiti da CurrentValues e GetDatabaseValues sulla voce dell'entità. Ad esempio:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            var entry = ex.Entries.Single();
            var currentValues = entry.CurrentValues;
            var databaseValues = entry.GetDatabaseValues();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValues = databaseValues.Clone();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency(currentValues, databaseValues, resolvedValues);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValues);
        }
    } while (saveFailed);
}

public void HaveUserResolveConcurrency(DbPropertyValues currentValues,
                                       DbPropertyValues databaseValues,
                                       DbPropertyValues resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them edit the resolved values to get the correct resolution.
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a>Risoluzione personalizzata delle eccezioni di concorrenza ottimistica tramite oggetti  

Il codice precedente Usa istanze DbPropertyValues per passaggio di corrente, il database e valori risolti. In alcuni casi potrebbe essere più facile da usare istanze del tipo di entità per questo oggetto. Questa operazione può essere eseguita usando i metodi ToObject e SetValues di DbPropertyValues. Ad esempio:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            // as instances of the entity type
            var entry = ex.Entries.Single();
            var databaseValues = entry.GetDatabaseValues();
            var databaseValuesAsBlog = (Blog)databaseValues.ToObject();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValuesAsBlog = (Blog)databaseValues.ToObject();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency((Blog)entry.Entity,
                                       databaseValuesAsBlog,
                                       resolvedValuesAsBlog);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValuesAsBlog);
        }

    } while (saveFailed);
}

public void HaveUserResolveConcurrency(Blog entity,
                                       Blog databaseValues,
                                       Blog resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them update the resolved values to get the correct resolution.
}
```  
