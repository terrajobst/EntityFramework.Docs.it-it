---
title: I tipi complessi - Entity Framework Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: d35504cbe60823249d54385962568802b3e41308
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994853"
---
# <a name="complex-types---ef-designer"></a><span data-ttu-id="72e90-102">Tipi complessi - finestra di progettazione di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="72e90-102">Complex Types - EF Designer</span></span>
<span data-ttu-id="72e90-103">Questo argomento viene illustrato come eseguire il mapping dei tipi complessi con Entity Framework Designer (Entity Framework Designer) e su come eseguire una query per le entità che contengono le proprietà del tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="72e90-103">This topic shows how to map complex types with the Entity Framework Designer (EF Designer) and how to query for entities that contain properties of complex type.</span></span>

<span data-ttu-id="72e90-104">L'immagine seguente mostra le finestre principali che vengono usate quando si lavora con la finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="72e90-104">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="72e90-106">Quando si compila il modello concettuale, avvisi relativi a entità non mappata e associazioni vengano visualizzati nell'elenco errori.</span><span class="sxs-lookup"><span data-stu-id="72e90-106">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="72e90-107">È possibile ignorare questi avvisi perché dopo aver scelto di generare il database dal modello, gli errori non verranno più visualizzato.</span><span class="sxs-lookup"><span data-stu-id="72e90-107">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="what-is-a-complex-type"></a><span data-ttu-id="72e90-108">Che cos'è un tipo complesso</span><span class="sxs-lookup"><span data-stu-id="72e90-108">What is a Complex Type</span></span>

<span data-ttu-id="72e90-109">I tipi complessi sono proprietà non scalari di tipi di entità che consentono l'organizzazione delle proprietà scalari nelle entità.</span><span class="sxs-lookup"><span data-stu-id="72e90-109">Complex types are non-scalar properties of entity types that enable scalar properties to be organized within entities.</span></span> <span data-ttu-id="72e90-110">Analogamente alle entità, i tipi complessi sono costituiti da proprietà scalari o da altre proprietà dei tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="72e90-110">Like entities, complex types consist of scalar properties or other complex type properties.</span></span>

<span data-ttu-id="72e90-111">Quando si lavora con gli oggetti che rappresentano tipi complessi, tenere presente quanto segue:</span><span class="sxs-lookup"><span data-stu-id="72e90-111">When you work with objects that represent complex types, be aware of the following:</span></span>

-   <span data-ttu-id="72e90-112">I tipi complessi non dispongono di chiavi e pertanto non possono esistere indipendentemente.</span><span class="sxs-lookup"><span data-stu-id="72e90-112">Complex types do not have keys and therefore cannot exist independently.</span></span> <span data-ttu-id="72e90-113">I tipi complessi possono esistere solo come proprietà di tipi di entità o gli altri tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="72e90-113">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="72e90-114">I tipi complessi non possono partecipare alle associazioni e non possono contenere le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="72e90-114">Complex types cannot participate in associations and cannot contain navigation properties.</span></span>
-   <span data-ttu-id="72e90-115">Proprietà del tipo complesso non può essere **null**.</span><span class="sxs-lookup"><span data-stu-id="72e90-115">Complex type properties cannot be **null**.</span></span> <span data-ttu-id="72e90-116">Un * * InvalidOperationException * * si verifica quando si **DbContext.SaveChanges** viene chiamato e viene rilevato un oggetto complesso null.</span><span class="sxs-lookup"><span data-stu-id="72e90-116">An **InvalidOperationException **occurs when **DbContext.SaveChanges** is called and a null complex object is encountered.</span></span> <span data-ttu-id="72e90-117">Le proprietà scalari degli oggetti complessi possono essere **null**.</span><span class="sxs-lookup"><span data-stu-id="72e90-117">Scalar properties of complex objects can be **null**.</span></span>
-   <span data-ttu-id="72e90-118">I tipi complessi non possono ereditare da altri tipi complessi.</span><span class="sxs-lookup"><span data-stu-id="72e90-118">Complex types cannot inherit from other complex types.</span></span>
-   <span data-ttu-id="72e90-119">È necessario definire il tipo complesso come un **classe**.</span><span class="sxs-lookup"><span data-stu-id="72e90-119">You must define the complex type as a **class**.</span></span> 
-   <span data-ttu-id="72e90-120">Entity Framework rileva le modifiche ai membri su un oggetto di tipo complesso quando **DbContext.DetectChanges** viene chiamato.</span><span class="sxs-lookup"><span data-stu-id="72e90-120">EF detects changes to members on a complex type object when **DbContext.DetectChanges** is called.</span></span> <span data-ttu-id="72e90-121">Chiamata di Entity Framework **DetectChanges** automaticamente quando vengono chiamati i membri seguenti: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span><span class="sxs-lookup"><span data-stu-id="72e90-121">Entity Framework calls **DetectChanges** automatically when the following members are called: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span></span>

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a><span data-ttu-id="72e90-122">Effettuare il refactoring di proprietà di un'entità nel nuovo tipo complesso</span><span class="sxs-lookup"><span data-stu-id="72e90-122">Refactor an Entity’s Properties into New Complex Type</span></span>

<span data-ttu-id="72e90-123">Se si dispone già di un'entità nel modello concettuale è possibile effettuare il refactoring di alcune delle proprietà in una proprietà del tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="72e90-123">If you already have an entity in your conceptual model you may want to refactor some of the properties into a complex type property.</span></span>

<span data-ttu-id="72e90-124">Nell'area di progettazione, selezionare uno o più proprietà (escludendo le proprietà di navigazione) di un'entità, quindi fare clic e selezionare **refactoring -&gt; passare al nuovo tipo complesso**.</span><span class="sxs-lookup"><span data-stu-id="72e90-124">On the designer surface, select one or more properties (excluding navigation properties) of an entity, then right-click and select **Refactor -&gt; Move to New Complex Type**.</span></span>

![Refactoring](~/ef6/media/refactor.png)

<span data-ttu-id="72e90-126">Viene aggiunto un nuovo tipo complesso con le proprietà selezionate per il **Browser modello**.</span><span class="sxs-lookup"><span data-stu-id="72e90-126">A new complex type with the selected properties is added to the **Model Browser**.</span></span> <span data-ttu-id="72e90-127">Al tipo complesso viene assegnato un nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="72e90-127">The complex type is given a default name.</span></span>

<span data-ttu-id="72e90-128">Una proprietà complessa del tipo appena creato sostituisce le proprietà selezionate.</span><span class="sxs-lookup"><span data-stu-id="72e90-128">A complex property of the newly created type replaces the selected properties.</span></span> <span data-ttu-id="72e90-129">Tutti i mapping delle proprietà vengono mantenuti.</span><span class="sxs-lookup"><span data-stu-id="72e90-129">All property mappings are preserved.</span></span>

![Refactor2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a><span data-ttu-id="72e90-131">Creare un nuovo tipo complesso</span><span class="sxs-lookup"><span data-stu-id="72e90-131">Create a New Complex Type</span></span>

<span data-ttu-id="72e90-132">È anche possibile creare un nuovo tipo complesso che non contiene le proprietà di un'entità esistente.</span><span class="sxs-lookup"><span data-stu-id="72e90-132">You can also create a new complex type that does not contain properties of an existing entity.</span></span>

<span data-ttu-id="72e90-133">Fare doppio clic il **i tipi complessi** cartella nella finestra Browser modello, scegliere **AddNew tipo complesso...** .</span><span class="sxs-lookup"><span data-stu-id="72e90-133">Right-click the **Complex Types** folder in the Model Browser, point to **AddNew Complex Type…**.</span></span> <span data-ttu-id="72e90-134">In alternativa, è possibile selezionare i **i tipi complessi** cartella e premere il **Inserisci** sulla tastiera.</span><span class="sxs-lookup"><span data-stu-id="72e90-134">Alternatively, you can select the **Complex Types** folder and press the **Insert** key on your keyboard.</span></span>

![AddNewComplextype](~/ef6/media/addnewcomplextype.png)

<span data-ttu-id="72e90-136">Un nuovo tipo complesso verrà aggiunto nella cartella con un nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="72e90-136">A new complex type is added to the folder with a default name.</span></span> <span data-ttu-id="72e90-137">È ora possibile aggiungere proprietà al tipo.</span><span class="sxs-lookup"><span data-stu-id="72e90-137">You can now add properties to the type.</span></span>

## <a name="add-properties-to-a-complex-type"></a><span data-ttu-id="72e90-138">Aggiungere proprietà a un tipo complesso</span><span class="sxs-lookup"><span data-stu-id="72e90-138">Add Properties to a Complex Type</span></span>

<span data-ttu-id="72e90-139">Le proprietà di un tipo complesso possono essere tipi scalari o tipi complessi esistenti.</span><span class="sxs-lookup"><span data-stu-id="72e90-139">Properties of a complex type can be scalar types or existing complex types.</span></span> <span data-ttu-id="72e90-140">Tuttavia, le proprietà dei tipi complessi non possono avere riferimenti circolari.</span><span class="sxs-lookup"><span data-stu-id="72e90-140">However, complex type properties cannot have circular references.</span></span> <span data-ttu-id="72e90-141">Ad esempio, un tipo complesso **OnsiteCourseDetails** non può contenere una proprietà di tipo complesso **OnsiteCourseDetails**.</span><span class="sxs-lookup"><span data-stu-id="72e90-141">For example, a complex type **OnsiteCourseDetails** cannot have a property of complex type **OnsiteCourseDetails**.</span></span>

<span data-ttu-id="72e90-142">È possibile aggiungere una proprietà a un tipo complesso in uno dei modi elencati di seguito.</span><span class="sxs-lookup"><span data-stu-id="72e90-142">You can add a property to a complex type in any of the ways listed below.</span></span>

-   <span data-ttu-id="72e90-143">Fare doppio clic su un tipo complesso nella finestra Browser modello, scegliere **Add**, quindi scegliere **proprietà scalare** oppure **proprietà complessa**, quindi selezionare il tipo di proprietà desiderata.</span><span class="sxs-lookup"><span data-stu-id="72e90-143">Right-click a complex type in the Model Browser, point to **Add**, then point to **Scalar Property** or **Complex Property**, then select the desired property type.</span></span> <span data-ttu-id="72e90-144">In alternativa, è possibile selezionare un tipo complesso e quindi premere il **Inserisci** sulla tastiera.</span><span class="sxs-lookup"><span data-stu-id="72e90-144">Alternatively, you can select a complex type and then press the **Insert** key on your keyboard.</span></span>  

    ![AddPropertiestoComplexType](~/ef6/media/addpropertiestocomplextype.png)

    <span data-ttu-id="72e90-146">Una nuova proprietà verrà aggiunta al tipo complesso con un nome predefinito.</span><span class="sxs-lookup"><span data-stu-id="72e90-146">A new property is added to the complex type with a default name.</span></span>

- <span data-ttu-id="72e90-147">OR:</span><span class="sxs-lookup"><span data-stu-id="72e90-147">OR -</span></span>

-   <span data-ttu-id="72e90-148">Fare doppio clic su una proprietà di entità nel **Entity Framework Designer** surface e selezionare **copia**, quindi fare clic sul tipo complesso nel **Browser modello** e selezionare  **Incolla**.</span><span class="sxs-lookup"><span data-stu-id="72e90-148">Right-click an entity property on the **EF  Designer** surface and select **Copy**, then right-click the complex type in the **Model Browser** and select **Paste**.</span></span>

## <a name="rename-a-complex-type"></a><span data-ttu-id="72e90-149">Rinominare un tipo complesso</span><span class="sxs-lookup"><span data-stu-id="72e90-149">Rename a Complex Type</span></span>

<span data-ttu-id="72e90-150">Quando si rinomina un tipo complesso, tutti i riferimenti al tipo presenti nell'intero progetto vengono aggiornati.</span><span class="sxs-lookup"><span data-stu-id="72e90-150">When you rename a complex type, all references to the type are updated throughout the project.</span></span>

-   <span data-ttu-id="72e90-151">Fare lentamente doppio clic su un tipo complesso nel **Browser modello**.</span><span class="sxs-lookup"><span data-stu-id="72e90-151">Slowly double-click a complex type in the **Model Browser**.</span></span>
    <span data-ttu-id="72e90-152">Il nome verrà selezionato e sarà in modalità di modifica.</span><span class="sxs-lookup"><span data-stu-id="72e90-152">The name will be selected and in edit mode.</span></span>

- <span data-ttu-id="72e90-153">OR:</span><span class="sxs-lookup"><span data-stu-id="72e90-153">OR -</span></span>

-   <span data-ttu-id="72e90-154">Fare doppio clic su un tipo complesso nel **Visualizzatore modelli** e selezionare **rinominare**.</span><span class="sxs-lookup"><span data-stu-id="72e90-154">Right-click a complex type in the **Model Browser** and select **Rename**.</span></span>

- <span data-ttu-id="72e90-155">OR:</span><span class="sxs-lookup"><span data-stu-id="72e90-155">OR -</span></span>

-   <span data-ttu-id="72e90-156">Selezionare un tipo complesso nella finestra Browser modello e premere il tasto F2.</span><span class="sxs-lookup"><span data-stu-id="72e90-156">Select a complex type in the Model Browser and press the F2 key.</span></span>

- <span data-ttu-id="72e90-157">OR:</span><span class="sxs-lookup"><span data-stu-id="72e90-157">OR -</span></span>

-   <span data-ttu-id="72e90-158">Fare doppio clic su un tipo complesso nel **Visualizzatore modelli** e selezionare **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="72e90-158">Right-click a complex type in the **Model Browser** and select **Properties**.</span></span> <span data-ttu-id="72e90-159">Modificare il nome di **proprietà** finestra.</span><span class="sxs-lookup"><span data-stu-id="72e90-159">Edit the name in the **Properties** window.</span></span>

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a><span data-ttu-id="72e90-160">Aggiungere un tipo complesso esistente a un'entità ed eseguire il mapping le relative proprietà alle colonne della tabella</span><span class="sxs-lookup"><span data-stu-id="72e90-160">Add an Existing Complex Type to an Entity and Map its Properties to Table Columns</span></span>

1.  <span data-ttu-id="72e90-161">Fare doppio clic su un'entità, scegliere **Aggiungi nuovo**e selezionare **ComplexProperty**.</span><span class="sxs-lookup"><span data-stu-id="72e90-161">Right-click an entity, point to **Add New**, and select **Complex Property**.</span></span>
    <span data-ttu-id="72e90-162">Una proprietà del tipo complesso con un nome predefinito verrà aggiunta all'entità.</span><span class="sxs-lookup"><span data-stu-id="72e90-162">A complex type property with a default name is added to the entity.</span></span> <span data-ttu-id="72e90-163">Un tipo predefinito, scelto tra i tipi complessi esistenti, verrà assegnato alla proprietà.</span><span class="sxs-lookup"><span data-stu-id="72e90-163">A default type (chosen from the existing complex types) is assigned to the property.</span></span>
2.  <span data-ttu-id="72e90-164">Assegnare il tipo desiderato alla proprietà nella **proprietà** finestra.</span><span class="sxs-lookup"><span data-stu-id="72e90-164">Assign the desired type to the property in the **Properties** window.</span></span>
    <span data-ttu-id="72e90-165">Dopo avere aggiunto una proprietà del tipo complesso a un'entità, è necessario eseguire il mapping delle proprietà alle colonne della tabella.</span><span class="sxs-lookup"><span data-stu-id="72e90-165">After adding a complex type property to an entity, you must map its properties to table columns.</span></span>
3.  <span data-ttu-id="72e90-166">Fare doppio clic su un tipo di entità nell'area di progettazione o nel **Visualizzatore modelli** e selezionare **mapping delle tabelle**.</span><span class="sxs-lookup"><span data-stu-id="72e90-166">Right-click an entity type on the design surface or in the **Model Browser** and select **Table Mappings**.</span></span>
    <span data-ttu-id="72e90-167">Mapping di tabella vengono visualizzati nei **Dettagli Mapping** finestra.</span><span class="sxs-lookup"><span data-stu-id="72e90-167">The table mappings are displayed in the **Mapping Details** window.</span></span>
4.  <span data-ttu-id="72e90-168">Espandere la **esegue il mapping a &lt;nome tabella&gt;**  nodo.</span><span class="sxs-lookup"><span data-stu-id="72e90-168">Expand the **Maps to &lt;Table Name&gt;** node.</span></span>
    <span data-ttu-id="72e90-169">Oggetto **mapping delle colonne** nodo viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="72e90-169">A **Column Mappings** node appears.</span></span>
5.  <span data-ttu-id="72e90-170">Espandere la **mapping delle colonne** nodo.</span><span class="sxs-lookup"><span data-stu-id="72e90-170">Expand the **Column Mappings** node.</span></span>
    <span data-ttu-id="72e90-171">Verrà visualizzato un elenco di tutte le colonne della tabella.</span><span class="sxs-lookup"><span data-stu-id="72e90-171">A list of all the columns in the table appears.</span></span> <span data-ttu-id="72e90-172">Le proprietà predefinite (se presente) per cui eseguire il mapping di colonne sono elencati sotto la **valore/proprietà** intestazione.</span><span class="sxs-lookup"><span data-stu-id="72e90-172">The default properties (if any) to which the columns map are listed under the **Value/Property** heading.</span></span>
6.  <span data-ttu-id="72e90-173">Selezionare la colonna che si desidera eseguire il mapping e quindi fare doppio clic su corrispondente **valore/proprietà** campo.</span><span class="sxs-lookup"><span data-stu-id="72e90-173">Select the column you want to map, and then right-click the corresponding **Value/Property** field.</span></span>
    <span data-ttu-id="72e90-174">Verrà visualizzato un elenco a discesa di tutte le proprietà scalari.</span><span class="sxs-lookup"><span data-stu-id="72e90-174">A drop-down list of all the scalar properties is displayed.</span></span>
7.  <span data-ttu-id="72e90-175">Selezionare la proprietà appropriata.</span><span class="sxs-lookup"><span data-stu-id="72e90-175">Select the appropriate property.</span></span>

    ![MapComplexType](~/ef6/media/mapcomplextype.png)

8.  <span data-ttu-id="72e90-177">Ripetere i passaggi 6 e 7 per ogni colonna della tabella.</span><span class="sxs-lookup"><span data-stu-id="72e90-177">Repeat steps 6 and 7 for each table column.</span></span>

>[!NOTE]
> <span data-ttu-id="72e90-178">Per eliminare un mapping di colonne, selezionare la colonna che si desidera eseguire il mapping e quindi scegliere il **valore/proprietà** campo.</span><span class="sxs-lookup"><span data-stu-id="72e90-178">To delete a column mapping, select the column that you want to map, and then click the **Value/Property** field.</span></span> <span data-ttu-id="72e90-179">Quindi, selezionare **eliminare** nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="72e90-179">Then, select **Delete** from the drop-down list.</span></span>

## <a name="map-a-function-import-to-a-complex-type"></a><span data-ttu-id="72e90-180">Mappare un'importazione di funzioni a un tipo complesso</span><span class="sxs-lookup"><span data-stu-id="72e90-180">Map a Function Import to a Complex Type</span></span>

<span data-ttu-id="72e90-181">Le importazioni di funzioni sono basate sulle stored procedure.</span><span class="sxs-lookup"><span data-stu-id="72e90-181">Function imports are based on stored procedures.</span></span> <span data-ttu-id="72e90-182">Per mappare un'importazione di funzioni a un tipo complesso, le colonne restituite dalla stored procedure corrispondente devono corrispondere alle proprietà del tipo complesso nel numero e devono disporre di tipi di archiviazione che sono compatibili con i tipi di proprietà.</span><span class="sxs-lookup"><span data-stu-id="72e90-182">To map a function import to a complex type, the columns returned by the corresponding stored procedure must match the properties of the complex type in number and must have storage types that are compatible with the property types.</span></span>

-   <span data-ttu-id="72e90-183">Fare doppio clic su una funzione importata che si desidera eseguire il mapping a un tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="72e90-183">Double-click on an imported function that you want to map to a complex type.</span></span>

    ![FunctionImports](~/ef6/media/functionimports.png)

-   <span data-ttu-id="72e90-185">Specificare le impostazioni per la nuova importazione di funzioni come segue:</span><span class="sxs-lookup"><span data-stu-id="72e90-185">Fill in the settings for the new function import, as follows:</span></span>
    -   <span data-ttu-id="72e90-186">Specificare la stored procedure per il quale si sta creando un'importazione di funzioni nel **nome Stored Procedure** campo.</span><span class="sxs-lookup"><span data-stu-id="72e90-186">Specify the stored procedure for which you are creating a function import in the **Stored Procedure Name** field.</span></span> <span data-ttu-id="72e90-187">Questo campo è un elenco a discesa in cui sono visualizzate tutte le stored procedure contenute nel modello di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="72e90-187">This field is a drop-down list that displays all the stored procedures in the storage model.</span></span>
    -   <span data-ttu-id="72e90-188">Specificare il nome dell'oggetto function import nel **nome Function Import** campo.</span><span class="sxs-lookup"><span data-stu-id="72e90-188">Specify the name of the function import in the **Function Import Name** field.</span></span>
    -   <span data-ttu-id="72e90-189">Selezionare **complessi** come il valore restituito digitare e quindi specificare il tipo restituito complesso specifico scegliendo il tipo appropriato nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="72e90-189">Select **Complex** as the return type and then specify the specific complex return type by choosing the appropriate type from the drop-down list.</span></span>

        ![EditFunctionImport](~/ef6/media/editfunctionimport.png)

-   <span data-ttu-id="72e90-191">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="72e90-191">Click **OK**.</span></span>
    <span data-ttu-id="72e90-192">L'importazione di funzioni viene creata nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="72e90-192">The function import entry is created in the conceptual model.</span></span>

### <a name="customize-column-mapping-for-function-import"></a><span data-ttu-id="72e90-193">Personalizzare i Mapping per l'importazione di funzioni di colonne</span><span class="sxs-lookup"><span data-stu-id="72e90-193">Customize Column Mapping for Function Import</span></span>

-   <span data-ttu-id="72e90-194">Importazione di funzioni nella finestra Browser modello e scegliere **importare Mapping di funzione**.</span><span class="sxs-lookup"><span data-stu-id="72e90-194">Right-click the function import in the Model Browser and select **Function Import Mapping**.</span></span>
    <span data-ttu-id="72e90-195">Il **ulteriori dettagli sul Mapping** finestra viene visualizzata e viene illustrato il mapping predefinito per l'importazione di funzioni.</span><span class="sxs-lookup"><span data-stu-id="72e90-195">The **Mapping Details** window appears and shows the default mapping for the function import.</span></span> <span data-ttu-id="72e90-196">Le frecce indicano i mapping tra i valori della proprietà e i valori della colonna.</span><span class="sxs-lookup"><span data-stu-id="72e90-196">Arrows indicate the mappings between column values and property values.</span></span> <span data-ttu-id="72e90-197">Per impostazione predefinita, i nomi della colonna corrispondono ai nomi delle proprietà di tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="72e90-197">By default, the column names are assumed to be the same as the complex type's property names.</span></span> <span data-ttu-id="72e90-198">I nomi delle colonne predefiniti vengono visualizzati in grigio.</span><span class="sxs-lookup"><span data-stu-id="72e90-198">The default column names appear in gray text.</span></span>
-   <span data-ttu-id="72e90-199">Se necessario, modificare i nomi delle colonne per renderli corrispondenti ai nomi delle colonne restituiti dalla stored procedure che corrisponde all'importazione di funzioni.</span><span class="sxs-lookup"><span data-stu-id="72e90-199">If necessary, change the column names to match the column names that are returned by the stored procedure that corresponds to the function import.</span></span>

## <a name="delete-a-complex-type"></a><span data-ttu-id="72e90-200">Eliminare un tipo complesso</span><span class="sxs-lookup"><span data-stu-id="72e90-200">Delete a Complex Type</span></span>

<span data-ttu-id="72e90-201">Quando si elimina un tipo complesso, questo viene eliminato dal modello concettuale; vengono inoltre eliminati i mapping di tutte le istanze del tipo.</span><span class="sxs-lookup"><span data-stu-id="72e90-201">When you delete a complex type, the type is deleted from the conceptual model and mappings for all instances of the type are deleted.</span></span> <span data-ttu-id="72e90-202">I riferimenti al tipo non vengono tuttavia aggiornati.</span><span class="sxs-lookup"><span data-stu-id="72e90-202">However, references to the type are not updated.</span></span> <span data-ttu-id="72e90-203">Ad esempio, se un'entità dispone di una proprietà di tipo complesso di tipo Tipocomplesso1 e Tipocomplesso1 viene eliminato nel **Browser modello**, non viene aggiornata la proprietà dell'entità corrispondente.</span><span class="sxs-lookup"><span data-stu-id="72e90-203">For example, if an entity has a complex type property of type ComplexType1 and ComplexType1 is deleted in the **Model Browser**, the corresponding entity property is not updated.</span></span> <span data-ttu-id="72e90-204">Il modello non verrà convalidato poiché contiene un'entità che fa riferimento a un tipo complesso eliminato.</span><span class="sxs-lookup"><span data-stu-id="72e90-204">The model will not validate  because it contains an entity that references a deleted complex type.</span></span> <span data-ttu-id="72e90-205">Tramite Entity Designer è possibile aggiornare o eliminare i riferimenti ai tipi complessi eliminati.</span><span class="sxs-lookup"><span data-stu-id="72e90-205">You can update or delete references to deleted complex types by using the Entity Designer.</span></span>

-   <span data-ttu-id="72e90-206">Fare doppio clic su un tipo complesso nella finestra Browser modello e selezionare **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="72e90-206">Right-click a complex type in the Model Browser and select **Delete**.</span></span>

- <span data-ttu-id="72e90-207">OR:</span><span class="sxs-lookup"><span data-stu-id="72e90-207">OR -</span></span>

-   <span data-ttu-id="72e90-208">Selezionare un tipo complesso nella finestra Browser modello e premere il tasto CANC sulla tastiera.</span><span class="sxs-lookup"><span data-stu-id="72e90-208">Select a complex type in the Model Browser and press the Delete key on your keyboard.</span></span>

## <a name="query-for-entities-containing-properties-of-complex-type"></a><span data-ttu-id="72e90-209">Eseguire query sulle entità che contiene le proprietà del tipo complesso</span><span class="sxs-lookup"><span data-stu-id="72e90-209">Query for Entities Containing Properties of Complex Type</span></span>

<span data-ttu-id="72e90-210">Il codice seguente viene illustrato come eseguire una query che restituisce una raccolta di entità come oggetti di tipo che contengono una proprietà di tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="72e90-210">The following code shows how to execute a query that returns a collection of entity type objects that contain a complex type property.</span></span>

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
