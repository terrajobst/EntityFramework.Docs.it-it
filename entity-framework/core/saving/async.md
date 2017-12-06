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
# <a name="asynchronous-saving"></a>Salvataggio asincrono

Salvataggio asincrono si evita di bloccare un thread mentre le modifiche vengono scritte nel database. Questo può essere utile per evitare di bloccare l'interfaccia utente di un'applicazione thick client. Operazioni asincrone è anche possono aumentare la velocità effettiva in un'applicazione web, in cui il thread può liberato al completamento dell'operazione di database di altre richieste di servizio. Per ulteriori informazioni, vedere [la programmazione asincrona in c#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> Core EF non supporta più operazioni parallele in esecuzione nella stessa istanza del contesto. È sempre necessario attendere per un'operazione da completare prima di iniziare l'operazione successiva. Questa operazione viene in genere eseguita tramite il `await` (parola chiave) in ogni operazione asincrona.

Entity Framework Core fornisce `DbContext.SaveChangesAsync()` come alternativa asincrona `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
