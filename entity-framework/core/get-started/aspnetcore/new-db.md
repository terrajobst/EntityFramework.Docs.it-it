---
title: Guida introduttiva ad ASP.NET Core - Nuovo database - EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 4734586adc89e9c1d866a1b4accd8b5e51fe2bb0
ms.sourcegitcommit: ebf661025d2ad2b62466fa7bf0e0772a7811cbe7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/11/2019
ms.locfileid: "54211166"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Introduzione a EF Core in ASP.NET Core con un nuovo database

In questa esercitazione verrà creata un'applicazione ASP.NET Core MVC che esegue l'accesso ai dati di base usando Entity Framework Core. Nell'esercitazione vengono usate le migrazioni per creare il database dal modello di dati.

È possibile eseguire l'esercitazione usando Visual Studio 2017 in Windows, oppure usando l'interfaccia della riga di comando di .NET Core in Windows, macOS o Linux.

Visualizzare l'esempio di questo articolo su GitHub:
* [Visual Studio 2017 con SQL Server](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* [Interfaccia della riga di comando di .NET Core con SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).

## <a name="prerequisites"></a>Prerequisiti

Installare il software seguente:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017 versione 15.7 o versione successiva](https://www.visualstudio.com/downloads/) con i carichi di lavoro seguenti:
  * **Sviluppo ASP.NET e Web** (in **Web e Cloud**)
  * **Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).

---

## <a name="create-a-new-project"></a>Creare un nuovo progetto

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Aprire Visual Studio 2017
* **File > Nuovo > Progetto**
* Selezionare **Installati > Visual C# > .NET Core** dal menu a sinistra.
* Selezionare **Applicazione Web ASP.NET Core**.
* Immettere **EFGetStarted.AspNetCore.NewDb** come nome e fare clic su **OK**.
* Nella finestra di dialogo **Nuova applicazione Web ASP.NET Core**:
  * Assicurarsi che le opzioni **.NET Core** e **ASP.NET Core 2.1** siano selezionate negli elenchi a discesa
  * Selezionare il modello di progetto **Applicazione Web (MVC)**
  * Assicurarsi che l'opzione **Autenticazione** sia impostata su **Nessuna autenticazione**
  * Fare clic su **OK**

Avviso: se si usa **Account utente individuali** invece di **Nessuna** per **Autenticazione**, un modello di Entity Framework Core verrà aggiunto al progetto in `Models\IdentityModel.cs`. Usando le tecniche illustrate in questa esercitazione, è possibile scegliere di aggiungere un secondo modello o di estendere questo modello esistente in modo che contenga le classi di entità.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

* Eseguire il comando seguente per creare un progetto MVC:

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* Modificare il percorso alla directory del progetto. Per eseguire il nuovo progetto, immettere i successivi comandi.

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a>Installare Entity Framework Core

Per installare EF Core, installare il pacchetto per i provider di database di EF Core che si vuole usare come destinazione. Per un elenco dei provider disponibili, vedere [Provider di database](../../providers/index.md).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Per questa esercitazione, non è necessario installare un pacchetto di provider perché l'esercitazione usa SQL Server. Il pacchetto del provider SQL Server è incluso nel [metapacchetto Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Questa esercitazione usa SQLite in quanto viene eseguita su tutte le piattaforme supportate da .NET Core.

* Eseguire il comando seguente per installare il provider SQLite:

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a>Creare il modello

Definire una classe di contesto e le classi di entità che costituiscono il modello.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Fare clic con il pulsante destro del mouse sulla cartella **Models** e scegliere **Aggiungi > Classe**.
* Immettere **Model.cs** come nome e fare clic su **OK**.
* Sostituire tutti i contenuti del file con il codice seguente:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

* Nella cartella **Modelli** creare un file **Course.cs** con il codice seguente:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

Un'app di produzione inserisce generalmente ogni classe in un file separato. Per ragioni di semplicità, in questa esercitazione tutte le classi vengono inserite in un solo file.

## <a name="register-the-context-with-dependency-injection"></a>Registrare il contesto con l'inserimento delle dipendenze

I servizi (ad esempio, `BloggingContext`) vengono registrati con l'[inserimento delle dipendenze](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) durante l'avvio dell'applicazione. Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio i controller MVC) tramite i parametri del costruttore o le proprietà.

Per rendere `BloggingContext` disponibile per i controller MVC, registrarlo come servizio.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* In **Startup.cs** aggiungere le istruzioni `using` seguenti:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* Aggiungere il codice evidenziato seguente al metodo `ConfigureServices`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

* In **Startup.cs** aggiungere le istruzioni `using` seguenti:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* Aggiungere il codice evidenziato seguente al metodo `ConfigureServices`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

Una app di produzione inserisce generalmente la stringa di connessione in un file di configurazione o in una variabile di ambiente. Per ragioni di semplicità, per questa esercitazione viene definita nel codice. Per altre informazioni, vedere [Connection Strings](../../miscellaneous/connection-strings.md) (Stringhe di connessione).

## <a name="create-the-database"></a>Creare il database

La procedura seguente usa le [migrazioni](xref:core/managing-schemas/migrations/index) per creare un database.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**
* Eseguire i comandi seguenti:

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  Se viene visualizzato l'errore `The term 'add-migration' is not recognized as the name of a cmdlet`, chiudere e riaprire Visual Studio.

  Il comando `Add-Migration` esegue lo scaffolding di una migrazione per creare il set iniziale di tabelle per il modello. Il comando `Update-Database` crea il database e ne applica la nuova migrazione.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

* Eseguire i comandi seguenti:

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  Il comando `migrations` esegue lo scaffolding di una migrazione per creare il set iniziale di tabelle per il modello. Il comando `database update` crea il database e ne applica la nuova migrazione.

---

## <a name="create-a-controller"></a>Creare un controller

Eseguire lo scaffolding di un controller e delle visualizzazioni per l'entità `Blog`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Fare clic con il pulsante destro del mouse sulla cartella **Controllers** in **Esplora soluzioni** e scegliere **Aggiungi > Controller**.
* Selezionare **Controller MVC con visualizzazioni, che usa Entity Framework** e fare clic su **Aggiungi**.
* Impostare **Classe modello** su **Blog** e **Classe contesto dati** su **BloggingContext**.
* Fare clic su **Aggiungi**.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

* Eseguire i comandi seguenti:

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  I comandi `tool install` e `add package` installano gli strumenti che possano eseguire lo scaffolding di controller e visualizzazioni. Il comando `restore` assicura che vengano scaricati tutti i pacchetti del progetto, mentre il comando `aspnet-codegenerator` esegue lo scaffolding.
---

Il motore di scaffolding crea i file seguenti:

* Un controller (*Controllers/BlogsController.cs*)
* Visualizzazioni Razor per le pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice) (_Views/Blogs/*.cshtml_)

## <a name="run-the-application"></a>Esecuzione dell'applicazione

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Debug** > **Avvia senza eseguire debug**

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

```cli
dotnet run
```
---

* Passare a `/Blogs`

* Usare il collegamento **Crea nuova** per creare alcune voci del blog.

  ![Pagina Crea](_static/create.png)

* Testare i collegamenti **Dettagli**, **Modica** ed **Elimina**.

  ![Pagina Index (Indice)](_static/index-new-db.png)

## <a name="additional-resources"></a>Risorse aggiuntive

* [Esercitazione: Introduzione a EF Core in ASP.NET Core con un nuovo database usando SQLite](xref:core/get-started/netcore/new-db-sqlite)
* [Esercitazione: Introduzione all'uso di Razor Pages in ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [Esercitazione: Razor Pages con EF Core in ASP.NET Core ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
