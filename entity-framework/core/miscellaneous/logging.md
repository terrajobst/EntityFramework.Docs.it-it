---
title: Registrazione - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 0a996403afdbe076b1690c98eeb305b40c4d1f4a
ms.sourcegitcommit: 109a16478de498b65717a6e09be243647e217fb3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/10/2019
ms.locfileid: "55985574"
---
# <a name="logging"></a>Registrazione

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) di questo articolo in GitHub.

## <a name="aspnet-core-applications"></a>Applicazioni ASP.NET Core

EF Core si integra automaticamente con i meccanismi di registrazione di ASP.NET Core ogni volta che `AddDbContext` o `AddDbContextPool` viene usato. Pertanto, quando si usa ASP.NET Core, la registrazione deve essere configurata come descritto nel [documentazione di ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).

## <a name="other-applications"></a>Altre applicazioni

EF Core attualmente la registrazione richiede una ILoggerFactory che a sua volta è configurato con uno o più ILoggerProvider. Provider di utilizzo comune vengono forniti nei pacchetti seguenti:

* [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): Un logger di console semplice.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supporta la funzionalità 'Log di flusso' e 'Log di diagnostica' servizi App di Azure.
* [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): I log a un debugger monitorare System.Diagnostics.Debug.WriteLine().
* [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Log per il registro eventi di Windows.
* [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supporta EventSource/EventListener.
* [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Un listener di traccia utilizzando System.Diagnostics.TraceSource.TraceEvent(). log.

> [!NOTE]
> Nell'esempio di codice di esempio Usa un `ConsoleLoggerProvider` costruttore che ha stato obsoleta nella versione 2.2. Sostituzioni appropriate per API di registrazione obsoleta saranno disponibili nella versione 3.0. Nel frattempo, è consigliabile ignorare ed eliminare gli avvisi.

Dopo aver installato i pacchetti appropriati, l'applicazione deve creare un'istanza singleton/globale di un LoggerFactory. Ad esempio, usando il logger della console:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

L'istanza singleton/globale andrebbero poi registrata con EF Core nel `DbContextOptionsBuilder`. Ad esempio:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> È molto importante che le applicazioni non creano una nuova istanza ILoggerFactory per ogni istanza del contesto. Questa operazione comporterà una perdita di memoria e una riduzione delle prestazioni.

## <a name="filtering-what-is-logged"></a>Il filtro elementi registrati

> [!NOTE]
> Nell'esempio di codice di esempio Usa un `ConsoleLoggerProvider` costruttore che ha stato obsoleta nella versione 2.2. Sostituzioni appropriate per API di registrazione obsoleta saranno disponibili nella versione 3.0. Nel frattempo, è consigliabile ignorare ed eliminare gli avvisi.

Il modo più semplice per filtrare elementi registrati consiste nel configurare, quando si registra il ILoggerProvider. Ad esempio:

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

In questo esempio, il log viene filtrato per restituire solo i messaggi:
 * Nella categoria 'Microsoft.EntityFrameworkCore.Database.Command'
 * a livello di "Informazioni"

Per Entity Framework Core, vengono definite le categorie del logger nel `DbLoggerCategory` classe per renderlo più semplice trovare le categorie, ma questi risolvere in stringhe semplici.

Altre informazioni sull'infrastruttura sottostante di registrazione sono reperibile nel [documentazione di registrazione di ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).
