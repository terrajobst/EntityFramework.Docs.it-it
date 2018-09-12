---
title: Associazione dati con Windows Form - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 48e6d997875a25a5954484f854953df69a267d05
ms.sourcegitcommit: 8d04a2ad98036f32ca70c77ce3040c5edb1cdf82
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44384852"
---
# <a name="databinding-with-winforms"></a><span data-ttu-id="7412b-102">Associazione dati con Windows Form</span><span class="sxs-lookup"><span data-stu-id="7412b-102">Databinding with WinForms</span></span>
<span data-ttu-id="7412b-103">Questa procedura dettagliata illustra come associare tipi POCO ai controlli Windows Form (WinForms) sotto forma di "master-dettagli".</span><span class="sxs-lookup"><span data-stu-id="7412b-103">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="7412b-104">L'applicazione usa Entity Framework per popolare gli oggetti con i dati dal database, tenere traccia delle modifiche e rendere persistenti i dati nel database.</span><span class="sxs-lookup"><span data-stu-id="7412b-104">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="7412b-105">Il modello definisce due tipi che fanno parte di relazione uno-a-molti: categoria (principal\\master) e il prodotto (dipendenti\\dettaglio).</span><span class="sxs-lookup"><span data-stu-id="7412b-105">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="7412b-106">Quindi, gli strumenti di Visual Studio vengono utilizzati per associare i tipi definiti nel modello per i controlli Windows Form.</span><span class="sxs-lookup"><span data-stu-id="7412b-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="7412b-107">Il framework di data binding di Windows Form consente la navigazione tra gli oggetti correlati: se si seleziona le righe della visualizzazione master, la visualizzazione di dettaglio da aggiornare con i dati figlio corrispondente.</span><span class="sxs-lookup"><span data-stu-id="7412b-107">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="7412b-108">Le schermate e i listati di codice in questa procedura dettagliata vengono forniti da Visual Studio 2013, ma è possibile completare questa procedura dettagliata con Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="7412b-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="7412b-109">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="7412b-109">Pre-Requisites</span></span>

<span data-ttu-id="7412b-110">È necessario disporre di Visual Studio 2013, Visual Studio 2012 o Visual Studio 2010 per poter completare questa procedura dettagliata.</span><span class="sxs-lookup"><span data-stu-id="7412b-110">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="7412b-111">Se si usa Visual Studio 2010, è necessario anche installare NuGet.</span><span class="sxs-lookup"><span data-stu-id="7412b-111">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="7412b-112">Per altre informazioni, vedere [installazione di NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span><span class="sxs-lookup"><span data-stu-id="7412b-112">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="7412b-113">Creare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="7412b-113">Create the Application</span></span>

-   <span data-ttu-id="7412b-114">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7412b-114">Open Visual Studio</span></span>
-   <span data-ttu-id="7412b-115">**File -&gt; New -&gt; progetto...**</span><span class="sxs-lookup"><span data-stu-id="7412b-115">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="7412b-116">Selezionare **Windows** nel riquadro sinistro e **FormsApplication Windows** nel riquadro di destra</span><span class="sxs-lookup"><span data-stu-id="7412b-116">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="7412b-117">Immettere **WinFormswithEFSample** come nome</span><span class="sxs-lookup"><span data-stu-id="7412b-117">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="7412b-118">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="7412b-118">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="7412b-119">Installare il pacchetto NuGet di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="7412b-119">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="7412b-120">In Esplora soluzioni fare clic sui **WinFormswithEFSample** progetto</span><span class="sxs-lookup"><span data-stu-id="7412b-120">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="7412b-121">Selezionare **Gestisci pacchetti NuGet...**</span><span class="sxs-lookup"><span data-stu-id="7412b-121">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="7412b-122">Nella finestra di dialogo Gestisci pacchetti NuGet, selezionare la **Online** scheda e scegliere il **EntityFramework** pacchetto</span><span class="sxs-lookup"><span data-stu-id="7412b-122">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="7412b-123">Fare clic su **installare**</span><span class="sxs-lookup"><span data-stu-id="7412b-123">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="7412b-124">Oltre all'assembly EntityFramework viene anche aggiunto un riferimento a System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="7412b-124">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="7412b-125">Se il progetto contiene un riferimento a Data. Entity, quindi verrà rimosso quando viene installato il pacchetto di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7412b-125">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="7412b-126">L'assembly Data. Entity non è più utilizzato per le applicazioni Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="7412b-126">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="7412b-127">Implementazione di IListSource per le raccolte</span><span class="sxs-lookup"><span data-stu-id="7412b-127">Implementing IListSource for Collections</span></span>

<span data-ttu-id="7412b-128">Le proprietà della raccolta devono implementare l'interfaccia IListSource per abilitare l'associazione dati bidirezionale con l'ordinamento quando si usa Windows Form.</span><span class="sxs-lookup"><span data-stu-id="7412b-128">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="7412b-129">Per eseguire questa operazione si intende estendere ObservableCollection per aggiungere funzionalità IListSource.</span><span class="sxs-lookup"><span data-stu-id="7412b-129">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="7412b-130">Aggiungere un **ObservableListSource** classe al progetto:</span><span class="sxs-lookup"><span data-stu-id="7412b-130">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="7412b-131">Fare doppio clic sul nome del progetto</span><span class="sxs-lookup"><span data-stu-id="7412b-131">Right-click on the project name</span></span>
    -   <span data-ttu-id="7412b-132">Selezionare **Add -&gt; nuovo elemento**</span><span class="sxs-lookup"><span data-stu-id="7412b-132">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="7412b-133">Selezionare **classe** e immettere **ObservableListSource** per il nome della classe</span><span class="sxs-lookup"><span data-stu-id="7412b-133">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="7412b-134">Sostituire il codice generato per impostazione predefinita con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7412b-134">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="7412b-135">*Questa classe consente bidirezionale dei dati di associazione, nonché l'ordinamento. La classe deriva dalla classe ObservableCollection&lt;T&gt; e aggiunge un'implementazione esplicita di IListSource. Il metodo di IListSource GetList () viene implementato per restituire un'implementazione di IBindingList che resta sincronizzata con la classe ObservableCollection. L'implementazione di IBindingList generato da ToBindingList supporta l'ordinamento. Il metodo di estensione ToBindingList è definito nell'assembly EntityFramework.*</span><span class="sxs-lookup"><span data-stu-id="7412b-135">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extention method is defined in the EntityFramework assembly.*</span></span>

``` csharp
    using System.Collections;
    using System.Collections.Generic;
    using System.Collections.ObjectModel;
    using System.ComponentModel;
    using System.Diagnostics.CodeAnalysis;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public class ObservableListSource<T> : ObservableCollection<T>, IListSource
            where T : class
        {
            private IBindingList _bindingList;

            bool IListSource.ContainsListCollection { get { return false; } }

            IList IListSource.GetList()
            {
                return _bindingList ?? (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a><span data-ttu-id="7412b-136">Definire un modello</span><span class="sxs-lookup"><span data-stu-id="7412b-136">Define a Model</span></span>

<span data-ttu-id="7412b-137">In questa procedura dettagliata che è possibile scelta di implementare un modello utilizzando Code First o Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="7412b-137">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="7412b-138">Completare una delle due sezioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="7412b-138">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="7412b-139">Opzione 1: Definire un modello utilizzando Code First</span><span class="sxs-lookup"><span data-stu-id="7412b-139">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="7412b-140">In questa sezione viene illustrato come creare un modello e il database associato utilizzando Code First.</span><span class="sxs-lookup"><span data-stu-id="7412b-140">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="7412b-141">Passare alla sezione successiva (**Option 2: definire un modello utilizzando Database First)** se si preferisce usare Database First da invertire progettare il modello da un database utilizzando la finestra di progettazione di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="7412b-141">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="7412b-142">Quando si usa lo sviluppo Code First è in genere iniziare la scrittura di classi .NET Framework che definiscono il modello concettuale (dominio).</span><span class="sxs-lookup"><span data-stu-id="7412b-142">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="7412b-143">Aggiungere un nuovo **prodotto** classe al progetto</span><span class="sxs-lookup"><span data-stu-id="7412b-143">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="7412b-144">Sostituire il codice generato per impostazione predefinita con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7412b-144">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }
```

-   <span data-ttu-id="7412b-145">Aggiungere un **categoria** classe al progetto.</span><span class="sxs-lookup"><span data-stu-id="7412b-145">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="7412b-146">Sostituire il codice generato per impostazione predefinita con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7412b-146">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Category
        {
            private readonly ObservableListSource<Product> _products =
                    new ObservableListSource<Product>();

            public int CategoryId { get; set; }
            public string Name { get; set; }
            public virtual ObservableListSource<Product> Products { get { return _products; } }
        }
    }
```

<span data-ttu-id="7412b-147">Oltre a definire le entità, è necessario definire una classe che deriva da **DbContext** ed espone **DbSet&lt;TEntity&gt;**  proprietà.</span><span class="sxs-lookup"><span data-stu-id="7412b-147">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="7412b-148">Il **DbSet** proprietà informare il contesto di quali tipi di cui si desidera includere nel modello.</span><span class="sxs-lookup"><span data-stu-id="7412b-148">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="7412b-149">Il **DbContext** e **DbSet** tipi sono definiti nell'assembly EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="7412b-149">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="7412b-150">Un'istanza del tipo DbContext derivata gestisce gli oggetti entità durante la fase di esecuzione, che include la compilazione di oggetti con i dati da un database, modificare i dati di rilevamento e persistenza nel database.</span><span class="sxs-lookup"><span data-stu-id="7412b-150">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="7412b-151">Aggiungere un nuovo **ProductContext** classe al progetto.</span><span class="sxs-lookup"><span data-stu-id="7412b-151">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="7412b-152">Sostituire il codice generato per impostazione predefinita con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7412b-152">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Text;

    namespace WinFormswithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="7412b-153">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="7412b-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="7412b-154">Opzione 2: Definire un modello utilizzando Database First</span><span class="sxs-lookup"><span data-stu-id="7412b-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="7412b-155">Questa sezione illustra come usare Database First per decompilare il modello da un database utilizzando la finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7412b-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="7412b-156">Se è stata completata la sezione precedente (**Option 1: definire un modello utilizzando Code First)**, quindi ignorare questa sezione e passare direttamente alla sezione la **il caricamento Lazy** sezione.</span><span class="sxs-lookup"><span data-stu-id="7412b-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="7412b-157">Creare un Database esistente</span><span class="sxs-lookup"><span data-stu-id="7412b-157">Create an Existing Database</span></span>

<span data-ttu-id="7412b-158">In genere quando si intende usare un database esistente che verrà già creato, ma per questa procedura dettagliata è necessario creare un database a cui accedere.</span><span class="sxs-lookup"><span data-stu-id="7412b-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="7412b-159">Il server di database che viene installato con Visual Studio è diverso a seconda della versione di Visual Studio installata:</span><span class="sxs-lookup"><span data-stu-id="7412b-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="7412b-160">Se si usa Visual Studio 2010 si creerà un database SQL Express.</span><span class="sxs-lookup"><span data-stu-id="7412b-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="7412b-161">Se si usa Visual Studio 2012, si creerà una [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span><span class="sxs-lookup"><span data-stu-id="7412b-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="7412b-162">È possibile procedere e generare il database.</span><span class="sxs-lookup"><span data-stu-id="7412b-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="7412b-163">**Visualizzazione -&gt; Esplora Server**</span><span class="sxs-lookup"><span data-stu-id="7412b-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="7412b-164">Fare clic con il pulsante destro sul **connessioni dati -&gt; Aggiungi connessione...**</span><span class="sxs-lookup"><span data-stu-id="7412b-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="7412b-165">Se si è ancora connessi a un database da Esplora Server prima che è necessario selezionare Microsoft SQL Server come origine dati</span><span class="sxs-lookup"><span data-stu-id="7412b-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Modifica origine dati](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="7412b-167">Connettersi a LocalDB o SQL Express, in base alla quale è stato installato e immettere **prodotti** come nome del database</span><span class="sxs-lookup"><span data-stu-id="7412b-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Aggiungi connessione LocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![Aggiungi connessione Express](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="7412b-170">Selezionare **OK** e verrà richiesto se si desidera creare un nuovo database, selezionare **Sì**</span><span class="sxs-lookup"><span data-stu-id="7412b-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Crea database](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="7412b-172">Il nuovo database verrà ora visualizzato in Esplora Server, su di esso e scegliere **nuova Query**</span><span class="sxs-lookup"><span data-stu-id="7412b-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="7412b-173">Copiare il codice SQL seguente nella nuova query, quindi fare clic su query e selezionare **Execute**</span><span class="sxs-lookup"><span data-stu-id="7412b-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="7412b-174">Modelli di reverse engineering</span><span class="sxs-lookup"><span data-stu-id="7412b-174">Reverse Engineer Model</span></span>

<span data-ttu-id="7412b-175">Dobbiamo usare Entity Framework Designer, che è incluso come parte di Visual Studio, per creare il modello.</span><span class="sxs-lookup"><span data-stu-id="7412b-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="7412b-176">**Progetto -&gt; Aggiungi nuovo elemento...**</span><span class="sxs-lookup"><span data-stu-id="7412b-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="7412b-177">Selezionare **Data** nel menu a sinistra e quindi **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="7412b-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="7412b-178">Immettere **ProductModel** come nome, quindi fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="7412b-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="7412b-179">Verrà avviata la **procedura guidata Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="7412b-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="7412b-180">Selezionare **genera da Database** e fare clic su **successivo**</span><span class="sxs-lookup"><span data-stu-id="7412b-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="7412b-182">Selezionare la connessione al database creato nella prima sezione, immettere **ProductContext** come nome della stringa di connessione, scegliere **successivo**</span><span class="sxs-lookup"><span data-stu-id="7412b-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Scegliere la connessione](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="7412b-184">Selezionare la casella di controllo accanto a 'Tabelle' per importare tutte le tabelle e fare clic su 'Fine'</span><span class="sxs-lookup"><span data-stu-id="7412b-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Scegli oggetti di](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="7412b-186">Una volta completato il processo di reverse engineering il nuovo modello viene aggiunto al progetto e aperto per la visualizzazione in Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="7412b-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="7412b-187">Un file app. config è stato aggiunto anche al progetto con i dettagli della connessione per il database.</span><span class="sxs-lookup"><span data-stu-id="7412b-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="7412b-188">Passaggi aggiuntivi in Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="7412b-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="7412b-189">Se si lavora in Visual Studio 2010 è necessario aggiornare la finestra di progettazione di Entity Framework per utilizzare la generazione del codice di Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="7412b-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="7412b-190">Fare clic su un punto vuoto del modello in Entity Framework Designer e selezionare **Aggiungi elemento di generazione di codice...**</span><span class="sxs-lookup"><span data-stu-id="7412b-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="7412b-191">Selezionare **modelli Online** dal menu a sinistra e cercare **DbContext**</span><span class="sxs-lookup"><span data-stu-id="7412b-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="7412b-192">Selezionare il **EF 6.x generatore di DbContext per C\#,** immettere **ProductsModel** come il nome e fare clic su Aggiungi</span><span class="sxs-lookup"><span data-stu-id="7412b-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="7412b-193">L'aggiornamento di generazione del codice per il data binding</span><span class="sxs-lookup"><span data-stu-id="7412b-193">Updating code generation for data binding</span></span>

<span data-ttu-id="7412b-194">Entity Framework genera codice dal modello usando i modelli T4.</span><span class="sxs-lookup"><span data-stu-id="7412b-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="7412b-195">I modelli forniti con Visual Studio o scaricato da Visual Studio gallery servono per utilizzo generico.</span><span class="sxs-lookup"><span data-stu-id="7412b-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="7412b-196">Ciò significa che le entità generate da questi modelli semplici ICollection&lt;T&gt; proprietà.</span><span class="sxs-lookup"><span data-stu-id="7412b-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="7412b-197">Tuttavia, quando si esegue il data binding è preferibile usare proprietà di raccolta che implementano IListSource.</span><span class="sxs-lookup"><span data-stu-id="7412b-197">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="7412b-198">Questo è il motivo per cui viene creata la classe ObservableListSource precedente e verrà ora modificare i modelli per rendere utilizzare di questa classe.</span><span class="sxs-lookup"><span data-stu-id="7412b-198">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="7412b-199">Aprire il **Esplora soluzioni** e trovare **ProductModel.edmx** file</span><span class="sxs-lookup"><span data-stu-id="7412b-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="7412b-200">Trovare il **ProductModel.tt** file che verrà annidato in file ProductModel.edmx</span><span class="sxs-lookup"><span data-stu-id="7412b-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Modello di prodotto](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="7412b-202">Fare doppio clic sul file ProductModel.tt per aprirlo nell'editor di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7412b-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="7412b-203">Trovare e sostituire le due occorrenze di "**ICollection**"con"**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="7412b-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="7412b-204">Questi si trovano in circa righe 296 e 484.</span><span class="sxs-lookup"><span data-stu-id="7412b-204">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="7412b-205">Trova e Sostituisci la prima occorrenza di "**HashSet**"con"**ObservableListSource**".</span><span class="sxs-lookup"><span data-stu-id="7412b-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="7412b-206">Questo evento si trova circa alla riga 50.</span><span class="sxs-lookup"><span data-stu-id="7412b-206">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="7412b-207">**No** sostituire la seconda occorrenza della HashSet trovato in un secondo momento nel codice.</span><span class="sxs-lookup"><span data-stu-id="7412b-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="7412b-208">Salvare il file ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="7412b-208">Save the ProductModel.tt file.</span></span> <span data-ttu-id="7412b-209">Ciò deve generare il codice per le entità da rigenerare.</span><span class="sxs-lookup"><span data-stu-id="7412b-209">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="7412b-210">Se il codice non verranno rigenerati automaticamente, quindi fare clic con il pulsante destro su ProductModel.tt e scegliere "Esegui strumento personalizzato".</span><span class="sxs-lookup"><span data-stu-id="7412b-210">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="7412b-211">Se si apre ora il file Category.cs (che è annidato sotto ProductModel.tt), si dovrebbe vedere che la raccolta di prodotti ha il tipo **ObservableListSource&lt;prodotto&gt;**.</span><span class="sxs-lookup"><span data-stu-id="7412b-211">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="7412b-212">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="7412b-212">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="7412b-213">Caricamento lazy</span><span class="sxs-lookup"><span data-stu-id="7412b-213">Lazy Loading</span></span>

<span data-ttu-id="7412b-214">Il **prodotti** proprietà il **categoria** classe e **categoria** proprietà la **prodotto** classe sono le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="7412b-214">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="7412b-215">In Entity Framework, le proprietà di navigazione offrono un modo per spostarsi in una relazione tra due tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="7412b-215">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="7412b-216">Entity Framework offre un'opzione per caricare entità correlate dal database automaticamente la prima volta che si accede alla proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="7412b-216">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="7412b-217">Con questo tipo di caricamento (denominato caricamento lazy), tenere presente che la prima volta che si accede a ogni proprietà di navigazione una query separata verrà eseguita sul database se il contenuto non è già nel contesto.</span><span class="sxs-lookup"><span data-stu-id="7412b-217">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="7412b-218">Quando si usano i tipi di entità POCO, Entity Framework consente di ottenere il caricamento lazy creando istanze di tipi proxy derivato in fase di esecuzione e quindi si esegue l'override proprietà virtuali nelle classi per aggiungere l'hook del caricamento.</span><span class="sxs-lookup"><span data-stu-id="7412b-218">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="7412b-219">Per ottenere il caricamento lazy di oggetti correlati, è necessario dichiarare navigazione getter di proprietà come **pubbliche** e **virtual** (**Overridable** in Visual Basic), e di classe non deve essere **sealed** (**NotOverridable** in Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="7412b-219">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="7412b-220">Quando il Database di utilizzando le proprietà di navigazione prima vengono eseguite automaticamente virtuale per abilitare il caricamento lazy.</span><span class="sxs-lookup"><span data-stu-id="7412b-220">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="7412b-221">Nella sezione Code First è stato scelto di rendere le proprietà di navigazione virtuale per lo stesso motivo</span><span class="sxs-lookup"><span data-stu-id="7412b-221">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="7412b-222">Associare oggetti ai controlli</span><span class="sxs-lookup"><span data-stu-id="7412b-222">Bind Object to Controls</span></span>

<span data-ttu-id="7412b-223">Aggiungere le classi definite nel modello come origini dati per questa applicazione Windows Form.</span><span class="sxs-lookup"><span data-stu-id="7412b-223">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="7412b-224">Dal menu principale, selezionare **progetto -&gt; Aggiungi nuova origine dati...**</span><span class="sxs-lookup"><span data-stu-id="7412b-224">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="7412b-225">(in Visual Studio 2010, è necessario selezionare **dati -&gt; Aggiungi nuova origine dati...** )</span><span class="sxs-lookup"><span data-stu-id="7412b-225">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="7412b-226">Nella pagina di scegliere una finestra del tipo di origine dati, selezionare **oggetti** e fare clic su **successivo**</span><span class="sxs-lookup"><span data-stu-id="7412b-226">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="7412b-227">Selezionare la finestra di dialogo di oggetti dati, Espandi il **WinFormswithEFSample** due volte e selezionare **categoria** non è necessario selezionare l'origine dati del prodotto, perché si otterrà ad esso tramite il prodotto proprietà dell'origine dati di categoria.</span><span class="sxs-lookup"><span data-stu-id="7412b-227">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![origine dati](~/ef6/media/datasource.png)

-   <span data-ttu-id="7412b-229">Fare clic su **Finish.** 
     *Se la finestra Origini dati non verrà visualizzati, selezionare \* \* \* - visualizzazione&gt; Other Windows -&gt; Zdroje dat*\*</span><span class="sxs-lookup"><span data-stu-id="7412b-229">Click **Finish.**
*If the Data Sources window is not showing up, select\*\*\*View -&gt; Other Windows-&gt; Data Sources*\*</span></span>
-   <span data-ttu-id="7412b-230">Premere l'icona della puntina, in modo che la finestra Origini dati non automaticamente nascondere.</span><span class="sxs-lookup"><span data-stu-id="7412b-230">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="7412b-231">Potrebbe essere necessario premere il pulsante Aggiorna se è già visualizzata la finestra.</span><span class="sxs-lookup"><span data-stu-id="7412b-231">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Origine dati 2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="7412b-233">In Esplora soluzioni fare doppio clic il **Form1.cs** file per aprire il form principale nella finestra di progettazione.</span><span class="sxs-lookup"><span data-stu-id="7412b-233">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="7412b-234">Selezionare il **categoria** origine dati e trascinarlo nel form.</span><span class="sxs-lookup"><span data-stu-id="7412b-234">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="7412b-235">Per impostazione predefinita, un DataGridView nuovo (**categoryDataGridView**) e i controlli della barra degli strumenti di navigazione vengono aggiunti alla finestra di progettazione.</span><span class="sxs-lookup"><span data-stu-id="7412b-235">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="7412b-236">Questi controlli sono associati a BindingSource (**categoryBindingSource**) e associazione Navigator (**categoryBindingNavigator**) che vengono creati anche i componenti.</span><span class="sxs-lookup"><span data-stu-id="7412b-236">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="7412b-237">Modificare le colonne nel **categoryDataGridView**.</span><span class="sxs-lookup"><span data-stu-id="7412b-237">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="7412b-238">Sarà necessario impostare il **CategoryId** colonna in sola lettura.</span><span class="sxs-lookup"><span data-stu-id="7412b-238">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="7412b-239">Il valore per il **CategoryId** proprietà viene generata dal database risultato del salvataggio dei dati.</span><span class="sxs-lookup"><span data-stu-id="7412b-239">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="7412b-240">Il pulsante destro del controllo DataGridView e selezionare Modifica colonne...</span><span class="sxs-lookup"><span data-stu-id="7412b-240">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="7412b-241">Selezionare la colonna CategoryId e imposta ReadOnly su True</span><span class="sxs-lookup"><span data-stu-id="7412b-241">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="7412b-242">Fare clic su OK</span><span class="sxs-lookup"><span data-stu-id="7412b-242">Press OK</span></span>
-   <span data-ttu-id="7412b-243">Selezionare i prodotti che si trova sotto l'origine dati di categoria e trascinarla nel form.</span><span class="sxs-lookup"><span data-stu-id="7412b-243">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="7412b-244">Il productDataGridView e productBindingSource vengono aggiunti al form.</span><span class="sxs-lookup"><span data-stu-id="7412b-244">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="7412b-245">Modificare le colonne di productDataGridView.</span><span class="sxs-lookup"><span data-stu-id="7412b-245">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="7412b-246">Si vuole nascondere le colonne CategoryId e la categoria e impostare ProductId in sola lettura.</span><span class="sxs-lookup"><span data-stu-id="7412b-246">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="7412b-247">Risultato del salvataggio dei dati, il valore per la proprietà ProductId viene generato dal database.</span><span class="sxs-lookup"><span data-stu-id="7412b-247">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="7412b-248">Il pulsante destro del controllo DataGridView e selezionare **Modifica colonne...** .</span><span class="sxs-lookup"><span data-stu-id="7412b-248">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="7412b-249">Selezionare il **ProductId** colonna e impostare **ReadOnly** al **True**.</span><span class="sxs-lookup"><span data-stu-id="7412b-249">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="7412b-250">Selezionare il **CategoryId** colonna e premere il **rimuovere** pulsante.</span><span class="sxs-lookup"><span data-stu-id="7412b-250">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="7412b-251">Eseguire la stessa operazione con il **categoria** colonna.</span><span class="sxs-lookup"><span data-stu-id="7412b-251">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="7412b-252">Premere **OK**.</span><span class="sxs-lookup"><span data-stu-id="7412b-252">Press **OK**.</span></span>

    <span data-ttu-id="7412b-253">Finora, sono associati ai nostri controlli DataGridView BindingSource componenti nella finestra di progettazione.</span><span class="sxs-lookup"><span data-stu-id="7412b-253">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="7412b-254">Nella sezione successiva si aggiungerà codice per il code-behind per impostare categoryBindingSource.DataSource alla raccolta di entità che vengono attualmente rilevate dall'oggetto DbContext.</span><span class="sxs-lookup"><span data-stu-id="7412b-254">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="7412b-255">Quando viene trascinato e rilasciato i prodotti che si trova sotto la categoria, il Windows Form ha richiesto automaticamente di impostazione di proprietà productsBindingSource.DataSource da proprietà categoryBindingSource e productsBindingSource.DataMember ai prodotti.</span><span class="sxs-lookup"><span data-stu-id="7412b-255">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="7412b-256">A causa di questa associazione, nel productDataGridView verranno visualizzati solo i prodotti appartenenti alla categoria correntemente selezionata.</span><span class="sxs-lookup"><span data-stu-id="7412b-256">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="7412b-257">Abilitare la **salvare** sulla barra degli strumenti di spostamento fare clic sul pulsante destro del mouse e selezionando **abilitato**.</span><span class="sxs-lookup"><span data-stu-id="7412b-257">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![Finestra di progettazione modulo 1](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="7412b-259">Aggiungere il gestore eventi per il salvataggio pulsante facendo doppio clic sul pulsante.</span><span class="sxs-lookup"><span data-stu-id="7412b-259">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="7412b-260">Questo verrà aggiunto il gestore dell'evento e consentono di ottenere il code-behind del form.</span><span class="sxs-lookup"><span data-stu-id="7412b-260">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="7412b-261">Il codice per il **categoryBindingNavigatorSaveItem\_fare clic su** gestore eventi verrà aggiunto nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="7412b-261">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="7412b-262">Aggiungere il codice che gestisce l'interazione dei dati</span><span class="sxs-lookup"><span data-stu-id="7412b-262">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="7412b-263">A questo punto si aggiungerà il codice per usare il ProductContext per eseguire l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="7412b-263">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="7412b-264">Aggiornare il codice per la finestra del form principale, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="7412b-264">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="7412b-265">Il codice dichiara un'istanza con esecuzione prolungata di ProductContext.</span><span class="sxs-lookup"><span data-stu-id="7412b-265">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="7412b-266">L'oggetto ProductContext viene usato per eseguire query e salvare i dati nel database.</span><span class="sxs-lookup"><span data-stu-id="7412b-266">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="7412b-267">Il metodo Dispose () sull'istanza ProductContext viene quindi chiamato dal metodo OnClosing sottoposto a override.</span><span class="sxs-lookup"><span data-stu-id="7412b-267">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="7412b-268">I commenti del codice forniscono dettagli sulle operazioni eseguite dal codice.</span><span class="sxs-lookup"><span data-stu-id="7412b-268">The code comments provide details about what the code does.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public partial class Form1 : Form
        {
            ProductContext _context;
            public Form1()
            {
                InitializeComponent();
            }

            protected override void OnLoad(EventArgs e)
            {
                base.OnLoad(e);
                _context = new ProductContext();

                // Call the Load method to get the data for the given DbSet
                // from the database.
                // The data is materialized as entities. The entities are managed by
                // the DbContext instance.
                _context.Categories.Load();

                // Bind the categoryBindingSource.DataSource to
                // all the Unchanged, Modified and Added Category objects that
                // are currently tracked by the DbContext.
                // Note that we need to call ToBindingList() on the
                // ObservableCollection<TEntity> returned by
                // the DbSet.Local property to get the BindingList<T>
                // in order to facilitate two-way binding in WinForms.
                this.categoryBindingSource.DataSource =
                    _context.Categories.Local.ToBindingList();
            }

            private void categoryBindingNavigatorSaveItem_Click(object sender, EventArgs e)
            {
                this.Validate();

                // Currently, the Entity Framework doesn’t mark the entities
                // that are removed from a navigation property (in our example the Products)
                // as deleted in the context.
                // The following code uses LINQ to Objects against the Local collection
                // to find all products and marks any that do not have
                // a Category reference as deleted.
                // The ToList call is required because otherwise
                // the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can do LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                // Save the changes to the database.
                this._context.SaveChanges();

                // Refresh the controls to show the values         
                // that were generated by the database.
                this.categoryDataGridView.Refresh();
                this.productsDataGridView.Refresh();
            }

            protected override void OnClosing(CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }
    }
```

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="7412b-269">Testare l'applicazione di Windows Form</span><span class="sxs-lookup"><span data-stu-id="7412b-269">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="7412b-270">Compilazione ed eseguire l'applicazione è possibile testare la funzionalità.</span><span class="sxs-lookup"><span data-stu-id="7412b-270">Compile and run the application and you can test out the functionality.</span></span>

    ![Modulo 1 prima del salvataggio](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="7412b-272">Dopo il salvataggio le chiavi di archiviazione generati vengono visualizzate sullo schermo.</span><span class="sxs-lookup"><span data-stu-id="7412b-272">After saving the store generated keys are shown on the screen.</span></span>

    ![Modulo 1 dopo salvataggio](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="7412b-274">Se è stato utilizzato Code First, quindi si noterà anche che un **WinFormswithEFSample.ProductContext** database viene creato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="7412b-274">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![Esplora oggetti di server](~/ef6/media/serverobjexplorer.png)
