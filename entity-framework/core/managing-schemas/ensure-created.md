---
title: Creare ed eliminare API-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2018
ms.openlocfilehash: 88c1403d2fae740ad78bb7c41d404b0dd91e86ae
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813435"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="be7b9-102">API di creazione ed eliminazione</span><span class="sxs-lookup"><span data-stu-id="be7b9-102">Create and Drop APIs</span></span>

<span data-ttu-id="be7b9-103">I metodi EnsureCreated e EnsureDeleted forniscono un'alternativa semplice alle [migrazioni](migrations/index.md) per la gestione dello schema del database.</span><span class="sxs-lookup"><span data-stu-id="be7b9-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="be7b9-104">Questi metodi sono utili negli scenari in cui i dati sono temporanei e possono essere eliminati quando lo schema viene modificato.</span><span class="sxs-lookup"><span data-stu-id="be7b9-104">These methods are useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="be7b9-105">Ad esempio, durante la fase di prototipo, nei test o per le cache locali.</span><span class="sxs-lookup"><span data-stu-id="be7b9-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="be7b9-106">Alcuni provider (soprattutto quelli non relazionali) non supportano le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="be7b9-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="be7b9-107">Per questi provider, EnsureCreated è spesso il modo più semplice per inizializzare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="be7b9-107">For these providers, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="be7b9-108">EnsureCreated e le migrazioni non funzionano bene insieme.</span><span class="sxs-lookup"><span data-stu-id="be7b9-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="be7b9-109">Se si usano le migrazioni, non usare EnsureCreated per inizializzare lo schema.</span><span class="sxs-lookup"><span data-stu-id="be7b9-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="be7b9-110">La transizione da EnsureCreated a migrazioni non è un'esperienza senza problemi.</span><span class="sxs-lookup"><span data-stu-id="be7b9-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="be7b9-111">Il modo più semplice per eseguire questa operazione consiste nell'eliminare il database e ricrearlo usando le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="be7b9-111">The simplest way to do it is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="be7b9-112">Se si prevede di usare le migrazioni in futuro, è preferibile iniziare con le migrazioni invece di usare EnsureCreated.</span><span class="sxs-lookup"><span data-stu-id="be7b9-112">If you anticipate using migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="be7b9-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="be7b9-113">EnsureDeleted</span></span>

<span data-ttu-id="be7b9-114">Se esistente, il metodo EnsureDeleted eliminerà il database.</span><span class="sxs-lookup"><span data-stu-id="be7b9-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="be7b9-115">Se non si dispone delle autorizzazioni appropriate, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="be7b9-115">If you don't have the appropriate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="be7b9-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="be7b9-116">EnsureCreated</span></span>

<span data-ttu-id="be7b9-117">EnsureCreated creerà il database se non esiste e inizializza lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="be7b9-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="be7b9-118">Se sono presenti tabelle (incluse le tabelle per un'altra classe DbContext), lo schema non verrà inizializzato.</span><span class="sxs-lookup"><span data-stu-id="be7b9-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="be7b9-119">Sono disponibili anche le versioni asincrone di questi metodi.</span><span class="sxs-lookup"><span data-stu-id="be7b9-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="be7b9-120">Script SQL</span><span class="sxs-lookup"><span data-stu-id="be7b9-120">SQL Script</span></span>

<span data-ttu-id="be7b9-121">Per ottenere l'oggetto SQL usato da EnsureCreated, è possibile usare il Metodo GenerateCreateScript.</span><span class="sxs-lookup"><span data-stu-id="be7b9-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="be7b9-122">Più classi DbContext</span><span class="sxs-lookup"><span data-stu-id="be7b9-122">Multiple DbContext classes</span></span>

<span data-ttu-id="be7b9-123">EnsureCreated funziona solo quando nel database non è presente alcuna tabella.</span><span class="sxs-lookup"><span data-stu-id="be7b9-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="be7b9-124">Se necessario, è possibile scrivere un controllo personalizzato per verificare se lo schema deve essere inizializzato e usare il servizio IRelationalDatabaseCreator sottostante per inizializzare lo schema.</span><span class="sxs-lookup"><span data-stu-id="be7b9-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
