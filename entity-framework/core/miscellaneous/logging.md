---
title: Registrazione - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
ms.technology: entity-framework-core
uid: core/miscellaneous/logging
ms.openlocfilehash: 807560e563eddfb72d4286353b1403a0d2e2a441
ms.sourcegitcommit: 5367516f063cb42804ec92c31cdf76322554f2b5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/08/2017
---
# <a name="logging"></a>Registrazione

> [!TIP]  
> È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) su GitHub.

## <a name="aspnet-core-applications"></a>Applicazioni ASP.NET Core

Core EF automaticamente integra mechanims la registrazione di ASP.NET Core ogni volta che `AddDbContext` o `AddDbContextPool` viene utilizzato. Pertanto, quando si utilizza ASP.NET Core, la registrazione deve essere configurata come descritto nel [documentazione di ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Altre applicazioni

Core EF registrazione attualmente richiede una ILoggerFactory che a sua volta configurato con uno o più ILoggerProvider. Provider comuni vengono forniti nei seguenti pacchetti:

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): un logger di console semplice.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): servizi di App di Azure supporta 'Log di diagnostica' e 'Log di flusso' funzionalità.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): log a un monitoraggio del debugger utilizzando System.Diagnostics.Debug.WriteLine().
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): log registro eventi di Windows.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): supporta EventSource/EventListener.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): log a un listener di traccia utilizzando System.Diagnostics.TraceSource.TraceEvent().

Dopo l'installazione per i pacchetti appropriati, l'applicazione deve creare un'istanza singleton/globale di un LoggerFactory. Ad esempio, tramite il logger di console:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

L'istanza singleton/globale quindi deve essere registrata con Entity Framework di base sul `DbContextOptionsBuilder`. Ad esempio:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> È molto importante che le applicazioni non creare una nuova istanza di ILoggerFactory per ogni istanza del contesto. Questa operazione provocherà una perdita di memoria e una riduzione delle prestazioni.

## <a name="filtering-what-is-logged"></a>Filtro di elementi registrati

Il modo più semplice per filtrare informazioni di connessione è configurarla durante la registrazione di ILoggerProvider. Ad esempio:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

In questo esempio, il log viene filtrato per restituire solo i messaggi:
 * Nella categoria 'Microsoft.EntityFrameworkCore.Database.Command'
 * a livello di "Informazioni"

Per Entity Framework Core, categorie di logger sono definite nella `DbLoggerCategory` classe per renderlo più facile trovare le categorie, ma questi risolvere semplici stringhe.

Ulteriori informazioni sull'infrastruttura di registrazione sottostante sono reperibile nel [documentazione di registrazione di ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).