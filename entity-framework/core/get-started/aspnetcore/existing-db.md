---
title: Guida introduttiva ad ASP.NET Core - Database esistente - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: e28149346ccd7531449ea696505588317471e6dd
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949153"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="66d83-102">Introduzione a EF Core in ASP.NET Core con un database esistente</span><span class="sxs-lookup"><span data-stu-id="66d83-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="66d83-103">In questa procedura dettagliata si compilerà un'applicazione ASP.NET Core MVC che esegue l'accesso ai dati di base usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="66d83-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="66d83-104">Si userà il reverse engineering per creare un modello di Entity Framework basato su un database esistente.</span><span class="sxs-lookup"><span data-stu-id="66d83-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="66d83-105">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="66d83-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66d83-106">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="66d83-106">Prerequisites</span></span>

<span data-ttu-id="66d83-107">Per completare la procedura dettagliata, sono necessari i prerequisiti seguenti:</span><span class="sxs-lookup"><span data-stu-id="66d83-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="66d83-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) con questi carichi di lavoro:</span><span class="sxs-lookup"><span data-stu-id="66d83-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="66d83-109">**Sviluppo ASP.NET e Web** (in **Web e Cloud**)</span><span class="sxs-lookup"><span data-stu-id="66d83-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="66d83-110">**Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)</span><span class="sxs-lookup"><span data-stu-id="66d83-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="66d83-111">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="66d83-111">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* [<span data-ttu-id="66d83-112">Database Blogging</span><span class="sxs-lookup"><span data-stu-id="66d83-112">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="66d83-113">Database Blogging</span><span class="sxs-lookup"><span data-stu-id="66d83-113">Blogging database</span></span>

<span data-ttu-id="66d83-114">Questa esercitazione usa un database **Blogging** nell'istanza di LocalDb come database esistente.</span><span class="sxs-lookup"><span data-stu-id="66d83-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="66d83-115">Se si è già creato il database **Blogging** durante un'altra esercitazione, è possibile ignorare questi passaggi.</span><span class="sxs-lookup"><span data-stu-id="66d83-115">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="66d83-116">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="66d83-116">Open Visual Studio</span></span>
* <span data-ttu-id="66d83-117">**Strumenti -> Connetti al database**</span><span class="sxs-lookup"><span data-stu-id="66d83-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="66d83-118">Selezionare **Microsoft SQL Server** e fare clic su **Continua**</span><span class="sxs-lookup"><span data-stu-id="66d83-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="66d83-119">Immettere **(localdb)\mssqllocaldb** in **Nome server**</span><span class="sxs-lookup"><span data-stu-id="66d83-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="66d83-120">Immettere **master** in **Nome database** e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="66d83-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="66d83-121">Il database master viene ora visualizzato sotto **Connessioni dati** in **Esplora server**</span><span class="sxs-lookup"><span data-stu-id="66d83-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="66d83-122">Fare clic con il pulsante destro del mouse sul database in **Esplora server** e scegliere **Nuova query**</span><span class="sxs-lookup"><span data-stu-id="66d83-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="66d83-123">Copiare lo script, riportato sotto, nell'editor di query</span><span class="sxs-lookup"><span data-stu-id="66d83-123">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="66d83-124">Fare clic con il pulsante destro del mouse sull'editor di query e scegliere **Esegui**</span><span class="sxs-lookup"><span data-stu-id="66d83-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="66d83-125">Creare un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="66d83-125">Create a new project</span></span>

* <span data-ttu-id="66d83-126">Aprire Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="66d83-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="66d83-127">**File -> Nuovo -> Progetto**</span><span class="sxs-lookup"><span data-stu-id="66d83-127">**File -> New -> Project...**</span></span>
* <span data-ttu-id="66d83-128">Scegliere **Installati > Modelli > Visual C# > Web** dal menu a sinistra</span><span class="sxs-lookup"><span data-stu-id="66d83-128">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="66d83-129">Selezionare il modello di progetto **Applicazione Web ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="66d83-129">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="66d83-130">Immettere **EFGetStarted.AspNetCore.ExistingDb** come nome e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="66d83-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="66d83-131">Attendere che venga visualizzata la finestra di dialogo **Nuova applicazione Web ASP.NET Core**</span><span class="sxs-lookup"><span data-stu-id="66d83-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="66d83-132">In **ASP.NET Core Templates 2.0** (Modelli di ASP.NET Core 2.0) selezionare **Applicazione Web (MVC)**</span><span class="sxs-lookup"><span data-stu-id="66d83-132">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="66d83-133">Assicurarsi che **Autenticazione** sia impostato su **Nessuna autenticazione**</span><span class="sxs-lookup"><span data-stu-id="66d83-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="66d83-134">Fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="66d83-134">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="66d83-135">Installare Entity Framework</span><span class="sxs-lookup"><span data-stu-id="66d83-135">Install Entity Framework</span></span>

<span data-ttu-id="66d83-136">Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione.</span><span class="sxs-lookup"><span data-stu-id="66d83-136">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="66d83-137">Questa procedura dettagliata usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="66d83-137">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="66d83-138">Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="66d83-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="66d83-139">**Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="66d83-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="66d83-140">Eseguire `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="66d83-140">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="66d83-141">Verranno usati alcuni strumenti di Entity Framework per creare un modello dal database,</span><span class="sxs-lookup"><span data-stu-id="66d83-141">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="66d83-142">quindi verrà anche installato il pacchetto di strumenti:</span><span class="sxs-lookup"><span data-stu-id="66d83-142">So we will install the tools package as well:</span></span>

* <span data-ttu-id="66d83-143">Eseguire `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="66d83-143">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="66d83-144">Più avanti verranno usati alcuni strumenti di scaffolding di ASP.NET Core per creare controller e visualizzazioni,</span><span class="sxs-lookup"><span data-stu-id="66d83-144">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="66d83-145">quindi verrà anche installato il pacchetto di progettazioni:</span><span class="sxs-lookup"><span data-stu-id="66d83-145">So we will install this design package as well:</span></span>

* <span data-ttu-id="66d83-146">Eseguire `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span><span class="sxs-lookup"><span data-stu-id="66d83-146">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="66d83-147">Decompilare il modello</span><span class="sxs-lookup"><span data-stu-id="66d83-147">Reverse engineer your model</span></span>

<span data-ttu-id="66d83-148">È ora possibile creare il modello di EF basato sul database esistente.</span><span class="sxs-lookup"><span data-stu-id="66d83-148">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="66d83-149">**Strumenti –> Gestione pacchetti NuGet –> Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="66d83-149">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="66d83-150">Eseguire il comando seguente per creare un modello dal database esistente:</span><span class="sxs-lookup"><span data-stu-id="66d83-150">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="66d83-151">Se viene visualizzato l'errore `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, chiudere e riaprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="66d83-151">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="66d83-152">È possibile specificare per quali tabelle si vogliono generare le entità aggiungendo l'argomento `-Tables` al comando precedente.</span><span class="sxs-lookup"><span data-stu-id="66d83-152">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="66d83-153">Ad esempio `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="66d83-153">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="66d83-154">Il processo di reverse engineering ha creato le classi di entità (`Blog.cs` & `Post.cs`) e un contesto derivato (`BloggingContext.cs`) in base allo schema del database esistente.</span><span class="sxs-lookup"><span data-stu-id="66d83-154">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="66d83-155">Le classi di entità sono semplici oggetti C# che rappresentano i dati di cui verranno eseguite query e che verranno salvati.</span><span class="sxs-lookup"><span data-stu-id="66d83-155">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="66d83-156">Il contesto rappresenta una sessione con il database e consente di eseguire query delle istanze delle classi di entità e di salvarle.</span><span class="sxs-lookup"><span data-stu-id="66d83-156">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="66d83-157">Registrare il contesto con l'inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="66d83-157">Register your context with dependency injection</span></span>

<span data-ttu-id="66d83-158">Il concetto di inserimento delle dipendenze è fondamentale per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="66d83-158">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="66d83-159">I servizi (ad esempio, `BloggingContext`) vengono registrati con l'inserimento delle dipendenze durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="66d83-159">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="66d83-160">Questi servizi vengono quindi forniti ai componenti per cui sono necessari (ad esempio, i controller MVC) tramite proprietà o parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="66d83-160">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="66d83-161">Per altre informazioni sull'inserimento delle dipendenze, vedere l'articolo [Inserimento delle dipendenze](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) nel sito di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="66d83-161">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="66d83-162">Rimuovere la configurazione nel contesto inline</span><span class="sxs-lookup"><span data-stu-id="66d83-162">Remove inline context configuration</span></span>

<span data-ttu-id="66d83-163">In ASP.NET Core la configurazione viene in genere eseguita in **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="66d83-163">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="66d83-164">Per conformità a questo modello, la configurazione del provider di database verrà spostata in **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="66d83-164">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="66d83-165">Aprire `Models\BloggingContext.cs`</span><span class="sxs-lookup"><span data-stu-id="66d83-165">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="66d83-166">Eliminare il metodo `OnConfiguring(...)`</span><span class="sxs-lookup"><span data-stu-id="66d83-166">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="66d83-167">Aggiungere il costruttore seguente, che consente di passare la configurazione nel contesto tramite l'inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="66d83-167">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="66d83-168">Registrare e configurare il contesto in Startup.cs</span><span class="sxs-lookup"><span data-stu-id="66d83-168">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="66d83-169">`BloggingContext` verrà registrato come servizio per consentire ai controller MVC di usarlo.</span><span class="sxs-lookup"><span data-stu-id="66d83-169">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="66d83-170">Aprire **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="66d83-170">Open **Startup.cs**</span></span>
* <span data-ttu-id="66d83-171">Aggiungere le istruzioni `using` seguenti all'inizio del file</span><span class="sxs-lookup"><span data-stu-id="66d83-171">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="66d83-172">È ora possibile usare il metodo `AddDbContext(...)` per registrarlo come servizio.</span><span class="sxs-lookup"><span data-stu-id="66d83-172">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="66d83-173">Individuare il metodo `ConfigureServices(...)`</span><span class="sxs-lookup"><span data-stu-id="66d83-173">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="66d83-174">Aggiungere il codice seguente per registrare il contesto come servizio</span><span class="sxs-lookup"><span data-stu-id="66d83-174">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="66d83-175">In una vera applicazione in genere si inserisce la stringa di connessione in un file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="66d83-175">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="66d83-176">Per ragioni di semplicità, viene definita nel codice.</span><span class="sxs-lookup"><span data-stu-id="66d83-176">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="66d83-177">Per altre informazioni, vedere [Connection Strings](../../miscellaneous/connection-strings.md) (Stringhe di connessione).</span><span class="sxs-lookup"><span data-stu-id="66d83-177">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="66d83-178">Creare un controller</span><span class="sxs-lookup"><span data-stu-id="66d83-178">Create a controller</span></span>

<span data-ttu-id="66d83-179">Verrà ora abilitato lo scaffolding nel progetto.</span><span class="sxs-lookup"><span data-stu-id="66d83-179">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="66d83-180">Fare clic con il pulsante destro del mouse sulla cartella **Controllers** in **Esplora soluzioni** e scegliere **Aggiungi -> Controller**.</span><span class="sxs-lookup"><span data-stu-id="66d83-180">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="66d83-181">Selezionare **Dipendenze complete** e fare clic su **Aggiungi**</span><span class="sxs-lookup"><span data-stu-id="66d83-181">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="66d83-182">È possibile ignorare le istruzioni nel file `ScaffoldingReadMe.txt` visualizzato</span><span class="sxs-lookup"><span data-stu-id="66d83-182">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="66d83-183">Quando è abilitato, è possibile eseguire lo scaffolding di un controller per l'entità `Blog`.</span><span class="sxs-lookup"><span data-stu-id="66d83-183">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="66d83-184">Fare clic con il pulsante destro del mouse sulla cartella **Controllers** in **Esplora soluzioni** e scegliere **Aggiungi -> Controller**.</span><span class="sxs-lookup"><span data-stu-id="66d83-184">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="66d83-185">Selezionare **Controller MVC con visualizzazioni, che usa Entity Framework** e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="66d83-185">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="66d83-186">Impostare **Classe modello** su **Blog** e **Classe contesto dati** su **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="66d83-186">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="66d83-187">Fare clic su **Aggiungi**</span><span class="sxs-lookup"><span data-stu-id="66d83-187">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="66d83-188">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="66d83-188">Run the application</span></span>

<span data-ttu-id="66d83-189">È ora possibile eseguire l'applicazione per vederla in azione.</span><span class="sxs-lookup"><span data-stu-id="66d83-189">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="66d83-190">**Debug - > Avvia senza eseguire debug**</span><span class="sxs-lookup"><span data-stu-id="66d83-190">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="66d83-191">L'applicazione verrà compilata e aperta in un Web browser</span><span class="sxs-lookup"><span data-stu-id="66d83-191">The application will build and open in a web browser</span></span>
* <span data-ttu-id="66d83-192">Passare a `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="66d83-192">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="66d83-193">Fare clic su **Crea nuovo**</span><span class="sxs-lookup"><span data-stu-id="66d83-193">Click **Create New**</span></span>
* <span data-ttu-id="66d83-194">Immettere un **URL** per il nuovo blog e fare clic su **Crea**</span><span class="sxs-lookup"><span data-stu-id="66d83-194">Enter a **Url** for the new blog and click **Create**</span></span>

![immagine](_static/create.png)

![immagine](_static/index-existing-db.png)
