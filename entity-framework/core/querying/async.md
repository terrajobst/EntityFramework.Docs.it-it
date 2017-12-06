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
# <a name="asynchronous-queries"></a>Query asincrone

Query asincrone evitare di bloccare un thread mentre viene eseguita la query nel database. Questo può essere utile per evitare di bloccare l'interfaccia utente di un'applicazione thick client. Operazioni asincrone è anche possono aumentare la velocità effettiva in un'applicazione web, in cui il thread può liberato al completamento dell'operazione di database di altre richieste di servizio. Per ulteriori informazioni, vedere [la programmazione asincrona in c#](https://docs.microsoft.com/dotnet/csharp/async).

> [!WARNING]  
> Core EF non supporta più operazioni parallele in esecuzione nella stessa istanza del contesto. È sempre necessario attendere per un'operazione da completare prima di iniziare l'operazione successiva. Questa operazione viene in genere eseguita tramite il `await` (parola chiave) in ogni operazione asincrona.

Componenti di base di Entity Framework fornisce un set di metodi di estensione asincrona che può essere utilizzato come alternativa ai metodi LINQ che causano l'esecuzione di una query e risultati restituiti. Gli esempi includono `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`e così via. Non esistono versioni asincrone di operatori LINQ, ad esempio `Where(...)`, `OrderBy(...)`, e così via. perché questi metodi solo creare l'albero delle espressioni LINQ e non causano la query da eseguire nel database.

> [!IMPORTANT]  
> I metodi di estensione async EF Core sono definiti nel `Microsoft.EntityFrameworkCore` dello spazio dei nomi. Questo spazio dei nomi deve essere importato per i metodi siano disponibili.

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
