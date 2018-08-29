---
title: 'Conversione da EF6 a EF Core: convalidare i requisiti'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: fd378c72a3c8de4a441861b3c52b94eba5f7e5b4
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993440"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="afaae-102">Prima della conversione da EF6 a EF Core: convalidare i requisiti dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="afaae-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="afaae-103">Prima di iniziare il processo di porting è importante convalidare che EF Core soddisfi i requisiti di accesso di dati per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="afaae-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="afaae-104">Funzionalità mancanti</span><span class="sxs-lookup"><span data-stu-id="afaae-104">Missing features</span></span>

<span data-ttu-id="afaae-105">Assicurarsi che EF Core include tutte le funzionalità da usare nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="afaae-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="afaae-106">Visualizzare [confronto tra le funzionalità](../features.md) per un confronto dettagliato delle modalità con cui confrontare di funzionalità disponibili in EF Core per Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="afaae-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="afaae-107">Se tutte le funzionalità necessarie sono mancante, verificare che è possibile compensare la mancanza di queste funzionalità prima della conversione a EF Core.</span><span class="sxs-lookup"><span data-stu-id="afaae-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="afaae-108">Modifiche del comportamento</span><span class="sxs-lookup"><span data-stu-id="afaae-108">Behavior changes</span></span>

<span data-ttu-id="afaae-109">Si tratta di un elenco non esaustivo di alcuni cambiamenti nel comportamento tra EF6 ed EF Core.</span><span class="sxs-lookup"><span data-stu-id="afaae-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="afaae-110">È importante tenere presente quanto segue come la porta dell'applicazione perché queste potrebbero cambiare il modo l'applicazione si comporta, ma non verrà visualizzata come errori di compilazione dopo lo scambio in EF Core.</span><span class="sxs-lookup"><span data-stu-id="afaae-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="afaae-111">Comportamento DbSet.Add/Attach e un grafo</span><span class="sxs-lookup"><span data-stu-id="afaae-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="afaae-112">In EF6, chiamata `DbSet.Add()` ai risultati entità in una ricerca ricorsiva per tutte le entità a cui fa riferimento la proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="afaae-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="afaae-113">Qualsiasi entità che vengono individuati e non sono già rilevato dal contesto, sono anche essere contrassegnato come aggiunto.</span><span class="sxs-lookup"><span data-stu-id="afaae-113">Any entities that are found, and are not already tracked by the context, are also be marked as added.</span></span> <span data-ttu-id="afaae-114">`DbSet.Attach()` si comporta, ad eccezione del fatto tutte le entità sono contrassegnate come non modificato.</span><span class="sxs-lookup"><span data-stu-id="afaae-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="afaae-115">**EF Core esegue una ricerca ricorsiva simile, ma con alcune regole leggermente diverse.**</span><span class="sxs-lookup"><span data-stu-id="afaae-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="afaae-116">L'entità radice è sempre nello stato richiesto (aggiunte per `DbSet.Add` e invariato per `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="afaae-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="afaae-117">**Per le entità che si verificano durante la ricerca ricorsiva delle proprietà di navigazione:**</span><span class="sxs-lookup"><span data-stu-id="afaae-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="afaae-118">**Se la chiave primaria dell'entità archivio generato**</span><span class="sxs-lookup"><span data-stu-id="afaae-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="afaae-119">Se la chiave primaria non è impostata su un valore, lo stato viene impostato a aggiunto.</span><span class="sxs-lookup"><span data-stu-id="afaae-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="afaae-120">Il valore della chiave primaria viene considerato "non impostato" Se viene assegnato il valore predefinito CLR per il tipo di proprietà (ad esempio, `0` per `int`, `null` per `string`e così via.).</span><span class="sxs-lookup"><span data-stu-id="afaae-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="afaae-121">Se la chiave primaria è impostata su un valore, lo stato è impostato su non modificato.</span><span class="sxs-lookup"><span data-stu-id="afaae-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="afaae-122">Se la chiave primaria non è generato dal database, l'entità viene inserita nello stesso stato come radice.</span><span class="sxs-lookup"><span data-stu-id="afaae-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="afaae-123">Inizializzazione del database prima del codice</span><span class="sxs-lookup"><span data-stu-id="afaae-123">Code First database initialization</span></span>

<span data-ttu-id="afaae-124">**EF6 è una quantità significativa di magic che esegue tutto selezionando la connessione al database e l'inizializzazione del database. Alcune di queste regole includono:**</span><span class="sxs-lookup"><span data-stu-id="afaae-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="afaae-125">Se non viene eseguita alcuna configurazione, Entity Framework 6 selezionerà un database in SQL Express o LocalDb.</span><span class="sxs-lookup"><span data-stu-id="afaae-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="afaae-126">Se è una stringa di connessione con lo stesso nome di contesto nelle applicazioni `App/Web.config` file, verrà utilizzata questa connessione.</span><span class="sxs-lookup"><span data-stu-id="afaae-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="afaae-127">Se il database non esiste, viene creato.</span><span class="sxs-lookup"><span data-stu-id="afaae-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="afaae-128">Se nessuna delle tabelle dal modello esiste nel database, lo schema per il modello corrente viene aggiunto al database.</span><span class="sxs-lookup"><span data-stu-id="afaae-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="afaae-129">Se sono abilitate le migrazioni, quindi vengono utilizzati per creare il database.</span><span class="sxs-lookup"><span data-stu-id="afaae-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="afaae-130">Se il database esista ed EF6 è stata creata in precedenza lo schema, lo schema viene verificato la compatibilità con il modello corrente.</span><span class="sxs-lookup"><span data-stu-id="afaae-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="afaae-131">Il modello è stato modificato dopo lo schema è stato creato, viene generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="afaae-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="afaae-132">**EF Core non esegue alcun questo speciale.**</span><span class="sxs-lookup"><span data-stu-id="afaae-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="afaae-133">La connessione al database deve essere configurata in modo esplicito nel codice.</span><span class="sxs-lookup"><span data-stu-id="afaae-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="afaae-134">Viene eseguita alcuna inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="afaae-134">No initialization is performed.</span></span> <span data-ttu-id="afaae-135">È necessario utilizzare `DbContext.Database.Migrate()` per applicare le migrazioni (o `DbContext.Database.EnsureCreated()` e `EnsureDeleted()` per creare o eliminare il database senza usare le migrazioni).</span><span class="sxs-lookup"><span data-stu-id="afaae-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="afaae-136">Prima tabella codice convenzione di denominazione</span><span class="sxs-lookup"><span data-stu-id="afaae-136">Code First table naming convention</span></span>

<span data-ttu-id="afaae-137">EF6 viene eseguito il nome di classe di entità tramite un servizio di pluralizzazione per la quale calcolare il nome di tabella predefinito che l'entità è mappata a.</span><span class="sxs-lookup"><span data-stu-id="afaae-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="afaae-138">EF Core Usa il nome del `DbSet` proprietà dell'entità esposta nel contesto derivato.</span><span class="sxs-lookup"><span data-stu-id="afaae-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="afaae-139">Se l'entità non dispone di un `DbSet` proprietà, quindi il nome della classe viene utilizzata.</span><span class="sxs-lookup"><span data-stu-id="afaae-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
