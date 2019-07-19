---
title: Modifiche che causano un'interruzione in EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 7cc0bd3946be2e63d9fb46a023bf6abe750ae0e3
ms.sourcegitcommit: e90d6cfa3e96f10b8b5275430759a66a0c714ed1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286487"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="abd2d-102">Modifiche che causano un'interruzione incluse in EF Core 3.0 (attualmente in anteprima)</span><span class="sxs-lookup"><span data-stu-id="abd2d-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="abd2d-103">Si tenga presente che i set di funzionalità e le pianificazioni delle versioni future sono sempre soggette a modifiche e che questa pagina, nonostante l'impegno profuso per mantenerla aggiornata, potrebbe non riflettere sempre i piani più recenti.</span><span class="sxs-lookup"><span data-stu-id="abd2d-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="abd2d-104">Le modifiche all'API e al comportamento seguenti possono potenzialmente interrompere le applicazioni sviluppate per EF Core 2.2.x durante l'aggiornamento alla versione 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="abd2d-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="abd2d-105">Le modifiche che si prevede abbiano impatto solo sui provider di database sono documentate nelle [modifiche che influiscono sul provider](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="abd2d-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="abd2d-106">Le interruzioni nelle nuove funzionalità introdotte da un'anteprima 3.0 a un'altra anteprima 3.0 non sono documentate.</span><span class="sxs-lookup"><span data-stu-id="abd2d-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="abd2d-107">Le query LINQ non vengono più valutate nel client</span><span class="sxs-lookup"><span data-stu-id="abd2d-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="abd2d-108">[Problema n. 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Vedere anche il problema n. 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="abd2d-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="abd2d-109">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-109">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-110">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-110">**Old behavior**</span></span>

<span data-ttu-id="abd2d-111">Nelle versioni precedenti alla versione 3.0, quando EF Core non era in grado di convertire un'espressione inclusa in una query in SQL o in un parametro, l'espressione veniva automaticamente valutata nel client.</span><span class="sxs-lookup"><span data-stu-id="abd2d-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="abd2d-112">Per impostazione predefinita, la valutazione client di espressioni potenzialmente dispendiose si limitava ad attivare solo un avviso.</span><span class="sxs-lookup"><span data-stu-id="abd2d-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="abd2d-113">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-113">**New behavior**</span></span>

<span data-ttu-id="abd2d-114">A partire dalla versione 3.0, EF Core consente solo la valutazione delle espressioni nella proiezione di primo livello (l'ultima chiamata `Select()` nella query) nel client.</span><span class="sxs-lookup"><span data-stu-id="abd2d-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="abd2d-115">Quando le espressioni in altre posizioni all'interno della query non possono essere convertite in SQL o in un parametro, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="abd2d-116">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-116">**Why**</span></span>

<span data-ttu-id="abd2d-117">La valutazione client automatica delle query consente di eseguire numerose query anche nel caso in cui parti importanti delle query non possono essere convertite.</span><span class="sxs-lookup"><span data-stu-id="abd2d-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="abd2d-118">Questo comportamento può causare un comportamento imprevisto e potenzialmente dannoso che può diventare evidente solo in produzione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="abd2d-119">Ad esempio, una condizione in una chiamata `Where()` che non può essere convertita può causare il trasferimento di tutte le righe della tabella del server di database e l'applicazione del filtro nel client.</span><span class="sxs-lookup"><span data-stu-id="abd2d-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="abd2d-120">È probabile che questa situazione non venga rilevata se la tabella contiene solo alcune righe in fase di sviluppo, ma che abbia un grande impatto quando l'applicazione passa in produzione dove la tabella può contenere milioni di righe.</span><span class="sxs-lookup"><span data-stu-id="abd2d-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="abd2d-121">Gli avvisi di valutazione client inoltre si sono rivelati molto facili da ignorare durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="abd2d-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="abd2d-122">Inoltre, la valutazione client automatica può causare problemi in cui il miglioramento della conversione di query per espressioni specifiche causa modifiche impreviste che causano un'interruzione da una versione all'altra.</span><span class="sxs-lookup"><span data-stu-id="abd2d-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="abd2d-123">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-123">**Mitigations**</span></span>

<span data-ttu-id="abd2d-124">Se una query non può essere convertita completamente, riscrivere la query in un formato che possa essere convertito o usare `AsEnumerable()`, `ToList()` o un elemento simile per riportare in modo esplicito i dati nel client dove possono essere quindi ulteriormente elaborati usando LINQ to Objects.</span><span class="sxs-lookup"><span data-stu-id="abd2d-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="abd2d-125">Entity Framework Core non è più incluso nel framework condiviso di ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="abd2d-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="abd2d-126">Annunci problema n. 325</span><span class="sxs-lookup"><span data-stu-id="abd2d-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="abd2d-127">Questa modifica è stata introdotta in ASP.NET Core 3.0 anteprima 1.</span><span class="sxs-lookup"><span data-stu-id="abd2d-127">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="abd2d-128">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-128">**Old behavior**</span></span>

<span data-ttu-id="abd2d-129">Nelle versioni precedenti ad ASP.NET Core 3.0, quando si aggiungeva un riferimento a un pacchetto in `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, veniva inserito EF Core e alcuni dei provider di dati di EF Core come il provider di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="abd2d-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="abd2d-130">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-130">**New behavior**</span></span>

<span data-ttu-id="abd2d-131">A partire dalla versione 3.0, il framework condiviso di ASP.NET Core non include EF Core o provider di dati di EF Core.</span><span class="sxs-lookup"><span data-stu-id="abd2d-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="abd2d-132">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-132">**Why**</span></span>

<span data-ttu-id="abd2d-133">Prima di questa modifica, per ottenere EF Core erano necessarie procedure diverse, a seconda che l'applicazione avesse o meno come destinazione ASP.NET Core e SQL Server.</span><span class="sxs-lookup"><span data-stu-id="abd2d-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="abd2d-134">Inoltre, l'aggiornamento di ASP.NET Core forzava l'aggiornamento di EF Core e del provider di SQL Server, non sempre auspicabile.</span><span class="sxs-lookup"><span data-stu-id="abd2d-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="abd2d-135">Con questa modifica, la procedura per ottenere EF Core è la stessa in tutti i provider, le implementazioni .NET supportate e i tipi di applicazioni.</span><span class="sxs-lookup"><span data-stu-id="abd2d-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="abd2d-136">Gli sviluppatori possono ora controllare anche in modo preciso quando vengono aggiornati EF Core e i provider di dati di EF Core.</span><span class="sxs-lookup"><span data-stu-id="abd2d-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="abd2d-137">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-137">**Mitigations**</span></span>

<span data-ttu-id="abd2d-138">Per usare EF Core in un'applicazione ASP.NET Core 3.0 o in un'altra applicazione supportata, aggiungere in modo esplicito un riferimento al pacchetto al provider di database di EF Core che verrà usato dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="abd2d-139">Lo strumento della riga di comando di EF Core, dotnet ef, non è più incluso in .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="abd2d-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="abd2d-140">Problema n. 14016</span><span class="sxs-lookup"><span data-stu-id="abd2d-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="abd2d-141">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4 e nella versione corrispondente di .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="abd2d-141">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="abd2d-142">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-142">**Old behavior**</span></span>

<span data-ttu-id="abd2d-143">Prima della versione 3.0, lo strumento `dotnet ef` era incluso in .NET Core SDK ed era immediatamente disponibile dalla riga di comando di qualsiasi progetto senza richiedere passaggi aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="abd2d-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="abd2d-144">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-144">**New behavior**</span></span>

<span data-ttu-id="abd2d-145">A partire dalla versione 3.0, .NET SDK non include lo strumento `dotnet ef` pertanto, prima di poterlo usare, è necessario installarlo in modo esplicito come strumento locale o globale.</span><span class="sxs-lookup"><span data-stu-id="abd2d-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="abd2d-146">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-146">**Why**</span></span>

<span data-ttu-id="abd2d-147">Questa modifica consente di distribuire e aggiornare `dotnet ef` come uno strumento della riga di comando di .NET in NuGet, coerentemente con il fatto che anche EF Core 3.0 viene distribuito come pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="abd2d-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="abd2d-148">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-148">**Mitigations**</span></span>

<span data-ttu-id="abd2d-149">Per essere in grado di gestire le migrazioni o eseguire lo scaffolding di `DbContext`, installare `dotnet-ef` come strumento globale:</span><span class="sxs-lookup"><span data-stu-id="abd2d-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="abd2d-150">È inoltre possibile ottenerlo come strumento locale quando si ripristinano le dipendenze di un progetto che lo dichiara come dipendenza di strumenti utilizzando un [file manifesto dello strumento](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="abd2d-150">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="abd2d-151">I metodi FromSql, ExecuteSql ed ExecuteSqlAsync sono stati rinominati</span><span class="sxs-lookup"><span data-stu-id="abd2d-151">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="abd2d-152">Problema n. 10996</span><span class="sxs-lookup"><span data-stu-id="abd2d-152">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="abd2d-153">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-153">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-154">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-154">**Old behavior**</span></span>

<span data-ttu-id="abd2d-155">Prima di EF Core 3.0, erano disponibili overload per questi nomi di metodo per supportare l'uso con una stringa normale o una stringa che deve essere interpolata in SQL e parametri.</span><span class="sxs-lookup"><span data-stu-id="abd2d-155">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="abd2d-156">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-156">**New behavior**</span></span>

<span data-ttu-id="abd2d-157">A partire da EF Core 3.0, usare `FromSqlRaw`, `ExecuteSqlRaw` e `ExecuteSqlRawAsync` per creare una query con parametri in cui i parametri vengono passati separatamente dalla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="abd2d-157">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="abd2d-158">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="abd2d-158">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="abd2d-159">Usare `FromSqlInterpolated`, `ExecuteSqlInterpolated`, e `ExecuteSqlInterpolatedAsync` per creare una query con parametri in cui i parametri vengono passati come parte di una stringa di query interpolata.</span><span class="sxs-lookup"><span data-stu-id="abd2d-159">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="abd2d-160">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="abd2d-160">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="abd2d-161">Si noti che entrambe le query precedenti produrranno lo stesso codice SQL con parametri con gli stessi parametri SQL.</span><span class="sxs-lookup"><span data-stu-id="abd2d-161">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="abd2d-162">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-162">**Why**</span></span>

<span data-ttu-id="abd2d-163">Con gli overload di metodi come questi, è molto facile chiamare accidentalmente il metodo con stringa non elaborata anche se l'intento era chiamare il metodo con stringa interpolata e viceversa.</span><span class="sxs-lookup"><span data-stu-id="abd2d-163">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="abd2d-164">Il risultato potrebbero essere query senza parametri, quando invece è prevista la parametrizzazione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-164">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="abd2d-165">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-165">**Mitigations**</span></span>

<span data-ttu-id="abd2d-166">Passare all'uso dei nuovi nomi di metodo.</span><span class="sxs-lookup"><span data-stu-id="abd2d-166">Switch to use the new method names.</span></span>

## <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="abd2d-167">I metodi FromSql possono essere specificati solo in radici di query</span><span class="sxs-lookup"><span data-stu-id="abd2d-167">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="abd2d-168">Problema n. 15704</span><span class="sxs-lookup"><span data-stu-id="abd2d-168">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="abd2d-169">Questa modifica è stata introdotta in EF Core 3.0 anteprima 6.</span><span class="sxs-lookup"><span data-stu-id="abd2d-169">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="abd2d-170">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-170">**Old behavior**</span></span>

<span data-ttu-id="abd2d-171">Prima di EF Core 3.0, il metodo `FromSql` poteva essere specificato in un punto qualsiasi nella query.</span><span class="sxs-lookup"><span data-stu-id="abd2d-171">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="abd2d-172">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-172">**New behavior**</span></span>

<span data-ttu-id="abd2d-173">A partire da EF Core 3.0, i nuovi metodi `FromSqlRaw` e `FromSqlInterpolated` (che sostituiscono`FromSql`) possono essere specificati solo per radici di query, ad esempio direttamente in `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-173">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="abd2d-174">Qualsiasi tentativo di specificarli altrove causerà un errore di compilazione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-174">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="abd2d-175">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-175">**Why**</span></span>

<span data-ttu-id="abd2d-176">La specifica di `FromSql` in qualsiasi posizione diversa da un `DbSet` non ha alcun significato aggiuntivo oppure valore aggiunto e può causare ambiguità in determinati scenari.</span><span class="sxs-lookup"><span data-stu-id="abd2d-176">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="abd2d-177">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-177">**Mitigations**</span></span>

<span data-ttu-id="abd2d-178">Le chiamate di `FromSql` devono essere spostate in modo da comparire direttamente nel `DbSet` a cui si applicano.</span><span class="sxs-lookup"><span data-stu-id="abd2d-178">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

## <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="abd2d-179">~~L'esecuzione di query viene registrata a livello di debug~~ - Modifica annullata</span><span class="sxs-lookup"><span data-stu-id="abd2d-179">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="abd2d-180">Problema n. 14523</span><span class="sxs-lookup"><span data-stu-id="abd2d-180">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="abd2d-181">Questa modifica è stata annullata in EF Core 3.0 anteprima 7.</span><span class="sxs-lookup"><span data-stu-id="abd2d-181">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="abd2d-182">Questa modifica è stata annullata perché la nuova configurazione in EF Core 3.0 consente all'applicazione di specificare il livello di log per qualsiasi evento.</span><span class="sxs-lookup"><span data-stu-id="abd2d-182">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="abd2d-183">Ad esempio, per impostare la registrazione di SQL sul livello `Debug`, configurare il livello in modo esplicito in `OnConfiguring` o `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="abd2d-183">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="abd2d-184">I valori di chiave temporanei non sono più impostati nelle istanze di entità</span><span class="sxs-lookup"><span data-stu-id="abd2d-184">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="abd2d-185">Problema n. 12378</span><span class="sxs-lookup"><span data-stu-id="abd2d-185">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="abd2d-186">Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.</span><span class="sxs-lookup"><span data-stu-id="abd2d-186">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="abd2d-187">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-187">**Old behavior**</span></span>

<span data-ttu-id="abd2d-188">Nelle versioni precedenti a EF Core 3.0 i valori temporanei venivano assegnati a tutte le proprietà di chiave per cui veniva in seguito generato un valore reale dal database.</span><span class="sxs-lookup"><span data-stu-id="abd2d-188">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="abd2d-189">In genere questi valori temporanei erano numeri negativi elevati.</span><span class="sxs-lookup"><span data-stu-id="abd2d-189">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="abd2d-190">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-190">**New behavior**</span></span>

<span data-ttu-id="abd2d-191">A partire dalla versione 3.0, EF Core archivia il valore di chiave temporaneo come parte delle informazioni di rilevamento dell'entità e non modifica la proprietà di chiave.</span><span class="sxs-lookup"><span data-stu-id="abd2d-191">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="abd2d-192">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-192">**Why**</span></span>

<span data-ttu-id="abd2d-193">Questa modifica è stata apportata per impedire che i valori di chiave temporanei diventino erroneamente permanenti quando un'entità rilevata in precedenza da un'istanza `DbContext` viene spostata in un'altra istanza `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-193">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="abd2d-194">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-194">**Mitigations**</span></span>

<span data-ttu-id="abd2d-195">Le applicazioni che assegnano valori di chiave primaria in chiavi esterne per creare associazioni tra le entità possono dipendere dal comportamento precedente se le chiavi primarie vengono generate dall'archivio e appartengono a entità con stato `Added`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-195">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="abd2d-196">Questo può essere evitato:</span><span class="sxs-lookup"><span data-stu-id="abd2d-196">This can be avoided by:</span></span>
* <span data-ttu-id="abd2d-197">Non usando chiavi generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="abd2d-197">Not using store-generated keys.</span></span>
* <span data-ttu-id="abd2d-198">Impostando le proprietà di navigazione in modo da creare relazioni anziché impostando valori di chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="abd2d-198">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="abd2d-199">Ottenendo i valori di chiave temporanei effettivi dalle informazioni di rilevamento dell'entità.</span><span class="sxs-lookup"><span data-stu-id="abd2d-199">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="abd2d-200">Ad esempio, `context.Entry(blog).Property(e => e.Id).CurrentValue` restituisce il valore temporaneo anche quando `blog.Id` non è stato impostato.</span><span class="sxs-lookup"><span data-stu-id="abd2d-200">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="abd2d-201">DetectChanges rispetta i valori di chiave generati dall'archivio</span><span class="sxs-lookup"><span data-stu-id="abd2d-201">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="abd2d-202">Problema n. 14616</span><span class="sxs-lookup"><span data-stu-id="abd2d-202">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="abd2d-203">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="abd2d-203">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="abd2d-204">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-204">**Old behavior**</span></span>

<span data-ttu-id="abd2d-205">Nelle versioni precedenti a EF Core 3.0 un'entità non rilevata individuata da `DetectChanges` veniva rilevata nello stato `Added` e inserita come nuova riga quando veniva eseguita una chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-205">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="abd2d-206">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-206">**New behavior**</span></span>

<span data-ttu-id="abd2d-207">A partire da EF Core 3.0, se un'entità usa valori di chiave generati e viene impostato un valore di chiave, l'entità viene rilevata nello stato `Modified`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-207">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="abd2d-208">Ciò significa che si presuppone l'esistenza di una riga per l'entità che viene aggiornata quando viene eseguita una chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-208">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="abd2d-209">Se il valore di chiave non viene impostato o se il tipo di entità non usa chiavi generate, la nuova entità viene rilevata come `Added` come nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="abd2d-209">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="abd2d-210">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-210">**Why**</span></span>

<span data-ttu-id="abd2d-211">Questa modifica è stata apportata per rendere più semplice e coerente l'uso di grafici di entità disconnesse con chiavi generate dall'archivio.</span><span class="sxs-lookup"><span data-stu-id="abd2d-211">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="abd2d-212">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-212">**Mitigations**</span></span>

<span data-ttu-id="abd2d-213">Questa modifica può interrompere un'applicazione se un tipo di entità è configurato per l'uso di chiavi generate ma i valori di chiave sono impostati in modo esplicito per le nuove istanze.</span><span class="sxs-lookup"><span data-stu-id="abd2d-213">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="abd2d-214">La correzione consiste nel configurare in modo esplicito le proprietà di chiave per non usare valori generati.</span><span class="sxs-lookup"><span data-stu-id="abd2d-214">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="abd2d-215">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="abd2d-215">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="abd2d-216">Oppure con annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="abd2d-216">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="abd2d-217">Le eliminazioni a catena vengono ora eseguite immediatamente per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="abd2d-217">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="abd2d-218">Problema n. 10114</span><span class="sxs-lookup"><span data-stu-id="abd2d-218">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="abd2d-219">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="abd2d-219">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="abd2d-220">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-220">**Old behavior**</span></span>

<span data-ttu-id="abd2d-221">Nelle versioni precedenti alla versione 3.0, EF Core applicava azioni a catena (eliminazione delle entità dipendenti quando veniva eliminata un'entità di sicurezza obbligatoria o veniva recisa la relazione con un'entità di sicurezza obbligatoria) solo dopo la chiamata a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="abd2d-221">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="abd2d-222">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-222">**New behavior**</span></span>

<span data-ttu-id="abd2d-223">A partire dalla versione 3.0, EF Core applica le azioni a catena non appena viene rilevata la condizione di attivazione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-223">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="abd2d-224">Ad esempio, la chiamata a `context.Remove()` per eliminare un'entità di sicurezza causa anche l'impostazione immediata di tutti i dipendenti obbligatori correlati rilevati su `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-224">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="abd2d-225">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-225">**Why**</span></span>

<span data-ttu-id="abd2d-226">Questa modifica è stata apportata per migliorare l'esperienza di associazione di dati e degli scenari di controllo in cui è importante individuare le entità che verranno eliminate _prima_ della chiamata a `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-226">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="abd2d-227">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-227">**Mitigations**</span></span>

<span data-ttu-id="abd2d-228">Il comportamento precedente può essere ripristinato tramite le impostazioni in `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-228">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="abd2d-229">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="abd2d-229">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="abd2d-230">Semantica più chiara per DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="abd2d-230">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="abd2d-231">Problema n. 12661</span><span class="sxs-lookup"><span data-stu-id="abd2d-231">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="abd2d-232">Questa modifica è stata introdotta in EF Core 3.0 anteprima 5.</span><span class="sxs-lookup"><span data-stu-id="abd2d-232">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="abd2d-233">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-233">**Old behavior**</span></span>

<span data-ttu-id="abd2d-234">Prima della versione 3.0, `DeleteBehavior.Restrict` creava chiavi esterne nel database con la semantica `Restrict`, ma modificava anche la correzione interna in modo non ovvio.</span><span class="sxs-lookup"><span data-stu-id="abd2d-234">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="abd2d-235">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-235">**New behavior**</span></span>

<span data-ttu-id="abd2d-236">A partire dalla versione 3.0, `DeleteBehavior.Restrict` assicura che le chiavi esterne vengano create con la semantica `Restrict`, ovvero non a cascata e con generazione di un'eccezione in caso di violazione di vincolo, senza influire sulla correzione interna di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="abd2d-236">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="abd2d-237">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-237">**Why**</span></span>

<span data-ttu-id="abd2d-238">Questa modifica è stata apportata per migliorare l'esperienza di uso di `DeleteBehavior` in modo intuitivo, senza effetti collaterali imprevisti.</span><span class="sxs-lookup"><span data-stu-id="abd2d-238">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="abd2d-239">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-239">**Mitigations**</span></span>

<span data-ttu-id="abd2d-240">Il comportamento precedente può essere ripristinato tramite `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-240">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="abd2d-241">I tipi di query vengono consolidati con tipi di entità</span><span class="sxs-lookup"><span data-stu-id="abd2d-241">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="abd2d-242">Problema n. 14194</span><span class="sxs-lookup"><span data-stu-id="abd2d-242">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="abd2d-243">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="abd2d-243">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="abd2d-244">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-244">**Old behavior**</span></span>

<span data-ttu-id="abd2d-245">Nelle versioni precedenti a EF Core 3.0 i [tipi di query](xref:core/modeling/query-types) erano uno strumento per eseguire query su dati che non definiscono una chiave primaria in modo strutturato.</span><span class="sxs-lookup"><span data-stu-id="abd2d-245">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="abd2d-246">Veniva infatti usato un tipo di query per eseguire il mapping di tipi di entità senza chiavi (più probabilmente da una vista, ma anche da una tabella), mentre veniva usato un tipo di entità normale quando era disponibile una chiave (più probabilmente da una tabella, ma anche da una vista).</span><span class="sxs-lookup"><span data-stu-id="abd2d-246">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="abd2d-247">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-247">**New behavior**</span></span>

<span data-ttu-id="abd2d-248">Un tipo di query diventa ora semplicemente un tipo di entità senza chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="abd2d-248">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="abd2d-249">I tipi di entità senza chiave hanno la stessa funzionalità dei tipi di query nelle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="abd2d-249">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="abd2d-250">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-250">**Why**</span></span>

<span data-ttu-id="abd2d-251">Questa modifica è stata apportata per ridurre la confusione riguardo lo scopo dei tipi di query.</span><span class="sxs-lookup"><span data-stu-id="abd2d-251">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="abd2d-252">In particolare, si tratta di tipi di entità senza chiave che sono intrinsecamente di sola lettura per questo motivo ma non dovrebbero essere usati solo perché un tipo di entità deve essere di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="abd2d-252">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="abd2d-253">Analogamente, spesso vengono mappati alle viste solo perché le viste spesso non definiscono le chiavi.</span><span class="sxs-lookup"><span data-stu-id="abd2d-253">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="abd2d-254">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-254">**Mitigations**</span></span>

<span data-ttu-id="abd2d-255">Le parti dell'API seguenti sono ora obsolete:</span><span class="sxs-lookup"><span data-stu-id="abd2d-255">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="abd2d-256">**`ModelBuilder.Query<>()`** - È necessario chiamare `ModelBuilder.Entity<>().HasNoKey()` per contrassegnare un tipo di entità come tipo senza chiavi.</span><span class="sxs-lookup"><span data-stu-id="abd2d-256">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="abd2d-257">Non ne viene eseguita la configurazione per convenzione per evitare una configurazione errata quando è prevista una chiave primaria che tuttavia non corrisponde alla convenzione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-257">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="abd2d-258">**`DbQuery<>`** - Usare `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-258">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="abd2d-259">**`DbContext.Query<>()`** - Usare `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-259">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="abd2d-260">L'API di configurazione per le relazioni di tipo di proprietà è stata modificata</span><span class="sxs-lookup"><span data-stu-id="abd2d-260">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="abd2d-261">[Problema n. 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema n. 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema n. 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="abd2d-261">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="abd2d-262">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="abd2d-262">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="abd2d-263">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-263">**Old behavior**</span></span>

<span data-ttu-id="abd2d-264">Nelle versioni precedenti a EF Core 3.0 la configurazione della relazione di proprietà veniva eseguita direttamente dopo la chiamata a `OwnsOne` o `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-264">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="abd2d-265">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-265">**New behavior**</span></span>

<span data-ttu-id="abd2d-266">A partire da EF Core 3.0, è disponibile l'API Fluent per configurare una proprietà di navigazione per il proprietario usando `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-266">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="abd2d-267">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="abd2d-267">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="abd2d-268">La configurazione correlata alla relazione tra proprietario e elemento di proprietà deve essere ora concatenata dopo `WithOwner()` in modo analogo a come vengono configurate altre relazioni.</span><span class="sxs-lookup"><span data-stu-id="abd2d-268">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="abd2d-269">La configurazione per il tipo di proprietà viene comunque concatenata dopo `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-269">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="abd2d-270">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="abd2d-270">For example:</span></span>

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

<span data-ttu-id="abd2d-271">Inoltre, la chiamata a `Entity()`, `HasOne()` o `Set()` con una destinazione di tipo di proprietà genera ora un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-271">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="abd2d-272">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-272">**Why**</span></span>

<span data-ttu-id="abd2d-273">Questa modifica è stata apportata per creare una separazione più netta tra la configurazione del tipo di proprietà e la _relazione_ con il tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="abd2d-273">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="abd2d-274">Ciò consente di eliminare ambiguità e confusione su metodi come `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-274">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="abd2d-275">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-275">**Mitigations**</span></span>

<span data-ttu-id="abd2d-276">Modificare la configurazione delle relazioni dei tipi di proprietà per usare la superficie della nuova API come illustrato nell'esempio precedente.</span><span class="sxs-lookup"><span data-stu-id="abd2d-276">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="abd2d-277">Le entità dipendenti che condividono la tabella con l'entità di sicurezza sono ora facoltative</span><span class="sxs-lookup"><span data-stu-id="abd2d-277">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="abd2d-278">Problema n. 9005</span><span class="sxs-lookup"><span data-stu-id="abd2d-278">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="abd2d-279">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-279">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-280">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-280">**Old behavior**</span></span>

<span data-ttu-id="abd2d-281">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="abd2d-281">Consider the following model:</span></span>
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
<span data-ttu-id="abd2d-282">Prima di EF Core 3.0, se `OrderDetails` è di proprietà di `Order` o viene mappato in modo esplicito alla stessa tabella, era sempre necessaria un'istanza di `OrderDetails` per l'aggiunta di un nuovo `Order`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-282">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="abd2d-283">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-283">**New behavior**</span></span>

<span data-ttu-id="abd2d-284">A partire dalla versione 3.0, EF Core consente di aggiungere un `Order` senza un `OrderDetails` ed esegue il mapping di tutte le proprietà di `OrderDetails`, tranne che la chiave primaria, a colonne che ammettono valori Null.</span><span class="sxs-lookup"><span data-stu-id="abd2d-284">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="abd2d-285">In fase di query, EF Core imposta `OrderDetails` su `null` se una delle relative proprietà obbligatorie non ha un valore o se non sono presenti proprietà obbligatorie oltre alla chiave primaria e tutte le proprietà sono `null`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-285">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="abd2d-286">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-286">**Mitigations**</span></span>

<span data-ttu-id="abd2d-287">Se il modello include una tabella condivisa dipendente con tutte le colonne facoltative, ma è previsto che la navigazione che punta a essa non sia `null`, l'applicazione deve essere modificata per gestire casi in cui la navigazione è `null`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-287">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="abd2d-288">Se questo non è possibile, è consigliabile aggiungere una proprietà obbligatoria al tipo di entità o assegnare un valore non `null` ad almeno una proprietà.</span><span class="sxs-lookup"><span data-stu-id="abd2d-288">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="abd2d-289">Tutte le entità che condividono una tabella con una colonna di token di concorrenza devono eseguirne il mapping a una proprietà</span><span class="sxs-lookup"><span data-stu-id="abd2d-289">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="abd2d-290">Problema n. 14154</span><span class="sxs-lookup"><span data-stu-id="abd2d-290">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="abd2d-291">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-291">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-292">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-292">**Old behavior**</span></span>

<span data-ttu-id="abd2d-293">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="abd2d-293">Consider the following model:</span></span>
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
<span data-ttu-id="abd2d-294">Prima di EF Core 3.0, se `OrderDetails` è di proprietà di `Order` o è mappato in modo esplicito alla stessa tabella, il solo aggiornamento di `OrderDetails` non aggiornerà il valore `Version` nel client e l'aggiornamento successivo avrà esito negativo.</span><span class="sxs-lookup"><span data-stu-id="abd2d-294">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="abd2d-295">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-295">**New behavior**</span></span>

<span data-ttu-id="abd2d-296">A partire dalla versione 3.0, EF Core propaga il nuovo valore `Version` a `Order` se è proprietario di `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-296">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="abd2d-297">In caso contrario, viene generata un'eccezione durante la convalida del modello.</span><span class="sxs-lookup"><span data-stu-id="abd2d-297">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="abd2d-298">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-298">**Why**</span></span>

<span data-ttu-id="abd2d-299">Questa modifica è stata apportata per evitare un valore del token di concorrenza non aggiornato quando viene aggiornata solo una delle entità mappate alla stessa tabella.</span><span class="sxs-lookup"><span data-stu-id="abd2d-299">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="abd2d-300">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-300">**Mitigations**</span></span>

<span data-ttu-id="abd2d-301">Tutte le entità che condividono la tabella devono includere una proprietà mappata alla colonna del token di concorrenza.</span><span class="sxs-lookup"><span data-stu-id="abd2d-301">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="abd2d-302">È possibile crearne una in stato shadow:</span><span class="sxs-lookup"><span data-stu-id="abd2d-302">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="abd2d-303">Per le proprietà ereditate da tipi senza mapping viene ora eseguito il mapping a una singola colonna per tutti i tipi derivati</span><span class="sxs-lookup"><span data-stu-id="abd2d-303">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="abd2d-304">Problema n. 13998</span><span class="sxs-lookup"><span data-stu-id="abd2d-304">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="abd2d-305">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-305">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-306">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-306">**Old behavior**</span></span>

<span data-ttu-id="abd2d-307">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="abd2d-307">Consider the following model:</span></span>
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

<span data-ttu-id="abd2d-308">Prima di EF Core 3.0, per la proprietà `ShippingAddress` sarebbe stato eseguito il mapping a colonne separate per `BulkOrder` e `Order` per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="abd2d-308">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="abd2d-309">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-309">**New behavior**</span></span>

<span data-ttu-id="abd2d-310">A partire dalla versione 3.0, EF Core crea solo una colonna per `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-310">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="abd2d-311">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-311">**Why**</span></span>

<span data-ttu-id="abd2d-312">Il comportamento precedente non era previsto.</span><span class="sxs-lookup"><span data-stu-id="abd2d-312">The old behavoir was unexpected.</span></span>

<span data-ttu-id="abd2d-313">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-313">**Mitigations**</span></span>

<span data-ttu-id="abd2d-314">È ancora possibile eseguire il mapping esplicito della proprietà a una colonna separata per i tipi derivati:</span><span class="sxs-lookup"><span data-stu-id="abd2d-314">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="abd2d-315">La convenzione di proprietà di chiave esterna non ha più lo stesso nome della proprietà dell'entità di sicurezza</span><span class="sxs-lookup"><span data-stu-id="abd2d-315">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="abd2d-316">Problema n. 13274</span><span class="sxs-lookup"><span data-stu-id="abd2d-316">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="abd2d-317">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="abd2d-317">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="abd2d-318">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-318">**Old behavior**</span></span>

<span data-ttu-id="abd2d-319">Si consideri il modello seguente:</span><span class="sxs-lookup"><span data-stu-id="abd2d-319">Consider the following model:</span></span>
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
<span data-ttu-id="abd2d-320">Nelle versioni precedenti a EF Core 3.0 veniva usata la proprietà `CustomerId` per la chiave esterna per convenzione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-320">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="abd2d-321">Tuttavia, se `Order` è un tipo di proprietà, `CustomerId` sarebbe la chiave primaria e ciò non è in genere auspicabile.</span><span class="sxs-lookup"><span data-stu-id="abd2d-321">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="abd2d-322">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-322">**New behavior**</span></span>

<span data-ttu-id="abd2d-323">A partire da 3.0, EF Core non tenta di usare le proprietà per le chiavi esterne per convenzione se hanno lo stesso nome della proprietà dell'entità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="abd2d-323">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="abd2d-324">Viene ancora eseguita la corrispondenza tra i criteri del nome del tipo dell'entità di sicurezza concatenato al nome della proprietà dell'entità di sicurezza e il nome di navigazione concatenato al nome della proprietà dell'entità di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="abd2d-324">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="abd2d-325">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="abd2d-325">For example:</span></span>

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

<span data-ttu-id="abd2d-326">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-326">**Why**</span></span>

<span data-ttu-id="abd2d-327">Questa modifica è stata apportata per evitare una definizione errata della proprietà di chiave primaria nel tipo di proprietà.</span><span class="sxs-lookup"><span data-stu-id="abd2d-327">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="abd2d-328">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-328">**Mitigations**</span></span>

<span data-ttu-id="abd2d-329">Se la proprietà è stata progettata per essere la chiave esterna e di conseguenza parte della chiave primaria, configurarla in modo esplicito come chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="abd2d-329">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="abd2d-330">La connessione al database viene ora chiusa se non viene più usata prima del completamento di TransactionScope</span><span class="sxs-lookup"><span data-stu-id="abd2d-330">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="abd2d-331">Problema n. 14218</span><span class="sxs-lookup"><span data-stu-id="abd2d-331">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="abd2d-332">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-332">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-333">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-333">**Old behavior**</span></span>

<span data-ttu-id="abd2d-334">Prima di EF Core 3.0, se il contesto apre la connessione all'interno di un `TransactionScope`, la connessione rimane aperta mentre è attivo il `TransactionScope` corrente.</span><span class="sxs-lookup"><span data-stu-id="abd2d-334">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="abd2d-335">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-335">**New behavior**</span></span>

<span data-ttu-id="abd2d-336">A partire dalla versione 3.0, EF Core chiude la connessione non appena non viene più usata.</span><span class="sxs-lookup"><span data-stu-id="abd2d-336">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="abd2d-337">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-337">**Why**</span></span>

<span data-ttu-id="abd2d-338">Questa modifica consente di usare più contesti nello stesso `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-338">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="abd2d-339">Il nuovo comportamento corrisponde anche a EF6.</span><span class="sxs-lookup"><span data-stu-id="abd2d-339">The new behavior also matches EF6.</span></span>

<span data-ttu-id="abd2d-340">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-340">**Mitigations**</span></span>

<span data-ttu-id="abd2d-341">Se la connessione deve rimanere aperta, la chiamata esplicita di `OpenConnection()` garantirà che EF Core non la chiuda prematuramente:</span><span class="sxs-lookup"><span data-stu-id="abd2d-341">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="abd2d-342">Ogni proprietà usa la generazione di chiavi di tipo intero in memoria indipendenti</span><span class="sxs-lookup"><span data-stu-id="abd2d-342">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="abd2d-343">Problema n. 6872</span><span class="sxs-lookup"><span data-stu-id="abd2d-343">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="abd2d-344">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-344">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-345">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-345">**Old behavior**</span></span>

<span data-ttu-id="abd2d-346">Nelle versioni precedenti a EF Core 3.0 veniva usato un unico generatore di valori condiviso per tutte le proprietà di chiavi di tipo intero in memoria.</span><span class="sxs-lookup"><span data-stu-id="abd2d-346">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="abd2d-347">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-347">**New behavior**</span></span>

<span data-ttu-id="abd2d-348">A partire da EF Core 3.0, ogni proprietà di chiave di tipo intero riceve un generatore di valori quando viene usato il database in memoria.</span><span class="sxs-lookup"><span data-stu-id="abd2d-348">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="abd2d-349">Inoltre, se il database viene eliminato, la generazione di chiavi viene reimpostata per tutte le tabelle.</span><span class="sxs-lookup"><span data-stu-id="abd2d-349">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="abd2d-350">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-350">**Why**</span></span>

<span data-ttu-id="abd2d-351">Questa modifica è stata apportata per allineare maggiormente la generazione di chiavi in memoria alla generazione di chiavi del database reale e per migliorare la possibilità di isolare i test l'uno dall'altro quando viene usato il database in memoria.</span><span class="sxs-lookup"><span data-stu-id="abd2d-351">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="abd2d-352">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-352">**Mitigations**</span></span>

<span data-ttu-id="abd2d-353">Ciò può interrompere un'applicazione che si basa sull'impostazione di valori di chiave in memoria specifici.</span><span class="sxs-lookup"><span data-stu-id="abd2d-353">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="abd2d-354">È consigliabile non basare l'applicazione su valori di chiave specifici o eseguire l'aggiornamento per passare al nuovo comportamento.</span><span class="sxs-lookup"><span data-stu-id="abd2d-354">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="abd2d-355">I campi sottostanti vengono usati per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="abd2d-355">Backing fields are used by default</span></span>

[<span data-ttu-id="abd2d-356">Problema n. 12430</span><span class="sxs-lookup"><span data-stu-id="abd2d-356">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="abd2d-357">Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.</span><span class="sxs-lookup"><span data-stu-id="abd2d-357">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="abd2d-358">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-358">**Old behavior**</span></span>

<span data-ttu-id="abd2d-359">Nelle versioni precedenti alla versione 3.0, anche se il campo sottostante di una proprietà era noto, per impostazione predefinita EF Core eseguiva la lettura e la scrittura del valore della proprietà usando i metodi getter e setter della proprietà.</span><span class="sxs-lookup"><span data-stu-id="abd2d-359">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="abd2d-360">L'eccezione era costituita dall'esecuzione di query in cui il campo sottostante, se noto, veniva impostato direttamente.</span><span class="sxs-lookup"><span data-stu-id="abd2d-360">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="abd2d-361">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-361">**New behavior**</span></span>

<span data-ttu-id="abd2d-362">A partire da EF Core 3.0, se il campo sottostante di una proprietà è noto, la lettura e la scrittura della proprietà vengono sempre eseguite usando il campo sottostante.</span><span class="sxs-lookup"><span data-stu-id="abd2d-362">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="abd2d-363">Ciò potrebbe causare un'interruzione dell'applicazione se l'applicazione si basa su un comportamento aggiuntivo codificato nei metodi getter o setter.</span><span class="sxs-lookup"><span data-stu-id="abd2d-363">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="abd2d-364">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-364">**Why**</span></span>

<span data-ttu-id="abd2d-365">Questa modifica è stata apportata per impedire a EF Core di attivare per errore la logica di business per impostazione predefinita quando si eseguono operazioni di database che interessano le entità.</span><span class="sxs-lookup"><span data-stu-id="abd2d-365">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="abd2d-366">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-366">**Mitigations**</span></span>

<span data-ttu-id="abd2d-367">È possibile ripristinare il comportamento delle versioni precedenti alla versione 3.0 tramite la configurazione della modalità di accesso delle proprietà in `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-367">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="abd2d-368">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="abd2d-368">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="abd2d-369">Viene generata un'eccezione se vengono trovati più campi sottostanti compatibili</span><span class="sxs-lookup"><span data-stu-id="abd2d-369">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="abd2d-370">Problema n. 12523</span><span class="sxs-lookup"><span data-stu-id="abd2d-370">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="abd2d-371">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-371">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-372">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-372">**Old behavior**</span></span>

<span data-ttu-id="abd2d-373">Nelle versioni precedenti a EF Core 3.0, se più campi soddisfacevano le regole di ricerca del campo sottostante di una proprietà, veniva selezionato un solo campo in base a un ordine di precedenza.</span><span class="sxs-lookup"><span data-stu-id="abd2d-373">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="abd2d-374">Ciò poteva causare l'uso di un campo non corretto nei casi ambigui.</span><span class="sxs-lookup"><span data-stu-id="abd2d-374">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="abd2d-375">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-375">**New behavior**</span></span>

<span data-ttu-id="abd2d-376">A partire da EF Core 3.0, se più campi corrispondono alla stessa proprietà, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-376">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="abd2d-377">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-377">**Why**</span></span>

<span data-ttu-id="abd2d-378">Questa modifica è stata apportata per evitare di usare automaticamente un campo rispetto a un altro quando un solo campo può essere quello corretto.</span><span class="sxs-lookup"><span data-stu-id="abd2d-378">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="abd2d-379">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-379">**Mitigations**</span></span>

<span data-ttu-id="abd2d-380">Per le proprietà con campi sottostanti ambigui, il campo da usare deve essere specificato in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="abd2d-380">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="abd2d-381">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="abd2d-381">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="abd2d-382">I nomi delle proprietà solo campo devono corrispondere al nome di campo</span><span class="sxs-lookup"><span data-stu-id="abd2d-382">Field-only property names should match the field name</span></span>

<span data-ttu-id="abd2d-383">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-383">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-384">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-384">**Old behavior**</span></span>

<span data-ttu-id="abd2d-385">Prima di EF Core 3.0, era possibile specificare una proprietà con un valore stringa e se non veniva trovata alcuna proprietà con tale nome nel tipo CLR, EF Core tentava di trovare una corrispondenza con un campo tramite regole di convenzione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-385">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="abd2d-386">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-386">**New behavior**</span></span>

<span data-ttu-id="abd2d-387">A partire da EF Core 3.0, una proprietà solo campo deve corrispondere esattamente al nome del campo.</span><span class="sxs-lookup"><span data-stu-id="abd2d-387">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="abd2d-388">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-388">**Why**</span></span>

<span data-ttu-id="abd2d-389">Questa modifica è stata introdotta per evitare di usare lo stesso campo per due proprietà con nome simile. Le regole di corrispondenza per le proprietà solo campo sono state anche uniformate a quelle per le proprietà mappate a proprietà CLR.</span><span class="sxs-lookup"><span data-stu-id="abd2d-389">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="abd2d-390">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-390">**Mitigations**</span></span>

<span data-ttu-id="abd2d-391">Le proprietà solo campo devono avere lo stesso nome del campo a cui vengono mappate.</span><span class="sxs-lookup"><span data-stu-id="abd2d-391">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="abd2d-392">In un'anteprima successiva di EF Core 3.0 è prevista la riabilitazione della possibilità di configurare in modo esplicito un nome di campo diverso dal nome della proprietà:</span><span class="sxs-lookup"><span data-stu-id="abd2d-392">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="abd2d-393">AddDbContext/AddDbContextPool non chiamano più AddLogging e AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="abd2d-393">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="abd2d-394">Problema n. 14756</span><span class="sxs-lookup"><span data-stu-id="abd2d-394">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="abd2d-395">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-395">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-396">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-396">**Old behavior**</span></span>

<span data-ttu-id="abd2d-397">Prima di EF Core 3.0, la chiamata di `AddDbContext` oppure `AddDbContextPool` comporta anche la registrazione dei servizi di registrazione e memorizzazione nella cache con inserimento delle dipendenze tramite chiamate a [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="abd2d-397">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="abd2d-398">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-398">**New behavior**</span></span>

<span data-ttu-id="abd2d-399">A partire da EF Core 3.0, `AddDbContext` e `AddDbContextPool` non registreranno più questi servizi con inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="abd2d-399">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="abd2d-400">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-400">**Why**</span></span>

<span data-ttu-id="abd2d-401">EF Core 3.0 non richiede che questi servizi siano inclusi nel contenitore di inserimento delle dipendenze dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-401">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="abd2d-402">Tuttavia, se `ILoggerFactory` è registrato nel contenitore di inserimento delle dipendenze dell'applicazione, verrà ancora usato da EF Core.</span><span class="sxs-lookup"><span data-stu-id="abd2d-402">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="abd2d-403">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-403">**Mitigations**</span></span>

<span data-ttu-id="abd2d-404">Se l'applicazione necessita di questi servizi, registrarli in modo esplicito con il contenitore di inserimento delle dipendenze usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="abd2d-404">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="abd2d-405">DbContext.Entry esegue ora un DetectChanges locale</span><span class="sxs-lookup"><span data-stu-id="abd2d-405">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="abd2d-406">Problema n. 13552</span><span class="sxs-lookup"><span data-stu-id="abd2d-406">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="abd2d-407">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="abd2d-407">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="abd2d-408">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-408">**Old behavior**</span></span>

<span data-ttu-id="abd2d-409">Nelle versioni precedenti a EF Core 3.0 la chiamata a `DbContext.Entry` causava il rilevamento delle modifiche per tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="abd2d-409">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="abd2d-410">Ciò garantiva l'aggiornamento dello stato esposto in `EntityEntry`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-410">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="abd2d-411">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-411">**New behavior**</span></span>

<span data-ttu-id="abd2d-412">A partire da EF Core 3.0, la chiamata a `DbContext.Entry` causa ora solo il tentativo di rilevare le modifiche nell'entità specificata e in tutte le relative entità di sicurezza rilevate.</span><span class="sxs-lookup"><span data-stu-id="abd2d-412">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="abd2d-413">Ciò significa che le modifiche apportate altrove potrebbero non essere state rilevate tramite la chiamata al metodo e ciò potrebbe avere implicazioni sullo stato dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-413">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="abd2d-414">Si noti che se `ChangeTracker.AutoDetectChangesEnabled` è impostato su `false`, verrà disabilitato anche questo tipo di rilevamento delle modifiche locali.</span><span class="sxs-lookup"><span data-stu-id="abd2d-414">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="abd2d-415">Gli altri metodi che causano il rilevamento delle modifiche, ad esempio `ChangeTracker.Entries` e `SaveChanges`, causano ancora un `DetectChanges` completo di tutte le entità rilevate.</span><span class="sxs-lookup"><span data-stu-id="abd2d-415">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="abd2d-416">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-416">**Why**</span></span>

<span data-ttu-id="abd2d-417">Questa modifica è stata apportata per migliorare le prestazioni predefinite dell'uso di `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-417">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="abd2d-418">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-418">**Mitigations**</span></span>

<span data-ttu-id="abd2d-419">Chiamare `ChgangeTracker.DetectChanges()` in modo esplicito prima di chiamare `Entry` per garantire il comportamento precedente alla versione 3.0.</span><span class="sxs-lookup"><span data-stu-id="abd2d-419">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="abd2d-420">Le chiavi matrice di byte e di stringhe non vengono generate dal client per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="abd2d-420">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="abd2d-421">Problema n. 14617</span><span class="sxs-lookup"><span data-stu-id="abd2d-421">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="abd2d-422">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-422">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-423">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-423">**Old behavior**</span></span>

<span data-ttu-id="abd2d-424">Nelle versioni precedenti a EF Core 3.0 le proprietà di chiave `string` e `byte[]` potevano essere usate senza impostare in modo esplicito un valore non Null.</span><span class="sxs-lookup"><span data-stu-id="abd2d-424">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="abd2d-425">In questi casi, il valore di chiave veniva generato nel client come GUID, serializzato in byte per `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-425">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="abd2d-426">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-426">**New behavior**</span></span>

<span data-ttu-id="abd2d-427">A partire da EF Core 3.0 viene generata un'eccezione che indica che non è stato impostato alcun valore di chiave.</span><span class="sxs-lookup"><span data-stu-id="abd2d-427">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="abd2d-428">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-428">**Why**</span></span>

<span data-ttu-id="abd2d-429">Questa modifica è stata apportata poiché i valori `string`/`byte[]` generati dal client non risultano in genere utili e il comportamento predefinito rendeva complesso ragionare sui valori di chiave generati in un modo comune.</span><span class="sxs-lookup"><span data-stu-id="abd2d-429">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="abd2d-430">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-430">**Mitigations**</span></span>

<span data-ttu-id="abd2d-431">È possibile ripristinare il comportamento precedente alla versione 3.0 specificando in modo esplicito che le proprietà di chiave devono usare i valori generati se non viene impostato alcun altro valore non Null.</span><span class="sxs-lookup"><span data-stu-id="abd2d-431">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="abd2d-432">Ad esempio, con l'API Fluent:</span><span class="sxs-lookup"><span data-stu-id="abd2d-432">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="abd2d-433">Oppure con annotazioni dei dati:</span><span class="sxs-lookup"><span data-stu-id="abd2d-433">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="abd2d-434">ILoggerFactory è ora un servizio con ambito</span><span class="sxs-lookup"><span data-stu-id="abd2d-434">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="abd2d-435">Problema n. 14698</span><span class="sxs-lookup"><span data-stu-id="abd2d-435">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="abd2d-436">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="abd2d-436">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="abd2d-437">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-437">**Old behavior**</span></span>

<span data-ttu-id="abd2d-438">Nelle versioni precedenti a EF Core 3.0 `ILoggerFactory` veniva registrato come servizio singleton.</span><span class="sxs-lookup"><span data-stu-id="abd2d-438">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="abd2d-439">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-439">**New behavior**</span></span>

<span data-ttu-id="abd2d-440">A partire da EF Core 3.0, `ILoggerFactory` viene registrato come servizio con ambito.</span><span class="sxs-lookup"><span data-stu-id="abd2d-440">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="abd2d-441">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-441">**Why**</span></span>

<span data-ttu-id="abd2d-442">Questa modifica è stata apportata per consentire l'associazione di un logger a un'istanza `DbContext` che abilita altre funzionalità e rimuove alcuni casi di comportamento anomalo, ad esempio un'esplosione dei provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="abd2d-442">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="abd2d-443">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-443">**Mitigations**</span></span>

<span data-ttu-id="abd2d-444">Questa modifica non dovrebbe influire sul codice dell'applicazione a meno che non vengano registrati e usati servizi personalizzati nel provider di servizi interno di EF Core.</span><span class="sxs-lookup"><span data-stu-id="abd2d-444">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="abd2d-445">Questa condizione, tuttavia, non è comune.</span><span class="sxs-lookup"><span data-stu-id="abd2d-445">This isn't common.</span></span>
<span data-ttu-id="abd2d-446">In questi casi la maggior parte delle operazioni continuano a essere eseguite correttamente, ma qualsiasi servizio singleton dipendente da `ILoggerFactory` dovrà essere modificato per ottenere `ILoggerFactory` in modo diverso.</span><span class="sxs-lookup"><span data-stu-id="abd2d-446">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="abd2d-447">Se si verificano situazioni simili, inviare una segnalazione nello [strumento di gestione dei problemi in GitHub relativo a EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) per comunicare in che modo viene usato `ILoggerFactory` per consentirci di comprendere meglio come evitare ulteriori interruzioni in futuro.</span><span class="sxs-lookup"><span data-stu-id="abd2d-447">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="abd2d-448">I proxy di caricamento lazy non presuppongono più che le proprietà di navigazione vengano caricate completamente</span><span class="sxs-lookup"><span data-stu-id="abd2d-448">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="abd2d-449">Problema n. 12780</span><span class="sxs-lookup"><span data-stu-id="abd2d-449">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="abd2d-450">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-450">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-451">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-451">**Old behavior**</span></span>

<span data-ttu-id="abd2d-452">Nelle versioni precedenti a EF Core 3.0, quando `DbContext` veniva eliminato non esisteva alcun metodo per scoprire se una determinata proprietà di navigazione in un'entità ottenuta da un contesto specifico veniva caricata completamente o meno.</span><span class="sxs-lookup"><span data-stu-id="abd2d-452">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="abd2d-453">I proxy presupponevano invece che venisse caricata una navigazione di riferimento se era presente un valore non Null e che venisse caricata una navigazione di raccolta se era presente un valore.</span><span class="sxs-lookup"><span data-stu-id="abd2d-453">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="abd2d-454">In questi casi il tentativo di eseguire un caricamento lazy non avrebbe avuto alcun esito.</span><span class="sxs-lookup"><span data-stu-id="abd2d-454">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="abd2d-455">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-455">**New behavior**</span></span>

<span data-ttu-id="abd2d-456">A partire da Entity Framework Core 3.0, i proxy tengono traccia del caricamento o mancato caricamento di una proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-456">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="abd2d-457">Ciò significa che il tentativo di accedere a una proprietà di navigazione caricata dopo l'eliminazione del contesto non avrà mai alcun esito, anche quando la navigazione caricata è vuota o ha valore Null.</span><span class="sxs-lookup"><span data-stu-id="abd2d-457">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="abd2d-458">Al contrario, il tentativo di accedere a una proprietà di navigazione non caricata genera un'eccezione se il contesto viene eliminato anche se la proprietà di navigazione è una raccolta non vuota.</span><span class="sxs-lookup"><span data-stu-id="abd2d-458">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="abd2d-459">Se si verifica questa situazione significa che il codice dell'applicazione sta tentando di usare il caricamento lazy in un momento non valido e l'applicazione deve essere modificata in modo da non eseguire questa operazione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-459">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="abd2d-460">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-460">**Why**</span></span>

<span data-ttu-id="abd2d-461">Questa modifica è stata apportata per rendere coerente e corretto il comportamento durante un tentativo di caricamento lazy in un'istanza `DbContext` eliminata.</span><span class="sxs-lookup"><span data-stu-id="abd2d-461">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="abd2d-462">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-462">**Mitigations**</span></span>

<span data-ttu-id="abd2d-463">Aggiornare il codice dell'applicazione per fare in modo che non venga tentato il caricamento lazy con un contesto eliminato oppure specificare una configurazione in modo che non venga eseguita alcuna operazione come descritto nel messaggio di eccezione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-463">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="abd2d-464">La creazione di un numero eccessivo di provider di servizi interni è ora un errore per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="abd2d-464">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="abd2d-465">Problema n. 10236</span><span class="sxs-lookup"><span data-stu-id="abd2d-465">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="abd2d-466">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="abd2d-466">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="abd2d-467">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-467">**Old behavior**</span></span>

<span data-ttu-id="abd2d-468">Nelle versioni precedenti a EF Core 3.0 veniva registrato un avviso per le applicazioni che creavano un numero eccessivo di provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="abd2d-468">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="abd2d-469">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-469">**New behavior**</span></span>

<span data-ttu-id="abd2d-470">A partire da EF Core 3.0, l'avviso viene considerato un errore e viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-470">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="abd2d-471">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-471">**Why**</span></span>

<span data-ttu-id="abd2d-472">Questa modifica è stata apportata per gestire meglio il codice dell'applicazione tramite un'esposizione più esplicita di questa situazione di errore.</span><span class="sxs-lookup"><span data-stu-id="abd2d-472">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="abd2d-473">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-473">**Mitigations**</span></span>

<span data-ttu-id="abd2d-474">L'azione più appropriata quando si verifica questo errore consiste nell'individuare la causa radice e nell'interrompere la creazione di numerosi provider di servizi interni.</span><span class="sxs-lookup"><span data-stu-id="abd2d-474">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="abd2d-475">È possibile tuttavia convertire nuovamente l'errore in avviso o ignorarlo tramite una configurazione in `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-475">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="abd2d-476">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="abd2d-476">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="abd2d-477">Nuovo comportamento per la chiamata di HasOne/HasMany con una singola stringa</span><span class="sxs-lookup"><span data-stu-id="abd2d-477">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="abd2d-478">Problema n. 9171</span><span class="sxs-lookup"><span data-stu-id="abd2d-478">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="abd2d-479">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-479">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-480">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-480">**Old behavior**</span></span>

<span data-ttu-id="abd2d-481">Prima di EF Core 3.0, il codice che chiama `HasOne` o `HasMany` con una singola stringa era interpretato in modo poco chiaro.</span><span class="sxs-lookup"><span data-stu-id="abd2d-481">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="abd2d-482">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="abd2d-482">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="abd2d-483">Apparentemente, il codice mette in relazione `Samurai` con un altro tipo di entità tramite la proprietà di navigazione `Entrance`, che può essere privata.</span><span class="sxs-lookup"><span data-stu-id="abd2d-483">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="abd2d-484">In realtà, il codice tenta di creare una relazione con un tipo di entità denominato `Entrance` senza proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-484">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="abd2d-485">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-485">**New behavior**</span></span>

<span data-ttu-id="abd2d-486">A partire da EF Core 3.0, il codice sopra riportato ora esegue quello che avrebbe dovuto fare in precedenza.</span><span class="sxs-lookup"><span data-stu-id="abd2d-486">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="abd2d-487">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-487">**Why**</span></span>

<span data-ttu-id="abd2d-488">Il comportamento precedente era molto poco chiaro, soprattutto durante la lettura del codice di configurazione e la ricerca di errori.</span><span class="sxs-lookup"><span data-stu-id="abd2d-488">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="abd2d-489">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-489">**Mitigations**</span></span>

<span data-ttu-id="abd2d-490">Questa modifica causerà problemi solo nelle applicazioni che configurano relazioni in modo esplicito usando stringhe per i nomi dei tipi e senza specificare in modo esplicito la proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-490">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="abd2d-491">Non è uno scenario comune.</span><span class="sxs-lookup"><span data-stu-id="abd2d-491">This is not common.</span></span>
<span data-ttu-id="abd2d-492">Il comportamento precedente può essere ottenuto passando esplicitamente `null` per il nome della proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-492">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="abd2d-493">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="abd2d-493">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="abd2d-494">Il tipo restituito per diversi metodi asincroni è cambiato da Task a ValueTask</span><span class="sxs-lookup"><span data-stu-id="abd2d-494">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="abd2d-495">Problema n. 15184</span><span class="sxs-lookup"><span data-stu-id="abd2d-495">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="abd2d-496">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-496">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-497">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-497">**Old behavior**</span></span>

<span data-ttu-id="abd2d-498">I metodi asincroni seguenti in precedenza restituivano il tipo `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="abd2d-498">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="abd2d-499">`ValueGenerator.NextValueAsync()` (e classi derivate)</span><span class="sxs-lookup"><span data-stu-id="abd2d-499">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="abd2d-500">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-500">**New behavior**</span></span>

<span data-ttu-id="abd2d-501">I metodi indicati in precedenza ora restituiscono il tipo `ValueTask<T>` sullo stesso `T` come in precedenza.</span><span class="sxs-lookup"><span data-stu-id="abd2d-501">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="abd2d-502">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-502">**Why**</span></span>

<span data-ttu-id="abd2d-503">Questa modifica riduce il numero delle allocazioni di heap sostenute quando si richiamano questi metodi, con un miglioramento generale delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="abd2d-503">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="abd2d-504">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-504">**Mitigations**</span></span>

<span data-ttu-id="abd2d-505">Le applicazioni semplicemente in attesa delle API precedenti devono solo essere ricompilate e non sono richieste modifiche del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="abd2d-505">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="abd2d-506">Per scenari di utilizzo più complessi (ad esempio, il passaggio del tipo `Task` restituito a `Task.WhenAny()`) è richiesto in genere che il tipo `ValueTask<T>` restituito venga convertito in `Task<T>` chiamando `AsTask()` su di esso.</span><span class="sxs-lookup"><span data-stu-id="abd2d-506">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="abd2d-507">Si noti che in questo modo si annulla la riduzione delle allocazioni consentita da questa modifica.</span><span class="sxs-lookup"><span data-stu-id="abd2d-507">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="abd2d-508">L'annotazione Relational:TypeMapping è ora TypeMapping</span><span class="sxs-lookup"><span data-stu-id="abd2d-508">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="abd2d-509">Problema n. 9913</span><span class="sxs-lookup"><span data-stu-id="abd2d-509">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="abd2d-510">Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.</span><span class="sxs-lookup"><span data-stu-id="abd2d-510">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="abd2d-511">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-511">**Old behavior**</span></span>

<span data-ttu-id="abd2d-512">Il nome di annotazione delle annotazioni di mapping del tipo era "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="abd2d-512">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="abd2d-513">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-513">**New behavior**</span></span>

<span data-ttu-id="abd2d-514">Il nome di annotazione delle annotazioni di mapping del tipo è ora "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="abd2d-514">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="abd2d-515">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-515">**Why**</span></span>

<span data-ttu-id="abd2d-516">Il mapping dei tipi non viene più usato solo per i provider di database relazionali.</span><span class="sxs-lookup"><span data-stu-id="abd2d-516">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="abd2d-517">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-517">**Mitigations**</span></span>

<span data-ttu-id="abd2d-518">Ciò causa un'interruzione solo nelle applicazioni che accedono al mapping dei tipi direttamente come annotazione. Questa situazione non è comune.</span><span class="sxs-lookup"><span data-stu-id="abd2d-518">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="abd2d-519">L'azione più appropriata per risolvere il problema consiste nell'usare la superficie dell'API per accedere al mapping dei tipi anziché l'annotazione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-519">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="abd2d-520">ToTable in un tipo derivato genera un'eccezione</span><span class="sxs-lookup"><span data-stu-id="abd2d-520">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="abd2d-521">Problema n. 11811</span><span class="sxs-lookup"><span data-stu-id="abd2d-521">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="abd2d-522">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="abd2d-522">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="abd2d-523">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-523">**Old behavior**</span></span>

<span data-ttu-id="abd2d-524">Nelle versioni precedenti a EF Core 3.0, la chiamata a `ToTable()` in un tipo derivato veniva ignorata poiché soltanto la strategia di mapping dell'ereditarietà era una tabella per gerarchia dove la chiamata non era valida.</span><span class="sxs-lookup"><span data-stu-id="abd2d-524">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="abd2d-525">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-525">**New behavior**</span></span>

<span data-ttu-id="abd2d-526">A partire da EF Core 3.0 e in preparazione all'aggiunta del supporto per la tabella per tipo e per TPC in una versione successiva, la chiamata a `ToTable()` in un tipo derivato genera un'eccezione per evitare una modifica del mapping imprevista in futuro.</span><span class="sxs-lookup"><span data-stu-id="abd2d-526">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="abd2d-527">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-527">**Why**</span></span>

<span data-ttu-id="abd2d-528">Attualmente il mapping di un tipo derivato in una tabella diversa non è un'operazione valida.</span><span class="sxs-lookup"><span data-stu-id="abd2d-528">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="abd2d-529">Questa modifica consente di evitare interruzioni future quando l'operazione diventerà un'operazione valida.</span><span class="sxs-lookup"><span data-stu-id="abd2d-529">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="abd2d-530">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-530">**Mitigations**</span></span>

<span data-ttu-id="abd2d-531">Rimuovere qualsiasi tentativo di mapping di tipi derivati in altre tabelle.</span><span class="sxs-lookup"><span data-stu-id="abd2d-531">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="abd2d-532">ForSqlServerHasIndex sostituito con HasIndex</span><span class="sxs-lookup"><span data-stu-id="abd2d-532">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="abd2d-533">Problema n. 12366</span><span class="sxs-lookup"><span data-stu-id="abd2d-533">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="abd2d-534">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="abd2d-534">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="abd2d-535">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-535">**Old behavior**</span></span>

<span data-ttu-id="abd2d-536">Nelle versioni precedenti a EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offriva un metodo per configurare le colonne usate con `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-536">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="abd2d-537">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-537">**New behavior**</span></span>

<span data-ttu-id="abd2d-538">A partire da EF Core 3.0, l'uso di `Include` in un indice è supportato a livello relazionale.</span><span class="sxs-lookup"><span data-stu-id="abd2d-538">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="abd2d-539">Usare `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-539">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="abd2d-540">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-540">**Why**</span></span>

<span data-ttu-id="abd2d-541">Questa modifica è stata apportata per consolidare l'API per gli indici con `Include` in un'unica posizione per tutti i provider di database.</span><span class="sxs-lookup"><span data-stu-id="abd2d-541">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="abd2d-542">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-542">**Mitigations**</span></span>

<span data-ttu-id="abd2d-543">Usare la nuova API, come illustrato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="abd2d-543">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="abd2d-544">Modifiche dell'API dei metadati</span><span class="sxs-lookup"><span data-stu-id="abd2d-544">Metadata API changes</span></span>

[<span data-ttu-id="abd2d-545">Problema n. 214</span><span class="sxs-lookup"><span data-stu-id="abd2d-545">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="abd2d-546">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-546">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-547">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-547">**New behavior**</span></span>

<span data-ttu-id="abd2d-548">Le proprietà seguenti sono state convertite in metodi di estensione:</span><span class="sxs-lookup"><span data-stu-id="abd2d-548">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="abd2d-549">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-549">**Why**</span></span>

<span data-ttu-id="abd2d-550">Questa modifica semplifica l'implementazione delle interfacce menzionate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="abd2d-550">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="abd2d-551">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-551">**Mitigations**</span></span>

<span data-ttu-id="abd2d-552">Usare i nuovi metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-552">Use the new extension methods.</span></span>

## <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="abd2d-553">Modifiche dell'API dei metadati specifiche del provider</span><span class="sxs-lookup"><span data-stu-id="abd2d-553">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="abd2d-554">Problema n. 214</span><span class="sxs-lookup"><span data-stu-id="abd2d-554">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="abd2d-555">Questa modifica è stata introdotta in EF Core 3.0 anteprima 6.</span><span class="sxs-lookup"><span data-stu-id="abd2d-555">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="abd2d-556">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-556">**New behavior**</span></span>

<span data-ttu-id="abd2d-557">I metodi di estensione specifici del provider verranno resi flat:</span><span class="sxs-lookup"><span data-stu-id="abd2d-557">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="abd2d-558">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-558">**Why**</span></span>

<span data-ttu-id="abd2d-559">Questa modifica semplifica l'implementazione dei metodi di estensione menzionati in precedenza.</span><span class="sxs-lookup"><span data-stu-id="abd2d-559">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="abd2d-560">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-560">**Mitigations**</span></span>

<span data-ttu-id="abd2d-561">Usare i nuovi metodi di estensione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-561">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="abd2d-562">EF Core non invia più pragma per l'imposizione della chiave esterna di SQLite</span><span class="sxs-lookup"><span data-stu-id="abd2d-562">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="abd2d-563">Problema n. 12151</span><span class="sxs-lookup"><span data-stu-id="abd2d-563">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="abd2d-564">Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.</span><span class="sxs-lookup"><span data-stu-id="abd2d-564">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="abd2d-565">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-565">**Old behavior**</span></span>

<span data-ttu-id="abd2d-566">Nelle versioni precedenti a EF Core 3.0, EF Core inviava `PRAGMA foreign_keys = 1` quando veniva aperta una connessione a SQLite.</span><span class="sxs-lookup"><span data-stu-id="abd2d-566">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="abd2d-567">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-567">**New behavior**</span></span>

<span data-ttu-id="abd2d-568">A partire da EF Core 3.0, EF Core non invia più `PRAGMA foreign_keys = 1` quando viene aperta una connessione a SQLite.</span><span class="sxs-lookup"><span data-stu-id="abd2d-568">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="abd2d-569">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-569">**Why**</span></span>

<span data-ttu-id="abd2d-570">Questa modifica è stata apportata poiché EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` per impostazione predefinita. Ciò significa che l'imposizione della chiave esterna è abilitata per impostazione predefinita e non deve essere abilitata in modo esplicito ogni volta che viene aperta una connessione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-570">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="abd2d-571">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-571">**Mitigations**</span></span>

<span data-ttu-id="abd2d-572">Le chiavi esterne sono abilitate per impostazione predefinita in SQLitePCLRaw.bundle_e_sqlite3, usato per impostazione predefinita per EF Core.</span><span class="sxs-lookup"><span data-stu-id="abd2d-572">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="abd2d-573">Per gli altri casi, è possibile abilitare le chiavi esterne specificando `Foreign Keys=True` nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-573">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="abd2d-574">Microsoft.EntityFrameworkCore.Sqlite dipende ora da SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="abd2d-574">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="abd2d-575">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-575">**Old behavior**</span></span>

<span data-ttu-id="abd2d-576">Nelle versioni precedenti a EF Core 3.0, EF Core usava `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-576">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="abd2d-577">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-577">**New behavior**</span></span>

<span data-ttu-id="abd2d-578">A partire da EF Core 3.0, EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-578">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="abd2d-579">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-579">**Why**</span></span>

<span data-ttu-id="abd2d-580">Questa modifica è stata apportata per rendere coerente la versione di SQLite usata in iOS con le altre piattaforme.</span><span class="sxs-lookup"><span data-stu-id="abd2d-580">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="abd2d-581">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-581">**Mitigations**</span></span>

<span data-ttu-id="abd2d-582">Per usare la versione di SQLite nativa in iOS, configurare `Microsoft.Data.Sqlite` per l'uso di un'aggregazione `SQLitePCLRaw` diversa.</span><span class="sxs-lookup"><span data-stu-id="abd2d-582">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="abd2d-583">I valori Guid vengono ora archiviati come TEXT in SQLite</span><span class="sxs-lookup"><span data-stu-id="abd2d-583">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="abd2d-584">Problema n. 15078</span><span class="sxs-lookup"><span data-stu-id="abd2d-584">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="abd2d-585">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-585">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-586">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-586">**Old behavior**</span></span>

<span data-ttu-id="abd2d-587">I valori Guid in precedenza venivano archiviati come valori BLOB in SQLite.</span><span class="sxs-lookup"><span data-stu-id="abd2d-587">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="abd2d-588">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-588">**New behavior**</span></span>

<span data-ttu-id="abd2d-589">I valori Guid vengono ora archiviati come TEXT.</span><span class="sxs-lookup"><span data-stu-id="abd2d-589">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="abd2d-590">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-590">**Why**</span></span>

<span data-ttu-id="abd2d-591">Il formato binario dei valori Guid non è standardizzato.</span><span class="sxs-lookup"><span data-stu-id="abd2d-591">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="abd2d-592">L'archiviazione dei valori come TEXT rende il database più compatibile con altre tecnologie.</span><span class="sxs-lookup"><span data-stu-id="abd2d-592">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="abd2d-593">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-593">**Mitigations**</span></span>

<span data-ttu-id="abd2d-594">È possibile eseguire la migrazione dei database esistenti al nuovo formato eseguendo SQL nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="abd2d-594">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="abd2d-595">In EF Core è anche possibile continuare a usare il comportamento precedente configurando un convertitore di valori per queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="abd2d-595">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="abd2d-596">Microsoft.Data.Sqlite rimane in grado di leggere i valori Guid sia da colonne BLOB che TEXT. Tuttavia, poiché è stato modificato il formato predefinito per i parametri e le costanti, probabilmente sarà necessario intervenire per la maggior parte degli scenari che coinvolgono valori Guid.</span><span class="sxs-lookup"><span data-stu-id="abd2d-596">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="abd2d-597">I valori char vengono ora archiviati come testo in SQLite</span><span class="sxs-lookup"><span data-stu-id="abd2d-597">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="abd2d-598">Problema n. 15020</span><span class="sxs-lookup"><span data-stu-id="abd2d-598">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="abd2d-599">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-599">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-600">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-600">**Old behavior**</span></span>

<span data-ttu-id="abd2d-601">I valori char in precedenza venivano archiviati come valori interi in SQLite.</span><span class="sxs-lookup"><span data-stu-id="abd2d-601">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="abd2d-602">Un valore char di *A* veniva ad esempio archiviato come valore intero 65.</span><span class="sxs-lookup"><span data-stu-id="abd2d-602">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="abd2d-603">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-603">**New behavior**</span></span>

<span data-ttu-id="abd2d-604">I valori char vengono ora archiviati come testo.</span><span class="sxs-lookup"><span data-stu-id="abd2d-604">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="abd2d-605">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-605">**Why**</span></span>

<span data-ttu-id="abd2d-606">L'archiviazione dei valori come testo è un'operazione più naturale e rende il database più compatibile con altre tecnologie.</span><span class="sxs-lookup"><span data-stu-id="abd2d-606">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="abd2d-607">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-607">**Mitigations**</span></span>

<span data-ttu-id="abd2d-608">È possibile eseguire la migrazione dei database esistenti al nuovo formato eseguendo SQL nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="abd2d-608">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="abd2d-609">In EF Core è anche possibile continuare a usare il comportamento precedente configurando un convertitore di valori per queste proprietà.</span><span class="sxs-lookup"><span data-stu-id="abd2d-609">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="abd2d-610">Microsoft.Data.Sqlite rimane comunque in grado di leggere i valori di caratteri presenti sia nelle colonne di valori interi sia in quelle di testo, quindi alcuni scenari potrebbero non richiedere alcuna azione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-610">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="abd2d-611">Gli ID di migrazione vengono ora generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica</span><span class="sxs-lookup"><span data-stu-id="abd2d-611">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="abd2d-612">Problema n. 12978</span><span class="sxs-lookup"><span data-stu-id="abd2d-612">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="abd2d-613">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-613">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-614">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-614">**Old behavior**</span></span>

<span data-ttu-id="abd2d-615">Gli ID di migrazione venivano inavvertitamente generati usando il calendario delle impostazioni cultura correnti.</span><span class="sxs-lookup"><span data-stu-id="abd2d-615">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="abd2d-616">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-616">**New behavior**</span></span>

<span data-ttu-id="abd2d-617">Gli ID di migrazione ora vengono sempre generati con il calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica (calendario gregoriano).</span><span class="sxs-lookup"><span data-stu-id="abd2d-617">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="abd2d-618">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-618">**Why**</span></span>

<span data-ttu-id="abd2d-619">L'ordine delle migrazioni è importante quando si esegue l'aggiornamento del database o si risolvono i conflitti di unione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-619">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="abd2d-620">L'uso del calendario delle impostazioni cultura inglese non dipendenti da paese/area geografica evita i problemi che possono verificarsi quando i membri del team hanno calendari di sistema diversi.</span><span class="sxs-lookup"><span data-stu-id="abd2d-620">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="abd2d-621">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-621">**Mitigations**</span></span>

<span data-ttu-id="abd2d-622">Questa modifica interessa gli utenti che usano un calendario non gregoriano in cui l'anno ha un'estensione superiore al calendario gregoriano (come il calendario buddista tailandese).</span><span class="sxs-lookup"><span data-stu-id="abd2d-622">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="abd2d-623">Gli ID di migrazione esistenti dovranno essere aggiornati in modo che le nuove migrazioni vengano collocate dopo le migrazioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="abd2d-623">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="abd2d-624">L'ID di migrazione è disponibile nell'attributo di migrazione presente nei file di progettazione delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="abd2d-624">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="abd2d-625">È necessario aggiornare anche la tabella della cronologia delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="abd2d-625">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="abd2d-626">Il metodo UseRowNumberForPaging è stato rimosso</span><span class="sxs-lookup"><span data-stu-id="abd2d-626">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="abd2d-627">Problema n. 16400</span><span class="sxs-lookup"><span data-stu-id="abd2d-627">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="abd2d-628">Questa modifica è stata introdotta in EF Core 3.0 anteprima 6.</span><span class="sxs-lookup"><span data-stu-id="abd2d-628">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="abd2d-629">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-629">**Old behavior**</span></span>

<span data-ttu-id="abd2d-630">Prima di EF Core 3.0 si poteva usare `UseRowNumberForPaging` per generare codice SQL per la suddivisione in pagine compatibile con SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="abd2d-630">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="abd2d-631">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-631">**New behavior**</span></span>

<span data-ttu-id="abd2d-632">A partire da EF Core 3.0, EF genererà solo codice SQL per la suddivisione in pagine compatibile solo con versioni successive di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="abd2d-632">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="abd2d-633">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-633">**Why**</span></span>

<span data-ttu-id="abd2d-634">Questa modifica è stata apportata perché [SQL Server 2008 non è più un prodotto supportato](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) e l'aggiornamento di questa funzionalità per interagire con le modifiche apportate per le query in EF Core 3.0 è un lavoro significativo.</span><span class="sxs-lookup"><span data-stu-id="abd2d-634">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="abd2d-635">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-635">**Mitigations**</span></span>

<span data-ttu-id="abd2d-636">È consigliabile eseguire l'aggiornamento a una versione più recente di SQL Server o usare un livello di compatibilità superiore, in modo che il codice SQL generato sia supportato.</span><span class="sxs-lookup"><span data-stu-id="abd2d-636">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="abd2d-637">Detto questo, se non è possibile procedere in questo modo, [aggiungere un commento per il problema](https://github.com/aspnet/EntityFrameworkCore/issues/16400) con indicazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="abd2d-637">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="abd2d-638">Microsoft potrebbe rivedere questa decisione in base ai commenti e suggerimenti.</span><span class="sxs-lookup"><span data-stu-id="abd2d-638">We may revisit this decision based on feedback.</span></span>

## <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="abd2d-639">Info/metadati dell'estensione rimossi da IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="abd2d-639">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="abd2d-640">Problema n. 16119</span><span class="sxs-lookup"><span data-stu-id="abd2d-640">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="abd2d-641">Questa modifica è stata introdotta in EF Core 3.0 anteprima 7.</span><span class="sxs-lookup"><span data-stu-id="abd2d-641">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="abd2d-642">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-642">**Old behavior**</span></span>

<span data-ttu-id="abd2d-643">`IDbContextOptionsExtension` conteneva metodi per fornire i metadati relativi all'estensione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-643">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="abd2d-644">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-644">**New behavior**</span></span>

<span data-ttu-id="abd2d-645">Questi metodi sono stati spostati in una nuova classe di base astratta `DbContextOptionsExtensionInfo`, restituita da una nuova proprietà `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-645">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="abd2d-646">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-646">**Why**</span></span>

<span data-ttu-id="abd2d-647">Nelle versioni dalla 2.0 alla 3.0 è stato necessario aggiungere o modificare questi metodi più volte.</span><span class="sxs-lookup"><span data-stu-id="abd2d-647">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="abd2d-648">Suddividendoli in una nuova classe di base astratta sarà più facile apportare questo tipo di modifiche senza compromettere il funzionamento delle estensioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="abd2d-648">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="abd2d-649">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-649">**Mitigations**</span></span>

<span data-ttu-id="abd2d-650">Aggiornare le estensioni per seguire il nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="abd2d-650">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="abd2d-651">Sono disponibili esempi nelle numerose implementazioni di `IDbContextOptionsExtension` per diversi tipi di estensioni nel codice sorgente di EF Core.</span><span class="sxs-lookup"><span data-stu-id="abd2d-651">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="abd2d-652">LogQueryPossibleExceptionWithAggregateOperator è stato rinominato</span><span class="sxs-lookup"><span data-stu-id="abd2d-652">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="abd2d-653">Problema n. 10985</span><span class="sxs-lookup"><span data-stu-id="abd2d-653">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="abd2d-654">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-654">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-655">**Modifica**</span><span class="sxs-lookup"><span data-stu-id="abd2d-655">**Change**</span></span>

<span data-ttu-id="abd2d-656">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` è stato rinominato in `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="abd2d-656">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="abd2d-657">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-657">**Why**</span></span>

<span data-ttu-id="abd2d-658">Allineamento del nome di questo evento di avviso con tutti gli altri eventi di avviso.</span><span class="sxs-lookup"><span data-stu-id="abd2d-658">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="abd2d-659">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-659">**Mitigations**</span></span>

<span data-ttu-id="abd2d-660">Usare il nuovo nome.</span><span class="sxs-lookup"><span data-stu-id="abd2d-660">Use the new name.</span></span> <span data-ttu-id="abd2d-661">(Si noti che il numero di ID evento non è stato modificato.)</span><span class="sxs-lookup"><span data-stu-id="abd2d-661">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="abd2d-662">Chiarimenti per l'API per i nomi di vincolo di chiave esterna</span><span class="sxs-lookup"><span data-stu-id="abd2d-662">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="abd2d-663">Problema n. 10730</span><span class="sxs-lookup"><span data-stu-id="abd2d-663">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="abd2d-664">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-664">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-665">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-665">**Old behavior**</span></span>

<span data-ttu-id="abd2d-666">Prima di EF Core 3.0, si faceva riferimento ai nomi di vincolo di chiave esterna semplicemente con "Name".</span><span class="sxs-lookup"><span data-stu-id="abd2d-666">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="abd2d-667">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="abd2d-667">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="abd2d-668">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-668">**New behavior**</span></span>

<span data-ttu-id="abd2d-669">A partire da EF Core 3.0, si fa ora riferimento ai nomi di vincolo di chiave esterna con "ConstraintName".</span><span class="sxs-lookup"><span data-stu-id="abd2d-669">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="abd2d-670">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="abd2d-670">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="abd2d-671">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-671">**Why**</span></span>

<span data-ttu-id="abd2d-672">Questa modifica introduce coerenza per la denominazione in quest'area e chiarisce anche che si tratta del nome del vincolo di chiave esterna e non del nome della colonna o della proprietà per cui è definita la chiave esterna.</span><span class="sxs-lookup"><span data-stu-id="abd2d-672">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="abd2d-673">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-673">**Mitigations**</span></span>

<span data-ttu-id="abd2d-674">Usare il nuovo nome.</span><span class="sxs-lookup"><span data-stu-id="abd2d-674">Use the new name.</span></span>

## <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="abd2d-675">IRelationalDatabaseCreator.HasTables/HasTablesAsync sono diventati pubblici</span><span class="sxs-lookup"><span data-stu-id="abd2d-675">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="abd2d-676">Problema n. 15997</span><span class="sxs-lookup"><span data-stu-id="abd2d-676">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="abd2d-677">Questa modifica è stata introdotta in EF Core 3.0 anteprima 7.</span><span class="sxs-lookup"><span data-stu-id="abd2d-677">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="abd2d-678">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-678">**Old behavior**</span></span>

<span data-ttu-id="abd2d-679">Prima di EF Core 3.0 questi metodi erano protetti.</span><span class="sxs-lookup"><span data-stu-id="abd2d-679">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="abd2d-680">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-680">**New behavior**</span></span>

<span data-ttu-id="abd2d-681">A partire da EF Core 3.0 questi metodi sono pubblici.</span><span class="sxs-lookup"><span data-stu-id="abd2d-681">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="abd2d-682">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-682">**Why**</span></span>

<span data-ttu-id="abd2d-683">Questi metodi vengono usati da EF per determinare se un database viene creato, ma vuoto.</span><span class="sxs-lookup"><span data-stu-id="abd2d-683">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="abd2d-684">Ciò risulta utile all'esterno di EF quando occorre determinare se applicare o meno le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="abd2d-684">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="abd2d-685">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-685">**Mitigations**</span></span>

<span data-ttu-id="abd2d-686">Modificare l'accessibilità di eventuali override.</span><span class="sxs-lookup"><span data-stu-id="abd2d-686">Change the accessibility of any overrides.</span></span>

## <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="abd2d-687">Microsoft.EntityFrameworkCore.Design è ora un pacchetto DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="abd2d-687">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="abd2d-688">Problema n. 11506</span><span class="sxs-lookup"><span data-stu-id="abd2d-688">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="abd2d-689">Questa modifica è stata introdotta in EF Core 3.0 anteprima 4.</span><span class="sxs-lookup"><span data-stu-id="abd2d-689">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="abd2d-690">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-690">**Old behavior**</span></span>

<span data-ttu-id="abd2d-691">Prima di EF Core 3.0, Microsoft.EntityFrameworkCore.Design era un pacchetto NuGet normale ed era possibile fare riferimento al relativo assembly dai progetti dipendenti.</span><span class="sxs-lookup"><span data-stu-id="abd2d-691">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="abd2d-692">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-692">**New behavior**</span></span>

<span data-ttu-id="abd2d-693">A partire da EF Core 3.0 è un pacchetto DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="abd2d-693">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="abd2d-694">Questo significa che la dipendenza non verrà trasferita in modo transitivo in altri progetti e che non è più possibile fare riferimento all'assembly per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="abd2d-694">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="abd2d-695">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-695">**Why**</span></span>

<span data-ttu-id="abd2d-696">Questo pacchetto è destinato solo all'uso in fase di progettazione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-696">This package is only intended to be used at design time.</span></span> <span data-ttu-id="abd2d-697">Le applicazioni distribuite non devono farvi riferimento.</span><span class="sxs-lookup"><span data-stu-id="abd2d-697">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="abd2d-698">Questa raccomandazione è rafforzata dall'impostazione del pacchetto come DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="abd2d-698">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="abd2d-699">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-699">**Mitigations**</span></span>

<span data-ttu-id="abd2d-700">Se è necessario fare riferimento a questo pacchetto per eseguire l'override del comportamento in fase di progettazione di EF Core, è possibile aggiornare i metadati dell'elemento PackageReference nel progetto.</span><span class="sxs-lookup"><span data-stu-id="abd2d-700">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="abd2d-701">In presenza di riferimenti transitivi al pacchetto tramite Microsoft.EntityFrameworkCore.Tools, sarà necessario aggiungere un PackageReference esplicito al pacchetto per modificare i relativi metadati.</span><span class="sxs-lookup"><span data-stu-id="abd2d-701">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

## <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="abd2d-702">Aggiornamento di SQLitePCL.raw alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="abd2d-702">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="abd2d-703">Problema n. 14824</span><span class="sxs-lookup"><span data-stu-id="abd2d-703">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="abd2d-704">Questa modifica è stata introdotta in EF Core 3.0 anteprima 7.</span><span class="sxs-lookup"><span data-stu-id="abd2d-704">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="abd2d-705">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-705">**Old behavior**</span></span>

<span data-ttu-id="abd2d-706">Microsoft.EntityFrameworkCore.Sqlite dipendeva in precedenza dalla versione 1.1.12 di SQLitePCL.raw.</span><span class="sxs-lookup"><span data-stu-id="abd2d-706">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="abd2d-707">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-707">**New behavior**</span></span>

<span data-ttu-id="abd2d-708">Il pacchetto è stato aggiornato in modo da dipendere dalla versione 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="abd2d-708">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="abd2d-709">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-709">**Why**</span></span>

<span data-ttu-id="abd2d-710">La versione 2.0.0 di SQLitePCL.raw è destinata a .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="abd2d-710">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="abd2d-711">Era in precedenza destinata a .NET Standard 1.1 e ciò richiedeva un notevole impegno di chiusura di pacchetti transitivi per il funzionamento.</span><span class="sxs-lookup"><span data-stu-id="abd2d-711">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="abd2d-712">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-712">**Mitigations**</span></span>

<span data-ttu-id="abd2d-713">SQLitePCL.raw versione 2.0.0 include alcune modifiche che causano un'interruzione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-713">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="abd2d-714">Per informazioni dettagliate, vedere le [note sulla versione](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="abd2d-714">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>


## <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="abd2d-715">NetTopologySuite aggiornato alla versione 2.0.0</span><span class="sxs-lookup"><span data-stu-id="abd2d-715">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="abd2d-716">Problema n. 14825</span><span class="sxs-lookup"><span data-stu-id="abd2d-716">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="abd2d-717">Questa modifica è stata introdotta in EF Core 3.0 anteprima 7.</span><span class="sxs-lookup"><span data-stu-id="abd2d-717">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="abd2d-718">**Comportamento precedente**</span><span class="sxs-lookup"><span data-stu-id="abd2d-718">**Old behavior**</span></span>

<span data-ttu-id="abd2d-719">I pacchetti spaziali dipendevano in precedenza dalla versione 1.15.1 di NetTopologySuite.</span><span class="sxs-lookup"><span data-stu-id="abd2d-719">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="abd2d-720">**Nuovo comportamento**</span><span class="sxs-lookup"><span data-stu-id="abd2d-720">**New behavior**</span></span>

<span data-ttu-id="abd2d-721">Il pacchetto è stato aggiornato in modo da dipendere dalla versione 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="abd2d-721">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="abd2d-722">**Perché?**</span><span class="sxs-lookup"><span data-stu-id="abd2d-722">**Why**</span></span>

<span data-ttu-id="abd2d-723">La versione 2.0.0 di NetTopologySuite risolve vari problemi di usabilità riscontrati dagli utenti di EF Core.</span><span class="sxs-lookup"><span data-stu-id="abd2d-723">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="abd2d-724">**Mitigazioni**</span><span class="sxs-lookup"><span data-stu-id="abd2d-724">**Mitigations**</span></span>

<span data-ttu-id="abd2d-725">NetTopologySuite versione 2.0.0 include alcune modifiche che causano un'interruzione.</span><span class="sxs-lookup"><span data-stu-id="abd2d-725">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="abd2d-726">Per informazioni dettagliate, vedere le [note sulla versione](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="abd2d-726">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>
