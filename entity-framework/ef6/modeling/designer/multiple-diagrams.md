---
title: Più diagrammi per ogni modello - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: b95db5c8-de8d-43bd-9ccc-5df6a5e25e1b
caps.latest.revision: 3
ms.openlocfilehash: 3a022b3e44ecd4b6b62224cb6494c794397a9739
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121346"
---
# <a name="multiple-diagrams-per-model"></a>Più diagrammi per ogni modello
> [!NOTE]
> **EF5 e versioni successive solo** -le funzionalità, le API, e così via illustrati in questa pagina sono stati introdotti in Entity Framework 5. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.

Questa pagina e il video mostra come suddividere un modello in più diagrammi usando Entity Framework Designer (Entity Framework Designer). È possibile usare questa funzionalità quando il modello diventa troppo grande per visualizzare o modificare.

Nelle versioni precedenti di Entity Framework Designer si potrebbe avere solo un unico diagramma per ogni file con estensione EDMX. A partire da Visual Studio 2012, è possibile utilizzare la finestra di progettazione di Entity Framework per suddividere il file EDMX in più diagrammi.

## <a name="watch-the-video"></a>Guarda il video
In questo video mostra come suddividere un modello in più diagrammi usando Entity Framework Designer (Entity Framework Designer). È possibile usare questa funzionalità quando il modello diventa troppo grande per visualizzare o modificare.

**Presentato da**: Julia Kornich

**Video**: [WMV](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.wmv) | [MP4](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-mp4video-multiplediagrams.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.zip)

## <a name="ef-designer-overview"></a>Panoramica della finestra di progettazione di Entity Framework

Quando si crea un modello di utilizzo Entity Data Model Wizard della finestra di progettazione EF, un file con estensione edmx viene creato e aggiunto alla soluzione. Questo file definisce la forma delle entità e sul relativo mapping al database.

La finestra di progettazione di Entity Framework include i componenti seguenti:

-   Un'area di progettazione visiva per la modifica del modello. È possibile creare, modificare o eliminare entità e associazioni.
-   Oggetto **Browser modello** finestra che fornisce visualizzazioni dell'albero del modello.  Le entità e le relative associazioni si trovano sotto i *\[ModelName\]* cartella. Le tabelle di database e i vincoli si trovano sotto i  *\[ModelName\]*. Cartella Store.
-   Oggetto **Dettagli Mapping** finestra per visualizzare e modificare i mapping. È possibile mappare i tipi di entità o le associazioni a stored procedure, colonne e tabelle di database. 

La finestra della superficie di progettazione visiva viene aperto automaticamente quando viene completata la procedura guidata Entity Data Model. Se il Browser di modello non è visibile, fare doppio clic su principale area di progettazione e seleziona **Browser modello**.

Lo screenshot seguente mostra un file con estensione edmx aperto in Entity Framework Designer. Lo screenshot Mostra la superficie di progettazione visiva (a sinistra) e il **Browser modello** finestra (a destra).

![EFDesigner2](~/ef6/media/efdesigner2.png)

Per annullare un'operazione eseguita nella finestra di progettazione di Entity Framework, fare clic Ctrl-Z.

## <a name="working-with-diagrams"></a>Uso di diagrammi

Per impostazione predefinita Entity Framework Designer crea chiamato Diagram1 in un diagramma. Se si dispone di un diagramma con un numero elevato di entità e associazioni, si apprezzeranno più da suddividere logicamente tali. A partire da Visual Studio 2012, è possibile visualizzare il modello concettuale in più diagrammi.   

Quando si aggiungono nuovi diagrammi, vengono visualizzate sotto la cartella di diagrammi nella finestra Browser modello. Per rinominare un diagramma: selezionare il diagramma nella finestra Browser modello, fare clic una volta sul nome e digitare il nuovo nome.  È anche possibile fare clic sul nome del diagramma e scegliere **Rinomina**.

Il nome del diagramma viene visualizzato accanto al nome del file con estensione edmx, nell'editor di Visual Studio. Ad esempio Model1.edmx\[Diagram1\].

![DiagramName](~/ef6/media/diagramname.png)

Il contenuto di diagrammi (forma e colore di entità e associazioni) viene archiviato nel. file edmx.diagram. Per visualizzare questo file, selezionare Esplora soluzioni, quindi espandere il file con estensione edmx. 

![DiagramFiles](~/ef6/media/diagramfiles.png)

È consigliabile non modificare il. edmx.diagram file manualmente, il contenuto di questo file potrebbe essere sovrascritto da Entity Framework Designer.
 
## <a name="splitting-entities-and-associations-into-a-new-diagram"></a>Suddivisione di entità e associazioni in un nuovo diagramma

È possibile selezionare le entità nel diagramma esistente (tenere premuto MAIUSC per selezionare più entità). Scegliere il pulsante destro del mouse e selezionare **passa a diagramma nuovo**. Viene creato il nuovo diagramma e le entità selezionate e le relative associazioni vengono spostate al diagramma.

In alternativa, è possibile fare clic sulla cartella i diagrammi nella finestra Browser modello e selezionare **aggiungere nuovo diagramma.** È quindi possibile trascinare e rilasciare le entità che si trova sotto la cartella tipi di entità nella finestra Browser modello nell'area di progettazione.

È inoltre possibile tagliare o copiare le entità (utilizzando i tasti Ctrl + X o Ctrl + C) da un diagramma e incollare (usando il tasto Ctrl + V) in altro. Se il diagramma in cui si sta incollando un'entità già contiene un'entità con lo stesso nome, una nuova entità verrà creata e aggiunta al modello.  Ad esempio: diagramma 2 contiene l'entità Department. Incollare quindi un altro reparto sul diagramma 2. L'entità Department1 viene creato e aggiunto al modello concettuale.   

Per includere le entità correlate in un diagramma, fare clic tenendo premuto il tasto rick l'entità e selezionare **correlate includono**. In questo modo una copia delle entità correlate e le associazioni nel diagramma specificato.

## <a name="changing-the-color-of-entities"></a>Modifica del colore di entità

Oltre a suddividere un modello in più diagrammi, è anche possibile modificare i colori delle entità.

Per modificare il colore, selezionare un'entità (o più entità) nell'area di progettazione. Quindi, scegliere il pulsante destro del mouse e selezionare **proprietà**. Nella finestra Proprietà selezionare la **colore riempimento** proprietà. Specificare il colore usando un nome di colore validi (ad esempio, rosso) o un RGB valido (ad esempio, 255, 128, 128). 

![Colore](~/ef6/media/color.png)

## <a name="summary"></a>Riepilogo

In questo argomento è stato illustrato come suddividere un modello in più diagrammi e anche come specificare un colore diverso per un'entità utilizzando la finestra di progettazione di Entity Framework. 
