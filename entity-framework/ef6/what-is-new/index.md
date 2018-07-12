---
title: Novità - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
caps.latest.revision: 3
ms.openlocfilehash: dba6403fa341e1abfe8da488a19cf8520e3ea574
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914176"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="337f1-102">Novità di EF6</span><span class="sxs-lookup"><span data-stu-id="337f1-102">What's New in EF6</span></span>

<span data-ttu-id="337f1-103">Per ottenere le funzionalità più recenti e il livello di stabilità più elevato è consigliabile usare l'ultima versione rilasciata di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="337f1-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="337f1-104">È possibile, tuttavia, usare una versione precedente o provare i miglioramenti resi disponibili nella versione provvisoria più recente.</span><span class="sxs-lookup"><span data-stu-id="337f1-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="337f1-105">Per installare versioni specifiche di Entity Framework, vedere [Get Entity Framework](~/ef6/fundamentals/install.md) (Ottenere Entity Framework).</span><span class="sxs-lookup"><span data-stu-id="337f1-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

<span data-ttu-id="337f1-106">Questa pagina documenta le funzionalità incluse in ogni nuova versione.</span><span class="sxs-lookup"><span data-stu-id="337f1-106">This page documents the features that are included on each new release.</span></span>

## <a name="recent-releases"></a><span data-ttu-id="337f1-107">Versioni recenti</span><span class="sxs-lookup"><span data-stu-id="337f1-107">Recent releases</span></span>

### <a name="ef-tools-update-in-visual-studio-2017-157"></a><span data-ttu-id="337f1-108">Aggiornamento di Entity Framework Tools in Visual Studio 2017 15.7</span><span class="sxs-lookup"><span data-stu-id="337f1-108">EF Tools Update in Visual Studio 2017 15.7</span></span>

<span data-ttu-id="337f1-109">Nel mese di maggio 2018 è stata rilasciata una versione aggiornata di EF Tools come parte di Visual Studio 2017 15.7.</span><span class="sxs-lookup"><span data-stu-id="337f1-109">In May 2018, we released an updated version of the EF Tools as part of Visual Studio 2017 15.7.</span></span>
<span data-ttu-id="337f1-110">Include miglioramenti per alcune aree problematiche comuni:</span><span class="sxs-lookup"><span data-stu-id="337f1-110">It includes improvements for some common pain points:</span></span>

- <span data-ttu-id="337f1-111">Correzioni di vari bug di accessibilità dell'interfaccia utente</span><span class="sxs-lookup"><span data-stu-id="337f1-111">Fixes for several user interface accessibility bugs</span></span>
- <span data-ttu-id="337f1-112">Soluzione per la regressione delle prestazioni di SQL Server durante la generazione di modelli da database esistenti [n. 4](https://github.com/aspnet/entityframework6/issues/4)</span><span class="sxs-lookup"><span data-stu-id="337f1-112">Workaround for SQL Server performance regression when generating models from existing databases [#4](https://github.com/aspnet/entityframework6/issues/4)</span></span>
- <span data-ttu-id="337f1-113">Supporto dell'aggiornamento dei modelli per modelli di dimensioni superiori in SQL Server [n. 185](https://github.com/aspnet/EntityFramework6/issues/185)</span><span class="sxs-lookup"><span data-stu-id="337f1-113">Support for updating models for larger models on SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span></span>

<span data-ttu-id="337f1-114">Un altro miglioramento in questa nuova versione di EF Tools è l'installazione del runtime di EF 6.2 durante la creazione di un modello in un nuovo progetto.</span><span class="sxs-lookup"><span data-stu-id="337f1-114">Another improvement in this this new version of EF Tools is that it installs the EF 6.2 runtime when creating a model in a new project.</span></span> <span data-ttu-id="337f1-115">Con le versioni precedenti di Visual Studio è possibile usare il runtime di EF 6.2 (o altre versioni precedenti di EF) installando la versione corrispondente del pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="337f1-115">With older versions of Visual Studio, it is possible to use the EF 6.2 runtime (as well as any past version of EF) by installing the corresponding version of the NuGet package.</span></span>

### <a name="ef-62-runtime"></a><span data-ttu-id="337f1-116">Runtime di EF 6.2</span><span class="sxs-lookup"><span data-stu-id="337f1-116">EF 6.2 Runtime</span></span>

<span data-ttu-id="337f1-117">Il runtime di EF 6.2 è stato rilasciato in NuGet nel mese di ottobre 2017.</span><span class="sxs-lookup"><span data-stu-id="337f1-117">The EF 6.2 runtime was released to NuGet in October of 2017.</span></span>
<span data-ttu-id="337f1-118">Grazie principalmente al lavoro svolto dalla community di contributori open source, EF 6.2 include numerose [correzioni di bug](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) e [miglioramenti del prodotto](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span><span class="sxs-lookup"><span data-stu-id="337f1-118">Thanks in great part to the efforts our community of open source contributors, EF 6.2 includes numerous [bugs fixes](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) and [product enhancements](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span></span>

<span data-ttu-id="337f1-119">Di seguito viene riportato un breve elenco delle modifiche più importanti che interessano il runtime di EF 6.2:</span><span class="sxs-lookup"><span data-stu-id="337f1-119">Here is a brief list of the most important changes affecting the EF 6.2 runtime:</span></span>

- <span data-ttu-id="337f1-120">Riduzione del tempo di avvio grazie al caricamento di modelli Code First finiti da una cache persistente [n. 275](https://github.com/aspnet/EntityFramework6/issues/275)</span><span class="sxs-lookup"><span data-stu-id="337f1-120">Reduce start up time by loading finished code first models from a persistent cache [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span></span>
- <span data-ttu-id="337f1-121">API Fluent per definire gli indici [n. 274](https://github.com/aspnet/EntityFramework6/issues/274)</span><span class="sxs-lookup"><span data-stu-id="337f1-121">Fluent API to define indexes [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span></span>
- <span data-ttu-id="337f1-122">DbFunctions.Like() per abilitare la scrittura di query LINQ che vengono convertite in LIKE in SQL [n. 241](https://github.com/aspnet/EntityFramework6/issues/241)</span><span class="sxs-lookup"><span data-stu-id="337f1-122">DbFunctions.Like() to enable writing LINQ queries that translate to LIKE in SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span></span>
- <span data-ttu-id="337f1-123">Migrate.exe ora supporta l'opzione -script [n. 240](https://github.com/aspnet/EntityFramework6/issues/240)</span><span class="sxs-lookup"><span data-stu-id="337f1-123">Migrate.exe now supports -script option [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span></span>
- <span data-ttu-id="337f1-124">EF6 consente ora di usare i valori chiave generati da una sequenza in SQL Server [n. 165](https://github.com/aspnet/EntityFramework6/issues/165)</span><span class="sxs-lookup"><span data-stu-id="337f1-124">EF6 can now work with key values generated by a sequence in SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span></span>
- <span data-ttu-id="337f1-125">Aggiornamento dell'elenco di errori temporanei per la strategia di esecuzione SQL Azure [n. 83](https://github.com/aspnet/EntityFramework6/issues/83)</span><span class="sxs-lookup"><span data-stu-id="337f1-125">Update list of transient errors for SQL Azure Execution Strategy [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span></span>
- <span data-ttu-id="337f1-126">Bug: i nuovi tentativi di query o comandi SQL restituiscono l'errore "SqlParameter già contenuto in un altro elemento SqlParameterCollection" [n. 81](https://github.com/aspnet/EntityFramework6/issues/81)</span><span class="sxs-lookup"><span data-stu-id="337f1-126">Bug: Retrying queries or SQL commands fails with "The SqlParameter is already contained by another SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span></span>
- <span data-ttu-id="337f1-127">Bug: frequente timeout della valutazione di DbQuery.ToString() nel debugger [n. 73](https://github.com/aspnet/EntityFramework6/issues/73)</span><span class="sxs-lookup"><span data-stu-id="337f1-127">Bug: Evaluation of DbQuery.ToString() frequently times out in the debugger [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span></span>

## <a name="future-releases"></a><span data-ttu-id="337f1-128">Versioni future</span><span class="sxs-lookup"><span data-stu-id="337f1-128">Future Releases</span></span>

<span data-ttu-id="337f1-129">Per informazioni sulla versione futura di EF6, vedere la [Roadmap](roadmap.md).</span><span class="sxs-lookup"><span data-stu-id="337f1-129">For information on future version of EF6, please look at our [Roadmap](roadmap.md).</span></span>

## <a name="past-releases"></a><span data-ttu-id="337f1-130">Versioni precedenti</span><span class="sxs-lookup"><span data-stu-id="337f1-130">Past Releases</span></span>

<span data-ttu-id="337f1-131">La pagina [Versioni precedenti](past-releases.md) contiene un archivio di tutte le versioni precedenti di Entity Framework e delle funzionalità principali introdotte in ogni versione.</span><span class="sxs-lookup"><span data-stu-id="337f1-131">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
