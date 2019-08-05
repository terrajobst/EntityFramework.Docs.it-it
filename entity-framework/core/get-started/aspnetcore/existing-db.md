---
title: Guida introduttiva ad ASP.NET Core - Database esistente - EF Core
author: rowanmiller
description: Introduzione a EF Core in ASP.NET Core con un database esistente
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: 6b0ed0a9222644bee31d23234aa27b2084137f4a
ms.sourcegitcommit: 755a15a789631cc4ea581e2262a2dcc49c219eef
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/25/2019
ms.locfileid: "68497518"
---
# <a name="get-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="3c706-103">Guida introduttiva a EF Core in ASP.NET Core con un database esistente</span><span class="sxs-lookup"><span data-stu-id="3c706-103">Get started with EF Core on ASP.NET Core with an existing database</span></span>

<span data-ttu-id="3c706-104">In questa esercitazione verrà creata un'applicazione ASP.NET Core MVC che esegue l'accesso ai dati di base usando Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="3c706-104">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="3c706-105">Si creerà un modello di Entity Framework tramite reverse engineering di un database esistente.</span><span class="sxs-lookup"><span data-stu-id="3c706-105">You reverse engineer an existing database to create an Entity Framework model.</span></span>

<span data-ttu-id="3c706-106">[Visualizzare l'esempio di questo articolo su GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="3c706-106">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c706-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="3c706-107">Prerequisites</span></span>

<span data-ttu-id="3c706-108">Installare il software seguente:</span><span class="sxs-lookup"><span data-stu-id="3c706-108">Install the following software:</span></span>

* <span data-ttu-id="3c706-109">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) con questi carichi di lavoro:</span><span class="sxs-lookup"><span data-stu-id="3c706-109">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="3c706-110">**Sviluppo ASP.NET e Web** (in **Web e Cloud**)</span><span class="sxs-lookup"><span data-stu-id="3c706-110">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="3c706-111">**Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)</span><span class="sxs-lookup"><span data-stu-id="3c706-111">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="3c706-112">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="3c706-112">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-blogging-database"></a><span data-ttu-id="3c706-113">Creare il database Blogging</span><span class="sxs-lookup"><span data-stu-id="3c706-113">Create Blogging database</span></span>

<span data-ttu-id="3c706-114">Questa esercitazione usa un database **Blogging** nell'istanza di LocalDb come database esistente.</span><span class="sxs-lookup"><span data-stu-id="3c706-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span> <span data-ttu-id="3c706-115">Se il database **Blogging** è già stato creato durante un'altra esercitazione, ignorare questi passaggi.</span><span class="sxs-lookup"><span data-stu-id="3c706-115">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="3c706-116">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c706-116">Open Visual Studio</span></span>
* <span data-ttu-id="3c706-117">**Strumenti -> Connetti al database**</span><span class="sxs-lookup"><span data-stu-id="3c706-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="3c706-118">Selezionare **Microsoft SQL Server** e fare clic su **Continua**</span><span class="sxs-lookup"><span data-stu-id="3c706-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="3c706-119">Immettere **(localdb)\mssqllocaldb** in **Nome server**</span><span class="sxs-lookup"><span data-stu-id="3c706-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="3c706-120">Immettere **master** in **Nome database** e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="3c706-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="3c706-121">Il database master viene ora visualizzato sotto **Connessioni dati** in **Esplora server**</span><span class="sxs-lookup"><span data-stu-id="3c706-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="3c706-122">Fare clic con il pulsante destro del mouse sul database in **Esplora server** e scegliere **Nuova query**</span><span class="sxs-lookup"><span data-stu-id="3c706-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="3c706-123">Copiare lo script riportato di seguito nell'editor di query</span><span class="sxs-lookup"><span data-stu-id="3c706-123">Copy the script listed below into the query editor</span></span>
* <span data-ttu-id="3c706-124">Fare clic con il pulsante destro del mouse sull'editor di query e scegliere **Esegui**</span><span class="sxs-lookup"><span data-stu-id="3c706-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="3c706-125">Creare un nuovo progetto</span><span class="sxs-lookup"><span data-stu-id="3c706-125">Create a new project</span></span>

* <span data-ttu-id="3c706-126">Aprire Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="3c706-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="3c706-127">**File > Nuovo > Progetto**</span><span class="sxs-lookup"><span data-stu-id="3c706-127">**File > New > Project...**</span></span>
* <span data-ttu-id="3c706-128">Scegliere **Installati > Visual C# > Web** dal menu a sinistra</span><span class="sxs-lookup"><span data-stu-id="3c706-128">From the left menu select **Installed > Visual C# > Web**</span></span>
* <span data-ttu-id="3c706-129">Selezionare il modello di progetto **Applicazione Web ASP.NET Core**</span><span class="sxs-lookup"><span data-stu-id="3c706-129">Select the **ASP.NET Core Web Application** project template</span></span>
* <span data-ttu-id="3c706-130">Immettere **EFGetStarted.AspNetCore.ExistingDb** come nome (deve corrispondere esattamente allo spazio dei nomi usato successivamente nel codice) e quindi fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="3c706-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name (it has to match exactly the namespace later used in the code) and click **OK**</span></span> 
* <span data-ttu-id="3c706-131">Attendere che venga visualizzata la finestra di dialogo **Nuova applicazione Web ASP.NET Core**</span><span class="sxs-lookup"><span data-stu-id="3c706-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="3c706-132">Assicurarsi che l'elenco a discesa dei framework di destinazione sia impostato su **.NET Core** e che l'elenco a discesa delle versioni sia impostato su **ASP.NET Core 2.1**</span><span class="sxs-lookup"><span data-stu-id="3c706-132">Make sure that the target framework dropdown is set to **.NET Core**, and the version dropdown is set to **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="3c706-133">Selezionare il modello **Applicazione Web (MVC)**</span><span class="sxs-lookup"><span data-stu-id="3c706-133">Select the **Web Application (Model-View-Controller)** template</span></span>
* <span data-ttu-id="3c706-134">Assicurarsi che **Autenticazione** sia impostato su **Nessuna autenticazione**</span><span class="sxs-lookup"><span data-stu-id="3c706-134">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="3c706-135">Fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="3c706-135">Click **OK**</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="3c706-136">Installare Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="3c706-136">Install Entity Framework Core</span></span>

<span data-ttu-id="3c706-137">Per installare EF Core, installare il pacchetto per i provider di database di EF Core che si vuole usare come destinazione.</span><span class="sxs-lookup"><span data-stu-id="3c706-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="3c706-138">Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="3c706-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="3c706-139">Per questa esercitazione, non è necessario installare un pacchetto di provider perché l'esercitazione usa SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3c706-139">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="3c706-140">Il pacchetto del provider SQL Server è incluso nel [metapacchetto Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="3c706-140">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="3c706-141">Decompilare il modello</span><span class="sxs-lookup"><span data-stu-id="3c706-141">Reverse engineer your model</span></span>

<span data-ttu-id="3c706-142">È ora possibile creare il modello di EF basato sul database esistente.</span><span class="sxs-lookup"><span data-stu-id="3c706-142">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="3c706-143">**Strumenti –> Gestione pacchetti NuGet –> Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="3c706-143">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="3c706-144">Eseguire il comando seguente per creare un modello dal database esistente:</span><span class="sxs-lookup"><span data-stu-id="3c706-144">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="3c706-145">Se viene visualizzato l'errore `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, chiudere e riaprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3c706-145">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="3c706-146">È possibile specificare per quali tabelle si vogliono generare le entità aggiungendo l'argomento `-Tables` al comando precedente.</span><span class="sxs-lookup"><span data-stu-id="3c706-146">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="3c706-147">Ad esempio `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="3c706-147">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="3c706-148">Il processo di reverse engineering ha creato le classi di entità (`Blog.cs` & `Post.cs`) e un contesto derivato (`BloggingContext.cs`) in base allo schema del database esistente.</span><span class="sxs-lookup"><span data-stu-id="3c706-148">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="3c706-149">Le classi di entità sono semplici oggetti C# che rappresentano i dati di cui verranno eseguite query e che verranno salvati.</span><span class="sxs-lookup"><span data-stu-id="3c706-149">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="3c706-150">Di seguito sono riportate le classi di entità `Blog` e `Post`:</span><span class="sxs-lookup"><span data-stu-id="3c706-150">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> <span data-ttu-id="3c706-151">Per abilitare il caricamento lazy, è possibile impostare proprietà di navigazione `virtual` (Blog.Post e Post.Blog).</span><span class="sxs-lookup"><span data-stu-id="3c706-151">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

 <span data-ttu-id="3c706-152">Il contesto rappresenta una sessione con il database e consente di eseguire query delle istanze delle classi di entità e di salvarle.</span><span class="sxs-lookup"><span data-stu-id="3c706-152">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the tutorial modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public BloggingContext()
    {
    }

    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    {
    }

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="3c706-153">Registrare il contesto con l'inserimento delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="3c706-153">Register your context with dependency injection</span></span>

<span data-ttu-id="3c706-154">Il concetto di inserimento delle dipendenze è fondamentale per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c706-154">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="3c706-155">I servizi (ad esempio, `BloggingContext`) vengono registrati con l'inserimento delle dipendenze durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="3c706-155">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="3c706-156">Questi servizi vengono quindi forniti ai componenti per cui sono necessari (ad esempio, i controller MVC) tramite proprietà o parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="3c706-156">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="3c706-157">Per altre informazioni sull'inserimento delle dipendenze, vedere l'articolo [Inserimento delle dipendenze](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) nel sito di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3c706-157">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="3c706-158">Registrare e configurare il contesto in Startup.cs</span><span class="sxs-lookup"><span data-stu-id="3c706-158">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="3c706-159">Per rendere `BloggingContext` disponibile per i controller MVC, registrarlo come servizio.</span><span class="sxs-lookup"><span data-stu-id="3c706-159">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="3c706-160">Aprire **Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="3c706-160">Open **Startup.cs**</span></span>
* <span data-ttu-id="3c706-161">Aggiungere le istruzioni `using` seguenti all'inizio del file</span><span class="sxs-lookup"><span data-stu-id="3c706-161">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="3c706-162">È ora possibile usare il metodo `AddDbContext(...)` per registrarlo come servizio.</span><span class="sxs-lookup"><span data-stu-id="3c706-162">Now you can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="3c706-163">Individuare il metodo `ConfigureServices(...)`</span><span class="sxs-lookup"><span data-stu-id="3c706-163">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="3c706-164">Aggiungere il codice evidenziato seguente per registrare il contesto come servizio</span><span class="sxs-lookup"><span data-stu-id="3c706-164">Add the following highlighted code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> <span data-ttu-id="3c706-165">In una vera applicazione in genere si inserisce la stringa di connessione in un file di configurazione o una variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="3c706-165">In a real application you would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="3c706-166">Per ragioni di semplicità, per questa esercitazione viene definita nel codice.</span><span class="sxs-lookup"><span data-stu-id="3c706-166">For the sake of simplicity, this tutorial has you define it in code.</span></span> <span data-ttu-id="3c706-167">Per altre informazioni, vedere [Connection Strings](../../miscellaneous/connection-strings.md) (Stringhe di connessione).</span><span class="sxs-lookup"><span data-stu-id="3c706-167">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="3c706-168">Creare controller e visualizzazioni</span><span class="sxs-lookup"><span data-stu-id="3c706-168">Create a controller and views</span></span>

* <span data-ttu-id="3c706-169">Fare clic con il pulsante destro del mouse sulla cartella **Controllers** in **Esplora soluzioni** e scegliere **Aggiungi -> Controller**.</span><span class="sxs-lookup"><span data-stu-id="3c706-169">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="3c706-170">Selezionare **Controller MVC con visualizzazioni, che usa Entity Framework** e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="3c706-170">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="3c706-171">Impostare **Classe modello** su **Blog** e **Classe contesto dati** su **BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="3c706-171">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="3c706-172">Fare clic su **Aggiungi**</span><span class="sxs-lookup"><span data-stu-id="3c706-172">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="3c706-173">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="3c706-173">Run the application</span></span>

<span data-ttu-id="3c706-174">È ora possibile eseguire l'applicazione per vederla in azione.</span><span class="sxs-lookup"><span data-stu-id="3c706-174">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="3c706-175">**Debug - > Avvia senza eseguire debug**</span><span class="sxs-lookup"><span data-stu-id="3c706-175">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="3c706-176">L'applicazione viene compilata e aperta in un Web browser</span><span class="sxs-lookup"><span data-stu-id="3c706-176">The application builds and opens in a web browser</span></span>
* <span data-ttu-id="3c706-177">Passare a `/Blogs`</span><span class="sxs-lookup"><span data-stu-id="3c706-177">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="3c706-178">Fare clic su **Crea nuovo**</span><span class="sxs-lookup"><span data-stu-id="3c706-178">Click **Create New**</span></span>
* <span data-ttu-id="3c706-179">Immettere un **URL** per il nuovo blog e fare clic su **Crea**</span><span class="sxs-lookup"><span data-stu-id="3c706-179">Enter a **Url** for the new blog and click **Create**</span></span>

  ![Pagina Crea](_static/create.png)

  ![Pagina Index (Indice)](_static/index-existing-db.png)

## <a name="next-steps"></a><span data-ttu-id="3c706-182">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="3c706-182">Next steps</span></span>

<span data-ttu-id="3c706-183">Per altre informazioni su come eseguire lo scaffolding di un contesto e delle classi di entità, vedere gli articoli seguenti:</span><span class="sxs-lookup"><span data-stu-id="3c706-183">For more information about how to scaffold a context and entity classes, see the following articles:</span></span>
* [<span data-ttu-id="3c706-184">Reverse engineering</span><span class="sxs-lookup"><span data-stu-id="3c706-184">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
* [<span data-ttu-id="3c706-185">Informazioni di riferimento sugli strumenti di Entity Framework Core - Interfaccia della riga di comando di .NET</span><span class="sxs-lookup"><span data-stu-id="3c706-185">Entity Framework Core tools reference - .NET CLI</span></span>](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [<span data-ttu-id="3c706-186">Informazioni di riferimento sugli strumenti di Entity Framework Core - Console di Gestione pacchetti</span><span class="sxs-lookup"><span data-stu-id="3c706-186">Entity Framework Core tools reference - Package Manager Console</span></span>](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)
