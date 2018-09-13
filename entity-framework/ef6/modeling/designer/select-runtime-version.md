---
title: Se si seleziona versione di Runtime di Entity Framework per i modelli della finestra di progettazione di Entity Framework - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 7ace90a6-46f8-4f55-a88c-7cad9620085c
ms.openlocfilehash: 40ad05c1f015e6a51150d04eee8d9aa581d72c33
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488491"
---
# <a name="selecting-entity-framework-runtime-version-for-ef-designer-models"></a>Se si seleziona versione di Runtime di Entity Framework per i modelli di progettazione di Entity Framework
> [!NOTE]
> **Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.

A partire da Entity Framework 6 la schermata seguente è stata aggiunta alla finestra di progettazione di Entity Framework consente di selezionare la versione del runtime di destinazione durante la creazione di un modello. Verrà visualizzata la schermata quando la versione più recente di Entity Framework non è già installata nel progetto. Se la versione più recente è già installata verrà usato solo per impostazione predefinita.

![Schermata](~/ef6/media/screen.png)


## <a name="targeting-ef6x"></a>Targeting EF6.x

È possibile scegliere EF6 nella schermata "Scegliere Version" per aggiungere il runtime di Entity Framework 6 per il progetto. Dopo aver aggiunto EF6, sarà più visualizzata questa schermata nel progetto corrente.

EF6 verrà disabilitata se si dispone già di una versione precedente di Entity Framework installato (poiché non è possibile destinazione più versioni del runtime dallo stesso progetto). Se l'opzione di Entity Framework 6 non è abilitata in questo caso, seguire questi passaggi per aggiornare il progetto a EF6:

1.  Pulsante destro del mouse sul progetto in Esplora soluzioni e selezionare **Gestisci pacchetti NuGet...**
2.  Selezionare **aggiornamenti**
3.  Selezionare **EntityFramework** (assicurarsi che sta per aggiornarlo alla versione desiderata)
4.  Fare clic su **Update**

 

## <a name="targeting-ef5x"></a>Targeting EF5.x

È possibile scegliere EF5 dalla schermata "Scegliere Version" per aggiungere il runtime EF5 al progetto. Dopo aver aggiunto EF5, si verrà comunque visualizzata la schermata con l'opzione di EF6 disabilitato.

Se si dispone di una versione EF4.x del runtime già installata è verrà visualizzato tale versione di Entity Framework elencati nella schermata anziché EF5. In questo caso è possibile aggiornare a EF5 usando la procedura seguente:

1.  Selezionare **- Tools&gt; Gestione pacchetti libreria -&gt; Console di gestione pacchetti**
2.  Eseguire **Install-Package EntityFramework-versione 5.0.0**

 

## <a name="targeting-ef4x"></a>Targeting EF4.x

È possibile installare il runtime EF4.x al progetto usando la procedura seguente:

1.  Selezionare **- Tools&gt; Gestione pacchetti libreria -&gt; Console di gestione pacchetti**
2.  Eseguire **Install-Package EntityFramework-versione 4.3.0**
