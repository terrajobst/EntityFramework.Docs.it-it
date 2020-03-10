---
title: Testare i componenti usando EF Core - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 1ca900528ed42ad4b41016f22964c3494b0286eb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412796"
---
# <a name="testing-components-using-ef-core"></a><span data-ttu-id="3370f-102">Test dei componenti usando EF Core</span><span class="sxs-lookup"><span data-stu-id="3370f-102">Testing components using EF Core</span></span>

<span data-ttu-id="3370f-103">È consigliabile testare i componenti tramite un elemento che si avvicina alla connessione al database reale senza il sovraccarico delle operazioni I/O del database effettivo.</span><span class="sxs-lookup"><span data-stu-id="3370f-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="3370f-104">A tale scopo sono disponibili due opzioni principali:</span><span class="sxs-lookup"><span data-stu-id="3370f-104">There are two main options for doing this:</span></span>

* <span data-ttu-id="3370f-105">La [modalità InMemory SQLite](sqlite.md) consente di scrivere test efficienti in base a un provider che si comporta come un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="3370f-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
* <span data-ttu-id="3370f-106">Il [provider InMemory](in-memory.md) è un provider semplice con dipendenze minime, ma non sempre si comporta come un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="3370f-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
