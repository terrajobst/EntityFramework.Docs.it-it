---
title: Conversione da EF6 a EF Core - conversione di un modello basato su EDMX
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: 2c3336ac675a830566001a0ddb3777839f52db18
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997411"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Conversione di un modello basato su EDMX EF6 a EF Core

EF Core non supporta il formato di file EDMX per modelli. L'opzione migliore per questi modelli, la porta è generare un nuovo modello basato su codice dal database per l'applicazione.

## <a name="install-ef-core-nuget-packages"></a>Installare i pacchetti NuGet di Entity Framework Core

Installare il `Microsoft.EntityFrameworkCore.Tools` pacchetto NuGet.

## <a name="regenerate-the-model"></a>Rigenerare il modello

È ora possibile usare la funzionalità di reverse engineering per creare un modello basato sul database esistente.

Eseguire il comando seguente nella Console di gestione pacchetti (Strumenti -> Gestione pacchetti NuGet -> Console di gestione pacchetti). Visualizzare [Console di gestione pacchetti (Visual Studio)](../../core/miscellaneous/cli/powershell.md) alle opzioni di comando per eseguire lo scaffolding di un subset di tabelle e così via.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Ad esempio, ecco il comando per eseguire lo scaffolding di un modello dal database Blogging nell'istanza di LocalDB di SQL Server.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Rimuovi modello EF6

A questo punto si annullerebbe il modello di Entity Framework 6 dall'applicazione.

È possibile lasciare il pacchetto NuGet di Entity Framework 6 (Entity Framework) installato, come EF Core ed EF6 può essere utilizzata side-by-side nella stessa applicazione. Tuttavia, se non si intende usare EF6 in tutte le aree dell'applicazione, quindi la disinstallazione del pacchetto documentazione offre errori di compilazione in parti di codice che richiedono attenzione.

## <a name="update-your-code"></a>Aggiornare il codice

A questo punto, è una questione di indirizzamento degli errori di compilazione e revisione codice per vedere se le modifiche di comportamento tra EF6 ed EF Core possibili conseguenze per te.

## <a name="test-the-port"></a>La porta di test

Il fatto che l'applicazione viene compilata, non significa che è al termine del trasferimento in EF Core. È necessario testare tutte le aree dell'applicazione per garantire che nessuna delle modifiche di comportamento hanno effetti negativi a causa dell'applicazione.

> [!TIP]
> Visualizzare [Introduzione a EF Core in ASP.NET Core con un Database esistente](xref:core/get-started/aspnetcore/existing-db) per un riferimento aggiuntivo relativo all'utilizzo con un database esistente, 
