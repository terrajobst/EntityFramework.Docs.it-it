---
title: Guida introduttiva a .NET Core - Nuovo database - EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Introduzione a .NET Core con Entity Framework Core
keywords: .NET Core, Entity Framework Core, VS Code, Visual Studio Code, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: fcace3c0f259b1a456d9ca1086e6a1549c070d57
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Introduzione a EF Core nell'app console .NET Core con un nuovo database

In questa procedura dettagliata verrà creata un'app console .NET Core che esegue l'accesso ai dati di base in un database SQLite usando Entity Framework Core. Si useranno le migrazioni per creare il database dal modello. Per una versione di Visual Studio che usa ASP.NET Core MVC, vedere [ASP.NET Core - Nuovo database](xref:core/get-started/aspnetcore/new-db).

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) di questo articolo in GitHub.

## <a name="prerequisites"></a>Prerequisiti

Per completare la procedura dettagliata, sono necessari i prerequisiti seguenti:
* Sistema operativo che supporta .NET Core.
* [.NET Core SDK](https://www.microsoft.com/net/core) 2.0, anche se le istruzioni possono essere usate per creare un'applicazione con una versione precedente con pochissime modifiche.

## <a name="create-a-new-project"></a>Creare un nuovo progetto

* Creare una nuova cartella `ConsoleApp.SQLite` per il progetto e usare il comando `dotnet` per popolarla con un 'app .NET Core.

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a>Installare Entity Framework Core

Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione. Questa procedura dettagliata usa SQLite. Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).

* Installare Microsoft.EntityFrameworkCore.Sqlite e Microsoft.EntityFrameworkCore.Design

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* Modificare manualmente `ConsoleApp.SQLite.csproj` per aggiungere DotNetCliToolReference a Microsoft.EntityFrameworkCore.Tools.DotNet:

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

`ConsoleApp.SQLite.csproj` conterrà ora quanto segue:

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 Nota: i numeri di versione usati sopra erano corretti al momento della pubblicazione.

*  Eseguire `dotnet restore` per installare i nuovi pacchetti.

## <a name="create-the-model"></a>Creare il modello

Definire un contesto e le classi di entità che costituiscono il modello.

* Creare un nuovo file *Model.cs* con i contenuti seguenti.

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

Suggerimento: in una vera applicazione si inserisce ogni classe in un file separato e la stringa di connessione in un file di configurazione. Per semplicità, in questa esercitazione viene inserito tutto in un solo file.

## <a name="create-the-database"></a>Creare il database

Dopo avere creato un modello, è possibile usare le [migrazioni](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) per creare un database.

* Eseguire `dotnet ef migrations add InitialCreate` per effettuare lo scaffolding di una migrazione e creare il set iniziale di tabelle per il modello.
* Eseguire `dotnet ef database update` per applicare la nuova migrazione al database. Questo comando crea il database prima di applicare le migrazioni.

> [!NOTE]  
> Quando si usano i percorsi relativi con SQLite, il percorso sarà relativo all'assembly principale dell'applicazione. In questo esempio il file binario principale è `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, quindi il database SQLite sarà in `bin/Debug/netcoreapp2.0/blogging.db`.

## <a name="use-your-model"></a>Usare il modello

* Aprire *Program.cs* e sostituire il contenuto con il codice seguente:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Eseguire il test dell'app:

  `dotnet run`

  Un blog è salvato nel database e i dettagli di tutti i blog vengono visualizzati nella console.

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Modifica del modello:

- Se si apportano modifiche al modello, è possibile usare il comando `dotnet ef migrations add` per eseguire lo scaffolding di una nuova [migrazione](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) e poter apportare le corrispondenti modifiche dello schema al database. Dopo avere controllato il codice di scaffolding e avere apportato eventuali modifiche necessarie, è possibile usare il comando `dotnet ef database update` per applicare le modifiche al database.
- EF usa una tabella `__EFMigrationsHistory` nel database per tenere traccia delle migrazioni che sono già state applicate al database.
- SQLite non supporta tutte le migrazioni (modifiche dello schema) a causa di limitazioni in SQLite. Vedere [SQLite Limitations](../../providers/sqlite/limitations.md) (Limitazioni di SQLite). Per una nuova fase di sviluppo, prendere in considerazione la possibilità di eliminare il database e di crearne uno nuovo invece di usare le migrazioni quando il modello viene modificato.

## <a name="additional-resources"></a>Risorse aggiuntive

* [.NET Core - Nuovo database con SQLite](xref:core/get-started/netcore/new-db-sqlite): esercitazione su EF per la console multipiattaforma.
* [Introduzione ad ASP.NET Core MVC in Mac o Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Introduzione ad ASP.NET Core MVC con Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Introduzione ad ASP.NET Core ed Entity Framework Core con Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
