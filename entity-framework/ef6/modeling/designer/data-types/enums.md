---
title: Supporto di enum - Entity Framework Designer - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 4c2562ade28dc30df20eb2a35c179582bf6e0777
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490792"
---
# <a name="enum-support---ef-designer"></a>Supporto di enum - finestra di progettazione di Entity Framework
> [!NOTE]
> **EF5 e versioni successive solo** -le funzionalità, le API, e così via illustrati in questa pagina sono stati introdotti in Entity Framework 5. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.

Questa procedura dettagliata video e procedura dettagliata illustra come usare i tipi di enumerazione con Entity Framework Designer. Viene inoltre illustrato come usare enumerazioni in una query LINQ.

Questa procedura dettagliata verrà utilizzato il primo modello per creare un nuovo database, ma la finestra di progettazione di Entity Framework è anche utilizzabile con il [Database First](~/ef6/modeling/designer/workflows/database-first.md) flusso di lavoro per eseguire il mapping a un database esistente.

Supporto di enumerazione è stata introdotta in Entity Framework 5. Per usare le nuove funzionalità come le enumerazioni, tipi di dati spaziali e funzioni con valori di tabella, deve avere come destinazione .NET Framework 4.5. Visual Studio 2012 è destinato a .NET 4.5 per impostazione predefinita.

In Entity Framework, un'enumerazione può avere i seguenti tipi sottostanti: **Byte**, **Int16**, **Int32**, **Int64** , o **SByte**.

## <a name="watch-the-video"></a>Guarda il Video
In questo video viene illustrato come utilizzare tipi enum con Entity Framework Designer. Viene inoltre illustrato come usare enumerazioni in una query LINQ.

**Presentato da**: Julia Kornich

**Video**: [WMV](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)

## <a name="pre-requisites"></a>Prerequisiti

È necessario disporre di Visual Studio 2012, Ultimate, Premium, Professional o Web Express edition per completare questa procedura dettagliata.

## <a name="set-up-the-project"></a>Configurare il progetto

1.  Aprire Visual Studio 2012
2.  Nel **File** dal menu **New**, quindi fare clic su **progetto**
3.  Nel riquadro sinistro, fare clic su **Visual C#\#**, quindi selezionare il **Console** modello
4.  Immettere **EnumEFDesigner** come nome del progetto e fare clic su **OK**

## <a name="create-a-new-model-using-the-ef-designer"></a>Creare un nuovo modello utilizzando la finestra di progettazione di Entity Framework

1.  Fare clic sul nome del progetto in Esplora soluzioni, scegliere **Add**, quindi fare clic su **nuovo elemento**
2.  Selezionare **Data** dal menu a sinistra e quindi selezionare **ADO.NET Entity Data Model** nel riquadro dei modelli
3.  Immettere **EnumTestModel.edmx** per il nome del file e quindi fare clic su **Add**
4.  Nella pagina di procedura guidata Entity Data Model, selezionare **modello vuoto** nella finestra di dialogo Scegli contenuto Model
5.  Fare clic su **fine**

Entity Designer, che fornisce un'area di progettazione per la modifica del modello, viene visualizzato.

La procedura guidata consente di effettuare le azioni seguenti:

-   Genera il file EnumTestModel.edmx che definisce il modello concettuale, il modello di archiviazione e il mapping tra di essi. Imposta la proprietà Metadata Artifact Processing del file con estensione edmx da incorporare nell'Assembly di Output in modo che i file di metadati generati ottengano incorporati nell'assembly.
-   Aggiunge un riferimento agli assembly seguenti: EntityFramework System.ComponentModel.DataAnnotations e Data. Entity.
-   Crea file EnumTestModel.tt ed EnumTestModel.Context.tt e li aggiunge sotto il file con estensione edmx. Questi file di modello T4 generano il codice che definisce il tipo DbContext derivata e i tipi POCO mappate alle entità nel modello con estensione edmx.

## <a name="add-a-new-entity-type"></a>Aggiungere un nuovo tipo di entità

1.  Fare doppio clic su un'area vuota dell'area di progettazione, seleziona **Add -&gt; entità**, viene visualizzata la finestra di dialogo nuova entità
2.  Specificare **reparto** per il tipo di nome e specificare **DepartmentID** per il nome della proprietà chiave, lasciare il tipo **Int32**
3.  Fare clic su **OK**
4.  L'entità e scegliere **Aggiungi nuovo -&gt; proprietà scalare**
5.  Rinominare la nuova proprietà a **nome**
6.  Modificare il tipo della nuova proprietà al **Int32** (per impostazione predefinita, la nuova proprietà è di tipo stringa) per modificare il tipo, aprire la finestra proprietà e modificare la proprietà Type per **Int32**
7.  Aggiungere un'altra proprietà scalare e rinominarlo **Budget**, impostare il tipo su **decimale**

## <a name="add-an-enum-type"></a>Aggiungere un tipo Enum

1.  In Entity Framework Designer, fare clic su proprietà Name, selezionare **convertire all'enumerazione**

    ![Converti in Enum](~/ef6/media/converttoenum.png)

2.  Nel **Enum aggiungere** finestra di dialogo digitare **DepartmentNames** per nome del tipo di enumerazione, modificare il tipo sottostante per **Int32**, e quindi aggiungere i membri seguenti per il tipo: inglese, Matematiche e la convenienza

    ![Aggiungere il tipo di enumerazione](~/ef6/media/addenumtype.png)

3.  Premere **OK**
4.  Salvare il modello e compilare il progetto
    > [!NOTE]
    > Quando si compila, avvisi relativi a entità non mappata e associazioni vengano visualizzati nell'elenco errori. È possibile ignorare questi avvisi perché dopo che si sceglie di generare il database dal modello, gli errori non verranno più visualizzato.

Se si osserva la finestra Proprietà, si noterà che il tipo della proprietà del nome sia stato modificato **DepartmentNames** e il tipo di enumerazione appena aggiunta è stata aggiunta all'elenco dei tipi.

Se si passa alla finestra Browser modello, si noterà che il tipo è stato inoltre aggiunto il nodo di tipi di enumerazione.

![Finestra Browser modello](~/ef6/media/modelbrowser.png)

>[!NOTE]
> È anche possibile aggiungere nuovi tipi di enumerazione da questa finestra facendo clic sul pulsante destro del mouse e selezionando **Aggiungi tipo Enum**. Dopo aver creato il tipo verrà visualizzato nell'elenco dei tipi e sarebbe in grado di associare a una proprietà

## <a name="generate-database-from-model"></a>Generare il Database da modello

A questo punto è possibile generare un database che si basa sul modello.

1.  Fare doppio clic su uno spazio vuoto in Entity Designer e selezionare **genera Database da modello**
2.  Scegliere la finestra di dialogo di connessione dati della procedura guidata genera Database viene visualizzato fare clic sui **nuova connessione** pulsante specificare **(localdb)\\mssqllocaldb** per il nome del server e la  **EnumTest** per il database e fare clic su **OK**
3.  Verrà visualizzata, fare clic su una finestra di dialogo che chiede se si desidera creare un nuovo database **Sì**.
4.  Fare clic su **successivo** e la procedura guidata Crea Database genera data definition language (DDL) per la creazione di un database il linguaggio DDL generato viene visualizzata nel riepilogo delle impostazioni di finestra di dialogo casella si noti che l'istruzione DDL non contiene una definizione per un tabella che esegue il mapping al tipo di enumerazione
5.  Fare clic su **fine** facendo clic su Fine non viene eseguito lo script DDL.
6.  La procedura guidata Crea Database esegue le operazioni seguenti: apre la **EnumTest.edmx.sql** nell'Editor T-SQL genera le sezioni di mapping dello schema e archivio dell'elemento EDMX file consente di aggiungere informazioni sulla stringa di connessione del file app. config
7.  Scegliere il pulsante destro del mouse nell'Editor T-SQL e selezionare **Execute** Connetti al finestra di dialogo Server visualizzata, immettere le informazioni di connessione del passaggio 2 e fare clic su **Connect**
8.  Per visualizzare lo schema generato, fare doppio clic sul nome del database in Esplora oggetti di SQL Server e selezionare **Aggiorna**

## <a name="persist-and-retrieve-data"></a>Salvare in modo permanente e recuperare i dati

Aprire il file Program.cs in cui è definito il metodo Main. Aggiungere il codice seguente nella funzione Main. Il codice aggiunge un nuovo oggetto reparto al contesto. Quindi Salva i dati. Inoltre, il codice esegue una query LINQ che restituisce un reparto dove name è DepartmentNames.English.

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

Compilare l'applicazione ed eseguirla. Il programma produce l'output seguente:

```
DepartmentID: 1 Name: English
```

Per visualizzare i dati nel database, fare doppio clic sul nome del database in Esplora oggetti di SQL Server e selezionare **Aggiorna**. Quindi, scegliere il pulsante destro del mouse sulla tabella e selezionare **dati della visualizzazione**.

## <a name="summary"></a>Riepilogo

In questa procedura dettagliata è stato illustrato come eseguire il mapping di tipi di enumerazione mediante Entity Framework Designer e su come usare le enumerazioni nel codice. 
