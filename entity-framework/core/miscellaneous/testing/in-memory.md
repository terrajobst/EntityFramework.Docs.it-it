---
title: Test con InMemory - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 2754d1deba98fcee0eb88669293b2197545c8874
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997892"
---
# <a name="testing-with-inmemory"></a>Test con InMemory

Il provider InMemory è utile quando si desidera testare i componenti tramite un elemento che si avvicina alla connessione al database reale, senza l'overhead delle operazioni di database effettivo.

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) di questo articolo in GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>InMemory non è un database relazionale

Provider di database EF Core non è necessario essere database relazionali. InMemory è progettato per essere un database generico per il test e non è progettato per simulare un database relazionale.

Alcuni esempi sono:

* InMemory consentirà di salvare i dati che violano i vincoli di integrità referenziale in un database relazionale.
* Se si usa DefaultValueSql(string) per una proprietà nel modello, questa è un'API di database relazionali e non avrà effetto durante l'esecuzione di InMemory.
* [Concorrenza tramite la versione di riga/Timestamp](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` o `IsRowVersion`) non è supportato. No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) verrà generata se un aggiornamento viene eseguito usando un token di concorrenza precedente.

> [!TIP]  
> Per molti scopi di test queste differenze verranno è rilevante. Tuttavia, se si desidera testare qualcosa che si comporta più come un database relazionale true, quindi è consigliabile usare [modalità in-memory SQLite](sqlite.md).

## <a name="example-testing-scenario"></a>Scenario di test di esempio

Si consideri il seguente servizio che consente al codice dell'applicazione di eseguire alcune operazioni correlate ai blog. Usa internamente un `DbContext` che si connette a un database di SQL Server. Sarebbe utile scambiare il contesto per la connessione a un database InMemory in modo che è possibile scrivere test efficienti per questo servizio senza dover modificare il codice o eseguire molte operazioni per creare un test double del contesto.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Preparare il contesto

### <a name="avoid-configuring-two-database-providers"></a>Evitare di configurare due provider di database

Nei test si intende configurare esternamente il contesto per usare il provider InMemory. Se si configura un provider di database eseguendo l'override `OnConfiguring` nel contesto, è necessario aggiungere codice condizionale per assicurarsi di configurare il provider di database solo se non ne è stato configurato.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Se si usa ASP.NET Core, quindi non è necessario questo codice perché il provider di database è già configurato all'esterno del contesto (in Startup.cs).

### <a name="add-a-constructor-for-testing"></a>Aggiungere un costruttore per il test

Il modo più semplice per abilitare il test su un altro database consiste nel modificare il contesto per esporre un costruttore che accetta un `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` indica il contesto di tutte le relative impostazioni, ad esempio il database a cui connettersi. Questo è lo stesso oggetto che viene compilato mediante l'esecuzione del metodo OnConfiguring nel contesto.

## <a name="writing-tests"></a>Scrittura di test

La chiave per il test con questo provider è la capacità di comunicare il contesto da utilizzare il provider InMemory e controllare l'ambito del database in memoria. In genere si esegue un database pulito per ogni metodo di test.

Ecco un esempio di una classe di test che usa il database InMemory. Ogni metodo di test consente di specificare un nome univoco del database, ovvero che ogni metodo ha un proprio database InMemory.

>[!TIP]
> Usare la `.UseInMemoryDatabase()` metodo di estensione, il pacchetto NuGet di riferimento `Microsoft.EntityFrameworkCore.InMemory`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
