---
title: Suddivisione di tabelle della finestra di progettazione - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: 87b6e1bd0374f77dfffab342c659cf4e16c8a337
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994503"
---
# <a name="designer-table-splitting"></a><span data-ttu-id="b3d16-102">Suddivisione di tabelle della finestra di progettazione</span><span class="sxs-lookup"><span data-stu-id="b3d16-102">Designer Table Splitting</span></span>
<span data-ttu-id="b3d16-103">Questa procedura dettagliata illustra come eseguire il mapping di più tipi di entità a una singola tabella modificando un modello con Entity Framework Designer (Entity Framework Designer).</span><span class="sxs-lookup"><span data-stu-id="b3d16-103">This walkthrough shows how to map multiple entity types to a single table by modifying a model with the Entity Framework Designer (EF Designer).</span></span>

<span data-ttu-id="b3d16-104">Un motivo per cui che è possibile usare suddivisione di tabelle sta ritardando il caricamento di alcune proprietà quando si usa per caricare gli oggetti di caricamento lazy.</span><span class="sxs-lookup"><span data-stu-id="b3d16-104">One reason you may want to use table splitting is delaying the loading of some properties when using lazy loading to load your objects.</span></span> <span data-ttu-id="b3d16-105">È possibile separare le proprietà che potrebbero contenere grandi quantità di dati in un'entità distinta e caricare solo quando richiesto.</span><span class="sxs-lookup"><span data-stu-id="b3d16-105">You can separate the properties that might contain very large amount of data into a seperate entity and only load it when required.</span></span>

<span data-ttu-id="b3d16-106">L'immagine seguente mostra le finestre principali che vengono usate quando si lavora con la finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b3d16-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="b3d16-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="b3d16-108">Prerequisites</span></span>

<span data-ttu-id="b3d16-109">Per completare questa procedura dettagliata, è necessario disporre di:</span><span class="sxs-lookup"><span data-stu-id="b3d16-109">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="b3d16-110">Una versione recente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b3d16-110">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="b3d16-111">Il [database di esempio School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="b3d16-111">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="b3d16-112">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="b3d16-112">Set up the Project</span></span>

<span data-ttu-id="b3d16-113">Questa procedura dettagliata Usa Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="b3d16-113">This walkthrough is using Visual Studio 2012.</span></span>

-   <span data-ttu-id="b3d16-114">Aprire Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="b3d16-114">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="b3d16-115">Scegliere **Nuovo** dal menu **File**, quindi fare clic su **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="b3d16-115">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="b3d16-116">Nel riquadro sinistro, fare clic su Visual C\#e quindi selezionare il modello di applicazione Console.</span><span class="sxs-lookup"><span data-stu-id="b3d16-116">In the left pane, click Visual C\#, and then select the Console Application template.</span></span>
-   <span data-ttu-id="b3d16-117">Immettere **TableSplittingSample** come nome del progetto e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3d16-117">Enter **TableSplittingSample** as the name of the project and click **OK**.</span></span>

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="b3d16-118">Creare un modello basato sul Database School</span><span class="sxs-lookup"><span data-stu-id="b3d16-118">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="b3d16-119">Fare clic sul nome del progetto in Esplora soluzioni, scegliere **Add**, quindi fare clic su **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="b3d16-119">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="b3d16-120">Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro dei modelli.</span><span class="sxs-lookup"><span data-stu-id="b3d16-120">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="b3d16-121">Immettere **TableSplittingModel.edmx** per il nome del file e quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="b3d16-121">Enter **TableSplittingModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="b3d16-122">Nella finestra di dialogo Scegli contenuto Model, selezionare **genera da database**, quindi fare clic su **successivo.**</span><span class="sxs-lookup"><span data-stu-id="b3d16-122">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="b3d16-123">Fare clic su nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="b3d16-123">Click New Connection.</span></span> <span data-ttu-id="b3d16-124">Nella finestra di dialogo proprietà di connessione, immettere il nome del server (ad esempio, **(localdb)\\mssqllocaldb**), selezionare il metodo di autenticazione, il tipo **School** per il nome del database e quindi Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3d16-124">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="b3d16-125">La finestra di dialogo Seleziona connessione dati viene aggiornata con l'impostazione di connessione di database.</span><span class="sxs-lookup"><span data-stu-id="b3d16-125">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="b3d16-126">Nella finestra di dialogo Scegli oggetti di Database, espandere la **tabelle** nodo e selezionare il **persona** tabella.</span><span class="sxs-lookup"><span data-stu-id="b3d16-126">In the Choose Your Database Objects dialog box, unfold the **Tables** node and check the **Person** table.</span></span> <span data-ttu-id="b3d16-127">Verrà aggiunta la tabella specificata per il **School** modello.</span><span class="sxs-lookup"><span data-stu-id="b3d16-127">This will add the specified table to the **School** model.</span></span>
-   <span data-ttu-id="b3d16-128">Scegliere **Fine**.</span><span class="sxs-lookup"><span data-stu-id="b3d16-128">Click **Finish**.</span></span>

<span data-ttu-id="b3d16-129">Entity Designer, che fornisce un'area di progettazione per la modifica del modello, viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="b3d16-129">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="b3d16-130">Tutti gli oggetti selezionati nella **Scegli oggetti di Database** nella finestra di dialogo vengono aggiunti al modello.</span><span class="sxs-lookup"><span data-stu-id="b3d16-130">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>

## <a name="map-two-entities-to-a-single-table"></a><span data-ttu-id="b3d16-131">Eseguire il mapping di due entità a una singola tabella</span><span class="sxs-lookup"><span data-stu-id="b3d16-131">Map Two Entities to a Single Table</span></span>

<span data-ttu-id="b3d16-132">In questa sezione suddivideranno i **persona** entità in due entità e quindi eseguirne il mapping a una singola tabella.</span><span class="sxs-lookup"><span data-stu-id="b3d16-132">In this section you will split the **Person** entity into two entities and then map them to a single table.</span></span>

> [!NOTE]
> <span data-ttu-id="b3d16-133">Il **persona** entità non contiene le proprietà che potrebbero contenere grandi quantità di dati; viene usata solo come esempio.</span><span class="sxs-lookup"><span data-stu-id="b3d16-133">The **Person** entity does not contain any properties that may contain large amount of data; it is just used as an example.</span></span>

-   <span data-ttu-id="b3d16-134">Fare doppio clic su un'area vuota dell'area di progettazione, scegliere **Aggiungi nuovo**, fare clic su **entità**.</span><span class="sxs-lookup"><span data-stu-id="b3d16-134">Right-click an empty area of the design surface, point to **Add New**, and click **Entity**.</span></span>
    <span data-ttu-id="b3d16-135">Il **nuova entità** verrà visualizzata la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="b3d16-135">The **New Entity** dialog box appears.</span></span>
-   <span data-ttu-id="b3d16-136">Tipo di **HireInfo** per il **nome entità** e **PersonID** per il **proprietà Key** nome.</span><span class="sxs-lookup"><span data-stu-id="b3d16-136">Type **HireInfo** for the **Entity name** and **PersonID** for the **Key Property** name.</span></span>
-   <span data-ttu-id="b3d16-137">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3d16-137">Click **OK**.</span></span>
-   <span data-ttu-id="b3d16-138">Nell'area di progettazione verrà creato e visualizzato un nuovo tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="b3d16-138">A new entity type is created and displayed on the design surface.</span></span>
-   <span data-ttu-id="b3d16-139">Selezionare il **HireDate** proprietà delle **persona** tipo di entità e premere **Ctrl + X** chiavi.</span><span class="sxs-lookup"><span data-stu-id="b3d16-139">Select the **HireDate** property of the **Person** entity type and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="b3d16-140">Selezionare il **HireInfo** entità e premere **Ctrl + V** chiavi.</span><span class="sxs-lookup"><span data-stu-id="b3d16-140">Select the **HireInfo** entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="b3d16-141">Creare un'associazione tra **Person** e **HireInfo**.</span><span class="sxs-lookup"><span data-stu-id="b3d16-141">Create an association between **Person** and **HireInfo**.</span></span> <span data-ttu-id="b3d16-142">A tale scopo, fare doppio clic su un'area vuota dell'area di progettazione, scegliere **Aggiungi nuovo**, fare clic su **Association**.</span><span class="sxs-lookup"><span data-stu-id="b3d16-142">To do this, right-click an empty area of the design surface, point to **Add New**, and click **Association**.</span></span>
-   <span data-ttu-id="b3d16-143">Il **Aggiungi associazione** verrà visualizzata la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="b3d16-143">The **Add Association** dialog box appears.</span></span> <span data-ttu-id="b3d16-144">Il **PersonHireInfo** viene specificato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="b3d16-144">The **PersonHireInfo** name is given by default.</span></span>
-   <span data-ttu-id="b3d16-145">Specificare la molteplicità **1(One)** a entrambe le estremità della relazione.</span><span class="sxs-lookup"><span data-stu-id="b3d16-145">Specify multiplicity **1(One)** on both ends of the relationship.</span></span>
-   <span data-ttu-id="b3d16-146">Premere **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3d16-146">Press **OK**.</span></span>

<span data-ttu-id="b3d16-147">Il passaggio successivo richiede la **Dettagli Mapping** finestra.</span><span class="sxs-lookup"><span data-stu-id="b3d16-147">The next step requires the **Mapping Details** window.</span></span> <span data-ttu-id="b3d16-148">Se non è possibile visualizzare questa finestra, fare doppio clic su area di progettazione e seleziona **Dettagli Mapping**.</span><span class="sxs-lookup"><span data-stu-id="b3d16-148">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="b3d16-149">Selezionare il **HireInfo** tipo di entità e fare clic su **&lt;aggiungere una tabella o vista&gt;** nel **Dettagli Mapping** finestra.</span><span class="sxs-lookup"><span data-stu-id="b3d16-149">Select the **HireInfo** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="b3d16-150">Selezionare **Person** dalle **&lt;aggiungere una tabella o vista&gt;** elenco campo.</span><span class="sxs-lookup"><span data-stu-id="b3d16-150">Select **Person** from the **&lt;Add a Table or View&gt;** field drop-down list.</span></span> <span data-ttu-id="b3d16-151">L'elenco contiene tabelle o visualizzazioni a cui può essere mappato l'entità selezionata.</span><span class="sxs-lookup"><span data-stu-id="b3d16-151">The list contains tables or views to which the selected entity can be mapped.</span></span>
    <span data-ttu-id="b3d16-152">Le proprietà appropriate devono eseguire il mapping per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="b3d16-152">The appropriate properties should be mapped by default.</span></span>

    ![Mapping](~/ef6/media/mapping.png)

-   <span data-ttu-id="b3d16-154">Selezionare il **PersonHireInfo** associazione nell'area di progettazione.</span><span class="sxs-lookup"><span data-stu-id="b3d16-154">Select the **PersonHireInfo** association on the design surface.</span></span>
-   <span data-ttu-id="b3d16-155">Pulsante destro del mouse su area di progettazione e seleziona l'associazione **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="b3d16-155">Right-click the association on the design surface and select **Properties**.</span></span>
-   <span data-ttu-id="b3d16-156">Nel **delle proprietà** finestra, seleziona la **vincoli referenziali** proprietà e fare clic sul pulsante dei puntini di sospensione.</span><span class="sxs-lookup"><span data-stu-id="b3d16-156">In the **Properties** window, select the **Referential Constraints** property and click the ellipses button.</span></span>
-   <span data-ttu-id="b3d16-157">Selezionare **Person** dalle **dell'entità** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="b3d16-157">Select **Person** from the **Principal** drop-down list.</span></span>
-   <span data-ttu-id="b3d16-158">Premere **OK**.</span><span class="sxs-lookup"><span data-stu-id="b3d16-158">Press **OK**.</span></span>

 

## <a name="use-the-model"></a><span data-ttu-id="b3d16-159">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="b3d16-159">Use the Model</span></span>

-   <span data-ttu-id="b3d16-160">Incollare il codice seguente nel metodo Main.</span><span class="sxs-lookup"><span data-stu-id="b3d16-160">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   <span data-ttu-id="b3d16-161">Compilare l'applicazione ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="b3d16-161">Compile and run the application.</span></span>

<span data-ttu-id="b3d16-162">Le istruzioni T-SQL seguenti sono state eseguite nel **School** database come risultato dell'esecuzione di questa applicazione.</span><span class="sxs-lookup"><span data-stu-id="b3d16-162">The following T-SQL statements were executed against the **School** database as a result of running this application.</span></span> 

-   <span data-ttu-id="b3d16-163">Quanto segue **Inserisci** è stata eseguita in seguito all'esecuzione rapida. SaveChanges () e che combina dati dal **Person** e **HireInfo** entità</span><span class="sxs-lookup"><span data-stu-id="b3d16-163">The following **INSERT** was executed as a result of executing context.SaveChanges() and combines data from the **Person** and **HireInfo** entities</span></span>

    ![INS](~/ef6/media/insert.png)

-   <span data-ttu-id="b3d16-165">Quanto segue **seleziona** è stata eseguita in seguito all'esecuzione rapida. People.FirstOrDefault() e seleziona solo le colonne di cui è stato eseguito il mapping a **persona**</span><span class="sxs-lookup"><span data-stu-id="b3d16-165">The following **SELECT** was executed as a result of executing context.People.FirstOrDefault() and selects just the columns mapped to **Person**</span></span>

    ![Select1](~/ef6/media/select1.png)

-   <span data-ttu-id="b3d16-167">Quanto segue **selezionate** è stato eseguito come risultato l'accesso a existingPerson.Instructor di proprietà di navigazione e consente di selezionare solo le colonne mappate a **HireInfo**</span><span class="sxs-lookup"><span data-stu-id="b3d16-167">The following **SELECT** was executed as a result of accessing the navigation property existingPerson.Instructor and selects just the columns mapped to **HireInfo**</span></span>

    ![Select2](~/ef6/media/select2.png)
