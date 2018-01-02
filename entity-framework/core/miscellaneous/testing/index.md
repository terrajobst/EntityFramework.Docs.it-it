---
title: Testare i componenti tramite Entity Framework - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/index
ms.openlocfilehash: c82c25da393c39cf5e2deb46c7322e7395051937
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="testing"></a><span data-ttu-id="a8422-102">Test</span><span class="sxs-lookup"><span data-stu-id="a8422-102">Testing</span></span>

<span data-ttu-id="a8422-103">È consigliabile testare i componenti tramite un elemento che si avvicina alla connessione al database reale senza il sovraccarico delle operazioni I/O del database effettivo.</span><span class="sxs-lookup"><span data-stu-id="a8422-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="a8422-104">A tale scopo sono disponibili due opzioni principali:</span><span class="sxs-lookup"><span data-stu-id="a8422-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="a8422-105">La [modalità InMemory SQLite](sqlite.md) consente di scrivere test efficienti in base a un provider che si comporta come un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="a8422-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="a8422-106">Il [provider InMemory](in-memory.md) è un provider semplice con dipendenze minime, ma non sempre si comporta come un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="a8422-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
