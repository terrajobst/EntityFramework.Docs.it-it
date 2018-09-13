---
title: Ereditarietà tabella per gerarchia della finestra di progettazione - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490123"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="3c7ad-102">Ereditarietà tabella per gerarchia della finestra di progettazione</span><span class="sxs-lookup"><span data-stu-id="3c7ad-102">Designer TPH Inheritance</span></span>
<span data-ttu-id="3c7ad-103">Questa procedura dettagliata viene illustrato come implementare l'ereditarietà di tabella per gerarchia (TPH) nel modello concettuale con Entity Framework Designer (Entity Framework Designer).</span><span class="sxs-lookup"><span data-stu-id="3c7ad-103">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="3c7ad-104">Ereditarietà tabella per gerarchia utilizza una tabella di database per gestire i dati per tutti i tipi di entità in una gerarchia di ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-104">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="3c7ad-105">In questa procedura dettagliata verrà eseguito il mapping della tabella Person ai tre tipi di entità: Person (tipo di base), (deriva da Person) Student e Instructor (deriva dalla persona).</span><span class="sxs-lookup"><span data-stu-id="3c7ad-105">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="3c7ad-106">Verrà creato un modello concettuale dal database (Database First) e quindi modificare il modello per implementare l'ereditarietà tabella per gerarchia usando la finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-106">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="3c7ad-107">È possibile eseguire il mapping a un'ereditarietà tabella per gerarchia utilizzando Model First, ma si sarebbe necessario scrivere il proprio database generazione del flusso di lavoro che è complesso.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-107">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="3c7ad-108">È quindi necessario assegnare questo flusso di lavoro di **flusso di lavoro di Database generazione** proprietà nella finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-108">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="3c7ad-109">Un'alternativa più semplice consiste nell'utilizzare Code First.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-109">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="3c7ad-110">Altre opzioni di ereditarietà</span><span class="sxs-lookup"><span data-stu-id="3c7ad-110">Other Inheritance Options</span></span>

<span data-ttu-id="3c7ad-111">Tabella per tipo TPT () è un altro tipo di ereditarietà in cui tabelle separate nel database vengono mappate alle entità che partecipano l'ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-111">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span>  <span data-ttu-id="3c7ad-112">Per informazioni su come eseguire il mapping dell'ereditarietà tabella per tipo con la finestra di progettazione di Entity Framework, vedere [TPT (ereditarietà) finestra di progettazione EF](~/ef6/modeling/designer/inheritance/tpt.md).</span><span class="sxs-lookup"><span data-stu-id="3c7ad-112">For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span></span>

<span data-ttu-id="3c7ad-113">Ereditarietà dei tipi di tabella per tipo concreto (TP) e modelli di ereditarietà misti sono supportati dal runtime di Entity Framework, ma non sono supportati dalla finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-113">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="3c7ad-114">Se si desidera utilizzare TPC o misto eredità, sono disponibili due opzioni: utilizzare Code First, oppure modificare manualmente i file con estensione EDMX.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="3c7ad-115">Se si sceglie di usare il file EDMX, nella finestra Dettagli Mapping verrà inserita nella "modalità provvisoria" e non sarà in grado di utilizzare la finestra di progettazione per modificare i mapping.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c7ad-116">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="3c7ad-116">Prerequisites</span></span>

<span data-ttu-id="3c7ad-117">Per completare questa procedura dettagliata, è necessario disporre di:</span><span class="sxs-lookup"><span data-stu-id="3c7ad-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="3c7ad-118">Una versione recente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="3c7ad-119">Il [database di esempio School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="3c7ad-119">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="3c7ad-120">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="3c7ad-120">Set up the Project</span></span>

-   <span data-ttu-id="3c7ad-121">Aprire Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="3c7ad-122">Selezionare **File -&gt; Novità -&gt; progetto**</span><span class="sxs-lookup"><span data-stu-id="3c7ad-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="3c7ad-123">Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **Console** modello.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="3c7ad-124">Immettere **TPHDBFirstSample** come nome.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-124">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="3c7ad-125">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="3c7ad-126">Creare un modello</span><span class="sxs-lookup"><span data-stu-id="3c7ad-126">Create a Model</span></span>

-   <span data-ttu-id="3c7ad-127">Il nome del progetto in Esplora soluzioni e scegliere **Add -&gt; nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-127">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="3c7ad-128">Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro dei modelli.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="3c7ad-129">Immettere **TPHModel.edmx** per il nome del file e quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-129">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="3c7ad-130">Nella finestra di dialogo Scegli contenuto Model, selezionare **genera da database**, quindi fare clic su **successivo**.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-130">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="3c7ad-131">Fare clic su **nuova connessione**.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-131">Click **New Connection**.</span></span>
    <span data-ttu-id="3c7ad-132">Nella finestra di dialogo proprietà di connessione, immettere il nome del server (ad esempio, **(localdb)\\mssqllocaldb**), selezionare il metodo di autenticazione, il tipo **School** per il nome del database e quindi Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="3c7ad-133">La finestra di dialogo Seleziona connessione dati viene aggiornata con l'impostazione di connessione di database.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="3c7ad-134">Nella finestra di dialogo Scegli oggetti di Database, sotto il nodo tabelle, selezionare la **persona** tabella.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="3c7ad-135">Scegliere **Fine**.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-135">Click **Finish**.</span></span>

<span data-ttu-id="3c7ad-136">Entity Designer, che fornisce un'area di progettazione per la modifica del modello, viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="3c7ad-137">Tutti gli oggetti selezionati nella finestra di dialogo Scegli oggetti di Database vengono aggiunti al modello.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="3c7ad-138">Vale a dire come il **persona** l'aspetto di tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-138">That is how the **Person** table looks in the database.</span></span>

![Tabella Person](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="3c7ad-140">Implementare l'ereditarietà tabella per gerarchia</span><span class="sxs-lookup"><span data-stu-id="3c7ad-140">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="3c7ad-141">Il **Person** tabella include le **discriminatore** colonna, che può avere uno dei due valori: "Student" e "Instructor".</span><span class="sxs-lookup"><span data-stu-id="3c7ad-141">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="3c7ad-142">A seconda del valore di **persona** verrà mappata alla tabella il **studente** entità o il **insegnante** entità.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-142">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="3c7ad-143">Il **Person** tabella include anche due colonne, **HireDate** e **EnrollmentDate**, che deve essere **nullable** perché non può essere una persona un studente e un insegnante nello stesso momento (almeno non in questa procedura dettagliata).</span><span class="sxs-lookup"><span data-stu-id="3c7ad-143">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="3c7ad-144">Aggiungi nuova entità</span><span class="sxs-lookup"><span data-stu-id="3c7ad-144">Add new Entities</span></span>

-   <span data-ttu-id="3c7ad-145">Aggiungere una nuova entità.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-145">Add a new entity.</span></span>
    <span data-ttu-id="3c7ad-146">A tale scopo, fare clic su uno spazio vuoto dell'area di progettazione di Entity Framework Designer e selezionare **Add -&gt;entità**.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-146">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="3c7ad-147">Tipo **insegnante** per il **nome entità** e selezionare **persona** nell'elenco a discesa per il **tipo Base**.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-147">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="3c7ad-148">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-148">Click **OK**.</span></span>
-   <span data-ttu-id="3c7ad-149">Aggiungere un'altra nuova entità.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-149">Add another new entity.</span></span> <span data-ttu-id="3c7ad-150">Tipo **studente** per il **nome entità** e selezionare **persona** nell'elenco a discesa per il **tipo Base**.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-150">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="3c7ad-151">Sono stati aggiunti due nuovi tipi di entità all'area di progettazione.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-151">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="3c7ad-152">Una freccia che punta dalla nuovi tipi di entità per il **Person** tipo di entità; ciò indica che **persona** è il tipo base per i nuovi tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-152">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="3c7ad-153">Fare doppio clic sui **HireDate** proprietà delle **persona** entità.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-153">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="3c7ad-154">Selezionare **Taglia** (oppure usare il tasto Ctrl-X).</span><span class="sxs-lookup"><span data-stu-id="3c7ad-154">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="3c7ad-155">Fare doppio clic il **insegnante** entità e selezionare **Incolla** (oppure usare il tasto Ctrl + V).</span><span class="sxs-lookup"><span data-stu-id="3c7ad-155">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="3c7ad-156">Fare doppio clic il **HireDate** proprietà e selezionare **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-156">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="3c7ad-157">Nel **le proprietà** impostare nella finestra di **Nullable** proprietà **false**.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-157">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="3c7ad-158">Fare doppio clic sui **EnrollmentDate** proprietà delle **persona** entità.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-158">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="3c7ad-159">Selezionare **Taglia** (oppure usare il tasto Ctrl-X).</span><span class="sxs-lookup"><span data-stu-id="3c7ad-159">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="3c7ad-160">Fare doppio clic il **studente** entità e selezionare **Incolla (o chiave di uso di Ctrl + V).**</span><span class="sxs-lookup"><span data-stu-id="3c7ad-160">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="3c7ad-161">Selezionare il **EnrollmentDate** proprietà e impostare le **Nullable** proprietà **false**.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-161">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="3c7ad-162">Selezionare il **persona** tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-162">Select the **Person** entity type.</span></span> <span data-ttu-id="3c7ad-163">Nel **le proprietà** impostare nella finestra relativa **astratto** proprietà **true**.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-163">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="3c7ad-164">Eliminare il **discriminatore** proprietà dal **persona**.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-164">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="3c7ad-165">Nella sezione seguente è illustrato il motivo che deve essere eliminata.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-165">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="3c7ad-166">Eseguire il mapping di entità</span><span class="sxs-lookup"><span data-stu-id="3c7ad-166">Map the entities</span></span>

-   <span data-ttu-id="3c7ad-167">Fare doppio clic il **insegnante** e selezionare **Mapping della tabella.**</span><span class="sxs-lookup"><span data-stu-id="3c7ad-167">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="3c7ad-168">L'entità Instructor è selezionata nella finestra Dettagli Mapping.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-168">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="3c7ad-169">Fare clic su **&lt;aggiungere una tabella o vista&gt;** nel **Dettagli Mapping** finestra.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-169">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="3c7ad-170">Il **&lt;aggiungere una tabella o vista&gt;** campo diventa un elenco a discesa di tabelle o visualizzazioni a cui può essere mappato l'entità selezionata.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-170">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="3c7ad-171">Selezionare **persona** nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-171">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="3c7ad-172">Il **Dettagli Mapping** finestra viene aggiornata con i mapping delle colonne predefinite e un'opzione per l'aggiunta di una condizione.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-172">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="3c7ad-173">Fare clic su  **&lt;Aggiungi una condizione&gt;**.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-173">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="3c7ad-174">Il **&lt;Aggiungi una condizione&gt;** campo diventa un elenco a discesa delle colonne per cui è possibile impostare le condizioni.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-174">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="3c7ad-175">Selezionare **discriminatore** nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-175">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="3c7ad-176">Nel **operatore** della colonna della **Dettagli Mapping** finestra selezionare = nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-176">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="3c7ad-177">Nel **valore/proprietà** colonna, digitare **insegnante**.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-177">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="3c7ad-178">Il risultato finale dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="3c7ad-178">The end result should look like this:</span></span>

    ![Dettagli di mapping](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="3c7ad-180">Ripetere questi passaggi per la **studente** tipo di entità, ma la condizione uguale alla marca **studente** valore.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-180">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="3c7ad-181">*Il motivo, desiderassimo per rimuovere il **discriminatore** proprietà, è perché non è possibile associare più di una volta una colonna di tabella. Questa colonna verrà utilizzata per eseguire il mapping condizionale, pertanto non può essere usato per il mapping anche di proprietà. L'unico modo può essere utilizzato per entrambi, se una condizione Usa un' **Is Null** oppure **Is Not Null** confronto.*</span><span class="sxs-lookup"><span data-stu-id="3c7ad-181">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="3c7ad-182">A questo punto l'ereditarietà tabella per gerarchia risulta implementata.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-182">Table-per-hierarchy inheritance is now implemented.</span></span>

![Finale della tabella per gerarchia](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="3c7ad-184">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="3c7ad-184">Use the Model</span></span>

<span data-ttu-id="3c7ad-185">Aprire il **Program.cs** file dove il **Main** metodo è definito.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-185">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="3c7ad-186">Incollare il codice seguente nel **Main** (funzione).</span><span class="sxs-lookup"><span data-stu-id="3c7ad-186">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="3c7ad-187">Il codice esegue tre query.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-187">The code executes three queries.</span></span> <span data-ttu-id="3c7ad-188">La prima query restituisce tutti i **persona** oggetti.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-188">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="3c7ad-189">La seconda query Usa la **OfType** metodo restituisca **insegnante** oggetti.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-189">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="3c7ad-190">La terza query Usa la **OfType** metodo restituisca **studente** oggetti.</span><span class="sxs-lookup"><span data-stu-id="3c7ad-190">The third query uses the **OfType** method to return **Student** objects.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
