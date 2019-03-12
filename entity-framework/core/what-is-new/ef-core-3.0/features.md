---
title: Nuove funzionalità di EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: cf0d2cf032b9aa319fe706aece5b1ea66a5d6251
ms.sourcegitcommit: a013e243a14f384999ceccaf9c779b8c1ae3b936
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2019
ms.locfileid: "57463363"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="62336-102">Nuove funzionalità incluse in EF Core 3.0 (attualmente in anteprima)</span><span class="sxs-lookup"><span data-stu-id="62336-102">New features included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="62336-103">Si tenga presente che i set di funzionalità e le pianificazioni delle versioni future sono sempre soggette a modifiche e che questa pagina, nonostante l'impegno profuso per mantenerla aggiornata, potrebbe non riflettere sempre i piani più recenti.</span><span class="sxs-lookup"><span data-stu-id="62336-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="62336-104">Nell'elenco seguente sono riportate le principali nuove funzionalità pianificate per EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="62336-104">The following list includes the major new features planned for EF Core 3.0.</span></span>
<span data-ttu-id="62336-105">La maggior parte delle funzionalità non è inclusa nell'anteprima corrente, ma sarà disponibile non appena si faranno passi avanti rispetto a RTM.</span><span class="sxs-lookup"><span data-stu-id="62336-105">Most of these features are not included in the current preview, but will become available as we make progress towards RTM.</span></span>

<span data-ttu-id="62336-106">Il motivo è che all'inizio del rilascio ci si concentra sull'implementazione di [modifiche pianificate che causano un'interruzione](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span><span class="sxs-lookup"><span data-stu-id="62336-106">The reason is that at the beginning of the release we are focusing on implementing planned [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span></span>
<span data-ttu-id="62336-107">Molte di queste modifiche sono miglioramenti apportati a EF Core in modo indipendente.</span><span class="sxs-lookup"><span data-stu-id="62336-107">Many of these breaking changes are improvements to EF Core on their own.</span></span>
<span data-ttu-id="62336-108">Per sbloccare ulteriori miglioramenti sono necessarie molte altre modifiche.</span><span class="sxs-lookup"><span data-stu-id="62336-108">Many others are required to unblock further improvements.</span></span> 

<span data-ttu-id="62336-109">Per un elenco completo dei miglioramenti e delle correzioni di bug in corso, vedere [questa query nello strumento di gestione dei problemi](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="62336-109">For a complete list of bug fixes and enhancements underway, you can see [this query in our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span></span>

## <a name="linq-improvements"></a><span data-ttu-id="62336-110">Miglioramenti di LINQ</span><span class="sxs-lookup"><span data-stu-id="62336-110">LINQ improvements</span></span> 

[<span data-ttu-id="62336-111">Problema n. 12795</span><span class="sxs-lookup"><span data-stu-id="62336-111">Tracking Issue #12795</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

<span data-ttu-id="62336-112">Il lavoro su questa funzionalità è iniziato ma non è incluso nell'anteprima corrente.</span><span class="sxs-lookup"><span data-stu-id="62336-112">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="62336-113">LINQ consente di scrivere query di database senza uscire dal linguaggio preferito, sfruttando i vantaggi delle informazioni complete sui tipi per ottenere IntelliSense e il controllo dei tipi in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="62336-113">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to get IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="62336-114">LINQ consente però di scrivere anche un numero illimitato di query complesse e ciò ha sempre rappresentato una grande sfida per i provider LINQ.</span><span class="sxs-lookup"><span data-stu-id="62336-114">But LINQ also enables you to write an unlimited number of complicated queries, and that has always been a huge challenge for LINQ providers.</span></span>
<span data-ttu-id="62336-115">Nelle prime versioni di EF Core, questa complicazione è stata risolta in parte, cercando di individuare quali parti di una query possono essere convertite in SQL e consentendo quindi l'esecuzione del resto della query in memoria nel client.</span><span class="sxs-lookup"><span data-stu-id="62336-115">In the first few versions of EF Core, we solved that in part by figuring out what portions of a query could be translated to SQL, and then by allowing the rest of the query to execute in memory on the client.</span></span>
<span data-ttu-id="62336-116">L'esecuzione sul lato client può essere appropriata in alcuni casi, ma in molti altri casi può causare query inefficienti che potrebbero non essere identificate fino a quando un'applicazione viene distribuita nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="62336-116">This client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries that may not be identified until an application is deployed to production.</span></span>
<span data-ttu-id="62336-117">In EF Core 3.0 sono previste modifiche sostanziali del funzionamento dell'implementazione di LINQ e delle procedure per testarla.</span><span class="sxs-lookup"><span data-stu-id="62336-117">In EF Core 3.0, we're planning to make profound changes to how our LINQ implementation works, and how we test it.</span></span>
<span data-ttu-id="62336-118">Gli obiettivi sono: ottenere una maggiore solidità, ad esempio per evitare di compromettere il funzionamento delle query con il rilascio di patch, riuscire a convertire più espressioni correttamente in SQL, generare query efficienti in un maggior numero di casi ed evitare che query inefficienti non vengano rilevate.</span><span class="sxs-lookup"><span data-stu-id="62336-118">The goals are to make it more robust (for example, to avoid breaking queries in patch releases), to enable translating more expressions correctly into SQL, to generate efficient queries in more cases, and to prevent inefficient queries from going undetected.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="62336-119">Supporto di Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="62336-119">Cosmos DB support</span></span> 

[<span data-ttu-id="62336-120">Problema n. 8443</span><span class="sxs-lookup"><span data-stu-id="62336-120">Tracking Issue #8443</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

<span data-ttu-id="62336-121">Questa funzionalità è inclusa nell'anteprima corrente, ma non è ancora completa.</span><span class="sxs-lookup"><span data-stu-id="62336-121">This feature is included in the current preview, but isn't complete yet.</span></span> 

<span data-ttu-id="62336-122">è in corso lo sviluppo di un provider Cosmos DB per EF Core, per consentire agli sviluppatori che hanno familiarità con il modello di programmazione di EF di selezionare facilmente Azure Cosmos DB come destinazione per il database dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="62336-122">We're working on a Cosmos DB provider for EF Core, to enable developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="62336-123">L'obiettivo è quello di rendere alcuni dei vantaggi di Cosmos DB, ad esempio la distribuzione globale, la disponibilità "AlwaysOn", la scalabilità elastica e la bassa latenza, ancora più accessibili per gli sviluppatori .NET.</span><span class="sxs-lookup"><span data-stu-id="62336-123">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="62336-124">Il provider abiliterà la maggior parte delle funzionalità di EF Core, come il rilevamento delle modifiche automatico, LINQ e le conversioni dei valori, con l'API SQL in Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="62336-124">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>
<span data-ttu-id="62336-125">Questo progetto è stato avviato prima di EF Core 2.2 e [sono state pubblicate alcune versioni di anteprima del provider](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span><span class="sxs-lookup"><span data-stu-id="62336-125">We started this effort before EF Core 2.2, and [we have made some preview versions of the provider available](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span></span>
<span data-ttu-id="62336-126">Il nuovo piano prevede di continuare a sviluppare il provider insieme a EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="62336-126">The new plan is to continue developing the provider alongside EF Core 3.0.</span></span> 

## <a name="c-80-support"></a><span data-ttu-id="62336-127">Supporto di C# 8.0</span><span class="sxs-lookup"><span data-stu-id="62336-127">C# 8.0 support</span></span>

<span data-ttu-id="62336-128">[Problema n. 12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Problema n. 10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span><span class="sxs-lookup"><span data-stu-id="62336-128">[Tracking Issue #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Tracking Issue #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span></span>

<span data-ttu-id="62336-129">Il lavoro su questa funzionalità è iniziato ma non è incluso nell'anteprima corrente.</span><span class="sxs-lookup"><span data-stu-id="62336-129">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="62336-130">Lo scopo è consentire ai clienti Microsoft di sfruttare alcune delle [nuove funzionalità previste per C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/), ad esempio i flussi asincroni (tra cui `await foreach`) e i tipi riferimento nullable durante l'uso di EF Core.</span><span class="sxs-lookup"><span data-stu-id="62336-130">We want our customers to take advantage of some of the [new features coming in C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) like async streams (including `await foreach`) and nullable reference types while using EF Core.</span></span>

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="62336-131">Decompilazione delle viste di database</span><span class="sxs-lookup"><span data-stu-id="62336-131">Reverse engineering of database views</span></span>

[<span data-ttu-id="62336-132">Problema n. 1679</span><span class="sxs-lookup"><span data-stu-id="62336-132">Tracking Issue #1679</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

<span data-ttu-id="62336-133">Questa funzionalità è inclusa nell'anteprima corrente.</span><span class="sxs-lookup"><span data-stu-id="62336-133">This feature isn't included in the current preview.</span></span>

<span data-ttu-id="62336-134">I [tipi di query](xref:core/modeling/query-types), introdotti in EF Core 2.1 e considerati tipi di entità senza chiavi in EF Core 3.0, rappresentano i dati che possono essere letti dal database, ma non aggiornati.</span><span class="sxs-lookup"><span data-stu-id="62336-134">[Query types](xref:core/modeling/query-types), introduced in EF Core 2.1 and considered entity types without keys in EF Core 3.0, represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="62336-135">Questa caratteristica li rende la scelta ideale per le viste di database nella maggior parte degli scenari, quindi si prevede di automatizzare la creazione dei tipi di entità senza chiavi durante la decompilazione delle viste di database.</span><span class="sxs-lookup"><span data-stu-id="62336-135">This characteristic makes them an excellent fit for database views in most scenarios, so we plan to automate the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="property-bag-entities"></a><span data-ttu-id="62336-136">Entità elenco proprietà</span><span class="sxs-lookup"><span data-stu-id="62336-136">Property bag entities</span></span> 

<span data-ttu-id="62336-137">[Problemi n. 13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) e [9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span><span class="sxs-lookup"><span data-stu-id="62336-137">[Tracking Issue #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) and [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span></span>

<span data-ttu-id="62336-138">Il lavoro su questa funzionalità è iniziato ma non è incluso nell'anteprima corrente.</span><span class="sxs-lookup"><span data-stu-id="62336-138">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="62336-139">questa funzionalità riguarda l'abilitazione di entità che archiviano dati in proprietà indicizzate anziché in proprietà regolari, nonché la possibilità di usare istanze della stessa classe .NET (in modo potenzialmente semplice come `Dictionary<string, object>`) per rappresentare tipi di entità diversi nello stesso modello di EF Core.</span><span class="sxs-lookup"><span data-stu-id="62336-139">This feature is about enabling entities that store data in indexed properties instead of regular properties, and also about being able to use instances of the same .NET class (potentially something as simple as a `Dictionary<string, object>`) to represent different entity types in the same EF Core model.</span></span>
<span data-ttu-id="62336-140">Questa funzionalità è un trampolino di lancio per il supporto delle relazioni molti-a-molti senza un'entità di join, ovvero uno dei miglioramenti più richiesti per EF Core.</span><span class="sxs-lookup"><span data-stu-id="62336-140">This feature is a stepping stone to support many-to-many relationships without a join entity, which is one of the most requested improvements for EF Core.</span></span>

## <a name="ef-63-on-net-core"></a><span data-ttu-id="62336-141">EF 6.3 in .NET Core</span><span class="sxs-lookup"><span data-stu-id="62336-141">EF 6.3 on .NET Core</span></span> 

[<span data-ttu-id="62336-142">Problema EF6 n. 271</span><span class="sxs-lookup"><span data-stu-id="62336-142">Tracking Issue EF6#271</span></span>](https://github.com/aspnet/EntityFramework6/issues/271)

<span data-ttu-id="62336-143">Il lavoro su questa funzionalità è iniziato ma non è incluso nell'anteprima corrente.</span><span class="sxs-lookup"><span data-stu-id="62336-143">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="62336-144">siamo consapevoli che molte applicazioni esistenti usano versioni precedenti di EF e che la loro conversione per EF Core al solo scopo di sfruttare i vantaggi di .NET Core può talvolta richiedere notevoli sforzi.</span><span class="sxs-lookup"><span data-stu-id="62336-144">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="62336-145">Per questo motivo, la prossima versione di EF 6 verrà adattata per supportare l'esecuzione in .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="62336-145">For that reason, we will be adapting the next version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="62336-146">Lo scopo è quello di facilitare la conversione delle applicazioni esistenti con modifiche minime.</span><span class="sxs-lookup"><span data-stu-id="62336-146">We are doing this to facilitate porting existing applications with minimal changes.</span></span>
<span data-ttu-id="62336-147">Sono previste alcune limitazioni.</span><span class="sxs-lookup"><span data-stu-id="62336-147">There are going to be some limitations.</span></span> <span data-ttu-id="62336-148">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="62336-148">For example:</span></span>
- <span data-ttu-id="62336-149">Saranno necessari nuovi provider per lavorare con altri database oltre al supporto per SQL Server incluso in .NET Core</span><span class="sxs-lookup"><span data-stu-id="62336-149">It will require new providers to work with other databases besides the included SQL Server support on .NET Core</span></span>
- <span data-ttu-id="62336-150">Il supporto spaziale con SQL Server non sarà attivato</span><span class="sxs-lookup"><span data-stu-id="62336-150">Spatial support with SQL Server won't be enabled</span></span>

<span data-ttu-id="62336-151">Si noti anche che in questa fase non sono previste nuove funzionalità per EF 6.</span><span class="sxs-lookup"><span data-stu-id="62336-151">Note also that there are no new features planned for EF 6 at this point.</span></span>
