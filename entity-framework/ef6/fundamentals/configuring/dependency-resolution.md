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
# <a name="dependency-resolution"></a><span data-ttu-id="53ca0-102">Risoluzione delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="53ca0-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="53ca0-103">**Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="53ca0-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="53ca0-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="53ca0-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="53ca0-105">A partire da Entity Framework 6, Entity Framework contiene un meccanismo per utilizzo generico per il recupero delle implementazioni dei servizi richiesti.</span><span class="sxs-lookup"><span data-stu-id="53ca0-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="53ca0-106">Vale a dire quando Entity Framework Usa un'istanza di alcune interfacce o classi di base verrà richiesto per un'implementazione concreta della classe di base o interfaccia da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="53ca0-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="53ca0-107">Questo risultato viene ottenuto tramite l'uso dell'interfaccia IDbDependencyResolver:</span><span class="sxs-lookup"><span data-stu-id="53ca0-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="53ca0-108">Il metodo GetService solitamente viene chiamato da Entity Framework e viene gestito da un'implementazione di IDbDependencyResolver forniti da Entity Framework o tramite l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="53ca0-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="53ca0-109">Quando viene chiamato, l'argomento tipo è il tipo di classe base o interfaccia del servizio di richiesta e l'oggetto chiave è null o un oggetto che fornisce informazioni contestuali relative al servizio richiesto.</span><span class="sxs-lookup"><span data-stu-id="53ca0-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="53ca0-110">Se non indicato diversamente qualsiasi oggetto restituito deve essere thread-safe perché può essere utilizzata come un singleton.</span><span class="sxs-lookup"><span data-stu-id="53ca0-110">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="53ca0-111">In molti casi che l'oggetto restituito è una factory nel qual caso la factory stesso deve essere thread-safe, ma l'oggetto restituito dalla factory non dovrà essere thread-safe in quanto viene richiesta una nuova istanza dalla factory per ogni utilizzo.</span><span class="sxs-lookup"><span data-stu-id="53ca0-111">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>

<span data-ttu-id="53ca0-112">Questo articolo non contiene i dettagli completi per implementare IDbDependencyResolver, ma funge invece da un riferimento per i tipi di servizio (vale a dire, la base e delle interfacce tipi di classe) per il quale EF chiamate GetService e la semantica dell'oggetto chiave per ciascuno di essi chiamate.</span><span class="sxs-lookup"><span data-stu-id="53ca0-112">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span>

## <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="53ca0-113">System.Data.Entity.IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="53ca0-113">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="53ca0-114">**Versione introdotta**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="53ca0-114">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="53ca0-115">**Oggetto restituito**: un inizializzatore di database per il tipo di contesto specificato</span><span class="sxs-lookup"><span data-stu-id="53ca0-115">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="53ca0-116">**Chiave**: non usato, sarà null</span><span class="sxs-lookup"><span data-stu-id="53ca0-116">**Key**: Not used; will be null</span></span>  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="53ca0-117">Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="53ca0-117">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="53ca0-118">**Versione introdotta**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="53ca0-118">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="53ca0-119">**Oggetto restituito**: una factory per creare un generatore di SQL che può essere utilizzato per le migrazioni e altre azioni che causano un database da creare, ad esempio la creazione del database con gli inizializzatori di database.</span><span class="sxs-lookup"><span data-stu-id="53ca0-119">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="53ca0-120">**Chiave**: una stringa contenente il nome invariante del provider ADO.NET che specifica il tipo di database per il quale verrà generato SQL.</span><span class="sxs-lookup"><span data-stu-id="53ca0-120">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="53ca0-121">Ad esempio, viene restituito il generatore SQL di SQL Server per la chiave "SqlClient".</span><span class="sxs-lookup"><span data-stu-id="53ca0-121">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="53ca0-122">Per altre informazioni sui relativi al provider di servizi in EF6, vedere la [modello di provider di Entity Framework 6](~/ef6/fundamentals/providers/provider-model.md) sezione.</span><span class="sxs-lookup"><span data-stu-id="53ca0-122">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="53ca0-123">System.Data.Entity.Core.Common.DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="53ca0-123">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="53ca0-124">**Versione introdotta**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="53ca0-124">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="53ca0-125">**Oggetto restituito**: provider di Entity Framework da utilizzare per un nome invariante del provider specificato</span><span class="sxs-lookup"><span data-stu-id="53ca0-125">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="53ca0-126">**Chiave**: una stringa contenente il nome invariante del provider ADO.NET che specifica il tipo di database per cui è necessario un provider.</span><span class="sxs-lookup"><span data-stu-id="53ca0-126">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="53ca0-127">Ad esempio, viene restituito il provider SQL Server per la chiave "SqlClient".</span><span class="sxs-lookup"><span data-stu-id="53ca0-127">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="53ca0-128">Per altre informazioni sui relativi al provider di servizi in EF6, vedere la [modello di provider di Entity Framework 6](~/ef6/fundamentals/providers/provider-model.md) sezione.</span><span class="sxs-lookup"><span data-stu-id="53ca0-128">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="53ca0-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="53ca0-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="53ca0-130">**Versione introdotta**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="53ca0-130">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="53ca0-131">**Oggetto restituito**: la factory di connessione che verrà utilizzata quando Entity Framework crea una connessione al database per convenzione.</span><span class="sxs-lookup"><span data-stu-id="53ca0-131">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="53ca0-132">Vale a dire quando nessuna connessione o una stringa di connessione viene assegnato a Entity Framework, e nessuna stringa di connessione sono reperibili nel file app. config o Web. config, quindi questo servizio consente di creare una connessione per convenzione.</span><span class="sxs-lookup"><span data-stu-id="53ca0-132">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="53ca0-133">Modificare la factory di connessione può consentire di Entity Framework da usare un tipo diverso di database (ad esempio, SQL Server Compact Edition) per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="53ca0-133">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="53ca0-134">**Chiave**: non usato, sarà null</span><span class="sxs-lookup"><span data-stu-id="53ca0-134">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="53ca0-135">Per altre informazioni sui relativi al provider di servizi in EF6, vedere la [modello di provider di Entity Framework 6](~/ef6/fundamentals/providers/provider-model.md) sezione.</span><span class="sxs-lookup"><span data-stu-id="53ca0-135">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="53ca0-136">System.Data.Entity.Infrastructure.IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="53ca0-136">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="53ca0-137">**Versione introdotta**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="53ca0-137">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="53ca0-138">**Oggetto restituito**: un servizio che può generare un token del manifesto del provider da una connessione.</span><span class="sxs-lookup"><span data-stu-id="53ca0-138">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="53ca0-139">Questo servizio viene utilizzato in genere in due modi.</span><span class="sxs-lookup"><span data-stu-id="53ca0-139">This service is typically used in two ways.</span></span> <span data-ttu-id="53ca0-140">In primo luogo, può essere utilizzato per evitare la connessione al database quando si compila un modello Code First.</span><span class="sxs-lookup"><span data-stu-id="53ca0-140">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="53ca0-141">In secondo luogo, può essere utilizzato per forzare Code First per compilare un modello per una versione di database specifico, ad esempio, per forzare un modello per SQL Server 2005, anche se in alcuni casi viene usato SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="53ca0-141">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="53ca0-142">**Durata dell'oggetto**: Singleton, l'oggetto stesso può essere utilizzato più volte e in simultanea da thread diversi</span><span class="sxs-lookup"><span data-stu-id="53ca0-142">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="53ca0-143">**Chiave**: non usato, sarà null</span><span class="sxs-lookup"><span data-stu-id="53ca0-143">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="53ca0-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="53ca0-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="53ca0-145">**Versione introdotta**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="53ca0-145">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="53ca0-146">**Oggetto restituito**: un servizio che è possibile ottenere una factory del provider da una determinata connessione.</span><span class="sxs-lookup"><span data-stu-id="53ca0-146">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="53ca0-147">In .NET 4.5, il provider è accessibile pubblicamente dalla connessione.</span><span class="sxs-lookup"><span data-stu-id="53ca0-147">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="53ca0-148">In .NET 4 l'implementazione predefinita di questo servizio Usa alcune regole euristiche per trovare il provider corrisponda.</span><span class="sxs-lookup"><span data-stu-id="53ca0-148">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="53ca0-149">Se questi non riescono quindi una nuova implementazione di questo servizio può essere registrato per fornire una risoluzione appropriata.</span><span class="sxs-lookup"><span data-stu-id="53ca0-149">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="53ca0-150">**Chiave**: non usato, sarà null</span><span class="sxs-lookup"><span data-stu-id="53ca0-150">**Key**: Not used; will be null</span></span>  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="53ca0-151">Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="53ca0-151">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="53ca0-152">**Versione introdotta**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="53ca0-152">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="53ca0-153">**Oggetto restituito**: una factory che genera una chiave di cache del modello per un determinato contesto.</span><span class="sxs-lookup"><span data-stu-id="53ca0-153">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="53ca0-154">Per impostazione predefinita, Entity Framework memorizza nella cache di un modello per ogni tipo DbContext per ogni provider.</span><span class="sxs-lookup"><span data-stu-id="53ca0-154">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="53ca0-155">Un'implementazione diversa di questo servizio è utilizzabile per aggiungere altre informazioni, ad esempio nome dello schema, alla chiave di cache.</span><span class="sxs-lookup"><span data-stu-id="53ca0-155">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="53ca0-156">**Chiave**: non usato, sarà null</span><span class="sxs-lookup"><span data-stu-id="53ca0-156">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="53ca0-157">System.Data.Entity.Spatial.DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="53ca0-157">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="53ca0-158">**Versione introdotta**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="53ca0-158">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="53ca0-159">**Oggetto restituito**: provider spaziali di Entity Framework che aggiunge il supporto per il provider di Entity Framework di base per i tipi spaziali di geografia e geometria.</span><span class="sxs-lookup"><span data-stu-id="53ca0-159">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="53ca0-160">**Chiave**: DbSptialServices viene chiesta in due modi.</span><span class="sxs-lookup"><span data-stu-id="53ca0-160">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="53ca0-161">In primo luogo, specifico del provider servizi spaziali vengono richiesti utilizzando un oggetto DbProviderInfo (che contiene invariante token del manifesto e nome) come chiave.</span><span class="sxs-lookup"><span data-stu-id="53ca0-161">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="53ca0-162">In secondo luogo, è possibile chiedere DbSpatialServices per senza chiave.</span><span class="sxs-lookup"><span data-stu-id="53ca0-162">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="53ca0-163">Questo consente di risolvere il "globale spaziale provider" che viene usato quando si creano tipi di DbGeography o DbGeometry autonomi.</span><span class="sxs-lookup"><span data-stu-id="53ca0-163">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="53ca0-164">Per altre informazioni sui relativi al provider di servizi in EF6, vedere la [modello di provider di Entity Framework 6](~/ef6/fundamentals/providers/provider-model.md) sezione.</span><span class="sxs-lookup"><span data-stu-id="53ca0-164">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="53ca0-165">Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="53ca0-165">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="53ca0-166">**Versione introdotta**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="53ca0-166">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="53ca0-167">**Oggetto restituito**: una factory per creare un servizio che consente a un provider implementare nuovi tentativi o altri comportamenti quando vengono eseguiti query e comandi sul database.</span><span class="sxs-lookup"><span data-stu-id="53ca0-167">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="53ca0-168">Se non viene fornita alcuna implementazione, quindi EF verranno semplicemente eseguire i comandi e propaga le eccezioni generate.</span><span class="sxs-lookup"><span data-stu-id="53ca0-168">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="53ca0-169">Per SQL Server questo servizio viene utilizzato per fornire un criterio di ripetizione dei tentativi che risulta particolarmente utile durante l'esecuzione per i server di database basato sul cloud, ad esempio SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="53ca0-169">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="53ca0-170">**Chiave**: ExecutionStrategyKey un oggetto che contiene il nome invariante del provider e, facoltativamente, un nome di server per il quale verrà utilizzata la strategia di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="53ca0-170">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="53ca0-171">Per altre informazioni sui relativi al provider di servizi in EF6, vedere la [modello di provider di Entity Framework 6](~/ef6/fundamentals/providers/provider-model.md) sezione.</span><span class="sxs-lookup"><span data-stu-id="53ca0-171">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="53ca0-172">Func < DbConnection, stringa, System.Data.Entity.Migrations.History.HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="53ca0-172">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="53ca0-173">**Versione introdotta**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="53ca0-173">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="53ca0-174">**Oggetto restituito**: una factory che consente a un provider configurare il mapping del HistoryContext al `__MigrationHistory` tabella utilizzata dal migrazioni di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="53ca0-174">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="53ca0-175">Il HistoryContext è un primo oggetto DbContext di codice e può essere configurato usando l'API fluent normale per modificare elementi come il nome della tabella e le specifiche di mapping di colonna.</span><span class="sxs-lookup"><span data-stu-id="53ca0-175">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="53ca0-176">**Chiave**: non usato, sarà null</span><span class="sxs-lookup"><span data-stu-id="53ca0-176">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="53ca0-177">Per altre informazioni sui relativi al provider di servizi in EF6, vedere la [modello di provider di Entity Framework 6](~/ef6/fundamentals/providers/provider-model.md) sezione.</span><span class="sxs-lookup"><span data-stu-id="53ca0-177">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="53ca0-178">DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="53ca0-178">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="53ca0-179">**Versione introdotta**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="53ca0-179">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="53ca0-180">**Oggetto restituito**: ADO.NET il provider da utilizzare per un nome invariante del provider specificato.</span><span class="sxs-lookup"><span data-stu-id="53ca0-180">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="53ca0-181">**Chiave**: una stringa contenente il nome invariante del provider ADO.NET</span><span class="sxs-lookup"><span data-stu-id="53ca0-181">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="53ca0-182">Questo servizio non viene in genere modificato direttamente poiché l'implementazione predefinita Usa la registrazione del provider ADO.NET normale.</span><span class="sxs-lookup"><span data-stu-id="53ca0-182">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="53ca0-183">Per altre informazioni sui relativi al provider di servizi in EF6, vedere la [modello di provider di Entity Framework 6](~/ef6/fundamentals/providers/provider-model.md) sezione.</span><span class="sxs-lookup"><span data-stu-id="53ca0-183">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="53ca0-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="53ca0-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="53ca0-185">**Versione introdotta**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="53ca0-185">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="53ca0-186">**Oggetto restituito**: un servizio che consente di determinare un nome invariante del provider per un determinato tipo di oggetto DbProviderFactory.</span><span class="sxs-lookup"><span data-stu-id="53ca0-186">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="53ca0-187">L'implementazione predefinita di questo servizio Usa la registrazione del provider ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="53ca0-187">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="53ca0-188">Ciò significa che se il provider ADO.NET non è registrato in modo normale perché è in fase di risoluzione DbProviderFactory da Entity Framework, quindi verrà anche essere necessario risolvere questo servizio.</span><span class="sxs-lookup"><span data-stu-id="53ca0-188">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="53ca0-189">**Chiave**: istanza di DbProviderFactory per il quale è richiesto un nome invariante.</span><span class="sxs-lookup"><span data-stu-id="53ca0-189">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="53ca0-190">Per altre informazioni sui relativi al provider di servizi in EF6, vedere la [modello di provider di Entity Framework 6](~/ef6/fundamentals/providers/provider-model.md) sezione.</span><span class="sxs-lookup"><span data-stu-id="53ca0-190">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="53ca0-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="53ca0-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="53ca0-192">**Versione introdotta**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="53ca0-192">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="53ca0-193">**Oggetto restituito**: una cache di assembly che contengono le visualizzazioni pregenerate.</span><span class="sxs-lookup"><span data-stu-id="53ca0-193">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="53ca0-194">Sostituzione viene generalmente utilizzata per informare EF gli assembly che contengono le visualizzazioni pregenerate non esegue qualsiasi individuazione.</span><span class="sxs-lookup"><span data-stu-id="53ca0-194">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="53ca0-195">**Chiave**: non usato, sarà null</span><span class="sxs-lookup"><span data-stu-id="53ca0-195">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="53ca0-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="53ca0-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="53ca0-197">**Versione introdotta**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="53ca0-197">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="53ca0-198">**Oggetto restituito**: un servizio usato da Entity Framework per rendere plurali e singolari i nomi.</span><span class="sxs-lookup"><span data-stu-id="53ca0-198">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="53ca0-199">Per impostazione predefinita viene usato un servizio di pluralizzazione in lingua inglese.</span><span class="sxs-lookup"><span data-stu-id="53ca0-199">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="53ca0-200">**Chiave**: non usato, sarà null</span><span class="sxs-lookup"><span data-stu-id="53ca0-200">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="53ca0-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="53ca0-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="53ca0-202">**Versione introdotta**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="53ca0-202">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="53ca0-203">**Gli oggetti restituiti**: qualsiasi degli intercettori che devono essere registrati all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="53ca0-203">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="53ca0-204">Si noti che questi oggetti vengono richiesti tramite la chiamata GetServices e tutti gli intercettori restituiti da qualsiasi sistema di risoluzione delle dipendenze verranno registrato.</span><span class="sxs-lookup"><span data-stu-id="53ca0-204">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="53ca0-205">**Chiave**: non usato, sarà null.</span><span class="sxs-lookup"><span data-stu-id="53ca0-205">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="53ca0-206">Func < DbContext, azione < stringa\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="53ca0-206">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="53ca0-207">**Versione introdotta**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="53ca0-207">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="53ca0-208">**Oggetto restituito**: una factory che verrà utilizzata per creare il formattatore di log del database che verrà usata quando il contesto. Database. log viene impostata nel contesto specificato.</span><span class="sxs-lookup"><span data-stu-id="53ca0-208">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="53ca0-209">**Chiave**: non usato, sarà null.</span><span class="sxs-lookup"><span data-stu-id="53ca0-209">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="53ca0-210">Func < DbContext\></span><span class="sxs-lookup"><span data-stu-id="53ca0-210">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="53ca0-211">**Versione introdotta**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="53ca0-211">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="53ca0-212">**Oggetto restituito**: una factory che verrà utilizzata per creare istanze del contesto per le migrazioni quando il contesto non dispone di un costruttore senza parametri accessibile.</span><span class="sxs-lookup"><span data-stu-id="53ca0-212">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="53ca0-213">**Chiave**: oggetto del tipo per il tipo di DbContext derivato per cui è necessaria una factory.</span><span class="sxs-lookup"><span data-stu-id="53ca0-213">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="53ca0-214">Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="53ca0-214">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="53ca0-215">**Versione introdotta**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="53ca0-215">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="53ca0-216">**Oggetto restituito**: una factory che verrà utilizzata per creare i serializzatori per la serializzazione delle annotazioni personalizzate fortemente tipizzata in modo che possano essere serializzati e desterilized in XML per l'uso di migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="53ca0-216">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="53ca0-217">**Chiave**: il nome dell'annotazione che viene serializzata o deserializzata.</span><span class="sxs-lookup"><span data-stu-id="53ca0-217">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="53ca0-218">Func < System.Data.Entity.Infrastructure.TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="53ca0-218">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="53ca0-219">**Versione introdotta**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="53ca0-219">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="53ca0-220">**Oggetto restituito**: una factory che verrà utilizzata per creare gestori per le transazioni in modo che possano essere applicata una gestione speciale per le situazioni, ad esempio la gestione degli errori di commit.</span><span class="sxs-lookup"><span data-stu-id="53ca0-220">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="53ca0-221">**Chiave**: ExecutionStrategyKey un oggetto che contiene il nome invariante del provider e, facoltativamente, un nome di server per il quale verrà utilizzato il gestore delle transazioni.</span><span class="sxs-lookup"><span data-stu-id="53ca0-221">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
