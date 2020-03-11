---
title: Personalizzazione della tabella di cronologia delle migrazioni-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: eb19f367611a86f685557a6741a5f2f0bad6b718
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418979"
---
# <a name="customizing-the-migrations-history-table"></a><span data-ttu-id="ec060-102">Personalizzazione della tabella di cronologia delle migrazioni</span><span class="sxs-lookup"><span data-stu-id="ec060-102">Customizing the migrations history table</span></span>
> [!NOTE]
> <span data-ttu-id="ec060-103">**Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="ec060-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="ec060-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="ec060-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

> [!NOTE]
> <span data-ttu-id="ec060-105">Questo articolo presuppone che l'utente sappia come usare Migrazioni Code First negli scenari di base.</span><span class="sxs-lookup"><span data-stu-id="ec060-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="ec060-106">In caso contrario, sarà necessario leggere [migrazioni Code First](~/ef6/modeling/code-first/migrations/index.md) prima di continuare.</span><span class="sxs-lookup"><span data-stu-id="ec060-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="what-is-migrations-history-table"></a><span data-ttu-id="ec060-107">Che cos'è la tabella di cronologia delle migrazioni?</span><span class="sxs-lookup"><span data-stu-id="ec060-107">What is Migrations History Table?</span></span>

<span data-ttu-id="ec060-108">Tabella di cronologia migrazioni è una tabella utilizzata da Migrazioni Code First per archiviare i dettagli sulle migrazioni applicate al database.</span><span class="sxs-lookup"><span data-stu-id="ec060-108">Migrations history table is a table used by Code First Migrations to store details about migrations applied to the database.</span></span> <span data-ttu-id="ec060-109">Per impostazione predefinita, il nome della tabella nel database è \_\_MigrationHistory e viene creato durante l'applicazione della prima migrazione al database.</span><span class="sxs-lookup"><span data-stu-id="ec060-109">By default the name of the table in the database is \_\_MigrationHistory and it is created when applying the first migration to the database.</span></span> <span data-ttu-id="ec060-110">In Entity Framework 5 questa tabella è una tabella di sistema se l'applicazione utilizza il database di Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ec060-110">In Entity Framework 5 this table was a system table if the application used Microsoft Sql Server database.</span></span> <span data-ttu-id="ec060-111">Questa operazione è stata modificata in Entity Framework 6, ma la tabella di cronologia delle migrazioni non è più contrassegnata come tabella di sistema.</span><span class="sxs-lookup"><span data-stu-id="ec060-111">This has changed in Entity Framework 6 however and the migrations history table is no longer marked a system table.</span></span>

## <a name="why-customize-migrations-history-table"></a><span data-ttu-id="ec060-112">Perché personalizzare la tabella di cronologia delle migrazioni?</span><span class="sxs-lookup"><span data-stu-id="ec060-112">Why customize Migrations History Table?</span></span>

<span data-ttu-id="ec060-113">La tabella di cronologia delle migrazioni dovrebbe essere utilizzata esclusivamente da Migrazioni Code First e modificarla manualmente può suddividere le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="ec060-113">Migrations history table is supposed to be used solely by Code First Migrations and changing it manually can break migrations.</span></span> <span data-ttu-id="ec060-114">Tuttavia, a volte la configurazione predefinita non è adatta e la tabella deve essere personalizzata, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="ec060-114">However sometimes the default configuration is not suitable and the table needs to be customized, for instance:</span></span>

-   <span data-ttu-id="ec060-115">Per abilitare un provider di migrazioni<sup>di terze</sup> parti, è necessario modificare i nomi e/o i facet delle colonne</span><span class="sxs-lookup"><span data-stu-id="ec060-115">You need to change names and/or facets of the columns to enable a 3<sup>rd</sup> party Migrations provider</span></span>
-   <span data-ttu-id="ec060-116">Si desidera modificare il nome della tabella</span><span class="sxs-lookup"><span data-stu-id="ec060-116">You want to change the name of the table</span></span>
-   <span data-ttu-id="ec060-117">È necessario usare uno schema non predefinito per la tabella \_\_MigrationHistory</span><span class="sxs-lookup"><span data-stu-id="ec060-117">You need to use a non-default schema for the \_\_MigrationHistory table</span></span>
-   <span data-ttu-id="ec060-118">È necessario archiviare dati aggiuntivi per una determinata versione del contesto e pertanto è necessario aggiungere una colonna aggiuntiva alla tabella.</span><span class="sxs-lookup"><span data-stu-id="ec060-118">You need to store additional data for a given version of the context and therefore you need to add an additional column to the table</span></span>

## <a name="words-of-precaution"></a><span data-ttu-id="ec060-119">Parole di precauzione</span><span class="sxs-lookup"><span data-stu-id="ec060-119">Words of precaution</span></span>

<span data-ttu-id="ec060-120">Modificare la tabella di cronologia della migrazione è potente, ma è necessario prestare attenzione a non esagerare.</span><span class="sxs-lookup"><span data-stu-id="ec060-120">Changing the migration history table is powerful but you need to be careful to not overdo it.</span></span> <span data-ttu-id="ec060-121">EF Runtime attualmente non controlla se la tabella di cronologia delle migrazioni personalizzate è compatibile con il Runtime.</span><span class="sxs-lookup"><span data-stu-id="ec060-121">EF runtime currently does not check whether the customized migrations history table is compatible with the runtime.</span></span> <span data-ttu-id="ec060-122">In caso contrario, l'applicazione potrebbe interrompersi in fase di esecuzione o comportarsi in modi imprevedibili.</span><span class="sxs-lookup"><span data-stu-id="ec060-122">If it is not your application may break at runtime or behave in unpredictable ways.</span></span> <span data-ttu-id="ec060-123">Questa operazione è ancora più importante se si usano più contesti per database, nel qual caso più contesti possono usare la stessa tabella di cronologia della migrazione per archiviare le informazioni sulle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="ec060-123">This is even more important if you use multiple contexts per database in which case multiple contexts can use the same migration history table to store information about migrations.</span></span>

## <a name="how-to-customize-migrations-history-table"></a><span data-ttu-id="ec060-124">Come personalizzare la tabella di cronologia delle migrazioni</span><span class="sxs-lookup"><span data-stu-id="ec060-124">How to customize Migrations History Table?</span></span>

<span data-ttu-id="ec060-125">Prima di iniziare, è necessario tenere presente che è possibile personalizzare la tabella di cronologia delle migrazioni solo prima di applicare la prima migrazione.</span><span class="sxs-lookup"><span data-stu-id="ec060-125">Before you start you need to know that you can customize the migrations history table only before you apply the first migration.</span></span> <span data-ttu-id="ec060-126">A questo punto, al codice.</span><span class="sxs-lookup"><span data-stu-id="ec060-126">Now, to the code.</span></span>

<span data-ttu-id="ec060-127">In primo luogo, sarà necessario creare una classe derivata dalla classe System. Data. Entity. Migrations. History. HistoryContext.</span><span class="sxs-lookup"><span data-stu-id="ec060-127">First, you will need to create a class derived from System.Data.Entity.Migrations.History.HistoryContext class.</span></span> <span data-ttu-id="ec060-128">La classe HistoryContext è derivata dalla classe DbContext, quindi la configurazione della tabella di cronologia delle migrazioni è molto simile alla configurazione dei modelli EF con l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="ec060-128">The HistoryContext class is derived from the DbContext class so configuring the migrations history table is very similar to configuring EF models with fluent API.</span></span> <span data-ttu-id="ec060-129">È sufficiente eseguire l'override del metodo OnModelCreating e usare l'API Fluent per configurare la tabella.</span><span class="sxs-lookup"><span data-stu-id="ec060-129">You just need to override the OnModelCreating method and use fluent API to configure the table.</span></span>

>[!NOTE]
> <span data-ttu-id="ec060-130">In genere, quando si configurano i modelli EF non è necessario chiamare base. OnModelCreating () dal metodo OnModelCreating sottoposto a override poiché DbContext. OnModelCreating () ha un corpo vuoto.</span><span class="sxs-lookup"><span data-stu-id="ec060-130">Typically when you configure EF models you don’t need to call base.OnModelCreating() from the overridden OnModelCreating method since the DbContext.OnModelCreating() has empty body.</span></span> <span data-ttu-id="ec060-131">Questa situazione non si verifica quando si configura la tabella di cronologia delle migrazioni.</span><span class="sxs-lookup"><span data-stu-id="ec060-131">This is not the case when configuring the migrations history table.</span></span> <span data-ttu-id="ec060-132">In questo caso, la prima operazione da eseguire nell'override di OnModelCreating () consiste nell'eseguire effettivamente la chiamata di base. OnModelCreating ().</span><span class="sxs-lookup"><span data-stu-id="ec060-132">In this case the first thing to do in your OnModelCreating() override is to actually call base.OnModelCreating().</span></span> <span data-ttu-id="ec060-133">Verrà configurata la tabella di cronologia delle migrazioni con la modalità predefinita, che verrà quindi modificata nel metodo di override.</span><span class="sxs-lookup"><span data-stu-id="ec060-133">This will configure the migrations history table in the default way which you then tweak in the overriding method.</span></span>

<span data-ttu-id="ec060-134">Si noti che si vuole rinominare la tabella di cronologia delle migrazioni e inserirla in uno schema personalizzato denominato "admin".</span><span class="sxs-lookup"><span data-stu-id="ec060-134">Let’s say you want to rename the migrations history table and put it to a custom schema called “admin”.</span></span> <span data-ttu-id="ec060-135">Inoltre, l'amministratore di database desidera rinominare la colonna MigrationId in Migration\_ID.</span><span class="sxs-lookup"><span data-stu-id="ec060-135">In addition your DBA would like you to rename the MigrationId column to Migration\_ID.</span></span> <span data-ttu-id="ec060-136"> È possibile ottenere questo risultato creando la classe seguente derivata da HistoryContext:</span><span class="sxs-lookup"><span data-stu-id="ec060-136"> You could achieve this by creating the following class derived from HistoryContext:</span></span>

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

<span data-ttu-id="ec060-137">Quando il HistoryContext personalizzato è pronto, è necessario fare in modo che EF lo renda conto mediante la registrazione tramite la [configurazione basata su codice](https://msdn.com/data/jj680699):</span><span class="sxs-lookup"><span data-stu-id="ec060-137">Once your custom HistoryContext is ready you need to make EF aware of it by registering it via [code-based configuration](https://msdn.com/data/jj680699):</span></span>

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

<span data-ttu-id="ec060-138">Questo è molto simile.</span><span class="sxs-lookup"><span data-stu-id="ec060-138">That’s pretty much it.</span></span> <span data-ttu-id="ec060-139">A questo punto è possibile passare alla console di gestione pacchetti, abilitare-Migrations, Add-Migration e infine Update-database.</span><span class="sxs-lookup"><span data-stu-id="ec060-139">Now you can go to the Package Manager Console, Enable-Migrations, Add-Migration and finally Update-Database.</span></span> <span data-ttu-id="ec060-140">Ciò comporta l'aggiunta al database di una tabella di cronologia delle migrazioni configurata in base ai dettagli specificati nella classe derivata HistoryContext.</span><span class="sxs-lookup"><span data-stu-id="ec060-140">This should result in adding to the database a migrations history table configured according to the details you specified in your HistoryContext derived class.</span></span>

![Database](~/ef6/media/database.png)
