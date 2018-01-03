---
title: Migrazioni - Entity Framework Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: 24fbe344eba9b99929d905ac2b9e49c68a1a4323
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/22/2017
---
<a name="migrations"></a><span data-ttu-id="94fff-102">Migrazioni</span><span class="sxs-lookup"><span data-stu-id="94fff-102">Migrations</span></span>
==========
<span data-ttu-id="94fff-103">Le migrazioni rappresentano un metodo per l'applicazione di modifiche dello schema al database, per mantenere quest'ultimo sincronizzato con il modello Entity Framework Core mantenendo i dati esistenti nel database.</span><span class="sxs-lookup"><span data-stu-id="94fff-103">Migrations provide a way to incrementally apply schema changes to the database to keep it in sync with your EF Core model while preserving existing data in the database.</span></span>

<a name="creating-the-database"></a><span data-ttu-id="94fff-104">Creazione del database</span><span class="sxs-lookup"><span data-stu-id="94fff-104">Creating the database</span></span>
---------------------
<span data-ttu-id="94fff-105">Dopo la [definizione del modello iniziale ][1] è il momento di creare il database.</span><span class="sxs-lookup"><span data-stu-id="94fff-105">After you've [defined your initial model][1], it's time to create the database.</span></span> <span data-ttu-id="94fff-106">A tale scopo, aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="94fff-106">To do this, add an initial migration.</span></span>
<span data-ttu-id="94fff-107">Installare [Entity Framework Core Tools] [ 2] ed eseguire il comando appropriato.</span><span class="sxs-lookup"><span data-stu-id="94fff-107">Install the [EF Core Tools][2] and run the appropriate command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="94fff-108">Nella directory **Migrations** del progetto vengono aggiunti tre file:</span><span class="sxs-lookup"><span data-stu-id="94fff-108">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="94fff-109">**00000000000000_InitialCreate.cs**: file principale della migrazione.</span><span class="sxs-lookup"><span data-stu-id="94fff-109">**00000000000000_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="94fff-110">Contiene le operazioni necessarie per applicare la migrazione (in `Up()`) e per ripristinarla (in `Down()`).</span><span class="sxs-lookup"><span data-stu-id="94fff-110">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="94fff-111">**00000000000000_InitialCreate.Designer.cs**: file di metadati della migrazione.</span><span class="sxs-lookup"><span data-stu-id="94fff-111">**00000000000000_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="94fff-112">Contiene informazioni usate da Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="94fff-112">Contains information used by EF.</span></span>
* <span data-ttu-id="94fff-113">**MyContextModelSnapshot.cs**: snapshot del modello corrente.</span><span class="sxs-lookup"><span data-stu-id="94fff-113">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="94fff-114">Usato per determinare cosa è stato modificato durante l'aggiunta della migrazione successiva.</span><span class="sxs-lookup"><span data-stu-id="94fff-114">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="94fff-115">Il timestamp nel nome file consente di mantenerne l'ordine cronologico e di visualizzare quindi la successione delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="94fff-115">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="94fff-116">È consentito spostare i file della cartella Migrations e modificarne lo spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="94fff-116">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="94fff-117">Le nuove migrazioni vengono create come migrazioni di pari livello rispetto all'ultima.</span><span class="sxs-lookup"><span data-stu-id="94fff-117">New migrations are created as siblings of the last migration.</span></span>

<span data-ttu-id="94fff-118">Applicare quindi la migrazione al database per creare lo schema.</span><span class="sxs-lookup"><span data-stu-id="94fff-118">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a><span data-ttu-id="94fff-119">Aggiunta di un'altra migrazione</span><span class="sxs-lookup"><span data-stu-id="94fff-119">Adding another migration</span></span>
------------------------
<span data-ttu-id="94fff-120">Dopo le modifiche al modello di Entity Framework Core, lo schema del database non è sincronizzato. Per aggiornarlo, aggiungere un'altra migrazione.</span><span class="sxs-lookup"><span data-stu-id="94fff-120">After making changes to your EF Core model, the database schema will be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="94fff-121">Il nome della migrazione può essere usato come messaggio di commit in un sistema di controllo della versione.</span><span class="sxs-lookup"><span data-stu-id="94fff-121">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="94fff-122">Se, ad esempio, si apportano modifiche per salvare le recensioni di prodotti create dai clienti, un nome possibile potrebbe essere *AddProductReviews*.</span><span class="sxs-lookup"><span data-stu-id="94fff-122">For example, if I made changes to save customer reviews of products, I might choose something like *AddProductReviews*.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="94fff-123">Dopo lo scaffolding della migrazione è consigliabile verificare l'accuratezza della migrazione stessa, aggiungendo eventuali operazioni necessarie a garantirne una corretta applicazione.</span><span class="sxs-lookup"><span data-stu-id="94fff-123">Once the migration is scaffolded, you should review it for accuracy and add any additional operations required to apply it correctly.</span></span> <span data-ttu-id="94fff-124">La migrazione, ad esempio, può contenere le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="94fff-124">For example, your migration might contain the following operations:</span></span>

``` csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);
```

<span data-ttu-id="94fff-125">Queste operazioni rendono compatibile lo schema del database, ma non consentono di mantenere i nomi dei clienti esistenti.</span><span class="sxs-lookup"><span data-stu-id="94fff-125">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="94fff-126">Per migliorarle, riscriverle come segue.</span><span class="sxs-lookup"><span data-stu-id="94fff-126">To make it better, rewrite it as follows.</span></span>

``` csharp
migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET Name = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

> [!TIP]
> <span data-ttu-id="94fff-127">Durante l'aggiunta di una nuova migrazione viene visualizzato un avviso se lo scaffolding di un'operazione può causare una perdita di dati, ad esempio nel caso della rimozione di una colonna.</span><span class="sxs-lookup"><span data-stu-id="94fff-127">Adding a new migration warns when an operation is scaffolded that may result in data loss (like dropping a column).</span></span> <span data-ttu-id="94fff-128">Assicurarsi di controllare l'accuratezza di queste migrazioni con particolare attenzione.</span><span class="sxs-lookup"><span data-stu-id="94fff-128">Be sure to especially review these migrations for accuracy.</span></span>

<span data-ttu-id="94fff-129">Applicare la migrazione al database tramite il comando appropriato.</span><span class="sxs-lookup"><span data-stu-id="94fff-129">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a><span data-ttu-id="94fff-130">Rimozione di una migrazione</span><span class="sxs-lookup"><span data-stu-id="94fff-130">Removing a migration</span></span>
--------------------
<span data-ttu-id="94fff-131">Dopo l'aggiunta di una migrazione ci si rende talvolta conto che prima di applicarla sono necessarie altre modifiche al modello di Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="94fff-131">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span>
<span data-ttu-id="94fff-132">Per rimuovere l'ultima migrazione, usare questo comando.</span><span class="sxs-lookup"><span data-stu-id="94fff-132">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="94fff-133">Dopo la rimozione è possibile apportare le modifiche aggiuntive al modello. La migrazione può quindi essere aggiunta di nuovo.</span><span class="sxs-lookup"><span data-stu-id="94fff-133">After removing it, you can make the additional model changes and add it again.</span></span>

<a name="reverting-a-migration"></a><span data-ttu-id="94fff-134">Ripristino di una migrazione</span><span class="sxs-lookup"><span data-stu-id="94fff-134">Reverting a migration</span></span>
---------------------
<span data-ttu-id="94fff-135">Se una o più migrazioni sono già state applicate ma è necessario ripristinarle, è possibile usare lo stesso comando impiegato per applicarle, specificando però il nome della migrazione a cui si vuole eseguire il rollback.</span><span class="sxs-lookup"><span data-stu-id="94fff-135">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a><span data-ttu-id="94fff-136">Migrazioni vuote</span><span class="sxs-lookup"><span data-stu-id="94fff-136">Empty migrations</span></span>
----------------
<span data-ttu-id="94fff-137">È talvolta utile aggiungere una migrazione senza apportare alcuna modifica al modello.</span><span class="sxs-lookup"><span data-stu-id="94fff-137">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="94fff-138">In questo caso, l'aggiunta di una nuova migrazione crea una migrazione vuota.</span><span class="sxs-lookup"><span data-stu-id="94fff-138">In this case, adding a new migration creates an empty one.</span></span> <span data-ttu-id="94fff-139">È possibile personalizzare questa migrazione per eseguire operazioni non direttamente correlate al modello di Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="94fff-139">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span>
<span data-ttu-id="94fff-140">Ecco alcune operazioni che è necessario gestire in questo modo:</span><span class="sxs-lookup"><span data-stu-id="94fff-140">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="94fff-141">Ricerca full-text</span><span class="sxs-lookup"><span data-stu-id="94fff-141">Full-Text Search</span></span>
* <span data-ttu-id="94fff-142">Funzioni</span><span class="sxs-lookup"><span data-stu-id="94fff-142">Functions</span></span>
* <span data-ttu-id="94fff-143">Stored procedure</span><span class="sxs-lookup"><span data-stu-id="94fff-143">Stored procedures</span></span>
* <span data-ttu-id="94fff-144">Trigger</span><span class="sxs-lookup"><span data-stu-id="94fff-144">Triggers</span></span>
* <span data-ttu-id="94fff-145">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="94fff-145">Views</span></span>
* <span data-ttu-id="94fff-146">e così via</span><span class="sxs-lookup"><span data-stu-id="94fff-146">etc.</span></span>

<a name="generating-a-sql-script"></a><span data-ttu-id="94fff-147">Generazione di uno script SQL</span><span class="sxs-lookup"><span data-stu-id="94fff-147">Generating a SQL script</span></span>
-----------------------
<span data-ttu-id="94fff-148">Per il debug delle migrazioni o la distribuzione di queste in un database di produzione è utile generare uno script SQL.</span><span class="sxs-lookup"><span data-stu-id="94fff-148">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="94fff-149">Lo script può quindi essere rivisto per garantirne la correttezza e ottimizzato in base alle esigenze del database di produzione.</span><span class="sxs-lookup"><span data-stu-id="94fff-149">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="94fff-150">È anche possibile combinare lo script con una tecnologia di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="94fff-150">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="94fff-151">Il comando di base è il seguente.</span><span class="sxs-lookup"><span data-stu-id="94fff-151">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="94fff-152">Con questo comando è possibile usare diverse opzioni.</span><span class="sxs-lookup"><span data-stu-id="94fff-152">There are several options to this command.</span></span>

<span data-ttu-id="94fff-153">La migrazione **di origine** deve essere l'ultima migrazione applicata al database prima dell'esecuzione dello script.</span><span class="sxs-lookup"><span data-stu-id="94fff-153">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="94fff-154">Se non è stata applicata alcuna migrazione, specificare `0` (valore predefinito).</span><span class="sxs-lookup"><span data-stu-id="94fff-154">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="94fff-155">La migrazione **di destinazione**  è l'ultima migrazione applicata al database dopo l'esecuzione dello script.</span><span class="sxs-lookup"><span data-stu-id="94fff-155">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="94fff-156">L'impostazione predefinita corrisponde all'ultima migrazione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="94fff-156">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="94fff-157">Facoltativamente, è possibile generare uno script **idempotente**.</span><span class="sxs-lookup"><span data-stu-id="94fff-157">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="94fff-158">Questo tipo di script applica le migrazioni solo se non sono già state applicate al database</span><span class="sxs-lookup"><span data-stu-id="94fff-158">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="94fff-159">ed è utile se non si sa esattamente quale migrazione è stata applicata per ultima al database o se si stanno applicando migrazioni a più database a cui in precedenza sono state applicate migrazioni diverse.</span><span class="sxs-lookup"><span data-stu-id="94fff-159">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="applying-migrations-at-runtime"></a><span data-ttu-id="94fff-160">Applicazione di migrazioni in runtime</span><span class="sxs-lookup"><span data-stu-id="94fff-160">Applying migrations at runtime</span></span>
------------------------------
<span data-ttu-id="94fff-161">Alcune applicazioni devono applicare migrazioni in runtime durante l'avvio o la prima esecuzione.</span><span class="sxs-lookup"><span data-stu-id="94fff-161">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="94fff-162">In questo caso usare il metodo `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="94fff-162">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="94fff-163">È tuttavia necessario prestare attenzione, perché questo approccio non è adatto a tutte le situazioni.</span><span class="sxs-lookup"><span data-stu-id="94fff-163">Caution, this approach isn't for everyone.</span></span> <span data-ttu-id="94fff-164">È utile per applicazioni con un database locale, ma la maggior parte delle applicazioni richiede una strategia di distribuzione più solida, ad esempio la generazione di script SQL.</span><span class="sxs-lookup"><span data-stu-id="94fff-164">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> <span data-ttu-id="94fff-165">Non chiamare `EnsureCreated()` prima di `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="94fff-165">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="94fff-166">`EnsureCreated()` ignora la cartella Migrations per creare lo schema, causando un errore di `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="94fff-166">`EnsureCreated()` bypasses Migrations to create the schema and cause `Migrate()` to fail.</span></span>

> [!NOTE]
> <span data-ttu-id="94fff-167">Questo metodo si basa sul servizio `IMigrator`, che può essere usato per scenari più avanzati.</span><span class="sxs-lookup"><span data-stu-id="94fff-167">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="94fff-168">Usare `DbContext.GetService<IMigrator>()` per accedervi.</span><span class="sxs-lookup"><span data-stu-id="94fff-168">Use `DbContext.GetService<IMigrator>()` to access it.</span></span>


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
