---
title: Test con SQLite - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: bc9d6768a90ce17160c4126d2a68fddaa30d63de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996868"
---
# <a name="testing-with-sqlite"></a>Test con SQLite

SQLite presenta una modalità in memoria che consente di utilizzare SQLite per scrivere i test in un database relazionale, senza l'overhead delle operazioni di database effettivo.

> [!TIP]  
> È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) su GitHub

## <a name="example-testing-scenario"></a>Scenario di test di esempio

Si consideri il seguente servizio che consente al codice dell'applicazione di eseguire alcune operazioni correlate ai blog. Usa internamente un `DbContext` che si connette a un database di SQL Server. Sarebbe utile scambiare il contesto per la connessione a un database SQLite in memoria in modo che è possibile scrivere test efficienti per questo servizio senza dover modificare il codice o eseguire molte operazioni per creare un test double del contesto.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Preparare il contesto

### <a name="avoid-configuring-two-database-providers"></a>Evitare di configurare due provider di database

Nei test si intende configurare esternamente il contesto per usare il provider InMemory. Se si configura un provider di database eseguendo l'override `OnConfiguring` nel contesto, è necessario aggiungere codice condizionale per assicurarsi di configurare il provider di database solo se non ne è stato configurato.

> [!TIP]  
> Se si usa ASP.NET Core, quindi non è necessario questo codice perché il provider di database è configurato all'esterno del contesto (in Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Aggiungere un costruttore per il test

Il modo più semplice per abilitare il test su un altro database consiste nel modificare il contesto per esporre un costruttore che accetta un `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` indica il contesto di tutte le relative impostazioni, ad esempio il database a cui connettersi. Questo è lo stesso oggetto che viene compilato mediante l'esecuzione del metodo OnConfiguring nel contesto.

## <a name="writing-tests"></a>Scrittura di test

La chiave per il test con questo provider è la capacità di comunicare il contesto da utilizzare SQLite e controllare l'ambito del database in memoria. L'ambito del database viene controllato dall'apertura e chiusura della connessione. Il database ha come ambito la durata della connessione è aperta. In genere si esegue un database pulito per ogni metodo di test.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
