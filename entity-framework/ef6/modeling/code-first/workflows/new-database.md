---
title: Code First a un nuovo database-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: d540fc6e84049f345ae22998f94c309e0be73fc3
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182566"
---
# <a name="code-first-to-a-new-database"></a><span data-ttu-id="59e97-102">Code First a un nuovo database</span><span class="sxs-lookup"><span data-stu-id="59e97-102">Code First to a New Database</span></span>
<span data-ttu-id="59e97-103">Questo video e una procedura dettagliata forniscono un'introduzione allo sviluppo di Code First destinati a un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="59e97-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="59e97-104">Questo scenario include la destinazione di un database che non esiste e Code First creerà oppure un database vuoto a cui Code First aggiungeranno nuove tabelle.</span><span class="sxs-lookup"><span data-stu-id="59e97-104">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="59e97-105">Code First consente di definire il modello utilizzando le classi C @ no__t-0 o VB.Net.</span><span class="sxs-lookup"><span data-stu-id="59e97-105">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="59e97-106">Facoltativamente, è possibile eseguire una configurazione aggiuntiva usando gli attributi delle classi e delle proprietà o usando un'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="59e97-106">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="59e97-107">Guarda il video</span><span class="sxs-lookup"><span data-stu-id="59e97-107">Watch the video</span></span>
<span data-ttu-id="59e97-108">In questo video viene fornita un'introduzione allo sviluppo di Code First destinati a un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="59e97-108">This video provides an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="59e97-109">Questo scenario include la destinazione di un database che non esiste e Code First creerà oppure un database vuoto a cui Code First aggiungeranno nuove tabelle.</span><span class="sxs-lookup"><span data-stu-id="59e97-109">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="59e97-110">Code First consente di definire il modello utilizzando C# o le classi VB.NET.</span><span class="sxs-lookup"><span data-stu-id="59e97-110">Code First allows you to define your model using C# or VB.Net classes.</span></span> <span data-ttu-id="59e97-111">Facoltativamente, è possibile eseguire una configurazione aggiuntiva usando gli attributi delle classi e delle proprietà o usando un'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="59e97-111">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

<span data-ttu-id="59e97-112">**Presentato da**: [Rowan Miller](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="59e97-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="59e97-113">**Video**: [WMV](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span><span class="sxs-lookup"><span data-stu-id="59e97-113">**Video**: [WMV](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="59e97-114">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="59e97-114">Pre-Requisites</span></span>

<span data-ttu-id="59e97-115">Per completare questa procedura dettagliata, è necessario che sia installato almeno Visual Studio 2010 o Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="59e97-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="59e97-116">Se si usa Visual Studio 2010, sarà anche necessario che [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) sia installato.</span><span class="sxs-lookup"><span data-stu-id="59e97-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="59e97-117">1. Creare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="59e97-117">1. Create the Application</span></span>

<span data-ttu-id="59e97-118">Per semplificare le operazioni, verrà compilata un'applicazione console di base che usa Code First per eseguire l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="59e97-118">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="59e97-119">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59e97-119">Open Visual Studio</span></span>
-   <span data-ttu-id="59e97-120">**Progetto New-&gt; del file-...**</span><span class="sxs-lookup"><span data-stu-id="59e97-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="59e97-121">Selezionare **Windows** nel menu a sinistra e nell' **applicazione console**</span><span class="sxs-lookup"><span data-stu-id="59e97-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="59e97-122">Immettere **CodeFirstNewDatabaseSample** come nome</span><span class="sxs-lookup"><span data-stu-id="59e97-122">Enter **CodeFirstNewDatabaseSample** as the name</span></span>
-   <span data-ttu-id="59e97-123">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="59e97-123">Select **OK**</span></span>

## <a name="2-create-the-model"></a><span data-ttu-id="59e97-124">2. Creare il modello</span><span class="sxs-lookup"><span data-stu-id="59e97-124">2. Create the Model</span></span>

<span data-ttu-id="59e97-125">Definire un modello molto semplice con le classi.</span><span class="sxs-lookup"><span data-stu-id="59e97-125">Let’s define a very simple model using classes.</span></span> <span data-ttu-id="59e97-126">Sono state semplicemente definite nel file Program.cs, ma in un'applicazione reale è possibile suddividere le classi in file separati e potenzialmente un progetto separato.</span><span class="sxs-lookup"><span data-stu-id="59e97-126">We’re just defining them in the Program.cs file but in a real world application you would split your classes out into separate files and potentially a separate project.</span></span>

<span data-ttu-id="59e97-127">Sotto la definizione della classe Program in Program.cs aggiungere le due classi seguenti.</span><span class="sxs-lookup"><span data-stu-id="59e97-127">Below the Program class definition in Program.cs add the following two classes.</span></span>

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

<span data-ttu-id="59e97-128">Si noterà che vengono apportate le due proprietà di navigazione (Blog. post e post. Blog) virtuali.</span><span class="sxs-lookup"><span data-stu-id="59e97-128">You’ll notice that we’re making the two navigation properties (Blog.Posts and Post.Blog) virtual.</span></span> <span data-ttu-id="59e97-129">In questo modo viene abilitata la funzionalità di caricamento lazy di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="59e97-129">This enables the Lazy Loading feature of Entity Framework.</span></span> <span data-ttu-id="59e97-130">Il caricamento lazy significa che il contenuto di queste proprietà verrà caricato automaticamente dal database quando si tenta di accedervi.</span><span class="sxs-lookup"><span data-stu-id="59e97-130">Lazy Loading means that the contents of these properties will be automatically loaded from the database when you try to access them.</span></span>

## <a name="3-create-a-context"></a><span data-ttu-id="59e97-131">3. Creare un contesto</span><span class="sxs-lookup"><span data-stu-id="59e97-131">3. Create a Context</span></span>

<span data-ttu-id="59e97-132">A questo punto è possibile definire un contesto derivato, che rappresenta una sessione con il database, consentendo di eseguire query e salvare i dati.</span><span class="sxs-lookup"><span data-stu-id="59e97-132">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="59e97-133">Si definisce un contesto che deriva da System. Data. Entity. DbContext ed espone un DbSet @ no__t-0TEntity @ no__t-1 tipizzato per ogni classe del modello.</span><span class="sxs-lookup"><span data-stu-id="59e97-133">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

<span data-ttu-id="59e97-134">A questo punto si inizia a usare i tipi del Entity Framework quindi è necessario aggiungere il pacchetto NuGet EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="59e97-134">We’re now starting to use types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="59e97-135">**Progetto-&gt; Gestisci pacchetti NuGet...**</span><span class="sxs-lookup"><span data-stu-id="59e97-135">**Project –&gt; Manage NuGet Packages…**</span></span>
    <span data-ttu-id="59e97-136">Nota: Se non si dispone dei **pacchetti NuGet di gestione...**</span><span class="sxs-lookup"><span data-stu-id="59e97-136">Note: If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="59e97-137">opzione è necessario installare la [versione più recente di NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span><span class="sxs-lookup"><span data-stu-id="59e97-137">option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="59e97-138">Selezionare la scheda **online**</span><span class="sxs-lookup"><span data-stu-id="59e97-138">Select the **Online** tab</span></span>
-   <span data-ttu-id="59e97-139">Selezionare il pacchetto **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="59e97-139">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="59e97-140">Fare clic su **Installa**</span><span class="sxs-lookup"><span data-stu-id="59e97-140">Click **Install**</span></span>

<span data-ttu-id="59e97-141">Aggiungere un'istruzione using per System. Data. Entity nella parte superiore di Program.cs.</span><span class="sxs-lookup"><span data-stu-id="59e97-141">Add a using statement for System.Data.Entity at the top of Program.cs.</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="59e97-142">Sotto la classe post in Program.cs aggiungere il seguente contesto derivato.</span><span class="sxs-lookup"><span data-stu-id="59e97-142">Below the Post class in Program.cs add the following derived context.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

<span data-ttu-id="59e97-143">Di seguito è riportato un elenco completo degli elementi che Program.cs dovrebbe contenere.</span><span class="sxs-lookup"><span data-stu-id="59e97-143">Here is a complete listing of what Program.cs should now contain.</span></span>

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

<span data-ttu-id="59e97-144">Questo è tutto il codice necessario per avviare l'archiviazione e il recupero dei dati.</span><span class="sxs-lookup"><span data-stu-id="59e97-144">That is all the code we need to start storing and retrieving data.</span></span> <span data-ttu-id="59e97-145">Ovviamente, c'è un po' di più dietro le quinte, che verranno esaminate in un certo momento, ma prima vediamo in azione.</span><span class="sxs-lookup"><span data-stu-id="59e97-145">Obviously there is quite a bit going on behind the scenes and we’ll take a look at that in a moment but first let’s see it in action.</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="59e97-146">4. Lettura & scrittura di dati</span><span class="sxs-lookup"><span data-stu-id="59e97-146">4. Reading & Writing Data</span></span>

<span data-ttu-id="59e97-147">Implementare il metodo Main in Program.cs, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="59e97-147">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="59e97-148">Questo codice crea una nuova istanza del contesto e la usa per inserire un nuovo blog.</span><span class="sxs-lookup"><span data-stu-id="59e97-148">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="59e97-149">USA quindi una query LINQ per recuperare tutti i Blog dal database ordinati alfabeticamente in base al titolo.</span><span class="sxs-lookup"><span data-stu-id="59e97-149">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="59e97-150">È ora possibile eseguire l'applicazione ed eseguirne il test.</span><span class="sxs-lookup"><span data-stu-id="59e97-150">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a><span data-ttu-id="59e97-151">Dove sono i dati?</span><span class="sxs-lookup"><span data-stu-id="59e97-151">Where’s My Data?</span></span>

<span data-ttu-id="59e97-152">Per convenzione DbContext ha creato automaticamente un database.</span><span class="sxs-lookup"><span data-stu-id="59e97-152">By convention DbContext has created a database for you.</span></span>

-   <span data-ttu-id="59e97-153">Se è disponibile un'istanza di SQL Express locale (installata per impostazione predefinita con Visual Studio 2010), Code First ha creato il database nell'istanza</span><span class="sxs-lookup"><span data-stu-id="59e97-153">If a local SQL Express instance is available (installed by default with Visual Studio 2010) then Code First has created the database on that instance</span></span>
-   <span data-ttu-id="59e97-154">Se SQL Express non è disponibile, Code First tenterà di usare il [database locale](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installato per impostazione predefinita con Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="59e97-154">If SQL Express isn’t available then Code First will try and use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installed by default with Visual Studio 2012)</span></span>
-   <span data-ttu-id="59e97-155">Il database viene denominato dopo il nome completo del contesto derivato, in questo caso **CodeFirstNewDatabaseSample. BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="59e97-155">The database is named after the fully qualified name of the derived context, in our case that is **CodeFirstNewDatabaseSample.BloggingContext**</span></span>

<span data-ttu-id="59e97-156">Queste sono solo le convenzioni predefinite e sono disponibili vari modi per modificare il database usato da Code First. per informazioni su **come DbContext individua l'argomento relativo al modello e alla connessione al database** , sono disponibili altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="59e97-156">These are just the default conventions and there are various ways to change the database that Code First uses, more information is available in the **How DbContext Discovers the Model and Database Connection** topic.</span></span>
<span data-ttu-id="59e97-157">È possibile connettersi a questo database usando Esplora server in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59e97-157">You can connect to this database using Server Explorer in Visual Studio</span></span>

-   <span data-ttu-id="59e97-158">**Visualizzazione-&gt; Esplora server**</span><span class="sxs-lookup"><span data-stu-id="59e97-158">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="59e97-159">Fare clic con il pulsante destro del mouse su **connessioni dati** e scegliere **Aggiungi connessione...**</span><span class="sxs-lookup"><span data-stu-id="59e97-159">Right click on **Data Connections** and select **Add Connection…**</span></span>
-   <span data-ttu-id="59e97-160">Se non si è connessi a un database da Esplora server prima di selezionare Microsoft SQL Server come origine dati</span><span class="sxs-lookup"><span data-stu-id="59e97-160">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Seleziona origine dati](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="59e97-162">Connettersi a un database locale o a SQL Express, a seconda di quale installato</span><span class="sxs-lookup"><span data-stu-id="59e97-162">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>

<span data-ttu-id="59e97-163">È ora possibile esaminare lo schema che Code First creato.</span><span class="sxs-lookup"><span data-stu-id="59e97-163">We can now inspect the schema that Code First created.</span></span>

![Schema iniziale](~/ef6/media/schemainitial.png)

<span data-ttu-id="59e97-165">DbContext ha elaborato le classi da includere nel modello esaminando le proprietà DbSet definite.</span><span class="sxs-lookup"><span data-stu-id="59e97-165">DbContext worked out what classes to include in the model by looking at the DbSet properties that we defined.</span></span> <span data-ttu-id="59e97-166">USA quindi il set predefinito di Code First convenzioni per determinare i nomi di tabella e colonna, determinare i tipi di dati, trovare chiavi primarie e così via.</span><span class="sxs-lookup"><span data-stu-id="59e97-166">It then uses the default set of Code First conventions to determine table and column names, determine data types, find primary keys, etc.</span></span> <span data-ttu-id="59e97-167">Più avanti in questa procedura dettagliata si esaminerà come è possibile eseguire l'override di queste convenzioni.</span><span class="sxs-lookup"><span data-stu-id="59e97-167">Later in this walkthrough we’ll look at how you can override these conventions.</span></span>

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="59e97-168">5. Gestione delle modifiche al modello</span><span class="sxs-lookup"><span data-stu-id="59e97-168">5. Dealing with Model Changes</span></span>

<span data-ttu-id="59e97-169">A questo punto è possibile apportare alcune modifiche al modello. quando si apportano queste modifiche, è necessario aggiornare anche lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="59e97-169">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span> <span data-ttu-id="59e97-170">A tale scopo, verrà usata una funzionalità denominata Migrazioni Code First o le migrazioni per brevità.</span><span class="sxs-lookup"><span data-stu-id="59e97-170">To do this we are going to use a feature called Code First Migrations, or Migrations for short.</span></span>

<span data-ttu-id="59e97-171">Le migrazioni consentono di avere un set ordinato di passaggi che descrivono come eseguire l'aggiornamento (e il downgrade) dello schema del database.</span><span class="sxs-lookup"><span data-stu-id="59e97-171">Migrations allows us to have an ordered set of steps that describe how to upgrade (and downgrade) our database schema.</span></span> <span data-ttu-id="59e97-172">Ognuno di questi passaggi, noto come migrazione, contiene codice che descrive le modifiche da applicare.</span><span class="sxs-lookup"><span data-stu-id="59e97-172">Each of these steps, known as a migration, contains some code that describes the changes to be applied.</span></span> 

<span data-ttu-id="59e97-173">Il primo passaggio consiste nell'abilitare Migrazioni Code First per la BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="59e97-173">The first step is to enable Code First Migrations for our BloggingContext.</span></span>

-   <span data-ttu-id="59e97-174">**Strumenti-&gt; libreria Package Manager-&gt; console di gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="59e97-174">**Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
-   <span data-ttu-id="59e97-175">Eseguire il comando **Enable-Migrations** nella console di Gestione pacchetti</span><span class="sxs-lookup"><span data-stu-id="59e97-175">Run the **Enable-Migrations** command in Package Manager Console</span></span>
-   <span data-ttu-id="59e97-176">Al progetto è stata aggiunta una nuova cartella migrazioni che contiene due elementi:</span><span class="sxs-lookup"><span data-stu-id="59e97-176">A new Migrations folder has been added to our project that contains two items:</span></span>
    -   <span data-ttu-id="59e97-177">**Configuration.cs** : questo file contiene le impostazioni che le migrazioni utilizzeranno per la migrazione di BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="59e97-177">**Configuration.cs** – This file contains the settings that Migrations will use for migrating BloggingContext.</span></span> <span data-ttu-id="59e97-178">Non è necessario apportare alcuna modifica per questa procedura dettagliata, ma qui è possibile specificare i dati di inizializzazione, registrare i provider per altri database, modificare lo spazio dei nomi in cui vengono generate migrazioni e così via.</span><span class="sxs-lookup"><span data-stu-id="59e97-178">We don’t need to change anything for this walkthrough, but here is where you can specify seed data, register providers for other databases, changes the namespace that migrations are generated in etc.</span></span>
    -   <span data-ttu-id="59e97-179">**&lt;timestamp @ no__t-2\_InitialCreate.cs** : si tratta della prima migrazione, che rappresenta le modifiche che sono già state applicate al database in modo da essere un database vuoto a uno che include le tabelle Blog e post.</span><span class="sxs-lookup"><span data-stu-id="59e97-179">**&lt;timestamp&gt;\_InitialCreate.cs** – This is your first migration, it represents the changes that have already been applied to the database to take it from being an empty database to one that includes the Blogs and Posts tables.</span></span> <span data-ttu-id="59e97-180">Sebbene si consenta di Code First creare automaticamente queste tabelle, ora che è stato scelto di eseguire la migrazione, le migrazioni sono state convertite in una migrazione.</span><span class="sxs-lookup"><span data-stu-id="59e97-180">Although we let Code First automatically create these tables for us, now that we have opted in to Migrations they have been converted into a Migration.</span></span> <span data-ttu-id="59e97-181">Code First ha registrato anche nel database locale che questa migrazione è già stata applicata.</span><span class="sxs-lookup"><span data-stu-id="59e97-181">Code First has also recorded in our local database that this Migration has already been applied.</span></span> <span data-ttu-id="59e97-182">Il timestamp nel nome file viene usato a scopo di ordinamento.</span><span class="sxs-lookup"><span data-stu-id="59e97-182">The timestamp on the filename is used for ordering purposes.</span></span>

    <span data-ttu-id="59e97-183">A questo punto è possibile apportare una modifica al modello, aggiungere una proprietà URL alla classe Blog:</span><span class="sxs-lookup"><span data-stu-id="59e97-183">Now let’s make a change to our model, add a Url property to the Blog class:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   <span data-ttu-id="59e97-184">Eseguire il comando **Add-Migration addurl** nella console di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="59e97-184">Run the **Add-Migration AddUrl** command in Package Manager Console.</span></span>
    <span data-ttu-id="59e97-185">Il comando Add-Migration controlla le modifiche apportate dall'ultima migrazione e ponteggi una nuova migrazione con le eventuali modifiche trovate.</span><span class="sxs-lookup"><span data-stu-id="59e97-185">The Add-Migration command checks for changes since your last migration and scaffolds a new migration with any changes that are found.</span></span> <span data-ttu-id="59e97-186">È possibile assegnare un nome alle migrazioni. in questo caso viene chiamata la migrazione ' AddUrl '.</span><span class="sxs-lookup"><span data-stu-id="59e97-186">We can give migrations a name; in this case we are calling the migration ‘AddUrl’.</span></span>
    <span data-ttu-id="59e97-187">Il codice con impalcature dice che è necessario aggiungere una colonna URL, che può conservare dati stringa, al dbo. Tabella Blog.</span><span class="sxs-lookup"><span data-stu-id="59e97-187">The scaffolded code is saying that we need to add a Url column, that can hold string data, to the dbo.Blogs table.</span></span> <span data-ttu-id="59e97-188">Se necessario, è possibile modificare il codice con impalcature, ma ciò non è necessario in questo caso.</span><span class="sxs-lookup"><span data-stu-id="59e97-188">If needed, we could edit the scaffolded code but that’s not required in this case.</span></span>

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

-   <span data-ttu-id="59e97-189">Eseguire il comando **Update-database** nella console di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="59e97-189">Run the **Update-Database** command in Package Manager Console.</span></span> <span data-ttu-id="59e97-190">Questo comando consente di applicare tutte le migrazioni in sospeso al database.</span><span class="sxs-lookup"><span data-stu-id="59e97-190">This command will apply any pending migrations to the database.</span></span> <span data-ttu-id="59e97-191">La migrazione di InitialCreate è già stata applicata, quindi le migrazioni applicheranno solo la nuova migrazione di AddUrl.</span><span class="sxs-lookup"><span data-stu-id="59e97-191">Our InitialCreate migration has already been applied so migrations will just apply our new AddUrl migration.</span></span>
    <span data-ttu-id="59e97-192">Suggerimento: È possibile usare l'opzione **-verbose** quando si chiama Update-database per visualizzare il SQL in esecuzione sul database.</span><span class="sxs-lookup"><span data-stu-id="59e97-192">Tip: You can use the **–Verbose** switch when calling Update-Database to see the SQL that is being executed against the database.</span></span>

<span data-ttu-id="59e97-193">La nuova colonna URL verrà ora aggiunta alla tabella Blogs del database:</span><span class="sxs-lookup"><span data-stu-id="59e97-193">The new Url column is now added to the Blogs table in the database:</span></span>

![Schema con URL](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a><span data-ttu-id="59e97-195">6. Annotazioni dei dati</span><span class="sxs-lookup"><span data-stu-id="59e97-195">6. Data Annotations</span></span>

<span data-ttu-id="59e97-196">Fino a questo momento abbiamo solo consentito a EF di individuare il modello usando le convenzioni predefinite, ma in alcuni casi le nostre classi non seguono le convenzioni ed è necessario essere in grado di eseguire ulteriori operazioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="59e97-196">So far we’ve just let EF discover the model using its default conventions, but there are going to be times when our classes don’t follow the conventions and we need to be able to perform further configuration.</span></span> <span data-ttu-id="59e97-197">Sono disponibili due opzioni: in questa sezione verranno esaminate le annotazioni dei dati e quindi l'API Fluent nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="59e97-197">There are two options for this; we’ll look at Data Annotations in this section and then the fluent API in the next section.</span></span>

-   <span data-ttu-id="59e97-198">Aggiungere una classe utente al modello</span><span class="sxs-lookup"><span data-stu-id="59e97-198">Let’s add a User class to our model</span></span>

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="59e97-199">È anche necessario aggiungere un set al contesto derivato</span><span class="sxs-lookup"><span data-stu-id="59e97-199">We also need to add a set to our derived context</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   <span data-ttu-id="59e97-200">Se si è tentato di aggiungere una migrazione, viene segnalato un errore che indica che "*EntityType" User "non è stata definita alcuna chiave. Definire la chiave per questo EntityType ".*</span><span class="sxs-lookup"><span data-stu-id="59e97-200">If we tried to add a migration we’d get an error saying “*EntityType ‘User’ has no key defined. Define the key for this EntityType.”*</span></span> <span data-ttu-id="59e97-201">Poiché EF non ha modo di sapere che il nome utente deve essere la chiave primaria per l'utente.</span><span class="sxs-lookup"><span data-stu-id="59e97-201">because EF has no way of knowing that Username should be the primary key for User.</span></span>
-   <span data-ttu-id="59e97-202">Ora verranno usate le annotazioni dei dati, quindi è necessario aggiungere un'istruzione using nella parte superiore di Program.cs</span><span class="sxs-lookup"><span data-stu-id="59e97-202">We’re going to use Data Annotations now so we need to add a using statement at the top of Program.cs</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
```

-   <span data-ttu-id="59e97-203">Annotare ora la proprietà UserName per identificare che si tratta della chiave primaria</span><span class="sxs-lookup"><span data-stu-id="59e97-203">Now annotate the Username property to identify that it is the primary key</span></span>

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="59e97-204">Usare il comando **Add-Migration adduser** per eseguire il patibolo di una migrazione per applicare queste modifiche al database</span><span class="sxs-lookup"><span data-stu-id="59e97-204">Use the **Add-Migration AddUser** command to scaffold a migration to apply these changes to the database</span></span>
-   <span data-ttu-id="59e97-205">Eseguire il comando **Update-database** per applicare la nuova migrazione al database</span><span class="sxs-lookup"><span data-stu-id="59e97-205">Run the **Update-Database** command to apply the new migration to the database</span></span>

<span data-ttu-id="59e97-206">La nuova tabella verrà ora aggiunta al database:</span><span class="sxs-lookup"><span data-stu-id="59e97-206">The new table is now added to the database:</span></span>

![Schema con utenti](~/ef6/media/schemawithusers.png)

<span data-ttu-id="59e97-208">L'elenco completo delle annotazioni supportate da EF è il seguente:</span><span class="sxs-lookup"><span data-stu-id="59e97-208">The full list of annotations supported by EF is:</span></span>

-   [<span data-ttu-id="59e97-209">KeyAttribute</span><span class="sxs-lookup"><span data-stu-id="59e97-209">KeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [<span data-ttu-id="59e97-210">StringLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="59e97-210">StringLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [<span data-ttu-id="59e97-211">MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="59e97-211">MaxLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [<span data-ttu-id="59e97-212">ConcurrencyCheckAttribute</span><span class="sxs-lookup"><span data-stu-id="59e97-212">ConcurrencyCheckAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [<span data-ttu-id="59e97-213">RequiredAttribute</span><span class="sxs-lookup"><span data-stu-id="59e97-213">RequiredAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [<span data-ttu-id="59e97-214">TimestampAttribute</span><span class="sxs-lookup"><span data-stu-id="59e97-214">TimestampAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [<span data-ttu-id="59e97-215">ComplexTypeAttribute</span><span class="sxs-lookup"><span data-stu-id="59e97-215">ComplexTypeAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [<span data-ttu-id="59e97-216">ColumnAttribute</span><span class="sxs-lookup"><span data-stu-id="59e97-216">ColumnAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [<span data-ttu-id="59e97-217">TableAttribute</span><span class="sxs-lookup"><span data-stu-id="59e97-217">TableAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [<span data-ttu-id="59e97-218">InversePropertyAttribute</span><span class="sxs-lookup"><span data-stu-id="59e97-218">InversePropertyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [<span data-ttu-id="59e97-219">ForeignKeyAttribute</span><span class="sxs-lookup"><span data-stu-id="59e97-219">ForeignKeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [<span data-ttu-id="59e97-220">DatabaseGeneratedAttribute</span><span class="sxs-lookup"><span data-stu-id="59e97-220">DatabaseGeneratedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [<span data-ttu-id="59e97-221">NotMappedAttribute</span><span class="sxs-lookup"><span data-stu-id="59e97-221">NotMappedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a><span data-ttu-id="59e97-222">7. API Fluent</span><span class="sxs-lookup"><span data-stu-id="59e97-222">7. Fluent API</span></span>

<span data-ttu-id="59e97-223">Nella sezione precedente è stato esaminato l'uso delle annotazioni dei dati per integrare o ignorare gli elementi rilevati per convenzione.</span><span class="sxs-lookup"><span data-stu-id="59e97-223">In the previous section we looked at using Data Annotations to supplement or override what was detected by convention.</span></span> <span data-ttu-id="59e97-224">L'altro modo per configurare il modello è tramite l'API Code First Fluent.</span><span class="sxs-lookup"><span data-stu-id="59e97-224">The other way to configure the model is via the Code First fluent API.</span></span>

<span data-ttu-id="59e97-225">La maggior parte della configurazione del modello può essere eseguita usando le annotazioni di dati semplici.</span><span class="sxs-lookup"><span data-stu-id="59e97-225">Most model configuration can be done using simple data annotations.</span></span> <span data-ttu-id="59e97-226">L'API Fluent è un modo più avanzato per specificare la configurazione del modello che copre tutto ciò che può fare le annotazioni di dati, oltre a una configurazione più avanzata non possibile con le annotazioni dei dati.</span><span class="sxs-lookup"><span data-stu-id="59e97-226">The fluent API is a more advanced way of specifying model configuration that covers everything that data annotations can do in addition to some more advanced configuration not possible with data annotations.</span></span> <span data-ttu-id="59e97-227">Le annotazioni dei dati e l'API Fluent possono essere usate insieme.</span><span class="sxs-lookup"><span data-stu-id="59e97-227">Data annotations and the fluent API can be used together.</span></span>

<span data-ttu-id="59e97-228">Per accedere all'API Fluent, eseguire l'override del metodo OnModelCreating in DbContext.</span><span class="sxs-lookup"><span data-stu-id="59e97-228">To access the fluent API you override the OnModelCreating method in DbContext.</span></span> <span data-ttu-id="59e97-229">Supponiamo di voler rinominare la colonna in cui è archiviato User. DisplayName per visualizzare @ no__t-0name.</span><span class="sxs-lookup"><span data-stu-id="59e97-229">Let’s say we wanted to rename the column that User.DisplayName is stored in to display\_name.</span></span>

-   <span data-ttu-id="59e97-230">Eseguire l'override del metodo OnModelCreating in BloggingContext con il codice seguente</span><span class="sxs-lookup"><span data-stu-id="59e97-230">Override the OnModelCreating method on BloggingContext with the following code</span></span>

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

-   <span data-ttu-id="59e97-231">Usare il comando **Add-Migration ChangeDisplayName** per eseguire il patibolo di una migrazione per applicare queste modifiche al database.</span><span class="sxs-lookup"><span data-stu-id="59e97-231">Use the **Add-Migration ChangeDisplayName** command to scaffold a migration to apply these changes to the database.</span></span>
-   <span data-ttu-id="59e97-232">Eseguire il comando **Update-database** per applicare la nuova migrazione al database.</span><span class="sxs-lookup"><span data-stu-id="59e97-232">Run the **Update-Database** command to apply the new migration to the database.</span></span>

<span data-ttu-id="59e97-233">La colonna DisplayName è stata rinominata per visualizzare @ no__t-0name:</span><span class="sxs-lookup"><span data-stu-id="59e97-233">The DisplayName column is now renamed to display\_name:</span></span>

![Schema con nome visualizzato rinominato](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a><span data-ttu-id="59e97-235">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="59e97-235">Summary</span></span>

<span data-ttu-id="59e97-236">In questa procedura dettagliata è stato esaminato Code First sviluppo tramite un nuovo database.</span><span class="sxs-lookup"><span data-stu-id="59e97-236">In this walkthrough we looked at Code First development using a new database.</span></span> <span data-ttu-id="59e97-237">È stato definito un modello usando le classi, quindi è stato usato il modello per creare un database e archiviare e recuperare i dati.</span><span class="sxs-lookup"><span data-stu-id="59e97-237">We defined a model using classes then used that model to create a database and store and retrieve data.</span></span> <span data-ttu-id="59e97-238">Una volta creato il database, è stato usato Migrazioni Code First per modificare lo schema man mano che il modello è stato sviluppato.</span><span class="sxs-lookup"><span data-stu-id="59e97-238">Once the database was created we used Code First Migrations to change the schema as our model evolved.</span></span> <span data-ttu-id="59e97-239">È stato anche illustrato come configurare un modello usando le annotazioni dei dati e l'API Fluent.</span><span class="sxs-lookup"><span data-stu-id="59e97-239">We also saw how to configure a model using Data Annotations and the Fluent API.</span></span>
