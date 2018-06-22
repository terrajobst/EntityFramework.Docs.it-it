---
title: Test con InMemory - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 33690e3424d0777930d3cb8167575fb0f4ddd8f7
ms.sourcegitcommit: d096484dcf9eff73d9943fa60db7a418b10ca0b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2018
ms.locfileid: "27995587"
---
# <a name="testing-with-inmemory"></a>Test con InMemory

Il provider InMemory è utile quando si desidera testare i componenti dell'utilizzo di un elemento che si avvicinano a connettersi al database reale, senza l'overhead delle operazioni di database effettivo.

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) di questo articolo in GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>InMemory non è un database relazionale

Provider di database di sistema di EF non debbano essere database relazionali. InMemory è progettato per essere un database generico per il testing e non è progettato per simulare un database relazionale.

Alcuni esempi sono:
* InMemory consente di salvare i dati che violano i vincoli di integrità referenziale in un database relazionale.

* Se si utilizza DefaultValueSql(string) per una proprietà nel modello, questa è un'API di database relazionale e si avrà alcun effetto durante l'esecuzione InMemory.

> [!TIP]  
> Queste differenze verranno è importante per molti scopi di test. Tuttavia, se si desidera confrontare con un elemento che è più simile a un database relazionale true, è consigliabile utilizzare [modalità in-memory SQLite](sqlite.md).

## <a name="example-testing-scenario"></a>Scenario di test di esempio

Si consideri il seguente servizio che consente al codice dell'applicazione eseguire alcune operazioni correlate ai blog. Utilizza internamente un `DbContext` che si connette a un database di SQL Server. Potrebbe essere utile scambiare il contesto per la connessione a un database InMemory in modo che è possibile scrivere test efficiente per questo servizio senza dover modificare il codice o eseguire molte operazioni per creare un test double del contesto.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Preparare il contesto

### <a name="avoid-configuring-two-database-providers"></a>Evitare di configurare due provider di database

I test si intende configurare esternamente il contesto per utilizzare il provider InMemory. Se si sta configurando un provider di database eseguendo l'override `OnConfiguring` nel contesto, è necessario aggiungere il codice condizionale per assicurarsi di configurare il provider del database solo se non ne è stato configurato.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Se si utilizza ASP.NET Core, quindi non è necessario il codice perché il provider di database è già configurato all'esterno del contesto (in Startup.cs).

### <a name="add-a-constructor-for-testing"></a>Aggiungere un costruttore per il test

Il modo più semplice per consentire i test in un database diverso consiste nel modificare il contesto per esporre un costruttore che accetta un `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`indica il contesto di tutte le relative impostazioni, ad esempio il database a cui connettersi. Questo è lo stesso oggetto che viene compilato mediante l'esecuzione del metodo OnConfiguring nel contesto.

## <a name="writing-tests"></a>Scrittura di test

La chiave per il test con questo provider è la possibilità di stabilire il contesto da utilizzare il provider InMemory e controllare l'ambito del database in memoria. In genere si desidera il database originale per ogni metodo di test.

Di seguito è riportato un esempio di una classe di test che utilizza il database InMemory. Ogni metodo di test specifica un nome di database univoco, vale a dire che ogni metodo presenta un proprio database InMemory.

>[!TIP]
> Utilizzare il `.UseInMemoryDatabase()` metodo di estensione, il pacchetto NuGet di riferimento `Microsoft.EntityFrameworkCore.InMemory`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
