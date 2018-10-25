---
title: Registrazione - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 65501b5ac03ae544c51b7fc1a07fa9eea849f1e3
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022145"
---
# <a name="logging"></a><span data-ttu-id="ba870-102">Registrazione</span><span class="sxs-lookup"><span data-stu-id="ba870-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="ba870-103">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="ba870-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="ba870-104">Applicazioni ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ba870-104">ASP.NET Core applications</span></span>

<span data-ttu-id="ba870-105">EF Core si integra automaticamente con i meccanismi di registrazione di ASP.NET Core ogni volta che `AddDbContext` o `AddDbContextPool` viene usato.</span><span class="sxs-lookup"><span data-stu-id="ba870-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="ba870-106">Pertanto, quando si usa ASP.NET Core, la registrazione deve essere configurata come descritto nel [documentazione di ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="ba870-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="ba870-107">Altre applicazioni</span><span class="sxs-lookup"><span data-stu-id="ba870-107">Other applications</span></span>

<span data-ttu-id="ba870-108">EF Core attualmente la registrazione richiede una ILoggerFactory che a sua volta è configurato con uno o più ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="ba870-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="ba870-109">Provider di utilizzo comune vengono forniti nei pacchetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="ba870-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="ba870-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): un logger di console semplice.</span><span class="sxs-lookup"><span data-stu-id="ba870-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="ba870-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): servizi App di Azure supporta 'Log di diagnostica' e 'Log di flusso' funzionalità.</span><span class="sxs-lookup"><span data-stu-id="ba870-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="ba870-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): i log per un monitor di debugger usando System.Diagnostics.Debug.WriteLine().</span><span class="sxs-lookup"><span data-stu-id="ba870-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="ba870-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): i log al registro eventi di Windows.</span><span class="sxs-lookup"><span data-stu-id="ba870-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="ba870-114">[EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): supporta EventSource/EventListener.</span><span class="sxs-lookup"><span data-stu-id="ba870-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="ba870-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): i log a un listener di traccia utilizzando System.Diagnostics.TraceSource.TraceEvent().</span><span class="sxs-lookup"><span data-stu-id="ba870-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

<span data-ttu-id="ba870-116">Dopo aver installato i pacchetti appropriati, l'applicazione deve creare un'istanza singleton/globale di un LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="ba870-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="ba870-117">Ad esempio, usando il logger della console:</span><span class="sxs-lookup"><span data-stu-id="ba870-117">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="ba870-118">L'istanza singleton/globale andrebbero poi registrata con EF Core nel `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="ba870-118">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="ba870-119">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ba870-119">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="ba870-120">È molto importante che le applicazioni non creano una nuova istanza ILoggerFactory per ogni istanza del contesto.</span><span class="sxs-lookup"><span data-stu-id="ba870-120">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="ba870-121">Questa operazione comporterà una perdita di memoria e una riduzione delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="ba870-121">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="ba870-122">Il filtro elementi registrati</span><span class="sxs-lookup"><span data-stu-id="ba870-122">Filtering what is logged</span></span>

<span data-ttu-id="ba870-123">Il modo più semplice per filtrare elementi registrati consiste nel configurare, quando si registra il ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="ba870-123">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="ba870-124">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ba870-124">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="ba870-125">In questo esempio, il log viene filtrato per restituire solo i messaggi:</span><span class="sxs-lookup"><span data-stu-id="ba870-125">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="ba870-126">Nella categoria 'Microsoft.EntityFrameworkCore.Database.Command'</span><span class="sxs-lookup"><span data-stu-id="ba870-126">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="ba870-127">a livello di "Informazioni"</span><span class="sxs-lookup"><span data-stu-id="ba870-127">at the 'Information' level</span></span>

<span data-ttu-id="ba870-128">Per Entity Framework Core, vengono definite le categorie del logger nel `DbLoggerCategory` classe per renderlo più semplice trovare le categorie, ma questi risolvere in stringhe semplici.</span><span class="sxs-lookup"><span data-stu-id="ba870-128">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="ba870-129">Altre informazioni sull'infrastruttura sottostante di registrazione sono reperibile nel [documentazione di registrazione di ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="ba870-129">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
