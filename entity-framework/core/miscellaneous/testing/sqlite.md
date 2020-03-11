---
title: Test con SQLite-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: f7f847d8c766c0d4d7577ea6760ee72a17f84933
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417302"
---
# <a name="testing-with-sqlite"></a>Test con SQLite

SQLite ha una modalità in memoria che consente di usare SQLite per scrivere i test in un database relazionale, senza l'overhead delle operazioni di database effettive.

> [!TIP]  
> È possibile visualizzare l' [esempio](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) di questo articolo in GitHub

## <a name="example-testing-scenario"></a>Scenario di test di esempio

Si consideri il seguente servizio che consente al codice dell'applicazione di eseguire alcune operazioni correlate ai Blog. Usa internamente un `DbContext` che si connette a un database SQL Server. Sarebbe utile scambiare questo contesto per connettersi a un database SQLite in memoria per poter scrivere test efficienti per questo servizio senza dover modificare il codice o eseguire numerose operazioni per creare un doppio di test del contesto.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Preparare il contesto

### <a name="avoid-configuring-two-database-providers"></a>Evitare di configurare due provider di database

Nei test verrà configurato esternamente il contesto per l'uso del provider InMemory. Se si sta configurando un provider di database eseguendo l'override di `OnConfiguring` nel contesto, è necessario aggiungere un codice condizionale per assicurarsi di configurare solo il provider di database, se non ne è già stato configurato uno.

> [!TIP]  
> Se si usa ASP.NET Core, questo codice non dovrebbe essere necessario perché il provider di database è configurato al di fuori del contesto (in Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Aggiungere un costruttore per il test

Il modo più semplice per abilitare i test in un database diverso consiste nel modificare il contesto per esporre un costruttore che accetta un `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` indica al contesto tutte le impostazioni, ad esempio il database a cui connettersi. Si tratta dello stesso oggetto compilato eseguendo il metodo Configuring nel contesto.

## <a name="writing-tests"></a>Scrittura di test

La chiave per eseguire il test con questo provider è la possibilità di indicare al contesto di usare SQLite e controllare l'ambito del database in memoria. L'ambito del database viene controllato aprendo e chiudendo la connessione. Il database ha come ambito la durata di apertura della connessione. In genere si vuole usare un database pulito per ogni metodo di test.

>[!TIP]
> Per usare `SqliteConnection()` e il metodo di estensione `.UseSqlite()`, fare riferimento al pacchetto NuGet [Microsoft. EntityFrameworkCore. sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
