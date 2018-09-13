---
title: Model-First - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 8e010f95db40261073b4af80a3c0e3225a2cd1cf
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490480"
---
# <a name="model-first"></a><span data-ttu-id="b1ff6-102">A partire dal modello</span><span class="sxs-lookup"><span data-stu-id="b1ff6-102">Model First</span></span>
<span data-ttu-id="b1ff6-103">Questa procedura dettagliata video e dettagliata forniscono un'introduzione allo sviluppo Model First usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-103">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="b1ff6-104">Modello prima di tutto consente di creare un nuovo modello utilizzando Entity Framework Designer e quindi generare uno schema di database dal modello.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-104">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="b1ff6-105">Il modello viene archiviato in un file EDMX (file con estensione edmx) e può essere visualizzato e modificato in Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-105">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="b1ff6-106">Le classi che si interagiscono con nell'applicazione vengono generate automaticamente dal file con estensione EDMX.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-106">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="b1ff6-107">Guarda il video</span><span class="sxs-lookup"><span data-stu-id="b1ff6-107">Watch the video</span></span>
<span data-ttu-id="b1ff6-108">Questa procedura dettagliata video e dettagliata forniscono un'introduzione allo sviluppo Model First usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-108">This video and step-by-step walkthrough provide an introduction to Model First development using Entity Framework.</span></span> <span data-ttu-id="b1ff6-109">Modello prima di tutto consente di creare un nuovo modello utilizzando Entity Framework Designer e quindi generare uno schema di database dal modello.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-109">Model First allows you to create a new model using the Entity Framework Designer and then generate a database schema from the model.</span></span> <span data-ttu-id="b1ff6-110">Il modello viene archiviato in un file EDMX (file con estensione edmx) e può essere visualizzato e modificato in Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-110">The model is stored in an EDMX file (.edmx extension) and can be viewed and edited in the Entity Framework Designer.</span></span> <span data-ttu-id="b1ff6-111">Le classi che si interagiscono con nell'applicazione vengono generate automaticamente dal file con estensione EDMX.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-111">The classes that you interact with in your application are automatically generated from the EDMX file.</span></span>

<span data-ttu-id="b1ff6-112">**Presentato da**: [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="b1ff6-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="b1ff6-113">**Video**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span><span class="sxs-lookup"><span data-stu-id="b1ff6-113">**Video**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="b1ff6-114">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="b1ff6-114">Pre-Requisites</span></span>

<span data-ttu-id="b1ff6-115">Dovrai disporre di Visual Studio 2010 o Visual Studio 2012 è installato per completare questa procedura dettagliata.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-115">You will need to have Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="b1ff6-116">Se si usa Visual Studio 2010, è necessario anche avere [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installato.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-116">If you are using Visual Studio 2010, you will also need to have [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="b1ff6-117">1. Creare l'applicazione</span><span class="sxs-lookup"><span data-stu-id="b1ff6-117">1. Create the Application</span></span>

<span data-ttu-id="b1ff6-118">Per semplificare le operazioni verranno creare un'applicazione console di base che usa il primo modello per eseguire l'accesso ai dati:</span><span class="sxs-lookup"><span data-stu-id="b1ff6-118">To keep things simple we’re going to build a basic console application that uses the Model First to perform data access:</span></span>

-   <span data-ttu-id="b1ff6-119">Aprire Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1ff6-119">Open Visual Studio</span></span>
-   <span data-ttu-id="b1ff6-120">**File -&gt; New -&gt; progetto...**</span><span class="sxs-lookup"><span data-stu-id="b1ff6-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="b1ff6-121">Selezionare **Windows** nel menu a sinistra e **applicazione Console**</span><span class="sxs-lookup"><span data-stu-id="b1ff6-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="b1ff6-122">Immettere **ModelFirstSample** come nome</span><span class="sxs-lookup"><span data-stu-id="b1ff6-122">Enter **ModelFirstSample** as the name</span></span>
-   <span data-ttu-id="b1ff6-123">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-123">Select **OK**</span></span>

## <a name="2-create-model"></a><span data-ttu-id="b1ff6-124">2. Crea modello</span><span class="sxs-lookup"><span data-stu-id="b1ff6-124">2. Create Model</span></span>

<span data-ttu-id="b1ff6-125">Dobbiamo usare Entity Framework Designer, che è incluso come parte di Visual Studio, per creare il modello.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-125">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="b1ff6-126">**Progetto -&gt; Aggiungi nuovo elemento...**</span><span class="sxs-lookup"><span data-stu-id="b1ff6-126">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="b1ff6-127">Selezionare **Data** nel menu a sinistra e quindi **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="b1ff6-127">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="b1ff6-128">Immettere **BloggingModel** come nome, quindi fare clic su **OK**, verrà avviata la procedura guidata Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="b1ff6-128">Enter **BloggingModel** as the name and click **OK**, this launches the Entity Data Model Wizard</span></span>
-   <span data-ttu-id="b1ff6-129">Selezionare **modello vuoto** e fare clic su **fine**</span><span class="sxs-lookup"><span data-stu-id="b1ff6-129">Select **Empty Model** and click **Finish**</span></span>

    ![Creare un modello vuoto](~/ef6/media/createemptymodel.png)

<span data-ttu-id="b1ff6-131">Entity Framework Designer viene aperto con un modello vuoto.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-131">The Entity Framework Designer is opened with a blank model.</span></span> <span data-ttu-id="b1ff6-132">A questo punto è possibile iniziare l'aggiunta di entità, proprietà e associazioni nel modello.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-132">Now we can start adding entities, properties and associations to the model.</span></span>

-   <span data-ttu-id="b1ff6-133">Fare clic su area di progettazione e seleziona **proprietà**</span><span class="sxs-lookup"><span data-stu-id="b1ff6-133">Right-click on the design surface and select **Properties**</span></span>
-   <span data-ttu-id="b1ff6-134">La modifica della finestra delle proprietà di **nome contenitore entità** al **BloggingContext**
    *si tratta del nome del contesto derivato che verrà generato automaticamente, il contesto rappresenta una sessione con il database, che consente di eseguire una query e salvare i dati*</span><span class="sxs-lookup"><span data-stu-id="b1ff6-134">In the Properties window change the **Entity Container Name** to **BloggingContext**
*This is the name of the derived context that will be generated for you, the context represents a session with the database, allowing us to query and save data*</span></span>
-   <span data-ttu-id="b1ff6-135">Fare clic su area di progettazione e seleziona **Aggiungi nuovo -&gt; Entity...**</span><span class="sxs-lookup"><span data-stu-id="b1ff6-135">Right-click on the design surface and select **Add New -&gt; Entity…**</span></span>
-   <span data-ttu-id="b1ff6-136">Immettere **Blog** come nome dell'entità e **BlogId** come nome della chiave e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="b1ff6-136">Enter **Blog** as the entity name and **BlogId** as the key name and click **OK**</span></span>

    ![Aggiungere entità Blog](~/ef6/media/addblogentity.png)

-   <span data-ttu-id="b1ff6-138">Pulsante destro del mouse sulla nuova entità in area di progettazione e seleziona **Aggiungi nuovo -&gt; proprietà scalare**, immettere **nome** come il nome della proprietà.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-138">Right-click on the new entity on the design surface and select **Add New -&gt; Scalar Property**, enter **Name** as the name of the property.</span></span>
-   <span data-ttu-id="b1ff6-139">Ripetere questo processo per aggiungere un **Url** proprietà.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-139">Repeat this process to add a **Url** property.</span></span>
-   <span data-ttu-id="b1ff6-140">Fare clic sul **Url** proprietà nell'area di progettazione e seleziona **proprietà**, la modifica della finestra delle proprietà di **Nullable** impostazione su **True** 
     *Ciò ci consente di risparmiare un Blog per il database senza assegnarle un Url*</span><span class="sxs-lookup"><span data-stu-id="b1ff6-140">Right-click on the **Url** property on the design surface and select **Properties**, in the Properties window change the **Nullable** setting to **True**
*This allows us to save a Blog to the database without assigning it a Url*</span></span>
-   <span data-ttu-id="b1ff6-141">Utilizzando le tecniche che appena appreso, aggiungere un **Post** entità con un **PostId** proprietà chiave</span><span class="sxs-lookup"><span data-stu-id="b1ff6-141">Using the techniques you just learnt, add a **Post** entity with a **PostId** key property</span></span>
-   <span data-ttu-id="b1ff6-142">Aggiungere **Title** e **contenuto** alle proprietà scalari per il **Post** entità</span><span class="sxs-lookup"><span data-stu-id="b1ff6-142">Add **Title** and **Content** scalar properties to the **Post** entity</span></span>

<span data-ttu-id="b1ff6-143">Ora che abbiamo un paio di entità, è possibile aggiungere un'associazione (o la relazione) tra di essi.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-143">Now that we have a couple of entities, it’s time to add an association (or relationship) between them.</span></span>

-   <span data-ttu-id="b1ff6-144">Fare clic su area di progettazione e seleziona **Aggiungi nuovo -&gt; Association...**</span><span class="sxs-lookup"><span data-stu-id="b1ff6-144">Right-click on the design surface and select **Add New -&gt; Association…**</span></span>
-   <span data-ttu-id="b1ff6-145">Rendere un'estremità della relazione punto a **Blog** con una molteplicità pari **uno** e l'altro punto di fine per **Post** con una molteplicità pari a **molti** 
     *Ciò significa che un Blog dispone di molti inserimenti e una richiesta Post a cui appartiene un Blog*</span><span class="sxs-lookup"><span data-stu-id="b1ff6-145">Make one end of the relationship point to **Blog** with a multiplicity of **One** and the other end point to **Post** with a multiplicity of **Many**
*This means that a Blog has many Posts and a Post belongs to one Blog*</span></span>
-   <span data-ttu-id="b1ff6-146">Verificare che il **aggiungere le proprietà di chiave esterna 'Post' entità** casella è selezionata e fare clic su **OK**</span><span class="sxs-lookup"><span data-stu-id="b1ff6-146">Ensure the **Add foreign key properties to 'Post' Entity** box is checked and click **OK**</span></span>

    ![Aggiungere un'associazione MF](~/ef6/media/addassociationmf.png)

<span data-ttu-id="b1ff6-148">È ora disponibile un modello semplice che è possibile generare un database e usare per leggere e scrivere dati.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-148">We now have a simple model that we can generate a database from and use to read and write data.</span></span>

![Modello iniziale](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="b1ff6-150">Passaggi aggiuntivi in Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="b1ff6-150">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="b1ff6-151">Se si lavora in Visual Studio 2010, esistono alcuni passaggi aggiuntivi da seguire per eseguire l'aggiornamento alla versione più recente di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-151">If you are working in Visual Studio 2010 there are some additional steps you need to follow to upgrade to the latest version of Entity Framework.</span></span> <span data-ttu-id="b1ff6-152">L'aggiornamento è importante perché consente di accedere a una superficie dell'API migliorata, che è molto più semplice da utilizzare, nonché le correzioni di bug più recenti.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-152">Upgrading is important because it gives you access to an improved API surface, that is much easier to use, as well as the latest bug fixes.</span></span>

<span data-ttu-id="b1ff6-153">Innanzitutto, è necessario ottenere la versione più recente di Entity Framework da NuGet.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-153">First up, we need to get the latest version of Entity Framework from NuGet.</span></span>

-   <span data-ttu-id="b1ff6-154">\*\*Progetto –&gt; Gestisci pacchetti NuGet... \*\* 
     \*Se non si dispone di \**Gestisci pacchetti NuGet... \*\* opzione, è necessario installare il [versione più recente di NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span><span class="sxs-lookup"><span data-stu-id="b1ff6-154">**Project –&gt; Manage NuGet Packages…**
*If you don’t have the **Manage NuGet Packages…** option you should install the [latest version of NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*</span></span>
-   <span data-ttu-id="b1ff6-155">Selezionare il **Online** scheda</span><span class="sxs-lookup"><span data-stu-id="b1ff6-155">Select the **Online** tab</span></span>
-   <span data-ttu-id="b1ff6-156">Selezionare il **EntityFramework** pacchetto</span><span class="sxs-lookup"><span data-stu-id="b1ff6-156">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="b1ff6-157">Fare clic su **installare**</span><span class="sxs-lookup"><span data-stu-id="b1ff6-157">Click **Install**</span></span>

<span data-ttu-id="b1ff6-158">Successivamente, è necessario scambiare il nostro modello per generare il codice che usa l'API DbContext, che è stato introdotto nelle versioni più recenti di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-158">Next, we need to swap our model to generate code that makes use of the DbContext API, which was introduced in later versions of Entity Framework.</span></span>

-   <span data-ttu-id="b1ff6-159">Fare clic su un punto vuoto del modello in Entity Framework Designer e selezionare **Aggiungi elemento di generazione di codice...**</span><span class="sxs-lookup"><span data-stu-id="b1ff6-159">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="b1ff6-160">Selezionare **modelli Online** dal menu a sinistra e cercare **DbContext**</span><span class="sxs-lookup"><span data-stu-id="b1ff6-160">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="b1ff6-161">Selezionare il EF **5.x generatore di DbContext per C\#**, immettere **BloggingModel** come nome, quindi fare clic su **Add**</span><span class="sxs-lookup"><span data-stu-id="b1ff6-161">Select the EF **5.x DbContext Generator for C\#**, enter **BloggingModel** as the name and click **Add**</span></span>

    ![Modello di DbContext](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a><span data-ttu-id="b1ff6-163">3. Generazione del Database</span><span class="sxs-lookup"><span data-stu-id="b1ff6-163">3. Generating the Database</span></span>

<span data-ttu-id="b1ff6-164">Dato il modello, Entity Framework consente di calcolare uno schema di database che ci consentirà di archiviare e recuperare dati usando il modello.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-164">Given our model, Entity Framework can calculate a database schema that will allow us to store and retrieve data using the model.</span></span>

<span data-ttu-id="b1ff6-165">Il server di database che viene installato con Visual Studio è diverso a seconda della versione di Visual Studio installata:</span><span class="sxs-lookup"><span data-stu-id="b1ff6-165">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="b1ff6-166">Se si usa Visual Studio 2010 si creerà un database SQL Express.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-166">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="b1ff6-167">Se si usa Visual Studio 2012, si creerà una [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-167">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) database.</span></span>

<span data-ttu-id="b1ff6-168">È possibile procedere e generare il database.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-168">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="b1ff6-169">Fare clic su area di progettazione e seleziona **genera Database da modello...**</span><span class="sxs-lookup"><span data-stu-id="b1ff6-169">Right-click on the design surface and select **Generate Database from Model…**</span></span>
-   <span data-ttu-id="b1ff6-170">Fare clic su **nuova connessione...**</span><span class="sxs-lookup"><span data-stu-id="b1ff6-170">Click **New Connection…**</span></span> <span data-ttu-id="b1ff6-171">e specificare LocalDB o SQL Express, a seconda di quale versione di Visual Studio in uso, immettere **ModelFirst.Blogging** come nome del database.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-171">and specify either LocalDB or SQL Express, depending on which version of Visual Studio you are using, enter **ModelFirst.Blogging** as the database name.</span></span>

    ![Connessione di Local DB MF](~/ef6/media/localdbconnectionmf.png)

    ![Connessione di SQL Express MF](~/ef6/media/sqlexpressconnectionmf.png)

-   <span data-ttu-id="b1ff6-174">Selezionare **OK** e verrà richiesto se si desidera creare un nuovo database, selezionare **Sì**</span><span class="sxs-lookup"><span data-stu-id="b1ff6-174">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="b1ff6-175">Selezionare **successivo** ed Entity Framework Designer consentirà di calcolare uno script per creare lo schema del database</span><span class="sxs-lookup"><span data-stu-id="b1ff6-175">Select **Next** and the Entity Framework Designer will calculate a script to create the database schema</span></span>
-   <span data-ttu-id="b1ff6-176">Quando viene visualizzato lo script, fare clic su **fine** e lo script verrà aggiunto al progetto e aperto</span><span class="sxs-lookup"><span data-stu-id="b1ff6-176">Once the script is displayed, click **Finish** and the script will be added to your project and opened</span></span>
-   <span data-ttu-id="b1ff6-177">Fare clic su script e selezionare **Execute**, verrà richiesto di specificare il database a cui connettersi, specificare LocalDB o Express di SQL Server, a seconda di quale versione di Visual Studio in uso</span><span class="sxs-lookup"><span data-stu-id="b1ff6-177">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="b1ff6-178">4. La lettura e scrittura dei dati</span><span class="sxs-lookup"><span data-stu-id="b1ff6-178">4. Reading & Writing Data</span></span>

<span data-ttu-id="b1ff6-179">Ora che abbiamo un modello è possibile usarlo per accedere ai dati.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-179">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="b1ff6-180">Le classi andremo a utilizzare per accedere ai dati vengono generati automaticamente in base al file con estensione EDMX.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-180">The classes we are going to use to access data are being automatically generated for you based on the EDMX file.</span></span>

<span data-ttu-id="b1ff6-181">*Questa schermata è da Visual Studio 2012, se si usa Visual Studio 2010 il BloggingModel.tt e file BloggingModel.Context.tt saranno direttamente sotto il progetto anziché da annidato in file con estensione EDMX.*</span><span class="sxs-lookup"><span data-stu-id="b1ff6-181">*This screen shot is from Visual Studio 2012, if you are using Visual Studio 2010 the BloggingModel.tt and BloggingModel.Context.tt files will be directly under your project rather than nested under the EDMX file.*</span></span>

![Classi generate](~/ef6/media/generatedclasses.png)

<span data-ttu-id="b1ff6-183">Implementare il metodo Main in Program.cs, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-183">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="b1ff6-184">Questo codice crea una nuova istanza del contesto e quindi lo usa per inserire un nuovo post di Blog.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-184">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="b1ff6-185">Usa quindi una query LINQ per recuperare tutti i blog dal database ordinato alfabeticamente in base al titolo.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-185">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="b1ff6-186">È ora possibile eseguire l'applicazione e testarlo.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-186">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="b1ff6-187">5. Gestione di modifiche al modello</span><span class="sxs-lookup"><span data-stu-id="b1ff6-187">5. Dealing with Model Changes</span></span>

<span data-ttu-id="b1ff6-188">A questo punto è possibile apportare alcune modifiche per il modello, quando si apportano queste modifiche è necessario anche aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-188">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span>

<span data-ttu-id="b1ff6-189">Si inizierà aggiungendo una nuova entità utente per il modello.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-189">We’ll start by adding a new User entity to our model.</span></span>

-   <span data-ttu-id="b1ff6-190">Aggiungere un nuovo **utente** nome dell'entità con **Username** come nome della chiave e **stringa** come tipo di proprietà per la chiave</span><span class="sxs-lookup"><span data-stu-id="b1ff6-190">Add a new **User** entity name with **Username** as the key name and **String** as the property type for the key</span></span>

    ![Aggiungere l'entità User](~/ef6/media/adduserentity.png)

-   <span data-ttu-id="b1ff6-192">Fare clic sul **Username** proprietà nell'area di progettazione e seleziona **proprietà**, modificano proprietà il **MaxLength** se si imposta su \*\*50 \*\* 
     *Limita i dati che possono essere archiviati in nome utente da 50 caratteri*</span><span class="sxs-lookup"><span data-stu-id="b1ff6-192">Right-click on the **Username** property on the design surface and select **Properties**, In the Properties window change the **MaxLength** setting to **50**
*This restricts the data that can be stored in username to 50 characters*</span></span>
-   <span data-ttu-id="b1ff6-193">Aggiungere un **DisplayName** proprietà scalare per il **utente** entità</span><span class="sxs-lookup"><span data-stu-id="b1ff6-193">Add a **DisplayName** scalar property to the **User** entity</span></span>

<span data-ttu-id="b1ff6-194">È ora disponibile un modello aggiornato e siamo pronti aggiornare il database per contenere il nuovo tipo di entità utente.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-194">We now have an updated model and we are ready to update the database to accommodate our new User entity type.</span></span>

-   <span data-ttu-id="b1ff6-195">Fare clic su area di progettazione e seleziona **genera Database da modello...** , Entity Framework calcolerà uno script per ricreare uno schema basato sul modello aggiornato.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-195">Right-click on the design surface and select **Generate Database from Model…**, Entity Framework will calculate a script to recreate a schema based on the updated model.</span></span>
-   <span data-ttu-id="b1ff6-196">Fare clic su **fine**</span><span class="sxs-lookup"><span data-stu-id="b1ff6-196">Click **Finish**</span></span>
-   <span data-ttu-id="b1ff6-197">Potrebbe ricevere avvisi relativi a sovrascrivere lo script DDL esistente e le parti di mapping e di archiviazione del modello, fare clic su **Sì** per entrambi questi avvisi</span><span class="sxs-lookup"><span data-stu-id="b1ff6-197">You may receive warnings about overwriting the existing DDL script and the mapping and storage parts of the model, click **Yes** for both these warnings</span></span>
-   <span data-ttu-id="b1ff6-198">Lo script aggiornato SQL per creare il database viene aperto automaticamente</span><span class="sxs-lookup"><span data-stu-id="b1ff6-198">The updated SQL script to create the database is opened for you</span></span>  
    <span data-ttu-id="b1ff6-199">*Lo script generato verrà eliminare tutte le tabelle esistenti e quindi ricreare lo schema da zero. Questo potrebbe funzionare per lo sviluppo locale ma non è un valido per il push delle modifiche a un database che è già stato distribuito. Se si desidera pubblicare le modifiche in un database in cui è già stato distribuito, è necessario modificare lo script o usare uno strumento di confronto schema per la quale calcolare uno script di migrazione.*</span><span class="sxs-lookup"><span data-stu-id="b1ff6-199">*The script that is generated will drop all existing tables and then recreate the schema from scratch. This may work for local development but is not a viable for pushing changes to a database that has already been deployed. If you need to publish changes to a database that has already been deployed, you will need to edit the script or use a schema compare tool to calculate a migration script.*</span></span>
-   <span data-ttu-id="b1ff6-200">Fare clic su script e selezionare **Execute**, verrà richiesto di specificare il database a cui connettersi, specificare LocalDB o Express di SQL Server, a seconda di quale versione di Visual Studio in uso</span><span class="sxs-lookup"><span data-stu-id="b1ff6-200">Right-click on the script and select **Execute**, you will be prompted to specify the database to connect to, specify LocalDB or SQL Server Express, depending on which version of Visual Studio you are using</span></span>

## <a name="summary"></a><span data-ttu-id="b1ff6-201">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="b1ff6-201">Summary</span></span>

<span data-ttu-id="b1ff6-202">In questa procedura dettagliata che è stata esaminata sviluppo Model First, che ha permesso di creare un modello nella finestra di progettazione di Entity Framework e quindi generare un database da tale modello.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-202">In this walkthrough we looked at Model First development, which allowed us to create a model in the EF Designer and then generate a database from that model.</span></span> <span data-ttu-id="b1ff6-203">Il modello viene quindi usato per leggere e scrivere alcuni dati dal database.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-203">We then used the model to read and write some data from the database.</span></span> <span data-ttu-id="b1ff6-204">Infine, abbiamo aggiornato il modello e quindi ricreata lo schema del database in modo che corrisponda il modello.</span><span class="sxs-lookup"><span data-stu-id="b1ff6-204">Finally, we updated the model and then recreated the database schema to match the model.</span></span>
