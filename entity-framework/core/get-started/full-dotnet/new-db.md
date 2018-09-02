---
title: Introduzione a .NET Framework - Nuovo database - EF Core
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 66d9011a5978fc3d8253a963bdb9df27848e9ff9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997587"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>Introduzione a EF Core in .NET Framework con un nuovo database

In questa esercitazione verrà creata un'applicazione console che esegue l'accesso ai dati di base in un database di Microsoft SQL Server usando Entity Framework. Vengono usate le migrazioni per creare il database da un modello.

[Visualizzare l'esempio di questo articolo su GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).

## <a name="prerequisites"></a>Prerequisiti

* [Visual Studio 2017 versione 15.7 o successive](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a>Creare un nuovo progetto

* Aprire Visual Studio 2017

* **File > Nuovo > Progetto**

* Scegliere **Installati > Visual C# > Desktop Windows** dal menu a sinistra

* Selezionare il modello di progetto **App console (.NET Framework)**

* Assicurarsi che il progetto sia destinato a **.NET Framework 4.6.1** o versione successiva

* Denominare il progetto *ConsoleApp.NewDb* e fare clic su **OK**

## <a name="install-entity-framework"></a>Installare Entity Framework

Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione. Questa esercitazione usa SQL Server. Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).

* Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti

* Eseguire `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Più avanti in questa esercitazione si useranno alcuni degli strumenti in Entity Framework Tools per gestire il database. Installare quindi anche il pacchetto degli strumenti.

* Eseguire `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="create-the-model"></a>Creare il modello

È ora possibile definire un contesto e le classi di entità che costituiscono il modello.

* **Progetto > Aggiungi classe**

* Immettere *Model.cs* come nome e fare clic su **OK**

* Sostituire tutti i contenuti del file con il codice seguente

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> In una vera applicazione ogni classe verrebbe inserita in un file separato e la stringa di connessione in un file di configurazione o in una variabile di ambiente. Per ragioni di semplicità, in questa esercitazione viene inserito tutto in un solo file.

## <a name="create-the-database"></a>Creare il database

Dopo avere creato un modello, è possibile usare le migrazioni per creare un database.

* **Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**

* Eseguire `Add-Migration InitialCreate` per effettuare lo scaffolding di una migrazione per creare il set iniziale di tabelle per il modello.

* Eseguire `Update-Database` per applicare la nuova migrazione al database. Poiché il database non esiste ancora, verrà creato automaticamente prima di applicare la migrazione.

> [!TIP]  
> Se si apportano modifiche al modello, è possibile usare il comando `Add-Migration` per eseguire lo scaffolding di una nuova migrazione per apportare le modifiche dello schema corrispondenti nel database. Dopo avere controllato il codice di scaffolding e avere apportato eventuali modifiche necessarie, è possibile usare il comando `Update-Database` per applicare le modifiche al database.
>
> EF usa una tabella `__EFMigrationsHistory` nel database per tenere traccia delle migrazioni che sono già state applicate al database.

## <a name="use-the-model"></a>Usare il modello

È ora possibile usare il modello per eseguire l'accesso ai dati.

* Aprire *Program.cs*

* Sostituire tutti i contenuti del file con il codice seguente

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* **Debug > Avvia senza eseguire debug**

  Si può notare che un blog viene salvato nel database e quindi che i dettagli di tutti i blog vengono visualizzati nella console.

  ![immagine](_static/output-new-db.png)

## <a name="additional-resources"></a>Risorse aggiuntive

* [EF Core in .NET Framework con un database esistente](xref:core/get-started/full-dotnet/existing-db)
* [EF Core in .NET Core con un nuovo database - SQLite](xref:core/get-started/netcore/new-db-sqlite): esercitazione su EF per la console multipiattaforma.
