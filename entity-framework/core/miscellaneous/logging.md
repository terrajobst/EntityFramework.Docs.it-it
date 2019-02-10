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
# <a name="logging"></a><span data-ttu-id="873f1-102">Registrazione</span><span class="sxs-lookup"><span data-stu-id="873f1-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="873f1-103">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="873f1-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="873f1-104">Applicazioni ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="873f1-104">ASP.NET Core applications</span></span>

<span data-ttu-id="873f1-105">EF Core si integra automaticamente con i meccanismi di registrazione di ASP.NET Core ogni volta che `AddDbContext` o `AddDbContextPool` viene usato.</span><span class="sxs-lookup"><span data-stu-id="873f1-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="873f1-106">Pertanto, quando si usa ASP.NET Core, la registrazione deve essere configurata come descritto nel [documentazione di ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="873f1-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="873f1-107">Altre applicazioni</span><span class="sxs-lookup"><span data-stu-id="873f1-107">Other applications</span></span>

<span data-ttu-id="873f1-108">EF Core attualmente la registrazione richiede una ILoggerFactory che a sua volta è configurato con uno o più ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="873f1-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="873f1-109">Provider di utilizzo comune vengono forniti nei pacchetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="873f1-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="873f1-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): Un logger di console semplice.</span><span class="sxs-lookup"><span data-stu-id="873f1-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="873f1-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supporta la funzionalità 'Log di flusso' e 'Log di diagnostica' servizi App di Azure.</span><span class="sxs-lookup"><span data-stu-id="873f1-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="873f1-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): I log a un debugger monitorare System.Diagnostics.Debug.WriteLine().</span><span class="sxs-lookup"><span data-stu-id="873f1-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="873f1-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Log per il registro eventi di Windows.</span><span class="sxs-lookup"><span data-stu-id="873f1-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="873f1-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supporta EventSource/EventListener.</span><span class="sxs-lookup"><span data-stu-id="873f1-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="873f1-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Un listener di traccia utilizzando System.Diagnostics.TraceSource.TraceEvent(). log.</span><span class="sxs-lookup"><span data-stu-id="873f1-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

> [!NOTE]
> <span data-ttu-id="873f1-116">Nell'esempio di codice di esempio Usa un `ConsoleLoggerProvider` costruttore che ha stato obsoleta nella versione 2.2.</span><span class="sxs-lookup"><span data-stu-id="873f1-116">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2.</span></span> <span data-ttu-id="873f1-117">Sostituzioni appropriate per API di registrazione obsoleta saranno disponibili nella versione 3.0.</span><span class="sxs-lookup"><span data-stu-id="873f1-117">Proper replacements for obsolete logging APIs will be available in version 3.0.</span></span> <span data-ttu-id="873f1-118">Nel frattempo, è consigliabile ignorare ed eliminare gli avvisi.</span><span class="sxs-lookup"><span data-stu-id="873f1-118">In the meantime, it is safe to ignore and suppress the warnings.</span></span>

<span data-ttu-id="873f1-119">Dopo aver installato i pacchetti appropriati, l'applicazione deve creare un'istanza singleton/globale di un LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="873f1-119">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="873f1-120">Ad esempio, usando il logger della console:</span><span class="sxs-lookup"><span data-stu-id="873f1-120">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="873f1-121">L'istanza singleton/globale andrebbero poi registrata con EF Core nel `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="873f1-121">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="873f1-122">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="873f1-122">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="873f1-123">È molto importante che le applicazioni non creano una nuova istanza ILoggerFactory per ogni istanza del contesto.</span><span class="sxs-lookup"><span data-stu-id="873f1-123">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="873f1-124">Questa operazione comporterà una perdita di memoria e una riduzione delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="873f1-124">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="873f1-125">Il filtro elementi registrati</span><span class="sxs-lookup"><span data-stu-id="873f1-125">Filtering what is logged</span></span>

> [!NOTE]
> <span data-ttu-id="873f1-126">Nell'esempio di codice di esempio Usa un `ConsoleLoggerProvider` costruttore che ha stato obsoleta nella versione 2.2.</span><span class="sxs-lookup"><span data-stu-id="873f1-126">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2.</span></span> <span data-ttu-id="873f1-127">Sostituzioni appropriate per API di registrazione obsoleta saranno disponibili nella versione 3.0.</span><span class="sxs-lookup"><span data-stu-id="873f1-127">Proper replacements for obsolete logging APIs will be available in version 3.0.</span></span> <span data-ttu-id="873f1-128">Nel frattempo, è consigliabile ignorare ed eliminare gli avvisi.</span><span class="sxs-lookup"><span data-stu-id="873f1-128">In the meantime, it is safe to ignore and suppress the warnings.</span></span>

<span data-ttu-id="873f1-129">Il modo più semplice per filtrare elementi registrati consiste nel configurare, quando si registra il ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="873f1-129">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="873f1-130">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="873f1-130">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="873f1-131">In questo esempio, il log viene filtrato per restituire solo i messaggi:</span><span class="sxs-lookup"><span data-stu-id="873f1-131">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="873f1-132">Nella categoria 'Microsoft.EntityFrameworkCore.Database.Command'</span><span class="sxs-lookup"><span data-stu-id="873f1-132">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="873f1-133">a livello di "Informazioni"</span><span class="sxs-lookup"><span data-stu-id="873f1-133">at the 'Information' level</span></span>

<span data-ttu-id="873f1-134">Per Entity Framework Core, vengono definite le categorie del logger nel `DbLoggerCategory` classe per renderlo più semplice trovare le categorie, ma questi risolvere in stringhe semplici.</span><span class="sxs-lookup"><span data-stu-id="873f1-134">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="873f1-135">Altre informazioni sull'infrastruttura sottostante di registrazione sono reperibile nel [documentazione di registrazione di ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="873f1-135">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
