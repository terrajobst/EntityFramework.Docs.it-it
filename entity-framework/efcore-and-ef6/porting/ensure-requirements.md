---
title: Porting da EF6 a requisiti di convalida EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 07caa39e8a56db71e493331ea9f0286309abc52a
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565354"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="4309f-102">Prima di eseguire il porting da EF6 a EF Core: Convalidare i requisiti dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="4309f-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="4309f-103">Prima di iniziare il processo di porting, è importante verificare che EF Core soddisfi i requisiti di accesso ai dati per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4309f-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="4309f-104">Funzionalità mancanti</span><span class="sxs-lookup"><span data-stu-id="4309f-104">Missing features</span></span>

<span data-ttu-id="4309f-105">Assicurarsi che EF Core disponga di tutte le funzionalità che è necessario usare nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4309f-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="4309f-106">Per un confronto dettagliato del modo in cui il set di funzionalità in EF Core viene confrontato con EF6, vedere [confronto delle funzionalità](../features.md) .</span><span class="sxs-lookup"><span data-stu-id="4309f-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="4309f-107">Se mancano alcune funzionalità necessarie, assicurarsi che sia possibile compensare la mancanza di queste funzionalità prima di eseguire il porting a EF Core.</span><span class="sxs-lookup"><span data-stu-id="4309f-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="4309f-108">Modifiche del comportamento</span><span class="sxs-lookup"><span data-stu-id="4309f-108">Behavior changes</span></span>

<span data-ttu-id="4309f-109">Si tratta di un elenco non esaustivo di alcune modifiche del comportamento tra EF6 e EF Core.</span><span class="sxs-lookup"><span data-stu-id="4309f-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="4309f-110">È importante tenere presenti queste considerazioni come la porta dell'applicazione, in quanto possono cambiare il comportamento dell'applicazione, ma non verranno visualizzati come errori di compilazione dopo lo scambio in EF Core.</span><span class="sxs-lookup"><span data-stu-id="4309f-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="4309f-111">Comportamento di DbSet. Add/Connetti e Graph</span><span class="sxs-lookup"><span data-stu-id="4309f-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="4309f-112">In EF6 la chiamata `DbSet.Add()` a su un'entità comporta una ricerca ricorsiva di tutte le entità a cui viene fatto riferimento nelle proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="4309f-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="4309f-113">Anche le entità trovate e non ancora rilevate dal contesto vengono contrassegnate come aggiunte.</span><span class="sxs-lookup"><span data-stu-id="4309f-113">Any entities that are found, and are not already tracked by the context, are also marked as added.</span></span> <span data-ttu-id="4309f-114">`DbSet.Attach()`si comporta allo stesso modo, ad eccezione del fatto che tutte le entità sono contrassegnate come invariate.</span><span class="sxs-lookup"><span data-stu-id="4309f-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="4309f-115">**EF Core esegue una ricerca ricorsiva simile, ma con alcune regole leggermente diverse.**</span><span class="sxs-lookup"><span data-stu-id="4309f-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="4309f-116">L'entità radice è sempre nello stato richiesto (aggiunta per `DbSet.Add` e senza modifiche per `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="4309f-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="4309f-117">**Per le entità rilevate durante la ricerca ricorsiva delle proprietà di navigazione:**</span><span class="sxs-lookup"><span data-stu-id="4309f-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="4309f-118">**Se la chiave primaria dell'entità è l'archivio generato**</span><span class="sxs-lookup"><span data-stu-id="4309f-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="4309f-119">Se la chiave primaria non è impostata su un valore, lo stato viene impostato su added.</span><span class="sxs-lookup"><span data-stu-id="4309f-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="4309f-120">Il valore della chiave primaria viene considerato "non impostato" Se viene assegnato il valore predefinito CLR per il tipo di proprietà, ad esempio `0` per `int`, `null` per `string`e così via.</span><span class="sxs-lookup"><span data-stu-id="4309f-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="4309f-121">Se la chiave primaria è impostata su un valore, lo stato viene impostato su invariato.</span><span class="sxs-lookup"><span data-stu-id="4309f-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="4309f-122">Se la chiave primaria non è generata dal database, l'entità viene inserita nello stesso stato della radice.</span><span class="sxs-lookup"><span data-stu-id="4309f-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="4309f-123">Inizializzazione del database Code First</span><span class="sxs-lookup"><span data-stu-id="4309f-123">Code First database initialization</span></span>

<span data-ttu-id="4309f-124">**EF6 ha una notevole quantità di magie che esegue per selezionare la connessione al database e inizializzare il database. Di seguito sono riportate alcune di queste regole:**</span><span class="sxs-lookup"><span data-stu-id="4309f-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="4309f-125">Se non viene eseguita alcuna configurazione, in EF6 verrà selezionato un database in SQL Express o nel database locale.</span><span class="sxs-lookup"><span data-stu-id="4309f-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="4309f-126">Se una stringa di connessione con lo stesso nome del contesto si trova nel file `App/Web.config` di applicazioni, verrà utilizzata questa connessione.</span><span class="sxs-lookup"><span data-stu-id="4309f-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="4309f-127">Se il database non esiste, viene creato.</span><span class="sxs-lookup"><span data-stu-id="4309f-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="4309f-128">Se nessuna tabella del modello esiste nel database, lo schema per il modello corrente viene aggiunto al database.</span><span class="sxs-lookup"><span data-stu-id="4309f-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="4309f-129">Se le migrazioni sono abilitate, verranno utilizzate per creare il database.</span><span class="sxs-lookup"><span data-stu-id="4309f-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="4309f-130">Se il database esiste e EF6 lo schema è stato creato in precedenza, viene verificata la compatibilità dello schema con il modello corrente.</span><span class="sxs-lookup"><span data-stu-id="4309f-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="4309f-131">Se il modello è stato modificato dopo la creazione dello schema, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="4309f-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="4309f-132">**EF Core non esegue questa operazione magica.**</span><span class="sxs-lookup"><span data-stu-id="4309f-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="4309f-133">La connessione al database deve essere configurata in modo esplicito nel codice.</span><span class="sxs-lookup"><span data-stu-id="4309f-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="4309f-134">Non viene eseguita alcuna inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="4309f-134">No initialization is performed.</span></span> <span data-ttu-id="4309f-135">Per applicare le `DbContext.Database.Migrate()` migrazioni `DbContext.Database.EnsureCreated()` `EnsureDeleted()` o per creare o eliminare il database senza utilizzare le migrazioni, è necessario utilizzare.</span><span class="sxs-lookup"><span data-stu-id="4309f-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="4309f-136">Convenzione di denominazione della tabella Code First</span><span class="sxs-lookup"><span data-stu-id="4309f-136">Code First table naming convention</span></span>

<span data-ttu-id="4309f-137">EF6 esegue il nome della classe di entità tramite un servizio di pluralismo per calcolare il nome della tabella predefinito a cui è stato eseguito il mapping dell'entità.</span><span class="sxs-lookup"><span data-stu-id="4309f-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="4309f-138">EF Core usa il nome della `DbSet` proprietà in cui l'entità è esposta nel contesto derivato.</span><span class="sxs-lookup"><span data-stu-id="4309f-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="4309f-139">Se l'entità non dispone di una `DbSet` proprietà, viene utilizzato il nome della classe.</span><span class="sxs-lookup"><span data-stu-id="4309f-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
