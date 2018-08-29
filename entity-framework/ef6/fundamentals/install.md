---
title: Ottenere Entity Framework - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.assetid: 122c38a2-f9e8-4ecc-9c72-a83bc9af7814
ms.openlocfilehash: 91b78e56f60edf7ebc8769b1c385f8547f63cd3d
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152388"
---
# <a name="get-entity-framework"></a>Ottenere Entity Framework
Entity Framework è costituito da strumenti di Entity Framework per Visual Studio e il Runtime di Entity Framework.

## <a name="ef-tools-for-visual-studio"></a>Entity Framework Tools per Visual Studio

Entity Framework Tools per Visual Studio includono Entity Framework Designer e la creazione guidata modello di Entity Framework e sono necessari per il database prima di tutto e modellare i flussi di lavoro prima. Gli strumenti di Entity Framework sono inclusi in tutte le versioni recenti di Visual Studio. Se si esegue un'installazione personalizzata di Visual Studio è necessario assicurarsi che l'elemento "Strumenti di Entity Framework 6" è selezionato uno scegliendo un carico di lavoro di cui è incluso o selezionandolo come un singolo componente.

Per alcune versioni precedenti di Visual Studio, sono disponibili come download aggiornato gli strumenti di Entity Framework. Visualizzare [versioni di Visual Studio](~/ef6/what-is-new/visual-studio.md) per indicazioni su come ottenere la versione più recente degli strumenti di Entity Framework disponibili per la versione di Visual Studio.

## <a name="ef-runtime"></a>Runtime di Entity Framework

La versione più recente di Entity Framework è disponibile come le [pacchetto EntityFramework NuGet](http://nuget.org/packages/EntityFramework/). Se non si ha familiarità con Gestione pacchetti NuGet, si consiglia di leggere il [Panoramica di NuGet](https://docs.microsoft.com/nuget/consume-packages/overview-and-workflow).

### <a name="installing-the-ef-nuget-package"></a>Installazione del pacchetto NuGet di Entity Framework

È possibile installare il pacchetto di EntityFramework facendo clic sui **riferimenti** cartella del progetto e selezionando **Gestisci pacchetti NuGet...**

![ManageNuGetPackages](~/ef6/media/managenugetpackages.png)

### <a name="installing-from-package-manager-console"></a>Installazione dalla Console di gestione pacchetti

In alternativa, è possibile installare EntityFramework eseguendo il comando seguente nel [Console di gestione pacchetti](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework
```

## <a name="installing-a-specific-version-of-ef"></a>Installare una versione specifica di Entity Framework

Da Entity Framework 4.1 e versioni successive, sono state rilasciate nuove versioni del runtime di Entity Framework come la [pacchetto NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/). Qualsiasi di queste versioni possono essere aggiunti a un progetto basato su .NET Framework eseguendo il comando seguente in Visual Studio [Console di gestione pacchetti](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):

``` powershell
Install-Package EntityFramework -Version <number>
```

Si noti che `<number>` rappresenta la specifica versione di Entity Framework per l'installazione. Ad esempio, 6.2.0 è la versione del numero di EF 6.2.   

Runtime di Entity Framework prima 4.1 facevano parte di .NET Framework e non può essere installato separatamente.

### <a name="installing-the-latest-preview"></a>Installare l'anteprima più recente

I metodi sopra indicati fornirà la versione più recente versione di Entity Framework è supportata completamente. Spesso esistono versioni non definitive di Entity Framework che mi piacerebbe poter provare e inviare commenti e suggerimenti su disponibili.

Per installare l'anteprima più recente di Entity Framework è possibile selezionare **Includi versione provvisoria** nella finestra Gestisci pacchetti NuGet. Se non le versioni non definitive sono disponibili si otterrà automaticamente la versione più recente versione completamente supportata di Entity Framework.

![IncludePreRelease](~/ef6/media/includeprerelease.png)

In alternativa, è possibile eseguire il comando seguente [Console di gestione pacchetti](http://docs.nuget.org/docs/start-here/using-the-package-manager-console).

``` powershell
Install-Package EntityFramework -Pre
```
