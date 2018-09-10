---
title: L'aggiornamento da versioni precedenti a EF Core 2 - EF Core
author: divega
ms.date: 8/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 18c7fc841affc2776d054e447aa231a5f4bcd585
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251180"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Aggiornamento di applicazioni rispetto alle versioni precedenti di EF Core 2.0

Abbiamo colto l'opportunità per perfezionare in modo significativo le API esistenti e i comportamenti in 2.0. Esistono alcuni miglioramenti che possono richiedere la modifica del codice dell'applicazione esistente, anche se Microsoft ritiene che per la maggior parte delle applicazioni l'impatto è bassa, nella maggior parte dei casi che richiedono solo la ricompilazione e modifiche guidate minime per sostituire API obsolete.

## <a name="procedures-common-to-all-applications"></a>Procedure comuni a tutte le applicazioni

Può richiedere l'aggiornamento di un'applicazione esistente a EF Core 2.0:

1. L'aggiornamento la piattaforma .NET di destinazione dell'applicazione che supporta .NET Standard 2.0. Visualizzare [piattaforme supportate](../platforms/index.md) per altri dettagli.

2. Identificare un provider per il database di destinazione che è compatibile con EF Core 2.0. Visualizzare [EF Core 2.0 richiede un provider di 2.0 database](#ef-core-20-requires-a-20-database-provider) sotto.

3. L'aggiornamento di tutti i pacchetti di EF Core (runtime e degli strumenti) 2.0. Fare riferimento a [installazione di EF Core](../get-started/install/index.md) per altri dettagli.

4. Apportare modifiche al codice necessarie per compensare le modifiche di rilievo. Vedere le [modifiche di rilievo nelle](#breaking-changes) sezione di seguito per altri dettagli.

## <a name="aspnet-core-applications"></a>Applicazioni ASP.NET Core

1. In particolare vedere il [nuovo modello per l'inizializzazione del provider del servizio dell'applicazione](#new-way-of-getting-application-services) descritto di seguito.

> [!TIP]  
> L'adozione di questo nuovo modello durante l'aggiornamento di applicazioni a 2.0 è altamente consigliabile ed è necessaria per un funzionamento delle funzionalità del prodotto, ad esempio le migrazioni di Entity Framework Core. L'altra alternativa comune consiste [implementare *IDesignTimeDbContextFactory\<TContext >*](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

2. Le applicazioni associate ad ASP.NET Core 2.0 possono usare EF Core 2.0 senza dipendenze aggiuntive oltre ai provider di database di terze parti. Tuttavia, le applicazioni destinate a versioni precedenti di ASP.NET Core è necessario eseguire l'aggiornamento ad ASP.NET Core 2.0 per usare EF Core 2.0. Per altre informazioni sull'aggiornamento delle applicazioni ASP.NET Core 2.0, vedere [documentazione di ASP.NET Core in materia](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).

## <a name="new-way-of-getting-application-services"></a>Nuovo modo per ottenere i servizi delle applicazioni

Il modello consigliato per applicazioni web ASP.NET Core è stato aggiornato per 2.0 in modo che ha interrotto la logica in fase di progettazione che EF Core usata nella versione 1.x. In precedenza in fase di progettazione, EF Core tenta di richiamare `Startup.ConfigureServices` direttamente per accedere ai provider di servizi dell'applicazione. In ASP.NET Core 2.0, configurazione viene inizializzata di fuori del `Startup` classe. Le applicazioni usando Entity Framework Core in genere accedere relativa stringa di connessione dalla configurazione, pertanto `Startup` da solo non è più sufficiente. Se si aggiorna un'applicazione di ASP.NET Core 1.x, si potrebbe ricevere l'errore seguente quando si usano gli strumenti di Entity Framework Core.

> In 'ApplicationContext' è stato trovato alcun costruttore senza parametri. Aggiungere un costruttore senza parametri per 'ApplicationContext' o aggiungere un'implementazione di ' IDesignTimeDbContextFactory&lt;ApplicationContext&gt;' nello stesso assembly come 'ApplicationContext'

È stato aggiunto un nuovo hook in fase di progettazione nel modello predefinito di ASP.NET Core 2.0. Il metodo statico `Program.BuildWebHost` metodo consente a Entity Framework Core accedere ai provider di servizi dell'applicazione in fase di progettazione. Se si esegue l'aggiornamento di un'applicazione di ASP.NET Core 1.x, è necessario fornire aggiornamenti `Program` classe simile a quella seguente.

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

## <a name="idbcontextfactory-renamed"></a>IDbContextFactory rinominato

Per supportare i modelli di applicazione diversi e concedere agli utenti maggiore controllo sulle modalità di loro `DbContext` viene usato in fase di progettazione, è possibile, in passato, forniti dal `IDbContextFactory<TContext>` interfaccia. In fase di progettazione, gli strumenti di EF Core saranno individuare le implementazioni di questa interfaccia nel progetto e usarlo per creare `DbContext` oggetti.

Questa interfaccia ha un nome molto generico che trarre in inganno ad alcuni utenti di provare a usarla nuovamente per altri `DbContext`-creazione di scenari. Sono state colta impreparata quando gli strumenti di Entity Framework ha cercato di utilizzare l'implementazione in fase di progettazione e comandi, ad esempio causati `Update-Database` o `dotnet ef database update` esito negativo.

Per poter comunicare la semantica in fase di progettazione forte di questa interfaccia, è stato rinominato in `IDesignTimeDbContextFactory<TContext>`.

Per la versione 2.0 di rilascio di `IDbContextFactory<TContext>` esiste ancora, ma è contrassegnato come obsoleto.

## <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions rimosso

A causa di modifiche di ASP.NET Core 2.0 descritte in precedenza, abbiamo scoperto che `DbContextFactoryOptions` non era più necessario nel nuovo `IDesignTimeDbContextFactory<TContext>` interfaccia. Ecco le alternative che è consigliabile usare invece.

| DbContextFactoryOptions | Alternativa                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext.BaseDirectory                                     |
| ContentRootPath         | Directory.GetCurrentDirectory()                              |
| EnvironmentName         | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") |

## <a name="design-time-working-directory-changed"></a>Directory di lavoro in fase di progettazione modificata

Le modifiche di ASP.NET Core 2.0 necessarie anche la directory di lavoro usata da `dotnet ef` in modo da allinearsi con la directory di lavoro usata da Visual Studio quando si esegue l'applicazione. Un osservabile degli effetti collaterali è tale SQLite nomi di file sono ora relativa rispetto alla directory del progetto e non alla directory di output, ad esempio essere.

## <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2.0 richiede un provider di 2.0 database

Per EF Core 2.0 sono stati apportati molti miglioramenti nel provider di database modo e semplificazioni di lavoro. Ciò significa che i provider di 1.0.x e 1.1.x non funzioneranno con EF Core 2.0.

I provider di SQLite e SQL Server vengono forniti dal team di Entity Framework e le 2.0 versioni saranno disponibili come parte di 2.0 release. I provider di terze parti open source per [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), e [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) vengono aggiornati per 2.0. Per tutti gli altri provider, contatta il writer di provider.

## <a name="logging-and-diagnostics-events-have-changed"></a>Sono stati modificati gli eventi di registrazione e diagnostica

Nota: queste modifiche non dovrebbero influire sulla gran parte del codice dell'applicazione.

L'ID dell'evento per i messaggi inviati a un [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) sono stati modificati in 2.0. Gli ID evento sono ora univoci nel codice EF Core. Questi messaggi ora seguono inoltre il modello standard per la registrazione strutturata usato, ad esempio, da MVC.

Anche le categorie del logger sono state modificate. È ora disponibile un set di categorie ben noto accessibile tramite [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) eventi ora utilizzano gli stessi nomi di ID evento corrispondenti `ILogger` messaggi. I payload dell'evento sono tutti i tipi nominali derivati da [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs).

ID evento, tipi di payload e le categorie sono documentate nel [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) e il [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) classi.

Gli ID sono anche spostati da Microsoft.EntityFrameworkCore.Infraestructure allo spazio dei nomi Microsoft.EntityFrameworkCore.Diagnostics nuovo.

## <a name="ef-core-relational-metadata-api-changes"></a>Modifiche di metadati relazionali API Entity Framework Core

EF Core 2.0 ora compila un elemento [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) diverso per ogni provider in uso. L'operazione è in genere trasparente per l'applicazione. Questa scelta ha agevolato la semplificazione delle API di metadati di livello inferiore in modo tale che l'accesso ai _concetti di metadati relazionali comuni_ viene sempre eseguito tramite una chiamata a `.Relational` anziché `.SqlServer`, `.Sqlite` e così via. Ad esempio, 1.1.x codice simile al seguente:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

A questo punto devono essere scritti come segue:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Invece di usare metodi quali `ForSqlServerToTable`, i metodi di estensione sono ora disponibili per scrivere codice condizionale in base al provider corrente in uso. Ad esempio:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Si noti che questa modifica si applica solo per le API o i metadati definiti per _tutti_ provider relazionale. L'API e i metadati rimane invariato quando è specifico per un singolo provider. Ad esempio, gli indici cluster sono specifici di SQL Server, in modo `ForSqlServerIsClustered` e `.SqlServer().IsClustered()` deve comunque essere usato.

## <a name="dont-take-control-of-the-ef-service-provider"></a>Non assumere il controllo del provider di servizi di Entity Framework

EF Core Usa interna `IServiceProvider` (un contenitore di inserimento delle dipendenze) per l'implementazione interna. Le applicazioni dovrebbero consentire di EF Core creare e gestire il provider tranne in casi particolari. Consigliabile prendere in considerazione la rimozione di tutte le chiamate a `UseInternalServiceProvider`. Se un'applicazione è necessario chiamare `UseInternalServiceProvider`, quindi prendere in considerazione [segnalare un problema](https://github.com/aspnet/EntityFramework/Issues) perché sia possibile analizzare altri modi per gestire lo scenario.

La chiamata `AddEntityFramework`, `AddEntityFrameworkSqlServer`, e così via non è richiesto dal codice dell'applicazione a meno che non `UseInternalServiceProvider` viene anche chiamato. Rimuovere le eventuali chiamate a `AddEntityFramework` oppure `AddEntityFrameworkSqlServer`e così via `AddDbContext` deve comunque essere usato nello stesso modo come indicato in precedenza.

## <a name="in-memory-databases-must-be-named"></a>Database in memoria devono essere denominati

È stato rimosso il database in memoria senza nome globale e invece tutti i database in memoria devono essere denominati. Ad esempio:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Si crea o si usa un database con il nome "MyDatabase". Se `UseInMemoryDatabase` viene chiamato nuovamente con lo stesso nome, verrà usato lo stesso database in memoria, in modo che possa essere condiviso da più istanze di contesto.

## <a name="read-only-api-changes"></a>Modifiche delle API di sola lettura

`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`, e `IsStoreGeneratedAlways` sono stati obsoleto e sostituito con [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) e [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55). Questi comportamenti si applicano a qualsiasi proprietà (non solo proprietà generate dall'archivio) e determinare come il valore della proprietà deve essere utilizzato durante l'inserimento in una riga di database (`BeforeSaveBehavior`) o quando l'aggiornamento di un oggetto esistente database riga (`AfterSaveBehavior`).

Le proprietà contrassegnate come [ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (ad esempio, per le colonne calcolate) per impostazione predefinita ignorerà qualsiasi valore impostato sulla proprietà. Ciò significa che un valore generato dall'archivio verrà ottenuto sempre indipendentemente dal fatto qualsiasi valore è stato impostato o modificato nell'entità rilevate. Può essere modificato impostando un altro `Before\AfterSaveBehavior`.

## <a name="new-clientsetnull-delete-behavior"></a>Nuovo comportamento di eliminazione ClientSetNull

Nelle versioni precedenti [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) era un comportamento per le entità rilevate dal contesto che più chiusa corrispondente `SetNull` semantica. In EF Core 2.0, un nuovo `ClientSetNull` comportamento è stato introdotto come predefinito per le relazioni facoltative. Questo comportamento è rappresentato `SetNull` semantica per le entità rilevate e `Restrict` comportamento per i database creati mediante Entity Framework Core. Nella nostra esperienza, questi sono i comportamenti previsti/utili per le entità rilevate e il database. `DeleteBehavior.Restrict` viene ora rispettata per le entità rilevate quando viene impostate per le relazioni facoltative.

## <a name="provider-design-time-packages-removed"></a>Pacchetti in fase di progettazione di provider rimossi

Il `Microsoft.EntityFrameworkCore.Relational.Design` pacchetto è stato rimosso. Il relativo contenuto sono stato consolidato in `Microsoft.EntityFrameworkCore.Relational` e `Microsoft.EntityFrameworkCore.Design`.

Ciò viene propagato nei pacchetti in fase di progettazione del provider. Tali pacchetti (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`e così via) sono state rimosse e i relativi contenuti consolidati nei pacchetti di principale provider.

Per abilitare `Scaffold-DbContext` o `dotnet ef dbcontext scaffold` in EF Core 2.0, è sufficiente fare riferimento al pacchetto unico provider:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
