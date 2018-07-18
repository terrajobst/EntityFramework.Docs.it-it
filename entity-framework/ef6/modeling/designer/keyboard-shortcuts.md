---
title: Scelte rapide da tastiera della finestra di progettazione di Framework di entità - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 3c76cdd5-17c5-4c54-a6a5-cf21b974636b
caps.latest.revision: 3
ms.openlocfilehash: a4ed95647872949d9e34a6bb5d83d5d6119b0022
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122820"
---
# <a name="entity-framework-designer-keyboard-shortcuts"></a>Tasti di scelta rapida della finestra di progettazione di Entity Framework
Questa pagina fornisce un elenco di scelta rapida da tastiera disponibili nelle diverse schermate di Entity Framework Tools per Visual Studio.

## <a name="adonet-entity-data-model-wizard"></a>Procedura guidata ADO.NET Entity Data Model

### <a name="step-one-choose-model-contents"></a>Passaggio 1: Scegli contenuto Model

![WizardOne](~/ef6/media/wizardone.png)

| Collegamento  | Operazione                                                     | Note                                               |
|:----------|:-----------------------------------------------------------|:----------------------------------------------------|
| **ALT + n** | Passare alla schermata successiva                                        | Non è disponibile per tutte le selezioni di contenuto del modello. |
| **ALT + f** | Completamento procedura guidata                                              | Non è disponibile per tutte le selezioni di contenuto del modello. |
| **ALT + w** | Spostare lo stato attivo per il "ciò che il contenuto del modello?" riquadro. |                                                     |

### <a name="step-two-choose-your-connection"></a>Passaggio 2: Scegliere la connessione

![WizardTwo](~/ef6/media/wizardtwo.png)

| Collegamento  | Operazione                                                     | Note                                                   |
|:----------|:-----------------------------------------------------------|:--------------------------------------------------------|
| **ALT + n** | Passare alla schermata successiva                                        |                                                         |
| **ALT + p** | Passare alla schermata precedente                                    |                                                         |
| **ALT + w** | Spostare lo stato attivo per il "ciò che il contenuto del modello?" riquadro. |                                                         |
| **ALT + c** | Aprire la finestra "Proprietà connessione"                    | Consente la definizione di una nuova connessione al database. |
| **ALT + e** | Escludi i dati sensibili dalla stringa di connessione          |                                                         |
| **ALT + i** | Includere i dati sensibili nella stringa di connessione            |                                                         |
| **ALT + s** | Attivare o disattivare l'opzione "Salva le impostazioni di connessione nel file app. config" |                                                         |

### <a name="step-three-choose-your-version"></a>Il terzo passaggio: Scegliere la versione in uso

![WizardThree](~/ef6/media/wizardthree.png)

| Collegamento  | Operazione                                             | Note                                                                                 |
|:----------|:---------------------------------------------------|:--------------------------------------------------------------------------------------|
| **ALT + n** | Passare alla schermata successiva                                |                                                                                       |
| **ALT + p** | Passare alla schermata precedente                            |                                                                                       |
| **ALT + w** | Passare lo stato attivo alla selezione della versione di Entity Framework | Consente di specificare una versione diversa di Entity Framework per l'uso nel progetto. |

### <a name="step-four-choose-your-database-objects-and-settings"></a>Passaggio 4: Scegliere le impostazioni e oggetti di Database

![WizardFour](~/ef6/media/wizardfour.png)

| Collegamento  | Operazione                                                                                    | Note                                                               |
|:----------|:------------------------------------------------------------------------------------------|:--------------------------------------------------------------------|
| **ALT + f** | Completamento procedura guidata                                                                             |                                                                     |
| **ALT + p** | Passare alla schermata precedente                                                                   |                                                                     |
| **ALT + w** | Spostare lo stato attivo al riquadro di selezione degli oggetti di database                                            | Consente la specifica di oggetti di database per decodificare progettato.    |
| **ALT + s** | Attiva/disattiva la "Rendi plurali o singolari i nomi degli oggetti generati" opzione                       |                                                                     |
| **ALT + k** | Attivare o disattivare l'opzione "Includi colonne chiavi esterne nel modello di"                              | Non è disponibile per tutte le selezioni di contenuto del modello.                 |
| **ALT + i** | Attivare o disattivare l'opzione "Importa selezionate stored procedure e funzioni nel modello di entità" | Non è disponibile per tutte le selezioni di contenuto del modello.                 |
| **ALT + m** | Sposta lo stato attivo per il campo di testo "Namespace Model"                                        | Non è disponibile per tutte le selezioni di contenuto del modello.                 |
| **Barra spaziatrice** | Attiva/Disattiva selezione sull'elemento                                                               | Se l'elemento contiene elementi figlio, tutti gli elementi figlio saranno essere attivata/disattivati anche |
| **A sinistra**  | Albero figlio Comprimi                                                                       |                                                                     |
| **A destra** | Espandere l'albero figlio                                                                         |                                                                     |
| **Su**    | Passare all'elemento precedente nella struttura ad albero                                                      |                                                                     |
| **Giù**  | Passare all'elemento successivo nell'albero                                                          |                                                                     |

## <a name="ef-designer-surface"></a>Nell'area di progettazione di Entity Framework

![DesignerSurface](~/ef6/media/designersurface.png)

| Collegamento                                                                                | Operazione                      | Note                                                                                                                                                                                                                               |
|:----------------------------------------------------------------------------------------|:----------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Spazio o immettere**                                                                         | Attiva/Disattiva selezione            | Attiva o disattiva la selezione dell'oggetto con lo stato attivo.                                                                                                                                                                                         |
| **ESC**                                                                                 | Annulla selezione            | Consente di annullare la selezione corrente.                                                                                                                                                                                                      |
| **CTRL + A**                                                                            | Seleziona tutto                  | Seleziona tutte le forme nell'area di progettazione.                                                                                                                                                                                       |
| **Freccia su**                                                                            | Sposta su                     | Sposta selezionati entità di un incremento nella griglia. <br/> Se in un elenco, sposta il precedente sottocampo di pari livello.                                                                                                                            |
| **Freccia giù**                                                                          | Sposta giù                   | Sposta selezionati entità verso il basso un incremento nella griglia. <br/> Se in un elenco, sposta il sottocampo di pari livello successivo.                                                                                                                              |
| **Freccia SINISTRA**                                                                          | Lo spostamento verso sinistra                   | Sposta selezionati entità a sinistra un incremento nella griglia. <br/> Se in un elenco, sposta il precedente sottocampo di pari livello.                                                                                                                          |
| **Freccia DESTRA**                                                                         | Sposta a destra                  | Sposta selezionati incremento nella griglia a destra di una entità. <br/> Se in un elenco, sposta il sottocampo di pari livello successivo.                                                                                                                             |
| **MAIUSC + freccia sinistra**                                                                  | Forma di dimensione a sinistra             | Consente di ridurre la larghezza dell'entità selezionata da un incremento nella griglia.                                                                                                                                                                     |
| **MAIUSC + freccia destra**                                                                 | Forma di dimensione a destra            | Aumenta la larghezza dell'entità selezionata di un incremento nella griglia.                                                                                                                                                                   |
| **Home**                                                                                | Primo Peer                  | Sposta lo stato attivo e la selezione al primo oggetto nell'area di progettazione allo stesso livello di peer.                                                                                                                                         |
| **FINE**                                                                                 | Ultimo Peer                   | Sposta lo stato attivo e la selezione per l'ultimo oggetto nell'area di progettazione allo stesso livello di peer.                                                                                                                                          |
| **CTRL + Home**                                                                         | Primo Peer (messa a fuoco)          | Come primo peer, ma lo stato attivo si sposta anziché spostare lo stato attivo e la selezione.                                                                                                                                                          |
| **CTRL + fine**                                                                          | Ultimo Peer (messa a fuoco)           | Come ultimo il peering, ma si sposta lo stato attivo anziché spostare lo stato attivo e la selezione.                                                                                                                                                           |
| **TAB**                                                                                 | Peer successivo                   | Sposta lo stato attivo e la selezione all'oggetto successivo nella finestra di progettazione allo stesso livello di peer.                                                                                                                                          |
| **MAIUSC+TAB**                                                                           | Peer precedente               | Sposta lo stato attivo e la selezione per l'oggetto precedente nella finestra di progettazione allo stesso livello di peer.                                                                                                                                      |
| **Ctrl + Alt + Tab**                                                                        | Peer successivo (messa a fuoco)           | Come successivo peer, ma si sposta lo stato attivo anziché spostare lo stato attivo e la selezione.                                                                                                                                                           |
| **Alt + Ctrl + Maiusc + Tab**                                                                  | Peer precedente (lo stato attivo)       | Uguale a peer precedente, ma lo stato attivo si sposta anziché spostare lo stato attivo e la selezione.                                                                                                                                                       |
| **&lt;**                                                                                | Salire                      | Passa all'oggetto successivo nella superficie di progettazione di un livello superiore nella gerarchia. Se esistono forme di livello superiore nella gerarchia (vale a dire l'oggetto viene inserito direttamente nella finestra di progettazione), il diagramma è selezionato. |
| **&gt;**                                                                                | Discendono                     | Passa al successivo oggetto contenuto in area di progettazione un livello di sotto di esso nella gerarchia. Se non sono presenti alcun oggetto contenuto, questo è un no-op.                                                                              |
| **CTRL + &lt;**                                                                         | Salire (messa a fuoco)              | Uguale a salire comando, ma sposta lo stato attivo senza selezione.                                                                                                                                                                          |
| **CTRL + &gt;**                                                                         | Discendono (messa a fuoco)             | Uguale a discendono comando, ma sposta lo stato attivo senza selezione.                                                                                                                                                                         |
| **MAIUSC + End**                                                                         | Attenersi a connesso         | Da un'entità, viene spostato a un'entità a cui è connessa questa entità.                                                                                                                                                               |
| **CANC**                                                                                 | Eliminare                      | Eliminare un oggetto o il connettore del diagramma.                                                                                                                                                                                     |
| **Componenti aggiuntivi**                                                                                 | INS                      | Aggiunge una nuova proprietà a un'entità quando viene selezionata l'intestazione del raggruppamento "Proprietà scalari" o una proprietà stessa.                                                                                                           |
| **PG backup**                                                                               | Diagramma di scorrimento backup           | Scorre l'area di progettazione verso l'alto, in base a incrementi pari al 75% dell'altezza dell'area di progettazione attualmente visibile.                                                                                                                    |
| **Pg giù**                                                                             | Diagramma di scorrimento verso il basso         | L'area di progettazione scorre verso il basso.                                                                                                                                                                                                    |
| **MAIUSC + Pg giù**                                                                     | Diagramma di scorrimento a destra        | Scorre verso l'area di progettazione verso destra.                                                                                                                                                                                            |
| **MAIUSC + Pg backup**                                                                       | Diagramma di scorrimento a sinistra         | Scorre verso l'area di progettazione verso sinistra.                                                                                                                                                                                             |
| **F2**                                                                                  | Modalità di modifica             | Standard di tasti di scelta rapida per l'immissione di modalità di modifica per un controllo testo.                                                                                                                                                               |
| **MAIUSC+F10**                                                                         | Visualizza menu di scelta rapida       | Standard di tasti di scelta rapida per la visualizzazione di menu di scelta rapida di un elemento selezionato.                                                                                                                                                          |
| **CTRL + MAIUSC + Mouse a sinistra fare clic su**  <br/> **CTRL + MAIUSC + rotellina del mouse avanti**  | Zoom semantico In            | Ingrandisce l'area di visualizzazione del diagramma sotto il puntatore del mouse.                                                                                                                                                                 |
| **CTRL + MAIUSC + Mouse con il pulsante destro fare clic su** <br/> **CTRL + MAIUSC + rotellina del mouse con le versioni precedenti** | Zoom semantico Out           | Esegue lo zoom indietro dell'area di visualizzazione del diagramma sotto il puntatore del mouse. Il diagramma Centra nuovamente quando eseguire lo zoom indietro troppo lontano per il centro del diagramma corrente.                                                                          |
| **CTRL + MAIUSC + '+'** <br/> **CTRL + rotellina del mouse avanti**                        | Zoom avanti                     | Esegue lo zoom avanti al centro della visualizzazione del diagramma.                                                                                                                                                                                         |
| **CTRL + MAIUSC + '-'** <br/> **CTRL + rotellina del mouse con le versioni precedenti**                       | Zoom indietro                    | Esegue lo zoom indietro nell'area selezionato della visualizzazione Diagramma. Il diagramma Centra nuovamente quando eseguire lo zoom indietro troppo lontano per il centro del diagramma corrente.                                                                                            |
| **CTRL + MAIUSC e disegnare un rettangolo con il pulsante sinistro del mouse verso il basso**                  | Zoom Area                   | Ingrandisce centrata nell'area selezionata. Quando si tiene premuto la combinazione CTRL + MAIUSC, si noterà che assume la forma di lente di ingrandimento, che consente di definire l'area per eseguire lo zoom avanti.                        |
| **Chiave del Menu contesto + sto '**                                                              | Aprire la finestra Dettagli Mapping | Apre la finestra Dettagli Mapping per modificare i mapping per l'entità selezionata                                                                                                                                                               |

## <a name="mapping-details-window"></a>Finestra Dettagli Mapping

![MappingDetailsShortcuts](~/ef6/media/mappingdetailsshortcuts.png)

| Collegamento                  | Operazione         | Note                                                                                                                                 |
|:--------------------------|:---------------|:--------------------------------------------------------------------------------------------------------------------------------------|
| **TAB**                   | Cambiare contesto | Alterna l'area della finestra principale e la barra degli strumenti a sinistra                                                                     |
| **Tasti di direzione**            | Navigazione     | Spostarsi su e giù righe, o a destra e sinistra tra le colonne nell'area della finestra principale. Spostarsi tra i pulsanti sulla barra degli strumenti a sinistra. |
| **INVIO** <br/> **Barra spaziatrice** | Seleziona         | Seleziona un pulsante sulla barra degli strumenti a sinistra.                                                                                          |
| **ALT + freccia giù**      | Aprire l'elenco      | Un elenco a discesa se è selezionata una cella che contiene un elenco a discesa.                                                                     |
| **INVIO**                 | Elenco Select    | Seleziona un elemento in un elenco a discesa.                                                                                               |
| **ESC**                   | Chiudi elenco     | Chiude un menu a discesa.                                                                                                              |

## <a name="visual-studio-navigation"></a>Esplorazione di Visual Studio

Entity Framework fornisce inoltre una serie di azioni che possono avere tasti di scelta rapida personalizzati mappati (nessun tasti di scelta rapida viene eseguito il mapping per impostazione predefinita). Per creare questi tasti di scelta rapida personalizzati, fare clic sul menu Strumenti e opzioni.  In ambiente scegliere della tastiera.  Scorrere verso il basso l'elenco al centro fino a quando non è possibile selezionare il comando desiderato, immettere il collegamento nella casella di testo "Premere i tasti di scelta rapida" e fare clic su Assegna. I tasti di scelta rapida possibili sono i seguenti:

| Collegamento                                                                                       |
|:-----------------------------------------------------------------------------------------------|
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Add.ComplexProperty.ComplexTypes**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddCodeGenerationItem**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddFunctionImport**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.AddEnumType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Association**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexProperty**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.FunctionImport**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Inheritance**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.NavigationProperty**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ScalarProperty**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNewDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddtoDiagram**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Close**                                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Collapse**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ConverttoEnum**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.CollapseAll**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExpandAll**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExportasImage**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.LayoutDiagram**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Edit**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.EntityKey**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Expand**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.FunctionImportMapping**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GenerateDatabasefromModel**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GoToDefinition**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.ShowGrid**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.SnaptoGrid**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.IncludeRelated**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MappingDetails**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ModelBrowser**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveDiagramstoSeparateFile**              |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down5**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToBottom**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToTop**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up5**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MovetonewDiagram**                        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Open**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.MovetoNewComplexType**           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.Rename**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.RemovefromDiagram**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Rename**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayName**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayNameandType** |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.BaseType**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Property**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Subtype**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAll**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAssociation**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinModelBrowser**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.StoredProcedureMapping**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.TableMapping**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.UpdateModelfromDatabase**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Validate**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.10**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.100**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.125**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.150**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.200**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.25**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.300**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.33**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.400**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.50**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.66**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.75**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.Custom**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomIn**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomOut**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomtoFit**                          |
| **View.EntityDataModelBrowser**                                                                |
| **View.EntityDataModelMappingDetails**                                                         |
