---
title: Gestione dei conflitti di concorrenza - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
ms.openlocfilehash: f233af217287dd6bf35e5b7fea8e44974168b312
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997810"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="b1969-102">Gestione dei conflitti di concorrenza</span><span class="sxs-lookup"><span data-stu-id="b1969-102">Handling Concurrency Conflicts</span></span>
<span data-ttu-id="b1969-103">Ottimistica concorrenza implica ottimisticamente tenta di salvare l'entità al database nella speranza che i dati non è stata modificata l'entità è stata caricata.</span><span class="sxs-lookup"><span data-stu-id="b1969-103">Optimistic concurrency involves optimistically attempting to save your entity to the database in the hope that the data there has not changed since the entity was loaded.</span></span> <span data-ttu-id="b1969-104">Se si scopre che i dati sono stati modificati, viene generata un'eccezione e il conflitto è necessario risolvere prima di provare a salvare di nuovo.</span><span class="sxs-lookup"><span data-stu-id="b1969-104">If it turns out that the data has changed then an exception is thrown and you must resolve the conflict before attempting to save again.</span></span> <span data-ttu-id="b1969-105">Questo argomento illustra come gestire tali eccezioni in Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b1969-105">This topic covers how to handle such exceptions in Entity Framework.</span></span> <span data-ttu-id="b1969-106">Le tecniche illustrate in questo argomento si applicano in modo analogo ai modelli creati con Code First ed EF Designer.</span><span class="sxs-lookup"><span data-stu-id="b1969-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="b1969-107">Questo post non è nella posizione appropriata per una spiegazione completa della concorrenza ottimistica.</span><span class="sxs-lookup"><span data-stu-id="b1969-107">This post is not the appropriate place for a full discussion of optimistic concurrency.</span></span> <span data-ttu-id="b1969-108">Le sezioni seguenti si presuppongono una conoscenza di risoluzione della concorrenza e visualizzazione dei modelli per attività comuni.</span><span class="sxs-lookup"><span data-stu-id="b1969-108">The sections below assume some knowledge of concurrency resolution and show patterns for common tasks.</span></span>  

<span data-ttu-id="b1969-109">Molti di questi modelli fanno uso degli argomenti illustrati in [utilizzano i valori di proprietà](~/ef6/saving/change-tracking/property-values.md).</span><span class="sxs-lookup"><span data-stu-id="b1969-109">Many of these patterns make use of the topics discussed in [Working with Property Values](~/ef6/saving/change-tracking/property-values.md).</span></span>  

<span data-ttu-id="b1969-110">La risoluzione dei problemi di concorrenza quando si utilizzano associazioni indipendenti (in cui la chiave esterna non è mappata a una proprietà nell'entità) è molto più difficile che quando si utilizzano associazioni di chiavi esterne.</span><span class="sxs-lookup"><span data-stu-id="b1969-110">Resolving concurrency issues when you are using independent associations (where the foreign key is not mapped to a property in your entity) is much more difficult than when you are using foreign key associations.</span></span> <span data-ttu-id="b1969-111">Pertanto se si intende eseguire la risoluzione di concorrenza nell'applicazione è consigliabile che sempre eseguire il mapping di chiavi esterne nelle entità.</span><span class="sxs-lookup"><span data-stu-id="b1969-111">Therefore if you are going to do concurrency resolution in your application it is advised that you always map foreign keys into your entities.</span></span> <span data-ttu-id="b1969-112">Tutti gli esempi seguenti presuppongono che si siano utilizzando associazioni di chiavi esterne.</span><span class="sxs-lookup"><span data-stu-id="b1969-112">All the examples below assume that you are using foreign key associations.</span></span>  

<span data-ttu-id="b1969-113">Un DbUpdateConcurrencyException generata dal metodo SaveChanges quando viene rilevata un'eccezione di concorrenza ottimistica durante il tentativo di salvare un'entità che utilizza associazioni di chiavi esterne.</span><span class="sxs-lookup"><span data-stu-id="b1969-113">A DbUpdateConcurrencyException is thrown by SaveChanges when an optimistic concurrency exception is detected while attempting to save an entity that uses foreign key associations.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a><span data-ttu-id="b1969-114">Risoluzione delle eccezioni di concorrenza ottimistica con Ricarica database wins)</span><span class="sxs-lookup"><span data-stu-id="b1969-114">Resolving optimistic concurrency exceptions with Reload (database wins)</span></span>  

<span data-ttu-id="b1969-115">Il metodo di ricaricamento è utilizzabile per sovrascrivere i valori correnti delle entità con i valori ora nel database.</span><span class="sxs-lookup"><span data-stu-id="b1969-115">The Reload method can be used to overwrite the current values of the entity with the values now in the database.</span></span> <span data-ttu-id="b1969-116">L'entità viene quindi in genere inviato nuovamente all'utente in un modulo e deve provare a effettuare nuovamente le modifiche e salvare di nuovo.</span><span class="sxs-lookup"><span data-stu-id="b1969-116">The entity is then typically given back to the user in some form and they must try to make their changes again and re-save.</span></span> <span data-ttu-id="b1969-117">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b1969-117">For example:</span></span>  

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

<span data-ttu-id="b1969-118">Un ottimo modo per simulare un'eccezione di concorrenza è impostare un punto di interruzione nella chiamata a SaveChanges e quindi modificare un'entità a cui viene salvata nel database di utilizzando un altro strumento come SQL Management Studio.</span><span class="sxs-lookup"><span data-stu-id="b1969-118">A good way to simulate a concurrency exception is to set a breakpoint on the SaveChanges call and then modify an entity that is being saved in the database using another tool such as SQL Management Studio.</span></span> <span data-ttu-id="b1969-119">È anche possibile inserire una riga prima SaveChanges per aggiornare il database usando direttamente SqlCommand.</span><span class="sxs-lookup"><span data-stu-id="b1969-119">You could also insert a line before SaveChanges to update the database directly using SqlCommand.</span></span> <span data-ttu-id="b1969-120">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b1969-120">For example:</span></span>  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

<span data-ttu-id="b1969-121">Il metodo di voci in DbUpdateConcurrencyException restituisce le istanze di DbEntityEntry per le entità che non è stato possibile aggiornare.</span><span class="sxs-lookup"><span data-stu-id="b1969-121">The Entries method on DbUpdateConcurrencyException returns the DbEntityEntry instances for the entities that failed to update.</span></span> <span data-ttu-id="b1969-122">(Questa proprietà attualmente restituisce sempre un solo valore per i problemi di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="b1969-122">(This property currently always returns a single value for concurrency issues.</span></span> <span data-ttu-id="b1969-123">Può restituire più valori per le eccezioni generali di aggiornamento.) Un'alternativa per alcune situazioni potrebbe essere di ottenere le voci per tutte le entità che potrebbero dover essere ricaricata dal database e ricaricare la chiamata per ciascuno di essi.</span><span class="sxs-lookup"><span data-stu-id="b1969-123">It may return multiple values for general update exceptions.) An alternative for some situations might be to get entries for all entities that may need to be reloaded from the database and call reload for each of these.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a><span data-ttu-id="b1969-124">Risoluzione delle eccezioni di concorrenza ottimistica come prevale il client</span><span class="sxs-lookup"><span data-stu-id="b1969-124">Resolving optimistic concurrency exceptions as client wins</span></span>  

<span data-ttu-id="b1969-125">L'esempio precedente che usa Ricarica viene talvolta chiamato wins database o store wins perché i valori dell'entità vengono sovrascritti dai valori dal database.</span><span class="sxs-lookup"><span data-stu-id="b1969-125">The example above that uses Reload is sometimes called database wins or store wins because the values in the entity are overwritten by values from the database.</span></span> <span data-ttu-id="b1969-126">A volte si desidera effettuare l'operazione inversa e sovrascrivere i valori nel database con i valori attualmente nell'entità.</span><span class="sxs-lookup"><span data-stu-id="b1969-126">Sometimes you may wish to do the opposite and overwrite the values in the database with the values currently in the entity.</span></span> <span data-ttu-id="b1969-127">Ciò è talvolta chiamato prevale il client e può essere eseguita ottenendo i valori del database corrente e impostarli come valori originali per l'entità.</span><span class="sxs-lookup"><span data-stu-id="b1969-127">This is sometimes called client wins and can be done by getting the current database values and setting them as the original values for the entity.</span></span> <span data-ttu-id="b1969-128">(Vedere [utilizzano i valori di proprietà](~/ef6/saving/change-tracking/property-values.md) per informazioni sui valori correnti e originali.) Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b1969-128">(See [Working with Property Values](~/ef6/saving/change-tracking/property-values.md) for information on current and original values.) For example:</span></span>  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a><span data-ttu-id="b1969-129">Risoluzione personalizzata delle eccezioni di concorrenza ottimistica</span><span class="sxs-lookup"><span data-stu-id="b1969-129">Custom resolution of optimistic concurrency exceptions</span></span>  

<span data-ttu-id="b1969-130">In alcuni casi è possibile combinare i valori presenti nel database con i valori attualmente nell'entità.</span><span class="sxs-lookup"><span data-stu-id="b1969-130">Sometimes you may want to combine the values currently in the database with the values currently in the entity.</span></span> <span data-ttu-id="b1969-131">Ciò richiede in genere l'interazione dell'utente o per la logica personalizzata.</span><span class="sxs-lookup"><span data-stu-id="b1969-131">This usually requires some custom logic or user interaction.</span></span> <span data-ttu-id="b1969-132">Ad esempio, si potrà presentare un form all'utente che contiene i valori correnti, i valori nel database, e imposta un valore predefinito di valori risolti.</span><span class="sxs-lookup"><span data-stu-id="b1969-132">For example, you might present a form to the user containing the current values, the values in the database, and a default set of resolved values.</span></span> <span data-ttu-id="b1969-133">L'utente potrebbe quindi modificare i valori risolti in base alle esigenze e sarebbe questi valori risolti che vengono salvati nel database.</span><span class="sxs-lookup"><span data-stu-id="b1969-133">The user would then edit the resolved values as necessary and it would be these resolved values that get saved to the database.</span></span> <span data-ttu-id="b1969-134">Questa operazione può essere eseguita utilizzando gli oggetti DbPropertyValues restituiti da CurrentValues e GetDatabaseValues sulla voce dell'entità.</span><span class="sxs-lookup"><span data-stu-id="b1969-134">This can be done using the DbPropertyValues objects returned from CurrentValues and GetDatabaseValues on the entity’s entry.</span></span> <span data-ttu-id="b1969-135">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b1969-135">For example:</span></span>  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a><span data-ttu-id="b1969-136">Risoluzione personalizzata delle eccezioni di concorrenza ottimistica tramite oggetti</span><span class="sxs-lookup"><span data-stu-id="b1969-136">Custom resolution of optimistic concurrency exceptions using objects</span></span>  

<span data-ttu-id="b1969-137">Il codice precedente Usa istanze DbPropertyValues per passaggio di corrente, il database e valori risolti.</span><span class="sxs-lookup"><span data-stu-id="b1969-137">The code above uses DbPropertyValues instances for passing around current, database, and resolved values.</span></span> <span data-ttu-id="b1969-138">In alcuni casi potrebbe essere più facile da usare istanze del tipo di entità per questo oggetto.</span><span class="sxs-lookup"><span data-stu-id="b1969-138">Sometimes it may be easier to use instances of your entity type for this.</span></span> <span data-ttu-id="b1969-139">Questa operazione può essere eseguita usando i metodi ToObject e SetValues di DbPropertyValues.</span><span class="sxs-lookup"><span data-stu-id="b1969-139">This can be done using the ToObject and SetValues methods of DbPropertyValues.</span></span> <span data-ttu-id="b1969-140">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b1969-140">For example:</span></span>  

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
