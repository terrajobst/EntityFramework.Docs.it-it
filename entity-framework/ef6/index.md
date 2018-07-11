---
title: Breve panoramica su Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
caps.latest.revision: 5
uid: ef6/index
ms.openlocfilehash: df661f19afdeef53257c86bdd32b1444737c9b0a
ms.sourcegitcommit: 45494121254ad4fdcec613d1dd22d850068d6f39
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "37913499"
---
# <a name="entity-framework-6-quick-overview"></a><span data-ttu-id="75f26-102">Breve panoramica su Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="75f26-102">Entity Framework 6 Quick Overview</span></span>

<span data-ttu-id="75f26-103">Entity Framework 6 (EF6) è un mapper relazionale a oggetti comprovato e testato per .NET le cui funzionalità vengono sviluppate e stabilizzate da anni.</span><span class="sxs-lookup"><span data-stu-id="75f26-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="75f26-104">EF6 implementa diverse funzionalità del mapping relazionale a oggetti molto comuni:</span><span class="sxs-lookup"><span data-stu-id="75f26-104">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="75f26-105">Mapping delle classi di entità non in grado di riconoscere la persistenza (note anche come "POCO", Plain Old CLR Object) che non dipendono da tipi EF</span><span class="sxs-lookup"><span data-stu-id="75f26-105">Mapping of "persistence ignorant" (also known as "POCO", for Plan-Old CLR Object) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="75f26-106">Rilevamento automatico delle modifiche</span><span class="sxs-lookup"><span data-stu-id="75f26-106">Automatic change tracking</span></span>
- <span data-ttu-id="75f26-107">Risoluzione di identità e unità di lavoro</span><span class="sxs-lookup"><span data-stu-id="75f26-107">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="75f26-108">Caricamento eager, lazy ed esplicito</span><span class="sxs-lookup"><span data-stu-id="75f26-108">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="75f26-109">Traduzione di query fortemente tipizzate con uso di LINQ (Language Integrated Query)</span><span class="sxs-lookup"><span data-stu-id="75f26-109">Translation of strongly-typed queries using LINQ (Language INtegrated Query)</span></span> 
- <span data-ttu-id="75f26-110">Funzionalità di mapping complete che includono il supporto per:</span><span class="sxs-lookup"><span data-stu-id="75f26-110">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="75f26-111">Ereditarietà (tabella per gerarchia, tabella per tipo e tabella per classe concreta)</span><span class="sxs-lookup"><span data-stu-id="75f26-111">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="75f26-112">Tipi complessi</span><span class="sxs-lookup"><span data-stu-id="75f26-112">Complex types</span></span>
  - <span data-ttu-id="75f26-113">Stored procedure</span><span class="sxs-lookup"><span data-stu-id="75f26-113">Stored procedures</span></span>
- <span data-ttu-id="75f26-114">Una finestra di progettazione per creare modelli di entità.</span><span class="sxs-lookup"><span data-stu-id="75f26-114">A visual designer to create entity models.</span></span>
- <span data-ttu-id="75f26-115">Un'esperienza Code First che supporta la creazione di modelli di entità mediante la scrittura di codice.</span><span class="sxs-lookup"><span data-stu-id="75f26-115">A "Code First" experience that supports creating entity models by writing code.</span></span>
- <span data-ttu-id="75f26-116">I modelli possono essere generati da database esistenti e poi modificati manualmente, oppure possono essere creati da zero e usati per generare nuovi database.</span><span class="sxs-lookup"><span data-stu-id="75f26-116">Models can either be generated form existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="75f26-117">Integrazione con i modelli di applicazione .NET Framework, tra cui ASP.NET e, tramite associazione dati, con WPF e WinForms.</span><span class="sxs-lookup"><span data-stu-id="75f26-117">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="75f26-118">Connettività del database basata su ADO.NET e numerosi provider disponibili per la connessione a SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 e così via.</span><span class="sxs-lookup"><span data-stu-id="75f26-118">Database connectivity based on ADO.NET and numerous providers available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

<span data-ttu-id="75f26-119">In quanto mapper relazionale a oggetti, EF6 riduce la mancata corrispondenza dell'impedenza tra i mondi relazionale e orientato agli oggetti, consentendo agli sviluppatori di scrivere applicazioni che interagiscono con i dati archiviati in database relazionali mediante oggetti .NET fortemente tipizzati che rappresentano il dominio dell'applicazione ed eliminando la necessità di gran parte del codice di accesso ai dati complesso che di solito devono scrivere.</span><span class="sxs-lookup"><span data-stu-id="75f26-119">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="75f26-120">È meglio usare EF6 o EF Core?</span><span class="sxs-lookup"><span data-stu-id="75f26-120">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="75f26-121">EF Core è una versione più moderna, leggera ed estendibile di Entity Framework che offre molte funzionalità e vantaggi simili a quelli di EF6.</span><span class="sxs-lookup"><span data-stu-id="75f26-121">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="75f26-122">EF Core è una versione completamente riscritta e contiene molte nuove funzionalità non disponibili in EF6, anche se non include ancora alcune delle funzionalità di mapping più avanzate di EF6.</span><span class="sxs-lookup"><span data-stu-id="75f26-122">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="75f26-123">È consigliabile usare EF Core nelle nuove applicazioni, purché il set di funzionalità soddisfi i propri requisiti.</span><span class="sxs-lookup"><span data-stu-id="75f26-123">We recommend using EF Core in new applications as long as the feature set matches your requirements.</span></span>
<span data-ttu-id="75f26-124">[Confronto tra EF Core ed EF6](xref:efcore-and-ef6/index) esamina questa scelta in modo più dettagliato.</span><span class="sxs-lookup"><span data-stu-id="75f26-124">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-startedef6get-startedmd"></a>[<span data-ttu-id="75f26-125">Introduzione</span><span class="sxs-lookup"><span data-stu-id="75f26-125">Get Started</span></span>](~/ef6/get-started.md)

<span data-ttu-id="75f26-126">Aggiungere al progetto il pacchetto NuGet EntityFramework o installare Entity Framework Tools per Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75f26-126">Add the EntityFramework NuGet package to your project or install the Entity Framework Tools for Visual Studio.</span></span> <span data-ttu-id="75f26-127">In seguito guardare i video e leggere le esercitazioni e la documentazione avanzata per sfruttare al meglio Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="75f26-127">Then watch videos, read tutorials, and advanced documentation to help you make the most of Entity Framework 6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="75f26-128">Versioni precedenti di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="75f26-128">Past Entity Framework Versions</span></span>

<span data-ttu-id="75f26-129">Questa è la documentazione per la versione più recente di Entity Framework 6, anche se la maggior parte di essa si applica anche alle versioni precedenti.</span><span class="sxs-lookup"><span data-stu-id="75f26-129">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="75f26-130">Vedere [Novità](~/ef6/what-is-new/index.md) e [Versioni precedenti](~/ef6/what-is-new/past-releases.md) per un elenco completo delle versioni di Entity Framework e delle funzionalità introdotte.</span><span class="sxs-lookup"><span data-stu-id="75f26-130">Check out [What's New](~/ef6/what-is-new/index.md) and [Past Releases](~/ef6/what-is-new/past-releases.md) for a complete list of EF releases and the features they introduced.</span></span>
