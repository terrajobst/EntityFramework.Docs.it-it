---
title: Guida introduttiva ad ASP.NET Core - Nuovo database - EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 80477ca57b8b3df6de8ba3595c9056c6b8412040
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Introduzione a EF Core in ASP.NET Core con un nuovo database

In questa procedura dettagliata si compilerà un'applicazione ASP.NET Core MVC che esegue l'accesso ai dati di base usando Entity Framework Core. Si useranno le migrazioni per creare il database dal modello di EF Core. Per altre esercitazioni su Entity Framework Core, vedere [Risorse aggiuntive](#additional-resources).

Per questa esercitazione sono necessari:
* [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) con questi carichi di lavoro:
  * **Sviluppo ASP.NET e Web** (in **Web e Cloud**)
  * **Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)
* [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) di questo articolo in GitHub.

## <a name="create-a-new-project-in-visual-studio-2017"></a>Creare un nuovo progetto in Visual Studio 2017

* **File > Nuovo > Progetto**
* Scegliere **Installati > Modelli > Visual C# > .NET Core** dal menu a sinistra.
* Selezionare **Applicazione Web ASP.NET Core**.
* Immettere **EFGetStarted.AspNetCore.NewDb** come nome e fare clic su **OK**.
* Nella finestra di dialogo **Nuova applicazione Web ASP.NET Core**:
  * Assicurarsi che le opzioni **.NET Core** e **ASP.NET Core 2.0** siano selezionate negli elenchi a discesa
  * Selezionare il modello di progetto **Applicazione Web (MVC)**
  * Assicurarsi che **Autenticazione** sia impostato su **Nessuna autenticazione**
  * Fare clic su **OK**

Avviso: se si usa **Account utente individuali** invece di **Nessuna** per **Autenticazione**, un modello di Entity Framework Core verrà aggiunto al progetto in `Models\IdentityModel.cs`. Usando le tecniche illustrate in questa procedura dettaglia, è possibile scegliere di aggiungere un secondo modello o di estendere questo modello esistente in modo che contenga le classi di entità.

## <a name="install-entity-framework-core"></a>Installare Entity Framework Core

Installare il pacchetto per i provider di database di EF Core che si vuole specificare come destinazione. Questa procedura dettagliata usa SQL Server. Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).

* **Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**

* Eseguire `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Verranno usati alcuni strumenti di Entity Framework Core per creare un database dal modello di EF Core, quindi verrà anche installato il pacchetto di strumenti:

* Eseguire `Install-Package Microsoft.EntityFrameworkCore.Tools`

Più avanti verranno usati alcuni strumenti di scaffolding di ASP.NET Core per creare controller e visualizzazioni, quindi verrà anche installato il pacchetto di progettazioni:

* Eseguire `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`

## <a name="create-the-model"></a>Creare il modello

Definire un contesto e le classi di entità che costituiscono il modello:

* Fare clic con il pulsante destro del mouse sulla cartella **Models** e scegliere **Aggiungi > Classe**.
* Immettere **Model.cs** come nome e fare clic su **OK**.
* Sostituire tutti i contenuti del file con il codice seguente:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

Nota: in una vera app in genere si inserisce ogni classe del modello in un file separato. Per ragioni di semplicità, in questa esercitazione tutte le classi vengono inserite in un solo file.

## <a name="register-your-context-with-dependency-injection"></a>Registrare il contesto con l'inserimento delle dipendenze

I servizi (ad esempio, `BloggingContext`) vengono registrati con l'[inserimento delle dipendenze](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) durante l'avvio dell'applicazione. Questi servizi vengono quindi forniti ai componenti per cui sono necessari (ad esempio, i controller MVC) tramite proprietà o parametri del costruttore.

`BloggingContext` verrà registrato come servizio per consentire ai controller MVC di usarlo.

* Aprire **Startup.cs**
* Aggiungere le istruzioni `using` seguenti:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

Aggiungere il metodo `AddDbContext` per registrarlo come servizio:

* Aggiungere al metodo `ConfigureServices` il codice seguente:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

Nota: una vera app in genere inserisce la stringa di connessione in un file di configurazione. Per ragioni di semplicità, viene definita nel codice. Per altre informazioni, vedere [Connection Strings](../../miscellaneous/connection-strings.md) (Stringhe di connessione).

## <a name="create-your-database"></a>Creare il database

Dopo avere creato un modello, è possibile usare le [migrazioni](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) per creare un database.

* Aprire la console di Gestione pacchetti:

  **Strumenti –> Gestione pacchetti NuGet –> Console di Gestione pacchetti**
* Eseguire `Add-Migration InitialCreate` per effettuare lo scaffolding di una migrazione per poter creare il set iniziale di tabelle per il modello. Se viene visualizzato l'errore `The term 'add-migration' is not recognized as the name of a cmdlet`, chiudere e riaprire Visual Studio.
* Eseguire `Update-Database` per applicare la nuova migrazione al database. Questo comando crea il database prima di applicare le migrazioni.

## <a name="create-a-controller"></a>Creare un controller

Abilitare lo scaffolding nel progetto:

* Fare clic con il pulsante destro del mouse sulla cartella **Controllers** in **Esplora soluzioni** e scegliere **Aggiungi > Controller**.
* Selezionare **Dipendenze minime** e fare clic su **Aggiungi**.
* È possibile ignorare o eliminare il file *ScaffoldingReadMe.txt*.

Quando è abilitato, è possibile eseguire lo scaffolding di un controller per l'entità `Blog`.

* Fare clic con il pulsante destro del mouse sulla cartella **Controllers** in **Esplora soluzioni** e scegliere **Aggiungi > Controller**.
* Selezionare **Controller MVC con visualizzazioni, che usa Entity Framework** e fare clic su **OK**.
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
