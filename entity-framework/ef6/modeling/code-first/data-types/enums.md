---
title: Supporto di enum - Code First - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 986991c2dd9470867aaf5424ecb176d7db172426
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489505"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="15f15-102">Supporto di enum - Code First</span><span class="sxs-lookup"><span data-stu-id="15f15-102">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="15f15-103">**EF5 e versioni successive solo** -le funzionalità, le API, e così via illustrati in questa pagina sono stati introdotti in Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="15f15-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="15f15-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="15f15-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="15f15-105">Questa procedura dettagliata video e procedura dettagliata viene illustrato come utilizzare tipi enum con Code First di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="15f15-105">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="15f15-106">Viene inoltre illustrato come usare enumerazioni in una query LINQ.</span><span class="sxs-lookup"><span data-stu-id="15f15-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="15f15-107">Questa procedura dettagliata utilizzerà Code First per creare un nuovo database, ma è anche possibile usare [Code First per eseguire il mapping a un database esistente](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="15f15-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="15f15-108">Supporto di enumerazione è stata introdotta in Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="15f15-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="15f15-109">Per usare le nuove funzionalità come le enumerazioni, tipi di dati spaziali e funzioni con valori di tabella, deve avere come destinazione .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="15f15-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="15f15-110">Visual Studio 2012 è destinato a .NET 4.5 per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="15f15-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="15f15-111">In Entity Framework, un'enumerazione può avere i seguenti tipi sottostanti: **Byte**, **Int16**, **Int32**, **Int64** , o **SByte**.</span><span class="sxs-lookup"><span data-stu-id="15f15-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="15f15-112">Guarda il video</span><span class="sxs-lookup"><span data-stu-id="15f15-112">Watch the video</span></span>
<span data-ttu-id="15f15-113">In questo video viene illustrato come utilizzare tipi enum con Code First di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="15f15-113">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="15f15-114">Viene inoltre illustrato come usare enumerazioni in una query LINQ.</span><span class="sxs-lookup"><span data-stu-id="15f15-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="15f15-115">**Presentato da**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="15f15-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="15f15-116">**Video**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="15f15-116">**Video**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="15f15-117">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="15f15-117">Pre-Requisites</span></span>

<span data-ttu-id="15f15-118">È necessario disporre di Visual Studio 2012, Ultimate, Premium, Professional o Web Express edition per completare questa procedura dettagliata.</span><span class="sxs-lookup"><span data-stu-id="15f15-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="15f15-119">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="15f15-119">Set up the Project</span></span>

1.  <span data-ttu-id="15f15-120">Aprire Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="15f15-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="15f15-121">Nel **File** dal menu **New**, quindi fare clic su **progetto**</span><span class="sxs-lookup"><span data-stu-id="15f15-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="15f15-122">Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **Console** modello</span><span class="sxs-lookup"><span data-stu-id="15f15-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="15f15-123">Immettere **EnumCodeFirst** come nome del progetto e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="15f15-123">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="15f15-124">Definire un nuovo modello tramite Code First</span><span class="sxs-lookup"><span data-stu-id="15f15-124">Define a New Model using Code First</span></span>

<span data-ttu-id="15f15-125">Quando si usa lo sviluppo Code First è in genere iniziare la scrittura di classi .NET Framework che definiscono il modello concettuale (dominio).</span><span class="sxs-lookup"><span data-stu-id="15f15-125">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="15f15-126">Il codice seguente definisce la classe di reparto.</span><span class="sxs-lookup"><span data-stu-id="15f15-126">The code below defines the Department class.</span></span>

<span data-ttu-id="15f15-127">Il codice definisce anche l'enumerazione DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="15f15-127">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="15f15-128">Per impostazione predefinita, l'enumerazione presenta **int** tipo.</span><span class="sxs-lookup"><span data-stu-id="15f15-128">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="15f15-129">La proprietà Name nella classe Department è del tipo DepartmentNames.</span><span class="sxs-lookup"><span data-stu-id="15f15-129">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="15f15-130">Aprire il file Program.cs e incollare le seguenti definizioni di classe.</span><span class="sxs-lookup"><span data-stu-id="15f15-130">Open the Program.cs file and paste the following class definitions.</span></span>

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
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="15f15-131">Definire DbContext tipo derivato</span><span class="sxs-lookup"><span data-stu-id="15f15-131">Define the DbContext Derived Type</span></span>

<span data-ttu-id="15f15-132">Oltre a definire le entità, è necessario definire una classe che deriva da DbContext ed espone DbSet&lt;TEntity&gt; proprietà.</span><span class="sxs-lookup"><span data-stu-id="15f15-132">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="15f15-133">Alla classe DbSet&lt;TEntity&gt; proprietà informare il contesto di quali tipi di cui si desidera includere nel modello.</span><span class="sxs-lookup"><span data-stu-id="15f15-133">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="15f15-134">Un'istanza del tipo DbContext derivata gestisce gli oggetti entità durante la fase di esecuzione, che include la compilazione di oggetti con i dati da un database, modificare i dati di rilevamento e persistenza nel database.</span><span class="sxs-lookup"><span data-stu-id="15f15-134">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="15f15-135">I tipi DbContext e DbSet definiti nell'assembly EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="15f15-135">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="15f15-136">Si aggiungerà un riferimento a questa DLL usando il pacchetto EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="15f15-136">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="15f15-137">In Esplora soluzioni fare doppio clic sul nome del progetto.</span><span class="sxs-lookup"><span data-stu-id="15f15-137">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="15f15-138">Selezionare **Gestisci pacchetti NuGet...**</span><span class="sxs-lookup"><span data-stu-id="15f15-138">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="15f15-139">Nella finestra di dialogo Gestisci pacchetti NuGet, selezionare la **Online** scheda e scegliere il **EntityFramework** pacchetto.</span><span class="sxs-lookup"><span data-stu-id="15f15-139">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="15f15-140">Fare clic su **installare**</span><span class="sxs-lookup"><span data-stu-id="15f15-140">Click **Install**</span></span>

<span data-ttu-id="15f15-141">Si noti che oltre all'assembly di Entity Framework, i riferimenti agli assembly System.ComponentModel.DataAnnotations e Data. Entity vengono aggiunti anche.</span><span class="sxs-lookup"><span data-stu-id="15f15-141">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="15f15-142">Nella parte superiore del file Program.cs, aggiungere la seguente istruzione using:</span><span class="sxs-lookup"><span data-stu-id="15f15-142">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="15f15-143">Nel file Program.cs aggiungere la definizione di contesto.</span><span class="sxs-lookup"><span data-stu-id="15f15-143">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="15f15-144">Salvare in modo permanente e recuperare i dati</span><span class="sxs-lookup"><span data-stu-id="15f15-144">Persist and Retrieve Data</span></span>

<span data-ttu-id="15f15-145">Aprire il file Program.cs in cui è definito il metodo Main.</span><span class="sxs-lookup"><span data-stu-id="15f15-145">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="15f15-146">Aggiungere il codice seguente nella funzione Main.</span><span class="sxs-lookup"><span data-stu-id="15f15-146">Add the following code into the Main function.</span></span> <span data-ttu-id="15f15-147">Il codice aggiunge un nuovo oggetto reparto al contesto.</span><span class="sxs-lookup"><span data-stu-id="15f15-147">The code adds a new Department object to the context.</span></span> <span data-ttu-id="15f15-148">Quindi Salva i dati.</span><span class="sxs-lookup"><span data-stu-id="15f15-148">It then saves the data.</span></span> <span data-ttu-id="15f15-149">Inoltre, il codice esegue una query LINQ che restituisce un reparto dove name è DepartmentNames.English.</span><span class="sxs-lookup"><span data-stu-id="15f15-149">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

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

<span data-ttu-id="15f15-150">Compilare l'applicazione ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="15f15-150">Compile and run the application.</span></span> <span data-ttu-id="15f15-151">Il programma produce l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="15f15-151">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="15f15-152">Visualizzazione Database generato</span><span class="sxs-lookup"><span data-stu-id="15f15-152">View the Generated Database</span></span>

<span data-ttu-id="15f15-153">Quando si esegue la prima volta che l'applicazione, Entity Framework crea un database.</span><span class="sxs-lookup"><span data-stu-id="15f15-153">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="15f15-154">Perché abbiamo installato Visual Studio 2012, il database verrà creato nell'istanza di Local DB.</span><span class="sxs-lookup"><span data-stu-id="15f15-154">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="15f15-155">Per impostazione predefinita, Entity Framework nome del database dopo il nome completo del contesto derivato (per questo esempio corrisponde **EnumCodeFirst.EnumTestContext**).</span><span class="sxs-lookup"><span data-stu-id="15f15-155">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="15f15-156">Le volte successive che verrà utilizzato il database esistente.</span><span class="sxs-lookup"><span data-stu-id="15f15-156">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="15f15-157">Si noti che se si apportano modifiche al modello dopo aver creato il database, è necessario utilizzare migrazioni Code First per aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="15f15-157">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="15f15-158">Visualizzare [Code First per un nuovo Database](~/ef6/modeling/code-first/workflows/new-database.md) per un esempio di usare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="15f15-158">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="15f15-159">Per visualizzare i database e i dati, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="15f15-159">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="15f15-160">Nel menu principale di Visual Studio 2012, selezionare **View**  - &gt; **Esplora oggetti di SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="15f15-160">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="15f15-161">Se Local DB non è presente nell'elenco dei server, fare clic il pulsante destro del mouse su **SQL Server** e selezionare **Aggiungi SQL Server** usare il valore predefinito **l'autenticazione di Windows** per connettersi al Istanza di Local DB</span><span class="sxs-lookup"><span data-stu-id="15f15-161">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="15f15-162">Espandere il nodo del database locale</span><span class="sxs-lookup"><span data-stu-id="15f15-162">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="15f15-163">Espandi la **database** per visualizzare il nuovo database e passare alla cartella il **reparto** si noti che Code First non crea una tabella in cui viene eseguito il mapping al tipo di enumerazione di tabella</span><span class="sxs-lookup"><span data-stu-id="15f15-163">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="15f15-164">Per visualizzare i dati, fare doppio clic sulla tabella e selezionare **Visualizza dati**</span><span class="sxs-lookup"><span data-stu-id="15f15-164">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="15f15-165">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="15f15-165">Summary</span></span>

<span data-ttu-id="15f15-166">In questa procedura dettagliata è stato illustrato come usare i tipi di enumerazione con Code First di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="15f15-166">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 
