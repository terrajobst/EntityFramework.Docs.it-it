---
title: Relazioni - finestra di progettazione di Entity Framework - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: 72efe76956c930a787449e6cce453ab0317adc7c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994648"
---
# <a name="relationships---ef-designer"></a>Relazioni - finestra di progettazione di Entity Framework
> [!NOTE]
> Questa pagina fornisce informazioni sulla configurazione di relazioni nel modello utilizzando la finestra di progettazione di Entity Framework. Per informazioni generali sulle relazioni in Entity Framework e su come accedere e modificare dati tramite relazioni, vedere [relazioni di & proprietà di navigazione](~/ef6/fundamentals/relationships.md).

Le associazioni definiscono le relazioni tra i tipi di entità in un modello. Questo argomento illustra come eseguire il mapping delle associazioni con Entity Framework Designer (Entity Framework Designer). L'immagine seguente mostra le finestre principali che vengono usate quando si lavora con la finestra di progettazione di Entity Framework.

![EFDesigner](~/ef6/media/efdesigner.png)

> [!NOTE]
> Quando si compila il modello concettuale, avvisi relativi a entità non mappata e associazioni vengano visualizzati nell'elenco errori. È possibile ignorare questi avvisi perché dopo aver scelto di generare il database dal modello, gli errori non verranno più visualizzato.

## <a name="associations-overview"></a>Panoramica delle associazioni

Quando si progetta il modello utilizzando la finestra di progettazione di Entity Framework, un file con estensione edmx rappresenta il modello. Nel file con estensione edmx, un' **Association** elemento definisce una relazione tra due tipi di entità. Un'associazione deve specificare i tipi di entità coinvolti nella relazione e il possibile numero di tipi di entità a ogni entità finale della relazione, che è noto come molteplicità. La molteplicità di un'entità finale dell'associazione può avere un valore di uno (1), zero o uno (0..1) o molti (\*). Queste informazioni vengono specificate in due figlio **End** elementi.

In fase di esecuzione, istanze del tipo di entità in un'entità finale di un'associazione accessibili attraverso proprietà di navigazione o chiavi esterne (se si sceglie di esporre le chiavi esterne nelle entità). Con le chiavi esterne esposte, la relazione tra l'entità viene gestita con un **ReferentialConstraint** elemento (un elemento figlio delle **Association** elemento). È consigliabile esporre sempre le chiavi esterne per le relazioni nelle entità.

> [!NOTE]
> In molti-a-molti (\*:\*) non è possibile aggiungere chiavi esterne alle entità. In un \*:\* relazione, le informazioni di associazione viene gestito con un oggetto indipendente.

Per informazioni sugli elementi CSDL (**ReferentialConstraint**, **Association**e così via) vedere il [specifica CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).

## <a name="create-and-delete-associations"></a>Creare ed eliminare le associazioni

Creazione di un'associazione con gli aggiornamenti di Entity Framework Designer il contenuto del modello del file con estensione edmx. Dopo aver creato un'associazione, è necessario creare i mapping per l'associazione (descritti più avanti in questo argomento).

> [!NOTE]
> In questa sezione si presuppone che già stato aggiunto l'entità che si desidera creare un'associazione tra il modello.

### <a name="to-create-an-association"></a>Per creare un'associazione

1.  Fare doppio clic su un'area vuota dell'area di progettazione, scegliere **Aggiungi nuovo**e selezionare **Association...** .
2.  Specificare le impostazioni per l'associazione nel **Aggiungi associazione** finestra di dialogo.

    ![AddAssociation](~/ef6/media/addassociation.png)

    > [!NOTE]
    > È possibile scegliere di non aggiungere le proprietà di navigazione o proprietà di chiave esterna per le entità finali dell'associazione deselezionando le * * proprietà di navigazione * * e * * aggiungere proprietà di chiave esterna per il &lt;nome tipo di entità&gt; entità * * caselle di controllo. Se si aggiunge una sola proprietà di navigazione, l'associazione sarà attraversabile in una sola direzione. Se non si aggiungono proprietà di navigazione, è necessario scegliere di aggiungere proprietà di chiave esterna per accedere alle entità finali dell'associazione.
    
3.  Fare clic su **OK**.

### <a name="to-delete-an-association"></a>Per eliminare un'associazione

Per eliminare, eseguire un associazione una delle operazioni seguenti:

-   Fare doppio clic sull'associazione in Entity Framework Designer e selezionare **Elimina**.

- OR:

-   Selezionare una o più associazioni e premere il tasto CANC.

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a>Includere proprietà di chiave esterna nelle entità (vincoli referenziali)

È consigliabile esporre sempre le chiavi esterne per le relazioni nelle entità. Entity Framework Usa un vincolo referenziale per identificare una proprietà funge da chiave esterna per una relazione.

Se è stata selezionata la ***aggiungere le proprietà di chiave esterna per il &lt;nome tipo di entità&gt; entità*** casella di controllo quando si crea una relazione, il vincolo referenziale è stato aggiunto automaticamente.

Quando si usa Entity Framework Designer per aggiungere o modificare un vincolo referenziale, la finestra di progettazione di Entity Framework aggiunge o modifica una **ReferentialConstraint** elemento nel contenuto CSDL del file con estensione edmx.

-   Fare doppio clic sull'associazione che si desidera modificare.
    Il **vincolo referenziale** verrà visualizzata la finestra di dialogo.
-   Dal **Principal** elenco a discesa selezionare l'entità principale nel vincolo referenziale.
    Le proprietà di chiave dell'entità vengono aggiunti per il **chiave dell'entità** elenco nella finestra di dialogo.
-   Dal **dipendenti** elenco a discesa selezionare l'entità dipendente nel vincolo referenziale.
-   Per ogni chiave dell'entità che dispone di una chiave dipendente, selezionare una chiave dipendente corrispondente dagli elenchi a discesa scegliere il **chiave dipendente** colonna.

    ![RefConstraint](~/ef6/media/refconstraint.png)

-   Fare clic su **OK**.

## <a name="create-and-edit-association-mappings"></a>Creare e modificare i mapping di associazione

È possibile specificare come un'associazione esegue il mapping al database di **Dettagli Mapping** finestra della finestra di progettazione di Entity Framework.

> [!NOTE]
> È possibile mappare solo i dettagli per le associazioni che non dispongono di un vincolo referenziale specificato. Se viene specificato un vincolo referenziale una proprietà di chiave esterna è incluso nell'entità e i dettagli di Mapping è possibile usare per l'entità al quale colonna chiave esterna viene eseguito il mapping al controllo.

### <a name="create-an-association-mapping"></a>Creare un mapping di associazione

-   Fare doppio clic su un'associazione nell'area di progettazione e seleziona **Mapping tabella**.
    Ciò consente di visualizzare il mapping di associazione nel **ulteriori dettagli sul Mapping** finestra.
-   Fare clic su **aggiungere una tabella o vista**.
    Verrà visualizzato un elenco a discesa che include tutte le tabelle presenti nel modello di archiviazione.
-   Selezionare la tabella alla quale l'associazione verrà mappata.
    Il **Dettagli Mapping** finestra vengono visualizzate entrambe le estremità dell'associazione e le proprietà chiave per il tipo di entità in ogni **End**.
-   Per ogni proprietà chiave, scegliere il **colonna** campo e selezionare la colonna a cui verrà eseguito il mapping della proprietà.

    ![MappingDetails4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a>Modifica un mapping di associazione

-   Fare doppio clic su un'associazione nell'area di progettazione e seleziona **Mapping tabella**.
    Ciò consente di visualizzare il mapping di associazione nel **ulteriori dettagli sul Mapping** finestra.
-   Fare clic su **esegue il mapping a &lt;nome tabella&gt;**.
    Verrà visualizzato un elenco a discesa che include tutte le tabelle presenti nel modello di archiviazione.
-   Selezionare la tabella alla quale l'associazione verrà mappata.
    Il **Dettagli Mapping** finestra vengono visualizzate entrambe le estremità dell'associazione e le proprietà chiave per il tipo di entità in ciascuna estremità.
-   Per ogni proprietà chiave, scegliere il **colonna** campo e selezionare la colonna a cui verrà eseguito il mapping della proprietà.

## <a name="edit-and-delete-navigation-properties"></a>Modifica e le proprietà di navigazione Delete

Proprietà di navigazione sono proprietà di collegamento utilizzate per individuare le entità finali di un'associazione in un modello. È possibile creare le proprietà di navigazione quando si crea un'associazione tra due tipi di entità.

#### <a name="to-edit-navigation-properties"></a>Per modificare le proprietà di navigazione

-   Selezionare una proprietà di navigazione nell'area di progettazione di Entity Framework.
    In Visual Studio vengono visualizzate informazioni sulla proprietà di navigazione **proprietà** finestra.
-   Modificare le impostazioni delle proprietà nel **proprietà** finestra.

#### <a name="to-delete-navigation-properties"></a>Per eliminare le proprietà di navigazione

-   Se le chiavi esterne non sono esposte nei tipi di entità nel modello concettuale, è possibile che l'eliminazione di una proprietà di navigazione renda l'associazione corrispondente attraversabile in un'unica direzione o non attraversabile.
-   Fare doppio clic su una proprietà di navigazione della finestra di progettazione di Entity Framework e selezionare **Elimina**.
