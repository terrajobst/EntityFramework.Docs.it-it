---
title: Supporto di enum - Entity Framework Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: d4c5528c4dc13ab7189421feebf84c2cb2f4b2bb
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995637"
---
# <a name="enum-support---ef-designer"></a><span data-ttu-id="008c0-102">Supporto di enum - finestra di progettazione di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="008c0-102">Enum Support - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="008c0-103">**EF5 e versioni successive solo** -le funzionalità, le API, e così via illustrati in questa pagina sono stati introdotti in Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="008c0-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="008c0-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="008c0-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="008c0-105">Questa procedura dettagliata video e procedura dettagliata illustra come usare i tipi di enumerazione con Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="008c0-105">This video and step-by-step walkthrough shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="008c0-106">Viene inoltre illustrato come usare enumerazioni in una query LINQ.</span><span class="sxs-lookup"><span data-stu-id="008c0-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="008c0-107">Questa procedura dettagliata verrà utilizzato il primo modello per creare un nuovo database, ma la finestra di progettazione di Entity Framework è anche utilizzabile con il [Database First](~/ef6/modeling/designer/workflows/database-first.md) flusso di lavoro per eseguire il mapping a un database esistente.</span><span class="sxs-lookup"><span data-stu-id="008c0-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="008c0-108">Supporto di enumerazione è stata introdotta in Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="008c0-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="008c0-109">Per usare le nuove funzionalità come le enumerazioni, tipi di dati spaziali e funzioni con valori di tabella, deve avere come destinazione .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="008c0-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="008c0-110">Visual Studio 2012 è destinato a .NET 4.5 per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="008c0-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="008c0-111">In Entity Framework, un'enumerazione può avere i seguenti tipi sottostanti: **Byte**, **Int16**, **Int32**, **Int64** , o **SByte**.</span><span class="sxs-lookup"><span data-stu-id="008c0-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="008c0-112">Guarda il Video</span><span class="sxs-lookup"><span data-stu-id="008c0-112">Watch the Video</span></span>
<span data-ttu-id="008c0-113">In questo video viene illustrato come utilizzare tipi enum con Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="008c0-113">This video shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="008c0-114">Viene inoltre illustrato come usare enumerazioni in una query LINQ.</span><span class="sxs-lookup"><span data-stu-id="008c0-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="008c0-115">**Presentato da**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="008c0-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="008c0-116">**Video**: [WMV](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span><span class="sxs-lookup"><span data-stu-id="008c0-116">**Video**: [WMV](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="008c0-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="008c0-117">Pre-Requisites</span></span>

<span data-ttu-id="008c0-118">È necessario disporre di Visual Studio 2012, Ultimate, Premium, Professional o Web Express edition per completare questa procedura dettagliata.</span><span class="sxs-lookup"><span data-stu-id="008c0-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="008c0-119">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="008c0-119">Set up the Project</span></span>

1.  <span data-ttu-id="008c0-120">Aprire Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="008c0-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="008c0-121">Nel **File** dal menu **New**, quindi fare clic su **progetto**</span><span class="sxs-lookup"><span data-stu-id="008c0-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="008c0-122">Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **Console** modello</span><span class="sxs-lookup"><span data-stu-id="008c0-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="008c0-123">Immettere **EnumEFDesigner** come nome del progetto e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="008c0-123">Enter **EnumEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="008c0-124">Creare un nuovo modello utilizzando la finestra di progettazione di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="008c0-124">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="008c0-125">Fare clic sul nome del progetto in Esplora soluzioni, scegliere **Add**, quindi fare clic su **nuovo elemento**</span><span class="sxs-lookup"><span data-stu-id="008c0-125">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="008c0-126">Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro dei modelli</span><span class="sxs-lookup"><span data-stu-id="008c0-126">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="008c0-127">Immettere **EnumTestModel.edmx** per il nome del file e quindi fare clic su **Add**</span><span class="sxs-lookup"><span data-stu-id="008c0-127">Enter **EnumTestModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="008c0-128">Nella pagina di procedura guidata Entity Data Model, selezionare **modello vuoto** nella finestra di dialogo Scegli contenuto Model</span><span class="sxs-lookup"><span data-stu-id="008c0-128">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="008c0-129">Fare clic su **fine**</span><span class="sxs-lookup"><span data-stu-id="008c0-129">Click **Finish**</span></span>

<span data-ttu-id="008c0-130">Entity Designer, che fornisce un'area di progettazione per la modifica del modello, viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="008c0-130">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="008c0-131">La procedura guidata consente di effettuare le azioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="008c0-131">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="008c0-132">Genera il file EnumTestModel.edmx che definisce il modello concettuale, il modello di archiviazione e il mapping tra di essi.</span><span class="sxs-lookup"><span data-stu-id="008c0-132">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="008c0-133">Imposta la proprietà Metadata Artifact Processing del file con estensione edmx da incorporare nell'Assembly di Output in modo che i file di metadati generati ottengano incorporati nell'assembly.</span><span class="sxs-lookup"><span data-stu-id="008c0-133">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="008c0-134">Aggiunge un riferimento agli assembly seguenti: EntityFramework System.ComponentModel.DataAnnotations e Data. Entity.</span><span class="sxs-lookup"><span data-stu-id="008c0-134">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="008c0-135">Crea file EnumTestModel.tt ed EnumTestModel.Context.tt e li aggiunge sotto il file con estensione edmx.</span><span class="sxs-lookup"><span data-stu-id="008c0-135">Creates EnumTestModel.tt and EnumTestModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="008c0-136">Questi file di modello T4 generano il codice che definisce il tipo DbContext derivata e i tipi POCO mappate alle entità nel modello con estensione edmx.</span><span class="sxs-lookup"><span data-stu-id="008c0-136">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model.</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="008c0-137">Aggiungere un nuovo tipo di entità</span><span class="sxs-lookup"><span data-stu-id="008c0-137">Add a New Entity Type</span></span>

1.  <span data-ttu-id="008c0-138">Fare doppio clic su un'area vuota dell'area di progettazione, seleziona **Add -&gt; entità**, viene visualizzata la finestra di dialogo nuova entità</span><span class="sxs-lookup"><span data-stu-id="008c0-138">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="008c0-139">Specificare **reparto** per il tipo di nome e specificare **DepartmentID** per il nome della proprietà chiave, lasciare il tipo **Int32**</span><span class="sxs-lookup"><span data-stu-id="008c0-139">Specify **Department** for the type name and specify **DepartmentID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="008c0-140">Fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="008c0-140">Click **OK**</span></span>
4.  <span data-ttu-id="008c0-141">L'entità e scegliere **Aggiungi nuovo -&gt; proprietà scalare**</span><span class="sxs-lookup"><span data-stu-id="008c0-141">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="008c0-142">Rinominare la nuova proprietà a **nome**</span><span class="sxs-lookup"><span data-stu-id="008c0-142">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="008c0-143">Modificare il tipo della nuova proprietà al **Int32** (per impostazione predefinita, la nuova proprietà è di tipo stringa) per modificare il tipo, aprire la finestra proprietà e modificare la proprietà Type per **Int32**</span><span class="sxs-lookup"><span data-stu-id="008c0-143">Change the type of the new property to **Int32** (by default, the new property is of String type) To change the type, open the Properties window and change the Type property to **Int32**</span></span>
7.  <span data-ttu-id="008c0-144">Aggiungere un'altra proprietà scalare e rinominarlo **Budget**, impostare il tipo su **decimale**</span><span class="sxs-lookup"><span data-stu-id="008c0-144">Add another scalar property and rename it to **Budget**, change the type to **Decimal**</span></span>

## <a name="add-an-enum-type"></a><span data-ttu-id="008c0-145">Aggiungere un tipo Enum</span><span class="sxs-lookup"><span data-stu-id="008c0-145">Add an Enum Type</span></span>

1.  <span data-ttu-id="008c0-146">In Entity Framework Designer, fare clic su proprietà Name, selezionare **convertire all'enumerazione**</span><span class="sxs-lookup"><span data-stu-id="008c0-146">In the Entity Framework Designer, right-click the Name property, select **Convert to enum**</span></span>

    ![ConvertToEnum](~/ef6/media/converttoenum.png)

2.  <span data-ttu-id="008c0-148">Nel **Enum aggiungere** finestra di dialogo digitare **DepartmentNames** per nome del tipo di enumerazione, modificare il tipo sottostante per **Int32**, e quindi aggiungere i membri seguenti per il tipo: inglese, Matematiche e la convenienza</span><span class="sxs-lookup"><span data-stu-id="008c0-148">In the **Add Enum** dialog box type **DepartmentNames** for the Enum Type Name, change the Underlying Type to **Int32**, and then add the following members to the type: English, Math, and Economics</span></span>

    ![AddEnumType](~/ef6/media/addenumtype.png)

3.  <span data-ttu-id="008c0-150">Premere **OK**</span><span class="sxs-lookup"><span data-stu-id="008c0-150">Press **OK**</span></span>
4.  <span data-ttu-id="008c0-151">Salvare il modello e compilare il progetto</span><span class="sxs-lookup"><span data-stu-id="008c0-151">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="008c0-152">Quando si compila, avvisi relativi a entità non mappata e associazioni vengano visualizzati nell'elenco errori.</span><span class="sxs-lookup"><span data-stu-id="008c0-152">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="008c0-153">È possibile ignorare questi avvisi perché dopo che si sceglie di generare il database dal modello, gli errori non verranno più visualizzato.</span><span class="sxs-lookup"><span data-stu-id="008c0-153">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

<span data-ttu-id="008c0-154">Se si osserva la finestra Proprietà, si noterà che il tipo della proprietà del nome sia stato modificato **DepartmentNames** e il tipo di enumerazione appena aggiunta è stata aggiunta all'elenco dei tipi.</span><span class="sxs-lookup"><span data-stu-id="008c0-154">If you look at the Properties window, you will notice that the type of the Name property was changed to **DepartmentNames** and the newly added enum type was added to the list of types.</span></span>

<span data-ttu-id="008c0-155">Se si passa alla finestra Browser modello, si noterà che il tipo è stato inoltre aggiunto il nodo di tipi di enumerazione.</span><span class="sxs-lookup"><span data-stu-id="008c0-155">If you switch to the Model Browser window, you will see that the type was also added to the Enum Types node.</span></span>

![Modelbrowser non](~/ef6/media/modelbrowser.png)

>[!NOTE]
> <span data-ttu-id="008c0-157">È anche possibile aggiungere nuovi tipi di enumerazione da questa finestra facendo clic sul pulsante destro del mouse e selezionando **Aggiungi tipo Enum**.</span><span class="sxs-lookup"><span data-stu-id="008c0-157">You can also add new enum types from this window by clicking the right mouse button and selecting **Add Enum Type**.</span></span> <span data-ttu-id="008c0-158">Dopo aver creato il tipo verrà visualizzato nell'elenco dei tipi e sarebbe in grado di associare a una proprietà</span><span class="sxs-lookup"><span data-stu-id="008c0-158">Once the type is created it will appear in the list of types and you would be able to associate with a property</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="008c0-159">Generare il Database da modello</span><span class="sxs-lookup"><span data-stu-id="008c0-159">Generate Database from Model</span></span>

<span data-ttu-id="008c0-160">A questo punto è possibile generare un database che si basa sul modello.</span><span class="sxs-lookup"><span data-stu-id="008c0-160">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="008c0-161">Fare doppio clic su uno spazio vuoto in Entity Designer e selezionare **genera Database da modello**</span><span class="sxs-lookup"><span data-stu-id="008c0-161">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="008c0-162">Scegliere la finestra di dialogo di connessione dati della procedura guidata genera Database viene visualizzato fare clic sui **nuova connessione** pulsante specificare **(localdb)\\mssqllocaldb** per il nome del server e la  **EnumTest** per il database e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="008c0-162">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **EnumTest** for the database and click **OK**</span></span>
3.  <span data-ttu-id="008c0-163">Verrà visualizzata, fare clic su una finestra di dialogo che chiede se si desidera creare un nuovo database **Sì**.</span><span class="sxs-lookup"><span data-stu-id="008c0-163">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="008c0-164">Fare clic su **successivo** e la procedura guidata Crea Database genera data definition language (DDL) per la creazione di un database il linguaggio DDL generato viene visualizzata nel riepilogo delle impostazioni di finestra di dialogo casella si noti che l'istruzione DDL non contiene una definizione per un tabella che esegue il mapping al tipo di enumerazione</span><span class="sxs-lookup"><span data-stu-id="008c0-164">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="008c0-165">Fare clic su **fine** facendo clic su Fine non viene eseguito lo script DDL.</span><span class="sxs-lookup"><span data-stu-id="008c0-165">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="008c0-166">La procedura guidata Crea Database esegue le operazioni seguenti: apre la **EnumTest.edmx.sql** nell'Editor T-SQL genera le sezioni di mapping dello schema e archivio dell'elemento EDMX file consente di aggiungere informazioni sulla stringa di connessione del file app. config</span><span class="sxs-lookup"><span data-stu-id="008c0-166">The Create Database Wizard does the following: Opens the **EnumTest.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="008c0-167">Scegliere il pulsante destro del mouse nell'Editor T-SQL e selezionare **Execute** Connetti al finestra di dialogo Server visualizzata, immettere le informazioni di connessione del passaggio 2 e fare clic su **Connect**</span><span class="sxs-lookup"><span data-stu-id="008c0-167">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="008c0-168">Per visualizzare lo schema generato, fare doppio clic sul nome del database in Esplora oggetti di SQL Server e selezionare **Aggiorna**</span><span class="sxs-lookup"><span data-stu-id="008c0-168">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="008c0-169">Salvare in modo permanente e recuperare i dati</span><span class="sxs-lookup"><span data-stu-id="008c0-169">Persist and Retrieve Data</span></span>

<span data-ttu-id="008c0-170">Aprire il file Program.cs in cui è definito il metodo Main.</span><span class="sxs-lookup"><span data-stu-id="008c0-170">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="008c0-171">Aggiungere il codice seguente nella funzione Main.</span><span class="sxs-lookup"><span data-stu-id="008c0-171">Add the following code into the Main function.</span></span> <span data-ttu-id="008c0-172">Il codice aggiunge un nuovo oggetto reparto al contesto.</span><span class="sxs-lookup"><span data-stu-id="008c0-172">The code adds a new Department object to the context.</span></span> <span data-ttu-id="008c0-173">Quindi Salva i dati.</span><span class="sxs-lookup"><span data-stu-id="008c0-173">It then saves the data.</span></span> <span data-ttu-id="008c0-174">Inoltre, il codice esegue una query LINQ che restituisce un reparto dove name è DepartmentNames.English.</span><span class="sxs-lookup"><span data-stu-id="008c0-174">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="008c0-175">Compilare l'applicazione ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="008c0-175">Compile and run the application.</span></span> <span data-ttu-id="008c0-176">Il programma produce l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="008c0-176">The program produces the following output:</span></span>

```
DepartmentID: 1 Name: English
```

<span data-ttu-id="008c0-177">Per visualizzare i dati nel database, fare doppio clic sul nome del database in Esplora oggetti di SQL Server e selezionare **Aggiorna**.</span><span class="sxs-lookup"><span data-stu-id="008c0-177">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="008c0-178">Quindi, scegliere il pulsante destro del mouse sulla tabella e selezionare **dati della visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="008c0-178">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="008c0-179">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="008c0-179">Summary</span></span>

<span data-ttu-id="008c0-180">In questa procedura dettagliata è stato illustrato come eseguire il mapping di tipi di enumerazione mediante Entity Framework Designer e su come usare le enumerazioni nel codice.</span><span class="sxs-lookup"><span data-stu-id="008c0-180">In this walkthrough we looked at how to map enum types using the Entity Framework Designer and how to use enums in code.</span></span> 
