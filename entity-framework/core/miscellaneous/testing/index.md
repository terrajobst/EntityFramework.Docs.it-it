---
title: Testare i componenti tramite Entity Framework - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: fc751b9053c337e4911f4016b65b370d1276046b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997837"
---
# <a name="testing"></a><span data-ttu-id="4d283-102">Test</span><span class="sxs-lookup"><span data-stu-id="4d283-102">Testing</span></span>

<span data-ttu-id="4d283-103">È consigliabile testare i componenti tramite un elemento che si avvicina alla connessione al database reale senza il sovraccarico delle operazioni I/O del database effettivo.</span><span class="sxs-lookup"><span data-stu-id="4d283-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="4d283-104">A tale scopo sono disponibili due opzioni principali:</span><span class="sxs-lookup"><span data-stu-id="4d283-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="4d283-105">La [modalità InMemory SQLite](sqlite.md) consente di scrivere test efficienti in base a un provider che si comporta come un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="4d283-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="4d283-106">Il [provider InMemory](in-memory.md) è un provider semplice con dipendenze minime, ma non sempre si comporta come un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="4d283-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
