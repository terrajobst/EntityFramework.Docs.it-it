---
title: Finestra di progettazione TPT (ereditarietà)-Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 68980fa89446940b8b7f5f73c519d38e727a9039
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996348"
---
# <a name="designer-tpt-inheritance"></a><span data-ttu-id="f460c-102">Finestra di progettazione TPT (ereditarietà)</span><span class="sxs-lookup"><span data-stu-id="f460c-102">Designer TPT Inheritance</span></span>
<span data-ttu-id="f460c-103">Questa procedura dettagliata viene illustrato come implementare l'ereditarietà di (TPT) per ogni tipo di tabella nel modello usando Entity Framework Designer (Entity Framework Designer).</span><span class="sxs-lookup"><span data-stu-id="f460c-103">This step-by-step walkthrough shows how to implement table-per-type (TPT) inheritance in your model using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="f460c-104">L'ereditarietà tabella per tipo prevede l'utilizzo di una tabella separata nel database per la gestione dei dati relativi alle proprietà non ereditate e alle proprietà chiave di ogni tipo della gerarchia di ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="f460c-104">Table-per-type inheritance uses a separate table in the database to maintain data for non-inherited properties and key properties for each type in the inheritance hierarchy.</span></span>

<span data-ttu-id="f460c-105">In questa procedura dettagliata verrà eseguito il mapping di **Course** (tipo di base), **OnlineCourse** (deriva dal corso), e **OnsiteCourse** (deriva da **corso**) entità alle tabelle con gli stessi nomi.</span><span class="sxs-lookup"><span data-stu-id="f460c-105">In this walkthrough we will map the **Course** (base type), **OnlineCourse** (derives from Course), and **OnsiteCourse** (derives from **Course**) entities to tables with the same names.</span></span> <span data-ttu-id="f460c-106">Verrà creato un modello dal database e quindi modificare il modello per implementare l'ereditarietà tabella per tipo.</span><span class="sxs-lookup"><span data-stu-id="f460c-106">We'll create a model from the database and then alter the model to implement the TPT inheritance.</span></span>

<span data-ttu-id="f460c-107">È possibile inoltre iniziare con il primo modello e quindi generare il database dal modello.</span><span class="sxs-lookup"><span data-stu-id="f460c-107">You can also start with the Model First and then generate the database from the model.</span></span> <span data-ttu-id="f460c-108">Per impostazione predefinita, la finestra di progettazione di Entity Framework utilizza la strategia TPT e pertanto qualsiasi ereditarietà nel modello verranno mappati a tabelle distinte.</span><span class="sxs-lookup"><span data-stu-id="f460c-108">The EF Designer uses the TPT strategy by default and so any inheritance in the model will be mapped to separate tables.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="f460c-109">Altre opzioni di ereditarietà</span><span class="sxs-lookup"><span data-stu-id="f460c-109">Other Inheritance Options</span></span>

<span data-ttu-id="f460c-110">Tabella per gerarchia (TPH) è un altro tipo di ereditarietà nelle quali un database tabella viene utilizzata per gestire i dati per tutti i tipi di entità in una gerarchia di ereditarietà.</span><span class="sxs-lookup"><span data-stu-id="f460c-110">Table-per-Hierarchy (TPH) is another type of inheritance in which one database table is used to maintain data for all of the entity types in an inheritance hierarchy.</span></span>  <span data-ttu-id="f460c-111">Per informazioni su come eseguire il mapping di ereditarietà tabella per gerarchia con Entity Designer, vedere [Entity Framework Designer l'ereditarietà](~/ef6/modeling/designer/inheritance/tph.md).</span><span class="sxs-lookup"><span data-stu-id="f460c-111">For information about how to map Table-per-Hierarchy inheritance with the Entity Designer, see [EF Designer TPH Inheritance](~/ef6/modeling/designer/inheritance/tph.md).</span></span> 

<span data-ttu-id="f460c-112">Si noti che, la tabella per tipo concreto tipo (TP) ed misto ereditarietà modelli sono supportati dal runtime di Entity Framework, ma non sono supportati dalla finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f460c-112">Note that, the Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="f460c-113">Se si desidera utilizzare TPC o misto eredità, sono disponibili due opzioni: utilizzare Code First, oppure modificare manualmente i file con estensione EDMX.</span><span class="sxs-lookup"><span data-stu-id="f460c-113">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="f460c-114">Se si sceglie di usare il file EDMX, nella finestra Dettagli Mapping verrà inserita nella "modalità provvisoria" e non sarà in grado di utilizzare la finestra di progettazione per modificare i mapping.</span><span class="sxs-lookup"><span data-stu-id="f460c-114">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f460c-115">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="f460c-115">Prerequisites</span></span>

<span data-ttu-id="f460c-116">Per completare questa procedura dettagliata, è necessario disporre di:</span><span class="sxs-lookup"><span data-stu-id="f460c-116">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="f460c-117">Una versione recente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f460c-117">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="f460c-118">Il [database di esempio School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="f460c-118">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="f460c-119">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="f460c-119">Set up the Project</span></span>

-   <span data-ttu-id="f460c-120">Aprire Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="f460c-120">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="f460c-121">Selezionare **File -&gt; Novità -&gt; progetto**</span><span class="sxs-lookup"><span data-stu-id="f460c-121">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="f460c-122">Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **Console** modello.</span><span class="sxs-lookup"><span data-stu-id="f460c-122">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="f460c-123">Immettere **TPTDBFirstSample** come nome.</span><span class="sxs-lookup"><span data-stu-id="f460c-123">Enter **TPTDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="f460c-124">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="f460c-124">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="f460c-125">Creare un modello</span><span class="sxs-lookup"><span data-stu-id="f460c-125">Create a Model</span></span>

-   <span data-ttu-id="f460c-126">Il progetto in Esplora soluzioni e scegliere **Add -&gt; nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="f460c-126">Right-click the project in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="f460c-127">Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro dei modelli.</span><span class="sxs-lookup"><span data-stu-id="f460c-127">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="f460c-128">Immettere **TPTModel.edmx** per il nome del file e quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="f460c-128">Enter **TPTModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="f460c-129">Nella finestra di dialogo Scegli contenuto Model casella, selezionare * * Genera da database * * e quindi fare clic su **successivo**.</span><span class="sxs-lookup"><span data-stu-id="f460c-129">In the Choose Model Contents dialog box, select** Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="f460c-130">Fare clic su **nuova connessione**.</span><span class="sxs-lookup"><span data-stu-id="f460c-130">Click **New Connection**.</span></span>
    <span data-ttu-id="f460c-131">Nella finestra di dialogo proprietà di connessione, immettere il nome del server (ad esempio, **(localdb)\\mssqllocaldb**), selezionare il metodo di autenticazione, il tipo **School** per il nome del database e quindi Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="f460c-131">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="f460c-132">La finestra di dialogo Seleziona connessione dati viene aggiornata con l'impostazione di connessione di database.</span><span class="sxs-lookup"><span data-stu-id="f460c-132">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="f460c-133">Nella finestra di dialogo Scegli oggetti di Database, sotto il nodo tabelle, selezionare la **reparto**, **corso OnlineCourse e OnsiteCourse** tabelle.</span><span class="sxs-lookup"><span data-stu-id="f460c-133">In the Choose Your Database Objects dialog box, under the Tables node, select the **Department**, **Course, OnlineCourse, and OnsiteCourse** tables.</span></span>
-   <span data-ttu-id="f460c-134">Scegliere **Fine**.</span><span class="sxs-lookup"><span data-stu-id="f460c-134">Click **Finish**.</span></span>

<span data-ttu-id="f460c-135">Entity Designer, che fornisce un'area di progettazione per la modifica del modello, viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="f460c-135">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="f460c-136">Tutti gli oggetti selezionati nella finestra di dialogo Scegli oggetti di Database vengono aggiunti al modello.</span><span class="sxs-lookup"><span data-stu-id="f460c-136">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

## <a name="implement-table-per-type-inheritance"></a><span data-ttu-id="f460c-137">Implementare l'ereditarietà tabella per tipo</span><span class="sxs-lookup"><span data-stu-id="f460c-137">Implement Table-per-Type Inheritance</span></span>

-   <span data-ttu-id="f460c-138">Nell'area di progettazione, fare doppio clic il **OnlineCourse** tipo di entità e selezionare **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="f460c-138">On the design surface, right-click the **OnlineCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="f460c-139">Nel **delle proprietà** finestra, impostare la proprietà Type di Base **Course**.</span><span class="sxs-lookup"><span data-stu-id="f460c-139">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="f460c-140">Fare doppio clic il **OnsiteCourse** tipo di entità e selezionare **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="f460c-140">Right-click the **OnsiteCourse** entity type and select **Properties**.</span></span>
-   <span data-ttu-id="f460c-141">Nel **delle proprietà** finestra, impostare la proprietà Type di Base **Course**.</span><span class="sxs-lookup"><span data-stu-id="f460c-141">In the **Properties** window, set the Base Type property to **Course**.</span></span>
-   <span data-ttu-id="f460c-142">Fare doppio clic su (riga) dell'associazione tra il **OnlineCourse** e **corso** i tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="f460c-142">Right-click the association (the line) between the **OnlineCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="f460c-143">Selezionare **Elimina dal modello**.</span><span class="sxs-lookup"><span data-stu-id="f460c-143">Select **Delete from Model**.</span></span>
-   <span data-ttu-id="f460c-144">Fare doppio clic sull'associazione tra il **OnsiteCourse** e **corso** i tipi di entità.</span><span class="sxs-lookup"><span data-stu-id="f460c-144">Right-click the association between the **OnsiteCourse** and **Course** entity types.</span></span>
    <span data-ttu-id="f460c-145">Selezionare **Elimina dal modello**.</span><span class="sxs-lookup"><span data-stu-id="f460c-145">Select **Delete from Model**.</span></span>

<span data-ttu-id="f460c-146">Ora si eliminerà la **CourseID** proprietà dal **OnlineCourse** e **OnsiteCourse** perché queste classi ereditano **CourseID** da il **corso** tipo di base.</span><span class="sxs-lookup"><span data-stu-id="f460c-146">We will now delete the **CourseID** property from **OnlineCourse** and **OnsiteCourse** because these classes inherit **CourseID** from the **Course** base type.</span></span>

-   <span data-ttu-id="f460c-147">Fare doppio clic sui **CourseID** proprietà delle **OnlineCourse** tipo di entità e quindi selezionare **Elimina da modello**.</span><span class="sxs-lookup"><span data-stu-id="f460c-147">Right-click the **CourseID** property of the **OnlineCourse** entity type, and then select **Delete from Model**.</span></span>
-   <span data-ttu-id="f460c-148">Fare doppio clic sui **CourseID** proprietà delle **OnsiteCourse** tipo di entità e quindi selezionare **Elimina da modello**</span><span class="sxs-lookup"><span data-stu-id="f460c-148">Right-click the **CourseID** property of the **OnsiteCourse** entity type, and then select **Delete from Model**</span></span>
-   <span data-ttu-id="f460c-149">A questo punto l'ereditarietà tabella per tipo risulta implementata.</span><span class="sxs-lookup"><span data-stu-id="f460c-149">Table-per-type inheritance is now implemented.</span></span>

![TABELLA PER TIPO](~/ef6/media/tpt.png)

## <a name="use-the-model"></a><span data-ttu-id="f460c-151">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="f460c-151">Use the Model</span></span>

<span data-ttu-id="f460c-152">Aprire il **Program.cs** file dove il **Main** metodo è definito.</span><span class="sxs-lookup"><span data-stu-id="f460c-152">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="f460c-153">Incollare il codice seguente nel **Main** (funzione).</span><span class="sxs-lookup"><span data-stu-id="f460c-153">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="f460c-154">Il codice esegue tre query.</span><span class="sxs-lookup"><span data-stu-id="f460c-154">The code executes three queries.</span></span> <span data-ttu-id="f460c-155">La prima query restituisce tutti i **corsi** correlate al reparto specificato.</span><span class="sxs-lookup"><span data-stu-id="f460c-155">The first query brings back all **Courses** related to the specified department.</span></span> <span data-ttu-id="f460c-156">La seconda query Usa la **OfType** metodo restituisca **OnlineCourses** correlate al reparto specificato.</span><span class="sxs-lookup"><span data-stu-id="f460c-156">The second query uses the **OfType** method to return **OnlineCourses** related to the specified department.</span></span> <span data-ttu-id="f460c-157">Restituisce la terza query **OnsiteCourses**.</span><span class="sxs-lookup"><span data-stu-id="f460c-157">The third query returns **OnsiteCourses**.</span></span>

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
