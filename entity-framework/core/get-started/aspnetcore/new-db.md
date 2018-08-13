---
title: Guida introduttiva ad ASP.NET Core - Nuovo database - EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 08/03/2018
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 9e86bc9cff028ad9791f23cbb45f0a93110c0064
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614350"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Introduzione a EF Core in ASP.NET Core con un nuovo database

In questa esercitazione verrà creata un'applicazione ASP.NET Core MVC che esegue l'accesso ai dati di base usando Entity Framework Core. Si useranno le migrazioni per creare il database dal modello di EF Core.

[Visualizzare l'esempio di questo articolo su GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).

## <a name="prerequisites"></a>Prerequisiti

Installare il software seguente:

* [Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) con questi carichi di lavoro:
  * **Sviluppo ASP.NET e Web** (in **Web e Cloud**)
  * **Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).

## <a name="create-a-new-project-in-visual-studio-2017"></a>Creare un nuovo progetto in Visual Studio 2017

* Aprire Visual Studio 2017
* **File > Nuovo > Progetto**
* Scegliere **Installati > Visual C# > .NET Core** dal menu a sinistra.
* Selezionare **Applicazione Web ASP.NET Core**.
* Immettere **EFGetStarted.AspNetCore.NewDb** come nome e fare clic su **OK**.
* Nella finestra di dialogo **Nuova applicazione Web ASP.NET Core**:
  * Assicurarsi che le opzioni **.NET Core** e **ASP.NET Core 2.1** siano selezionate negli elenchi a discesa
  * Selezionare il modello di progetto **Applicazione Web (MVC)**
  * Assicurarsi che **Autenticazione** sia impostato su **Nessuna autenticazione**
  * Fare clic su **OK**

Avviso: se si usa **Account utente individuali** invece di **Nessuna** per **Autenticazione**, un modello di Entity Framework Core verrà aggiunto al progetto in `Models\IdentityModel.cs`. Usando le tecniche illustrate in questa esercitazione, è possibile scegliere di aggiungere un secondo modello o di estendere questo modello esistente in modo che contenga le classi di entità.

## <a name="install-entity-framework-core"></a>Installare Entity Framework Core

Per installare EF Core, installare il pacchetto per i provider di database di EF Core che si vuole usare come destinazione. Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md). 

Per questa esercitazione, non è necessario installare un pacchetto di provider perché l'esercitazione usa SQL Server. Il pacchetto del provider SQL Server è incluso nel [metapacchetto Microsoft.AspnetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

## <a name="create-the-model"></a>Creare il modello

Definire una classe di contesto e le classi di entità che costituiscono il modello:

* Fare clic con il pulsante destro del mouse sulla cartella **Models** e scegliere **Aggiungi > Classe**.
* Immettere **Model.cs** come nome e fare clic su **OK**.
* Sostituire tutti i contenuti del file con il codice seguente:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

In una vera app in genere si inserisce ogni classe del modello in un file separato. Per ragioni di semplicità, in questa esercitazione tutte le classi vengono inserite in un solo file.

## <a name="register-your-context-with-dependency-injection"></a>Registrare il contesto con l'inserimento delle dipendenze

I servizi (ad esempio, `BloggingContext`) vengono registrati con l'[inserimento delle dipendenze](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) durante l'avvio dell'applicazione. Questi servizi vengono quindi forniti ai componenti per cui sono necessari (ad esempio, i controller MVC) tramite proprietà o parametri del costruttore.

Per rendere `BloggingContext` disponibile per i controller MVC, registrarlo come servizio.

* Aprire **Startup.cs**
* Aggiungere le istruzioni `using` seguenti:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

Chiamare il metodo `AddDbContext` per registrare il contesto come servizio.

* Aggiungere il codice evidenziato seguente al metodo `ConfigureServices`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

Nota: una vera app in genere inserisce la stringa di connessione in un file di configurazione o in una variabile di ambiente. Per ragioni di semplicità, per questa esercitazione viene definita nel codice. Per altre informazioni, vedere [Connection Strings](../../miscellaneous/connection-strings.md) (Stringhe di connessione).

## <a name="create-the-database"></a>Creare il database

Dopo avere creato un modello, è possibile usare le [migrazioni](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) per creare un database.

* **Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**
* Eseguire `Add-Migration InitialCreate` per effettuare lo scaffolding di una migrazione per poter creare il set iniziale di tabelle per il modello. Se viene visualizzato l'errore `The term 'add-migration' is not recognized as the name of a cmdlet`, chiudere e riaprire Visual Studio.
* Eseguire `Update-Database` per applicare la nuova migrazione al database. Questo comando crea il database prima di applicare le migrazioni.

## <a name="create-a-controller"></a>Creare un controller

Eseguire lo scaffolding di un controller e delle visualizzazioni per l'entità `Blog`.

* Fare clic con il pulsante destro del mouse sulla cartella **Controllers** in **Esplora soluzioni** e scegliere **Aggiungi > Controller**.
* Selezionare **Controller MVC con visualizzazioni, che usa Entity Framework** e fare clic su **Aggiungi**.
* Impostare **Classe modello** su **Blog** e **Classe contesto dati** su **BloggingContext**.
* Fare clic su **Aggiungi**.


## <a name="run-the-application"></a>Esecuzione dell'applicazione

Premere F5 per eseguire e testare l'app.

* Passare a `/Blogs`
* Usare il collegamento Crea per creare alcune voci del blog. Testare i dettagli ed eliminare i collegamenti.

![immagine](_static/create.png)

![immagine](_static/index-new-db.png)

## <a name="additional-resources"></a>Risorse aggiuntive

* [EF - Nuovo database con SQLite](xref:core/get-started/netcore/new-db-sqlite): esercitazione su EF per la console multipiattaforma.
* [Introduzione ad ASP.NET Core MVC in Mac o Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Introduzione ad ASP.NET Core MVC con Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Introduzione ad ASP.NET Core ed Entity Framework Core con Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
