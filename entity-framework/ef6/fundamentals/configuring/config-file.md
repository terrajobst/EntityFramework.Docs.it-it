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
# <a name="configuration-file-settings"></a><span data-ttu-id="e5388-102">Impostazioni del File di configurazione</span><span class="sxs-lookup"><span data-stu-id="e5388-102">Configuration File Settings</span></span>
<span data-ttu-id="e5388-103">Entity Framework consente una serie di impostazioni per essere specificati dal file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="e5388-103">Entity Framework allows a number of settings to be specified from the configuration file.</span></span> <span data-ttu-id="e5388-104">In genere EF segue un principio 'convention over configuration': tutte le impostazioni descritte in questo post di un comportamento predefinito, è solo necessario preoccuparsi di modifica dell'impostazione quando il valore predefinito non soddisfa più i requisiti.</span><span class="sxs-lookup"><span data-stu-id="e5388-104">In general EF follows a ‘convention over configuration’ principle: all the settings discussed in this post have a default behavior, you only need to worry about changing the setting when the default no longer satisfies your requirements.</span></span>  

## <a name="a-code-based-alternative"></a><span data-ttu-id="e5388-105">Un'alternativa basata su codice</span><span class="sxs-lookup"><span data-stu-id="e5388-105">A Code-Based Alternative</span></span>  

<span data-ttu-id="e5388-106">Tutte queste impostazioni possono inoltre applicate usando il codice.</span><span class="sxs-lookup"><span data-stu-id="e5388-106">All of these settings can also be applied using code.</span></span> <span data-ttu-id="e5388-107">A partire da Entity Framework 6 è stata introdotta [configurazione basata su codice](code-based.md), che fornisce un modo centrale dell'applicazione di configurazione dal codice.</span><span class="sxs-lookup"><span data-stu-id="e5388-107">Starting in EF6 we introduced [code-based configuration](code-based.md), which provides a central way of applying configuration from code.</span></span> <span data-ttu-id="e5388-108">Prima di EF6, è comunque possibile applicare configurazione dal codice, ma è necessario usare varie API per configurare le diverse aree.</span><span class="sxs-lookup"><span data-stu-id="e5388-108">Prior to EF6, configuration can still be applied from code but you need to use various APIs to configure different areas.</span></span> <span data-ttu-id="e5388-109">L'opzione di file di configurazione consente a queste impostazioni possono essere facilmente modificati durante la distribuzione senza l'aggiornamento del codice.</span><span class="sxs-lookup"><span data-stu-id="e5388-109">The configuration file option allows these settings to be easily changed during deployment without updating your code.</span></span>

## <a name="the-entity-framework-configuration-section"></a><span data-ttu-id="e5388-110">La sezione di configurazione di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e5388-110">The Entity Framework Configuration Section</span></span>  

<span data-ttu-id="e5388-111">A partire da EF4.1 è possibile impostare l'inizializzatore del database per un contesto con il **appSettings** sezione del file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="e5388-111">Starting with EF4.1 you could set the database initializer for a context using the **appSettings** section of the configuration file.</span></span> <span data-ttu-id="e5388-112">4.3 di Entity Framework è stato introdotto l'oggetto personalizzato **entityFramework** sezione per gestire le nuove impostazioni.</span><span class="sxs-lookup"><span data-stu-id="e5388-112">In EF 4.3 we introduced the custom **entityFramework** section to handle the new settings.</span></span> <span data-ttu-id="e5388-113">Entity Framework riconosceranno ancora gli inizializzatori di database impostati utilizzando il formato precedente, ma è consigliabile spostare nel nuovo formato, dove possibile.</span><span class="sxs-lookup"><span data-stu-id="e5388-113">Entity Framework will still recognize database initializers set using the old format, but we recommend moving to the new format where possible.</span></span>

<span data-ttu-id="e5388-114">Il **entityFramework** sezione è stato aggiunto automaticamente al file di configurazione del progetto durante l'installazione del pacchetto EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="e5388-114">The **entityFramework** section was automatically added to the configuration file of your project when you installed the EntityFramework NuGet package.</span></span>  

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

## <a name="connection-strings"></a><span data-ttu-id="e5388-115">Stringhe di connessione</span><span class="sxs-lookup"><span data-stu-id="e5388-115">Connection Strings</span></span>  

<span data-ttu-id="e5388-116">[Questa pagina](~/ef6/fundamentals/configuring/connection-strings.md) vengono fornite informazioni dettagliate sul modo in cui Entity Framework determina il database da usare, incluse le stringhe di connessione nel file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="e5388-116">[This page](~/ef6/fundamentals/configuring/connection-strings.md) provides more details on how Entity Framework determines the database to be used, including connection strings in the configuration file.</span></span>  

<span data-ttu-id="e5388-117">Le stringhe di connessione passare nello standard **connectionStrings** elemento e non richiedono la **entityFramework** sezione.</span><span class="sxs-lookup"><span data-stu-id="e5388-117">Connection strings go in the standard **connectionStrings** element and do not require the **entityFramework** section.</span></span>  

<span data-ttu-id="e5388-118">Modelli di codice basato innanzitutto utilizzano normali stringhe di connessione ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="e5388-118">Code First based models use normal ADO.NET connection strings.</span></span> <span data-ttu-id="e5388-119">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e5388-119">For example:</span></span>  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

<span data-ttu-id="e5388-120">Finestra di progettazione di Entity Framework basato su modelli usare stringhe di connessione speciale Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e5388-120">EF Designer based models use special EF connection strings.</span></span> <span data-ttu-id="e5388-121">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="e5388-121">For example:</span></span>  

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

## <a name="code-based-configuration-type-ef6-onwards"></a><span data-ttu-id="e5388-122">Tipo di configurazione basato sul codice (6 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="e5388-122">Code-Based Configuration Type (EF6 Onwards)</span></span>  

<span data-ttu-id="e5388-123">A partire da Entity Framework 6, è possibile specificare per Entity Framework da usare per il DbConfiguration [configurazione basata su codice](code-based.md) nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e5388-123">Starting with EF6, you can specify the DbConfiguration for EF to use for [code-based configuration](code-based.md) in your application.</span></span> <span data-ttu-id="e5388-124">Nella maggior parte dei casi non occorre specificare questa impostazione come Entity Framework rileverà automaticamente i DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="e5388-124">In most cases you don't need to specify this setting as EF will automatically discover your DbConfiguration.</span></span> <span data-ttu-id="e5388-125">Per i dettagli di quando potrebbe essere necessario specificare DbConfiguration nel file di configurazione, vedere la **lo spostamento DbConfiguration** sezione del [configurazione basata su codice](code-based.md).</span><span class="sxs-lookup"><span data-stu-id="e5388-125">For details of when you may need to specify DbConfiguration in your config file see the **Moving DbConfiguration** section of [Code-Based Configuration](code-based.md).</span></span>  

<span data-ttu-id="e5388-126">Per impostare un tipo DbConfiguration, si specifica il nome completo del tipo dell'assembly nel **codeConfigurationType** elemento.</span><span class="sxs-lookup"><span data-stu-id="e5388-126">To set a DbConfiguration type, you specify the assembly qualified type name in the **codeConfigurationType** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="e5388-127">Nome di assembly completo è il nome completo dello spazio dei nomi, seguito da una virgola, quindi l'assembly che il tipo si trova in.</span><span class="sxs-lookup"><span data-stu-id="e5388-127">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="e5388-128">Facoltativamente è possibile specificare anche la versione dell'assembly, delle impostazioni cultura e token di chiave pubblica.</span><span class="sxs-lookup"><span data-stu-id="e5388-128">You can optionally also specify the assembly version, culture and public key token.</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a><span data-ttu-id="e5388-129">Provider di Database EF (6 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="e5388-129">EF Database Providers (EF6 Onwards)</span></span>  

<span data-ttu-id="e5388-130">Prima di EF6, parti specifiche di Entity Framework di un provider di database dovevano essere inclusi come parte del provider ADO.NET core.</span><span class="sxs-lookup"><span data-stu-id="e5388-130">Prior to EF6, Entity Framework-specific parts of a database provider had to be included as part of the core ADO.NET provider.</span></span> <span data-ttu-id="e5388-131">A partire da Entity Framework 6, le parti specifiche di Entity Framework vengono ora gestite e registrate separatamente.</span><span class="sxs-lookup"><span data-stu-id="e5388-131">Starting with EF6, the EF specific parts are now managed and registered separately.</span></span>  

<span data-ttu-id="e5388-132">In genere non è necessario registrare manualmente i provider.</span><span class="sxs-lookup"><span data-stu-id="e5388-132">Normally you won't need to register providers yourself.</span></span> <span data-ttu-id="e5388-133">Questo verrà in genere essere eseguito dal provider durante l'installazione.</span><span class="sxs-lookup"><span data-stu-id="e5388-133">This will typically be done by the provider when you install it.</span></span>  

<span data-ttu-id="e5388-134">I provider vengono registrati tramite l'inclusione di un **provider** elemento sotto il **provider** sezione figlio del **entityFramework** sezione.</span><span class="sxs-lookup"><span data-stu-id="e5388-134">Providers are registered by including a **provider** element under the **providers** child section of the **entityFramework** section.</span></span> <span data-ttu-id="e5388-135">Esistono due attributi obbligatori per una voce del provider:</span><span class="sxs-lookup"><span data-stu-id="e5388-135">There are two required attributes for a provider entry:</span></span>  

- <span data-ttu-id="e5388-136">**invariantName** identifica il provider ADO.NET core da questo destinazioni di provider di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e5388-136">**invariantName** identifies the core ADO.NET provider that this EF provider targets</span></span>  
- <span data-ttu-id="e5388-137">**tipo** è il nome completo del tipo di assembly di implementazione del provider di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e5388-137">**type** is the assembly qualified type name of the EF provider implementation</span></span>  

> [!NOTE]
> <span data-ttu-id="e5388-138">Nome di assembly completo è il nome completo dello spazio dei nomi, seguito da una virgola, quindi l'assembly che il tipo si trova in.</span><span class="sxs-lookup"><span data-stu-id="e5388-138">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="e5388-139">Facoltativamente è possibile specificare anche la versione dell'assembly, delle impostazioni cultura e token di chiave pubblica.</span><span class="sxs-lookup"><span data-stu-id="e5388-139">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="e5388-140">Ad esempio, ecco la voce creata per registrare il provider di SQL Server predefinita, quando si installa Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e5388-140">As an example here is the entry created to register the default SQL Server provider when you install Entity Framework.</span></span>  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a><span data-ttu-id="e5388-141">Intercettori (EF6.1 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="e5388-141">Interceptors (EF6.1 Onwards)</span></span>  

<span data-ttu-id="e5388-142">A partire da EF6.1 è possibile registrare gli intercettori nel file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="e5388-142">Starting with EF6.1 you can register interceptors in the configuration file.</span></span> <span data-ttu-id="e5388-143">Gli intercettori consentono di eseguire la logica aggiuntiva quando Entity Framework consente di eseguire determinate operazioni, ad esempio l'esecuzione di query di database, aprire le connessioni e così via.</span><span class="sxs-lookup"><span data-stu-id="e5388-143">Interceptors allow you to run additional logic when EF performs certain operations, such as executing database queries, opening connections, etc.</span></span>  

<span data-ttu-id="e5388-144">Gli intercettori vengono registrati tramite l'inclusione di un' **intercettore** elemento sotto il **intercettori** sezione figlio del **entityFramework** sezione.</span><span class="sxs-lookup"><span data-stu-id="e5388-144">Interceptors are registered by including an **interceptor** element under the **interceptors** child section of the **entityFramework** section.</span></span> <span data-ttu-id="e5388-145">Ad esempio, la configurazione seguente registra l'oggetto incorporato **DatabaseLogger** degli intercettori che vengono registrate tutte le operazioni di database nella console.</span><span class="sxs-lookup"><span data-stu-id="e5388-145">For example, the following configuration registers the built-in **DatabaseLogger** interceptor that will log all database operations to the Console.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a><span data-ttu-id="e5388-146">La registrazione operazioni di Database in un File (EF6.1 e versioni successive)</span><span class="sxs-lookup"><span data-stu-id="e5388-146">Logging Database Operations to a File (EF6.1 Onwards)</span></span>  

<span data-ttu-id="e5388-147">La registrazione di intercettori tramite il file di configurazione è particolarmente utile quando si desidera aggiungere la registrazione a un'applicazione esistente per eseguire il debug di un problema.</span><span class="sxs-lookup"><span data-stu-id="e5388-147">Registering interceptors via the config file is especially useful when you want to add logging to an existing application to help debug an issue.</span></span> <span data-ttu-id="e5388-148">**DatabaseLogger** supporta la registrazione in un file specificando il nome del file come parametro costruttore.</span><span class="sxs-lookup"><span data-stu-id="e5388-148">**DatabaseLogger** supports logging to a file by supplying the file name as a constructor parameter.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="e5388-149">Per impostazione predefinita in questo modo il file di log devono essere sovrascritti con un nuovo file ogni volta che l'app viene avviata.</span><span class="sxs-lookup"><span data-stu-id="e5388-149">By default this will cause the log file to be overwritten with a new file each time the app starts.</span></span> <span data-ttu-id="e5388-150">Per aggiungere invece al log file se esiste già usare simile:</span><span class="sxs-lookup"><span data-stu-id="e5388-150">To instead append to the log file if it already exists use something like:</span></span>  

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

<span data-ttu-id="e5388-151">Per altre informazioni sul **DatabaseLogger** e la registrazione degli intercettori, vedere il blog post [6.1 EF: attivazione della registrazione senza ricompilare](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span><span class="sxs-lookup"><span data-stu-id="e5388-151">For additional information on **DatabaseLogger** and registering interceptors, see the blog post [EF 6.1: Turning on logging without recompiling](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span></span>  

## <a name="code-first-default-connection-factory"></a><span data-ttu-id="e5388-152">Factory di connessione predefinita primo codice</span><span class="sxs-lookup"><span data-stu-id="e5388-152">Code First Default Connection Factory</span></span>  

<span data-ttu-id="e5388-153">La sezione di configurazione consente di specificare una factory di connessione predefinito che Code First deve usare per individuare un database da utilizzare per un contesto.</span><span class="sxs-lookup"><span data-stu-id="e5388-153">The configuration section allows you to specify a default connection factory that Code First should use to locate a database to use for a context.</span></span> <span data-ttu-id="e5388-154">La factory di connessione predefinita viene usata solo quando non è stata aggiunta alcuna stringa di connessione nel file di configurazione per un contesto.</span><span class="sxs-lookup"><span data-stu-id="e5388-154">The default connection factory is only used when no connection string has been added to the configuration file for a context.</span></span>  

<span data-ttu-id="e5388-155">Quando è stato installato il pacchetto NuGet di Entity Framework è stata registrata una factory di connessione predefinita che punta a SQL Express o LocalDB, a seconda di quale è stato installato.</span><span class="sxs-lookup"><span data-stu-id="e5388-155">When you installed the EF NuGet package a default connection factory was registered that points to either SQL Express or LocalDB, depending on which one you have installed.</span></span>  

<span data-ttu-id="e5388-156">Per impostare una factory di connessione, si specifica il nome completo del tipo dell'assembly nel **deafultConnectionFactory** elemento.</span><span class="sxs-lookup"><span data-stu-id="e5388-156">To set a connection factory, you specify the assembly qualified type name in the **deafultConnectionFactory** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="e5388-157">Nome di assembly completo è il nome completo dello spazio dei nomi, seguito da una virgola, quindi l'assembly che il tipo si trova in.</span><span class="sxs-lookup"><span data-stu-id="e5388-157">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="e5388-158">Facoltativamente è possibile specificare anche la versione dell'assembly, delle impostazioni cultura e token di chiave pubblica.</span><span class="sxs-lookup"><span data-stu-id="e5388-158">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="e5388-159">Di seguito è riportato un esempio di impostazione di una propria factory di connessione predefinita:</span><span class="sxs-lookup"><span data-stu-id="e5388-159">Here is an example of setting your own default connection factory:</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

<span data-ttu-id="e5388-160">Nell'esempio precedente richiede la factory personalizzata per avere un costruttore senza parametri.</span><span class="sxs-lookup"><span data-stu-id="e5388-160">The above example requires the custom factory to have a parameterless constructor.</span></span> <span data-ttu-id="e5388-161">Se necessario, è possibile specificare i parametri del costruttore usando il **parametri** elemento.</span><span class="sxs-lookup"><span data-stu-id="e5388-161">If needed, you can specify constructor parameters using the **parameters** element.</span></span>  

<span data-ttu-id="e5388-162">Il SqlCeConnectionFactory, che è incluso in Entity Framework, ad esempio, richiede di specificare un nome invariante del provider al costruttore.</span><span class="sxs-lookup"><span data-stu-id="e5388-162">For example, the SqlCeConnectionFactory, that is included in Entity Framework, requires you to supply a provider invariant name to the constructor.</span></span> <span data-ttu-id="e5388-163">Nome invariante del provider identifica la versione di SQL Compact si desidera utilizzare.</span><span class="sxs-lookup"><span data-stu-id="e5388-163">The provider invariant name identifies the version of SQL Compact you want to use.</span></span> <span data-ttu-id="e5388-164">La configurazione seguente causerà contesti per usare la versione di SQL Compact 4.0 per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="e5388-164">The following configuration will cause contexts to use SQL Compact version 4.0 by default.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="e5388-165">Se non si imposta una factory di connessione predefinita, Code First utilizza SqlConnectionFactory, che punta a `.\SQLEXPRESS`.</span><span class="sxs-lookup"><span data-stu-id="e5388-165">If you don’t set a default connection factory, Code First uses the SqlConnectionFactory, pointing to `.\SQLEXPRESS`.</span></span> <span data-ttu-id="e5388-166">SqlConnectionFactory ha anche un costruttore che consente di ignorare parti della stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="e5388-166">SqlConnectionFactory also has a constructor that allows you to override parts of the connection string.</span></span> <span data-ttu-id="e5388-167">Se si desidera utilizzare un'istanza di SQL Server diverso da `.\SQLEXPRESS` è possibile utilizzare questo costruttore per impostare il server.</span><span class="sxs-lookup"><span data-stu-id="e5388-167">If you want to use a SQL Server instance other than `.\SQLEXPRESS` you can use this constructor to set the server.</span></span>  

<span data-ttu-id="e5388-168">La configurazione seguente causerà Code First per utilizzare **ServerDatabase** per contesti che non sono una stringa di connessione esplicita impostato.</span><span class="sxs-lookup"><span data-stu-id="e5388-168">The following configuration will cause Code First to use **MyDatabaseServer** for contexts that don’t have an explicit connection string set.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="e5388-169">Per impostazione predefinita, si presuppone che gli argomenti del costruttore siano di tipo stringa.</span><span class="sxs-lookup"><span data-stu-id="e5388-169">By default, it’s assumed that constructor arguments are of type string.</span></span> <span data-ttu-id="e5388-170">Per modificare questa impostazione, è possibile utilizzare l'attributo di tipo.</span><span class="sxs-lookup"><span data-stu-id="e5388-170">You can use the type attribute to change this.</span></span>  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a><span data-ttu-id="e5388-171">Inizializzatori di database</span><span class="sxs-lookup"><span data-stu-id="e5388-171">Database Initializers</span></span>  

<span data-ttu-id="e5388-172">Gli inizializzatori di database sono configurati in base al contesto.</span><span class="sxs-lookup"><span data-stu-id="e5388-172">Database initializers are configured on a per-context basis.</span></span> <span data-ttu-id="e5388-173">Possono essere impostate in file di configurazione utilizzando il **contesto** elemento.</span><span class="sxs-lookup"><span data-stu-id="e5388-173">They can be set in the configuration file using the **context** element.</span></span> <span data-ttu-id="e5388-174">Questo elemento Usa il nome completo dell'assembly per identificare il contesto di configurazione in corso.</span><span class="sxs-lookup"><span data-stu-id="e5388-174">This element uses the assembly qualified name to identify the context being configured.</span></span>  

<span data-ttu-id="e5388-175">Per impostazione predefinita, i contesti di Code First sono configurati per usare l'inizializzatore CreateDatabaseIfNotExists.</span><span class="sxs-lookup"><span data-stu-id="e5388-175">By default, Code First contexts are configured to use the CreateDatabaseIfNotExists initializer.</span></span> <span data-ttu-id="e5388-176">È presente una **disableDatabaseInitialization** attributo il **contesto** elemento che può essere usato per disabilitare l'inizializzazione del database.</span><span class="sxs-lookup"><span data-stu-id="e5388-176">There is a **disableDatabaseInitialization** attribute on the **context** element that can be used to disable database initialization.</span></span>  

<span data-ttu-id="e5388-177">Ad esempio, la configurazione seguente disabilita l'inizializzazione del database per il contesto Blogging.BlogContext definito myAssembly. dll.</span><span class="sxs-lookup"><span data-stu-id="e5388-177">For example, the following configuration disables database initialization for the Blogging.BlogContext context defined in MyAssembly.dll.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

<span data-ttu-id="e5388-178">È possibile usare la **databaseInitializer** elemento per cui impostare un inizializzatore personalizzato.</span><span class="sxs-lookup"><span data-stu-id="e5388-178">You can use the **databaseInitializer** element to set a custom initializer.</span></span>  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

<span data-ttu-id="e5388-179">I parametri del costruttore usano la stessa sintassi come factory di connessione predefinito.</span><span class="sxs-lookup"><span data-stu-id="e5388-179">Constructor parameters use the same syntax as default connection factories.</span></span>  

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

<span data-ttu-id="e5388-180">È possibile configurare uno degli inizializzatori di database generico incluse in Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e5388-180">You can configure one of the generic database initializers that are included in Entity Framework.</span></span> <span data-ttu-id="e5388-181">Il **tipo** attributo Usa il formato di .NET Framework per i tipi generici.</span><span class="sxs-lookup"><span data-stu-id="e5388-181">The **type** attribute uses the .NET Framework format for generic types.</span></span>  

<span data-ttu-id="e5388-182">Ad esempio, se si usa migrazioni Code First, è possibile configurare il database da migrare automaticamente utilizzando il `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` inizializzatore.</span><span class="sxs-lookup"><span data-stu-id="e5388-182">For example, if you are using Code First Migrations, you can configure the database to be migrated automatically using the `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` initializer.</span></span>  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
