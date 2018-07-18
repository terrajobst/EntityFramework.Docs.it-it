---
title: Finestra di progettazione CUD Stored procedure - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
caps.latest.revision: 3
ms.openlocfilehash: 6b6a1f843142713153fa86309ef55f9d6e804766
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120855"
---
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="d1498-102">Finestra di progettazione CUD Stored procedure</span><span class="sxs-lookup"><span data-stu-id="d1498-102">Designer CUD Stored Procedures</span></span>
<span data-ttu-id="d1498-103">Questa procedura dettagliata mostra come eseguire il mapping di creazione\\inserire, aggiornare ed eliminare le operazioni (CUD) di un tipo di entità alle stored procedure mediante Entity Framework Designer (Entity Framework Designer).</span><span class="sxs-lookup"><span data-stu-id="d1498-103">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span>  <span data-ttu-id="d1498-104">Per impostazione predefinita, Entity Framework genera automaticamente le istruzioni SQL per le operazioni CUD, ma è anche possibile mappare le stored procedure a queste operazioni.</span><span class="sxs-lookup"><span data-stu-id="d1498-104">By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="d1498-105">Si noti che Code First non supporta il mapping a stored procedure o funzioni.</span><span class="sxs-lookup"><span data-stu-id="d1498-105">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="d1498-106">Tuttavia, è possibile chiamare stored procedure o funzioni tramite il metodo System.Data.Entity.DbSet.SqlQuery.</span><span class="sxs-lookup"><span data-stu-id="d1498-106">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="d1498-107">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d1498-107">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="d1498-108">Considerazioni quando le operazioni CUD per il Mapping di Stored procedure</span><span class="sxs-lookup"><span data-stu-id="d1498-108">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="d1498-109">Quando le operazioni CUD il mapping a stored procedure, si applicano le considerazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="d1498-109">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span> 

-   <span data-ttu-id="d1498-110">Se si esegue una delle operazioni CUD il mapping a una stored procedure, eseguire il mapping di tutti gli elementi.</span><span class="sxs-lookup"><span data-stu-id="d1498-110">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="d1498-111">Se non si esegue il mapping tutte e tre, le operazioni non mappate avranno esito negativo se eseguita e un' **UpdateException** verrà generata.</span><span class="sxs-lookup"><span data-stu-id="d1498-111">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
-   <span data-ttu-id="d1498-112">È necessario eseguire il mapping di ogni parametro della stored procedure alle proprietà dell'entità.</span><span class="sxs-lookup"><span data-stu-id="d1498-112">You must map every parameter of the stored procedure to entity properties.</span></span>
-   <span data-ttu-id="d1498-113">Se il server genera il valore di chiave primaria per la riga inserita, è necessario eseguire il mapping di questo valore alla proprietà chiave dell'entità.</span><span class="sxs-lookup"><span data-stu-id="d1498-113">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="d1498-114">Nell'esempio seguente, il **InsertPerson** stored procedure restituisce la chiave primaria appena creata come parte del set di risultati della stored procedure.</span><span class="sxs-lookup"><span data-stu-id="d1498-114">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="d1498-115">La chiave primaria è mappata alla chiave di entità (**PersonID**) tramite il **&lt;Aggiungi Result binding&gt;** funzionalità della finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d1498-115">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
-   <span data-ttu-id="d1498-116">Le chiamate a stored procedure sono mappate 1:1 con le entità nel modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="d1498-116">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="d1498-117">Ad esempio, se si implementa una gerarchia di ereditarietà nel modello concettuale ed eseguire il mapping di CUD stored procedure per la **padre** (base) e il **figlio** entità (derivata), salvare il **Figlio** modifiche solo chiamerà il **figlio**della stored procedure, non attiva il **padre**della stored procedure chiama.</span><span class="sxs-lookup"><span data-stu-id="d1498-117">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d1498-118">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="d1498-118">Prerequisites</span></span>

<span data-ttu-id="d1498-119">Per completare questa procedura dettagliata, è necessario disporre di:</span><span class="sxs-lookup"><span data-stu-id="d1498-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="d1498-120">Una versione recente di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d1498-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="d1498-121">Il [database di esempio School](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="d1498-121">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="d1498-122">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="d1498-122">Set up the Project</span></span>

-   <span data-ttu-id="d1498-123">Aprire Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="d1498-123">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="d1498-124">Selezionare **File -&gt; Novità -&gt; progetto**</span><span class="sxs-lookup"><span data-stu-id="d1498-124">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="d1498-125">Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **Console** modello.</span><span class="sxs-lookup"><span data-stu-id="d1498-125">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="d1498-126">Immettere **CUDSProcsSample** come nome.</span><span class="sxs-lookup"><span data-stu-id="d1498-126">Enter **CUDSProcsSample** as the name.</span></span>
-   <span data-ttu-id="d1498-127">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="d1498-127">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="d1498-128">Creare un modello</span><span class="sxs-lookup"><span data-stu-id="d1498-128">Create a Model</span></span>

-   <span data-ttu-id="d1498-129">Il nome del progetto in Esplora soluzioni e scegliere **Add -&gt; nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="d1498-129">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="d1498-130">Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro dei modelli.</span><span class="sxs-lookup"><span data-stu-id="d1498-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="d1498-131">Immettere **CUDSProcs.edmx** per il nome del file e quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="d1498-131">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="d1498-132">Nella finestra di dialogo Scegli contenuto Model, selezionare **genera da database**, quindi fare clic su **successivo**.</span><span class="sxs-lookup"><span data-stu-id="d1498-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="d1498-133">Fare clic su **nuova connessione**.</span><span class="sxs-lookup"><span data-stu-id="d1498-133">Click **New Connection**.</span></span> <span data-ttu-id="d1498-134">Nella finestra di dialogo proprietà di connessione, immettere il nome del server (ad esempio, **(localdb)\\mssqllocaldb**), selezionare il metodo di autenticazione, il tipo **School** per il nome del database e quindi Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="d1498-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="d1498-135">La finestra di dialogo Seleziona connessione dati viene aggiornata con l'impostazione di connessione di database.</span><span class="sxs-lookup"><span data-stu-id="d1498-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="d1498-136">Nella pagina di scegliere di oggetti di Database nella finestra di dialogo il **tabelle** nodo, seleziona la **persona** tabella.</span><span class="sxs-lookup"><span data-stu-id="d1498-136">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
-   <span data-ttu-id="d1498-137">Selezionare anche le seguenti stored procedure con il **funzioni e Stored procedure** nodo: **DeletePerson**, **InsertPerson**, e **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="d1498-137">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span> 
-   <span data-ttu-id="d1498-138">Partire da Visual Studio 2012 nella finestra di progettazione di Entity Framework supporta l'importazione bulk di stored procedure.</span><span class="sxs-lookup"><span data-stu-id="d1498-138">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="d1498-139">Il **Importa selezionate stored procedure e funzioni nel modello di entità** è selezionata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="d1498-139">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="d1498-140">Poiché in questo esempio è stata stored procedure che inseriscono, aggiornano ed eliminano i tipi di entità, non si desidera importare tali e verrà deselezionare questa casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="d1498-140">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span> 

    ![ImportSProcs](~/ef6/media/importsprocs.jpg)

-   <span data-ttu-id="d1498-142">Scegliere **Fine**.</span><span class="sxs-lookup"><span data-stu-id="d1498-142">Click **Finish**.</span></span>
    <span data-ttu-id="d1498-143">La finestra di progettazione di Entity Framework, che fornisce un'area di progettazione per la modifica del modello, viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="d1498-143">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="d1498-144">Eseguire il mapping dell'entità Person alle Stored procedure</span><span class="sxs-lookup"><span data-stu-id="d1498-144">Map the Person Entity to Stored Procedures</span></span>

-   <span data-ttu-id="d1498-145">Fare doppio clic il **Person** tipo di entità e selezionare **Mapping Stored Procedure**.</span><span class="sxs-lookup"><span data-stu-id="d1498-145">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
-   <span data-ttu-id="d1498-146">Il mapping delle stored procedure vengono visualizzati nei **Dettagli Mapping** finestra.</span><span class="sxs-lookup"><span data-stu-id="d1498-146">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="d1498-147">Fare clic su  **&lt;seleziona Inserisci funzione&gt;**.</span><span class="sxs-lookup"><span data-stu-id="d1498-147">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="d1498-148">Questo campo diventa l'elenco a discesa delle stored procedure contenute nel modello di archiviazione che possono essere mappate ai tipi di entità del modello concettuale.</span><span class="sxs-lookup"><span data-stu-id="d1498-148">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="d1498-149">Selezionare **InsertPerson** nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="d1498-149">Select **InsertPerson** from the drop-down list.</span></span>
-   <span data-ttu-id="d1498-150">Vengono visualizzati i mapping predefiniti tra i parametri delle stored procedure e le proprietà dell'entità.</span><span class="sxs-lookup"><span data-stu-id="d1498-150">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="d1498-151">Come indicato dalle frecce, che mostrano la direzione del mapping, per i parametri delle stored procedure vengono specificati i valori delle proprietà.</span><span class="sxs-lookup"><span data-stu-id="d1498-151">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
-   <span data-ttu-id="d1498-152">Fare clic su  **&lt;Aggiungi Result Binding&gt;**.</span><span class="sxs-lookup"><span data-stu-id="d1498-152">Click **&lt;Add Result Binding&gt;**.</span></span>
-   <span data-ttu-id="d1498-153">Tipo di **NewPersonID**, il nome del parametro restituito dalle **InsertPerson** stored procedure.</span><span class="sxs-lookup"><span data-stu-id="d1498-153">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="d1498-154">Assicurarsi di non inserire spazi iniziali o finali.</span><span class="sxs-lookup"><span data-stu-id="d1498-154">Make sure not to type leading or trailing spaces.</span></span>
-   <span data-ttu-id="d1498-155">Premere **INVIO**.</span><span class="sxs-lookup"><span data-stu-id="d1498-155">Press **Enter**.</span></span>
-   <span data-ttu-id="d1498-156">Per impostazione predefinita **NewPersonID** viene eseguito il mapping alla chiave di entità **PersonID**.</span><span class="sxs-lookup"><span data-stu-id="d1498-156">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="d1498-157">Come indicato dalle frecce, che mostrano la direzione del mapping, per la proprietà viene specificato il valore della colonna dei risultati.</span><span class="sxs-lookup"><span data-stu-id="d1498-157">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![MappingDetails](~/ef6/media/mappingdetails.png)

-   <span data-ttu-id="d1498-159">Fare clic su **&lt;seleziona Update Function&gt;** e selezionare **UpdatePerson** dall'elenco a discesa risultante.</span><span class="sxs-lookup"><span data-stu-id="d1498-159">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="d1498-160">Vengono visualizzati i mapping predefiniti tra i parametri delle stored procedure e le proprietà dell'entità.</span><span class="sxs-lookup"><span data-stu-id="d1498-160">Default mappings between stored procedure parameters and entity properties appear.</span></span>
-   <span data-ttu-id="d1498-161">Fare clic su **&lt;seleziona Delete Function&gt;** e selezionare **DeletePerson** dall'elenco a discesa risultante.</span><span class="sxs-lookup"><span data-stu-id="d1498-161">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="d1498-162">Vengono visualizzati i mapping predefiniti tra i parametri delle stored procedure e le proprietà dell'entità.</span><span class="sxs-lookup"><span data-stu-id="d1498-162">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="d1498-163">L'inserimento, aggiornare ed eliminare le operazioni dei **persona** viene ora eseguito il mapping di tipo di entità alle stored procedure.</span><span class="sxs-lookup"><span data-stu-id="d1498-163">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="d1498-164">Se si desidera abilitare controllo della concorrenza durante l'aggiornamento o eliminazione di un'entità con stored procedure, usare una delle opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="d1498-164">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

-   <span data-ttu-id="d1498-165">Usa un' **OUTPUT** parametro per restituire il numero di righe da stored procedure e controllo interessate le **parametro delle righe interessate** casella di controllo accanto al nome del parametro.</span><span class="sxs-lookup"><span data-stu-id="d1498-165">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="d1498-166">Se il valore restituito è zero quando viene chiamata l'operazione, un' [ **OptimisticConcurrencyException** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) verrà generata.</span><span class="sxs-lookup"><span data-stu-id="d1498-166">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
-   <span data-ttu-id="d1498-167">Verificare i **utilizza Original Value** casella di controllo accanto a una proprietà che si desidera utilizzare per controllo della concorrenza.</span><span class="sxs-lookup"><span data-stu-id="d1498-167">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="d1498-168">Quando si tenta un aggiornamento, il valore della proprietà che è stato originariamente letta dal database da utilizzare durante la scrittura dati nel database.</span><span class="sxs-lookup"><span data-stu-id="d1498-168">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="d1498-169">Se il valore non corrisponde al valore nel database, un' **OptimisticConcurrencyException** verrà generata.</span><span class="sxs-lookup"><span data-stu-id="d1498-169">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="d1498-170">Usare il modello</span><span class="sxs-lookup"><span data-stu-id="d1498-170">Use the Model</span></span>

<span data-ttu-id="d1498-171">Aprire il **Program.cs** file dove il **Main** metodo è definito.</span><span class="sxs-lookup"><span data-stu-id="d1498-171">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="d1498-172">Aggiungere il codice seguente nella funzione Main.</span><span class="sxs-lookup"><span data-stu-id="d1498-172">Add the following code into the Main function.</span></span>

<span data-ttu-id="d1498-173">Il codice crea un nuovo **persona** oggetto, quindi l'oggetto viene aggiornato e infine elimina l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="d1498-173">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>         

``` csharp
    using (var context = new SchoolEntities())
    {
        var newInstructor = new Person
        {
            FirstName = "Robyn",
            LastName = "Martin",
            HireDate = DateTime.Now,
            Discriminator = "Instructor"
        }

        // Add the new object to the context.
        context.People.Add(newInstructor);

        Console.WriteLine("Added {0} {1} to the context.",
            newInstructor.FirstName, newInstructor.LastName);

        Console.WriteLine("Before SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // SaveChanges will call the InsertPerson sproc.  
        // The PersonID property will be assigned the value
        // returned by the sproc.
        context.SaveChanges();

        Console.WriteLine("After SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // Modify the object and call SaveChanges.
        // This time, the UpdatePerson will be called.
        newInstructor.FirstName = "Rachel";
        context.SaveChanges();

        // Remove the object from the context and call SaveChanges.
        // The DeletePerson sproc will be called.
        context.People.Remove(newInstructor);
        context.SaveChanges();

        Person deletedInstructor = context.People.
            Where(p => p.PersonID == newInstructor.PersonID).
            FirstOrDefault();

        if (deletedInstructor == null)
            Console.WriteLine("A person with PersonID {0} was deleted.",
                newInstructor.PersonID);
    }
```

-   <span data-ttu-id="d1498-174">Compilare l'applicazione ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="d1498-174">Compile and run the application.</span></span> <span data-ttu-id="d1498-175">Il programma produce l'output seguente \*</span><span class="sxs-lookup"><span data-stu-id="d1498-175">The program produces the following output \*</span></span>
    >[!NOTE]
> <span data-ttu-id="d1498-176">PersonID viene generato automaticamente dal server, pertanto si noteranno probabilmente un numero diverso \*</span><span class="sxs-lookup"><span data-stu-id="d1498-176">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

```
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="d1498-177">Se si lavora con la versione di Visual Studio Ultimate, è possibile utilizzare Intellitrace con il debugger per visualizzare le istruzioni SQL che vengono eseguite.</span><span class="sxs-lookup"><span data-stu-id="d1498-177">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![IntelliTrace](~/ef6/media/intellitrace.png)
