---
title: Code First per un Database esistente - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
caps.latest.revision: 3
ms.openlocfilehash: 47581a7ae9ff534d26ce82365bcbe2b254247495
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121125"
---
# <a name="code-first-to-an-existing-database"></a><span data-ttu-id="6b564-102">Code First per un Database esistente</span><span class="sxs-lookup"><span data-stu-id="6b564-102">Code First to an Existing Database</span></span>
<span data-ttu-id="6b564-103">Questa procedura dettagliata video e dettagliata forniscono un'introduzione allo sviluppo Code First come destinazione di un database esistente.</span><span class="sxs-lookup"><span data-stu-id="6b564-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting an existing database.</span></span> <span data-ttu-id="6b564-104">Codice prima di tutto consente di definire il modello usando C\# o classi di Visual Basic.NET.</span><span class="sxs-lookup"><span data-stu-id="6b564-104">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="6b564-105">Configurazione aggiuntiva, facoltativamente, può essere eseguita usando gli attributi delle classi e proprietà o tramite un'API fluent.</span><span class="sxs-lookup"><span data-stu-id="6b564-105">Optionally additional configuration can be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="6b564-106">Guarda il video</span><span class="sxs-lookup"><span data-stu-id="6b564-106">Watch the video</span></span>
<span data-ttu-id="6b564-107">In questo video viene [ora disponibile su Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span><span class="sxs-lookup"><span data-stu-id="6b564-107">This video is [now available on Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="6b564-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="6b564-108">Pre-Requisites</span></span>

<span data-ttu-id="6b564-109">Dovrai disporre **Visual Studio 2012** oppure **Visual Studio 2013** per completare questa procedura dettagliata.</span><span class="sxs-lookup"><span data-stu-id="6b564-109">You will need to have **Visual Studio 2012** or **Visual Studio 2013** installed to complete this walkthrough.</span></span>

<span data-ttu-id="6b564-110">È anche necessaria versione **6.1** (o versioni successive) delle **Entity Framework Tools per Visual Studio** installato.</span><span class="sxs-lookup"><span data-stu-id="6b564-110">You will also need version **6.1** (or later) of the **Entity Framework Tools for Visual Studio** installed.</span></span> <span data-ttu-id="6b564-111">Visualizzare [ottenere Entity Framework](~/ef6/fundamentals/install.md) per informazioni sull'installazione della versione più recente di Entity Framework Tools.</span><span class="sxs-lookup"><span data-stu-id="6b564-111">See [Get Entity Framework](~/ef6/fundamentals/install.md) for information on installing the latest version of the Entity Framework Tools.</span></span>

## <a name="1-create-an-existing-database"></a><span data-ttu-id="6b564-112">1. Creare un Database esistente</span><span class="sxs-lookup"><span data-stu-id="6b564-112">1. Create an Existing Database</span></span>

<span data-ttu-id="6b564-113">In genere quando si intende usare un database esistente che verrà già creato, ma per questa procedura dettagliata è necessario creare un database a cui accedere.</span><span class="sxs-lookup"><span data-stu-id="6b564-113">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="6b564-114">È possibile procedere e generare il database.</span><span class="sxs-lookup"><span data-stu-id="6b564-114">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="6b564-115">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6b564-115">Open Visual Studio</span></span>
-   <span data-ttu-id="6b564-116">**Visualizzazione -&gt; Esplora Server**</span><span class="sxs-lookup"><span data-stu-id="6b564-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="6b564-117">Fare clic con il pulsante destro sul **connessioni dati -&gt; Aggiungi connessione...**</span><span class="sxs-lookup"><span data-stu-id="6b564-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="6b564-118">Se si è ancora connessi a un database da **Esplora Server** prima di, devi selezionare **Microsoft SQL Server** come origine dati</span><span class="sxs-lookup"><span data-stu-id="6b564-118">If you haven’t connected to a database from **Server Explorer** before you’ll need to select **Microsoft SQL Server** as the data source</span></span>

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="6b564-120">Connettersi all'istanza di Local DB e immettere **Blogging** come nome del database</span><span class="sxs-lookup"><span data-stu-id="6b564-120">Connect to your LocalDB instance, and enter **Blogging** as the database name</span></span>

    ![LocalDBConnection](~/ef6/media/localdbconnection.png)

-   <span data-ttu-id="6b564-122">Selezionare **OK** e verrà richiesto se si desidera creare un nuovo database, selezionare **Sì**</span><span class="sxs-lookup"><span data-stu-id="6b564-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![CreateDatabaseDialog](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="6b564-124">Il nuovo database verrà ora visualizzato in Esplora Server, su di esso e scegliere **nuova Query**</span><span class="sxs-lookup"><span data-stu-id="6b564-124">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="6b564-125">Copiare il codice SQL seguente nella nuova query, quindi fare clic su query e selezionare **Execute**</span><span class="sxs-lookup"><span data-stu-id="6b564-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a><span data-ttu-id="6b564-126">2. Creare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="6b564-126">2. Create the Application</span></span>

<span data-ttu-id="6b564-127">Per semplificare le operazioni verranno creare un'applicazione console di base che usa Code First per eseguire l'accesso ai dati:</span><span class="sxs-lookup"><span data-stu-id="6b564-127">To keep things simple we’re going to build a basic console application that uses Code First to perform data access:</span></span>

-   <span data-ttu-id="6b564-128">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6b564-128">Open Visual Studio</span></span>
-   <span data-ttu-id="6b564-129">**File -&gt; New -&gt; progetto...**</span><span class="sxs-lookup"><span data-stu-id="6b564-129">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="6b564-130">Selezionare **Windows** nel menu a sinistra e **applicazione Console**</span><span class="sxs-lookup"><span data-stu-id="6b564-130">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="6b564-131">Immettere **CodeFirstExistingDatabaseSample** come nome</span><span class="sxs-lookup"><span data-stu-id="6b564-131">Enter **CodeFirstExistingDatabaseSample** as the name</span></span>
-   <span data-ttu-id="6b564-132">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b564-132">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="6b564-133">3. Modelli di reverse engineering</span><span class="sxs-lookup"><span data-stu-id="6b564-133">3. Reverse Engineer Model</span></span>

<span data-ttu-id="6b564-134">Dobbiamo usare Entity Framework Tools per Visual Studio per consentire agli utenti di generare il codice iniziale per eseguire il mapping al database.</span><span class="sxs-lookup"><span data-stu-id="6b564-134">We’re going to make use of the Entity Framework Tools for Visual Studio to help us generate some initial code to map to the database.</span></span> <span data-ttu-id="6b564-135">Questi strumenti sono semplicemente la generazione del codice che è possibile anche digitare manualmente se si preferisce.</span><span class="sxs-lookup"><span data-stu-id="6b564-135">These tools are just generating code that you could also type by hand if you prefer.</span></span>

-   <span data-ttu-id="6b564-136">**Progetto -&gt; Aggiungi nuovo elemento...**</span><span class="sxs-lookup"><span data-stu-id="6b564-136">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="6b564-137">Selezionare **Data** nel menu a sinistra e quindi **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="6b564-137">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="6b564-138">Immettere **BloggingContext** come nome, quindi fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="6b564-138">Enter **BloggingContext** as the name and click **OK**</span></span>
-   <span data-ttu-id="6b564-139">Verrà avviata la **procedura guidata Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="6b564-139">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="6b564-140">Selezionare **Code First dal Database** e fare clic su **successivo**</span><span class="sxs-lookup"><span data-stu-id="6b564-140">Select **Code First from Database** and click **Next**</span></span>

    ![WizardOneCFE](~/ef6/media/wizardonecfe.png)

-   <span data-ttu-id="6b564-142">Selezionare la connessione al database creato nella prima sezione e fare clic su **successivo**</span><span class="sxs-lookup"><span data-stu-id="6b564-142">Select the connection to the database you created in the first section and click **Next**</span></span>

    ![WizardTwoCFE](~/ef6/media/wizardtwocfe.png)

-   <span data-ttu-id="6b564-144">Fare clic sulla casella di controllo accanto a **tabelle** per importare tutte le tabelle e fare clic su **fine**</span><span class="sxs-lookup"><span data-stu-id="6b564-144">Click the checkbox next to **Tables** to import all tables and click **Finish**</span></span>

    ![WizardThreeCFE](~/ef6/media/wizardthreecfe.png)

<span data-ttu-id="6b564-146">Una volta che un numero di elementi viene completato il processo di reverse engineering verranno aggiunti al progetto, è possibile ottenere un quadro di ciò che è stato aggiunto.</span><span class="sxs-lookup"><span data-stu-id="6b564-146">Once the reverse engineer process completes a number of items will have been added to the project, let's take a look at what's been added.</span></span>

### <a name="configuration-file"></a><span data-ttu-id="6b564-147">File di configurazione</span><span class="sxs-lookup"><span data-stu-id="6b564-147">Configuration file</span></span>

<span data-ttu-id="6b564-148">Un file app. config è stato aggiunto al progetto, questo file contiene la stringa di connessione al database esistente.</span><span class="sxs-lookup"><span data-stu-id="6b564-148">An App.config file has been added to the project, this file contains the connection string to the existing database.</span></span>

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

<span data-ttu-id="6b564-149">*Si noterà anche diverse altre impostazioni nel file di configurazione, queste sono impostazioni di Entity Framework predefinite che indicano a Code First in cui creare i database. Poiché viene mappato a un database esistente, queste impostazioni verranno ignorate nella nostra applicazione.*</span><span class="sxs-lookup"><span data-stu-id="6b564-149">*You’ll notice some other settings in the configuration file too, these are default EF settings that tell Code First where to create databases. Since we are mapping to an existing database these setting will be ignored in our application.*</span></span>

### <a name="derived-context"></a><span data-ttu-id="6b564-150">Contesto derivato</span><span class="sxs-lookup"><span data-stu-id="6b564-150">Derived Context</span></span>

<span data-ttu-id="6b564-151">Oggetto **BloggingContext** classe è stato aggiunto al progetto.</span><span class="sxs-lookup"><span data-stu-id="6b564-151">A **BloggingContext** class has been added to the project.</span></span> <span data-ttu-id="6b564-152">Il contesto rappresenta una sessione con il database, che consente di eseguire una query e salvare i dati.</span><span class="sxs-lookup"><span data-stu-id="6b564-152">The context represents a session with the database, allowing us to query and save data.</span></span>
<span data-ttu-id="6b564-153">Espone il contesto di un **DbSet&lt;TEntity&gt;**  per ogni tipo nel modello.</span><span class="sxs-lookup"><span data-stu-id="6b564-153">The context exposes a **DbSet&lt;TEntity&gt;** for each type in our model.</span></span> <span data-ttu-id="6b564-154">Si noterà anche che il costruttore predefinito chiama un costruttore di base usando la **nome =** sintassi.</span><span class="sxs-lookup"><span data-stu-id="6b564-154">You’ll also notice that the default constructor calls a base constructor using the **name=** syntax.</span></span> <span data-ttu-id="6b564-155">Indica a Code First che la stringa di connessione da utilizzare per questo contesto deve essere caricata dal file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="6b564-155">This tells Code First that the connection string to use for this context should be loaded from the configuration file.</span></span>

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

<span data-ttu-id="6b564-156">*È consigliabile usare sempre la **nome =** sintassi quando si usa una stringa di connessione nel file di configurazione. In questo modo si garantisce che se la stringa di connessione non è presente l'Entity Framework genererà invece di creare un nuovo database per convenzione.*</span><span class="sxs-lookup"><span data-stu-id="6b564-156">*You should always use the **name=** syntax when you are using a connection string in the config file. This ensures that if the connection string is not present then Entity Framework will throw rather than creating a new database by convention.*</span></span>

### <a name="model-classes"></a><span data-ttu-id="6b564-157">Classi del modello</span><span class="sxs-lookup"><span data-stu-id="6b564-157">Model classes</span></span>

<span data-ttu-id="6b564-158">Infine, un **Blog** e **Post** classe sono stati aggiunti anche al progetto.</span><span class="sxs-lookup"><span data-stu-id="6b564-158">Finally, a **Blog** and **Post** class have also been added to the project.</span></span> <span data-ttu-id="6b564-159">Queste sono le classi di dominio che costituiscono il modello.</span><span class="sxs-lookup"><span data-stu-id="6b564-159">These are the domain classes that make up our model.</span></span> <span data-ttu-id="6b564-160">Si noterà annotazioni dei dati applicate alle classi per specificare la configurazione in cui le convenzioni Code First non sarà allineata con la struttura del database esistente.</span><span class="sxs-lookup"><span data-stu-id="6b564-160">You'll see Data Annotations applied to the classes to specify configuration where the Code First conventions would not align with the structure of the existing database.</span></span> <span data-ttu-id="6b564-161">Ad esempio, si noterà il **StringLength** annotazione in **Blog.Name** e **Blog.Url** poiché presentano una lunghezza massima di **200** nel database (il valore predefinito di Code First consiste nell'usare la lunghezza massima supportata dal provider di database - **nvarchar (max)** in SQL Server).</span><span class="sxs-lookup"><span data-stu-id="6b564-161">For example, you'll see the **StringLength** annotation on **Blog.Name** and **Blog.Url** since they have a maximum length of **200** in the database (the Code First default is to use the maximun length supported by the database provider - **nvarchar(max)** in SQL Server).</span></span>

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a><span data-ttu-id="6b564-162">4. La lettura e scrittura dei dati</span><span class="sxs-lookup"><span data-stu-id="6b564-162">4. Reading & Writing Data</span></span>

<span data-ttu-id="6b564-163">Ora che abbiamo un modello è possibile usarlo per accedere ai dati.</span><span class="sxs-lookup"><span data-stu-id="6b564-163">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="6b564-164">Implementare il **Main** metodo nella **Program.cs** come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="6b564-164">Implement the **Main** method in **Program.cs** as shown below.</span></span> <span data-ttu-id="6b564-165">Questo codice crea una nuova istanza del contesto e quindi lo usa per inserire un nuovo **Blog**.</span><span class="sxs-lookup"><span data-stu-id="6b564-165">This code creates a new instance of our context and then uses it to insert a new **Blog**.</span></span> <span data-ttu-id="6b564-166">Quindi, viene utilizzata una query LINQ per recuperare tutti **blog** dal database ordinato alfabeticamente in base **titolo**.</span><span class="sxs-lookup"><span data-stu-id="6b564-166">Then it uses a LINQ query to retrieve all **Blogs** from the database ordered alphabetically by **Title**.</span></span>

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="6b564-167">È ora possibile eseguire l'applicazione e testarlo.</span><span class="sxs-lookup"><span data-stu-id="6b564-167">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a><span data-ttu-id="6b564-168">Cosa accade se cambia mio Database?</span><span class="sxs-lookup"><span data-stu-id="6b564-168">What if My Database Changes?</span></span>

<span data-ttu-id="6b564-169">Code First guidata Database è progettato per generare un set di punto di partenza di classi che è possibile quindi perfezionare e modificare.</span><span class="sxs-lookup"><span data-stu-id="6b564-169">The Code First to Database wizard is designed to generate a starting point set of classes that you can then tweak and modify.</span></span> <span data-ttu-id="6b564-170">Se viene modificato lo schema del database è possibile manualmente modificare le classi o eseguire un altro reverse engineering per sovrascrivere le classi.</span><span class="sxs-lookup"><span data-stu-id="6b564-170">If your database schema changes you can either manually edit the classes or perform another reverse engineer to overwrite the classes.</span></span>

## <a name="using-code-first-migrations-to-an-existing-database"></a><span data-ttu-id="6b564-171">Con migrazioni Code First per un Database esistente</span><span class="sxs-lookup"><span data-stu-id="6b564-171">Using Code First Migrations to an Existing Database</span></span>

<span data-ttu-id="6b564-172">Se si desidera utilizzare migrazioni Code First con un database esistente, vedere [migrazioni Code First per un database esistente](~/ef6/modeling/code-first/migrations/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="6b564-172">If you want to use Code First Migrations with an existing database, see [Code First Migrations to an existing database](~/ef6/modeling/code-first/migrations/existing-database.md).</span></span>

## <a name="summary"></a><span data-ttu-id="6b564-173">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="6b564-173">Summary</span></span>

<span data-ttu-id="6b564-174">In questa procedura dettagliata viene esaminato lo sviluppo Code First usando un database esistente.</span><span class="sxs-lookup"><span data-stu-id="6b564-174">In this walkthrough we looked at Code First development using an existing database.</span></span> <span data-ttu-id="6b564-175">Abbiamo usato Entity Framework Tools per Visual Studio per decompilare un set di classi che è mappato al database e poteva essere usato per archiviare e recuperare i dati.</span><span class="sxs-lookup"><span data-stu-id="6b564-175">We used the Entity Framework Tools for Visual Studio to reverse engineer a set of classes that mapped to the database and could be used to store and retrieve data.</span></span>