---
title: Guida introduttiva ad ASP.NET Core - Database esistente - EF Core
author: rowanmiller
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: bba2742c3f3b6da93dd4b4f170a3878fc0473bc8
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022197"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Introduzione a EF Core in ASP.NET Core con un database esistente

In questa esercitazione verrà creata un'applicazione ASP.NET Core MVC che esegue l'accesso ai dati di base usando Entity Framework Core. Si creerà un modello di Entity Framework tramite reverse engineering di un database esistente.

[Visualizzare l'esempio di questo articolo su GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).

## <a name="prerequisites"></a>Prerequisiti

Installare il software seguente:

* [Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) con questi carichi di lavoro:
  * **Sviluppo ASP.NET e Web** (in **Web e Cloud**)
  * **Sviluppo multipiattaforma .NET Core** (in **Altri set di strumenti**)
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).

## <a name="create-blogging-database"></a>Creare il database Blogging

Questa esercitazione usa un database **Blogging** nell'istanza di LocalDb come database esistente. Se il database **Blogging** è già stato creato durante un'altra esercitazione, ignorare questi passaggi.

* Aprire Visual Studio
* **Strumenti -> Connetti al database**
* Selezionare **Microsoft SQL Server** e fare clic su **Continua**
* Immettere **(localdb)\mssqllocaldb** in **Nome server**
* Immettere **master** in **Nome database** e fare clic su **OK**
* Il database master viene ora visualizzato sotto **Connessioni dati** in **Esplora server**
* Fare clic con il pulsante destro del mouse sul database in **Esplora server** e scegliere **Nuova query**
* Copiare lo script riportato di seguito nell'editor di query
* Fare clic con il pulsante destro del mouse sull'editor di query e scegliere **Esegui**

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Creare un nuovo progetto

* Aprire Visual Studio 2017
* **File > Nuovo > Progetto**
* Scegliere **Installati > Visual C# > Web** dal menu a sinistra
* Selezionare il modello di progetto **Applicazione Web ASP.NET Core**
* Immettere **EFGetStarted.AspNetCore.ExistingDb** come nome e fare clic su **OK**
* Attendere che venga visualizzata la finestra di dialogo **Nuova applicazione Web ASP.NET Core**
* Assicurarsi che l'elenco a discesa dei framework di destinazione sia impostato su **.NET Core** e che l'elenco a discesa delle versioni sia impostato su **ASP.NET Core 2.1**
* Selezionare il modello **Applicazione Web (MVC)**
* Assicurarsi che **Autenticazione** sia impostato su **Nessuna autenticazione**
* Fare clic su **OK**

## <a name="install-entity-framework-core"></a>Installare Entity Framework Core

Per installare EF Core, installare il pacchetto per i provider di database di EF Core che si vuole usare come destinazione. Per un elenco di provider disponibili, vedere [Provider di database](../../providers/index.md). 

Per questa esercitazione, non è necessario installare un pacchetto di provider perché l'esercitazione usa SQL Server. Il pacchetto del provider SQL Server è incluso nel [metapacchetto Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

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

 Le classi di entità sono semplici oggetti C# che rappresentano i dati di cui verranno eseguite query e che verranno salvati. Di seguito sono riportate le classi di entità `Blog` e `Post`:

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> Per abilitare il caricamento lazy, è possibile impostare proprietà di navigazione `virtual` (Blog.Post e Post.Blog).

 Il contesto rappresenta una sessione con il database e consente di eseguire query delle istanze delle classi di entità e di salvarle.

<!-- Static code listing, rather than a linked file, because the tutorial modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public BloggingContext()
    {
    }

    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    {
    }

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

### <a name="register-and-configure-your-context-in-startupcs"></a>Registrare e configurare il contesto in Startup.cs

Per rendere `BloggingContext` disponibile per i controller MVC, registrarlo come servizio.

* Aprire **Startup.cs**
* Aggiungere le istruzioni `using` seguenti all'inizio del file

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

È ora possibile usare il metodo `AddDbContext(...)` per registrarlo come servizio.
* Individuare il metodo `ConfigureServices(...)`
* Aggiungere il codice evidenziato seguente per registrare il contesto come servizio

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> In una vera applicazione in genere si inserisce la stringa di connessione in un file di configurazione o una variabile di ambiente. Per ragioni di semplicità, per questa esercitazione viene definita nel codice. Per altre informazioni, vedere [Connection Strings](../../miscellaneous/connection-strings.md) (Stringhe di connessione).

## <a name="create-a-controller-and-views"></a>Creare controller e visualizzazioni

* Fare clic con il pulsante destro del mouse sulla cartella **Controllers** in **Esplora soluzioni** e scegliere **Aggiungi -> Controller**.
* Selezionare **Controller MVC con visualizzazioni, che usa Entity Framework** e fare clic su **OK**
* Impostare **Classe modello** su **Blog** e **Classe contesto dati** su **BloggingContext**
* Fare clic su **Aggiungi**

## <a name="run-the-application"></a>Esecuzione dell'applicazione

È ora possibile eseguire l'applicazione per vederla in azione.

* **Debug - > Avvia senza eseguire debug**
* L'applicazione viene compilata e aperta in un Web browser
* Passare a `/Blogs`
* Fare clic su **Crea nuovo**
* Immettere un **URL** per il nuovo blog e fare clic su **Crea**

  ![Pagina Crea](_static/create.png)

  ![Pagina Index (Indice)](_static/index-existing-db.png)

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni su come eseguire lo scaffolding di un contesto e delle classi di entità, vedere gli articoli seguenti:
* [Informazioni di riferimento sugli strumenti di Entity Framework Core - Interfaccia della riga di comando di .NET](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [Informazioni di riferimento sugli strumenti di Entity Framework Core - Console di Gestione pacchetti](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)