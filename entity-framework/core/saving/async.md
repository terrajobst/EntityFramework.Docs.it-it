---
title: Salvataggio asincrono - Core EF
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
ms.technology: entity-framework-core
uid: core/saving/async
ms.openlocfilehash: 640e5f131b0e9ef4e5028d1dcaf80f3e5bbd9d9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-saving"></a><span data-ttu-id="e3467-102">Salvataggio asincrono</span><span class="sxs-lookup"><span data-stu-id="e3467-102">Asynchronous Saving</span></span>

<span data-ttu-id="e3467-103">Salvataggio asincrono si evita di bloccare un thread mentre le modifiche vengono scritte nel database.</span><span class="sxs-lookup"><span data-stu-id="e3467-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="e3467-104">Questo può essere utile per evitare di bloccare l'interfaccia utente di un'applicazione thick client.</span><span class="sxs-lookup"><span data-stu-id="e3467-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="e3467-105">Operazioni asincrone è anche possono aumentare la velocità effettiva in un'applicazione web, in cui il thread può liberato al completamento dell'operazione di database di altre richieste di servizio.</span><span class="sxs-lookup"><span data-stu-id="e3467-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="e3467-106">Per ulteriori informazioni, vedere [la programmazione asincrona in c#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="e3467-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="e3467-107">Core EF non supporta più operazioni parallele in esecuzione nella stessa istanza del contesto.</span><span class="sxs-lookup"><span data-stu-id="e3467-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="e3467-108">È sempre necessario attendere per un'operazione da completare prima di iniziare l'operazione successiva.</span><span class="sxs-lookup"><span data-stu-id="e3467-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="e3467-109">Questa operazione viene in genere eseguita tramite il `await` (parola chiave) in ogni operazione asincrona.</span><span class="sxs-lookup"><span data-stu-id="e3467-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="e3467-110">Entity Framework Core fornisce `DbContext.SaveChangesAsync()` come alternativa asincrona `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="e3467-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
