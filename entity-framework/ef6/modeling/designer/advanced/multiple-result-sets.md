---
title: Le stored procedure con più set di risultati - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
caps.latest.revision: 3
ms.openlocfilehash: 68d544b0c553868ad1ff36cd24db19cff08db073
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121202"
---
# <a name="stored-procedures-with-multiple-result-sets"></a><span data-ttu-id="e8a5f-102">Stored procedure con più set di risultati</span><span class="sxs-lookup"><span data-stu-id="e8a5f-102">Stored Procedures with Multiple Result Sets</span></span>
<span data-ttu-id="e8a5f-103">In alcuni casi quando si utilizzano le stored procedure è necessario restituire più risultati impostano.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-103">Sometimes when using stored procedures you will need to return more than one result set.</span></span> <span data-ttu-id="e8a5f-104">Questo scenario viene comunemente usato per ridurre il numero di database di andata e ritorno necessari per creare una singola schermata.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-104">This scenario is commonly used to reduce the number of database round trips required to compose a single screen.</span></span> <span data-ttu-id="e8a5f-105">Prima EF5, Entity Framework consentirebbe la stored procedure da chiamare, ma solo restituisce il primo set di risultati al codice chiamante.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-105">Prior to EF5, Entity Framework would allow the stored procedure to be called but would only return the first result set to the calling code.</span></span>

<span data-ttu-id="e8a5f-106">Questo articolo illustra due modi in cui è possibile usare per accedere a più di un set di risultati da una stored procedure in Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-106">This article will show you two ways that you can use to access more than one result set from a stored procedure in Entity Framework.</span></span> <span data-ttu-id="e8a5f-107">Uno che usa solo il codice e funziona con entrambi codice prima di tutto ed Entity Framework Designer e uno che funziona solo con la finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-107">One that uses just code and works with both Code first and the EF Designer and one that only works with the EF Designer.</span></span> <span data-ttu-id="e8a5f-108">Di strumenti e supporto API per questo dovrebbero migliorare in futuro le versioni di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-108">The tooling and API support for this should improve in future versions of Entity Framework.</span></span>

## <a name="model"></a><span data-ttu-id="e8a5f-109">Modello</span><span class="sxs-lookup"><span data-stu-id="e8a5f-109">Model</span></span>

<span data-ttu-id="e8a5f-110">Gli esempi in questo articolo usano un Blog di base e modello post in cui un blog dispone di molti inserimenti e post appartiene a un singolo blog.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-110">The examples in this article use a basic Blog and Posts model where a blog has many posts and a post belongs to a single blog.</span></span> <span data-ttu-id="e8a5f-111">Si userà una stored procedure nel database che restituisce tutti i blog e post, simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="e8a5f-111">We will use a stored procedure in the database that returns all blogs and posts, something like this:</span></span>

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a><span data-ttu-id="e8a5f-112">L'accesso Multiple Result set con codice</span><span class="sxs-lookup"><span data-stu-id="e8a5f-112">Accessing Multiple Result Sets with Code</span></span>

<span data-ttu-id="e8a5f-113">È possibile eseguire il codice per inviare un comando SQL non elaborato per eseguire la stored procedure.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-113">We can execute use code to issue a raw SQL command to execute our stored procedure.</span></span> <span data-ttu-id="e8a5f-114">Il vantaggio di questo approccio è che funziona con il codice sia prima e la finestra di progettazione di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-114">The advantage of this approach is that it works with both Code first and the EF Designer.</span></span>

<span data-ttu-id="e8a5f-115">Per ottenere risultati più imposta funzionante che è necessario eliminare per l'API ObjectContext utilizzando l'interfaccia IObjectContextAdapter.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-115">In order to get multiple result sets working we need to drop to the ObjectContext API by using the IObjectContextAdapter interface.</span></span>

<span data-ttu-id="e8a5f-116">Dopo aver ottenuto un oggetto ObjectContext è possibile usare il metodo Translate per convertire i risultati della stored procedure in entità che possono essere rilevate e utilizzate in Entity Framework come di consueto.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-116">Once we have an ObjectContext then we can use the Translate method to translate the results of our stored procedure into entities that can be tracked and used in EF as normal.</span></span> <span data-ttu-id="e8a5f-117">Nell'esempio seguente viene illustrata questa procedura in azione.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-117">The following code sample demonstrates this in action.</span></span>

``` csharp
    using (var db = new BloggingContext())
    {
        // If using Code First we need to make sure the model is built before we open the connection
        // This isn't required for models created with the EF Designer
        db.Database.Initialize(force: false);

        // Create a SQL command to execute the sproc
        var cmd = db.Database.Connection.CreateCommand();
        cmd.CommandText = "[dbo].[GetAllBlogsAndPosts]";

        try
        {

            db.Database.Connection.Open();
            // Run the sproc
            var reader = cmd.ExecuteReader();

            // Read Blogs from the first result set
            var blogs = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Blog>(reader, "Blogs", MergeOption.AppendOnly);   


            foreach (var item in blogs)
            {
                Console.WriteLine(item.Name);
            }        

            // Move to second result set and read Posts
            reader.NextResult();
            var posts = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Post>(reader, "Posts", MergeOption.AppendOnly);


            foreach (var item in posts)
            {
                Console.WriteLine(item.Title);
            }
        }
        finally
        {
            db.Database.Connection.Close();
        }
    }
```

<span data-ttu-id="e8a5f-118">Il metodo Translate accetta il lettore che abbiamo ricevuto quando viene eseguita la procedura, un nome di EntitySet e un MergeOption.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-118">The Translate method accepts the reader that we received when we executed the procedure, an EntitySet name, and a MergeOption.</span></span> <span data-ttu-id="e8a5f-119">Il nome di EntitySet sarà quello utilizzato per la proprietà DbSet nel contesto derivato.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-119">The EntitySet name will be the same as the DbSet property on your derived context.</span></span> <span data-ttu-id="e8a5f-120">L'enumerazione MergeOption controlla come vengono gestiti i risultati se la stessa entità esiste già in memoria.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-120">The MergeOption enum controls how results are handled if the same entity already exists in memory.</span></span>

<span data-ttu-id="e8a5f-121">Qui viene iterato più la raccolta dei blog prima chiamiamo NextResult, questo è importante considerata il codice sopra riportato perché il primo set di risultati deve essere utilizzato prima di passare al set di risultati successivo.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-121">Here we iterate through the collection of blogs before we call NextResult, this is important given the above code because the first result set must be consumed before moving to the next result set.</span></span>

<span data-ttu-id="e8a5f-122">Una volta convertire i due metodi vengono chiamati da Entity Framework vengono rilevate le entità Blog e Post esattamente come qualsiasi altra entità, quindi in modo che possono essere modificati o eliminati e salvati come di consueto.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-122">Once the two translate methods are called then the Blog and Post entities are tracked by EF the same way as any other entity and so can be modified or deleted and saved as normal.</span></span>

>[!NOTE]
> <span data-ttu-id="e8a5f-123">EF non accetta alcun mapping in considerazione durante la creazione di entità tramite il metodo di traslazione.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-123">EF does not take any mapping into account when it creates entities using the Translate method.</span></span> <span data-ttu-id="e8a5f-124">Corrisponderà semplicemente i nomi delle colonne nel set con i nomi delle proprietà nelle classi di risultati.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-124">It will simply match column names in the result set with property names on your classes.</span></span>

>[!NOTE]
> <span data-ttu-id="e8a5f-125">Che se si dispone di caricamento lazy abilitata, l'accesso alla proprietà post su una delle entità blog quindi EF stabilirà la connessione al database per caricare in ritardo tutti i post, anche se è stato già caricato tutti.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-125">That if you have lazy loading enabled, accessing the posts property on one of the blog entities then EF will connect to the database to lazily load all posts, even though we have already loaded them all.</span></span> <span data-ttu-id="e8a5f-126">Questo avviene perché Entity Framework non è possibile sapere se sono stati caricati tutti i post o se sono presenti più nel database.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-126">This is because EF cannot know whether or not you have loaded all posts or if there are more in the database.</span></span> <span data-ttu-id="e8a5f-127">Se si desidera evitare questo problema è necessario disattivare il caricamento lazy.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-127">If you want to avoid this then you will need to disable lazy loading.</span></span>

## <a name="multiple-result-sets-with-configured-in-edmx"></a><span data-ttu-id="e8a5f-128">Più set di risultati con configurata in EDMX</span><span class="sxs-lookup"><span data-stu-id="e8a5f-128">Multiple Result Sets with Configured in EDMX</span></span>

>[!NOTE]
> <span data-ttu-id="e8a5f-129">Deve avere come destinazione .NET Framework 4.5 per essere in grado di configurare più set di risultati in EDMX.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-129">You must target .NET Framework 4.5 to be able to configure multiple result sets in EDMX.</span></span> <span data-ttu-id="e8a5f-130">Se la destinazione è .NET 4.0 è possibile usare il metodo basato sul codice mostrato nella sezione precedente.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-130">If you are targeting .NET 4.0 you can use the code-based method shown in the previous section.</span></span>

<span data-ttu-id="e8a5f-131">Se si usa la finestra di progettazione di Entity Framework, è possibile modificare anche il modello in modo che riconosca diversi set di risultati che verranno restituite.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-131">If you are using the EF Designer, you can also modify your model so that it knows about the different result sets that will be returned.</span></span> <span data-ttu-id="e8a5f-132">Una cosa da sapere prima di disponibilità è che gli strumenti non sono il risultato di più set dipendente, pertanto sarà necessario modificare manualmente i file con estensione edmx.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-132">One thing to know before hand is that the tooling is not multiple result set aware, so you will need to manually edit the edmx file.</span></span> <span data-ttu-id="e8a5f-133">Modifica di file con estensione edmx, come questa tecnica funziona ma verrà interrotto anche la convalida del modello in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-133">Editing the edmx file like this will work but it will also break the validation of the model in VS.</span></span> <span data-ttu-id="e8a5f-134">Pertanto, se si convalida il modello si otterranno sempre errori.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-134">So if you validate your model you will always get errors.</span></span>

-   <span data-ttu-id="e8a5f-135">A questo scopo è necessario aggiungere la stored procedure al modello come si farebbe per una query di set di risultati singolo.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-135">In order to do this you need to add the stored procedure to your model as you would for a single result set query.</span></span>
-   <span data-ttu-id="e8a5f-136">Una volta ottenuto ciò, è necessario fare clic con il pulsante destro sul modello e selezionare **Apri con...**</span><span class="sxs-lookup"><span data-stu-id="e8a5f-136">Once you have this then you need to right click on your model and select **Open With..**</span></span> <span data-ttu-id="e8a5f-137">quindi **Xml**</span><span class="sxs-lookup"><span data-stu-id="e8a5f-137">then **Xml**</span></span>

    ![OpenAs](~/ef6/media/openas.png)

<span data-ttu-id="e8a5f-139">Dopo aver ottenuto il modello aperto in formato XML, è necessario procedere come segue:</span><span class="sxs-lookup"><span data-stu-id="e8a5f-139">Once you have the model opened as XML then you need to do the following steps:</span></span>

-   <span data-ttu-id="e8a5f-140">Trovare l'importazione di tipo e funzione complesso nel modello:</span><span class="sxs-lookup"><span data-stu-id="e8a5f-140">Find the complex type and function import in your model:</span></span>

``` xml
    <!-- CSDL content -->
    <edmx:ConceptualModels>

    ...

      <FunctionImport Name="GetAllBlogsAndPosts" ReturnType="Collection(BlogModel.GetAllBlogsAndPosts_Result)" />

    ...

      <ComplexType Name="GetAllBlogsAndPosts_Result">
        <Property Type="Int32" Name="BlogId" Nullable="false" />
        <Property Type="String" Name="Name" Nullable="false" MaxLength="255" />
        <Property Type="String" Name="Description" Nullable="true" />
      </ComplexType>

    ...

    </edmx:ConceptualModels>
```

 

-   <span data-ttu-id="e8a5f-141">Rimuovere il tipo complesso</span><span class="sxs-lookup"><span data-stu-id="e8a5f-141">Remove the complex type</span></span>
-   <span data-ttu-id="e8a5f-142">Aggiornare l'importazione di funzioni in modo che viene eseguito il mapping alle entità, in questo caso che avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="e8a5f-142">Update the function import so that it maps to your entities, in our case it will look like the following:</span></span>

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

<span data-ttu-id="e8a5f-143">In questo modo il modello che la stored procedure restituirà due raccolte, una delle voci dei blog e una delle voci di post.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-143">This tells the model that the stored procedure will return two collections, one of blog entries and one of post entries.</span></span>

-   <span data-ttu-id="e8a5f-144">Trovare l'elemento di mapping di funzione:</span><span class="sxs-lookup"><span data-stu-id="e8a5f-144">Find the function mapping element:</span></span>

``` xml
    <!-- C-S mapping content -->
    <edmx:Mappings>

    ...

      <FunctionImportMapping FunctionImportName="GetAllBlogsAndPosts" FunctionName="BlogModel.Store.GetAllBlogsAndPosts">
        <ResultMapping>
          <ComplexTypeMapping TypeName="BlogModel.GetAllBlogsAndPosts_Result">
            <ScalarProperty Name="BlogId" ColumnName="BlogId" />
            <ScalarProperty Name="Name" ColumnName="Name" />
            <ScalarProperty Name="Description" ColumnName="Description" />
          </ComplexTypeMapping>
        </ResultMapping>
      </FunctionImportMapping>

    ...

    </edmx:Mappings>
```

-   <span data-ttu-id="e8a5f-145">Sostituire il mapping del risultato con uno per ogni entità restituita, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e8a5f-145">Replace the result mapping with one for each entity being returned, such as the following:</span></span>

``` xml
    <ResultMapping>
      <EntityTypeMapping TypeName ="BlogModel.Blog">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="Name" ColumnName="Name" />
        <ScalarProperty Name="Description" ColumnName="Description" />
      </EntityTypeMapping>
    </ResultMapping>
    <ResultMapping>
      <EntityTypeMapping TypeName="BlogModel.Post">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="PostId" ColumnName="PostId"/>
        <ScalarProperty Name="Title" ColumnName="Title" />
        <ScalarProperty Name="Text" ColumnName="Text" />
      </EntityTypeMapping>
    </ResultMapping>
```

<span data-ttu-id="e8a5f-146">È anche possibile eseguire il mapping dei set di risultati per i tipi complessi, ad esempio quello creato per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-146">It is also possible to map the result sets to complex types, such as the one created by default.</span></span> <span data-ttu-id="e8a5f-147">A tale scopo si crea un nuovo tipo complesso, invece di rimuoverli e usano i tipi complessi ovunque si utilizzava i nomi di entità negli esempi precedenti.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-147">To do this you create a new complex type, instead of removing them, and use the complex types everywhere that you had used the entity names in the examples above.</span></span>

<span data-ttu-id="e8a5f-148">Una volta che questi mapping sono stati modificati, quindi è possibile salvare il modello ed eseguire il codice seguente per usare la stored procedure:</span><span class="sxs-lookup"><span data-stu-id="e8a5f-148">Once these mappings have been changed then you can save the model and execute the following code to use the stored procedure:</span></span>

``` csharp
    using (var db = new BlogEntities())
    {
        var results = db.GetAllBlogsAndPosts();

        foreach (var result in results)
        {
            Console.WriteLine("Blog: " + result.Name);
        }

        var posts = results.GetNextResult<Post>();

        foreach (var result in posts)
        {
            Console.WriteLine("Post: " + result.Title);
        }

        Console.ReadLine();
    }
```

>[!NOTE]
> <span data-ttu-id="e8a5f-149">Se si modifica manualmente il file edmx per il modello verrà sovrascritto se si rigenera sempre il modello dal database.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-149">If you manually edit the edmx file for your model it will be overwritten if you ever regenerate the model from the database.</span></span>

## <a name="summary"></a><span data-ttu-id="e8a5f-150">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="e8a5f-150">Summary</span></span>

<span data-ttu-id="e8a5f-151">Di seguito sono stati mostrati due metodi diversi per l'accesso ai risultati più imposta mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-151">Here we have shown two different methods of accessing multiple result sets using Entity Framework.</span></span> <span data-ttu-id="e8a5f-152">Entrambi sono ugualmente validi in base alla situazione specifica e le preferenze e si deve scegliere quello che sembra adatta alle circostanze.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-152">Both of them are equally valid depending on your situation and preferences and you should choose the one that seems best for your circumstances.</span></span> <span data-ttu-id="e8a5f-153">Ne è prevista che il supporto per il risultato di più gruppi saranno migliorate nelle future versioni di Entity Framework e che esegue i passaggi descritti in questo documento non potrà più necessarie.</span><span class="sxs-lookup"><span data-stu-id="e8a5f-153">It is planned that the support for multiple result sets will be improved in future versions of Entity Framework and that performing the steps in this document will no longer be necessary.</span></span>
