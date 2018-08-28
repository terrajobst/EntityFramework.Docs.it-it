---
title: Procedura dettagliata entità - Entity Framework 6 con rilevamento automatico
author: divega
ms.date: 2016-10-23
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: 64ca9ae42df1a1c740131e254b8f80f67b2f9f97
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995421"
---
# <a name="self-tracking-entities-walkthrough"></a><span data-ttu-id="ad9dd-102">Procedura dettagliata rilevamento automatico di entità</span><span class="sxs-lookup"><span data-stu-id="ad9dd-102">Self-Tracking Entities Walkthrough</span></span>
> [!IMPORTANT]
> <span data-ttu-id="ad9dd-103">L'uso del modello di entità con rilevamento automatico non è più consigliabile.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-103">We no longer recommend using the self-tracking-entities template.</span></span> <span data-ttu-id="ad9dd-104">Continuerà a essere disponibile solo per supportare le applicazioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-104">It will only continue to be available to support existing applications.</span></span> <span data-ttu-id="ad9dd-105">Se l'applicazione richiede l'uso con grafici di entità disconnesse, prendere in considerazione altre alternative, come ad esempio [Trackable Entities](http://trackableentities.github.io/), che è una tecnologia simile alle entità con rilevamento automatico ma viene sviluppata in modo più attivo dalla community, oppure la scrittura di codice personalizzato usando le API di rilevamento delle modifiche di basso livello.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-105">If your application requires working with disconnected graphs of entities, consider other alternatives such as [Trackable Entities](http://trackableentities.github.io/), which is a technology similar to Self-Tracking-Entities that is more actively developed by the community, or writing custom code using the low-level change tracking APIs.</span></span>

<span data-ttu-id="ad9dd-106">Questa procedura dettagliata illustra lo scenario in cui un servizio Windows Communication Foundation (WCF) espone un'operazione che restituisce un grafico di entità.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-106">This walkthrough demonstrates the scenario in which a Windows Communication Foundation (WCF) service exposes an operation that returns an entity graph.</span></span> <span data-ttu-id="ad9dd-107">Successivamente, un'applicazione client modifica il grafico e inoltra le modifiche a un'operazione del servizio che convalida e Salva gli aggiornamenti a un database tramite Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-107">Next, a client application manipulates that graph and submits the modifications to a service operation that validates and saves the updates to a database using Entity Framework.</span></span>

<span data-ttu-id="ad9dd-108">Prima di completare questa procedura dettagliata assicurarsi di leggere il [entità con rilevamento automatico](index.md) pagina.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-108">Before completing this walkthrough make sure you read the [Self-Tracking Entities](index.md) page.</span></span>

<span data-ttu-id="ad9dd-109">In questa procedura dettagliata verranno completate le seguenti azioni:</span><span class="sxs-lookup"><span data-stu-id="ad9dd-109">This walkthrough completes the following actions:</span></span>

-   <span data-ttu-id="ad9dd-110">Crea un database a cui accedere.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-110">Creates a database to access.</span></span>
-   <span data-ttu-id="ad9dd-111">Crea una libreria di classi che contiene il modello.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-111">Creates a class library that contains the model.</span></span>
-   <span data-ttu-id="ad9dd-112">Scambi al modello generatore di entità con rilevamento automatico.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-112">Swaps to the Self-Tracking Entity Generator template.</span></span>
-   <span data-ttu-id="ad9dd-113">Sposta le classi di entità in un progetto separato.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-113">Moves the entity classes to a separate project.</span></span>
-   <span data-ttu-id="ad9dd-114">Crea un servizio WCF che espone operazioni per eseguire query e salvare le entità.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-114">Creates a WCF service that exposes operations to query and save entities.</span></span>
-   <span data-ttu-id="ad9dd-115">Crea applicazioni (Console e WPF) che utilizzano il servizio client.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-115">Creates client applications (Console and WPF) that consume the service.</span></span>

<span data-ttu-id="ad9dd-116">Useremo Database First in questa procedura dettagliata, ma le stesse tecniche sono ugualmente applicabili a Model First.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-116">We'll use Database First in this walkthrough but the same techniques apply equally to Model First.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="ad9dd-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="ad9dd-117">Pre-Requisites</span></span>

<span data-ttu-id="ad9dd-118">Per completare questa procedura dettagliata è necessario utilizzare una versione recente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-118">To complete this walkthrough you will need a recent version of Visual Studio.</span></span>

## <a name="create-a-database"></a><span data-ttu-id="ad9dd-119">Creare un Database</span><span class="sxs-lookup"><span data-stu-id="ad9dd-119">Create a Database</span></span>

<span data-ttu-id="ad9dd-120">Il server di database che viene installato con Visual Studio è diverso a seconda della versione di Visual Studio installata:</span><span class="sxs-lookup"><span data-stu-id="ad9dd-120">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="ad9dd-121">Se si usa Visual Studio 2012, quindi si creerà un database LocalDB.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-121">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="ad9dd-122">Se si usa Visual Studio 2010 si creerà un database SQL Express.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-122">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="ad9dd-123">È possibile procedere e generare il database.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-123">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="ad9dd-124">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ad9dd-124">Open Visual Studio</span></span>
-   <span data-ttu-id="ad9dd-125">**Visualizzazione -&gt; Esplora Server**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-125">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="ad9dd-126">Fare clic con il pulsante destro sul **connessioni dati -&gt; Aggiungi connessione...**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-126">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="ad9dd-127">Se si è ancora connessi a un database da Esplora Server prima che è necessario selezionare **Microsoft SQL Server** come origine dati</span><span class="sxs-lookup"><span data-stu-id="ad9dd-127">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="ad9dd-128">Connettersi a LocalDB o SQL Express, in base alla quale è stato installato</span><span class="sxs-lookup"><span data-stu-id="ad9dd-128">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="ad9dd-129">Immettere **STESample** come nome del database</span><span class="sxs-lookup"><span data-stu-id="ad9dd-129">Enter **STESample** as the database name</span></span>
-   <span data-ttu-id="ad9dd-130">Selezionare **OK** e verrà richiesto se si desidera creare un nuovo database, selezionare **Sì**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-130">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="ad9dd-131">Il nuovo database verrà ora visualizzato in Esplora Server</span><span class="sxs-lookup"><span data-stu-id="ad9dd-131">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="ad9dd-132">Se si usa Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="ad9dd-132">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="ad9dd-133">Fare doppio clic sul database in Esplora Server e selezionare **nuova Query**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-133">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="ad9dd-134">Copiare il codice SQL seguente nella nuova query, quindi fare clic su query e selezionare **Execute**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="ad9dd-135">Se si usa Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="ad9dd-135">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="ad9dd-136">Selezionare **- Data&gt; Transact SQL Editor -&gt; nuova connessione Query...**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-136">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="ad9dd-137">Immettere **.\\ SQLEXPRESS** come nome del server e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-137">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="ad9dd-138">Selezionare il **STESample** database dall'elenco a discesa nella parte superiore dell'editor di query</span><span class="sxs-lookup"><span data-stu-id="ad9dd-138">Select the **STESample** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="ad9dd-139">Copiare il codice SQL seguente nella nuova query, quindi fare clic su query e selezionare **Esegui SQL**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-139">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

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

    SET IDENTITY_INSERT [dbo].[Blogs] ON
    INSERT INTO [dbo].[Blogs] ([BlogId], [Name], [Url]) VALUES (1, N'ADO.NET Blog', N'blogs.msdn.com/adonet')
    SET IDENTITY_INSERT [dbo].[Blogs] OFF
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'Intro to EF', N'Interesting stuff...', 1)
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'What is New', N'More interesting stuff...', 1)
```

## <a name="create-the-model"></a><span data-ttu-id="ad9dd-140">Creare il modello</span><span class="sxs-lookup"><span data-stu-id="ad9dd-140">Create the Model</span></span>

<span data-ttu-id="ad9dd-141">Innanzitutto, è necessario inserire il modello in un progetto.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-141">First up, we need a project to put the model in.</span></span>

-   <span data-ttu-id="ad9dd-142">**File -&gt; New -&gt; progetto...**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-142">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="ad9dd-143">Selezionare **Visual C#\#**  nel riquadro a sinistra e quindi **libreria di classi**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-143">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="ad9dd-144">Immettere **STESample** come nome, quindi fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-144">Enter **STESample** as the name and click **OK**</span></span>

<span data-ttu-id="ad9dd-145">A questo punto si creerà un modello semplice nella finestra di progettazione di Entity Framework per accedere ai database:</span><span class="sxs-lookup"><span data-stu-id="ad9dd-145">Now we'll create a simple model in the EF Designer to access our database:</span></span>

-   <span data-ttu-id="ad9dd-146">**Progetto -&gt; Aggiungi nuovo elemento...**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-146">**Project -&gt; Add New Item...**</span></span>
-   <span data-ttu-id="ad9dd-147">Selezionare **Data** nel riquadro a sinistra e quindi **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-147">Select **Data** from the left pane and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="ad9dd-148">Immettere **BloggingModel** come nome, quindi fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-148">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="ad9dd-149">Selezionare **genera da database** e fare clic su **successivo**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-149">Select **Generate from database** and click **Next**</span></span>
-   <span data-ttu-id="ad9dd-150">Immettere le informazioni di connessione per il database creato nella sezione precedente</span><span class="sxs-lookup"><span data-stu-id="ad9dd-150">Enter the connection information for the database that you created in the previous section</span></span>
-   <span data-ttu-id="ad9dd-151">Immettere **BloggingContext** come nome per la stringa di connessione e fare clic su **successivo**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-151">Enter **BloggingContext** as the name for the connection string and click **Next**</span></span>
-   <span data-ttu-id="ad9dd-152">Selezionare la casella accanto a **tabelle** e fare clic su **fine**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-152">Check the box next to **Tables** and click **Finish**</span></span>

## <a name="swap-to-ste-code-generation"></a><span data-ttu-id="ad9dd-153">Scambio per la generazione di codice STE</span><span class="sxs-lookup"><span data-stu-id="ad9dd-153">Swap to STE Code Generation</span></span>

<span data-ttu-id="ad9dd-154">A questo punto è necessario disabilitare la generazione di codice predefinita e lo scambio di entità con rilevamento automatico.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-154">Now we need to disable the default code generation and swap to Self-Tracking Entities.</span></span>

### <a name="if-you-are-using-visual-studio-2012"></a><span data-ttu-id="ad9dd-155">Se si usa Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="ad9dd-155">If you are using Visual Studio 2012</span></span>

-   <span data-ttu-id="ad9dd-156">Espandere **BloggingModel.edmx** nelle **Esplora soluzioni** ed eliminare i **BloggingModel.tt** e **BloggingModel.Context.tt** 
     *Verrà disabilitata la generazione di codice predefinita*</span><span class="sxs-lookup"><span data-stu-id="ad9dd-156">Expand **BloggingModel.edmx** in **Solution Explorer** and delete the **BloggingModel.tt** and **BloggingModel.Context.tt**
*This will disable the default code generation*</span></span>
-   <span data-ttu-id="ad9dd-157">Fare doppio clic su un'area vuota di Entity Framework Designer e selezionare **Aggiungi elemento di generazione di codice...**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-157">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="ad9dd-158">Selezionare **Online** dal riquadro a sinistra e cercare **STE Generator**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-158">Select **Online** from the left pane and search for **STE Generator**</span></span>
-   <span data-ttu-id="ad9dd-159">Selezionare il **generatore di Incolla per C\#**  modello, immettere **STETemplate** come nome, quindi fare clic su **Add**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-159">Select the **STE Generator for C\#** template, enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="ad9dd-160">Il **STETemplate.tt** e **STETemplate.Context.tt** vengono aggiunti file annidati sotto il file BloggingModel.edmx</span><span class="sxs-lookup"><span data-stu-id="ad9dd-160">The **STETemplate.tt** and **STETemplate.Context.tt** files are added nested under the BloggingModel.edmx file</span></span>

### <a name="if-you-are-using-visual-studio-2010"></a><span data-ttu-id="ad9dd-161">Se si usa Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="ad9dd-161">If you are using Visual Studio 2010</span></span>

-   <span data-ttu-id="ad9dd-162">Fare doppio clic su un'area vuota di Entity Framework Designer e selezionare **Aggiungi elemento di generazione di codice...**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-162">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="ad9dd-163">Selezionare **codice** nel riquadro a sinistra e quindi **ADO.NET self-Tracking Entity Generator**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-163">Select **Code** from the left pane and then **ADO.NET Self-Tracking Entity Generator**</span></span>
-   <span data-ttu-id="ad9dd-164">Immettere **STETemplate** come nome, quindi fare clic su **Add**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-164">Enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="ad9dd-165">Il **STETemplate.tt** e **STETemplate.Context.tt** file vengono aggiunti direttamente al progetto</span><span class="sxs-lookup"><span data-stu-id="ad9dd-165">The **STETemplate.tt** and **STETemplate.Context.tt** files are added directly to your project</span></span>

## <a name="move-entity-types-into-separate-project"></a><span data-ttu-id="ad9dd-166">Spostare i tipi di entità nel progetto separato</span><span class="sxs-lookup"><span data-stu-id="ad9dd-166">Move Entity Types into Separate Project</span></span>

<span data-ttu-id="ad9dd-167">Per usare le entità con rilevamento automatico l'applicazione client deve accedere alle classi di entità generato dal modello.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-167">To use Self-Tracking Entities our client application needs access to the entity classes generated from our model.</span></span> <span data-ttu-id="ad9dd-168">Poiché non si desidera esporre l'intero modello per l'applicazione client si userà per spostare le classi di entità in un progetto separato.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-168">Because we don't want to expose the whole model to the client application we're going to move the entity classes into a separate project.</span></span>

<span data-ttu-id="ad9dd-169">Il primo passaggio consiste arrestano la generazione di classi di entità nel progetto esistente:</span><span class="sxs-lookup"><span data-stu-id="ad9dd-169">The first step is to stop generating entity classes in the existing project:</span></span>

-   <span data-ttu-id="ad9dd-170">Fare clic su **STETemplate.tt** nelle **Esplora soluzioni** e selezionare **proprietà**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-170">Right-click on **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="ad9dd-171">Nel **delle proprietà** finestra chiaro **TextTemplatingFileGenerator** dal **CustomTool** proprietà</span><span class="sxs-lookup"><span data-stu-id="ad9dd-171">In the **Properties** window clear **TextTemplatingFileGenerator** from the **CustomTool** property</span></span>
-   <span data-ttu-id="ad9dd-172">Espandere **STETemplate.tt** nelle **Esplora soluzioni** ed eliminare tutti i file annidati</span><span class="sxs-lookup"><span data-stu-id="ad9dd-172">Expand **STETemplate.tt** in **Solution Explorer** and delete all files nested under it</span></span>

<span data-ttu-id="ad9dd-173">Successivamente, si userà per aggiungere un nuovo progetto e generare le classi di entità in esso</span><span class="sxs-lookup"><span data-stu-id="ad9dd-173">Next, we are going to add a new project and generate the entity classes in it</span></span>

-   <span data-ttu-id="ad9dd-174">**File -&gt; Add -&gt; progetto...**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-174">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="ad9dd-175">Selezionare **Visual C#\#**  nel riquadro a sinistra e quindi **libreria di classi**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-175">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="ad9dd-176">Immettere **STESample.Entities** come nome, quindi fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-176">Enter **STESample.Entities** as the name and click **OK**</span></span>
-   <span data-ttu-id="ad9dd-177">**Progetto -&gt; Aggiungi elemento esistente...**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-177">**Project -&gt; Add Existing Item...**</span></span>
-   <span data-ttu-id="ad9dd-178">Passare il **STESample** cartella del progetto</span><span class="sxs-lookup"><span data-stu-id="ad9dd-178">Navigate to the **STESample** project folder</span></span>
-   <span data-ttu-id="ad9dd-179">Selezionare questa opzione per visualizzare **tutti i file (\*.\*)**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-179">Select to view **All Files (\*.\*)**</span></span>
-   <span data-ttu-id="ad9dd-180">Selezionare il **STETemplate.tt** file</span><span class="sxs-lookup"><span data-stu-id="ad9dd-180">Select the **STETemplate.tt** file</span></span>
-   <span data-ttu-id="ad9dd-181">Fare clic sulla freccia a discesa accanto al **Add** e selezionare **Aggiungi come collegamento**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-181">Click on the drop down arrow next to the **Add** button and select **Add As Link**</span></span>

    ![AddLinkedTemplate](~/ef6/media/addlinkedtemplate.png)

<span data-ttu-id="ad9dd-183">Eseguiremo, inoltre, per assicurarsi che le classi di entità vengono generate nella directory lo stesso spazio dei nomi come contesto.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-183">We're also going to make sure the entity classes get generated in the same namespace as the context.</span></span> <span data-ttu-id="ad9dd-184">Solo questo riduce il numero dell'utilizzo delle istruzioni che è necessario aggiungere in tutta la nostra applicazione.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-184">This just reduces the number of using statements we need to add throughout our application.</span></span>

-   <span data-ttu-id="ad9dd-185">Fare clic su collegato **STETemplate.tt** nelle **Esplora soluzioni** e selezionare **proprietà**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-185">Right-click on the linked **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="ad9dd-186">Nel **delle proprietà** finestra set **Custom Tool Namespace** a **STESample**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-186">In the **Properties** window set **Custom Tool Namespace** to **STESample**</span></span>

<span data-ttu-id="ad9dd-187">Il codice generato dal modello di STE sarà necessario un riferimento a **Serialization** per compilare.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-187">The code generated by the STE template will need a reference to **System.Runtime.Serialization** in order to compile.</span></span> <span data-ttu-id="ad9dd-188">Questa libreria è necessaria per WCF **DataContract** e **DataMember** gli attributi utilizzati sui tipi di entità serializzabili.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-188">This library is needed for the WCF **DataContract** and **DataMember** attributes that are used on the serializable entity types.</span></span>

-   <span data-ttu-id="ad9dd-189">Fare clic con il pulsante destro sul **STESample.Entities** del progetto **Esplora soluzioni** e selezionare **Aggiungi riferimento...**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-189">Right click on the **STESample.Entities** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="ad9dd-190">In Visual Studio 2012 - selezionare la casella accanto a **Serialization** e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-190">In Visual Studio 2012 - check the box next to **System.Runtime.Serialization** and click **OK**</span></span>
    -   <span data-ttu-id="ad9dd-191">In Visual Studio 2010 - selezionare **Serialization** e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-191">In Visual Studio 2010 - select **System.Runtime.Serialization** and click **OK**</span></span>

<span data-ttu-id="ad9dd-192">Infine, il progetto con il contesto in esso sarà necessario un riferimento ai tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-192">Finally, the project with our context in it will need a reference to the entity types.</span></span>

-   <span data-ttu-id="ad9dd-193">Fare clic con il pulsante destro sul **STESample** del progetto **Esplora soluzioni** e selezionare **Aggiungi riferimento...**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-193">Right click on the **STESample** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="ad9dd-194">In Visual Studio 2012 - selezionare **soluzione** nel riquadro sinistro, selezionare la casella accanto a **STESample.Entities** e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-194">In Visual Studio 2012 - select **Solution** from the left pane, check the box next to **STESample.Entities** and click **OK**</span></span>
    -   <span data-ttu-id="ad9dd-195">In Visual Studio 2010 - selezionare il **progetti** scheda, seleziona **STESample.Entities** e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-195">In Visual Studio 2010 - select the **Projects** tab, select **STESample.Entities** and click **OK**</span></span>

>[!NOTE]
> <span data-ttu-id="ad9dd-196">Un'altra opzione per spostare i tipi di entità in un progetto separato consiste nello spostare il file di modello, anziché il relativo collegamento dalla posizione predefinita.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-196">Another option for moving the entity types to a separate project is to move the template file, rather than linking it from its default location.</span></span> <span data-ttu-id="ad9dd-197">In questo caso, è necessario aggiornare il **inputFile** variabili nel modello per fornire il percorso relativo al file con estensione edmx (in questo esempio che sarà **.. \\BloggingModel.edmx**).</span><span class="sxs-lookup"><span data-stu-id="ad9dd-197">If you do this, you will need to update the **inputFile** variable in the template to provide the relative path to the edmx file (in this example that would be **..\\BloggingModel.edmx**).</span></span>

## <a name="create-a-wcf-service"></a><span data-ttu-id="ad9dd-198">Creare un servizio WCF</span><span class="sxs-lookup"><span data-stu-id="ad9dd-198">Create a WCF Service</span></span>

<span data-ttu-id="ad9dd-199">A questo punto è possibile aggiungere un servizio WCF di esporre i dati, si inizierà creando il progetto.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-199">Now it's time to add a WCF Service to expose our data, we'll start by creating the project.</span></span>

-   <span data-ttu-id="ad9dd-200">**File -&gt; Add -&gt; progetto...**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-200">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="ad9dd-201">Selezionare **Visual C#\#**  nel riquadro a sinistra e quindi **applicazione del servizio WCF**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-201">Select **Visual C\#** from the left pane and then **WCF Service Application**</span></span>
-   <span data-ttu-id="ad9dd-202">Immettere **STESample.Service** come nome, quindi fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-202">Enter **STESample.Service** as the name and click **OK**</span></span>
-   <span data-ttu-id="ad9dd-203">Aggiungere un riferimento per la **Data. Entity** assembly</span><span class="sxs-lookup"><span data-stu-id="ad9dd-203">Add a reference to the **System.Data.Entity** assembly</span></span>
-   <span data-ttu-id="ad9dd-204">Aggiungere un riferimento per la **STESample** e **STESample.Entities** progetti</span><span class="sxs-lookup"><span data-stu-id="ad9dd-204">Add a reference to the **STESample** and **STESample.Entities** projects</span></span>

<span data-ttu-id="ad9dd-205">È necessario copiare la stringa di connessione Entity Framework per questo progetto in modo che si trova in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-205">We need to copy the EF connection string to this project so that it is found at runtime.</span></span>

-   <span data-ttu-id="ad9dd-206">Aprire il **app. config** del file per la * * STESample * * progetto e copiare la **connectionStrings** elemento</span><span class="sxs-lookup"><span data-stu-id="ad9dd-206">Open the **App.Config** file for the **STESample **project and copy the **connectionStrings** element</span></span>
-   <span data-ttu-id="ad9dd-207">Incollare il **connectionStrings** come elemento figlio dell'elemento il **configuration** elemento del **Web. config** del file nel **STESample.Service** progetto</span><span class="sxs-lookup"><span data-stu-id="ad9dd-207">Paste the **connectionStrings** element as a child element of the **configuration** element of the **Web.Config** file in the **STESample.Service** project</span></span>

<span data-ttu-id="ad9dd-208">A questo punto è possibile implementare il servizio effettivo.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-208">Now it's time to implement the actual service.</span></span>

-   <span data-ttu-id="ad9dd-209">Aprire **IService1.cs** e sostituire il contenuto con il codice seguente</span><span class="sxs-lookup"><span data-stu-id="ad9dd-209">Open **IService1.cs** and replace the contents with the following code</span></span>

``` csharp
    using System.Collections.Generic;
    using System.ServiceModel;

    namespace STESample.Service
    {
        [ServiceContract]
        public interface IService1
        {
            [OperationContract]
            List<Blog> GetBlogs();

            [OperationContract]
            void UpdateBlog(Blog blog);
        }
    }
```

-   <span data-ttu-id="ad9dd-210">Aprire **Service1.svc** e sostituire il contenuto con il codice seguente</span><span class="sxs-lookup"><span data-stu-id="ad9dd-210">Open **Service1.svc** and replace the contents with the following code</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Linq;

    namespace STESample.Service
    {
        public class Service1 : IService1
        {
            /// <summary>
            /// Gets all the Blogs and related Posts.
            /// </summary>
            public List<Blog> GetBlogs()
            {
                using (BloggingContext context = new BloggingContext())
                {
                    return context.Blogs.Include("Posts").ToList();
                }
            }

            /// <summary>
            /// Updates Blog and its related Posts.
            /// </summary>
            public void UpdateBlog(Blog blog)
            {
                using (BloggingContext context = new BloggingContext())
                {
                    try
                    {
                        // TODO: Perform validation on the updated order before applying the changes.

                        // The ApplyChanges method examines the change tracking information
                        // contained in the graph of self-tracking entities to infer the set of operations
                        // that need to be performed to reflect the changes in the database.
                        context.Blogs.ApplyChanges(blog);
                        context.SaveChanges();

                    }
                    catch (UpdateException)
                    {
                        // To avoid propagating exception messages that contain sensitive data to the client tier
                        // calls to ApplyChanges and SaveChanges should be wrapped in exception handling code.
                        throw new InvalidOperationException("Failed to update. Try your request again.");
                    }
                }
            }        
        }
    }
```

## <a name="consume-the-service-from-a-console-application"></a><span data-ttu-id="ad9dd-211">Uso del servizio da un'applicazione Console</span><span class="sxs-lookup"><span data-stu-id="ad9dd-211">Consume the Service from a Console Application</span></span>

<span data-ttu-id="ad9dd-212">Creare un'applicazione console che usa il nostro servizio.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-212">Let's create a console application that uses our service.</span></span>

-   <span data-ttu-id="ad9dd-213">**File -&gt; New -&gt; progetto...**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-213">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="ad9dd-214">Selezionare **Visual C#\#**  nel riquadro a sinistra e quindi **applicazione Console**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-214">Select **Visual C\#** from the left pane and then **Console Application**</span></span>
-   <span data-ttu-id="ad9dd-215">Immettere **STESample.ConsoleTest** come nome, quindi fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-215">Enter **STESample.ConsoleTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="ad9dd-216">Aggiungere un riferimento per la **STESample.Entities** progetto</span><span class="sxs-lookup"><span data-stu-id="ad9dd-216">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="ad9dd-217">È necessario un riferimento al nostro servizio WCF</span><span class="sxs-lookup"><span data-stu-id="ad9dd-217">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="ad9dd-218">Fare doppio clic sui **STESample.ConsoleTest** del progetto nelle **Esplora soluzioni** e selezionare **Aggiungi riferimento al servizio...**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-218">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="ad9dd-219">Fare clic su **individuare**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-219">Click **Discover**</span></span>
-   <span data-ttu-id="ad9dd-220">Immettere **BloggingService** come spazio dei nomi e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-220">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="ad9dd-221">Sarà quindi possibile scrivere codice per utilizzare il servizio.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-221">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="ad9dd-222">Aprire **Program.cs** e sostituire il contenuto con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-222">Open **Program.cs** and replace the contents with the following code.</span></span>

``` csharp
    using STESample.ConsoleTest.BloggingService;
    using System;
    using System.Linq;

    namespace STESample.ConsoleTest
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Print out the data before we change anything
                Console.WriteLine("Initial Data:");
                DisplayBlogsAndPosts();

                // Add a new Blog and some Posts
                AddBlogAndPost();
                Console.WriteLine("After Adding:");
                DisplayBlogsAndPosts();

                // Modify the Blog and one of its Posts
                UpdateBlogAndPost();
                Console.WriteLine("After Update:");
                DisplayBlogsAndPosts();

                // Delete the Blog and its Posts
                DeleteBlogAndPost();
                Console.WriteLine("After Delete:");
                DisplayBlogsAndPosts();

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            static void DisplayBlogsAndPosts()
            {
                using (var service = new Service1Client())
                {
                    // Get all Blogs (and Posts) from the service
                    // and print them to the console
                    var blogs = service.GetBlogs();
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(blog.Name);
                        foreach (var post in blog.Posts)
                        {
                            Console.WriteLine(" - {0}", post.Title);
                        }
                    }
                }

                Console.WriteLine();
                Console.WriteLine();
            }

            static void AddBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Create a new Blog with a couple of Posts
                    var newBlog = new Blog
                    {
                        Name = "The New Blog",
                        Posts =
                        {
                            new Post { Title = "Welcome to the new blog"},
                            new Post { Title = "What's new on the new blog"}
                        }
                    };

                    // Save the changes using the service
                    service.UpdateBlog(newBlog);
                }
            }

            static void UpdateBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The New Blog
                    var blog = blogs.First(b => b.Name == "The New Blog");

                    // Update the Blogs name
                    blog.Name = "The Not-So-New Blog";

                    // Update one of the related posts
                    blog.Posts.First().Content = "Some interesting content...";

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }

            static void DeleteBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The Not-So-New Blog
                    var blog = blogs.First(b => b.Name == "The Not-So-New Blog");

                    // Mark all related Posts for deletion
                    // We need to call ToList because each Post will be removed from the
                    // Posts collection when we call MarkAsDeleted
                    foreach (var post in blog.Posts.ToList())
                    {
                        post.MarkAsDeleted();
                    }

                    // Mark the Blog for deletion
                    blog.MarkAsDeleted();

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }
        }
    }
```

<span data-ttu-id="ad9dd-223">È ora possibile eseguire l'applicazione per vederla in azione.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-223">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="ad9dd-224">Fare doppio clic sui **STESample.ConsoleTest** del progetto nelle **Esplora soluzioni** e selezionare **Debug -&gt; Avvia nuova istanza**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-224">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>

<span data-ttu-id="ad9dd-225">Si noterà l'output seguente quando viene eseguita l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-225">You'll see the following output when the application executes.</span></span>

```
Initial Data:
ADO.NET Blog
- Intro to EF
- What is New

After Adding:
ADO.NET Blog
- Intro to EF
- What is New
The New Blog
- Welcome to the new blog
- What's new on the new blog

After Update:
ADO.NET Blog
- Intro to EF
- What is New
The Not-So-New Blog
- Welcome to the new blog
- What's new on the new blog

After Delete:
ADO.NET Blog
- Intro to EF
- What is New

Press any key to exit...
```

## <a name="consume-the-service-from-a-wpf-application"></a><span data-ttu-id="ad9dd-226">Uso del servizio da un'applicazione WPF</span><span class="sxs-lookup"><span data-stu-id="ad9dd-226">Consume the Service from a WPF Application</span></span>

<span data-ttu-id="ad9dd-227">Creare un'applicazione WPF che usa il nostro servizio.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-227">Let's create a WPF application that uses our service.</span></span>

-   <span data-ttu-id="ad9dd-228">**File -&gt; New -&gt; progetto...**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-228">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="ad9dd-229">Selezionare **Visual C#\#**  nel riquadro a sinistra e quindi **applicazione WPF**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-229">Select **Visual C\#** from the left pane and then **WPF Application**</span></span>
-   <span data-ttu-id="ad9dd-230">Immettere **STESample.WPFTest** come nome, quindi fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-230">Enter **STESample.WPFTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="ad9dd-231">Aggiungere un riferimento per la **STESample.Entities** progetto</span><span class="sxs-lookup"><span data-stu-id="ad9dd-231">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="ad9dd-232">È necessario un riferimento al nostro servizio WCF</span><span class="sxs-lookup"><span data-stu-id="ad9dd-232">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="ad9dd-233">Fare doppio clic sui **STESample.WPFTest** del progetto nelle **Esplora soluzioni** e selezionare **Aggiungi riferimento al servizio...**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-233">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="ad9dd-234">Fare clic su **individuare**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-234">Click **Discover**</span></span>
-   <span data-ttu-id="ad9dd-235">Immettere **BloggingService** come spazio dei nomi e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-235">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="ad9dd-236">Sarà quindi possibile scrivere codice per utilizzare il servizio.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-236">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="ad9dd-237">Aprire **MainWindow. XAML** e sostituire il contenuto con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-237">Open **MainWindow.xaml** and replace the contents with the following code.</span></span>

``` xaml
    <Window
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:STESample="clr-namespace:STESample;assembly=STESample.Entities"
        mc:Ignorable="d" x:Class="STESample.WPFTest.MainWindow"
        Title="MainWindow" Height="350" Width="525" Loaded="Window_Loaded">

        <Window.Resources>
            <CollectionViewSource
                x:Key="blogViewSource"
                d:DesignSource="{d:DesignInstance {x:Type STESample:Blog}, CreateList=True}"/>
            <CollectionViewSource
                x:Key="blogPostsViewSource"
                Source="{Binding Posts, Source={StaticResource blogViewSource}}"/>
        </Window.Resources>

        <Grid DataContext="{StaticResource blogViewSource}">
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding}" Margin="10,10,10,179">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding BlogId}" Header="Id" Width="Auto" IsReadOnly="True" />
                    <DataGridTextColumn Binding="{Binding Name}" Header="Name" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Url}" Header="Url" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding Source={StaticResource blogPostsViewSource}}" Margin="10,145,10,38">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding PostId}" Header="Id" Width="Auto"  IsReadOnly="True"/>
                    <DataGridTextColumn Binding="{Binding Title}" Header="Title" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Content}" Header="Content" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <Button Width="68" Height="23" HorizontalAlignment="Right" VerticalAlignment="Bottom"
                    Margin="0,0,10,10" Click="buttonSave_Click">Save</Button>
        </Grid>
    </Window>
```

-   <span data-ttu-id="ad9dd-238">Aprire il code-behind per MainWindow (**MainWindow.xaml.cs**) e sostituire il contenuto con il codice seguente</span><span class="sxs-lookup"><span data-stu-id="ad9dd-238">Open the code behind for MainWindow (**MainWindow.xaml.cs**) and replace the contents with the following code</span></span>

``` csharp
    using STESample.WPFTest.BloggingService;
    using System.Collections.Generic;
    using System.Linq;
    using System.Windows;
    using System.Windows.Data;

    namespace STESample.WPFTest
    {
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Find the view source for Blogs and populate it with all Blogs (and related Posts)
                    // from the Service. The default editing functionality of WPF will allow the objects
                    // to be manipulated on the screen.
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Get the blogs that are bound to the screen
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    var blogs = (List<Blog>)blogsViewSource.Source;

                    // Save all Blogs and related Posts
                    foreach (var blog in blogs)
                    {
                        service.UpdateBlog(blog);
                    }

                    // Re-query for data to get database-generated keys etc.
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }
        }
    }
```

<span data-ttu-id="ad9dd-239">È ora possibile eseguire l'applicazione per vederla in azione.</span><span class="sxs-lookup"><span data-stu-id="ad9dd-239">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="ad9dd-240">Fare doppio clic sui **STESample.WPFTest** del progetto nelle **Esplora soluzioni** e selezionare **Debug -&gt; Avvia nuova istanza**</span><span class="sxs-lookup"><span data-stu-id="ad9dd-240">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>
-   <span data-ttu-id="ad9dd-241">È possibile modificare i dati utilizzando la schermata e salvare il file tramite il servizio utilizzando il **salvare** pulsante</span><span class="sxs-lookup"><span data-stu-id="ad9dd-241">You can manipulate the data using the screen and save it via the service using the **Save** button</span></span>

![WPF](~/ef6/media/wpf.png)
