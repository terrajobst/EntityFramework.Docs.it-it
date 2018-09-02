---
title: Query asincrone - EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: de00e25279e29355a4eb3e55597a8578ceccecb6
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993565"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="0e735-102">Query asincrone</span><span class="sxs-lookup"><span data-stu-id="0e735-102">Asynchronous Queries</span></span>

<span data-ttu-id="0e735-103">Le query asincrone consentono di evitare il blocco di un thread mentre viene eseguita la query nel database.</span><span class="sxs-lookup"><span data-stu-id="0e735-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="0e735-104">Questo può essere utile per evitare di bloccare l'interfaccia utente di un'applicazione thick-client.</span><span class="sxs-lookup"><span data-stu-id="0e735-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="0e735-105">Le operazioni asincrone possono anche aumentare la velocità effettiva in un'applicazione Web, in cui il thread può essere liberato per gestire altre richieste mentre viene completata l'operazione di database.</span><span class="sxs-lookup"><span data-stu-id="0e735-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="0e735-106">Per altre informazioni, vedere [Programmazione asincrona in C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="0e735-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="0e735-107">EF Core non supporta più operazioni parallele in esecuzione nella stessa istanza di contesto.</span><span class="sxs-lookup"><span data-stu-id="0e735-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="0e735-108">È sempre necessario attendere il completamento di un'operazione prima di iniziare l'operazione successiva.</span><span class="sxs-lookup"><span data-stu-id="0e735-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="0e735-109">Questa operazione viene in genere eseguita tramite la parola chiave `await` per ogni operazione asincrona.</span><span class="sxs-lookup"><span data-stu-id="0e735-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="0e735-110">Entity Framework Core fornisce un set di metodi di estensione asincroni che possono essere usati in alternativa ai metodi LINQ che causano l'esecuzione di una query e la restituzione di risultati.</span><span class="sxs-lookup"><span data-stu-id="0e735-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="0e735-111">Alcuni esempi sono `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()` e così via. Non esistono versioni asincrone di operatori LINQ come `Where(...)`, `OrderBy(...)` e così via, perché questi metodi consentono solo di comporre l'albero delle espressioni LINQ e non causano l'esecuzione della query nel database.</span><span class="sxs-lookup"><span data-stu-id="0e735-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="0e735-112">I metodi di estensione asincroni di EF Core sono definiti nello spazio dei nomi `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="0e735-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="0e735-113">Questo spazio dei nomi deve essere importato affinché i metodi siano disponibili.</span><span class="sxs-lookup"><span data-stu-id="0e735-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
