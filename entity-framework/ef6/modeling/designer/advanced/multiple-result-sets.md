---
title: Le stored procedure con più set di risultati - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
ms.openlocfilehash: 098ed88ba52e211965baf3660f0e51bd74c71efd
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489310"
---
# <a name="stored-procedures-with-multiple-result-sets"></a>Stored procedure con più set di risultati
In alcuni casi quando si utilizzano le stored procedure è necessario restituire più risultati impostano. Questo scenario viene comunemente usato per ridurre il numero di database di andata e ritorno necessari per creare una singola schermata. Prima EF5, Entity Framework consentirebbe la stored procedure da chiamare, ma solo restituisce il primo set di risultati al codice chiamante.

Questo articolo illustra due modi in cui è possibile usare per accedere a più di un set di risultati da una stored procedure in Entity Framework. Uno che usa solo il codice e funziona con entrambi codice prima di tutto ed Entity Framework Designer e uno che funziona solo con la finestra di progettazione di Entity Framework. Di strumenti e supporto API per questo dovrebbero migliorare in futuro le versioni di Entity Framework.

## <a name="model"></a>Modello

Gli esempi in questo articolo usano un Blog di base e modello post in cui un blog dispone di molti inserimenti e post appartiene a un singolo blog. Si userà una stored procedure nel database che restituisce tutti i blog e post, simile al seguente:

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a>L'accesso Multiple Result set con codice

È possibile eseguire il codice per inviare un comando SQL non elaborato per eseguire la stored procedure. Il vantaggio di questo approccio è che funziona con il codice sia prima e la finestra di progettazione di Entity Framework.

Per ottenere risultati più imposta funzionante che è necessario eliminare per l'API ObjectContext utilizzando l'interfaccia IObjectContextAdapter.

Dopo aver ottenuto un oggetto ObjectContext è possibile usare il metodo Translate per convertire i risultati della stored procedure in entità che possono essere rilevate e utilizzate in Entity Framework come di consueto. Nell'esempio seguente viene illustrata questa procedura in azione.

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

Il metodo Translate accetta il lettore che abbiamo ricevuto quando viene eseguita la procedura, un nome di EntitySet e un MergeOption. Il nome di EntitySet sarà quello utilizzato per la proprietà DbSet nel contesto derivato. L'enumerazione MergeOption controlla come vengono gestiti i risultati se la stessa entità esiste già in memoria.

Qui viene iterato più la raccolta dei blog prima chiamiamo NextResult, questo è importante considerata il codice sopra riportato perché il primo set di risultati deve essere utilizzato prima di passare al set di risultati successivo.

Una volta convertire i due metodi vengono chiamati da Entity Framework vengono rilevate le entità Blog e Post esattamente come qualsiasi altra entità, quindi in modo che possono essere modificati o eliminati e salvati come di consueto.

>[!NOTE]
> EF non accetta alcun mapping in considerazione durante la creazione di entità tramite il metodo di traslazione. Corrisponderà semplicemente i nomi delle colonne nel set con i nomi delle proprietà nelle classi di risultati.

>[!NOTE]
> Che se si dispone di caricamento lazy abilitata, l'accesso alla proprietà post su una delle entità blog quindi EF stabilirà la connessione al database per caricare in ritardo tutti i post, anche se è stato già caricato tutti. Questo avviene perché Entity Framework non è possibile sapere se sono stati caricati tutti i post o se sono presenti più nel database. Se si desidera evitare questo problema è necessario disattivare il caricamento lazy.

## <a name="multiple-result-sets-with-configured-in-edmx"></a>Più set di risultati con configurata in EDMX

>[!NOTE]
> Deve avere come destinazione .NET Framework 4.5 per essere in grado di configurare più set di risultati in EDMX. Se la destinazione è .NET 4.0 è possibile usare il metodo basato sul codice mostrato nella sezione precedente.

Se si usa la finestra di progettazione di Entity Framework, è possibile modificare anche il modello in modo che riconosca diversi set di risultati che verranno restituite. Una cosa da sapere prima di disponibilità è che gli strumenti non sono il risultato di più set dipendente, pertanto sarà necessario modificare manualmente i file con estensione edmx. Modifica di file con estensione edmx, come questa tecnica funziona ma verrà interrotto anche la convalida del modello in Visual Studio. Pertanto, se si convalida il modello si otterranno sempre errori.

-   A questo scopo è necessario aggiungere la stored procedure al modello come si farebbe per una query di set di risultati singolo.
-   Una volta ottenuto ciò, è necessario fare clic con il pulsante destro sul modello e selezionare **Apri con...** quindi **Xml**

    ![Apri come](~/ef6/media/openas.png)

Dopo aver ottenuto il modello aperto in formato XML, è necessario procedere come segue:

-   Trovare l'importazione di tipo e funzione complesso nel modello:

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

 

-   Rimuovere il tipo complesso
-   Aggiornare l'importazione di funzioni in modo che viene eseguito il mapping alle entità, in questo caso che avrà un aspetto simile al seguente:

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

In questo modo il modello che la stored procedure restituirà due raccolte, una delle voci dei blog e una delle voci di post.

-   Trovare l'elemento di mapping di funzione:

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

-   Sostituire il mapping del risultato con uno per ogni entità restituita, ad esempio:

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

È anche possibile eseguire il mapping dei set di risultati per i tipi complessi, ad esempio quello creato per impostazione predefinita. A tale scopo si crea un nuovo tipo complesso, invece di rimuoverli e usano i tipi complessi ovunque si utilizzava i nomi di entità negli esempi precedenti.

Una volta che questi mapping sono stati modificati, quindi è possibile salvare il modello ed eseguire il codice seguente per usare la stored procedure:

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
> Se si modifica manualmente il file edmx per il modello verrà sovrascritto se si rigenera sempre il modello dal database.

## <a name="summary"></a>Riepilogo

Di seguito sono stati mostrati due metodi diversi per l'accesso ai risultati più imposta mediante Entity Framework. Entrambi sono ugualmente validi in base alla situazione specifica e le preferenze e si deve scegliere quello che sembra adatta alle circostanze. Ne è prevista che il supporto per il risultato di più gruppi saranno migliorate nelle future versioni di Entity Framework e che esegue i passaggi descritti in questo documento non potrà più necessarie.
