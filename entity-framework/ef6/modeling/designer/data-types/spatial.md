---
title: Spaziali - Entity Framework Designer - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
caps.latest.revision: 3
ms.openlocfilehash: 2332818275763fd90274f426b7fced4c3c14ac17
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121323"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="12fa2-102">Spaziali - finestra di progettazione di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="12fa2-102">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="12fa2-103">**EF5 e versioni successive solo** -le funzionalità, le API, e così via illustrati in questa pagina sono stati introdotti in Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="12fa2-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="12fa2-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="12fa2-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="12fa2-105">La procedura dettagliata video e procedura dettagliata illustra come eseguire il mapping di tipi di dati spaziali con Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="12fa2-105">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="12fa2-106">Viene inoltre illustrato come utilizzare una query LINQ per trovare una distanza tra due posizioni.</span><span class="sxs-lookup"><span data-stu-id="12fa2-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="12fa2-107">Questa procedura dettagliata verrà utilizzato il primo modello per creare un nuovo database, ma la finestra di progettazione di Entity Framework è anche utilizzabile con il [Database First](~/ef6/modeling/designer/workflows/database-first.md) flusso di lavoro per eseguire il mapping a un database esistente.</span><span class="sxs-lookup"><span data-stu-id="12fa2-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="12fa2-108">Supporto per il tipo spaziale è stata introdotta in Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="12fa2-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="12fa2-109">Si noti che per usare le nuove funzionalità, ad esempio tipo spaziale, enumerazioni e funzioni con valori di tabella, deve avere come destinazione .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="12fa2-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="12fa2-110">Visual Studio 2012 è destinato a .NET 4.5 per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="12fa2-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="12fa2-111">Per utilizzare i tipi di dati spaziali è anche necessario usare un provider di Entity Framework con supporto spaziale.</span><span class="sxs-lookup"><span data-stu-id="12fa2-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="12fa2-112">Visualizzare [supporto del provider per i tipi spaziali](~/ef6/fundamentals/providers/spatial-support.md) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="12fa2-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="12fa2-113">Esistono due tipi di dati spaziali principale: geografia e geometria.</span><span class="sxs-lookup"><span data-stu-id="12fa2-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="12fa2-114">Il tipo di dati geography archivia dati ellissoidali (ad esempio, cui è coordinate GPS latitudine e longitudine).</span><span class="sxs-lookup"><span data-stu-id="12fa2-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="12fa2-115">Il tipo di dati geometry rappresenta un sistema di coordinate Euclideo (piano).</span><span class="sxs-lookup"><span data-stu-id="12fa2-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="12fa2-116">Guarda il video</span><span class="sxs-lookup"><span data-stu-id="12fa2-116">Watch the video</span></span>
<span data-ttu-id="12fa2-117">In questo video viene illustrato come eseguire il mapping di tipi di dati spaziali con Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="12fa2-117">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="12fa2-118">Viene inoltre illustrato come utilizzare una query LINQ per trovare una distanza tra due posizioni.</span><span class="sxs-lookup"><span data-stu-id="12fa2-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="12fa2-119">**Presentato da**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="12fa2-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="12fa2-120">**Video**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="12fa2-120">**Video**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="12fa2-121">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="12fa2-121">Pre-Requisites</span></span>

<span data-ttu-id="12fa2-122">È necessario disporre di Visual Studio 2012, Ultimate, Premium, Professional o Web Express edition per completare questa procedura dettagliata.</span><span class="sxs-lookup"><span data-stu-id="12fa2-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="12fa2-123">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="12fa2-123">Set up the Project</span></span>

1.  <span data-ttu-id="12fa2-124">Aprire Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="12fa2-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="12fa2-125">Nel **File** dal menu **New**, quindi fare clic su **progetto**</span><span class="sxs-lookup"><span data-stu-id="12fa2-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="12fa2-126">Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **Console** modello</span><span class="sxs-lookup"><span data-stu-id="12fa2-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="12fa2-127">Immettere **SpatialEFDesigner** come nome del progetto e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="12fa2-127">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="12fa2-128">Creare un nuovo modello utilizzando la finestra di progettazione di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="12fa2-128">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="12fa2-129">Fare clic sul nome del progetto in Esplora soluzioni, scegliere **Add**, quindi fare clic su **nuovo elemento**</span><span class="sxs-lookup"><span data-stu-id="12fa2-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="12fa2-130">Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro dei modelli</span><span class="sxs-lookup"><span data-stu-id="12fa2-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="12fa2-131">Immettere **UniversityModel.edmx** per il nome del file e quindi fare clic su **Add**</span><span class="sxs-lookup"><span data-stu-id="12fa2-131">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="12fa2-132">Nella pagina di procedura guidata Entity Data Model, selezionare **modello vuoto** nella finestra di dialogo Scegli contenuto Model</span><span class="sxs-lookup"><span data-stu-id="12fa2-132">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="12fa2-133">Fare clic su **fine**</span><span class="sxs-lookup"><span data-stu-id="12fa2-133">Click **Finish**</span></span>

<span data-ttu-id="12fa2-134">Entity Designer, che fornisce un'area di progettazione per la modifica del modello, viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="12fa2-134">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="12fa2-135">La procedura guidata consente di effettuare le azioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="12fa2-135">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="12fa2-136">Genera il file EnumTestModel.edmx che definisce il modello concettuale, il modello di archiviazione e il mapping tra di essi.</span><span class="sxs-lookup"><span data-stu-id="12fa2-136">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="12fa2-137">Imposta la proprietà Metadata Artifact Processing del file con estensione edmx da incorporare nell'Assembly di Output in modo che i file di metadati generati ottengano incorporati nell'assembly.</span><span class="sxs-lookup"><span data-stu-id="12fa2-137">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="12fa2-138">Aggiunge un riferimento agli assembly seguenti: EntityFramework System.ComponentModel.DataAnnotations e Data. Entity.</span><span class="sxs-lookup"><span data-stu-id="12fa2-138">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="12fa2-139">Crea file UniversityModel.tt e UniversityModel.Context.tt e li aggiunge sotto il file con estensione edmx.</span><span class="sxs-lookup"><span data-stu-id="12fa2-139">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="12fa2-140">Questi file di modello T4 generano il codice che definisce il tipo DbContext derivata e i tipi POCO mappate alle entità nel modello con estensione edmx</span><span class="sxs-lookup"><span data-stu-id="12fa2-140">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="12fa2-141">Aggiungere un nuovo tipo di entità</span><span class="sxs-lookup"><span data-stu-id="12fa2-141">Add a New Entity Type</span></span>

1.  <span data-ttu-id="12fa2-142">Fare doppio clic su un'area vuota dell'area di progettazione, seleziona **Add -&gt; entità**, viene visualizzata la finestra di dialogo nuova entità</span><span class="sxs-lookup"><span data-stu-id="12fa2-142">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="12fa2-143">Specificare **University** per il tipo di nome e specificare **UniversityID una** per il nome della proprietà chiave, lasciare il tipo **Int32**</span><span class="sxs-lookup"><span data-stu-id="12fa2-143">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="12fa2-144">Fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="12fa2-144">Click **OK**</span></span>
4.  <span data-ttu-id="12fa2-145">L'entità e scegliere **Aggiungi nuovo -&gt; proprietà scalare**</span><span class="sxs-lookup"><span data-stu-id="12fa2-145">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="12fa2-146">Rinominare la nuova proprietà a **nome**</span><span class="sxs-lookup"><span data-stu-id="12fa2-146">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="12fa2-147">Aggiungere un'altra proprietà scalare e rinominarlo **ubicazione** aprire la finestra proprietà e modificare il tipo della nuova proprietà a **Geography**</span><span class="sxs-lookup"><span data-stu-id="12fa2-147">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="12fa2-148">Salvare il modello e compilare il progetto</span><span class="sxs-lookup"><span data-stu-id="12fa2-148">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="12fa2-149">Quando si compila, avvisi relativi a entità non mappata e associazioni vengano visualizzati nell'elenco errori.</span><span class="sxs-lookup"><span data-stu-id="12fa2-149">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="12fa2-150">È possibile ignorare questi avvisi perché dopo che si sceglie di generare il database dal modello, gli errori non verranno più visualizzato.</span><span class="sxs-lookup"><span data-stu-id="12fa2-150">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="12fa2-151">Generare il Database da modello</span><span class="sxs-lookup"><span data-stu-id="12fa2-151">Generate Database from Model</span></span>

<span data-ttu-id="12fa2-152">A questo punto è possibile generare un database che si basa sul modello.</span><span class="sxs-lookup"><span data-stu-id="12fa2-152">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="12fa2-153">Fare doppio clic su uno spazio vuoto in Entity Designer e selezionare **genera Database da modello**</span><span class="sxs-lookup"><span data-stu-id="12fa2-153">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="12fa2-154">Scegliere la finestra di dialogo di connessione dati della procedura guidata genera Database viene visualizzato fare clic sui **nuova connessione** pulsante specificare **(localdb)\\mssqllocaldb** per il nome del server e la  **Università** per il database e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="12fa2-154">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="12fa2-155">Verrà visualizzata, fare clic su una finestra di dialogo che chiede se si desidera creare un nuovo database **Sì**.</span><span class="sxs-lookup"><span data-stu-id="12fa2-155">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="12fa2-156">Fare clic su **successivo** e la procedura guidata Crea Database genera data definition language (DDL) per la creazione di un database il linguaggio DDL generato viene visualizzata nel riepilogo delle impostazioni di finestra di dialogo casella si noti che l'istruzione DDL non contiene una definizione per un tabella che esegue il mapping al tipo di enumerazione</span><span class="sxs-lookup"><span data-stu-id="12fa2-156">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="12fa2-157">Fare clic su **fine** facendo clic su Fine non viene eseguito lo script DDL.</span><span class="sxs-lookup"><span data-stu-id="12fa2-157">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="12fa2-158">La procedura guidata Crea Database esegue le operazioni seguenti: apre la **UniversityModel.edmx.sql** nell'Editor T-SQL genera le sezioni di mapping dello schema e archivio dell'elemento EDMX file consente di aggiungere informazioni sulla stringa di connessione del file app. config</span><span class="sxs-lookup"><span data-stu-id="12fa2-158">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="12fa2-159">Scegliere il pulsante destro del mouse nell'Editor T-SQL e selezionare **Execute** Connetti al finestra di dialogo Server visualizzata, immettere le informazioni di connessione del passaggio 2 e fare clic su **Connect**</span><span class="sxs-lookup"><span data-stu-id="12fa2-159">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="12fa2-160">Per visualizzare lo schema generato, fare doppio clic sul nome del database in Esplora oggetti di SQL Server e selezionare **Aggiorna**</span><span class="sxs-lookup"><span data-stu-id="12fa2-160">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="12fa2-161">Salvare in modo permanente e recuperare i dati</span><span class="sxs-lookup"><span data-stu-id="12fa2-161">Persist and Retrieve Data</span></span>

<span data-ttu-id="12fa2-162">Aprire il file Program.cs in cui è definito il metodo Main.</span><span class="sxs-lookup"><span data-stu-id="12fa2-162">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="12fa2-163">Aggiungere il codice seguente nella funzione Main.</span><span class="sxs-lookup"><span data-stu-id="12fa2-163">Add the following code into the Main function.</span></span>

<span data-ttu-id="12fa2-164">Il codice aggiunge due nuovi oggetti University al contesto.</span><span class="sxs-lookup"><span data-stu-id="12fa2-164">The code adds two new University objects to the context.</span></span> <span data-ttu-id="12fa2-165">Proprietà spaziali vengono inizializzate tramite il metodo DbGeography.FromText.</span><span class="sxs-lookup"><span data-stu-id="12fa2-165">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="12fa2-166">Il punto di geografia è rappresentato come WellKnownText viene passato al metodo.</span><span class="sxs-lookup"><span data-stu-id="12fa2-166">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="12fa2-167">Il codice quindi Salva i dati.</span><span class="sxs-lookup"><span data-stu-id="12fa2-167">The code then saves the data.</span></span> <span data-ttu-id="12fa2-168">Quindi, LINQ query che restituisce un oggetto University dove il percorso è più vicino alla posizione specificata, viene costruita ed eseguita.</span><span class="sxs-lookup"><span data-stu-id="12fa2-168">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
    {
        Name = "School of Fine Art",
        Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
    });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                                orderby u.Location.Distance(myLocation)
                                select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

<span data-ttu-id="12fa2-169">Compilare l'applicazione ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="12fa2-169">Compile and run the application.</span></span> <span data-ttu-id="12fa2-170">Il programma produce l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="12fa2-170">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="12fa2-171">Per visualizzare i dati nel database, fare doppio clic sul nome del database in Esplora oggetti di SQL Server e selezionare **Aggiorna**.</span><span class="sxs-lookup"><span data-stu-id="12fa2-171">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="12fa2-172">Quindi, scegliere il pulsante destro del mouse sulla tabella e selezionare **dati della visualizzazione**.</span><span class="sxs-lookup"><span data-stu-id="12fa2-172">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="12fa2-173">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="12fa2-173">Summary</span></span>

<span data-ttu-id="12fa2-174">In questa procedura dettagliata è stato illustrato come eseguire il mapping di tipi spaziali usando Entity Framework Designer e su come usare i tipi spaziali nel codice.</span><span class="sxs-lookup"><span data-stu-id="12fa2-174">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 
