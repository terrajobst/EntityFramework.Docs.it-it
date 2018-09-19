---
title: Query della finestra di progettazione di Stored procedure - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 04478ea1c8cd43a7ba4ee788e464992af3de7f64
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283901"
---
# <a name="designer-query-stored-procedures"></a><span data-ttu-id="574e2-102">Query della finestra di progettazione di Stored procedure</span><span class="sxs-lookup"><span data-stu-id="574e2-102">Designer Query Stored Procedures</span></span>
<span data-ttu-id="574e2-103">Questa procedura dettagliata mostra come usare Entity Framework Designer (Entity Framework Designer) per importare le stored procedure in un modello e quindi chiamare le stored procedure importate per recuperare i risultati.</span><span class="sxs-lookup"><span data-stu-id="574e2-103">This step-by-step walkthrough show how to use the Entity Framework Designer (EF Designer) to import stored procedures into a model and then call the imported stored procedures to retrieve results.</span></span> 

<span data-ttu-id="574e2-104">Si noti che Code First non supporta il mapping a stored procedure o funzioni.</span><span class="sxs-lookup"><span data-stu-id="574e2-104">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="574e2-105">Tuttavia, è possibile chiamare stored procedure o funzioni tramite il metodo System.Data.Entity.DbSet.SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="574e2-105">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="574e2-106">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="574e2-106">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a><span data-ttu-id="574e2-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="574e2-107">Prerequisites</span></span>

<span data-ttu-id="574e2-108">Per completare questa procedura dettagliata, è necessario disporre di:</span><span class="sxs-lookup"><span data-stu-id="574e2-108">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="574e2-109">Una versione recente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="574e2-109">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="574e2-110">Il [database di esempio School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="574e2-110">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="574e2-111">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="574e2-111">Set up the Project</span></span>

-   <span data-ttu-id="574e2-112">Aprire Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="574e2-112">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="574e2-113">Selezionare **File -&gt; Novità -&gt; progetto**</span><span class="sxs-lookup"><span data-stu-id="574e2-113">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="574e2-114">Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **Console** modello.</span><span class="sxs-lookup"><span data-stu-id="574e2-114">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="574e2-115">Immettere **EFwithSProcsSample** come nome.</span><span class="sxs-lookup"><span data-stu-id="574e2-115">Enter **EFwithSProcsSample** as the name.</span></span>
-   <span data-ttu-id="574e2-116">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="574e2-116">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="574e2-117">Creare un modello</span><span class="sxs-lookup"><span data-stu-id="574e2-117">Create a Model</span></span>

-   <span data-ttu-id="574e2-118">Fare clic sul progetto in Esplora soluzioni e selezionare **Add -&gt; nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="574e2-118">Right-click the project in Solution Explorer and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="574e2-119">Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro dei modelli.</span><span class="sxs-lookup"><span data-stu-id="574e2-119">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="574e2-120">Immettere **EFwithSProcsModel.edmx** per il nome del file e quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="574e2-120">Enter **EFwithSProcsModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="574e2-121">Nella finestra di dialogo Scegli contenuto Model, selezionare **genera da database**, quindi fare clic su **successivo**.</span><span class="sxs-lookup"><span data-stu-id="574e2-121">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="574e2-122">Fare clic su **nuova connessione**.</span><span class="sxs-lookup"><span data-stu-id="574e2-122">Click **New Connection**.</span></span>  
    <span data-ttu-id="574e2-123">Nella finestra di dialogo proprietà di connessione, immettere il nome del server (ad esempio, **(localdb)\\mssqllocaldb**), selezionare il metodo di autenticazione, il tipo **School** per il nome del database e quindi Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="574e2-123">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>  
    <span data-ttu-id="574e2-124">La finestra di dialogo Seleziona connessione dati viene aggiornata con l'impostazione di connessione di database.</span><span class="sxs-lookup"><span data-stu-id="574e2-124">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="574e2-125">Nella finestra di dialogo Scegli oggetti di Database, verificare i **tabelle** casella di controllo per selezionare tutte le tabelle.</span><span class="sxs-lookup"><span data-stu-id="574e2-125">In the Choose Your Database Objects dialog box, check the **Tables** checkbox to select all the tables.</span></span>  
    <span data-ttu-id="574e2-126">Selezionare anche le seguenti stored procedure con il **funzioni e Stored procedure** nodo: **GetStudentGrades** e **GetDepartmentName**.</span><span class="sxs-lookup"><span data-stu-id="574e2-126">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **GetStudentGrades** and **GetDepartmentName**.</span></span> 

    ![Import](~/ef6/media/import.jpg)

    <span data-ttu-id="574e2-128">*Partire da Visual Studio 2012 nella finestra di progettazione di Entity Framework supporta l'importazione bulk di stored procedure. Il **Importa selezionate stored procedure e funzioni nel modello theentity** è selezionata per impostazione predefinita.*</span><span class="sxs-lookup"><span data-stu-id="574e2-128">*Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures. The **Import selected stored procedures and functions into theentity model** is checked by default.*</span></span>
-   <span data-ttu-id="574e2-129">Scegliere **Fine**.</span><span class="sxs-lookup"><span data-stu-id="574e2-129">Click **Finish**.</span></span>

<span data-ttu-id="574e2-130">Per impostazione predefinita, la forma dei risultati di ogni importate stored procedure o funzione che restituisce più di una colonna verrà portato automaticamente un nuovo tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="574e2-130">By default, the result shape of each imported stored procedure or function that returns more than one column will automatically become a new complex type.</span></span> <span data-ttu-id="574e2-131">In questo esempio si vuole mappare i risultati del **GetStudentGrades** funzionare per il **StudentGrade** entità e i risultati del **GetDepartmentName** a **Nessuna** (**none** è il valore predefinito).</span><span class="sxs-lookup"><span data-stu-id="574e2-131">In this example we want to map the results of the **GetStudentGrades** function to the **StudentGrade** entity and the results of the **GetDepartmentName** to **none** (**none** is the default value).</span></span>

<span data-ttu-id="574e2-132">Per un'importazione di funzioni restituire un tipo di entità, le colonne restituite dalla stored procedure corrispondente devono corrispondere esattamente le proprietà scalari del tipo di entità restituite.</span><span class="sxs-lookup"><span data-stu-id="574e2-132">For a function import to return an entity type, the columns returned by the corresponding stored procedure must exactly match the scalar properties of the returned entity type.</span></span> <span data-ttu-id="574e2-133">Un'importazione di funzioni può inoltre restituire raccolte di tipi semplici, tipi complessi o nessun valore.</span><span class="sxs-lookup"><span data-stu-id="574e2-133">A function import can also return collections of simple types, complex types, or no value.</span></span>

-   <span data-ttu-id="574e2-134">Fare doppio clic su area di progettazione e seleziona **Browser modello**.</span><span class="sxs-lookup"><span data-stu-id="574e2-134">Right-click the design surface and select **Model Browser**.</span></span>
-   <span data-ttu-id="574e2-135">In **Visualizzatore modelli**, selezionare **importazioni di funzioni**e quindi fare doppio clic il **GetStudentGrades** (funzione).</span><span class="sxs-lookup"><span data-stu-id="574e2-135">In **Model Browser**, select **Function Imports**, and then double-click the **GetStudentGrades** function.</span></span>
-   <span data-ttu-id="574e2-136">Nella finestra di dialogo Modifica importazione di funzioni, selezionare **Entities** e scegliere **StudentGrade**.</span><span class="sxs-lookup"><span data-stu-id="574e2-136">In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**.</span></span>  
    <span data-ttu-id="574e2-137">*Il **importazione della funzione è componibile** casella di controllo nella parte superiore delle **importazioni di funzioni** finestra di dialogo consente di eseguire il mapping di funzioni componibile. Se si seleziona questa casella, verranno visualizzato solo funzioni componibili (funzioni con valori di tabella) nei **Stored Procedure / nome funzione** elenco a discesa. Se non si seleziona questa casella, solo le funzioni non componibili verranno visualizzate nell'elenco.*</span><span class="sxs-lookup"><span data-stu-id="574e2-137">*The **Function Import is composable** checkbox at the top of the **Function Imports** dialog will let you map to composable functions. If you do check this box, only composable functions (Table-valued Functions) will appear in the **Stored Procedure / Function Name** drop-down list. If you do not check this box, only non-composable functions will be shown in the list.*</span></span>

## <a name="use-the-model"></a><span data-ttu-id="574e2-138">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="574e2-138">Use the Model</span></span>

<span data-ttu-id="574e2-139">Aprire il **Program.cs** file dove il **Main** metodo è definito.</span><span class="sxs-lookup"><span data-stu-id="574e2-139">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="574e2-140">Aggiungere il codice seguente nella funzione Main.</span><span class="sxs-lookup"><span data-stu-id="574e2-140">Add the following code into the Main function.</span></span>

<span data-ttu-id="574e2-141">Il codice chiama due stored procedure: **GetStudentGrades** (restituisce **StudentGrades** per l'oggetto specificato *StudentId*) e **GetDepartmentName** (restituisce il nome del dipartimento nel parametro di output).</span><span class="sxs-lookup"><span data-stu-id="574e2-141">The code calls two stored procedures: **GetStudentGrades** (returns **StudentGrades** for the specified *StudentId*) and **GetDepartmentName** (returns the name of the department in the output parameter).</span></span>  

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        // Specify the Student ID.
        int studentId = 2;

        // Call GetStudentGrades and iterate through the returned collection.
        foreach (StudentGrade grade in context.GetStudentGrades(studentId))
        {
            Console.WriteLine("StudentID: {0}\tSubject={1}", studentId, grade.Subject);
            Console.WriteLine("Student grade: " + grade.Grade);
        }

        // Call GetDepartmentName.
        // Declare the name variable that will contain the value returned by the output parameter.
        ObjectParameter name = new ObjectParameter("Name", typeof(String));
        context.GetDepartmentName(1, name);
        Console.WriteLine("The department name is {0}", name.Value);

    }
```

<span data-ttu-id="574e2-142">Compilare l'applicazione ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="574e2-142">Compile and run the application.</span></span> <span data-ttu-id="574e2-143">Il programma produce l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="574e2-143">The program produces the following output:</span></span>

```
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a><span data-ttu-id="574e2-144">Parametri di output</span><span class="sxs-lookup"><span data-stu-id="574e2-144">Output Parameters</span></span>
-----------------

<span data-ttu-id="574e2-145">Se si utilizzano parametri di output, i relativi valori non sarà disponibili fino a quando i risultati sono stati letti completamente.</span><span class="sxs-lookup"><span data-stu-id="574e2-145">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="574e2-146">Ciò è dovuto il comportamento sottostante di DbDataReader, vedere [recupero di dati mediante DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="574e2-146">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>
