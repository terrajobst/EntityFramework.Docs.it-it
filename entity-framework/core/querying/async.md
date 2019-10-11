---
title: Query asincrone - EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181815"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="754af-102">Query asincrone</span><span class="sxs-lookup"><span data-stu-id="754af-102">Asynchronous Queries</span></span>

<span data-ttu-id="754af-103">Le query asincrone consentono di evitare il blocco di un thread mentre viene eseguita la query nel database.</span><span class="sxs-lookup"><span data-stu-id="754af-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="754af-104">Le query asincrone sono importanti per mantenere un'interfaccia utente reattiva nelle applicazioni client thick.</span><span class="sxs-lookup"><span data-stu-id="754af-104">Async queries are important for keeping a responsive UI in thick-client applications.</span></span> <span data-ttu-id="754af-105">Possono anche aumentare la velocità effettiva nelle applicazioni Web in cui liberano il thread per soddisfare altre richieste nelle applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="754af-105">They can also increase throughput in web applications where they free up the thread to service other requests in web applications.</span></span> <span data-ttu-id="754af-106">Per altre informazioni, vedere [Programmazione asincrona in C#](/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="754af-106">For more information, see [Asynchronous Programming in C#](/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="754af-107">EF Core non supporta l'esecuzione di più operazioni parallele nella stessa istanza del contesto.</span><span class="sxs-lookup"><span data-stu-id="754af-107">EF Core doesn't support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="754af-108">È sempre necessario attendere il completamento di un'operazione prima di iniziare l'operazione successiva.</span><span class="sxs-lookup"><span data-stu-id="754af-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="754af-109">Questa operazione viene in genere eseguita utilizzando la parola chiave `await` in ogni operazione asincrona.</span><span class="sxs-lookup"><span data-stu-id="754af-109">This is typically done by using the `await` keyword on each async operation.</span></span>

<span data-ttu-id="754af-110">Entity Framework Core fornisce un set di metodi di estensione asincroni simili ai metodi LINQ, che eseguono una query e restituiscono risultati.</span><span class="sxs-lookup"><span data-stu-id="754af-110">Entity Framework Core provides a set of async extension methods similar to the LINQ methods, which execute a query and return results.</span></span> <span data-ttu-id="754af-111">Gli esempi includono `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`.</span><span class="sxs-lookup"><span data-stu-id="754af-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`.</span></span> <span data-ttu-id="754af-112">Non esistono versioni asincrone di alcuni operatori LINQ, ad esempio `Where(...)` o `OrderBy(...)`, perché questi metodi compilano solo l'albero delle espressioni LINQ e non causano l'esecuzione della query nel database.</span><span class="sxs-lookup"><span data-stu-id="754af-112">There are no async versions of some LINQ operators such as `Where(...)` or `OrderBy(...)` because these methods only build up the LINQ expression tree and don't cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="754af-113">I metodi di estensione asincroni di EF Core sono definiti nello spazio dei nomi `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="754af-113">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="754af-114">Questo spazio dei nomi deve essere importato affinché i metodi siano disponibili.</span><span class="sxs-lookup"><span data-stu-id="754af-114">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
