---
title: Reverse Engineering - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 6e61d2ebcf5ada365dcdb264bc371199574e12fa
ms.sourcegitcommit: 33b2e84dae96040f60a613186a24ff3c7b00b6db
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/21/2019
ms.locfileid: "56459185"
---
# <a name="reverse-engineering"></a><span data-ttu-id="ec41f-102">Reverse Engineering</span><span class="sxs-lookup"><span data-stu-id="ec41f-102">Reverse Engineering</span></span>

<span data-ttu-id="ec41f-103">Decodifica è il processo di scaffolding classi del tipo di entità e una classe DbContext basati su uno schema di database.</span><span class="sxs-lookup"><span data-stu-id="ec41f-103">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="ec41f-104">Può essere eseguita usando il `Scaffold-DbContext` comando degli strumenti di Entity Framework Core Package Manager Console (console di gestione pacchetti) o `dotnet ef dbcontext scaffold` comando degli strumenti dell'interfaccia della riga di comando (CLI di .NET).</span><span class="sxs-lookup"><span data-stu-id="ec41f-104">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="ec41f-105">Installazione di</span><span class="sxs-lookup"><span data-stu-id="ec41f-105">Installing</span></span>

<span data-ttu-id="ec41f-106">Prima la decompilazione, è necessario eseguire l'installazione di [strumenti console di gestione pacchetti](xref:core/miscellaneous/cli/powershell) (solo Visual Studio) o il [gli strumenti CLI](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="ec41f-106">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="ec41f-107">Vedere i collegamenti per informazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="ec41f-107">See links for details.</span></span>

<span data-ttu-id="ec41f-108">È necessario anche installare appropriata [provider di database](xref:core/providers/index) per lo schema del database che si desidera decodificare.</span><span class="sxs-lookup"><span data-stu-id="ec41f-108">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="ec41f-109">Stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="ec41f-109">Connection string</span></span>

<span data-ttu-id="ec41f-110">Il primo argomento per il comando è una stringa di connessione al database.</span><span class="sxs-lookup"><span data-stu-id="ec41f-110">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="ec41f-111">Gli strumenti userà questa stringa di connessione per leggere lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="ec41f-111">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="ec41f-112">Come utilizzare le virgolette ed escape alla stringa di connessione dipende dal quale shell in uso per eseguire il comando.</span><span class="sxs-lookup"><span data-stu-id="ec41f-112">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="ec41f-113">Fare riferimento alla documentazione della shell per le specifiche.</span><span class="sxs-lookup"><span data-stu-id="ec41f-113">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="ec41f-114">Ad esempio, PowerShell richiede di eseguire l'escape di `$` carattere, ma non `\`.</span><span class="sxs-lookup"><span data-stu-id="ec41f-114">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="ec41f-115">Configurazione e segreti utente</span><span class="sxs-lookup"><span data-stu-id="ec41f-115">Configuration and User Secrets</span></span>

<span data-ttu-id="ec41f-116">Se si dispone di un progetto ASP.NET Core, è possibile usare il `Name=<connection-string>` sintassi per leggere la stringa di connessione dalla configurazione.</span><span class="sxs-lookup"><span data-stu-id="ec41f-116">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="ec41f-117">Questo funziona bene con la [strumento Secret Manager](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) separare la password del database dalla codebase.</span><span class="sxs-lookup"><span data-stu-id="ec41f-117">This works well with the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="ec41f-118">Nome del provider</span><span class="sxs-lookup"><span data-stu-id="ec41f-118">Provider name</span></span>

<span data-ttu-id="ec41f-119">Il secondo argomento è il nome del provider.</span><span class="sxs-lookup"><span data-stu-id="ec41f-119">The second argument is the provider name.</span></span> <span data-ttu-id="ec41f-120">Il nome del provider è in genere lo stesso nome di pacchetto NuGet del provider.</span><span class="sxs-lookup"><span data-stu-id="ec41f-120">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="ec41f-121">Specifica le tabelle</span><span class="sxs-lookup"><span data-stu-id="ec41f-121">Specifying tables</span></span>

<span data-ttu-id="ec41f-122">Tutte le tabelle nello schema del database vengono decodificati in tipi di entità per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="ec41f-122">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="ec41f-123">È possibile limitare le tabelle che vengono decodificati specificando gli schemi e tabelle.</span><span class="sxs-lookup"><span data-stu-id="ec41f-123">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

<span data-ttu-id="ec41f-124">Il `-Schemas` parametro nella console di gestione pacchetti e `--schema` opzione della riga di comando può essere usata per includere tutte le tabelle all'interno di uno schema.</span><span class="sxs-lookup"><span data-stu-id="ec41f-124">The `-Schemas` parameter in PMC and the `--schema` option in the CLI can be used to include every table within a schema.</span></span>

<span data-ttu-id="ec41f-125">`-Tables` (PMC) e `--table` (CLI) può essere utilizzato per includere tabelle specifiche.</span><span class="sxs-lookup"><span data-stu-id="ec41f-125">`-Tables` (PMC) and `--table` (CLI) can be used to include specific tables.</span></span>

<span data-ttu-id="ec41f-126">Per includere più tabelle nella console di gestione pacchetti, usare una matrice.</span><span class="sxs-lookup"><span data-stu-id="ec41f-126">To include multiple tables in PMC, use an array.</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

<span data-ttu-id="ec41f-127">Per includere più tabelle della riga di comando, specificare l'opzione più volte.</span><span class="sxs-lookup"><span data-stu-id="ec41f-127">To include multiple tables in the CLI, specify the option multiple times.</span></span>

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a><span data-ttu-id="ec41f-128">Nomi di mantenimento</span><span class="sxs-lookup"><span data-stu-id="ec41f-128">Preserving names</span></span>

<span data-ttu-id="ec41f-129">I nomi di tabella e colonna sono fisse per adattarlo meglio le convenzioni di denominazione .NET per i tipi e le proprietà per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="ec41f-129">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="ec41f-130">Specificando il `-UseDatabaseNames` console di gestione pacchetti di attivazione o la `--use-database-names` opzione della riga di comando disabiliterà il problema mantenendo quanto più possibile i nomi di database originali.</span><span class="sxs-lookup"><span data-stu-id="ec41f-130">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="ec41f-131">Verranno risolti ancora identificatori .NET non è validi e nomi sintetizzati come proprietà di navigazione saranno ancora conforme alle convenzioni di denominazione .NET.</span><span class="sxs-lookup"><span data-stu-id="ec41f-131">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="ec41f-132">API Fluent o annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="ec41f-132">Fluent API or Data Annotations</span></span>

<span data-ttu-id="ec41f-133">Per impostazione predefinita, i tipi di entità sono configurati con l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="ec41f-133">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="ec41f-134">Specificare `-DataAnnotations` (console di gestione pacchetti) o `--data-annotations` (CLI) per usare invece le annotazioni dei dati quando possibile.</span><span class="sxs-lookup"><span data-stu-id="ec41f-134">Specify `-DataAnnotations` (PMC) or `--data-annotations` (CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="ec41f-135">Ad esempio, tramite l'API Fluent eseguirà lo scaffolding questo:</span><span class="sxs-lookup"><span data-stu-id="ec41f-135">For example, using the Fluent API will scaffold this:</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="ec41f-136">Durante l'utilizzo di annotazioni dei dati eseguirà lo scaffolding questo:</span><span class="sxs-lookup"><span data-stu-id="ec41f-136">While using Data Annotations will scaffold this:</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="ec41f-137">Nome oggetto DbContext</span><span class="sxs-lookup"><span data-stu-id="ec41f-137">DbContext name</span></span>

<span data-ttu-id="ec41f-138">Il nome della classe DbContext sottoposto a scaffolding sarà il nome del database con suffisso *contesto* per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="ec41f-138">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="ec41f-139">Per specificare una diversa, usare `-Context` nella console di gestione pacchetti e `--context` della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="ec41f-139">To specify a different one, use `-Context` in PMC and `--context` in the CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="ec41f-140">Le directory e gli spazi dei nomi</span><span class="sxs-lookup"><span data-stu-id="ec41f-140">Directories and namespaces</span></span>

<span data-ttu-id="ec41f-141">Le classi di entità e una classe DbContext è sottoposto a scaffolding nella directory radice del progetto e usare lo spazio dei nomi predefinito del progetto.</span><span class="sxs-lookup"><span data-stu-id="ec41f-141">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span> <span data-ttu-id="ec41f-142">È possibile specificare la directory in cui vengano eseguito lo scaffolding di classi usando `-OutputDir` (console di gestione pacchetti) o `--output-dir` (CLI).</span><span class="sxs-lookup"><span data-stu-id="ec41f-142">You can specify the directory where classes are scaffolded using `-OutputDir` (PMC) or `--output-dir` (CLI).</span></span> <span data-ttu-id="ec41f-143">Lo spazio dei nomi sarà lo spazio dei nomi radice e i nomi di tutte le sottodirectory nella directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="ec41f-143">The namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span>

<span data-ttu-id="ec41f-144">È anche possibile usare `-ContextDir` (console di gestione pacchetti) e `--context-dir` (CLI) per eseguire lo scaffolding della classe DbContext in una directory diversa dalle classi di tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="ec41f-144">You can also use `-ContextDir` (PMC) and `--context-dir` (CLI) to scaffold the DbContext class into a separate directory from the entity type classes.</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a><span data-ttu-id="ec41f-145">Come funziona</span><span class="sxs-lookup"><span data-stu-id="ec41f-145">How it works</span></span>

<span data-ttu-id="ec41f-146">Verrà avviata la decompilazione leggendo lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="ec41f-146">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="ec41f-147">Legge le informazioni sulle tabelle, colonne, vincoli e indici.</span><span class="sxs-lookup"><span data-stu-id="ec41f-147">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="ec41f-148">Successivamente, Usa le informazioni sullo schema per creare un modello di EF Core.</span><span class="sxs-lookup"><span data-stu-id="ec41f-148">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="ec41f-149">Le tabelle vengono utilizzate per creare tipi di entità. le colonne vengono utilizzate per creare le proprietà; e le chiavi esterne vengono utilizzate per creare relazioni.</span><span class="sxs-lookup"><span data-stu-id="ec41f-149">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="ec41f-150">Infine, il modello viene utilizzato per generare il codice.</span><span class="sxs-lookup"><span data-stu-id="ec41f-150">Finally, the model is used to generate code.</span></span> <span data-ttu-id="ec41f-151">Il corrispondente entità dati classi e API Fluent annotazioni del tipo sono sottoposto a scaffolding per ricreare lo stesso modello dalla propria app.</span><span class="sxs-lookup"><span data-stu-id="ec41f-151">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="what-doesnt-work"></a><span data-ttu-id="ec41f-152">Cosa non funziona</span><span class="sxs-lookup"><span data-stu-id="ec41f-152">What doesn't work</span></span>

<span data-ttu-id="ec41f-153">Non tutti gli aspetti relativi a un modello possono essere rappresentati usando uno schema di database.</span><span class="sxs-lookup"><span data-stu-id="ec41f-153">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="ec41f-154">Ad esempio, le informazioni sulle **gerarchie di ereditarietà**, **tipi di proprietà**, e **tabella suddivisione** non sono presenti nello schema del database.</span><span class="sxs-lookup"><span data-stu-id="ec41f-154">For example, information about **inheritance hierarchies**, **owned types**, and **table splitting** are not present in the database schema.</span></span> <span data-ttu-id="ec41f-155">Per questo motivo, questi costrutti non verrà mai essere decodificata.</span><span class="sxs-lookup"><span data-stu-id="ec41f-155">Because of this, these constructs will never be reverse engineered.</span></span>

<span data-ttu-id="ec41f-156">È inoltre **alcuni tipi di colonna** potrebbero non essere supportate dal provider di EF Core.</span><span class="sxs-lookup"><span data-stu-id="ec41f-156">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="ec41f-157">Queste colonne non verranno incluse nel modello.</span><span class="sxs-lookup"><span data-stu-id="ec41f-157">These columns won't be included in the model.</span></span>

<span data-ttu-id="ec41f-158">EF Core richiede ogni tipo di entità dispone di una chiave.</span><span class="sxs-lookup"><span data-stu-id="ec41f-158">EF Core requires every entity type to have a key.</span></span> <span data-ttu-id="ec41f-159">Le tabelle, tuttavia, non sono necessari per specificare una chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="ec41f-159">Tables, however, aren't required to specify a primary key.</span></span> <span data-ttu-id="ec41f-160">**Le tabelle senza una chiave primaria** attualmente non vengono decodificati.</span><span class="sxs-lookup"><span data-stu-id="ec41f-160">**Tables without a primary key** are currently not reverse engineered.</span></span>

<span data-ttu-id="ec41f-161">È possibile definire **token di concorrenza** in un modello di EF Core per evitare che due utenti di aggiornare la stessa entità contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="ec41f-161">You can define **concurrency tokens** in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="ec41f-162">Alcuni database dispongono di un tipo speciale per rappresentare questo tipo di colonna (ad esempio, rowversion in SQL Server) in questo caso è possibile invertire progettare queste informazioni. Tuttavia, altri token di concorrenza saranno non essere decodificato.</span><span class="sxs-lookup"><span data-stu-id="ec41f-162">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="ec41f-163">Personalizzazione del modello</span><span class="sxs-lookup"><span data-stu-id="ec41f-163">Customizing the model</span></span>

<span data-ttu-id="ec41f-164">Il codice generato da EF Core è il codice.</span><span class="sxs-lookup"><span data-stu-id="ec41f-164">The code generated by EF Core is your code.</span></span> <span data-ttu-id="ec41f-165">È possibile modificarlo.</span><span class="sxs-lookup"><span data-stu-id="ec41f-165">Feel free to change it.</span></span> <span data-ttu-id="ec41f-166">Verrà rigenerato solo se si decodifica lo stesso modello anche in questo caso.</span><span class="sxs-lookup"><span data-stu-id="ec41f-166">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="ec41f-167">Rappresenta il codice con scaffolding *uno* modello che può essere utilizzato per accedere al database, ma sicuramente non è la *solo* modello che può essere utilizzato.</span><span class="sxs-lookup"><span data-stu-id="ec41f-167">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="ec41f-168">Personalizzare le classi del tipo di entità e classe DbContext in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="ec41f-168">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="ec41f-169">Ad esempio, è possibile scegliere di rinominare i tipi e le proprietà, introdurre le gerarchie di ereditarietà, o dividere una tabella in più entità.</span><span class="sxs-lookup"><span data-stu-id="ec41f-169">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="ec41f-170">È anche possibile rimuovere gli indici non univoci, sequenze inutilizzate e le proprietà di navigazione, facoltativo delle proprietà scalari e nomi di vincoli dal modello.</span><span class="sxs-lookup"><span data-stu-id="ec41f-170">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="ec41f-171">È anche possibile aggiungere altri costruttori, metodi, proprietà, e così via.</span><span class="sxs-lookup"><span data-stu-id="ec41f-171">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="ec41f-172">utilizzo di un'altra classe parziale in un file separato.</span><span class="sxs-lookup"><span data-stu-id="ec41f-172">using another partial class in a separate file.</span></span> <span data-ttu-id="ec41f-173">Questo approccio funziona anche quando si intende decompilare il modello di nuovo.</span><span class="sxs-lookup"><span data-stu-id="ec41f-173">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="ec41f-174">L'aggiornamento del modello</span><span class="sxs-lookup"><span data-stu-id="ec41f-174">Updating the model</span></span>

<span data-ttu-id="ec41f-175">Dopo aver apportato modifiche al database, si potrebbe essere necessario aggiornare il modello di EF Core per riflettere tali modifiche.</span><span class="sxs-lookup"><span data-stu-id="ec41f-175">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="ec41f-176">Se le modifiche del database sono semplici, potrebbe essere più semplice apportare manualmente le modifiche al modello di EF Core.</span><span class="sxs-lookup"><span data-stu-id="ec41f-176">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="ec41f-177">Ad esempio, la ridenominazione di una tabella o colonna, rimuovere una colonna o si aggiorna tipo di una colonna è semplici modifiche da apportare nel codice.</span><span class="sxs-lookup"><span data-stu-id="ec41f-177">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="ec41f-178">Altre modifiche significative, tuttavia, non sono come facile apportare manualmente.</span><span class="sxs-lookup"><span data-stu-id="ec41f-178">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="ec41f-179">Un flusso di lavoro comune è quello inverso progettare il modello dal database di utilizzando nuovamente `-Force` (console di gestione pacchetti) o `--force` (CLI) per sovrascrivere il modello esistente con una aggiornata.</span><span class="sxs-lookup"><span data-stu-id="ec41f-179">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="ec41f-180">Un'altra funzionalità comunemente richiesta è la possibilità di aggiornare il modello dal database mantenendo la personalizzazione, ad esempio le operazioni di ridenominazione, le gerarchie dei tipi e così via. Usare problema [831 #](https://github.com/aspnet/EntityFrameworkCore/issues/831) per tenere traccia dell'avanzamento di questa funzionalità.</span><span class="sxs-lookup"><span data-stu-id="ec41f-180">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="ec41f-181">Se il modello dal database la decodificazione anche in questo caso, eventuali modifiche apportate ai file andranno perse.</span><span class="sxs-lookup"><span data-stu-id="ec41f-181">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>
