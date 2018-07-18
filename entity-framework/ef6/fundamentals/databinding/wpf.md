---
title: Data Binding con WPF - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
caps.latest.revision: 3
ms.openlocfilehash: 1756ec14fe83d80199b6040bd345dc2fe6294281
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121137"
---
# <a name="databinding-with-wpf"></a><span data-ttu-id="a9eab-102">Data Binding con WPF</span><span class="sxs-lookup"><span data-stu-id="a9eab-102">Databinding with WPF</span></span>
<span data-ttu-id="a9eab-103">Questa procedura dettagliata viene illustrato come associare controlli WPF in un form di "master-detail" tipi POCO.</span><span class="sxs-lookup"><span data-stu-id="a9eab-103">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="a9eab-104">L'applicazione usa le API di Entity Framework per popolare gli oggetti con i dati dal database, tenere traccia delle modifiche e rendere persistenti i dati nel database.</span><span class="sxs-lookup"><span data-stu-id="a9eab-104">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="a9eab-105">Il modello definisce due tipi che fanno parte di relazione uno-a-molti: **categoria** (principal\\master) e **prodotto** (dipendenti\\dettaglio).</span><span class="sxs-lookup"><span data-stu-id="a9eab-105">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="a9eab-106">Quindi, gli strumenti di Visual Studio vengono utilizzati per associare i tipi definiti nel modello per i controlli WPF.</span><span class="sxs-lookup"><span data-stu-id="a9eab-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="a9eab-107">Il framework di associazione dati WPF consente la navigazione tra gli oggetti correlati: se si seleziona le righe della visualizzazione master, la visualizzazione di dettaglio da aggiornare con i dati figlio corrispondente.</span><span class="sxs-lookup"><span data-stu-id="a9eab-107">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="a9eab-108">Le schermate e i listati di codice in questa procedura dettagliata vengono forniti da Visual Studio 2013, ma è possibile completare questa procedura dettagliata con Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="a9eab-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="a9eab-109">Usare l'opzione 'Object' per la creazione di origini dati WPF</span><span class="sxs-lookup"><span data-stu-id="a9eab-109">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="a9eab-110">Con la versione precedente di Entity Framework è stato usato per consiglia di usare la **Database** durante la creazione di una nuova origine dati basata su un modello creato con la finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a9eab-110">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="a9eab-111">Questo era perché la finestra di progettazione genera classi di entità derivato da EntityObject e un contesto che deriva da ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="a9eab-111">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="a9eab-112">Usando l'opzione di Database sarebbe consentono di scrivere il codice più efficace per l'interazione con questa superficie dell'API.</span><span class="sxs-lookup"><span data-stu-id="a9eab-112">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="a9eab-113">Le finestre di progettazione di Entity Framework per Visual Studio 2012 e Visual Studio 2013 generano un contesto che deriva da DbContext insieme alle classi di entità POCO semplice.</span><span class="sxs-lookup"><span data-stu-id="a9eab-113">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="a9eab-114">Con Visual Studio 2010 è consigliabile dello swapping in un modello di generazione di codice che usa DbContext, come descritto più avanti in questa procedura dettagliata.</span><span class="sxs-lookup"><span data-stu-id="a9eab-114">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="a9eab-115">Quando si usa la superficie dell'API DbContext è consigliabile usare la **oggetto** opzione quando si crea una nuova origine dati, come illustrato in questa procedura dettagliata.</span><span class="sxs-lookup"><span data-stu-id="a9eab-115">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="a9eab-116">Se necessario, è possibile [ripristinare la generazione di codice basato su ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) per i modelli creati con la finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a9eab-116">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="a9eab-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="a9eab-117">Pre-Requisites</span></span>

<span data-ttu-id="a9eab-118">È necessario disporre di Visual Studio 2013, Visual Studio 2012 o Visual Studio 2010 per poter completare questa procedura dettagliata.</span><span class="sxs-lookup"><span data-stu-id="a9eab-118">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="a9eab-119">Se si usa Visual Studio 2010, è necessario anche installare NuGet.</span><span class="sxs-lookup"><span data-stu-id="a9eab-119">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="a9eab-120">Per altre informazioni, vedere [installazione di NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="a9eab-120">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="a9eab-121">Creare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="a9eab-121">Create the Application</span></span>

-   <span data-ttu-id="a9eab-122">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9eab-122">Open Visual Studio</span></span>
-   <span data-ttu-id="a9eab-123">**File -&gt; New -&gt; progetto...**</span><span class="sxs-lookup"><span data-stu-id="a9eab-123">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="a9eab-124">Selezionare **Windows** nel riquadro sinistro e **WPFApplication** nel riquadro di destra</span><span class="sxs-lookup"><span data-stu-id="a9eab-124">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="a9eab-125">Immettere **WPFwithEFSample** come nome</span><span class="sxs-lookup"><span data-stu-id="a9eab-125">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="a9eab-126">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="a9eab-126">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="a9eab-127">Installare il pacchetto NuGet di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="a9eab-127">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="a9eab-128">In Esplora soluzioni fare clic sui **WinFormswithEFSample** progetto</span><span class="sxs-lookup"><span data-stu-id="a9eab-128">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="a9eab-129">Selezionare **Gestisci pacchetti NuGet...**</span><span class="sxs-lookup"><span data-stu-id="a9eab-129">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="a9eab-130">Nella finestra di dialogo Gestisci pacchetti NuGet, selezionare la **Online** scheda e scegliere il **EntityFramework** pacchetto</span><span class="sxs-lookup"><span data-stu-id="a9eab-130">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="a9eab-131">Fare clic su **installare**</span><span class="sxs-lookup"><span data-stu-id="a9eab-131">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="a9eab-132">Oltre all'assembly EntityFramework viene anche aggiunto un riferimento a System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="a9eab-132">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="a9eab-133">Se il progetto contiene un riferimento a Data. Entity, quindi verrà rimosso quando viene installato il pacchetto di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a9eab-133">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="a9eab-134">L'assembly Data. Entity non è più utilizzato per le applicazioni Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="a9eab-134">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="a9eab-135">Definire un modello</span><span class="sxs-lookup"><span data-stu-id="a9eab-135">Define a Model</span></span>

<span data-ttu-id="a9eab-136">In questa procedura dettagliata che è possibile scelta di implementare un modello utilizzando Code First o Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="a9eab-136">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="a9eab-137">Completare una delle due sezioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="a9eab-137">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="a9eab-138">Opzione 1: Definire un modello utilizzando Code First</span><span class="sxs-lookup"><span data-stu-id="a9eab-138">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="a9eab-139">In questa sezione viene illustrato come creare un modello e il database associato utilizzando Code First.</span><span class="sxs-lookup"><span data-stu-id="a9eab-139">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="a9eab-140">Passare alla sezione successiva (**Option 2: definire un modello utilizzando Database First)** se si preferisce usare Database First da invertire progettare il modello da un database utilizzando la finestra di progettazione di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="a9eab-140">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="a9eab-141">Quando si usa lo sviluppo Code First è in genere iniziare la scrittura di classi .NET Framework che definiscono il modello concettuale (dominio).</span><span class="sxs-lookup"><span data-stu-id="a9eab-141">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="a9eab-142">Aggiungere una nuova classe per il **WPFwithEFSample:**</span><span class="sxs-lookup"><span data-stu-id="a9eab-142">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="a9eab-143">Fare doppio clic sul nome del progetto</span><span class="sxs-lookup"><span data-stu-id="a9eab-143">Right-click on the project name</span></span>
    -   <span data-ttu-id="a9eab-144">Selezionare **aggiungere**, quindi **nuovo elemento**</span><span class="sxs-lookup"><span data-stu-id="a9eab-144">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="a9eab-145">Selezionare **classe** e immettere **prodotto** per il nome della classe</span><span class="sxs-lookup"><span data-stu-id="a9eab-145">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="a9eab-146">Sostituire il **prodotto** definizione con il codice seguente della classe:</span><span class="sxs-lookup"><span data-stu-id="a9eab-146">Replace the **Product** class definition with the following code:</span></span>

``` csharp
    namespace WPFwithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }

-   Add a **Category** class with the following definition:

    using System.Collections.ObjectModel;

    namespace WPFwithEFSample
    {
        public class Category
        {
            public Category()
            {
                this.Products = new ObservableCollection<Product>();
            }

            public int CategoryId { get; set; }
            public string Name { get; set; }

            public virtual ObservableCollection<Product> Products { get; private set; }
        }
    }
```

<span data-ttu-id="a9eab-147">Il **prodotti** proprietà il **categoria** classe e **categoria** proprietà la **prodotto** classe sono le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a9eab-147">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="a9eab-148">In Entity Framework, le proprietà di navigazione offrono un modo per spostarsi in una relazione tra due tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="a9eab-148">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="a9eab-149">Oltre a definire le entità, è necessario definire una classe che deriva da DbContext ed espone DbSet&lt;TEntity&gt; proprietà.</span><span class="sxs-lookup"><span data-stu-id="a9eab-149">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="a9eab-150">Alla classe DbSet&lt;TEntity&gt; proprietà informare il contesto di quali tipi di cui si desidera includere nel modello.</span><span class="sxs-lookup"><span data-stu-id="a9eab-150">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="a9eab-151">Un'istanza del tipo DbContext derivata gestisce gli oggetti entità durante la fase di esecuzione, che include la compilazione di oggetti con i dati da un database, modificare i dati di rilevamento e persistenza nel database.</span><span class="sxs-lookup"><span data-stu-id="a9eab-151">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="a9eab-152">Aggiungere un nuovo **ProductContext** classe al progetto con la definizione seguente:</span><span class="sxs-lookup"><span data-stu-id="a9eab-152">Add a new **ProductContext** class to the project with the following definition:</span></span>

``` csharp
    using System.Data.Entity;

    namespace WPFwithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="a9eab-153">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="a9eab-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="a9eab-154">Opzione 2: Definire un modello utilizzando Database First</span><span class="sxs-lookup"><span data-stu-id="a9eab-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="a9eab-155">Questa sezione illustra come usare Database First per decompilare il modello da un database utilizzando la finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a9eab-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="a9eab-156">Se è stata completata la sezione precedente (**Option 1: definire un modello utilizzando Code First)**, quindi ignorare questa sezione e passare direttamente alla sezione la **il caricamento Lazy** sezione.</span><span class="sxs-lookup"><span data-stu-id="a9eab-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="a9eab-157">Creare un Database esistente</span><span class="sxs-lookup"><span data-stu-id="a9eab-157">Create an Existing Database</span></span>

<span data-ttu-id="a9eab-158">In genere quando si intende usare un database esistente che verrà già creato, ma per questa procedura dettagliata è necessario creare un database a cui accedere.</span><span class="sxs-lookup"><span data-stu-id="a9eab-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="a9eab-159">Il server di database che viene installato con Visual Studio è diverso a seconda della versione di Visual Studio installata:</span><span class="sxs-lookup"><span data-stu-id="a9eab-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="a9eab-160">Se si usa Visual Studio 2010 si creerà un database SQL Express.</span><span class="sxs-lookup"><span data-stu-id="a9eab-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="a9eab-161">Se si usa Visual Studio 2012, si creerà una [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span><span class="sxs-lookup"><span data-stu-id="a9eab-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="a9eab-162">È possibile procedere e generare il database.</span><span class="sxs-lookup"><span data-stu-id="a9eab-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="a9eab-163">**Visualizzazione -&gt; Esplora Server**</span><span class="sxs-lookup"><span data-stu-id="a9eab-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="a9eab-164">Fare clic con il pulsante destro sul **connessioni dati -&gt; Aggiungi connessione...**</span><span class="sxs-lookup"><span data-stu-id="a9eab-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="a9eab-165">Se si è ancora connessi a un database da Esplora Server prima che è necessario selezionare Microsoft SQL Server come origine dati</span><span class="sxs-lookup"><span data-stu-id="a9eab-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![ChangeDataSource](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="a9eab-167">Connettersi a LocalDB o SQL Express, in base alla quale è stato installato e immettere **prodotti** come nome del database</span><span class="sxs-lookup"><span data-stu-id="a9eab-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![AddConnectionLocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![AddConnectionExpress](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="a9eab-170">Selezionare **OK** e verrà richiesto se si desidera creare un nuovo database, selezionare **Sì**</span><span class="sxs-lookup"><span data-stu-id="a9eab-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![CreateDatabase](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="a9eab-172">Il nuovo database verrà ora visualizzato in Esplora Server, su di esso e scegliere **nuova Query**</span><span class="sxs-lookup"><span data-stu-id="a9eab-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="a9eab-173">Copiare il codice SQL seguente nella nuova query, quindi fare clic su query e selezionare **Execute**</span><span class="sxs-lookup"><span data-stu-id="a9eab-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a><span data-ttu-id="a9eab-174">Modelli di reverse engineering</span><span class="sxs-lookup"><span data-stu-id="a9eab-174">Reverse Engineer Model</span></span>

<span data-ttu-id="a9eab-175">Dobbiamo usare Entity Framework Designer, che è incluso come parte di Visual Studio, per creare il modello.</span><span class="sxs-lookup"><span data-stu-id="a9eab-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="a9eab-176">**Progetto -&gt; Aggiungi nuovo elemento...**</span><span class="sxs-lookup"><span data-stu-id="a9eab-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="a9eab-177">Selezionare **Data** nel menu a sinistra e quindi **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="a9eab-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="a9eab-178">Immettere **ProductModel** come nome, quindi fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="a9eab-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="a9eab-179">Verrà avviata la **procedura guidata Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="a9eab-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="a9eab-180">Selezionare **genera da Database** e fare clic su **successivo**</span><span class="sxs-lookup"><span data-stu-id="a9eab-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="a9eab-182">Selezionare la connessione al database creato nella prima sezione, immettere **ProductContext** come nome della stringa di connessione, scegliere **successivo**</span><span class="sxs-lookup"><span data-stu-id="a9eab-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![ChooseYourConnection](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="a9eab-184">Selezionare la casella di controllo accanto a 'Tabelle' per importare tutte le tabelle e fare clic su 'Fine'</span><span class="sxs-lookup"><span data-stu-id="a9eab-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![ChooseYourObjects](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="a9eab-186">Una volta completato il processo di reverse engineering il nuovo modello viene aggiunto al progetto e aperto per la visualizzazione in Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="a9eab-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="a9eab-187">Un file app. config è stato aggiunto anche al progetto con i dettagli della connessione per il database.</span><span class="sxs-lookup"><span data-stu-id="a9eab-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="a9eab-188">Passaggi aggiuntivi in Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="a9eab-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="a9eab-189">Se si lavora in Visual Studio 2010 è necessario aggiornare la finestra di progettazione di Entity Framework per utilizzare la generazione del codice di Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="a9eab-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="a9eab-190">Fare clic su un punto vuoto del modello in Entity Framework Designer e selezionare **Aggiungi elemento di generazione di codice...**</span><span class="sxs-lookup"><span data-stu-id="a9eab-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="a9eab-191">Selezionare **modelli Online** dal menu a sinistra e cercare **DbContext**</span><span class="sxs-lookup"><span data-stu-id="a9eab-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="a9eab-192">Selezionare il **EF 6.x generatore di DbContext per C\#,** immettere **ProductsModel** come il nome e fare clic su Aggiungi</span><span class="sxs-lookup"><span data-stu-id="a9eab-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="a9eab-193">L'aggiornamento di generazione del codice per il data binding</span><span class="sxs-lookup"><span data-stu-id="a9eab-193">Updating code generation for data binding</span></span>

<span data-ttu-id="a9eab-194">Entity Framework genera codice dal modello usando i modelli T4.</span><span class="sxs-lookup"><span data-stu-id="a9eab-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="a9eab-195">I modelli forniti con Visual Studio o scaricato da Visual Studio gallery servono per utilizzo generico.</span><span class="sxs-lookup"><span data-stu-id="a9eab-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="a9eab-196">Ciò significa che le entità generate da questi modelli semplici ICollection&lt;T&gt; proprietà.</span><span class="sxs-lookup"><span data-stu-id="a9eab-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="a9eab-197">Tuttavia, quando si esegue data binding WPF con è opportuno utilizzare **ObservableCollection** per le proprietà della raccolta in modo che WPF può tenere traccia delle modifiche apportate alle raccolte.</span><span class="sxs-lookup"><span data-stu-id="a9eab-197">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="a9eab-198">A tal fine verrà modificare i modelli per l'uso ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="a9eab-198">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="a9eab-199">Aprire il **Esplora soluzioni** e trovare **ProductModel.edmx** file</span><span class="sxs-lookup"><span data-stu-id="a9eab-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="a9eab-200">Trovare il **ProductModel.tt** file che verrà annidato in file ProductModel.edmx</span><span class="sxs-lookup"><span data-stu-id="a9eab-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![WpfProductModelTemplate](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="a9eab-202">Fare doppio clic sul file ProductModel.tt per aprirlo nell'editor di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9eab-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="a9eab-203">Trovare e sostituire le due occorrenze di "**ICollection**"con"**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="a9eab-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="a9eab-204">Questi si trovano approssimativamente alle righe 296 e 484.</span><span class="sxs-lookup"><span data-stu-id="a9eab-204">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="a9eab-205">Trova e Sostituisci la prima occorrenza di "**HashSet**"con"**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="a9eab-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="a9eab-206">Questo evento si trova approssimativamente alla riga 50.</span><span class="sxs-lookup"><span data-stu-id="a9eab-206">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="a9eab-207">**No** sostituire la seconda occorrenza della HashSet trovato in un secondo momento nel codice.</span><span class="sxs-lookup"><span data-stu-id="a9eab-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="a9eab-208">Trovare e sostituire l'unica occorrenza di "**System.Collections.Generic**"con"**ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="a9eab-208">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="a9eab-209">Questo si trova approssimativamente alla riga 424.</span><span class="sxs-lookup"><span data-stu-id="a9eab-209">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="a9eab-210">Salvare il file ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="a9eab-210">Save the ProductModel.tt file.</span></span> <span data-ttu-id="a9eab-211">Ciò deve generare il codice per le entità da rigenerare.</span><span class="sxs-lookup"><span data-stu-id="a9eab-211">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="a9eab-212">Se il codice non verranno rigenerati automaticamente, quindi fare clic con il pulsante destro su ProductModel.tt e scegliere "Esegui strumento personalizzato".</span><span class="sxs-lookup"><span data-stu-id="a9eab-212">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="a9eab-213">Se si apre ora il file Category.cs (che è annidato sotto ProductModel.tt), si dovrebbe vedere che la raccolta di prodotti ha il tipo **ObservableCollection&lt;prodotto&gt;**.</span><span class="sxs-lookup"><span data-stu-id="a9eab-213">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="a9eab-214">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="a9eab-214">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="a9eab-215">Caricamento lazy</span><span class="sxs-lookup"><span data-stu-id="a9eab-215">Lazy Loading</span></span>

<span data-ttu-id="a9eab-216">Il **prodotti** proprietà il **categoria** classe e **categoria** proprietà la **prodotto** classe sono le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a9eab-216">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="a9eab-217">In Entity Framework, le proprietà di navigazione offrono un modo per spostarsi in una relazione tra due tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="a9eab-217">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="a9eab-218">Entity Framework offre un'opzione per caricare entità correlate dal database automaticamente la prima volta che si accede alla proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="a9eab-218">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="a9eab-219">Con questo tipo di caricamento (denominato caricamento lazy), tenere presente che la prima volta che si accede a ogni proprietà di navigazione una query separata verrà eseguita sul database se il contenuto non è già nel contesto.</span><span class="sxs-lookup"><span data-stu-id="a9eab-219">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="a9eab-220">Quando si usano i tipi di entità POCO, Entity Framework consente di ottenere il caricamento lazy creando istanze di tipi proxy derivato in fase di esecuzione e quindi si esegue l'override proprietà virtuali nelle classi per aggiungere l'hook del caricamento.</span><span class="sxs-lookup"><span data-stu-id="a9eab-220">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="a9eab-221">Per ottenere il caricamento lazy di oggetti correlati, è necessario dichiarare navigazione getter di proprietà come **pubbliche** e **virtual** (**Overridable** in Visual Basic), e di classe non deve essere **sealed** (**NotOverridable** in Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="a9eab-221">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="a9eab-222">Quando il Database di utilizzando le proprietà di navigazione prima vengono eseguite automaticamente virtuale per abilitare il caricamento lazy.</span><span class="sxs-lookup"><span data-stu-id="a9eab-222">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="a9eab-223">Nella sezione Code First è stato scelto di rendere le proprietà di navigazione virtuale per lo stesso motivo</span><span class="sxs-lookup"><span data-stu-id="a9eab-223">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="a9eab-224">Associare oggetti ai controlli</span><span class="sxs-lookup"><span data-stu-id="a9eab-224">Bind Object to Controls</span></span>

<span data-ttu-id="a9eab-225">Aggiungere le classi definite nel modello come origini dati per questa applicazione WPF.</span><span class="sxs-lookup"><span data-stu-id="a9eab-225">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="a9eab-226">Fare doppio clic su **MainWindow. XAML** in Esplora soluzioni aprire il form principale</span><span class="sxs-lookup"><span data-stu-id="a9eab-226">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="a9eab-227">Dal menu principale, selezionare **progetto -&gt; Aggiungi nuova origine dati...**</span><span class="sxs-lookup"><span data-stu-id="a9eab-227">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="a9eab-228">(in Visual Studio 2010, è necessario selezionare **dati -&gt; Aggiungi nuova origine dati...** )</span><span class="sxs-lookup"><span data-stu-id="a9eab-228">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="a9eab-229">Nella pagina scegliere Typewindow un'origine dati, selezionare **oggetti** e fare clic su **successivo**</span><span class="sxs-lookup"><span data-stu-id="a9eab-229">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="a9eab-230">Selezionare la finestra di dialogo di oggetti dati, Espandi il **WPFwithEFSample** due volte e selezionare **categoria**</span><span class="sxs-lookup"><span data-stu-id="a9eab-230">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="a9eab-231">*Non è necessario selezionare il **prodotto** dell'origine dati, perché verrà visualizzato a esso tramite il **prodotto**del proprietà il **categoria** zdroj dat*</span><span class="sxs-lookup"><span data-stu-id="a9eab-231">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![SelectDataObjects](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="a9eab-233">Fare clic su **fine.**</span><span class="sxs-lookup"><span data-stu-id="a9eab-233">Click **Finish.**</span></span>
-   <span data-ttu-id="a9eab-234">Viene aperta la finestra Origini dati accanto alla finestra di MainWindow. XAML *se la finestra Origini dati non verrà visualizzati, selezionare **visualizzazione -&gt; Other Windows -&gt; Zdroje dat***</span><span class="sxs-lookup"><span data-stu-id="a9eab-234">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="a9eab-235">Premere l'icona della puntina, in modo che la finestra Origini dati non automaticamente nascondere.</span><span class="sxs-lookup"><span data-stu-id="a9eab-235">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="a9eab-236">Potrebbe essere necessario premere il pulsante Aggiorna se è già visualizzata la finestra.</span><span class="sxs-lookup"><span data-stu-id="a9eab-236">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Origini dati](~/ef6/media/datasources.png)

-   <span data-ttu-id="a9eab-238">Selezionare la * * categoria * * dei dati di origine e trascinarla nel form.</span><span class="sxs-lookup"><span data-stu-id="a9eab-238">Select the **Category **data source and drag it on the form.</span></span>

<span data-ttu-id="a9eab-239">Di seguito è accaduto quando abbiamo trascinato per questa origine:</span><span class="sxs-lookup"><span data-stu-id="a9eab-239">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="a9eab-240">Il **categoryViewSource** risorse e il * * controllo categoryDataGrid * * sono stati aggiunti a XAML.</span><span class="sxs-lookup"><span data-stu-id="a9eab-240">The **categoryViewSource** resource and the** categoryDataGrid** control were added to XAML.</span></span> <span data-ttu-id="a9eab-241">Per altre informazioni sulle DataViewSources, vedere http://bea.stollnitz.com/blog/?p=387.</span><span class="sxs-lookup"><span data-stu-id="a9eab-241">For more information about DataViewSources, see http://bea.stollnitz.com/blog/?p=387.</span></span>
-   <span data-ttu-id="a9eab-242">La proprietà DataContext sull'elemento della griglia padre è stata impostata su "{StaticResource **categoryViewSource** }".</span><span class="sxs-lookup"><span data-stu-id="a9eab-242">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span>  <span data-ttu-id="a9eab-243">Il **categoryViewSource** risorsa viene usata come origine del binding per l'outer\\elemento griglia padre.</span><span class="sxs-lookup"><span data-stu-id="a9eab-243">The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="a9eab-244">Gli elementi della griglia interni ereditano quindi il valore di DataContext dall'elemento padre della griglia (del categoryDataGrid ItemsSource è impostata su "{Binding}").</span><span class="sxs-lookup"><span data-stu-id="a9eab-244">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}").</span></span> 

``` xml
    <Window.Resources>
        <CollectionViewSource x:Key="categoryViewSource"
                                d:DesignSource="{d:DesignInstance {x:Type local:Category}, CreateList=True}"/>
    </Window.Resources>
    <Grid DataContext="{StaticResource categoryViewSource}">
        <DataGrid x:Name="categoryDataGrid" AutoGenerateColumns="False" EnableRowVirtualization="True"
                    ItemsSource="{Binding}" Margin="13,13,43,191"
                    RowDetailsVisibilityMode="VisibleWhenSelected">
            <DataGrid.Columns>
                <DataGridTextColumn x:Name="categoryIdColumn" Binding="{Binding CategoryId}"
                                    Header="Category Id" Width="SizeToHeader"/>
                <DataGridTextColumn x:Name="nameColumn" Binding="{Binding Name}"
                                    Header="Name" Width="SizeToHeader"/>
            </DataGrid.Columns>
        </DataGrid>
    </Grid>
```

## <a name="adding-a-details-grid"></a><span data-ttu-id="a9eab-245">Aggiungere una griglia dei dettagli</span><span class="sxs-lookup"><span data-stu-id="a9eab-245">Adding a Details Grid</span></span>

<span data-ttu-id="a9eab-246">Ora che abbiamo una griglia per visualizzare le categorie è possibile aggiungere una griglia di dettagli per visualizzare i prodotti associati.</span><span class="sxs-lookup"><span data-stu-id="a9eab-246">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="a9eab-247">Selezionare la * * prodotti * * proprietà sotto la * * categoria * * dei dati di origine e trascinarla nel form.</span><span class="sxs-lookup"><span data-stu-id="a9eab-247">Select the **Products **property from under the **Category **data source and drag it on the form.</span></span>
    -   <span data-ttu-id="a9eab-248">Il **categoryProductsViewSource** risorse e **productDataGrid** griglia vengono aggiunti a XAML</span><span class="sxs-lookup"><span data-stu-id="a9eab-248">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="a9eab-249">Il percorso di associazione per questa risorsa è impostato su prodotti</span><span class="sxs-lookup"><span data-stu-id="a9eab-249">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="a9eab-250">Framework di associazione dati WPF assicura che solo i prodotti correlati alla categoria selezionata viene visualizzata **productDataGrid**</span><span class="sxs-lookup"><span data-stu-id="a9eab-250">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="a9eab-251">Dalla casella degli strumenti, trascinare **pulsante** al form.</span><span class="sxs-lookup"><span data-stu-id="a9eab-251">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="a9eab-252">Impostare il **nome** proprietà **buttonSave** e il **contenuto** proprietà **Salva**.</span><span class="sxs-lookup"><span data-stu-id="a9eab-252">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="a9eab-253">Il modulo dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="a9eab-253">The form should look similar to this:</span></span>

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="a9eab-255">Aggiungere il codice che gestisce l'interazione dei dati</span><span class="sxs-lookup"><span data-stu-id="a9eab-255">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="a9eab-256">È possibile aggiungere alcuni gestori di eventi alla finestra principale.</span><span class="sxs-lookup"><span data-stu-id="a9eab-256">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="a9eab-257">Nella finestra XAML, fare clic sui  **&lt;finestra** elemento, si seleziona la finestra principale</span><span class="sxs-lookup"><span data-stu-id="a9eab-257">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="a9eab-258">Nel **delle proprietà** finestra scegliere **eventi** in alto a destra, quindi fare doppio clic sulla casella di testo a destra del **Loaded** etichetta</span><span class="sxs-lookup"><span data-stu-id="a9eab-258">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![MainWindowProperties](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="a9eab-260">Aggiungere anche il **fare clic su** evento per il **salvare** pulsante facendo doppio clic sul pulsante Salva nella finestra di progettazione.</span><span class="sxs-lookup"><span data-stu-id="a9eab-260">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="a9eab-261">Verrà visualizzata per il code-behind del form, ora si modificherà il codice per usare il ProductContext per eseguire l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="a9eab-261">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="a9eab-262">Aggiornare il codice per MainWindow come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="a9eab-262">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="a9eab-263">Il codice dichiara un'istanza con esecuzione prolungata **ProductContext**.</span><span class="sxs-lookup"><span data-stu-id="a9eab-263">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="a9eab-264">Il **ProductContext** oggetto viene usato per eseguire query e salvare i dati nel database.</span><span class="sxs-lookup"><span data-stu-id="a9eab-264">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="a9eab-265">Il **Dispose**() sulle **ProductContext** istanza viene quindi chiamata da sottoposto a override **OnClosing** (metodo).</span><span class="sxs-lookup"><span data-stu-id="a9eab-265">The **Dispose**() on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span> <span data-ttu-id="a9eab-266">I commenti del codice forniscono dettagli sulle operazioni eseguite dal codice.</span><span class="sxs-lookup"><span data-stu-id="a9eab-266">The code comments provide details about what the code does.</span></span>

``` csharp
    using System.Data.Entity;
    using System.Linq;
    using System.Windows;

    namespace WPFwithEFSample
    {
        public partial class MainWindow : Window
        {
            private ProductContext _context = new ProductContext();
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                System.Windows.Data.CollectionViewSource categoryViewSource =
                    ((System.Windows.Data.CollectionViewSource)(this.FindResource("categoryViewSource")));

                // Load is an extension method on IQueryable,
                // defined in the System.Data.Entity namespace.
                // This method enumerates the results of the query,
                // similar to ToList but without creating a list.
                // When used with Linq to Entities this method
                // creates entity objects and adds them to the context.
                _context.Categories.Load();

                // After the data is loaded call the DbSet<T>.Local property
                // to use the DbSet<T> as a binding source.
                categoryViewSource.Source = _context.Categories.Local;
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                // When you delete an object from the related entities collection
                // (in this case Products), the Entity Framework doesn’t mark
                // these child entities as deleted.
                // Instead, it removes the relationship between the parent and the child
                // by setting the parent reference to null.
                // So we manually have to delete the products
                // that have a Category reference set to null.

                // The following code uses LINQ to Objects
                // against the Local collection of Products.
                // The ToList call is required because otherwise the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can use LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                _context.SaveChanges();
                // Refresh the grids so the database generated values show up.
                this.categoryDataGrid.Items.Refresh();
                this.productsDataGrid.Items.Refresh();
            }

            protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }

    }
```

## <a name="test-the-wpf-application"></a><span data-ttu-id="a9eab-267">Testare l'applicazione WPF</span><span class="sxs-lookup"><span data-stu-id="a9eab-267">Test the WPF Application</span></span>

-   <span data-ttu-id="a9eab-268">Compilare l'applicazione ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="a9eab-268">Compile and run the application.</span></span> <span data-ttu-id="a9eab-269">Se è stato utilizzato Code First, quindi è possibile notare che un **WPFwithEFSample.ProductContext** database viene creato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="a9eab-269">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="a9eab-270">Immettere un nome di categoria nei nomi di prodotto e griglia superiore nella griglia inferiore *non immette alcuna stringa nelle colonne ID, poiché la chiave primaria è generata dal database*</span><span class="sxs-lookup"><span data-stu-id="a9eab-270">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Screen1](~/ef6/media/screen1.png)

-   <span data-ttu-id="a9eab-272">Premere il **salvare** consente di salvare i dati nel database</span><span class="sxs-lookup"><span data-stu-id="a9eab-272">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="a9eab-273">Dopo la chiamata a di DbContext **SaveChanges**(), gli ID vengono popolati con i valori del database generato.</span><span class="sxs-lookup"><span data-stu-id="a9eab-273">After the call to DbContext’s **SaveChanges**(), the IDs are populated with the database generated values.</span></span> <span data-ttu-id="a9eab-274">Perché abbiamo chiamato **Refresh**() dopo **SaveChanges**() la **DataGrid** controlli vengono aggiornati con i nuovi valori.</span><span class="sxs-lookup"><span data-stu-id="a9eab-274">Because we called **Refresh**() after **SaveChanges**() the **DataGrid** controls are updated with the new values as well.</span></span>

![Screen2](~/ef6/media/screen2.png)
