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
# <a name="asynchronous-queries"></a>Query asincrone

Le query asincrone consentono di evitare il blocco di un thread mentre viene eseguita la query nel database. Le query asincrone sono importanti per mantenere un'interfaccia utente reattiva nelle applicazioni client thick. Possono anche aumentare la velocità effettiva nelle applicazioni Web in cui liberano il thread per soddisfare altre richieste nelle applicazioni Web. Per altre informazioni, vedere [Programmazione asincrona in C#](/dotnet/csharp/async).

> [!WARNING]  
> EF Core non supporta l'esecuzione di più operazioni parallele nella stessa istanza del contesto. È sempre necessario attendere il completamento di un'operazione prima di iniziare l'operazione successiva. Questa operazione viene in genere eseguita utilizzando la parola chiave `await` in ogni operazione asincrona.

Entity Framework Core fornisce un set di metodi di estensione asincroni simili ai metodi LINQ, che eseguono una query e restituiscono risultati. Gli esempi includono `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`. Non esistono versioni asincrone di alcuni operatori LINQ, ad esempio `Where(...)` o `OrderBy(...)`, perché questi metodi compilano solo l'albero delle espressioni LINQ e non causano l'esecuzione della query nel database.

> [!IMPORTANT]  
> I metodi di estensione asincroni di EF Core sono definiti nello spazio dei nomi `Microsoft.EntityFrameworkCore`. Questo spazio dei nomi deve essere importato affinché i metodi siano disponibili.

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
