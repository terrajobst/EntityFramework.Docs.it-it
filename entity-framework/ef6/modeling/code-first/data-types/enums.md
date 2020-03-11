---
title: Supporto enum-Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78419108"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="faec0-102">Supporto enum-Code First</span><span class="sxs-lookup"><span data-stu-id="faec0-102">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="faec0-103">**EF5 solo** le funzionalità, le API e così via descritte in questa pagina sono state introdotte in Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="faec0-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="faec0-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="faec0-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="faec0-105">Questo video e la procedura dettagliata illustrano come usare i tipi enum con Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="faec0-105">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="faec0-106">Viene inoltre illustrato come utilizzare le enumerazioni in una query LINQ.</span><span class="sxs-lookup"><span data-stu-id="faec0-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="faec0-107">In questa procedura dettagliata verrà utilizzato Code First per creare un nuovo database, ma è possibile utilizzare anche [Code First per eseguire il mapping a un database esistente](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="faec0-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="faec0-108">Il supporto enum è stato introdotto in Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="faec0-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="faec0-109">Per utilizzare le nuove funzionalità come enum, i tipi di dati spaziali e le funzioni con valori di tabella, è necessario specificare come destinazione .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="faec0-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="faec0-110">Per impostazione predefinita, Visual Studio 2012 è destinato a .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="faec0-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="faec0-111">In Entity Framework, un'enumerazione può includere i seguenti tipi sottostanti: **byte**, **Int16**, **Int32**, **Int64** o **SByte**.</span><span class="sxs-lookup"><span data-stu-id="faec0-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="faec0-112">Video</span><span class="sxs-lookup"><span data-stu-id="faec0-112">Watch the video</span></span>
<span data-ttu-id="faec0-113">In questo video viene illustrato come utilizzare i tipi enum con Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="faec0-113">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="faec0-114">Viene inoltre illustrato come utilizzare le enumerazioni in una query LINQ.</span><span class="sxs-lookup"><span data-stu-id="faec0-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="faec0-115">**Presentato da**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="faec0-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="faec0-116">**Video**: [wmv](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (zip)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="faec0-116">**Video**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="faec0-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="faec0-117">Pre-Requisites</span></span>

<span data-ttu-id="faec0-118">Per completare questa procedura dettagliata, è necessario che Visual Studio 2012, Ultimate, Premium, Professional o Web Express Edition sia installato.</span><span class="sxs-lookup"><span data-stu-id="faec0-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="faec0-119">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="faec0-119">Set up the Project</span></span>

1.  <span data-ttu-id="faec0-120">Aprire Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="faec0-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="faec0-121">Scegliere **nuovo**dal menu **file** , quindi fare clic su **progetto** .</span><span class="sxs-lookup"><span data-stu-id="faec0-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="faec0-122">Nel riquadro sinistro fare clic su **Visual C\#** e quindi selezionare il modello **console** .</span><span class="sxs-lookup"><span data-stu-id="faec0-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="faec0-123">Immettere **EnumCodeFirst** come nome del progetto e fare clic su **OK** .</span><span class="sxs-lookup"><span data-stu-id="faec0-123">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="faec0-124">Definire un nuovo modello utilizzando Code First</span><span class="sxs-lookup"><span data-stu-id="faec0-124">Define a New Model using Code First</span></span>

<span data-ttu-id="faec0-125">Quando si usa Code First lo sviluppo si inizia in genere scrivendo .NET Framework classi che definiscono il modello concettuale (dominio).</span><span class="sxs-lookup"><span data-stu-id="faec0-125">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="faec0-126">Il codice seguente definisce la classe Department.</span><span class="sxs-lookup"><span data-stu-id="faec0-126">The code below defines the Department class.</span></span>

<span data-ttu-id="faec0-127">Il codice definisce anche l'enumerazione DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="faec0-127">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="faec0-128">Per impostazione predefinita, l'enumerazione è di tipo **int** .</span><span class="sxs-lookup"><span data-stu-id="faec0-128">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="faec0-129">La proprietà Name della classe Department è del tipo DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="faec0-129">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="faec0-130">Aprire il file Program.cs e incollare le definizioni delle classi seguenti.</span><span class="sxs-lookup"><span data-stu-id="faec0-130">Open the Program.cs file and paste the following class definitions.</span></span>

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
}
```
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="faec0-131">Definire il tipo derivato DbContext</span><span class="sxs-lookup"><span data-stu-id="faec0-131">Define the DbContext Derived Type</span></span>

<span data-ttu-id="faec0-132">Oltre alla definizione delle entità, è necessario definire una classe che deriva da DbContext ed espone DbSet&lt;TEntity&gt; proprietà.</span><span class="sxs-lookup"><span data-stu-id="faec0-132">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="faec0-133">Le proprietà DbSet&lt;TEntity&gt; consentono al contesto di individuare i tipi che si desidera includere nel modello.</span><span class="sxs-lookup"><span data-stu-id="faec0-133">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="faec0-134">Un'istanza del tipo derivato DbContext gestisce gli oggetti entità in fase di esecuzione, che include il popolamento di oggetti con dati da un database, il rilevamento delle modifiche e il salvataggio permanente dei dati nel database.</span><span class="sxs-lookup"><span data-stu-id="faec0-134">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="faec0-135">I tipi DbContext e DbSet sono definiti nell'assembly EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="faec0-135">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="faec0-136">Si aggiungerà un riferimento a questa DLL usando il pacchetto NuGet EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="faec0-136">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="faec0-137">In Esplora soluzioni fare clic con il pulsante destro del mouse sul nome del progetto.</span><span class="sxs-lookup"><span data-stu-id="faec0-137">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="faec0-138">Selezionare **Gestisci pacchetti NuGet...**</span><span class="sxs-lookup"><span data-stu-id="faec0-138">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="faec0-139">Nella finestra di dialogo Gestisci pacchetti NuGet selezionare la scheda **online** e scegliere il pacchetto **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="faec0-139">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="faec0-140">Fare clic su **Installa**</span><span class="sxs-lookup"><span data-stu-id="faec0-140">Click **Install**</span></span>

<span data-ttu-id="faec0-141">Si noti che, oltre all'assembly EntityFramework, vengono aggiunti anche i riferimenti agli assembly System. ComponentModel. DataAnnotations e System. Data. Entity.</span><span class="sxs-lookup"><span data-stu-id="faec0-141">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="faec0-142">Nella parte superiore del file Program.cs aggiungere l'istruzione using seguente:</span><span class="sxs-lookup"><span data-stu-id="faec0-142">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="faec0-143">In Program.cs aggiungere la definizione del contesto.</span><span class="sxs-lookup"><span data-stu-id="faec0-143">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="faec0-144">Mantieni e recupera dati</span><span class="sxs-lookup"><span data-stu-id="faec0-144">Persist and Retrieve Data</span></span>

<span data-ttu-id="faec0-145">Aprire il file Program.cs in cui è definito il metodo Main.</span><span class="sxs-lookup"><span data-stu-id="faec0-145">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="faec0-146">Aggiungere il codice seguente nella funzione Main.</span><span class="sxs-lookup"><span data-stu-id="faec0-146">Add the following code into the Main function.</span></span> <span data-ttu-id="faec0-147">Il codice aggiunge un nuovo oggetto Department al contesto.</span><span class="sxs-lookup"><span data-stu-id="faec0-147">The code adds a new Department object to the context.</span></span> <span data-ttu-id="faec0-148">Quindi Salva i dati.</span><span class="sxs-lookup"><span data-stu-id="faec0-148">It then saves the data.</span></span> <span data-ttu-id="faec0-149">Il codice esegue anche una query LINQ che restituisce un reparto in cui il nome è DepartmentNames. English.</span><span class="sxs-lookup"><span data-stu-id="faec0-149">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="faec0-150">Compilare l'applicazione ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="faec0-150">Compile and run the application.</span></span> <span data-ttu-id="faec0-151">Il programma produce l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="faec0-151">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="faec0-152">Visualizzare il database generato</span><span class="sxs-lookup"><span data-stu-id="faec0-152">View the Generated Database</span></span>

<span data-ttu-id="faec0-153">Quando si esegue l'applicazione per la prima volta, il Entity Framework crea automaticamente un database.</span><span class="sxs-lookup"><span data-stu-id="faec0-153">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="faec0-154">Poiché è installato Visual Studio 2012, il database verrà creato nell'istanza del database locale.</span><span class="sxs-lookup"><span data-stu-id="faec0-154">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="faec0-155">Per impostazione predefinita, il Entity Framework denomina il database dopo il nome completo del contesto derivato (per questo esempio **EnumCodeFirst. EnumTestContext**).</span><span class="sxs-lookup"><span data-stu-id="faec0-155">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="faec0-156">Le volte successive verrà utilizzato il database esistente.</span><span class="sxs-lookup"><span data-stu-id="faec0-156">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="faec0-157">Si noti che, se si apportano modifiche al modello dopo che è stato creato il database, è necessario utilizzare Migrazioni Code First per aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="faec0-157">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="faec0-158">Per un esempio di utilizzo delle migrazioni, vedere [Code First a un nuovo database](~/ef6/modeling/code-first/workflows/new-database.md) .</span><span class="sxs-lookup"><span data-stu-id="faec0-158">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="faec0-159">Per visualizzare il database e i dati, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="faec0-159">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="faec0-160">Nel menu principale di Visual Studio 2012 selezionare **visualizza** -&gt; **Esplora oggetti di SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="faec0-160">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="faec0-161">Se il database locale non è presente nell'elenco dei server, fare clic con il pulsante destro del mouse su **SQL Server** e selezionare **Aggiungi SQL Server** utilizzare l' **autenticazione di Windows** predefinita per la connessione all'istanza del database locale.</span><span class="sxs-lookup"><span data-stu-id="faec0-161">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="faec0-162">Espandere il nodo del database locale</span><span class="sxs-lookup"><span data-stu-id="faec0-162">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="faec0-163">Espandere la cartella **database** per visualizzare il nuovo database e passare alla tabella **Department** nota, che Code First non crea una tabella che esegue il mapping al tipo di enumerazione</span><span class="sxs-lookup"><span data-stu-id="faec0-163">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="faec0-164">Per visualizzare i dati, fare clic con il pulsante destro del mouse sulla tabella e selezionare **Visualizza dati** .</span><span class="sxs-lookup"><span data-stu-id="faec0-164">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="faec0-165">Summary</span><span class="sxs-lookup"><span data-stu-id="faec0-165">Summary</span></span>

<span data-ttu-id="faec0-166">In questa procedura dettagliata è stato esaminato come usare i tipi enum con Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="faec0-166">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 
