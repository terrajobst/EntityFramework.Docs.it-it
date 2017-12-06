---
title: Porting da EF6 a EF Core - il Porting di un modello basato su EDMX
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 1880d5ce49d4c9cb891d0e8fb230770bb44799e5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/23/2017
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Porting di un modello basato su EDMX EF6 a EF Core

Core EF non supporta il formato del file EDMX per i modelli. L'opzione migliore per questi modelli, la porta consiste nel generare un nuovo modello basato su codice dal database dell'applicazione.

## <a name="install-ef-core-nuget-packages"></a>Installare i pacchetti NuGet Core EF

Installare il `Microsoft.EntityFrameworkCore.Tools` pacchetto NuGet.

## <a name="regenerate-the-model"></a>Rigenerare il modello

È ora possibile utilizzare la funzionalità di decodifica per creare un modello basato sul database esistente.

Eseguire il comando seguente nella Console di gestione pacchetti (Strumenti -> Gestione pacchetti NuGet -> Console di gestione pacchetti). Vedere [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) per opzioni di comando per eseguire lo scaffolding di un subset di tabelle e così via.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Ad esempio, ecco il comando per eseguire lo scaffolding di un modello dal database di blog sull'istanza del database locale di SQL Server.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Rimuovere il modello EF6

Rimuovere questo punto il modello EF6 dall'applicazione.

È possibile lasciare il pacchetto NuGet EF6 (EntityFramework) installato, come EF Core ed EF6 possono essere utilizzati side-by-side nella stessa applicazione. Tuttavia, se non si intende utilizzare EF6 nelle aree dell'applicazione, quindi disinstallare il pacchetto consente di assegnare gli errori di compilazione in parti di codice che richiedono attenzione.

## <a name="update-your-code"></a>Aggiornare il codice

A questo punto, si tratta di errori di compilazione di indirizzamento e revisione codice per vedere se le modifiche di comportamento tra EF6 ed EF Core è necessario tenere conto.

## <a name="test-the-port"></a>La porta di test

Il fatto che l'applicazione viene compilata, non significa che correttamente eseguito il porting a EF Core. È necessario testare tutte le aree dell'applicazione per assicurarsi che nessuna delle modifiche del comportamento hanno effetti negativi sull'applicazione.

> [!TIP]
> Vedere [Introduzione a Entity Framework Core in ASP.NET Core con un Database esistente](xref:core/get-started/aspnetcore/existing-db) per un riferimento aggiuntivo sulla modalità di utilizzo con un database esistente, 
