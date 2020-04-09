---
title: Novità di EF Core 5.0
description: Panoramica delle nuove funzionalità di EF Core 5.0Overview of new features in EF Core 5.0
author: ajcvickers
ms.date: 03/30/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: c047a308cadf44eea577dcab29b68b36942a50df
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634272"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="6c282-103">Novità di EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="6c282-103">What's New in EF Core 5.0</span></span>

<span data-ttu-id="6c282-104">EF Core 5.0 è attualmente in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="6c282-104">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="6c282-105">Questa pagina conterrà una panoramica delle modifiche interessanti introdotte in ogni anteprima.</span><span class="sxs-lookup"><span data-stu-id="6c282-105">This page will contain an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="6c282-106">Questa pagina non duplica il [piano per EF Core 5.0.](plan.md)</span><span class="sxs-lookup"><span data-stu-id="6c282-106">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="6c282-107">Il piano descrive i temi generali per EF Core 5.0, incluso tutto ciò che si prevede di includere prima della spedizione della versione finale.</span><span class="sxs-lookup"><span data-stu-id="6c282-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="6c282-108">Aggiungeremo link da qui alla documentazione ufficiale così come viene pubblicata.</span><span class="sxs-lookup"><span data-stu-id="6c282-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-2"></a><span data-ttu-id="6c282-109">Preview 2</span><span class="sxs-lookup"><span data-stu-id="6c282-109">Preview 2</span></span>

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a><span data-ttu-id="6c282-110">Usare un attributo di C' per specificare un campo di sostegno delle proprietà</span><span class="sxs-lookup"><span data-stu-id="6c282-110">Use a C# attribute to specify a property backing field</span></span>

<span data-ttu-id="6c282-111">È ora possibile usare un attributo di C, per specificare il campo di backup per una proprietà.</span><span class="sxs-lookup"><span data-stu-id="6c282-111">A C# attribute can now be used to specify the backing field for a property.</span></span>
<span data-ttu-id="6c282-112">Questo attributo consente a EF Core di scrivere e leggere dal campo di backup come si verificherebbe normalmente, anche quando non è possibile trovare automaticamente il campo di backup.</span><span class="sxs-lookup"><span data-stu-id="6c282-112">This attribute allows EF Core to still write to and read from the backing field as would normally happen, even when the backing field cannot be found automatically.</span></span>
<span data-ttu-id="6c282-113">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6c282-113">For example:</span></span>

```CSharp
public class Blog
{
    private string _mainTitle;

    public int Id { get; set; }

    [BackingField(nameof(_mainTitle))]
    public string Title
    {
        get => _mainTitle;
        set => _mainTitle = value;
    }
}
```

<span data-ttu-id="6c282-114">La documentazione viene tracciata per problema [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span><span class="sxs-lookup"><span data-stu-id="6c282-114">Documentation is tracked by issue [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span></span>

### <a name="complete-discriminator-mapping"></a><span data-ttu-id="6c282-115">Mappatura completa dei discriminatori</span><span class="sxs-lookup"><span data-stu-id="6c282-115">Complete discriminator mapping</span></span>

<span data-ttu-id="6c282-116">EF Core utilizza una colonna discriminatore per il [mapping TPH di una gerarchia di ereditarietà](/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="6c282-116">EF Core uses a discriminator column for [TPH mapping of an inheritance hierarchy](/ef/core/modeling/inheritance).</span></span>
<span data-ttu-id="6c282-117">Alcuni miglioramenti delle prestazioni sono possibili finché EF Core conosce tutti i valori possibili per il discriminatore.</span><span class="sxs-lookup"><span data-stu-id="6c282-117">Some performance enhancements are possible so long as EF Core knows all possible values for the discriminator.</span></span>
<span data-ttu-id="6c282-118">EF Core 5.0 implementa ora questi miglioramenti.</span><span class="sxs-lookup"><span data-stu-id="6c282-118">EF Core 5.0 now implements these enhancements.</span></span>

<span data-ttu-id="6c282-119">Ad esempio, le versioni precedenti di Entity Framework Core genererebbe sempre questo codice SQL per una query che restituisce tutti i tipi in una gerarchia:For example, previous versions of EF Core would always generate this SQL for a query returning all types in a hierarchy:</span><span class="sxs-lookup"><span data-stu-id="6c282-119">For example, previous versions of EF Core would always generate this SQL for a query returning all types in a hierarchy:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

<span data-ttu-id="6c282-120">EF Core 5.0 genererà ora quanto segue quando viene configurato un mapping completo del discriminatore:EF Core 5.0 will now generate the following when a complete discriminator mapping is configured:</span><span class="sxs-lookup"><span data-stu-id="6c282-120">EF Core 5.0 will now generate the following when a complete discriminator mapping is configured:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

<span data-ttu-id="6c282-121">Sarà il comportamento predefinito a partire dall'anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="6c282-121">It will be the default behavior starting with preview 3.</span></span>

### <a name="performance-improvements-in-microsoftdatasqlite"></a><span data-ttu-id="6c282-122">Miglioramenti delle prestazioni in Microsoft.Data.SqlitePerformance improvements in Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="6c282-122">Performance improvements in Microsoft.Data.Sqlite</span></span>

<span data-ttu-id="6c282-123">Sono stati apportati due miglioramenti delle prestazioni per SQLIte:We have made two performance improvements for SQLIte:</span><span class="sxs-lookup"><span data-stu-id="6c282-123">We have made two performance improvements for SQLIte:</span></span>

* <span data-ttu-id="6c282-124">Il recupero di dati binari e di stringa con GetBytes, GetChars e GetTextReader è ora più efficiente utilizzando SqliteBlob e flussi.</span><span class="sxs-lookup"><span data-stu-id="6c282-124">Retrieving binary and string data with GetBytes, GetChars, and GetTextReader is now more efficient by making use of SqliteBlob and streams.</span></span>
* <span data-ttu-id="6c282-125">L'inizializzazione di SqliteConnection è ora lazy.</span><span class="sxs-lookup"><span data-stu-id="6c282-125">Initialization of SqliteConnection is now lazy.</span></span>

<span data-ttu-id="6c282-126">Questi miglioramenti sono nel ADO.NET Microsoft.Data.Sqlite provider e quindi migliorare anche le prestazioni al di fuori di Entity Framework Core.These improvements are in the ADO.NET Microsoft.Data.Sqlite provider and so it also improve performance outside of EF Core.</span><span class="sxs-lookup"><span data-stu-id="6c282-126">These improvements are in the ADO.NET Microsoft.Data.Sqlite provider and hence also improve performance outside of EF Core.</span></span>

## <a name="preview-1"></a><span data-ttu-id="6c282-127">Preview 1</span><span class="sxs-lookup"><span data-stu-id="6c282-127">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="6c282-128">Registrazione semplice</span><span class="sxs-lookup"><span data-stu-id="6c282-128">Simple logging</span></span>

<span data-ttu-id="6c282-129">Questa funzionalità aggiunge `Database.Log` funzionalità simili a In EF6.</span><span class="sxs-lookup"><span data-stu-id="6c282-129">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="6c282-130">Vale a dire, fornisce un modo semplice per ottenere i log da EF Core senza la necessità di configurare qualsiasi tipo di framework di registrazione esterna.</span><span class="sxs-lookup"><span data-stu-id="6c282-130">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="6c282-131">La documentazione preliminare è inclusa nello [stato settimanale EF per il 5 dicembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="6c282-131">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="6c282-132">La documentazione aggiuntiva viene monitorata per problema [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span><span class="sxs-lookup"><span data-stu-id="6c282-132">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="6c282-133">Modo semplice per ottenere codice SQL generato</span><span class="sxs-lookup"><span data-stu-id="6c282-133">Simple way to get generated SQL</span></span>

<span data-ttu-id="6c282-134">EF Core 5.0 `ToQueryString` introduce il metodo di estensione, che restituirà il codice SQL generato da Entity Framework Core durante l'esecuzione di una query LINQ.</span><span class="sxs-lookup"><span data-stu-id="6c282-134">EF Core 5.0 introduces the `ToQueryString` extension method, which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="6c282-135">La documentazione preliminare è inclusa nello [stato settimanale EF per il 9 gennaio 2020.](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246)</span><span class="sxs-lookup"><span data-stu-id="6c282-135">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="6c282-136">La documentazione aggiuntiva viene monitorata per [#1331.](https://github.com/dotnet/EntityFramework.Docs/issues/1331)</span><span class="sxs-lookup"><span data-stu-id="6c282-136">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="6c282-137">Usare un attributo di C' per indicare che un'entità non ha una chiaveUse a C' attribute to indicate that an entity has no key</span><span class="sxs-lookup"><span data-stu-id="6c282-137">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="6c282-138">Un tipo di entità può ora essere `KeylessAttribute`configurato come senza chiave utilizzando il nuovo .</span><span class="sxs-lookup"><span data-stu-id="6c282-138">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span>
<span data-ttu-id="6c282-139">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6c282-139">For example:</span></span>

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="6c282-140">La documentazione viene tracciata per [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span><span class="sxs-lookup"><span data-stu-id="6c282-140">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="6c282-141">Connection or connection string can be changed on initialized DbContext</span><span class="sxs-lookup"><span data-stu-id="6c282-141">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="6c282-142">Ora è più semplice creare un'istanza DbContext senza alcuna stringa di connessione o di connessione.</span><span class="sxs-lookup"><span data-stu-id="6c282-142">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="6c282-143">Inoltre, la connessione o la stringa di connessione può ora essere attivata nell'istanza del contesto.</span><span class="sxs-lookup"><span data-stu-id="6c282-143">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="6c282-144">Questa funzionalità consente alla stessa istanza di contesto di connettersi dinamicamente a database diversi.</span><span class="sxs-lookup"><span data-stu-id="6c282-144">This feature allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="6c282-145">La documentazione viene tracciata per [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span><span class="sxs-lookup"><span data-stu-id="6c282-145">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="6c282-146">Proxy di rilevamento delle modifiche</span><span class="sxs-lookup"><span data-stu-id="6c282-146">Change-tracking proxies</span></span>

<span data-ttu-id="6c282-147">EF Core può ora generare proxy di runtime che implementano automaticamente [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) e [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="6c282-147">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="6c282-148">Questi quindi segnalare le modifiche al valore delle proprietà dell'entità direttamente in Entity Framework Core, evitando la necessità di eseguire la scansione per le modifiche.</span><span class="sxs-lookup"><span data-stu-id="6c282-148">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="6c282-149">Tuttavia, i proxy sono dotati di una propria serie di limitazioni, quindi non sono per tutti.</span><span class="sxs-lookup"><span data-stu-id="6c282-149">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="6c282-150">La documentazione viene tracciata per problema [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span><span class="sxs-lookup"><span data-stu-id="6c282-150">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="6c282-151">Visualizzazioni di debug migliorate</span><span class="sxs-lookup"><span data-stu-id="6c282-151">Enhanced debug views</span></span>

<span data-ttu-id="6c282-152">Le visualizzazioni di debug sono un modo semplice per esaminare le funzionalità interne di EF Core durante il debug dei problemi.</span><span class="sxs-lookup"><span data-stu-id="6c282-152">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="6c282-153">Una visualizzazione di debug per il modello è stata implementata qualche tempo fa.</span><span class="sxs-lookup"><span data-stu-id="6c282-153">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="6c282-154">Per EF Core 5.0, abbiamo reso la visualizzazione del modello più facile da leggere e aggiunto una nuova visualizzazione di debug per le entità rilevate nel gestore dello stato.</span><span class="sxs-lookup"><span data-stu-id="6c282-154">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="6c282-155">La documentazione preliminare è inclusa nello [stato settimanale EF per il 12 dicembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="6c282-155">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="6c282-156">La documentazione aggiuntiva viene tenuta traccia in base al problema [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span><span class="sxs-lookup"><span data-stu-id="6c282-156">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="6c282-157">Gestione dei valori null del database migliorata</span><span class="sxs-lookup"><span data-stu-id="6c282-157">Improved handling of database null semantics</span></span>

<span data-ttu-id="6c282-158">I database relazionali in genere considerano NULL come valore sconosciuto e pertanto non sono uguali a qualsiasi altro valore NULL.</span><span class="sxs-lookup"><span data-stu-id="6c282-158">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="6c282-159">Mentre il linguaggio C' considera null come un valore definito, che confronta uguale a qualsiasi altro valore null.</span><span class="sxs-lookup"><span data-stu-id="6c282-159">While C# treats null as a defined value, which compares equal to any other null.</span></span>
<span data-ttu-id="6c282-160">EF Core per impostazione predefinita converte le query in modo che utilizzino la semantica null di C.</span><span class="sxs-lookup"><span data-stu-id="6c282-160">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="6c282-161">EF Core 5.0 migliora notevolmente l'efficienza di queste traduzioni.</span><span class="sxs-lookup"><span data-stu-id="6c282-161">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="6c282-162">La documentazione viene tracciata dal problema [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span><span class="sxs-lookup"><span data-stu-id="6c282-162">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="6c282-163">Proprietà dell'indicizzatore</span><span class="sxs-lookup"><span data-stu-id="6c282-163">Indexer properties</span></span>

<span data-ttu-id="6c282-164">EF Core 5.0 supporta il mapping delle proprietà dell'indicizzatore di C.</span><span class="sxs-lookup"><span data-stu-id="6c282-164">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="6c282-165">Queste proprietà consentono alle entità di fungere da contenitori di proprietà in cui le colonne sono mappate alle proprietà denominate nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="6c282-165">These properties allow entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="6c282-166">La documentazione viene tracciata per problema [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span><span class="sxs-lookup"><span data-stu-id="6c282-166">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="6c282-167">Generazione di vincoli CHECK per i mapping di enumerazione</span><span class="sxs-lookup"><span data-stu-id="6c282-167">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="6c282-168">Le migrazioni di EF Core 5.0 possono ora generare vincoli CHECK per i mapping delle proprietà enum.</span><span class="sxs-lookup"><span data-stu-id="6c282-168">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="6c282-169">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6c282-169">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="6c282-170">La documentazione viene tracciata per problema [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span><span class="sxs-lookup"><span data-stu-id="6c282-170">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="6c282-171">IsRelational</span><span class="sxs-lookup"><span data-stu-id="6c282-171">IsRelational</span></span>

<span data-ttu-id="6c282-172">È `IsRelational` stato aggiunto un nuovo metodo `IsSqlServer` `IsSqlite`oltre `IsInMemory`all'oggetto esistente , , e .</span><span class="sxs-lookup"><span data-stu-id="6c282-172">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span>
<span data-ttu-id="6c282-173">Questo metodo può essere utilizzato per verificare se il DbContext utilizza qualsiasi provider di database relazionale.</span><span class="sxs-lookup"><span data-stu-id="6c282-173">This method can be used to test if the DbContext is using any relational database provider.</span></span>
<span data-ttu-id="6c282-174">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6c282-174">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="6c282-175">La documentazione viene tracciata per problema [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span><span class="sxs-lookup"><span data-stu-id="6c282-175">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="6c282-176">Cosmos concorrenza ottimistica con ETags</span><span class="sxs-lookup"><span data-stu-id="6c282-176">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="6c282-177">Il provider di database cosmos di Azure supporta ora la concorrenza ottimistica tramite ETag.</span><span class="sxs-lookup"><span data-stu-id="6c282-177">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span>
<span data-ttu-id="6c282-178">Utilizzare il generatore di modelli in OnModelCreating per configurare un ETag:Use the model builder in OnModelCreating to configure an ETag:</span><span class="sxs-lookup"><span data-stu-id="6c282-178">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="6c282-179">SaveChanges genererà `DbUpdateConcurrencyException` quindi un conflitto di concorrenza, che [può essere gestito](https://docs.microsoft.com/ef/core/saving/concurrency) per implementare i tentativi e così via.</span><span class="sxs-lookup"><span data-stu-id="6c282-179">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](https://docs.microsoft.com/ef/core/saving/concurrency) to implement retries, etc.</span></span>

<span data-ttu-id="6c282-180">La documentazione viene tracciata per problema [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span><span class="sxs-lookup"><span data-stu-id="6c282-180">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="6c282-181">Traduzioni di query per altri costrutti DateTimeQuery translations for more DateTime constructs</span><span class="sxs-lookup"><span data-stu-id="6c282-181">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="6c282-182">Le query contenenti nuova costruzione DateTime vengono ora convertite.</span><span class="sxs-lookup"><span data-stu-id="6c282-182">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="6c282-183">Inoltre, le seguenti funzioni di SQL Server sono ora mappate:</span><span class="sxs-lookup"><span data-stu-id="6c282-183">In addition, the following SQL Server functions are now mapped:</span></span>

* <span data-ttu-id="6c282-184">DateDiffWeek</span><span class="sxs-lookup"><span data-stu-id="6c282-184">DateDiffWeek</span></span>
* <span data-ttu-id="6c282-185">DateFromParts</span><span class="sxs-lookup"><span data-stu-id="6c282-185">DateFromParts</span></span>

<span data-ttu-id="6c282-186">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6c282-186">For example:</span></span>

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="6c282-187">La documentazione viene tracciata per problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="6c282-187">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="6c282-188">Conversioni di query per altri costrutti di matrici di byteQuery translations for more byte array constructs</span><span class="sxs-lookup"><span data-stu-id="6c282-188">Query translations for more byte array constructs</span></span>

<span data-ttu-id="6c282-189">Le query che utilizzano le proprietà Contains, Length, SequenceEqual e così via su byte[] vengono ora convertite in SQL.</span><span class="sxs-lookup"><span data-stu-id="6c282-189">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="6c282-190">La documentazione preliminare è inclusa nello [stato settimanale EF per il 5 dicembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="6c282-190">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="6c282-191">La documentazione aggiuntiva viene monitorata per [problema #2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="6c282-191">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="6c282-192">Query translation for Reverse</span><span class="sxs-lookup"><span data-stu-id="6c282-192">Query translation for Reverse</span></span>

<span data-ttu-id="6c282-193">Le `Reverse` query che utilizzano vengono ora tradotte.</span><span class="sxs-lookup"><span data-stu-id="6c282-193">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="6c282-194">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="6c282-194">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="6c282-195">La documentazione viene tracciata per problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="6c282-195">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="6c282-196">Conversione di query per operatori bit per bitQuery translation for bitwise operators</span><span class="sxs-lookup"><span data-stu-id="6c282-196">Query translation for bitwise operators</span></span>

<span data-ttu-id="6c282-197">Le query che utilizzano operatori bit per bit vengono ora convertite in più casi Ad esempio:Query using bitwise operators are now translated in more cases For example:</span><span class="sxs-lookup"><span data-stu-id="6c282-197">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="6c282-198">La documentazione viene tracciata per problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="6c282-198">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="6c282-199">Traduzione di query per stringhe su Cosmos</span><span class="sxs-lookup"><span data-stu-id="6c282-199">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="6c282-200">Le query che usano i metodi stringa Contains, StartsWith e EndsWith vengono ora convertite quando si usa il provider database Cosmos di Azure.Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span><span class="sxs-lookup"><span data-stu-id="6c282-200">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="6c282-201">La documentazione viene tracciata per problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="6c282-201">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
