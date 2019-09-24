---
title: EF Core di registrazione
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f6e35c6d-45b7-4258-be1d-87c1bb67438d
uid: core/miscellaneous/logging
ms.openlocfilehash: 6a8499f9f0220087e76f2e0b3a75ce551c4ddb80
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197513"
---
# <a name="logging"></a><span data-ttu-id="c144b-102">Registrazione</span><span class="sxs-lookup"><span data-stu-id="c144b-102">Logging</span></span>

> [!TIP]  
> <span data-ttu-id="c144b-103">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="c144b-103">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Logging) on GitHub.</span></span>

## <a name="aspnet-core-applications"></a><span data-ttu-id="c144b-104">Applicazioni ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c144b-104">ASP.NET Core applications</span></span>

<span data-ttu-id="c144b-105">EF core si integra automaticamente con i meccanismi di registrazione di ASP.NET Core `AddDbContext` ogni `AddDbContextPool` volta che viene utilizzato o.</span><span class="sxs-lookup"><span data-stu-id="c144b-105">EF Core integrates automatically with the logging mechanisms of ASP.NET Core whenever `AddDbContext` or `AddDbContextPool` is used.</span></span> <span data-ttu-id="c144b-106">Pertanto, quando si utilizza ASP.NET Core, la registrazione deve essere configurata come descritto nella [documentazione di ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="c144b-106">Therefore, when using ASP.NET Core, logging should be configured as described in the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>

## <a name="other-applications"></a><span data-ttu-id="c144b-107">Altre applicazioni</span><span class="sxs-lookup"><span data-stu-id="c144b-107">Other applications</span></span>

<span data-ttu-id="c144b-108">Per la registrazione EF Core è necessario un ILoggerFactory configurato con uno o più provider di registrazione.</span><span class="sxs-lookup"><span data-stu-id="c144b-108">EF Core logging requires an ILoggerFactory which is itself configured with one or more logging providers.</span></span> <span data-ttu-id="c144b-109">I provider comuni sono forniti nei pacchetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="c144b-109">Common providers are shipped in the following packages:</span></span>

* <span data-ttu-id="c144b-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): Un semplice logger console.</span><span class="sxs-lookup"><span data-stu-id="c144b-110">[Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/): A simple console logger.</span></span>
* <span data-ttu-id="c144b-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supporta le funzionalità "log di diagnostica" e "flusso di log" dei servizi app Azure.</span><span class="sxs-lookup"><span data-stu-id="c144b-111">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/): Supports Azure App Services 'Diagnostics logs' and 'Log stream' features.</span></span>
* <span data-ttu-id="c144b-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Registra in un monitoraggio del debugger utilizzando System. Diagnostics. debug. WriteLine ().</span><span class="sxs-lookup"><span data-stu-id="c144b-112">[Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/): Logs to a debugger monitor using System.Diagnostics.Debug.WriteLine().</span></span>
* <span data-ttu-id="c144b-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Registra nel registro eventi di Windows.</span><span class="sxs-lookup"><span data-stu-id="c144b-113">[Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog/): Logs to Windows Event Log.</span></span>
* <span data-ttu-id="c144b-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supporta EventSource/EventListener.</span><span class="sxs-lookup"><span data-stu-id="c144b-114">[Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource/): Supports EventSource/EventListener.</span></span>
* <span data-ttu-id="c144b-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Registra un listener di traccia utilizzando `System.Diagnostics.TraceSource.TraceEvent()`.</span><span class="sxs-lookup"><span data-stu-id="c144b-115">[Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource/): Logs to a trace listener using `System.Diagnostics.TraceSource.TraceEvent()`.</span></span>

<span data-ttu-id="c144b-116">Dopo l'installazione dei pacchetti appropriati, l'applicazione deve creare un'istanza singleton/globale di un LoggerFactory.</span><span class="sxs-lookup"><span data-stu-id="c144b-116">After installing the appropriate package(s), the application should create a singleton/global instance of a LoggerFactory.</span></span> <span data-ttu-id="c144b-117">Ad esempio, usando il logger della console:</span><span class="sxs-lookup"><span data-stu-id="c144b-117">For example, using the console logger:</span></span>

# <a name="version-30tabv3"></a>[<span data-ttu-id="c144b-118">Versione 3,0</span><span class="sxs-lookup"><span data-stu-id="c144b-118">Version 3.0</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#DefineLoggerFactory)]

# <a name="version-2xtabv2"></a>[<span data-ttu-id="c144b-119">Versione 2. x</span><span class="sxs-lookup"><span data-stu-id="c144b-119">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="c144b-120">Nell'esempio di codice seguente viene `ConsoleLoggerProvider` usato un costruttore obsoleto nella versione 2,2 e sostituito in 3,0.</span><span class="sxs-lookup"><span data-stu-id="c144b-120">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="c144b-121">È possibile ignorare ed escludere gli avvisi quando si usa 2,2.</span><span class="sxs-lookup"><span data-stu-id="c144b-121">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[] { new ConsoleLoggerProvider((_, __) => true, true) });
```

***

<span data-ttu-id="c144b-122">Questa istanza singleton/globale deve quindi essere registrata con EF Core su `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c144b-122">This singleton/global instance should then be registered with EF Core on the `DbContextOptionsBuilder`.</span></span> <span data-ttu-id="c144b-123">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c144b-123">For example:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContext.cs#RegisterLoggerFactory)]

> [!WARNING]
> <span data-ttu-id="c144b-124">È molto importante che le applicazioni non creino una nuova istanza di ILoggerFactory per ogni istanza del contesto.</span><span class="sxs-lookup"><span data-stu-id="c144b-124">It is very important that applications do not create a new ILoggerFactory instance for each context instance.</span></span> <span data-ttu-id="c144b-125">Questa operazione comporterà una perdita di memoria e una riduzione delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="c144b-125">Doing so will result in a memory leak and poor performance.</span></span>

## <a name="filtering-what-is-logged"></a><span data-ttu-id="c144b-126">Filtraggio degli elementi registrati</span><span class="sxs-lookup"><span data-stu-id="c144b-126">Filtering what is logged</span></span>

<span data-ttu-id="c144b-127">L'applicazione può controllare ciò che viene registrato configurando un filtro in ILoggerProvider.</span><span class="sxs-lookup"><span data-stu-id="c144b-127">The application can control what is logged by configuring a filter on the ILoggerProvider.</span></span> <span data-ttu-id="c144b-128">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c144b-128">For example:</span></span>

# <a name="version-30tabv3"></a>[<span data-ttu-id="c144b-129">Versione 3,0</span><span class="sxs-lookup"><span data-stu-id="c144b-129">Version 3.0</span></span>](#tab/v3)

[!code-csharp[Main](../../../samples/core/Miscellaneous/Logging/Logging/BloggingContextWithFiltering.cs#DefineLoggerFactory)]

# <a name="version-2xtabv2"></a>[<span data-ttu-id="c144b-130">Versione 2. x</span><span class="sxs-lookup"><span data-stu-id="c144b-130">Version 2.x</span></span>](#tab/v2)

> [!NOTE]
> <span data-ttu-id="c144b-131">Nell'esempio di codice seguente viene `ConsoleLoggerProvider` usato un costruttore obsoleto nella versione 2,2 e sostituito in 3,0.</span><span class="sxs-lookup"><span data-stu-id="c144b-131">The following code sample uses a `ConsoleLoggerProvider` constructor that has been obsoleted in version 2.2 and replaced in 3.0.</span></span> <span data-ttu-id="c144b-132">È possibile ignorare ed escludere gli avvisi quando si usa 2,2.</span><span class="sxs-lookup"><span data-stu-id="c144b-132">It is safe to ignore and suppress the warnings when using 2.2.</span></span>

``` csharp
public static readonly LoggerFactory MyLoggerFactory
    = new LoggerFactory(new[]
    {
        new ConsoleLoggerProvider((category, level)
            => category == DbLoggerCategory.Database.Command.Name
               && level == LogLevel.Information, true)
    });
```

***

<span data-ttu-id="c144b-133">In questo esempio, il log viene filtrato in modo da restituire solo i messaggi:</span><span class="sxs-lookup"><span data-stu-id="c144b-133">In this example, the log is filtered to only return messages:</span></span>
 * <span data-ttu-id="c144b-134">nella categoria ' Microsoft. EntityFrameworkCore. database. Command '</span><span class="sxs-lookup"><span data-stu-id="c144b-134">in the 'Microsoft.EntityFrameworkCore.Database.Command' category</span></span>
 * <span data-ttu-id="c144b-135">a livello di "informazioni"</span><span class="sxs-lookup"><span data-stu-id="c144b-135">at the 'Information' level</span></span>

<span data-ttu-id="c144b-136">Per EF core, le `DbLoggerCategory` categorie di logger sono definite nella classe per facilitare la ricerca delle categorie, ma vengono risolte in stringhe semplici.</span><span class="sxs-lookup"><span data-stu-id="c144b-136">For EF Core, logger categories are defined in the `DbLoggerCategory` class to make it easy to find categories, but these resolve to simple strings.</span></span>

<span data-ttu-id="c144b-137">Per ulteriori informazioni sull'infrastruttura di registrazione sottostante, vedere la [documentazione relativa alla registrazione del ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span><span class="sxs-lookup"><span data-stu-id="c144b-137">More details on the underlying logging infrastructure can be found in the [ASP.NET Core logging documentation](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x).</span></span>
