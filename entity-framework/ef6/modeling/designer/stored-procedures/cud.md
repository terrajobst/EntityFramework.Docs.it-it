---
title: Finestra di progettazione CUD Stored procedure - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: 7a3176e1057816dd11ced5fc545aa3baa672bd03
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993889"
---
# <a name="designer-cud-stored-procedures"></a>Finestra di progettazione CUD Stored procedure
Questa procedura dettagliata mostra come eseguire il mapping di creazione\\inserire, aggiornare ed eliminare le operazioni (CUD) di un tipo di entità alle stored procedure mediante Entity Framework Designer (Entity Framework Designer).  Per impostazione predefinita, Entity Framework genera automaticamente le istruzioni SQL per le operazioni CUD, ma è anche possibile mappare le stored procedure a queste operazioni.  

Si noti che Code First non supporta il mapping a stored procedure o funzioni. Tuttavia, è possibile chiamare stored procedure o funzioni tramite il metodo System.Data.Entity.DbSet.SqlQuery. Ad esempio:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a>Considerazioni quando le operazioni CUD per il Mapping di Stored procedure

Quando le operazioni CUD il mapping a stored procedure, si applicano le considerazioni seguenti: 

-   Se si esegue una delle operazioni CUD il mapping a una stored procedure, eseguire il mapping di tutti gli elementi. Se non si esegue il mapping tutte e tre, le operazioni non mappate avranno esito negativo se eseguita e un' **UpdateException** verrà generata.
-   È necessario eseguire il mapping di ogni parametro della stored procedure alle proprietà dell'entità.
-   Se il server genera il valore di chiave primaria per la riga inserita, è necessario eseguire il mapping di questo valore alla proprietà chiave dell'entità. Nell'esempio seguente, il **InsertPerson** stored procedure restituisce la chiave primaria appena creata come parte del set di risultati della stored procedure. La chiave primaria è mappata alla chiave di entità (**PersonID**) tramite il **&lt;Aggiungi Result binding&gt;** funzionalità della finestra di progettazione di Entity Framework.
-   Le chiamate a stored procedure sono mappate 1:1 con le entità nel modello concettuale. Ad esempio, se si implementa una gerarchia di ereditarietà nel modello concettuale ed eseguire il mapping di CUD stored procedure per la **padre** (base) e il **figlio** entità (derivata), salvare il **Figlio** modifiche solo chiamerà il **figlio**della stored procedure, non attiva il **padre**della stored procedure chiama.

## <a name="prerequisites"></a>Prerequisiti

Per completare questa procedura dettagliata, è necessario disporre di:

- Una versione recente di Visual Studio.
- Il [database di esempio School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurare il progetto

-   Aprire Visual Studio 2012.
-   Selezionare **File -&gt; Novità -&gt; progetto**
-   Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **Console** modello.
-   Immettere **CUDSProcsSample** come nome.
-   Scegliere **OK**.

## <a name="create-a-model"></a>Creare un modello

-   Il nome del progetto in Esplora soluzioni e scegliere **Add -&gt; nuovo elemento**.
-   Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro dei modelli.
-   Immettere **CUDSProcs.edmx** per il nome del file e quindi fare clic su **Add**.
-   Nella finestra di dialogo Scegli contenuto Model, selezionare **genera da database**, quindi fare clic su **successivo**.
-   Fare clic su **nuova connessione**. Nella finestra di dialogo proprietà di connessione, immettere il nome del server (ad esempio, **(localdb)\\mssqllocaldb**), selezionare il metodo di autenticazione, il tipo **School** per il nome del database e quindi Fare clic su **OK**.
    La finestra di dialogo Seleziona connessione dati viene aggiornata con l'impostazione di connessione di database.
-   Nella pagina di scegliere di oggetti di Database nella finestra di dialogo il **tabelle** nodo, seleziona la **persona** tabella.
-   Selezionare anche le seguenti stored procedure con il **funzioni e Stored procedure** nodo: **DeletePerson**, **InsertPerson**, e **UpdatePerson** . 
-   Partire da Visual Studio 2012 nella finestra di progettazione di Entity Framework supporta l'importazione bulk di stored procedure. Il **Importa selezionate stored procedure e funzioni nel modello di entità** è selezionata per impostazione predefinita. Poiché in questo esempio è stata stored procedure che inseriscono, aggiornano ed eliminano i tipi di entità, non si desidera importare tali e verrà deselezionare questa casella di controllo. 

    ![ImportSProcs](~/ef6/media/importsprocs.jpg)

-   Scegliere **Fine**.
    La finestra di progettazione di Entity Framework, che fornisce un'area di progettazione per la modifica del modello, viene visualizzato.

## <a name="map-the-person-entity-to-stored-procedures"></a>Eseguire il mapping dell'entità Person alle Stored procedure

-   Fare doppio clic il **Person** tipo di entità e selezionare **Mapping Stored Procedure**.
-   Il mapping delle stored procedure vengono visualizzati nei **Dettagli Mapping** finestra.
-   Fare clic su  **&lt;seleziona Inserisci funzione&gt;**.
    Questo campo diventa l'elenco a discesa delle stored procedure contenute nel modello di archiviazione che possono essere mappate ai tipi di entità del modello concettuale.
    Selezionare **InsertPerson** nell'elenco a discesa.
-   Vengono visualizzati i mapping predefiniti tra i parametri delle stored procedure e le proprietà dell'entità. Come indicato dalle frecce, che mostrano la direzione del mapping, per i parametri delle stored procedure vengono specificati i valori delle proprietà.
-   Fare clic su  **&lt;Aggiungi Result Binding&gt;**.
-   Tipo di **NewPersonID**, il nome del parametro restituito dalle **InsertPerson** stored procedure. Assicurarsi di non inserire spazi iniziali o finali.
-   Premere **INVIO**.
-   Per impostazione predefinita **NewPersonID** viene eseguito il mapping alla chiave di entità **PersonID**. Come indicato dalle frecce, che mostrano la direzione del mapping, per la proprietà viene specificato il valore della colonna dei risultati.

    ![MappingDetails](~/ef6/media/mappingdetails.png)

-   Fare clic su **&lt;seleziona Update Function&gt;** e selezionare **UpdatePerson** dall'elenco a discesa risultante.
-   Vengono visualizzati i mapping predefiniti tra i parametri delle stored procedure e le proprietà dell'entità.
-   Fare clic su **&lt;seleziona Delete Function&gt;** e selezionare **DeletePerson** dall'elenco a discesa risultante.
-   Vengono visualizzati i mapping predefiniti tra i parametri delle stored procedure e le proprietà dell'entità.

L'inserimento, aggiornare ed eliminare le operazioni dei **persona** viene ora eseguito il mapping di tipo di entità alle stored procedure.

Se si desidera abilitare controllo della concorrenza durante l'aggiornamento o eliminazione di un'entità con stored procedure, usare una delle opzioni seguenti:

-   Usa un' **OUTPUT** parametro per restituire il numero di righe da stored procedure e controllo interessate le **parametro delle righe interessate** casella di controllo accanto al nome del parametro. Se il valore restituito è zero quando viene chiamata l'operazione, un' [ **OptimisticConcurrencyException** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) verrà generata.
-   Verificare i **utilizza Original Value** casella di controllo accanto a una proprietà che si desidera utilizzare per controllo della concorrenza. Quando si tenta un aggiornamento, il valore della proprietà che è stato originariamente letta dal database da utilizzare durante la scrittura dati nel database. Se il valore non corrisponde al valore nel database, un' **OptimisticConcurrencyException** verrà generata.

## <a name="use-the-model"></a>Usare il modello

Aprire il **Program.cs** file dove il **Main** metodo è definito. Aggiungere il codice seguente nella funzione Main.

Il codice crea un nuovo **persona** oggetto, quindi l'oggetto viene aggiornato e infine elimina l'oggetto.         

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

-   Compilare l'applicazione ed eseguirla. Il programma produce l'output seguente *
    >[!NOTE]
> PersonID viene generato automaticamente dal server, pertanto si noteranno probabilmente un numero diverso *

```
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

Se si lavora con la versione di Visual Studio Ultimate, è possibile utilizzare Intellitrace con il debugger per visualizzare le istruzioni SQL che vengono eseguite.

![IntelliTrace](~/ef6/media/intellitrace.png)
