---
title: Guida introduttiva ad ASP.NET Core - Database esistente - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: e28149346ccd7531449ea696505588317471e6dd
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949153"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Introduzione a EF Core in ASP.NET Core con un database esistente

In questa procedura dettagliata si compilerà un'applicazione ASP.NET Core MVC che esegue l'accesso ai dati di base usando Entity Framework. Si userà il reverse engineering per creare un modello di Entity Framework basato su un database esistente.

> [!TIP]  
> È possibile visualizzare l'[esempio](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) di questo articolo in GitHub.

## <a name="prerequisites"></a>Prerequisiti

Per completare la procedura dettagliata, sono necessari i prerequisiti seguenti:

* [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) con questi carichi di lavoro:
  * **Sviluppo ASP.NET e Web** (in **Web e Cloud**)
  * **Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)
* [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).
* [Database Blogging](#blogging-database)

### <a name="blogging-database"></a>Database Blogging

Questa esercitazione usa un database **Blogging** nell'istanza di LocalDb come database esistente.

> [!TIP]  
> Se si è già creato il database **Blogging** durante un'altra esercitazione, è possibile ignorare questi passaggi.

* Aprire Visual Studio
* **Strumenti -> Connetti al database**
* Selezionare **Microsoft SQL Server** e fare clic su **Continua**
* Immettere **(localdb)\mssqllocaldb** in **Nome server**
* Immettere **master** in **Nome database** e fare clic su **OK**
* Il database master viene ora visualizzato sotto **Connessioni dati** in **Esplora server**
* Fare clic con il pulsante destro del mouse sul database in **Esplora server** e scegliere **Nuova query**
* Copiare lo script, riportato sotto, nell'editor di query
* Fare clic con il pulsante destro del mouse sull'editor di query e scegliere **Esegui**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Creare un nuovo progetto

* Aprire Visual Studio 2017
* **File -> Nuovo -> Progetto**
* Scegliere **Installati > Modelli > Visual C# > Web** dal menu a sinistra
* Selezionare il modello di progetto **Applicazione Web ASP.NET Core (.NET Core)**
* Immettere **EFGetStarted.AspNetCore.ExistingDb** come nome e fare clic su **OK**
* Attendere che venga visualizzata la finestra di dialogo **Nuova applicazione Web ASP.NET Core**
* In **ASP.NET Core Templates 2.0** (Modelli di ASP.NET Core 2.0) selezionare **Applicazione Web (MVC)**
* Assicurarsi che **Autenticazione** sia impostato su **Nessuna autenticazione**
* Fare clic su **OK**

## <a name="install-entity-framework"></a>Installare Entity Framework

Per usare EF Core, installare il pacchetto per i provider di database che si vuole specificare come destinazione. Questa procedura dettagliata usa SQL Server. Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md).

* **Strumenti > Gestione pacchetti NuGet > Console di Gestione pacchetti**

* Eseguire `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Verranno usati alcuni strumenti di Entity Framework per creare un modello dal database, quindi verrà anche installato il pacchetto di strumenti:

* Eseguire `Install-Package Microsoft.EntityFrameworkCore.Tools`

Più avanti verranno usati alcuni strumenti di scaffolding di ASP.NET Core per creare controller e visualizzazioni, quindi verrà anche installato il pacchetto di progettazioni:

* Eseguire `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`

## <a name="reverse-engineer-your-model"></a>Decompilare il modello

È ora possibile creare il modello di EF basato sul database esistente.

* **Strumenti –> Gestione pacchetti NuGet –> Console di Gestione pacchetti**
* Eseguire il comando seguente per creare un modello dal database esistente:

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Se viene visualizzato l'errore `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, chiudere e riaprire Visual Studio.

> [!TIP]  
> È possibile specificare per quali tabelle si vogliono generare le entità aggiungendo l'argomento `-Tables` al comando precedente. Ad esempio `-Tables Blog,Post`.

Il processo di reverse engineering ha creato le classi di entità (`Blog.cs` & `Post.cs`) e un contesto derivato (`BloggingContext.cs`) in base allo schema del database esistente.

 Le classi di entità sono semplici oggetti C# che rappresentano i dati di cui verranno eseguite query e che verranno salvati.

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 Il contesto rappresenta una sessione con il database e consente di eseguire query delle istanze delle classi di entità e di salvarle.

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a>Registrare il contesto con l'inserimento delle dipendenze

Il concetto di inserimento delle dipendenze è fondamentale per ASP.NET Core. I servizi (ad esempio, `BloggingContext`) vengono registrati con l'inserimento delle dipendenze durante l'avvio dell'applicazione. Questi servizi vengono quindi forniti ai componenti per cui sono necessari (ad esempio, i controller MVC) tramite proprietà o parametri del costruttore. Per altre informazioni sull'inserimento delle dipendenze, vedere l'articolo [Inserimento delle dipendenze](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) nel sito di ASP.NET.

### <a name="remove-inline-context-configuration"></a>Rimuovere la configurazione nel contesto inline

In ASP.NET Core la configurazione viene in genere eseguita in **Startup.cs**. Per conformità a questo modello, la configurazione del provider di database verrà spostata in **Startup.cs**.

* Aprire `Models\BloggingContext.cs`
* Eliminare il metodo `OnConfiguring(...)`

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* Aggiungere il costruttore seguente, che consente di passare la configurazione nel contesto tramite l'inserimento delle dipendenze

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a>Registrare e configurare il contesto in Startup.cs

`BloggingContext` verrà registrato come servizio per consentire ai controller MVC di usarlo.

* Aprire **Startup.cs**
* Aggiungere le istruzioni `using` seguenti all'inizio del file

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

È ora possibile usare il metodo `AddDbContext(...)` per registrarlo come servizio.
* Individuare il metodo `ConfigureServices(...)`
* Aggiungere il codice seguente per registrare il contesto come servizio

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> In una vera applicazione in genere si inserisce la stringa di connessione in un file di configurazione. Per ragioni di semplicità, viene definita nel codice. Per altre informazioni, vedere [Connection Strings](../../miscellaneous/connection-strings.md) (Stringhe di connessione).

## <a name="create-a-controller"></a>Creare un controller

Verrà ora abilitato lo scaffolding nel progetto.

* Fare clic con il pulsante destro del mouse sulla cartella **Controllers** in **Esplora soluzioni** e scegliere **Aggiungi -> Controller**.
* Selezionare **Dipendenze complete** e fare clic su **Aggiungi**
* È possibile ignorare le istruzioni nel file `ScaffoldingReadMe.txt` visualizzato

Quando è abilitato, è possibile eseguire lo scaffolding di un controller per l'entità `Blog`.

* Fare clic con il pulsante destro del mouse sulla cartella **Controllers** in **Esplora soluzioni** e scegliere **Aggiungi -> Controller**.
* Selezionare **Controller MVC con visualizzazioni, che usa Entity Framework** e fare clic su **OK**
* Impostare **Classe modello** su **Blog** e **Classe contesto dati** su **BloggingContext**
* Fare clic su **Aggiungi**

## <a name="run-the-application"></a>Esecuzione dell'applicazione

È ora possibile eseguire l'applicazione per vederla in azione.

* **Debug - > Avvia senza eseguire debug**
* L'applicazione verrà compilata e aperta in un Web browser
* Passare a `/Blogs`
* Fare clic su **Crea nuovo**
* Immettere un **URL** per il nuovo blog e fare clic su **Crea**

![immagine](_static/create.png)

![immagine](_static/index-existing-db.png)
