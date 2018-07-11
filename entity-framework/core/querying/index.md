---
title: Esecuzione di query su dati - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: 447f48b780bc48b7a79153d17dcc1b8ef0cc508c
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949275"
---
# <a name="querying-data"></a><span data-ttu-id="73b9c-102">Esecuzione di query su dati</span><span class="sxs-lookup"><span data-stu-id="73b9c-102">Querying Data</span></span>

<span data-ttu-id="73b9c-103">Entity Framework Core usa query LINQ (Language Integrated Query) per eseguire query sui dati dal database.</span><span class="sxs-lookup"><span data-stu-id="73b9c-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="73b9c-104">Le query LINQ consentono di usare C#, o il linguaggio .NET che si preferisce, per generare query fortemente tipizzate in base alle classi di contesto ed entità derivate.</span><span class="sxs-lookup"><span data-stu-id="73b9c-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="73b9c-105">Una rappresentazione della query LINQ viene passata al provider di database, per essere convertita nel linguaggio di query specifico del database, come ad esempio SQL per un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="73b9c-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (for example, SQL for a relational database).</span></span> <span data-ttu-id="73b9c-106">Per altre informazioni sulla modalità di elaborazione di una query, vedere [How Queries Work](overview.md) (Funzionamento delle query).</span><span class="sxs-lookup"><span data-stu-id="73b9c-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
