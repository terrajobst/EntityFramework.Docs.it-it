---
title: Novità di EF Core 5,0
author: ajcvickers
ms.date: 03/15/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 08a93555fd76d8a9f6d3011f59d9a34f76d0b22f
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136248"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="09d79-102">Novità di EF Core 5,0</span><span class="sxs-lookup"><span data-stu-id="09d79-102">What's New in EF Core 5.0</span></span>

<span data-ttu-id="09d79-103">EF Core 5,0 è attualmente in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="09d79-103">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="09d79-104">Questa pagina conterrà una panoramica delle modifiche interessanti introdotte in ogni anteprima.</span><span class="sxs-lookup"><span data-stu-id="09d79-104">This page will contain an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="09d79-105">Questa pagina non duplica il [piano per EF Core 5,0](plan.md).</span><span class="sxs-lookup"><span data-stu-id="09d79-105">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="09d79-106">Il piano descrive i temi generali per EF Core 5,0, inclusi tutti gli elementi che si prevede di includere prima di distribuire la versione finale.</span><span class="sxs-lookup"><span data-stu-id="09d79-106">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="09d79-107">I collegamenti da qui vengono aggiunti alla documentazione ufficiale appena pubblicata.</span><span class="sxs-lookup"><span data-stu-id="09d79-107">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-1"></a><span data-ttu-id="09d79-108">Preview 1</span><span class="sxs-lookup"><span data-stu-id="09d79-108">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="09d79-109">Registrazione semplice</span><span class="sxs-lookup"><span data-stu-id="09d79-109">Simple logging</span></span>

<span data-ttu-id="09d79-110">Questa funzionalità aggiunge funzionalità simili a `Database.Log` in EF6.</span><span class="sxs-lookup"><span data-stu-id="09d79-110">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="09d79-111">Ovvero, fornisce un modo semplice per ottenere i log da EF Core senza la necessità di configurare alcun tipo di Framework di registrazione esterno.</span><span class="sxs-lookup"><span data-stu-id="09d79-111">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="09d79-112">La documentazione preliminare è inclusa nello [stato settimanale EF del 5 dicembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="09d79-112">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="09d79-113">La documentazione aggiuntiva viene rilevata in base al problema [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span><span class="sxs-lookup"><span data-stu-id="09d79-113">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="09d79-114">Modo semplice per ottenere SQL generato</span><span class="sxs-lookup"><span data-stu-id="09d79-114">Simple way to get generated SQL</span></span>

<span data-ttu-id="09d79-115">EF Core 5,0 introduce il metodo di estensione `ToQueryString` che restituirà il SQL che EF Core genererà durante l'esecuzione di una query LINQ.</span><span class="sxs-lookup"><span data-stu-id="09d79-115">EF Core 5.0 introduces the `ToQueryString` extension method which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="09d79-116">La documentazione preliminare è inclusa nello [stato settimanale EF per il 9 gennaio 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span><span class="sxs-lookup"><span data-stu-id="09d79-116">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="09d79-117">La documentazione aggiuntiva viene rilevata in base al problema [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span><span class="sxs-lookup"><span data-stu-id="09d79-117">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="09d79-118">Usare un C# attributo per indicare che un'entità non ha una chiave</span><span class="sxs-lookup"><span data-stu-id="09d79-118">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="09d79-119">È ora possibile configurare un tipo di entità senza chiavi usando il nuovo `KeylessAttribute`.</span><span class="sxs-lookup"><span data-stu-id="09d79-119">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span>
<span data-ttu-id="09d79-120">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="09d79-120">For example:</span></span>

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="09d79-121">La documentazione viene rilevata in base al problema [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span><span class="sxs-lookup"><span data-stu-id="09d79-121">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="09d79-122">La stringa di connessione o di connessione può essere modificata in DbContext inizializzato</span><span class="sxs-lookup"><span data-stu-id="09d79-122">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="09d79-123">È ora più semplice creare un'istanza di DbContext senza connessione o stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="09d79-123">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="09d79-124">Inoltre, la connessione o la stringa di connessione possono ora essere mutate sull'istanza del contesto.</span><span class="sxs-lookup"><span data-stu-id="09d79-124">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="09d79-125">Ciò consente alla stessa istanza del contesto di connettersi dinamicamente a database diversi.</span><span class="sxs-lookup"><span data-stu-id="09d79-125">This allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="09d79-126">La documentazione viene rilevata in base al problema [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span><span class="sxs-lookup"><span data-stu-id="09d79-126">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="09d79-127">Proxy di rilevamento delle modifiche</span><span class="sxs-lookup"><span data-stu-id="09d79-127">Change-tracking proxies</span></span>

<span data-ttu-id="09d79-128">EF Core ora possono generare proxy di runtime che implementano automaticamente [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) e [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="09d79-128">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="09d79-129">Queste segnalano quindi le modifiche ai valori delle proprietà dell'entità direttamente a EF Core, evitando la necessità di analizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="09d79-129">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="09d79-130">Tuttavia, i proxy presentano un set di limitazioni, quindi non sono destinati a tutti.</span><span class="sxs-lookup"><span data-stu-id="09d79-130">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="09d79-131">La documentazione viene rilevata in base al problema [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span><span class="sxs-lookup"><span data-stu-id="09d79-131">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="09d79-132">Viste di debug migliorate</span><span class="sxs-lookup"><span data-stu-id="09d79-132">Enhanced debug views</span></span>

<span data-ttu-id="09d79-133">Le visualizzazioni di debug rappresentano un modo semplice per esaminare gli elementi interni di EF Core durante il debug dei problemi.</span><span class="sxs-lookup"><span data-stu-id="09d79-133">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="09d79-134">Una visualizzazione di debug per il modello è stata implementata qualche tempo fa.</span><span class="sxs-lookup"><span data-stu-id="09d79-134">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="09d79-135">Per EF Core 5,0, la visualizzazione modello è stata semplificata per la lettura e l'aggiunta di una nuova visualizzazione debug per le entità rilevate nel gestore di stato.</span><span class="sxs-lookup"><span data-stu-id="09d79-135">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="09d79-136">La documentazione preliminare è inclusa nello [stato settimanale EF per il 12 dicembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="09d79-136">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="09d79-137">La documentazione aggiuntiva viene rilevata in base al problema [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span><span class="sxs-lookup"><span data-stu-id="09d79-137">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="09d79-138">Gestione migliorata della semantica null del database</span><span class="sxs-lookup"><span data-stu-id="09d79-138">Improved handling of database null semantics</span></span>

<span data-ttu-id="09d79-139">I database relazionali considerano in genere NULL come valore sconosciuto e pertanto non uguale a qualsiasi altro valore NULL.</span><span class="sxs-lookup"><span data-stu-id="09d79-139">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="09d79-140">C#d'altra parte, considera null come un valore definito che confronta uguale a qualsiasi altro valore null.</span><span class="sxs-lookup"><span data-stu-id="09d79-140">C#, on the other hand, treats null as a defined value which compares equal to any other null.</span></span>
<span data-ttu-id="09d79-141">EF Core per impostazione predefinita converte le query in modo che utilizzino C# la semantica null.</span><span class="sxs-lookup"><span data-stu-id="09d79-141">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="09d79-142">EF Core 5,0 migliora notevolmente l'efficienza di queste traduzioni.</span><span class="sxs-lookup"><span data-stu-id="09d79-142">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="09d79-143">La documentazione viene rilevata in base al problema [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span><span class="sxs-lookup"><span data-stu-id="09d79-143">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="09d79-144">Proprietà indicizzatore</span><span class="sxs-lookup"><span data-stu-id="09d79-144">Indexer properties</span></span>

<span data-ttu-id="09d79-145">EF Core 5,0 supporta il mapping C# delle proprietà dell'indicizzatore.</span><span class="sxs-lookup"><span data-stu-id="09d79-145">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="09d79-146">In questo modo le entità possono fungere da contenitori di proprietà in cui viene eseguito il mapping delle colonne alle proprietà denominate nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="09d79-146">This allows entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="09d79-147">La documentazione viene rilevata in base al problema [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span><span class="sxs-lookup"><span data-stu-id="09d79-147">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="09d79-148">Generazione di vincoli check per i mapping enum</span><span class="sxs-lookup"><span data-stu-id="09d79-148">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="09d79-149">EF Core 5,0 le migrazioni possono ora generare vincoli CHECK per i mapping delle proprietà di enumerazione.</span><span class="sxs-lookup"><span data-stu-id="09d79-149">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="09d79-150">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="09d79-150">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="09d79-151">La documentazione viene rilevata in base al problema [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span><span class="sxs-lookup"><span data-stu-id="09d79-151">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="09d79-152">Relazionale</span><span class="sxs-lookup"><span data-stu-id="09d79-152">IsRelational</span></span>

<span data-ttu-id="09d79-153">È stato aggiunto un nuovo metodo di `IsRelational`, oltre ai `IsSqlServer`, `IsSqlite`e `IsInMemory`esistenti.</span><span class="sxs-lookup"><span data-stu-id="09d79-153">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span>
<span data-ttu-id="09d79-154">Questa operazione può essere utilizzata per verificare se DbContext utilizza un provider di database relazionale.</span><span class="sxs-lookup"><span data-stu-id="09d79-154">This can be used to test if the DbContext is using any relational database provider.</span></span>
<span data-ttu-id="09d79-155">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="09d79-155">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="09d79-156">La documentazione viene rilevata in base al problema [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span><span class="sxs-lookup"><span data-stu-id="09d79-156">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="09d79-157">Concorrenza ottimistica di Cosmos con ETag</span><span class="sxs-lookup"><span data-stu-id="09d79-157">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="09d79-158">Il provider di database Azure Cosmos DB supporta ora la concorrenza ottimistica tramite ETag.</span><span class="sxs-lookup"><span data-stu-id="09d79-158">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span>
<span data-ttu-id="09d79-159">Usare il generatore di modelli in OnModelCreating per configurare un ETag:</span><span class="sxs-lookup"><span data-stu-id="09d79-159">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="09d79-160">SaveChanges genera quindi un `DbUpdateConcurrencyException` in un conflitto di concorrenza, che [può essere gestito](https://docs.microsoft.com/ef/core/saving/concurrency) per implementare i tentativi e così via.</span><span class="sxs-lookup"><span data-stu-id="09d79-160">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](https://docs.microsoft.com/ef/core/saving/concurrency) to implement retries, etc.</span></span>


<span data-ttu-id="09d79-161">La documentazione viene rilevata in base al problema [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span><span class="sxs-lookup"><span data-stu-id="09d79-161">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="09d79-162">Eseguire query sulle traduzioni per più costrutti DateTime</span><span class="sxs-lookup"><span data-stu-id="09d79-162">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="09d79-163">Vengono ora convertite le query contenenti nuove costruzioni DateTime.</span><span class="sxs-lookup"><span data-stu-id="09d79-163">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="09d79-164">Inoltre, vengono ora mappate le funzioni di SQL Server seguenti:</span><span class="sxs-lookup"><span data-stu-id="09d79-164">In addition, the following SQL Server functions are now mapped:</span></span>
* <span data-ttu-id="09d79-165">DateDiffWeek</span><span class="sxs-lookup"><span data-stu-id="09d79-165">DateDiffWeek</span></span>
* <span data-ttu-id="09d79-166">DateFromParts</span><span class="sxs-lookup"><span data-stu-id="09d79-166">DateFromParts</span></span>

<span data-ttu-id="09d79-167">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="09d79-167">For example:</span></span>

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="09d79-168">La documentazione viene rilevata in base al problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="09d79-168">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="09d79-169">Eseguire query sulle traduzioni per altri costrutti di matrici di byte</span><span class="sxs-lookup"><span data-stu-id="09d79-169">Query translations for more byte array constructs</span></span>

<span data-ttu-id="09d79-170">Le query che usano le proprietà Contains, length, SequenceEqual e così via su byte [] vengono ora convertite in SQL.</span><span class="sxs-lookup"><span data-stu-id="09d79-170">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="09d79-171">La documentazione preliminare è inclusa nello [stato settimanale EF del 5 dicembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="09d79-171">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="09d79-172">La documentazione aggiuntiva viene rilevata in base al problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="09d79-172">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="09d79-173">Conversione di query per invertire</span><span class="sxs-lookup"><span data-stu-id="09d79-173">Query translation for Reverse</span></span>

<span data-ttu-id="09d79-174">Le query che usano `Reverse` ora vengono convertite.</span><span class="sxs-lookup"><span data-stu-id="09d79-174">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="09d79-175">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="09d79-175">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="09d79-176">La documentazione viene rilevata in base al problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="09d79-176">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="09d79-177">Conversione di query per operatori bit per bit</span><span class="sxs-lookup"><span data-stu-id="09d79-177">Query translation for bitwise operators</span></span>

<span data-ttu-id="09d79-178">Le query che utilizzano operatori bit per bit vengono ora convertite in più casi, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="09d79-178">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="09d79-179">La documentazione viene rilevata in base al problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="09d79-179">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="09d79-180">Conversione di query per le stringhe in Cosmos</span><span class="sxs-lookup"><span data-stu-id="09d79-180">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="09d79-181">Le query che usano i metodi String contengono, StartsWith e EndsWith vengono convertite quando si usa il provider di Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="09d79-181">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="09d79-182">La documentazione viene rilevata in base al problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="09d79-182">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
