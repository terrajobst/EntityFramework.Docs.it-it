---
title: Log delle modifiche che influiscono sul provider - EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: ee73940e3c0030b76e73438b1852cc29ebeadb45
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998350"
---
# <a name="provider-impacting-changes"></a><span data-ttu-id="7ce78-102">Modifiche che influiscono sul provider</span><span class="sxs-lookup"><span data-stu-id="7ce78-102">Provider-impacting changes</span></span>

<span data-ttu-id="7ce78-103">Questa pagina contiene collegamenti per eseguire il pull le richieste effettuate nel repository EF Core che possono richiedere gli autori di altri provider di database per rispondere.</span><span class="sxs-lookup"><span data-stu-id="7ce78-103">This page contains links to pull requests made on the EF Core repo that may require authors of other database providers to react.</span></span> <span data-ttu-id="7ce78-104">Lo scopo consiste nel fornire un punto di partenza per gli autori di provider di database di terze parti esistenti quando si aggiorna il provider a una nuova versione.</span><span class="sxs-lookup"><span data-stu-id="7ce78-104">The intention is to provide a starting point for authors of existing third-party database providers when updating their provider to a new version.</span></span>

<span data-ttu-id="7ce78-105">Questo log è iniziata con le modifiche da 2.1 a 2.2.</span><span class="sxs-lookup"><span data-stu-id="7ce78-105">We are starting this log with changes from 2.1 to 2.2.</span></span> <span data-ttu-id="7ce78-106">Prima di 2.1 è stato usato il [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) e [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) etichette nei nostri problemi e richieste pull.</span><span class="sxs-lookup"><span data-stu-id="7ce78-106">Prior to 2.1 we used the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) and [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) labels on our issues and pull requests.</span></span>

### <a name="21-----22"></a><span data-ttu-id="7ce78-107">2.1---> 2.2</span><span class="sxs-lookup"><span data-stu-id="7ce78-107">2.1 ---> 2.2</span></span>

#### <a name="test-only-changes"></a><span data-ttu-id="7ce78-108">Modifiche apportate a solo scopo di test</span><span class="sxs-lookup"><span data-stu-id="7ce78-108">Test-only changes</span></span>

* <span data-ttu-id="7ce78-109">https://github.com/aspnet/EntityFrameworkCore/pull/12057 -Consenti personalizzabile delimeters SQL nei test</span><span class="sxs-lookup"><span data-stu-id="7ce78-109">https://github.com/aspnet/EntityFrameworkCore/pull/12057 - Allow customizable SQL delimeters in tests</span></span>
  * <span data-ttu-id="7ce78-110">Testare le modifiche che consentono di BuiltInDataTypesTestBase non strict confronti completi dei punti a virgola mobile</span><span class="sxs-lookup"><span data-stu-id="7ce78-110">Test changes that allow non-strict floating point comparisons in BuiltInDataTypesTestBase</span></span>
  * <span data-ttu-id="7ce78-111">Modifiche di test che consentono i test di query essere riutilizzata con diversi delimeters SQL</span><span class="sxs-lookup"><span data-stu-id="7ce78-111">Test changes that allow query tests to be re-used with different SQL delimeters</span></span>
* <span data-ttu-id="7ce78-112">https://github.com/aspnet/EntityFrameworkCore/pull/12072 -Aggiungere test DbFunction ai test relazionale specifica</span><span class="sxs-lookup"><span data-stu-id="7ce78-112">https://github.com/aspnet/EntityFrameworkCore/pull/12072 - Add DbFunction tests to the relational specification tests</span></span>
  * <span data-ttu-id="7ce78-113">In modo che questi test possono essere eseguiti su tutti i provider di database</span><span class="sxs-lookup"><span data-stu-id="7ce78-113">Such that these tests can be run against all database providers</span></span>
* <span data-ttu-id="7ce78-114">https://github.com/aspnet/EntityFrameworkCore/pull/12362 -Pulizia del test Async</span><span class="sxs-lookup"><span data-stu-id="7ce78-114">https://github.com/aspnet/EntityFrameworkCore/pull/12362 - Async test cleanup</span></span>
  * <span data-ttu-id="7ce78-115">Rimuovere `Wait` chiamate, non necessari async e rinominati alcuni metodi di test</span><span class="sxs-lookup"><span data-stu-id="7ce78-115">Remove `Wait` calls, unneeded async, and renamed some test methods</span></span>
* <span data-ttu-id="7ce78-116">https://github.com/aspnet/EntityFrameworkCore/pull/12666 -Unificare l'infrastruttura di test di registrazione</span><span class="sxs-lookup"><span data-stu-id="7ce78-116">https://github.com/aspnet/EntityFrameworkCore/pull/12666 - Unify logging test infrastructure</span></span>
  * <span data-ttu-id="7ce78-117">Aggiunto `CreateListLoggerFactory` e rimossi alcuni precedente infrastruttura di registrazione, in modo da richiederla provider utilizzando questi test per rispondere</span><span class="sxs-lookup"><span data-stu-id="7ce78-117">Added `CreateListLoggerFactory` and removed some previous logging infrastructure, which will require providers using these tests to react</span></span>
* <span data-ttu-id="7ce78-118">https://github.com/aspnet/EntityFrameworkCore/pull/12500 -Eseguire altri test di query sia in modo sincrono e asincrono</span><span class="sxs-lookup"><span data-stu-id="7ce78-118">https://github.com/aspnet/EntityFrameworkCore/pull/12500 - Run more query tests both synchronously and asynchronously</span></span>
  * <span data-ttu-id="7ce78-119">I nomi di test e le stesse caratteristiche è stato modificato, che richiederà l'uso nei provider tramite questi test per rispondere</span><span class="sxs-lookup"><span data-stu-id="7ce78-119">Test names and factoring has changed, which will require providers using these tests to react</span></span>
* <span data-ttu-id="7ce78-120">https://github.com/aspnet/EntityFrameworkCore/pull/12766 -Spostamenti a ridenominazione del modello ComplexNavigations</span><span class="sxs-lookup"><span data-stu-id="7ce78-120">https://github.com/aspnet/EntityFrameworkCore/pull/12766 - Renaming navigations in the ComplexNavigations model</span></span>
  * <span data-ttu-id="7ce78-121">Provider di questi test potrebbe essere necessario rispondere</span><span class="sxs-lookup"><span data-stu-id="7ce78-121">Providers using these tests may need to react</span></span>
* <span data-ttu-id="7ce78-122">https://github.com/aspnet/EntityFrameworkCore/pull/12141 -Restituire il contesto per il pool anziché eliminare in test funzionali</span><span class="sxs-lookup"><span data-stu-id="7ce78-122">https://github.com/aspnet/EntityFrameworkCore/pull/12141 - Return the context to the pool instead of disposing in functional tests</span></span>
  * <span data-ttu-id="7ce78-123">Questa modifica include alcuni test di refactoring che può richiedere ai provider di react</span><span class="sxs-lookup"><span data-stu-id="7ce78-123">This change includes some test refactoring which may require providers to react</span></span>


#### <a name="test-and-product-code-changes"></a><span data-ttu-id="7ce78-124">Modifiche al codice di test e prodotto</span><span class="sxs-lookup"><span data-stu-id="7ce78-124">Test and product code changes</span></span>

* <span data-ttu-id="7ce78-125">https://github.com/aspnet/EntityFrameworkCore/pull/12109 -Consolidare RelationalTypeMapping.Clone metodi</span><span class="sxs-lookup"><span data-stu-id="7ce78-125">https://github.com/aspnet/EntityFrameworkCore/pull/12109 - Consolidate RelationalTypeMapping.Clone methods</span></span>
  * <span data-ttu-id="7ce78-126">Le modifiche in 2.1 per il RelationalTypeMapping consentite per semplificare il lavoro per le classi derivate.</span><span class="sxs-lookup"><span data-stu-id="7ce78-126">Changes in 2.1 to the RelationalTypeMapping allowed for a simplification in derived classes.</span></span> <span data-ttu-id="7ce78-127">Non si ritiene che questo era di rilievo ai provider, ma i provider possono sfruttare i vantaggi di questa modifica nel relativo tipo derivato mapping di classi.</span><span class="sxs-lookup"><span data-stu-id="7ce78-127">We don't believe this was breaking to providers, but providers can take advantage of this change in their derived type mapping classes.</span></span>
* <span data-ttu-id="7ce78-128">https://github.com/aspnet/EntityFrameworkCore/pull/12069 -Query denominate o tag</span><span class="sxs-lookup"><span data-stu-id="7ce78-128">https://github.com/aspnet/EntityFrameworkCore/pull/12069 - Tagged or named queries</span></span>
  * <span data-ttu-id="7ce78-129">Aggiunge l'infrastruttura per l'assegnazione di tag query LINQ e presenza di tali tag visualizzato come commenti in SQL.</span><span class="sxs-lookup"><span data-stu-id="7ce78-129">Adds infrastructure for tagging LINQ queries and having those tags show up as comments in the SQL.</span></span> <span data-ttu-id="7ce78-130">Ciò può richiedere ai provider di reagire in generazione SQL.</span><span class="sxs-lookup"><span data-stu-id="7ce78-130">This may require providers to react in SQL generation.</span></span>
