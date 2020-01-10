---
title: Test con InMemory-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: fcd2f99ad06fd30ef9e36fd1e5a6a09fe0a45d07
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781118"
---
# <a name="testing-with-inmemory"></a>Test con InMemory

Il provider InMemory è utile quando si desidera testare i componenti utilizzando un elemento che si avvicina alla connessione al database reale, senza l'overhead delle operazioni di database effettive.

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) di questo articolo in GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>InMemory non è un database relazionale

Non è necessario che i provider di database EF Core siano database relazionali. InMemory è progettato per essere un database per utilizzo generico per il testing e non è progettato per simulare un database relazionale.

Di seguito sono riportati alcuni esempi:

* InMemory consente di salvare i dati che violano i vincoli di integrità referenziale in un database relazionale.
* Se si usa DefaultValueSql (String) per una proprietà nel modello, si tratta di un'API di database relazionale e non avrà alcun effetto in caso di esecuzione in memoria.
* La [concorrenza tramite la versione timestamp/Row](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` o `IsRowVersion`) non è supportata. Se un aggiornamento viene eseguito utilizzando un token di concorrenza precedente, non verrà generato alcun [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) .

> [!TIP]  
> Per molti scopi, queste differenze non sono importanti. Tuttavia, se si desidera eseguire il test su un elemento che si comporta in modo più simile a un database relazionale reale, provare a utilizzare la [modalità in memoria di SQLite](sqlite.md).

## <a name="example-testing-scenario"></a>Scenario di test di esempio

Si consideri il seguente servizio che consente al codice dell'applicazione di eseguire alcune operazioni correlate ai Blog. Usa internamente un `DbContext` che si connette a un database SQL Server. Sarebbe utile scambiare questo contesto per connettersi a un database in memoria, in modo da poter scrivere test efficienti per questo servizio senza dover modificare il codice o eseguire molto lavoro per creare un doppio di test del contesto.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Preparare il contesto

### <a name="avoid-configuring-two-database-providers"></a>Evitare di configurare due provider di database

Nei test verrà configurato esternamente il contesto per l'uso del provider InMemory. Se si sta configurando un provider di database eseguendo l'override di `OnConfiguring` nel contesto, è necessario aggiungere un codice condizionale per assicurarsi di configurare solo il provider di database, se non ne è già stato configurato uno.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Se si usa ASP.NET Core, questo codice non dovrebbe essere necessario perché il provider di database è già configurato al di fuori del contesto (in Startup.cs).

### <a name="add-a-constructor-for-testing"></a>Aggiungere un costruttore per il test

Il modo più semplice per abilitare i test in un database diverso consiste nel modificare il contesto per esporre un costruttore che accetta un `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` indica al contesto tutte le impostazioni, ad esempio il database a cui connettersi. Si tratta dello stesso oggetto compilato eseguendo il metodo Configuring nel contesto.

## <a name="writing-tests"></a>Scrittura di test

La chiave per eseguire il test con questo provider è la possibilità di indicare al contesto di usare il provider InMemory e controllare l'ambito del database in memoria. In genere si vuole usare un database pulito per ogni metodo di test.

Di seguito è riportato un esempio di una classe di test che usa il database in memoria. Ogni metodo di test specifica un nome di database univoco, ovvero ogni metodo dispone di un proprio database in memoria.

>[!TIP]
> Per usare il metodo di estensione `.UseInMemoryDatabase()`, fare riferimento al pacchetto NuGet [Microsoft. EntityFrameworkCore. InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
