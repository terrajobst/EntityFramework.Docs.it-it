---
title: Suddivisione di tabelle della finestra di progettazione - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: f07aeb0aa679f6fa8131c667ac808f17c3f03f20
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250985"
---
# <a name="designer-table-splitting"></a>Suddivisione di tabelle della finestra di progettazione
Questa procedura dettagliata illustra come eseguire il mapping di più tipi di entità a una singola tabella modificando un modello con Entity Framework Designer (Entity Framework Designer).

Un motivo per cui che è possibile usare suddivisione di tabelle sta ritardando il caricamento di alcune proprietà quando si usa per caricare gli oggetti di caricamento lazy. È possibile separare le proprietà che potrebbero contenere grandi quantità di dati in un'entità distinta e caricare solo quando richiesto.

L'immagine seguente mostra le finestre principali che vengono usate quando si lavora con la finestra di progettazione di Entity Framework.

![EF Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Prerequisiti

Per completare questa procedura dettagliata, è necessario disporre di:

- Una versione recente di Visual Studio.
- Il [database di esempio School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurare il progetto

Questa procedura dettagliata Usa Visual Studio 2012.

-   Aprire Visual Studio 2012.
-   Scegliere **Nuovo** dal menu **File**, quindi fare clic su **Progetto**.
-   Nel riquadro sinistro, fare clic su Visual C\#e quindi selezionare il modello di applicazione Console.
-   Immettere **TableSplittingSample** come nome del progetto e fare clic su **OK**.

## <a name="create-a-model-based-on-the-school-database"></a>Creare un modello basato sul Database School

-   Fare clic sul nome del progetto in Esplora soluzioni, scegliere **Add**, quindi fare clic su **nuovo elemento**.
-   Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro dei modelli.
-   Immettere **TableSplittingModel.edmx** per il nome del file e quindi fare clic su **Add**.
-   Nella finestra di dialogo Scegli contenuto Model, selezionare **genera da database**, quindi fare clic su **successivo.**
-   Fare clic su nuova connessione. Nella finestra di dialogo proprietà di connessione, immettere il nome del server (ad esempio, **(localdb)\\mssqllocaldb**), selezionare il metodo di autenticazione, il tipo **School** per il nome del database e quindi Fare clic su **OK**.
    La finestra di dialogo Seleziona connessione dati viene aggiornata con l'impostazione di connessione di database.
-   Nella finestra di dialogo Scegli oggetti di Database, espandere la **tabelle** nodo e selezionare il **persona** tabella. Verrà aggiunta la tabella specificata per il **School** modello.
-   Scegliere **Fine**.

Entity Designer, che fornisce un'area di progettazione per la modifica del modello, viene visualizzato. Tutti gli oggetti selezionati nella **Scegli oggetti di Database** nella finestra di dialogo vengono aggiunti al modello.

## <a name="map-two-entities-to-a-single-table"></a>Eseguire il mapping di due entità a una singola tabella

In questa sezione suddivideranno i **persona** entità in due entità e quindi eseguirne il mapping a una singola tabella.

> [!NOTE]
> Il **persona** entità non contiene le proprietà che potrebbero contenere grandi quantità di dati; viene usata solo come esempio.

-   Fare doppio clic su un'area vuota dell'area di progettazione, scegliere **Aggiungi nuovo**, fare clic su **entità**.
    Il **nuova entità** verrà visualizzata la finestra di dialogo.
-   Tipo di **HireInfo** per il **nome entità** e **PersonID** per il **proprietà Key** nome.
-   Fare clic su **OK**.
-   Nell'area di progettazione verrà creato e visualizzato un nuovo tipo di entità.
-   Selezionare il **HireDate** proprietà delle **persona** tipo di entità e premere **Ctrl + X** chiavi.
-   Selezionare il **HireInfo** entità e premere **Ctrl + V** chiavi.
-   Creare un'associazione tra **Person** e **HireInfo**. A tale scopo, fare doppio clic su un'area vuota dell'area di progettazione, scegliere **Aggiungi nuovo**, fare clic su **Association**.
-   Il **Aggiungi associazione** verrà visualizzata la finestra di dialogo. Il **PersonHireInfo** viene specificato per impostazione predefinita.
-   Specificare la molteplicità **1(One)** a entrambe le estremità della relazione.
-   Premere **OK**.

Il passaggio successivo richiede la **Dettagli Mapping** finestra. Se non è possibile visualizzare questa finestra, fare doppio clic su area di progettazione e seleziona **Dettagli Mapping**.

-   Selezionare il **HireInfo** tipo di entità e fare clic su **&lt;aggiungere una tabella o vista&gt;** nel **Dettagli Mapping** finestra.
-   Selezionare **Person** dalle **&lt;aggiungere una tabella o vista&gt;** elenco campo. L'elenco contiene tabelle o visualizzazioni a cui può essere mappato l'entità selezionata.
    Le proprietà appropriate devono eseguire il mapping per impostazione predefinita.

    ![Mapping](~/ef6/media/mapping.png)

-   Selezionare il **PersonHireInfo** associazione nell'area di progettazione.
-   Pulsante destro del mouse su area di progettazione e seleziona l'associazione **proprietà**.
-   Nel **delle proprietà** finestra, seleziona la **vincoli referenziali** proprietà e fare clic sul pulsante dei puntini di sospensione.
-   Selezionare **Person** dalle **dell'entità** elenco a discesa.
-   Premere **OK**.

 

## <a name="use-the-model"></a>Usare il modello

-   Incollare il codice seguente nel metodo Main.

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   Compilare l'applicazione ed eseguirla.

Le istruzioni T-SQL seguenti sono state eseguite nel **School** database come risultato dell'esecuzione di questa applicazione. 

-   Quanto segue **Inserisci** è stata eseguita in seguito all'esecuzione rapida. SaveChanges () e che combina dati dal **Person** e **HireInfo** entità

    ![INS](~/ef6/media/insert.png)

-   Quanto segue **seleziona** è stata eseguita in seguito all'esecuzione rapida. People.FirstOrDefault() e seleziona solo le colonne di cui è stato eseguito il mapping a **persona**

    ![Selezionare 1](~/ef6/media/select1.png)

-   Quanto segue **selezionate** è stato eseguito come risultato l'accesso a existingPerson.Instructor di proprietà di navigazione e consente di selezionare solo le colonne mappate a **HireInfo**

    ![Selezionare 2](~/ef6/media/select2.png)
