---
title: Personalizzare la tabella di cronologia migrazioni - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: eb19f367611a86f685557a6741a5f2f0bad6b718
ms.sourcegitcommit: e66745c9f91258b2cacf5ff263141be3cba4b09e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/06/2019
ms.locfileid: "54058747"
---
# <a name="customizing-the-migrations-history-table"></a><span data-ttu-id="235b0-102">La tabella di cronologia le migrazioni di personalizzazione</span><span class="sxs-lookup"><span data-stu-id="235b0-102">Customizing the migrations history table</span></span>
> [!NOTE]
> <span data-ttu-id="235b0-103">**Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="235b0-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="235b0-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="235b0-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

> [!NOTE]
> <span data-ttu-id="235b0-105">Questo articolo presuppone che l'utente sappia usare migrazioni Code First in scenari di base.</span><span class="sxs-lookup"><span data-stu-id="235b0-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="235b0-106">Se in caso contrario, allora sarà necessario leggere [migrazioni Code First](~/ef6/modeling/code-first/migrations/index.md) prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="235b0-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="what-is-migrations-history-table"></a><span data-ttu-id="235b0-107">Che cos'è la tabella di cronologia migrazioni?</span><span class="sxs-lookup"><span data-stu-id="235b0-107">What is Migrations History Table?</span></span>

<span data-ttu-id="235b0-108">Tabella di cronologia migrazioni è una tabella utilizzata dal migrazioni Code First per archiviare informazioni dettagliate sulle migrazioni applicate al database.</span><span class="sxs-lookup"><span data-stu-id="235b0-108">Migrations history table is a table used by Code First Migrations to store details about migrations applied to the database.</span></span> <span data-ttu-id="235b0-109">Per impostazione predefinita è il nome della tabella nel database \_ \_MigrationHistory e viene creato quando si applica la prima migrazione al database.</span><span class="sxs-lookup"><span data-stu-id="235b0-109">By default the name of the table in the database is \_\_MigrationHistory and it is created when applying the first migration to the database.</span></span> <span data-ttu-id="235b0-110">In Entity Framework 5 questa tabella è una tabella di sistema se l'applicazione utilizzata database Microsoft Sql Server.</span><span class="sxs-lookup"><span data-stu-id="235b0-110">In Entity Framework 5 this table was a system table if the application used Microsoft Sql Server database.</span></span> <span data-ttu-id="235b0-111">Questo è stato modificato in Entity Framework 6 tuttavia e la tabella di cronologia migrazioni non è più contrassegnata una tabella di sistema.</span><span class="sxs-lookup"><span data-stu-id="235b0-111">This has changed in Entity Framework 6 however and the migrations history table is no longer marked a system table.</span></span>

## <a name="why-customize-migrations-history-table"></a><span data-ttu-id="235b0-112">Il motivo per cui personalizzare la tabella di cronologia migrazioni?</span><span class="sxs-lookup"><span data-stu-id="235b0-112">Why customize Migrations History Table?</span></span>

<span data-ttu-id="235b0-113">Tabella di cronologia migrazioni dovrebbe essere utilizzate esclusivamente per le migrazioni Code First e può essere modificato manualmente può interrompere le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="235b0-113">Migrations history table is supposed to be used solely by Code First Migrations and changing it manually can break migrations.</span></span> <span data-ttu-id="235b0-114">Tuttavia in alcuni casi la configurazione predefinita non è appropriata e la tabella deve essere personalizzato, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="235b0-114">However sometimes the default configuration is not suitable and the table needs to be customized, for instance:</span></span>

-   <span data-ttu-id="235b0-115">È necessario modificare i nomi e/o i facet di colonne per abilitare un 3<sup>rd</sup> provider le migrazioni di terze parti</span><span class="sxs-lookup"><span data-stu-id="235b0-115">You need to change names and/or facets of the columns to enable a 3<sup>rd</sup> party Migrations provider</span></span>
-   <span data-ttu-id="235b0-116">Si desidera modificare il nome della tabella</span><span class="sxs-lookup"><span data-stu-id="235b0-116">You want to change the name of the table</span></span>
-   <span data-ttu-id="235b0-117">È necessario usare uno schema diverso per il \_ \_MigrationHistory tabella</span><span class="sxs-lookup"><span data-stu-id="235b0-117">You need to use a non-default schema for the \_\_MigrationHistory table</span></span>
-   <span data-ttu-id="235b0-118">È necessario archiviare dati aggiuntivi per una determinata versione del contesto e pertanto è necessario aggiungere un'altra colonna alla tabella</span><span class="sxs-lookup"><span data-stu-id="235b0-118">You need to store additional data for a given version of the context and therefore you need to add an additional column to the table</span></span>

## <a name="words-of-precaution"></a><span data-ttu-id="235b0-119">Parole di precauzione</span><span class="sxs-lookup"><span data-stu-id="235b0-119">Words of precaution</span></span>

<span data-ttu-id="235b0-120">Modificare la tabella di cronologia della migrazione è potente, ma è necessario prestare attenzione a non esagerare.</span><span class="sxs-lookup"><span data-stu-id="235b0-120">Changing the migration history table is powerful but you need to be careful to not overdo it.</span></span> <span data-ttu-id="235b0-121">Runtime di Entity Framework attualmente non verifica se la tabella di cronologia migrazioni personalizzato è compatibile con il runtime.</span><span class="sxs-lookup"><span data-stu-id="235b0-121">EF runtime currently does not check whether the customized migrations history table is compatible with the runtime.</span></span> <span data-ttu-id="235b0-122">Se non è l'applicazione potrebbe interrompersi durante l'esecuzione o si comportano in modo imprevedibile.</span><span class="sxs-lookup"><span data-stu-id="235b0-122">If it is not your application may break at runtime or behave in unpredictable ways.</span></span> <span data-ttu-id="235b0-123">Questo è ancora più importante se si usano più contesti per ogni database nel qual caso più contesti possono usare la stessa tabella di cronologia di migrazione per archiviare le informazioni sulle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="235b0-123">This is even more important if you use multiple contexts per database in which case multiple contexts can use the same migration history table to store information about migrations.</span></span>

## <a name="how-to-customize-migrations-history-table"></a><span data-ttu-id="235b0-124">Come personalizzare la tabella di cronologia migrazioni?</span><span class="sxs-lookup"><span data-stu-id="235b0-124">How to customize Migrations History Table?</span></span>

<span data-ttu-id="235b0-125">Prima di iniziare è necessario sapere che è possibile personalizzare la tabella di cronologia migrazioni solo prima di applicare la prima migrazione.</span><span class="sxs-lookup"><span data-stu-id="235b0-125">Before you start you need to know that you can customize the migrations history table only before you apply the first migration.</span></span> <span data-ttu-id="235b0-126">A questo punto, al codice.</span><span class="sxs-lookup"><span data-stu-id="235b0-126">Now, to the code.</span></span>

<span data-ttu-id="235b0-127">In primo luogo, è necessario creare una classe derivata dalla classe System.Data.Entity.Migrations.History.HistoryContext.</span><span class="sxs-lookup"><span data-stu-id="235b0-127">First, you will need to create a class derived from System.Data.Entity.Migrations.History.HistoryContext class.</span></span> <span data-ttu-id="235b0-128">La classe HistoryContext è derivata dalla classe DbContext in modo che la configurazione della tabella di cronologia migrazioni è molto simile alla configurazione dei modelli di Entity Framework con l'API fluent.</span><span class="sxs-lookup"><span data-stu-id="235b0-128">The HistoryContext class is derived from the DbContext class so configuring the migrations history table is very similar to configuring EF models with fluent API.</span></span> <span data-ttu-id="235b0-129">È sufficiente eseguire l'override del metodo OnModelCreating e usare l'API fluent per configurare la tabella.</span><span class="sxs-lookup"><span data-stu-id="235b0-129">You just need to override the OnModelCreating method and use fluent API to configure the table.</span></span>

>[!NOTE]
> <span data-ttu-id="235b0-130">In genere quando si configurano modelli EF non occorre chiamare base. Orderingcontext.cs dal metodo OnModelCreating sottoposto a override in quanto il DbContext.OnModelCreating() contiene un corpo vuoto.</span><span class="sxs-lookup"><span data-stu-id="235b0-130">Typically when you configure EF models you don’t need to call base.OnModelCreating() from the overridden OnModelCreating method since the DbContext.OnModelCreating() has empty body.</span></span> <span data-ttu-id="235b0-131">Ciò non avviene quando si configura la tabella di cronologia delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="235b0-131">This is not the case when configuring the migrations history table.</span></span> <span data-ttu-id="235b0-132">In questo caso la prima operazione da eseguire nell'override Orderingcontext.cs è effettivamente chiamare base. Orderingcontext.cs.</span><span class="sxs-lookup"><span data-stu-id="235b0-132">In this case the first thing to do in your OnModelCreating() override is to actually call base.OnModelCreating().</span></span> <span data-ttu-id="235b0-133">La tabella di cronologia migrazioni verrà configurato in modo predefinito che quindi possibile perfezionare nel metodo di overriding.</span><span class="sxs-lookup"><span data-stu-id="235b0-133">This will configure the migrations history table in the default way which you then tweak in the overriding method.</span></span>

<span data-ttu-id="235b0-134">Si supponga di che voler rinominare la tabella di cronologia le migrazioni e inserirlo in uno schema personalizzato denominato "admin".</span><span class="sxs-lookup"><span data-stu-id="235b0-134">Let’s say you want to rename the migrations history table and put it to a custom schema called “admin”.</span></span> <span data-ttu-id="235b0-135">Anche l'amministratore del database desidera è possibile rinominare la colonna MigrationId migrazione\_ID.</span><span class="sxs-lookup"><span data-stu-id="235b0-135">In addition your DBA would like you to rename the MigrationId column to Migration\_ID.</span></span> <span data-ttu-id="235b0-136"> È possibile ottenere questo risultato creando la seguente classe derivata da HistoryContext:</span><span class="sxs-lookup"><span data-stu-id="235b0-136"> You could achieve this by creating the following class derived from HistoryContext:</span></span>

``` csharp
    using System.Data.Common;
    using System.Data.Entity;
    using System.Data.Entity.Migrations.History;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class MyHistoryContext : HistoryContext
        {
            public MyHistoryContext(DbConnection dbConnection, string defaultSchema)
                : base(dbConnection, defaultSchema)
            {
            }

            protected override void OnModelCreating(DbModelBuilder modelBuilder)
            {
                base.OnModelCreating(modelBuilder);
                modelBuilder.Entity<HistoryRow>().ToTable(tableName: "MigrationHistory", schemaName: "admin");
                modelBuilder.Entity<HistoryRow>().Property(p => p.MigrationId).HasColumnName("Migration_ID");
            }
        }
    }
```

<span data-ttu-id="235b0-137">Una volta pronto il HistoryContext personalizzato è necessario apportare EF consapevole registrandolo tramite [configurazione basata su codice](https://msdn.com/data/jj680699):</span><span class="sxs-lookup"><span data-stu-id="235b0-137">Once your custom HistoryContext is ready you need to make EF aware of it by registering it via [code-based configuration](https://msdn.com/data/jj680699):</span></span>

``` csharp
    using System.Data.Entity;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class ModelConfiguration : DbConfiguration
        {
            public ModelConfiguration()
            {
                this.SetHistoryContext("System.Data.SqlClient",
                    (connection, defaultSchema) => new MyHistoryContext(connection, defaultSchema));
            }
        }
    }
```

<span data-ttu-id="235b0-138">È praticamente tutto.</span><span class="sxs-lookup"><span data-stu-id="235b0-138">That’s pretty much it.</span></span> <span data-ttu-id="235b0-139">A questo punto è possibile passare alla Console di gestione pacchetti, Enable-Migrations, Add-Migration e infine Update-Database.</span><span class="sxs-lookup"><span data-stu-id="235b0-139">Now you can go to the Package Manager Console, Enable-Migrations, Add-Migration and finally Update-Database.</span></span> <span data-ttu-id="235b0-140">Dovrebbe essere generato in aggiunta al database di una tabella di cronologia migrazioni configurata in base ai dettagli specificati nella classe HistoryContext derivato.</span><span class="sxs-lookup"><span data-stu-id="235b0-140">This should result in adding to the database a migrations history table configured according to the details you specified in your HistoryContext derived class.</span></span>

![Database](~/ef6/media/database.png)
