---
title: Migrazioni - Entity Framework Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: e9c4013d17a2d41772822f77b3ceba15702ffc48
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812055"
---
# <a name="migrations"></a><span data-ttu-id="0b914-102">Migrazioni</span><span class="sxs-lookup"><span data-stu-id="0b914-102">Migrations</span></span>

<span data-ttu-id="0b914-103">In fase di sviluppo un modello di dati viene modificato e non rimane sincronizzato con il database.</span><span class="sxs-lookup"><span data-stu-id="0b914-103">A data model changes during development and gets out of sync with the database.</span></span> <span data-ttu-id="0b914-104">È possibile eliminare il database e consentire a EF di crearne uno nuovo che corrisponda al modello, ma questa procedura comporta la perdita dei dati.</span><span class="sxs-lookup"><span data-stu-id="0b914-104">You can drop the database and let EF create a new one that matches the model, but this procedure results in the loss of data.</span></span> <span data-ttu-id="0b914-105">La funzionalità delle migrazioni in EF Core rappresenta un metodo per l'aggiornamento incrementale dello schema del database per mantenere quest'ultimo sincronizzato con il modello di dati dell'applicazione mantenendo i dati esistenti nel database.</span><span class="sxs-lookup"><span data-stu-id="0b914-105">The migrations feature in EF Core provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.</span></span>

<span data-ttu-id="0b914-106">Le migrazioni includono strumenti da riga di comando e API che consentono le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="0b914-106">Migrations includes command-line tools and APIs that help with the following tasks:</span></span>

* <span data-ttu-id="0b914-107">[Creare una migrazione](#create-a-migration).</span><span class="sxs-lookup"><span data-stu-id="0b914-107">[Create a migration](#create-a-migration).</span></span> <span data-ttu-id="0b914-108">Generare codice in grado di aggiornare il database in modo da sincronizzarlo con un set di modifiche al modello.</span><span class="sxs-lookup"><span data-stu-id="0b914-108">Generate code that can update the database to sync it with a set of model changes.</span></span>
* <span data-ttu-id="0b914-109">[Aggiornare il database](#update-the-database).</span><span class="sxs-lookup"><span data-stu-id="0b914-109">[Update the database](#update-the-database).</span></span> <span data-ttu-id="0b914-110">Applicare migrazioni in sospeso per aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="0b914-110">Apply pending migrations to update the database schema.</span></span>
* <span data-ttu-id="0b914-111">[Personalizzare il codice di migrazione](#customize-migration-code).</span><span class="sxs-lookup"><span data-stu-id="0b914-111">[Customize migration code](#customize-migration-code).</span></span> <span data-ttu-id="0b914-112">A volte il codice generato deve essere modificato o integrato.</span><span class="sxs-lookup"><span data-stu-id="0b914-112">Sometimes the generated code needs to be modified or supplemented.</span></span>
* <span data-ttu-id="0b914-113">[Rimuovere una migrazione](#remove-a-migration).</span><span class="sxs-lookup"><span data-stu-id="0b914-113">[Remove a migration](#remove-a-migration).</span></span> <span data-ttu-id="0b914-114">Eliminare il codice generato.</span><span class="sxs-lookup"><span data-stu-id="0b914-114">Delete the generated code.</span></span>
* <span data-ttu-id="0b914-115">[Ripristinare una migrazione](#revert-a-migration).</span><span class="sxs-lookup"><span data-stu-id="0b914-115">[Revert a migration](#revert-a-migration).</span></span> <span data-ttu-id="0b914-116">Annullare le modifiche al database.</span><span class="sxs-lookup"><span data-stu-id="0b914-116">Undo the database changes.</span></span>
* <span data-ttu-id="0b914-117">[Generare script SQL](#generate-sql-scripts).</span><span class="sxs-lookup"><span data-stu-id="0b914-117">[Generate SQL scripts](#generate-sql-scripts).</span></span> <span data-ttu-id="0b914-118">Potrebbe essere necessario uno script per aggiornare un database di produzione o per risolvere i problemi del codice di migrazione.</span><span class="sxs-lookup"><span data-stu-id="0b914-118">You might need a script to update a production database or to troubleshoot migration code.</span></span>
* <span data-ttu-id="0b914-119">[Applicare migrazioni al runtime](#apply-migrations-at-runtime).</span><span class="sxs-lookup"><span data-stu-id="0b914-119">[Apply migrations at runtime](#apply-migrations-at-runtime).</span></span> <span data-ttu-id="0b914-120">Quando gli aggiornamenti in fase di progettazione e l'esecuzione di script non sono le opzioni più appropriate, chiamare il metodo `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="0b914-120">When design-time updates and running scripts aren't the best options, call the `Migrate()` method.</span></span>

> [!TIP]
> <span data-ttu-id="0b914-121">Se `DbContext` si trova in un assembly diverso da quello del progetto di avvio, è possibile specificare in modo esplicito i progetti di destinazione e di avvio negli [strumenti della console di Gestione pacchetti](xref:core/miscellaneous/cli/powershell#target-and-startup-project) o negli [strumenti dell'interfaccia della riga di comando di .NET Core](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span><span class="sxs-lookup"><span data-stu-id="0b914-121">If the `DbContext` is in a different assembly than the startup project, you can explicitly specify the target and startup projects in either the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell#target-and-startup-project) or the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span></span>

## <a name="install-the-tools"></a><span data-ttu-id="0b914-122">Installare gli strumenti</span><span class="sxs-lookup"><span data-stu-id="0b914-122">Install the tools</span></span>

<span data-ttu-id="0b914-123">Installare gli [strumenti da riga di comando](xref:core/miscellaneous/cli/index):</span><span class="sxs-lookup"><span data-stu-id="0b914-123">Install the [command-line tools](xref:core/miscellaneous/cli/index):</span></span>

* <span data-ttu-id="0b914-124">Per Visual Studio, sono consigliati gli [strumenti della console di Gestione pacchetti](xref:core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="0b914-124">For Visual Studio, we recommend the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="0b914-125">Per gli altri ambienti di sviluppo, scegliere gli [strumenti dell'interfaccia della riga di comando di .NET Core](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="0b914-125">For other development environments, choose the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span>

## <a name="create-a-migration"></a><span data-ttu-id="0b914-126">Creare una migrazione</span><span class="sxs-lookup"><span data-stu-id="0b914-126">Create a migration</span></span>

<span data-ttu-id="0b914-127">Dopo la [definizione del modello iniziale](xref:core/modeling/index) è il momento di creare il database.</span><span class="sxs-lookup"><span data-stu-id="0b914-127">After you've [defined your initial model](xref:core/modeling/index), it's time to create the database.</span></span> <span data-ttu-id="0b914-128">Per aggiungere una migrazione iniziale, eseguire il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="0b914-128">To add an initial migration, run the following command.</span></span>

``` powershell
Add-Migration InitialCreate
```

``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="0b914-129">Nella directory **Migrations** del progetto vengono aggiunti tre file:</span><span class="sxs-lookup"><span data-stu-id="0b914-129">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="0b914-130">**XXXXXXXXXXXXXX_InitialCreate.cs**: file principale della migrazione.</span><span class="sxs-lookup"><span data-stu-id="0b914-130">**XXXXXXXXXXXXXX_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="0b914-131">Contiene le operazioni necessarie per applicare la migrazione (in `Up()`) e per ripristinarla (in `Down()`).</span><span class="sxs-lookup"><span data-stu-id="0b914-131">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="0b914-132">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs**: file di metadati della migrazione.</span><span class="sxs-lookup"><span data-stu-id="0b914-132">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="0b914-133">Contiene informazioni usate da Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0b914-133">Contains information used by EF.</span></span>
* <span data-ttu-id="0b914-134">**MyContextModelSnapshot.cs**: snapshot del modello corrente.</span><span class="sxs-lookup"><span data-stu-id="0b914-134">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="0b914-135">Usato per determinare cosa è stato modificato durante l'aggiunta della migrazione successiva.</span><span class="sxs-lookup"><span data-stu-id="0b914-135">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="0b914-136">Il timestamp nel nome file consente di mantenerne l'ordine cronologico e di visualizzare quindi la successione delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="0b914-136">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="0b914-137">È consentito spostare i file della cartella Migrations e modificarne lo spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="0b914-137">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="0b914-138">Le nuove migrazioni vengono create come migrazioni di pari livello rispetto all'ultima.</span><span class="sxs-lookup"><span data-stu-id="0b914-138">New migrations are created as siblings of the last migration.</span></span>

## <a name="update-the-database"></a><span data-ttu-id="0b914-139">Aggiornare il database</span><span class="sxs-lookup"><span data-stu-id="0b914-139">Update the database</span></span>

<span data-ttu-id="0b914-140">Applicare quindi la migrazione al database per creare lo schema.</span><span class="sxs-lookup"><span data-stu-id="0b914-140">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```

``` Console
dotnet ef database update
```

## <a name="customize-migration-code"></a><span data-ttu-id="0b914-141">Personalizzare il codice di migrazione</span><span class="sxs-lookup"><span data-stu-id="0b914-141">Customize migration code</span></span>

<span data-ttu-id="0b914-142">Dopo le modifiche al modello di EF Core, lo schema del database potrebbe non essere sincronizzato. Per aggiornarlo, aggiungere un'altra migrazione.</span><span class="sxs-lookup"><span data-stu-id="0b914-142">After making changes to your EF Core model, the database schema might be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="0b914-143">Il nome della migrazione può essere usato come messaggio di commit in un sistema di controllo della versione.</span><span class="sxs-lookup"><span data-stu-id="0b914-143">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="0b914-144">Ad esempio, è possibile scegliere un nome come *AddProductReviews* se la modifica è una nuova classe di entità per le revisioni.</span><span class="sxs-lookup"><span data-stu-id="0b914-144">For example, you might choose a name like *AddProductReviews* if the change is a new entity class for reviews.</span></span>

``` powershell
Add-Migration AddProductReviews
```

``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="0b914-145">Dopo che la migrazione è stata creata tramite scaffolding ed è stato generato il codice, esaminare tale codice per verificarne l'accuratezza e aggiungere, rimuovere o modificare eventuali operazioni necessarie a garantirne una corretta applicazione.</span><span class="sxs-lookup"><span data-stu-id="0b914-145">Once the migration is scaffolded (code generated for it), review the code for accuracy and add, remove or modify any operations required to apply it correctly.</span></span>

<span data-ttu-id="0b914-146">Una migrazione, ad esempio, può contenere le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="0b914-146">For example, a migration might contain the following operations:</span></span>

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

<span data-ttu-id="0b914-147">Queste operazioni rendono compatibile lo schema del database, ma non consentono di mantenere i nomi dei clienti esistenti.</span><span class="sxs-lookup"><span data-stu-id="0b914-147">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="0b914-148">Per migliorarle, riscriverle come segue.</span><span class="sxs-lookup"><span data-stu-id="0b914-148">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="0b914-149">Durante il processo di scaffolding della migrazione viene visualizzato un avviso se un'operazione può causare una perdita di dati, ad esempio nel caso della rimozione di una colonna.</span><span class="sxs-lookup"><span data-stu-id="0b914-149">The migration scaffolding process warns when an operation might result in data loss (like dropping a column).</span></span> <span data-ttu-id="0b914-150">Se viene visualizzato questo avviso, assicurarsi in particolare di esaminare il codice delle migrazioni per verificarne l'accuratezza.</span><span class="sxs-lookup"><span data-stu-id="0b914-150">If you see that warning, be especially sure to review the migrations code for accuracy.</span></span>

<span data-ttu-id="0b914-151">Applicare la migrazione al database tramite il comando appropriato.</span><span class="sxs-lookup"><span data-stu-id="0b914-151">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```

``` Console
dotnet ef database update
```

### <a name="empty-migrations"></a><span data-ttu-id="0b914-152">Migrazioni vuote</span><span class="sxs-lookup"><span data-stu-id="0b914-152">Empty migrations</span></span>

<span data-ttu-id="0b914-153">È talvolta utile aggiungere una migrazione senza apportare alcuna modifica al modello.</span><span class="sxs-lookup"><span data-stu-id="0b914-153">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="0b914-154">In questo caso, l'aggiunta di una nuova migrazione crea file di codice con classi vuote.</span><span class="sxs-lookup"><span data-stu-id="0b914-154">In this case, adding a new migration creates code files with empty classes.</span></span> <span data-ttu-id="0b914-155">È possibile personalizzare questa migrazione per eseguire operazioni non direttamente correlate al modello di Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0b914-155">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span> <span data-ttu-id="0b914-156">Ecco alcune operazioni che è necessario gestire in questo modo:</span><span class="sxs-lookup"><span data-stu-id="0b914-156">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="0b914-157">Ricerca full-text</span><span class="sxs-lookup"><span data-stu-id="0b914-157">Full-Text Search</span></span>
* <span data-ttu-id="0b914-158">Funzioni</span><span class="sxs-lookup"><span data-stu-id="0b914-158">Functions</span></span>
* <span data-ttu-id="0b914-159">Stored procedure</span><span class="sxs-lookup"><span data-stu-id="0b914-159">Stored procedures</span></span>
* <span data-ttu-id="0b914-160">Trigger</span><span class="sxs-lookup"><span data-stu-id="0b914-160">Triggers</span></span>
* <span data-ttu-id="0b914-161">Visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="0b914-161">Views</span></span>

## <a name="remove-a-migration"></a><span data-ttu-id="0b914-162">Rimuovere una migrazione</span><span class="sxs-lookup"><span data-stu-id="0b914-162">Remove a migration</span></span>

<span data-ttu-id="0b914-163">Dopo l'aggiunta di una migrazione ci si rende talvolta conto che prima di applicarla sono necessarie altre modifiche al modello di Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="0b914-163">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span> <span data-ttu-id="0b914-164">Per rimuovere l'ultima migrazione, usare questo comando.</span><span class="sxs-lookup"><span data-stu-id="0b914-164">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```

``` Console
dotnet ef migrations remove
```

<span data-ttu-id="0b914-165">Dopo la rimozione della migrazione, è possibile apportare le modifiche aggiuntive al modello. La migrazione può quindi essere aggiunta di nuovo.</span><span class="sxs-lookup"><span data-stu-id="0b914-165">After removing the migration, you can make the additional model changes and add it again.</span></span>

## <a name="revert-a-migration"></a><span data-ttu-id="0b914-166">Ripristinare una migrazione</span><span class="sxs-lookup"><span data-stu-id="0b914-166">Revert a migration</span></span>

<span data-ttu-id="0b914-167">Se una o più migrazioni sono già state applicate ma è necessario ripristinarle, è possibile usare lo stesso comando impiegato per applicarle, specificando però il nome della migrazione a cui si vuole eseguire il rollback.</span><span class="sxs-lookup"><span data-stu-id="0b914-167">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```

``` Console
dotnet ef database update LastGoodMigration
```

## <a name="generate-sql-scripts"></a><span data-ttu-id="0b914-168">Generare script SQL</span><span class="sxs-lookup"><span data-stu-id="0b914-168">Generate SQL scripts</span></span>

<span data-ttu-id="0b914-169">Per il debug delle migrazioni o la distribuzione di queste in un database di produzione è utile generare uno script SQL.</span><span class="sxs-lookup"><span data-stu-id="0b914-169">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="0b914-170">Lo script può quindi essere rivisto per garantirne la correttezza e ottimizzato in base alle esigenze del database di produzione.</span><span class="sxs-lookup"><span data-stu-id="0b914-170">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="0b914-171">È anche possibile combinare lo script con una tecnologia di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="0b914-171">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="0b914-172">Il comando di base è il seguente.</span><span class="sxs-lookup"><span data-stu-id="0b914-172">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```

``` Console
dotnet ef migrations script
```

<span data-ttu-id="0b914-173">Con questo comando è possibile usare diverse opzioni.</span><span class="sxs-lookup"><span data-stu-id="0b914-173">There are several options to this command.</span></span>

<span data-ttu-id="0b914-174">La migrazione **di origine** deve essere l'ultima migrazione applicata al database prima dell'esecuzione dello script.</span><span class="sxs-lookup"><span data-stu-id="0b914-174">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="0b914-175">Se non è stata applicata alcuna migrazione, specificare `0` (valore predefinito).</span><span class="sxs-lookup"><span data-stu-id="0b914-175">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="0b914-176">La migrazione **di destinazione**  è l'ultima migrazione applicata al database dopo l'esecuzione dello script.</span><span class="sxs-lookup"><span data-stu-id="0b914-176">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="0b914-177">L'impostazione predefinita corrisponde all'ultima migrazione nel progetto.</span><span class="sxs-lookup"><span data-stu-id="0b914-177">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="0b914-178">Facoltativamente, è possibile generare uno script **idempotente**.</span><span class="sxs-lookup"><span data-stu-id="0b914-178">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="0b914-179">Questo tipo di script applica le migrazioni solo se non sono già state applicate al database</span><span class="sxs-lookup"><span data-stu-id="0b914-179">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="0b914-180">ed è utile se non si sa esattamente quale migrazione è stata applicata per ultima al database o se si stanno applicando migrazioni a più database a cui in precedenza sono state applicate migrazioni diverse.</span><span class="sxs-lookup"><span data-stu-id="0b914-180">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

## <a name="apply-migrations-at-runtime"></a><span data-ttu-id="0b914-181">Applicare migrazioni al runtime</span><span class="sxs-lookup"><span data-stu-id="0b914-181">Apply migrations at runtime</span></span>

<span data-ttu-id="0b914-182">Alcune applicazioni devono applicare migrazioni in runtime durante l'avvio o la prima esecuzione.</span><span class="sxs-lookup"><span data-stu-id="0b914-182">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="0b914-183">In questo caso usare il metodo `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="0b914-183">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="0b914-184">Questo metodo si basa sul servizio `IMigrator`, che può essere usato per scenari più avanzati.</span><span class="sxs-lookup"><span data-stu-id="0b914-184">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="0b914-185">Usare `myDbContext.GetInfrastructure().GetService<IMigrator>()` per accedervi.</span><span class="sxs-lookup"><span data-stu-id="0b914-185">Use `myDbContext.GetInfrastructure().GetService<IMigrator>()` to access it.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
>
> * <span data-ttu-id="0b914-186">Questo approccio non è adatto a tutte le situazioni.</span><span class="sxs-lookup"><span data-stu-id="0b914-186">This approach isn't for everyone.</span></span> <span data-ttu-id="0b914-187">È utile per applicazioni con un database locale, ma la maggior parte delle applicazioni richiede una strategia di distribuzione più solida, ad esempio la generazione di script SQL.</span><span class="sxs-lookup"><span data-stu-id="0b914-187">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>
> * <span data-ttu-id="0b914-188">Non chiamare `EnsureCreated()` prima di `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="0b914-188">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="0b914-189">`EnsureCreated()` ignora la cartella Migrations per creare lo schema, causando un errore di `Migrate()`.</span><span class="sxs-lookup"><span data-stu-id="0b914-189">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0b914-190">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="0b914-190">Next steps</span></span>

<span data-ttu-id="0b914-191">Per altre informazioni, vedere <xref:core/miscellaneous/cli/index>.</span><span class="sxs-lookup"><span data-stu-id="0b914-191">For more information, see <xref:core/miscellaneous/cli/index>.</span></span>
