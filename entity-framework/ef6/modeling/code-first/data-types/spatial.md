---
title: Spaziali - Code - First per Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: 744ea58c7a8796ed20adc304b3928551eb6928a4
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490597"
---
# <a name="spatial---code-first"></a><span data-ttu-id="20a36-102">Spaziali - Code First</span><span class="sxs-lookup"><span data-stu-id="20a36-102">Spatial - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="20a36-103">**EF5 e versioni successive solo** -le funzionalità, le API, e così via illustrati in questa pagina sono stati introdotti in Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="20a36-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="20a36-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="20a36-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="20a36-105">La procedura dettagliata video e procedura dettagliata illustra come eseguire il mapping di tipi di dati spaziali con Code First di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="20a36-105">The video and step-by-step walkthrough shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="20a36-106">Viene inoltre illustrato come utilizzare una query LINQ per trovare una distanza tra due posizioni.</span><span class="sxs-lookup"><span data-stu-id="20a36-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="20a36-107">Questa procedura dettagliata utilizzerà Code First per creare un nuovo database, ma è anche possibile usare [Code First per un database esistente](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="20a36-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="20a36-108">Supporto per il tipo spaziale è stata introdotta in Entity Framework 5.</span><span class="sxs-lookup"><span data-stu-id="20a36-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="20a36-109">Si noti che per usare le nuove funzionalità, ad esempio tipo spaziale, enumerazioni e funzioni con valori di tabella, deve avere come destinazione .NET Framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="20a36-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="20a36-110">Visual Studio 2012 è destinato a .NET 4.5 per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="20a36-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="20a36-111">Per utilizzare i tipi di dati spaziali è anche necessario usare un provider di Entity Framework con supporto spaziale.</span><span class="sxs-lookup"><span data-stu-id="20a36-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="20a36-112">Visualizzare [supporto del provider per i tipi spaziali](~/ef6/fundamentals/providers/spatial-support.md) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="20a36-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="20a36-113">Esistono due tipi di dati spaziali principale: geografia e geometria.</span><span class="sxs-lookup"><span data-stu-id="20a36-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="20a36-114">Il tipo di dati geography archivia dati ellissoidali (ad esempio, cui è coordinate GPS latitudine e longitudine).</span><span class="sxs-lookup"><span data-stu-id="20a36-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="20a36-115">Il tipo di dati geometry rappresenta un sistema di coordinate Euclideo (piano).</span><span class="sxs-lookup"><span data-stu-id="20a36-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="20a36-116">Guarda il video</span><span class="sxs-lookup"><span data-stu-id="20a36-116">Watch the video</span></span>
<span data-ttu-id="20a36-117">In questo video viene illustrato come eseguire il mapping di tipi di dati spaziali con Code First di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="20a36-117">This video shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="20a36-118">Viene inoltre illustrato come utilizzare una query LINQ per trovare una distanza tra due posizioni.</span><span class="sxs-lookup"><span data-stu-id="20a36-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="20a36-119">**Presentato da**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="20a36-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="20a36-120">**Video**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="20a36-120">**Video**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="20a36-121">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="20a36-121">Pre-Requisites</span></span>

<span data-ttu-id="20a36-122">È necessario disporre di Visual Studio 2012, Ultimate, Premium, Professional o Web Express edition per completare questa procedura dettagliata.</span><span class="sxs-lookup"><span data-stu-id="20a36-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="20a36-123">Configurare il progetto</span><span class="sxs-lookup"><span data-stu-id="20a36-123">Set up the Project</span></span>

1.  <span data-ttu-id="20a36-124">Aprire Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="20a36-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="20a36-125">Nel **File** dal menu **New**, quindi fare clic su **progetto**</span><span class="sxs-lookup"><span data-stu-id="20a36-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="20a36-126">Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **Console** modello</span><span class="sxs-lookup"><span data-stu-id="20a36-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="20a36-127">Immettere **SpatialCodeFirst** come nome del progetto e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="20a36-127">Enter **SpatialCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="20a36-128">Definire un nuovo modello tramite Code First</span><span class="sxs-lookup"><span data-stu-id="20a36-128">Define a New Model using Code First</span></span>

<span data-ttu-id="20a36-129">Quando si usa lo sviluppo Code First è in genere iniziare la scrittura di classi .NET Framework che definiscono il modello concettuale (dominio).</span><span class="sxs-lookup"><span data-stu-id="20a36-129">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="20a36-130">Il codice seguente definisce la classe University.</span><span class="sxs-lookup"><span data-stu-id="20a36-130">The code below defines the University class.</span></span>

<span data-ttu-id="20a36-131">L'università ha la proprietà Location del tipo di DbGeography.</span><span class="sxs-lookup"><span data-stu-id="20a36-131">The University has the Location property of the DbGeography type.</span></span> <span data-ttu-id="20a36-132">Per usare il tipo di DbGeography, è necessario aggiungere un riferimento all'assembly Data. Entity e anche aggiungere il System.Data.Spatial istruzione using.</span><span class="sxs-lookup"><span data-stu-id="20a36-132">To use the DbGeography type, you must add a reference to the System.Data.Entity assembly and also add the System.Data.Spatial using statement.</span></span>

<span data-ttu-id="20a36-133">Aprire il file Program.cs e incollare quanto segue usando istruzioni all'inizio del file:</span><span class="sxs-lookup"><span data-stu-id="20a36-133">Open the Program.cs file and paste the following using statements at the top of the file:</span></span>

``` csharp
using System.Data.Spatial;
```

<span data-ttu-id="20a36-134">Aggiungere la definizione di classe University seguente al file Program.cs.</span><span class="sxs-lookup"><span data-stu-id="20a36-134">Add the following University class definition to the Program.cs file.</span></span>

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="20a36-135">Definire DbContext tipo derivato</span><span class="sxs-lookup"><span data-stu-id="20a36-135">Define the DbContext Derived Type</span></span>

<span data-ttu-id="20a36-136">Oltre a definire le entità, è necessario definire una classe che deriva da DbContext ed espone DbSet&lt;TEntity&gt; proprietà.</span><span class="sxs-lookup"><span data-stu-id="20a36-136">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="20a36-137">Alla classe DbSet&lt;TEntity&gt; proprietà informare il contesto di quali tipi di cui si desidera includere nel modello.</span><span class="sxs-lookup"><span data-stu-id="20a36-137">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="20a36-138">Un'istanza del tipo DbContext derivata gestisce gli oggetti entità durante la fase di esecuzione, che include la compilazione di oggetti con i dati da un database, modificare i dati di rilevamento e persistenza nel database.</span><span class="sxs-lookup"><span data-stu-id="20a36-138">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="20a36-139">I tipi DbContext e DbSet definiti nell'assembly EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="20a36-139">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="20a36-140">Si aggiungerà un riferimento a questa DLL usando il pacchetto EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="20a36-140">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="20a36-141">In Esplora soluzioni fare doppio clic sul nome del progetto.</span><span class="sxs-lookup"><span data-stu-id="20a36-141">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="20a36-142">Selezionare **Gestisci pacchetti NuGet...**</span><span class="sxs-lookup"><span data-stu-id="20a36-142">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="20a36-143">Nella finestra di dialogo Gestisci pacchetti NuGet, selezionare la **Online** scheda e scegliere il **EntityFramework** pacchetto.</span><span class="sxs-lookup"><span data-stu-id="20a36-143">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="20a36-144">Fare clic su **installare**</span><span class="sxs-lookup"><span data-stu-id="20a36-144">Click **Install**</span></span>

<span data-ttu-id="20a36-145">Si noti che oltre all'assembly EntityFramework, un riferimento all'assembly System.ComponentModel.DataAnnotations viene anche aggiunto.</span><span class="sxs-lookup"><span data-stu-id="20a36-145">Note, that in addition to the EntityFramework  assembly, a reference to the System.ComponentModel.DataAnnotations assembly is also added.</span></span>

<span data-ttu-id="20a36-146">Nella parte superiore del file Program.cs, aggiungere la seguente istruzione using:</span><span class="sxs-lookup"><span data-stu-id="20a36-146">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="20a36-147">Nel file Program.cs aggiungere la definizione di contesto.</span><span class="sxs-lookup"><span data-stu-id="20a36-147">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="20a36-148">Salvare in modo permanente e recuperare i dati</span><span class="sxs-lookup"><span data-stu-id="20a36-148">Persist and Retrieve Data</span></span>

<span data-ttu-id="20a36-149">Aprire il file Program.cs in cui è definito il metodo Main.</span><span class="sxs-lookup"><span data-stu-id="20a36-149">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="20a36-150">Aggiungere il codice seguente nella funzione Main.</span><span class="sxs-lookup"><span data-stu-id="20a36-150">Add the following code into the Main function.</span></span>

<span data-ttu-id="20a36-151">Il codice aggiunge due nuovi oggetti University al contesto.</span><span class="sxs-lookup"><span data-stu-id="20a36-151">The code adds two new University objects to the context.</span></span> <span data-ttu-id="20a36-152">Proprietà spaziali vengono inizializzate tramite il metodo DbGeography.FromText.</span><span class="sxs-lookup"><span data-stu-id="20a36-152">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="20a36-153">Il punto di geografia è rappresentato come WellKnownText viene passato al metodo.</span><span class="sxs-lookup"><span data-stu-id="20a36-153">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="20a36-154">Il codice quindi Salva i dati.</span><span class="sxs-lookup"><span data-stu-id="20a36-154">The code then saves the data.</span></span> <span data-ttu-id="20a36-155">Quindi, LINQ query che restituisce un oggetto University dove il percorso è più vicino alla posizione specificata, viene costruita ed eseguita.</span><span class="sxs-lookup"><span data-stu-id="20a36-155">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
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

<span data-ttu-id="20a36-156">Compilare l'applicazione ed eseguirla.</span><span class="sxs-lookup"><span data-stu-id="20a36-156">Compile and run the application.</span></span> <span data-ttu-id="20a36-157">Il programma produce l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="20a36-157">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a><span data-ttu-id="20a36-158">Visualizzazione Database generato</span><span class="sxs-lookup"><span data-stu-id="20a36-158">View the Generated Database</span></span>

<span data-ttu-id="20a36-159">Quando si esegue la prima volta che l'applicazione, Entity Framework crea un database.</span><span class="sxs-lookup"><span data-stu-id="20a36-159">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="20a36-160">Perché abbiamo installato Visual Studio 2012, il database verrà creato nell'istanza di Local DB.</span><span class="sxs-lookup"><span data-stu-id="20a36-160">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="20a36-161">Per impostazione predefinita, Entity Framework nome del database dopo il nome completo del contesto derivato (in questo esempio corrisponde **SpatialCodeFirst.UniversityContext**).</span><span class="sxs-lookup"><span data-stu-id="20a36-161">By default, the Entity Framework names the database after the fully qualified name of the derived context (in this example that is **SpatialCodeFirst.UniversityContext**).</span></span> <span data-ttu-id="20a36-162">Le volte successive che verrà utilizzato il database esistente.</span><span class="sxs-lookup"><span data-stu-id="20a36-162">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="20a36-163">Si noti che se si apportano modifiche al modello dopo aver creato il database, è necessario utilizzare migrazioni Code First per aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="20a36-163">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="20a36-164">Visualizzare [Code First per un nuovo Database](~/ef6/modeling/code-first/workflows/new-database.md) per un esempio di usare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="20a36-164">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="20a36-165">Per visualizzare i database e i dati, eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="20a36-165">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="20a36-166">Nel menu principale di Visual Studio 2012, selezionare **View**  - &gt; **Esplora oggetti di SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="20a36-166">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="20a36-167">Se Local DB non è presente nell'elenco dei server, fare clic il pulsante destro del mouse su **SQL Server** e selezionare **Aggiungi SQL Server** usare il valore predefinito **l'autenticazione di Windows** per connettersi al l'istanza di Local DB</span><span class="sxs-lookup"><span data-stu-id="20a36-167">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the the LocalDB instance</span></span>
3.  <span data-ttu-id="20a36-168">Espandere il nodo del database locale</span><span class="sxs-lookup"><span data-stu-id="20a36-168">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="20a36-169">Espandi la **database** per visualizzare il nuovo database e passare alla cartella il **università** tabella</span><span class="sxs-lookup"><span data-stu-id="20a36-169">Unfold the **Databases** folder to see the new database and browse to the **Universities** table</span></span>
5.  <span data-ttu-id="20a36-170">Per visualizzare i dati, fare doppio clic sulla tabella e selezionare **Visualizza dati**</span><span class="sxs-lookup"><span data-stu-id="20a36-170">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="20a36-171">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="20a36-171">Summary</span></span>

<span data-ttu-id="20a36-172">In questa procedura dettagliata è stato illustrato come usare i tipi spaziali con Code First di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="20a36-172">In this walkthrough we looked at how to use spatial types with Entity Framework Code First.</span></span> 
