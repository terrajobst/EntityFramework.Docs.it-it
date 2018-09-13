---
title: Risoluzione delle dipendenze - Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 6082124481f5795bbcb62fff2bb6a58ecdcb48e4
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490961"
---
# <a name="dependency-resolution"></a>Risoluzione delle dipendenze
> [!NOTE]
> **Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6. Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.  

A partire da Entity Framework 6, Entity Framework contiene un meccanismo per utilizzo generico per il recupero delle implementazioni dei servizi richiesti. Vale a dire quando Entity Framework Usa un'istanza di alcune interfacce o classi di base verrà richiesto per un'implementazione concreta della classe di base o interfaccia da utilizzare. Questo risultato viene ottenuto tramite l'uso dell'interfaccia IDbDependencyResolver:  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

Il metodo GetService solitamente viene chiamato da Entity Framework e viene gestito da un'implementazione di IDbDependencyResolver forniti da Entity Framework o tramite l'applicazione. Quando viene chiamato, l'argomento tipo è il tipo di classe base o interfaccia del servizio di richiesta e l'oggetto chiave è null o un oggetto che fornisce informazioni contestuali relative al servizio richiesto.  

Se non indicato diversamente qualsiasi oggetto restituito deve essere thread-safe perché può essere utilizzata come un singleton. In molti casi che l'oggetto restituito è una factory nel qual caso la factory stesso deve essere thread-safe, ma l'oggetto restituito dalla factory non dovrà essere thread-safe in quanto viene richiesta una nuova istanza dalla factory per ogni utilizzo.

Questo articolo non contiene i dettagli completi per implementare IDbDependencyResolver, ma funge invece da un riferimento per i tipi di servizio (vale a dire, la base e delle interfacce tipi di classe) per il quale EF chiamate GetService e la semantica dell'oggetto chiave per ciascuno di essi chiamate.

## <a name="systemdataentityidatabaseinitializertcontext"></a>System.Data.Entity.IDatabaseInitializer < TContext\>  

**Versione introdotta**: EF6.0.0  

**Oggetto restituito**: un inizializzatore di database per il tipo di contesto specificato  

**Chiave**: non usato, sarà null  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a>Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\>  

**Versione introdotta**: EF6.0.0

**Oggetto restituito**: una factory per creare un generatore di SQL che può essere utilizzato per le migrazioni e altre azioni che causano un database da creare, ad esempio la creazione del database con gli inizializzatori di database.  

**Chiave**: una stringa contenente il nome invariante del provider ADO.NET che specifica il tipo di database per il quale verrà generato SQL. Ad esempio, viene restituito il generatore SQL di SQL Server per la chiave "SqlClient".  

>[!NOTE]
> Per altre informazioni sui relativi al provider di servizi in EF6, vedere la [modello di provider di Entity Framework 6](~/ef6/fundamentals/providers/provider-model.md) sezione.  

## <a name="systemdataentitycorecommondbproviderservices"></a>System.Data.Entity.Core.Common.DbProviderServices  

**Versione introdotta**: EF6.0.0  

**Oggetto restituito**: provider di Entity Framework da utilizzare per un nome invariante del provider specificato  

**Chiave**: una stringa contenente il nome invariante del provider ADO.NET che specifica il tipo di database per cui è necessario un provider. Ad esempio, viene restituito il provider SQL Server per la chiave "SqlClient".  

>[!NOTE]
> Per altre informazioni sui relativi al provider di servizi in EF6, vedere la [modello di provider di Entity Framework 6](~/ef6/fundamentals/providers/provider-model.md) sezione.  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a>System.Data.Entity.Infrastructure.IDbConnectionFactory  

**Versione introdotta**: EF6.0.0  

**Oggetto restituito**: la factory di connessione che verrà utilizzata quando Entity Framework crea una connessione al database per convenzione. Vale a dire quando nessuna connessione o una stringa di connessione viene assegnato a Entity Framework, e nessuna stringa di connessione sono reperibili nel file app. config o Web. config, quindi questo servizio consente di creare una connessione per convenzione. Modificare la factory di connessione può consentire di Entity Framework da usare un tipo diverso di database (ad esempio, SQL Server Compact Edition) per impostazione predefinita.  

**Chiave**: non usato, sarà null  

>[!NOTE]
> Per altre informazioni sui relativi al provider di servizi in EF6, vedere la [modello di provider di Entity Framework 6](~/ef6/fundamentals/providers/provider-model.md) sezione.  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a>System.Data.Entity.Infrastructure.IManifestTokenService  

**Versione introdotta**: EF6.0.0  

**Oggetto restituito**: un servizio che può generare un token del manifesto del provider da una connessione. Questo servizio viene utilizzato in genere in due modi. In primo luogo, può essere utilizzato per evitare la connessione al database quando si compila un modello Code First. In secondo luogo, può essere utilizzato per forzare Code First per compilare un modello per una versione di database specifico, ad esempio, per forzare un modello per SQL Server 2005, anche se in alcuni casi viene usato SQL Server 2008.  

**Durata dell'oggetto**: Singleton, l'oggetto stesso può essere utilizzato più volte e in simultanea da thread diversi  

**Chiave**: non usato, sarà null  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a>System.Data.Entity.Infrastructure.IDbProviderFactoryService  

**Versione introdotta**: EF6.0.0  

**Oggetto restituito**: un servizio che è possibile ottenere una factory del provider da una determinata connessione. In .NET 4.5, il provider è accessibile pubblicamente dalla connessione. In .NET 4 l'implementazione predefinita di questo servizio Usa alcune regole euristiche per trovare il provider corrisponda. Se questi non riescono quindi una nuova implementazione di questo servizio può essere registrato per fornire una risoluzione appropriata.  

**Chiave**: non usato, sarà null  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a>Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\>  

**Versione introdotta**: EF6.0.0  

**Oggetto restituito**: una factory che genera una chiave di cache del modello per un determinato contesto. Per impostazione predefinita, Entity Framework memorizza nella cache di un modello per ogni tipo DbContext per ogni provider. Un'implementazione diversa di questo servizio è utilizzabile per aggiungere altre informazioni, ad esempio nome dello schema, alla chiave di cache.  

**Chiave**: non usato, sarà null  

## <a name="systemdataentityspatialdbspatialservices"></a>System.Data.Entity.Spatial.DbSpatialServices  

**Versione introdotta**: EF6.0.0  

**Oggetto restituito**: provider spaziali di Entity Framework che aggiunge il supporto per il provider di Entity Framework di base per i tipi spaziali di geografia e geometria.  

**Chiave**: DbSptialServices viene chiesta in due modi. In primo luogo, specifico del provider servizi spaziali vengono richiesti utilizzando un oggetto DbProviderInfo (che contiene invariante token del manifesto e nome) come chiave. In secondo luogo, è possibile chiedere DbSpatialServices per senza chiave. Questo consente di risolvere il "globale spaziale provider" che viene usato quando si creano tipi di DbGeography o DbGeometry autonomi.  

>[!NOTE]
> Per altre informazioni sui relativi al provider di servizi in EF6, vedere la [modello di provider di Entity Framework 6](~/ef6/fundamentals/providers/provider-model.md) sezione.  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a>Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\>  

**Versione introdotta**: EF6.0.0  

**Oggetto restituito**: una factory per creare un servizio che consente a un provider implementare nuovi tentativi o altri comportamenti quando vengono eseguiti query e comandi sul database. Se non viene fornita alcuna implementazione, quindi EF verranno semplicemente eseguire i comandi e propaga le eccezioni generate. Per SQL Server questo servizio viene utilizzato per fornire un criterio di ripetizione dei tentativi che risulta particolarmente utile durante l'esecuzione per i server di database basato sul cloud, ad esempio SQL Azure.  

**Chiave**: ExecutionStrategyKey un oggetto che contiene il nome invariante del provider e, facoltativamente, un nome di server per il quale verrà utilizzata la strategia di esecuzione.  

>[!NOTE]
> Per altre informazioni sui relativi al provider di servizi in EF6, vedere la [modello di provider di Entity Framework 6](~/ef6/fundamentals/providers/provider-model.md) sezione.  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a>Func < DbConnection, stringa, System.Data.Entity.Migrations.History.HistoryContext\>  

**Versione introdotta**: EF6.0.0  

**Oggetto restituito**: una factory che consente a un provider configurare il mapping del HistoryContext al `__MigrationHistory` tabella utilizzata dal migrazioni di Entity Framework. Il HistoryContext è un primo oggetto DbContext di codice e può essere configurato usando l'API fluent normale per modificare elementi come il nome della tabella e le specifiche di mapping di colonna.  

**Chiave**: non usato, sarà null  

>[!NOTE]
> Per altre informazioni sui relativi al provider di servizi in EF6, vedere la [modello di provider di Entity Framework 6](~/ef6/fundamentals/providers/provider-model.md) sezione.  

## <a name="systemdatacommondbproviderfactory"></a>DbProviderFactory  

**Versione introdotta**: EF6.0.0  

**Oggetto restituito**: ADO.NET il provider da utilizzare per un nome invariante del provider specificato.  

**Chiave**: una stringa contenente il nome invariante del provider ADO.NET  

>[!NOTE]
> Questo servizio non viene in genere modificato direttamente poiché l'implementazione predefinita Usa la registrazione del provider ADO.NET normale. Per altre informazioni sui relativi al provider di servizi in EF6, vedere la [modello di provider di Entity Framework 6](~/ef6/fundamentals/providers/provider-model.md) sezione.  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a>System.Data.Entity.Infrastructure.IProviderInvariantName  

**Versione introdotta**: EF6.0.0  

**Oggetto restituito**: un servizio che consente di determinare un nome invariante del provider per un determinato tipo di oggetto DbProviderFactory. L'implementazione predefinita di questo servizio Usa la registrazione del provider ADO.NET. Ciò significa che se il provider ADO.NET non è registrato in modo normale perché è in fase di risoluzione DbProviderFactory da Entity Framework, quindi verrà anche essere necessario risolvere questo servizio.  

**Chiave**: istanza di DbProviderFactory per il quale è richiesto un nome invariante.  

>[!NOTE]
> Per altre informazioni sui relativi al provider di servizi in EF6, vedere la [modello di provider di Entity Framework 6](~/ef6/fundamentals/providers/provider-model.md) sezione.  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a>System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache  

**Versione introdotta**: EF6.0.0  

**Oggetto restituito**: una cache di assembly che contengono le visualizzazioni pregenerate. Sostituzione viene generalmente utilizzata per informare EF gli assembly che contengono le visualizzazioni pregenerate non esegue qualsiasi individuazione.  

**Chiave**: non usato, sarà null  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a>System.Data.Entity.Infrastructure.Pluralization.IPluralizationService

**Versione introdotta**: EF6.0.0  

**Oggetto restituito**: un servizio usato da Entity Framework per rendere plurali e singolari i nomi. Per impostazione predefinita viene usato un servizio di pluralizzazione in lingua inglese.  

**Chiave**: non usato, sarà null  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a>System.Data.Entity.Infrastructure.Interception.IDbInterceptor  

**Versione introdotta**: EF6.0.0

**Gli oggetti restituiti**: qualsiasi degli intercettori che devono essere registrati all'avvio dell'applicazione. Si noti che questi oggetti vengono richiesti tramite la chiamata GetServices e tutti gli intercettori restituiti da qualsiasi sistema di risoluzione delle dipendenze verranno registrato.

**Chiave**: non usato, sarà null.  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a>Func < DbContext, azione < stringa\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\>  

**Versione introdotta**: EF6.0.0  

**Oggetto restituito**: una factory che verrà utilizzata per creare il formattatore di log del database che verrà usata quando il contesto. Database. log viene impostata nel contesto specificato.  

**Chiave**: non usato, sarà null.  

## <a name="funcsystemdataentitydbcontext"></a>Func < DbContext\>  

**Versione introdotta**: EF6.1.0  

**Oggetto restituito**: una factory che verrà utilizzata per creare istanze del contesto per le migrazioni quando il contesto non dispone di un costruttore senza parametri accessibile.  

**Chiave**: oggetto del tipo per il tipo di DbContext derivato per cui è necessaria una factory.  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a>Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\>  

**Versione introdotta**: EF6.1.0  

**Oggetto restituito**: una factory che verrà utilizzata per creare i serializzatori per la serializzazione delle annotazioni personalizzate fortemente tipizzata in modo che possano essere serializzati e desterilized in XML per l'uso di migrazioni Code First.  

**Chiave**: il nome dell'annotazione che viene serializzata o deserializzata.  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a>Func < System.Data.Entity.Infrastructure.TransactionHandler\>  

**Versione introdotta**: EF6.1.0  

**Oggetto restituito**: una factory che verrà utilizzata per creare gestori per le transazioni in modo che possano essere applicata una gestione speciale per le situazioni, ad esempio la gestione degli errori di commit.  

**Chiave**: ExecutionStrategyKey un oggetto che contiene il nome invariante del provider e, facoltativamente, un nome di server per il quale verrà utilizzato il gestore delle transazioni.  
