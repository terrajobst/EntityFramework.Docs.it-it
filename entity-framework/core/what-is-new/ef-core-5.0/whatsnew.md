---
title: Novità di EF Core 5,0
author: ajcvickers
ms.date: 01/29/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 65d7bd43e8a00c77fd6091a74c677635710d03e3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417966"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="d0917-102">Novità di EF Core 5,0</span><span class="sxs-lookup"><span data-stu-id="d0917-102">What's New in EF Core 5.0</span></span>

<span data-ttu-id="d0917-103">EF Core 5,0 è attualmente in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="d0917-103">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="d0917-104">Questa pagina conterrà una panoramica delle modifiche interessanti introdotte in ogni anteprima.</span><span class="sxs-lookup"><span data-stu-id="d0917-104">This page will contain an overview of interesting changes introduced in each preview.</span></span>
<span data-ttu-id="d0917-105">La prima anteprima di EF Core 5,0 è prevista provvisoriamente nel primo trimestre del 2020.</span><span class="sxs-lookup"><span data-stu-id="d0917-105">The first preview of EF Core 5.0 is tentatively expected in in the first quarter of 2020.</span></span>

<span data-ttu-id="d0917-106">Questa pagina non duplica il [piano per EF Core 5,0](plan.md).</span><span class="sxs-lookup"><span data-stu-id="d0917-106">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="d0917-107">Il piano descrive i temi generali per EF Core 5,0, inclusi tutti gli elementi che si prevede di includere prima di distribuire la versione finale.</span><span class="sxs-lookup"><span data-stu-id="d0917-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="d0917-108">I collegamenti da qui vengono aggiunti alla documentazione ufficiale appena pubblicata.</span><span class="sxs-lookup"><span data-stu-id="d0917-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-1-not-yet-shipped"></a><span data-ttu-id="d0917-109">Anteprima 1 (non ancora spedito)</span><span class="sxs-lookup"><span data-stu-id="d0917-109">Preview 1 (Not yet shipped)</span></span>

### <a name="simple-logging"></a><span data-ttu-id="d0917-110">Registrazione semplice</span><span class="sxs-lookup"><span data-stu-id="d0917-110">Simple logging</span></span>

<span data-ttu-id="d0917-111">Questa funzionalità aggiunge funzionalità simili a `Database.Log` in EF6.</span><span class="sxs-lookup"><span data-stu-id="d0917-111">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="d0917-112">Ovvero, fornisce un modo semplice per ottenere i log da EF Core senza la necessità di configurare alcun tipo di Framework di registrazione esterno.</span><span class="sxs-lookup"><span data-stu-id="d0917-112">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="d0917-113">La documentazione preliminare è inclusa nello [stato settimanale EF del 5 dicembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="d0917-113">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="d0917-114">La documentazione aggiuntiva viene rilevata in base al problema [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span><span class="sxs-lookup"><span data-stu-id="d0917-114">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="d0917-115">Modo semplice per ottenere SQL generato</span><span class="sxs-lookup"><span data-stu-id="d0917-115">Simple way to get generated SQL</span></span>

<span data-ttu-id="d0917-116">EF Core 5,0 introduce il metodo di estensione `ToQueryString` che restituirà il SQL che EF Core genererà durante l'esecuzione di una query LINQ.</span><span class="sxs-lookup"><span data-stu-id="d0917-116">EF Core 5.0 introduces the `ToQueryString` extension method which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="d0917-117">La documentazione preliminare è inclusa nello [stato settimanale EF per il 9 gennaio 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span><span class="sxs-lookup"><span data-stu-id="d0917-117">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="d0917-118">La documentazione aggiuntiva viene rilevata in base al problema [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span><span class="sxs-lookup"><span data-stu-id="d0917-118">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="d0917-119">Viste di debug migliorate</span><span class="sxs-lookup"><span data-stu-id="d0917-119">Enhanced debug views</span></span>

<span data-ttu-id="d0917-120">Le visualizzazioni di debug rappresentano un modo semplice per esaminare gli elementi interni di EF Core durante il debug dei problemi.</span><span class="sxs-lookup"><span data-stu-id="d0917-120">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="d0917-121">Una visualizzazione di debug per il modello è stata implementata qualche tempo fa.</span><span class="sxs-lookup"><span data-stu-id="d0917-121">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="d0917-122">Per EF Core 5,0, la visualizzazione modello è stata semplificata per la lettura e l'aggiunta di una nuova visualizzazione debug per le entità rilevate nel gestore di stato.</span><span class="sxs-lookup"><span data-stu-id="d0917-122">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="d0917-123">La documentazione preliminare è inclusa nello [stato settimanale EF per il 12 dicembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="d0917-123">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="d0917-124">La documentazione aggiuntiva viene rilevata in base al problema [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span><span class="sxs-lookup"><span data-stu-id="d0917-124">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="d0917-125">La stringa di connessione o di connessione può essere modificata in DbContext inizializzato</span><span class="sxs-lookup"><span data-stu-id="d0917-125">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="d0917-126">È ora più semplice creare un'istanza di DbContext senza connessione o stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="d0917-126">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="d0917-127">Inoltre, la connessione o la stringa di connessione possono ora essere mutate sull'istanza del contesto.</span><span class="sxs-lookup"><span data-stu-id="d0917-127">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="d0917-128">Ciò consente alla stessa istanza del contesto di connettersi dinamicamente a database diversi.</span><span class="sxs-lookup"><span data-stu-id="d0917-128">This allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="d0917-129">La documentazione viene rilevata in base al problema [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span><span class="sxs-lookup"><span data-stu-id="d0917-129">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="d0917-130">Proxy di rilevamento delle modifiche</span><span class="sxs-lookup"><span data-stu-id="d0917-130">Change-tracking proxies</span></span>

<span data-ttu-id="d0917-131">EF Core ora possono generare proxy di runtime che implementano automaticamente [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) e [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="d0917-131">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="d0917-132">Queste segnalano quindi le modifiche ai valori delle proprietà dell'entità direttamente a EF Core, evitando la necessità di analizzare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="d0917-132">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="d0917-133">Tuttavia, i proxy presentano un set di limitazioni, quindi non sono destinati a tutti.</span><span class="sxs-lookup"><span data-stu-id="d0917-133">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="d0917-134">La documentazione viene rilevata in base al problema [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span><span class="sxs-lookup"><span data-stu-id="d0917-134">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="d0917-135">Gestione migliorata della semantica null del database</span><span class="sxs-lookup"><span data-stu-id="d0917-135">Improved handling of database null semantics</span></span>

<span data-ttu-id="d0917-136">I database relazionali considerano in genere NULL come valore sconosciuto e pertanto non uguale a qualsiasi altro valore NULL.</span><span class="sxs-lookup"><span data-stu-id="d0917-136">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="d0917-137">C#d'altra parte, considera null come un valore definito che confronta uguale a qualsiasi altro valore null.</span><span class="sxs-lookup"><span data-stu-id="d0917-137">C#, on the other hand, treats null as a defined value which compares equal to any other null.</span></span>
<span data-ttu-id="d0917-138">EF Core per impostazione predefinita converte le query in modo che utilizzino C# la semantica null.</span><span class="sxs-lookup"><span data-stu-id="d0917-138">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="d0917-139">EF Core 5,0 migliora notevolmente l'efficienza di queste traduzioni.</span><span class="sxs-lookup"><span data-stu-id="d0917-139">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="d0917-140">La documentazione viene rilevata in base al problema [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span><span class="sxs-lookup"><span data-stu-id="d0917-140">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="d0917-141">Proprietà indicizzatore</span><span class="sxs-lookup"><span data-stu-id="d0917-141">Indexer properties</span></span>

<span data-ttu-id="d0917-142">EF Core 5,0 supporta il mapping C# delle proprietà dell'indicizzatore.</span><span class="sxs-lookup"><span data-stu-id="d0917-142">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="d0917-143">In questo modo le entità possono fungere da contenitori di proprietà in cui viene eseguito il mapping delle colonne alle proprietà denominate nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="d0917-143">This allows entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="d0917-144">La documentazione viene rilevata in base al problema [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span><span class="sxs-lookup"><span data-stu-id="d0917-144">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="d0917-145">Generazione di vincoli check per i mapping enum</span><span class="sxs-lookup"><span data-stu-id="d0917-145">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="d0917-146">EF Core 5,0 le migrazioni possono ora generare vincoli CHECK per i mapping delle proprietà di enumerazione.</span><span class="sxs-lookup"><span data-stu-id="d0917-146">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="d0917-147">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d0917-147">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="d0917-148">La documentazione viene rilevata in base al problema [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span><span class="sxs-lookup"><span data-stu-id="d0917-148">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="d0917-149">Eseguire query sulle traduzioni per più costrutti DateTime</span><span class="sxs-lookup"><span data-stu-id="d0917-149">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="d0917-150">Vengono ora convertite le query contenenti la nuova costruzione di data/ora.</span><span class="sxs-lookup"><span data-stu-id="d0917-150">Queries containing new DataTime construction are now translated.</span></span>
<span data-ttu-id="d0917-151">Inoltre, la funzione SQL Server DateDiffWeek è ora mappata.</span><span class="sxs-lookup"><span data-stu-id="d0917-151">Also, the SQL Server function DateDiffWeek is now mapped.</span></span>

<span data-ttu-id="d0917-152">La documentazione viene rilevata in base al problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="d0917-152">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="d0917-153">Eseguire query sulle traduzioni per altri costrutti di matrici di byte</span><span class="sxs-lookup"><span data-stu-id="d0917-153">Query translations for more byte array constructs</span></span>

<span data-ttu-id="d0917-154">Le query che usano le proprietà Contains, length, SequenceEqual e così via su byte [] vengono ora convertite in SQL.</span><span class="sxs-lookup"><span data-stu-id="d0917-154">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="d0917-155">La documentazione preliminare è inclusa nello [stato settimanale EF del 5 dicembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="d0917-155">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="d0917-156">La documentazione aggiuntiva viene rilevata in base al problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="d0917-156">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="d0917-157">Conversione di query per invertire</span><span class="sxs-lookup"><span data-stu-id="d0917-157">Query translation for Reverse</span></span>

<span data-ttu-id="d0917-158">Le query che usano `Reverse` ora vengono convertite.</span><span class="sxs-lookup"><span data-stu-id="d0917-158">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="d0917-159">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d0917-159">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="d0917-160">La documentazione viene rilevata in base al problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="d0917-160">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="d0917-161">Conversione di query per operatori bit per bit</span><span class="sxs-lookup"><span data-stu-id="d0917-161">Query translation for bitwise operators</span></span>

<span data-ttu-id="d0917-162">Le query che utilizzano operatori bit per bit vengono ora convertite in più casi, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d0917-162">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="d0917-163">La documentazione viene rilevata in base al problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="d0917-163">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="d0917-164">Conversione di query per le stringhe in Cosmos</span><span class="sxs-lookup"><span data-stu-id="d0917-164">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="d0917-165">Le query che usano i metodi String contengono, StartsWith e EndsWith vengono convertite quando si usa il provider di Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d0917-165">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="d0917-166">La documentazione viene rilevata in base al problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="d0917-166">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
