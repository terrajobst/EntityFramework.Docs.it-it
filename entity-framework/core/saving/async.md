---
title: Salvataggio asincrono - EF Core
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 6f482a77300ff2930953686751a579b022bf6f77
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997282"
---
# <a name="asynchronous-saving"></a>Salvataggio asincrono

Il salvataggio asincrono consente di evitare il blocco di un thread mentre le modifiche vengono scritte nel database. Questo può essere utile per evitare di bloccare l'interfaccia utente di un'applicazione thick-client. Le operazioni asincrone possono anche aumentare la velocità effettiva in un'applicazione Web, in cui il thread può essere liberato per gestire altre richieste mentre viene completata l'operazione di database. Per altre informazioni, vedere [Programmazione asincrona in C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core non supporta più operazioni parallele in esecuzione nella stessa istanza di contesto. È sempre necessario attendere il completamento di un'operazione prima di iniziare l'operazione successiva. Questa operazione viene in genere eseguita tramite la parola chiave `await` per ogni operazione asincrona.

Entity Framework Core offre `DbContext.SaveChangesAsync()` come alternativa asincrona a `DbContext.SaveChanges()`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
