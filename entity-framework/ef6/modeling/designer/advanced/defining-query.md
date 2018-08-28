---
title: La definizione di Query - finestra di progettazione di Entity Framework - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: 60d5310589bb9bc3fdb971673422e80537357e55
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996307"
---
# <a name="defining-query---ef-designer"></a><span data-ttu-id="e25f3-102">Definizione di Query - finestra di progettazione di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e25f3-102">Defining Query - EF Designer</span></span>
<span data-ttu-id="e25f3-103">Questa procedura dettagliata illustra come aggiungere una definizione di un tipo di query e un'entità corrispondente a un modello usando la finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e25f3-103">This walkthrough demonstrates how to add a defining query and a corresponding entity type to a model using the EF Designer.</span></span> <span data-ttu-id="e25f3-104">Una query di definizione viene comunemente utilizzata per fornire funzionalità simili a quelle fornite da una vista di database, ma la vista viene definita nel modello, non il database.</span><span class="sxs-lookup"><span data-stu-id="e25f3-104">A defining query is commonly used to provide functionality similar to that provided by a database view, but the view is defined in the model, not the database.</span></span> <span data-ttu-id="e25f3-105">Una query di definizione consente di eseguire un'istruzione SQL specificata nella **DefiningQuery** elemento di un file con estensione edmx.</span><span class="sxs-lookup"><span data-stu-id="e25f3-105">A defining query allows you to execute a SQL statement that is specified in the **DefiningQuery** element of an .edmx file.</span></span> <span data-ttu-id="e25f3-106">Per altre informazioni, vedere **DefiningQuery** nel [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="e25f3-106">For more information, see **DefiningQuery** in the [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="e25f3-107">Quando si usano query di definizione, è necessario anche definire un tipo di entità nel modello.</span><span class="sxs-lookup"><span data-stu-id="e25f3-107">When using defining queries, you also have to define an entity type in your model.</span></span> <span data-ttu-id="e25f3-108">Il tipo di entità viene utilizzato per far emergere i dati esposti dalla query di definizione.</span><span class="sxs-lookup"><span data-stu-id="e25f3-108">The entity type is used to surface data exposed by the defining query.</span></span> <span data-ttu-id="e25f3-109">Si noti che i dati emersi tramite questo tipo di entità sono di sola lettura.</span><span class="sxs-lookup"><span data-stu-id="e25f3-109">Note that data surfaced through this entity type is read-only.</span></span>

<span data-ttu-id="e25f3-110">Le query con parametri non possono essere eseguite come query di definizione.</span><span class="sxs-lookup"><span data-stu-id="e25f3-110">Parameterized queries cannot be executed as defining queries.</span></span> <span data-ttu-id="e25f3-111">Tuttavia, è possibile aggiornare i dati eseguendo il mapping delle funzioni di inserimento, aggiornamento ed eliminazione del tipo di entità che fa emergere i dati nelle stored procedure.</span><span class="sxs-lookup"><span data-stu-id="e25f3-111">However, the data can be updated by mapping the insert, update, and delete functions of the entity type that surfaces the data to stored procedures.</span></span> <span data-ttu-id="e25f3-112">Per altre informazioni, vedere [Insert, Update e Delete con Stored procedure](~/ef6/modeling/designer/stored-procedures/cud.md).</span><span class="sxs-lookup"><span data-stu-id="e25f3-112">For more information, see [Insert, Update, and Delete with Stored Procedures](~/ef6/modeling/designer/stored-procedures/cud.md).</span></span>

<span data-ttu-id="e25f3-113">In questo argomento viene illustrato come eseguire le attività seguenti.</span><span class="sxs-lookup"><span data-stu-id="e25f3-113">This topic shows how to perform the following tasks.</span></span>

-   <span data-ttu-id="e25f3-114">Aggiungere una Query di definizione</span><span class="sxs-lookup"><span data-stu-id="e25f3-114">Add a Defining Query</span></span>
-   <span data-ttu-id="e25f3-115">Aggiungere un tipo di entità al modello</span><span class="sxs-lookup"><span data-stu-id="e25f3-115">Add an Entity Type to the Model</span></span>
-   <span data-ttu-id="e25f3-116">Eseguire il mapping di Query di definizione per il tipo di entità</span><span class="sxs-lookup"><span data-stu-id="e25f3-116">Map the Defining Query to the Entity Type</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e25f3-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e25f3-117">Prerequisites</span></span>

<span data-ttu-id="e25f3-118">Per completare questa procedura dettagliata, è necessario disporre di:</span><span class="sxs-lookup"><span data-stu-id="e25f3-118">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="e25f3-119">Una versione recente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e25f3-119">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="e25f3-120">Il [database di esempio School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="e25f3-120">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="e25f3-121">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="e25f3-121">Set up the Project</span></span>

<span data-ttu-id="e25f3-122">Questa procedura dettagliata Usa Visual Studio 2012 o versioni successiva.</span><span class="sxs-lookup"><span data-stu-id="e25f3-122">This walkthrough is using Visual Studio 2012 or newer.</span></span>

-   <span data-ttu-id="e25f3-123">Aprire Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e25f3-123">Open Visual Studio.</span></span>
-   <span data-ttu-id="e25f3-124">Scegliere **Nuovo** dal menu **File**, quindi fare clic su **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="e25f3-124">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="e25f3-125">Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **applicazione Console** modello.</span><span class="sxs-lookup"><span data-stu-id="e25f3-125">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="e25f3-126">Immettere **DefiningQuerySample** come nome del progetto e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="e25f3-126">Enter **DefiningQuerySample** as the name of the project and click **OK**.</span></span>

 

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="e25f3-127">Creare un modello basato sul Database School</span><span class="sxs-lookup"><span data-stu-id="e25f3-127">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="e25f3-128">Fare clic sul nome del progetto in Esplora soluzioni, scegliere **Add**, quindi fare clic su **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="e25f3-128">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="e25f3-129">Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro dei modelli.</span><span class="sxs-lookup"><span data-stu-id="e25f3-129">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="e25f3-130">Immettere **DefiningQueryModel.edmx** per il nome del file e quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="e25f3-130">Enter **DefiningQueryModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="e25f3-131">Nella finestra di dialogo Scegli contenuto Model, selezionare **genera da database**, quindi fare clic su **successivo**.</span><span class="sxs-lookup"><span data-stu-id="e25f3-131">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="e25f3-132">Fare clic su nuova connessione.</span><span class="sxs-lookup"><span data-stu-id="e25f3-132">Click New Connection.</span></span> <span data-ttu-id="e25f3-133">Nella finestra di dialogo proprietà di connessione, immettere il nome del server (ad esempio, **(localdb)\\mssqllocaldb**), selezionare il metodo di autenticazione, il tipo **School** per il nome del database e quindi Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="e25f3-133">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="e25f3-134">La finestra di dialogo Seleziona connessione dati viene aggiornata con l'impostazione di connessione di database.</span><span class="sxs-lookup"><span data-stu-id="e25f3-134">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="e25f3-135">Nella finestra di dialogo Scegli oggetti di Database, verificare i **tabelle** nodo.</span><span class="sxs-lookup"><span data-stu-id="e25f3-135">In the Choose Your Database Objects dialog box, check the **Tables** node.</span></span> <span data-ttu-id="e25f3-136">Verrà aggiunto tutte le tabelle per il **School** modello.</span><span class="sxs-lookup"><span data-stu-id="e25f3-136">This will add all the tables to the **School** model.</span></span>
-   <span data-ttu-id="e25f3-137">Scegliere **Fine**.</span><span class="sxs-lookup"><span data-stu-id="e25f3-137">Click **Finish**.</span></span>
-   <span data-ttu-id="e25f3-138">In Esplora soluzioni fare doppio clic il **DefiningQueryModel.edmx** del file e selezionare **Apri con...** .</span><span class="sxs-lookup"><span data-stu-id="e25f3-138">In Solution Explorer, right-click the **DefiningQueryModel.edmx** file and select **Open With…**.</span></span>
-   <span data-ttu-id="e25f3-139">Selezionare **Editor XML (testo)**.</span><span class="sxs-lookup"><span data-stu-id="e25f3-139">Select **XML (Text) Editor**.</span></span>

    ![XMLEditor](~/ef6/media/xmleditor.png)

-   <span data-ttu-id="e25f3-141">Fare clic su **Sì** se viene richiesto con il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="e25f3-141">Click **Yes** if prompted with the following message:</span></span>

    ![Warning2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a><span data-ttu-id="e25f3-143">Aggiungere una Query di definizione</span><span class="sxs-lookup"><span data-stu-id="e25f3-143">Add a Defining Query</span></span>

<span data-ttu-id="e25f3-144">In questo passaggio si userà l'Editor XML per aggiungere una definizione di query e un tipo di entità alla sezione SSDL del file con estensione edmx.</span><span class="sxs-lookup"><span data-stu-id="e25f3-144">In this step we will use the XML Editor to add a defining query and an entity type to the SSDL section of the .edmx file.</span></span> 

-   <span data-ttu-id="e25f3-145">Aggiungere un **EntitySet** elemento alla sezione SSDL del file con estensione edmx (riga 5 a 13).</span><span class="sxs-lookup"><span data-stu-id="e25f3-145">Add an **EntitySet** element to the SSDL section of the .edmx file (line 5 thru 13).</span></span> <span data-ttu-id="e25f3-146">Specificare quanto segue:</span><span class="sxs-lookup"><span data-stu-id="e25f3-146">Specify the following:</span></span>
    -   <span data-ttu-id="e25f3-147">Solo le **Name** e **EntityType** gli attributi del **EntitySet** elemento vengono specificati.</span><span class="sxs-lookup"><span data-stu-id="e25f3-147">Only the **Name** and **EntityType** attributes of the **EntitySet** element are specified.</span></span>
    -   <span data-ttu-id="e25f3-148">Il nome completo del tipo di entità viene utilizzato nel **EntityType** attributo.</span><span class="sxs-lookup"><span data-stu-id="e25f3-148">The fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="e25f3-149">L'istruzione SQL da eseguire è specificata nel **DefiningQuery** elemento.</span><span class="sxs-lookup"><span data-stu-id="e25f3-149">The SQL statement to be executed is specified in the **DefiningQuery** element.</span></span>

``` xml
    <!-- SSDL content -->
    <edmx:StorageModels>
      <Schema Namespace="SchoolModel.Store" Alias="Self" Provider="System.Data.SqlClient" ProviderManifestToken="2008" xmlns:store="http://schemas.microsoft.com/ado/2007/12/edm/EntityStoreSchemaGenerator" xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
        <EntityContainer Name="SchoolModelStoreContainer">
           <EntitySet Name="GradeReport" EntityType="SchoolModel.Store.GradeReport">
              <DefiningQuery>
                SELECT CourseID, Grade, FirstName, LastName
                FROM StudentGrade
                JOIN
                (SELECT * FROM Person WHERE EnrollmentDate IS NOT NULL) AS p
                ON StudentID = p.PersonID
              </DefiningQuery>
          </EntitySet>
          <EntitySet Name="Course" EntityType="SchoolModel.Store.Course" store:Type="Tables" Schema="dbo" />
```

-   <span data-ttu-id="e25f3-150">Aggiungere il **EntityType** elemento alla sezione SSDL del file con estensione.</span><span class="sxs-lookup"><span data-stu-id="e25f3-150">Add the **EntityType** element to the SSDL section of the .edmx.</span></span> <span data-ttu-id="e25f3-151">file come mostrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="e25f3-151">file as shown below.</span></span> <span data-ttu-id="e25f3-152">Tenere presente quanto segue:</span><span class="sxs-lookup"><span data-stu-id="e25f3-152">Note the following:</span></span>
    -   <span data-ttu-id="e25f3-153">Il valore della **nome** attributo corrisponde al valore del **EntityType** attributo il **EntitySet** elemento precedente, anche se il nome completo del tipo di entità viene utilizzato il **EntityType** attributo.</span><span class="sxs-lookup"><span data-stu-id="e25f3-153">The value of the **Name** attribute corresponds to the value of the **EntityType** attribute in the **EntitySet** element above, although the fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="e25f3-154">I nomi di proprietà corrispondono ai nomi delle colonne restituiti dall'istruzione SQL nel **DefiningQuery** elemento (sopra).</span><span class="sxs-lookup"><span data-stu-id="e25f3-154">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element (above).</span></span>
    -   <span data-ttu-id="e25f3-155">In questo esempio la chiave di entità è composta da tre proprietà, per assicurare un valore di chiave univoco.</span><span class="sxs-lookup"><span data-stu-id="e25f3-155">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

``` xml
    <EntityType Name="GradeReport">
      <Key>
        <PropertyRef Name="CourseID" />
        <PropertyRef Name="FirstName" />
        <PropertyRef Name="LastName" />
      </Key>
      <Property Name="CourseID"
                Type="int"
                Nullable="false" />
      <Property Name="Grade"
                Type="decimal"
                Precision="3"
                Scale="2" />
      <Property Name="FirstName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
      <Property Name="LastName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
    </EntityType>
```

>[!NOTE]
> <span data-ttu-id="e25f3-156">Se in seguito si esegue la **procedura guidata Aggiorna modello** finestra di dialogo, tutte le modifiche apportate al modello di archiviazione, inclusa la definizione delle query, verrà sovrascritto.</span><span class="sxs-lookup"><span data-stu-id="e25f3-156">If later you run the **Update Model Wizard** dialog, any changes made to the storage model, including defining queries, will be overwritten.</span></span>

 

## <a name="add-an-entity-type-to-the-model"></a><span data-ttu-id="e25f3-157">Aggiungere un tipo di entità al modello</span><span class="sxs-lookup"><span data-stu-id="e25f3-157">Add an Entity Type to the Model</span></span>

<span data-ttu-id="e25f3-158">In questo passaggio si aggiungerà il tipo di entità al modello concettuale usando la finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e25f3-158">In this step we will add the entity type to the conceptual model using the EF Designer.</span></span>  <span data-ttu-id="e25f3-159">Tenere presente quanto segue:</span><span class="sxs-lookup"><span data-stu-id="e25f3-159">Note the following:</span></span>

-   <span data-ttu-id="e25f3-160">Il **Name** dell'entità corrispondente al valore del **EntityType** attributo il **EntitySet** elemento precedente.</span><span class="sxs-lookup"><span data-stu-id="e25f3-160">The **Name** of the entity corresponds to the value of the **EntityType** attribute in the **EntitySet** element above.</span></span>
-   <span data-ttu-id="e25f3-161">I nomi di proprietà corrispondono ai nomi delle colonne restituiti dall'istruzione SQL nel **DefiningQuery** elemento precedente.</span><span class="sxs-lookup"><span data-stu-id="e25f3-161">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element above.</span></span>
-   <span data-ttu-id="e25f3-162">In questo esempio la chiave di entità è composta da tre proprietà, per assicurare un valore di chiave univoco.</span><span class="sxs-lookup"><span data-stu-id="e25f3-162">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

<span data-ttu-id="e25f3-163">Aprire il modello nella finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e25f3-163">Open the model in the EF Designer.</span></span>

-   <span data-ttu-id="e25f3-164">Fare doppio clic il DefiningQueryModel.edmx.</span><span class="sxs-lookup"><span data-stu-id="e25f3-164">Double-click the DefiningQueryModel.edmx.</span></span>
-   <span data-ttu-id="e25f3-165">Pronunciare **Sì** per il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="e25f3-165">Say **Yes** to the following message:</span></span>

    ![Warning2](~/ef6/media/warning2.png)

 

<span data-ttu-id="e25f3-167">Entity Designer, che fornisce un'area di progettazione per la modifica del modello, viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="e25f3-167">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

-   <span data-ttu-id="e25f3-168">Fare doppio clic su area di designer e seleziona **Aggiungi nuovo**-&gt;**Entity...** .</span><span class="sxs-lookup"><span data-stu-id="e25f3-168">Right-click the designer surface and select **Add New**-&gt;**Entity…**.</span></span>
-   <span data-ttu-id="e25f3-169">Specificare **GradeReport** per il nome dell'entità e **CourseID** per il **proprietà Key**.</span><span class="sxs-lookup"><span data-stu-id="e25f3-169">Specify **GradeReport** for the entity name and **CourseID** for the **Key Property**.</span></span>
-   <span data-ttu-id="e25f3-170">Fare doppio clic il **GradeReport** entità e selezionare **Aggiungi nuovo** - &gt; **proprietà scalare**.</span><span class="sxs-lookup"><span data-stu-id="e25f3-170">Right-click the **GradeReport** entity and select **Add New**-&gt; **Scalar Property**.</span></span>
-   <span data-ttu-id="e25f3-171">Modificare il nome predefinito della proprietà **FirstName**.</span><span class="sxs-lookup"><span data-stu-id="e25f3-171">Change the default name of the property to **FirstName**.</span></span>
-   <span data-ttu-id="e25f3-172">Aggiungere un'altra proprietà scalare e specificare **LastName** per il nome.</span><span class="sxs-lookup"><span data-stu-id="e25f3-172">Add another scalar property and specify **LastName** for the name.</span></span>
-   <span data-ttu-id="e25f3-173">Aggiungere un'altra proprietà scalare e specificare **Grade** per il nome.</span><span class="sxs-lookup"><span data-stu-id="e25f3-173">Add another scalar property and specify **Grade** for the name.</span></span>
-   <span data-ttu-id="e25f3-174">Nel **proprietà** finestra Modifica il **Grade**del **tipo** proprietà **Decimal**.</span><span class="sxs-lookup"><span data-stu-id="e25f3-174">In the **Properties** window, change the **Grade**’s **Type** property to **Decimal**.</span></span>
-   <span data-ttu-id="e25f3-175">Selezionare il **FirstName** e **LastName** proprietà.</span><span class="sxs-lookup"><span data-stu-id="e25f3-175">Select the **FirstName** and **LastName** properties.</span></span>
-   <span data-ttu-id="e25f3-176">Nel **delle proprietà** finestra Modifica il **EntityKey** valore della proprietà **True**.</span><span class="sxs-lookup"><span data-stu-id="e25f3-176">In the **Properties** window, change the **EntityKey** property value to **True**.</span></span>

<span data-ttu-id="e25f3-177">Di conseguenza, gli elementi seguenti sono stati aggiunti per il **CSDL** sezione del file con estensione edmx.</span><span class="sxs-lookup"><span data-stu-id="e25f3-177">As a result, the following elements were added to the **CSDL** section of the .edmx file.</span></span>

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a><span data-ttu-id="e25f3-178">Eseguire il mapping di Query di definizione per il tipo di entità</span><span class="sxs-lookup"><span data-stu-id="e25f3-178">Map the Defining Query to the Entity Type</span></span>

<span data-ttu-id="e25f3-179">In questo passaggio si userà la finestra Dettagli Mapping per eseguire il mapping concettuale e i tipi di entità di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="e25f3-179">In this step, we will use the Mapping Details window to map the conceptual and storage entity types.</span></span>

-   <span data-ttu-id="e25f3-180">Fare doppio clic il **GradeReport** entità di un'area di progettazione e seleziona **Mapping tabella**.</span><span class="sxs-lookup"><span data-stu-id="e25f3-180">Right-click the **GradeReport** entity on the design surface and select **Table Mapping**.</span></span>  
    <span data-ttu-id="e25f3-181">Il **Dettagli Mapping** viene visualizzata la finestra.</span><span class="sxs-lookup"><span data-stu-id="e25f3-181">The **Mapping Details** window is displayed.</span></span>
-   <span data-ttu-id="e25f3-182">Selezionare **GradeReport** dalle **&lt;aggiungere una tabella o vista&gt;** nell'elenco a discesa (sotto **tabella**s).</span><span class="sxs-lookup"><span data-stu-id="e25f3-182">Select **GradeReport** from the **&lt;Add a Table or View&gt;** dropdown list (located under **Table**s).</span></span>  
    <span data-ttu-id="e25f3-183">Mapping tra concettuale predefinito e di archiviazione **GradeReport** vengono visualizzati il tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="e25f3-183">Default mappings between the conceptual and storage **GradeReport** entity type appear.</span></span>  
    <span data-ttu-id="e25f3-184">![MappingDetails3](~/ef6/media/mappingdetails.png)</span><span class="sxs-lookup"><span data-stu-id="e25f3-184">![MappingDetails3](~/ef6/media/mappingdetails.png)</span></span>

<span data-ttu-id="e25f3-185">Di conseguenza, il **EntitySetMapping** elemento viene aggiunto alla sezione del mapping del file con estensione edmx.</span><span class="sxs-lookup"><span data-stu-id="e25f3-185">As a result, the **EntitySetMapping** element is added to the mapping section of the .edmx file.</span></span> 

``` xml
    <EntitySetMapping Name="GradeReports">
      <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.GradeReport)">
        <MappingFragment StoreEntitySet="GradeReport">
          <ScalarProperty Name="LastName" ColumnName="LastName" />
          <ScalarProperty Name="FirstName" ColumnName="FirstName" />
          <ScalarProperty Name="Grade" ColumnName="Grade" />
          <ScalarProperty Name="CourseID" ColumnName="CourseID" />
        </MappingFragment>
      </EntityTypeMapping>
    </EntitySetMapping>
```

-   <span data-ttu-id="e25f3-186">Compilare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e25f3-186">Compile the application.</span></span>

 

## <a name="call-the-defining-query-in-your-code"></a><span data-ttu-id="e25f3-187">Chiamare la Query di definizione nel codice</span><span class="sxs-lookup"><span data-stu-id="e25f3-187">Call the Defining Query in your Code</span></span>

<span data-ttu-id="e25f3-188">È ora possibile eseguire la query di definizione usando il **GradeReport** tipo di entità.</span><span class="sxs-lookup"><span data-stu-id="e25f3-188">You can now execute the defining query by using the **GradeReport** entity type.</span></span> 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
