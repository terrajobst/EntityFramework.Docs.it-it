---
title: Il modello di provider di Entity Framework 6 - Entity Framework 6
author: divega
ms.date: 06/27/2018
ms.assetid: 066832F0-D51B-4655-8BE7-C983C557E0E4
ms.openlocfilehash: de2e0a24f1b5f67d28cb831491b50d32f45af60a
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283927"
---
# <a name="the-entity-framework-6-provider-model"></a>Il modello di provider di Entity Framework 6

Il modello di provider di Entity Framework consente a Entity Framework da usare con diversi tipi di server di database. Ad esempio, un provider può essere collegato per consentire all'utilizzo con Microsoft SQL Server, mentre un altro provider può essere inserito nel consentire a Entity Framework da usare con Microsoft SQL Server Compact Edition EF. I provider per Entity Framework 6 che siamo a conoscenza di sono reperibile nella [provider di Entity Framework](~/ef6/fundamentals/providers/index.md) pagina.

Determinate modifiche erano necessari al modo in cui che EF interagisce con i provider per consentire a Entity Framework deve essere rilasciato in una licenza open source. Queste modifiche richiedono la ricompilazione dei provider di Entity Framework per gli assembly di EF6 insieme ai nuovi meccanismi per la registrazione del provider.

## <a name="rebuilding"></a>La ricompilazione

Con Entity Framework 6 ora viene fornito il codice di base che precedentemente faceva parte di .NET Framework come assembly di fuori banda (OOB). Informazioni dettagliate su come creare applicazioni rispetto a EF6 sono reperibile nella [l'aggiornamento di applicazioni per EF6](~/ef6/what-is-new/upgrading-to-ef6.md) pagina. I provider verranno anche devono essere ricompilati usando le istruzioni seguenti.

## <a name="provider-types-overview"></a>Panoramica dei tipi di provider

Un provider di Entity Framework è davvero una raccolta di servizi specifici del provider definito da tipi CLR che questi servizi estendono da (per una classe di base) o implementano (per un'interfaccia). Due di questi servizi sono fondamentali e necessaria per funzionare correttamente a Entity Framework. Gli altri sono facoltativi e devono solo essere implementati se è necessaria la funzionalità specifiche e/o le implementazioni predefinite di questi servizi non funziona per il server di database specifici vengano specificate come destinazione.

## <a name="fundamental-provider-types"></a>Tipi fondamentali provider

### <a name="dbproviderfactory"></a>Classe DbProviderFactory

Entity Framework dipende dalla presenza di un tipo derivato da [DbProviderFactory](https://msdn.microsoft.com/en-us/library/system.data.common.dbproviderfactory.aspx) per l'esecuzione di tutti gli accessi di basso livello di database. DbProviderFactory non è in realtà parte di Entity Framework, ma è invece una classe in .NET Framework che fornisce un punto di ingresso per i provider ADO.NET che può essere usata da Entity Framework, altri O/RMs o direttamente da un'applicazione per ottenere le istanze di connessioni, comandi, parametri e altre astrazioni di ADO.NET in un provider indipendente dalla modalità. Altre informazioni sulla classe DbProviderFactory un reperibile nella [documentazione di MSDN per ADO.NET](https://msdn.microsoft.com/en-us/library/a6cd7c08.aspx).

### <a name="dbproviderservices"></a>DbProviderServices

Entity Framework dipende dalla presenza di un tipo derivato da DbProviderServices per fornire funzionalità aggiuntive necessarie da Entity Framework sopra la funzionalità già fornita dal provider ADO.NET. Nelle versioni precedenti di Entity Framework la classe DbProviderServices faceva parte di .NET Framework e in spazio dei nomi è stata trovata. Avvio con Entity Framework 6 di questa classe fa ora parte del file EntityFramework. dll e sia nello spazio dei nomi System.Data.Entity.Core.Common.

Altre informazioni sulle funzionalità fondamentali di un'implementazione di DbProviderServices sono reperibile nella [MSDN](https://msdn.microsoft.com/en-us/library/ee789835.aspx). Tuttavia, si noti che al momento della scrittura di queste informazioni non viene aggiornato per EF6 Sebbene la maggior parte dei concetti sono ancora valida. Le implementazioni di SQL Server e SQL Server Compact di DbProviderServices vengono controllate anche in per il [open source codebase](https://github.com/aspnet/EntityFramework6/) e può essere usato come riferimento utile per le altre implementazioni.

Nelle versioni precedenti di Entity Framework è stato ottenuto l'implementazione di DbProviderServices per utilizzare direttamente da un provider ADO.NET. Tale operazione viene eseguita eseguendo il cast DbProviderFactory a IServiceProvider e chiamare il metodo GetService. Ciò strettamente il provider di Entity Framework per l'elemento DbProviderFactory. Questo accoppiamento bloccato EF venga spostato all'esterno di .NET Framework e pertanto per Entity Framework 6 è stato rimosso l'accoppiamento stretto e un'implementazione di DbProviderServices è ora registrata direttamente nel file di configurazione dell'applicazione o in basata su codice configurazione, come descritto più dettagliatamente la _DbProviderServices registrazione_ sezione riportata di seguito.

## <a name="additional-services"></a>Servizi aggiuntivi

Oltre ai servizi fondamentali descritti in precedenza esistono anche molti altri servizi usati da Entity Framework che sono specifiche del provider sempre o talvolta. Le implementazioni specifiche del provider predefinito di questi servizi possono essere fornite da un'implementazione di DbProviderServices. Le applicazioni possono anche sostituire le implementazioni di questi servizi, o fornire implementazioni quando un tipo di DbProviderServices non fornisce un valore predefinito. Come descritto più dettagliatamente il _risoluzione di altri servizi_ sezione riportata di seguito.

I tipi di servizio aggiuntiva che un provider può essere di interesse a un provider sono elencati di seguito. Informazioni dettagliate su ognuno di questi tipi di servizio sono reperibile nella documentazione dell'API.

### <a name="idbexecutionstrategy"></a>IDbExecutionStrategy

Si tratta di un servizio facoltativo che consente a un provider implementare nuovi tentativi o altri comportamenti quando vengono eseguiti query e comandi sul database. Se non viene fornita alcuna implementazione, quindi EF verranno semplicemente eseguire i comandi e propaga le eccezioni generate. Per SQL Server questo servizio viene utilizzato per fornire un criterio di ripetizione dei tentativi che risulta particolarmente utile durante l'esecuzione per i server di database basato sul cloud, ad esempio SQL Azure.

### <a name="idbconnectionfactory"></a>IDbConnectionFactory

Si tratta di un servizio facoltativo che consente a un provider creare oggetti DbConnection per convenzione quando viene fornito solo un nome di database. Si noti che anche se questo servizio può essere risolto da un'implementazione di DbProviderServices fin Entity Framework 4.1 e può anche essere impostata esplicitamente nel file config o nel codice. Il provider recupera solo possibilità di risolvere il servizio se registrato come provider predefinito (vedere _il provider predefinito_ sotto) e, se non è stata impostata una factory di connessione predefinito in un' posizione.

### <a name="dbspatialservices"></a>DbSpatialServices

Si tratta di un servizi facoltativi che consente a un provider aggiungere il supporto per i tipi spaziali di geografia e geometria. Affinché un'applicazione per l'uso di Entity Framework con i tipi spaziali, è necessario specificare un'implementazione di questo servizio. DbSptialServices viene chiesta in due modi. In primo luogo, specifico del provider servizi spaziali vengono richiesti utilizzando un oggetto DbProviderInfo (che contiene invariante token del manifesto e nome) come chiave. In secondo luogo, è possibile chiedere DbSpatialServices per senza chiave. Ciò consente di risolvere il "globale spaziale provider" che viene usato quando si creano tipi di DbGeography o DbGeometry autonomi.

### <a name="migrationsqlgenerator"></a>MigrationSqlGenerator

Si tratta di un servizio facoltativo che consente migrazioni di Entity Framework da utilizzare per la generazione di SQL utilizzato per la creazione e modifica di schemi di database da Code First. Un'implementazione è necessaria per supportare le migrazioni. Se viene fornita un'implementazione quindi verrà inoltre utilizzato quando i database vengono creati usando gli inizializzatori di database o il metodo Database.Create.

### <a name="funcdbconnection-string-historycontextfactory"></a>Func < DbConnection, stringa, HistoryContextFactory >

Si tratta di un servizio facoltativo che consente a un provider configurare il mapping del HistoryContext al `__MigrationHistory` tabella utilizzata dal migrazioni di Entity Framework. Il HistoryContext è un primo oggetto DbContext di codice e può essere configurato usando l'API fluent normale per modificare elementi come il nome della tabella e le specifiche di mapping di colonna. L'implementazione predefinita di questo servizio restituito da Entity Framework per tutti i provider potrebbe funzionare per un server di database specificata se tutte le tabelle e colonne mapping predefiniti sono supportati da tale provider. In tal caso il provider non è necessario fornire un'implementazione di questo servizio.

### <a name="idbproviderfactoryresolver"></a>IDbProviderFactoryResolver

Si tratta di un servizio facoltativo per l'acquisizione di DbProviderFactory corretto da un oggetto DbConnection specificato. L'implementazione predefinita di questo servizio restituito da Entity Framework per tutti i provider deve funzionare per tutti i provider. Tuttavia, durante l'esecuzione in .NET 4, l'elemento DbProviderFactory non accessibile pubblicamente da uno se relativo DbConnections. Pertanto, Entity Framework Usa alcune regole euristiche per ricercare i provider registrati per trovare una corrispondenza. È possibile che per alcuni provider di questi sistemi euristici avrà esito negativo e in questi casi il provider deve fornire una nuova implementazione.

## <a name="registering-dbproviderservices"></a>La registrazione di DbProviderServices

È possibile registrare l'implementazione di DbProviderServices da utilizzare nel file di configurazione (app. config o Web. config) o utilizzando la configurazione basata su codice dell'applicazione. In entrambi i casi la registrazione viene utilizzato il nome del provider"inglese" come chiave. In questo modo più provider registrati e l'uso in una singola applicazione. Il nome invariante usato per le registrazioni di Entity Framework è lo stesso come il nome invariante usato per le stringhe di connessione e la registrazione del provider ADO.NET. Ad esempio, per SQL Server, il nome invariante viene usato "SqlClient".

### <a name="config-file-registration"></a>Registrazione nel file di configurazione

Il tipo di DbProviderServices da utilizzare viene registrato come un elemento provider nell'elenco di provider della sezione entityFramework del file di configurazione dell'applicazione. Ad esempio:

``` xml
<entityFramework>
  <providers>
    <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
  </providers>
</entityFramework>
```

Il _tipo_ stringa deve essere il nome del tipo qualificato dall'assembly di implementazione di DbProviderServices da utilizzare.

### <a name="code-based-registration"></a>Registrazione basata su codice

A partire da Entity Framework 6 provider possono essere registrati usando il codice. In questo modo un provider di Entity Framework da usare senza apportare modifiche al file di configurazione dell'applicazione. Per utilizzare la configurazione basata su codice un'applicazione deve creare una classe DbConfiguration come descritto nel [documentazione relativa alla configurazione basata su codice](https://msdn.com/data/jj680699). Il costruttore della classe DbConfiguration deve quindi chiamare SetProviderServices per registrare il provider di Entity Framework. Ad esempio:

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetProviderServices("My.New.Provider", new MyProviderServices());
    }
}
```

## <a name="resolving-additional-services"></a>La risoluzione dei servizi aggiuntivi

Come indicato in precedenza nel _Panoramica di Provider di tipi_ sezione DbProviderServices una classe può essere utilizzata anche per risolvere i servizi aggiuntivi. Questo è possibile perché DbProviderServices implementa IDbDependencyResolver e ogni tipo di DbProviderServices registrato viene aggiunto come un resolver"predefinito". Il meccanismo di IDbDpendencyResolver è descritto più dettagliatamente [risoluzione delle dipendenze](~/ef6/fundamentals/configuring/dependency-resolution.md). Non è tuttavia necessario comprendere tutti i concetti presentati in questa specifica per risolvere i servizi aggiuntivi in un provider.

Il modo più comune per un provider risolvere i servizi aggiuntivi consiste nel chiamare DbProviderServices.AddDependencyResolver per ogni servizio nel costruttore della classe DbProviderServices. Ad esempio, SqlProviderServices (provider di Entity Framework per SQL Server) con codice simile al seguente per l'inizializzazione:

``` csharp
private SqlProviderServices()
{
    AddDependencyResolver(new SingletonDependencyResolver<IDbConnectionFactory>(
        new SqlConnectionFactory()));

    AddDependencyResolver(new ExecutionStrategyResolver<DefaultSqlExecutionStrategy>(
        "System.data.SqlClient", null, () => new DefaultSqlExecutionStrategy()));

    AddDependencyResolver(new SingletonDependencyResolver<Func<MigrationSqlGenerator>>(
        () => new SqlServerMigrationSqlGenerator(), "System.data.SqlClient"));

    AddDependencyResolver(new SingletonDependencyResolver<DbSpatialServices>(
        SqlSpatialServices.Instance,
        k =>
        {
            var asSpatialKey = k as DbProviderInfo;
            return asSpatialKey == null
                || asSpatialKey.ProviderInvariantName == ProviderInvariantName;
        }));
}
```

Questo costruttore utilizza le classi helper seguenti:

*   SingletonDependencyResolver: fornisce un modo semplice per risolvere i servizi Singleton, vale a dire, i servizi per cui la stessa istanza viene restituita ogni volta che viene chiamato GetService. Servizi temporanei spesso vengono registrati come una factory di singleton che verrà utilizzata per creare istanze temporanee su richiesta.
*   ExecutionStrategyResolver: un sistema di risoluzione specifico per la restituzione IExecutionStrategy implementazioni.

Invece di usare DbProviderServices.AddDependencyResolver è anche possibile eseguire l'override DbProviderServices.GetService e risolvere i servizi aggiuntivi direttamente. Questo metodo verrà chiamato quando Entity Framework necessita di un servizio definito da un determinato tipo e, in alcuni casi, per una determinata chiave. Il metodo deve restituire il servizio, se possibile, o restituisce null per rifiutare esplicitamente la restituzione del servizio e consentire invece un'altra classe risolvere il problema. Ad esempio, per risolvere la factory di connessione predefinita il codice nel metodo GetService potrebbe essere un aspetto simile al seguente:

``` csharp
public override object GetService(Type type, object key)
{
    if (type == typeof(IDbConnectionFactory))
    {
        return new SqlConnectionFactory();
    }
    return null;
}
```

### <a name="registration-order"></a>Ordine di registrazione

Quando più implementazioni di DbProviderServices vengono registrate nel file di configurazione dell'applicazione verranno aggiunti come resolver secondario nell'ordine in cui sono elencati. Poiché i resolver vengono aggiunti sempre nella parte superiore della catena di resolver secondario che ciò significa che il provider alla fine dell'elenco otterranno possibilità di risolvere le dipendenze prima delle altre. (Ciò può sembrare poco plausibili inizialmente, ma ha senso se si immagina prendendo ogni provider dall'elenco e lo stacking sopra i provider esistenti).

Questo ordinamento in genere non è importante perché la maggior parte dei servizi del provider sono specifiche del provider e con chiave dal nome invariante del provider. Tuttavia, per i servizi che non sono ordinate per nome invariante del provider o alcuni altri specifico del provider key che verrà risolto al servizio basato su tale ordinamento. Ad esempio, se non è esplicitamente impostata in modo diverso in un punto qualsiasi caso contrario, la factory di connessione predefinita verrà provenire dal provider di livello più alto nella catena.

## <a name="additional-config-file-registrations"></a>Registrazioni dei file di configurazione aggiuntivi

È possibile registrare in modo esplicito alcuni dei servizi di provider aggiuntivo descritti in precedenza direttamente nel file di configurazione di un'applicazione. Quando questa operazione viene eseguita la registrazione nel file di configurazione verrà utilizzata invece di un valore restituito dal metodo GetService dell'implementazione di DbProviderServices.

### <a name="registering-the-default-connection-factory"></a>Registrazione della factory di connessione predefinito

Avvio con EF5 del pacchetto EntityFramework NuGet automaticamente registrata la factory di connessione SQL Express o factory di connessione LocalDb nel file di configurazione.

Ad esempio:

``` xml
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework" >
</entityFramework>
```

Il _tipo_ è il nome del tipo qualificato dall'assembly per la factory di connessione predefinito, che deve implementare IDbConnectionFactory.

È consigliabile che un pacchetto NuGet del provider impostato la factory di connessione predefinito in questo modo durante l'installazione. Visualizzare _i pacchetti NuGet per i provider_ sotto.

## <a name="additional-ef6-provider-changes"></a>Altre modifiche di provider di Entity Framework 6

### <a name="spatial-provider-changes"></a>Modifiche apportate al provider spaziali

I provider che supportano i tipi spaziali ora devono implementare alcuni metodi aggiuntivi sulle classi che derivano da DbSpatialDataReader:

*   `public abstract bool IsGeographyColumn(int ordinal)`
*   `public abstract bool IsGeometryColumn(int ordinal)`

Esistono anche le nuove versioni asincrone dei metodi esistenti che sono consigliabile essere sostituita in quanto le implementazioni predefinite delegare ai metodi sincroni e pertanto non vengono eseguite in modo asincrono:

*   `public virtual Task<DbGeography> GetGeographyAsync(int ordinal, CancellationToken cancellationToken)`
*   `public virtual Task<DbGeometry> GetGeometryAsync(int ordinal, CancellationToken cancellationToken)`

### <a name="native-support-for-enumerablecontains"></a>Supporto nativo di Enumerable

EF6 introduce un nuovo tipo di espressione, DbInExpression, che è stato aggiunto per risolvere i problemi di prestazioni per l'uso di Enumerable nelle query LINQ. La classe DbProviderManifest dispone di un nuovo metodo virtuale, SupportsInExpression, che viene chiamato da Entity Framework per determinare se un provider gestisce il nuovo tipo di espressione. Per garantire la compatibilità con le implementazioni del provider esistente, il metodo restituisce false. Per poter usufruire di questo miglioramento, un provider di Entity Framework 6 può aggiungere codice per gestire DbInExpression ed eseguire l'override SupportsInExpression a restituire true. È possibile creare un'istanza di DbInExpression chiamando il metodo DbExpressionBuilder.In. Un'istanza di DbInExpression è costituita da un DbExpression, in genere che rappresenta una colonna di tabella e un elenco di DbConstantExpression da testare per trovare una corrispondenza.

## <a name="nuget-packages-for-providers"></a>Pacchetti NuGet per i provider

Un modo per rendere disponibile un provider di Entity Framework 6 è rilasciarlo come pacchetto NuGet. Uso di un pacchetto NuGet offre i vantaggi seguenti:

*   È facile da usare NuGet per aggiungere la registrazione del provider al file di configurazione dell'applicazione
*   È possibile apportare altre modifiche al file di configurazione per impostare la factory di connessione predefinito in modo che le connessioni stabilite dalla convenzione utilizzerà il provider registrato
*   NuGet gestisce aggiungere reindirizzamenti di associazione in modo che il provider di Entity Framework 6 continueranno a funzionare anche dopo il rilascio di un nuovo pacchetto di Entity Framework

Un esempio di questo è il pacchetto EntityFramework.SqlServerCompact incluso nel [open source codebase](http://github.com/aspnet/entityframework6). Questo pacchetto fornisce un modello ottimo per la creazione di pacchetti NuGet del provider di Entity Framework.

### <a name="powershell-commands"></a>Comandi di PowerShell

Quando viene installato il pacchetto EntityFramework NuGet registra un modulo di PowerShell che contiene i due comandi che sono molto utili per i pacchetti di provider:

*   Aggiungere-EFProvider aggiunge una nuova entità per il provider nel file di configurazione del progetto di destinazione e assicura che si trova alla fine dell'elenco dei provider registrati.
*   Aggiungere-EFDefaultConnectionFactory aggiunge o aggiorna la registrazione defaultConnectionFactory nel file di configurazione del progetto di destinazione.

Entrambi questi comandi ci occupiamo dell'aggiunta di una sezione di entityFramework al file di configurazione e una raccolta di provider se necessario.

Lo scopo è che questi comandi chiamato da install.ps1 NuGet script. Ad esempio, per il provider di SQL Compact install.ps1 è simile al seguente:

``` powershell
param($installPath, $toolsPath, $package, $project)
Add-EFDefaultConnectionFactory $project 'System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework' -ConstructorArguments 'System.Data.SqlServerCe.4.0'
Add-EFProvider $project 'System.Data.SqlServerCe.4.0' 'System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact'</pre>
```

Altre informazioni su questi comandi possono essere ottenute usando get-help nella finestra della Console di gestione pacchetti.

## <a name="wrapping-providers"></a>Provider di ritorno a capo

Un provider di ritorno a capo è un provider di Entity Framework e/o ADO.NET che esegue il wrapping di un provider esistente per estenderlo con altre funzionalità, ad esempio di profilatura o funzionalità di traccia. Ritorno a capo provider può essere registrato in modo normale, ma è spesso più pratico configurare il provider di ritorno a capo in fase di esecuzione mediante l'intercettazione la risoluzione dei servizi relativi al provider. L'evento statico OnLockingConfiguration sulla classe DbConfiguration può essere utilizzato per eseguire questa operazione.

OnLockingConfiguration viene chiamato dopo che ha determinato EF dove verrà ottenuti dal tutta la configurazione di Entity Framework per il dominio dell'applicazione ma prima che venga bloccato per l'uso. All'avvio dell'app (prima che venga utilizzato Entity Framework) l'app deve registrare un gestore eventi per questo evento. (Si sta considerando di aggiunta del supporto per la registrazione di questo gestore nel file di configurazione ma non è ancora supportato). Il gestore eventi deve quindi effettuare una chiamata a ReplaceService per ogni servizio che deve essere inserito.  

Ad esempio, per eseguire il wrapping IDbConnectionFactory e DbProviderService, un gestore simile al seguente deve essere registrato:

``` csharp
DbConfiguration.OnLockingConfiguration +=
    (_, a) =>
    {
        a.ReplaceService<DbProviderServices>(
            (s, k) => new MyWrappedProviderServices(s));

        a.ReplaceService<IDbConnectionFactory>(
            (s, k) => new MyWrappedConnectionFactory(s));
    };
```

Il servizio che è stato risolto e ora deve essere incluso insieme alla chiave che è stata usata per risolvere il servizio vengono passati al gestore. Il gestore può quindi eseguire il wrapping di questo servizio e sostituire il servizio restituito con la versione sottoposta a wrapping.

## <a name="resolving-a-dbproviderfactory-with-ef"></a>Risoluzione di un oggetto DbProviderFactory con Entity Framework

Oggetto DbProviderFactory è uno dei tipi di provider fondamentali necessari da Entity Framework, come descritto nel _Panoramica di Provider di tipi_ sezione precedente. Come già accennato, non è un tipo di Entity Framework e in genere non fa parte della configurazione di Entity Framework di registrazione, ma viene invece la registrazione del provider ADO.NET normale nel file Machine. config e/o file di configurazione dell'applicazione.

Nonostante questo EF Usa ancora il relativo meccanismo di risoluzione dipendenza normale durante la ricerca di una classe DbProviderFactory da utilizzare. Il resolver predefinito Usa la registrazione ADO.NET normale nei file di configurazione e quindi, questa è solitamente trasparente all'utente. Ma a causa di risoluzione delle dipendenze normale meccanismo viene usato significa che un IDbDependencyResolver può essere usato per risolvere una classe DbProviderFactory anche quando il normale ADO.NET registrazione non è stata eseguita.

La risoluzione di DbProviderFactory in questo modo presenta diverse implicazioni:

*   Un'applicazione utilizzando la configurazione basata su codice è possibile aggiungere le chiamate all'interno della classe DbConfiguration per registrare l'elemento DbProviderFactory in appropriato. Ciò è particolarmente utile per le applicazioni che non si desiderano (o non possono) apportare usare affatto di qualsiasi configurazione basata su file.
*   Il servizio può essere eseguito il wrapping o sostituito utilizzando ReplaceService come descritto nel _Wrapping provider_ sezione precedente
*   Un'implementazione di DbProviderServices teoricamente, è possibile risolvere una classe DbProviderFactory.

Il punto importante da notare circa effettivamente eseguendo alcuna di queste operazioni è che essi influiranno solo la ricerca di DbProviderFactory da Entity Framework. Altro codice non EF potrebbe comunque prevede che il provider ADO.NET da registrare in modo normale e può non riuscire se la registrazione non viene trovata. Per questo motivo è preferibile in genere per una classe DbProviderFactory essere registrato in modo normale ADO.NET.

### <a name="related-services"></a>Servizi correlati

Se Entity Framework viene usato per risolvere una classe DbProviderFactory, inoltre consigliabile risolvere i servizi IProviderInvariantName e IDbProviderFactoryResolver.

IProviderInvariantName è un servizio che consente di determinare un nome invariante del provider per un determinato tipo di oggetto DbProviderFactory. L'implementazione predefinita di questo servizio Usa la registrazione del provider ADO.NET. Ciò significa che se il provider ADO.NET non è registrato in modo normale perché è in fase di risoluzione DbProviderFactory da Entity Framework, quindi verrà anche essere necessario risolvere questo servizio. Si noti che un sistema di risoluzione per questo servizio viene aggiunto automaticamente quando si usa il metodo DbConfiguration.SetProviderFactory.

Come descritto nel _Panoramica di Provider di tipi_ sezione precedente, il IDbProviderFactoryResolver viene usato per ottenere l'elemento DbProviderFactory in corretto da un oggetto DbConnection specificato. L'implementazione predefinita di questo servizio durante l'esecuzione in .NET 4 utilizza la registrazione del provider ADO.NET. Ciò significa che se il provider ADO.NET non è registrato in modo normale perché è in fase di risoluzione DbProviderFactory da Entity Framework, quindi verrà anche essere necessario risolvere questo servizio.
