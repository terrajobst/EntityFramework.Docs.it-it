---
title: Separazione delle entità della finestra di progettazione - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: ba1895ae491cec909ff88a8784eea82f1876f595
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490851"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="a4453-102">Suddivisione di entità della finestra di progettazione</span><span class="sxs-lookup"><span data-stu-id="a4453-102">Designer Entity Splitting</span></span>
<span data-ttu-id="a4453-103">Questa procedura dettagliata illustra come eseguire il mapping di un tipo di entità a due tabelle modificando un modello con Entity Framework Designer (Entity Framework Designer).</span><span class="sxs-lookup"><span data-stu-id="a4453-103">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="a4453-104">È possibile eseguire il mapping di un'entità a più tabelle quando le tabelle in questione condividono una chiave comune.</span><span class="sxs-lookup"><span data-stu-id="a4453-104">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="a4453-105">I concetti relativi all'esecuzione del mapping di un tipo di entità a due tabelle possono essere facilmente estesi anche all'esecuzione del mapping a più di due tabelle.</span><span class="sxs-lookup"><span data-stu-id="a4453-105">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="a4453-106">L'immagine seguente mostra le finestre principali che vengono usate quando si lavora con la finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a4453-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="a4453-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="a4453-108">Prerequisites</span></span>

<span data-ttu-id="a4453-109">Edizione Visual Studio 2012 o Visual Studio 2010, Ultimate, Premium, Professional o Web Express.</span><span class="sxs-lookup"><span data-stu-id="a4453-109">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="a4453-110">Creare il Database</span><span class="sxs-lookup"><span data-stu-id="a4453-110">Create the Database</span></span>

<span data-ttu-id="a4453-111">Il server di database che viene installato con Visual Studio è diverso a seconda della versione di Visual Studio installata:</span><span class="sxs-lookup"><span data-stu-id="a4453-111">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="a4453-112">Se si usa Visual Studio 2012, quindi si creerà un database LocalDB.</span><span class="sxs-lookup"><span data-stu-id="a4453-112">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="a4453-113">Se si usa Visual Studio 2010 si creerà un database SQL Express.</span><span class="sxs-lookup"><span data-stu-id="a4453-113">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="a4453-114">Prima di tutto si creerà un database con due tabelle che si intende combinare in una singola entità.</span><span class="sxs-lookup"><span data-stu-id="a4453-114">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="a4453-115">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a4453-115">Open Visual Studio</span></span>
-   <span data-ttu-id="a4453-116">**Visualizzazione -&gt; Esplora Server**</span><span class="sxs-lookup"><span data-stu-id="a4453-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="a4453-117">Fare clic con il pulsante destro sul **connessioni dati -&gt; Aggiungi connessione...**</span><span class="sxs-lookup"><span data-stu-id="a4453-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="a4453-118">Se si è ancora connessi a un database da Esplora Server prima che è necessario selezionare **Microsoft SQL Server** come origine dati</span><span class="sxs-lookup"><span data-stu-id="a4453-118">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="a4453-119">Connettersi a LocalDB o SQL Express, in base alla quale è stato installato</span><span class="sxs-lookup"><span data-stu-id="a4453-119">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="a4453-120">Immettere **EntitySplitting** come nome del database</span><span class="sxs-lookup"><span data-stu-id="a4453-120">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="a4453-121">Selezionare **OK** e verrà richiesto se si desidera creare un nuovo database, selezionare **Sì**</span><span class="sxs-lookup"><span data-stu-id="a4453-121">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="a4453-122">Il nuovo database verrà ora visualizzato in Esplora Server</span><span class="sxs-lookup"><span data-stu-id="a4453-122">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="a4453-123">Se si usa Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="a4453-123">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="a4453-124">Fare doppio clic sul database in Esplora Server e selezionare **nuova Query**</span><span class="sxs-lookup"><span data-stu-id="a4453-124">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="a4453-125">Copiare il codice SQL seguente nella nuova query, quindi fare clic su query e selezionare **Execute**</span><span class="sxs-lookup"><span data-stu-id="a4453-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="a4453-126">Se si usa Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="a4453-126">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="a4453-127">Selezionare **- Data&gt; Transact SQL Editor -&gt; nuova connessione Query...**</span><span class="sxs-lookup"><span data-stu-id="a4453-127">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="a4453-128">Immettere **.\\ SQLEXPRESS** come nome del server e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="a4453-128">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="a4453-129">Selezionare il **EntitySplitting** database dall'elenco a discesa nella parte superiore dell'editor di query</span><span class="sxs-lookup"><span data-stu-id="a4453-129">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="a4453-130">Copiare il codice SQL seguente nella nuova query, quindi fare clic su query e selezionare **Esegui SQL**</span><span class="sxs-lookup"><span data-stu-id="a4453-130">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a><span data-ttu-id="a4453-131">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="a4453-131">Create the Project</span></span>

-   <span data-ttu-id="a4453-132">Scegliere **Nuovo** dal menu **File**, quindi fare clic su **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="a4453-132">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="a4453-133">Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **applicazione Console** modello.</span><span class="sxs-lookup"><span data-stu-id="a4453-133">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="a4453-134">Immettere **MapEntityToTablesSample** come nome del progetto e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a4453-134">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="a4453-135">Fare clic su **No** se viene richiesto di salvare la query SQL creata nella prima sezione.</span><span class="sxs-lookup"><span data-stu-id="a4453-135">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="a4453-136">Creare un modello basato sul Database</span><span class="sxs-lookup"><span data-stu-id="a4453-136">Create a Model based on the Database</span></span>

-   <span data-ttu-id="a4453-137">Fare clic sul nome del progetto in Esplora soluzioni, scegliere **Add**, quindi fare clic su **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="a4453-137">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="a4453-138">Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro dei modelli.</span><span class="sxs-lookup"><span data-stu-id="a4453-138">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="a4453-139">Immettere **MapEntityToTablesModel.edmx** per il nome del file e quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="a4453-139">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="a4453-140">Nella finestra di dialogo Scegli contenuto Model, selezionare **genera da database**, quindi fare clic su **successivo.**</span><span class="sxs-lookup"><span data-stu-id="a4453-140">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="a4453-141">Selezionare il **EntitySplitting** connessione nell'elenco a discesa e fare clic su **successivo**.</span><span class="sxs-lookup"><span data-stu-id="a4453-141">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="a4453-142">Nella finestra di dialogo Scegli oggetti di Database, selezionare la casella accanto al **tabelle** nodo.</span><span class="sxs-lookup"><span data-stu-id="a4453-142">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="a4453-143">Tutte le tabelle da verrà aggiunto il **EntitySplitting** database al modello.</span><span class="sxs-lookup"><span data-stu-id="a4453-143">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="a4453-144">Scegliere **Fine**.</span><span class="sxs-lookup"><span data-stu-id="a4453-144">Click **Finish**.</span></span>

<span data-ttu-id="a4453-145">Entity Designer, che fornisce un'area di progettazione per la modifica del modello, viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="a4453-145">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="a4453-146">Eseguire il mapping di un'entità a due tabelle</span><span class="sxs-lookup"><span data-stu-id="a4453-146">Map an Entity to Two Tables</span></span>

<span data-ttu-id="a4453-147">In questo passaggio si aggiornerà il **Person** tipo di entità per combinare dati provenienti dal **persona** e **PersonInfo** tabelle.</span><span class="sxs-lookup"><span data-stu-id="a4453-147">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="a4453-148">Selezionare il **messaggio di posta elettronica** e **Phone** le proprietà del \* \* PersonInfo \* \* entità, quindi premere **Ctrl + X** chiavi.</span><span class="sxs-lookup"><span data-stu-id="a4453-148">Select the **Email** and **Phone** properties of the \*\*PersonInfo \*\*entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="a4453-149">Selezionare la \* \* persona \* \* entity e premere **Ctrl + V** chiavi.</span><span class="sxs-lookup"><span data-stu-id="a4453-149">Select the \*\*Person \*\*entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="a4453-150">Nell'area di progettazione, selezionare la **PersonInfo** entità e premere **eliminare** pulsante sulla tastiera.</span><span class="sxs-lookup"><span data-stu-id="a4453-150">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="a4453-151">Fare clic su **No** quando viene richiesto se si desidera rimuovere il **PersonInfo** tabella dal modello, si sta tentando di eseguire il mapping al **persona** entità.</span><span class="sxs-lookup"><span data-stu-id="a4453-151">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![Eliminare le tabelle](~/ef6/media/deletetables.png)

<span data-ttu-id="a4453-153">I passaggi successivi presuppongono il **Dettagli Mapping** finestra.</span><span class="sxs-lookup"><span data-stu-id="a4453-153">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="a4453-154">Se non è possibile visualizzare questa finestra, fare doppio clic su area di progettazione e seleziona **Dettagli Mapping**.</span><span class="sxs-lookup"><span data-stu-id="a4453-154">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="a4453-155">Selezionare il **Person** tipo di entità e fare clic su **&lt;aggiungere una tabella o vista&gt;** nel **Dettagli Mapping** finestra.</span><span class="sxs-lookup"><span data-stu-id="a4453-155">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="a4453-156">Selezionare \* \* PersonInfo \* \* nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="a4453-156">Select \*\*PersonInfo \*\* from the drop-down list.</span></span>
    <span data-ttu-id="a4453-157">Il **Dettagli Mapping** finestra viene aggiornata con i mapping delle colonne predefinite, queste sono accettabili per questo scenario.</span><span class="sxs-lookup"><span data-stu-id="a4453-157">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="a4453-158">Il **Person** tipo di entità è stato mappato il **persona** e **PersonInfo** tabelle.</span><span class="sxs-lookup"><span data-stu-id="a4453-158">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![Mapping 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="a4453-160">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="a4453-160">Use the Model</span></span>

-   <span data-ttu-id="a4453-161">Incollare il codice seguente nel metodo Main.</span><span class="sxs-lookup"><span data-stu-id="a4453-161">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   <span data-ttu-id="a4453-162">Compilare l'applicazione ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="a4453-162">Compile and run the application.</span></span>

<span data-ttu-id="a4453-163">Le istruzioni T-SQL seguenti sono state eseguite sul database come risultato dell'esecuzione di questa applicazione.</span><span class="sxs-lookup"><span data-stu-id="a4453-163">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="a4453-164">I seguenti due **Inserisci** le istruzioni sono state eseguite in seguito all'esecuzione rapida. SaveChanges ().</span><span class="sxs-lookup"><span data-stu-id="a4453-164">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="a4453-165">Adottano i dati dal **Person** entità e la divisione tra la **persona** e **PersonInfo** tabelle.</span><span class="sxs-lookup"><span data-stu-id="a4453-165">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![Inserisci 1](~/ef6/media/insert1.png)

    ![Inserisci 2](~/ef6/media/insert2.png)
-   <span data-ttu-id="a4453-168">Quanto segue **seleziona** è stata eseguita come risultato l'enumerazione degli utenti del database.</span><span class="sxs-lookup"><span data-stu-id="a4453-168">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="a4453-169">Combina i dati di **Person** e **PersonInfo** tabella.</span><span class="sxs-lookup"><span data-stu-id="a4453-169">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![Seleziona](~/ef6/media/select.png)
