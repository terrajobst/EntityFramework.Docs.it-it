---
title: Test con InMemory - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 8aaea52f22954ef6a2b7d9b9c5627597c61ac644
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562546"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="deb00-102">Test con InMemory</span><span class="sxs-lookup"><span data-stu-id="deb00-102">Testing with InMemory</span></span>

<span data-ttu-id="deb00-103">Il provider InMemory è utile quando si desidera testare i componenti tramite un elemento che si avvicina alla connessione al database reale, senza l'overhead delle operazioni di database effettivo.</span><span class="sxs-lookup"><span data-stu-id="deb00-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="deb00-104">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="deb00-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="deb00-105">InMemory non è un database relazionale</span><span class="sxs-lookup"><span data-stu-id="deb00-105">InMemory is not a relational database</span></span>

<span data-ttu-id="deb00-106">Provider di database EF Core non è necessario essere database relazionali.</span><span class="sxs-lookup"><span data-stu-id="deb00-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="deb00-107">InMemory è progettato per essere un database generico per il test e non è progettato per simulare un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="deb00-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="deb00-108">Alcuni esempi sono:</span><span class="sxs-lookup"><span data-stu-id="deb00-108">Some examples of this include:</span></span>

* <span data-ttu-id="deb00-109">InMemory consentirà di salvare i dati che violano i vincoli di integrità referenziale in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="deb00-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="deb00-110">Se si usa DefaultValueSql(string) per una proprietà nel modello, questa è un'API di database relazionali e non avrà effetto durante l'esecuzione di InMemory.</span><span class="sxs-lookup"><span data-stu-id="deb00-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="deb00-111">[Concorrenza tramite la versione di riga/Timestamp](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` o `IsRowVersion`) non è supportato.</span><span class="sxs-lookup"><span data-stu-id="deb00-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="deb00-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) verrà generata se un aggiornamento viene eseguito usando un token di concorrenza precedente.</span><span class="sxs-lookup"><span data-stu-id="deb00-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="deb00-113">Per molti scopi di test queste differenze verranno è rilevante.</span><span class="sxs-lookup"><span data-stu-id="deb00-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="deb00-114">Tuttavia, se si desidera testare qualcosa che si comporta più come un database relazionale true, quindi è consigliabile usare [modalità in-memory SQLite](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="deb00-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="deb00-115">Scenario di test di esempio</span><span class="sxs-lookup"><span data-stu-id="deb00-115">Example testing scenario</span></span>

<span data-ttu-id="deb00-116">Si consideri il seguente servizio che consente al codice dell'applicazione di eseguire alcune operazioni correlate ai blog.</span><span class="sxs-lookup"><span data-stu-id="deb00-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="deb00-117">Usa internamente un `DbContext` che si connette a un database di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="deb00-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="deb00-118">Sarebbe utile scambiare il contesto per la connessione a un database InMemory in modo che è possibile scrivere test efficienti per questo servizio senza dover modificare il codice o eseguire molte operazioni per creare un test double del contesto.</span><span class="sxs-lookup"><span data-stu-id="deb00-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="deb00-119">Preparare il contesto</span><span class="sxs-lookup"><span data-stu-id="deb00-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="deb00-120">Evitare di configurare due provider di database</span><span class="sxs-lookup"><span data-stu-id="deb00-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="deb00-121">Nei test si intende configurare esternamente il contesto per usare il provider InMemory.</span><span class="sxs-lookup"><span data-stu-id="deb00-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="deb00-122">Se si configura un provider di database eseguendo l'override `OnConfiguring` nel contesto, è necessario aggiungere codice condizionale per assicurarsi di configurare il provider di database solo se non ne è stato configurato.</span><span class="sxs-lookup"><span data-stu-id="deb00-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="deb00-123">Se si usa ASP.NET Core, quindi non è necessario questo codice perché il provider di database è già configurato all'esterno del contesto (in Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="deb00-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="deb00-124">Aggiungere un costruttore per il test</span><span class="sxs-lookup"><span data-stu-id="deb00-124">Add a constructor for testing</span></span>

<span data-ttu-id="deb00-125">Il modo più semplice per abilitare il test su un altro database consiste nel modificare il contesto per esporre un costruttore che accetta un `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="deb00-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="deb00-126">`DbContextOptions<TContext>` indica il contesto di tutte le relative impostazioni, ad esempio il database a cui connettersi.</span><span class="sxs-lookup"><span data-stu-id="deb00-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="deb00-127">Questo è lo stesso oggetto che viene compilato mediante l'esecuzione del metodo OnConfiguring nel contesto.</span><span class="sxs-lookup"><span data-stu-id="deb00-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="deb00-128">Scrittura di test</span><span class="sxs-lookup"><span data-stu-id="deb00-128">Writing tests</span></span>

<span data-ttu-id="deb00-129">La chiave per il test con questo provider è la capacità di comunicare il contesto da utilizzare il provider InMemory e controllare l'ambito del database in memoria.</span><span class="sxs-lookup"><span data-stu-id="deb00-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="deb00-130">In genere si esegue un database pulito per ogni metodo di test.</span><span class="sxs-lookup"><span data-stu-id="deb00-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="deb00-131">Ecco un esempio di una classe di test che usa il database InMemory.</span><span class="sxs-lookup"><span data-stu-id="deb00-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="deb00-132">Ogni metodo di test consente di specificare un nome univoco del database, ovvero che ogni metodo ha un proprio database InMemory.</span><span class="sxs-lookup"><span data-stu-id="deb00-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="deb00-133">Usare la `.UseInMemoryDatabase()` metodo di estensione, il pacchetto NuGet di riferimento [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="deb00-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
