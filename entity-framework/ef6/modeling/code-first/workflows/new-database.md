---
title: Code First per un nuovo Database - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: bb44a3300bc8ffc9d7050c4784e7b76b29c61796
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994618"
---
# <a name="code-first-to-a-new-database"></a><span data-ttu-id="f1f65-102">Code First per un nuovo Database</span><span class="sxs-lookup"><span data-stu-id="f1f65-102">Code First to a New Database</span></span>
<span data-ttu-id="f1f65-103">Questa procedura dettagliata video e dettagliata forniscono un'introduzione allo sviluppo Code First come destinazione di un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="f1f65-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="f1f65-104">Questo scenario include come destinazione di un database che non esiste e Code First creerà un database vuoto che Code First la aggiungerà nuove tabelle troppo.</span><span class="sxs-lookup"><span data-stu-id="f1f65-104">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables too.</span></span> <span data-ttu-id="f1f65-105">Codice prima di tutto consente di definire il modello usando C\# o classi di Visual Basic.NET.</span><span class="sxs-lookup"><span data-stu-id="f1f65-105">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="f1f65-106">Configurazione aggiuntiva, facoltativamente, può essere eseguita usando gli attributi delle classi e proprietà o tramite un'API fluent.</span><span class="sxs-lookup"><span data-stu-id="f1f65-106">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="f1f65-107">Guarda il video</span><span class="sxs-lookup"><span data-stu-id="f1f65-107">Watch the video</span></span>
<span data-ttu-id="f1f65-108">Questo video viene fornita un'introduzione allo sviluppo Code First come destinazione di un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="f1f65-108">This video provides an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="f1f65-109">Questo scenario include come destinazione di un database che non esiste e Code First creerà un database vuoto che Code First la aggiungerà nuove tabelle troppo.</span><span class="sxs-lookup"><span data-stu-id="f1f65-109">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables too.</span></span> <span data-ttu-id="f1f65-110">Codice prima di tutto consente di definire il modello di utilizzo di classi c# o VB.Net.</span><span class="sxs-lookup"><span data-stu-id="f1f65-110">Code First allows you to define your model using C# or VB.Net classes.</span></span> <span data-ttu-id="f1f65-111">Configurazione aggiuntiva, facoltativamente, può essere eseguita usando gli attributi delle classi e proprietà o tramite un'API fluent.</span><span class="sxs-lookup"><span data-stu-id="f1f65-111">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

<span data-ttu-id="f1f65-112">**Presentato da**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="f1f65-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="f1f65-113">**Video**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span><span class="sxs-lookup"><span data-stu-id="f1f65-113">**Video**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span></span>

# <a name="pre-requisites"></a><span data-ttu-id="f1f65-114">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="f1f65-114">Pre-Requisites</span></span>

<span data-ttu-id="f1f65-115">È necessario avere almeno Visual Studio 2010 o Visual Studio 2012 è installato per completare questa procedura dettagliata.</span><span class="sxs-lookup"><span data-stu-id="f1f65-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="f1f65-116">Se si usa Visual Studio 2010, è necessario anche avere [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installato.</span><span class="sxs-lookup"><span data-stu-id="f1f65-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="f1f65-117">1. Creare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="f1f65-117">1. Create the Application</span></span>

<span data-ttu-id="f1f65-118">Per semplificare le operazioni verranno creare un'applicazione console di base che usa Code First per eseguire l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="f1f65-118">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="f1f65-119">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f1f65-119">Open Visual Studio</span></span>
-   <span data-ttu-id="f1f65-120">**File -&gt; New -&gt; progetto...**</span><span class="sxs-lookup"><span data-stu-id="f1f65-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="f1f65-121">Selezionare **Windows** nel menu a sinistra e **applicazione Console**</span><span class="sxs-lookup"><span data-stu-id="f1f65-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="f1f65-122">Immettere **CodeFirstNewDatabaseSample** come nome</span><span class="sxs-lookup"><span data-stu-id="f1f65-122">Enter **CodeFirstNewDatabaseSample** as the name</span></span>
-   <span data-ttu-id="f1f65-123">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="f1f65-123">Select **OK**</span></span>

## <a name="2-create-the-model"></a><span data-ttu-id="f1f65-124">2. Creare il modello</span><span class="sxs-lookup"><span data-stu-id="f1f65-124">2. Create the Model</span></span>

<span data-ttu-id="f1f65-125">È importante definire un modello molto semplice usando le classi.</span><span class="sxs-lookup"><span data-stu-id="f1f65-125">Let’s define a very simple model using classes.</span></span> <span data-ttu-id="f1f65-126">Abbiamo appena stiamo definirli nel file Program.cs, ma in un'applicazione reale che si sarebbero suddividere le classi out in file separati e potenzialmente un progetto separato.</span><span class="sxs-lookup"><span data-stu-id="f1f65-126">We’re just defining them in the Program.cs file but in a real world application you would split your classes out into separate files and potentially a separate project.</span></span>

<span data-ttu-id="f1f65-127">Sotto la definizione della classe Program nel file Program.cs aggiungere le due classi seguenti.</span><span class="sxs-lookup"><span data-stu-id="f1f65-127">Below the Program class definition in Program.cs add the following two classes.</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public virtual Blog Blog { get; set; }
}
```

<span data-ttu-id="f1f65-128">Si noterà che stiamo rendendo le due proprietà di navigazione (Blog.Posts e Post.Blog) virtuale.</span><span class="sxs-lookup"><span data-stu-id="f1f65-128">You’ll notice that we’re making the two navigation properties (Blog.Posts and Post.Blog) virtual.</span></span> <span data-ttu-id="f1f65-129">In questo modo la funzionalità di caricamento Lazy di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f1f65-129">This enables the Lazy Loading feature of Entity Framework.</span></span> <span data-ttu-id="f1f65-130">Il caricamento lazy significa che il contenuto di queste proprietà verrà caricato automaticamente dal database quando si tenta di accedervi.</span><span class="sxs-lookup"><span data-stu-id="f1f65-130">Lazy Loading means that the contents of these properties will be automatically loaded from the database when you try to access them.</span></span>

## <a name="3-create-a-context"></a><span data-ttu-id="f1f65-131">3. Creare un contesto</span><span class="sxs-lookup"><span data-stu-id="f1f65-131">3. Create a Context</span></span>

<span data-ttu-id="f1f65-132">A questo punto è possibile definire un contesto derivato che rappresenta una sessione con il database, che consente di eseguire una query e salvare i dati.</span><span class="sxs-lookup"><span data-stu-id="f1f65-132">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="f1f65-133">Definiamo un contesto che deriva da DbContext ed espone un elemento DbSet tipizzato&lt;TEntity&gt; per ogni classe nel modello.</span><span class="sxs-lookup"><span data-stu-id="f1f65-133">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

<span data-ttu-id="f1f65-134">Stiamo iniziando adesso a usare tipi da Entity Framework, pertanto è necessario aggiungere il pacchetto EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="f1f65-134">We’re now starting to use types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="f1f65-135">**Progetto –&gt; Gestisci pacchetti NuGet...**</span><span class="sxs-lookup"><span data-stu-id="f1f65-135">**Project –&gt; Manage NuGet Packages…**</span></span>
    <span data-ttu-id="f1f65-136">Nota: Se non si dispone di **Gestisci pacchetti NuGet...**</span><span class="sxs-lookup"><span data-stu-id="f1f65-136">Note: If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="f1f65-137">opzione, è necessario installare il [versione più recente di NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span><span class="sxs-lookup"><span data-stu-id="f1f65-137">option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="f1f65-138">Selezionare il **Online** scheda</span><span class="sxs-lookup"><span data-stu-id="f1f65-138">Select the **Online** tab</span></span>
-   <span data-ttu-id="f1f65-139">Selezionare il **EntityFramework** pacchetto</span><span class="sxs-lookup"><span data-stu-id="f1f65-139">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="f1f65-140">Fare clic su **installare**</span><span class="sxs-lookup"><span data-stu-id="f1f65-140">Click **Install**</span></span>

<span data-ttu-id="f1f65-141">Aggiungere un tramite l'istruzione per Data. Entity nella parte superiore del file Program.cs.</span><span class="sxs-lookup"><span data-stu-id="f1f65-141">Add a using statement for System.Data.Entity at the top of Program.cs.</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="f1f65-142">Sotto il Post di classe nel file Program.cs aggiungere contesto derivato seguente.</span><span class="sxs-lookup"><span data-stu-id="f1f65-142">Below the Post class in Program.cs add the following derived context.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

<span data-ttu-id="f1f65-143">Ecco un elenco completo delle novità Program.cs dovrebbe ora contenere.</span><span class="sxs-lookup"><span data-stu-id="f1f65-143">Here is a complete listing of what Program.cs should now contain.</span></span>

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

<span data-ttu-id="f1f65-144">Questo è tutto il codice che è necessario iniziare l'archiviazione e recupero dei dati.</span><span class="sxs-lookup"><span data-stu-id="f1f65-144">That is all the code we need to start storing and retrieving data.</span></span> <span data-ttu-id="f1f65-145">Ovviamente non c'è qualche accade dietro le quinte e prenderemo in esame che a breve ma prima è opportuno vederlo in azione.</span><span class="sxs-lookup"><span data-stu-id="f1f65-145">Obviously there is quite a bit going on behind the scenes and we’ll take a look at that in a moment but first let’s see it in action.</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="f1f65-146">4. La lettura e scrittura dei dati</span><span class="sxs-lookup"><span data-stu-id="f1f65-146">4. Reading & Writing Data</span></span>

<span data-ttu-id="f1f65-147">Implementare il metodo Main in Program.cs, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="f1f65-147">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="f1f65-148">Questo codice crea una nuova istanza del contesto e quindi lo usa per inserire un nuovo post di Blog.</span><span class="sxs-lookup"><span data-stu-id="f1f65-148">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="f1f65-149">Usa quindi una query LINQ per recuperare tutti i blog dal database ordinato alfabeticamente in base al titolo.</span><span class="sxs-lookup"><span data-stu-id="f1f65-149">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="f1f65-150">È ora possibile eseguire l'applicazione e testarlo.</span><span class="sxs-lookup"><span data-stu-id="f1f65-150">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a><span data-ttu-id="f1f65-151">Dove si trova i miei dati?</span><span class="sxs-lookup"><span data-stu-id="f1f65-151">Where’s My Data?</span></span>

<span data-ttu-id="f1f65-152">Per convenzione DbContext ha creato un database.</span><span class="sxs-lookup"><span data-stu-id="f1f65-152">By convention DbContext has created a database for you.</span></span>

-   <span data-ttu-id="f1f65-153">Se è disponibile un'istanza di SQL Express locale (installato per impostazione predefinita con Visual Studio 2010) quindi Code First ha creato il database in tale istanza</span><span class="sxs-lookup"><span data-stu-id="f1f65-153">If a local SQL Express instance is available (installed by default with Visual Studio 2010) then Code First has created the database on that instance</span></span>
-   <span data-ttu-id="f1f65-154">Se SQL Express non è disponibile, Code First tenta di usare [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installato per impostazione predefinita con Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="f1f65-154">If SQL Express isn’t available then Code First will try and use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installed by default with Visual Studio 2012)</span></span>
-   <span data-ttu-id="f1f65-155">Il database è denominato dopo il nome completo del contesto derivato, in questo caso che è **CodeFirstNewDatabaseSample.BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="f1f65-155">The database is named after the fully qualified name of the derived context, in our case that is **CodeFirstNewDatabaseSample.BloggingContext**</span></span>

<span data-ttu-id="f1f65-156">Questi sono solo le convenzioni predefinite e sono disponibili vari modi per modificare il database che usa Code First, altre informazioni sono disponibili nel **come DbContext consente di individuare il modello e connessione al Database** argomento.</span><span class="sxs-lookup"><span data-stu-id="f1f65-156">These are just the default conventions and there are various ways to change the database that Code First uses, more information is available in the **How DbContext Discovers the Model and Database Connection** topic.</span></span>
<span data-ttu-id="f1f65-157">È possibile connettersi al database tramite Esplora Server in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f1f65-157">You can connect to this database using Server Explorer in Visual Studio</span></span>

-   <span data-ttu-id="f1f65-158">**Visualizzazione -&gt; Esplora Server**</span><span class="sxs-lookup"><span data-stu-id="f1f65-158">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="f1f65-159">Fare clic con il pulsante destro sul **connessioni dati** e selezionare **Aggiungi connessione...**</span><span class="sxs-lookup"><span data-stu-id="f1f65-159">Right click on **Data Connections** and select **Add Connection…**</span></span>
-   <span data-ttu-id="f1f65-160">Se si è ancora connessi a un database da Esplora Server prima che è necessario selezionare Microsoft SQL Server come origine dati</span><span class="sxs-lookup"><span data-stu-id="f1f65-160">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="f1f65-162">Connettersi a LocalDB o SQL Express, in base alla quale è stato installato</span><span class="sxs-lookup"><span data-stu-id="f1f65-162">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>

<span data-ttu-id="f1f65-163">È ora possibile controllare lo schema che Code First ha creato.</span><span class="sxs-lookup"><span data-stu-id="f1f65-163">We can now inspect the schema that Code First created.</span></span>

![SchemaInitial](~/ef6/media/schemainitial.png)

<span data-ttu-id="f1f65-165">DbContext elaborati quali classi da includere nel modello esaminando le proprietà DbSet che è stato definito.</span><span class="sxs-lookup"><span data-stu-id="f1f65-165">DbContext worked out what classes to include in the model by looking at the DbSet properties that we defined.</span></span> <span data-ttu-id="f1f65-166">Viene quindi utilizzato il set predefinito di convenzioni Code First per determinare i nomi di tabella e colonna, determinare i tipi di dati, trovare le chiavi primarie e così via.</span><span class="sxs-lookup"><span data-stu-id="f1f65-166">It then uses the default set of Code First conventions to determine table and column names, determine data types, find primary keys, etc.</span></span> <span data-ttu-id="f1f65-167">Più avanti in questa procedura dettagliata esamineremo come è possibile eseguire l'override di queste convenzioni.</span><span class="sxs-lookup"><span data-stu-id="f1f65-167">Later in this walkthrough we’ll look at how you can override these conventions.</span></span>

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="f1f65-168">5. Gestione di modifiche al modello</span><span class="sxs-lookup"><span data-stu-id="f1f65-168">5. Dealing with Model Changes</span></span>

<span data-ttu-id="f1f65-169">A questo punto è possibile apportare alcune modifiche per il modello, quando si apportano queste modifiche è necessario anche aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="f1f65-169">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span> <span data-ttu-id="f1f65-170">Per eseguire questa operazione si intende utilizzare una funzionalità denominata migrazioni Code First o migrazioni per brevità.</span><span class="sxs-lookup"><span data-stu-id="f1f65-170">To do this we are going to use a feature called Code First Migrations, or Migrations for short.</span></span>

<span data-ttu-id="f1f65-171">Le migrazioni consente di disporre di un set ordinato di procedure che descrivono come aggiornare lo schema di database (e il downgrade).</span><span class="sxs-lookup"><span data-stu-id="f1f65-171">Migrations allows us to have an ordered set of steps that describe how to upgrade (and downgrade) our database schema.</span></span> <span data-ttu-id="f1f65-172">Ognuno di questi passaggi, noti come una migrazione, contiene un codice che descrive le modifiche da applicare.</span><span class="sxs-lookup"><span data-stu-id="f1f65-172">Each of these steps, known as a migration, contains some code that describes the changes to be applied.</span></span> 

<span data-ttu-id="f1f65-173">Il primo passaggio è abilitare migrazioni Code First per nostro BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="f1f65-173">The first step is to enable Code First Migrations for our BloggingContext.</span></span>

-   <span data-ttu-id="f1f65-174">**Strumenti -&gt; Gestione pacchetti libreria -&gt; Console di gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="f1f65-174">**Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
-   <span data-ttu-id="f1f65-175">Eseguire il comando **Enable-Migrations** nella console di Gestione pacchetti</span><span class="sxs-lookup"><span data-stu-id="f1f65-175">Run the **Enable-Migrations** command in Package Manager Console</span></span>
-   <span data-ttu-id="f1f65-176">È stata aggiunta una nuova cartella Migrations per il progetto che contiene due elementi:</span><span class="sxs-lookup"><span data-stu-id="f1f65-176">A new Migrations folder has been added to our project that contains two items:</span></span>
    -   <span data-ttu-id="f1f65-177">**Configuration.cs** : questo file contiene le impostazioni che userà Migrations per eseguire la migrazione BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="f1f65-177">**Configuration.cs** – This file contains the settings that Migrations will use for migrating BloggingContext.</span></span> <span data-ttu-id="f1f65-178">Non è necessario modificare alcun valore per questa procedura dettagliata, ma Ecco dove è possibile specificare dati di seeding, i provider di registrazione per gli altri database, viene modificato lo spazio dei nomi che vengono generate le migrazioni e così via.</span><span class="sxs-lookup"><span data-stu-id="f1f65-178">We don’t need to change anything for this walkthrough, but here is where you can specify seed data, register providers for other databases, changes the namespace that migrations are generated in etc.</span></span>
    -   <span data-ttu-id="f1f65-179">**&lt;timestamp&gt;\_InitialCreate.cs** – si tratta di una prima migrazione, rappresenta le modifiche che sono già state applicate al database per portarlo da in corso di un database vuoto che includa le tabelle di blog e post .</span><span class="sxs-lookup"><span data-stu-id="f1f65-179">**&lt;timestamp&gt;\_InitialCreate.cs** – This is your first migration, it represents the changes that have already been applied to the database to take it from being an empty database to one that includes the Blogs and Posts tables.</span></span> <span data-ttu-id="f1f65-180">Anche se viene consentito di Code First di creare automaticamente le tabelle seguenti, ora che si è scelto alle migrazioni sono stati convertiti in una migrazione.</span><span class="sxs-lookup"><span data-stu-id="f1f65-180">Although we let Code First automatically create these tables for us, now that we have opted in to Migrations they have been converted into a Migration.</span></span> <span data-ttu-id="f1f65-181">Prima di tutto i codice anche è registrato nel nostro database locale che questa migrazione è già stata applicata.</span><span class="sxs-lookup"><span data-stu-id="f1f65-181">Code First has also recorded in our local database that this Migration has already been applied.</span></span> <span data-ttu-id="f1f65-182">Il timestamp nel nome file viene usato per scopi di ordinamento.</span><span class="sxs-lookup"><span data-stu-id="f1f65-182">The timestamp on the filename is used for ordering purposes.</span></span>

    <span data-ttu-id="f1f65-183">A questo punto è possibile apportare una modifica al modello, aggiungere una proprietà Url per la classe di Blog:</span><span class="sxs-lookup"><span data-stu-id="f1f65-183">Now let’s make a change to our model, add a Url property to the Blog class:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   <span data-ttu-id="f1f65-184">Eseguire la **Add-Migration AddUrl** comando nella Console di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="f1f65-184">Run the **Add-Migration AddUrl** command in Package Manager Console.</span></span>
    <span data-ttu-id="f1f65-185">Il comando Add-Migration controlli le modifiche apportate dopo l'ultima migrazione ed esegue scaffolding di una nuova migrazione con eventuali modifiche che si trovano.</span><span class="sxs-lookup"><span data-stu-id="f1f65-185">The Add-Migration command checks for changes since your last migration and scaffolds a new migration with any changes that are found.</span></span> <span data-ttu-id="f1f65-186">In questo modo, le migrazioni da un nome. In questo caso stiamo chiamando il 'AddUrl' la migrazione.</span><span class="sxs-lookup"><span data-stu-id="f1f65-186">We can give migrations a name; in this case we are calling the migration ‘AddUrl’.</span></span>
    <span data-ttu-id="f1f65-187">Il codice con scaffolding è che informa che è necessario aggiungere una colonna di Url, che può contenere dati di tipo stringa, dbo. Blog su tabella.</span><span class="sxs-lookup"><span data-stu-id="f1f65-187">The scaffolded code is saying that we need to add a Url column, that can hold string data, to the dbo.Blogs table.</span></span> <span data-ttu-id="f1f65-188">Se necessario, è stato possibile modificare il codice con scaffolding ma che in questo caso non è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="f1f65-188">If needed, we could edit the scaffolded code but that’s not required in this case.</span></span>

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Blogs", "Url", c => c.String());
        }

        public override void Down()
        {
            DropColumn("dbo.Blogs", "Url");
        }
    }
}
```

-   <span data-ttu-id="f1f65-189">Eseguire la **Update-Database** comando nella Console di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="f1f65-189">Run the **Update-Database** command in Package Manager Console.</span></span> <span data-ttu-id="f1f65-190">Questo comando applicherà eventuali migrazioni in sospeso al database.</span><span class="sxs-lookup"><span data-stu-id="f1f65-190">This command will apply any pending migrations to the database.</span></span> <span data-ttu-id="f1f65-191">La nostra migrazione InitialCreate è già stato applicato in modo che le migrazioni solo applicherà la nostra nuova migrazione AddUrl.</span><span class="sxs-lookup"><span data-stu-id="f1f65-191">Our InitialCreate migration has already been applied so migrations will just apply our new AddUrl migration.</span></span>
    <span data-ttu-id="f1f65-192">Suggerimento: È possibile usare la **– Verbose** quando si chiama Update-Database per visualizzare il codice SQL in esecuzione sul database.</span><span class="sxs-lookup"><span data-stu-id="f1f65-192">Tip: You can use the **–Verbose** switch when calling Update-Database to see the SQL that is being executed against the database.</span></span>

<span data-ttu-id="f1f65-193">La nuova colonna Url viene ora aggiunto alla tabella nel database di blog:</span><span class="sxs-lookup"><span data-stu-id="f1f65-193">The new Url column is now added to the Blogs table in the database:</span></span>

![SchemaWithUrl](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a><span data-ttu-id="f1f65-195">6. Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="f1f65-195">6. Data Annotations</span></span>

<span data-ttu-id="f1f65-196">Finora abbiamo semplicemente facciamo EF individuare il modello usando le convenzioni predefinite, ma vi saranno ora le classi non seguono le convenzioni e dobbiamo essere in grado di eseguire un'ulteriore configurazione.</span><span class="sxs-lookup"><span data-stu-id="f1f65-196">So far we’ve just let EF discover the model using its default conventions, but there are going to be times when our classes don’t follow the conventions and we need to be able to perform further configuration.</span></span> <span data-ttu-id="f1f65-197">Sono disponibili due opzioni per questo oggetto. Esamineremo le annotazioni dei dati in questa sezione e quindi l'API fluent nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="f1f65-197">There are two options for this; we’ll look at Data Annotations in this section and then the fluent API in the next section.</span></span>

-   <span data-ttu-id="f1f65-198">È possibile aggiungere una classe utente per il nostro modello</span><span class="sxs-lookup"><span data-stu-id="f1f65-198">Let’s add a User class to our model</span></span>

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="f1f65-199">È inoltre necessario aggiungere un set per il contesto derivato</span><span class="sxs-lookup"><span data-stu-id="f1f65-199">We also need to add a set to our derived context</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   <span data-ttu-id="f1f65-200">Se si tentasse di aggiungere una migrazione, si otterrebbe un errore che indica che "*EntityType 'User' non ha chiavi definite. Definire la chiave per EntityType."*</span><span class="sxs-lookup"><span data-stu-id="f1f65-200">If we tried to add a migration we’d get an error saying “*EntityType ‘User’ has no key defined. Define the key for this EntityType.”*</span></span> <span data-ttu-id="f1f65-201">Poiché Entity Framework non ha modo di sapere che il nome utente deve essere la chiave primaria per l'utente.</span><span class="sxs-lookup"><span data-stu-id="f1f65-201">because EF has no way of knowing that Username should be the primary key for User.</span></span>
-   <span data-ttu-id="f1f65-202">Si intende usare le annotazioni dei dati a questo punto dobbiamo aggiungere using all'inizio del file Program.cs l'istruzione</span><span class="sxs-lookup"><span data-stu-id="f1f65-202">We’re going to use Data Annotations now so we need to add a using statement at the top of Program.cs</span></span>

```
using System.ComponentModel.DataAnnotations;
```

-   <span data-ttu-id="f1f65-203">A questo punto annotare la proprietà Username per identificare la chiave primaria</span><span class="sxs-lookup"><span data-stu-id="f1f65-203">Now annotate the Username property to identify that it is the primary key</span></span>

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="f1f65-204">Usare la **Add-Migration AddUser** comando per eseguire lo scaffolding di una migrazione per applicare queste modifiche al database</span><span class="sxs-lookup"><span data-stu-id="f1f65-204">Use the **Add-Migration AddUser** command to scaffold a migration to apply these changes to the database</span></span>
-   <span data-ttu-id="f1f65-205">Eseguire la **Update-Database** comando per applicare la nuova migrazione al database</span><span class="sxs-lookup"><span data-stu-id="f1f65-205">Run the **Update-Database** command to apply the new migration to the database</span></span>

<span data-ttu-id="f1f65-206">La nuova tabella viene ora aggiunto al database:</span><span class="sxs-lookup"><span data-stu-id="f1f65-206">The new table is now added to the database:</span></span>

![SchemaWithUsers](~/ef6/media/schemawithusers.png)

<span data-ttu-id="f1f65-208">L'elenco completo di annotazioni supportate da Entity Framework è:</span><span class="sxs-lookup"><span data-stu-id="f1f65-208">The full list of annotations supported by EF is:</span></span>

-   [<span data-ttu-id="f1f65-209">KeyAttribute</span><span class="sxs-lookup"><span data-stu-id="f1f65-209">KeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [<span data-ttu-id="f1f65-210">Oggetto StringLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="f1f65-210">StringLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [<span data-ttu-id="f1f65-211">MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="f1f65-211">MaxLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [<span data-ttu-id="f1f65-212">ConcurrencyCheckAttribute</span><span class="sxs-lookup"><span data-stu-id="f1f65-212">ConcurrencyCheckAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [<span data-ttu-id="f1f65-213">RequiredAttribute</span><span class="sxs-lookup"><span data-stu-id="f1f65-213">RequiredAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [<span data-ttu-id="f1f65-214">TimestampAttribute</span><span class="sxs-lookup"><span data-stu-id="f1f65-214">TimestampAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [<span data-ttu-id="f1f65-215">ComplexTypeAttribute</span><span class="sxs-lookup"><span data-stu-id="f1f65-215">ComplexTypeAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [<span data-ttu-id="f1f65-216">ColumnAttribute</span><span class="sxs-lookup"><span data-stu-id="f1f65-216">ColumnAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [<span data-ttu-id="f1f65-217">TableAttribute</span><span class="sxs-lookup"><span data-stu-id="f1f65-217">TableAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [<span data-ttu-id="f1f65-218">InversePropertyAttribute</span><span class="sxs-lookup"><span data-stu-id="f1f65-218">InversePropertyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [<span data-ttu-id="f1f65-219">ForeignKeyAttribute</span><span class="sxs-lookup"><span data-stu-id="f1f65-219">ForeignKeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [<span data-ttu-id="f1f65-220">DatabaseGeneratedAttribute</span><span class="sxs-lookup"><span data-stu-id="f1f65-220">DatabaseGeneratedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [<span data-ttu-id="f1f65-221">NotMappedAttribute</span><span class="sxs-lookup"><span data-stu-id="f1f65-221">NotMappedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a><span data-ttu-id="f1f65-222">7. API Fluent</span><span class="sxs-lookup"><span data-stu-id="f1f65-222">7. Fluent API</span></span>

<span data-ttu-id="f1f65-223">Nella sezione precedente è stato illustrato l'utilizzo delle annotazioni dei dati per integrare o sostituire elementi rilevati mediante la convenzione.</span><span class="sxs-lookup"><span data-stu-id="f1f65-223">In the previous section we looked at using Data Annotations to supplement or override what was detected by convention.</span></span> <span data-ttu-id="f1f65-224">L'altro modo per configurare il modello è tramite l'API Office fluent Code First.</span><span class="sxs-lookup"><span data-stu-id="f1f65-224">The other way to configure the model is via the Code First fluent API.</span></span>

<span data-ttu-id="f1f65-225">La maggior parte delle configurazione del modello può essere eseguita tramite le annotazioni di dati semplici.</span><span class="sxs-lookup"><span data-stu-id="f1f65-225">Most model configuration can be done using simple data annotations.</span></span> <span data-ttu-id="f1f65-226">L'API fluent è un modo più avanzato di specifica di configurazione del modello che vengono trattate tutte le annotazioni dei dati possono eseguire anche per alcune configurazioni più avanzate non sono disponibili con le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="f1f65-226">The fluent API is a more advanced way of specifying model configuration that covers everything that data annotations can do in addition to some more advanced configuration not possible with data annotations.</span></span> <span data-ttu-id="f1f65-227">Le annotazioni dei dati e l'API fluent possono essere usati insieme.</span><span class="sxs-lookup"><span data-stu-id="f1f65-227">Data annotations and the fluent API can be used together.</span></span>

<span data-ttu-id="f1f65-228">Per accedere all'API fluent è eseguire l'override del metodo OnModelCreating in DbContext.</span><span class="sxs-lookup"><span data-stu-id="f1f65-228">To access the fluent API you override the OnModelCreating method in DbContext.</span></span> <span data-ttu-id="f1f65-229">Si supponga di voler rinominare la colonna archiviata in User. DisplayName per visualizzare\_nome.</span><span class="sxs-lookup"><span data-stu-id="f1f65-229">Let’s say we wanted to rename the column that User.DisplayName is stored in to display\_name.</span></span>

-   <span data-ttu-id="f1f65-230">Eseguire l'override del metodo OnModelCreating in BloggingContext con il codice seguente</span><span class="sxs-lookup"><span data-stu-id="f1f65-230">Override the OnModelCreating method on BloggingContext with the following code</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   <span data-ttu-id="f1f65-231">Usare la **Add-Migration ChangeDisplayName** comando per eseguire lo scaffolding di una migrazione per applicare queste modifiche al database.</span><span class="sxs-lookup"><span data-stu-id="f1f65-231">Use the **Add-Migration ChangeDisplayName** command to scaffold a migration to apply these changes to the database.</span></span>
-   <span data-ttu-id="f1f65-232">Eseguire la **Update-Database** comando per applicare la nuova migrazione al database.</span><span class="sxs-lookup"><span data-stu-id="f1f65-232">Run the **Update-Database** command to apply the new migration to the database.</span></span>

<span data-ttu-id="f1f65-233">La colonna DisplayName è stata rinominata per visualizzare\_nome:</span><span class="sxs-lookup"><span data-stu-id="f1f65-233">The DisplayName column is now renamed to display\_name:</span></span>

![SchemaWithDisplayNameRenamed](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a><span data-ttu-id="f1f65-235">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="f1f65-235">Summary</span></span>

<span data-ttu-id="f1f65-236">In questa procedura dettagliata viene esaminato lo sviluppo Code First con un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="f1f65-236">In this walkthrough we looked at Code First development using a new database.</span></span> <span data-ttu-id="f1f65-237">Viene definito un modello usando le classi quindi tale modello consente di creare un database e archiviare e recuperare i dati.</span><span class="sxs-lookup"><span data-stu-id="f1f65-237">We defined a model using classes then used that model to create a database and store and retrieve data.</span></span> <span data-ttu-id="f1f65-238">Dopo che è stato creato il database viene usato migrazioni Code First per modificare lo schema come il nostro modello sviluppato.</span><span class="sxs-lookup"><span data-stu-id="f1f65-238">Once the database was created we used Code First Migrations to change the schema as our model evolved.</span></span> <span data-ttu-id="f1f65-239">È stato anche illustrato come configurare un modello tramite le annotazioni dei dati e l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="f1f65-239">We also saw how to configure a model using Data Annotations and the Fluent API.</span></span>
