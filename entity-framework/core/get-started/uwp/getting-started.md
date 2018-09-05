---
title: Guida introduttiva alla piattaforma UWP - Nuovo database - EF Core
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996910"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Introduzione a EF Core nella piattaforma UWP (Universal Windows Platform) con un nuovo database

In questa esercitazione si compila un'applicazione UWP (Universal Windows Platform) che esegue l'accesso ai dati di base in un database SQLite locale usando Entity Framework Core.

[Visualizzare l'esempio di questo articolo su GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).

## <a name="prerequisites"></a>Prerequisiti

* [Windows 10 Fall Creators Update (10.0; Build 16299) o successive](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).

* [Visual Studio 2017 versione 15.7 o successiva](https://www.visualstudio.com/downloads/) con il carico di lavoro **Sviluppo per la piattaforma UWP (Universal Windows Platform)**.

* [.NET Core 2.1 SDK o versione successiva](https://www.microsoft.com/net/core).

## <a name="create-a-model-project"></a>Creare un progetto modello

> [!IMPORTANT]
> A causa delle limitazioni nell'interazione degli strumenti di .NET Core con i progetti della piattaforma UWP, il modello deve essere inserito in un progetto non UWP per poter eseguire i comandi delle migrazioni nella **console di Gestione pacchetti**.

* Aprire Visual Studio

* **File > Nuovo > Progetto**

* Scegliere **Installati > Visual C# > .NET Standard** dal menu a sinistra.

* Selezionare il modello **Libreria di classi (.NET Standard)**.

* Assegnare al progetto il nome *Blogging.Model*.

* Assegnare alla soluzione il nome *Blogging*.

* Fare clic su **OK**.

## <a name="install-entity-framework-core"></a>Installare Entity Framework Core

Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione. Questa esercitazione usa SQLite. Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).

* **Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**.

* Eseguire `Install-Package Microsoft.EntityFrameworkCore.Sqlite`

Più avanti in questa esercitazione si usano alcuni strumenti di Entity Framework Core per gestire il database. Installare quindi anche il pacchetto degli strumenti.

* Eseguire `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="create-the-model"></a>Creare il modello

È ora possibile definire un contesto e le classi di entità che costituiscono il modello.

* Eliminare *Class1.cs*.

* Creare *Model.cs* con il codice seguente:

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>Creare un nuovo progetto UWP

* In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla soluzione, quindi scegliere **Aggiungi > Nuovo progetto**.

* Scegliere **Installati > Visual C# > Universale di Windows** dal menu a sinistra.

* Selezionare il modello di progetto **App vuota (Universale di Windows)**.

* Assegnare al progetto il nome *Blogging.UWP* e fare clic su **OK**

* Impostare le versioni di destinazione e minima almeno su **Windows 10 Fall Creators Update (10.0; build 16299.0)**.

## <a name="create-the-initial-migration"></a>Creare la migrazione iniziale

Ora che è presente un modello, è possibile configurare l'app in modo che crei un database alla prima esecuzione. In questa sezione si crea la migrazione iniziale. Nella sezione seguente si aggiunge il codice che applica questa migrazione all'avvio dell'app.

Gli strumenti di migrazione richiedono un progetto di avvio non UWP. Per iniziare, creare tale progetto.

* In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla soluzione, quindi scegliere **Aggiungi > Nuovo progetto**.

* Scegliere **Installati > Visual C# > .NET Core** dal menu a sinistra.

* Selezionare il modello di progetto **App Console (.NET Core)**.

* Denominare il progetto *Blogging.Migrations.Startup* e fare clic su **OK**.

* Aggiungere un riferimento progetto dal progetto *Blogging.Migrations.Startup* al progetto *Blogging.Model*.

Ora è possibile creare la migrazione iniziale.

* **Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**

* Selezionare il progetto *Blogging.Model* come **Progetto predefinito**.

* In **Esplora soluzioni** impostare il progetto *Blogging.Migrations.Startup* come progetto di avvio.

* Eseguire `Add-Migration InitialCreate`.

  Questo comando esegue lo scaffolding di una migrazione che crea il set iniziale di tabelle per il modello.

## <a name="create-the-database-on-app-startup"></a>Creare il database all'avvio dell'app

Poiché si vuole creare il database nel dispositivo in cui viene eseguita l'app, aggiungere codice per applicare eventuali migrazioni in sospeso al database locale all'avvio dell'applicazione. In questo modo il database locale viene creato alla prima esecuzione dell'app.

* Aggiungere un riferimento progetto dal progetto *Blogging.UWP* al progetto *Blogging.Model*.

* Aprire *App.xaml.cs*.

* Aggiungere il codice evidenziato per applicare eventuali migrazioni in sospeso.

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> Se si apportano modifiche al modello, usare il comando `Add-Migration` per eseguire lo scaffolding di una nuova migrazione e applicare le modifiche corrispondenti al database. Eventuali migrazioni in sospeso verranno applicate al database locale in ogni dispositivo all'avvio dell'applicazione.
>
>EF usa una tabella `__EFMigrationsHistory` nel database per tenere traccia delle migrazioni che sono già state applicate al database.

## <a name="use-the-model"></a>Usare il modello

È ora possibile usare il modello per eseguire l'accesso ai dati.

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
