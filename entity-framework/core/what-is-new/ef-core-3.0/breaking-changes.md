---
title: Modifiche che causano un'interruzione in EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 4b251638de43af6525f3e6faa0bd4113ab1714b9
ms.sourcegitcommit: 5280dcac4423acad8b440143433459b18886115b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/16/2019
ms.locfileid: "59619259"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="b3788-102">Modifiche che causano un'interruzione incluse in EF Core 3.0 (attualmente in anteprima)</span><span class="sxs-lookup"><span data-stu-id="b3788-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b3788-103">Si tenga presente che i set di funzionalità e le pianificazioni delle versioni future sono sempre soggette a modifiche e che questa pagina, nonostante l'impegno profuso per mantenerla aggiornata, potrebbe non riflettere sempre i piani più recenti.</span><span class="sxs-lookup"><span data-stu-id="b3788-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="b3788-104">Le modifiche all'API e al comportamento seguenti possono potenzialmente interrompere le applicazioni sviluppate per EF Core 2.2.x durante l'aggiornamento alla versione 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="b3788-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="b3788-105">Le modifiche che si prevede abbiano impatto solo sui provider di database sono documentate nelle [modifiche che influiscono sul provider](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="b3788-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="b3788-106">Le interruzioni nelle nuove funzionalità introdotte da un'anteprima 3.0 a un'altra anteprima 3.0 non sono documentate.</span><span class="sxs-lookup"><span data-stu-id="b3788-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="b3788-107">Le query LINQ non vengono più valutate nel client</span><span class="sxs-lookup"><span data-stu-id="b3788-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="b3788-108">[Problema n. 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Vedere anche il problema n. 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="b3788-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="b3788-109">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-110">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-110">**Old behavior**</span></span>

<span data-ttu-id="b3788-111">Nelle versioni precedenti alla versione 3.0, quando EF Core non era in grado di convertire un'espressione inclusa in una query in SQL o in un parametro, l'espressione veniva automaticamente valutata nel client.</span><span class="sxs-lookup"><span data-stu-id="b3788-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="b3788-112">Per impostazione predefinita, la valutazione client di espressioni potenzialmente dispendiose si limitava ad attivare solo un avviso.</span><span class="sxs-lookup"><span data-stu-id="b3788-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="b3788-113">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-113">**New behavior**</span></span>

<span data-ttu-id="b3788-114">A partire dalla versione 3.0, EF Core consente solo la valutazione delle espressioni nella proiezione di primo livello (l'ultima chiamata `Select()` nella query) nel client.</span><span class="sxs-lookup"><span data-stu-id="b3788-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="b3788-115">Quando le espressioni in altre posizioni all'interno della query non possono essere convertite in SQL o in un parametro, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="b3788-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="b3788-116">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-116">**Why**</span></span>

<span data-ttu-id="b3788-117">La valutazione client automatica delle query consente di eseguire numerose query anche nel caso in cui parti importanti delle query non possono essere convertite.</span><span class="sxs-lookup"><span data-stu-id="b3788-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="b3788-118">Questo comportamento può causare un comportamento imprevisto e potenzialmente dannoso che può diventare evidente solo in produzione.</span><span class="sxs-lookup"><span data-stu-id="b3788-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="b3788-119">Ad esempio, una condizione in una chiamata `Where()` che non può essere convertita può causare il trasferimento di tutte le righe della tabella del server di database e l'applicazione del filtro nel client.</span><span class="sxs-lookup"><span data-stu-id="b3788-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="b3788-120">È probabile che questa situazione non venga rilevata se la tabella contiene solo alcune righe in fase di sviluppo, ma che abbia un grande impatto quando l'applicazione passa in produzione dove la tabella può contenere milioni di righe.</span><span class="sxs-lookup"><span data-stu-id="b3788-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="b3788-121">Gli avvisi di valutazione client inoltre si sono rivelati molto facili da ignorare durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="b3788-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="b3788-122">Inoltre, la valutazione client automatica può causare problemi in cui il miglioramento della conversione di query per espressioni specifiche causa modifiche impreviste che causano un'interruzione da una versione all'altra.</span><span class="sxs-lookup"><span data-stu-id="b3788-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="b3788-123">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-123">**Mitigations**</span></span>

<span data-ttu-id="b3788-124">Se una query non può essere convertita completamente, riscrivere la query in un formato che possa essere convertito o usare `AsEnumerable()`, `ToList()` o un elemento simile per riportare in modo esplicito i dati nel client dove possono essere quindi ulteriormente elaborati usando LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="b3788-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="b3788-125">Entity Framework Core non è più incluso nel framework condiviso di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b3788-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="b3788-126">Annunci problema n. 325</span><span class="sxs-lookup"><span data-stu-id="b3788-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="b3788-127">Questa modifica è stata introdotta in ASP.NET Core 3.0 anteprima 1.</span><span class="sxs-lookup"><span data-stu-id="b3788-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="b3788-128">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-128">**Old behavior**</span></span>

<span data-ttu-id="b3788-129">Nelle versioni precedenti ad ASP.NET Core 3.0, quando si aggiungeva un riferimento a un pacchetto in `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, veniva inserito EF Core e alcuni dei provider di dati di EF Core come il provider di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b3788-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="b3788-130">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-130">**New behavior**</span></span>

<span data-ttu-id="b3788-131">A partire dalla versione 3.0, il framework condiviso di ASP.NET Core non include EF Core o provider di dati di EF Core.</span><span class="sxs-lookup"><span data-stu-id="b3788-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="b3788-132">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-132">**Why**</span></span>

<span data-ttu-id="b3788-133">Prima di questa modifica, per ottenere EF Core erano necessarie procedure diverse, a seconda che l'applicazione avesse o meno come destinazione ASP.NET Core e SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b3788-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="b3788-134">Inoltre, l'aggiornamento di ASP.NET Core forzava l'aggiornamento di EF Core e del provider di SQL Server, non sempre auspicabile.</span><span class="sxs-lookup"><span data-stu-id="b3788-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="b3788-135">Con questa modifica, la procedura per ottenere EF Core è la stessa in tutti i provider, le implementazioni .NET supportate e i tipi di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="b3788-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="b3788-136">Gli sviluppatori possono ora controllare anche in modo preciso quando vengono aggiornati EF Core e i provider di dati di EF Core.</span><span class="sxs-lookup"><span data-stu-id="b3788-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="b3788-137">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-137">**Mitigations**</span></span>

<span data-ttu-id="b3788-138">Per usare EF Core in un'applicazione ASP.NET Core 3.0 o in un'altra applicazione supportata, aggiungere in modo esplicito un riferimento al pacchetto al provider di database di EF Core che verrà usato dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b3788-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="b3788-139">I metodi FromSql, ExecuteSql ed ExecuteSqlAsync sono stati rinominati</span><span class="sxs-lookup"><span data-stu-id="b3788-139">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="b3788-140">Problema n. 10996</span><span class="sxs-lookup"><span data-stu-id="b3788-140">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="b3788-141">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-141">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-142">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-142">**Old behavior**</span></span>

<span data-ttu-id="b3788-143">Prima di EF Core 3.0, erano disponibili overload per questi nomi di metodo per supportare l'uso con una stringa normale o una stringa che deve essere interpolata in SQL e parametri.</span><span class="sxs-lookup"><span data-stu-id="b3788-143">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="b3788-144">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-144">**New behavior**</span></span>

<span data-ttu-id="b3788-145">A partire da EF Core 3.0, usare `FromSqlRaw`, `ExecuteSqlRaw` e `ExecuteSqlRawAsync` per creare una query con parametri in cui i parametri vengono passati separatamente dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="b3788-145">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="b3788-146">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b3788-146">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="b3788-147">Usare `FromSqlInterpolated`, `ExecuteSqlInterpolated`, e `ExecuteSqlInterpolatedAsync` per creare una query con parametri in cui i parametri vengono passati come parte di una stringa di query interpolata.</span><span class="sxs-lookup"><span data-stu-id="b3788-147">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="b3788-148">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b3788-148">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="b3788-149">Si noti che entrambe le query precedenti produrranno lo stesso codice SQL con parametri con gli stessi parametri SQL.</span><span class="sxs-lookup"><span data-stu-id="b3788-149">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="b3788-150">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-150">**Why**</span></span>

<span data-ttu-id="b3788-151">Con gli overload di metodi come questi, è molto semplice chiamare accidentalmente il metodo con stringa non elaborata anche se l'intento era chiamare il metodo con stringa interpolata e viceversa.</span><span class="sxs-lookup"><span data-stu-id="b3788-151">Method overloads like this make it very easy to accidentally call the raw srting method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="b3788-152">Il risultato potrebbero essere query senza parametri, quando invece è prevista la parametrizzazione.</span><span class="sxs-lookup"><span data-stu-id="b3788-152">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="b3788-153">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-153">**Mitigations**</span></span>

<span data-ttu-id="b3788-154">Passare all'uso dei nuovi nomi di metodo.</span><span class="sxs-lookup"><span data-stu-id="b3788-154">Switch to use the new method names.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="b3788-155">L'esecuzione di query viene registrata a livello di debug</span><span class="sxs-lookup"><span data-stu-id="b3788-155">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="b3788-156">Problema n. 14523</span><span class="sxs-lookup"><span data-stu-id="b3788-156">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="b3788-157">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="b3788-157">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3788-158">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-158">**Old behavior**</span></span>

<span data-ttu-id="b3788-159">Nelle versioni precedenti a EF Core 3.0 l'esecuzione di query e altri comandi veniva registrata a livello `Info`.</span><span class="sxs-lookup"><span data-stu-id="b3788-159">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="b3788-160">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-160">**New behavior**</span></span>

<span data-ttu-id="b3788-161">A partire da EF Core 3.0, la registrazione dell'esecuzione di comandi/SQL è a livello `Debug`.</span><span class="sxs-lookup"><span data-stu-id="b3788-161">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="b3788-162">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-162">**Why**</span></span>

<span data-ttu-id="b3788-163">Questa modifica è stata apportata per ridurre i disturbi a livello di registrazione `Info`.</span><span class="sxs-lookup"><span data-stu-id="b3788-163">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="b3788-164">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-164">**Mitigations**</span></span>

<span data-ttu-id="b3788-165">Questo evento di registrazione è definito da `RelationalEventId.CommandExecuting` con l'ID evento 20100.</span><span class="sxs-lookup"><span data-stu-id="b3788-165">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="b3788-166">Per registrare SQL di nuovo al livello `Info`, configurare in modo esplicito il livello in `OnConfiguring` o `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="b3788-166">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="b3788-167">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b3788-167">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="b3788-168">I valori di chiave temporanei non sono più impostati nelle istanze di entità</span><span class="sxs-lookup"><span data-stu-id="b3788-168">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="b3788-169">Problema n. 12378</span><span class="sxs-lookup"><span data-stu-id="b3788-169">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="b3788-170">Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.</span><span class="sxs-lookup"><span data-stu-id="b3788-170">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="b3788-171">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-171">**Old behavior**</span></span>

<span data-ttu-id="b3788-172">Nelle versioni precedenti a EF Core 3.0 i valori temporanei venivano assegnati a tutte le proprietà di chiave per cui veniva in seguito generato un valore reale dal database.</span><span class="sxs-lookup"><span data-stu-id="b3788-172">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="b3788-173">In genere questi valori temporanei erano numeri negativi elevati.</span><span class="sxs-lookup"><span data-stu-id="b3788-173">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="b3788-174">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-174">**New behavior**</span></span>

<span data-ttu-id="b3788-175">A partire dalla versione 3.0, EF Core archivia il valore di chiave temporaneo come parte delle informazioni di rilevamento dell'entità e non modifica la proprietà di chiave.</span><span class="sxs-lookup"><span data-stu-id="b3788-175">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="b3788-176">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-176">**Why**</span></span>

<span data-ttu-id="b3788-177">Questa modifica è stata apportata per impedire che i valori di chiave temporanei diventino erroneamente permanenti quando un'entità rilevata in precedenza da un'istanza `DbContext` viene spostata in un'altra istanza `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="b3788-177">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="b3788-178">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-178">**Mitigations**</span></span>

<span data-ttu-id="b3788-179">Le applicazioni che assegnano valori di chiave primaria in chiavi esterne per creare associazioni tra le entità possono dipendere dal comportamento precedente se le chiavi primarie vengono generate dall'archivio e appartengono a entità con stato `Added`.</span><span class="sxs-lookup"><span data-stu-id="b3788-179">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="b3788-180">Questo può essere evitato:</span><span class="sxs-lookup"><span data-stu-id="b3788-180">This can be avoided by:</span></span>
* <span data-ttu-id="b3788-181">Non usando chiavi generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="b3788-181">Not using store-generated keys.</span></span>
* <span data-ttu-id="b3788-182">Impostando le proprietà di navigazione in modo da creare relazioni anziché impostando valori di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="b3788-182">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="b3788-183">Ottenendo i valori di chiave temporanei effettivi dalle informazioni di rilevamento dell'entità.</span><span class="sxs-lookup"><span data-stu-id="b3788-183">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="b3788-184">Ad esempio, `context.Entry(blog).Property(e => e.Id).CurrentValue` restituisce il valore temporaneo anche quando `blog.Id` non è stato impostato.</span><span class="sxs-lookup"><span data-stu-id="b3788-184">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="b3788-185">DetectChanges rispetta i valori di chiave generati dall'archivio</span><span class="sxs-lookup"><span data-stu-id="b3788-185">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="b3788-186">Problema n. 14616</span><span class="sxs-lookup"><span data-stu-id="b3788-186">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="b3788-187">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="b3788-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3788-188">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-188">**Old behavior**</span></span>

<span data-ttu-id="b3788-189">Nelle versioni precedenti a EF Core 3.0 un'entità non rilevata individuata da `DetectChanges` veniva rilevata nello stato `Added` e inserita come nuova riga quando veniva eseguita una chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="b3788-189">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="b3788-190">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-190">**New behavior**</span></span>

<span data-ttu-id="b3788-191">A partire da EF Core 3.0, se un'entità usa valori di chiave generati e viene impostato un valore di chiave, l'entità viene rilevata nello stato `Modified`.</span><span class="sxs-lookup"><span data-stu-id="b3788-191">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="b3788-192">Ciò significa che si presuppone l'esistenza di una riga per l'entità che viene aggiornata quando viene eseguita una chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="b3788-192">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="b3788-193">Se il valore di chiave non viene impostato o se il tipo di entità non usa chiavi generate, la nuova entità viene rilevata come `Added` come nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="b3788-193">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="b3788-194">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-194">**Why**</span></span>

<span data-ttu-id="b3788-195">Questa modifica è stata apportata per rendere più semplice e coerente l'uso di grafici di entità disconnesse con chiavi generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="b3788-195">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="b3788-196">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-196">**Mitigations**</span></span>

<span data-ttu-id="b3788-197">Questa modifica può interrompere un'applicazione se un tipo di entità è configurato per l'uso di chiavi generate ma i valori di chiave sono impostati in modo esplicito per le nuove istanze.</span><span class="sxs-lookup"><span data-stu-id="b3788-197">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="b3788-198">La correzione consiste nel configurare in modo esplicito le proprietà di chiave per non usare valori generati.</span><span class="sxs-lookup"><span data-stu-id="b3788-198">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="b3788-199">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="b3788-199">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="b3788-200">Oppure con annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="b3788-200">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="b3788-201">Le eliminazioni a catena vengono ora eseguite immediatamente per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="b3788-201">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="b3788-202">Problema n. 10114</span><span class="sxs-lookup"><span data-stu-id="b3788-202">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="b3788-203">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="b3788-203">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3788-204">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-204">**Old behavior**</span></span>

<span data-ttu-id="b3788-205">Nelle versioni precedenti alla versione 3.0, EF Core applicava azioni a catena (eliminazione delle entità dipendenti quando veniva eliminata un'entità di sicurezza obbligatoria o veniva recisa la relazione con un'entità di sicurezza obbligatoria) solo dopo la chiamata a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="b3788-205">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="b3788-206">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-206">**New behavior**</span></span>

<span data-ttu-id="b3788-207">A partire dalla versione 3.0, EF Core applica le azioni a catena non appena viene rilevata la condizione di attivazione.</span><span class="sxs-lookup"><span data-stu-id="b3788-207">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="b3788-208">Ad esempio, la chiamata a `context.Remove()` per eliminare un'entità di sicurezza causa anche l'impostazione immediata di tutti i dipendenti obbligatori correlati rilevati su `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="b3788-208">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="b3788-209">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-209">**Why**</span></span>

<span data-ttu-id="b3788-210">Questa modifica è stata apportata per migliorare l'esperienza di associazione di dati e degli scenari di controllo in cui è importante individuare le entità che verranno eliminate _prima_ della chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="b3788-210">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="b3788-211">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-211">**Mitigations**</span></span>

<span data-ttu-id="b3788-212">Il comportamento precedente può essere ripristinato tramite le impostazioni in `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="b3788-212">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="b3788-213">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b3788-213">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="b3788-214">Semantica più chiara per DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="b3788-214">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="b3788-215">Problema n. 12661</span><span class="sxs-lookup"><span data-stu-id="b3788-215">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="b3788-216">Questa modifica verrà introdotta in EF Core 3.0 anteprima 5.</span><span class="sxs-lookup"><span data-stu-id="b3788-216">This change will be introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="b3788-217">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-217">**Old behavior**</span></span>

<span data-ttu-id="b3788-218">Prima della versione 3.0, `DeleteBehavior.Restrict` creava chiavi esterne nel database con la semantica `Restrict`, ma modificava anche la correzione interna in modo non ovvio.</span><span class="sxs-lookup"><span data-stu-id="b3788-218">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="b3788-219">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-219">**New behavior**</span></span>

<span data-ttu-id="b3788-220">A partire dalla versione 3.0, `DeleteBehavior.Restrict` assicura che le chiavi esterne vengano create con la semantica `Restrict`, ovvero non a cascata e con generazione di un'eccezione in caso di violazione di vincolo, senza influire sulla correzione interna di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b3788-220">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="b3788-221">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-221">**Why**</span></span>

<span data-ttu-id="b3788-222">Questa modifica è stata apportata per migliorare l'esperienza di uso di `DeleteBehavior` in modo intuitivo, senza effetti collaterali imprevisti.</span><span class="sxs-lookup"><span data-stu-id="b3788-222">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="b3788-223">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-223">**Mitigations**</span></span>

<span data-ttu-id="b3788-224">Il comportamento precedente può essere ripristinato tramite `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="b3788-224">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="b3788-225">I tipi di query vengono consolidati con tipi di entità</span><span class="sxs-lookup"><span data-stu-id="b3788-225">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="b3788-226">Problema n. 14194</span><span class="sxs-lookup"><span data-stu-id="b3788-226">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="b3788-227">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="b3788-227">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3788-228">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-228">**Old behavior**</span></span>

<span data-ttu-id="b3788-229">Nelle versioni precedenti a EF Core 3.0 i [tipi di query](xref:core/modeling/query-types) erano uno strumento per eseguire query su dati che non definiscono una chiave primaria in modo strutturato.</span><span class="sxs-lookup"><span data-stu-id="b3788-229">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="b3788-230">Veniva infatti usato un tipo di query per eseguire il mapping di tipi di entità senza chiavi (più probabilmente da una vista, ma anche da una tabella), mentre veniva usato un tipo di entità normale quando era disponibile una chiave (più probabilmente da una tabella, ma anche da una vista).</span><span class="sxs-lookup"><span data-stu-id="b3788-230">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="b3788-231">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-231">**New behavior**</span></span>

<span data-ttu-id="b3788-232">Un tipo di query diventa ora semplicemente un tipo di entità senza chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="b3788-232">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="b3788-233">I tipi di entità senza chiave hanno la stessa funzionalità dei tipi di query nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="b3788-233">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="b3788-234">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-234">**Why**</span></span>

<span data-ttu-id="b3788-235">Questa modifica è stata apportata per ridurre la confusione riguardo lo scopo dei tipi di query.</span><span class="sxs-lookup"><span data-stu-id="b3788-235">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="b3788-236">In particolare, si tratta di tipi di entità senza chiave che sono intrinsecamente di sola lettura per questo motivo ma non dovrebbero essere usati solo perché un tipo di entità deve essere di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="b3788-236">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="b3788-237">Analogamente, spesso vengono mappati alle viste solo perché le viste spesso non definiscono le chiavi.</span><span class="sxs-lookup"><span data-stu-id="b3788-237">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="b3788-238">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-238">**Mitigations**</span></span>

<span data-ttu-id="b3788-239">Le parti dell'API seguenti sono ora obsolete:</span><span class="sxs-lookup"><span data-stu-id="b3788-239">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="b3788-240">**`ModelBuilder.Query<>()`** - È necessario chiamare `ModelBuilder.Entity<>().HasNoKey()` per contrassegnare un tipo di entità come tipo senza chiavi.</span><span class="sxs-lookup"><span data-stu-id="b3788-240">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="b3788-241">Non ne viene eseguita la configurazione per convenzione per evitare una configurazione errata quando è prevista una chiave primaria che tuttavia non corrisponde alla convenzione.</span><span class="sxs-lookup"><span data-stu-id="b3788-241">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="b3788-242">**`DbQuery<>`** - Usare `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="b3788-242">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="b3788-243">**`DbContext.Query<>()`** - Usare `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="b3788-243">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="b3788-244">L'API di configurazione per le relazioni di tipo di proprietà è stata modificata</span><span class="sxs-lookup"><span data-stu-id="b3788-244">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="b3788-245">[Problema n. 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema n. 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema n. 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="b3788-245">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="b3788-246">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="b3788-246">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3788-247">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-247">**Old behavior**</span></span>

<span data-ttu-id="b3788-248">Nelle versioni precedenti a EF Core 3.0 la configurazione della relazione di proprietà veniva eseguita direttamente dopo la chiamata a `OwnsOne` o `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="b3788-248">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="b3788-249">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-249">**New behavior**</span></span>

<span data-ttu-id="b3788-250">A partire da EF Core 3.0, è disponibile l'API Fluent per configurare una proprietà di navigazione per il proprietario usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="b3788-250">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="b3788-251">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b3788-251">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="b3788-252">La configurazione correlata alla relazione tra proprietario e elemento di proprietà deve essere ora concatenata dopo `WithOwner()` in modo analogo a come vengono configurate altre relazioni.</span><span class="sxs-lookup"><span data-stu-id="b3788-252">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="b3788-253">La configurazione per il tipo di proprietà viene comunque concatenata dopo `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="b3788-253">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="b3788-254">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b3788-254">For example:</span></span>

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

<span data-ttu-id="b3788-255">Inoltre, la chiamata a `Entity()`, `HasOne()` o `Set()` con una destinazione di tipo di proprietà genera ora un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="b3788-255">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="b3788-256">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-256">**Why**</span></span>

<span data-ttu-id="b3788-257">Questa modifica è stata apportata per creare una separazione più netta tra la configurazione del tipo di proprietà e la _relazione_ con il tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="b3788-257">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="b3788-258">Ciò consente di eliminare ambiguità e confusione su metodi come `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="b3788-258">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="b3788-259">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-259">**Mitigations**</span></span>

<span data-ttu-id="b3788-260">Modificare la configurazione delle relazioni dei tipi di proprietà per usare la superficie della nuova API come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="b3788-260">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="b3788-261">Le entità dipendenti che condividono la tabella con l'entità di sicurezza sono ora facoltative</span><span class="sxs-lookup"><span data-stu-id="b3788-261">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="b3788-262">Problema n. 9005</span><span class="sxs-lookup"><span data-stu-id="b3788-262">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="b3788-263">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-263">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-264">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-264">**Old behavior**</span></span>

<span data-ttu-id="b3788-265">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="b3788-265">Consider the following model:</span></span>
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
<span data-ttu-id="b3788-266">Prima di EF Core 3.0, se `OrderDetails` è di proprietà di `Order` o viene mappato in modo esplicito alla stessa tabella, era sempre necessaria un'istanza di `OrderDetails` per l'aggiunta di un nuovo `Order`.</span><span class="sxs-lookup"><span data-stu-id="b3788-266">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="b3788-267">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-267">**New behavior**</span></span>

<span data-ttu-id="b3788-268">A partire dalla versione 3.0, EF Core consente di aggiungere un `Order` senza un `OrderDetails` ed esegue il mapping di tutte le proprietà di `OrderDetails`, tranne che la chiave primaria, a colonne che ammettono valori Null.</span><span class="sxs-lookup"><span data-stu-id="b3788-268">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="b3788-269">In fase di query, EF Core imposta `OrderDetails` su `null` se una delle relative proprietà obbligatorie non ha un valore o se non sono presenti proprietà obbligatorie oltre alla chiave primaria e tutte le proprietà sono `null`.</span><span class="sxs-lookup"><span data-stu-id="b3788-269">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="b3788-270">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-270">**Mitigations**</span></span>

<span data-ttu-id="b3788-271">Se il modello include una tabella condivisa dipendente con tutte le colonne facoltative, ma è previsto che la navigazione che punta a essa non sia `null`, l'applicazione deve essere modificata per gestire casi in cui la navigazione è `null`.</span><span class="sxs-lookup"><span data-stu-id="b3788-271">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="b3788-272">Se questo non è possibile, è consigliabile aggiungere una proprietà obbligatoria al tipo di entità o assegnare un valore non `null` ad almeno una proprietà.</span><span class="sxs-lookup"><span data-stu-id="b3788-272">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="b3788-273">Tutte le entità che condividono una tabella con una colonna di token di concorrenza devono eseguirne il mapping a una proprietà</span><span class="sxs-lookup"><span data-stu-id="b3788-273">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="b3788-274">Problema n. 14154</span><span class="sxs-lookup"><span data-stu-id="b3788-274">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="b3788-275">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-275">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-276">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-276">**Old behavior**</span></span>

<span data-ttu-id="b3788-277">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="b3788-277">Consider the following model:</span></span>
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
<span data-ttu-id="b3788-278">Prima di EF Core 3.0, se `OrderDetails` è di proprietà di `Order` o è mappato in modo esplicito alla stessa tabella, il solo aggiornamento di `OrderDetails` non aggiornerà il valore `Version` nel client e l'aggiornamento successivo avrà esito negativo.</span><span class="sxs-lookup"><span data-stu-id="b3788-278">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="b3788-279">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-279">**New behavior**</span></span>

<span data-ttu-id="b3788-280">A partire dalla versione 3.0, EF Core propaga il nuovo valore `Version` a `Order` se è proprietario di `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="b3788-280">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="b3788-281">In caso contrario, viene generata un'eccezione durante la convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="b3788-281">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="b3788-282">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-282">**Why**</span></span>

<span data-ttu-id="b3788-283">Questa modifica è stata apportata per evitare un valore del token di concorrenza non aggiornato quando viene aggiornata solo una delle entità mappate alla stessa tabella.</span><span class="sxs-lookup"><span data-stu-id="b3788-283">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="b3788-284">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-284">**Mitigations**</span></span>

<span data-ttu-id="b3788-285">Tutte le entità che condividono la tabella devono includere una proprietà mappata alla colonna del token di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="b3788-285">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="b3788-286">È possibile crearne una in stato shadow:</span><span class="sxs-lookup"><span data-stu-id="b3788-286">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="b3788-287">Per le proprietà ereditate da tipi senza mapping viene ora eseguito il mapping a una singola colonna per tutti i tipi derivati</span><span class="sxs-lookup"><span data-stu-id="b3788-287">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="b3788-288">Problema n. 13998</span><span class="sxs-lookup"><span data-stu-id="b3788-288">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="b3788-289">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-289">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-290">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-290">**Old behavior**</span></span>

<span data-ttu-id="b3788-291">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="b3788-291">Consider the following model:</span></span>
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

<span data-ttu-id="b3788-292">Prima di EF Core 3.0, per la proprietà `ShippingAddress` sarebbe stato eseguito il mapping a colonne separate per `BulkOrder` e `Order` per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="b3788-292">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="b3788-293">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-293">**New behavior**</span></span>

<span data-ttu-id="b3788-294">A partire dalla versione 3.0, EF Core crea solo una colonna per `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="b3788-294">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="b3788-295">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-295">**Why**</span></span>

<span data-ttu-id="b3788-296">Il comportamento precedente non era previsto.</span><span class="sxs-lookup"><span data-stu-id="b3788-296">The old behavoir was unexpected.</span></span>

<span data-ttu-id="b3788-297">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-297">**Mitigations**</span></span>

<span data-ttu-id="b3788-298">È ancora possibile eseguire il mapping esplicito della proprietà a una colonna separata per i tipi derivati:</span><span class="sxs-lookup"><span data-stu-id="b3788-298">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="b3788-299">La convenzione di proprietà di chiave esterna non ha più lo stesso nome della proprietà dell'entità di sicurezza</span><span class="sxs-lookup"><span data-stu-id="b3788-299">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="b3788-300">Problema n. 13274</span><span class="sxs-lookup"><span data-stu-id="b3788-300">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="b3788-301">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="b3788-301">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3788-302">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-302">**Old behavior**</span></span>

<span data-ttu-id="b3788-303">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="b3788-303">Consider the following model:</span></span>
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
<span data-ttu-id="b3788-304">Nelle versioni precedenti a EF Core 3.0 veniva usata la proprietà `CustomerId` per la chiave esterna per convenzione.</span><span class="sxs-lookup"><span data-stu-id="b3788-304">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="b3788-305">Tuttavia, se `Order` è un tipo di proprietà, `CustomerId` sarebbe la chiave primaria e ciò non è in genere auspicabile.</span><span class="sxs-lookup"><span data-stu-id="b3788-305">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="b3788-306">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-306">**New behavior**</span></span>

<span data-ttu-id="b3788-307">A partire da 3.0, EF Core non tenta di usare le proprietà per le chiavi esterne per convenzione se hanno lo stesso nome della proprietà dell'entità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="b3788-307">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="b3788-308">Viene ancora eseguita la corrispondenza tra i criteri del nome del tipo dell'entità di sicurezza concatenato al nome della proprietà dell'entità di sicurezza e il nome di navigazione concatenato al nome della proprietà dell'entità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="b3788-308">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="b3788-309">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b3788-309">For example:</span></span>

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

<span data-ttu-id="b3788-310">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-310">**Why**</span></span>

<span data-ttu-id="b3788-311">Questa modifica è stata apportata per evitare una definizione errata della proprietà di chiave primaria nel tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="b3788-311">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="b3788-312">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-312">**Mitigations**</span></span>

<span data-ttu-id="b3788-313">Se la proprietà è stata progettata per essere la chiave esterna e di conseguenza parte della chiave primaria, configurarla in modo esplicito come chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="b3788-313">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="b3788-314">La connessione al database viene ora chiusa se non viene più usata prima del completamento di TransactionScope</span><span class="sxs-lookup"><span data-stu-id="b3788-314">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="b3788-315">Problema n. 14218</span><span class="sxs-lookup"><span data-stu-id="b3788-315">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="b3788-316">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-316">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-317">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-317">**Old behavior**</span></span>

<span data-ttu-id="b3788-318">Prima di EF Core 3.0, se il contesto apre la connessione all'interno di un `TransactionScope`, la connessione rimane aperta mentre è attivo il `TransactionScope` corrente.</span><span class="sxs-lookup"><span data-stu-id="b3788-318">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="b3788-319">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-319">**New behavior**</span></span>

<span data-ttu-id="b3788-320">A partire dalla versione 3.0, EF Core chiude la connessione non appena non viene più usata.</span><span class="sxs-lookup"><span data-stu-id="b3788-320">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="b3788-321">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-321">**Why**</span></span>

<span data-ttu-id="b3788-322">Questa modifica consente di usare più contesti nello stesso `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="b3788-322">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="b3788-323">Il nuovo comportamento corrisponde anche a EF6.</span><span class="sxs-lookup"><span data-stu-id="b3788-323">The new behavior alose matches EF6.</span></span>

<span data-ttu-id="b3788-324">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-324">**Mitigations**</span></span>

<span data-ttu-id="b3788-325">Se la connessione deve rimanere aperta, la chiamata esplicita di `OpenConnection()` garantirà che EF Core non la chiuda prematuramente:</span><span class="sxs-lookup"><span data-stu-id="b3788-325">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="b3788-326">Ogni proprietà usa la generazione di chiavi di tipo intero in memoria indipendenti</span><span class="sxs-lookup"><span data-stu-id="b3788-326">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="b3788-327">Problema n. 6872</span><span class="sxs-lookup"><span data-stu-id="b3788-327">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="b3788-328">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-328">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-329">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-329">**Old behavior**</span></span>

<span data-ttu-id="b3788-330">Nelle versioni precedenti a EF Core 3.0 veniva usato un unico generatore di valori condiviso per tutte le proprietà di chiavi di tipo intero in memoria.</span><span class="sxs-lookup"><span data-stu-id="b3788-330">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="b3788-331">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-331">**New behavior**</span></span>

<span data-ttu-id="b3788-332">A partire da EF Core 3.0, ogni proprietà di chiave di tipo intero riceve un generatore di valori quando viene usato il database in memoria.</span><span class="sxs-lookup"><span data-stu-id="b3788-332">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="b3788-333">Inoltre, se il database viene eliminato, la generazione di chiavi viene reimpostata per tutte le tabelle.</span><span class="sxs-lookup"><span data-stu-id="b3788-333">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="b3788-334">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-334">**Why**</span></span>

<span data-ttu-id="b3788-335">Questa modifica è stata apportata per allineare maggiormente la generazione di chiavi in memoria alla generazione di chiavi del database reale e per migliorare la possibilità di isolare i test l'uno dall'altro quando viene usato il database in memoria.</span><span class="sxs-lookup"><span data-stu-id="b3788-335">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="b3788-336">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-336">**Mitigations**</span></span>

<span data-ttu-id="b3788-337">Ciò può interrompere un'applicazione che si basa sull'impostazione di valori di chiave in memoria specifici.</span><span class="sxs-lookup"><span data-stu-id="b3788-337">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="b3788-338">È consigliabile non basare l'applicazione su valori di chiave specifici o eseguire l'aggiornamento per passare al nuovo comportamento.</span><span class="sxs-lookup"><span data-stu-id="b3788-338">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="b3788-339">I campi sottostanti vengono usati per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="b3788-339">Backing fields are used by default</span></span>

[<span data-ttu-id="b3788-340">Problema n. 12430</span><span class="sxs-lookup"><span data-stu-id="b3788-340">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="b3788-341">Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.</span><span class="sxs-lookup"><span data-stu-id="b3788-341">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="b3788-342">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-342">**Old behavior**</span></span>

<span data-ttu-id="b3788-343">Nelle versioni precedenti alla versione 3.0, anche se il campo sottostante di una proprietà era noto, per impostazione predefinita EF Core eseguiva la lettura e la scrittura del valore della proprietà usando i metodi getter e setter della proprietà.</span><span class="sxs-lookup"><span data-stu-id="b3788-343">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="b3788-344">L'eccezione era costituita dall'esecuzione di query in cui il campo sottostante, se noto, veniva impostato direttamente.</span><span class="sxs-lookup"><span data-stu-id="b3788-344">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="b3788-345">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-345">**New behavior**</span></span>

<span data-ttu-id="b3788-346">A partire da EF Core 3.0, se il campo sottostante di una proprietà è noto, la lettura e la scrittura della proprietà vengono sempre eseguite usando il campo sottostante.</span><span class="sxs-lookup"><span data-stu-id="b3788-346">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="b3788-347">Ciò potrebbe causare un'interruzione dell'applicazione se l'applicazione si basa su un comportamento aggiuntivo codificato nei metodi getter o setter.</span><span class="sxs-lookup"><span data-stu-id="b3788-347">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="b3788-348">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-348">**Why**</span></span>

<span data-ttu-id="b3788-349">Questa modifica è stata apportata per impedire a EF Core di attivare per errore la logica di business per impostazione predefinita quando si eseguono operazioni di database che interessano le entità.</span><span class="sxs-lookup"><span data-stu-id="b3788-349">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="b3788-350">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-350">**Mitigations**</span></span>

<span data-ttu-id="b3788-351">È possibile ripristinare il comportamento delle versioni precedenti alla versione 3.0 tramite la configurazione della modalità di accesso delle proprietà nell'API Fluent modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="b3788-351">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="b3788-352">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b3788-352">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="b3788-353">Viene generata un'eccezione se vengono trovati più campi sottostanti compatibili</span><span class="sxs-lookup"><span data-stu-id="b3788-353">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="b3788-354">Problema n. 12523</span><span class="sxs-lookup"><span data-stu-id="b3788-354">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="b3788-355">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-355">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-356">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-356">**Old behavior**</span></span>

<span data-ttu-id="b3788-357">Nelle versioni precedenti a EF Core 3.0, se più campi soddisfacevano le regole di ricerca del campo sottostante di una proprietà, veniva selezionato un solo campo in base a un ordine di precedenza.</span><span class="sxs-lookup"><span data-stu-id="b3788-357">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="b3788-358">Ciò poteva causare l'uso di un campo non corretto nei casi ambigui.</span><span class="sxs-lookup"><span data-stu-id="b3788-358">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="b3788-359">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-359">**New behavior**</span></span>

<span data-ttu-id="b3788-360">A partire da EF Core 3.0, se più campi corrispondono alla stessa proprietà, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="b3788-360">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="b3788-361">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-361">**Why**</span></span>

<span data-ttu-id="b3788-362">Questa modifica è stata apportata per evitare di usare automaticamente un campo rispetto a un altro quando un solo campo può essere quello corretto.</span><span class="sxs-lookup"><span data-stu-id="b3788-362">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="b3788-363">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-363">**Mitigations**</span></span>

<span data-ttu-id="b3788-364">Per le proprietà con campi sottostanti ambigui, il campo da usare deve essere specificato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="b3788-364">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="b3788-365">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="b3788-365">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="b3788-366">I nomi delle proprietà solo campo devono corrispondere al nome di campo</span><span class="sxs-lookup"><span data-stu-id="b3788-366">Field-only property names should match the field name</span></span>

<span data-ttu-id="b3788-367">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-367">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-368">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-368">**Old behavior**</span></span>

<span data-ttu-id="b3788-369">Prima di EF Core 3.0, era possibile specificare una proprietà con un valore stringa e se non veniva trovata alcuna proprietà con tale nome nel tipo CLR, EF Core tentava di trovare una corrispondenza con un campo tramite regole di convenzione.</span><span class="sxs-lookup"><span data-stu-id="b3788-369">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convetion rules.</span></span>
```C#
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```
```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="b3788-370">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-370">**New behavior**</span></span>

<span data-ttu-id="b3788-371">A partire da EF Core 3.0, una proprietà solo campo deve corrispondere esattamente al nome del campo.</span><span class="sxs-lookup"><span data-stu-id="b3788-371">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="b3788-372">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-372">**Why**</span></span>

<span data-ttu-id="b3788-373">Questa modifica è stata introdotta per evitare di usare lo stesso campo per due proprietà con nome simile. Le regole di corrispondenza per le proprietà solo campo sono state anche uniformate a quelle per le proprietà mappate a proprietà CLR.</span><span class="sxs-lookup"><span data-stu-id="b3788-373">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="b3788-374">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-374">**Mitigations**</span></span>

<span data-ttu-id="b3788-375">Le proprietà solo campo devono avere lo stesso nome del campo a cui vengono mappate.</span><span class="sxs-lookup"><span data-stu-id="b3788-375">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="b3788-376">In un'anteprima successiva di EF Core 3.0 è prevista la riabilitazione della possibilità di configurare in modo esplicito un nome di campo diverso dal nome della proprietà:</span><span class="sxs-lookup"><span data-stu-id="b3788-376">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="b3788-377">AddDbContext/AddDbContextPool non chiamano più AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="b3788-377">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="b3788-378">Problema n. 14756</span><span class="sxs-lookup"><span data-stu-id="b3788-378">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="b3788-379">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-379">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-380">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-380">**Old behavior**</span></span>

<span data-ttu-id="b3788-381">Prima di EF Core 3.0, la chiamata di `AddDbContext` oppure `AddDbContextPool` comporta anche la registrazione dei servizi di registrazione e memorizzazione nella cache con inserimento delle dipendenze tramite chiamate a [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="b3788-381">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="b3788-382">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-382">**New behavior**</span></span>

<span data-ttu-id="b3788-383">A partire da EF Core 3.0, `AddDbContext` e `AddDbContextPool` non registreranno più questi servizi con inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="b3788-383">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="b3788-384">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-384">**Why**</span></span>

<span data-ttu-id="b3788-385">EF Core 3.0 non richiede che questi servizi siano inclusi nel contenitore di inserimento delle dipendenze dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b3788-385">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="b3788-386">Tuttavia, se `ILoggerFactory` è registrato nel contenitore di inserimento delle dipendenze dell'applicazione, verrà ancora usato da EF Core.</span><span class="sxs-lookup"><span data-stu-id="b3788-386">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="b3788-387">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-387">**Mitigations**</span></span>

<span data-ttu-id="b3788-388">Se l'applicazione necessita di questi servizi, registrarli in modo esplicito con il contenitore di inserimento delle dipendenze usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="b3788-388">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="b3788-389">DbContext.Entry esegue ora un DetectChanges locale</span><span class="sxs-lookup"><span data-stu-id="b3788-389">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="b3788-390">Problema n. 13552</span><span class="sxs-lookup"><span data-stu-id="b3788-390">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="b3788-391">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="b3788-391">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3788-392">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-392">**Old behavior**</span></span>

<span data-ttu-id="b3788-393">Nelle versioni precedenti a EF Core 3.0 la chiamata a `DbContext.Entry` causava il rilevamento delle modifiche per tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="b3788-393">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="b3788-394">Ciò garantiva l'aggiornamento dello stato esposto in `EntityEntry`.</span><span class="sxs-lookup"><span data-stu-id="b3788-394">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="b3788-395">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-395">**New behavior**</span></span>

<span data-ttu-id="b3788-396">A partire da EF Core 3.0, la chiamata a `DbContext.Entry` causa ora solo il tentativo di rilevare le modifiche nell'entità specificata e in tutte le relative entità di sicurezza rilevate.</span><span class="sxs-lookup"><span data-stu-id="b3788-396">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="b3788-397">Ciò significa che le modifiche apportate altrove potrebbero non essere state rilevate tramite la chiamata al metodo e ciò potrebbe avere implicazioni sullo stato dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b3788-397">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="b3788-398">Si noti che se `ChangeTracker.AutoDetectChangesEnabled` è impostato su `false`, verrà disabilitato anche questo tipo di rilevamento delle modifiche locali.</span><span class="sxs-lookup"><span data-stu-id="b3788-398">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="b3788-399">Gli altri metodi che causano il rilevamento delle modifiche, ad esempio `ChangeTracker.Entries` e `SaveChanges`, causano ancora un `DetectChanges` completo di tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="b3788-399">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="b3788-400">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-400">**Why**</span></span>

<span data-ttu-id="b3788-401">Questa modifica è stata apportata per migliorare le prestazioni predefinite dell'uso di `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="b3788-401">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="b3788-402">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-402">**Mitigations**</span></span>

<span data-ttu-id="b3788-403">Chiamare `ChgangeTracker.DetectChanges()` in modo esplicito prima di chiamare `Entry` per garantire il comportamento precedente alla versione 3.0.</span><span class="sxs-lookup"><span data-stu-id="b3788-403">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="b3788-404">Le chiavi matrice di byte e di stringhe non vengono generate dal client per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="b3788-404">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="b3788-405">Problema n. 14617</span><span class="sxs-lookup"><span data-stu-id="b3788-405">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="b3788-406">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-406">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-407">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-407">**Old behavior**</span></span>

<span data-ttu-id="b3788-408">Nelle versioni precedenti a EF Core 3.0 le proprietà di chiave `string` e `byte[]` potevano essere usate senza impostare in modo esplicito un valore non Null.</span><span class="sxs-lookup"><span data-stu-id="b3788-408">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="b3788-409">In questi casi, il valore di chiave veniva generato nel client come GUID, serializzato in byte per `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="b3788-409">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="b3788-410">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-410">**New behavior**</span></span>

<span data-ttu-id="b3788-411">A partire da EF Core 3.0 viene generata un'eccezione che indica che non è stato impostato alcun valore di chiave.</span><span class="sxs-lookup"><span data-stu-id="b3788-411">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="b3788-412">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-412">**Why**</span></span>

<span data-ttu-id="b3788-413">Questa modifica è stata apportata poiché i valori `string`/`byte[]` generati dal client non risultano in genere utili e il comportamento predefinito rendeva complesso ragionare sui valori di chiave generati in un modo comune.</span><span class="sxs-lookup"><span data-stu-id="b3788-413">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="b3788-414">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-414">**Mitigations**</span></span>

<span data-ttu-id="b3788-415">È possibile ripristinare il comportamento precedente alla versione 3.0 specificando in modo esplicito che le proprietà di chiave devono usare i valori generati se non viene impostato alcun altro valore non Null.</span><span class="sxs-lookup"><span data-stu-id="b3788-415">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="b3788-416">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="b3788-416">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="b3788-417">Oppure con annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="b3788-417">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="b3788-418">ILoggerFactory è ora un servizio con ambito</span><span class="sxs-lookup"><span data-stu-id="b3788-418">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="b3788-419">Problema n. 14698</span><span class="sxs-lookup"><span data-stu-id="b3788-419">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="b3788-420">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="b3788-420">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3788-421">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-421">**Old behavior**</span></span>

<span data-ttu-id="b3788-422">Nelle versioni precedenti a EF Core 3.0 `ILoggerFactory` veniva registrato come servizio singleton.</span><span class="sxs-lookup"><span data-stu-id="b3788-422">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="b3788-423">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-423">**New behavior**</span></span>

<span data-ttu-id="b3788-424">A partire da EF Core 3.0, `ILoggerFactory` viene registrato come servizio con ambito.</span><span class="sxs-lookup"><span data-stu-id="b3788-424">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="b3788-425">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-425">**Why**</span></span>

<span data-ttu-id="b3788-426">Questa modifica è stata apportata per consentire l'associazione di un logger a un'istanza `DbContext` che abilita altre funzionalità e rimuove alcuni casi di comportamento anomalo, ad esempio un'esplosione dei provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="b3788-426">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="b3788-427">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-427">**Mitigations**</span></span>

<span data-ttu-id="b3788-428">Questa modifica non dovrebbe influire sul codice dell'applicazione a meno che non vengano registrati e usati servizi personalizzati nel provider di servizi interno di EF Core.</span><span class="sxs-lookup"><span data-stu-id="b3788-428">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="b3788-429">Questa condizione, tuttavia, non è comune.</span><span class="sxs-lookup"><span data-stu-id="b3788-429">This isn't common.</span></span>
<span data-ttu-id="b3788-430">In questi casi la maggior parte delle operazioni continuano a essere eseguite correttamente, ma qualsiasi servizio singleton dipendente da `ILoggerFactory` dovrà essere modificato per ottenere `ILoggerFactory` in modo diverso.</span><span class="sxs-lookup"><span data-stu-id="b3788-430">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="b3788-431">Se si verificano situazioni simili, inviare una segnalazione nello [strumento di gestione dei problemi in GitHub relativo a EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) per comunicare in che modo viene usato `ILoggerFactory` per consentirci di comprendere meglio come evitare ulteriori interruzioni in futuro.</span><span class="sxs-lookup"><span data-stu-id="b3788-431">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="b3788-432">IDbContextOptionsExtensionWithDebugInfo unito in IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="b3788-432">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="b3788-433">Problema n. 13552</span><span class="sxs-lookup"><span data-stu-id="b3788-433">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="b3788-434">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="b3788-434">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3788-435">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-435">**Old behavior**</span></span>

<span data-ttu-id="b3788-436">`IDbContextOptionsExtensionWithDebugInfo` era un'interfaccia facoltativa aggiuntiva estesa da `IDbContextOptionsExtension` che consentiva di evitare le modifiche che causavano interruzioni nell'interfaccia durante il ciclo di versioni 2.x.</span><span class="sxs-lookup"><span data-stu-id="b3788-436">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="b3788-437">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-437">**New behavior**</span></span>

<span data-ttu-id="b3788-438">Le interfacce sono ora unite in `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="b3788-438">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="b3788-439">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-439">**Why**</span></span>

<span data-ttu-id="b3788-440">Questa modifica è stata apportata perché le interfacce sono concettualmente un'unica interfaccia.</span><span class="sxs-lookup"><span data-stu-id="b3788-440">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="b3788-441">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-441">**Mitigations**</span></span>

<span data-ttu-id="b3788-442">Tutte le implementazioni di `IDbContextOptionsExtension` dovranno essere aggiornate per supportare il nuovo membro.</span><span class="sxs-lookup"><span data-stu-id="b3788-442">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="b3788-443">I proxy di caricamento lazy non presuppongono più che le proprietà di navigazione vengano caricate completamente</span><span class="sxs-lookup"><span data-stu-id="b3788-443">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="b3788-444">Problema n. 12780</span><span class="sxs-lookup"><span data-stu-id="b3788-444">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="b3788-445">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-445">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-446">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-446">**Old behavior**</span></span>

<span data-ttu-id="b3788-447">Nelle versioni precedenti a EF Core 3.0, quando `DbContext` veniva eliminato non esisteva alcun metodo per scoprire se una determinata proprietà di navigazione in un'entità ottenuta da un contesto specifico veniva caricata completamente o meno.</span><span class="sxs-lookup"><span data-stu-id="b3788-447">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="b3788-448">I proxy presupponevano invece che venisse caricata una navigazione di riferimento se era presente un valore non Null e che venisse caricata una navigazione di raccolta se era presente un valore.</span><span class="sxs-lookup"><span data-stu-id="b3788-448">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="b3788-449">In questi casi il tentativo di eseguire un caricamento lazy non avrebbe avuto alcun esito.</span><span class="sxs-lookup"><span data-stu-id="b3788-449">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="b3788-450">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-450">**New behavior**</span></span>

<span data-ttu-id="b3788-451">A partire da Entity Framework Core 3.0, i proxy tengono traccia del caricamento o mancato caricamento di una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="b3788-451">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="b3788-452">Ciò significa che il tentativo di accedere a una proprietà di navigazione caricata dopo l'eliminazione del contesto non avrà mai alcun esito, anche quando la navigazione caricata è vuota o ha valore Null.</span><span class="sxs-lookup"><span data-stu-id="b3788-452">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="b3788-453">Al contrario, il tentativo di accedere a una proprietà di navigazione non caricata genera un'eccezione se il contesto viene eliminato anche se la proprietà di navigazione è una raccolta non vuota.</span><span class="sxs-lookup"><span data-stu-id="b3788-453">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="b3788-454">Se si verifica questa situazione significa che il codice dell'applicazione sta tentando di usare il caricamento lazy in un momento non valido e l'applicazione deve essere modificata in modo da non eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="b3788-454">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="b3788-455">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-455">**Why**</span></span>

<span data-ttu-id="b3788-456">Questa modifica è stata apportata per rendere coerente e corretto il comportamento durante un tentativo di caricamento lazy in un'istanza `DbContext` eliminata.</span><span class="sxs-lookup"><span data-stu-id="b3788-456">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="b3788-457">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-457">**Mitigations**</span></span>

<span data-ttu-id="b3788-458">Aggiornare il codice dell'applicazione per fare in modo che non venga tentato il caricamento lazy con un contesto eliminato oppure specificare una configurazione in modo che non venga eseguita alcuna operazione come descritto nel messaggio di eccezione.</span><span class="sxs-lookup"><span data-stu-id="b3788-458">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="b3788-459">La creazione di un numero eccessivo di provider di servizi interni è ora un errore per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="b3788-459">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="b3788-460">Problema n. 10236</span><span class="sxs-lookup"><span data-stu-id="b3788-460">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="b3788-461">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="b3788-461">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3788-462">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-462">**Old behavior**</span></span>

<span data-ttu-id="b3788-463">Nelle versioni precedenti a EF Core 3.0 veniva registrato un avviso per le applicazioni che creavano un numero eccessivo di provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="b3788-463">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="b3788-464">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-464">**New behavior**</span></span>

<span data-ttu-id="b3788-465">A partire da EF Core 3.0, l'avviso viene considerato un errore e viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="b3788-465">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="b3788-466">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-466">**Why**</span></span>

<span data-ttu-id="b3788-467">Questa modifica è stata apportata per gestire meglio il codice dell'applicazione tramite un'esposizione più esplicita di questa situazione di errore.</span><span class="sxs-lookup"><span data-stu-id="b3788-467">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="b3788-468">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-468">**Mitigations**</span></span>

<span data-ttu-id="b3788-469">L'azione più appropriata quando si verifica questo errore consiste nell'individuare la causa radice e nell'interrompere la creazione di numerosi provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="b3788-469">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="b3788-470">È possibile tuttavia convertire nuovamente l'errore in avviso o ignorarlo tramite una configurazione in `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b3788-470">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="b3788-471">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b3788-471">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="b3788-472">Nuovo comportamento per la chiamata di HasOne/HasMany con una singola stringa</span><span class="sxs-lookup"><span data-stu-id="b3788-472">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="b3788-473">Problema n. 9171</span><span class="sxs-lookup"><span data-stu-id="b3788-473">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="b3788-474">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-474">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-475">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-475">**Old behavior**</span></span>

<span data-ttu-id="b3788-476">Prima di EF Core 3.0, il codice che chiama `HasOne` o `HasMany` con una singola stringa era interpretato in modo poco chiaro.</span><span class="sxs-lookup"><span data-stu-id="b3788-476">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="b3788-477">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b3788-477">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="b3788-478">Apparentemente, il codice mette in relazione `Samurai` con un altro tipo di entità tramite la proprietà di navigazione `Entrance`, che può essere privata.</span><span class="sxs-lookup"><span data-stu-id="b3788-478">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="b3788-479">In realtà, il codice tenta di creare una relazione con un tipo di entità denominato `Entrance` senza proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="b3788-479">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="b3788-480">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-480">**New behavior**</span></span>

<span data-ttu-id="b3788-481">A partire da EF Core 3.0, il codice sopra riportato ora esegue quello che avrebbe dovuto fare in precedenza.</span><span class="sxs-lookup"><span data-stu-id="b3788-481">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="b3788-482">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-482">**Why**</span></span>

<span data-ttu-id="b3788-483">Il comportamento precedente era molto poco chiaro, soprattutto durante la lettura del codice di configurazione e la ricerca di errori.</span><span class="sxs-lookup"><span data-stu-id="b3788-483">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="b3788-484">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-484">**Mitigations**</span></span>

<span data-ttu-id="b3788-485">Questa modifica causerà problemi solo nelle applicazioni che configurano relazioni in modo esplicito usando stringhe per i nomi dei tipi e senza specificare in modo esplicito la proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="b3788-485">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="b3788-486">Non è uno scenario comune.</span><span class="sxs-lookup"><span data-stu-id="b3788-486">This is not common.</span></span>
<span data-ttu-id="b3788-487">Il comportamento precedente può essere ottenuto passando esplicitamente `null` per il nome della proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="b3788-487">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="b3788-488">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b3788-488">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="b3788-489">Il tipo restituito per diversi metodi asincroni è cambiato da Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="b3788-489">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="b3788-490">Problema n. 15184</span><span class="sxs-lookup"><span data-stu-id="b3788-490">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="b3788-491">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-491">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-492">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-492">**Old behavior**</span></span>

<span data-ttu-id="b3788-493">I metodi asincroni seguenti in precedenza restituivano il tipo `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="b3788-493">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="b3788-494">`ValueGenerator.NextValueAsync()` (e classi derivate)</span><span class="sxs-lookup"><span data-stu-id="b3788-494">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="b3788-495">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-495">**New behavior**</span></span>

<span data-ttu-id="b3788-496">I metodi indicati in precedenza ora restituiscono il tipo `ValueTask<T>` sullo stesso `T` come in precedenza.</span><span class="sxs-lookup"><span data-stu-id="b3788-496">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="b3788-497">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-497">**Why**</span></span>

<span data-ttu-id="b3788-498">Questa modifica riduce il numero delle allocazioni di heap sostenute quando si richiamano questi metodi, con un miglioramento generale delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="b3788-498">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="b3788-499">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-499">**Mitigations**</span></span>

<span data-ttu-id="b3788-500">Le applicazioni semplicemente in attesa delle API precedenti devono solo essere ricompilate e non sono richieste modifiche del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="b3788-500">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="b3788-501">Per scenari di utilizzo più complessi (ad esempio, il passaggio del tipo `Task` restituito a `Task.WhenAny()`) è richiesto in genere che il tipo `ValueTask<T>` restituito venga convertito in `Task<T>` chiamando `AsTask()` su di esso.</span><span class="sxs-lookup"><span data-stu-id="b3788-501">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="b3788-502">Si noti che in questo modo si annulla la riduzione delle allocazioni consentita da questa modifica.</span><span class="sxs-lookup"><span data-stu-id="b3788-502">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="b3788-503">L'annotazione Relational:TypeMapping è ora TypeMapping</span><span class="sxs-lookup"><span data-stu-id="b3788-503">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="b3788-504">Problema n. 9913</span><span class="sxs-lookup"><span data-stu-id="b3788-504">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="b3788-505">Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.</span><span class="sxs-lookup"><span data-stu-id="b3788-505">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="b3788-506">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-506">**Old behavior**</span></span>

<span data-ttu-id="b3788-507">Il nome di annotazione delle annotazioni di mapping del tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="b3788-507">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="b3788-508">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-508">**New behavior**</span></span>

<span data-ttu-id="b3788-509">Il nome di annotazione delle annotazioni di mapping del tipo è ora "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="b3788-509">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="b3788-510">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-510">**Why**</span></span>

<span data-ttu-id="b3788-511">Il mapping dei tipi non viene più usato solo per i provider di database relazionali.</span><span class="sxs-lookup"><span data-stu-id="b3788-511">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="b3788-512">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-512">**Mitigations**</span></span>

<span data-ttu-id="b3788-513">Ciò causa un'interruzione solo nelle applicazioni che accedono al mapping dei tipi direttamente come annotazione. Questa situazione non è comune.</span><span class="sxs-lookup"><span data-stu-id="b3788-513">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="b3788-514">L'azione più appropriata per risolvere il problema consiste nell'usare la superficie dell'API per accedere al mapping dei tipi anziché l'annotazione.</span><span class="sxs-lookup"><span data-stu-id="b3788-514">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="b3788-515">ToTable in un tipo derivato genera un'eccezione</span><span class="sxs-lookup"><span data-stu-id="b3788-515">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="b3788-516">Problema n. 11811</span><span class="sxs-lookup"><span data-stu-id="b3788-516">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="b3788-517">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="b3788-517">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3788-518">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-518">**Old behavior**</span></span>

<span data-ttu-id="b3788-519">Nelle versioni precedenti a EF Core 3.0, la chiamata a `ToTable()` in un tipo derivato veniva ignorata poiché soltanto la strategia di mapping dell'ereditarietà era una tabella per gerarchia dove la chiamata non era valida.</span><span class="sxs-lookup"><span data-stu-id="b3788-519">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="b3788-520">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-520">**New behavior**</span></span>

<span data-ttu-id="b3788-521">A partire da EF Core 3.0 e in preparazione all'aggiunta del supporto per la tabella per tipo e per TPC in una versione successiva, la chiamata a `ToTable()` in un tipo derivato genera un'eccezione per evitare una modifica del mapping imprevista in futuro.</span><span class="sxs-lookup"><span data-stu-id="b3788-521">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="b3788-522">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-522">**Why**</span></span>

<span data-ttu-id="b3788-523">Attualmente il mapping di un tipo derivato in una tabella diversa non è un'operazione valida.</span><span class="sxs-lookup"><span data-stu-id="b3788-523">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="b3788-524">Questa modifica consente di evitare interruzioni future quando l'operazione diventerà un'operazione valida.</span><span class="sxs-lookup"><span data-stu-id="b3788-524">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="b3788-525">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-525">**Mitigations**</span></span>

<span data-ttu-id="b3788-526">Rimuovere qualsiasi tentativo di mapping di tipi derivati in altre tabelle.</span><span class="sxs-lookup"><span data-stu-id="b3788-526">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="b3788-527">ForSqlServerHasIndex sostituito con HasIndex</span><span class="sxs-lookup"><span data-stu-id="b3788-527">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="b3788-528">Problema n. 12366</span><span class="sxs-lookup"><span data-stu-id="b3788-528">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="b3788-529">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="b3788-529">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3788-530">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-530">**Old behavior**</span></span>

<span data-ttu-id="b3788-531">Nelle versioni precedenti a EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offriva un metodo per configurare le colonne usate con `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="b3788-531">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="b3788-532">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-532">**New behavior**</span></span>

<span data-ttu-id="b3788-533">A partire da EF Core 3.0, l'uso di `Include` in un indice è supportato a livello relazionale.</span><span class="sxs-lookup"><span data-stu-id="b3788-533">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="b3788-534">Usare `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="b3788-534">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="b3788-535">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-535">**Why**</span></span>

<span data-ttu-id="b3788-536">Questa modifica è stata apportata per consolidare l'API per gli indici con `Include` in un'unica posizione per tutti i provider di database.</span><span class="sxs-lookup"><span data-stu-id="b3788-536">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="b3788-537">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-537">**Mitigations**</span></span>

<span data-ttu-id="b3788-538">Usare la nuova API, come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="b3788-538">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="b3788-539">Modifiche dell'API dei metadati</span><span class="sxs-lookup"><span data-stu-id="b3788-539">Metadata API changes</span></span>

[<span data-ttu-id="b3788-540">Problema n. 214</span><span class="sxs-lookup"><span data-stu-id="b3788-540">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="b3788-541">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-541">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-542">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-542">**New behavior**</span></span>

<span data-ttu-id="b3788-543">Le proprietà seguenti sono state convertite in metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="b3788-543">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="b3788-544">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-544">**Why**</span></span>

<span data-ttu-id="b3788-545">Questa modifica semplifica l'implementazione delle interfacce menzionate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="b3788-545">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="b3788-546">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-546">**Mitigations**</span></span>

<span data-ttu-id="b3788-547">Usare i nuovi metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="b3788-547">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="b3788-548">EF Core non invia più pragma per l'imposizione della chiave esterna di SQLite</span><span class="sxs-lookup"><span data-stu-id="b3788-548">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="b3788-549">Problema n. 12151</span><span class="sxs-lookup"><span data-stu-id="b3788-549">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="b3788-550">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="b3788-550">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="b3788-551">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-551">**Old behavior**</span></span>

<span data-ttu-id="b3788-552">Nelle versioni precedenti a EF Core 3.0, EF Core inviava `PRAGMA foreign_keys = 1` quando veniva aperta una connessione a SQLite.</span><span class="sxs-lookup"><span data-stu-id="b3788-552">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="b3788-553">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-553">**New behavior**</span></span>

<span data-ttu-id="b3788-554">A partire da EF Core 3.0, EF Core non invia più `PRAGMA foreign_keys = 1` quando viene aperta una connessione a SQLite.</span><span class="sxs-lookup"><span data-stu-id="b3788-554">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="b3788-555">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-555">**Why**</span></span>

<span data-ttu-id="b3788-556">Questa modifica è stata apportata poiché EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` per impostazione predefinita. Ciò significa che l'imposizione della chiave esterna è abilitata per impostazione predefinita e non deve essere abilitata in modo esplicito ogni volta che viene aperta una connessione.</span><span class="sxs-lookup"><span data-stu-id="b3788-556">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="b3788-557">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-557">**Mitigations**</span></span>

<span data-ttu-id="b3788-558">Le chiavi esterne sono abilitate per impostazione predefinita in SQLitePCLRaw.bundle_e_sqlite3, usato per impostazione predefinita per EF Core.</span><span class="sxs-lookup"><span data-stu-id="b3788-558">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="b3788-559">Per gli altri casi, è possibile abilitare le chiavi esterne specificando `Foreign Keys=True` nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="b3788-559">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="b3788-560">Microsoft.EntityFrameworkCore.Sqlite dipende ora da SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="b3788-560">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="b3788-561">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-561">**Old behavior**</span></span>

<span data-ttu-id="b3788-562">Nelle versioni precedenti a EF Core 3.0, EF Core usava `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="b3788-562">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="b3788-563">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-563">**New behavior**</span></span>

<span data-ttu-id="b3788-564">A partire da EF Core 3.0, EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="b3788-564">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="b3788-565">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-565">**Why**</span></span>

<span data-ttu-id="b3788-566">Questa modifica è stata apportata per rendere coerente la versione di SQLite usata in iOS con le altre piattaforme.</span><span class="sxs-lookup"><span data-stu-id="b3788-566">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="b3788-567">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-567">**Mitigations**</span></span>

<span data-ttu-id="b3788-568">Per usare la versione di SQLite nativa in iOS, configurare `Microsoft.Data.Sqlite` per l'uso di un'aggregazione `SQLitePCLRaw` diversa.</span><span class="sxs-lookup"><span data-stu-id="b3788-568">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="b3788-569">I valori Guid vengono ora archiviati come TEXT in SQLite</span><span class="sxs-lookup"><span data-stu-id="b3788-569">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="b3788-570">Problema n. 15078</span><span class="sxs-lookup"><span data-stu-id="b3788-570">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="b3788-571">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-571">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-572">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-572">**Old behavior**</span></span>

<span data-ttu-id="b3788-573">I valori Guid in precedenza venivano archiviati come valori BLOB in SQLite.</span><span class="sxs-lookup"><span data-stu-id="b3788-573">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="b3788-574">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-574">**New behavior**</span></span>

<span data-ttu-id="b3788-575">I valori Guid vengono ora archiviati come TEXT.</span><span class="sxs-lookup"><span data-stu-id="b3788-575">Guid values are now sotred as TEXT.</span></span>

<span data-ttu-id="b3788-576">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-576">**Why**</span></span>

<span data-ttu-id="b3788-577">Il formato binario dei valori Guid non è standardizzato.</span><span class="sxs-lookup"><span data-stu-id="b3788-577">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="b3788-578">L'archiviazione dei valori come TEXT rende il database più compatibile con altre tecnologie.</span><span class="sxs-lookup"><span data-stu-id="b3788-578">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="b3788-579">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-579">**Mitigations**</span></span>

<span data-ttu-id="b3788-580">È possibile eseguire la migrazione dei database esistenti al nuovo formato eseguendo SQL nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="b3788-580">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="b3788-581">In EF Core è anche possibile continuare a usare il comportamento precedente configurando un convertitore di valori in queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="b3788-581">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="b3788-582">Microsoft.Data.Sqlite rimane in grado di leggere i valori Guid sia da colonne BLOB che TEXT. Tuttavia, poiché è stato modificato il formato predefinito per i parametri e le costanti, probabilmente sarà necessario intervenire per la maggior parte degli scenari che coinvolgono valori Guid.</span><span class="sxs-lookup"><span data-stu-id="b3788-582">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="b3788-583">I valori char vengono ora archiviati come testo in SQLite</span><span class="sxs-lookup"><span data-stu-id="b3788-583">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="b3788-584">Problema n. 15020</span><span class="sxs-lookup"><span data-stu-id="b3788-584">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="b3788-585">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-585">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-586">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-586">**Old behavior**</span></span>

<span data-ttu-id="b3788-587">I valori char in precedenza venivano archiviati come valori interi in SQLite.</span><span class="sxs-lookup"><span data-stu-id="b3788-587">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="b3788-588">Un valore char di *A* veniva ad esempio archiviato come valore intero 65.</span><span class="sxs-lookup"><span data-stu-id="b3788-588">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="b3788-589">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-589">**New behavior**</span></span>

<span data-ttu-id="b3788-590">I valori char vengono ora archiviati come testo.</span><span class="sxs-lookup"><span data-stu-id="b3788-590">Char values are now sotred as TEXT.</span></span>

<span data-ttu-id="b3788-591">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-591">**Why**</span></span>

<span data-ttu-id="b3788-592">L'archiviazione dei valori come testo è un'operazione più naturale e rende il database più compatibile con altre tecnologie.</span><span class="sxs-lookup"><span data-stu-id="b3788-592">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="b3788-593">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-593">**Mitigations**</span></span>

<span data-ttu-id="b3788-594">È possibile eseguire la migrazione dei database esistenti al nuovo formato eseguendo SQL nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="b3788-594">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="b3788-595">In EF Core è anche possibile continuare a usare il comportamento precedente configurando un convertitore di valori in queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="b3788-595">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="b3788-596">Microsoft.Data.Sqlite rimane comunque in grado di leggere i valori di caratteri presenti sia nelle colonne di valori interi sia in quelle di testo, quindi alcuni scenari potrebbero non richiedere alcuna azione.</span><span class="sxs-lookup"><span data-stu-id="b3788-596">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="b3788-597">Gli ID di migrazione vengono ora generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica</span><span class="sxs-lookup"><span data-stu-id="b3788-597">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="b3788-598">Problema n. 12978</span><span class="sxs-lookup"><span data-stu-id="b3788-598">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="b3788-599">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-599">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-600">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-600">**Old behavior**</span></span>

<span data-ttu-id="b3788-601">Gli ID di migrazione venivano inavvertitamente generati usando con il calendario delle impostazioni cultura correnti.</span><span class="sxs-lookup"><span data-stu-id="b3788-601">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

<span data-ttu-id="b3788-602">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-602">**New behavior**</span></span>

<span data-ttu-id="b3788-603">Gli ID di migrazione ora vengono sempre generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica (calendario gregoriano).</span><span class="sxs-lookup"><span data-stu-id="b3788-603">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="b3788-604">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-604">**Why**</span></span>

<span data-ttu-id="b3788-605">L'ordine delle migrazioni è importante quando si esegue l'aggiornamento del database o si risolvono i conflitti di unione.</span><span class="sxs-lookup"><span data-stu-id="b3788-605">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="b3788-606">L'uso del calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica evita i problemi che possono verificarsi quando i membri del team hanno calendari di sistema diversi.</span><span class="sxs-lookup"><span data-stu-id="b3788-606">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="b3788-607">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-607">**Mitigations**</span></span>

<span data-ttu-id="b3788-608">Questa modifica interessa gli utenti che usano un calendario non gregoriano in cui l'anno ha un'estensione superiore al calendario gregoriano (come il calendario buddista tailandese).</span><span class="sxs-lookup"><span data-stu-id="b3788-608">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="b3788-609">Gli ID di migrazione esistenti dovranno essere aggiornati in modo che le nuove migrazioni vengano collocate dopo le migrazioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="b3788-609">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="b3788-610">L'ID di migrazione è disponibile nell'attributo di migrazione presente nei file di progettazione delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="b3788-610">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="b3788-611">È necessario aggiornare anche la tabella della cronologia delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="b3788-611">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="b3788-612">LogQueryPossibleExceptionWithAggregateOperator è stato rinominato</span><span class="sxs-lookup"><span data-stu-id="b3788-612">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="b3788-613">Problema n. 10985</span><span class="sxs-lookup"><span data-stu-id="b3788-613">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="b3788-614">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-614">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-615">**Modifica**</span><span class="sxs-lookup"><span data-stu-id="b3788-615">**Change**</span></span>

<span data-ttu-id="b3788-616">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` è stato rinominato in `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="b3788-616">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="b3788-617">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-617">**Why**</span></span>

<span data-ttu-id="b3788-618">Allineamento del nome di questo evento di avviso con tutti gli altri eventi di avviso.</span><span class="sxs-lookup"><span data-stu-id="b3788-618">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="b3788-619">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-619">**Mitigations**</span></span>

<span data-ttu-id="b3788-620">Usare il nuovo nome.</span><span class="sxs-lookup"><span data-stu-id="b3788-620">Use the new name.</span></span> <span data-ttu-id="b3788-621">(Si noti che il numero di ID evento non è stato modificato.)</span><span class="sxs-lookup"><span data-stu-id="b3788-621">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="b3788-622">Chiarimenti per l'API per i nomi di vincolo di chiave esterna</span><span class="sxs-lookup"><span data-stu-id="b3788-622">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="b3788-623">Problema n. 10730</span><span class="sxs-lookup"><span data-stu-id="b3788-623">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="b3788-624">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="b3788-624">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="b3788-625">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="b3788-625">**Old behavior**</span></span>

<span data-ttu-id="b3788-626">Prima di EF Core 3.0, si faceva riferimento ai nomi di vincolo di chiave esterna semplicemente con "Name".</span><span class="sxs-lookup"><span data-stu-id="b3788-626">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="b3788-627">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b3788-627">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="b3788-628">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="b3788-628">**New behavior**</span></span>

<span data-ttu-id="b3788-629">A partire da EF Core 3.0, si fa ora riferimento ai nomi di vincolo di chiave esterna con "ConstraintName".</span><span class="sxs-lookup"><span data-stu-id="b3788-629">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constaint name".</span></span> <span data-ttu-id="b3788-630">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b3788-630">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="b3788-631">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="b3788-631">**Why**</span></span>

<span data-ttu-id="b3788-632">Questa modifica introduce coerenza per la denominazione in quest'area e chiarisce anche che si tratta del nome del vincolo di chiave esterna e non del nome della colonna o della proprietà per cui è definita la chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="b3788-632">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constaint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="b3788-633">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="b3788-633">**Mitigations**</span></span>

<span data-ttu-id="b3788-634">Usare il nuovo nome.</span><span class="sxs-lookup"><span data-stu-id="b3788-634">Use the new name.</span></span>
