---
title: Ripristino di ObjectContext in Entity Framework Designer - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 36550569-a1de-47cb-ba6d-544794ffd500
ms.openlocfilehash: 3e436f0d9cf94720be9c424b327816438d571ae8
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488947"
---
# <a name="reverting-to-objectcontext-in-entity-framework-designer"></a>Ripristino di ObjectContext in Entity Framework Designer
Con la versione precedente di Entity Framework, un modello creato con la finestra di progettazione di Entity Framework genera classi di entità derivato da EntityObject e un contesto che deriva da ObjectContext.

È consigliabile partire EF4.1 dello swapping in un modello di generazione di codice che genera un contesto che deriva da DbContext e POCO classi di entità.

In Visual Studio 2012 si ottiene codice DbContext generato per impostazione predefinita per tutti i nuovi modelli creati con la finestra di progettazione di Entity Framework. I modelli esistenti continueranno a generare il codice di ObjectContext in base a meno che non si decide di scambiare al generatore di codice di base di DbContext.

## <a name="reverting-back-to-objectcontext-code-generation"></a>Eseguendo il ripristino di generazione del codice di ObjectContext

### <a name="1-disable-dbcontext-code-generation"></a>1. Disabilitare la generazione del codice DbContext

Generazione delle classi derivate DbContext e POCO è gestita da due file con estensione tt nel progetto, se si espande il file con estensione edmx in Esplora soluzioni verrà visualizzato questi file. Eliminare entrambi i file dal progetto.

![File di generazione di codice](~/ef6/media/codegenfiles.png)

Se si usa Visual Basic.NET è necessario selezionare la **Mostra tutti i file** pulsante per visualizzare i file annidati.

![Mostra tutti i file](~/ef6/media/showallfiles.png)

### <a name="2-re-enable-objectcontext-code-generation"></a>2. Abilitare nuovamente la generazione del codice di ObjectContext

Open è modellare in Entity Framework Designer, fare clic su una sezione vuota dell'area di progettazione e seleziona **proprietà**.

La modifica della finestra delle proprietà di **Code Generation Strategy** da **None** a **predefinito**.

![Strategia di generazione del codice](~/ef6/media/codegenstrategy.png)
