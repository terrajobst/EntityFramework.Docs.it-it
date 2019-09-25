---
title: Testare i componenti usando EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: a946387718546f14e1485b4093e6c8046188f62d
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197500"
---
# <a name="testing-components-using-ef-core"></a><span data-ttu-id="27b73-102">Test dei componenti usando EF Core</span><span class="sxs-lookup"><span data-stu-id="27b73-102">Testing components using EF Core</span></span>

<span data-ttu-id="27b73-103">È consigliabile testare i componenti tramite un elemento che si avvicina alla connessione al database reale senza il sovraccarico delle operazioni I/O del database effettivo.</span><span class="sxs-lookup"><span data-stu-id="27b73-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="27b73-104">A tale scopo sono disponibili due opzioni principali:</span><span class="sxs-lookup"><span data-stu-id="27b73-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="27b73-105">La [modalità InMemory SQLite](sqlite.md) consente di scrivere test efficienti in base a un provider che si comporta come un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="27b73-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="27b73-106">Il [provider InMemory](in-memory.md) è un provider semplice con dipendenze minime, ma non sempre si comporta come un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="27b73-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
