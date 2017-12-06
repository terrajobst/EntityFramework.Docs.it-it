---
title: L'aggiornamento da versioni precedenti a EF Core 2 - Core a Entity Framework
author: divega
ms.author: divega
ms.date: 8/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
ms.technology: entity-framework-core
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 0bd1ea2476621f826cca7d4a526a49a1b902acf8
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2017
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Aggiornamento di applicazioni da versioni precedenti a EF Core 2.0

## <a name="procedures-common-to-all-applications"></a>Procedure comuni a tutte le applicazioni

Può richiedere l'aggiornamento di un'applicazione esistente a EF Core 2.0:

1. L'aggiornamento la piattaforma .NET di destinazione dell'applicazione che supporta .NET 2.0 Standard. Vedere [piattaforme supportate](../platforms/index.md) per altri dettagli.

2. Identificare il provider per il database di destinazione che è compatibile con Entity Framework Core 2.0. Vedere [EF Core 2.0 richiede un provider di 2.0 database](#ef-core-20-requires-a-20-database-provider) sotto.

3. L'aggiornamento di tutti i pacchetti di EF principali (runtime e gli strumenti) a 2.0. Fare riferimento a [installazione Core EF](../get-started/install/index.md) per altri dettagli.

4. Apportare le modifiche di codice necessario per compensare le modifiche di rilievo. Vedere il [modifiche di rilievo](#breaking-changes) sezione riportata di seguito per ulteriori dettagli.

## <a name="aspnet-core-applications"></a>Applicazioni ASP.NET Core

1. Vedere in particolare il [nuovo modello per l'inizializzazione del provider del servizio dell'applicazione](#new-way-of-getting-application-services) descritto di seguito.

> [!TIP]  
> L'adozione di questo nuovo modello, quando l'aggiornamento di applicazioni a 2.0 è consigliabile ed è necessaria affinché le funzionalità del prodotto come Entity Framework Core migrazioni di lavoro. L'altra alternativa comune consiste nel [implementare *IDesignTimeDbContextFactory\<TContext >*](configuring-dbcontext.md#using-idesigntimedbcontextfactorytcontext).

2. Applicazioni destinate alla base di ASP.NET 2.0 è possono utilizzare EF Core 2.0 senza dipendenze aggiuntive oltre a provider di database di terze parti. Applicazioni destinate a versioni precedenti di ASP.NET Core, tuttavia, necessario eseguire l'aggiornamento a componenti di base di ASP.NET 2.0 per poter utilizzare EF Core 2.0. Per ulteriori informazioni sull'aggiornamento delle applicazioni ASP.NET Core a 2.0, vedere [la documentazione di base di ASP.NET in materia](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).

## <a name="breaking-changes"></a>Modifiche di interruzione

È stato reindirizzato opportunità per perfezionare in modo significativo l'API esistenti e i comportamenti di 2.0. Esistono alcuni miglioramenti che possono richiedere la modifica di codice dell'applicazione esistente, anche se si ritiene che per la maggior parte delle applicazioni l'impatto è bassa, nella maggior parte dei casi che richiedono solo la ricompilazione e modifiche interattiva minime per sostituire le API obsolete.

### <a name="new-way-of-getting-application-services"></a>Nuova modalità di acquisizione di servizi delle applicazioni

Il modello consigliato per le applicazioni web ASP.NET Core è stato aggiornato per 2.0 in modo che ha superato la logica della fase di progettazione che EF principali utilizzati in 1. x. In precedenza in fase di progettazione EF Core tenta di richiamare `Startup.ConfigureServices` direttamente per accedere ai provider di servizi dell'applicazione. In ASP.NET 2.0 Core configurazione viene inizializzata di fuori del `Startup` classe. Applicazioni che usano in genere EF Core accedere relativa stringa di connessione dalla configurazione, pertanto `Startup` da sola non è più sufficiente. Se si aggiorna un'applicazione di ASP.NET Core 1. x, si verifichi l'errore seguente quando si utilizzano gli strumenti di Entity Framework Core.

> Nessun costruttore senza parametri è stato trovato in 'ApplicationContext'. Aggiungere un costruttore senza parametri 'ApplicationContext' o aggiungere un'implementazione di ' IDesignTimeDbContextFactory&lt;ApplicationContext&gt;' nello stesso assembly come 'ApplicationContext'

Modello predefinito di ASP.NET Core 2.0 è stato aggiunto un nuovo hook in fase di progettazione. Il metodo statico `Program.BuildWebHost` consente principale di Entity Framework accedere ai provider di servizi dell'applicazione in fase di progettazione. Se si esegue l'aggiornamento di un'applicazione di ASP.NET Core 1. x, sarà necessario aggiornarla `Program` classe per essere simile al seguente.

``` csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;

namespace AspNetCoreDotNetCore2._0App
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
}
```

### <a name="idbcontextfactory-renamed"></a>IDbContextFactory rinominato

Per supportare modelli di applicazione diverse e fornire agli utenti maggiore controllo sulla modalità loro `DbContext` viene utilizzato in fase di progettazione, è disponibile, in passato, purché il `IDbContextFactory<TContext>` interfaccia. In fase di progettazione, gli strumenti di Entity Framework Core verranno individuata implementazioni di questa interfaccia nel progetto e utilizzarlo per creare `DbContext` oggetti.

Questa interfaccia ha un nome molto generale che indurre gli utenti a provare a utilizzarlo nuovamente per altri `DbContext`-creazione di scenari. Sono state sprovvista se gli strumenti di Entity Framework ha cercato di utilizzare la relativa implementazione in fase di progettazione e ha i comandi come `Update-Database` o `dotnet ef database update` esito negativo.

Per poter comunicare la semantica della fase di progettazione sicura di questa interfaccia, è stata rinominata in `IDesignTimeDbContextFactory<TContext>`.

Per 2.0 rilasciare il `IDbContextFactory<TContext>` esiste ancora, ma è contrassegnato come obsoleto.

### <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions rimosso

A causa di modifiche di ASP.NET 2.0 Core descritte in precedenza, si è scoperto che `DbContextFactoryOptions` non stato non è più necessario nel nuovo `IDesignTimeDbContextFactory<TContext>` interfaccia. Di seguito sono disponibili le alternative, che è consigliabile usare invece.

DbContextFactoryOptions | Alternativa
--- | ---
ApplicationBasePath | AppContext.BaseDirectory
ContentRootPath | Directory.GetCurrentDirectory()
EnvironmentName | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT")

### <a name="design-time-working-directory-changed"></a>Directory di lavoro in fase di progettazione modificata

Le modifiche principali di ASP.NET 2.0 è inoltre necessario directory di lavoro utilizzata da `dotnet ef` per allinearlo con la directory di lavoro usata da Visual Studio quando si esegue l'applicazione. Un effetto collaterale osservabile di questo oggetto è di SQLite i nomi di file sono ora relativa rispetto alla directory del progetto e non la directory di output come alle precedenti.

### <a name="ef-core-20-requires-a-20-database-provider"></a>Core EF 2.0 richiede un provider di 2.0 database

Per Entity Framework Core 2.0 sono stati apportati molti semplificazione e miglioramenti nei provider di database modalità di lavoro. Ciò significa che i provider 1.0 e 1.1.x non funzioneranno con Entity Framework Core 2.0.

I provider di SQL Server e SQLite vengono forniti dal team di Entity Framework e le 2.0 versioni saranno disponibili come parte di 2.0 rilasciare. I provider di terze parti open source per [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), e [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) in corso l'aggiornamento per 2.0. Per tutti gli altri provider, contattare il writer di provider.

### <a name="logging-and-diagnostics-events-have-changed"></a>Sono stati modificati gli eventi di registrazione e diagnostica

Nota: queste modifiche dovrebbero influire gran parte del codice dell'applicazione.

Gli ID degli eventi per i messaggi inviati a un [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) sono stati modificati in 2.0. ID evento ora sono univoci per il codice di Entity Framework Core. Questi messaggi ora anche seguono il modello standard per la registrazione strutturato utilizzato, ad esempio, MVC.

Categorie di logger sono stato modificato. Viene visualizzato tramite un set noto di categorie [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) eventi ora utilizzano gli stessi nomi di ID evento corrispondenti `ILogger` messaggi. I payload dell'evento sono tutti i tipi nominali derivati da [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs).

Gli ID degli eventi, tipi di payload e le categorie sono documentate nel [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) e [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) classi.

Gli ID sono stati spostati anche da Microsoft.EntityFrameworkCore.Infraestructure Microsoft.EntityFrameworkCore.Diagnostics nuovo spazio dei nomi.

### <a name="ef-core-relational-metadata-api-changes"></a>Modifiche ai metadati relazionali API Core EF

Core EF 2.0 ora creerà un altro [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) per ogni provider diversi in uso. Si tratta in genere trasparente all'applicazione. Questo è facilitata una semplificazione dell'API dei metadati di livello inferiore in modo che qualsiasi accesso a _concetti comuni di metadati relazionali_ viene sempre eseguita tramite una chiamata a `.Relational` anziché `.SqlServer`, `.Sqlite`e così via. Ad esempio, 1.1.x codice simile al seguente:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Deve essere scritto ora simile al seguente:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Anziché utilizzare i metodi, ad esempio `ForSqlServerToTable`, metodi di estensione sono ora disponibili per scrivere il codice condizionale in base al provider corrente in uso. Ad esempio:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Si noti che questa modifica si applica solo ai API/metadati definito per _tutti_ provider relazionale. Le API e i metadati rimane invariato quando è specifica per un singolo provider. Ad esempio, gli indici cluster sono specifici di SQL Server, in modo `ForSqlServerIsClustered` e `.SqlServer().IsClustered()` deve comunque essere utilizzato.

### <a name="dont-take-control-of-the-ef-service-provider"></a>Non assumere il controllo del provider di servizi di Entity Framework

Utilizza un oggetto interno EF Core `IServiceProvider` (ad esempio un contenitore dipendenza injection) per l'implementazione interna. Le applicazioni dovrebbero consentire di EF Core creare e gestire questo provider, tranne in casi particolari. Considerare la rimozione di tutte le chiamate a `UseInternalServiceProvider`. Se un'applicazione è necessario chiamare `UseInternalServiceProvider`, quindi provare a [un problema di archiviazione](https://github.com/aspnet/EntityFramework/Issues) in modo sarà possibile esaminare altri modi per gestire lo scenario.

La chiamata `AddEntityFramework`, `AddEntityFrameworkSqlServer`, e così via. non è richiesto dal codice dell'applicazione a meno che non `UseInternalServiceProvider` viene anche chiamato. Rimuovere le eventuali chiamate al `AddEntityFramework` o `AddEntityFrameworkSqlServer`e così via `AddDbContext` deve ancora essere utilizzata nello stesso modo come prima.

### <a name="in-memory-databases-must-be-named"></a>Database in memoria devono essere denominati

È stato rimosso il database in memoria senza nome globale e invece tutti i database in memoria devono essere denominati. Ad esempio:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Si crea o si utilizza un database con nome "MyDatabase". Se `UseInMemoryDatabase` viene chiamato nuovamente con lo stesso nome, verrà utilizzato lo stesso database in memoria, in modo che possa essere condiviso da più istanze di contesto.

### <a name="read-only-api-changes"></a>Modifiche all'API di sola lettura

`IsReadOnlyBeforeSave`, `IsReadOnlyAferSave`, e `IsStoreGeneratedAlways` sono stati obsoleto e sostituito con [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) e [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55). Questi comportamenti si applicano a qualsiasi proprietà (non solo proprietà generate dall'archivio) e determinare come il valore della proprietà deve essere utilizzato durante l'inserimento in una riga di database (`BeforeSaveBehavior`) o quando l'aggiornamento di un oggetto esistente database riga (`AfterSaveBehavior`).

Le proprietà contrassegnate come [ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (ad esempio, per le colonne calcolate) per impostazione predefinita ignorerà qualsiasi valore attualmente impostato sulla proprietà. Ciò significa che un valore generato dall'archivio verrà sempre ottenuto indipendentemente dal fatto di impostare o modificato in entità rilevata di qualsiasi valore. Può essere modificato impostando un altro `Before\AfterSaveBehavior`.

### <a name="new-clientsetnull-delete-behavior"></a>Nuovo comportamento di eliminazione ClientSetNull

Nelle versioni precedenti, [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) ha un comportamento per le entità rilevato dal contesto che più chiusa corrispondente `SetNull` semantica. In Entity Framework Core 2.0, un nuovo `ClientSetNull` comportamento è stato introdotto come il valore predefinito per le relazioni facoltative. Questo comportamento è `SetNull` semantica per le entità rilevate e `Restrict` comportamento per i database creati mediante EF Core. Nella nostra esperienza, questi sono i comportamenti previsti/utili per le entità rilevate e il database. `DeleteBehavior.Restrict`viene ora applicata per le entità rilevate quando viene impostate per le relazioni facoltative.

### <a name="provider-design-time-packages-removed"></a>Pacchetti in fase di progettazione provider rimossi

Il `Microsoft.EntityFrameworkCore.Relational.Design` pacchetto è stato rimosso. Il relativo contenuto sono stato consolidato in `Microsoft.EntityFrameworkCore.Relational` e `Microsoft.EntityFrameworkCore.Design`.

Propaga nei pacchetti in fase di progettazione del provider. Tali pacchetti (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`e così via) sono state rimosse e il relativo contenuto consolidati nei pacchetti provider principale.

Per abilitare `Scaffold-DbContext` o `dotnet ef dbcontext scaffold` EF Core 2.0, è sufficiente fanno riferimento al pacchetto unico provider:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
