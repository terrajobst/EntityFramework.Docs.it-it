---
title: Prima di tutto - database Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: c60108c09fcbaaa1f86e77fa52cb13fe018975e1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995910"
---
# <a name="database-first"></a><span data-ttu-id="056fa-102">Prima di tutto di database</span><span class="sxs-lookup"><span data-stu-id="056fa-102">Database First</span></span>
<span data-ttu-id="056fa-103">Questa procedura dettagliata video e dettagliata forniscono un'introduzione allo sviluppo di Database First con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="056fa-103">This video and step-by-step walkthrough provide an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="056fa-104">Database prima di tutto consente di decodificare un modello da un database esistente.</span><span class="sxs-lookup"><span data-stu-id="056fa-104">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="056fa-105">Il modello viene archiviato in un file EDMX (file con estensione edmx) e può essere visualizzato e modificato in Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="056fa-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="056fa-106">Le classi che si interagiscono con nell'applicazione vengono generate automaticamente dal file con estensione EDMX.</span><span class="sxs-lookup"><span data-stu-id="056fa-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="056fa-107">Guarda il video</span><span class="sxs-lookup"><span data-stu-id="056fa-107">Watch the video</span></span>
<span data-ttu-id="056fa-108">Questo video viene fornita un'introduzione allo sviluppo di Database First usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="056fa-108">This video provides an introduction to Database First development using Entity Framework.</span></span> <span data-ttu-id="056fa-109">Database prima di tutto consente di decodificare un modello da un database esistente.</span><span class="sxs-lookup"><span data-stu-id="056fa-109">Database First allows you to reverse engineer a model from an existing database.</span></span> <span data-ttu-id="056fa-110">Il modello viene archiviato in un file EDMX (file con estensione edmx) e può essere visualizzato e modificato in Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="056fa-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="056fa-111">Le classi che si interagiscono con nell'applicazione vengono generate automaticamente dal file con estensione EDMX.</span><span class="sxs-lookup"><span data-stu-id="056fa-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="056fa-112">**Presentato da**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="056fa-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="056fa-113">**Video**: [WMV](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="056fa-113">**Video**: [WMV](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="056fa-114">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="056fa-114">Pre-Requisites</span></span>

<span data-ttu-id="056fa-115">È necessario avere almeno Visual Studio 2010 o Visual Studio 2012 è installato per completare questa procedura dettagliata.</span><span class="sxs-lookup"><span data-stu-id="056fa-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="056fa-116">Se si usa Visual Studio 2010, è necessario anche avere [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installato.</span><span class="sxs-lookup"><span data-stu-id="056fa-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

 

## <a name="1-create-an-existing-database"></a><span data-ttu-id="056fa-117">1. Creare un Database esistente</span><span class="sxs-lookup"><span data-stu-id="056fa-117">1. Create an Existing Database</span></span>

<span data-ttu-id="056fa-118">In genere quando si intende usare un database esistente che verrà già creato, ma per questa procedura dettagliata è necessario creare un database a cui accedere.</span><span class="sxs-lookup"><span data-stu-id="056fa-118">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="056fa-119">Il server di database che viene installato con Visual Studio è diverso a seconda della versione di Visual Studio installata:</span><span class="sxs-lookup"><span data-stu-id="056fa-119">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="056fa-120">Se si usa Visual Studio 2010 si creerà un database SQL Express.</span><span class="sxs-lookup"><span data-stu-id="056fa-120">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="056fa-121">Se si usa Visual Studio 2012, si creerà una [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span><span class="sxs-lookup"><span data-stu-id="056fa-121">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

 

<span data-ttu-id="056fa-122">È possibile procedere e generare il database.</span><span class="sxs-lookup"><span data-stu-id="056fa-122">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="056fa-123">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="056fa-123">Open Visual Studio</span></span>
-   <span data-ttu-id="056fa-124">**Visualizzazione -&gt; Esplora Server**</span><span class="sxs-lookup"><span data-stu-id="056fa-124">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="056fa-125">Fare clic con il pulsante destro sul **connessioni dati -&gt; Aggiungi connessione...**</span><span class="sxs-lookup"><span data-stu-id="056fa-125">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="056fa-126">Se si è ancora connessi a un database da Esplora Server prima che è necessario selezionare Microsoft SQL Server come origine dati</span><span class="sxs-lookup"><span data-stu-id="056fa-126">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="056fa-128">Connettersi a LocalDB o SQL Express, in base alla quale è stato installato e immettere **DatabaseFirst.Blogging** come nome del database</span><span class="sxs-lookup"><span data-stu-id="056fa-128">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **DatabaseFirst.Blogging** as the database name</span></span>

    ![SqlExpressConnectionDF](~/ef6/media/sqlexpressconnectiondf.png)

    ![LocalDBConnectionDF](~/ef6/media/localdbconnectiondf.png)

-   <span data-ttu-id="056fa-131">Selezionare **OK** e verrà richiesto se si desidera creare un nuovo database, selezionare **Sì**</span><span class="sxs-lookup"><span data-stu-id="056fa-131">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![CreateDatabaseDialog](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="056fa-133">Il nuovo database verrà ora visualizzato in Esplora Server, su di esso e scegliere **nuova Query**</span><span class="sxs-lookup"><span data-stu-id="056fa-133">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="056fa-134">Copiare il codice SQL seguente nella nuova query, quindi fare clic su query e selezionare **Execute**</span><span class="sxs-lookup"><span data-stu-id="056fa-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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
```

## <a name="2-create-the-application"></a><span data-ttu-id="056fa-135">2. Creare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="056fa-135">2. Create the Application</span></span>

<span data-ttu-id="056fa-136">Per semplificare le operazioni verranno creare un'applicazione console di base che usa il Database First di eseguire l'accesso ai dati:</span><span class="sxs-lookup"><span data-stu-id="056fa-136">To keep things simple we’re going to build a basic console application that uses the Database First to perform data access:</span></span>

-   <span data-ttu-id="056fa-137">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="056fa-137">Open Visual Studio</span></span>
-   <span data-ttu-id="056fa-138">**File -&gt; New -&gt; progetto...**</span><span class="sxs-lookup"><span data-stu-id="056fa-138">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="056fa-139">Selezionare **Windows** nel menu a sinistra e **applicazione Console**</span><span class="sxs-lookup"><span data-stu-id="056fa-139">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="056fa-140">Immettere **DatabaseFirstSample** come nome</span><span class="sxs-lookup"><span data-stu-id="056fa-140">Enter **DatabaseFirstSample** as the name</span></span>
-   <span data-ttu-id="056fa-141">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="056fa-141">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="056fa-142">3. Modelli di reverse engineering</span><span class="sxs-lookup"><span data-stu-id="056fa-142">3. Reverse Engineer Model</span></span>

<span data-ttu-id="056fa-143">Dobbiamo usare Entity Framework Designer, che è incluso come parte di Visual Studio, per creare il modello.</span><span class="sxs-lookup"><span data-stu-id="056fa-143">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="056fa-144">**Progetto -&gt; Aggiungi nuovo elemento...**</span><span class="sxs-lookup"><span data-stu-id="056fa-144">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="056fa-145">Selezionare **Data** nel menu a sinistra e quindi **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="056fa-145">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="056fa-146">Immettere **BloggingModel** come nome, quindi fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="056fa-146">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="056fa-147">Verrà avviata la **procedura guidata Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="056fa-147">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="056fa-148">Selezionare **genera da Database** e fare clic su **successivo**</span><span class="sxs-lookup"><span data-stu-id="056fa-148">Select **Generate from Database** and click **Next**</span></span>

    ![WizardStep1](~/ef6/media/wizardstep1.png)

-   <span data-ttu-id="056fa-150">Selezionare la connessione al database creato nella prima sezione, immettere **BloggingContext** come nome della stringa di connessione, scegliere **successivo**</span><span class="sxs-lookup"><span data-stu-id="056fa-150">Select the connection to the database you created in the first section, enter **BloggingContext** as the name of the connection string and click **Next**</span></span>

    ![WizardStep2](~/ef6/media/wizardstep2.png)

-   <span data-ttu-id="056fa-152">Selezionare la casella di controllo accanto a 'Tabelle' per importare tutte le tabelle e fare clic su 'Fine'</span><span class="sxs-lookup"><span data-stu-id="056fa-152">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![WizardStep3](~/ef6/media/wizardstep3.png)

 

<span data-ttu-id="056fa-154">Una volta completato il processo di reverse engineering il nuovo modello viene aggiunto al progetto e aperto per la visualizzazione in Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="056fa-154">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="056fa-155">Un file app. config è stato aggiunto anche al progetto con i dettagli della connessione per il database.</span><span class="sxs-lookup"><span data-stu-id="056fa-155">An App.config file has also been added to your project with the connection details for the database.</span></span>

![ModelInitial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="056fa-157">Passaggi aggiuntivi in Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="056fa-157">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="056fa-158">Se si lavora in Visual Studio 2010, esistono alcuni passaggi aggiuntivi da seguire per eseguire l'aggiornamento alla versione più recente di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="056fa-158">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="056fa-159">L'aggiornamento è importante perché consente di accedere a una superficie dell'API migliorata, che è molto più semplice da utilizzare, nonché le correzioni di bug più recenti.</span><span class="sxs-lookup"><span data-stu-id="056fa-159">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="056fa-160">Innanzitutto, è necessario ottenere la versione più recente di Entity Framework da NuGet.</span><span class="sxs-lookup"><span data-stu-id="056fa-160">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="056fa-161">**Progetto –&gt; Gestisci pacchetti NuGet... ** 
     *Se non si dispone di **Gestisci pacchetti NuGet... ** opzione, è necessario installare il [versione più recente di NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="056fa-161">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="056fa-162">Selezionare il **Online** scheda</span><span class="sxs-lookup"><span data-stu-id="056fa-162">Select the **Online** tab</span></span>
-   <span data-ttu-id="056fa-163">Selezionare il **EntityFramework** pacchetto</span><span class="sxs-lookup"><span data-stu-id="056fa-163">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="056fa-164">Fare clic su **installare**</span><span class="sxs-lookup"><span data-stu-id="056fa-164">Click **Install**</span></span>

<span data-ttu-id="056fa-165">Successivamente, è necessario scambiare il nostro modello per generare il codice che usa l'API DbContext, che è stato introdotto nelle versioni più recenti di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="056fa-165">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="056fa-166">Fare clic su un punto vuoto del modello in Entity Framework Designer e selezionare **Aggiungi elemento di generazione di codice...**</span><span class="sxs-lookup"><span data-stu-id="056fa-166">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="056fa-167">Selezionare **modelli Online** dal menu a sinistra e cercare **DbContext**</span><span class="sxs-lookup"><span data-stu-id="056fa-167">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="056fa-168">Selezionare il EF **5.x generatore di DbContext per C\#**, immettere **BloggingModel** come nome, quindi fare clic su **Add**</span><span class="sxs-lookup"><span data-stu-id="056fa-168">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![DbContextTemplate](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a><span data-ttu-id="056fa-170">4. La lettura e scrittura dei dati</span><span class="sxs-lookup"><span data-stu-id="056fa-170">4. Reading & Writing Data</span></span>

<span data-ttu-id="056fa-171">Ora che abbiamo un modello è possibile usarlo per accedere ai dati.</span><span class="sxs-lookup"><span data-stu-id="056fa-171">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="056fa-172">Le classi andremo a utilizzare per accedere ai dati vengono generati automaticamente in base al file con estensione EDMX.</span><span class="sxs-lookup"><span data-stu-id="056fa-172">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="056fa-173">*Questa schermata è da Visual Studio 2012, se si usa Visual Studio 2010 il BloggingModel.tt e file BloggingModel.Context.tt saranno direttamente sotto il progetto anziché da annidato in file con estensione EDMX.*</span><span class="sxs-lookup"><span data-stu-id="056fa-173">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![GeneratedClassesDF](~/ef6/media/generatedclassesdf.png)

 

<span data-ttu-id="056fa-175">Implementare il metodo Main in Program.cs, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="056fa-175">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="056fa-176">Questo codice crea una nuova istanza del contesto e quindi lo usa per inserire un nuovo post di Blog.</span><span class="sxs-lookup"><span data-stu-id="056fa-176">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="056fa-177">Usa quindi una query LINQ per recuperare tutti i blog dal database ordinato alfabeticamente in base al titolo.</span><span class="sxs-lookup"><span data-stu-id="056fa-177">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="056fa-178">È ora possibile eseguire l'applicazione e testarlo.</span><span class="sxs-lookup"><span data-stu-id="056fa-178">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a><span data-ttu-id="056fa-179">5. Gestione di modifiche del Database</span><span class="sxs-lookup"><span data-stu-id="056fa-179">5. Dealing with Database Changes</span></span>

<span data-ttu-id="056fa-180">A questo punto è possibile apportare alcune modifiche per lo schema di database, quando si apportano queste modifiche è necessario anche aggiornare il modello per riflettere tali modifiche.</span><span class="sxs-lookup"><span data-stu-id="056fa-180">Now it’s time to make some changes to our database schema, when we make these changes we also need to update our model to reflect those changes.</span></span>

<span data-ttu-id="056fa-181">Il primo passaggio è necessario apportare modifiche allo schema del database.</span><span class="sxs-lookup"><span data-stu-id="056fa-181">The first step is to make some changes to the database schema.</span></span> <span data-ttu-id="056fa-182">Dobbiamo aggiungere una tabella di utenti allo schema.</span><span class="sxs-lookup"><span data-stu-id="056fa-182">We’re going to add a Users table to the schema.</span></span>

-   <span data-ttu-id="056fa-183">Fare clic sui **DatabaseFirst.Blogging** del database in Esplora Server e selezionare **nuova Query**</span><span class="sxs-lookup"><span data-stu-id="056fa-183">Right-click on the **DatabaseFirst.Blogging** database in Server Explorer and select **New Query**</span></span>
-   <span data-ttu-id="056fa-184">Copiare il codice SQL seguente nella nuova query, quindi fare clic su query e selezionare **Execute**</span><span class="sxs-lookup"><span data-stu-id="056fa-184">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

<span data-ttu-id="056fa-185">Ora che lo schema viene aggiornato, è possibile aggiornare il modello con tali modifiche.</span><span class="sxs-lookup"><span data-stu-id="056fa-185">Now that the schema is updated, it’s time to update the model with those changes.</span></span>

-   <span data-ttu-id="056fa-186">Fare clic su un punto vuoto del modello in Entity Framework Designer e selezionare 'Aggiorna modello da Database... ', verrà avviata la procedura guidata Aggiorna</span><span class="sxs-lookup"><span data-stu-id="056fa-186">Right-click on an empty spot of your model in the EF Designer and select ‘Update Model from Database…’, this will launch the Update Wizard</span></span>
-   <span data-ttu-id="056fa-187">Nella scheda Aggiungi dei controlli effettuati dalla procedura guidata Aggiorna la casella accanto alle tabelle, ciò indica che si desidera aggiungere nuove tabelle dallo schema.</span><span class="sxs-lookup"><span data-stu-id="056fa-187">On the Add tab of the Update Wizard check the box next to Tables, this indicates that we want to add any new tables from the schema.</span></span>
    <span data-ttu-id="056fa-188">*La scheda aggiornamento Mostra le tabelle esistenti nel modello che verrà controllato per le modifiche durante l'aggiornamento. Le schede Delete mostrano tutte le tabelle che sono stati rimossi dallo schema e verranno anche rimosso dal modello come parte dell'aggiornamento. Le informazioni su queste due schede viene rilevate automaticamente e viene fornite esclusivamente per scopi informativi, è possibile modificare le impostazioni.*</span><span class="sxs-lookup"><span data-stu-id="056fa-188">*The Refresh tab shows any existing tables in the model that will be checked for changes during the update. The Delete tabs show any tables that have been removed from the schema and will also be removed from the model as part of the update. The information on these two tabs is automatically detected and is provided for informational purposes only, you cannot change any settings.*</span></span>

    ![RefreshWizard](~/ef6/media/refreshwizard.png)

-   <span data-ttu-id="056fa-190">Fare clic su Fine della procedura guidata aggiornamento</span><span class="sxs-lookup"><span data-stu-id="056fa-190">Click Finish on the Update Wizard</span></span>

 

<span data-ttu-id="056fa-191">Il modello è ora aggiornato per includere una nuova entità utente che esegue il mapping alla tabella Users che viene aggiunto al database.</span><span class="sxs-lookup"><span data-stu-id="056fa-191">The model is now updated to include a new User entity that maps to the Users table we added to the database.</span></span>

![ModelUpdated](~/ef6/media/modelupdated.png)

## <a name="summary"></a><span data-ttu-id="056fa-193">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="056fa-193">Summary</span></span>

<span data-ttu-id="056fa-194">In questa procedura dettagliata che è stato esaminato lo sviluppo di Database First, che ha permesso di creare un modello nella finestra di progettazione di Entity Framework basato su un database esistente.</span><span class="sxs-lookup"><span data-stu-id="056fa-194">In this walkthrough we looked at Database First development, which allowed us to create a model in the EF Designer based on an existing database.</span></span> <span data-ttu-id="056fa-195">Tale modello viene quindi usato per leggere e scrivere alcuni dati dal database.</span><span class="sxs-lookup"><span data-stu-id="056fa-195">We then used that model to read and write some data from the database.</span></span> <span data-ttu-id="056fa-196">Infine, abbiamo aggiornato il modello per riflettere le modifiche sono apportate allo schema del database.</span><span class="sxs-lookup"><span data-stu-id="056fa-196">Finally, we updated the model to reflect changes we made to the database schema.</span></span>
