---
title: Tabella di funzioni con valori () - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: 6130e6bd550497509f3dcc0242077046d12c601a
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489323"
---
# <a name="table-valued-functions-tvfs"></a><span data-ttu-id="1d3e6-102">Funzioni con valori di tabella (TVF)</span><span class="sxs-lookup"><span data-stu-id="1d3e6-102">Table-Valued Functions (TVFs)</span></span>
> [!NOTE]
> <span data-ttu-id="1d3e6-103">**EF5 e versioni successive solo** -le funzionalità, le API, e così via illustrati in questa pagina sono stati introdotti in Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="1d3e6-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="1d3e6-105">La procedura dettagliata video e procedura dettagliata illustra come eseguire il mapping di funzioni con valori di tabella (TVF) usando la finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-105">The video and step-by-step walkthrough shows how to map table-valued functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="1d3e6-106">Viene inoltre illustrato come chiamare una funzione TVF da una query LINQ.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-106">It also demonstrates how to call a TVF from a LINQ query.</span></span>

<span data-ttu-id="1d3e6-107">Le TVF sono attualmente supportati solo nel flusso di lavoro Database First.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-107">TVFs are currently only supported in the Database First workflow.</span></span>

<span data-ttu-id="1d3e6-108">Supporto TVF è stato introdotto in Entity Framework versione 5.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-108">TVF support was introduced in Entity Framework version 5.</span></span> <span data-ttu-id="1d3e6-109">Si noti che per utilizzare le nuove funzionalità, ad esempio funzioni con valori di tabella, enumerazioni e tipi spaziali, deve avere come destinazione .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-109">Note that to use the new features like table-valued functions, enums, and spatial types you must target .NET Framework 4.5.</span></span> <span data-ttu-id="1d3e6-110">Visual Studio 2012 è destinato a .NET 4.5 per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="1d3e6-111">Le TVF sono molto simili alle stored procedure con una differenza fondamentale: il risultato di una funzione TVF è componibile.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-111">TVFs are very similar to stored procedures with one key difference: the result of a TVF is composable.</span></span> <span data-ttu-id="1d3e6-112">Ciò significa che i risultati di una funzione TVF possono essere usati in una query LINQ mentre i risultati di una stored procedure non è possibile.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-112">That means the results from a TVF can be used in a LINQ query while the results of a stored procedure cannot.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="1d3e6-113">Guarda il video</span><span class="sxs-lookup"><span data-stu-id="1d3e6-113">Watch the video</span></span>

<span data-ttu-id="1d3e6-114">**Presentato da**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="1d3e6-114">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="1d3e6-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span><span class="sxs-lookup"><span data-stu-id="1d3e6-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="1d3e6-116">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="1d3e6-116">Pre-Requisites</span></span>

<span data-ttu-id="1d3e6-117">Per completare questa procedura dettagliata, è necessario:</span><span class="sxs-lookup"><span data-stu-id="1d3e6-117">To complete this walkthrough, you need to:</span></span>

- <span data-ttu-id="1d3e6-118">Installare il [database School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="1d3e6-118">Install the [School database](~/ef6/resources/school-database.md).</span></span>

- <span data-ttu-id="1d3e6-119">Dispone di una versione recente di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1d3e6-119">Have a recent version of Visual Studio</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="1d3e6-120">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="1d3e6-120">Set up the Project</span></span>

1.  <span data-ttu-id="1d3e6-121">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1d3e6-121">Open Visual Studio</span></span>
2.  <span data-ttu-id="1d3e6-122">Nel **File** dal menu **New**, quindi fare clic su **progetto**</span><span class="sxs-lookup"><span data-stu-id="1d3e6-122">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="1d3e6-123">Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **Console** modello</span><span class="sxs-lookup"><span data-stu-id="1d3e6-123">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="1d3e6-124">Immettere **TVF** come nome del progetto e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="1d3e6-124">Enter **TVF** as the name of the project and click **OK**</span></span>

## <a name="add-a-tvf-to-the-database"></a><span data-ttu-id="1d3e6-125">Aggiungere una funzione TVF al Database</span><span class="sxs-lookup"><span data-stu-id="1d3e6-125">Add a TVF to the Database</span></span>

-   <span data-ttu-id="1d3e6-126">Selezionare **vista -&gt; Esplora oggetti di SQL Server**</span><span class="sxs-lookup"><span data-stu-id="1d3e6-126">Select **View -&gt; SQL Server Object Explorer**</span></span>
-   <span data-ttu-id="1d3e6-127">Se Local DB non è presente nell'elenco dei server: fare clic su **SQL Server** e selezionare **Aggiungi SQL Server** usare il valore predefinito **l'autenticazione di Windows** per connettersi al server di database locale</span><span class="sxs-lookup"><span data-stu-id="1d3e6-127">If LocalDB is not in the list of servers: Right-click on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB server</span></span>
-   <span data-ttu-id="1d3e6-128">Espandere il nodo del database locale</span><span class="sxs-lookup"><span data-stu-id="1d3e6-128">Expand the LocalDB node</span></span>
-   <span data-ttu-id="1d3e6-129">Sotto il nodo database, fare clic sul nodo del database School e selezionare **nuova Query...**</span><span class="sxs-lookup"><span data-stu-id="1d3e6-129">Under the Databases node, right-click the School database node and select **New Query…**</span></span>
-   <span data-ttu-id="1d3e6-130">Nell'Editor T-SQL, incollare la seguente definizione di funzione TVF</span><span class="sxs-lookup"><span data-stu-id="1d3e6-130">In T-SQL Editor, paste the following TVF definition</span></span>

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   <span data-ttu-id="1d3e6-131">Scegliere il pulsante destro del mouse sull'editor T-SQL e selezionare **Execute**</span><span class="sxs-lookup"><span data-stu-id="1d3e6-131">Click the right mouse button on the T-SQL editor and select **Execute**</span></span>
-   <span data-ttu-id="1d3e6-132">La funzione GetStudentGradesForCourse viene aggiunto al database School</span><span class="sxs-lookup"><span data-stu-id="1d3e6-132">The GetStudentGradesForCourse function is added to the School database</span></span>

 

## <a name="create-a-model"></a><span data-ttu-id="1d3e6-133">Creare un modello</span><span class="sxs-lookup"><span data-stu-id="1d3e6-133">Create a Model</span></span>

1.  <span data-ttu-id="1d3e6-134">Fare clic sul nome del progetto in Esplora soluzioni, scegliere **Add**, quindi fare clic su **nuovo elemento**</span><span class="sxs-lookup"><span data-stu-id="1d3e6-134">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="1d3e6-135">Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel **modelli** riquadro</span><span class="sxs-lookup"><span data-stu-id="1d3e6-135">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the **Templates** pane</span></span>
3.  <span data-ttu-id="1d3e6-136">Immettere **TVFModel.edmx** per il nome del file e quindi fare clic su **Add**</span><span class="sxs-lookup"><span data-stu-id="1d3e6-136">Enter **TVFModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="1d3e6-137">Nella finestra di dialogo Scegli contenuto Model, selezionare **genera da database**, quindi fare clic su **successivo**</span><span class="sxs-lookup"><span data-stu-id="1d3e6-137">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**</span></span>
5.  <span data-ttu-id="1d3e6-138">Fare clic su **nuova connessione** invio **(localdb)\\mssqllocaldb** nel testo del nome Server casella immettere **School** per il database fare clic su nome **OK**</span><span class="sxs-lookup"><span data-stu-id="1d3e6-138">Click **New Connection** Enter **(localdb)\\mssqllocaldb** in the Server name text box Enter **School** for the database name Click **OK**</span></span>
6.  <span data-ttu-id="1d3e6-139">In Scegli di oggetti di Database nella finestra di dialogo di **tabelle** nodo, seleziona la **persona**, **StudentGrade**, e **corso** tabelle</span><span class="sxs-lookup"><span data-stu-id="1d3e6-139">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person**, **StudentGrade**, and **Course** tables</span></span>
7.  <span data-ttu-id="1d3e6-140">Selezionare il **GetStudentGradesForCourse** funzione che si trova sotto il **Stored procedure e funzioni** nodo nota, che inizia con Visual Studio 2012, Entity Designer consente di importazione di batch la Stored procedure e funzioni</span><span class="sxs-lookup"><span data-stu-id="1d3e6-140">Select the **GetStudentGradesForCourse** function located under the **Stored Procedures and Functions** node Note, that starting with Visual Studio 2012, the Entity Designer allows you to batch import your Stored Procedures and Functions</span></span>
8.  <span data-ttu-id="1d3e6-141">Fare clic su **fine**</span><span class="sxs-lookup"><span data-stu-id="1d3e6-141">Click **Finish**</span></span>
9.  <span data-ttu-id="1d3e6-142">Entity Designer, che fornisce un'area di progettazione per la modifica del modello, viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-142">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="1d3e6-143">Tutti gli oggetti selezionati nella **Scegli oggetti di Database** nella finestra di dialogo vengono aggiunti al modello.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-143">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>
10. <span data-ttu-id="1d3e6-144">Per impostazione predefinita, la forma dei risultati di ogni importate stored procedure o funzione diventerà automaticamente un nuovo tipo complesso nel modello di entità.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-144">By default, the result shape of each imported stored procedure or function will automatically become a new complex type in your entity model.</span></span> <span data-ttu-id="1d3e6-145">Ma per mappare i risultati della funzione GetStudentGradesForCourse all'entità StudentGrade: fare doppio clic su area di progettazione e seleziona **Visualizzatore modelli** nella finestra Browser modello selezionare **importazioni di funzioni**e quindi fare doppio clic il **GetStudentGradesForCourse** funzione In the modifica importazione di funzioni finestra di dialogo **entità** e scegliere **StudentGrade**</span><span class="sxs-lookup"><span data-stu-id="1d3e6-145">But we want to map the results of the GetStudentGradesForCourse function to the StudentGrade entity: Right-click the design surface and select **Model Browser** In Model Browser, select **Function Imports**, and then double-click the **GetStudentGradesForCourse** function In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="1d3e6-146">Salvare in modo permanente e recuperare i dati</span><span class="sxs-lookup"><span data-stu-id="1d3e6-146">Persist and Retrieve Data</span></span>

<span data-ttu-id="1d3e6-147">Aprire il file in cui è definito il metodo Main.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-147">Open the file where the Main method is defined.</span></span> <span data-ttu-id="1d3e6-148">Aggiungere il codice seguente nella funzione Main.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-148">Add the following code into the Main function.</span></span>

<span data-ttu-id="1d3e6-149">Il codice seguente viene illustrato come compilare una query che utilizza una funzione con valori di tabella.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-149">The following code demonstrates how to build a query that uses a Table-valued Function.</span></span> <span data-ttu-id="1d3e6-150">La query proietta i risultati in un tipo anonimo che contiene il titolo del corso correlato e correlate agli studenti con un livello maggiore o uguale a 3.5.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-150">The query projects the results into an anonymous type that contains the related Course title and related students with a grade greater or equal to 3.5.</span></span>

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

<span data-ttu-id="1d3e6-151">Compilare l'applicazione ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-151">Compile and run the application.</span></span> <span data-ttu-id="1d3e6-152">Il programma produce l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="1d3e6-152">The program produces the following output:</span></span>

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a><span data-ttu-id="1d3e6-153">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="1d3e6-153">Summary</span></span>

<span data-ttu-id="1d3e6-154">In questa procedura dettagliata è stato descritto come eseguire il mapping di funzioni con valori di tabella (TVF) usando la finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-154">In this walkthrough we looked at how to map Table-valued Functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="1d3e6-155">È stato anche illustrato come chiamare una funzione TVF da una query LINQ.</span><span class="sxs-lookup"><span data-stu-id="1d3e6-155">It also demonstrated how to call a TVF from a LINQ query.</span></span>
