---
title: Guida introduttiva alla piattaforma UWP - Nuovo database - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/01/2017
ms.locfileid: "26049834"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Introduzione a EF Core nella piattaforma UWP (Universal Windows Platform) con un nuovo database

> [!NOTE]
> Questa esercitazione usa EF Core 2.0.1 (rilasciato con ASP.NET Core e .NET Core SDK 2.0.3). In EF Core 2.0.0 mancano alcune importanti correzioni di bug necessarie per un'esperienza ottimale con la piattaforma UWP.

In questa procedura dettagliata verrà compilata un'applicazione UWP (Universal Windows Platform) che esegue l'accesso ai dati di base in un database SQLite locale usando Entity Framework.

> [!IMPORTANT]
> **Considerare la possibilità di evitare i tipi anonimi nelle query LINQ sulla piattaforma UWP**. Per distribuire un'applicazione UWP nell'App Store, l'applicazione deve essere compilata con .NET Native. Le query con tipi anonimi hanno prestazioni peggiori in .NET Native.

> [!TIP]
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) di questo articolo in GitHub.

## <a name="prerequisites"></a>Prerequisiti

Per completare questa procedura dettagliata, sono necessari gli elementi seguenti:

* [Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)

* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva.

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) versione 15.4 o successiva con il carico di lavoro **Sviluppo di app per la piattaforma UWP (Universal Windows Platform)**.

## <a name="create-a-new-model-project"></a>Creare un nuovo progetto modello

> [!WARNING]
> A causa delle limitazioni nell'interazione degli strumenti di .NET Core con i progetti della piattaforma UWP, il modello deve essere inserito in un progetto non UWP per poter eseguire i comandi delle migrazioni nella console di Gestione pacchetti

* Aprire Visual Studio

* File > Nuovo > Progetto

* Scegliere Modelli > Visual C# dal menu a sinistra

* Selezionare il modello di progetto **Libreria di classi (.NET Standard)**

* Specificare un nome per il progetto e fare clic su **OK**

## <a name="install-entity-framework"></a>Installare Entity Framework

Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione. Questa procedura dettagliata usa SQLite. Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).

* Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti

* Eseguire `Install-Package Microsoft.EntityFrameworkCore.Sqlite`

Più avanti in questa procedura dettagliata verrà anche usato Entity Framework Tools per gestire il database, quindi verrà anche installato il pacchetto di strumenti.

* Eseguire `Install-Package Microsoft.EntityFrameworkCore.Tools`

* Modificare il file con estensione csproj e sostituire `<TargetFramework>netstandard2.0</TargetFramework>` con `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`

## <a name="create-your-model"></a>Creare il modello

È ora possibile definire un contesto e le classi di entità che costituiscono il modello.

* Progetto > Aggiungi classe

* Immettere *Model.cs* come nome e fare clic su **OK**

* Sostituire tutti i contenuti del file con il codice seguente

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>Creare un nuovo progetto UWP

* Aprire Visual Studio

* File > Nuovo > Progetto

* Scegliere Modelli > Visual C# > Universale di Windows dal menu a sinistra

* Selezionare il modello di progetto **App vuota (Windows universale)**

* Specificare un nome per il progetto e fare clic su **OK**

* Impostare le versioni di destinazione e minima almeno su `Windows 10 Fall Creators Update (10.0; build 16299.0)`

## <a name="create-your-database"></a>Creare il database

Dopo avere creato un modello, è possibile usare le migrazioni per creare un database.

* Strumenti –> Gestione pacchetti NuGet –> Console di Gestione pacchetti

* Selezionare il progetto modello come progetto predefinito e impostarlo come progetto di avvio

* Eseguire `Add-Migration MyFirstMigration` per effettuare lo scaffolding di una migrazione per poter creare il set iniziale di tabelle per il modello.

Poiché si vuole creare il database nel dispositivo in cui viene eseguita l'app, verrà aggiunto il codice per applicare eventuali migrazioni in sospeso al database locale all'avvio dell'applicazione. In questo modo, alla prima esecuzione dell'app, il database locale verrà creato automaticamente.

* In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **App.xaml** e scegliere **Visualizza codice**

* Aggiungere l'istruzione using evidenziata all'inizio del file

* Aggiungere il codice evidenziato per applicare eventuali migrazioni in sospeso

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> Se si apporteranno modifiche al modello in futuro, sarà possibile usare il comando `Add-Migration` per eseguire lo scaffolding di una nuova migrazione e poter applicare le corrispondenti modifiche al database. Eventuali migrazioni in sospeso verranno applicate al database locale in ogni dispositivo all'avvio dell'applicazione.
>
>EF usa una tabella `__EFMigrationsHistory` nel database per tenere traccia delle migrazioni che sono già state applicate al database.

## <a name="use-your-model"></a>Usare il modello

È ora possibile usare il modello per eseguire l'accesso ai dati.

* Aprire *MainPage.xaml*

* Aggiungere il gestore di caricamento pagina e il contenuto dell'interfaccia utente evidenziati sotto

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

Verrà ora aggiunto il codice per associare l'interfaccia utente al database

* In **Esplora soluzioni** fare clic con il pulsante destro del mouse su **MainPage.xaml** e scegliere **Visualizza codice**

* Aggiungere il codice evidenziato nel listato seguente

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

È ora possibile eseguire l'applicazione per vederla in azione.

* Debug > Avvia senza eseguire debug

* L'applicazione verrà compilata e avviata

* Immettere un URL e fare clic sul pulsante **Aggiungi**

![image](_static/create.png)

![image](_static/list.png)

## <a name="next-steps"></a>Passaggi successivi

> [!TIP]
> Le prestazioni di `SaveChanges()` possono essere migliorate implementando [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) nei tipi di entità e usando `ChangeTrackingStrategy.ChangingAndChangedNotifications`.

È ora disponibile una semplice app UWP che esegue Entity Framework.

Per altre informazioni sulle funzionalità di Entity Framework, vedere gli altri articoli di questa documentazione.
