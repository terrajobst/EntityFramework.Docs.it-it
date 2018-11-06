---
title: Seeding dei dati - EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 791f7afff36aac52fe2ffdc16ab580db22011b99
ms.sourcegitcommit: 082946dcaa1ee5174e692dbfe53adeed40609c6a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51028096"
---
# <a name="data-seeding"></a><span data-ttu-id="f385e-102">Seeding dei dati</span><span class="sxs-lookup"><span data-stu-id="f385e-102">Data Seeding</span></span>

<span data-ttu-id="f385e-103">Seeding dei dati è il processo di popolamento di un database con un set di dati iniziale.</span><span class="sxs-lookup"><span data-stu-id="f385e-103">Data seeding is the process of populating a database with an initial set of data.</span></span>

<span data-ttu-id="f385e-104">Esistono diversi modi a questo scopo in EF Core:</span><span class="sxs-lookup"><span data-stu-id="f385e-104">There are several ways this can be accomplished in EF Core:</span></span>
* <span data-ttu-id="f385e-105">Dati del valore di inizializzazione modello</span><span class="sxs-lookup"><span data-stu-id="f385e-105">Model seed data</span></span>
* <span data-ttu-id="f385e-106">Personalizzazione della migrazione manuale</span><span class="sxs-lookup"><span data-stu-id="f385e-106">Manual migration customization</span></span>
* <span data-ttu-id="f385e-107">Per la logica l'inizializzazione personalizzata</span><span class="sxs-lookup"><span data-stu-id="f385e-107">Custom initialization logic</span></span>

## <a name="model-seed-data"></a><span data-ttu-id="f385e-108">Dati del valore di inizializzazione modello</span><span class="sxs-lookup"><span data-stu-id="f385e-108">Model seed data</span></span>

> [!NOTE]
> <span data-ttu-id="f385e-109">Questa funzionalità è stata introdotta in EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="f385e-109">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="f385e-110">Diversamente da ef6, in EF Core, il seeding dei dati può essere associato a un tipo di entità come parte della configurazione del modello.</span><span class="sxs-lookup"><span data-stu-id="f385e-110">Unlike in EF6, in EF Core, seeding data can be associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="f385e-111">Quindi Entity Framework Core [migrazioni](xref:core/managing-schemas/migrations/index) può calcolare automaticamente quali inserire, aggiornare o eliminare le operazioni devono essere applicate durante l'aggiornamento del database a una nuova versione del modello.</span><span class="sxs-lookup"><span data-stu-id="f385e-111">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

> [!NOTE]
> <span data-ttu-id="f385e-112">Le migrazioni considera solo le modifiche al modello quando si determina quale operazione deve essere eseguita per ottenere i dati di seeding, lo stato desiderato.</span><span class="sxs-lookup"><span data-stu-id="f385e-112">Migrations only considers model changes when determining what operation should be performed to get the seed data into the desired state.</span></span> <span data-ttu-id="f385e-113">In questo modo tutte le modifiche ai dati eseguite di fuori di migrazioni potrebbero andare persi o provocare un errore.</span><span class="sxs-lookup"><span data-stu-id="f385e-113">Thus any changes to the data performed outside of migrations might be lost or cause an error.</span></span>

<span data-ttu-id="f385e-114">Ad esempio, si configurerà i dati di inizializzazione per un `Blog` in `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="f385e-114">As an example, this will configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="f385e-115">Per aggiungere entità contenenti una relazione di valori di chiave esterna devono essere specificate:</span><span class="sxs-lookup"><span data-stu-id="f385e-115">To add entities that have a relationship the foreign key values need to be specified:</span></span>

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="f385e-116">Se il tipo di entità dispone di qualsiasi proprietà in una classe anonima può essere utilizzata per fornire i valori dello stato di ombreggiatura:</span><span class="sxs-lookup"><span data-stu-id="f385e-116">If the entity type has any properties in shadow state an anonymous class can be used to provide the values:</span></span>

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

<span data-ttu-id="f385e-117">Proprietà dell'entità di tipi possono essere inizializzati in modo analogo:</span><span class="sxs-lookup"><span data-stu-id="f385e-117">Owned entity types can be seeded in a similar fashion:</span></span>

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

<span data-ttu-id="f385e-118">Vedere le [progetto di esempio completo](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) per altre informazioni sul contesto.</span><span class="sxs-lookup"><span data-stu-id="f385e-118">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) for more context.</span></span>

<span data-ttu-id="f385e-119">Dopo aver aggiunto i dati al modello [migrazioni](xref:core/managing-schemas/migrations/index) deve essere usato per applicare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="f385e-119">Once the data has been added to the model, [migrations](xref:core/managing-schemas/migrations/index) should be used to apply the changes.</span></span>

> [!TIP]
> <span data-ttu-id="f385e-120">Se è necessario applicare le migrazioni come parte di una distribuzione automatizzata è possibile [creare uno script SQL](xref:core/managing-schemas/migrations/index#generate-sql-scripts) che può essere visualizzato in anteprima prima dell'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f385e-120">If you need to apply migrations as part of an automated deployment you can [create a SQL script](xref:core/managing-schemas/migrations/index#generate-sql-scripts) that can be previewed before execution.</span></span>

<span data-ttu-id="f385e-121">In alternativa, è possibile usare `context.Database.EnsureCreated()` per creare un nuovo database che contiene i dati di seeding, ad esempio per un database di test o quando si utilizza il provider in memoria o in qualsiasi database di non-relazione.</span><span class="sxs-lookup"><span data-stu-id="f385e-121">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider or any non-relation database.</span></span> <span data-ttu-id="f385e-122">Si noti che se il database esiste già, `EnsureCreated()` né aggiornerà lo schema, né i dati di seeding del database.</span><span class="sxs-lookup"><span data-stu-id="f385e-122">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span> <span data-ttu-id="f385e-123">Per i database relazionali non devono chiamare `EnsureCreated()` se si prevede di usare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="f385e-123">For relational databases you shouldn't call `EnsureCreated()` if you plan to use Migrations.</span></span>

<span data-ttu-id="f385e-124">Questo tipo di dati di seeding viene gestito mediante le migrazioni e lo script per aggiornare i dati che si trova già nel database deve essere generato senza stabilire la connessione al database.</span><span class="sxs-lookup"><span data-stu-id="f385e-124">This type of seed data is managed by migrations and the script to update the data that's already in the database needs to be generated without connecting to the database.</span></span> <span data-ttu-id="f385e-125">Ciò impone alcune limitazioni:</span><span class="sxs-lookup"><span data-stu-id="f385e-125">This imposes some restrictions:</span></span>
* <span data-ttu-id="f385e-126">Il valore della chiave primaria deve essere specificata anche se in genere generato dal database.</span><span class="sxs-lookup"><span data-stu-id="f385e-126">The primary key value needs to be specified even if it's usually generated by the database.</span></span> <span data-ttu-id="f385e-127">Da utilizzare per rilevare le modifiche dei dati tra le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="f385e-127">It will be used to detect data changes between migrations.</span></span>
* <span data-ttu-id="f385e-128">In precedenza verranno rimosso dei dati di seeding se la chiave primaria viene modificata in alcun modo.</span><span class="sxs-lookup"><span data-stu-id="f385e-128">Previously seeded data will be removed if the primary key is changed in any way.</span></span>

<span data-ttu-id="f385e-129">Pertanto questa funzionalità è particolarmente utile per i dati statici che non sono prevista la modifica di fuori di migrazioni e non dipendono da altre operazioni nel database, ad esempio i codici postali ZIP.</span><span class="sxs-lookup"><span data-stu-id="f385e-129">Therefore this feature is most useful for static data that's not expected to change outside of migrations and does not depend on anything else in the database, for example ZIP codes.</span></span>

<span data-ttu-id="f385e-130">Se lo scenario include i seguenti, è consigliabile usare la logica di inizializzazione personalizzato descritta nell'ultima sezione:</span><span class="sxs-lookup"><span data-stu-id="f385e-130">If your scenario includes any of the following it is recommended to use custom initialization logic described in the last section:</span></span>
* <span data-ttu-id="f385e-131">Dati temporanei per il test</span><span class="sxs-lookup"><span data-stu-id="f385e-131">Temporary data for testing</span></span>
* <span data-ttu-id="f385e-132">Dati che dipende dallo stato del database</span><span class="sxs-lookup"><span data-stu-id="f385e-132">Data that depends on database state</span></span>
* <span data-ttu-id="f385e-133">Dati che richiede valori di chiave generato dal database, incluse le entità che usano le chiavi alternative come identità</span><span class="sxs-lookup"><span data-stu-id="f385e-133">Data that needs key values to be generated by the database, including entities that use alternate keys as the identity</span></span>
* <span data-ttu-id="f385e-134">I dati che richiede la trasformazione personalizzati (che non è gestita da [valore conversioni](xref:core/modeling/value-conversions)), ad esempio alcuni hashing della password</span><span class="sxs-lookup"><span data-stu-id="f385e-134">Data that requires custom transformation (that is not handled by [value conversions](xref:core/modeling/value-conversions)), such as some password hashing</span></span>
* <span data-ttu-id="f385e-135">Dati che richiedono le chiamate all'API esterna, ad esempio la creazione di utenti e ruoli di ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="f385e-135">Data that requires calls to external API, such as ASP.NET Core Identity roles and users creation</span></span>

## <a name="manual-migration-customization"></a><span data-ttu-id="f385e-136">Personalizzazione della migrazione manuale</span><span class="sxs-lookup"><span data-stu-id="f385e-136">Manual migration customization</span></span>

<span data-ttu-id="f385e-137">Quando viene aggiunta le modifiche ai dati specificati con una migrazione `HasData` vengono trasformate in chiamate a `InsertData()`, `UpdateData()`, e `DeleteData()`.</span><span class="sxs-lookup"><span data-stu-id="f385e-137">When a migration is added the changes to the data specified with `HasData` are transformed to calls to `InsertData()`, `UpdateData()`, and `DeleteData()`.</span></span> <span data-ttu-id="f385e-138">Un modo per aggirare alcune delle limitazioni della `HasData` consiste nell'aggiungere manualmente queste chiamate oppure [operazioni personalizzate](xref:core/managing-schemas/migrations/operations) per la migrazione invece.</span><span class="sxs-lookup"><span data-stu-id="f385e-138">One way of working around some of the limitations of `HasData` is to manually add these calls or [custom operations](xref:core/managing-schemas/migrations/operations) to the migration instead.</span></span>

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a><span data-ttu-id="f385e-139">Per la logica l'inizializzazione personalizzata</span><span class="sxs-lookup"><span data-stu-id="f385e-139">Custom initialization logic</span></span>

<span data-ttu-id="f385e-140">Un modo semplice e potente per eseguire il seeding dei dati consiste nell'utilizzare [ `DbContext.SaveChanges()` ](xref:core/saving/index) prima che l'applicazione principale per la logica inizia l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f385e-140">A straightforward and powerful way to perform data seeding is to use [`DbContext.SaveChanges()`](xref:core/saving/index) before the main application logic begins execution.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> <span data-ttu-id="f385e-141">Il codice di seeding non deve far parte dell'esecuzione normale app perché ciò può causare problemi di concorrenza quando sono in esecuzione e l'app con l'autorizzazione per modificare lo schema del database può richiedere anche più istanze.</span><span class="sxs-lookup"><span data-stu-id="f385e-141">The seeding code should not be part of the normal app execution as this can cause concurrency issues when multiple instances are running and would also require the app having permission to modify the database schema.</span></span>

<span data-ttu-id="f385e-142">In base ai vincoli della distribuzione è possibile eseguire il codice di inizializzazione in modi diversi:</span><span class="sxs-lookup"><span data-stu-id="f385e-142">Depending on the constraints of your deployment the initialization code can be executed in different ways:</span></span>
* <span data-ttu-id="f385e-143">Esecuzione locale dell'app di inizializzazione</span><span class="sxs-lookup"><span data-stu-id="f385e-143">Running the initialization app locally</span></span>
* <span data-ttu-id="f385e-144">Distribuzione dell'app di inizializzazione con l'app principale, chiamata di routine di inizializzazione e la disabilitazione o la rimozione dell'app di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="f385e-144">Deploying the initialization app with the main app, invoking the initialization routine and disabling or removing the initialization app.</span></span>

<span data-ttu-id="f385e-145">Ciò in genere può essere automatizzata mediante [profili di pubblicazione](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="f385e-145">This can usually be automated by using [publish profiles](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span></span>
