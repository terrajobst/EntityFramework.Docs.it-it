---
title: Panoramica - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 35292d2a724d02ecdc96b53550858eb0b6dd79d1
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489232"
---
# <a name="entity-framework-6"></a><span data-ttu-id="08797-102">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="08797-102">Entity Framework 6</span></span>
<span data-ttu-id="08797-103">Entity Framework 6 (EF6) è un mapper relazionale a oggetti comprovato e testato per .NET le cui funzionalità vengono sviluppate e stabilizzate da anni.</span><span class="sxs-lookup"><span data-stu-id="08797-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="08797-104">In quanto mapper relazionale a oggetti, EF6 riduce la mancata corrispondenza dell'impedenza tra i mondi relazionale e orientato agli oggetti, consentendo agli sviluppatori di scrivere applicazioni che interagiscono con i dati archiviati in database relazionali mediante oggetti .NET fortemente tipizzati che rappresentano il dominio dell'applicazione ed eliminando la necessità di gran parte del codice di accesso ai dati complesso che di solito devono scrivere.</span><span class="sxs-lookup"><span data-stu-id="08797-104">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

<span data-ttu-id="08797-105">EF6 implementa diverse funzionalità del mapping relazionale a oggetti molto comuni:</span><span class="sxs-lookup"><span data-stu-id="08797-105">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="08797-106">Mapping delle classi di entità [POCO](~/ef6/resources/glossary.md#poco) che non dipendono da alcun tipo di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="08797-106">Mapping of [POCO](~/ef6/resources/glossary.md#poco) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="08797-107">Rilevamento automatico delle modifiche</span><span class="sxs-lookup"><span data-stu-id="08797-107">Automatic change tracking</span></span>
- <span data-ttu-id="08797-108">Risoluzione di identità e unità di lavoro</span><span class="sxs-lookup"><span data-stu-id="08797-108">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="08797-109">Caricamento eager, lazy ed esplicito</span><span class="sxs-lookup"><span data-stu-id="08797-109">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="08797-110">Traduzione di query fortemente tipizzate con uso di LINQ (Language Integrated Query)</span><span class="sxs-lookup"><span data-stu-id="08797-110">Translation of strongly-typed queries using LINQ (Language INtegrated Query)</span></span>
- <span data-ttu-id="08797-111">Funzionalità di mapping complete che includono il supporto per:</span><span class="sxs-lookup"><span data-stu-id="08797-111">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="08797-112">Relazioni uno-a-uno, uno-a-molti e molti-a-molti</span><span class="sxs-lookup"><span data-stu-id="08797-112">One-to-one, one-to-many and many-to-many relationships</span></span>
  - <span data-ttu-id="08797-113">Ereditarietà (tabella per gerarchia, tabella per tipo e tabella per classe concreta)</span><span class="sxs-lookup"><span data-stu-id="08797-113">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="08797-114">Tipi complessi</span><span class="sxs-lookup"><span data-stu-id="08797-114">Complex types</span></span>
  - <span data-ttu-id="08797-115">Stored procedure</span><span class="sxs-lookup"><span data-stu-id="08797-115">Stored procedures</span></span>
- <span data-ttu-id="08797-116">Una finestra di progettazione per creare modelli di entità.</span><span class="sxs-lookup"><span data-stu-id="08797-116">A visual designer to create entity models.</span></span>
- <span data-ttu-id="08797-117">Un'esperienza Code First per creare modelli di entità mediante la scrittura di codice.</span><span class="sxs-lookup"><span data-stu-id="08797-117">A "Code First" experience to create entity models by writing code.</span></span>
- <span data-ttu-id="08797-118">I modelli possono essere generati da database esistenti e poi modificati manualmente, oppure possono essere creati da zero e usati per generare nuovi database.</span><span class="sxs-lookup"><span data-stu-id="08797-118">Models can either be generated from existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="08797-119">Integrazione con i modelli di applicazione .NET Framework, tra cui ASP.NET e, tramite associazione dati, con WPF e WinForms.</span><span class="sxs-lookup"><span data-stu-id="08797-119">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="08797-120">Connettività del database basata su ADO.NET e numerosi provider disponibili per la connessione a SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 e così via.</span><span class="sxs-lookup"><span data-stu-id="08797-120">Database connectivity based on ADO.NET and numerous providers available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="08797-121">È meglio usare EF6 o EF Core?</span><span class="sxs-lookup"><span data-stu-id="08797-121">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="08797-122">EF Core è una versione più moderna, leggera ed estendibile di Entity Framework che offre molte funzionalità e vantaggi simili a quelli di EF6.</span><span class="sxs-lookup"><span data-stu-id="08797-122">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="08797-123">EF Core è una versione completamente riscritta e contiene molte nuove funzionalità non disponibili in EF6, anche se non include ancora alcune delle funzionalità di mapping più avanzate di EF6.</span><span class="sxs-lookup"><span data-stu-id="08797-123">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="08797-124">È consigliabile usare EF Core nelle nuove applicazioni, purché il set di funzionalità soddisfi i propri requisiti.</span><span class="sxs-lookup"><span data-stu-id="08797-124">We recommend using EF Core in new applications as long as the feature set matches your requirements.</span></span>
<span data-ttu-id="08797-125">[Confronto tra EF Core ed EF6](xref:efcore-and-ef6/index) esamina questa scelta in modo più dettagliato.</span><span class="sxs-lookup"><span data-stu-id="08797-125">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-startedef6get-startedmd"></a>[<span data-ttu-id="08797-126">Introduzione</span><span class="sxs-lookup"><span data-stu-id="08797-126">Get Started</span></span>](~/ef6/get-started.md)

<span data-ttu-id="08797-127">Aggiungere al progetto il pacchetto NuGet EntityFramework o installare Entity Framework Tools per Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="08797-127">Add the EntityFramework NuGet package to your project or install the Entity Framework Tools for Visual Studio.</span></span> <span data-ttu-id="08797-128">In seguito guardare i video e leggere le esercitazioni e la documentazione avanzata per sfruttare al meglio EF6.</span><span class="sxs-lookup"><span data-stu-id="08797-128">Then watch videos, read tutorials, and advanced documentation to help you make the most of EF6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="08797-129">Versioni precedenti di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="08797-129">Past Entity Framework Versions</span></span>

<span data-ttu-id="08797-130">Questa è la documentazione per la versione più recente di Entity Framework 6, anche se la maggior parte di essa si applica anche alle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="08797-130">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="08797-131">Vedere [Novità](~/ef6/what-is-new/index.md) e [Versioni precedenti](~/ef6/what-is-new/past-releases.md) per un elenco completo delle versioni di Entity Framework e delle funzionalità introdotte.</span><span class="sxs-lookup"><span data-stu-id="08797-131">Check out [What's New](~/ef6/what-is-new/index.md) and [Past Releases](~/ef6/what-is-new/past-releases.md) for a complete list of EF releases and the features they introduced.</span></span>
