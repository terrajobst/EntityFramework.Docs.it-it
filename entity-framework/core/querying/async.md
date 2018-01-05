---
title: Query asincrone - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-queries"></a><span data-ttu-id="0a0cd-102">Query asincrone</span><span class="sxs-lookup"><span data-stu-id="0a0cd-102">Asynchronous Queries</span></span>

<span data-ttu-id="0a0cd-103">Query asincrone evitare di bloccare un thread mentre viene eseguita la query nel database.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="0a0cd-104">Questo può essere utile per evitare di bloccare l'interfaccia utente di un'applicazione thick client.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="0a0cd-105">Operazioni asincrone è anche possono aumentare la velocità effettiva in un'applicazione web, in cui il thread può liberato al completamento dell'operazione di database di altre richieste di servizio.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="0a0cd-106">Per ulteriori informazioni, vedere [la programmazione asincrona in c#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="0a0cd-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="0a0cd-107">Core EF non supporta più operazioni parallele in esecuzione nella stessa istanza del contesto.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="0a0cd-108">È sempre necessario attendere per un'operazione da completare prima di iniziare l'operazione successiva.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="0a0cd-109">Questa operazione viene in genere eseguita tramite il `await` (parola chiave) in ogni operazione asincrona.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="0a0cd-110">Componenti di base di Entity Framework fornisce un set di metodi di estensione asincrona che può essere utilizzato come alternativa ai metodi LINQ che causano l'esecuzione di una query e risultati restituiti.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="0a0cd-111">Gli esempi includono `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`e così via. Non esistono versioni asincrone di operatori LINQ, ad esempio `Where(...)`, `OrderBy(...)`, e così via. perché questi metodi solo creare l'albero delle espressioni LINQ e non causano la query da eseguire nel database.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="0a0cd-112">I metodi di estensione async EF Core sono definiti nel `Microsoft.EntityFrameworkCore` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="0a0cd-113">Questo spazio dei nomi deve essere importato per i metodi siano disponibili.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]