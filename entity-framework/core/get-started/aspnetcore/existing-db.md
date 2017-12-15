---
title: Guida introduttiva ad ASP.NET Core - Database esistente - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: afd99d68d2ba25ce58a21dc48d2c7ce27f208807
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="ac952-102">Introduzione a EF Core in ASP.NET Core con un database esistente</span><span class="sxs-lookup"><span data-stu-id="ac952-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="ac952-103">[.NET Core SDK](https://www.microsoft.com/net/download/core) non supporta più `project.json` o Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="ac952-103">The [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or Visual Studio 2015.</span></span> <span data-ttu-id="ac952-104">Si consiglia agli sviluppatori .NET Core di [eseguire la migrazione da project.json a csproj](https://docs.microsoft.com/dotnet/articles/core/migration/) e a [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="ac952-104">Everyone doing .NET Core development is encouraged to [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/) and [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

<span data-ttu-id="ac952-105">In questa procedura dettagliata si compilerà un'applicazione ASP.NET Core MVC che esegue l'accesso ai dati di base usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ac952-105">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="ac952-106">Si userà il reverse engineering per creare un modello di Entity Framework basato su un database esistente.</span><span class="sxs-lookup"><span data-stu-id="ac952-106">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="ac952-107">È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) di questo articolo in GitHub.</span><span class="sxs-lookup"><span data-stu-id="ac952-107">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ac952-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="ac952-108">Prerequisites</span></span>

<span data-ttu-id="ac952-109">Per completare la procedura dettagliata, sono necessari i prerequisiti seguenti:</span><span class="sxs-lookup"><span data-stu-id="ac952-109">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="ac952-110">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) con questi carichi di lavoro:</span><span class="sxs-lookup"><span data-stu-id="ac952-110">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="ac952-111">**Sviluppo ASP.NET e Web** (in **Web e Cloud**)</span><span class="sxs-lookup"><span data-stu-id="ac952-111">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="ac952-112">**Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)</span><span class="sxs-lookup"><span data-stu-id="ac952-112">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="ac952-113">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="ac952-113">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* [<span data-ttu-id="ac952-114">Database Blogging</span><span class="sxs-lookup"><span data-stu-id="ac952-114">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="ac952-115">Database Blogging</span><span class="sxs-lookup"><span data-stu-id="ac952-115">Blogging database</span></span>

<span data-ttu-id="ac952-116">Questa esercitazione usa un database **Blogging** nell'istanza di LocalDb come database esistente.</span><span class="sxs-lookup"><span data-stu-id="ac952-116">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="ac952-117">Se si è già creato il database **Blogging** durante un'altra esercitazione, è possibile ignorare questi passaggi.</span><span class="sxs-lookup"><span data-stu-id="ac952-117">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="ac952-118">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac952-118">Open Visual Studio</span></span>
* <span data-ttu-id="ac952-119">**Strumenti -> Connetti al database**</span><span class="sxs-lookup"><span data-stu-id="ac952-119">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="ac952-120">Selezionare **Microsoft SQL Server** e fare clic su **Continua**</span><span class="sxs-lookup"><span data-stu-id="ac952-120">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="ac952-121">Immettere **(localdb)\mssqllocaldb** in **Nome server**</span><span class="sxs-lookup"><span data-stu-id="ac952-121">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="ac952-122">Immettere **master** in **Nome database** e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="ac952-122">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="ac952-123">Il database master viene ora visualizzato sotto **Connessioni dati** in **Esplora server**</span><span class="sxs-lookup"><span data-stu-id="ac952-123">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="ac952-124">Fare clic con il pulsante destro del mouse sul database in **Esplora server** e scegliere **Nuova query**</span><span class="sxs-lookup"><span data-stu-id="ac952-124">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="ac952-125">Copiare lo script, riportato sotto, nell'editor di query</span><span class="sxs-lookup"><span data-stu-id="ac952-125">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="ac952-126">Fare clic con il pulsante destro del mouse sull'editor di query e scegliere **Esegui**</span><span class="sxs-lookup"><span data-stu-id="ac952-126">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="ac952-127">Creare un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="ac952-127">Create a new project</span></span>

* <span data-ttu-id="ac952-128">Aprire Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="ac952-128">Open Visual Studio 2017</span></span>
* <span data-ttu-id="ac952-129">**File -> Nuovo -> Progetto**</span><span class="sxs-lookup"><span data-stu-id="ac952-129">**File -> New -> Project...**</span></span>
* <span data-ttu-id="ac952-130">Scegliere **Installati > Modelli > Visual C# > Web** dal menu a sinistra</span><span class="sxs-lookup"><span data-stu-id="ac952-130">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="ac952-131">Selezionare il modello di progetto **Applicazione Web ASP.NET Core (.NET Core)**</span><span class="sxs-lookup"><span data-stu-id="ac952-131">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="ac952-132">Immettere **EFGetStarted.AspNetCore.ExistingDb** come nome e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="ac952-132">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="ac952-133">Attendere che venga visualizzata la finestra di dialogo **Nuova applicazione Web ASP.NET Core**</span><span class="sxs-lookup"><span data-stu-id="ac952-133">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="ac952-134">In **ASP.NET Core Templates 2.0** (Modelli di ASP.NET Core 2.0) selezionare **Applicazione Web (MVC)**</span><span class="sxs-lookup"><span data-stu-id="ac952-134">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="ac952-135">Assicurarsi che **Autenticazione** sia impostato su **Nessuna autenticazione**</span><span class="sxs-lookup"><span data-stu-id="ac952-135">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="ac952-136">Fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="ac952-136">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="ac952-137">Installare Entity Framework</span><span class="sxs-lookup"><span data-stu-id="ac952-137">Install Entity Framework</span></span>

<span data-ttu-id="ac952-138">Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione.</span><span class="sxs-lookup"><span data-stu-id="ac952-138">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="ac952-139">Questa procedura dettagliata usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ac952-139">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="ac952-140">Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="ac952-140">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="ac952-141">**Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="ac952-141">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="ac952-142">Eseguire `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="ac952-142">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="ac952-143">Verranno usati alcuni strumenti di Entity Framework per creare un modello dal database,</span><span class="sxs-lookup"><span data-stu-id="ac952-143">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="ac952-144">quindi verrà anche installato il pacchetto di strumenti:</span><span class="sxs-lookup"><span data-stu-id="ac952-144">So we will install the tools package as well:</span></span>

* <span data-ttu-id="ac952-145">Eseguire `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="ac952-145">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="ac952-146">Più avanti verranno usati alcuni strumenti di scaffolding di ASP.NET Core per creare controller e visualizzazioni,</span><span class="sxs-lookup"><span data-stu-id="ac952-146">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="ac952-147">quindi verrà anche installato il pacchetto di progettazioni:</span><span class="sxs-lookup"><span data-stu-id="ac952-147">So we will install this design package as well:</span></span>

* <span data-ttu-id="ac952-148">Eseguire `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span><span class="sxs-lookup"><span data-stu-id="ac952-148">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="ac952-149">Decompilare il modello</span><span class="sxs-lookup"><span data-stu-id="ac952-149">Reverse engineer your model</span></span>

<span data-ttu-id="ac952-150">È ora possibile creare il modello di EF basato sul database esistente.</span><span class="sxs-lookup"><span data-stu-id="ac952-150">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="ac952-151">**Strumenti –> Gestione pacchetti NuGet –> Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="ac952-151">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="ac952-152">Eseguire il comando seguente per creare un modello dal database esistente:</span><span class="sxs-lookup"><span data-stu-id="ac952-152">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="ac952-153">Se viene visualizzato l'errore `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, chiudere e riaprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ac952-153">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="ac952-154">È possibile specificare per quali tabelle si vogliono generare le entità aggiungendo l'argomento `-Tables` al comando precedente.</span><span class="sxs-lookup"><span data-stu-id="ac952-154">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="ac952-155">Ad esempio,</span><span class="sxs-lookup"><span data-stu-id="ac952-155">E.g.</span></span> <span data-ttu-id="ac952-156">`-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="ac952-156">`-Tables Blog,Post`.</span></span>

<span data-ttu-id="ac952-157">Il processo di reverse engineering ha creato le classi di entità (`Blog.cs` & `Post.cs`) e un contesto derivato (`BloggingContext.cs`) in base allo schema del database esistente.</span><span class="sxs-lookup"><span data-stu-id="ac952-157">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="ac952-158">Le classi di entità sono semplici oggetti C# che rappresentano i dati di cui verranno eseguite query e che verranno salvati.</span><span class="sxs-lookup"><span data-stu-id="ac952-158">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="ac952-159">Il contesto rappresenta una sessione con il database e consente di eseguire query delle istanze delle classi di entità e di salvarle.</span><span class="sxs-lookup"><span data-stu-id="ac952-159">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="ac952-160">Registrare il contesto con l'inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="ac952-160">Register your context with dependency injection</span></span>

<span data-ttu-id="ac952-161">Il concetto di inserimento delle dipendenze è fondamentale per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ac952-161">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="ac952-162">I servizi (ad esempio, `BloggingContext`) vengono registrati con l'inserimento delle dipendenze durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ac952-162">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="ac952-163">Questi servizi vengono quindi forniti ai componenti per cui sono necessari (ad esempio, i controller MVC) tramite proprietà o parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="ac952-163">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="ac952-164">Per altre informazioni sull'inserimento delle dipendenze, vedere l'articolo [Inserimento delle dipendenze](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) nel sito di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ac952-164">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="ac952-165">Rimuovere la configurazione nel contesto inline</span><span class="sxs-lookup"><span data-stu-id="ac952-165">Remove inline context configuration</span></span>

<span data-ttu-id="ac952-166">In ASP.NET Core la configurazione viene in genere eseguita in **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="ac952-166">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="ac952-167">Per conformità a questo modello, la configurazione del provider di database verrà spostata in **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="ac952-167">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="ac952-168">Apertura `Models\BloggingContext.cs`</span><span class="sxs-lookup"><span data-stu-id="ac952-168">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="ac952-169">Eliminare il metodo `OnConfiguring(...)`</span><span class="sxs-lookup"><span data-stu-id="ac952-169">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="ac952-170">Aggiungere il costruttore seguente, che consente di passare la configurazione nel contesto tramite l'inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="ac952-170">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="ac952-171">Registrare e configurare il contesto in Startup.cs</span><span class="sxs-lookup"><span data-stu-id="ac952-171">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="ac952-172">`BloggingContext` verrà registrato come servizio per consentire ai controller MVC di usarlo.</span><span class="sxs-lookup"><span data-stu-id="ac952-172">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="ac952-173">Aprire **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="ac952-173">Open **Startup.cs**</span></span>
* <span data-ttu-id="ac952-174">Aggiungere le istruzioni `using` seguenti all'inizio del file</span><span class="sxs-lookup"><span data-stu-id="ac952-174">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="ac952-175">È ora possibile usare il metodo `AddDbContext(...)` per registrarlo come servizio.</span><span class="sxs-lookup"><span data-stu-id="ac952-175">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="ac952-176">Individuare il metodo `ConfigureServices(...)`</span><span class="sxs-lookup"><span data-stu-id="ac952-176">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="ac952-177">Aggiungere il codice seguente per registrare il contesto come servizio</span><span class="sxs-lookup"><span data-stu-id="ac952-177">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="ac952-178">In una vera applicazione in genere si inserisce la stringa di connessione in un file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="ac952-178">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="ac952-179">Per ragioni di semplicità, viene definita nel codice.</span><span class="sxs-lookup"><span data-stu-id="ac952-179">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="ac952-180">Per altre informazioni, vedere [Connection Strings](../../miscellaneous/connection-strings.md) (Stringhe di connessione).</span><span class="sxs-lookup"><span data-stu-id="ac952-180">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="ac952-181">Creare un controller</span><span class="sxs-lookup"><span data-stu-id="ac952-181">Create a controller</span></span>

<span data-ttu-id="ac952-182">Verrà ora abilitato lo scaffolding nel progetto.</span><span class="sxs-lookup"><span data-stu-id="ac952-182">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="ac952-183">Fare clic con il pulsante destro del mouse sulla cartella **Controllers** in **Esplora soluzioni** e scegliere **Aggiungi -> Controller**.</span><span class="sxs-lookup"><span data-stu-id="ac952-183">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="ac952-184">Selezionare **Dipendenze complete** e fare clic su **Aggiungi**</span><span class="sxs-lookup"><span data-stu-id="ac952-184">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="ac952-185">È possibile ignorare le istruzioni nel file `ScaffoldingReadMe.txt` visualizzato</span><span class="sxs-lookup"><span data-stu-id="ac952-185">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="ac952-186">Quando è abilitato, è possibile eseguire lo scaffolding di un controller per l'entità `Blog`.</span><span class="sxs-lookup"><span data-stu-id="ac952-186">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="ac952-187">Fare clic con il pulsante destro del mouse sulla cartella **Controllers** in **Esplora soluzioni** e scegliere **Aggiungi -> Controller**.</span><span class="sxs-lookup"><span data-stu-id="ac952-187">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="ac952-188">Selezionare **Controller MVC con visualizzazioni, che usa Entity Framework** e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="ac952-188">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="ac952-189">Impostare **Classe modello** su **Blog** e **Classe contesto dati** su **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="ac952-189">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="ac952-190">Fare clic su **Aggiungi**</span><span class="sxs-lookup"><span data-stu-id="ac952-190">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="ac952-191">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="ac952-191">Run the application</span></span>

<span data-ttu-id="ac952-192">È ora possibile eseguire l'applicazione per vederla in azione.</span><span class="sxs-lookup"><span data-stu-id="ac952-192">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="ac952-193">**Debug - > Avvia senza eseguire debug**</span><span class="sxs-lookup"><span data-stu-id="ac952-193">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="ac952-194">L'applicazione verrà compilata e aperta in un Web browser</span><span class="sxs-lookup"><span data-stu-id="ac952-194">The application will build and open in a web browser</span></span>
* <span data-ttu-id="ac952-195">Passare a `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="ac952-195">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="ac952-196">Fare clic su **Crea nuovo**</span><span class="sxs-lookup"><span data-stu-id="ac952-196">Click **Create New**</span></span>
* <span data-ttu-id="ac952-197">Immettere un **URL** per il nuovo blog e fare clic su **Crea**</span><span class="sxs-lookup"><span data-stu-id="ac952-197">Enter a **Url** for the new blog and click **Create**</span></span>

![image](_static/create.png)

![image](_static/index-existing-db.png)
