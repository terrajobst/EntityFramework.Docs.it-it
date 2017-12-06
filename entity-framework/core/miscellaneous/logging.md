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
# <a name="logging"></a><span data-ttu-id="a031f-102">Registrazione</span><span class="sxs-lookup"><span data-stu-id="a031f-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="a031f-103">È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) su GitHub.</span><span class="sxs-lookup"><span data-stu-id="a031f-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="a031f-104">Applicazioni ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a031f-104">ASP.NET Core applications</span></span>

<span data-ttu-id="a031f-105">Core EF automaticamente integra mechanims la registrazione di ASP.NET Core ogni volta che `AddDbContext` o `AddDbContextPool` viene utilizzato.</span><span class="sxs-lookup"><span data-stu-id="a031f-105">EF Core integrates automatically with the logging mechanims of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="a031f-106">Pertanto, quando si utilizza ASP.NET Core, la registrazione deve essere configurata come descritto nel [documentazione di ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="a031f-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="a031f-107">Altre applicazioni</span><span class="sxs-lookup"><span data-stu-id="a031f-107">Other applications</span></span>

<span data-ttu-id="a031f-108">Core EF registrazione attualmente richiede una ILoggerFactory che a sua volta configurato con uno o più ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="a031f-108">EF Core logging currently requires an ILoggerFactory which is itself configured with one or more ILoggerProvider.</span></span> <span data-ttu-id="a031f-109">Provider comuni vengono forniti nei seguenti pacchetti:</span><span class="sxs-lookup"><span data-stu-id="a031f-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="a031f-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): un logger di console semplice.</span><span class="sxs-lookup"><span data-stu-id="a031f-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="a031f-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): servizi di App di Azure supporta 'Log di diagnostica' e 'Log di flusso' funzionalità.</span><span class="sxs-lookup"><span data-stu-id="a031f-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="a031f-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): log a un monitoraggio del debugger utilizzando System.Diagnostics.Debug.WriteLine().</span><span class="sxs-lookup"><span data-stu-id="a031f-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="a031f-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): log registro eventi di Windows.</span><span class="sxs-lookup"><span data-stu-id="a031f-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="a031f-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): supporta EventSource/EventListener.</span><span class="sxs-lookup"><span data-stu-id="a031f-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="a031f-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): log a un listener di traccia utilizzando System.Diagnostics.TraceSource.TraceEvent().</span><span class="sxs-lookup"><span data-stu-id="a031f-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using System.Diagnostics.TraceSource.TraceEvent().</span></span>

<span data-ttu-id="a031f-116">Dopo l'installazione per i pacchetti appropriati, l'applicazione deve creare un'istanza singleton/globale di un LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="a031f-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="a031f-117">Ad esempio, tramite il logger di console:</span><span class="sxs-lookup"><span data-stu-id="a031f-117">For example, using the console logger:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

<span data-ttu-id="a031f-118">L'istanza singleton/globale quindi deve essere registrata con Entity Framework di base sul `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a031f-118">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="a031f-119">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a031f-119">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="a031f-120">È molto importante che le applicazioni non creare una nuova istanza di ILoggerFactory per ogni istanza del contesto.</span><span class="sxs-lookup"><span data-stu-id="a031f-120">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="a031f-121">Questa operazione provocherà una perdita di memoria e una riduzione delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="a031f-121">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="a031f-122">Filtro di elementi registrati</span><span class="sxs-lookup"><span data-stu-id="a031f-122">Filtering what is logged</span></span>

<span data-ttu-id="a031f-123">Il modo più semplice per filtrare informazioni di connessione è configurarla durante la registrazione di ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="a031f-123">The easiest way to filter what is logged is to configure it when registering the ILoggerProvider.</span></span> <span data-ttu-id="a031f-124">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a031f-124">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

<span data-ttu-id="a031f-125">In questo esempio, il log viene filtrato per restituire solo i messaggi:</span><span class="sxs-lookup"><span data-stu-id="a031f-125">In this example, the log is filtered to return only messages:</span></span>
 * <span data-ttu-id="a031f-126">Nella categoria 'Microsoft.EntityFrameworkCore.Database.Command'</span><span class="sxs-lookup"><span data-stu-id="a031f-126">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="a031f-127">a livello di "Informazioni"</span><span class="sxs-lookup"><span data-stu-id="a031f-127">at the 'Information' level</span></span>

<span data-ttu-id="a031f-128">Per Entity Framework Core, categorie di logger sono definite nella `DbLoggerCategory` classe per renderlo più facile trovare le categorie, ma questi risolvere semplici stringhe.</span><span class="sxs-lookup"><span data-stu-id="a031f-128">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="a031f-129">Ulteriori informazioni sull'infrastruttura di registrazione sottostante sono reperibile nel [documentazione di registrazione di ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="a031f-129">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>