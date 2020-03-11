---
title: Procedure dettagliate per le entità con rilevamento automatico-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: 9bd644461f50a7eff1006cb8866ca9a3b08b6b8d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419533"
---
# <a name="self-tracking-entities-walkthrough"></a><span data-ttu-id="1e834-102">Procedura dettagliata sulle entità con rilevamento automatico</span><span class="sxs-lookup"><span data-stu-id="1e834-102">Self-Tracking Entities Walkthrough</span></span>
> [!IMPORTANT]
> <span data-ttu-id="1e834-103">Non è più consigliabile usare il modello di entità con rilevamento automatico.</span><span class="sxs-lookup"><span data-stu-id="1e834-103">We no longer recommend using the self-tracking-entities template.</span></span> <span data-ttu-id="1e834-104">Continuerà a essere disponibile solo per supportare le applicazioni esistenti.</span><span class="sxs-lookup"><span data-stu-id="1e834-104">It will only continue to be available to support existing applications.</span></span> <span data-ttu-id="1e834-105">Se l'applicazione richiede l'uso con grafici di entità disconnesse, prendere in considerazione altre alternative, come ad esempio [Trackable Entities](https://trackableentities.github.io/), che è una tecnologia simile alle entità con rilevamento automatico ma viene sviluppata in modo più attivo dalla community, oppure la scrittura di codice personalizzato usando le API di rilevamento delle modifiche di basso livello.</span><span class="sxs-lookup"><span data-stu-id="1e834-105">If your application requires working with disconnected graphs of entities, consider other alternatives such as [Trackable Entities](https://trackableentities.github.io/), which is a technology similar to Self-Tracking-Entities that is more actively developed by the community, or writing custom code using the low-level change tracking APIs.</span></span>

<span data-ttu-id="1e834-106">In questa procedura dettagliata viene illustrato lo scenario in cui un servizio Windows Communication Foundation (WCF) espone un'operazione che restituisce un grafico di entità.</span><span class="sxs-lookup"><span data-stu-id="1e834-106">This walkthrough demonstrates the scenario in which a Windows Communication Foundation (WCF) service exposes an operation that returns an entity graph.</span></span> <span data-ttu-id="1e834-107">Successivamente, un'applicazione client modifica il grafico e invia le modifiche a un'operazione del servizio che convalida e salva gli aggiornamenti in un database utilizzando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1e834-107">Next, a client application manipulates that graph and submits the modifications to a service operation that validates and saves the updates to a database using Entity Framework.</span></span>

<span data-ttu-id="1e834-108">Prima di completare questa procedura dettagliata, assicurarsi di leggere la pagina [entità con rilevamento automatico](index.md) .</span><span class="sxs-lookup"><span data-stu-id="1e834-108">Before completing this walkthrough make sure you read the [Self-Tracking Entities](index.md) page.</span></span>

<span data-ttu-id="1e834-109">In questa procedura dettagliata verranno completate le seguenti azioni:</span><span class="sxs-lookup"><span data-stu-id="1e834-109">This walkthrough completes the following actions:</span></span>

-   <span data-ttu-id="1e834-110">Crea un database a cui accedere.</span><span class="sxs-lookup"><span data-stu-id="1e834-110">Creates a database to access.</span></span>
-   <span data-ttu-id="1e834-111">Crea una libreria di classi che contiene il modello.</span><span class="sxs-lookup"><span data-stu-id="1e834-111">Creates a class library that contains the model.</span></span>
-   <span data-ttu-id="1e834-112">Scambia il modello generatore di entità con rilevamento automatico.</span><span class="sxs-lookup"><span data-stu-id="1e834-112">Swaps to the Self-Tracking Entity Generator template.</span></span>
-   <span data-ttu-id="1e834-113">Sposta le classi di entità in un progetto separato.</span><span class="sxs-lookup"><span data-stu-id="1e834-113">Moves the entity classes to a separate project.</span></span>
-   <span data-ttu-id="1e834-114">Crea un servizio WCF che espone operazioni per eseguire query e salvare entità.</span><span class="sxs-lookup"><span data-stu-id="1e834-114">Creates a WCF service that exposes operations to query and save entities.</span></span>
-   <span data-ttu-id="1e834-115">Crea applicazioni client (console e WPF) che utilizzano il servizio.</span><span class="sxs-lookup"><span data-stu-id="1e834-115">Creates client applications (Console and WPF) that consume the service.</span></span>

<span data-ttu-id="1e834-116">In questa procedura dettagliata verranno usati Database First, ma le stesse tecniche si applicano ugualmente ai Model First.</span><span class="sxs-lookup"><span data-stu-id="1e834-116">We'll use Database First in this walkthrough but the same techniques apply equally to Model First.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="1e834-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="1e834-117">Pre-Requisites</span></span>

<span data-ttu-id="1e834-118">Per completare questa procedura dettagliata, sarà necessaria una versione recente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1e834-118">To complete this walkthrough you will need a recent version of Visual Studio.</span></span>

## <a name="create-a-database"></a><span data-ttu-id="1e834-119">Creare un database</span><span class="sxs-lookup"><span data-stu-id="1e834-119">Create a Database</span></span>

<span data-ttu-id="1e834-120">Il server di database installato con Visual Studio è diverso a seconda della versione di Visual Studio installata:</span><span class="sxs-lookup"><span data-stu-id="1e834-120">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="1e834-121">Se si usa Visual Studio 2012, si creerà un database del database locale.</span><span class="sxs-lookup"><span data-stu-id="1e834-121">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="1e834-122">Se si usa Visual Studio 2010 verrà creato un database di SQL Express.</span><span class="sxs-lookup"><span data-stu-id="1e834-122">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="1e834-123">Procediamo con la generazione del database.</span><span class="sxs-lookup"><span data-stu-id="1e834-123">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="1e834-124">Aprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1e834-124">Open Visual Studio</span></span>
-   <span data-ttu-id="1e834-125">**Visualizza-&gt; Esplora server**</span><span class="sxs-lookup"><span data-stu-id="1e834-125">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="1e834-126">Fare clic con il pulsante destro del mouse su **connessioni dati-&gt; Aggiungi connessione...**</span><span class="sxs-lookup"><span data-stu-id="1e834-126">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="1e834-127">Se non si è connessi a un database da Esplora server prima di selezionare **Microsoft SQL Server** come origine dati</span><span class="sxs-lookup"><span data-stu-id="1e834-127">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="1e834-128">Connettersi a un database locale o a SQL Express, a seconda di quale installato</span><span class="sxs-lookup"><span data-stu-id="1e834-128">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="1e834-129">Immettere **STESample** come nome del database</span><span class="sxs-lookup"><span data-stu-id="1e834-129">Enter **STESample** as the database name</span></span>
-   <span data-ttu-id="1e834-130">Selezionare **OK** . verrà richiesto se si desidera creare un nuovo database, selezionare **Sì** .</span><span class="sxs-lookup"><span data-stu-id="1e834-130">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="1e834-131">Il nuovo database verrà ora visualizzato in Esplora server</span><span class="sxs-lookup"><span data-stu-id="1e834-131">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="1e834-132">Se si usa Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="1e834-132">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="1e834-133">Fare clic con il pulsante destro del mouse sul database in Esplora server e selezionare **nuova query** .</span><span class="sxs-lookup"><span data-stu-id="1e834-133">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="1e834-134">Copiare il codice SQL seguente nella nuova query, quindi fare clic con il pulsante destro del mouse sulla query e scegliere **Esegui** .</span><span class="sxs-lookup"><span data-stu-id="1e834-134">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="1e834-135">Se si usa Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="1e834-135">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="1e834-136">Selezione **dati-&gt; editor Transact-SQL-&gt; nuova connessione query...**</span><span class="sxs-lookup"><span data-stu-id="1e834-136">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="1e834-137">Immettere **.\\SQLEXPRESS** come nome del server e fare clic su **OK** .</span><span class="sxs-lookup"><span data-stu-id="1e834-137">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="1e834-138">Selezionare il database **STESample** dall'elenco a discesa nella parte superiore dell'editor di query</span><span class="sxs-lookup"><span data-stu-id="1e834-138">Select the **STESample** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="1e834-139">Copiare il codice SQL seguente nella nuova query, quindi fare clic con il pulsante destro del mouse sulla query e scegliere **Esegui SQL**</span><span class="sxs-lookup"><span data-stu-id="1e834-139">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

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

## <a name="create-the-model"></a><span data-ttu-id="1e834-140">Creare il modello</span><span class="sxs-lookup"><span data-stu-id="1e834-140">Create the Model</span></span>

<span data-ttu-id="1e834-141">Per prima cosa, abbiamo bisogno di un progetto in cui inserire il modello.</span><span class="sxs-lookup"><span data-stu-id="1e834-141">First up, we need a project to put the model in.</span></span>

-   <span data-ttu-id="1e834-142">**Nuovo progetto&gt; di&gt; file...**</span><span class="sxs-lookup"><span data-stu-id="1e834-142">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="1e834-143">Selezionare **Visual C\#** dal riquadro a sinistra e quindi **libreria di classi**</span><span class="sxs-lookup"><span data-stu-id="1e834-143">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="1e834-144">Immettere **STESample** come nome e fare clic su **OK** .</span><span class="sxs-lookup"><span data-stu-id="1e834-144">Enter **STESample** as the name and click **OK**</span></span>

<span data-ttu-id="1e834-145">Verrà ora creato un modello semplice in EF designer per accedere al database:</span><span class="sxs-lookup"><span data-stu-id="1e834-145">Now we'll create a simple model in the EF Designer to access our database:</span></span>

-   <span data-ttu-id="1e834-146">**Progetto-&gt; Aggiungi nuovo elemento...**</span><span class="sxs-lookup"><span data-stu-id="1e834-146">**Project -&gt; Add New Item...**</span></span>
-   <span data-ttu-id="1e834-147">Selezionare i **dati** nel riquadro a sinistra e quindi **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="1e834-147">Select **Data** from the left pane and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="1e834-148">Immettere **BloggingModel** come nome e fare clic su **OK** .</span><span class="sxs-lookup"><span data-stu-id="1e834-148">Enter **BloggingModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="1e834-149">Selezionare **genera da database** e fare clic su **Avanti** .</span><span class="sxs-lookup"><span data-stu-id="1e834-149">Select **Generate from database** and click **Next**</span></span>
-   <span data-ttu-id="1e834-150">Immettere le informazioni di connessione per il database creato nella sezione precedente</span><span class="sxs-lookup"><span data-stu-id="1e834-150">Enter the connection information for the database that you created in the previous section</span></span>
-   <span data-ttu-id="1e834-151">Immettere **BloggingContext** come nome della stringa di connessione e fare clic su **Next (avanti** ).</span><span class="sxs-lookup"><span data-stu-id="1e834-151">Enter **BloggingContext** as the name for the connection string and click **Next**</span></span>
-   <span data-ttu-id="1e834-152">Selezionare la casella accanto a **tabelle** e fare clic su **fine** .</span><span class="sxs-lookup"><span data-stu-id="1e834-152">Check the box next to **Tables** and click **Finish**</span></span>

## <a name="swap-to-ste-code-generation"></a><span data-ttu-id="1e834-153">Scambia alla generazione del codice STE</span><span class="sxs-lookup"><span data-stu-id="1e834-153">Swap to STE Code Generation</span></span>

<span data-ttu-id="1e834-154">A questo punto è necessario disabilitare la generazione di codice predefinita e scambiare le entità con rilevamento automatico.</span><span class="sxs-lookup"><span data-stu-id="1e834-154">Now we need to disable the default code generation and swap to Self-Tracking Entities.</span></span>

### <a name="if-you-are-using-visual-studio-2012"></a><span data-ttu-id="1e834-155">Se si usa Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="1e834-155">If you are using Visual Studio 2012</span></span>

-   <span data-ttu-id="1e834-156">Espandere **BloggingModel. edmx** in **Esplora soluzioni** ed eliminare **BloggingModel.TT** e **BloggingModel.Context.TT**
    *verrà disabilitata la generazione di codice predefinita* .</span><span class="sxs-lookup"><span data-stu-id="1e834-156">Expand **BloggingModel.edmx** in **Solution Explorer** and delete the **BloggingModel.tt** and **BloggingModel.Context.tt**
*This will disable the default code generation*</span></span>
-   <span data-ttu-id="1e834-157">Fare clic con il pulsante destro del mouse su un'area vuota nell'area di progettazione EF e scegliere **Aggiungi elemento di generazione codice...**</span><span class="sxs-lookup"><span data-stu-id="1e834-157">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="1e834-158">Selezionare **online** dal riquadro a sinistra e cercare il **Generatore di ste**</span><span class="sxs-lookup"><span data-stu-id="1e834-158">Select **Online** from the left pane and search for **STE Generator**</span></span>
-   <span data-ttu-id="1e834-159">Selezionare il modello **Ste Generator per C\#** , immettere **STETemplate** come nome e fare clic su **Aggiungi** .</span><span class="sxs-lookup"><span data-stu-id="1e834-159">Select the **STE Generator for C\#** template, enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="1e834-160">I file **STETemplate.TT** e **STETemplate.Context.TT** vengono aggiunti annidati nel file BloggingModel. edmx</span><span class="sxs-lookup"><span data-stu-id="1e834-160">The **STETemplate.tt** and **STETemplate.Context.tt** files are added nested under the BloggingModel.edmx file</span></span>

### <a name="if-you-are-using-visual-studio-2010"></a><span data-ttu-id="1e834-161">Se si usa Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="1e834-161">If you are using Visual Studio 2010</span></span>

-   <span data-ttu-id="1e834-162">Fare clic con il pulsante destro del mouse su un'area vuota nell'area di progettazione EF e scegliere **Aggiungi elemento di generazione codice...**</span><span class="sxs-lookup"><span data-stu-id="1e834-162">Right-click an empty area on the EF Designer surface and select **Add Code Generation Item...**</span></span>
-   <span data-ttu-id="1e834-163">Selezionare il **codice** nel riquadro a sinistra e quindi **ADO.NET generatore di entità con rilevamento automatico**</span><span class="sxs-lookup"><span data-stu-id="1e834-163">Select **Code** from the left pane and then **ADO.NET Self-Tracking Entity Generator**</span></span>
-   <span data-ttu-id="1e834-164">Immettere **STETemplate** come nome e fare clic su **Aggiungi**</span><span class="sxs-lookup"><span data-stu-id="1e834-164">Enter **STETemplate** as the name and click **Add**</span></span>
-   <span data-ttu-id="1e834-165">I file **STETemplate.TT** e **STETemplate.Context.TT** vengono aggiunti direttamente al progetto</span><span class="sxs-lookup"><span data-stu-id="1e834-165">The **STETemplate.tt** and **STETemplate.Context.tt** files are added directly to your project</span></span>

## <a name="move-entity-types-into-separate-project"></a><span data-ttu-id="1e834-166">Spostare i tipi di entità in un progetto separato</span><span class="sxs-lookup"><span data-stu-id="1e834-166">Move Entity Types into Separate Project</span></span>

<span data-ttu-id="1e834-167">Per usare le entità con rilevamento automatico, l'applicazione client deve accedere alle classi di entità generate dal modello.</span><span class="sxs-lookup"><span data-stu-id="1e834-167">To use Self-Tracking Entities our client application needs access to the entity classes generated from our model.</span></span> <span data-ttu-id="1e834-168">Poiché non si vuole esporre l'intero modello all'applicazione client, le classi di entità verranno spostate in un progetto distinto.</span><span class="sxs-lookup"><span data-stu-id="1e834-168">Because we don't want to expose the whole model to the client application we're going to move the entity classes into a separate project.</span></span>

<span data-ttu-id="1e834-169">Il primo passaggio consiste nell'arrestare la generazione delle classi di entità nel progetto esistente:</span><span class="sxs-lookup"><span data-stu-id="1e834-169">The first step is to stop generating entity classes in the existing project:</span></span>

-   <span data-ttu-id="1e834-170">Fare clic con il pulsante destro del mouse su **STETemplate.TT** in **Esplora soluzioni** e scegliere **proprietà** .</span><span class="sxs-lookup"><span data-stu-id="1e834-170">Right-click on **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="1e834-171">Nella finestra **Proprietà** deselezionare **TextTemplatingFileGenerator** dalla proprietà **CustomTool**</span><span class="sxs-lookup"><span data-stu-id="1e834-171">In the **Properties** window clear **TextTemplatingFileGenerator** from the **CustomTool** property</span></span>
-   <span data-ttu-id="1e834-172">Espandere **STETemplate.TT** in **Esplora soluzioni** ed eliminare tutti i file annidati sotto di esso</span><span class="sxs-lookup"><span data-stu-id="1e834-172">Expand **STETemplate.tt** in **Solution Explorer** and delete all files nested under it</span></span>

<span data-ttu-id="1e834-173">Successivamente, verrà aggiunto un nuovo progetto e verranno generate le classi di entità</span><span class="sxs-lookup"><span data-stu-id="1e834-173">Next, we are going to add a new project and generate the entity classes in it</span></span>

-   <span data-ttu-id="1e834-174">**Progetto Add-&gt;&gt; file...**</span><span class="sxs-lookup"><span data-stu-id="1e834-174">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="1e834-175">Selezionare **Visual C\#** dal riquadro a sinistra e quindi **libreria di classi**</span><span class="sxs-lookup"><span data-stu-id="1e834-175">Select **Visual C\#** from the left pane and then **Class Library**</span></span>
-   <span data-ttu-id="1e834-176">Immettere **STESample. Entities** come nome e fare clic su **OK** .</span><span class="sxs-lookup"><span data-stu-id="1e834-176">Enter **STESample.Entities** as the name and click **OK**</span></span>
-   <span data-ttu-id="1e834-177">**Progetto-&gt; Aggiungi elemento esistente...**</span><span class="sxs-lookup"><span data-stu-id="1e834-177">**Project -&gt; Add Existing Item...**</span></span>
-   <span data-ttu-id="1e834-178">Passare alla cartella del progetto **STESample**</span><span class="sxs-lookup"><span data-stu-id="1e834-178">Navigate to the **STESample** project folder</span></span>
-   <span data-ttu-id="1e834-179">Selezionare per visualizzare **tutti i file (\*.\*)**</span><span class="sxs-lookup"><span data-stu-id="1e834-179">Select to view **All Files (\*.\*)**</span></span>
-   <span data-ttu-id="1e834-180">Selezionare il file **STETemplate.TT**</span><span class="sxs-lookup"><span data-stu-id="1e834-180">Select the **STETemplate.tt** file</span></span>
-   <span data-ttu-id="1e834-181">Fare clic sulla freccia a discesa accanto al pulsante **Aggiungi** e selezionare **Aggiungi come collegamento** .</span><span class="sxs-lookup"><span data-stu-id="1e834-181">Click on the drop down arrow next to the **Add** button and select **Add As Link**</span></span>

    ![Aggiungi modello collegato](~/ef6/media/addlinkedtemplate.png)

<span data-ttu-id="1e834-183">Si verifica inoltre che le classi di entità vengano generate nello stesso spazio dei nomi del contesto.</span><span class="sxs-lookup"><span data-stu-id="1e834-183">We're also going to make sure the entity classes get generated in the same namespace as the context.</span></span> <span data-ttu-id="1e834-184">In questo modo si riduce il numero di istruzioni using che è necessario aggiungere all'interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="1e834-184">This just reduces the number of using statements we need to add throughout our application.</span></span>

-   <span data-ttu-id="1e834-185">Fare clic con il pulsante destro del mouse sul **STETemplate.TT** collegato in **Esplora soluzioni** e selezionare **proprietà** .</span><span class="sxs-lookup"><span data-stu-id="1e834-185">Right-click on the linked **STETemplate.tt** in **Solution Explorer** and select **Properties**</span></span>
-   <span data-ttu-id="1e834-186">Nella finestra **Proprietà** impostare lo **spazio dei nomi dello strumento personalizzato** su **STESample**</span><span class="sxs-lookup"><span data-stu-id="1e834-186">In the **Properties** window set **Custom Tool Namespace** to **STESample**</span></span>

<span data-ttu-id="1e834-187">Per la compilazione del codice generato dal modello STE è necessario un riferimento a **System. Runtime. Serialization** .</span><span class="sxs-lookup"><span data-stu-id="1e834-187">The code generated by the STE template will need a reference to **System.Runtime.Serialization** in order to compile.</span></span> <span data-ttu-id="1e834-188">Questa libreria è necessaria per gli attributi **DataContract** e **DataMember** WCF utilizzati sui tipi di entità serializzabili.</span><span class="sxs-lookup"><span data-stu-id="1e834-188">This library is needed for the WCF **DataContract** and **DataMember** attributes that are used on the serializable entity types.</span></span>

-   <span data-ttu-id="1e834-189">Fare clic con il pulsante destro del mouse sul progetto **STESample. Entities** in **Esplora soluzioni** e scegliere **Aggiungi riferimento.**</span><span class="sxs-lookup"><span data-stu-id="1e834-189">Right click on the **STESample.Entities** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="1e834-190">In Visual Studio 2012-selezionare la casella accanto a **System. Runtime. Serialization** e fare clic su **OK** .</span><span class="sxs-lookup"><span data-stu-id="1e834-190">In Visual Studio 2012 - check the box next to **System.Runtime.Serialization** and click **OK**</span></span>
    -   <span data-ttu-id="1e834-191">In Visual Studio 2010 selezionare **System. Runtime. Serialization** e fare clic su **OK** .</span><span class="sxs-lookup"><span data-stu-id="1e834-191">In Visual Studio 2010 - select **System.Runtime.Serialization** and click **OK**</span></span>

<span data-ttu-id="1e834-192">Infine, il progetto con il contesto in esso contenuto sarà necessario un riferimento ai tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="1e834-192">Finally, the project with our context in it will need a reference to the entity types.</span></span>

-   <span data-ttu-id="1e834-193">Fare clic con il pulsante destro del mouse sul progetto **STESample** in **Esplora soluzioni** e scegliere **Aggiungi riferimento.**</span><span class="sxs-lookup"><span data-stu-id="1e834-193">Right click on the **STESample** project in **Solution Explorer** and select **Add Reference...**</span></span>
    -   <span data-ttu-id="1e834-194">In Visual Studio 2012 selezionare **soluzione** nel riquadro a sinistra, selezionare la casella accanto a **STESample. Entities** e fare clic su **OK** .</span><span class="sxs-lookup"><span data-stu-id="1e834-194">In Visual Studio 2012 - select **Solution** from the left pane, check the box next to **STESample.Entities** and click **OK**</span></span>
    -   <span data-ttu-id="1e834-195">In Visual Studio 2010 selezionare la scheda **progetti** , selezionare **STESample. Entities** , quindi fare clic su **OK** .</span><span class="sxs-lookup"><span data-stu-id="1e834-195">In Visual Studio 2010 - select the **Projects** tab, select **STESample.Entities** and click **OK**</span></span>

>[!NOTE]
> <span data-ttu-id="1e834-196">Un'altra opzione per spostare i tipi di entità in un progetto separato consiste nello spostare il file modello, anziché collegarlo dal percorso predefinito.</span><span class="sxs-lookup"><span data-stu-id="1e834-196">Another option for moving the entity types to a separate project is to move the template file, rather than linking it from its default location.</span></span> <span data-ttu-id="1e834-197">In tal caso, sarà necessario aggiornare la variabile **inputfile** nel modello per fornire il percorso relativo del file edmx (in questo esempio, che sarebbe **..\\BloggingModel. edmx**).</span><span class="sxs-lookup"><span data-stu-id="1e834-197">If you do this, you will need to update the **inputFile** variable in the template to provide the relative path to the edmx file (in this example that would be **..\\BloggingModel.edmx**).</span></span>

## <a name="create-a-wcf-service"></a><span data-ttu-id="1e834-198">Creazione di un servizio WCF</span><span class="sxs-lookup"><span data-stu-id="1e834-198">Create a WCF Service</span></span>

<span data-ttu-id="1e834-199">A questo punto è possibile aggiungere un servizio WCF per esporre i dati. si inizierà creando il progetto.</span><span class="sxs-lookup"><span data-stu-id="1e834-199">Now it's time to add a WCF Service to expose our data, we'll start by creating the project.</span></span>

-   <span data-ttu-id="1e834-200">**Progetto Add-&gt;&gt; file...**</span><span class="sxs-lookup"><span data-stu-id="1e834-200">**File -&gt; Add -&gt; Project...**</span></span>
-   <span data-ttu-id="1e834-201">Selezionare **Visual C\#** dal riquadro a sinistra e quindi **applicazione di servizio WCF**</span><span class="sxs-lookup"><span data-stu-id="1e834-201">Select **Visual C\#** from the left pane and then **WCF Service Application**</span></span>
-   <span data-ttu-id="1e834-202">Immettere **STESample. Service** come nome e fare clic su **OK** .</span><span class="sxs-lookup"><span data-stu-id="1e834-202">Enter **STESample.Service** as the name and click **OK**</span></span>
-   <span data-ttu-id="1e834-203">Aggiungere un riferimento all'assembly **System. Data. Entity**</span><span class="sxs-lookup"><span data-stu-id="1e834-203">Add a reference to the **System.Data.Entity** assembly</span></span>
-   <span data-ttu-id="1e834-204">Aggiungere un riferimento ai progetti **STESample** e **STESample. Entities**</span><span class="sxs-lookup"><span data-stu-id="1e834-204">Add a reference to the **STESample** and **STESample.Entities** projects</span></span>

<span data-ttu-id="1e834-205">È necessario copiare la stringa di connessione EF in questo progetto in modo che venga individuata in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="1e834-205">We need to copy the EF connection string to this project so that it is found at runtime.</span></span>

-   <span data-ttu-id="1e834-206">Aprire il file **app. config** per il progetto **STESample **e copiare l'elemento **connectionStrings**</span><span class="sxs-lookup"><span data-stu-id="1e834-206">Open the **App.Config** file for the **STESample **project and copy the **connectionStrings** element</span></span>
-   <span data-ttu-id="1e834-207">Incollare l'elemento **connectionStrings** come elemento figlio dell'elemento **Configuration** del file **Web. config** nel progetto **STESample. Service**</span><span class="sxs-lookup"><span data-stu-id="1e834-207">Paste the **connectionStrings** element as a child element of the **configuration** element of the **Web.Config** file in the **STESample.Service** project</span></span>

<span data-ttu-id="1e834-208">A questo punto è possibile implementare il servizio effettivo.</span><span class="sxs-lookup"><span data-stu-id="1e834-208">Now it's time to implement the actual service.</span></span>

-   <span data-ttu-id="1e834-209">Aprire **IService1.cs** e sostituire il contenuto con il codice seguente</span><span class="sxs-lookup"><span data-stu-id="1e834-209">Open **IService1.cs** and replace the contents with the following code</span></span>

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

-   <span data-ttu-id="1e834-210">Aprire **Service1. svc** e sostituire il contenuto con il codice seguente</span><span class="sxs-lookup"><span data-stu-id="1e834-210">Open **Service1.svc** and replace the contents with the following code</span></span>

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

## <a name="consume-the-service-from-a-console-application"></a><span data-ttu-id="1e834-211">Utilizzare il servizio da un'applicazione console</span><span class="sxs-lookup"><span data-stu-id="1e834-211">Consume the Service from a Console Application</span></span>

<span data-ttu-id="1e834-212">Viene ora creata un'applicazione console che usa il servizio.</span><span class="sxs-lookup"><span data-stu-id="1e834-212">Let's create a console application that uses our service.</span></span>

-   <span data-ttu-id="1e834-213">**Nuovo progetto&gt; di&gt; file...**</span><span class="sxs-lookup"><span data-stu-id="1e834-213">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="1e834-214">Selezionare **Visual C\#** dal riquadro a sinistra e quindi **applicazione console**</span><span class="sxs-lookup"><span data-stu-id="1e834-214">Select **Visual C\#** from the left pane and then **Console Application**</span></span>
-   <span data-ttu-id="1e834-215">Immettere **STESample. ConsoleTest** come nome e fare clic su **OK** .</span><span class="sxs-lookup"><span data-stu-id="1e834-215">Enter **STESample.ConsoleTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="1e834-216">Aggiungere un riferimento al progetto **STESample. Entities**</span><span class="sxs-lookup"><span data-stu-id="1e834-216">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="1e834-217">È necessario un riferimento al servizio WCF</span><span class="sxs-lookup"><span data-stu-id="1e834-217">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="1e834-218">Fare clic con il pulsante destro del mouse sul progetto **STESample. ConsoleTest** in **Esplora soluzioni** e selezionare **Aggiungi riferimento al servizio...**</span><span class="sxs-lookup"><span data-stu-id="1e834-218">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="1e834-219">Fare clic su **individua**</span><span class="sxs-lookup"><span data-stu-id="1e834-219">Click **Discover**</span></span>
-   <span data-ttu-id="1e834-220">Immettere **BloggingService** come spazio dei nomi e fare clic su **OK** .</span><span class="sxs-lookup"><span data-stu-id="1e834-220">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="1e834-221">A questo punto è possibile scrivere codice per utilizzare il servizio.</span><span class="sxs-lookup"><span data-stu-id="1e834-221">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="1e834-222">Aprire **Program.cs** e sostituire il contenuto con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="1e834-222">Open **Program.cs** and replace the contents with the following code.</span></span>

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

<span data-ttu-id="1e834-223">È ora possibile eseguire l'applicazione per vederla in azione.</span><span class="sxs-lookup"><span data-stu-id="1e834-223">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="1e834-224">Fare clic con il pulsante destro del mouse sul progetto **STESample. ConsoleTest** in **Esplora soluzioni** e selezionare **debug-&gt; avvia nuova istanza**</span><span class="sxs-lookup"><span data-stu-id="1e834-224">Right-click the **STESample.ConsoleTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>

<span data-ttu-id="1e834-225">Quando l'applicazione viene eseguita, verrà visualizzato il seguente output.</span><span class="sxs-lookup"><span data-stu-id="1e834-225">You'll see the following output when the application executes.</span></span>

```console
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

## <a name="consume-the-service-from-a-wpf-application"></a><span data-ttu-id="1e834-226">Utilizzare il servizio da un'applicazione WPF</span><span class="sxs-lookup"><span data-stu-id="1e834-226">Consume the Service from a WPF Application</span></span>

<span data-ttu-id="1e834-227">Viene ora creata un'applicazione WPF che usa il servizio.</span><span class="sxs-lookup"><span data-stu-id="1e834-227">Let's create a WPF application that uses our service.</span></span>

-   <span data-ttu-id="1e834-228">**Nuovo progetto&gt; di&gt; file...**</span><span class="sxs-lookup"><span data-stu-id="1e834-228">**File -&gt; New -&gt; Project...**</span></span>
-   <span data-ttu-id="1e834-229">Selezionare **Visual C\#** dal riquadro a sinistra e quindi **applicazione WPF**</span><span class="sxs-lookup"><span data-stu-id="1e834-229">Select **Visual C\#** from the left pane and then **WPF Application**</span></span>
-   <span data-ttu-id="1e834-230">Immettere **STESample. WPFTest** come nome e fare clic su **OK** .</span><span class="sxs-lookup"><span data-stu-id="1e834-230">Enter **STESample.WPFTest** as the name and click **OK**</span></span>
-   <span data-ttu-id="1e834-231">Aggiungere un riferimento al progetto **STESample. Entities**</span><span class="sxs-lookup"><span data-stu-id="1e834-231">Add a reference to the **STESample.Entities** project</span></span>

<span data-ttu-id="1e834-232">È necessario un riferimento al servizio WCF</span><span class="sxs-lookup"><span data-stu-id="1e834-232">We need a service reference to our WCF service</span></span>

-   <span data-ttu-id="1e834-233">Fare clic con il pulsante destro del mouse sul progetto **STESample. WPFTest** in **Esplora soluzioni** e selezionare **Aggiungi riferimento al servizio...**</span><span class="sxs-lookup"><span data-stu-id="1e834-233">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Add Service Reference...**</span></span>
-   <span data-ttu-id="1e834-234">Fare clic su **individua**</span><span class="sxs-lookup"><span data-stu-id="1e834-234">Click **Discover**</span></span>
-   <span data-ttu-id="1e834-235">Immettere **BloggingService** come spazio dei nomi e fare clic su **OK** .</span><span class="sxs-lookup"><span data-stu-id="1e834-235">Enter **BloggingService** as the namespace and click **OK**</span></span>

<span data-ttu-id="1e834-236">A questo punto è possibile scrivere codice per utilizzare il servizio.</span><span class="sxs-lookup"><span data-stu-id="1e834-236">Now we can write some code to consume the service.</span></span>

-   <span data-ttu-id="1e834-237">Aprire **MainWindow. XAML** e sostituire il contenuto con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="1e834-237">Open **MainWindow.xaml** and replace the contents with the following code.</span></span>

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

-   <span data-ttu-id="1e834-238">Aprire il code-behind per MainWindow (**MainWindow.XAML.cs**) e sostituire il contenuto con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="1e834-238">Open the code behind for MainWindow (**MainWindow.xaml.cs**) and replace the contents with the following code</span></span>

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

<span data-ttu-id="1e834-239">È ora possibile eseguire l'applicazione per vederla in azione.</span><span class="sxs-lookup"><span data-stu-id="1e834-239">You can now run the application to see it in action.</span></span>

-   <span data-ttu-id="1e834-240">Fare clic con il pulsante destro del mouse sul progetto **STESample. WPFTest** in **Esplora soluzioni** e selezionare **debug-&gt; avvia nuova istanza**</span><span class="sxs-lookup"><span data-stu-id="1e834-240">Right-click the **STESample.WPFTest** project in **Solution Explorer** and select **Debug -&gt; Start new instance**</span></span>
-   <span data-ttu-id="1e834-241">È possibile modificare i dati usando la schermata e salvarli tramite il servizio usando il pulsante **Salva**</span><span class="sxs-lookup"><span data-stu-id="1e834-241">You can manipulate the data using the screen and save it via the service using the **Save** button</span></span>

![Finestra principale WPF](~/ef6/media/wpf.png)
