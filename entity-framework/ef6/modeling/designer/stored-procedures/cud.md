---
title: Stored procedure CUD della finestra di progettazione-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: bdb0df969c33d5ad3f103bfa9af6002c9c2bb9b3
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813553"
---
# <a name="designer-cud-stored-procedures"></a>Stored procedure CUD della finestra di progettazione

Questa procedura dettagliata illustra come eseguire il mapping delle operazioni di creazione @ no__t-0insert, aggiornamento ed eliminazione di un tipo di entità alle stored procedure usando il Entity Framework Designer (EF designer).  Per impostazione predefinita, il Entity Framework genera automaticamente le istruzioni SQL per le operazioni CUD, ma è anche possibile eseguire il mapping delle stored procedure a queste operazioni.  

Si noti che Code First non supporta il mapping a stored procedure o funzioni. Tuttavia, è possibile chiamare stored procedure o funzioni utilizzando il metodo System. Data. Entity. DbSet. sqlQuery. Esempio:

``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a>Considerazioni relative al mapping delle operazioni CUD alle stored procedure

Quando si esegue il mapping delle operazioni CUD alle stored procedure, si applicano le considerazioni seguenti:

- Se si esegue il mapping di una delle operazioni CUD a una stored procedure, eseguirne il mapping. Se non si esegue il mapping di tutti e tre, le operazioni non mappate avranno esito negativo se eseguite e verrà generata un'eccezione **updateexception** will.
- È necessario eseguire il mapping di ogni parametro del stored procedure alle proprietà dell'entità.
- Se il server genera il valore della chiave primaria per la riga inserita, è necessario eseguire il mapping di questo valore alla proprietà della chiave dell'entità. Nell'esempio seguente, la procedura **InsertPerson** stored restituisce la chiave primaria appena creata come parte del set di risultati della stored procedure. Viene eseguito il mapping della chiave primaria alla chiave di entità (**PersonID**) utilizzando le **associazioni di risultati &lt;Add @ no__t-3** feature di EF designer.
- Viene eseguito il mapping delle chiamate stored procedure 1:1 con le entità del modello concettuale. Se, ad esempio, si implementa una gerarchia di ereditarietà nel modello concettuale ed è quindi necessario eseguire il mapping delle stored procedure CUD per le entità **padre** (base) e **figlio** (derivata), il salvataggio delle modifiche **figlio** chiamerà solo l' **elemento figlio**' le stored procedure di, non attiveranno le chiamate alle stored procedure **padre**.

## <a name="prerequisites"></a>Prerequisiti

Per completare questa procedura dettagliata, è necessario disporre di:

- Una versione recente di Visual Studio.
- [Database di esempio School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurare il progetto

- Aprire Visual Studio 2012.
- Selezionare il **progetto file-&gt; New-&gt;**
- Nel riquadro sinistro fare clic su **Visual C @ no__t-1**, quindi selezionare il modello **console** .
- Immettere **CUDSProcsSample**@no__t-aggiungere1come il nome.
- Fare clic su **OK**.

## <a name="create-a-model"></a>Creazione di un modello

- Fare clic con il pulsante destro del mouse sul nome del progetto in Esplora soluzioni e scegliere **Aggiungi-&gt; nuovo elemento**.
- Selezionare **dati** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro modelli.
- Immettere **CUDSProcs. edmx** per il nome del file e quindi fare clic su **Aggiungi**.
- Nella finestra di dialogo Scegli contenuto Model selezionare **genera da database**, quindi fare clic su **Avanti**.
- Fare clic su **nuova connessione**. Nella finestra di dialogo Proprietà connessione immettere il nome del server (ad esempio, **(local DB\\) mssqllocaldb**), selezionare il metodo di autenticazione, digitare **School** come nome del database, quindi fare clic su **OK**.
    La finestra di dialogo scegliere la connessione dati viene aggiornata con l'impostazione di connessione al database.
- Nella finestra di dialogo Scegli oggetti di database, sotto le **tabelle** node, selezionare la tabella **Person** .
- Inoltre, nel nodo **stored procedure e funzioni** selezionare le stored procedure seguenti: **DeletePerson**, **InsertPerson**e **UpdatePerson**.
- A partire da Visual Studio 2012, EF Designer supporta l'importazione bulk di stored procedure. Per impostazione predefinita, l' **importazione delle stored procedure e delle funzioni selezionate nel modello di entità** è selezionata. Poiché in questo esempio sono presenti stored procedure che inseriscono, aggiornano ed eliminano i tipi di entità, non è necessario importarli e deselezionare questa casella di controllo.

    ![Procedure Import S](~/ef6/media/importsprocs.jpg)

- Fare clic su **fine**.
    Viene visualizzata la finestra di progettazione EF, che fornisce un'area di progettazione per la modifica del modello.

## <a name="map-the-person-entity-to-stored-procedures"></a>Eseguire il mapping dell'entità Person alle stored procedure

- Fare clic con il pulsante destro del mouse sul tipo **Person** entity e selezionare **Mapping stored procedure**.
- I mapping di stored procedure vengono visualizzati nei **Dettagli di mapping** window.
- Fare clic su **&lt;Selezionare Inserisci funzione @ no__t-2**.
    Questo campo diventa l'elenco a discesa delle stored procedure contenute nel modello di archiviazione che possono essere mappate ai tipi di entità del modello concettuale.
    Selezionare **InsertPerson** from nell'elenco a discesa.
- Vengono visualizzati i mapping predefiniti tra i parametri delle stored procedure e le proprietà dell'entità. Si noti che le frecce indicano la direzione del mapping: I valori delle proprietà vengono specificati per stored procedure parametri.
- Fare clic su **&lt;Add result binding @ no__t-2**.
- Digitare **NewPersonID**, il nome del parametro restituito dalla procedura **InsertPerson** stored. Assicurarsi di non digitare spazi iniziali o finali.
- Premere **invio**.
- Per impostazione predefinita, **NewPersonID** is mappato alla chiave di entità **PersonID**. Si noti che una freccia indica la direzione del mapping: Il valore della colonna risultato viene fornito alla proprietà.

    ![Dettagli mapping](~/ef6/media/mappingdetails.png)

- Fare clic su **&lt;Selezionare Update Function @ no__t-2** and selezionare **UpdatePerson** from l'elenco a discesa risultante.
- Vengono visualizzati i mapping predefiniti tra i parametri delle stored procedure e le proprietà dell'entità.
- Fare clic su **&lt;Selezionare Delete Function @ no__t-2** and selezionare **DeletePerson** from l'elenco a discesa risultante.
- Vengono visualizzati i mapping predefiniti tra i parametri delle stored procedure e le proprietà dell'entità.

Le operazioni di inserimento, aggiornamento ed eliminazione del tipo **Person** entity vengono ora mappate alle stored procedure.

Per abilitare il controllo della concorrenza durante l'aggiornamento o l'eliminazione di un'entità con stored procedure, utilizzare una delle opzioni seguenti:

- Usare un parametro di **output** per restituire il numero di righe interessate dal stored procedure e controllare il **parametro delle righe interessate** checkbox accanto al nome del parametro. Se il valore restituito è zero quando viene chiamata l'operazione, viene generata un'eccezione  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will.
- Selezionare la casella di controllo **Usa valore originale** accanto a una proprietà che si desidera utilizzare per il controllo della concorrenza. Quando si tenta di eseguire un aggiornamento, il valore della proprietà originariamente letta dal database verrà utilizzato per la scrittura di dati nel database. Se il valore non corrisponde al valore nel database, viene generata un'eccezione **OptimisticConcurrencyException** will.

## <a name="use-the-model"></a>Usare il modello

Aprire il file **Program.cs** in cui è definito il metodo **Main** . Aggiungere il codice seguente nella funzione Main.

Il codice crea un nuovo oggetto **Person** , quindi aggiorna l'oggetto e infine elimina l'oggetto.

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

- Compilare l'applicazione ed eseguirla. Il programma produce l'output seguente *

> [!NOTE]
> PersonID viene generato automaticamente dal server, pertanto è probabile che venga visualizzato un numero diverso *

``` Output
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

Se si utilizza la versione finale di Visual Studio, è possibile utilizzare IntelliTrace con il debugger per visualizzare le istruzioni SQL che vengono eseguite.

![IntelliTrace](~/ef6/media/intellitrace.png)
