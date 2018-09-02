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
# <a name="asynchronous-queries"></a>Query asincrone

Le query asincrone consentono di evitare il blocco di un thread mentre viene eseguita la query nel database. Questo può essere utile per evitare di bloccare l'interfaccia utente di un'applicazione thick-client. Le operazioni asincrone possono anche aumentare la velocità effettiva in un'applicazione Web, in cui il thread può essere liberato per gestire altre richieste mentre viene completata l'operazione di database. Per altre informazioni, vedere [Programmazione asincrona in C#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> EF Core non supporta più operazioni parallele in esecuzione nella stessa istanza di contesto. È sempre necessario attendere il completamento di un'operazione prima di iniziare l'operazione successiva. Questa operazione viene in genere eseguita tramite la parola chiave `await` per ogni operazione asincrona.

Entity Framework Core fornisce un set di metodi di estensione asincroni che possono essere usati in alternativa ai metodi LINQ che causano l'esecuzione di una query e la restituzione di risultati. Alcuni esempi sono `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()` e così via. Non esistono versioni asincrone di operatori LINQ come `Where(...)`, `OrderBy(...)` e così via, perché questi metodi consentono solo di comporre l'albero delle espressioni LINQ e non causano l'esecuzione della query nel database.

> [!IMPORTANT]  
> I metodi di estensione asincroni di EF Core sono definiti nello spazio dei nomi `Microsoft.EntityFrameworkCore`. Questo spazio dei nomi deve essere importato affinché i metodi siano disponibili.

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
