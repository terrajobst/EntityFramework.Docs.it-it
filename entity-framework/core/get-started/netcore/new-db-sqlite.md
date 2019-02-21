---
title: Guida introduttiva a .NET Core - Nuovo database - EF Core
author: rick-anderson
ms.author: riande
description: Introduzione a .NET Core con Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: a0df80a8fe96be4f8cc3177919e2b087e14cb49c
ms.sourcegitcommit: 735715f10cc8a231c213e4f055d79f0effd86570
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/16/2019
ms.locfileid: "56325327"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Introduzione a EF Core nell'app console .NET Core con un nuovo database

In questa esercitazione viene creata un'app console .NET Core che esegue l'accesso ai dati in un database SQLite usando Entity Framework Core. Vengono usate le migrazioni per creare il database dal modello. Per una versione di Visual Studio che usa ASP.NET Core MVC, vedere [ASP.NET Core - Nuovo database](xref:core/get-started/aspnetcore/new-db).

[Visualizzare l'esempio di questo articolo su GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).

## <a name="prerequisites"></a>Prerequisiti

* [.NET Core 2.1 SDK](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a>Creare un nuovo progetto

* Creare un nuovo progetto console:

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a>Selezionare la directory corrente

Nei passaggi successivi è necessario eseguire i comandi `dotnet` nell'applicazione.

* La directory corrente viene modificata nella directory dell'applicazione, come di seguito riportato:

  ``` Console
  cd ConsoleApp.SQLite/
  ```
## <a name="install-entity-framework-core"></a>Installare Entity Framework Core

Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione. Questa procedura dettagliata usa SQLite. Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).

* Installare Microsoft.EntityFrameworkCore.Sqlite e Microsoft.EntityFrameworkCore.Design

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* Eseguire `dotnet restore` per installare i nuovi pacchetti.

## <a name="create-the-model"></a>Creare il modello

Definire un contesto e le classi di entità che costituiscono il modello.

* Creare un nuovo file *Model.cs* con i contenuti seguenti.

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

Suggerimento: in una vera applicazione, ogni classe verrebbe inserita in un file separato e la stringa di connessione verrebbe posizionata in un file di configurazione o in una variabile di ambiente. Per semplicità, in questa esercitazione tutto è contenuto in un solo file.

## <a name="create-the-database"></a>Creare il database

Dopo avere creato un modello,usare le [migrazioni](xref:core/managing-schemas/migrations/index) per creare un database.

* Eseguire `dotnet ef migrations add InitialCreate` per effettuare lo scaffolding di una migrazione e creare il set iniziale di tabelle per il modello.
* Eseguire `dotnet ef database update` per applicare la nuova migrazione al database. Questo comando crea il database prima di applicare le migrazioni.

Il database SQLite *blogging.db* si trova nella directory del progetto.

## <a name="use-the-model"></a>Usare il modello

* Aprire *Program.cs* e sostituire il contenuto con il codice seguente:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Testare l'app dalla console. Vedere la [nota di Visual Studio](#vs) per eseguire l'app da Visual Studio.

  `dotnet run`

  Un blog è salvato nel database e i dettagli di tutti i blog vengono visualizzati nella console.

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Modifica del modello:

- Se si apportano modifiche al modello, è possibile usare il comando `dotnet ef migrations add` per eseguire lo scaffolding di una nuova [migrazione](xref:core/managing-schemas/migrations/index). Dopo avere controllato il codice di scaffolding e avere apportato le eventuali modifiche necessarie, è possibile usare il comando `dotnet ef database update` per applicare le modifiche dello schema al database.
- EF Core usa una tabella `__EFMigrationsHistory` nel database per tenere traccia delle migrazioni che sono già state applicate al database.
- Il motore di database SQLite non supporta determinate modifiche dello schema, supportate dalla maggior parte degli altri database relazionali. Ad esempio l'operazione `DropColumn` non è supportata. Le migrazioni EF Core generano codice per queste operazioni. Se tuttavia si prova ad applicarle a un database o di generare uno script, EF Core produce eccezioni. Vedere [SQLite Limitations](../../providers/sqlite/limitations.md) (Limitazioni di SQLite). Per lo sviluppo di nuovo codice, prendere in considerazione la possibilità di eliminare il database e di crearne uno nuovo invece di usare le migrazioni quando il modello viene modificato.

<a name="vs"></a>
### <a name="run-from-visual-studio"></a>Eseguire da Visual Studio

Per eseguire questo esempio da Visual Studio, è necessario impostare la directory di lavoro manualmente in modo che diventi la radice del progetto. Se la directory di lavoro non viene impostata, viene generata l'eccezione `Microsoft.Data.Sqlite.SqliteException` seguente: `SQLite Error 1: 'no such table: Blogs'`.

Per impostare la directory di lavoro:

* In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto e scegliere **Proprietà**.
* Selezionare la scheda **Debug** nel riquadro sinistro.
* Impostare la **directory di lavoro** sul percorso del progetto.
* Salvare le modifiche.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Esercitazione: Introduzione a EF Core in ASP.NET Core con un nuovo database usando SQLite](xref:core/get-started/aspnetcore/new-db)
* [Esercitazione: Introduzione all'uso di Razor Pages in ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [Esercitazione: Razor Pages con EF Core in ASP.NET Core ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
