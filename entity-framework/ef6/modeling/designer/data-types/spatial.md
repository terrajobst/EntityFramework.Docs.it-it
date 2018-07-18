---
title: Spaziali - Entity Framework Designer - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
caps.latest.revision: 3
ms.openlocfilehash: 2332818275763fd90274f426b7fced4c3c14ac17
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121323"
---
# <a name="spatial---ef-designer"></a>Spaziali - finestra di progettazione di Entity Framework
> [!NOTE]
> **EF5 e versioni successive solo** -le funzionalità, le API, e così via illustrati in questa pagina sono stati introdotti in Entity Framework 5. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.

La procedura dettagliata video e procedura dettagliata illustra come eseguire il mapping di tipi di dati spaziali con Entity Framework Designer. Viene inoltre illustrato come utilizzare una query LINQ per trovare una distanza tra due posizioni.

Questa procedura dettagliata verrà utilizzato il primo modello per creare un nuovo database, ma la finestra di progettazione di Entity Framework è anche utilizzabile con il [Database First](~/ef6/modeling/designer/workflows/database-first.md) flusso di lavoro per eseguire il mapping a un database esistente.

Supporto per il tipo spaziale è stata introdotta in Entity Framework 5. Si noti che per usare le nuove funzionalità, ad esempio tipo spaziale, enumerazioni e funzioni con valori di tabella, deve avere come destinazione .NET Framework 4.5. Visual Studio 2012 è destinato a .NET 4.5 per impostazione predefinita.

Per utilizzare i tipi di dati spaziali è anche necessario usare un provider di Entity Framework con supporto spaziale. Visualizzare [supporto del provider per i tipi spaziali](~/ef6/fundamentals/providers/spatial-support.md) per altre informazioni.

Esistono due tipi di dati spaziali principale: geografia e geometria. Il tipo di dati geography archivia dati ellissoidali (ad esempio, cui è coordinate GPS latitudine e longitudine). Il tipo di dati geometry rappresenta un sistema di coordinate Euclideo (piano).

## <a name="watch-the-video"></a>Guarda il video
In questo video viene illustrato come eseguire il mapping di tipi di dati spaziali con Entity Framework Designer. Viene inoltre illustrato come utilizzare una query LINQ per trovare una distanza tra due posizioni.

**Presentato da**: Julia Kornich

**Video**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)

## <a name="pre-requisites"></a>Prerequisiti

È necessario disporre di Visual Studio 2012, Ultimate, Premium, Professional o Web Express edition per completare questa procedura dettagliata.

## <a name="set-up-the-project"></a>Configurare il progetto

1.  Aprire Visual Studio 2012
2.  Nel **File** dal menu **New**, quindi fare clic su **progetto**
3.  Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **Console** modello
4.  Immettere **SpatialEFDesigner** come nome del progetto e fare clic su **OK**

## <a name="create-a-new-model-using-the-ef-designer"></a>Creare un nuovo modello utilizzando la finestra di progettazione di Entity Framework

1.  Fare clic sul nome del progetto in Esplora soluzioni, scegliere **Add**, quindi fare clic su **nuovo elemento**
2.  Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro dei modelli
3.  Immettere **UniversityModel.edmx** per il nome del file e quindi fare clic su **Add**
4.  Nella pagina di procedura guidata Entity Data Model, selezionare **modello vuoto** nella finestra di dialogo Scegli contenuto Model
5.  Fare clic su **fine**

Entity Designer, che fornisce un'area di progettazione per la modifica del modello, viene visualizzato.

La procedura guidata consente di effettuare le azioni seguenti:

-   Genera il file EnumTestModel.edmx che definisce il modello concettuale, il modello di archiviazione e il mapping tra di essi. Imposta la proprietà Metadata Artifact Processing del file con estensione edmx da incorporare nell'Assembly di Output in modo che i file di metadati generati ottengano incorporati nell'assembly.
-   Aggiunge un riferimento agli assembly seguenti: EntityFramework System.ComponentModel.DataAnnotations e Data. Entity.
-   Crea file UniversityModel.tt e UniversityModel.Context.tt e li aggiunge sotto il file con estensione edmx. Questi file di modello T4 generano il codice che definisce il tipo DbContext derivata e i tipi POCO mappate alle entità nel modello con estensione edmx

## <a name="add-a-new-entity-type"></a>Aggiungere un nuovo tipo di entità

1.  Fare doppio clic su un'area vuota dell'area di progettazione, seleziona **Add -&gt; entità**, viene visualizzata la finestra di dialogo nuova entità
2.  Specificare **University** per il tipo di nome e specificare **UniversityID una** per il nome della proprietà chiave, lasciare il tipo **Int32**
3.  Fare clic su **OK**
4.  L'entità e scegliere **Aggiungi nuovo -&gt; proprietà scalare**
5.  Rinominare la nuova proprietà a **nome**
6.  Aggiungere un'altra proprietà scalare e rinominarlo **ubicazione** aprire la finestra proprietà e modificare il tipo della nuova proprietà a **Geography**
7.  Salvare il modello e compilare il progetto
    > [!NOTE]
    > Quando si compila, avvisi relativi a entità non mappata e associazioni vengano visualizzati nell'elenco errori. È possibile ignorare questi avvisi perché dopo che si sceglie di generare il database dal modello, gli errori non verranno più visualizzato.

## <a name="generate-database-from-model"></a>Generare il Database da modello

A questo punto è possibile generare un database che si basa sul modello.

1.  Fare doppio clic su uno spazio vuoto in Entity Designer e selezionare **genera Database da modello**
2.  Scegliere la finestra di dialogo di connessione dati della procedura guidata genera Database viene visualizzato fare clic sui **nuova connessione** pulsante specificare **(localdb)\\mssqllocaldb** per il nome del server e la  **Università** per il database e fare clic su **OK**
3.  Verrà visualizzata, fare clic su una finestra di dialogo che chiede se si desidera creare un nuovo database **Sì**.
4.  Fare clic su **successivo** e la procedura guidata Crea Database genera data definition language (DDL) per la creazione di un database il linguaggio DDL generato viene visualizzata nel riepilogo delle impostazioni di finestra di dialogo casella si noti che l'istruzione DDL non contiene una definizione per un tabella che esegue il mapping al tipo di enumerazione
5.  Fare clic su **fine** facendo clic su Fine non viene eseguito lo script DDL.
6.  La procedura guidata Crea Database esegue le operazioni seguenti: apre la **UniversityModel.edmx.sql** nell'Editor T-SQL genera le sezioni di mapping dello schema e archivio dell'elemento EDMX file consente di aggiungere informazioni sulla stringa di connessione del file app. config
7.  Scegliere il pulsante destro del mouse nell'Editor T-SQL e selezionare **Execute** Connetti al finestra di dialogo Server visualizzata, immettere le informazioni di connessione del passaggio 2 e fare clic su **Connect**
8.  Per visualizzare lo schema generato, fare doppio clic sul nome del database in Esplora oggetti di SQL Server e selezionare **Aggiorna**

## <a name="persist-and-retrieve-data"></a>Salvare in modo permanente e recuperare i dati

Aprire il file Program.cs in cui è definito il metodo Main. Aggiungere il codice seguente nella funzione Main.

Il codice aggiunge due nuovi oggetti University al contesto. Proprietà spaziali vengono inizializzate tramite il metodo DbGeography.FromText. Il punto di geografia è rappresentato come WellKnownText viene passato al metodo. Il codice quindi Salva i dati. Quindi, LINQ query che restituisce un oggetto University dove il percorso è più vicino alla posizione specificata, viene costruita ed eseguita.

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
    {
        Name = "School of Fine Art",
        Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
    });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                                orderby u.Location.Distance(myLocation)
                                select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

Compilare l'applicazione ed eseguirla. Il programma produce l'output seguente:

```
The closest University to you is: School of Fine Art.
```

Per visualizzare i dati nel database, fare doppio clic sul nome del database in Esplora oggetti di SQL Server e selezionare **Aggiorna**. Quindi, scegliere il pulsante destro del mouse sulla tabella e selezionare **dati della visualizzazione**.

## <a name="summary"></a>Riepilogo

In questa procedura dettagliata è stato illustrato come eseguire il mapping di tipi spaziali usando Entity Framework Designer e su come usare i tipi spaziali nel codice. 
