---
title: I tipi complessi - Entity Framework Designer - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
caps.latest.revision: 3
ms.openlocfilehash: 6d0744921085afdafa35d137d276c54738cdd465
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121118"
---
# <a name="complex-types---ef-designer"></a>Tipi complessi - finestra di progettazione di Entity Framework
Questo argomento viene illustrato come eseguire il mapping dei tipi complessi con Entity Framework Designer (Entity Framework Designer) e su come eseguire una query per le entità che contengono le proprietà del tipo complesso.

L'immagine seguente mostra le finestre principali che vengono usate quando si lavora con la finestra di progettazione di Entity Framework.

![EFDesigner](~/ef6/media/efdesigner.png)

> [!NOTE]
> Quando si compila il modello concettuale, avvisi relativi a entità non mappata e associazioni vengano visualizzati nell'elenco errori. È possibile ignorare questi avvisi perché dopo aver scelto di generare il database dal modello, gli errori non verranno più visualizzato.

## <a name="what-is-a-complex-type"></a>Che cos'è un tipo complesso

I tipi complessi sono proprietà non scalari di tipi di entità che consentono l'organizzazione delle proprietà scalari nelle entità. Analogamente alle entità, i tipi complessi sono costituiti da proprietà scalari o da altre proprietà dei tipi complessi.

Quando si lavora con gli oggetti che rappresentano tipi complessi, tenere presente quanto segue:

-   I tipi complessi non dispongono di chiavi e pertanto non possono esistere indipendentemente. I tipi complessi possono esistere solo come proprietà di tipi di entità o gli altri tipi complessi.
-   I tipi complessi non possono partecipare alle associazioni e non possono contenere le proprietà di navigazione.
-   Proprietà del tipo complesso non può essere **null**. Un * * InvalidOperationException * * si verifica quando si **DbContext.SaveChanges** viene chiamato e viene rilevato un oggetto complesso null. Le proprietà scalari degli oggetti complessi possono essere **null**.
-   I tipi complessi non possono ereditare da altri tipi complessi.
-   È necessario definire il tipo complesso come un **classe**. 
-   Entity Framework rileva le modifiche ai membri su un oggetto di tipo complesso quando **DbContext.DetectChanges** viene chiamato. Chiamata di Entity Framework **DetectChanges** automaticamente quando vengono chiamati i membri seguenti: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a>Effettuare il refactoring di proprietà di un'entità nel nuovo tipo complesso

Se si dispone già di un'entità nel modello concettuale è possibile effettuare il refactoring di alcune delle proprietà in una proprietà del tipo complesso.

Nell'area di progettazione, selezionare uno o più proprietà (escludendo le proprietà di navigazione) di un'entità, quindi fare clic e selezionare **refactoring -&gt; passare al nuovo tipo complesso**.

![Refactoring](~/ef6/media/refactor.png)

Viene aggiunto un nuovo tipo complesso con le proprietà selezionate per il **Browser modello**. Al tipo complesso viene assegnato un nome predefinito.

Una proprietà complessa del tipo appena creato sostituisce le proprietà selezionate. Tutti i mapping delle proprietà vengono mantenuti.

![Refactor2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a>Creare un nuovo tipo complesso

È anche possibile creare un nuovo tipo complesso che non contiene le proprietà di un'entità esistente.

Fare doppio clic il **i tipi complessi** cartella nella finestra Browser modello, scegliere **AddNew tipo complesso...** . In alternativa, è possibile selezionare i **i tipi complessi** cartella e premere il **Inserisci** sulla tastiera.

![AddNewComplextype](~/ef6/media/addnewcomplextype.png)

Un nuovo tipo complesso verrà aggiunto nella cartella con un nome predefinito. È ora possibile aggiungere proprietà al tipo.

## <a name="add-properties-to-a-complex-type"></a>Aggiungere proprietà a un tipo complesso

Le proprietà di un tipo complesso possono essere tipi scalari o tipi complessi esistenti. Tuttavia, le proprietà dei tipi complessi non possono avere riferimenti circolari. Ad esempio, un tipo complesso **OnsiteCourseDetails** non può contenere una proprietà di tipo complesso **OnsiteCourseDetails**.

È possibile aggiungere una proprietà a un tipo complesso in uno dei modi elencati di seguito.

-   Fare doppio clic su un tipo complesso nella finestra Browser modello, scegliere **Add**, quindi scegliere **proprietà scalare** oppure **proprietà complessa**, quindi selezionare il tipo di proprietà desiderata. In alternativa, è possibile selezionare un tipo complesso e quindi premere il **Inserisci** sulla tastiera.  

    ![AddPropertiestoComplexType](~/ef6/media/addpropertiestocomplextype.png)

    Una nuova proprietà verrà aggiunta al tipo complesso con un nome predefinito.

- OR:

-   Fare doppio clic su una proprietà di entità nel **Entity Framework Designer** surface e selezionare **copia**, quindi fare clic sul tipo complesso nel **Browser modello** e selezionare  **Incolla**.

## <a name="rename-a-complex-type"></a>Rinominare un tipo complesso

Quando si rinomina un tipo complesso, tutti i riferimenti al tipo presenti nell'intero progetto vengono aggiornati.

-   Fare lentamente doppio clic su un tipo complesso nel **Browser modello**.
    Il nome verrà selezionato e sarà in modalità di modifica.

- OR:

-   Fare doppio clic su un tipo complesso nel **Visualizzatore modelli** e selezionare **rinominare**.

- OR:

-   Selezionare un tipo complesso nella finestra Browser modello e premere il tasto F2.

- OR:

-   Fare doppio clic su un tipo complesso nel **Visualizzatore modelli** e selezionare **proprietà**. Modificare il nome di **proprietà** finestra.

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a>Aggiungere un tipo complesso esistente a un'entità ed eseguire il mapping le relative proprietà alle colonne della tabella

1.  Fare doppio clic su un'entità, scegliere **Aggiungi nuovo**e selezionare **ComplexProperty**.
    Una proprietà del tipo complesso con un nome predefinito verrà aggiunta all'entità. Un tipo predefinito, scelto tra i tipi complessi esistenti, verrà assegnato alla proprietà.
2.  Assegnare il tipo desiderato alla proprietà nella **proprietà** finestra.
    Dopo avere aggiunto una proprietà del tipo complesso a un'entità, è necessario eseguire il mapping delle proprietà alle colonne della tabella.
3.  Fare doppio clic su un tipo di entità nell'area di progettazione o nel **Visualizzatore modelli** e selezionare **mapping delle tabelle**.
    Mapping di tabella vengono visualizzati nei **Dettagli Mapping** finestra.
4.  Espandere la **esegue il mapping a &lt;nome tabella&gt;**  nodo.
    Oggetto **mapping delle colonne** nodo viene visualizzato.
5.  Espandere la **mapping delle colonne** nodo.
    Verrà visualizzato un elenco di tutte le colonne della tabella. Le proprietà predefinite (se presente) per cui eseguire il mapping di colonne sono elencati sotto la **valore/proprietà** intestazione.
6.  Selezionare la colonna che si desidera eseguire il mapping e quindi fare doppio clic su corrispondente **valore/proprietà** campo.
    Verrà visualizzato un elenco a discesa di tutte le proprietà scalari.
7.  Selezionare la proprietà appropriata.

    ![MapComplexType](~/ef6/media/mapcomplextype.png)

8.  Ripetere i passaggi 6 e 7 per ogni colonna della tabella.

>[!NOTE]
> Per eliminare un mapping di colonne, selezionare la colonna che si desidera eseguire il mapping e quindi scegliere il **valore/proprietà** campo. Quindi, selezionare **eliminare** nell'elenco a discesa.

## <a name="map-a-function-import-to-a-complex-type"></a>Mappare un'importazione di funzioni a un tipo complesso

Le importazioni di funzioni sono basate sulle stored procedure. Per mappare un'importazione di funzioni a un tipo complesso, le colonne restituite dalla stored procedure corrispondente devono corrispondere alle proprietà del tipo complesso nel numero e devono disporre di tipi di archiviazione che sono compatibili con i tipi di proprietà.

-   Fare doppio clic su una funzione importata che si desidera eseguire il mapping a un tipo complesso.

    ![FunctionImports](~/ef6/media/functionimports.png)

-   Specificare le impostazioni per la nuova importazione di funzioni come segue:
    -   Specificare la stored procedure per il quale si sta creando un'importazione di funzioni nel **nome Stored Procedure** campo. Questo campo è un elenco a discesa in cui sono visualizzate tutte le stored procedure contenute nel modello di archiviazione.
    -   Specificare il nome dell'oggetto function import nel **nome Function Import** campo.
    -   Selezionare **complessi** come il valore restituito digitare e quindi specificare il tipo restituito complesso specifico scegliendo il tipo appropriato nell'elenco a discesa.

        ![EditFunctionImport](~/ef6/media/editfunctionimport.png)

-   Fare clic su **OK**.
    L'importazione di funzioni viene creata nel modello concettuale.

### <a name="customize-column-mapping-for-function-import"></a>Personalizzare i Mapping per l'importazione di funzioni di colonne

-   Importazione di funzioni nella finestra Browser modello e scegliere **importare Mapping di funzione**.
    Il **ulteriori dettagli sul Mapping** finestra viene visualizzata e viene illustrato il mapping predefinito per l'importazione di funzioni. Le frecce indicano i mapping tra i valori della proprietà e i valori della colonna. Per impostazione predefinita, i nomi della colonna corrispondono ai nomi delle proprietà di tipo complesso. I nomi delle colonne predefiniti vengono visualizzati in grigio.
-   Se necessario, modificare i nomi delle colonne per renderli corrispondenti ai nomi delle colonne restituiti dalla stored procedure che corrisponde all'importazione di funzioni.

## <a name="delete-a-complex-type"></a>Eliminare un tipo complesso

Quando si elimina un tipo complesso, questo viene eliminato dal modello concettuale; vengono inoltre eliminati i mapping di tutte le istanze del tipo. I riferimenti al tipo non vengono tuttavia aggiornati. Ad esempio, se un'entità dispone di una proprietà di tipo complesso di tipo Tipocomplesso1 e Tipocomplesso1 viene eliminato nel **Browser modello**, non viene aggiornata la proprietà dell'entità corrispondente. Il modello non verrà convalidato poiché contiene un'entità che fa riferimento a un tipo complesso eliminato. Tramite Entity Designer è possibile aggiornare o eliminare i riferimenti ai tipi complessi eliminati.

-   Fare doppio clic su un tipo complesso nella finestra Browser modello e selezionare **Elimina**.

- OR:

-   Selezionare un tipo complesso nella finestra Browser modello e premere il tasto CANC sulla tastiera.

## <a name="query-for-entities-containing-properties-of-complex-type"></a>Eseguire query sulle entità che contiene le proprietà del tipo complesso

Il codice seguente viene illustrato come eseguire una query che restituisce una raccolta di entità come oggetti di tipo che contengono una proprietà di tipo complesso.

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        var courses =
            from c in context.OnsiteCourses
            order by c.Details.Time
            select c;

        foreach (var c in courses)
        {
            Console.WriteLine("Time: " + c.Details.Time);
            Console.WriteLine("Days: " + c.Details.Days);
            Console.WriteLine("Location: " + c.Details.Location);
        }
    }
```
