---
title: Test con SQLite - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: e8ff204a09d50064b4f0d4376f02b05c8681ac25
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562533"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="073fd-102">Test con SQLite</span><span class="sxs-lookup"><span data-stu-id="073fd-102">Testing with SQLite</span></span>

<span data-ttu-id="073fd-103">SQLite presenta una modalità in memoria che consente di utilizzare SQLite per scrivere i test in un database relazionale, senza l'overhead delle operazioni di database effettivo.</span><span class="sxs-lookup"><span data-stu-id="073fd-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="073fd-104">È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) su GitHub</span><span class="sxs-lookup"><span data-stu-id="073fd-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="073fd-105">Scenario di test di esempio</span><span class="sxs-lookup"><span data-stu-id="073fd-105">Example testing scenario</span></span>

<span data-ttu-id="073fd-106">Si consideri il seguente servizio che consente al codice dell'applicazione di eseguire alcune operazioni correlate ai blog.</span><span class="sxs-lookup"><span data-stu-id="073fd-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="073fd-107">Usa internamente un `DbContext` che si connette a un database di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="073fd-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="073fd-108">Sarebbe utile scambiare il contesto per la connessione a un database SQLite in memoria in modo che è possibile scrivere test efficienti per questo servizio senza dover modificare il codice o eseguire molte operazioni per creare un test double del contesto.</span><span class="sxs-lookup"><span data-stu-id="073fd-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="073fd-109">Preparare il contesto</span><span class="sxs-lookup"><span data-stu-id="073fd-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="073fd-110">Evitare di configurare due provider di database</span><span class="sxs-lookup"><span data-stu-id="073fd-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="073fd-111">Nei test si intende configurare esternamente il contesto per usare il provider InMemory.</span><span class="sxs-lookup"><span data-stu-id="073fd-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="073fd-112">Se si configura un provider di database eseguendo l'override `OnConfiguring` nel contesto, è necessario aggiungere codice condizionale per assicurarsi di configurare il provider di database solo se non ne è stato configurato.</span><span class="sxs-lookup"><span data-stu-id="073fd-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="073fd-113">Se si usa ASP.NET Core, quindi non è necessario questo codice perché il provider di database è configurato all'esterno del contesto (in Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="073fd-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="073fd-114">Aggiungere un costruttore per il test</span><span class="sxs-lookup"><span data-stu-id="073fd-114">Add a constructor for testing</span></span>

<span data-ttu-id="073fd-115">Il modo più semplice per abilitare il test su un altro database consiste nel modificare il contesto per esporre un costruttore che accetta un `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="073fd-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="073fd-116">`DbContextOptions<TContext>` indica il contesto di tutte le relative impostazioni, ad esempio il database a cui connettersi.</span><span class="sxs-lookup"><span data-stu-id="073fd-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="073fd-117">Questo è lo stesso oggetto che viene compilato mediante l'esecuzione del metodo OnConfiguring nel contesto.</span><span class="sxs-lookup"><span data-stu-id="073fd-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="073fd-118">Scrittura di test</span><span class="sxs-lookup"><span data-stu-id="073fd-118">Writing tests</span></span>

<span data-ttu-id="073fd-119">La chiave per il test con questo provider è la capacità di comunicare il contesto da utilizzare SQLite e controllare l'ambito del database in memoria.</span><span class="sxs-lookup"><span data-stu-id="073fd-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="073fd-120">L'ambito del database viene controllato dall'apertura e chiusura della connessione.</span><span class="sxs-lookup"><span data-stu-id="073fd-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="073fd-121">Il database ha come ambito la durata della connessione è aperta.</span><span class="sxs-lookup"><span data-stu-id="073fd-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="073fd-122">In genere si esegue un database pulito per ogni metodo di test.</span><span class="sxs-lookup"><span data-stu-id="073fd-122">Typically you want a clean database for each test method.</span></span>

>[!TIP]
> <span data-ttu-id="073fd-123">Per utilizzare `SqliteConnection()` e il `.UseSqlite()` metodo di estensione, il pacchetto NuGet di riferimento [Microsoft](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="073fd-123">To use `SqliteConnection()` and the `.UseSqlite()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
