---
title: Suddivisione di tabelle della finestra di progettazione-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: f5e7532e6c0b473d8ce77cbd11e3e673b0af6cbe
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921788"
---
# <a name="designer-table-splitting"></a>Suddivisione di tabelle della finestra di progettazione
In questa procedura dettagliata viene illustrato come eseguire il mapping di più tipi di entità a una singola tabella modificando un modello con il Entity Framework Designer (EF designer).

Uno dei motivi per cui è consigliabile utilizzare la suddivisione delle tabelle è ritardare il caricamento di alcune proprietà quando si utilizza il caricamento lazy per caricare gli oggetti. È possibile separare le proprietà che potrebbero contenere quantità molto elevate di dati in un'entità distinta e caricarla solo quando necessario.

Nell'immagine seguente vengono illustrate le finestre principali che vengono usate quando si usa la finestra di progettazione EF.

![EF Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Prerequisiti

Per completare questa procedura dettagliata, è necessario disporre di:

- Una versione recente di Visual Studio.
- [Database di esempio School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurare il progetto

Questa procedura dettagliata usa Visual Studio 2012.

-   Aprire Visual Studio 2012.
-   Scegliere **Nuovo** dal menu **File**, quindi fare clic su **Progetto**.
-   Nel riquadro sinistro fare clic su Visual C\#, quindi selezionare il modello applicazione console.
-   Immettere **TableSplittingSample** come nome del progetto e fare clic su **OK**.

## <a name="create-a-model-based-on-the-school-database"></a>Creare un modello basato sul database School

-   Fare clic con il pulsante destro del mouse sul nome del progetto in Esplora soluzioni, scegliere **Aggiungi**, quindi fare clic su **nuovo elemento**.
-   Selezionare **dati** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro modelli.
-   Immettere **TableSplittingModel. edmx** per il nome del file e quindi fare clic su **Aggiungi**.
-   Nella finestra di dialogo Scegli contenuto Model selezionare **genera da database**, quindi fare clic su **Avanti.**
-   Fare clic su nuova connessione. Nella finestra di dialogo Proprietà connessione immettere il nome del server (ad esempio, **(local DB)\\mssqllocaldb**), selezionare il metodo di autenticazione, digitare **School** per il nome del database, quindi fare clic su **OK**.
    La finestra di dialogo scegliere la connessione dati viene aggiornata con l'impostazione di connessione al database.
-   Nella finestra di dialogo Seleziona oggetti di database espandere le **tabelle** nodo e controllare la tabella **Person** . La tabella specificata verrà aggiunta al modello **School** .
-   Fare clic su **fine**.

Viene visualizzata la Entity Designer, che fornisce un'area di progettazione per la modifica del modello. Tutti gli oggetti selezionati nella finestra di dialogo **Scegli oggetti di Database** vengono aggiunti al modello.

## <a name="map-two-entities-to-a-single-table"></a>Eseguire il mapping di due entità a una singola tabella

In questa sezione si suddividerà l'entità **Person** in due entità, quindi la si eseguirà il mapping a una singola tabella.

> [!NOTE]
> L'entità **Person** non contiene proprietà che possono contenere grandi quantità di dati; viene usato solo come esempio.

-   Fare clic con il pulsante destro del mouse su un'area vuota dell'area di progettazione, scegliere **Aggiungi nuovo**e fare clic su **entità**.
    Verrà visualizzata la finestra di dialogo **nuova entità** .
-   Digitare **HireInfo** per il **nome dell'entità** e **PersonID** per il nome della **proprietà della chiave** .
-   Fare clic su **OK**.
-   Nell'area di progettazione verrà creato e visualizzato un nuovo tipo di entità.
-   Selezionare la proprietà  **assunto** del tipo di entità **Person** e premere **CTRL + X** .
-   Selezionare l'entità **HireInfo** e premere **CTRL + V** .
-   Creare un'associazione tra **Person** e **HireInfo**. A tale scopo, fare clic con il pulsante destro del mouse su un'area vuota dell'area di progettazione, scegliere **Aggiungi nuovo**e fare clic su **associazione**.
-   Verrà visualizzata la finestra di dialogo Aggiungi di **associazione** . Per impostazione predefinita, viene fornito il nome **PersonHireInfo** .
-   Specificare la molteplicità **1 (una)** su entrambe le estremità della relazione.
-   Fare clic su **OK**.

Il passaggio successivo richiede la finestra **Dettagli Mapping** . Se questa finestra non è visibile, fare clic con il pulsante destro del mouse sull'area di progettazione e scegliere **Dettagli mapping**.

-   Selezionare il tipo di entità  **HireInfo** e fare clic su **&lt;aggiungere una tabella o una vista&gt;**  nella finestra  **Dettagli mapping** .
-   Selezionare **Person** dall'elenco **a discesa&lt;aggiungere una tabella o una vista&gt;**  campo. L'elenco contiene le tabelle o le viste a cui è possibile eseguire il mapping dell'entità selezionata.
    Per impostazione predefinita, è necessario eseguire il mapping delle proprietà appropriate.

    ![Mapping](~/ef6/media/mapping.png)

-   Selezionare l'associazione **PersonHireInfo** nell'area di progettazione.
-   Fare clic con il pulsante destro del mouse sull'associazione nell'area di progettazione e scegliere **Proprietà**.
-   Nella finestra **Proprietà** selezionare la proprietà **vincoli referenziali** , quindi fare clic sul pulsante con i puntini di sospensione.
-   Selezionare **Person** dall'elenco a discesa **principale** .
-   Fare clic su **OK**.

 

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

Le istruzioni T-SQL seguenti sono state eseguite nel database **School** in seguito all'esecuzione dell'applicazione. 

-   L' **inserimento** seguente è stato eseguito in seguito all'esecuzione del contesto. SaveChanges () e combina i dati delle entità **Person** e **HireInfo**

    ![Inserisci](~/ef6/media/insert.png)

-   L'istruzione **Select** seguente è stata eseguita in seguito all'esecuzione del contesto. People. FirstOrDefault () e seleziona solo le colonne di cui è stato eseguito il mapping a **Person**

    ![Selezionare 1](~/ef6/media/select1.png)

-   La seguente **selezione** è stata eseguita in seguito all'accesso alla proprietà di navigazione ExistingPerson. Instructor e seleziona solo le colonne mappate a **HireInfo**

    ![Selezionare 2](~/ef6/media/select2.png)
