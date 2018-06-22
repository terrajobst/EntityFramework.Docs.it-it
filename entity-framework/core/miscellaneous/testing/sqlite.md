---
title: Test con SQLite - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: 8fcb4b1a37da6f52219ebe3c672160627fe28fab
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052701"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="e906d-102">Test con SQLite</span><span class="sxs-lookup"><span data-stu-id="e906d-102">Testing with SQLite</span></span>

<span data-ttu-id="e906d-103">SQLite è una modalità in memoria che consente di utilizzare SQLite per scrivere i test in un database relazionale, senza l'overhead delle operazioni di database effettivo.</span><span class="sxs-lookup"><span data-stu-id="e906d-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="e906d-104">È possibile visualizzare in questo articolo [esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) su GitHub</span><span class="sxs-lookup"><span data-stu-id="e906d-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="e906d-105">Scenario di test di esempio</span><span class="sxs-lookup"><span data-stu-id="e906d-105">Example testing scenario</span></span>

<span data-ttu-id="e906d-106">Si consideri il seguente servizio che consente al codice dell'applicazione eseguire alcune operazioni correlate ai blog.</span><span class="sxs-lookup"><span data-stu-id="e906d-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="e906d-107">Utilizza internamente un `DbContext` che si connette a un database di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e906d-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="e906d-108">Potrebbe essere utile scambiare il contesto per la connessione a un database SQLite in memoria in modo che è possibile scrivere test efficiente per questo servizio senza dover modificare il codice o eseguire molte operazioni per creare un test double del contesto.</span><span class="sxs-lookup"><span data-stu-id="e906d-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="e906d-109">Preparare il contesto</span><span class="sxs-lookup"><span data-stu-id="e906d-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="e906d-110">Evitare di configurare due provider di database</span><span class="sxs-lookup"><span data-stu-id="e906d-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="e906d-111">I test si intende configurare esternamente il contesto per utilizzare il provider InMemory.</span><span class="sxs-lookup"><span data-stu-id="e906d-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="e906d-112">Se si sta configurando un provider di database eseguendo l'override `OnConfiguring` nel contesto, è necessario aggiungere il codice condizionale per assicurarsi di configurare il provider del database solo se non ne è stato configurato.</span><span class="sxs-lookup"><span data-stu-id="e906d-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="e906d-113">Se si utilizza ASP.NET Core, quindi non è necessario il codice perché il provider di database è configurato all'esterno del contesto (in Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="e906d-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="e906d-114">Aggiungere un costruttore per il test</span><span class="sxs-lookup"><span data-stu-id="e906d-114">Add a constructor for testing</span></span>

<span data-ttu-id="e906d-115">Il modo più semplice per consentire i test in un database diverso consiste nel modificare il contesto per esporre un costruttore che accetta un `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="e906d-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="e906d-116">`DbContextOptions<TContext>`indica il contesto di tutte le relative impostazioni, ad esempio il database a cui connettersi.</span><span class="sxs-lookup"><span data-stu-id="e906d-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="e906d-117">Questo è lo stesso oggetto che viene compilato mediante l'esecuzione del metodo OnConfiguring nel contesto.</span><span class="sxs-lookup"><span data-stu-id="e906d-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="e906d-118">Scrittura di test</span><span class="sxs-lookup"><span data-stu-id="e906d-118">Writing tests</span></span>

<span data-ttu-id="e906d-119">La chiave per il test con questo provider è la possibilità di stabilire il contesto da utilizzare SQLite e controllare l'ambito del database in memoria.</span><span class="sxs-lookup"><span data-stu-id="e906d-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="e906d-120">L'ambito del database è controllato da apertura e chiusura della connessione.</span><span class="sxs-lookup"><span data-stu-id="e906d-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="e906d-121">Il database è l'ambito per la durata che la connessione è aperta.</span><span class="sxs-lookup"><span data-stu-id="e906d-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="e906d-122">In genere si desidera il database originale per ogni metodo di test.</span><span class="sxs-lookup"><span data-stu-id="e906d-122">Typically you want a clean database for each test method.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
