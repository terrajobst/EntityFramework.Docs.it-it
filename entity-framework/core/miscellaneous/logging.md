---
title: Registrazione - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: efc78fbada3c59bf9cf2c4cb694835bb5ad60e76
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997005"
---
# <a name="logging"></a>Registrazione

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) di questo articolo in GitHub.

## <a name="aspnet-core-applications"></a>Applicazioni ASP.NET Core

EF Core si integra automaticamente con i meccanismi di registrazione di ASP.NET Core ogni volta che `AddDbContext` o `AddDbContextPool` viene usato. Pertanto, quando si usa ASP.NET Core, la registrazione deve essere configurata come descritto nel [documentazione di ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Altre applicazioni

EF Core attualmente la registrazione richiede una ILoggerFactory che a sua volta è configurato con uno o più ILoggerProvider. Provider di utilizzo comune vengono forniti nei pacchetti seguenti:

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): un logger di console semplice.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): servizi App di Azure supporta 'Log di diagnostica' e 'Log di flusso' funzionalità.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): i log per un monitor di debugger usando System.Diagnostics.Debug.WriteLine().
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): i log al registro eventi di Windows.
* [EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): supporta EventSource/EventListener.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): i log a un listener di traccia utilizzando System.Diagnostics.TraceSource.TraceEvent().

Dopo aver installato i pacchetti appropriati, l'applicazione deve creare un'istanza singleton/globale di un LoggerFactory. Ad esempio, usando il logger della console:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

L'istanza singleton/globale andrebbero poi registrata con EF Core nel `DbContextOptionsBuilder`. Ad esempio:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> È molto importante che le applicazioni non creano una nuova istanza ILoggerFactory per ogni istanza del contesto. Questa operazione comporterà una perdita di memoria e una riduzione delle prestazioni.

## <a name="filtering-what-is-logged"></a>Il filtro elementi registrati

Il modo più semplice per filtrare elementi registrati consiste nel configurare, quando si registra il ILoggerProvider. Ad esempio:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

In questo esempio, il log viene filtrato per restituire solo i messaggi:
 * Nella categoria 'Microsoft.EntityFrameworkCore.Database.Command'
 * a livello di "Informazioni"

Per Entity Framework Core, vengono definite le categorie del logger nel `DbLoggerCategory` classe per renderlo più semplice trovare le categorie, ma questi risolvere in stringhe semplici.

Altre informazioni sull'infrastruttura sottostante di registrazione sono reperibile nel [documentazione di registrazione di ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).
