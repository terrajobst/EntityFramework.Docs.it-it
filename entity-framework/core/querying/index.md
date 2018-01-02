---
title: Esecuzione di query su dati - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: a2dd830b25c64b007a881c105a87b5c631b00266
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="querying-data"></a><span data-ttu-id="4d2a0-102">Esecuzione di query su dati</span><span class="sxs-lookup"><span data-stu-id="4d2a0-102">Querying Data</span></span>

<span data-ttu-id="4d2a0-103">Entity Framework Core usa query LINQ (Language Integrated Query) per eseguire query sui dati dal database.</span><span class="sxs-lookup"><span data-stu-id="4d2a0-103">Entity Framework Core uses Language Integrate Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="4d2a0-104">Le query LINQ consentono di usare C#, o il linguaggio .NET che si preferisce, per generare query fortemente tipizzate in base alle classi di contesto ed entità derivate.</span><span class="sxs-lookup"><span data-stu-id="4d2a0-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="4d2a0-105">Una rappresentazione della query LINQ viene passata al provider di database, per essere convertita nel linguaggio di query specifico del database, come ad esempio SQL per un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="4d2a0-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (e.g. SQL for a relational database).</span></span> <span data-ttu-id="4d2a0-106">Per altre informazioni sulla modalità di elaborazione di una query, vedere [How Queries Work](overview.md) (Funzionamento delle query).</span><span class="sxs-lookup"><span data-stu-id="4d2a0-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
