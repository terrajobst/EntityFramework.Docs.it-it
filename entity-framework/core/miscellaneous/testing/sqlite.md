---
title: Test con SQLite-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: f7f847d8c766c0d4d7577ea6760ee72a17f84933
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417302"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="b8d43-102">Test con SQLite</span><span class="sxs-lookup"><span data-stu-id="b8d43-102">Testing with SQLite</span></span>

<span data-ttu-id="b8d43-103">SQLite ha una modalità in memoria che consente di usare SQLite per scrivere i test in un database relazionale, senza l'overhead delle operazioni di database effettive.</span><span class="sxs-lookup"><span data-stu-id="b8d43-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="b8d43-104">È possibile visualizzare l' [esempio](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) di questo articolo in GitHub</span><span class="sxs-lookup"><span data-stu-id="b8d43-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="b8d43-105">Scenario di test di esempio</span><span class="sxs-lookup"><span data-stu-id="b8d43-105">Example testing scenario</span></span>

<span data-ttu-id="b8d43-106">Si consideri il seguente servizio che consente al codice dell'applicazione di eseguire alcune operazioni correlate ai Blog.</span><span class="sxs-lookup"><span data-stu-id="b8d43-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="b8d43-107">Usa internamente un `DbContext` che si connette a un database SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b8d43-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="b8d43-108">Sarebbe utile scambiare questo contesto per connettersi a un database SQLite in memoria per poter scrivere test efficienti per questo servizio senza dover modificare il codice o eseguire numerose operazioni per creare un doppio di test del contesto.</span><span class="sxs-lookup"><span data-stu-id="b8d43-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="b8d43-109">Preparare il contesto</span><span class="sxs-lookup"><span data-stu-id="b8d43-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="b8d43-110">Evitare di configurare due provider di database</span><span class="sxs-lookup"><span data-stu-id="b8d43-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="b8d43-111">Nei test verrà configurato esternamente il contesto per l'uso del provider InMemory.</span><span class="sxs-lookup"><span data-stu-id="b8d43-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="b8d43-112">Se si sta configurando un provider di database eseguendo l'override di `OnConfiguring` nel contesto, è necessario aggiungere un codice condizionale per assicurarsi di configurare solo il provider di database, se non ne è già stato configurato uno.</span><span class="sxs-lookup"><span data-stu-id="b8d43-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="b8d43-113">Se si usa ASP.NET Core, questo codice non dovrebbe essere necessario perché il provider di database è configurato al di fuori del contesto (in Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="b8d43-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="b8d43-114">Aggiungere un costruttore per il test</span><span class="sxs-lookup"><span data-stu-id="b8d43-114">Add a constructor for testing</span></span>

<span data-ttu-id="b8d43-115">Il modo più semplice per abilitare i test in un database diverso consiste nel modificare il contesto per esporre un costruttore che accetta un `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="b8d43-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="b8d43-116">`DbContextOptions<TContext>` indica al contesto tutte le impostazioni, ad esempio il database a cui connettersi.</span><span class="sxs-lookup"><span data-stu-id="b8d43-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="b8d43-117">Si tratta dello stesso oggetto compilato eseguendo il metodo Configuring nel contesto.</span><span class="sxs-lookup"><span data-stu-id="b8d43-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="b8d43-118">Scrittura di test</span><span class="sxs-lookup"><span data-stu-id="b8d43-118">Writing tests</span></span>

<span data-ttu-id="b8d43-119">La chiave per eseguire il test con questo provider è la possibilità di indicare al contesto di usare SQLite e controllare l'ambito del database in memoria.</span><span class="sxs-lookup"><span data-stu-id="b8d43-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="b8d43-120">L'ambito del database viene controllato aprendo e chiudendo la connessione.</span><span class="sxs-lookup"><span data-stu-id="b8d43-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="b8d43-121">Il database ha come ambito la durata di apertura della connessione.</span><span class="sxs-lookup"><span data-stu-id="b8d43-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="b8d43-122">In genere si vuole usare un database pulito per ogni metodo di test.</span><span class="sxs-lookup"><span data-stu-id="b8d43-122">Typically you want a clean database for each test method.</span></span>

>[!TIP]
> <span data-ttu-id="b8d43-123">Per usare `SqliteConnection()` e il metodo di estensione `.UseSqlite()`, fare riferimento al pacchetto NuGet [Microsoft. EntityFrameworkCore. sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="b8d43-123">To use `SqliteConnection()` and the `.UseSqlite()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
