---
title: Aggiornamento da versioni precedenti a EF Core 2-EF Core
author: divega
ms.date: 08/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 1222f10811914f65822a49e18522c287ece12174
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306496"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Aggiornamento di applicazioni da versioni precedenti a EF Core 2,0

Abbiamo avuto la possibilità di affinare in modo significativo le API e i comportamenti esistenti in 2,0. Ci sono alcuni miglioramenti che possono richiedere la modifica del codice dell'applicazione esistente, sebbene ritengano che per la maggior parte delle applicazioni l'effetto sarà basso, nella maggior parte dei casi la necessità di ricompilare solo e modifiche guidate minime per sostituire le API obsolete.

L'aggiornamento di un'applicazione esistente a EF Core 2,0 potrebbe richiedere:

1. Aggiornamento dell'implementazione .NET di destinazione dell'applicazione a una che supporta .NET Standard 2,0. Per informazioni dettagliate, vedere implementazioni di [.NET supportate](../platforms/index.md) .

2. Identificare un provider per il database di destinazione compatibile con EF Core 2,0. Vedere [EF Core 2,0 richiede un provider di database 2,0 di](#ef-core-20-requires-a-20-database-provider) seguito.

3. Aggiornamento di tutti i pacchetti di EF Core (Runtime e strumenti) a 2,0. Per altri dettagli, vedere [installazione di EF Core](../get-started/install/index.md) .

4. Apportare le modifiche necessarie al codice per compensare le modifiche di rilievo descritte nel resto di questo documento.

## <a name="aspnet-core-now-includes-ef-core"></a>ASP.NET Core include ora EF Core

Le applicazioni associate ad ASP.NET Core 2.0 possono usare EF Core 2.0 senza dipendenze aggiuntive oltre ai provider di database di terze parti. Tuttavia, le applicazioni destinate alle versioni precedenti di ASP.NET Core necessario eseguire l'aggiornamento a ASP.NET Core 2,0 per poter utilizzare EF Core 2,0. Per ulteriori informazioni sull'aggiornamento delle applicazioni ASP.NET Core a 2,0, vedere [la documentazione ASP.NET Core sull'argomento](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).

## <a name="new-way-of-getting-application-services-in-aspnet-core"></a>Un nuovo modo per ottenere i servizi delle applicazioni in ASP.NET Core

Il modello consigliato per le applicazioni Web ASP.NET Core è stato aggiornato per 2,0 in modo da comportare la logica della fase di progettazione EF Core utilizzata in 1. x. In precedenza, in fase di progettazione, EF Core tenterà di `Startup.ConfigureServices` richiamare direttamente per accedere al provider di servizi dell'applicazione. In ASP.NET Core 2,0, la `Startup` configurazione viene inizializzata all'esterno della classe. Le applicazioni che usano EF core in genere accedono alla relativa stringa `Startup` di connessione dalla configurazione, quindi da solo non è più sufficiente. Se si aggiorna un'applicazione ASP.NET Core 1. x, è possibile che venga visualizzato l'errore seguente quando si usano gli strumenti di EF Core.

> Nessun costruttore senza parametri trovato in ' ApplicationContext '. Aggiungere un costruttore senza parametri a' ApplicationContext ' o aggiungere un'implementazione di ' IDesignTimeDbContextFactory&lt;ApplicationContext&gt;' nello stesso assembly di ' ApplicationContext '

Nel modello predefinito di ASP.NET Core 2.0 è stato aggiunto un nuovo hook della fase di progettazione. Il metodo `Program.BuildWebHost` statico consente EF Core di accedere al provider di servizi dell'applicazione in fase di progettazione. Se si sta aggiornando un'applicazione ASP.NET Core 1. x, sarà necessario aggiornare la `Program` classe in modo che sia simile alla seguente.

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

L'adozione di questo nuovo modello quando si aggiornano le applicazioni a 2,0 è altamente consigliata ed è necessaria per le funzionalità del prodotto, ad esempio Entity Framework Core le migrazioni funzionano. L'altra alternativa comune consiste nell' [implementare *IDesignTimeDbContextFactory\<TContext >* ](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

## <a name="idbcontextfactory-renamed"></a>IDbContextFactory rinominato

Per supportare diversi modelli di applicazione e fornire agli utenti un maggiore controllo sulle modalità `DbContext` di utilizzo di tali modelli in fase di progettazione, in passato è stata fornita `IDbContextFactory<TContext>` l'interfaccia. In fase di progettazione, gli strumenti di EF Core rileveranno le implementazioni di questa interfaccia nel progetto e la utilizzeranno per creare `DbContext` oggetti.

Questa interfaccia ha un nome molto generale che induce in inganno alcuni utenti a provare a riutilizzarlo per altri `DbContext`scenari di creazione. Sono stati rilevati quando gli strumenti EF tentavano di usare la propria implementazione in fase di progettazione e causavano `Update-Database` la `dotnet ef database update` mancata esecuzione di comandi come o.

Per comunicare la semantica avanzata della fase di progettazione di questa interfaccia, è `IDesignTimeDbContextFactory<TContext>`stata rinominata in.

Per la versione `IDbContextFactory<TContext>` 2,0, esiste ancora, ma è contrassegnato come obsoleto.

## <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions rimosso

A causa delle modifiche ASP.NET Core 2,0 descritte in precedenza, è stato `DbContextFactoryOptions` rilevato che non era più necessario sulla `IDesignTimeDbContextFactory<TContext>` nuova interfaccia. Ecco le alternative da usare.

| DbContextFactoryOptions | Alternativa                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext.BaseDirectory                                     |
| ContentRootPath         | Directory.GetCurrentDirectory()                              |
| EnvironmentName         | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") |

## <a name="design-time-working-directory-changed"></a>Directory di lavoro della fase di progettazione modificata

Le modifiche apportate ASP.NET Core 2,0 richiedono anche la directory `dotnet ef` di lavoro utilizzata da per allinearsi alla directory di lavoro utilizzata da Visual Studio durante l'esecuzione dell'applicazione. Un effetto collaterale osservabile è che i nomi dei file SQLite sono ora relativi alla directory del progetto e non alla directory di output come sono stati usati.

## <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2,0 richiede un provider di database 2,0

Per EF Core 2,0 sono state apportate numerose semplificazioni e miglioramenti nel modo in cui funzionano i provider di database. Ciò significa che i provider 1.0. x e 1.1. x non funzioneranno con EF Core 2,0.

I provider SQL Server e SQLite vengono spediti dal team EF e le versioni 2,0 saranno disponibili come parte della versione 2,0. I provider di terze parti open source per [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL)e [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) vengono aggiornati per 2,0. Per tutti gli altri provider, contattare il writer del provider.

## <a name="logging-and-diagnostics-events-have-changed"></a>Gli eventi di registrazione e diagnostica sono stati modificati

Nota: queste modifiche non dovrebbero avere un effetto sulla maggior parte del codice dell'applicazione.

Gli ID evento per i messaggi inviati a un [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) sono stati modificati in 2,0. Gli ID evento sono ora univoci nel codice EF Core. Questi messaggi ora seguono inoltre il modello standard per la registrazione strutturata usato, ad esempio, da MVC.

Anche le categorie del logger sono state modificate. È ora disponibile un set di categorie ben noto accessibile tramite [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

Gli eventi [DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) ora utilizzano gli stessi nomi di ID evento dei `ILogger` messaggi corrispondenti. I payload dell'evento sono tutti tipi nominali derivati da [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs).

Gli ID evento, i tipi di payload e le categorie sono documentati nelle classi [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) e [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) .

Gli ID sono stati spostati anche da Microsoft. EntityFrameworkCore. Infrastructure al nuovo spazio dei nomi Microsoft. EntityFrameworkCore. Diagnostics.

## <a name="ef-core-relational-metadata-api-changes"></a>EF Core modifiche API dei metadati relazionali

EF Core 2.0 ora compila un elemento [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) diverso per ogni provider in uso. L'operazione è in genere trasparente per l'applicazione. Questa scelta ha agevolato la semplificazione delle API di metadati di livello inferiore in modo tale che l'accesso ai _concetti di metadati relazionali comuni_ viene sempre eseguito tramite una chiamata a `.Relational` anziché `.SqlServer`, `.Sqlite` e così via. Ad esempio, il codice 1.1. x è simile al seguente:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Dovrebbe ora essere scritto come segue:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Anziché usare metodi come `ForSqlServerToTable`, i metodi di estensione sono ora disponibili per scrivere codice condizionale basato sul provider corrente in uso. Ad esempio:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Si noti che questa modifica si applica solo alle API/metadati definiti per _tutti i_ provider relazionali. L'API e i metadati rimangono invariati quando sono specifici di un singolo provider. Gli indici cluster, ad esempio, sono specifici del server SQL `ForSqlServerIsClustered` , `.SqlServer().IsClustered()` quindi è necessario utilizzare e.

## <a name="dont-take-control-of-the-ef-service-provider"></a>Non assumere il controllo del provider di servizi EF

EF Core usa un contenitore `IServiceProvider` interno (un contenitore di inserimento di dipendenze) per la relativa implementazione interna. Le applicazioni devono consentire EF Core di creare e gestire questo provider, tranne nei casi particolari. È consigliabile rimuovere tutte le chiamate `UseInternalServiceProvider`a. Se un'applicazione deve chiamare `UseInternalServiceProvider`, provare a [presentare un problema](https://github.com/aspnet/EntityFramework/Issues) per poter esaminare altri modi per gestire lo scenario.

La `AddEntityFramework`chiamata `AddEntityFrameworkSqlServer`di, e così via non è richiesta dal codice dell' `UseInternalServiceProvider` applicazione a meno che non venga chiamato anche. Rimuovere qualsiasi chiamata esistente a `AddEntityFramework` o `AddEntityFrameworkSqlServer`e così via `AddDbContext` . deve essere comunque utilizzata come prima.

## <a name="in-memory-databases-must-be-named"></a>I database in memoria devono essere denominati

Il database in memoria globale senza nome è stato rimosso, ma è necessario assegnare un nome a tutti i database in memoria. Ad esempio:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Viene creato un database con il nome "database". Se `UseInMemoryDatabase` viene chiamato di nuovo con lo stesso nome, verrà usato lo stesso database in memoria, in modo che possa essere condiviso da più istanze di contesto.

## <a name="read-only-api-changes"></a>Modifiche all'API di sola lettura

`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`e `IsStoreGeneratedAlways` sono stati obsoleti e sostituiti con [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) e [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55). Questi comportamenti si applicano a qualsiasi proprietà (non solo alle proprietà generate dall'archivio) e determinano il modo in cui il valore della proprietà deve essere utilizzato quando si`BeforeSaveBehavior`inserisce in una riga di database () o`AfterSaveBehavior`quando si aggiorna una riga di database esistente ().

Per impostazione predefinita, le proprietà contrassegnate come [ValueGenerated. OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (ad esempio per le colonne calcolate) ignoreranno tutti i valori attualmente impostati nella proprietà. Ciò significa che un valore generato dall'archivio sarà sempre ottenuto indipendentemente dal fatto che un valore sia stato impostato o modificato sull'entità rilevata. Questo può essere modificato impostando un diverso `Before\AfterSaveBehavior`.

## <a name="new-clientsetnull-delete-behavior"></a>Nuovo comportamento di eliminazione ClientSetNull

Nelle versioni precedenti, [DeleteBehavior. Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) aveva un comportamento per le entità rilevate dal contesto che ha più chiuso `SetNull` la semantica corrispondente. In EF Core 2,0 è stato introdotto `ClientSetNull` un nuovo comportamento come impostazione predefinita per le relazioni facoltative. Questo comportamento ha `SetNull` la semantica per le entità rilevate e `Restrict` il comportamento per i database creati con EF core. In questa esperienza, questi sono i comportamenti più attesi/utili per le entità rilevate e il database. `DeleteBehavior.Restrict`viene ora rispettato per le entità rilevate se impostate per le relazioni facoltative.

## <a name="provider-design-time-packages-removed"></a>Pacchetti della fase di progettazione del provider rimossi

Il `Microsoft.EntityFrameworkCore.Relational.Design` pacchetto è stato rimosso. I contenuti sono stati consolidati `Microsoft.EntityFrameworkCore.Design`in `Microsoft.EntityFrameworkCore.Relational` e.

Questa operazione viene propagata nei pacchetti della fase di progettazione del provider. Questi pacchetti (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`e così via) sono stati rimossi e il relativo contenuto è stato consolidato nei pacchetti del provider principali.

Per abilitare `Scaffold-DbContext` o `dotnet ef dbcontext scaffold` in EF Core 2,0, è sufficiente fare riferimento al pacchetto del singolo provider:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
