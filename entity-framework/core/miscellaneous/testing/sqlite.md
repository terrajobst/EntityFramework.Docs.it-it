---
title: Test con SQLite - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: 8fcb4b1a37da6f52219ebe3c672160627fe28fab
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="testing-with-sqlite"></a>Test con SQLite

SQLite è una modalità in memoria che consente di utilizzare SQLite per scrivere i test in un database relazionale, senza l'overhead delle operazioni di database effettivo.

> [!TIP]  
> È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) su GitHub

## <a name="example-testing-scenario"></a>Scenario di test di esempio

Si consideri il seguente servizio che consente al codice dell'applicazione eseguire alcune operazioni correlate ai blog. Utilizza internamente un `DbContext` che si connette a un database di SQL Server. Potrebbe essere utile scambiare il contesto per la connessione a un database SQLite in memoria in modo che è possibile scrivere test efficiente per questo servizio senza dover modificare il codice o eseguire molte operazioni per creare un test double del contesto.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Preparare il contesto

### <a name="avoid-configuring-two-database-providers"></a>Evitare di configurare due provider di database

I test si intende configurare esternamente il contesto per utilizzare il provider InMemory. Se si sta configurando un provider di database eseguendo l'override `OnConfiguring` nel contesto, è necessario aggiungere il codice condizionale per assicurarsi di configurare il provider del database solo se non ne è stato configurato.

> [!TIP]  
> Se si utilizza ASP.NET Core, quindi non è necessario il codice perché il provider di database è configurato all'esterno del contesto (in Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Aggiungere un costruttore per il test

Il modo più semplice per consentire i test in un database diverso consiste nel modificare il contesto per esporre un costruttore che accetta un `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`indica il contesto di tutte le relative impostazioni, ad esempio il database a cui connettersi. Questo è lo stesso oggetto che viene compilato mediante l'esecuzione del metodo OnConfiguring nel contesto.

## <a name="writing-tests"></a>Scrittura di test

La chiave per il test con questo provider è la possibilità di stabilire il contesto da utilizzare SQLite e controllare l'ambito del database in memoria. L'ambito del database è controllato da apertura e chiusura della connessione. Il database è l'ambito per la durata che la connessione è aperta. In genere si desidera il database originale per ogni metodo di test.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
