---
title: Query asincrone - EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2020
ms.locfileid: "78417753"
---
# <a name="asynchronous-queries"></a>Query asincrone

Le query asincrone consentono di evitare il blocco di un thread mentre viene eseguita la query nel database. Le query asincrone sono importanti per mantenere un'interfaccia utente reattiva nelle applicazioni thick-client. Possono anche aumentare la velocità effettiva nelle applicazioni Web in cui liberano il thread per soddisfare altre richieste nelle applicazioni Web. Per ulteriori informazioni, vedere [Programmazione asincrona in C.](/dotnet/csharp/async)

> [!WARNING]  
> EF Core non supporta più operazioni parallele eseguite nella stessa istanza di contesto. È sempre necessario attendere il completamento di un'operazione prima di iniziare l'operazione successiva. Questa operazione viene in `await` genere eseguita utilizzando la parola chiave in ogni operazione asincrona.

Entity Framework Core fornisce un set di metodi di estensione asincroni simili ai metodi LINQ, che eseguono una query e restituiscono risultati. Gli `ToListAsync()`esempi `ToArrayAsync()` `SingleAsync()`includono , , . Non esistono versioni asincrone di `Where(...)` alcuni `OrderBy(...)` operatori LINQ, ad esempio o perché questi metodi compilano solo l'albero delle espressioni LINQ e non causano l'esecuzione della query nel database.

> [!IMPORTANT]  
> I metodi di estensione asincroni di EF Core sono definiti nello spazio dei nomi `Microsoft.EntityFrameworkCore`. Questo spazio dei nomi deve essere importato affinché i metodi siano disponibili.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
