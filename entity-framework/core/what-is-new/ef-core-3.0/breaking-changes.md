---
title: Modifiche che causano un'interruzione in EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 534ac95cccc03e9797ba766e601e2fe86eaf8061
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319218"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="a2208-102">Modifiche che causano un'interruzione incluse in EF Core 3.0 (attualmente in anteprima)</span><span class="sxs-lookup"><span data-stu-id="a2208-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a2208-103">Si tenga presente che i set di funzionalità e le pianificazioni delle versioni future sono sempre soggette a modifiche e che questa pagina, nonostante l'impegno profuso per mantenerla aggiornata, potrebbe non riflettere sempre i piani più recenti.</span><span class="sxs-lookup"><span data-stu-id="a2208-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="a2208-104">Le modifiche all'API e al comportamento seguenti possono potenzialmente interrompere le applicazioni sviluppate per EF Core 2.2.x durante l'aggiornamento alla versione 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="a2208-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="a2208-105">Le modifiche che si prevede abbiano impatto solo sui provider di database sono documentate nelle [modifiche che influiscono sul provider](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="a2208-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="a2208-106">Le interruzioni nelle nuove funzionalità introdotte da un'anteprima 3.0 a un'altra anteprima 3.0 non sono documentate.</span><span class="sxs-lookup"><span data-stu-id="a2208-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="a2208-107">Le query LINQ non vengono più valutate nel client</span><span class="sxs-lookup"><span data-stu-id="a2208-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="a2208-108">[Problema n. 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Vedere anche il problema n. 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="a2208-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="a2208-109">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="a2208-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a2208-110">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-110">**Old behavior**</span></span>

<span data-ttu-id="a2208-111">Nelle versioni precedenti alla versione 3.0, quando EF Core non era in grado di convertire un'espressione inclusa in una query in SQL o in un parametro, l'espressione veniva automaticamente valutata nel client.</span><span class="sxs-lookup"><span data-stu-id="a2208-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="a2208-112">Per impostazione predefinita, la valutazione client di espressioni potenzialmente dispendiose si limitava ad attivare solo un avviso.</span><span class="sxs-lookup"><span data-stu-id="a2208-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="a2208-113">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-113">**New behavior**</span></span>

<span data-ttu-id="a2208-114">A partire dalla versione 3.0, EF Core consente solo la valutazione delle espressioni nella proiezione di primo livello (l'ultima chiamata `Select()` nella query) nel client.</span><span class="sxs-lookup"><span data-stu-id="a2208-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="a2208-115">Quando le espressioni in altre posizioni all'interno della query non possono essere convertite in SQL o in un parametro, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="a2208-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="a2208-116">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-116">**Why**</span></span>

<span data-ttu-id="a2208-117">La valutazione client automatica delle query consente di eseguire numerose query anche nel caso in cui parti importanti delle query non possono essere convertite.</span><span class="sxs-lookup"><span data-stu-id="a2208-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="a2208-118">Questo comportamento può causare un comportamento imprevisto e potenzialmente dannoso che può diventare evidente solo in produzione.</span><span class="sxs-lookup"><span data-stu-id="a2208-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="a2208-119">Ad esempio, una condizione in una chiamata `Where()` che non può essere convertita può causare il trasferimento di tutte le righe della tabella del server di database e l'applicazione del filtro nel client.</span><span class="sxs-lookup"><span data-stu-id="a2208-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="a2208-120">È probabile che questa situazione non venga rilevata se la tabella contiene solo alcune righe in fase di sviluppo, ma che abbia un grande impatto quando l'applicazione passa in produzione dove la tabella può contenere milioni di righe.</span><span class="sxs-lookup"><span data-stu-id="a2208-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="a2208-121">Gli avvisi di valutazione client inoltre si sono rivelati molto facili da ignorare durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="a2208-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="a2208-122">Inoltre, la valutazione client automatica può causare problemi in cui il miglioramento della conversione di query per espressioni specifiche causa modifiche impreviste che causano un'interruzione da una versione all'altra.</span><span class="sxs-lookup"><span data-stu-id="a2208-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="a2208-123">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-123">**Mitigations**</span></span>

<span data-ttu-id="a2208-124">Se una query non può essere convertita completamente, riscrivere la query in un formato che possa essere convertito o usare `AsEnumerable()`, `ToList()` o un elemento simile per riportare in modo esplicito i dati nel client dove possono essere quindi ulteriormente elaborati usando LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="a2208-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="a2208-125">Entity Framework Core non è più incluso nel framework condiviso di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2208-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="a2208-126">Annunci problema n. 325</span><span class="sxs-lookup"><span data-stu-id="a2208-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="a2208-127">Questa modifica è stata introdotta in ASP.NET Core 3.0 anteprima 1.</span><span class="sxs-lookup"><span data-stu-id="a2208-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="a2208-128">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-128">**Old behavior**</span></span>

<span data-ttu-id="a2208-129">Nelle versioni precedenti ad ASP.NET Core 3.0, quando si aggiungeva un riferimento a un pacchetto in `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, veniva inserito EF Core e alcuni dei provider di dati di EF Core come il provider di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a2208-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="a2208-130">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-130">**New behavior**</span></span>

<span data-ttu-id="a2208-131">A partire dalla versione 3.0, il framework condiviso di ASP.NET Core non include EF Core o provider di dati di EF Core.</span><span class="sxs-lookup"><span data-stu-id="a2208-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="a2208-132">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-132">**Why**</span></span>

<span data-ttu-id="a2208-133">Prima di questa modifica, per ottenere EF Core erano necessarie procedure diverse, a seconda che l'applicazione avesse o meno come destinazione ASP.NET Core e SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a2208-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="a2208-134">Inoltre, l'aggiornamento di ASP.NET Core forzava l'aggiornamento di EF Core e del provider di SQL Server, non sempre auspicabile.</span><span class="sxs-lookup"><span data-stu-id="a2208-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="a2208-135">Con questa modifica, la procedura per ottenere EF Core è la stessa in tutti i provider, le implementazioni .NET supportate e i tipi di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="a2208-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="a2208-136">Gli sviluppatori possono ora controllare anche in modo preciso quando vengono aggiornati EF Core e i provider di dati di EF Core.</span><span class="sxs-lookup"><span data-stu-id="a2208-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="a2208-137">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-137">**Mitigations**</span></span>

<span data-ttu-id="a2208-138">Per usare EF Core in un'applicazione ASP.NET Core 3.0 o in un'altra applicazione supportata, aggiungere in modo esplicito un riferimento al pacchetto al provider di database di EF Core che verrà usato dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a2208-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="a2208-139">L'esecuzione di query viene registrata a livello di debug</span><span class="sxs-lookup"><span data-stu-id="a2208-139">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="a2208-140">Problema n. 14523</span><span class="sxs-lookup"><span data-stu-id="a2208-140">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="a2208-141">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="a2208-141">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a2208-142">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-142">**Old behavior**</span></span>

<span data-ttu-id="a2208-143">Nelle versioni precedenti a EF Core 3.0 l'esecuzione di query e altri comandi veniva registrata a livello `Info`.</span><span class="sxs-lookup"><span data-stu-id="a2208-143">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="a2208-144">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-144">**New behavior**</span></span>

<span data-ttu-id="a2208-145">A partire da EF Core 3.0, la registrazione dell'esecuzione di comandi/SQL è a livello `Debug`.</span><span class="sxs-lookup"><span data-stu-id="a2208-145">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="a2208-146">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-146">**Why**</span></span>

<span data-ttu-id="a2208-147">Questa modifica è stata apportata per ridurre i disturbi a livello di registrazione `Info`.</span><span class="sxs-lookup"><span data-stu-id="a2208-147">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="a2208-148">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-148">**Mitigations**</span></span>

<span data-ttu-id="a2208-149">Questo evento di registrazione è definito da `RelationalEventId.CommandExecuting` con l'ID evento 20100.</span><span class="sxs-lookup"><span data-stu-id="a2208-149">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="a2208-150">Per registrare SQL di nuovo al livello `Info`, configurare in modo esplicito il livello in `OnConfiguring` o `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="a2208-150">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="a2208-151">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a2208-151">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="a2208-152">I valori di chiave temporanei non sono più impostati nelle istanze di entità</span><span class="sxs-lookup"><span data-stu-id="a2208-152">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="a2208-153">Problema n. 12378</span><span class="sxs-lookup"><span data-stu-id="a2208-153">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="a2208-154">Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.</span><span class="sxs-lookup"><span data-stu-id="a2208-154">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="a2208-155">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-155">**Old behavior**</span></span>

<span data-ttu-id="a2208-156">Nelle versioni precedenti a EF Core 3.0 i valori temporanei venivano assegnati a tutte le proprietà di chiave per cui veniva in seguito generato un valore reale dal database.</span><span class="sxs-lookup"><span data-stu-id="a2208-156">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="a2208-157">In genere questi valori temporanei erano numeri negativi elevati.</span><span class="sxs-lookup"><span data-stu-id="a2208-157">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="a2208-158">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-158">**New behavior**</span></span>

<span data-ttu-id="a2208-159">A partire dalla versione 3.0, EF Core archivia il valore di chiave temporaneo come parte delle informazioni di rilevamento dell'entità e non modifica la proprietà di chiave.</span><span class="sxs-lookup"><span data-stu-id="a2208-159">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="a2208-160">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-160">**Why**</span></span>

<span data-ttu-id="a2208-161">Questa modifica è stata apportata per impedire che i valori di chiave temporanei diventino erroneamente permanenti quando un'entità rilevata in precedenza da un'istanza `DbContext` viene spostata in un'altra istanza `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="a2208-161">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="a2208-162">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-162">**Mitigations**</span></span>

<span data-ttu-id="a2208-163">Le applicazioni che assegnano valori di chiave primaria in chiavi esterne per creare associazioni tra le entità possono dipendere dal comportamento precedente se le chiavi primarie vengono generate dall'archivio e appartengono a entità con stato `Added`.</span><span class="sxs-lookup"><span data-stu-id="a2208-163">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="a2208-164">Questo può essere evitato:</span><span class="sxs-lookup"><span data-stu-id="a2208-164">This can be avoided by:</span></span>
* <span data-ttu-id="a2208-165">Non usando chiavi generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="a2208-165">Not using store-generated keys.</span></span>
* <span data-ttu-id="a2208-166">Impostando le proprietà di navigazione in modo da creare relazioni anziché impostando valori di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="a2208-166">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="a2208-167">Ottenendo i valori di chiave temporanei effettivi dalle informazioni di rilevamento dell'entità.</span><span class="sxs-lookup"><span data-stu-id="a2208-167">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="a2208-168">Ad esempio, `context.Entry(blog).Property(e => e.Id).CurrentValue` restituisce il valore temporaneo anche quando `blog.Id` non è stato impostato.</span><span class="sxs-lookup"><span data-stu-id="a2208-168">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="a2208-169">DetectChanges rispetta i valori di chiave generati dall'archivio</span><span class="sxs-lookup"><span data-stu-id="a2208-169">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="a2208-170">Problema n. 14616</span><span class="sxs-lookup"><span data-stu-id="a2208-170">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="a2208-171">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="a2208-171">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a2208-172">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-172">**Old behavior**</span></span>

<span data-ttu-id="a2208-173">Nelle versioni precedenti a EF Core 3.0 un'entità non rilevata individuata da `DetectChanges` veniva rilevata nello stato `Added` e inserita come nuova riga quando veniva eseguita una chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="a2208-173">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="a2208-174">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-174">**New behavior**</span></span>

<span data-ttu-id="a2208-175">A partire da EF Core 3.0, se un'entità usa valori di chiave generati e viene impostato un valore di chiave, l'entità viene rilevata nello stato `Modified`.</span><span class="sxs-lookup"><span data-stu-id="a2208-175">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="a2208-176">Ciò significa che si presuppone l'esistenza di una riga per l'entità che viene aggiornata quando viene eseguita una chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="a2208-176">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="a2208-177">Se il valore di chiave non viene impostato o se il tipo di entità non usa chiavi generate, la nuova entità viene rilevata come `Added` come nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="a2208-177">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="a2208-178">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-178">**Why**</span></span>

<span data-ttu-id="a2208-179">Questa modifica è stata apportata per rendere più semplice e coerente l'uso di grafici di entità disconnesse con chiavi generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="a2208-179">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="a2208-180">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-180">**Mitigations**</span></span>

<span data-ttu-id="a2208-181">Questa modifica può interrompere un'applicazione se un tipo di entità è configurato per l'uso di chiavi generate ma i valori di chiave sono impostati in modo esplicito per le nuove istanze.</span><span class="sxs-lookup"><span data-stu-id="a2208-181">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="a2208-182">La correzione consiste nel configurare in modo esplicito le proprietà di chiave per non usare valori generati.</span><span class="sxs-lookup"><span data-stu-id="a2208-182">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="a2208-183">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="a2208-183">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="a2208-184">Oppure con annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="a2208-184">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="a2208-185">Le eliminazioni a catena vengono ora eseguite immediatamente per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="a2208-185">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="a2208-186">Problema n. 10114</span><span class="sxs-lookup"><span data-stu-id="a2208-186">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="a2208-187">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="a2208-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a2208-188">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-188">**Old behavior**</span></span>

<span data-ttu-id="a2208-189">Nelle versioni precedenti alla versione 3.0, EF Core applicava azioni a catena (eliminazione delle entità dipendenti quando veniva eliminata un'entità di sicurezza obbligatoria o veniva recisa la relazione con un'entità di sicurezza obbligatoria) solo dopo la chiamata a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="a2208-189">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="a2208-190">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-190">**New behavior**</span></span>

<span data-ttu-id="a2208-191">A partire dalla versione 3.0, EF Core applica le azioni a catena non appena viene rilevata la condizione di attivazione.</span><span class="sxs-lookup"><span data-stu-id="a2208-191">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="a2208-192">Ad esempio, la chiamata a `context.Remove()` per eliminare un'entità di sicurezza causa anche l'impostazione immediata di tutti i dipendenti obbligatori correlati rilevati su `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="a2208-192">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="a2208-193">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-193">**Why**</span></span>

<span data-ttu-id="a2208-194">Questa modifica è stata apportata per migliorare l'esperienza di associazione di dati e degli scenari di controllo in cui è importante individuare le entità che verranno eliminate _prima_ della chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="a2208-194">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="a2208-195">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-195">**Mitigations**</span></span>

<span data-ttu-id="a2208-196">Il comportamento precedente può essere ripristinato tramite le impostazioni in `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="a2208-196">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="a2208-197">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a2208-197">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="a2208-198">I tipi di query vengono consolidati con tipi di entità</span><span class="sxs-lookup"><span data-stu-id="a2208-198">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="a2208-199">Problema n. 14194</span><span class="sxs-lookup"><span data-stu-id="a2208-199">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="a2208-200">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="a2208-200">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a2208-201">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-201">**Old behavior**</span></span>

<span data-ttu-id="a2208-202">Nelle versioni precedenti a EF Core 3.0 i [tipi di query](xref:core/modeling/query-types) erano uno strumento per eseguire query su dati che non definiscono una chiave primaria in modo strutturato.</span><span class="sxs-lookup"><span data-stu-id="a2208-202">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="a2208-203">Veniva infatti usato un tipo di query per eseguire il mapping di tipi di entità senza chiavi (più probabilmente da una vista, ma anche da una tabella), mentre veniva usato un tipo di entità normale quando era disponibile una chiave (più probabilmente da una tabella, ma anche da una vista).</span><span class="sxs-lookup"><span data-stu-id="a2208-203">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="a2208-204">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-204">**New behavior**</span></span>

<span data-ttu-id="a2208-205">Un tipo di query diventa ora semplicemente un tipo di entità senza chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="a2208-205">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="a2208-206">I tipi di entità senza chiave hanno la stessa funzionalità dei tipi di query nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="a2208-206">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="a2208-207">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-207">**Why**</span></span>

<span data-ttu-id="a2208-208">Questa modifica è stata apportata per ridurre la confusione riguardo lo scopo dei tipi di query.</span><span class="sxs-lookup"><span data-stu-id="a2208-208">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="a2208-209">In particolare, si tratta di tipi di entità senza chiave che sono intrinsecamente di sola lettura per questo motivo ma non dovrebbero essere usati solo perché un tipo di entità deve essere di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="a2208-209">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="a2208-210">Analogamente, spesso vengono mappati alle viste solo perché le viste spesso non definiscono le chiavi.</span><span class="sxs-lookup"><span data-stu-id="a2208-210">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="a2208-211">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-211">**Mitigations**</span></span>

<span data-ttu-id="a2208-212">Le parti dell'API seguenti sono ora obsolete:</span><span class="sxs-lookup"><span data-stu-id="a2208-212">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="a2208-213">**`ModelBuilder.Query<>()`** - È necessario chiamare `ModelBuilder.Entity<>().HasNoKey()` per contrassegnare un tipo di entità come tipo senza chiavi.</span><span class="sxs-lookup"><span data-stu-id="a2208-213">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="a2208-214">Non ne viene eseguita la configurazione per convenzione per evitare una configurazione errata quando è prevista una chiave primaria che tuttavia non corrisponde alla convenzione.</span><span class="sxs-lookup"><span data-stu-id="a2208-214">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="a2208-215">**`DbQuery<>`** - Usare `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="a2208-215">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="a2208-216">**`DbContext.Query<>()`** - Usare `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="a2208-216">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="a2208-217">L'API di configurazione per le relazioni di tipo di proprietà è stata modificata</span><span class="sxs-lookup"><span data-stu-id="a2208-217">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="a2208-218">[Problema n. 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema n. 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema n. 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="a2208-218">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="a2208-219">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="a2208-219">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a2208-220">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-220">**Old behavior**</span></span>

<span data-ttu-id="a2208-221">Nelle versioni precedenti a EF Core 3.0 la configurazione della relazione di proprietà veniva eseguita direttamente dopo la chiamata a `OwnsOne` o `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="a2208-221">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="a2208-222">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-222">**New behavior**</span></span>

<span data-ttu-id="a2208-223">A partire da EF Core 3.0, è disponibile l'API Fluent per configurare una proprietà di navigazione per il proprietario usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="a2208-223">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="a2208-224">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a2208-224">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="a2208-225">La configurazione correlata alla relazione tra proprietario e elemento di proprietà deve essere ora concatenata dopo `WithOwner()` in modo analogo a come vengono configurate altre relazioni.</span><span class="sxs-lookup"><span data-stu-id="a2208-225">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="a2208-226">La configurazione per il tipo di proprietà viene comunque concatenata dopo `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="a2208-226">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="a2208-227">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a2208-227">For example:</span></span>

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

<span data-ttu-id="a2208-228">Inoltre, la chiamata a `Entity()`, `HasOne()` o `Set()` con una destinazione di tipo di proprietà genera ora un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="a2208-228">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="a2208-229">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-229">**Why**</span></span>

<span data-ttu-id="a2208-230">Questa modifica è stata apportata per creare una separazione più netta tra la configurazione del tipo di proprietà e la _relazione_ con il tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="a2208-230">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="a2208-231">Ciò consente di eliminare ambiguità e confusione su metodi come `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="a2208-231">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="a2208-232">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-232">**Mitigations**</span></span>

<span data-ttu-id="a2208-233">Modificare la configurazione delle relazioni dei tipi di proprietà per usare la superficie della nuova API come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="a2208-233">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="a2208-234">La convenzione di proprietà di chiave esterna non ha più lo stesso nome della proprietà dell'entità di sicurezza</span><span class="sxs-lookup"><span data-stu-id="a2208-234">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="a2208-235">Problema n. 13274</span><span class="sxs-lookup"><span data-stu-id="a2208-235">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="a2208-236">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="a2208-236">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a2208-237">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-237">**Old behavior**</span></span>

<span data-ttu-id="a2208-238">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="a2208-238">Consider the following model:</span></span>
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
<span data-ttu-id="a2208-239">Nelle versioni precedenti a EF Core 3.0 veniva usata la proprietà `CustomerId` per la chiave esterna per convenzione.</span><span class="sxs-lookup"><span data-stu-id="a2208-239">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="a2208-240">Tuttavia, se `Order` è un tipo di proprietà, `CustomerId` sarebbe la chiave primaria e ciò non è in genere auspicabile.</span><span class="sxs-lookup"><span data-stu-id="a2208-240">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="a2208-241">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-241">**New behavior**</span></span>

<span data-ttu-id="a2208-242">A partire da 3.0, EF Core non tenta di usare le proprietà per le chiavi esterne per convenzione se hanno lo stesso nome della proprietà dell'entità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="a2208-242">Starting with 3.0, EF Core won't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="a2208-243">Viene ancora eseguita la corrispondenza tra i criteri del nome del tipo dell'entità di sicurezza concatenato al nome della proprietà dell'entità di sicurezza e il nome di navigazione concatenato al nome della proprietà dell'entità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="a2208-243">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="a2208-244">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a2208-244">For example:</span></span>

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

<span data-ttu-id="a2208-245">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-245">**Why**</span></span>

<span data-ttu-id="a2208-246">Questa modifica è stata apportata per evitare una definizione errata della proprietà di chiave primaria nel tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="a2208-246">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="a2208-247">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-247">**Mitigations**</span></span>

<span data-ttu-id="a2208-248">Se la proprietà è stata progettata per essere la chiave esterna e di conseguenza parte della chiave primaria, configurarla in modo esplicito come chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="a2208-248">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="a2208-249">Ogni proprietà usa la generazione di chiavi di tipo intero in memoria indipendenti</span><span class="sxs-lookup"><span data-stu-id="a2208-249">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="a2208-250">Problema n. 6872</span><span class="sxs-lookup"><span data-stu-id="a2208-250">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="a2208-251">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="a2208-251">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a2208-252">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-252">**Old behavior**</span></span>

<span data-ttu-id="a2208-253">Nelle versioni precedenti a EF Core 3.0 veniva usato un unico generatore di valori condiviso per tutte le proprietà di chiavi di tipo intero in memoria.</span><span class="sxs-lookup"><span data-stu-id="a2208-253">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="a2208-254">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-254">**New behavior**</span></span>

<span data-ttu-id="a2208-255">A partire da EF Core 3.0, ogni proprietà di chiave di tipo intero riceve un generatore di valori quando viene usato il database in memoria.</span><span class="sxs-lookup"><span data-stu-id="a2208-255">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="a2208-256">Inoltre, se il database viene eliminato, la generazione di chiavi viene reimpostata per tutte le tabelle.</span><span class="sxs-lookup"><span data-stu-id="a2208-256">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="a2208-257">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-257">**Why**</span></span>

<span data-ttu-id="a2208-258">Questa modifica è stata apportata per allineare maggiormente la generazione di chiavi in memoria alla generazione di chiavi del database reale e per migliorare la possibilità di isolare i test l'uno dall'altro quando viene usato il database in memoria.</span><span class="sxs-lookup"><span data-stu-id="a2208-258">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="a2208-259">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-259">**Mitigations**</span></span>

<span data-ttu-id="a2208-260">Ciò può interrompere un'applicazione che si basa sull'impostazione di valori di chiave in memoria specifici.</span><span class="sxs-lookup"><span data-stu-id="a2208-260">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="a2208-261">È consigliabile non basare l'applicazione su valori di chiave specifici o eseguire l'aggiornamento per passare al nuovo comportamento.</span><span class="sxs-lookup"><span data-stu-id="a2208-261">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="a2208-262">I campi sottostanti vengono usati per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="a2208-262">Backing fields are used by default</span></span>

[<span data-ttu-id="a2208-263">Problema n. 12430</span><span class="sxs-lookup"><span data-stu-id="a2208-263">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="a2208-264">Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.</span><span class="sxs-lookup"><span data-stu-id="a2208-264">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="a2208-265">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-265">**Old behavior**</span></span>

<span data-ttu-id="a2208-266">Nelle versioni precedenti alla versione 3.0, anche se il campo sottostante di una proprietà era noto, per impostazione predefinita EF Core eseguiva la lettura e la scrittura del valore della proprietà usando i metodi getter e setter della proprietà.</span><span class="sxs-lookup"><span data-stu-id="a2208-266">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="a2208-267">L'eccezione era costituita dall'esecuzione di query in cui il campo sottostante, se noto, veniva impostato direttamente.</span><span class="sxs-lookup"><span data-stu-id="a2208-267">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="a2208-268">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-268">**New behavior**</span></span>

<span data-ttu-id="a2208-269">A partire da EF Core 3.0, se il campo sottostante di una proprietà è noto, la lettura e la scrittura della proprietà vengono sempre eseguite usando il campo sottostante.</span><span class="sxs-lookup"><span data-stu-id="a2208-269">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="a2208-270">Ciò potrebbe causare un'interruzione dell'applicazione se l'applicazione si basa su un comportamento aggiuntivo codificato nei metodi getter o setter.</span><span class="sxs-lookup"><span data-stu-id="a2208-270">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="a2208-271">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-271">**Why**</span></span>

<span data-ttu-id="a2208-272">Questa modifica è stata apportata per impedire a EF Core di attivare per errore la logica di business per impostazione predefinita quando si eseguono operazioni di database che interessano le entità.</span><span class="sxs-lookup"><span data-stu-id="a2208-272">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="a2208-273">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-273">**Mitigations**</span></span>

<span data-ttu-id="a2208-274">È possibile ripristinare il comportamento delle versioni precedenti alla versione 3.0 tramite la configurazione della modalità di accesso delle proprietà nell'API Fluent modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="a2208-274">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="a2208-275">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a2208-275">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="a2208-276">Viene generata un'eccezione se vengono trovati più campi sottostanti compatibili</span><span class="sxs-lookup"><span data-stu-id="a2208-276">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="a2208-277">Problema n. 12523</span><span class="sxs-lookup"><span data-stu-id="a2208-277">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="a2208-278">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="a2208-278">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a2208-279">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-279">**Old behavior**</span></span>

<span data-ttu-id="a2208-280">Nelle versioni precedenti a EF Core 3.0, se più campi soddisfacevano le regole di ricerca del campo sottostante di una proprietà, veniva selezionato un solo campo in base a un ordine di precedenza.</span><span class="sxs-lookup"><span data-stu-id="a2208-280">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="a2208-281">Ciò poteva causare l'uso di un campo non corretto nei casi ambigui.</span><span class="sxs-lookup"><span data-stu-id="a2208-281">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="a2208-282">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-282">**New behavior**</span></span>

<span data-ttu-id="a2208-283">A partire da EF Core 3.0, se più campi corrispondono alla stessa proprietà, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="a2208-283">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="a2208-284">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-284">**Why**</span></span>

<span data-ttu-id="a2208-285">Questa modifica è stata apportata per evitare di usare automaticamente un campo rispetto a un altro quando un solo campo può essere quello corretto.</span><span class="sxs-lookup"><span data-stu-id="a2208-285">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="a2208-286">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-286">**Mitigations**</span></span>

<span data-ttu-id="a2208-287">Per le proprietà con campi sottostanti ambigui, il campo da usare deve essere specificato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="a2208-287">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="a2208-288">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="a2208-288">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="a2208-289">AddDbContext/AddDbContextPool non chiamano più AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="a2208-289">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="a2208-290">Problema n. 14756</span><span class="sxs-lookup"><span data-stu-id="a2208-290">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="a2208-291">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="a2208-291">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a2208-292">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-292">**Old behavior**</span></span>

<span data-ttu-id="a2208-293">Prima di EF Core 3.0, la chiamata di `AddDbContext` oppure `AddDbContextPool` comporta anche la registrazione dei servizi di registrazione e memorizzazione nella cache con inserimento delle dipendenze tramite chiamate a [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="a2208-293">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="a2208-294">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-294">**New behavior**</span></span>

<span data-ttu-id="a2208-295">A partire da EF Core 3.0, `AddDbContext` e `AddDbContextPool` non registreranno più questi servizi con inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="a2208-295">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="a2208-296">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-296">**Why**</span></span>

<span data-ttu-id="a2208-297">EF Core 3.0 non richiede che questi servizi siano inclusi nel contenitore di inserimento delle dipendenze dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a2208-297">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="a2208-298">Tuttavia, se `ILoggerFactory` è registrato nel contenitore di inserimento delle dipendenze dell'applicazione, verrà ancora usato da EF Core.</span><span class="sxs-lookup"><span data-stu-id="a2208-298">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="a2208-299">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-299">**Mitigations**</span></span>

<span data-ttu-id="a2208-300">Se l'applicazione necessita di questi servizi, registrarli in modo esplicito con il contenitore di inserimento delle dipendenze usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="a2208-300">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="a2208-301">DbContext.Entry esegue ora un DetectChanges locale</span><span class="sxs-lookup"><span data-stu-id="a2208-301">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="a2208-302">Problema n. 13552</span><span class="sxs-lookup"><span data-stu-id="a2208-302">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="a2208-303">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="a2208-303">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a2208-304">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-304">**Old behavior**</span></span>

<span data-ttu-id="a2208-305">Nelle versioni precedenti a EF Core 3.0 la chiamata a `DbContext.Entry` causava il rilevamento delle modifiche per tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="a2208-305">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="a2208-306">Ciò garantiva l'aggiornamento dello stato esposto in `EntityEntry`.</span><span class="sxs-lookup"><span data-stu-id="a2208-306">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="a2208-307">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-307">**New behavior**</span></span>

<span data-ttu-id="a2208-308">A partire da EF Core 3.0, la chiamata a `DbContext.Entry` causa ora solo il tentativo di rilevare le modifiche nell'entità specificata e in tutte le relative entità di sicurezza rilevate.</span><span class="sxs-lookup"><span data-stu-id="a2208-308">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="a2208-309">Ciò significa che le modifiche apportate altrove potrebbero non essere state rilevate tramite la chiamata al metodo e ciò potrebbe avere implicazioni sullo stato dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a2208-309">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="a2208-310">Si noti che se `ChangeTracker.AutoDetectChangesEnabled` è impostato su `false`, verrà disabilitato anche questo tipo di rilevamento delle modifiche locali.</span><span class="sxs-lookup"><span data-stu-id="a2208-310">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="a2208-311">Gli altri metodi che causano il rilevamento delle modifiche, ad esempio `ChangeTracker.Entries` e `SaveChanges`, causano ancora un `DetectChanges` completo di tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="a2208-311">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="a2208-312">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-312">**Why**</span></span>

<span data-ttu-id="a2208-313">Questa modifica è stata apportata per migliorare le prestazioni predefinite dell'uso di `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="a2208-313">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="a2208-314">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-314">**Mitigations**</span></span>

<span data-ttu-id="a2208-315">Chiamare `ChgangeTracker.DetectChanges()` in modo esplicito prima di chiamare `Entry` per garantire il comportamento precedente alla versione 3.0.</span><span class="sxs-lookup"><span data-stu-id="a2208-315">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="a2208-316">Le chiavi matrice di byte e di stringhe non vengono generate dal client per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="a2208-316">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="a2208-317">Problema n. 14617</span><span class="sxs-lookup"><span data-stu-id="a2208-317">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="a2208-318">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="a2208-318">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a2208-319">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-319">**Old behavior**</span></span>

<span data-ttu-id="a2208-320">Nelle versioni precedenti a EF Core 3.0 le proprietà di chiave `string` e `byte[]` potevano essere usate senza impostare in modo esplicito un valore non Null.</span><span class="sxs-lookup"><span data-stu-id="a2208-320">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="a2208-321">In questi casi, il valore di chiave veniva generato nel client come GUID, serializzato in byte per `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="a2208-321">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="a2208-322">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-322">**New behavior**</span></span>

<span data-ttu-id="a2208-323">A partire da EF Core 3.0 viene generata un'eccezione che indica che non è stato impostato alcun valore di chiave.</span><span class="sxs-lookup"><span data-stu-id="a2208-323">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="a2208-324">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-324">**Why**</span></span>

<span data-ttu-id="a2208-325">Questa modifica è stata apportata poiché i valori `string`/`byte[]` generati dal client non risultano in genere utili e il comportamento predefinito rendeva complesso ragionare sui valori di chiave generati in un modo comune.</span><span class="sxs-lookup"><span data-stu-id="a2208-325">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="a2208-326">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-326">**Mitigations**</span></span>

<span data-ttu-id="a2208-327">È possibile ripristinare il comportamento precedente alla versione 3.0 specificando in modo esplicito che le proprietà di chiave devono usare i valori generati se non viene impostato alcun altro valore non Null.</span><span class="sxs-lookup"><span data-stu-id="a2208-327">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="a2208-328">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="a2208-328">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="a2208-329">Oppure con annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="a2208-329">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="a2208-330">ILoggerFactory è ora un servizio con ambito</span><span class="sxs-lookup"><span data-stu-id="a2208-330">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="a2208-331">Problema n. 14698</span><span class="sxs-lookup"><span data-stu-id="a2208-331">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="a2208-332">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="a2208-332">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a2208-333">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-333">**Old behavior**</span></span>

<span data-ttu-id="a2208-334">Nelle versioni precedenti a EF Core 3.0 `ILoggerFactory` veniva registrato come servizio singleton.</span><span class="sxs-lookup"><span data-stu-id="a2208-334">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="a2208-335">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-335">**New behavior**</span></span>

<span data-ttu-id="a2208-336">A partire da EF Core 3.0, `ILoggerFactory` viene registrato come servizio con ambito.</span><span class="sxs-lookup"><span data-stu-id="a2208-336">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="a2208-337">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-337">**Why**</span></span>

<span data-ttu-id="a2208-338">Questa modifica è stata apportata per consentire l'associazione di un logger a un'istanza `DbContext` che abilita altre funzionalità e rimuove alcuni casi di comportamento anomalo, ad esempio un'esplosione dei provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="a2208-338">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="a2208-339">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-339">**Mitigations**</span></span>

<span data-ttu-id="a2208-340">Questa modifica non dovrebbe influire sul codice dell'applicazione a meno che non vengano registrati e usati servizi personalizzati nel provider di servizi interno di EF Core.</span><span class="sxs-lookup"><span data-stu-id="a2208-340">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="a2208-341">Questa condizione, tuttavia, non è comune.</span><span class="sxs-lookup"><span data-stu-id="a2208-341">This isn't common.</span></span>
<span data-ttu-id="a2208-342">In questi casi la maggior parte delle operazioni continuano a essere eseguite correttamente, ma qualsiasi servizio singleton dipendente da `ILoggerFactory` dovrà essere modificato per ottenere `ILoggerFactory` in modo diverso.</span><span class="sxs-lookup"><span data-stu-id="a2208-342">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="a2208-343">Se si verificano situazioni simili, inviare una segnalazione nello [strumento di gestione dei problemi in GitHub relativo a EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) per comunicare in che modo viene usato `ILoggerFactory` per consentirci di comprendere meglio come evitare ulteriori interruzioni in futuro.</span><span class="sxs-lookup"><span data-stu-id="a2208-343">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="a2208-344">IDbContextOptionsExtensionWithDebugInfo unito in IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="a2208-344">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="a2208-345">Problema n. 13552</span><span class="sxs-lookup"><span data-stu-id="a2208-345">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="a2208-346">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="a2208-346">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a2208-347">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-347">**Old behavior**</span></span>

<span data-ttu-id="a2208-348">`IDbContextOptionsExtensionWithDebugInfo` era un'interfaccia facoltativa aggiuntiva estesa da `IDbContextOptionsExtension` che consentiva di evitare le modifiche che causavano interruzioni nell'interfaccia durante il ciclo di versioni 2.x.</span><span class="sxs-lookup"><span data-stu-id="a2208-348">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="a2208-349">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-349">**New behavior**</span></span>

<span data-ttu-id="a2208-350">Le interfacce sono ora unite in `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="a2208-350">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="a2208-351">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-351">**Why**</span></span>

<span data-ttu-id="a2208-352">Questa modifica è stata apportata perché le interfacce sono concettualmente un'unica interfaccia.</span><span class="sxs-lookup"><span data-stu-id="a2208-352">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="a2208-353">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-353">**Mitigations**</span></span>

<span data-ttu-id="a2208-354">Tutte le implementazioni di `IDbContextOptionsExtension` dovranno essere aggiornate per supportare il nuovo membro.</span><span class="sxs-lookup"><span data-stu-id="a2208-354">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="a2208-355">I proxy di caricamento lazy non presuppongono più che le proprietà di navigazione vengano caricate completamente</span><span class="sxs-lookup"><span data-stu-id="a2208-355">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="a2208-356">Problema n. 12780</span><span class="sxs-lookup"><span data-stu-id="a2208-356">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="a2208-357">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="a2208-357">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a2208-358">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-358">**Old behavior**</span></span>

<span data-ttu-id="a2208-359">Nelle versioni precedenti a EF Core 3.0, quando `DbContext` veniva eliminato non esisteva alcun metodo per scoprire se una determinata proprietà di navigazione in un'entità ottenuta da un contesto specifico veniva caricata completamente o meno.</span><span class="sxs-lookup"><span data-stu-id="a2208-359">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="a2208-360">I proxy presupponevano invece che venisse caricata una navigazione di riferimento se era presente un valore non Null e che venisse caricata una navigazione di raccolta se era presente un valore.</span><span class="sxs-lookup"><span data-stu-id="a2208-360">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="a2208-361">In questi casi il tentativo di eseguire un caricamento lazy non avrebbe avuto alcun esito.</span><span class="sxs-lookup"><span data-stu-id="a2208-361">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="a2208-362">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-362">**New behavior**</span></span>

<span data-ttu-id="a2208-363">A partire da Entity Framework Core 3.0, i proxy tengono traccia del caricamento o mancato caricamento di una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a2208-363">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="a2208-364">Ciò significa che il tentativo di accedere a una proprietà di navigazione caricata dopo l'eliminazione del contesto non avrà mai alcun esito, anche quando la navigazione caricata è vuota o ha valore Null.</span><span class="sxs-lookup"><span data-stu-id="a2208-364">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="a2208-365">Al contrario, il tentativo di accedere a una proprietà di navigazione non caricata genera un'eccezione se il contesto viene eliminato anche se la proprietà di navigazione è una raccolta non vuota.</span><span class="sxs-lookup"><span data-stu-id="a2208-365">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="a2208-366">Se si verifica questa situazione significa che il codice dell'applicazione sta tentando di usare il caricamento lazy in un momento non valido e l'applicazione deve essere modificata in modo da non eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="a2208-366">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="a2208-367">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-367">**Why**</span></span>

<span data-ttu-id="a2208-368">Questa modifica è stata apportata per rendere coerente e corretto il comportamento durante un tentativo di caricamento lazy in un'istanza `DbContext` eliminata.</span><span class="sxs-lookup"><span data-stu-id="a2208-368">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="a2208-369">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-369">**Mitigations**</span></span>

<span data-ttu-id="a2208-370">Aggiornare il codice dell'applicazione per fare in modo che non venga tentato il caricamento lazy con un contesto eliminato oppure specificare una configurazione in modo che non venga eseguita alcuna operazione come descritto nel messaggio di eccezione.</span><span class="sxs-lookup"><span data-stu-id="a2208-370">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="a2208-371">La creazione di un numero eccessivo di provider di servizi interni è ora un errore per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="a2208-371">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="a2208-372">Problema n. 10236</span><span class="sxs-lookup"><span data-stu-id="a2208-372">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="a2208-373">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="a2208-373">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a2208-374">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-374">**Old behavior**</span></span>

<span data-ttu-id="a2208-375">Nelle versioni precedenti a EF Core 3.0 veniva registrato un avviso per le applicazioni che creavano un numero eccessivo di provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="a2208-375">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="a2208-376">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-376">**New behavior**</span></span>

<span data-ttu-id="a2208-377">A partire da EF Core 3.0, l'avviso viene considerato un errore e viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="a2208-377">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="a2208-378">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-378">**Why**</span></span>

<span data-ttu-id="a2208-379">Questa modifica è stata apportata per gestire meglio il codice dell'applicazione tramite un'esposizione più esplicita di questa situazione di errore.</span><span class="sxs-lookup"><span data-stu-id="a2208-379">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="a2208-380">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-380">**Mitigations**</span></span>

<span data-ttu-id="a2208-381">L'azione più appropriata quando si verifica questo errore consiste nell'individuare la causa radice e nell'interrompere la creazione di numerosi provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="a2208-381">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="a2208-382">È possibile tuttavia convertire nuovamente l'errore in avviso o ignorarlo tramite una configurazione in `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a2208-382">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="a2208-383">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a2208-383">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="a2208-384">Nuovo comportamento per la chiamata di HasOne/HasMany con una singola stringa</span><span class="sxs-lookup"><span data-stu-id="a2208-384">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="a2208-385">Problema n. 9171</span><span class="sxs-lookup"><span data-stu-id="a2208-385">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="a2208-386">Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="a2208-386">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a2208-387">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-387">**Old behavior**</span></span>

<span data-ttu-id="a2208-388">Prima di EF Core 3.0, il codice che chiama `HasOne` o `HasMany` con una singola stringa era interpretato in modo poco chiaro.</span><span class="sxs-lookup"><span data-stu-id="a2208-388">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="a2208-389">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a2208-389">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="a2208-390">Apparentemente, il codice mette in relazione `Samurai` con un altro tipo di entità tramite la proprietà di navigazione `Entrance`, che può essere privata.</span><span class="sxs-lookup"><span data-stu-id="a2208-390">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="a2208-391">In realtà, il codice tenta di creare una relazione con un tipo di entità denominato `Entrance` senza proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a2208-391">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="a2208-392">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-392">**New behavior**</span></span>

<span data-ttu-id="a2208-393">A partire da EF Core 3.0, il codice sopra riportato ora esegue quello che avrebbe dovuto fare in precedenza.</span><span class="sxs-lookup"><span data-stu-id="a2208-393">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="a2208-394">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-394">**Why**</span></span>

<span data-ttu-id="a2208-395">Il comportamento precedente era molto poco chiaro, soprattutto durante la lettura del codice di configurazione e la ricerca di errori.</span><span class="sxs-lookup"><span data-stu-id="a2208-395">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="a2208-396">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-396">**Mitigations**</span></span>

<span data-ttu-id="a2208-397">Questa modifica causerà problemi solo nelle applicazioni che configurano relazioni in modo esplicito usando stringhe per i nomi dei tipi e senza specificare in modo esplicito la proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a2208-397">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="a2208-398">Non è uno scenario comune.</span><span class="sxs-lookup"><span data-stu-id="a2208-398">This is not common.</span></span>
<span data-ttu-id="a2208-399">Il comportamento precedente può essere ottenuto passando esplicitamente `null` per il nome della proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a2208-399">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="a2208-400">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a2208-400">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="a2208-401">L'annotazione Relational:TypeMapping è ora TypeMapping</span><span class="sxs-lookup"><span data-stu-id="a2208-401">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="a2208-402">Problema n. 9913</span><span class="sxs-lookup"><span data-stu-id="a2208-402">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="a2208-403">Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.</span><span class="sxs-lookup"><span data-stu-id="a2208-403">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="a2208-404">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-404">**Old behavior**</span></span>

<span data-ttu-id="a2208-405">Il nome di annotazione delle annotazioni di mapping del tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="a2208-405">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="a2208-406">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-406">**New behavior**</span></span>

<span data-ttu-id="a2208-407">Il nome di annotazione delle annotazioni di mapping del tipo è ora "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="a2208-407">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="a2208-408">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-408">**Why**</span></span>

<span data-ttu-id="a2208-409">Il mapping dei tipi non viene più usato solo per i provider di database relazionali.</span><span class="sxs-lookup"><span data-stu-id="a2208-409">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="a2208-410">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-410">**Mitigations**</span></span>

<span data-ttu-id="a2208-411">Ciò causa un'interruzione solo nelle applicazioni che accedono al mapping dei tipi direttamente come annotazione. Questa situazione non è comune.</span><span class="sxs-lookup"><span data-stu-id="a2208-411">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="a2208-412">L'azione più appropriata per risolvere il problema consiste nell'usare la superficie dell'API per accedere al mapping dei tipi anziché l'annotazione.</span><span class="sxs-lookup"><span data-stu-id="a2208-412">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="a2208-413">ToTable in un tipo derivato genera un'eccezione</span><span class="sxs-lookup"><span data-stu-id="a2208-413">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="a2208-414">Problema n. 11811</span><span class="sxs-lookup"><span data-stu-id="a2208-414">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="a2208-415">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="a2208-415">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a2208-416">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-416">**Old behavior**</span></span>

<span data-ttu-id="a2208-417">Nelle versioni precedenti a EF Core 3.0, la chiamata a `ToTable()` in un tipo derivato veniva ignorata poiché soltanto la strategia di mapping dell'ereditarietà era una tabella per gerarchia dove la chiamata non era valida.</span><span class="sxs-lookup"><span data-stu-id="a2208-417">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="a2208-418">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-418">**New behavior**</span></span>

<span data-ttu-id="a2208-419">A partire da EF Core 3.0 e in preparazione all'aggiunta del supporto per la tabella per tipo e per TPC in una versione successiva, la chiamata a `ToTable()` in un tipo derivato genera un'eccezione per evitare una modifica del mapping imprevista in futuro.</span><span class="sxs-lookup"><span data-stu-id="a2208-419">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="a2208-420">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-420">**Why**</span></span>

<span data-ttu-id="a2208-421">Attualmente il mapping di un tipo derivato in una tabella diversa non è un'operazione valida.</span><span class="sxs-lookup"><span data-stu-id="a2208-421">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="a2208-422">Questa modifica consente di evitare interruzioni future quando l'operazione diventerà un'operazione valida.</span><span class="sxs-lookup"><span data-stu-id="a2208-422">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="a2208-423">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-423">**Mitigations**</span></span>

<span data-ttu-id="a2208-424">Rimuovere qualsiasi tentativo di mapping di tipi derivati in altre tabelle.</span><span class="sxs-lookup"><span data-stu-id="a2208-424">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="a2208-425">ForSqlServerHasIndex sostituito con HasIndex</span><span class="sxs-lookup"><span data-stu-id="a2208-425">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="a2208-426">Problema n. 12366</span><span class="sxs-lookup"><span data-stu-id="a2208-426">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="a2208-427">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="a2208-427">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a2208-428">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-428">**Old behavior**</span></span>

<span data-ttu-id="a2208-429">Nelle versioni precedenti a EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offriva un metodo per configurare le colonne usate con `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="a2208-429">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="a2208-430">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-430">**New behavior**</span></span>

<span data-ttu-id="a2208-431">A partire da EF Core 3.0, l'uso di `Include` in un indice è supportato a livello relazionale.</span><span class="sxs-lookup"><span data-stu-id="a2208-431">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="a2208-432">Usare `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="a2208-432">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="a2208-433">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-433">**Why**</span></span>

<span data-ttu-id="a2208-434">Questa modifica è stata apportata per consolidare l'API per gli indici con `Includes` in un'unica posizione per tutti i provider di database.</span><span class="sxs-lookup"><span data-stu-id="a2208-434">This change was made to consolidate the API for indexes with `Includes` into one place for all database providers.</span></span>

<span data-ttu-id="a2208-435">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-435">**Mitigations**</span></span>

<span data-ttu-id="a2208-436">Usare la nuova API, come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="a2208-436">Use the new API, as shown above.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="a2208-437">EF Core non invia più pragma per l'imposizione della chiave esterna di SQLite</span><span class="sxs-lookup"><span data-stu-id="a2208-437">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="a2208-438">Problema n. 12151</span><span class="sxs-lookup"><span data-stu-id="a2208-438">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="a2208-439">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="a2208-439">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a2208-440">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-440">**Old behavior**</span></span>

<span data-ttu-id="a2208-441">Nelle versioni precedenti a EF Core 3.0, EF Core inviava `PRAGMA foreign_keys = 1` quando veniva aperta una connessione a SQLite.</span><span class="sxs-lookup"><span data-stu-id="a2208-441">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="a2208-442">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-442">**New behavior**</span></span>

<span data-ttu-id="a2208-443">A partire da EF Core 3.0, EF Core non invia più `PRAGMA foreign_keys = 1` quando viene aperta una connessione a SQLite.</span><span class="sxs-lookup"><span data-stu-id="a2208-443">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="a2208-444">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-444">**Why**</span></span>

<span data-ttu-id="a2208-445">Questa modifica è stata apportata poiché EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` per impostazione predefinita. Ciò significa che l'imposizione della chiave esterna è abilitata per impostazione predefinita e non deve essere abilitata in modo esplicito ogni volta che viene aperta una connessione.</span><span class="sxs-lookup"><span data-stu-id="a2208-445">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="a2208-446">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-446">**Mitigations**</span></span>

<span data-ttu-id="a2208-447">Le chiavi esterne sono abilitate per impostazione predefinita in SQLitePCLRaw.bundle_e_sqlite3, usato per impostazione predefinita per EF Core.</span><span class="sxs-lookup"><span data-stu-id="a2208-447">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="a2208-448">Per gli altri casi, è possibile abilitare le chiavi esterne specificando `Foreign Keys=True` nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="a2208-448">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="a2208-449">Microsoft.EntityFrameworkCore.Sqlite dipende ora da SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="a2208-449">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="a2208-450">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-450">**Old behavior**</span></span>

<span data-ttu-id="a2208-451">Nelle versioni precedenti a EF Core 3.0, EF Core usava `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="a2208-451">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="a2208-452">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-452">**New behavior**</span></span>

<span data-ttu-id="a2208-453">A partire da EF Core 3.0, EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="a2208-453">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="a2208-454">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-454">**Why**</span></span>

<span data-ttu-id="a2208-455">Questa modifica è stata apportata per rendere coerente la versione di SQLite usata in iOS con le altre piattaforme.</span><span class="sxs-lookup"><span data-stu-id="a2208-455">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="a2208-456">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-456">**Mitigations**</span></span>

<span data-ttu-id="a2208-457">Per usare la versione di SQLite nativa in iOS, configurare `Microsoft.Data.Sqlite` per l'uso di un'aggregazione `SQLitePCLRaw` diversa.</span><span class="sxs-lookup"><span data-stu-id="a2208-457">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="a2208-458">I valori char vengono ora archiviati come testo in SQLite</span><span class="sxs-lookup"><span data-stu-id="a2208-458">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="a2208-459">Problema n. 15020</span><span class="sxs-lookup"><span data-stu-id="a2208-459">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="a2208-460">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="a2208-460">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a2208-461">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-461">**Old behavior**</span></span>

<span data-ttu-id="a2208-462">I valori char in precedenza venivano archiviati come valori interi in SQLite.</span><span class="sxs-lookup"><span data-stu-id="a2208-462">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="a2208-463">Un valore char di *A* veniva ad esempio archiviato come valore intero 65.</span><span class="sxs-lookup"><span data-stu-id="a2208-463">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="a2208-464">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-464">**New behavior**</span></span>

<span data-ttu-id="a2208-465">I valori char vengono ora archiviati come testo.</span><span class="sxs-lookup"><span data-stu-id="a2208-465">Char values are now sotred as TEXT.</span></span>

<span data-ttu-id="a2208-466">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-466">**Why**</span></span>

<span data-ttu-id="a2208-467">L'archiviazione dei valori come testo è un'operazione più naturale e rende il database più compatibile con altre tecnologie.</span><span class="sxs-lookup"><span data-stu-id="a2208-467">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="a2208-468">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-468">**Mitigations**</span></span>

<span data-ttu-id="a2208-469">È possibile eseguire la migrazione dei database esistenti al nuovo formato eseguendo SQL nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="a2208-469">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="a2208-470">In EF Core è anche possibile continuare a usare il comportamento precedente configurando un convertitore di valori in queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="a2208-470">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="a2208-471">Microsoft.Data.Sqlite rimane comunque in grado di leggere i valori di caratteri presenti sia nelle colonne di valori interi sia in quelle di testo, quindi alcuni scenari potrebbero non richiedere alcuna azione.</span><span class="sxs-lookup"><span data-stu-id="a2208-471">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="a2208-472">Gli ID di migrazione vengono ora generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica</span><span class="sxs-lookup"><span data-stu-id="a2208-472">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="a2208-473">Problema n. 12978</span><span class="sxs-lookup"><span data-stu-id="a2208-473">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="a2208-474">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="a2208-474">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a2208-475">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="a2208-475">**Old behavior**</span></span>

<span data-ttu-id="a2208-476">Gli ID di migrazione venivano inavvertitamente generati usando con il calendario delle impostazioni cultura correnti.</span><span class="sxs-lookup"><span data-stu-id="a2208-476">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

<span data-ttu-id="a2208-477">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="a2208-477">**New behavior**</span></span>

<span data-ttu-id="a2208-478">Gli ID di migrazione ora vengono sempre generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica (calendario gregoriano).</span><span class="sxs-lookup"><span data-stu-id="a2208-478">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="a2208-479">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="a2208-479">**Why**</span></span>

<span data-ttu-id="a2208-480">L'ordine delle migrazioni è importante quando si esegue l'aggiornamento del database o si risolvono i conflitti di unione.</span><span class="sxs-lookup"><span data-stu-id="a2208-480">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="a2208-481">L'uso del calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica evita i problemi che possono verificarsi quando i membri del team hanno calendari di sistema diversi.</span><span class="sxs-lookup"><span data-stu-id="a2208-481">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="a2208-482">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="a2208-482">**Mitigations**</span></span>

<span data-ttu-id="a2208-483">Questa modifica interessa gli utenti che usano un calendario non gregoriano in cui l'anno ha un'estensione superiore al calendario gregoriano (come il calendario buddista tailandese).</span><span class="sxs-lookup"><span data-stu-id="a2208-483">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="a2208-484">Gli ID di migrazione esistenti dovranno essere aggiornati in modo che le nuove migrazioni vengano collocate dopo le migrazioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="a2208-484">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="a2208-485">L'ID di migrazione è disponibile nell'attributo di migrazione presente nei file di progettazione delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="a2208-485">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="a2208-486">È necessario aggiornare anche la tabella della cronologia delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="a2208-486">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```
