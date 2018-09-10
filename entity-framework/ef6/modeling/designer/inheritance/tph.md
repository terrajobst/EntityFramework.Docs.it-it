---
title: Ereditarietà tabella per gerarchia della finestra di progettazione - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 1eb935414b20d6e93e9d470ccc845bc13626ed3a
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250843"
---
# <a name="designer-tph-inheritance"></a>Ereditarietà tabella per gerarchia della finestra di progettazione
Questa procedura dettagliata viene illustrato come implementare l'ereditarietà di tabella per gerarchia (TPH) nel modello concettuale con Entity Framework Designer (Entity Framework Designer). Ereditarietà tabella per gerarchia utilizza una tabella di database per gestire i dati per tutti i tipi di entità in una gerarchia di ereditarietà.

In questa procedura dettagliata verrà eseguito il mapping della tabella Person ai tre tipi di entità: Person (tipo di base), (deriva da Person) Student e Instructor (deriva dalla persona). Verrà creato un modello concettuale dal database (Database First) e quindi modificare il modello per implementare l'ereditarietà tabella per gerarchia usando la finestra di progettazione di Entity Framework.

È possibile eseguire il mapping a un'ereditarietà tabella per gerarchia utilizzando Model First, ma si sarebbe necessario scrivere il proprio database generazione del flusso di lavoro che è complesso. È quindi necessario assegnare questo flusso di lavoro di **flusso di lavoro di Database generazione** proprietà nella finestra di progettazione di Entity Framework. Un'alternativa più semplice consiste nell'utilizzare Code First.

## <a name="other-inheritance-options"></a>Altre opzioni di ereditarietà

Tabella per tipo TPT () è un altro tipo di ereditarietà in cui tabelle separate nel database vengono mappate alle entità che partecipano l'ereditarietà.  Per informazioni su come eseguire il mapping dell'ereditarietà tabella per tipo con la finestra di progettazione di Entity Framework, vedere [TPT (ereditarietà) finestra di progettazione EF](~/ef6/modeling/designer/inheritance/tpt.md).

Ereditarietà dei tipi di tabella per tipo concreto (TP) e modelli di ereditarietà misti sono supportati dal runtime di Entity Framework, ma non sono supportati dalla finestra di progettazione di Entity Framework. Se si desidera utilizzare TPC o misto eredità, sono disponibili due opzioni: utilizzare Code First, oppure modificare manualmente i file con estensione EDMX. Se si sceglie di usare il file EDMX, nella finestra Dettagli Mapping verrà inserita nella "modalità provvisoria" e non sarà in grado di utilizzare la finestra di progettazione per modificare i mapping.

## <a name="prerequisites"></a>Prerequisiti

Per completare questa procedura dettagliata, è necessario disporre di:

- Una versione recente di Visual Studio.
- Il [database di esempio School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Configurare il progetto

-   Aprire Visual Studio 2012.
-   Selezionare **File -&gt; Novità -&gt; progetto**
-   Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **Console** modello.
-   Immettere **TPHDBFirstSample** come nome.
-   Scegliere **OK**.

## <a name="create-a-model"></a>Creare un modello

-   Il nome del progetto in Esplora soluzioni e scegliere **Add -&gt; nuovo elemento**.
-   Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro dei modelli.
-   Immettere **TPHModel.edmx** per il nome del file e quindi fare clic su **Add**.
-   Nella finestra di dialogo Scegli contenuto Model, selezionare **genera da database**, quindi fare clic su **successivo**.
-   Fare clic su **nuova connessione**.
    Nella finestra di dialogo proprietà di connessione, immettere il nome del server (ad esempio, **(localdb)\\mssqllocaldb**), selezionare il metodo di autenticazione, il tipo **School** per il nome del database e quindi Fare clic su **OK**.
    La finestra di dialogo Seleziona connessione dati viene aggiornata con l'impostazione di connessione di database.
-   Nella finestra di dialogo Scegli oggetti di Database, sotto il nodo tabelle, selezionare la **persona** tabella.
-   Scegliere **Fine**.

Entity Designer, che fornisce un'area di progettazione per la modifica del modello, viene visualizzato. Tutti gli oggetti selezionati nella finestra di dialogo Scegli oggetti di Database vengono aggiunti al modello.

Vale a dire come il **persona** l'aspetto di tabella nel database.

![Tabella Person](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a>Implementare l'ereditarietà tabella per gerarchia

Il **Person** tabella include le **discriminatore** colonna, che può avere uno dei due valori: "Student" e "Instructor". A seconda del valore di **persona** verrà mappata alla tabella il **studente** entità o il **insegnante** entità. Il **Person** tabella include anche due colonne, **HireDate** e **EnrollmentDate**, che deve essere **nullable** perché non può essere una persona un studente e un insegnante nello stesso momento (almeno non in questa procedura dettagliata).

### <a name="add-new-entities"></a>Aggiungi nuova entità

-   Aggiungere una nuova entità.
    A tale scopo, fare clic su uno spazio vuoto dell'area di progettazione di Entity Framework Designer e selezionare **Add -&gt;entità**.
-   Tipo **insegnante** per il **nome entità** e selezionare **persona** nell'elenco a discesa per il **tipo Base**.
-   Fare clic su **OK**.
-   Aggiungere un'altra nuova entità. Tipo **studente** per il **nome entità** e selezionare **persona** nell'elenco a discesa per il **tipo Base**.

Sono stati aggiunti due nuovi tipi di entità all'area di progettazione. Una freccia che punta dalla nuovi tipi di entità per il **Person** tipo di entità; ciò indica che **persona** è il tipo base per i nuovi tipi di entità.

-   Fare doppio clic sui **HireDate** proprietà delle **persona** entità. Selezionare **Taglia** (oppure usare il tasto Ctrl-X).
-   Fare doppio clic il **insegnante** entità e selezionare **Incolla** (oppure usare il tasto Ctrl + V).
-   Fare doppio clic il **HireDate** proprietà e selezionare **proprietà**.
-   Nel **le proprietà** impostare nella finestra di **Nullable** proprietà **false**.
-   Fare doppio clic sui **EnrollmentDate** proprietà delle **persona** entità. Selezionare **Taglia** (oppure usare il tasto Ctrl-X).
-   Fare doppio clic il **studente** entità e selezionare **Incolla (o chiave di uso di Ctrl + V).**
-   Selezionare il **EnrollmentDate** proprietà e impostare le **Nullable** proprietà **false**.
-   Selezionare il **persona** tipo di entità. Nel **le proprietà** impostare nella finestra relativa **astratto** proprietà **true**.
-   Eliminare il **discriminatore** proprietà dal **persona**. Nella sezione seguente è illustrato il motivo che deve essere eliminata.

### <a name="map-the-entities"></a>Eseguire il mapping di entità

-   Fare doppio clic il **insegnante** e selezionare **Mapping della tabella.**
    L'entità Instructor è selezionata nella finestra Dettagli Mapping.
-   Fare clic su **&lt;aggiungere una tabella o vista&gt;** nel **Dettagli Mapping** finestra.
    Il **&lt;aggiungere una tabella o vista&gt;** campo diventa un elenco a discesa di tabelle o visualizzazioni a cui può essere mappato l'entità selezionata.
-   Selezionare **persona** nell'elenco a discesa.
-   Il **Dettagli Mapping** finestra viene aggiornata con i mapping delle colonne predefinite e un'opzione per l'aggiunta di una condizione.
-   Fare clic su  **&lt;Aggiungi una condizione&gt;**.
    Il **&lt;Aggiungi una condizione&gt;** campo diventa un elenco a discesa delle colonne per cui è possibile impostare le condizioni.
-   Selezionare **discriminatore** nell'elenco a discesa.
-   Nel **operatore** della colonna della **Dettagli Mapping** finestra selezionare = nell'elenco a discesa.
-   Nel **valore/proprietà** colonna, digitare **insegnante**. Il risultato finale dovrebbe essere simile al seguente:

    ![Dettagli di mapping](~/ef6/media/mappingdetails2.png)

-   Ripetere questi passaggi per la **studente** tipo di entità, ma la condizione uguale alla marca **studente** valore.  
    *Il motivo, desiderassimo per rimuovere il **discriminatore** proprietà, è perché non è possibile associare più di una volta una colonna di tabella. Questa colonna verrà utilizzata per eseguire il mapping condizionale, pertanto non può essere usato per il mapping anche di proprietà. L'unico modo può essere utilizzato per entrambi, se una condizione Usa un' **Is Null** oppure **Is Not Null** confronto.*

A questo punto l'ereditarietà tabella per gerarchia risulta implementata.

![Finale della tabella per gerarchia](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a>Usare il modello

Aprire il **Program.cs** file dove il **Main** metodo è definito. Incollare il codice seguente nel **Main** (funzione). Il codice esegue tre query. La prima query restituisce tutti i **persona** oggetti. La seconda query Usa la **OfType** metodo restituisca **insegnante** oggetti. La terza query Usa la **OfType** metodo restituisca **studente** oggetti.

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
