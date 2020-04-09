---
title: Conversione da EF6 a EF Core - Porting di un modello basato su EDMX - EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2020
ms.locfileid: "78416923"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Conversione di un modello basato su EF6 EDMX in EF Core

EF Core non supporta il formato di file EDMX per i modelli. L'opzione migliore per eseguire il porting di questi modelli consiste nel generare un nuovo modello basato su codice dal database per l'applicazione.

## <a name="install-ef-core-nuget-packages"></a>Installare i pacchetti NuGet di EF CoreInstall EF Core NuGet packages

Installare il pacchetto NuGet `Microsoft.EntityFrameworkCore.Tools`.

## <a name="regenerate-the-model"></a>Rigenerare il modello

È ora possibile utilizzare la funzionalità di decodificazione per creare un modello basato sul database esistente.

Eseguire il comando seguente nella console di gestione pacchetti (Strumenti –> Gestione pacchetti NuGet –> console di gestione pacchetti). Vedere [Console di gestione pacchetti (Visual Studio)](../../core/miscellaneous/cli/powershell.md) per le opzioni di comando per eseguire lo scaffolding di un subset di tabelle e così via.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Ad esempio, di seguito è riportato il comando per eseguire lo scaffolding di un modello dal database Blogging nell'istanza del database locale di SQL Server.For example, here is the command to scaffold a model from the Blogging database on your SQL Server LocalDB instance.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Rimuovere il modello EF6Remove EF6 model

È ora necessario rimuovere il modello EF6 dall'applicazione.

È bene lasciare installato il pacchetto EF6 NuGet (EntityFramework), come Entity Core ed EF6 possono essere utilizzati side-by-side nella stessa applicazione. Tuttavia, se non si intende utilizzare EF6 in tutte le aree dell'applicazione, quindi la disinstallazione del pacchetto contribuirà a dare errori di compilazione su parti di codice che richiedono attenzione.

## <a name="update-your-code"></a>Aggiornare il codice

A questo punto, è una questione di risolvere gli errori di compilazione e la revisione del codice per vedere se il comportamento cambia tra EF6 ed EF Core avrà un impatto.

## <a name="test-the-port"></a>Testare la porta

Solo perché l'applicazione viene compilata, non significa che è stato eseguito correttamente il porting a EF Core.Just because your application compiles, does not mean it is successfully ported to EF Core. Sarà necessario testare tutte le aree dell'applicazione per assicurarsi che nessuna delle modifiche di comportamento abbia influito negativamente sull'applicazione.
