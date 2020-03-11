---
title: Ereditarietà TPH della finestra di progettazione-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418427"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="a0033-102">Ereditarietà TPH della finestra di progettazione</span><span class="sxs-lookup"><span data-stu-id="a0033-102">Designer TPH Inheritance</span></span>
<span data-ttu-id="a0033-103">Questa procedura dettagliata illustra come implementare l'ereditarietà tabella per gerarchia (TPH) nel modello concettuale con il Entity Framework Designer (EF designer).</span><span class="sxs-lookup"><span data-stu-id="a0033-103">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="a0033-104">L'ereditarietà TPH utilizza una tabella di database per gestire i dati per tutti i tipi di entità in una gerarchia di ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="a0033-104">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="a0033-105">In questa procedura dettagliata si eseguirà il mapping della tabella Person a tre tipi di entità: Person (il tipo di base), Student (deriva da Person) e Instructor (deriva da Person).</span><span class="sxs-lookup"><span data-stu-id="a0033-105">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="a0033-106">Si creerà un modello concettuale dal database (Database First), quindi si modificherà il modello per implementare l'ereditarietà TPH usando la finestra di progettazione EF.</span><span class="sxs-lookup"><span data-stu-id="a0033-106">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="a0033-107">È possibile eseguire il mapping a un'ereditarietà TPH usando Model First ma è necessario scrivere un flusso di lavoro di generazione del database personalizzato, che è complesso.</span><span class="sxs-lookup"><span data-stu-id="a0033-107">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="a0033-108">Il flusso di lavoro verrà quindi assegnato alla proprietà del **flusso di lavoro di generazione del database** nella finestra di progettazione EF.</span><span class="sxs-lookup"><span data-stu-id="a0033-108">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="a0033-109">Un'alternativa più semplice consiste nell'usare Code First.</span><span class="sxs-lookup"><span data-stu-id="a0033-109">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="a0033-110">Altre opzioni di ereditarietà</span><span class="sxs-lookup"><span data-stu-id="a0033-110">Other Inheritance Options</span></span>

<span data-ttu-id="a0033-111">Table-for-Type (TPT) è un altro tipo di ereditarietà in cui viene eseguito il mapping delle tabelle separate del database a entità che partecipano all'ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="a0033-111">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span> <span data-ttu-id="a0033-112"> Per informazioni su come eseguire il mapping dell'ereditarietà tabella per tipo con EF designer, vedere [EF designer TPT ereditarietà](~/ef6/modeling/designer/inheritance/tpt.md).</span><span class="sxs-lookup"><span data-stu-id="a0033-112"> For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span></span>

<span data-ttu-id="a0033-113">I modelli di ereditarietà della tabella per tipo concreto (TPC) e di ereditarietà mista sono supportati dal runtime di Entity Framework ma non sono supportati dalla finestra di progettazione EF.</span><span class="sxs-lookup"><span data-stu-id="a0033-113">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="a0033-114">Se si vuole usare TPC o l'ereditarietà mista, sono disponibili due opzioni: usare Code First o modificare manualmente il file EDMX.</span><span class="sxs-lookup"><span data-stu-id="a0033-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="a0033-115">Se si sceglie di utilizzare il file EDMX, la finestra Dettagli mapping verrà inserita in "modalità provvisoria" e non sarà possibile utilizzare la finestra di progettazione per modificare i mapping.</span><span class="sxs-lookup"><span data-stu-id="a0033-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a0033-116">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="a0033-116">Prerequisites</span></span>

<span data-ttu-id="a0033-117">Per completare questa procedura dettagliata, è necessario disporre di:</span><span class="sxs-lookup"><span data-stu-id="a0033-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="a0033-118">Una versione recente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a0033-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="a0033-119">[Database di esempio School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="a0033-119">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="a0033-120">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="a0033-120">Set up the Project</span></span>

-   <span data-ttu-id="a0033-121">Aprire Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="a0033-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="a0033-122">Seleziona **file-&gt; progetto nuovo-&gt;**</span><span class="sxs-lookup"><span data-stu-id="a0033-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="a0033-123">Nel riquadro sinistro fare clic su **Visual C\#** , quindi selezionare il modello **console** .</span><span class="sxs-lookup"><span data-stu-id="a0033-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="a0033-124">Immettere **TPHDBFirstSample** come nome.</span><span class="sxs-lookup"><span data-stu-id="a0033-124">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="a0033-125">Selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="a0033-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="a0033-126">Creare il modello</span><span class="sxs-lookup"><span data-stu-id="a0033-126">Create a Model</span></span>

-   <span data-ttu-id="a0033-127">Fare clic con il pulsante destro del mouse sul nome del progetto in Esplora soluzioni, quindi scegliere **aggiungi&gt; nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="a0033-127">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="a0033-128">Selezionare **dati** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro modelli.</span><span class="sxs-lookup"><span data-stu-id="a0033-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="a0033-129">Immettere **TPHModel. edmx** per il nome del file e quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="a0033-129">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="a0033-130">Nella finestra di dialogo Scegli contenuto Model selezionare **genera da database**, quindi fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="a0033-130">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="a0033-131">Fare clic su **nuova connessione**.</span><span class="sxs-lookup"><span data-stu-id="a0033-131">Click **New Connection**.</span></span>
    <span data-ttu-id="a0033-132">Nella finestra di dialogo Proprietà connessione immettere il nome del server (ad esempio, **(local DB)\\mssqllocaldb**), selezionare il metodo di autenticazione, digitare **School** per il nome del database, quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a0033-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="a0033-133">La finestra di dialogo scegliere la connessione dati viene aggiornata con l'impostazione di connessione al database.</span><span class="sxs-lookup"><span data-stu-id="a0033-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="a0033-134">Nella finestra di dialogo Scegli oggetti di database, nel nodo tabelle, selezionare la tabella **Person** .</span><span class="sxs-lookup"><span data-stu-id="a0033-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="a0033-135">Fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="a0033-135">Click **Finish**.</span></span>

<span data-ttu-id="a0033-136">Viene visualizzata la Entity Designer, che fornisce un'area di progettazione per la modifica del modello.</span><span class="sxs-lookup"><span data-stu-id="a0033-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="a0033-137">Tutti gli oggetti selezionati nella finestra di dialogo Scegli oggetti di database vengono aggiunti al modello.</span><span class="sxs-lookup"><span data-stu-id="a0033-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="a0033-138">Questo è il modo in cui la tabella **Person** Cerca nel database.</span><span class="sxs-lookup"><span data-stu-id="a0033-138">That is how the **Person** table looks in the database.</span></span>

![Tabella Person](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="a0033-140">Implementare l'ereditarietà tabella per gerarchia</span><span class="sxs-lookup"><span data-stu-id="a0033-140">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="a0033-141">La tabella **Person** contiene la colonna **Discriminator** , che può avere uno dei due valori seguenti: "Student" e "Instructor".</span><span class="sxs-lookup"><span data-stu-id="a0033-141">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="a0033-142">A seconda del valore della tabella **Person** verrà eseguito il mapping all'entità **Student** o all'entità **Instructor** .</span><span class="sxs-lookup"><span data-stu-id="a0033-142">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="a0033-143">La tabella **Person** include anche due colonne, **Hired** e **EnrollmentDate**, che devono **ammettere i valori null** perché una persona non può essere uno studente e un insegnante nello stesso momento, almeno non in questa procedura dettagliata.</span><span class="sxs-lookup"><span data-stu-id="a0033-143">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="a0033-144">Aggiungi nuove entità</span><span class="sxs-lookup"><span data-stu-id="a0033-144">Add new Entities</span></span>

-   <span data-ttu-id="a0033-145">Aggiungere una nuova entità.</span><span class="sxs-lookup"><span data-stu-id="a0033-145">Add a new entity.</span></span>
    <span data-ttu-id="a0033-146">A tale scopo, fare clic con il pulsante destro del mouse su uno spazio vuoto dell'area di progettazione dell'Entity Framework Designer e selezionare **aggiungi&gt;entità**.</span><span class="sxs-lookup"><span data-stu-id="a0033-146">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="a0033-147">Digitare **Instructor** per il **nome dell'entità** e selezionare **Person** dall'elenco a discesa per il **tipo di base**.</span><span class="sxs-lookup"><span data-stu-id="a0033-147">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="a0033-148">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="a0033-148">Click **OK**.</span></span>
-   <span data-ttu-id="a0033-149">Aggiungere un'altra nuova entità.</span><span class="sxs-lookup"><span data-stu-id="a0033-149">Add another new entity.</span></span> <span data-ttu-id="a0033-150">Digitare **Student** per il **nome dell'entità** e selezionare **Person** dall'elenco a discesa per il **tipo di base**.</span><span class="sxs-lookup"><span data-stu-id="a0033-150">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="a0033-151">Due nuovi tipi di entità sono stati aggiunti all'area di progettazione.</span><span class="sxs-lookup"><span data-stu-id="a0033-151">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="a0033-152">Una freccia punta dai nuovi tipi di entità alla **persona** tipo di entità; indica che **Person** è il tipo di base per i nuovi tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="a0033-152">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="a0033-153">Fare clic con il pulsante destro del mouse sulla proprietà di **assunzione** dell'entità  **persona** .</span><span class="sxs-lookup"><span data-stu-id="a0033-153">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="a0033-154">Selezionare **taglia** (oppure usare il tasto CTRL + X).</span><span class="sxs-lookup"><span data-stu-id="a0033-154">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="a0033-155">Fare clic con il pulsante destro del mouse sull'entità **Instructor** e selezionare **Incolla** (oppure utilizzare il tasto CTRL + V).</span><span class="sxs-lookup"><span data-stu-id="a0033-155">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="a0033-156">Fare clic con il pulsante destro del mouse sulla proprietà di **assunzione** e selezionare **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="a0033-156">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="a0033-157">Nella finestra **proprietà** impostare la proprietà **Nullable** su **false**.</span><span class="sxs-lookup"><span data-stu-id="a0033-157">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="a0033-158">Fare clic con il pulsante destro del mouse sulla proprietà **EnrollmentDate** dell'entità **Person** .</span><span class="sxs-lookup"><span data-stu-id="a0033-158">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="a0033-159">Selezionare **taglia** (oppure usare il tasto CTRL + X).</span><span class="sxs-lookup"><span data-stu-id="a0033-159">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="a0033-160">Fare clic con il pulsante destro del mouse sull'entità **Student** e selezionare **Incolla (oppure utilizzare il tasto CTRL + V).**</span><span class="sxs-lookup"><span data-stu-id="a0033-160">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="a0033-161">Selezionare la proprietà **EnrollmentDate** e impostare la proprietà **Nullable** su **false**.</span><span class="sxs-lookup"><span data-stu-id="a0033-161">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="a0033-162">Selezionare il tipo di entità **Person** .</span><span class="sxs-lookup"><span data-stu-id="a0033-162">Select the **Person** entity type.</span></span> <span data-ttu-id="a0033-163">Nella finestra **proprietà** impostare la proprietà **abstract** su **true**.</span><span class="sxs-lookup"><span data-stu-id="a0033-163">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="a0033-164">Elimina la proprietà **Discriminator** da **Person**.</span><span class="sxs-lookup"><span data-stu-id="a0033-164">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="a0033-165">Il motivo per cui deve essere eliminato è illustrato nella sezione seguente.</span><span class="sxs-lookup"><span data-stu-id="a0033-165">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="a0033-166">Eseguire il mapping delle entità</span><span class="sxs-lookup"><span data-stu-id="a0033-166">Map the entities</span></span>

-   <span data-ttu-id="a0033-167">Fare clic con il pulsante destro del mouse sull' **insegnante** e scegliere **mapping tabella.**</span><span class="sxs-lookup"><span data-stu-id="a0033-167">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="a0033-168">L'entità Instructor è selezionata nella finestra Dettagli mapping.</span><span class="sxs-lookup"><span data-stu-id="a0033-168">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="a0033-169">Fare clic su **&lt;aggiungere una tabella o una vista&gt;**  nella finestra  **Dettagli mapping** .</span><span class="sxs-lookup"><span data-stu-id="a0033-169">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="a0033-170">Il **&lt;aggiungere una tabella o una vista&gt;** campo diventa un elenco a discesa di tabelle o viste a cui è possibile eseguire il mapping dell'entità selezionata.</span><span class="sxs-lookup"><span data-stu-id="a0033-170">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="a0033-171">Selezionare **Person** dall'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="a0033-171">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="a0033-172">La finestra **Dettagli mapping** viene aggiornata con i mapping di colonna predefiniti e un'opzione per l'aggiunta di una condizione.</span><span class="sxs-lookup"><span data-stu-id="a0033-172">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="a0033-173">Fare clic su **&lt;aggiungere una condizione&gt;** .</span><span class="sxs-lookup"><span data-stu-id="a0033-173">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="a0033-174">Il **&lt;Aggiungi una condizione&gt;**  campo diventa un elenco a discesa di colonne per le quali è possibile impostare le condizioni.</span><span class="sxs-lookup"><span data-stu-id="a0033-174">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="a0033-175">Selezionare  **discriminatore** dall'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="a0033-175">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="a0033-176">Nella colonna **operatore** della finestra **Dettagli mapping** selezionare = nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="a0033-176">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="a0033-177">Nella colonna **valore/proprietà** digitare **Instructor**.</span><span class="sxs-lookup"><span data-stu-id="a0033-177">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="a0033-178">Il risultato finale dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="a0033-178">The end result should look like this:</span></span>

    ![Dettagli mapping](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="a0033-180">Ripetere questi passaggi per il tipo di entità **student** , ma fare in modo che la condizione sia uguale al valore **Student** .</span><span class="sxs-lookup"><span data-stu-id="a0033-180">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="a0033-181">*Il motivo per cui si voleva rimuovere la proprietà **Discriminator** è che non è possibile eseguire il mapping di una colonna di tabella più di una volta. Questa colonna verrà usata per il mapping condizionale, pertanto non può essere usata anche per il mapping delle proprietà. L'unico modo in cui può essere usato per entrambi, se una condizione usa un **è null** o **non è null** il confronto.*</span><span class="sxs-lookup"><span data-stu-id="a0033-181">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="a0033-182">A questo punto l'ereditarietà tabella per gerarchia risulta implementata.</span><span class="sxs-lookup"><span data-stu-id="a0033-182">Table-per-hierarchy inheritance is now implemented.</span></span>

![TPH finale](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="a0033-184">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="a0033-184">Use the Model</span></span>

<span data-ttu-id="a0033-185">Aprire il file **Program.cs** in cui è definito il metodo **Main** .</span><span class="sxs-lookup"><span data-stu-id="a0033-185">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="a0033-186">Incollare il codice seguente nella funzione **Main** .</span><span class="sxs-lookup"><span data-stu-id="a0033-186">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="a0033-187">Il codice esegue tre query.</span><span class="sxs-lookup"><span data-stu-id="a0033-187">The code executes three queries.</span></span> <span data-ttu-id="a0033-188">La prima query riporta tutti gli oggetti **Person** .</span><span class="sxs-lookup"><span data-stu-id="a0033-188">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="a0033-189">La seconda query usa il metodo **OfType** per restituire gli oggetti **Instructor** .</span><span class="sxs-lookup"><span data-stu-id="a0033-189">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="a0033-190">La terza query usa il metodo **OfType** per restituire gli oggetti **Student** .</span><span class="sxs-lookup"><span data-stu-id="a0033-190">The third query uses the **OfType** method to return **Student** objects.</span></span>

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
