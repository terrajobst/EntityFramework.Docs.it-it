---
title: Impostazioni del File di configurazione - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
caps.latest.revision: 3
ms.openlocfilehash: 24faed6bdae6cc4b31808dc5bbebddb94128d0d3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120861"
---
# <a name="configuration-file-settings"></a>Impostazioni del File di configurazione
Entity Framework consente una serie di impostazioni per essere specificati dal file di configurazione. In genere EF segue un principio 'convention over configuration': tutte le impostazioni descritte in questo post di un comportamento predefinito, è solo necessario preoccuparsi di modifica dell'impostazione quando il valore predefinito non soddisfa più i requisiti.  

## <a name="a-code-based-alternative"></a>Un'alternativa basata su codice  

Tutte queste impostazioni possono inoltre applicate usando il codice. A partire da Entity Framework 6 è stata introdotta [configurazione basata su codice](code-based.md), che fornisce un modo centrale dell'applicazione di configurazione dal codice. Prima di EF6, è comunque possibile applicare configurazione dal codice, ma è necessario usare varie API per configurare le diverse aree. L'opzione di file di configurazione consente a queste impostazioni possono essere facilmente modificati durante la distribuzione senza l'aggiornamento del codice.

## <a name="the-entity-framework-configuration-section"></a>La sezione di configurazione di Entity Framework  

A partire da EF4.1 è possibile impostare l'inizializzatore del database per un contesto con il **appSettings** sezione del file di configurazione. 4.3 di Entity Framework è stato introdotto l'oggetto personalizzato **entityFramework** sezione per gestire le nuove impostazioni. Entity Framework riconosceranno ancora gli inizializzatori di database impostati utilizzando il formato precedente, ma è consigliabile spostare nel nuovo formato, dove possibile.

Il **entityFramework** sezione è stato aggiunto automaticamente al file di configurazione del progetto durante l'installazione del pacchetto EntityFramework NuGet.  

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
    <section name="entityFramework"
       type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=4.3.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
  </configSections>
</configuration>
```  

## <a name="connection-strings"></a>Stringhe di connessione  

[Questa pagina](~/ef6/fundamentals/configuring/connection-strings.md) vengono fornite informazioni dettagliate sul modo in cui Entity Framework determina il database da usare, incluse le stringhe di connessione nel file di configurazione.  

Le stringhe di connessione passare nello standard **connectionStrings** elemento e non richiedono la **entityFramework** sezione.  

Modelli di codice basato innanzitutto utilizzano normali stringhe di connessione ADO.NET. Ad esempio:  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

Finestra di progettazione di Entity Framework basato su modelli usare stringhe di connessione speciale Entity Framework. Ad esempio:  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a>Tipo di configurazione basato sul codice (6 e versioni successive)  

A partire da Entity Framework 6, è possibile specificare per Entity Framework da usare per il DbConfiguration [configurazione basata su codice](code-based.md) nell'applicazione. Nella maggior parte dei casi non occorre specificare questa impostazione come Entity Framework rileverà automaticamente i DbConfiguration. Per i dettagli di quando potrebbe essere necessario specificare DbConfiguration nel file di configurazione, vedere la **lo spostamento DbConfiguration** sezione del [configurazione basata su codice](code-based.md).  

Per impostare un tipo DbConfiguration, si specifica il nome completo del tipo dell'assembly nel **codeConfigurationType** elemento.  

> [!NOTE]
> Nome di assembly completo è il nome completo dello spazio dei nomi, seguito da una virgola, quindi l'assembly che il tipo si trova in. Facoltativamente è possibile specificare anche la versione dell'assembly, delle impostazioni cultura e token di chiave pubblica.  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a>Provider di Database EF (6 e versioni successive)  

Prima di EF6, parti specifiche di Entity Framework di un provider di database dovevano essere inclusi come parte del provider ADO.NET core. A partire da Entity Framework 6, le parti specifiche di Entity Framework vengono ora gestite e registrate separatamente.  

In genere non è necessario registrare manualmente i provider. Questo verrà in genere essere eseguito dal provider durante l'installazione.  

I provider vengono registrati tramite l'inclusione di un **provider** elemento sotto il **provider** sezione figlio del **entityFramework** sezione. Esistono due attributi obbligatori per una voce del provider:  

- **invariantName** identifica il provider ADO.NET core da questo destinazioni di provider di Entity Framework  
- **tipo** è il nome completo del tipo di assembly di implementazione del provider di Entity Framework  

> [!NOTE]
> Nome di assembly completo è il nome completo dello spazio dei nomi, seguito da una virgola, quindi l'assembly che il tipo si trova in. Facoltativamente è possibile specificare anche la versione dell'assembly, delle impostazioni cultura e token di chiave pubblica.  

Ad esempio, ecco la voce creata per registrare il provider di SQL Server predefinita, quando si installa Entity Framework.  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a>Intercettori (EF6.1 e versioni successive)  

A partire da EF6.1 è possibile registrare gli intercettori nel file di configurazione. Gli intercettori consentono di eseguire la logica aggiuntiva quando Entity Framework consente di eseguire determinate operazioni, ad esempio l'esecuzione di query di database, aprire le connessioni e così via.  

Gli intercettori vengono registrati tramite l'inclusione di un' **intercettore** elemento sotto il **intercettori** sezione figlio del **entityFramework** sezione. Ad esempio, la configurazione seguente registra l'oggetto incorporato **DatabaseLogger** degli intercettori che vengono registrate tutte le operazioni di database nella console.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a>La registrazione operazioni di Database in un File (EF6.1 e versioni successive)  

La registrazione di intercettori tramite il file di configurazione è particolarmente utile quando si desidera aggiungere la registrazione a un'applicazione esistente per eseguire il debug di un problema. **DatabaseLogger** supporta la registrazione in un file specificando il nome del file come parametro costruttore.  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

Per impostazione predefinita in questo modo il file di log devono essere sovrascritti con un nuovo file ogni volta che l'app viene avviata. Per aggiungere invece al log file se esiste già usare simile:  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
      <parameter value="true" type="System.Boolean"/>
    </parameters>
  </interceptor>
</interceptors>
```  

Per altre informazioni sul **DatabaseLogger** e la registrazione degli intercettori, vedere il blog post [6.1 EF: attivazione della registrazione senza ricompilare](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).  

## <a name="code-first-default-connection-factory"></a>Factory di connessione predefinita primo codice  

La sezione di configurazione consente di specificare una factory di connessione predefinito che Code First deve usare per individuare un database da utilizzare per un contesto. La factory di connessione predefinita viene usata solo quando non è stata aggiunta alcuna stringa di connessione nel file di configurazione per un contesto.  

Quando è stato installato il pacchetto NuGet di Entity Framework è stata registrata una factory di connessione predefinita che punta a SQL Express o LocalDB, a seconda di quale è stato installato.  

Per impostare una factory di connessione, si specifica il nome completo del tipo dell'assembly nel **deafultConnectionFactory** elemento.  

> [!NOTE]
> Nome di assembly completo è il nome completo dello spazio dei nomi, seguito da una virgola, quindi l'assembly che il tipo si trova in. Facoltativamente è possibile specificare anche la versione dell'assembly, delle impostazioni cultura e token di chiave pubblica.  

Di seguito è riportato un esempio di impostazione di una propria factory di connessione predefinita:  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

Nell'esempio precedente richiede la factory personalizzata per avere un costruttore senza parametri. Se necessario, è possibile specificare i parametri del costruttore usando il **parametri** elemento.  

Il SqlCeConnectionFactory, che è incluso in Entity Framework, ad esempio, richiede di specificare un nome invariante del provider al costruttore. Nome invariante del provider identifica la versione di SQL Compact si desidera utilizzare. La configurazione seguente causerà contesti per usare la versione di SQL Compact 4.0 per impostazione predefinita.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Se non si imposta una factory di connessione predefinita, Code First utilizza SqlConnectionFactory, che punta a `.\SQLEXPRESS`. SqlConnectionFactory ha anche un costruttore che consente di ignorare parti della stringa di connessione. Se si desidera utilizzare un'istanza di SQL Server diverso da `.\SQLEXPRESS` è possibile utilizzare questo costruttore per impostare il server.  

La configurazione seguente causerà Code First per utilizzare **ServerDatabase** per contesti che non sono una stringa di connessione esplicita impostato.  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

Per impostazione predefinita, si presuppone che gli argomenti del costruttore siano di tipo stringa. Per modificare questa impostazione, è possibile utilizzare l'attributo di tipo.  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a>Inizializzatori di database  

Gli inizializzatori di database sono configurati in base al contesto. Possono essere impostate in file di configurazione utilizzando il **contesto** elemento. Questo elemento Usa il nome completo dell'assembly per identificare il contesto di configurazione in corso.  

Per impostazione predefinita, i contesti di Code First sono configurati per usare l'inizializzatore CreateDatabaseIfNotExists. È presente una **disableDatabaseInitialization** attributo il **contesto** elemento che può essere usato per disabilitare l'inizializzazione del database.  

Ad esempio, la configurazione seguente disabilita l'inizializzazione del database per il contesto Blogging.BlogContext definito myAssembly. dll.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

È possibile usare la **databaseInitializer** elemento per cui impostare un inizializzatore personalizzato.  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

I parametri del costruttore usano la stessa sintassi come factory di connessione predefinito.  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly">
      <parameters>
        <parameter value="MyConstructorParameter" />
      </parameters>
    </databaseInitializer>
  </context>
</contexts>
```  

È possibile configurare uno degli inizializzatori di database generico incluse in Entity Framework. Il **tipo** attributo Usa il formato di .NET Framework per i tipi generici.  

Ad esempio, se si usa migrazioni Code First, è possibile configurare il database da migrare automaticamente utilizzando il `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` inizializzatore.  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
