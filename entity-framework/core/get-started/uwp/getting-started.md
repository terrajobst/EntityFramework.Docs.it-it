---
title: Guida introduttiva alla piattaforma UWP - Nuovo database - EF Core
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 456967a0dc981053919064a19cc9c98bf7309865
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022376"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Introduzione a EF Core nella piattaforma UWP (Universal Windows Platform) con un nuovo database

In questa esercitazione si compila un'applicazione UWP (Universal Windows Platform) che esegue l'accesso ai dati di base in un database SQLite locale usando Entity Framework Core.

[Visualizzare l'esempio di questo articolo su GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).

## <a name="prerequisites"></a>Prerequisiti

* [Windows 10 Fall Creators Update (10.0; Build 16299) o successive](https://support.microsoft.com/help/4027667/windows-update-windows-10).

* [Visual Studio 2017 versione 15.7 o successiva](https://www.visualstudio.com/downloads/) con il carico di lavoro **Sviluppo per la piattaforma UWP (Universal Windows Platform)**.

* [.NET Core 2.1 SDK o versione successiva](https://www.microsoft.com/net/core).

> [!IMPORTANT]
> Questa esercitazione usa i comandi per le [migrazioni](xref:core/managing-schemas/migrations/index) di Entity Framework Core per creare e aggiornare lo schema del database.
> Questi comandi non funzionano direttamente con i progetti UWP.
> Per questo motivo, il modello di dati dell'applicazione viene inserito in un progetto di libreria condivisa e viene usata un'applicazione console .NET Core separata per eseguire i comandi.

## <a name="create-a-library-project-to-hold-the-data-model"></a>Creare un progetto di libreria per contenere il modello di dati

* Aprire Visual Studio

* **File > Nuovo > Progetto**

* Scegliere **Installati > Visual C# > .NET Standard** dal menu a sinistra.

* Selezionare il modello **Libreria di classi (.NET Standard)**.

* Assegnare al progetto il nome *Blogging.Model*.

* Assegnare alla soluzione il nome *Blogging*.

* Fare clic su **OK**.

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a>Installare il runtime di Entity Framework Core nel progetto del modello di dati

Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione. Questa esercitazione usa SQLite. Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).

* **Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**.

* Assicurarsi che il progetto di libreria *Blogging.Model* sia selezionato come **progetto predefinito** nella Console di Gestione pacchetti.

* Eseguire `Install-Package Microsoft.EntityFrameworkCore.Sqlite`

## <a name="create-the-data-model"></a>Creare il modello di dati

È ora possibile definire l'oggetto *DbContext* e le classi di entità che costituiscono il modello.

* Eliminare *Class1.cs*.

* Creare *Model.cs* con il codice seguente:

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a>Creare un nuovo progetto console per eseguire i comandi delle migrazioni

* In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla soluzione, quindi scegliere **Aggiungi > Nuovo progetto**.

* Scegliere **Installati > Visual C# > .NET Core** dal menu a sinistra.

* Selezionare il modello di progetto **App Console (.NET Core)**.

* Denominare il progetto *Blogging.Migrations.Startup* e fare clic su **OK**.

* Aggiungere un riferimento progetto dal progetto *Blogging.Migrations.Startup* al progetto *Blogging.Model*.

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a>Installare gli strumenti di Entity Framework Core nel progetto di avvio delle migrazioni

Per abilitare i comandi delle migrazioni di EF Core nella Console di Gestione pacchetti, installare il pacchetto di strumenti di EF Core nell'applicazione console.

* **Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**

* Eseguire `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`

## <a name="create-the-initial-migration"></a>Creare la migrazione iniziale

 Creare la migrazione iniziale, specificando l'applicazione console come progetto di avvio.

* Eseguire `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`

Questo comando esegue lo scaffolding di una migrazione che crea il set iniziale di tabelle del database per il modello di dati.

## <a name="create-the-uwp-project"></a>Creare il progetto UWP

* In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla soluzione, quindi scegliere **Aggiungi > Nuovo progetto**.

* Scegliere **Installati > Visual C# > Universale di Windows** dal menu a sinistra.

* Selezionare il modello di progetto **App vuota (Universale di Windows)**.

* Assegnare al progetto il nome *Blogging.UWP* e fare clic su **OK**

> [!IMPORTANT]
> Impostare le versioni di destinazione e minima almeno su **Windows 10 Fall Creators Update (10.0; build 16299.0)**.
> Le versioni precedenti di Windows 10 non supportano .NET Standard 2.0, che è necessario per Entity Framework Core.

## <a name="add-code-to-create-the-database-on-application-startup"></a>Aggiungere il codice per creare il database all'avvio dell'applicazione

Poiché si vuole creare il database nel dispositivo in cui viene eseguita l'app, aggiungere codice per applicare eventuali migrazioni in sospeso al database locale all'avvio dell'applicazione. In questo modo il database locale viene creato alla prima esecuzione dell'app.

* Aggiungere un riferimento progetto dal progetto *Blogging.UWP* al progetto *Blogging.Model*.

* Aprire *App.xaml.cs*.

* Aggiungere il codice evidenziato per applicare eventuali migrazioni in sospeso.

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> Se si apportano modifiche al modello, usare il comando `Add-Migration` per eseguire lo scaffolding di una nuova migrazione e applicare le modifiche corrispondenti al database. Eventuali migrazioni in sospeso verranno applicate al database locale in ogni dispositivo all'avvio dell'applicazione.
>
>EF Core usa una tabella `__EFMigrationsHistory` nel database per tenere traccia delle migrazioni che sono già state applicate al database.

## <a name="use-the-data-model"></a>Usare il modello di dati

È ora possibile usare EF Core per eseguire l'accesso ai dati.

* Aprire *MainPage.xaml*.

* Aggiungere il gestore di caricamento pagina e il contenuto dell'interfaccia utente evidenziati sotto

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

Ora aggiungere il codice per associare l'interfaccia utente al database

* Aprire *MainPage.xaml.cs*.

* Aggiungere il codice evidenziato nel listato seguente:

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

È ora possibile eseguire l'applicazione per vederla in azione.

* In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto *Blogging.UWP* e selezionare **Distribuisci**.

* Impostare *Blogging.UWP* come progetto di avvio.

* **Debug > Avvia senza eseguire debug**

  L'app viene compilata ed eseguita.

* Immettere un URL e fare clic sul pulsante **Aggiungi**

  ![immagine](_static/create.png)

  ![immagine](_static/list.png)

  È ora disponibile una semplice app UWP che esegue Entity Framework Core.

## <a name="next-steps"></a>Passaggi successivi

Per le informazioni sulle prestazioni e la compatibilità che è necessario conoscere quando si usa Entity Framework Core con UWP, vedere [Implementazioni di .NET supportate da Entity Framework Core](../../platforms/index.md#universal-windows-platform).

Per altre informazioni sulle funzionalità di Entity Framework Core, vedere gli altri articoli di questa documentazione.
