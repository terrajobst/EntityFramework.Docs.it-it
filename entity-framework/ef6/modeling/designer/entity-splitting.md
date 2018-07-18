---
title: Separazione delle entità della finestra di progettazione - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
caps.latest.revision: 3
ms.openlocfilehash: 386b739363fb78641d5ebd8130fd19008bc8c9f6
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121107"
---
# <a name="designer-entity-splitting"></a>Suddivisione di entità della finestra di progettazione
Questa procedura dettagliata illustra come eseguire il mapping di un tipo di entità a due tabelle modificando un modello con Entity Framework Designer (Entity Framework Designer). È possibile eseguire il mapping di un'entità a più tabelle quando le tabelle in questione condividono una chiave comune. I concetti relativi all'esecuzione del mapping di un tipo di entità a due tabelle possono essere facilmente estesi anche all'esecuzione del mapping a più di due tabelle.

L'immagine seguente mostra le finestre principali che vengono usate quando si lavora con la finestra di progettazione di Entity Framework.

![EFDesigner](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Prerequisiti

Edizione Visual Studio 2012 o Visual Studio 2010, Ultimate, Premium, Professional o Web Express.

## <a name="create-the-database"></a>Creare il Database

Il server di database che viene installato con Visual Studio è diverso a seconda della versione di Visual Studio installata:

-   Se si usa Visual Studio 2012, quindi si creerà un database LocalDB.
-   Se si usa Visual Studio 2010 si creerà un database SQL Express.

Prima di tutto si creerà un database con due tabelle che si intende combinare in una singola entità.

-   Aprire Visual Studio
-   **Visualizzazione -&gt; Esplora Server**
-   Fare clic con il pulsante destro sul **connessioni dati -&gt; Aggiungi connessione...**
-   Se si è ancora connessi a un database da Esplora Server prima che è necessario selezionare **Microsoft SQL Server** come origine dati
-   Connettersi a LocalDB o SQL Express, in base alla quale è stato installato
-   Immettere **EntitySplitting** come nome del database
-   Selezionare **OK** e verrà richiesto se si desidera creare un nuovo database, selezionare **Sì**
-   Il nuovo database verrà ora visualizzato in Esplora Server
-   Se si usa Visual Studio 2012
    -   Fare doppio clic sul database in Esplora Server e selezionare **nuova Query**
    -   Copiare il codice SQL seguente nella nuova query, quindi fare clic su query e selezionare **Execute**
-   Se si usa Visual Studio 2010
    -   Selezionare **- Data&gt; Transact SQL Editor -&gt; nuova connessione Query...**
    -   Immettere **.\\ SQLEXPRESS** come nome del server e fare clic su **OK**
    -   Selezionare il **EntitySplitting** database dall'elenco a discesa nella parte superiore dell'editor di query
    -   Copiare il codice SQL seguente nella nuova query, quindi fare clic su query e selezionare **Esegui SQL**

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a>Creare il progetto

-   Scegliere **Nuovo** dal menu **File**, quindi fare clic su **Progetto**.
-   Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **applicazione Console** modello.
-   Immettere **MapEntityToTablesSample** come nome del progetto e fare clic su **OK**.
-   Fare clic su **No** se viene richiesto di salvare la query SQL creata nella prima sezione.

## <a name="create-a-model-based-on-the-database"></a>Creare un modello basato sul Database

-   Fare clic sul nome del progetto in Esplora soluzioni, scegliere **Add**, quindi fare clic su **nuovo elemento**.
-   Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro dei modelli.
-   Immettere **MapEntityToTablesModel.edmx** per il nome del file e quindi fare clic su **Add**.
-   Nella finestra di dialogo Scegli contenuto Model, selezionare **genera da database**, quindi fare clic su **successivo.**
-   Selezionare il **EntitySplitting** connessione nell'elenco a discesa e fare clic su **successivo**.
-   Nella finestra di dialogo Scegli oggetti di Database, selezionare la casella accanto al **tabelle** nodo.
    Tutte le tabelle da verrà aggiunto il **EntitySplitting** database al modello.
-   Scegliere **Fine**.

Entity Designer, che fornisce un'area di progettazione per la modifica del modello, viene visualizzato.

## <a name="map-an-entity-to-two-tables"></a>Eseguire il mapping di un'entità a due tabelle

In questo passaggio si aggiornerà il **Person** tipo di entità per combinare dati provenienti dal **persona** e **PersonInfo** tabelle.

-   Selezionare il **messaggio di posta elettronica** e **Phone** le proprietà del * * PersonInfo * * entità, quindi premere **Ctrl + X** chiavi.
-   Selezionare la * * persona * * entity e premere **Ctrl + V** chiavi.
-   Nell'area di progettazione, selezionare la **PersonInfo** entità e premere **eliminare** pulsante sulla tastiera.
-   Fare clic su **No** quando viene richiesto se si desidera rimuovere il **PersonInfo** tabella dal modello, si sta tentando di eseguire il mapping al **persona** entità.

    ![DeleteTables](~/ef6/media/deletetables.png)

I passaggi successivi presuppongono il **Dettagli Mapping** finestra. Se non è possibile visualizzare questa finestra, fare doppio clic su area di progettazione e seleziona **Dettagli Mapping**.

-   Selezionare il **Person** tipo di entità e fare clic su **&lt;aggiungere una tabella o vista&gt;** nel **Dettagli Mapping** finestra.
-   Selezionare * * PersonInfo * * nell'elenco a discesa.
    Il **Dettagli Mapping** finestra viene aggiornata con i mapping delle colonne predefinite, queste sono accettabili per questo scenario.

Il **Person** tipo di entità è stato mappato il **persona** e **PersonInfo** tabelle.

![Mapping2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a>Usare il modello

-   Incollare il codice seguente nel metodo Main.

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   Compilare l'applicazione ed eseguirla.

Le istruzioni T-SQL seguenti sono state eseguite sul database come risultato dell'esecuzione di questa applicazione. 

-   I seguenti due **Inserisci** le istruzioni sono state eseguite in seguito all'esecuzione rapida. SaveChanges (). Adottano i dati dal **Person** entità e la divisione tra la **persona** e **PersonInfo** tabelle.

    ![Insert1](~/ef6/media/insert1.png)

    ![Insert2](~/ef6/media/insert2.png)
-   Quanto segue **seleziona** è stata eseguita come risultato l'enumerazione degli utenti del database. Combina i dati di **Person** e **PersonInfo** tabella.

    ![Seleziona](~/ef6/media/select.png)
