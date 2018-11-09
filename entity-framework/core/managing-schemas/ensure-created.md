---
title: Creare ed eliminare le API - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/10/2017
ms.openlocfilehash: 336f6fd655603a2474a58dfef377e121d9b04c3a
ms.sourcegitcommit: a088421ecac4f5dc5213208170490181ae2f5f0f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285639"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="d4b5f-102">Creare ed eliminare le API</span><span class="sxs-lookup"><span data-stu-id="d4b5f-102">Create and Drop APIs</span></span>

<span data-ttu-id="d4b5f-103">I metodi EnsureCreated ed EnsureDeleted forniscono una semplice alternativa [migrazioni](migrations/index.md) per la gestione dello schema del database.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="d4b5f-104">Ciò è utile negli scenari quando i dati sono temporanei e possono essere eliminati quando viene modificato lo schema.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-104">This is useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="d4b5f-105">Ad esempio durante la creazione di prototipi, nei test o per le cache locale.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="d4b5f-106">Alcuni provider (in particolare quelli non relazionali) non supporta le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="d4b5f-107">Per questo motivo, EnsureCreated è spesso il modo più semplice per inizializzare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-107">For these, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="d4b5f-108">Le migrazioni ed EnsureCreated non funzionano bene insieme.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="d4b5f-109">Se si usano le migrazioni, non usare EnsureCreated per inizializzare lo schema.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="d4b5f-110">Transizione da EnsureCreated alle migrazioni non è un'esperienza senza problemi.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="d4b5f-111">Il simpelest per ottenere questo risultato consiste nell'eliminare il database e ricrearlo con le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-111">The simpelest way to achieve this is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="d4b5f-112">Se si prevede di usare le migrazioni in futuro, è consigliabile iniziare le migrazioni anziché EnsureCreated.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-112">If you anticipate using Migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="d4b5f-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="d4b5f-113">EnsureDeleted</span></span>

<span data-ttu-id="d4b5f-114">Se esiste, il metodo EnsureDeleted eliminerà il database.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="d4b5f-115">Se non si dispone delle autorizzazioni appropriate, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-115">If you don't have the appropiate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="d4b5f-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="d4b5f-116">EnsureCreated</span></span>

<span data-ttu-id="d4b5f-117">EnsureCreated creerà il database se non esiste e inizializzare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="d4b5f-118">Se sono presenti eventuali tabelle (incluse le tabelle per un'altra classe DbContext), lo schema non inizializzato.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="d4b5f-119">Sono disponibili anche versioni asincrone di questi metodi.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="d4b5f-120">Script SQL</span><span class="sxs-lookup"><span data-stu-id="d4b5f-120">SQL Script</span></span>

<span data-ttu-id="d4b5f-121">Per ottenere il codice SQL utilizzato da EnsureCreated, è possibile usare il metodo GenerateCreateScript.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="d4b5f-122">Più classi di DbContext</span><span class="sxs-lookup"><span data-stu-id="d4b5f-122">Multiple DbContext classes</span></span>

<span data-ttu-id="d4b5f-123">EnsureCreated funziona solo quando alcun tabelle non sono presenti nel database.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="d4b5f-124">Se necessario, è possibile scrivere il proprio controllo per verificare se lo schema deve essere inizializzata e usare il servizio IRelationalDatabaseCreator sottostante per inizializzare lo schema.</span><span class="sxs-lookup"><span data-stu-id="d4b5f-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
