---
title: Tipi complessi-EF designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: a3c5578acee23688c04772d2dd0a2221779af562
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78418651"
---
# <a name="complex-types---ef-designer"></a><span data-ttu-id="c4285-102">Tipi complessi-EF designer</span><span class="sxs-lookup"><span data-stu-id="c4285-102">Complex Types - EF Designer</span></span>
<span data-ttu-id="c4285-103">In questo argomento viene illustrato come eseguire il mapping di tipi complessi con il Entity Framework Designer (Entity Designer) e come eseguire una query per le entità che contengono proprietà di tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="c4285-103">This topic shows how to map complex types with the Entity Framework Designer (EF Designer) and how to query for entities that contain properties of complex type.</span></span>

<span data-ttu-id="c4285-104">Nell'immagine seguente vengono illustrate le finestre principali che vengono usate quando si usa la finestra di progettazione EF.</span><span class="sxs-lookup"><span data-stu-id="c4285-104">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF Designer](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="c4285-106">Quando si compila il modello concettuale, è possibile che in Elenco errori vengano visualizzati avvisi riguardanti entità e associazioni non mappate.</span><span class="sxs-lookup"><span data-stu-id="c4285-106">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="c4285-107">È possibile ignorare questi avvisi perché, dopo aver scelto di generare il database dal modello, gli errori verranno rilasciati.</span><span class="sxs-lookup"><span data-stu-id="c4285-107">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="what-is-a-complex-type"></a><span data-ttu-id="c4285-108">Che cos'è un tipo complesso</span><span class="sxs-lookup"><span data-stu-id="c4285-108">What is a Complex Type</span></span>

<span data-ttu-id="c4285-109">I tipi complessi sono proprietà non scalari di tipi di entità che consentono l'organizzazione delle proprietà scalari nelle entità.</span><span class="sxs-lookup"><span data-stu-id="c4285-109">Complex types are non-scalar properties of entity types that enable scalar properties to be organized within entities.</span></span> <span data-ttu-id="c4285-110">Analogamente alle entità, i tipi complessi sono costituiti da proprietà scalari o da altre proprietà dei tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="c4285-110">Like entities, complex types consist of scalar properties or other complex type properties.</span></span>

<span data-ttu-id="c4285-111">Quando si utilizzano oggetti che rappresentano tipi complessi, tenere presente quanto segue:</span><span class="sxs-lookup"><span data-stu-id="c4285-111">When you work with objects that represent complex types, be aware of the following:</span></span>

-   <span data-ttu-id="c4285-112">I tipi complessi non dispongono di chiavi e pertanto non possono esistere in modo indipendente.</span><span class="sxs-lookup"><span data-stu-id="c4285-112">Complex types do not have keys and therefore cannot exist independently.</span></span> <span data-ttu-id="c4285-113">I tipi complessi possono esistere solo come proprietà di tipi di entità o gli altri tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="c4285-113">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="c4285-114">I tipi complessi non possono partecipare alle associazioni e non possono contenere proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="c4285-114">Complex types cannot participate in associations and cannot contain navigation properties.</span></span>
-   <span data-ttu-id="c4285-115">Le proprietà del tipo complesso non possono essere **null**.</span><span class="sxs-lookup"><span data-stu-id="c4285-115">Complex type properties cannot be **null**.</span></span> <span data-ttu-id="c4285-116">Viene generata un' **eccezione InvalidOperationException **quando viene chiamato **DbContext. SaveChanges** e viene rilevato un oggetto complesso null.</span><span class="sxs-lookup"><span data-stu-id="c4285-116">An **InvalidOperationException **occurs when **DbContext.SaveChanges** is called and a null complex object is encountered.</span></span> <span data-ttu-id="c4285-117">Le proprietà scalari degli oggetti complessi possono essere **null**.</span><span class="sxs-lookup"><span data-stu-id="c4285-117">Scalar properties of complex objects can be **null**.</span></span>
-   <span data-ttu-id="c4285-118">I tipi complessi non possono ereditare da altri tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="c4285-118">Complex types cannot inherit from other complex types.</span></span>
-   <span data-ttu-id="c4285-119">È necessario definire il tipo complesso come una **classe**.</span><span class="sxs-lookup"><span data-stu-id="c4285-119">You must define the complex type as a **class**.</span></span> 
-   <span data-ttu-id="c4285-120">EF rileva le modifiche apportate ai membri in un oggetto di tipo complesso quando viene chiamato **DbContext. DetectChanges** .</span><span class="sxs-lookup"><span data-stu-id="c4285-120">EF detects changes to members on a complex type object when **DbContext.DetectChanges** is called.</span></span> <span data-ttu-id="c4285-121">Entity Framework chiama automaticamente **DetectChanges** quando vengono chiamati i membri seguenti: **DbSet. Find**, **DbSet. local**, **DbSet. Remove**, **DbSet. Add**, **DbSet. Connetti**, **DbContext. SaveChanges**, **DbContext. GetValidationErrors**, **DbContext. entry**, DbChangeTracker **. Entries**.</span><span class="sxs-lookup"><span data-stu-id="c4285-121">Entity Framework calls **DetectChanges** automatically when the following members are called: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span></span>

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a><span data-ttu-id="c4285-122">Effettuare il refactoring delle proprietà di un'entità in un nuovo tipo complesso</span><span class="sxs-lookup"><span data-stu-id="c4285-122">Refactor an Entity’s Properties into New Complex Type</span></span>

<span data-ttu-id="c4285-123">Se si dispone già di un'entità nel modello concettuale, potrebbe essere necessario effettuare il refactoring di alcune proprietà in una proprietà di tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="c4285-123">If you already have an entity in your conceptual model you may want to refactor some of the properties into a complex type property.</span></span>

<span data-ttu-id="c4285-124">Nell'area di progettazione selezionare una o più proprietà (escluse le proprietà di navigazione) di un'entità, quindi fare clic con il pulsante destro del mouse e scegliere **refactoring-&gt; sposta in nuovo tipo complesso**.</span><span class="sxs-lookup"><span data-stu-id="c4285-124">On the designer surface, select one or more properties (excluding navigation properties) of an entity, then right-click and select **Refactor -&gt; Move to New Complex Type**.</span></span>

![Refactoring](~/ef6/media/refactor.png)

<span data-ttu-id="c4285-126">Un nuovo tipo complesso con le proprietà selezionate viene aggiunto a **browser modello**.</span><span class="sxs-lookup"><span data-stu-id="c4285-126">A new complex type with the selected properties is added to the **Model Browser**.</span></span> <span data-ttu-id="c4285-127">Al tipo complesso viene assegnato un nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="c4285-127">The complex type is given a default name.</span></span>

<span data-ttu-id="c4285-128">Una proprietà complessa del tipo appena creato sostituisce le proprietà selezionate.</span><span class="sxs-lookup"><span data-stu-id="c4285-128">A complex property of the newly created type replaces the selected properties.</span></span> <span data-ttu-id="c4285-129">Tutti i mapping delle proprietà vengono mantenuti.</span><span class="sxs-lookup"><span data-stu-id="c4285-129">All property mappings are preserved.</span></span>

![Refactoring 2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a><span data-ttu-id="c4285-131">Crea nuovo tipo complesso</span><span class="sxs-lookup"><span data-stu-id="c4285-131">Create a New Complex Type</span></span>

<span data-ttu-id="c4285-132">È anche possibile creare un nuovo tipo complesso che non contiene proprietà di un'entità esistente.</span><span class="sxs-lookup"><span data-stu-id="c4285-132">You can also create a new complex type that does not contain properties of an existing entity.</span></span>

<span data-ttu-id="c4285-133">Fare clic con il pulsante destro del mouse sulla cartella **tipi complessi** in browser modello, scegliere **tipo complesso AddNew...** .</span><span class="sxs-lookup"><span data-stu-id="c4285-133">Right-click the **Complex Types** folder in the Model Browser, point to **AddNew Complex Type…**.</span></span> <span data-ttu-id="c4285-134">In alternativa, è possibile selezionare la cartella **tipi complessi** e premere il tasto **Inserisci** sulla tastiera.</span><span class="sxs-lookup"><span data-stu-id="c4285-134">Alternatively, you can select the **Complex Types** folder and press the **Insert** key on your keyboard.</span></span>

![Aggiungi nuovo tipo complesso](~/ef6/media/addnewcomplextype.png)

<span data-ttu-id="c4285-136">Un nuovo tipo complesso verrà aggiunto nella cartella con un nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="c4285-136">A new complex type is added to the folder with a default name.</span></span> <span data-ttu-id="c4285-137">È ora possibile aggiungere proprietà al tipo.</span><span class="sxs-lookup"><span data-stu-id="c4285-137">You can now add properties to the type.</span></span>

## <a name="add-properties-to-a-complex-type"></a><span data-ttu-id="c4285-138">Aggiungere proprietà a un tipo complesso</span><span class="sxs-lookup"><span data-stu-id="c4285-138">Add Properties to a Complex Type</span></span>

<span data-ttu-id="c4285-139">Le proprietà di un tipo complesso possono essere tipi scalari o tipi complessi esistenti.</span><span class="sxs-lookup"><span data-stu-id="c4285-139">Properties of a complex type can be scalar types or existing complex types.</span></span> <span data-ttu-id="c4285-140">Tuttavia, le proprietà dei tipi complessi non possono avere riferimenti circolari.</span><span class="sxs-lookup"><span data-stu-id="c4285-140">However, complex type properties cannot have circular references.</span></span> <span data-ttu-id="c4285-141">Ad esempio, un tipo complesso **OnsiteCourseDetails** non può avere una proprietà di tipo complesso **OnsiteCourseDetails**.</span><span class="sxs-lookup"><span data-stu-id="c4285-141">For example, a complex type **OnsiteCourseDetails** cannot have a property of complex type **OnsiteCourseDetails**.</span></span>

<span data-ttu-id="c4285-142">È possibile aggiungere una proprietà a un tipo complesso in uno dei modi elencati di seguito.</span><span class="sxs-lookup"><span data-stu-id="c4285-142">You can add a property to a complex type in any of the ways listed below.</span></span>

-   <span data-ttu-id="c4285-143">Fare clic con il pulsante destro del mouse su un tipo complesso in browser modello, scegliere **Aggiungi**, scegliere **proprietà scalare** o **proprietà complessa**, quindi selezionare il tipo di proprietà desiderato.</span><span class="sxs-lookup"><span data-stu-id="c4285-143">Right-click a complex type in the Model Browser, point to **Add**, then point to **Scalar Property** or **Complex Property**, then select the desired property type.</span></span> <span data-ttu-id="c4285-144">In alternativa, è possibile selezionare un tipo complesso e quindi premere il tasto **inserisci** sulla tastiera.</span><span class="sxs-lookup"><span data-stu-id="c4285-144">Alternatively, you can select a complex type and then press the **Insert** key on your keyboard.</span></span>  

    ![Aggiungere proprietà a un tipo complesso](~/ef6/media/addpropertiestocomplextype.png)

    <span data-ttu-id="c4285-146">Una nuova proprietà verrà aggiunta al tipo complesso con un nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="c4285-146">A new property is added to the complex type with a default name.</span></span>

- <span data-ttu-id="c4285-147">oppure -</span><span class="sxs-lookup"><span data-stu-id="c4285-147">OR -</span></span>

-   <span data-ttu-id="c4285-148">Fare clic con il pulsante destro del mouse su una proprietà dell'entità nell'area di **progettazione EF** e scegliere **copia**, quindi fare clic con il pulsante destro del mouse sul tipo complesso in **browser modello** e scegliere **Incolla**.</span><span class="sxs-lookup"><span data-stu-id="c4285-148">Right-click an entity property on the **EF  Designer** surface and select **Copy**, then right-click the complex type in the **Model Browser** and select **Paste**.</span></span>

## <a name="rename-a-complex-type"></a><span data-ttu-id="c4285-149">Rinominare un tipo complesso</span><span class="sxs-lookup"><span data-stu-id="c4285-149">Rename a Complex Type</span></span>

<span data-ttu-id="c4285-150">Quando si rinomina un tipo complesso, tutti i riferimenti al tipo presenti nell'intero progetto vengono aggiornati.</span><span class="sxs-lookup"><span data-stu-id="c4285-150">When you rename a complex type, all references to the type are updated throughout the project.</span></span>

-   <span data-ttu-id="c4285-151">Fare doppio clic su un tipo complesso in **browser modello**.</span><span class="sxs-lookup"><span data-stu-id="c4285-151">Slowly double-click a complex type in the **Model Browser**.</span></span>
    <span data-ttu-id="c4285-152">Il nome verrà selezionato e sarà in modalità di modifica.</span><span class="sxs-lookup"><span data-stu-id="c4285-152">The name will be selected and in edit mode.</span></span>

- <span data-ttu-id="c4285-153">oppure -</span><span class="sxs-lookup"><span data-stu-id="c4285-153">OR -</span></span>

-   <span data-ttu-id="c4285-154">Fare clic con il pulsante destro del mouse su un tipo complesso in **browser modello** e scegliere **Rinomina**.</span><span class="sxs-lookup"><span data-stu-id="c4285-154">Right-click a complex type in the **Model Browser** and select **Rename**.</span></span>

- <span data-ttu-id="c4285-155">oppure -</span><span class="sxs-lookup"><span data-stu-id="c4285-155">OR -</span></span>

-   <span data-ttu-id="c4285-156">Selezionare un tipo complesso nella finestra Browser modello e premere il tasto F2.</span><span class="sxs-lookup"><span data-stu-id="c4285-156">Select a complex type in the Model Browser and press the F2 key.</span></span>

- <span data-ttu-id="c4285-157">oppure -</span><span class="sxs-lookup"><span data-stu-id="c4285-157">OR -</span></span>

-   <span data-ttu-id="c4285-158">Fare clic con il pulsante destro del mouse su un tipo complesso in **browser modello** e scegliere **Proprietà**.</span><span class="sxs-lookup"><span data-stu-id="c4285-158">Right-click a complex type in the **Model Browser** and select **Properties**.</span></span> <span data-ttu-id="c4285-159">Modificare il nome nella finestra **proprietà** .</span><span class="sxs-lookup"><span data-stu-id="c4285-159">Edit the name in the **Properties** window.</span></span>

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a><span data-ttu-id="c4285-160">Aggiungere un tipo complesso esistente a un'entità ed eseguire il mapping delle proprietà alle colonne della tabella</span><span class="sxs-lookup"><span data-stu-id="c4285-160">Add an Existing Complex Type to an Entity and Map its Properties to Table Columns</span></span>

1.  <span data-ttu-id="c4285-161">Fare clic con il pulsante destro del mouse su un'entità, scegliere **Aggiungi nuovo**e selezionare **proprietà complessa**.</span><span class="sxs-lookup"><span data-stu-id="c4285-161">Right-click an entity, point to **Add New**, and select **Complex Property**.</span></span>
    <span data-ttu-id="c4285-162">Una proprietà del tipo complesso con un nome predefinito verrà aggiunta all'entità.</span><span class="sxs-lookup"><span data-stu-id="c4285-162">A complex type property with a default name is added to the entity.</span></span> <span data-ttu-id="c4285-163">Un tipo predefinito, scelto tra i tipi complessi esistenti, verrà assegnato alla proprietà.</span><span class="sxs-lookup"><span data-stu-id="c4285-163">A default type (chosen from the existing complex types) is assigned to the property.</span></span>
2.  <span data-ttu-id="c4285-164">Assegnare il tipo desiderato alla proprietà nella finestra **proprietà** .</span><span class="sxs-lookup"><span data-stu-id="c4285-164">Assign the desired type to the property in the **Properties** window.</span></span>
    <span data-ttu-id="c4285-165">Dopo avere aggiunto una proprietà del tipo complesso a un'entità, è necessario eseguire il mapping delle proprietà alle colonne della tabella.</span><span class="sxs-lookup"><span data-stu-id="c4285-165">After adding a complex type property to an entity, you must map its properties to table columns.</span></span>
3.  <span data-ttu-id="c4285-166">Fare clic con il pulsante destro del mouse su un tipo di entità nell'area di progettazione o nel **browser modello** e selezionare **mapping tabella**.</span><span class="sxs-lookup"><span data-stu-id="c4285-166">Right-click an entity type on the design surface or in the **Model Browser** and select **Table Mappings**.</span></span>
    <span data-ttu-id="c4285-167">I mapping delle tabelle vengono visualizzati nella finestra **Dettagli mapping** .</span><span class="sxs-lookup"><span data-stu-id="c4285-167">The table mappings are displayed in the **Mapping Details** window.</span></span>
4.  <span data-ttu-id="c4285-168">Espandere il **nome della tabella Maps &lt;&gt;**  nodo.</span><span class="sxs-lookup"><span data-stu-id="c4285-168">Expand the **Maps to &lt;Table Name&gt;** node.</span></span>
    <span data-ttu-id="c4285-169">Viene visualizzato un **mapping di colonne** nodo.</span><span class="sxs-lookup"><span data-stu-id="c4285-169">A **Column Mappings** node appears.</span></span>
5.  <span data-ttu-id="c4285-170">Espandere il nodo **Mapping colonne** .</span><span class="sxs-lookup"><span data-stu-id="c4285-170">Expand the **Column Mappings** node.</span></span>
    <span data-ttu-id="c4285-171">Verrà visualizzato un elenco di tutte le colonne della tabella.</span><span class="sxs-lookup"><span data-stu-id="c4285-171">A list of all the columns in the table appears.</span></span> <span data-ttu-id="c4285-172">Le proprietà predefinite, se presenti, a cui è associato il mapping delle colonne sono elencate sotto l'intestazione **valore/proprietà** .</span><span class="sxs-lookup"><span data-stu-id="c4285-172">The default properties (if any) to which the columns map are listed under the **Value/Property** heading.</span></span>
6.  <span data-ttu-id="c4285-173">Selezionare la colonna di cui si desidera eseguire il mapping, quindi fare clic con il pulsante destro del mouse sul campo **valore/proprietà** corrispondente .</span><span class="sxs-lookup"><span data-stu-id="c4285-173">Select the column you want to map, and then right-click the corresponding **Value/Property** field.</span></span>
    <span data-ttu-id="c4285-174">Verrà visualizzato un elenco a discesa di tutte le proprietà scalari.</span><span class="sxs-lookup"><span data-stu-id="c4285-174">A drop-down list of all the scalar properties is displayed.</span></span>
7.  <span data-ttu-id="c4285-175">Selezionare la proprietà appropriata.</span><span class="sxs-lookup"><span data-stu-id="c4285-175">Select the appropriate property.</span></span>

    ![Tipo complesso mappa](~/ef6/media/mapcomplextype.png)

8.  <span data-ttu-id="c4285-177">Ripetere i passaggi 6 e 7 per ogni colonna della tabella.</span><span class="sxs-lookup"><span data-stu-id="c4285-177">Repeat steps 6 and 7 for each table column.</span></span>

>[!NOTE]
> <span data-ttu-id="c4285-178">Per eliminare un mapping di colonna, selezionare la colonna di cui si desidera eseguire il mapping, quindi fare clic sul campo **valore/proprietà** .</span><span class="sxs-lookup"><span data-stu-id="c4285-178">To delete a column mapping, select the column that you want to map, and then click the **Value/Property** field.</span></span> <span data-ttu-id="c4285-179">Selezionare quindi **Elimina** dall'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="c4285-179">Then, select **Delete** from the drop-down list.</span></span>

## <a name="map-a-function-import-to-a-complex-type"></a><span data-ttu-id="c4285-180">Eseguire il mapping di un'importazione di funzioni a un tipo complesso</span><span class="sxs-lookup"><span data-stu-id="c4285-180">Map a Function Import to a Complex Type</span></span>

<span data-ttu-id="c4285-181">Le importazioni di funzioni sono basate sulle stored procedure.</span><span class="sxs-lookup"><span data-stu-id="c4285-181">Function imports are based on stored procedures.</span></span> <span data-ttu-id="c4285-182">Per mappare un'importazione di funzioni a un tipo complesso, le colonne restituite dalla stored procedure corrispondente devono corrispondere alle proprietà del tipo complesso nel numero e devono disporre di tipi di archiviazione che sono compatibili con i tipi di proprietà.</span><span class="sxs-lookup"><span data-stu-id="c4285-182">To map a function import to a complex type, the columns returned by the corresponding stored procedure must match the properties of the complex type in number and must have storage types that are compatible with the property types.</span></span>

-   <span data-ttu-id="c4285-183">Fare doppio clic su una funzione importata di cui si desidera eseguire il mapping a un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="c4285-183">Double-click on an imported function that you want to map to a complex type.</span></span>

    ![Importazioni di funzioni](~/ef6/media/functionimports.png)

-   <span data-ttu-id="c4285-185">Specificare le impostazioni per la nuova importazione di funzioni come segue:</span><span class="sxs-lookup"><span data-stu-id="c4285-185">Fill in the settings for the new function import, as follows:</span></span>
    -   <span data-ttu-id="c4285-186">Specificare il stored procedure per cui si sta creando un'importazione di funzioni nel campo **Nome stored procedure** .</span><span class="sxs-lookup"><span data-stu-id="c4285-186">Specify the stored procedure for which you are creating a function import in the **Stored Procedure Name** field.</span></span> <span data-ttu-id="c4285-187">Questo campo è un elenco a discesa in cui sono visualizzate tutte le stored procedure contenute nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="c4285-187">This field is a drop-down list that displays all the stored procedures in the storage model.</span></span>
    -   <span data-ttu-id="c4285-188">Specificare il nome dell'importazione di funzioni nel campo **Nome importazione funzione** .</span><span class="sxs-lookup"><span data-stu-id="c4285-188">Specify the name of the function import in the **Function Import Name** field.</span></span>
    -   <span data-ttu-id="c4285-189">Selezionare  **complesse** come tipo restituito e quindi specificare il tipo restituito complesso specifico scegliendo il tipo appropriato dall'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="c4285-189">Select **Complex** as the return type and then specify the specific complex return type by choosing the appropriate type from the drop-down list.</span></span>

        ![Modifica importazione funzione](~/ef6/media/editfunctionimport.png)

-   <span data-ttu-id="c4285-191">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4285-191">Click **OK**.</span></span>
    <span data-ttu-id="c4285-192">L'importazione di funzioni viene creata nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="c4285-192">The function import entry is created in the conceptual model.</span></span>

### <a name="customize-column-mapping-for-function-import"></a><span data-ttu-id="c4285-193">Personalizzare il mapping delle colonne per l'importazione di funzioni</span><span class="sxs-lookup"><span data-stu-id="c4285-193">Customize Column Mapping for Function Import</span></span>

-   <span data-ttu-id="c4285-194">Fare clic con il pulsante destro del mouse sull'importazione di funzioni in browser modello, quindi scegliere **mapping importazione funzioni**.</span><span class="sxs-lookup"><span data-stu-id="c4285-194">Right-click the function import in the Model Browser and select **Function Import Mapping**.</span></span>
    <span data-ttu-id="c4285-195">Viene visualizzata la finestra **Dettagli mapping** e viene visualizzato il mapping predefinito per l'importazione di funzioni.</span><span class="sxs-lookup"><span data-stu-id="c4285-195">The **Mapping Details** window appears and shows the default mapping for the function import.</span></span> <span data-ttu-id="c4285-196">Le frecce indicano i mapping tra i valori della proprietà e i valori della colonna.</span><span class="sxs-lookup"><span data-stu-id="c4285-196">Arrows indicate the mappings between column values and property values.</span></span> <span data-ttu-id="c4285-197">Per impostazione predefinita, i nomi della colonna corrispondono ai nomi delle proprietà di tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="c4285-197">By default, the column names are assumed to be the same as the complex type's property names.</span></span> <span data-ttu-id="c4285-198">I nomi delle colonne predefiniti vengono visualizzati in grigio.</span><span class="sxs-lookup"><span data-stu-id="c4285-198">The default column names appear in gray text.</span></span>
-   <span data-ttu-id="c4285-199">Se necessario, modificare i nomi delle colonne per renderli corrispondenti ai nomi delle colonne restituiti dalla stored procedure che corrisponde all'importazione di funzioni.</span><span class="sxs-lookup"><span data-stu-id="c4285-199">If necessary, change the column names to match the column names that are returned by the stored procedure that corresponds to the function import.</span></span>

## <a name="delete-a-complex-type"></a><span data-ttu-id="c4285-200">Eliminare un tipo complesso</span><span class="sxs-lookup"><span data-stu-id="c4285-200">Delete a Complex Type</span></span>

<span data-ttu-id="c4285-201">Quando si elimina un tipo complesso, questo viene eliminato dal modello concettuale; vengono inoltre eliminati i mapping di tutte le istanze del tipo.</span><span class="sxs-lookup"><span data-stu-id="c4285-201">When you delete a complex type, the type is deleted from the conceptual model and mappings for all instances of the type are deleted.</span></span> <span data-ttu-id="c4285-202">I riferimenti al tipo non vengono tuttavia aggiornati.</span><span class="sxs-lookup"><span data-stu-id="c4285-202">However, references to the type are not updated.</span></span> <span data-ttu-id="c4285-203">Se, ad esempio, un'entità ha una proprietà di tipo complesso di tipo Tipocomplesso1 e Tipocomplesso1 viene eliminato in **browser modello**, la proprietà dell'entità corrispondente non viene aggiornata.</span><span class="sxs-lookup"><span data-stu-id="c4285-203">For example, if an entity has a complex type property of type ComplexType1 and ComplexType1 is deleted in the **Model Browser**, the corresponding entity property is not updated.</span></span> <span data-ttu-id="c4285-204">Il modello non verrà convalidato perché contiene un'entità che fa riferimento a un tipo complesso eliminato.</span><span class="sxs-lookup"><span data-stu-id="c4285-204">The model will not validate  because it contains an entity that references a deleted complex type.</span></span> <span data-ttu-id="c4285-205">Tramite Entity Designer è possibile aggiornare o eliminare i riferimenti ai tipi complessi eliminati.</span><span class="sxs-lookup"><span data-stu-id="c4285-205">You can update or delete references to deleted complex types by using the Entity Designer.</span></span>

-   <span data-ttu-id="c4285-206">Fare clic con il pulsante destro del mouse su un tipo complesso in browser modello e scegliere **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="c4285-206">Right-click a complex type in the Model Browser and select **Delete**.</span></span>

- <span data-ttu-id="c4285-207">oppure -</span><span class="sxs-lookup"><span data-stu-id="c4285-207">OR -</span></span>

-   <span data-ttu-id="c4285-208">Selezionare un tipo complesso nella finestra Browser modello e premere il tasto CANC sulla tastiera.</span><span class="sxs-lookup"><span data-stu-id="c4285-208">Select a complex type in the Model Browser and press the Delete key on your keyboard.</span></span>

## <a name="query-for-entities-containing-properties-of-complex-type"></a><span data-ttu-id="c4285-209">Query per entità contenenti proprietà di tipo complesso</span><span class="sxs-lookup"><span data-stu-id="c4285-209">Query for Entities Containing Properties of Complex Type</span></span>

<span data-ttu-id="c4285-210">Nel codice seguente viene illustrato come eseguire una query che restituisce una raccolta di oggetti del tipo di entità che contengono una proprietà di tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="c4285-210">The following code shows how to execute a query that returns a collection of entity type objects that contain a complex type property.</span></span>

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        var courses =
            from c in context.OnsiteCourses
            order by c.Details.Time
            select c;

        foreach (var c in courses)
        {
            Console.WriteLine("Time: " + c.Details.Time);
            Console.WriteLine("Days: " + c.Details.Days);
            Console.WriteLine("Location: " + c.Details.Location);
        }
    }
```
