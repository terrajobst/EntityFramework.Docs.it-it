---
title: Reverse Engineering-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 775a929982b9f4fb10aad9cd43bbb555ce632ad1
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149015"
---
# <a name="reverse-engineering"></a><span data-ttu-id="4982e-102">Reverse Engineering</span><span class="sxs-lookup"><span data-stu-id="4982e-102">Reverse Engineering</span></span>

<span data-ttu-id="4982e-103">Il Reverse Engineering è il processo di impalcature di classi di tipi di entità e una classe DbContext basata su uno schema di database.</span><span class="sxs-lookup"><span data-stu-id="4982e-103">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="4982e-104">Può essere eseguita usando il `Scaffold-DbContext` comando degli strumenti della console di gestione pacchetti di EF Core `dotnet ef dbcontext scaffold` o del comando degli strumenti dell'interfaccia della riga di comando di .NET (CLI).</span><span class="sxs-lookup"><span data-stu-id="4982e-104">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="4982e-105">Installazione di</span><span class="sxs-lookup"><span data-stu-id="4982e-105">Installing</span></span>

<span data-ttu-id="4982e-106">Prima di reverse engineering, è necessario installare gli strumenti di [PMC](xref:core/miscellaneous/cli/powershell) (solo Visual Studio) o gli [strumenti dell'interfaccia](xref:core/miscellaneous/cli/dotnet)della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="4982e-106">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="4982e-107">Per informazioni dettagliate, vedere i collegamenti.</span><span class="sxs-lookup"><span data-stu-id="4982e-107">See links for details.</span></span>

<span data-ttu-id="4982e-108">Sarà inoltre necessario installare un [provider di database](xref:core/providers/index) appropriato per lo schema del database che si desidera decodificare.</span><span class="sxs-lookup"><span data-stu-id="4982e-108">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="4982e-109">Stringa di connessione</span><span class="sxs-lookup"><span data-stu-id="4982e-109">Connection string</span></span>

<span data-ttu-id="4982e-110">Il primo argomento del comando è una stringa di connessione al database.</span><span class="sxs-lookup"><span data-stu-id="4982e-110">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="4982e-111">Questa stringa di connessione verrà utilizzata dagli strumenti per leggere lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="4982e-111">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="4982e-112">La modalità di citazione e di escape della stringa di connessione dipende dalla shell utilizzata per eseguire il comando.</span><span class="sxs-lookup"><span data-stu-id="4982e-112">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="4982e-113">Per informazioni dettagliate, vedere la documentazione della shell.</span><span class="sxs-lookup"><span data-stu-id="4982e-113">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="4982e-114">PowerShell, ad esempio, richiede l'escape del `$` carattere, ma non `\`.</span><span class="sxs-lookup"><span data-stu-id="4982e-114">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="4982e-115">Configurazione e segreti utente</span><span class="sxs-lookup"><span data-stu-id="4982e-115">Configuration and User Secrets</span></span>

<span data-ttu-id="4982e-116">Se si dispone di un progetto di ASP.NET Core, è possibile `Name=<connection-string>` utilizzare la sintassi per leggere la stringa di connessione dalla configurazione.</span><span class="sxs-lookup"><span data-stu-id="4982e-116">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="4982e-117">Questo funziona bene con lo [strumento di gestione dei segreti](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) per evitare che la password del database sia separata dalla codebase.</span><span class="sxs-lookup"><span data-stu-id="4982e-117">This works well with the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="4982e-118">Nome provider</span><span class="sxs-lookup"><span data-stu-id="4982e-118">Provider name</span></span>

<span data-ttu-id="4982e-119">Il secondo argomento è il nome del provider.</span><span class="sxs-lookup"><span data-stu-id="4982e-119">The second argument is the provider name.</span></span> <span data-ttu-id="4982e-120">Il nome del provider è in genere uguale al nome del pacchetto NuGet del provider.</span><span class="sxs-lookup"><span data-stu-id="4982e-120">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="4982e-121">Specifica di tabelle</span><span class="sxs-lookup"><span data-stu-id="4982e-121">Specifying tables</span></span>

<span data-ttu-id="4982e-122">Per impostazione predefinita, tutte le tabelle nello schema del database sono decodificate in tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="4982e-122">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="4982e-123">È possibile limitare le tabelle decodificate specificando schemi e tabelle.</span><span class="sxs-lookup"><span data-stu-id="4982e-123">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

<span data-ttu-id="4982e-124">Il `-Schemas` parametro in PMC e l' `--schema` opzione nell'interfaccia della riga di comando possono essere utilizzati per includere ogni tabella all'interno di uno schema.</span><span class="sxs-lookup"><span data-stu-id="4982e-124">The `-Schemas` parameter in PMC and the `--schema` option in the CLI can be used to include every table within a schema.</span></span>

<span data-ttu-id="4982e-125">`-Tables`È possibile usare ( `--table` PMC) e (CLI) per includere tabelle specifiche.</span><span class="sxs-lookup"><span data-stu-id="4982e-125">`-Tables` (PMC) and `--table` (CLI) can be used to include specific tables.</span></span>

<span data-ttu-id="4982e-126">Per includere più tabelle in PMC, usare una matrice.</span><span class="sxs-lookup"><span data-stu-id="4982e-126">To include multiple tables in PMC, use an array.</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

<span data-ttu-id="4982e-127">Per includere più tabelle nell'interfaccia della riga di comando, specificare l'opzione più volte.</span><span class="sxs-lookup"><span data-stu-id="4982e-127">To include multiple tables in the CLI, specify the option multiple times.</span></span>

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a><span data-ttu-id="4982e-128">Conservazione dei nomi</span><span class="sxs-lookup"><span data-stu-id="4982e-128">Preserving names</span></span>

<span data-ttu-id="4982e-129">Per impostazione predefinita, i nomi delle tabelle e delle colonne sono corretti per una migliore corrispondenza con le convenzioni di denominazione .NET per i tipi e le proprietà.</span><span class="sxs-lookup"><span data-stu-id="4982e-129">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="4982e-130">Se si `-UseDatabaseNames` specifica l'opzione nella console `--use-database-names` di gestione pacchetti o l'opzione nell'interfaccia della riga di comando, questo comportamento verrà disabilitato per quanto possibile.</span><span class="sxs-lookup"><span data-stu-id="4982e-130">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="4982e-131">Gli identificatori .NET non validi verranno comunque corretti e i nomi sintetizzati, come le proprietà di navigazione, saranno comunque conformi alle convenzioni di denominazione .NET.</span><span class="sxs-lookup"><span data-stu-id="4982e-131">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="4982e-132">API Fluent o annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="4982e-132">Fluent API or Data Annotations</span></span>

<span data-ttu-id="4982e-133">Per impostazione predefinita, i tipi di entità vengono configurati tramite l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="4982e-133">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="4982e-134">Specificare `-DataAnnotations` (PMC) o `--data-annotations` (CLI) per usare invece le annotazioni dei dati, quando possibile.</span><span class="sxs-lookup"><span data-stu-id="4982e-134">Specify `-DataAnnotations` (PMC) or `--data-annotations` (CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="4982e-135">Se ad esempio si usa l'API Fluent, l'impalcatura è la seguente:</span><span class="sxs-lookup"><span data-stu-id="4982e-135">For example, using the Fluent API will scaffold this:</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="4982e-136">Quando si usano le annotazioni dei dati, questo è l'impalcatura:</span><span class="sxs-lookup"><span data-stu-id="4982e-136">While using Data Annotations will scaffold this:</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="4982e-137">Nome DbContext</span><span class="sxs-lookup"><span data-stu-id="4982e-137">DbContext name</span></span>

<span data-ttu-id="4982e-138">Il nome della classe DbContext con impalcature sarà il nome del database con suffisso *per impostazione predefinita* .</span><span class="sxs-lookup"><span data-stu-id="4982e-138">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="4982e-139">Per specificarne uno diverso, usare `-Context` in PMC e `--context` nell'interfaccia della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="4982e-139">To specify a different one, use `-Context` in PMC and `--context` in the CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="4982e-140">Directory e spazi dei nomi</span><span class="sxs-lookup"><span data-stu-id="4982e-140">Directories and namespaces</span></span>

<span data-ttu-id="4982e-141">Le classi di entità e una classe DbContext sono impalcature nella directory radice del progetto e usano lo spazio dei nomi predefinito del progetto.</span><span class="sxs-lookup"><span data-stu-id="4982e-141">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span> <span data-ttu-id="4982e-142">È possibile specificare la directory in cui le classi sono sottoposto a impalcatura `--output-dir` usando `-OutputDir` (PMC) o (CLI).</span><span class="sxs-lookup"><span data-stu-id="4982e-142">You can specify the directory where classes are scaffolded using `-OutputDir` (PMC) or `--output-dir` (CLI).</span></span> <span data-ttu-id="4982e-143">Lo spazio dei nomi sarà lo spazio dei nomi radice più i nomi delle sottodirectory nella directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="4982e-143">The namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span>

<span data-ttu-id="4982e-144">È anche possibile usare `-ContextDir` (PMC) e `--context-dir` (CLI) per eseguire il patibolo della classe DbContext in una directory separata dalle classi dei tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="4982e-144">You can also use `-ContextDir` (PMC) and `--context-dir` (CLI) to scaffold the DbContext class into a separate directory from the entity type classes.</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a><span data-ttu-id="4982e-145">Funzionamento</span><span class="sxs-lookup"><span data-stu-id="4982e-145">How it works</span></span>

<span data-ttu-id="4982e-146">Il reverse engineering inizia con la lettura dello schema del database.</span><span class="sxs-lookup"><span data-stu-id="4982e-146">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="4982e-147">Legge le informazioni su tabelle, colonne, vincoli e indici.</span><span class="sxs-lookup"><span data-stu-id="4982e-147">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="4982e-148">USA quindi le informazioni sullo schema per creare un modello di EF Core.</span><span class="sxs-lookup"><span data-stu-id="4982e-148">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="4982e-149">Le tabelle vengono utilizzate per creare i tipi di entità. le colonne vengono utilizzate per creare le proprietà; e le chiavi esterne vengono utilizzate per creare relazioni.</span><span class="sxs-lookup"><span data-stu-id="4982e-149">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="4982e-150">Infine, il modello viene utilizzato per generare il codice.</span><span class="sxs-lookup"><span data-stu-id="4982e-150">Finally, the model is used to generate code.</span></span> <span data-ttu-id="4982e-151">Le classi del tipo di entità, l'API Fluent e le annotazioni dei dati corrispondenti sono basate su impalcature per ricreare lo stesso modello dall'app.</span><span class="sxs-lookup"><span data-stu-id="4982e-151">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="what-doesnt-work"></a><span data-ttu-id="4982e-152">Cosa non funziona</span><span class="sxs-lookup"><span data-stu-id="4982e-152">What doesn't work</span></span>

<span data-ttu-id="4982e-153">Non tutti gli elementi di un modello possono essere rappresentati utilizzando uno schema di database.</span><span class="sxs-lookup"><span data-stu-id="4982e-153">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="4982e-154">Ad esempio, le informazioni sulle [**gerarchie di ereditarietà**](../modeling/inheritance.md), i [**tipi di proprietà**](../modeling/owned-entities.md)e la suddivisione delle [**tabelle**](../modeling/table-splitting.md) non sono presenti nello schema del database.</span><span class="sxs-lookup"><span data-stu-id="4982e-154">For example, information about [**inheritance hierarchies**](../modeling/inheritance.md), [**owned types**](../modeling/owned-entities.md), and [**table splitting**](../modeling/table-splitting.md) are not present in the database schema.</span></span> <span data-ttu-id="4982e-155">Per questo motivo, questi costrutti non verranno mai decodificati.</span><span class="sxs-lookup"><span data-stu-id="4982e-155">Because of this, these constructs will never be reverse engineered.</span></span>

<span data-ttu-id="4982e-156">Inoltre, **alcuni tipi di colonna** potrebbero non essere supportati dal provider EF core.</span><span class="sxs-lookup"><span data-stu-id="4982e-156">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="4982e-157">Queste colonne non verranno incluse nel modello.</span><span class="sxs-lookup"><span data-stu-id="4982e-157">These columns won't be included in the model.</span></span>

<span data-ttu-id="4982e-158">È possibile definire i [**token di concorrenza**](../modeling/concurrency.md)in un modello di EF core per impedire a due utenti di aggiornare la stessa entità nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="4982e-158">You can define [**concurrency tokens**](../modeling/concurrency.md), in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="4982e-159">Alcuni database hanno un tipo speciale per rappresentare questo tipo di colonna (ad esempio, rowversion in SQL Server), nel qual caso è possibile decompilare queste informazioni; Tuttavia, altri token di concorrenza non verranno decodificati.</span><span class="sxs-lookup"><span data-stu-id="4982e-159">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="4982e-160">Personalizzazione del modello</span><span class="sxs-lookup"><span data-stu-id="4982e-160">Customizing the model</span></span>

<span data-ttu-id="4982e-161">Il codice generato da EF Core è il codice.</span><span class="sxs-lookup"><span data-stu-id="4982e-161">The code generated by EF Core is your code.</span></span> <span data-ttu-id="4982e-162">È possibile modificarlo.</span><span class="sxs-lookup"><span data-stu-id="4982e-162">Feel free to change it.</span></span> <span data-ttu-id="4982e-163">Verrà rigenerato solo se si esegue nuovamente la decompilazione dello stesso modello.</span><span class="sxs-lookup"><span data-stu-id="4982e-163">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="4982e-164">Il codice con impalcatura rappresenta *un* modello che può essere utilizzato per accedere al database, ma non è certo l' *unico* modello che può essere utilizzato.</span><span class="sxs-lookup"><span data-stu-id="4982e-164">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="4982e-165">Personalizzare le classi del tipo di entità e la classe DbContext in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="4982e-165">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="4982e-166">È ad esempio possibile scegliere di rinominare tipi e proprietà, introdurre gerarchie di ereditarietà o suddividere una tabella in più entità.</span><span class="sxs-lookup"><span data-stu-id="4982e-166">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="4982e-167">È inoltre possibile rimuovere indici non univoci, sequenze inutilizzate e proprietà di navigazione, proprietà scalari facoltative e nomi di vincoli dal modello.</span><span class="sxs-lookup"><span data-stu-id="4982e-167">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="4982e-168">È anche possibile aggiungere ulteriori costruttori, metodi, proprietà e così via.</span><span class="sxs-lookup"><span data-stu-id="4982e-168">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="4982e-169">uso di un'altra classe parziale in un file separato.</span><span class="sxs-lookup"><span data-stu-id="4982e-169">using another partial class in a separate file.</span></span> <span data-ttu-id="4982e-170">Questo approccio funziona anche quando si intende decodificare nuovamente il modello.</span><span class="sxs-lookup"><span data-stu-id="4982e-170">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="4982e-171">Aggiornamento del modello</span><span class="sxs-lookup"><span data-stu-id="4982e-171">Updating the model</span></span>

<span data-ttu-id="4982e-172">Dopo aver apportato modifiche al database, potrebbe essere necessario aggiornare il modello di EF Core per riflettere tali modifiche.</span><span class="sxs-lookup"><span data-stu-id="4982e-172">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="4982e-173">Se le modifiche apportate al database sono semplici, potrebbe essere più semplice apportare manualmente le modifiche al modello di EF Core.</span><span class="sxs-lookup"><span data-stu-id="4982e-173">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="4982e-174">Ad esempio, la ridenominazione di una tabella o di una colonna, la rimozione di una colonna o l'aggiornamento del tipo di una colonna sono semplici modifiche da apportare nel codice.</span><span class="sxs-lookup"><span data-stu-id="4982e-174">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="4982e-175">Le modifiche più significative, tuttavia, non sono semplici da creare manualmente.</span><span class="sxs-lookup"><span data-stu-id="4982e-175">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="4982e-176">Un flusso di lavoro comune è quello di decodificare il modello dal `-Force` database usando (PMC `--force` ) o (CLI) per sovrascrivere il modello esistente con uno aggiornato.</span><span class="sxs-lookup"><span data-stu-id="4982e-176">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="4982e-177">Un'altra funzionalità comunemente richiesta è la possibilità di aggiornare il modello dal database mantenendo la personalizzazione, ad esempio le rinominazioni, le gerarchie dei tipi e così via. Utilizzare Issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) per tenere traccia dello stato di avanzamento di questa funzionalità.</span><span class="sxs-lookup"><span data-stu-id="4982e-177">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="4982e-178">Se si esegue di nuovo la decompilazione del modello dal database, tutte le modifiche apportate ai file andranno perse.</span><span class="sxs-lookup"><span data-stu-id="4982e-178">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>
