---
title: Test con InMemory-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 18641677098c20d9172136b07868dcb647d189c6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416509"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="48d06-102">Test con InMemory</span><span class="sxs-lookup"><span data-stu-id="48d06-102">Testing with InMemory</span></span>

<span data-ttu-id="48d06-103">Il provider InMemory è utile quando si desidera testare i componenti utilizzando un elemento che si avvicina alla connessione al database reale, senza l'overhead delle operazioni di database effettive.</span><span class="sxs-lookup"><span data-stu-id="48d06-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="48d06-104">È possibile visualizzare l'[esempio](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="48d06-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="48d06-105">InMemory non è un database relazionale</span><span class="sxs-lookup"><span data-stu-id="48d06-105">InMemory is not a relational database</span></span>

<span data-ttu-id="48d06-106">Non è necessario che i provider di database EF Core siano database relazionali.</span><span class="sxs-lookup"><span data-stu-id="48d06-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="48d06-107">InMemory è progettato per essere un database per utilizzo generico per il testing e non è progettato per simulare un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="48d06-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="48d06-108">Di seguito sono riportati alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="48d06-108">Some examples of this include:</span></span>

* <span data-ttu-id="48d06-109">InMemory consente di salvare i dati che violano i vincoli di integrità referenziale in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="48d06-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="48d06-110">Se si usa DefaultValueSql (String) per una proprietà nel modello, si tratta di un'API di database relazionale e non avrà alcun effetto in caso di esecuzione in memoria.</span><span class="sxs-lookup"><span data-stu-id="48d06-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="48d06-111">La [concorrenza tramite la versione timestamp/Row](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` o `IsRowVersion`) non è supportata.</span><span class="sxs-lookup"><span data-stu-id="48d06-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="48d06-112">Se un aggiornamento viene eseguito utilizzando un token di concorrenza precedente, non verrà generato alcun [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) .</span><span class="sxs-lookup"><span data-stu-id="48d06-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="48d06-113">Per molti scopi, queste differenze non sono importanti.</span><span class="sxs-lookup"><span data-stu-id="48d06-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="48d06-114">Tuttavia, se si desidera eseguire il test su un elemento che si comporta in modo più simile a un database relazionale reale, provare a utilizzare la [modalità in memoria di SQLite](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="48d06-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="48d06-115">Scenario di test di esempio</span><span class="sxs-lookup"><span data-stu-id="48d06-115">Example testing scenario</span></span>

<span data-ttu-id="48d06-116">Si consideri il seguente servizio che consente al codice dell'applicazione di eseguire alcune operazioni correlate ai Blog.</span><span class="sxs-lookup"><span data-stu-id="48d06-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="48d06-117">Usa internamente un `DbContext` che si connette a un database SQL Server.</span><span class="sxs-lookup"><span data-stu-id="48d06-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="48d06-118">Sarebbe utile scambiare questo contesto per connettersi a un database in memoria, in modo da poter scrivere test efficienti per questo servizio senza dover modificare il codice o eseguire molto lavoro per creare un doppio di test del contesto.</span><span class="sxs-lookup"><span data-stu-id="48d06-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="48d06-119">Preparare il contesto</span><span class="sxs-lookup"><span data-stu-id="48d06-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="48d06-120">Evitare di configurare due provider di database</span><span class="sxs-lookup"><span data-stu-id="48d06-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="48d06-121">Nei test verrà configurato esternamente il contesto per l'uso del provider InMemory.</span><span class="sxs-lookup"><span data-stu-id="48d06-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="48d06-122">Se si sta configurando un provider di database eseguendo l'override di `OnConfiguring` nel contesto, è necessario aggiungere un codice condizionale per assicurarsi di configurare solo il provider di database, se non ne è già stato configurato uno.</span><span class="sxs-lookup"><span data-stu-id="48d06-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="48d06-123">Se si usa ASP.NET Core, questo codice non dovrebbe essere necessario perché il provider di database è già configurato al di fuori del contesto (in Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="48d06-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="48d06-124">Aggiungere un costruttore per il test</span><span class="sxs-lookup"><span data-stu-id="48d06-124">Add a constructor for testing</span></span>

<span data-ttu-id="48d06-125">Il modo più semplice per abilitare i test in un database diverso consiste nel modificare il contesto per esporre un costruttore che accetta un `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="48d06-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="48d06-126">`DbContextOptions<TContext>` indica al contesto tutte le impostazioni, ad esempio il database a cui connettersi.</span><span class="sxs-lookup"><span data-stu-id="48d06-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="48d06-127">Si tratta dello stesso oggetto compilato eseguendo il metodo Configuring nel contesto.</span><span class="sxs-lookup"><span data-stu-id="48d06-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="48d06-128">Scrittura di test</span><span class="sxs-lookup"><span data-stu-id="48d06-128">Writing tests</span></span>

<span data-ttu-id="48d06-129">La chiave per eseguire il test con questo provider è la possibilità di indicare al contesto di usare il provider InMemory e controllare l'ambito del database in memoria.</span><span class="sxs-lookup"><span data-stu-id="48d06-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="48d06-130">In genere si vuole usare un database pulito per ogni metodo di test.</span><span class="sxs-lookup"><span data-stu-id="48d06-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="48d06-131">Di seguito è riportato un esempio di una classe di test che usa il database in memoria.</span><span class="sxs-lookup"><span data-stu-id="48d06-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="48d06-132">Ogni metodo di test specifica un nome di database univoco, ovvero ogni metodo dispone di un proprio database in memoria.</span><span class="sxs-lookup"><span data-stu-id="48d06-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="48d06-133">Per usare il metodo di estensione `.UseInMemoryDatabase()`, fare riferimento al pacchetto NuGet [Microsoft. EntityFrameworkCore. InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="48d06-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
