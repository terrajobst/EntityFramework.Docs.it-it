---
title: Finestra di progettazione TPT (ereditarietà)-Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489453"
---
# <a name="designer-tpt-inheritance"></a><span data-ttu-id="e2bd4-102">Finestra di progettazione TPT (ereditarietà)</span><span class="sxs-lookup"><span data-stu-id="e2bd4-102">Designer TPT Inheritance</span></span>
<span data-ttu-id="e2bd4-103">Questa procedura dettagliata viene illustrato come implementare l'ereditarietà di (TPT) per ogni tipo di tabella nel modello usando Entity Framework Designer (Entity Framework Designer).</span><span class="sxs-lookup"><span data-stu-id="e2bd4-103">This step-by-step walkthrough shows how to implement table-per-type (TPT) inheritance in your model using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="e2bd4-104">L'ereditarietà tabella per tipo prevede l'utilizzo di una tabella separata nel database per la gestione dei dati relativi alle proprietà non ereditate e alle proprietà chiave di ogni tipo della gerarchia di ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-104">Table-per-type inheritance uses a separate table in the database to maintain data for non-inherited properties and key properties for each type in the inheritance hierarchy.</span></span>

<span data-ttu-id="e2bd4-105">In questa procedura dettagliata verrà eseguito il mapping di **Course** (tipo di base), **OnlineCourse** (deriva dal corso), e **OnsiteCourse** (deriva da **corso**) entità alle tabelle con gli stessi nomi.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-105">In this walkthrough we will map the **Course** (base type), **OnlineCourse** (derives from Course), and **OnsiteCourse** (derives from **Course**) entities to tables with the same names.</span></span> <span data-ttu-id="e2bd4-106">Verrà creato un modello dal database e quindi modificare il modello per implementare l'ereditarietà tabella per tipo.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-106">We'll create a model from the database and then alter the model to implement the TPT inheritance.</span></span>

<span data-ttu-id="e2bd4-107">È possibile inoltre iniziare con il primo modello e quindi generare il database dal modello.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-107">You can also start with the Model First and then generate the database from the model.</span></span> <span data-ttu-id="e2bd4-108">Per impostazione predefinita, la finestra di progettazione di Entity Framework utilizza la strategia TPT e pertanto qualsiasi ereditarietà nel modello verranno mappati a tabelle distinte.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-108">The EF Designer uses the TPT strategy by default and so any inheritance in the model will be mapped to separate tables.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="e2bd4-109">Altre opzioni di ereditarietà</span><span class="sxs-lookup"><span data-stu-id="e2bd4-109">Other Inheritance Options</span></span>

<span data-ttu-id="e2bd4-110">Tabella per gerarchia (TPH) è un altro tipo di ereditarietà nelle quali un database tabella viene utilizzata per gestire i dati per tutti i tipi di entità in una gerarchia di ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-110">Table-per-Hierarchy (TPH) is another type of inheritance in which one database table is used to maintain data for all of the entity types in an inheritance hierarchy.</span></span>  <span data-ttu-id="e2bd4-111">Per informazioni su come eseguire il mapping di ereditarietà tabella per gerarchia con Entity Designer, vedere [Entity Framework Designer l'ereditarietà](~/ef6/modeling/designer/inheritance/tph.md).</span><span class="sxs-lookup"><span data-stu-id="e2bd4-111">For information about how to map Table-per-Hierarchy inheritance with the Entity Designer, see [EF Designer TPH Inheritance](~/ef6/modeling/designer/inheritance/tph.md).</span></span> 

<span data-ttu-id="e2bd4-112">Si noti che, la tabella per tipo concreto tipo (TP) ed misto ereditarietà modelli sono supportati dal runtime di Entity Framework, ma non sono supportati dalla finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-112">Note that, the Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="e2bd4-113">Se si desidera utilizzare TPC o misto eredità, sono disponibili due opzioni: utilizzare Code First, oppure modificare manualmente i file con estensione EDMX.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-113">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="e2bd4-114">Se si sceglie di usare il file EDMX, nella finestra Dettagli Mapping verrà inserita nella "modalità provvisoria" e non sarà in grado di utilizzare la finestra di progettazione per modificare i mapping.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-114">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e2bd4-115">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e2bd4-115">Prerequisites</span></span>

<span data-ttu-id="e2bd4-116">Per completare questa procedura dettagliata, è necessario disporre di:</span><span class="sxs-lookup"><span data-stu-id="e2bd4-116">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="e2bd4-117">Una versione recente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-117">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="e2bd4-118">Il [database di esempio School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="e2bd4-118">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="e2bd4-119">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="e2bd4-119">Set up the Project</span></span>

-   <span data-ttu-id="e2bd4-120">Aprire Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-120">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="e2bd4-121">Selezionare **File -&gt; Novità -&gt; progetto**</span><span class="sxs-lookup"><span data-stu-id="e2bd4-121">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="e2bd4-122">Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **Console** modello.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-122">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="e2bd4-123">Immettere **TPTDBFirstSample** come nome.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-123">Enter **TPTDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="e2bd4-124">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-124">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="e2bd4-125">Creare un modello</span><span class="sxs-lookup"><span data-stu-id="e2bd4-125">Create a Model</span></span>

-   <span data-ttu-id="e2bd4-126">Il progetto in Esplora soluzioni e scegliere **Add -&gt; nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-126">Right-click the project in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="e2bd4-127">Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro dei modelli.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-127">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="e2bd4-128">Immettere **TPTModel.edmx** per il nome del file e quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-128">Enter **TPTModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="e2bd4-129">Nella finestra di dialogo Scegli contenuto Model casella, selezionare \* \* Genera da database \* \* e quindi fare clic su **successivo**.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-129">In the Choose Model Contents dialog box, select\*\* Generate from database\*\*, and then click **Next**.</span></span>
-   <span data-ttu-id="e2bd4-130">Fare clic su **nuova connessione**.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-130">Click **New Connection**.</span></span>
    <span data-ttu-id="e2bd4-131">Nella finestra di dialogo proprietà di connessione, immettere il nome del server (ad esempio, **(localdb)\\mssqllocaldb**), selezionare il metodo di autenticazione, il tipo **School** per il nome del database e quindi Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-131">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="e2bd4-132">La finestra di dialogo Seleziona connessione dati viene aggiornata con l'impostazione di connessione di database.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-132">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="e2bd4-133">Nella finestra di dialogo Scegli oggetti di Database, sotto il nodo tabelle, selezionare la **reparto**, **corso OnlineCourse e OnsiteCourse** tabelle.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-133">In the Choose Your Database Objects dialog box, under the Tables node, select the **Department**, **Course, OnlineCourse, and OnsiteCourse** tables.</span></span>
-   <span data-ttu-id="e2bd4-134">Scegliere **Fine**.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-134">Click **Finish**.</span></span>

<span data-ttu-id="e2bd4-135">Entity Designer, che fornisce un'area di progettazione per la modifica del modello, viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-135">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="e2bd4-136">Tutti gli oggetti selezionati nella finestra di dialogo Scegli oggetti di Database vengono aggiunti al modello.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-136">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

## <a name="implement-table-per-type-inheritance"></a><span data-ttu-id="e2bd4-137">Implementare l'ereditarietà tabella per tipo</span><span class="sxs-lookup"><span data-stu-id="e2bd4-137">Implement Table-per-Type Inheritance</span></span>

-   <span data-ttu-id="e2bd4-138">Nell'area di progettazione, fare doppio clic il **OnlineCourse** tipo di entità e selezionare **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-138">On the design surface, right-click the **OnlineCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="e2bd4-139">Nel **delle proprietà** finestra, impostare la proprietà Type di Base **Course**.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-139">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="e2bd4-140">Fare doppio clic il **OnsiteCourse** tipo di entità e selezionare **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-140">Right-click the **OnsiteCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="e2bd4-141">Nel **delle proprietà** finestra, impostare la proprietà Type di Base **Course**.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-141">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="e2bd4-142">Fare doppio clic su (riga) dell'associazione tra il **OnlineCourse** e **corso** i tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-142">Right-click the association (the line) between the **OnlineCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="e2bd4-143">Selezionare **Elimina dal modello**.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-143">Select **Delete from Model**.</span></span>
-   <span data-ttu-id="e2bd4-144">Fare doppio clic sull'associazione tra il **OnsiteCourse** e **corso** i tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-144">Right-click the association between the **OnsiteCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="e2bd4-145">Selezionare **Elimina dal modello**.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-145">Select **Delete from Model**.</span></span>

<span data-ttu-id="e2bd4-146">Ora si eliminerà la **CourseID** proprietà dal **OnlineCourse** e **OnsiteCourse** perché queste classi ereditano **CourseID** da il **corso** tipo di base.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-146">We will now delete the **CourseID** property from **OnlineCourse** and **OnsiteCourse** because these classes inherit **CourseID** from the **Course** base type.</span></span>

-   <span data-ttu-id="e2bd4-147">Fare doppio clic sui **CourseID** proprietà delle **OnlineCourse** tipo di entità e quindi selezionare **Elimina da modello**.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-147">Right-click the **CourseID** property of the **OnlineCourse** entity type, and then select **Delete from Model**.</span></span>
-   <span data-ttu-id="e2bd4-148">Fare doppio clic sui **CourseID** proprietà delle **OnsiteCourse** tipo di entità e quindi selezionare **Elimina da modello**</span><span class="sxs-lookup"><span data-stu-id="e2bd4-148">Right-click the **CourseID** property of the **OnsiteCourse** entity type, and then select **Delete from Model**</span></span>
-   <span data-ttu-id="e2bd4-149">A questo punto l'ereditarietà tabella per tipo risulta implementata.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-149">Table-per-type inheritance is now implemented.</span></span>

![TABELLA PER TIPO](~/ef6/media/tpt.png)

## <a name="use-the-model"></a><span data-ttu-id="e2bd4-151">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="e2bd4-151">Use the Model</span></span>

<span data-ttu-id="e2bd4-152">Aprire il **Program.cs** file dove il **Main** metodo è definito.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-152">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="e2bd4-153">Incollare il codice seguente nel **Main** (funzione).</span><span class="sxs-lookup"><span data-stu-id="e2bd4-153">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="e2bd4-154">Il codice esegue tre query.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-154">The code executes three queries.</span></span> <span data-ttu-id="e2bd4-155">La prima query restituisce tutti i **corsi** correlate al reparto specificato.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-155">The first query brings back all **Courses** related to the specified department.</span></span> <span data-ttu-id="e2bd4-156">La seconda query Usa la **OfType** metodo restituisca **OnlineCourses** correlate al reparto specificato.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-156">The second query uses the **OfType** method to return **OnlineCourses** related to the specified department.</span></span> <span data-ttu-id="e2bd4-157">Restituisce la terza query **OnsiteCourses**.</span><span class="sxs-lookup"><span data-stu-id="e2bd4-157">The third query returns **OnsiteCourses**.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
