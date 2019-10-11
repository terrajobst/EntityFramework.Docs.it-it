---
title: Stored procedure per query della finestra di progettazione-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 2e0092b526278597e8477d47eeb642598647bb91
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182475"
---
# <a name="designer-query-stored-procedures"></a><span data-ttu-id="0e965-102">Stored procedure di query della finestra di progettazione</span><span class="sxs-lookup"><span data-stu-id="0e965-102">Designer Query Stored Procedures</span></span>
<span data-ttu-id="0e965-103">In questa procedura dettagliata viene illustrato come utilizzare il Entity Framework Designer (Entity Designer) per importare stored procedure in un modello e quindi chiamare le stored procedure importate per recuperare i risultati.</span><span class="sxs-lookup"><span data-stu-id="0e965-103">This step-by-step walkthrough show how to use the Entity Framework Designer (EF Designer) to import stored procedures into a model and then call the imported stored procedures to retrieve results.</span></span> 

<span data-ttu-id="0e965-104">Si noti che Code First non supporta il mapping a stored procedure o funzioni.</span><span class="sxs-lookup"><span data-stu-id="0e965-104">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="0e965-105">Tuttavia, è possibile chiamare stored procedure o funzioni utilizzando il metodo System. Data. Entity. DbSet. sqlQuery.</span><span class="sxs-lookup"><span data-stu-id="0e965-105">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="0e965-106">Esempio:</span><span class="sxs-lookup"><span data-stu-id="0e965-106">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a><span data-ttu-id="0e965-107">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="0e965-107">Prerequisites</span></span>

<span data-ttu-id="0e965-108">Per completare questa procedura dettagliata, è necessario disporre di:</span><span class="sxs-lookup"><span data-stu-id="0e965-108">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="0e965-109">Una versione recente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0e965-109">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="0e965-110">[Database di esempio School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="0e965-110">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="0e965-111">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="0e965-111">Set up the Project</span></span>

-   <span data-ttu-id="0e965-112">Aprire Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="0e965-112">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="0e965-113">Selezionare il **progetto file-&gt; New-&gt;**</span><span class="sxs-lookup"><span data-stu-id="0e965-113">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="0e965-114">Nel riquadro sinistro fare clic su **Visual C @ no__t-1**, quindi selezionare il modello **console** .</span><span class="sxs-lookup"><span data-stu-id="0e965-114">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="0e965-115">Immettere **EFwithSProcsSample**@no__t-aggiungere1come il nome.</span><span class="sxs-lookup"><span data-stu-id="0e965-115">Enter **EFwithSProcsSample** as the name.</span></span>
-   <span data-ttu-id="0e965-116">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="0e965-116">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="0e965-117">Creazione di un modello</span><span class="sxs-lookup"><span data-stu-id="0e965-117">Create a Model</span></span>

-   <span data-ttu-id="0e965-118">Fare clic con il pulsante destro del mouse sul progetto Esplora soluzioni e scegliere **Aggiungi-&gt; nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="0e965-118">Right-click the project in Solution Explorer and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="0e965-119">Selezionare **dati** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro modelli.</span><span class="sxs-lookup"><span data-stu-id="0e965-119">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="0e965-120">Immettere **EFwithSProcsModel. edmx** per il nome del file e quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="0e965-120">Enter **EFwithSProcsModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="0e965-121">Nella finestra di dialogo Scegli contenuto Model selezionare **genera da database**, quindi fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="0e965-121">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="0e965-122">Fare clic su **nuova connessione**.</span><span class="sxs-lookup"><span data-stu-id="0e965-122">Click **New Connection**.</span></span>  
    <span data-ttu-id="0e965-123">Nella finestra di dialogo Proprietà connessione immettere il nome del server (ad esempio, **(local DB) \\mssqllocaldb**), selezionare il metodo di autenticazione, digitare **School** for il nome del database, quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="0e965-123">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>  
    <span data-ttu-id="0e965-124">La finestra di dialogo scegliere la connessione dati viene aggiornata con l'impostazione di connessione al database.</span><span class="sxs-lookup"><span data-stu-id="0e965-124">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="0e965-125">Nella finestra di dialogo Scegli oggetti di database selezionare le **tabelle** checkbox per selezionare tutte le tabelle.</span><span class="sxs-lookup"><span data-stu-id="0e965-125">In the Choose Your Database Objects dialog box, check the **Tables** checkbox to select all the tables.</span></span>  
    <span data-ttu-id="0e965-126">Inoltre, nel nodo **stored procedure e funzioni** selezionare le stored procedure seguenti: **GetStudentGrades** e **GetDepartmentName**.</span><span class="sxs-lookup"><span data-stu-id="0e965-126">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **GetStudentGrades** and **GetDepartmentName**.</span></span> 

    ![Import](~/ef6/media/import.jpg)

    <span data-ttu-id="0e965-128">*Starting con Visual Studio 2012 la finestra di progettazione EF supporta l'importazione bulk di stored procedure. Per impostazione predefinita, l' **importazione delle stored procedure e delle funzioni selezionate nel modello set** è selezionata.*</span><span class="sxs-lookup"><span data-stu-id="0e965-128">*Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures. The **Import selected stored procedures and functions into theentity model** is checked by default.*</span></span>
-   <span data-ttu-id="0e965-129">Fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="0e965-129">Click **Finish**.</span></span>

<span data-ttu-id="0e965-130">Per impostazione predefinita, la forma risultante di ogni stored procedure o funzione importata che restituisce più colonne diventerà automaticamente un nuovo tipo complesso.</span><span class="sxs-lookup"><span data-stu-id="0e965-130">By default, the result shape of each imported stored procedure or function that returns more than one column will automatically become a new complex type.</span></span> <span data-ttu-id="0e965-131">In questo esempio si vuole eseguire il mapping dei risultati della funzione **GetStudentGrades** all'entità **StudentGrade** e i risultati di **GetDepartmentName** su **None** (**nessuno** è il valore predefinito).</span><span class="sxs-lookup"><span data-stu-id="0e965-131">In this example we want to map the results of the **GetStudentGrades** function to the **StudentGrade** entity and the results of the **GetDepartmentName** to **none** (**none** is the default value).</span></span>

<span data-ttu-id="0e965-132">Affinché un'importazione di funzioni restituisca un tipo di entità, le colonne restituite dalla stored procedure corrispondente devono corrispondere esattamente alle proprietà scalari del tipo di entità restituito.</span><span class="sxs-lookup"><span data-stu-id="0e965-132">For a function import to return an entity type, the columns returned by the corresponding stored procedure must exactly match the scalar properties of the returned entity type.</span></span> <span data-ttu-id="0e965-133">Un'importazione di funzioni può inoltre restituire raccolte di tipi semplici, tipi complessi o nessun valore.</span><span class="sxs-lookup"><span data-stu-id="0e965-133">A function import can also return collections of simple types, complex types, or no value.</span></span>

-   <span data-ttu-id="0e965-134">Fare clic con il pulsante destro del mouse sull'area di progettazione e scegliere **browser modello**.</span><span class="sxs-lookup"><span data-stu-id="0e965-134">Right-click the design surface and select **Model Browser**.</span></span>
-   <span data-ttu-id="0e965-135">In **browser modello**selezionare **importazioni di funzioni**, quindi fare doppio clic sulla funzione **GetStudentGrades** .</span><span class="sxs-lookup"><span data-stu-id="0e965-135">In **Model Browser**, select **Function Imports**, and then double-click the **GetStudentGrades** function.</span></span>
-   <span data-ttu-id="0e965-136">Nella finestra di dialogo modifica importazione funzione selezionare **entità**  E scegliere **StudentGrade**.</span><span class="sxs-lookup"><span data-stu-id="0e965-136">In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**.</span></span>  
    <span data-ttu-id="0e965-137">l'importazione di funzioni *The **è componibile** nella parte superiore della finestra di dialogo **importazioni** di funzioni che consente di eseguire il mapping a funzioni componibili. Se si seleziona questa casella, solo le funzioni componibili (funzioni con valori di tabella) verranno visualizzate nell'elenco a discesa **stored procedure/nome funzione** . Se non si seleziona questa casella, nell'elenco verranno visualizzate solo le funzioni non componibili.*</span><span class="sxs-lookup"><span data-stu-id="0e965-137">*The **Function Import is composable** checkbox at the top of the **Function Imports** dialog will let you map to composable functions. If you do check this box, only composable functions (Table-valued Functions) will appear in the **Stored Procedure / Function Name** drop-down list. If you do not check this box, only non-composable functions will be shown in the list.*</span></span>

## <a name="use-the-model"></a><span data-ttu-id="0e965-138">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="0e965-138">Use the Model</span></span>

<span data-ttu-id="0e965-139">Aprire il file **Program.cs** in cui è definito il metodo **Main** .</span><span class="sxs-lookup"><span data-stu-id="0e965-139">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="0e965-140">Aggiungere il codice seguente nella funzione Main.</span><span class="sxs-lookup"><span data-stu-id="0e965-140">Add the following code into the Main function.</span></span>

<span data-ttu-id="0e965-141">Il codice chiama due stored procedure: **GetStudentGrades** (restituisce **StudentGrades** per il *StudentID*specificato) e **GetDepartmentName** (restituisce il nome del reparto nel parametro di output).</span><span class="sxs-lookup"><span data-stu-id="0e965-141">The code calls two stored procedures: **GetStudentGrades** (returns **StudentGrades** for the specified *StudentId*) and **GetDepartmentName** (returns the name of the department in the output parameter).</span></span>  

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

<span data-ttu-id="0e965-142">Compilare l'applicazione ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="0e965-142">Compile and run the application.</span></span> <span data-ttu-id="0e965-143">Il programma produce l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="0e965-143">The program produces the following output:</span></span>

```console
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a><span data-ttu-id="0e965-144">Parametri di output</span><span class="sxs-lookup"><span data-stu-id="0e965-144">Output Parameters</span></span>
-----------------

<span data-ttu-id="0e965-145">Se vengono utilizzati parametri di output, i relativi valori non saranno disponibili fino a quando i risultati non sono stati letti completamente.</span><span class="sxs-lookup"><span data-stu-id="0e965-145">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="0e965-146">A causa del comportamento sottostante di DbDataReader, vedere Recupero di [dati tramite un DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="0e965-146">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>
