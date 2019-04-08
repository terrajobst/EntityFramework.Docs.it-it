---
title: Modifiche che causano un'interruzione in EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: fd593b2832a5a6ffe27cd4493127b5d405f684ba
ms.sourcegitcommit: ce44f85a5bce32ef2d3d09b7682108d3473511b3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/04/2019
ms.locfileid: "58914127"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="cfe0d-102">Modifiche che causano un'interruzione incluse in EF Core 3.0 (attualmente in anteprima)</span><span class="sxs-lookup"><span data-stu-id="cfe0d-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cfe0d-103">Si tenga presente che i set di funzionalità e le pianificazioni delle versioni future sono sempre soggette a modifiche e che questa pagina, nonostante l'impegno profuso per mantenerla aggiornata, potrebbe non riflettere sempre i piani più recenti.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="cfe0d-104">Le modifiche all'API e al comportamento seguenti possono potenzialmente interrompere le applicazioni sviluppate per EF Core 2.2.x durante l'aggiornamento alla versione 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="cfe0d-105">Le modifiche che si prevede abbiano impatto solo sui provider di database sono documentate nelle [modifiche che influiscono sul provider](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="cfe0d-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="cfe0d-106">Le interruzioni nelle nuove funzionalità introdotte da un'anteprima 3.0 a un'altra anteprima 3.0 non sono documentate.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="cfe0d-107">Le query LINQ non vengono più valutate nel client</span><span class="sxs-lookup"><span data-stu-id="cfe0d-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="cfe0d-108">[Problema n. 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Vedere anche il problema n. 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="cfe0d-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="cfe0d-109">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="cfe0d-110">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-110">Old behavior</span></span>**

<span data-ttu-id="cfe0d-111">Nelle versioni precedenti alla versione 3.0, quando EF Core non era in grado di convertire un'espressione inclusa in una query in SQL o in un parametro, l'espressione veniva automaticamente valutata nel client.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="cfe0d-112">Per impostazione predefinita, la valutazione client di espressioni potenzialmente dispendiose si limitava ad attivare solo un avviso.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

**<span data-ttu-id="cfe0d-113">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-113">New behavior</span></span>**

<span data-ttu-id="cfe0d-114">A partire dalla versione 3.0, EF Core consente solo la valutazione delle espressioni nella proiezione di primo livello (l'ultima chiamata `Select()` nella query) nel client.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="cfe0d-115">Quando le espressioni in altre posizioni all'interno della query non possono essere convertite in SQL o in un parametro, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

**<span data-ttu-id="cfe0d-116">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-116">Why</span></span>**

<span data-ttu-id="cfe0d-117">La valutazione client automatica delle query consente di eseguire numerose query anche nel caso in cui parti importanti delle query non possono essere convertite.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="cfe0d-118">Questo comportamento può causare un comportamento imprevisto e potenzialmente dannoso che può diventare evidente solo in produzione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="cfe0d-119">Ad esempio, una condizione in una chiamata `Where()` che non può essere convertita può causare il trasferimento di tutte le righe della tabella del server di database e l'applicazione del filtro nel client.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="cfe0d-120">È probabile che questa situazione non venga rilevata se la tabella contiene solo alcune righe in fase di sviluppo, ma che abbia un grande impatto quando l'applicazione passa in produzione dove la tabella può contenere milioni di righe.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="cfe0d-121">Gli avvisi di valutazione client inoltre si sono rivelati molto facili da ignorare durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="cfe0d-122">Inoltre, la valutazione client automatica può causare problemi in cui il miglioramento della conversione di query per espressioni specifiche causa modifiche impreviste che causano un'interruzione da una versione all'altra.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

**<span data-ttu-id="cfe0d-123">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-123">Mitigations</span></span>**

<span data-ttu-id="cfe0d-124">Se una query non può essere convertita completamente, riscrivere la query in un formato che possa essere convertito o usare `AsEnumerable()`, `ToList()` o un elemento simile per riportare in modo esplicito i dati nel client dove possono essere quindi ulteriormente elaborati usando LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="cfe0d-125">Entity Framework Core non è più incluso nel framework condiviso di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cfe0d-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="cfe0d-126">Annunci problema n. 325</span><span class="sxs-lookup"><span data-stu-id="cfe0d-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="cfe0d-127">Questa modifica è stata introdotta in ASP.NET Core 3.0 anteprima 1.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

**<span data-ttu-id="cfe0d-128">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-128">Old behavior</span></span>**

<span data-ttu-id="cfe0d-129">Nelle versioni precedenti ad ASP.NET Core 3.0, quando si aggiungeva un riferimento a un pacchetto in `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, veniva inserito EF Core e alcuni dei provider di dati di EF Core come il provider di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

**<span data-ttu-id="cfe0d-130">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-130">New behavior</span></span>**

<span data-ttu-id="cfe0d-131">A partire dalla versione 3.0, il framework condiviso di ASP.NET Core non include EF Core o provider di dati di EF Core.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

**<span data-ttu-id="cfe0d-132">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-132">Why</span></span>**

<span data-ttu-id="cfe0d-133">Prima di questa modifica, per ottenere EF Core erano necessarie procedure diverse, a seconda che l'applicazione avesse o meno come destinazione ASP.NET Core e SQL Server.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="cfe0d-134">Inoltre, l'aggiornamento di ASP.NET Core forzava l'aggiornamento di EF Core e del provider di SQL Server, non sempre auspicabile.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="cfe0d-135">Con questa modifica, la procedura per ottenere EF Core è la stessa in tutti i provider, le implementazioni .NET supportate e i tipi di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="cfe0d-136">Gli sviluppatori possono ora controllare anche in modo preciso quando vengono aggiornati EF Core e i provider di dati di EF Core.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

**<span data-ttu-id="cfe0d-137">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-137">Mitigations</span></span>**

<span data-ttu-id="cfe0d-138">Per usare EF Core in un'applicazione ASP.NET Core 3.0 o in un'altra applicazione supportata, aggiungere in modo esplicito un riferimento al pacchetto al provider di database di EF Core che verrà usato dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="cfe0d-139">I metodi FromSql, ExecuteSql ed ExecuteSqlAsync sono stati rinominati</span><span class="sxs-lookup"><span data-stu-id="cfe0d-139">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="cfe0d-140">Problema n. 10996</span><span class="sxs-lookup"><span data-stu-id="cfe0d-140">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="cfe0d-141">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-141">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="cfe0d-142">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-142">Old behavior</span></span>**

<span data-ttu-id="cfe0d-143">Prima di EF Core 3.0, erano disponibili overload per questi nomi di metodo per supportare l'uso con una stringa normale o una stringa che deve essere interpolata in SQL e parametri.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-143">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

**<span data-ttu-id="cfe0d-144">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-144">New behavior</span></span>**

<span data-ttu-id="cfe0d-145">A partire da EF Core 3.0, usare `FromSqlRaw`, `ExecuteSqlRaw` e `ExecuteSqlRawAsync` per creare una query con parametri in cui i parametri vengono passati separatamente dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-145">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="cfe0d-146">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-146">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="cfe0d-147">Usare `FromSqlInterpolated`, `ExecuteSqlInterpolated`, e `ExecuteSqlInterpolatedAsync` per creare una query con parametri in cui i parametri vengono passati come parte di una stringa di query interpolata.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-147">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="cfe0d-148">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-148">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="cfe0d-149">Si noti che entrambe le query precedenti produrranno lo stesso codice SQL con parametri con gli stessi parametri SQL.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-149">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

**<span data-ttu-id="cfe0d-150">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-150">Why</span></span>**

<span data-ttu-id="cfe0d-151">Con gli overload di metodi come questi, è molto semplice chiamare accidentalmente il metodo con stringa non elaborata anche se l'intento era chiamare il metodo con stringa interpolata e viceversa.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-151">Method overloads like this make it very easy to accidentally call the raw srting method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="cfe0d-152">Il risultato potrebbero essere query senza parametri, quando invece è prevista la parametrizzazione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-152">This could result in queries not being parameterized when they should have been.</span></span>

**<span data-ttu-id="cfe0d-153">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-153">Mitigations</span></span>**

<span data-ttu-id="cfe0d-154">Passare all'uso dei nuovi nomi di metodo.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-154">Switch to use the new method names.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="cfe0d-155">L'esecuzione di query viene registrata a livello di debug</span><span class="sxs-lookup"><span data-stu-id="cfe0d-155">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="cfe0d-156">Problema n. 14523</span><span class="sxs-lookup"><span data-stu-id="cfe0d-156">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="cfe0d-157">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-157">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="cfe0d-158">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-158">Old behavior</span></span>**

<span data-ttu-id="cfe0d-159">Nelle versioni precedenti a EF Core 3.0 l'esecuzione di query e altri comandi veniva registrata a livello `Info`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-159">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

**<span data-ttu-id="cfe0d-160">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-160">New behavior</span></span>**

<span data-ttu-id="cfe0d-161">A partire da EF Core 3.0, la registrazione dell'esecuzione di comandi/SQL è a livello `Debug`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-161">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

**<span data-ttu-id="cfe0d-162">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-162">Why</span></span>**

<span data-ttu-id="cfe0d-163">Questa modifica è stata apportata per ridurre i disturbi a livello di registrazione `Info`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-163">This change was made to reduce the noise at the `Info` log level.</span></span>

**<span data-ttu-id="cfe0d-164">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-164">Mitigations</span></span>**

<span data-ttu-id="cfe0d-165">Questo evento di registrazione è definito da `RelationalEventId.CommandExecuting` con l'ID evento 20100.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-165">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="cfe0d-166">Per registrare SQL di nuovo al livello `Info`, configurare in modo esplicito il livello in `OnConfiguring` o `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-166">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="cfe0d-167">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-167">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="cfe0d-168">I valori di chiave temporanei non sono più impostati nelle istanze di entità</span><span class="sxs-lookup"><span data-stu-id="cfe0d-168">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="cfe0d-169">Problema n. 12378</span><span class="sxs-lookup"><span data-stu-id="cfe0d-169">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="cfe0d-170">Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-170">This change was introduced in EF Core 3.0-preview 2.</span></span>

**<span data-ttu-id="cfe0d-171">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-171">Old behavior</span></span>**

<span data-ttu-id="cfe0d-172">Nelle versioni precedenti a EF Core 3.0 i valori temporanei venivano assegnati a tutte le proprietà di chiave per cui veniva in seguito generato un valore reale dal database.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-172">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="cfe0d-173">In genere questi valori temporanei erano numeri negativi elevati.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-173">Usually these temporary values were large negative numbers.</span></span>

**<span data-ttu-id="cfe0d-174">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-174">New behavior</span></span>**

<span data-ttu-id="cfe0d-175">A partire dalla versione 3.0, EF Core archivia il valore di chiave temporaneo come parte delle informazioni di rilevamento dell'entità e non modifica la proprietà di chiave.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-175">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

**<span data-ttu-id="cfe0d-176">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-176">Why</span></span>**

<span data-ttu-id="cfe0d-177">Questa modifica è stata apportata per impedire che i valori di chiave temporanei diventino erroneamente permanenti quando un'entità rilevata in precedenza da un'istanza `DbContext` viene spostata in un'altra istanza `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-177">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

**<span data-ttu-id="cfe0d-178">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-178">Mitigations</span></span>**

<span data-ttu-id="cfe0d-179">Le applicazioni che assegnano valori di chiave primaria in chiavi esterne per creare associazioni tra le entità possono dipendere dal comportamento precedente se le chiavi primarie vengono generate dall'archivio e appartengono a entità con stato `Added`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-179">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="cfe0d-180">Questo può essere evitato:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-180">This can be avoided by:</span></span>
* <span data-ttu-id="cfe0d-181">Non usando chiavi generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-181">Not using store-generated keys.</span></span>
* <span data-ttu-id="cfe0d-182">Impostando le proprietà di navigazione in modo da creare relazioni anziché impostando valori di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-182">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="cfe0d-183">Ottenendo i valori di chiave temporanei effettivi dalle informazioni di rilevamento dell'entità.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-183">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="cfe0d-184">Ad esempio, `context.Entry(blog).Property(e => e.Id).CurrentValue` restituisce il valore temporaneo anche quando `blog.Id` non è stato impostato.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-184">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="cfe0d-185">DetectChanges rispetta i valori di chiave generati dall'archivio</span><span class="sxs-lookup"><span data-stu-id="cfe0d-185">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="cfe0d-186">Problema n. 14616</span><span class="sxs-lookup"><span data-stu-id="cfe0d-186">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="cfe0d-187">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="cfe0d-188">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-188">Old behavior</span></span>**

<span data-ttu-id="cfe0d-189">Nelle versioni precedenti a EF Core 3.0 un'entità non rilevata individuata da `DetectChanges` veniva rilevata nello stato `Added` e inserita come nuova riga quando veniva eseguita una chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-189">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

**<span data-ttu-id="cfe0d-190">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-190">New behavior</span></span>**

<span data-ttu-id="cfe0d-191">A partire da EF Core 3.0, se un'entità usa valori di chiave generati e viene impostato un valore di chiave, l'entità viene rilevata nello stato `Modified`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-191">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="cfe0d-192">Ciò significa che si presuppone l'esistenza di una riga per l'entità che viene aggiornata quando viene eseguita una chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-192">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="cfe0d-193">Se il valore di chiave non viene impostato o se il tipo di entità non usa chiavi generate, la nuova entità viene rilevata come `Added` come nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-193">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

**<span data-ttu-id="cfe0d-194">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-194">Why</span></span>**

<span data-ttu-id="cfe0d-195">Questa modifica è stata apportata per rendere più semplice e coerente l'uso di grafici di entità disconnesse con chiavi generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-195">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

**<span data-ttu-id="cfe0d-196">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-196">Mitigations</span></span>**

<span data-ttu-id="cfe0d-197">Questa modifica può interrompere un'applicazione se un tipo di entità è configurato per l'uso di chiavi generate ma i valori di chiave sono impostati in modo esplicito per le nuove istanze.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-197">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="cfe0d-198">La correzione consiste nel configurare in modo esplicito le proprietà di chiave per non usare valori generati.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-198">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="cfe0d-199">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-199">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="cfe0d-200">Oppure con annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-200">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="cfe0d-201">Le eliminazioni a catena vengono ora eseguite immediatamente per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="cfe0d-201">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="cfe0d-202">Problema n. 10114</span><span class="sxs-lookup"><span data-stu-id="cfe0d-202">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="cfe0d-203">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-203">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="cfe0d-204">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-204">Old behavior</span></span>**

<span data-ttu-id="cfe0d-205">Nelle versioni precedenti alla versione 3.0, EF Core applicava azioni a catena (eliminazione delle entità dipendenti quando veniva eliminata un'entità di sicurezza obbligatoria o veniva recisa la relazione con un'entità di sicurezza obbligatoria) solo dopo la chiamata a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-205">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

**<span data-ttu-id="cfe0d-206">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-206">New behavior</span></span>**

<span data-ttu-id="cfe0d-207">A partire dalla versione 3.0, EF Core applica le azioni a catena non appena viene rilevata la condizione di attivazione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-207">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="cfe0d-208">Ad esempio, la chiamata a `context.Remove()` per eliminare un'entità di sicurezza causa anche l'impostazione immediata di tutti i dipendenti obbligatori correlati rilevati su `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-208">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

**<span data-ttu-id="cfe0d-209">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-209">Why</span></span>**

<span data-ttu-id="cfe0d-210">Questa modifica è stata apportata per migliorare l'esperienza di associazione di dati e degli scenari di controllo in cui è importante individuare le entità che verranno eliminate _prima_ della chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-210">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

**<span data-ttu-id="cfe0d-211">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-211">Mitigations</span></span>**

<span data-ttu-id="cfe0d-212">Il comportamento precedente può essere ripristinato tramite le impostazioni in `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-212">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="cfe0d-213">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-213">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="cfe0d-214">I tipi di query vengono consolidati con tipi di entità</span><span class="sxs-lookup"><span data-stu-id="cfe0d-214">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="cfe0d-215">Problema n. 14194</span><span class="sxs-lookup"><span data-stu-id="cfe0d-215">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="cfe0d-216">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-216">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="cfe0d-217">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-217">Old behavior</span></span>**

<span data-ttu-id="cfe0d-218">Nelle versioni precedenti a EF Core 3.0 i [tipi di query](xref:core/modeling/query-types) erano uno strumento per eseguire query su dati che non definiscono una chiave primaria in modo strutturato.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-218">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="cfe0d-219">Veniva infatti usato un tipo di query per eseguire il mapping di tipi di entità senza chiavi (più probabilmente da una vista, ma anche da una tabella), mentre veniva usato un tipo di entità normale quando era disponibile una chiave (più probabilmente da una tabella, ma anche da una vista).</span><span class="sxs-lookup"><span data-stu-id="cfe0d-219">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

**<span data-ttu-id="cfe0d-220">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-220">New behavior</span></span>**

<span data-ttu-id="cfe0d-221">Un tipo di query diventa ora semplicemente un tipo di entità senza chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-221">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="cfe0d-222">I tipi di entità senza chiave hanno la stessa funzionalità dei tipi di query nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-222">Keyless entity types have the same functionality as query types in previous versions.</span></span>

**<span data-ttu-id="cfe0d-223">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-223">Why</span></span>**

<span data-ttu-id="cfe0d-224">Questa modifica è stata apportata per ridurre la confusione riguardo lo scopo dei tipi di query.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-224">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="cfe0d-225">In particolare, si tratta di tipi di entità senza chiave che sono intrinsecamente di sola lettura per questo motivo ma non dovrebbero essere usati solo perché un tipo di entità deve essere di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-225">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="cfe0d-226">Analogamente, spesso vengono mappati alle viste solo perché le viste spesso non definiscono le chiavi.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-226">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

**<span data-ttu-id="cfe0d-227">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-227">Mitigations</span></span>**

<span data-ttu-id="cfe0d-228">Le parti dell'API seguenti sono ora obsolete:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-228">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="cfe0d-229">**`ModelBuilder.Query<>()`** - È necessario chiamare `ModelBuilder.Entity<>().HasNoKey()` per contrassegnare un tipo di entità come tipo senza chiavi.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-229">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="cfe0d-230">Non ne viene eseguita la configurazione per convenzione per evitare una configurazione errata quando è prevista una chiave primaria che tuttavia non corrisponde alla convenzione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-230">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="cfe0d-231">**`DbQuery<>`** - Usare `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-231">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="cfe0d-232">**`DbContext.Query<>()`** - Usare `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-232">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="cfe0d-233">L'API di configurazione per le relazioni di tipo di proprietà è stata modificata</span><span class="sxs-lookup"><span data-stu-id="cfe0d-233">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="cfe0d-234">[Problema n. 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema n. 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema n. 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="cfe0d-234">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="cfe0d-235">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-235">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="cfe0d-236">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-236">Old behavior</span></span>**

<span data-ttu-id="cfe0d-237">Nelle versioni precedenti a EF Core 3.0 la configurazione della relazione di proprietà veniva eseguita direttamente dopo la chiamata a `OwnsOne` o `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-237">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

**<span data-ttu-id="cfe0d-238">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-238">New behavior</span></span>**

<span data-ttu-id="cfe0d-239">A partire da EF Core 3.0, è disponibile l'API Fluent per configurare una proprietà di navigazione per il proprietario usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-239">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="cfe0d-240">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-240">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="cfe0d-241">La configurazione correlata alla relazione tra proprietario e elemento di proprietà deve essere ora concatenata dopo `WithOwner()` in modo analogo a come vengono configurate altre relazioni.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-241">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="cfe0d-242">La configurazione per il tipo di proprietà viene comunque concatenata dopo `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-242">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="cfe0d-243">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-243">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

<span data-ttu-id="cfe0d-244">Inoltre, la chiamata a `Entity()`, `HasOne()` o `Set()` con una destinazione di tipo di proprietà genera ora un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-244">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

**<span data-ttu-id="cfe0d-245">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-245">Why</span></span>**

<span data-ttu-id="cfe0d-246">Questa modifica è stata apportata per creare una separazione più netta tra la configurazione del tipo di proprietà e la _relazione_ con il tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-246">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="cfe0d-247">Ciò consente di eliminare ambiguità e confusione su metodi come `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-247">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

**<span data-ttu-id="cfe0d-248">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-248">Mitigations</span></span>**

<span data-ttu-id="cfe0d-249">Modificare la configurazione delle relazioni dei tipi di proprietà per usare la superficie della nuova API come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-249">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="cfe0d-250">Le entità dipendenti che condividono la tabella con l'entità di sicurezza sono ora facoltative</span><span class="sxs-lookup"><span data-stu-id="cfe0d-250">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="cfe0d-251">Problema n. 9005</span><span class="sxs-lookup"><span data-stu-id="cfe0d-251">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="cfe0d-252">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-252">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="cfe0d-253">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-253">Old behavior</span></span>**

<span data-ttu-id="cfe0d-254">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-254">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```
<span data-ttu-id="cfe0d-255">Prima di EF Core 3.0, se `OrderDetails` è di proprietà di `Order` o viene mappato in modo esplicito alla stessa tabella, era sempre necessaria un'istanza di `OrderDetails` per l'aggiunta di un nuovo `Order`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-255">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


**<span data-ttu-id="cfe0d-256">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-256">New behavior</span></span>**

<span data-ttu-id="cfe0d-257">A partire dalla versione 3.0, EF Core consente di aggiungere un `Order` senza un `OrderDetails` ed esegue il mapping di tutte le proprietà di `OrderDetails`, tranne che la chiave primaria, a colonne che ammettono valori Null.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-257">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="cfe0d-258">In fase di query, EF Core imposta `OrderDetails` su `null` se una delle relative proprietà obbligatorie non ha un valore o se non sono presenti proprietà obbligatorie oltre alla chiave primaria e tutte le proprietà sono `null`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-258">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

**<span data-ttu-id="cfe0d-259">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-259">Mitigations</span></span>**

<span data-ttu-id="cfe0d-260">Se il modello include una tabella condivisa dipendente con tutte le colonne facoltative, ma è previsto che la navigazione che punta a essa non sia `null`, l'applicazione deve essere modificata per gestire casi in cui la navigazione è `null`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-260">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="cfe0d-261">Se questo non è possibile, è consigliabile aggiungere una proprietà obbligatoria al tipo di entità o assegnare un valore non `null` ad almeno una proprietà.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-261">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="cfe0d-262">Tutte le entità che condividono una tabella con una colonna di token di concorrenza devono eseguirne il mapping a una proprietà</span><span class="sxs-lookup"><span data-stu-id="cfe0d-262">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="cfe0d-263">Problema n. 14154</span><span class="sxs-lookup"><span data-stu-id="cfe0d-263">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="cfe0d-264">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-264">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="cfe0d-265">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-265">Old behavior</span></span>**

<span data-ttu-id="cfe0d-266">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-266">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
<span data-ttu-id="cfe0d-267">Prima di EF Core 3.0, se `OrderDetails` è di proprietà di `Order` o è mappato in modo esplicito alla stessa tabella, il solo aggiornamento di `OrderDetails` non aggiornerà il valore `Version` nel client e l'aggiornamento successivo avrà esito negativo.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-267">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


**<span data-ttu-id="cfe0d-268">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-268">New behavior</span></span>**

<span data-ttu-id="cfe0d-269">A partire dalla versione 3.0, EF Core propaga il nuovo valore `Version` a `Order` se è proprietario di `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-269">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="cfe0d-270">In caso contrario, viene generata un'eccezione durante la convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-270">Otherwise an exception is thrown during model validation.</span></span>

**<span data-ttu-id="cfe0d-271">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-271">Why</span></span>**

<span data-ttu-id="cfe0d-272">Questa modifica è stata apportata per evitare un valore del token di concorrenza non aggiornato quando viene aggiornata solo una delle entità mappate alla stessa tabella.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-272">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

**<span data-ttu-id="cfe0d-273">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-273">Mitigations</span></span>**

<span data-ttu-id="cfe0d-274">Tutte le entità che condividono la tabella devono includere una proprietà mappata alla colonna del token di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-274">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="cfe0d-275">È possibile crearne una in stato shadow:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-275">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="cfe0d-276">Per le proprietà ereditate da tipi senza mapping viene ora eseguito il mapping a una singola colonna per tutti i tipi derivati</span><span class="sxs-lookup"><span data-stu-id="cfe0d-276">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="cfe0d-277">Problema n. 13998</span><span class="sxs-lookup"><span data-stu-id="cfe0d-277">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="cfe0d-278">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-278">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="cfe0d-279">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-279">Old behavior</span></span>**

<span data-ttu-id="cfe0d-280">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-280">Consider the following model:</span></span>
```C#
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

<span data-ttu-id="cfe0d-281">Prima di EF Core 3.0, per la proprietà `ShippingAddress` sarebbe stato eseguito il mapping a colonne separate per `BulkOrder` e `Order` per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-281">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

**<span data-ttu-id="cfe0d-282">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-282">New behavior</span></span>**

<span data-ttu-id="cfe0d-283">A partire dalla versione 3.0, EF Core crea solo una colonna per `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-283">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

**<span data-ttu-id="cfe0d-284">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-284">Why</span></span>**

<span data-ttu-id="cfe0d-285">Il comportamento precedente non era previsto.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-285">The old behavoir was unexpected.</span></span>

**<span data-ttu-id="cfe0d-286">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-286">Mitigations</span></span>**

<span data-ttu-id="cfe0d-287">È ancora possibile eseguire il mapping esplicito della proprietà a una colonna separata per i tipi derivati:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-287">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="cfe0d-288">La convenzione di proprietà di chiave esterna non ha più lo stesso nome della proprietà dell'entità di sicurezza</span><span class="sxs-lookup"><span data-stu-id="cfe0d-288">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="cfe0d-289">Problema n. 13274</span><span class="sxs-lookup"><span data-stu-id="cfe0d-289">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="cfe0d-290">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-290">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="cfe0d-291">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-291">Old behavior</span></span>**

<span data-ttu-id="cfe0d-292">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-292">Consider the following model:</span></span>
```C#
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```
<span data-ttu-id="cfe0d-293">Nelle versioni precedenti a EF Core 3.0 veniva usata la proprietà `CustomerId` per la chiave esterna per convenzione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-293">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="cfe0d-294">Tuttavia, se `Order` è un tipo di proprietà, `CustomerId` sarebbe la chiave primaria e ciò non è in genere auspicabile.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-294">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

**<span data-ttu-id="cfe0d-295">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-295">New behavior</span></span>**

<span data-ttu-id="cfe0d-296">A partire da 3.0, EF Core non tenta di usare le proprietà per le chiavi esterne per convenzione se hanno lo stesso nome della proprietà dell'entità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-296">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="cfe0d-297">Viene ancora eseguita la corrispondenza tra i criteri del nome del tipo dell'entità di sicurezza concatenato al nome della proprietà dell'entità di sicurezza e il nome di navigazione concatenato al nome della proprietà dell'entità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-297">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="cfe0d-298">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-298">For example:</span></span>

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

**<span data-ttu-id="cfe0d-299">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-299">Why</span></span>**

<span data-ttu-id="cfe0d-300">Questa modifica è stata apportata per evitare una definizione errata della proprietà di chiave primaria nel tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-300">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

**<span data-ttu-id="cfe0d-301">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-301">Mitigations</span></span>**

<span data-ttu-id="cfe0d-302">Se la proprietà è stata progettata per essere la chiave esterna e di conseguenza parte della chiave primaria, configurarla in modo esplicito come chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-302">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="cfe0d-303">La connessione al database viene ora chiusa se non viene più usata prima del completamento di TransactionScope</span><span class="sxs-lookup"><span data-stu-id="cfe0d-303">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="cfe0d-304">Problema n. 14218</span><span class="sxs-lookup"><span data-stu-id="cfe0d-304">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="cfe0d-305">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-305">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="cfe0d-306">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-306">Old behavior</span></span>**

<span data-ttu-id="cfe0d-307">Prima di EF Core 3.0, se il contesto apre la connessione all'interno di un `TransactionScope`, la connessione rimane aperta mentre è attivo il `TransactionScope` corrente.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-307">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point
        
        var categories = context.ProductCategories().ToList();
    }
}
```

**<span data-ttu-id="cfe0d-308">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-308">New behavior</span></span>**

<span data-ttu-id="cfe0d-309">A partire dalla versione 3.0, EF Core chiude la connessione non appena non viene più usata.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-309">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

**<span data-ttu-id="cfe0d-310">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-310">Why</span></span>**

<span data-ttu-id="cfe0d-311">Questa modifica consente di usare più contesti nello stesso `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-311">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="cfe0d-312">Il nuovo comportamento corrisponde anche a EF6.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-312">The new behavior alose matches EF6.</span></span>

**<span data-ttu-id="cfe0d-313">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-313">Mitigations</span></span>**

<span data-ttu-id="cfe0d-314">Se la connessione deve rimanere aperta, la chiamata esplicita di `OpenConnection()` garantirà che EF Core non la chiuda prematuramente:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-314">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();
        
        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="cfe0d-315">Ogni proprietà usa la generazione di chiavi di tipo intero in memoria indipendenti</span><span class="sxs-lookup"><span data-stu-id="cfe0d-315">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="cfe0d-316">Problema n. 6872</span><span class="sxs-lookup"><span data-stu-id="cfe0d-316">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="cfe0d-317">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-317">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="cfe0d-318">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-318">Old behavior</span></span>**

<span data-ttu-id="cfe0d-319">Nelle versioni precedenti a EF Core 3.0 veniva usato un unico generatore di valori condiviso per tutte le proprietà di chiavi di tipo intero in memoria.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-319">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

**<span data-ttu-id="cfe0d-320">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-320">New behavior</span></span>**

<span data-ttu-id="cfe0d-321">A partire da EF Core 3.0, ogni proprietà di chiave di tipo intero riceve un generatore di valori quando viene usato il database in memoria.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-321">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="cfe0d-322">Inoltre, se il database viene eliminato, la generazione di chiavi viene reimpostata per tutte le tabelle.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-322">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

**<span data-ttu-id="cfe0d-323">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-323">Why</span></span>**

<span data-ttu-id="cfe0d-324">Questa modifica è stata apportata per allineare maggiormente la generazione di chiavi in memoria alla generazione di chiavi del database reale e per migliorare la possibilità di isolare i test l'uno dall'altro quando viene usato il database in memoria.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-324">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

**<span data-ttu-id="cfe0d-325">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-325">Mitigations</span></span>**

<span data-ttu-id="cfe0d-326">Ciò può interrompere un'applicazione che si basa sull'impostazione di valori di chiave in memoria specifici.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-326">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="cfe0d-327">È consigliabile non basare l'applicazione su valori di chiave specifici o eseguire l'aggiornamento per passare al nuovo comportamento.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-327">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="cfe0d-328">I campi sottostanti vengono usati per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="cfe0d-328">Backing fields are used by default</span></span>

[<span data-ttu-id="cfe0d-329">Problema n. 12430</span><span class="sxs-lookup"><span data-stu-id="cfe0d-329">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="cfe0d-330">Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-330">This change was introduced in EF Core 3.0-preview 2.</span></span>

**<span data-ttu-id="cfe0d-331">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-331">Old behavior</span></span>**

<span data-ttu-id="cfe0d-332">Nelle versioni precedenti alla versione 3.0, anche se il campo sottostante di una proprietà era noto, per impostazione predefinita EF Core eseguiva la lettura e la scrittura del valore della proprietà usando i metodi getter e setter della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-332">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="cfe0d-333">L'eccezione era costituita dall'esecuzione di query in cui il campo sottostante, se noto, veniva impostato direttamente.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-333">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

**<span data-ttu-id="cfe0d-334">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-334">New behavior</span></span>**

<span data-ttu-id="cfe0d-335">A partire da EF Core 3.0, se il campo sottostante di una proprietà è noto, la lettura e la scrittura della proprietà vengono sempre eseguite usando il campo sottostante.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-335">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="cfe0d-336">Ciò potrebbe causare un'interruzione dell'applicazione se l'applicazione si basa su un comportamento aggiuntivo codificato nei metodi getter o setter.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-336">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

**<span data-ttu-id="cfe0d-337">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-337">Why</span></span>**

<span data-ttu-id="cfe0d-338">Questa modifica è stata apportata per impedire a EF Core di attivare per errore la logica di business per impostazione predefinita quando si eseguono operazioni di database che interessano le entità.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-338">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

**<span data-ttu-id="cfe0d-339">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-339">Mitigations</span></span>**

<span data-ttu-id="cfe0d-340">È possibile ripristinare il comportamento delle versioni precedenti alla versione 3.0 tramite la configurazione della modalità di accesso delle proprietà nell'API Fluent modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-340">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="cfe0d-341">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-341">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="cfe0d-342">Viene generata un'eccezione se vengono trovati più campi sottostanti compatibili</span><span class="sxs-lookup"><span data-stu-id="cfe0d-342">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="cfe0d-343">Problema n. 12523</span><span class="sxs-lookup"><span data-stu-id="cfe0d-343">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="cfe0d-344">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-344">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="cfe0d-345">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-345">Old behavior</span></span>**

<span data-ttu-id="cfe0d-346">Nelle versioni precedenti a EF Core 3.0, se più campi soddisfacevano le regole di ricerca del campo sottostante di una proprietà, veniva selezionato un solo campo in base a un ordine di precedenza.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-346">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="cfe0d-347">Ciò poteva causare l'uso di un campo non corretto nei casi ambigui.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-347">This could cause the wrong field to be used in ambiguous cases.</span></span>

**<span data-ttu-id="cfe0d-348">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-348">New behavior</span></span>**

<span data-ttu-id="cfe0d-349">A partire da EF Core 3.0, se più campi corrispondono alla stessa proprietà, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-349">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

**<span data-ttu-id="cfe0d-350">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-350">Why</span></span>**

<span data-ttu-id="cfe0d-351">Questa modifica è stata apportata per evitare di usare automaticamente un campo rispetto a un altro quando un solo campo può essere quello corretto.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-351">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

**<span data-ttu-id="cfe0d-352">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-352">Mitigations</span></span>**

<span data-ttu-id="cfe0d-353">Per le proprietà con campi sottostanti ambigui, il campo da usare deve essere specificato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-353">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="cfe0d-354">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-354">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="cfe0d-355">AddDbContext/AddDbContextPool non chiamano più AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="cfe0d-355">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="cfe0d-356">Problema n. 14756</span><span class="sxs-lookup"><span data-stu-id="cfe0d-356">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="cfe0d-357">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-357">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="cfe0d-358">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-358">Old behavior</span></span>**

<span data-ttu-id="cfe0d-359">Prima di EF Core 3.0, la chiamata di `AddDbContext` oppure `AddDbContextPool` comporta anche la registrazione dei servizi di registrazione e memorizzazione nella cache con inserimento delle dipendenze tramite chiamate a [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="cfe0d-359">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

**<span data-ttu-id="cfe0d-360">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-360">New behavior</span></span>**

<span data-ttu-id="cfe0d-361">A partire da EF Core 3.0, `AddDbContext` e `AddDbContextPool` non registreranno più questi servizi con inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-361">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

**<span data-ttu-id="cfe0d-362">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-362">Why</span></span>**

<span data-ttu-id="cfe0d-363">EF Core 3.0 non richiede che questi servizi siano inclusi nel contenitore di inserimento delle dipendenze dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-363">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="cfe0d-364">Tuttavia, se `ILoggerFactory` è registrato nel contenitore di inserimento delle dipendenze dell'applicazione, verrà ancora usato da EF Core.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-364">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

**<span data-ttu-id="cfe0d-365">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-365">Mitigations</span></span>**

<span data-ttu-id="cfe0d-366">Se l'applicazione necessita di questi servizi, registrarli in modo esplicito con il contenitore di inserimento delle dipendenze usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="cfe0d-366">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="cfe0d-367">DbContext.Entry esegue ora un DetectChanges locale</span><span class="sxs-lookup"><span data-stu-id="cfe0d-367">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="cfe0d-368">Problema n. 13552</span><span class="sxs-lookup"><span data-stu-id="cfe0d-368">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="cfe0d-369">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-369">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="cfe0d-370">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-370">Old behavior</span></span>**

<span data-ttu-id="cfe0d-371">Nelle versioni precedenti a EF Core 3.0 la chiamata a `DbContext.Entry` causava il rilevamento delle modifiche per tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-371">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="cfe0d-372">Ciò garantiva l'aggiornamento dello stato esposto in `EntityEntry`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-372">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

**<span data-ttu-id="cfe0d-373">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-373">New behavior</span></span>**

<span data-ttu-id="cfe0d-374">A partire da EF Core 3.0, la chiamata a `DbContext.Entry` causa ora solo il tentativo di rilevare le modifiche nell'entità specificata e in tutte le relative entità di sicurezza rilevate.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-374">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="cfe0d-375">Ciò significa che le modifiche apportate altrove potrebbero non essere state rilevate tramite la chiamata al metodo e ciò potrebbe avere implicazioni sullo stato dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-375">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="cfe0d-376">Si noti che se `ChangeTracker.AutoDetectChangesEnabled` è impostato su `false`, verrà disabilitato anche questo tipo di rilevamento delle modifiche locali.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-376">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="cfe0d-377">Gli altri metodi che causano il rilevamento delle modifiche, ad esempio `ChangeTracker.Entries` e `SaveChanges`, causano ancora un `DetectChanges` completo di tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-377">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

**<span data-ttu-id="cfe0d-378">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-378">Why</span></span>**

<span data-ttu-id="cfe0d-379">Questa modifica è stata apportata per migliorare le prestazioni predefinite dell'uso di `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-379">This change was made to improve the default performance of using `context.Entry`.</span></span>

**<span data-ttu-id="cfe0d-380">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-380">Mitigations</span></span>**

<span data-ttu-id="cfe0d-381">Chiamare `ChgangeTracker.DetectChanges()` in modo esplicito prima di chiamare `Entry` per garantire il comportamento precedente alla versione 3.0.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-381">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="cfe0d-382">Le chiavi matrice di byte e di stringhe non vengono generate dal client per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="cfe0d-382">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="cfe0d-383">Problema n. 14617</span><span class="sxs-lookup"><span data-stu-id="cfe0d-383">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="cfe0d-384">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-384">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="cfe0d-385">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-385">Old behavior</span></span>**

<span data-ttu-id="cfe0d-386">Nelle versioni precedenti a EF Core 3.0 le proprietà di chiave `string` e `byte[]` potevano essere usate senza impostare in modo esplicito un valore non Null.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-386">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="cfe0d-387">In questi casi, il valore di chiave veniva generato nel client come GUID, serializzato in byte per `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-387">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

**<span data-ttu-id="cfe0d-388">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-388">New behavior</span></span>**

<span data-ttu-id="cfe0d-389">A partire da EF Core 3.0 viene generata un'eccezione che indica che non è stato impostato alcun valore di chiave.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-389">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

**<span data-ttu-id="cfe0d-390">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-390">Why</span></span>**

<span data-ttu-id="cfe0d-391">Questa modifica è stata apportata poiché i valori `string`/`byte[]` generati dal client non risultano in genere utili e il comportamento predefinito rendeva complesso ragionare sui valori di chiave generati in un modo comune.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-391">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

**<span data-ttu-id="cfe0d-392">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-392">Mitigations</span></span>**

<span data-ttu-id="cfe0d-393">È possibile ripristinare il comportamento precedente alla versione 3.0 specificando in modo esplicito che le proprietà di chiave devono usare i valori generati se non viene impostato alcun altro valore non Null.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-393">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="cfe0d-394">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-394">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="cfe0d-395">Oppure con annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-395">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="cfe0d-396">ILoggerFactory è ora un servizio con ambito</span><span class="sxs-lookup"><span data-stu-id="cfe0d-396">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="cfe0d-397">Problema n. 14698</span><span class="sxs-lookup"><span data-stu-id="cfe0d-397">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="cfe0d-398">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-398">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="cfe0d-399">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-399">Old behavior</span></span>**

<span data-ttu-id="cfe0d-400">Nelle versioni precedenti a EF Core 3.0 `ILoggerFactory` veniva registrato come servizio singleton.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-400">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

**<span data-ttu-id="cfe0d-401">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-401">New behavior</span></span>**

<span data-ttu-id="cfe0d-402">A partire da EF Core 3.0, `ILoggerFactory` viene registrato come servizio con ambito.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-402">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

**<span data-ttu-id="cfe0d-403">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-403">Why</span></span>**

<span data-ttu-id="cfe0d-404">Questa modifica è stata apportata per consentire l'associazione di un logger a un'istanza `DbContext` che abilita altre funzionalità e rimuove alcuni casi di comportamento anomalo, ad esempio un'esplosione dei provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-404">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

**<span data-ttu-id="cfe0d-405">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-405">Mitigations</span></span>**

<span data-ttu-id="cfe0d-406">Questa modifica non dovrebbe influire sul codice dell'applicazione a meno che non vengano registrati e usati servizi personalizzati nel provider di servizi interno di EF Core.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-406">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="cfe0d-407">Questa condizione, tuttavia, non è comune.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-407">This isn't common.</span></span>
<span data-ttu-id="cfe0d-408">In questi casi la maggior parte delle operazioni continuano a essere eseguite correttamente, ma qualsiasi servizio singleton dipendente da `ILoggerFactory` dovrà essere modificato per ottenere `ILoggerFactory` in modo diverso.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-408">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="cfe0d-409">Se si verificano situazioni simili, inviare una segnalazione nello [strumento di gestione dei problemi in GitHub relativo a EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) per comunicare in che modo viene usato `ILoggerFactory` per consentirci di comprendere meglio come evitare ulteriori interruzioni in futuro.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-409">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="cfe0d-410">IDbContextOptionsExtensionWithDebugInfo unito in IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="cfe0d-410">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="cfe0d-411">Problema n. 13552</span><span class="sxs-lookup"><span data-stu-id="cfe0d-411">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="cfe0d-412">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-412">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="cfe0d-413">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-413">Old behavior</span></span>**

`IDbContextOptionsExtensionWithDebugInfo` <span data-ttu-id="cfe0d-414">era un'interfaccia facoltativa aggiuntiva estesa da `IDbContextOptionsExtension` che consentiva di evitare modifiche che causavano interruzioni nell'interfaccia durante il ciclo di produzione delle versioni 2.x.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-414">was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

**<span data-ttu-id="cfe0d-415">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-415">New behavior</span></span>**

<span data-ttu-id="cfe0d-416">Le interfacce sono ora unite in `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-416">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

**<span data-ttu-id="cfe0d-417">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-417">Why</span></span>**

<span data-ttu-id="cfe0d-418">Questa modifica è stata apportata perché le interfacce sono concettualmente un'unica interfaccia.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-418">This change was made because the interfaces are conceptually one.</span></span>

**<span data-ttu-id="cfe0d-419">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-419">Mitigations</span></span>**

<span data-ttu-id="cfe0d-420">Tutte le implementazioni di `IDbContextOptionsExtension` dovranno essere aggiornate per supportare il nuovo membro.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-420">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="cfe0d-421">I proxy di caricamento lazy non presuppongono più che le proprietà di navigazione vengano caricate completamente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-421">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="cfe0d-422">Problema n. 12780</span><span class="sxs-lookup"><span data-stu-id="cfe0d-422">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="cfe0d-423">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-423">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="cfe0d-424">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-424">Old behavior</span></span>**

<span data-ttu-id="cfe0d-425">Nelle versioni precedenti a EF Core 3.0, quando `DbContext` veniva eliminato non esisteva alcun metodo per scoprire se una determinata proprietà di navigazione in un'entità ottenuta da un contesto specifico veniva caricata completamente o meno.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-425">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="cfe0d-426">I proxy presupponevano invece che venisse caricata una navigazione di riferimento se era presente un valore non Null e che venisse caricata una navigazione di raccolta se era presente un valore.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-426">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="cfe0d-427">In questi casi il tentativo di eseguire un caricamento lazy non avrebbe avuto alcun esito.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-427">In these cases, attempting to lazy-load would be a no-op.</span></span>

**<span data-ttu-id="cfe0d-428">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-428">New behavior</span></span>**

<span data-ttu-id="cfe0d-429">A partire da Entity Framework Core 3.0, i proxy tengono traccia del caricamento o mancato caricamento di una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-429">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="cfe0d-430">Ciò significa che il tentativo di accedere a una proprietà di navigazione caricata dopo l'eliminazione del contesto non avrà mai alcun esito, anche quando la navigazione caricata è vuota o ha valore Null.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-430">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="cfe0d-431">Al contrario, il tentativo di accedere a una proprietà di navigazione non caricata genera un'eccezione se il contesto viene eliminato anche se la proprietà di navigazione è una raccolta non vuota.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-431">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="cfe0d-432">Se si verifica questa situazione significa che il codice dell'applicazione sta tentando di usare il caricamento lazy in un momento non valido e l'applicazione deve essere modificata in modo da non eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-432">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

**<span data-ttu-id="cfe0d-433">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-433">Why</span></span>**

<span data-ttu-id="cfe0d-434">Questa modifica è stata apportata per rendere coerente e corretto il comportamento durante un tentativo di caricamento lazy in un'istanza `DbContext` eliminata.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-434">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

**<span data-ttu-id="cfe0d-435">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-435">Mitigations</span></span>**

<span data-ttu-id="cfe0d-436">Aggiornare il codice dell'applicazione per fare in modo che non venga tentato il caricamento lazy con un contesto eliminato oppure specificare una configurazione in modo che non venga eseguita alcuna operazione come descritto nel messaggio di eccezione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-436">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="cfe0d-437">La creazione di un numero eccessivo di provider di servizi interni è ora un errore per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="cfe0d-437">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="cfe0d-438">Problema n. 10236</span><span class="sxs-lookup"><span data-stu-id="cfe0d-438">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="cfe0d-439">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-439">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="cfe0d-440">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-440">Old behavior</span></span>**

<span data-ttu-id="cfe0d-441">Nelle versioni precedenti a EF Core 3.0 veniva registrato un avviso per le applicazioni che creavano un numero eccessivo di provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-441">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

**<span data-ttu-id="cfe0d-442">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-442">New behavior</span></span>**

<span data-ttu-id="cfe0d-443">A partire da EF Core 3.0, l'avviso viene considerato un errore e viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-443">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

**<span data-ttu-id="cfe0d-444">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-444">Why</span></span>**

<span data-ttu-id="cfe0d-445">Questa modifica è stata apportata per gestire meglio il codice dell'applicazione tramite un'esposizione più esplicita di questa situazione di errore.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-445">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

**<span data-ttu-id="cfe0d-446">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-446">Mitigations</span></span>**

<span data-ttu-id="cfe0d-447">L'azione più appropriata quando si verifica questo errore consiste nell'individuare la causa radice e nell'interrompere la creazione di numerosi provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-447">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="cfe0d-448">È possibile tuttavia convertire nuovamente l'errore in avviso o ignorarlo tramite una configurazione in `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-448">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="cfe0d-449">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-449">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="cfe0d-450">Nuovo comportamento per la chiamata di HasOne/HasMany con una singola stringa</span><span class="sxs-lookup"><span data-stu-id="cfe0d-450">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="cfe0d-451">Problema n. 9171</span><span class="sxs-lookup"><span data-stu-id="cfe0d-451">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="cfe0d-452">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-452">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="cfe0d-453">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-453">Old behavior</span></span>**

<span data-ttu-id="cfe0d-454">Prima di EF Core 3.0, il codice che chiama `HasOne` o `HasMany` con una singola stringa era interpretato in modo poco chiaro.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-454">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="cfe0d-455">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-455">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="cfe0d-456">Apparentemente, il codice mette in relazione `Samurai` con un altro tipo di entità tramite la proprietà di navigazione `Entrance`, che può essere privata.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-456">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="cfe0d-457">In realtà, il codice tenta di creare una relazione con un tipo di entità denominato `Entrance` senza proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-457">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

**<span data-ttu-id="cfe0d-458">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-458">New behavior</span></span>**

<span data-ttu-id="cfe0d-459">A partire da EF Core 3.0, il codice sopra riportato ora esegue quello che avrebbe dovuto fare in precedenza.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-459">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

**<span data-ttu-id="cfe0d-460">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-460">Why</span></span>**

<span data-ttu-id="cfe0d-461">Il comportamento precedente era molto poco chiaro, soprattutto durante la lettura del codice di configurazione e la ricerca di errori.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-461">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

**<span data-ttu-id="cfe0d-462">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-462">Mitigations</span></span>**

<span data-ttu-id="cfe0d-463">Questa modifica causerà problemi solo nelle applicazioni che configurano relazioni in modo esplicito usando stringhe per i nomi dei tipi e senza specificare in modo esplicito la proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-463">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="cfe0d-464">Non è uno scenario comune.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-464">This is not common.</span></span>
<span data-ttu-id="cfe0d-465">Il comportamento precedente può essere ottenuto passando esplicitamente `null` per il nome della proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-465">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="cfe0d-466">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-466">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="cfe0d-467">Il tipo restituito per diversi metodi asincroni è cambiato da Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="cfe0d-467">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="cfe0d-468">Problema n. 15184</span><span class="sxs-lookup"><span data-stu-id="cfe0d-468">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="cfe0d-469">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-469">This change will be introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="cfe0d-470">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-470">Old behavior</span></span>**

<span data-ttu-id="cfe0d-471">I metodi asincroni seguenti in precedenza restituivano il tipo `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-471">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* `ValueGenerator.NextValueAsync()` <span data-ttu-id="cfe0d-472">(e classi derivate)</span><span class="sxs-lookup"><span data-stu-id="cfe0d-472">(and deriving classes)</span></span>

**<span data-ttu-id="cfe0d-473">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-473">New behavior</span></span>**

<span data-ttu-id="cfe0d-474">I metodi indicati in precedenza ora restituiscono il tipo `ValueTask<T>` sullo stesso `T` come in precedenza.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-474">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

**<span data-ttu-id="cfe0d-475">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-475">Why</span></span>**

<span data-ttu-id="cfe0d-476">Questa modifica riduce il numero delle allocazioni di heap sostenute quando si richiamano questi metodi, con un miglioramento generale delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-476">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

**<span data-ttu-id="cfe0d-477">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-477">Mitigations</span></span>**

<span data-ttu-id="cfe0d-478">Le applicazioni semplicemente in attesa delle API precedenti devono solo essere ricompilate e non sono richieste modifiche del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-478">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="cfe0d-479">Per scenari di utilizzo più complessi (ad esempio, il passaggio del tipo `Task` restituito a `Task.WhenAny()`) è richiesto in genere che il tipo `ValueTask<T>` restituito venga convertito in `Task<T>` chiamando `AsTask()` su di esso.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-479">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="cfe0d-480">Si noti che in questo modo si annulla la riduzione delle allocazioni consentita da questa modifica.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-480">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="cfe0d-481">L'annotazione Relational:TypeMapping è ora TypeMapping</span><span class="sxs-lookup"><span data-stu-id="cfe0d-481">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="cfe0d-482">Problema n. 9913</span><span class="sxs-lookup"><span data-stu-id="cfe0d-482">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="cfe0d-483">Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-483">This change was introduced in EF Core 3.0-preview 2.</span></span>

**<span data-ttu-id="cfe0d-484">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-484">Old behavior</span></span>**

<span data-ttu-id="cfe0d-485">Il nome di annotazione delle annotazioni di mapping del tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="cfe0d-485">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

**<span data-ttu-id="cfe0d-486">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-486">New behavior</span></span>**

<span data-ttu-id="cfe0d-487">Il nome di annotazione delle annotazioni di mapping del tipo è ora "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="cfe0d-487">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

**<span data-ttu-id="cfe0d-488">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-488">Why</span></span>**

<span data-ttu-id="cfe0d-489">Il mapping dei tipi non viene più usato solo per i provider di database relazionali.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-489">Type mappings are now used for more than just relational database providers.</span></span>

**<span data-ttu-id="cfe0d-490">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-490">Mitigations</span></span>**

<span data-ttu-id="cfe0d-491">Ciò causa un'interruzione solo nelle applicazioni che accedono al mapping dei tipi direttamente come annotazione. Questa situazione non è comune.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-491">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="cfe0d-492">L'azione più appropriata per risolvere il problema consiste nell'usare la superficie dell'API per accedere al mapping dei tipi anziché l'annotazione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-492">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="cfe0d-493">ToTable in un tipo derivato genera un'eccezione</span><span class="sxs-lookup"><span data-stu-id="cfe0d-493">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="cfe0d-494">Problema n. 11811</span><span class="sxs-lookup"><span data-stu-id="cfe0d-494">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="cfe0d-495">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-495">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="cfe0d-496">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-496">Old behavior</span></span>**

<span data-ttu-id="cfe0d-497">Nelle versioni precedenti a EF Core 3.0, la chiamata a `ToTable()` in un tipo derivato veniva ignorata poiché soltanto la strategia di mapping dell'ereditarietà era una tabella per gerarchia dove la chiamata non era valida.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-497">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

**<span data-ttu-id="cfe0d-498">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-498">New behavior</span></span>**

<span data-ttu-id="cfe0d-499">A partire da EF Core 3.0 e in preparazione all'aggiunta del supporto per la tabella per tipo e per TPC in una versione successiva, la chiamata a `ToTable()` in un tipo derivato genera un'eccezione per evitare una modifica del mapping imprevista in futuro.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-499">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

**<span data-ttu-id="cfe0d-500">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-500">Why</span></span>**

<span data-ttu-id="cfe0d-501">Attualmente il mapping di un tipo derivato in una tabella diversa non è un'operazione valida.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-501">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="cfe0d-502">Questa modifica consente di evitare interruzioni future quando l'operazione diventerà un'operazione valida.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-502">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

**<span data-ttu-id="cfe0d-503">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-503">Mitigations</span></span>**

<span data-ttu-id="cfe0d-504">Rimuovere qualsiasi tentativo di mapping di tipi derivati in altre tabelle.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-504">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="cfe0d-505">ForSqlServerHasIndex sostituito con HasIndex</span><span class="sxs-lookup"><span data-stu-id="cfe0d-505">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="cfe0d-506">Problema n. 12366</span><span class="sxs-lookup"><span data-stu-id="cfe0d-506">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="cfe0d-507">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-507">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="cfe0d-508">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-508">Old behavior</span></span>**

<span data-ttu-id="cfe0d-509">Nelle versioni precedenti a EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offriva un metodo per configurare le colonne usate con `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-509">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

**<span data-ttu-id="cfe0d-510">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-510">New behavior</span></span>**

<span data-ttu-id="cfe0d-511">A partire da EF Core 3.0, l'uso di `Include` in un indice è supportato a livello relazionale.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-511">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="cfe0d-512">Usare `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-512">Use `HasIndex().ForSqlServerInclude()`.</span></span>

**<span data-ttu-id="cfe0d-513">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-513">Why</span></span>**

<span data-ttu-id="cfe0d-514">Questa modifica è stata apportata per consolidare l'API per gli indici con `Includes` in un'unica posizione per tutti i provider di database.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-514">This change was made to consolidate the API for indexes with `Includes` into one place for all database providers.</span></span>

**<span data-ttu-id="cfe0d-515">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-515">Mitigations</span></span>**

<span data-ttu-id="cfe0d-516">Usare la nuova API, come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-516">Use the new API, as shown above.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="cfe0d-517">EF Core non invia più pragma per l'imposizione della chiave esterna di SQLite</span><span class="sxs-lookup"><span data-stu-id="cfe0d-517">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="cfe0d-518">Problema n. 12151</span><span class="sxs-lookup"><span data-stu-id="cfe0d-518">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="cfe0d-519">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-519">This change was introduced in EF Core 3.0-preview 3.</span></span>

**<span data-ttu-id="cfe0d-520">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-520">Old behavior</span></span>**

<span data-ttu-id="cfe0d-521">Nelle versioni precedenti a EF Core 3.0, EF Core inviava `PRAGMA foreign_keys = 1` quando veniva aperta una connessione a SQLite.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-521">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

**<span data-ttu-id="cfe0d-522">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-522">New behavior</span></span>**

<span data-ttu-id="cfe0d-523">A partire da EF Core 3.0, EF Core non invia più `PRAGMA foreign_keys = 1` quando viene aperta una connessione a SQLite.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-523">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

**<span data-ttu-id="cfe0d-524">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-524">Why</span></span>**

<span data-ttu-id="cfe0d-525">Questa modifica è stata apportata poiché EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` per impostazione predefinita. Ciò significa che l'imposizione della chiave esterna è abilitata per impostazione predefinita e non deve essere abilitata in modo esplicito ogni volta che viene aperta una connessione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-525">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

**<span data-ttu-id="cfe0d-526">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-526">Mitigations</span></span>**

<span data-ttu-id="cfe0d-527">Le chiavi esterne sono abilitate per impostazione predefinita in SQLitePCLRaw.bundle_e_sqlite3, usato per impostazione predefinita per EF Core.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-527">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="cfe0d-528">Per gli altri casi, è possibile abilitare le chiavi esterne specificando `Foreign Keys=True` nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-528">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="cfe0d-529">Microsoft.EntityFrameworkCore.Sqlite dipende ora da SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="cfe0d-529">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

**<span data-ttu-id="cfe0d-530">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-530">Old behavior</span></span>**

<span data-ttu-id="cfe0d-531">Nelle versioni precedenti a EF Core 3.0, EF Core usava `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-531">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

**<span data-ttu-id="cfe0d-532">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-532">New behavior</span></span>**

<span data-ttu-id="cfe0d-533">A partire da EF Core 3.0, EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-533">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

**<span data-ttu-id="cfe0d-534">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-534">Why</span></span>**

<span data-ttu-id="cfe0d-535">Questa modifica è stata apportata per rendere coerente la versione di SQLite usata in iOS con le altre piattaforme.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-535">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

**<span data-ttu-id="cfe0d-536">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-536">Mitigations</span></span>**

<span data-ttu-id="cfe0d-537">Per usare la versione di SQLite nativa in iOS, configurare `Microsoft.Data.Sqlite` per l'uso di un'aggregazione `SQLitePCLRaw` diversa.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-537">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="cfe0d-538">I valori Guid vengono ora archiviati come TEXT in SQLite</span><span class="sxs-lookup"><span data-stu-id="cfe0d-538">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="cfe0d-539">Problema n. 15078</span><span class="sxs-lookup"><span data-stu-id="cfe0d-539">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="cfe0d-540">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-540">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="cfe0d-541">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-541">Old behavior</span></span>**

<span data-ttu-id="cfe0d-542">I valori Guid in precedenza venivano archiviati come valori BLOB in SQLite.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-542">Guid values were previously sored as BLOB values on SQLite.</span></span>

**<span data-ttu-id="cfe0d-543">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-543">New behavior</span></span>**

<span data-ttu-id="cfe0d-544">I valori Guid vengono ora archiviati come TEXT.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-544">Guid values are now sotred as TEXT.</span></span>

**<span data-ttu-id="cfe0d-545">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-545">Why</span></span>**

<span data-ttu-id="cfe0d-546">Il formato binario dei valori Guid non è standardizzato.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-546">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="cfe0d-547">L'archiviazione dei valori come TEXT rende il database più compatibile con altre tecnologie.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-547">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

**<span data-ttu-id="cfe0d-548">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-548">Mitigations</span></span>**

<span data-ttu-id="cfe0d-549">È possibile eseguire la migrazione dei database esistenti al nuovo formato eseguendo SQL nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-549">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

<span data-ttu-id="cfe0d-550">In EF Core è anche possibile continuare a usare il comportamento precedente configurando un convertitore di valori in queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-550">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="cfe0d-551">Microsoft.Data.Sqlite rimane in grado di leggere i valori Guid sia da colonne BLOB che TEXT. Tuttavia, poiché è stato modificato il formato predefinito per i parametri e le costanti, probabilmente sarà necessario intervenire per la maggior parte degli scenari che coinvolgono valori Guid.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-551">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="cfe0d-552">I valori char vengono ora archiviati come testo in SQLite</span><span class="sxs-lookup"><span data-stu-id="cfe0d-552">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="cfe0d-553">Problema n. 15020</span><span class="sxs-lookup"><span data-stu-id="cfe0d-553">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="cfe0d-554">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-554">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="cfe0d-555">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-555">Old behavior</span></span>**

<span data-ttu-id="cfe0d-556">I valori char in precedenza venivano archiviati come valori interi in SQLite.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-556">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="cfe0d-557">Un valore char di *A* veniva ad esempio archiviato come valore intero 65.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-557">For example, a char value of *A* was stored as the integer value 65.</span></span>

**<span data-ttu-id="cfe0d-558">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-558">New behavior</span></span>**

<span data-ttu-id="cfe0d-559">I valori char vengono ora archiviati come testo.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-559">Char values are now sotred as TEXT.</span></span>

**<span data-ttu-id="cfe0d-560">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-560">Why</span></span>**

<span data-ttu-id="cfe0d-561">L'archiviazione dei valori come testo è un'operazione più naturale e rende il database più compatibile con altre tecnologie.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-561">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

**<span data-ttu-id="cfe0d-562">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-562">Mitigations</span></span>**

<span data-ttu-id="cfe0d-563">È possibile eseguire la migrazione dei database esistenti al nuovo formato eseguendo SQL nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-563">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="cfe0d-564">In EF Core è anche possibile continuare a usare il comportamento precedente configurando un convertitore di valori in queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-564">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="cfe0d-565">Microsoft.Data.Sqlite rimane comunque in grado di leggere i valori di caratteri presenti sia nelle colonne di valori interi sia in quelle di testo, quindi alcuni scenari potrebbero non richiedere alcuna azione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-565">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="cfe0d-566">Gli ID di migrazione vengono ora generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica</span><span class="sxs-lookup"><span data-stu-id="cfe0d-566">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="cfe0d-567">Problema n. 12978</span><span class="sxs-lookup"><span data-stu-id="cfe0d-567">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="cfe0d-568">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-568">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="cfe0d-569">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-569">Old behavior</span></span>**

<span data-ttu-id="cfe0d-570">Gli ID di migrazione venivano inavvertitamente generati usando con il calendario delle impostazioni cultura correnti.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-570">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

**<span data-ttu-id="cfe0d-571">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-571">New behavior</span></span>**

<span data-ttu-id="cfe0d-572">Gli ID di migrazione ora vengono sempre generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica (calendario gregoriano).</span><span class="sxs-lookup"><span data-stu-id="cfe0d-572">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

**<span data-ttu-id="cfe0d-573">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-573">Why</span></span>**

<span data-ttu-id="cfe0d-574">L'ordine delle migrazioni è importante quando si esegue l'aggiornamento del database o si risolvono i conflitti di unione.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-574">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="cfe0d-575">L'uso del calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica evita i problemi che possono verificarsi quando i membri del team hanno calendari di sistema diversi.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-575">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

**<span data-ttu-id="cfe0d-576">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-576">Mitigations</span></span>**

<span data-ttu-id="cfe0d-577">Questa modifica interessa gli utenti che usano un calendario non gregoriano in cui l'anno ha un'estensione superiore al calendario gregoriano (come il calendario buddista tailandese).</span><span class="sxs-lookup"><span data-stu-id="cfe0d-577">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="cfe0d-578">Gli ID di migrazione esistenti dovranno essere aggiornati in modo che le nuove migrazioni vengano collocate dopo le migrazioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-578">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="cfe0d-579">L'ID di migrazione è disponibile nell'attributo di migrazione presente nei file di progettazione delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-579">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="cfe0d-580">È necessario aggiornare anche la tabella della cronologia delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-580">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="cfe0d-581">LogQueryPossibleExceptionWithAggregateOperator è stato rinominato</span><span class="sxs-lookup"><span data-stu-id="cfe0d-581">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="cfe0d-582">Problema n. 10985</span><span class="sxs-lookup"><span data-stu-id="cfe0d-582">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="cfe0d-583">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-583">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="cfe0d-584">Modifica</span><span class="sxs-lookup"><span data-stu-id="cfe0d-584">Change</span></span>**

`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` <span data-ttu-id="cfe0d-585">è stato rinominato in `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-585">has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

**<span data-ttu-id="cfe0d-586">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-586">Why</span></span>**

<span data-ttu-id="cfe0d-587">Allineamento del nome di questo evento di avviso con tutti gli altri eventi di avviso.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-587">Aligns the naming of this warning event with all other warning events.</span></span>

**<span data-ttu-id="cfe0d-588">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-588">Mitigations</span></span>**

<span data-ttu-id="cfe0d-589">Usare il nuovo nome.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-589">Use the new name.</span></span> <span data-ttu-id="cfe0d-590">(Si noti che il numero di ID evento non è stato modificato.)</span><span class="sxs-lookup"><span data-stu-id="cfe0d-590">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="cfe0d-591">Chiarimenti per l'API per i nomi di vincolo di chiave esterna</span><span class="sxs-lookup"><span data-stu-id="cfe0d-591">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="cfe0d-592">Problema n. 10730</span><span class="sxs-lookup"><span data-stu-id="cfe0d-592">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="cfe0d-593">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-593">This change was introduced in EF Core 3.0-preview 4.</span></span>

**<span data-ttu-id="cfe0d-594">Comportamento precedente</span><span class="sxs-lookup"><span data-stu-id="cfe0d-594">Old behavior</span></span>**

<span data-ttu-id="cfe0d-595">Prima di EF Core 3.0, si faceva riferimento ai nomi di vincolo di chiave esterna semplicemente con "Name".</span><span class="sxs-lookup"><span data-stu-id="cfe0d-595">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="cfe0d-596">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-596">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

**<span data-ttu-id="cfe0d-597">Nuovo comportamento</span><span class="sxs-lookup"><span data-stu-id="cfe0d-597">New behavior</span></span>**

<span data-ttu-id="cfe0d-598">A partire da EF Core 3.0, si fa ora riferimento ai nomi di vincolo di chiave esterna con "ConstraintName".</span><span class="sxs-lookup"><span data-stu-id="cfe0d-598">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constaint name".</span></span> <span data-ttu-id="cfe0d-599">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cfe0d-599">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

**<span data-ttu-id="cfe0d-600">Perché?</span><span class="sxs-lookup"><span data-stu-id="cfe0d-600">Why</span></span>**

<span data-ttu-id="cfe0d-601">Questa modifica introduce coerenza per la denominazione in quest'area e chiarisce anche che si tratta del nome del vincolo di chiave esterna e non del nome della colonna o della proprietà per cui è definita la chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-601">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constaint, and not the column or property name that the foreign key is defined on.</span></span>

**<span data-ttu-id="cfe0d-602">Mitigazioni</span><span class="sxs-lookup"><span data-stu-id="cfe0d-602">Mitigations</span></span>**

<span data-ttu-id="cfe0d-603">Usare il nuovo nome.</span><span class="sxs-lookup"><span data-stu-id="cfe0d-603">Use the new name.</span></span>
