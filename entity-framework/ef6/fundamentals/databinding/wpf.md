---
title: Associazione dati con WPF - EF6Databinding with WPF - EF6
author: divega
ms.date: 04/02/2020
ms.assetid: e90d48e6-bea7785-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 6908e2a7597d0c199620c6015ed3ea06226c5ea9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639148"
---
> [!IMPORTANT]
> <span data-ttu-id="2b107-102">**Questo documento è valido solo per WPF in .NET Framework**</span><span class="sxs-lookup"><span data-stu-id="2b107-102">**This document is valid for WPF on the .NET Framework only**</span></span>
>
> <span data-ttu-id="2b107-103">In questo documento viene descritta l'associazione dati per WPF in .NET Framework.This document describes databinding for WPF on the .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2b107-103">This document describes databinding for WPF on the .NET Framework.</span></span> <span data-ttu-id="2b107-104">Per i nuovi progetti .NET Core, è consigliabile usare Entity Framework Core anziché Entity Framework 6.For new .NET Core projects, we recommend use [EF Core](/ef/core) instead of Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="2b107-104">For new .NET Core projects, we recommend you use [EF Core](/ef/core) instead of Entity Framework 6.</span></span> <span data-ttu-id="2b107-105">La documentazione per l'associazione dati in EF Core viene tenuta traccia in [Issue #778](https://github.com/dotnet/EntityFramework.Docs/issues/778).</span><span class="sxs-lookup"><span data-stu-id="2b107-105">The documentation for databinding in EF Core is tracked in [Issue #778](https://github.com/dotnet/EntityFramework.Docs/issues/778).</span></span>

# <a name="databinding-with-wpf"></a><span data-ttu-id="2b107-106">Data binding con WPF</span><span class="sxs-lookup"><span data-stu-id="2b107-106">Databinding with WPF</span></span>
<span data-ttu-id="2b107-107">In questa procedura dettagliata viene illustrato come associare i tipi POCO ai controlli WPF in un form "master-detail".</span><span class="sxs-lookup"><span data-stu-id="2b107-107">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="2b107-108">L'applicazione usa le API di Entity Framework per popolare gli oggetti con i dati dal database, tenere traccia delle modifiche e rendere persistenti i dati nel database.</span><span class="sxs-lookup"><span data-stu-id="2b107-108">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="2b107-109">Il modello definisce due tipi che partecipano **Category** alla relazione\\uno-a-molti: Categoria (principale master) e **Prodotto** (dettaglio dipendente).\\</span><span class="sxs-lookup"><span data-stu-id="2b107-109">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="2b107-110">Quindi, gli strumenti di Visual Studio vengono utilizzati per associare i tipi definiti nel modello per i controlli WPFWPF.</span><span class="sxs-lookup"><span data-stu-id="2b107-110">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="2b107-111">Il framework di associazione dati WPFWPF consente lo spostamento tra oggetti correlati: la selezione di righe nella visualizzazione master causa l'aggiornamento della visualizzazione dettagli con i dati figlio corrispondenti.</span><span class="sxs-lookup"><span data-stu-id="2b107-111">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="2b107-112">Le schermate e gli elenchi di codice in questa procedura dettagliata sono tratti da Visual Studio 2013, ma è possibile completare questa procedura dettagliata con Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="2b107-112">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="2b107-113">Usare l'opzione 'Oggetto' per la creazione di origini dati WPFUse the 'Object' Option for Creating WPF Data Sources</span><span class="sxs-lookup"><span data-stu-id="2b107-113">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="2b107-114">Con la versione precedente di Entity Framework è consigliabile usare l'opzione Database quando si crea una nuova origine dati basata su un modello creato con la finestra di progettazione di Entity Framework.With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span><span class="sxs-lookup"><span data-stu-id="2b107-114">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="2b107-115">Ciò è dovuto al fatto che la finestra di progettazione genera un contesto derivato da ObjectContext e le classi di entità derivate da EntityObject.This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span><span class="sxs-lookup"><span data-stu-id="2b107-115">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="2b107-116">L'uso dell'opzione Database ti aiuterà a scrivere il codice migliore per interagire con questa superficie API.</span><span class="sxs-lookup"><span data-stu-id="2b107-116">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="2b107-117">Le finestre di progettazione di Entity Framework per Visual Studio 2012 e Visual Studio 2013 generano un contesto che deriva da DbContext insieme a semplici classi di entità POCO.</span><span class="sxs-lookup"><span data-stu-id="2b107-117">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="2b107-118">Con Visual Studio 2010 è consigliabile passare a un modello di generazione di codice che usa DbContext come descritto più avanti in questa procedura dettagliata.</span><span class="sxs-lookup"><span data-stu-id="2b107-118">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="2b107-119">Quando si usa la superficie dell'API DbContext è necessario usare l'opzione **Oggetto** quando si crea una nuova origine dati, come illustrato in questa procedura dettagliata.</span><span class="sxs-lookup"><span data-stu-id="2b107-119">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="2b107-120">Se necessario, è possibile ripristinare la generazione di [codice basata su ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) per i modelli creati con la finestra di progettazione di EF.</span><span class="sxs-lookup"><span data-stu-id="2b107-120">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="2b107-121">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="2b107-121">Pre-Requisites</span></span>

<span data-ttu-id="2b107-122">Per completare questa procedura dettagliata, è necessario che Visual Studio 2013, Visual Studio 2012 o Visual Studio 2010 sia installato.</span><span class="sxs-lookup"><span data-stu-id="2b107-122">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="2b107-123">Se si utilizza Visual Studio 2010, è necessario installare anche NuGet.</span><span class="sxs-lookup"><span data-stu-id="2b107-123">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="2b107-124">Per ulteriori informazioni, vedere [Installazione di NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span><span class="sxs-lookup"><span data-stu-id="2b107-124">For more information, see [Installing NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="2b107-125">Creare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="2b107-125">Create the Application</span></span>

-   <span data-ttu-id="2b107-126">Aprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2b107-126">Open Visual Studio</span></span>
-   <span data-ttu-id="2b107-127">**File&gt; -&gt; Nuovo - Progetto....**</span><span class="sxs-lookup"><span data-stu-id="2b107-127">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="2b107-128">Selezionare **Windows** nel riquadro sinistro e **WPFApplication** nel riquadro destro</span><span class="sxs-lookup"><span data-stu-id="2b107-128">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="2b107-129">Immettere **WPFwithEFSample** come nome</span><span class="sxs-lookup"><span data-stu-id="2b107-129">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="2b107-130">Selezionare **OK**</span><span class="sxs-lookup"><span data-stu-id="2b107-130">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="2b107-131">Installare il pacchetto NuGet di Entity FrameworkInstall the Entity Framework NuGet package</span><span class="sxs-lookup"><span data-stu-id="2b107-131">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="2b107-132">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **WinFormswithEFSample**</span><span class="sxs-lookup"><span data-stu-id="2b107-132">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="2b107-133">Selezionare **Gestisci pacchetti NuGet...**</span><span class="sxs-lookup"><span data-stu-id="2b107-133">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="2b107-134">Nella finestra di dialogo Gestisci pacchetti NuGet, selezionare la scheda **Online** e scegliere il pacchetto **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="2b107-134">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="2b107-135">Fare clic su **Installa**</span><span class="sxs-lookup"><span data-stu-id="2b107-135">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="2b107-136">Oltre all'assembly EntityFramework, viene aggiunto anche un riferimento a System.ComponentModel.DataAnnotations.</span><span class="sxs-lookup"><span data-stu-id="2b107-136">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="2b107-137">Se il progetto contiene un riferimento a System.Data.Entity, verrà rimosso quando viene installato il pacchetto EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="2b107-137">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="2b107-138">L'assembly System.Data.Entity non viene più utilizzato per le applicazioni Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="2b107-138">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="2b107-139">Definizione di un modello</span><span class="sxs-lookup"><span data-stu-id="2b107-139">Define a Model</span></span>

<span data-ttu-id="2b107-140">In questa procedura dettagliata è possibile scegliere di implementare un modello utilizzando Code First o la finestra di progettazione di EF.</span><span class="sxs-lookup"><span data-stu-id="2b107-140">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="2b107-141">Completare una delle due sezioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="2b107-141">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="2b107-142">Opzione 1: Definire un modello utilizzando Code FirstOption 1: Define a Model using Code First</span><span class="sxs-lookup"><span data-stu-id="2b107-142">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="2b107-143">In questa sezione viene illustrato come creare un modello e il database associato utilizzando Code First.</span><span class="sxs-lookup"><span data-stu-id="2b107-143">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="2b107-144">Passare alla sezione successiva **(Opzione 2: Definire un modello utilizzando Prima Database)** se si preferisce utilizzare Database First per decodificare il modello da un database usando la finestra di progettazione di EF</span><span class="sxs-lookup"><span data-stu-id="2b107-144">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="2b107-145">Quando si usa lo sviluppo Code First, in genere si inizia scrivendo classi .NET Framework che definiscono il modello concettuale (dominio).</span><span class="sxs-lookup"><span data-stu-id="2b107-145">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="2b107-146">Aggiungere una nuova classe a **WPFwithEFSample:**</span><span class="sxs-lookup"><span data-stu-id="2b107-146">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="2b107-147">Fare clic con il pulsante destro del mouse sul nome del progetto</span><span class="sxs-lookup"><span data-stu-id="2b107-147">Right-click on the project name</span></span>
    -   <span data-ttu-id="2b107-148">Selezionare **Aggiungi**, quindi **Nuovo elemento**</span><span class="sxs-lookup"><span data-stu-id="2b107-148">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="2b107-149">Selezionare **Classe** e immettere **Prodotto** come nome della classe</span><span class="sxs-lookup"><span data-stu-id="2b107-149">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="2b107-150">Sostituire la definizione della classe **Product** con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="2b107-150">Replace the **Product** class definition with the following code:</span></span>

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

<span data-ttu-id="2b107-151">Il **Products** proprietà il **Category** classe e **Category** proprietà il **Product** classe sono proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="2b107-151">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="2b107-152">In Entity Framework, le proprietà di navigazione consentono di esplorare una relazione tra due tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="2b107-152">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="2b107-153">Oltre a definire le entità, è necessario definire una classe che deriva&lt;da&gt; DbContext ed espone le proprietà DbSet TEntity.</span><span class="sxs-lookup"><span data-stu-id="2b107-153">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="2b107-154">Le proprietà&lt;DbSet TEntity&gt; informa il contesto dei tipi che si desidera includere nel modello.</span><span class="sxs-lookup"><span data-stu-id="2b107-154">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="2b107-155">Un'istanza del tipo derivato DbContext gestisce gli oggetti entità in fase di esecuzione, che include il popolamento di oggetti con i dati di un database, il rilevamento delle modifiche e la persistenza dei dati nel database.</span><span class="sxs-lookup"><span data-stu-id="2b107-155">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="2b107-156">Aggiungere una nuova classe ProductContext al progetto con la definizione seguente:Add a new **ProductContext** class to the project with the following definition:</span><span class="sxs-lookup"><span data-stu-id="2b107-156">Add a new **ProductContext** class to the project with the following definition:</span></span>

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

<span data-ttu-id="2b107-157">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="2b107-157">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="2b107-158">Opzione 2: Definire un modello utilizzando Database FirstOption 2: Define a model using Database First</span><span class="sxs-lookup"><span data-stu-id="2b107-158">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="2b107-159">In questa sezione viene illustrato come utilizzare Database First per decodificare il modello da un database utilizzando la finestra di progettazione di EF.</span><span class="sxs-lookup"><span data-stu-id="2b107-159">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="2b107-160">Se è stata completata la sezione precedente (**Opzione 1: Definire un modello utilizzando Code First),** ignorare questa sezione e passare direttamente alla sezione **Caricamento lazy.**</span><span class="sxs-lookup"><span data-stu-id="2b107-160">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="2b107-161">Creare un database esistenteCreate an Existing Database</span><span class="sxs-lookup"><span data-stu-id="2b107-161">Create an Existing Database</span></span>

<span data-ttu-id="2b107-162">In genere quando si utilizza la destinazione di un database esistente, questo verrà già creato, ma per questa procedura dettagliata è necessario creare un database a cui accedere.</span><span class="sxs-lookup"><span data-stu-id="2b107-162">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="2b107-163">Il server di database installato con Visual Studio è diverso a seconda della versione di Visual Studio installata:</span><span class="sxs-lookup"><span data-stu-id="2b107-163">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="2b107-164">Se si utilizza Visual Studio 2010 verrà creato un database SQL Express.</span><span class="sxs-lookup"><span data-stu-id="2b107-164">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="2b107-165">Se si utilizza Visual Studio 2012 quindi verrà creato un database [LocalDB.](https://msdn.microsoft.com/library/hh510202.aspx)</span><span class="sxs-lookup"><span data-stu-id="2b107-165">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="2b107-166">Andiamo avanti e generare il database.</span><span class="sxs-lookup"><span data-stu-id="2b107-166">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="2b107-167">**Visualizza&gt; - Esplora server**</span><span class="sxs-lookup"><span data-stu-id="2b107-167">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="2b107-168">Fare clic con il pulsante destro del mouse su **Connessioni dati -&gt; Aggiungi connessione...**</span><span class="sxs-lookup"><span data-stu-id="2b107-168">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="2b107-169">Se non si è connessi a un database da Esplora server prima di dover selezionare Microsoft SQL Server come origine dati</span><span class="sxs-lookup"><span data-stu-id="2b107-169">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Modifica origine dati](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="2b107-171">Connettersi a LocalDB o SQL Express, a seconda di quale è stato installato, e immettere **Products** come nome del database</span><span class="sxs-lookup"><span data-stu-id="2b107-171">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Aggiungi localDB connessione](~/ef6/media/addconnectionlocaldb.png)

    ![Aggiungi Connessione Rapida](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="2b107-174">Selezionare **OK** e verrà chiesto se si desidera creare un nuovo database, selezionare **Sì**</span><span class="sxs-lookup"><span data-stu-id="2b107-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Create Database](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="2b107-176">Il nuovo database verrà visualizzato in Esplora server, fare clic con il pulsante destro del mouse su di esso e selezionare **Nuova query**</span><span class="sxs-lookup"><span data-stu-id="2b107-176">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="2b107-177">Copiare il codice SQL seguente nella nuova query, quindi fare clic con il pulsante destro del mouse sulla query e selezionare **Esegui**</span><span class="sxs-lookup"><span data-stu-id="2b107-177">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="2b107-178">Modello Ingegnere inverso</span><span class="sxs-lookup"><span data-stu-id="2b107-178">Reverse Engineer Model</span></span>

<span data-ttu-id="2b107-179">Utilizzeremo Entity Framework Designer, incluso come parte di Visual Studio, per creare il modello.</span><span class="sxs-lookup"><span data-stu-id="2b107-179">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="2b107-180">**Progetto&gt; - Aggiungi nuovo elemento...**</span><span class="sxs-lookup"><span data-stu-id="2b107-180">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="2b107-181">Selezionare **Dati** dal menu a sinistra e quindi **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="2b107-181">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="2b107-182">Immettere **ProductModel** come nome e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="2b107-182">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="2b107-183">Verrà avviata la **procedura guidata Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="2b107-183">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="2b107-184">Selezionare **Genera da database** e fare clic su **Avanti**</span><span class="sxs-lookup"><span data-stu-id="2b107-184">Select **Generate from Database** and click **Next**</span></span>

    ![Scelta dei contenuti del modello](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="2b107-186">Selezionare la connessione al database creato nella prima sezione, immettere **ProductContext** come nome della stringa di connessione e fare clic su **Avanti**</span><span class="sxs-lookup"><span data-stu-id="2b107-186">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Scegli la tua connessione](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="2b107-188">Fare clic sulla casella di controllo accanto a 'Tabelle' per importare tutte le tabelle e fare clic su 'Fine'</span><span class="sxs-lookup"><span data-stu-id="2b107-188">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Scegli i tuoi oggetti](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="2b107-190">Una volta completato il processo di decodifica, il nuovo modello viene aggiunto al progetto e aperto per la visualizzazione in Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="2b107-190">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="2b107-191">Un file App.config è stato aggiunto al progetto con i dettagli di connessione per il database.</span><span class="sxs-lookup"><span data-stu-id="2b107-191">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="2b107-192">Passaggi aggiuntivi in Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="2b107-192">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="2b107-193">Se si lavora in Visual Studio 2010 quindi sarà necessario aggiornare la finestra di progettazione di EF per utilizzare la generazione di codice EF6.</span><span class="sxs-lookup"><span data-stu-id="2b107-193">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="2b107-194">Fare clic con il pulsante destro del mouse su un punto vuoto del modello nella finestra di progettazione di EF e selezionare **Aggiungi elemento di generazione codice...**</span><span class="sxs-lookup"><span data-stu-id="2b107-194">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="2b107-195">Selezionare **Modelli online** dal menu a sinistra e cercare **DbContext**</span><span class="sxs-lookup"><span data-stu-id="2b107-195">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="2b107-196">Selezionare il **generatore DbContext di\#EF 6.x per C ,** immettere **ProductsModel** come nome e fare clic su Aggiungi</span><span class="sxs-lookup"><span data-stu-id="2b107-196">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="2b107-197">Aggiornamento della generazione di codice per l'associazione datiUpdating code generation for data binding</span><span class="sxs-lookup"><span data-stu-id="2b107-197">Updating code generation for data binding</span></span>

<span data-ttu-id="2b107-198">EF genera codice dal modello utilizzando i modelli T4.</span><span class="sxs-lookup"><span data-stu-id="2b107-198">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="2b107-199">I modelli forniti con Visual Studio o scaricati dalla raccolta di Visual Studio sono destinati a scopi generali.</span><span class="sxs-lookup"><span data-stu-id="2b107-199">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="2b107-200">Ciò significa che le entità generate da&lt;&gt; questi modelli dispongono di proprietà ICollection T semplici.</span><span class="sxs-lookup"><span data-stu-id="2b107-200">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="2b107-201">Tuttavia, quando si esegue l'associazione dati utilizzando WPFWPF è consigliabile usare **ObservableCollection** per le proprietà della raccolta in modo che WPFWPF possa tenere traccia delle modifiche apportate alle raccolte.</span><span class="sxs-lookup"><span data-stu-id="2b107-201">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="2b107-202">A tal fine si modificheranno i modelli per utilizzare ObservableCollection.</span><span class="sxs-lookup"><span data-stu-id="2b107-202">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="2b107-203">Aprire **Esplora soluzioni** e trovare il file **ProductModel.edmx**</span><span class="sxs-lookup"><span data-stu-id="2b107-203">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="2b107-204">Trovare il file **di ProductModel.tt** che verrà nidificato nel file ProductModel.edmx</span><span class="sxs-lookup"><span data-stu-id="2b107-204">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![Modello di modello di prodotto WPFWPF Product Model Template](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="2b107-206">Fare doppio clic sul file ProductModel.tt per aprirlo nell'editor di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2b107-206">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="2b107-207">Trovare e sostituire le due occorrenze di "**ICollection**" con "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="2b107-207">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="2b107-208">Questi si trovano approssimativamente alle linee 296 e 484.</span><span class="sxs-lookup"><span data-stu-id="2b107-208">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="2b107-209">Trovare e sostituire la prima occorrenza di "**HashSet**" con "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="2b107-209">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="2b107-210">Questa occorrenza si trova approssimativamente alla riga 50.</span><span class="sxs-lookup"><span data-stu-id="2b107-210">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="2b107-211">**Non** sostituire la seconda occorrenza di HashSet presente più avanti nel codice.</span><span class="sxs-lookup"><span data-stu-id="2b107-211">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="2b107-212">Trovare e sostituire l'unica occorrenza di "**System.Collections.Generic**" con "**System.Collections.ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="2b107-212">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="2b107-213">Questo si trova approssimativamente alla linea 424.</span><span class="sxs-lookup"><span data-stu-id="2b107-213">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="2b107-214">Salvare il file di ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="2b107-214">Save the ProductModel.tt file.</span></span> <span data-ttu-id="2b107-215">In questo modo il codice per le entità da rigenerare.</span><span class="sxs-lookup"><span data-stu-id="2b107-215">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="2b107-216">Se il codice non viene rigenerato automaticamente, fare clic con il pulsante destro del mouse su ProductModel.tt e scegliere "Esegui strumento personalizzato".</span><span class="sxs-lookup"><span data-stu-id="2b107-216">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="2b107-217">Se ora si apre il file di Category.cs (annidato in ProductModel.tt), si note che la raccolta Products ha il tipo **ObservableCollection&lt;Product&gt;**.</span><span class="sxs-lookup"><span data-stu-id="2b107-217">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="2b107-218">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="2b107-218">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="2b107-219">Caricamento lazy</span><span class="sxs-lookup"><span data-stu-id="2b107-219">Lazy Loading</span></span>

<span data-ttu-id="2b107-220">Il **Products** proprietà il **Category** classe e **Category** proprietà il **Product** classe sono proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="2b107-220">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="2b107-221">In Entity Framework, le proprietà di navigazione consentono di esplorare una relazione tra due tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="2b107-221">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="2b107-222">EF offre un'opzione di caricamento delle entità correlate dal database automaticamente la prima volta che si accede alla proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="2b107-222">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="2b107-223">Con questo tipo di caricamento (denominato caricamento lazy), tenere presente che la prima volta che si accede a ogni proprietà di navigazione verrà eseguita una query separata sul database se il contenuto non è già presente nel contesto.</span><span class="sxs-lookup"><span data-stu-id="2b107-223">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="2b107-224">Quando si usano i tipi di entità POCO, Entity Framework raggiunge il caricamento lazy creando istanze di tipi proxy derivati durante il runtime e quindi eseguire l'override delle proprietà virtuali nelle classi per aggiungere l'hook di caricamento.</span><span class="sxs-lookup"><span data-stu-id="2b107-224">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="2b107-225">Per ottenere il caricamento lazy di oggetti correlati, è necessario dichiarare i getter di proprietà di navigazione come **public** e **virtual** **(Overridable** in Visual Basic) e la classe non deve essere **sealed** (**NotOverridable** in Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="2b107-225">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="2b107-226">Quando si usano le proprietà di navigazione Di primo Database vengono automaticamente rese virtuali per abilitare il caricamento lazy.</span><span class="sxs-lookup"><span data-stu-id="2b107-226">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="2b107-227">Nella sezione Code First abbiamo scelto di rendere virtuali le proprietà di navigazione per lo stesso motivo</span><span class="sxs-lookup"><span data-stu-id="2b107-227">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="2b107-228">Associa oggetto ai controlli</span><span class="sxs-lookup"><span data-stu-id="2b107-228">Bind Object to Controls</span></span>

<span data-ttu-id="2b107-229">Aggiungere le classi definite nel modello come origini dati per questa applicazione WPFWPF.</span><span class="sxs-lookup"><span data-stu-id="2b107-229">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="2b107-230">Fare doppio clic su **MainWindow.xaml** in Esplora soluzioni per aprire il form principale</span><span class="sxs-lookup"><span data-stu-id="2b107-230">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="2b107-231">Dal menu principale, selezionare **Progetto -&gt; Aggiungi nuova origine dati ...**</span><span class="sxs-lookup"><span data-stu-id="2b107-231">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="2b107-232">(in Visual Studio 2010, è necessario selezionare **dati -&gt; Aggiungi nuova origine dati...**)</span><span class="sxs-lookup"><span data-stu-id="2b107-232">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="2b107-233">Nella finestra Scegliere un tipo di origine dati, selezionare **Oggetto** e fare clic su **Avanti**</span><span class="sxs-lookup"><span data-stu-id="2b107-233">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="2b107-234">Nella finestra di dialogo Seleziona oggetti dati, aprire il **WPFwithEFSample** due volte e selezionare **Categoria**</span><span class="sxs-lookup"><span data-stu-id="2b107-234">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="2b107-235">*Non è necessario selezionare l'origine dati **Product,** perché verrà eseguito il percorso tramite la proprietà **del prodotto**nell'origine dati **Category***</span><span class="sxs-lookup"><span data-stu-id="2b107-235">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Selezione di oggetti dati](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="2b107-237">Fare clic su **Fine**.</span><span class="sxs-lookup"><span data-stu-id="2b107-237">Click **Finish.**</span></span>
-   <span data-ttu-id="2b107-238">La finestra Origini dati viene aperta accanto alla finestra MainWindow.xaml \*Se la finestra Origini dati non viene visualizzata, selezionare Visualizza - **&gt; Altre origini&gt; dati Windows** \*</span><span class="sxs-lookup"><span data-stu-id="2b107-238">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="2b107-239">Premere l'icona a forma di puntina, in modo che la finestra Origini dati non si nasconda automaticamente.</span><span class="sxs-lookup"><span data-stu-id="2b107-239">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="2b107-240">Potrebbe essere necessario premere il pulsante di aggiornamento se la finestra era già visibile.</span><span class="sxs-lookup"><span data-stu-id="2b107-240">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Origini dati](~/ef6/media/datasources.png)

-   <span data-ttu-id="2b107-242">Selezionare l'origine dati **Categoria** e trascinarla nel modulo.</span><span class="sxs-lookup"><span data-stu-id="2b107-242">Select the **Category** data source and drag it on the form.</span></span>

<span data-ttu-id="2b107-243">Quando abbiamo trascinato questa fonte, è successo quanto segue:</span><span class="sxs-lookup"><span data-stu-id="2b107-243">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="2b107-244">La risorsa **categoryViewSource** e il controllo **categoryDataGrid** sono stati aggiunti a XAML</span><span class="sxs-lookup"><span data-stu-id="2b107-244">The **categoryViewSource** resource and the **categoryDataGrid** control were added to XAML</span></span> 
-   <span data-ttu-id="2b107-245">La proprietà DataContext nell'elemento Grid padre è stata impostata su "'StaticResource **categoryViewSource'".**</span><span class="sxs-lookup"><span data-stu-id="2b107-245">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span><span data-ttu-id="2b107-246">La risorsa **categoryViewSource** funge da origine\\di associazione per l'elemento Grid padre esterno.</span><span class="sxs-lookup"><span data-stu-id="2b107-246"> The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="2b107-247">Gli elementi Grid interni ereditano quindi il valore DataContext dall'elemento padre Grid (la proprietà ItemsSource di categoryDataGrid è impostata su ""Binding")</span><span class="sxs-lookup"><span data-stu-id="2b107-247">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}")</span></span>

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

## <a name="adding-a-details-grid"></a><span data-ttu-id="2b107-248">Aggiunta di una griglia dettagli</span><span class="sxs-lookup"><span data-stu-id="2b107-248">Adding a Details Grid</span></span>

<span data-ttu-id="2b107-249">Ora che abbiamo una griglia per visualizzare categorie possiamo aggiungere una griglia di dettagli per visualizzare i prodotti associati.</span><span class="sxs-lookup"><span data-stu-id="2b107-249">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="2b107-250">Selezionare la proprietà **Products** nell'origine dati **Categoria** e trascinarla nel form.</span><span class="sxs-lookup"><span data-stu-id="2b107-250">Select the **Products** property from under the **Category** data source and drag it on the form.</span></span>
    -   <span data-ttu-id="2b107-251">La risorsa **categoryProductsViewSource** e la griglia **productDataGrid** vengono aggiunte a XAML</span><span class="sxs-lookup"><span data-stu-id="2b107-251">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="2b107-252">Il percorso di associazione per questa risorsa è impostato su Products</span><span class="sxs-lookup"><span data-stu-id="2b107-252">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="2b107-253">Il framework di associazione dati WPF garantisce che in productDataGrid vengano visualizzati solo i prodotti correlati alla categoria selezionataWPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span><span class="sxs-lookup"><span data-stu-id="2b107-253">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="2b107-254">Dalla casella degli strumenti trascinare **Button** nel form.</span><span class="sxs-lookup"><span data-stu-id="2b107-254">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="2b107-255">Impostare la proprietà **Name** su **buttonSave** e la proprietà **Content su** **Save**.</span><span class="sxs-lookup"><span data-stu-id="2b107-255">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="2b107-256">Il modulo dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="2b107-256">The form should look similar to this:</span></span>

![Finestra di progettazione](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="2b107-258">Aggiungere il codice che gestisce l'interazione dei datiAdd Code that Handles Data Interaction</span><span class="sxs-lookup"><span data-stu-id="2b107-258">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="2b107-259">È il momento di aggiungere alcuni gestori eventi alla finestra principale.</span><span class="sxs-lookup"><span data-stu-id="2b107-259">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="2b107-260">Nella finestra XAML, fare clic sull'elemento \*\* &lt;Window,\*\* selezionare la finestra principale</span><span class="sxs-lookup"><span data-stu-id="2b107-260">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="2b107-261">Nella finestra **Proprietà** scegliere **Eventi** in alto a destra, quindi fare doppio clic sulla casella di testo a destra dell'etichetta **Caricato**</span><span class="sxs-lookup"><span data-stu-id="2b107-261">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Proprietà della finestra principale](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="2b107-263">Aggiungere anche l'evento **Click** per il pulsante **Salva** facendo doppio clic sul pulsante Salva nella finestra di progettazione.</span><span class="sxs-lookup"><span data-stu-id="2b107-263">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="2b107-264">In questo modo si accede al code-behind per il form, verrà ora modificato il codice per utilizzare il ProductContext per eseguire l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="2b107-264">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="2b107-265">Aggiornare il codice per MainWindow come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="2b107-265">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="2b107-266">Il codice dichiara un'istanza a esecuzione prolungata di **ProductContext**.</span><span class="sxs-lookup"><span data-stu-id="2b107-266">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="2b107-267">**L'oggetto ProductContext** viene utilizzato per eseguire query e salvare i dati nel database.</span><span class="sxs-lookup"><span data-stu-id="2b107-267">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="2b107-268">**Dispose()** nell'istanza **ProductContext** viene quindi chiamato dal metodo **OnClosing** sottoposto a override.</span><span class="sxs-lookup"><span data-stu-id="2b107-268">The **Dispose()** on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span><span data-ttu-id="2b107-269">I commenti del codice forniscono dettagli sulle operazioni eseguite dal codice.</span><span class="sxs-lookup"><span data-stu-id="2b107-269"> The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-wpf-application"></a><span data-ttu-id="2b107-270">Testare l'applicazione WPFTest the WPF Application</span><span class="sxs-lookup"><span data-stu-id="2b107-270">Test the WPF Application</span></span>

-   <span data-ttu-id="2b107-271">Compilare l'applicazione ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="2b107-271">Compile and run the application.</span></span> <span data-ttu-id="2b107-272">Se è stato utilizzato Code First, si noterà che viene creato automaticamente un database **WPFwithEFSample.ProductContext.**</span><span class="sxs-lookup"><span data-stu-id="2b107-272">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="2b107-273">Immettere un nome di categoria nella griglia superiore e i nomi dei prodotti nella griglia inferiore *Non immettere nulla nelle colonne ID, perché la chiave primaria viene generata dal database*</span><span class="sxs-lookup"><span data-stu-id="2b107-273">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Finestra principale con nuove categorie e prodotti](~/ef6/media/screen1.png)

-   <span data-ttu-id="2b107-275">Premere il pulsante **Salva** per salvare i dati nel database</span><span class="sxs-lookup"><span data-stu-id="2b107-275">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="2b107-276">Dopo la chiamata a **SaveChanges()** di DbContext, gli ID vengono popolati con i valori generati dal database.</span><span class="sxs-lookup"><span data-stu-id="2b107-276">After the call to DbContext’s **SaveChanges()**, the IDs are populated with the database generated values.</span></span> <span data-ttu-id="2b107-277">Poiché abbiamo chiamato **Refresh()** dopo **SaveChanges()** i controlli **DataGrid** vengono aggiornati anche con i nuovi valori.</span><span class="sxs-lookup"><span data-stu-id="2b107-277">Because we called **Refresh()** after **SaveChanges()** the **DataGrid** controls are updated with the new values as well.</span></span>

![Finestra principale con gli URL popolati](~/ef6/media/screen2.png)

## <a name="additional-resources"></a><span data-ttu-id="2b107-279">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="2b107-279">Additional Resources</span></span>

<span data-ttu-id="2b107-280">Per altre informazioni sull'associazione dati alle raccolte tramite WPFWPF, vedere [questo argomento](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) nella documentazione di WPFWPF.</span><span class="sxs-lookup"><span data-stu-id="2b107-280">To learn more about data binding to collections using WPF, see [this topic](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) in the WPF documentation.</span></span>  
