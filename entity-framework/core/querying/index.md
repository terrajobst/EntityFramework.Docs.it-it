---
title: Esecuzione di query su dati - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 51aaa5de11d3fe38b4fba82db8dcb5658088cc27
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993534"
---
# <a name="querying-data"></a><span data-ttu-id="4f9d3-102">Esecuzione di query su dati</span><span class="sxs-lookup"><span data-stu-id="4f9d3-102">Querying Data</span></span>

<span data-ttu-id="4f9d3-103">Entity Framework Core usa query LINQ (Language Integrated Query) per eseguire query sui dati dal database.</span><span class="sxs-lookup"><span data-stu-id="4f9d3-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="4f9d3-104">Le query LINQ consentono di usare C#, o il linguaggio .NET che si preferisce, per generare query fortemente tipizzate in base alle classi di contesto ed entità derivate.</span><span class="sxs-lookup"><span data-stu-id="4f9d3-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="4f9d3-105">Una rappresentazione della query LINQ viene passata al provider di database, per essere convertita nel linguaggio di query specifico del database, come ad esempio SQL per un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="4f9d3-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (for example, SQL for a relational database).</span></span> <span data-ttu-id="4f9d3-106">Per altre informazioni sulla modalità di elaborazione di una query, vedere [How Queries Work](overview.md) (Funzionamento delle query).</span><span class="sxs-lookup"><span data-stu-id="4f9d3-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
