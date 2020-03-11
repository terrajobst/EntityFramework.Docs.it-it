---
title: Risoluzione delle dipendenze-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 6082124481f5795bbcb62fff2bb6a58ecdcb48e4
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417862"
---
# <a name="dependency-resolution"></a><span data-ttu-id="85815-102">Risoluzione delle dipendenze</span><span class="sxs-lookup"><span data-stu-id="85815-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="85815-103">**Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="85815-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="85815-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="85815-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="85815-105">A partire da EF6, Entity Framework contiene un meccanismo di utilizzo generico per ottenere le implementazioni dei servizi richiesti.</span><span class="sxs-lookup"><span data-stu-id="85815-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="85815-106">Ovvero, quando Entity Framework usa un'istanza di alcune interfacce o classi di base, richiede un'implementazione concreta dell'interfaccia o della classe di base da usare.</span><span class="sxs-lookup"><span data-stu-id="85815-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="85815-107">Questa operazione viene eseguita usando l'interfaccia IDbDependencyResolver:</span><span class="sxs-lookup"><span data-stu-id="85815-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="85815-108">Il metodo GetService viene in genere chiamato da EF ed è gestito da un'implementazione di IDbDependencyResolver fornita da EF o dall'applicazione.</span><span class="sxs-lookup"><span data-stu-id="85815-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="85815-109">Quando viene chiamato, l'argomento di tipo è l'interfaccia o il tipo di classe di base del servizio richiesto e l'oggetto chiave è null o un oggetto che fornisce informazioni contestuali sul servizio richiesto.</span><span class="sxs-lookup"><span data-stu-id="85815-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="85815-110">Se non diversamente specificato, qualsiasi oggetto restituito deve essere thread-safe poiché può essere usato come singleton.</span><span class="sxs-lookup"><span data-stu-id="85815-110">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="85815-111">In molti casi l'oggetto restituito è una factory, nel qual caso la factory deve essere thread-safe, ma non è necessario che l'oggetto restituito dalla factory sia thread-safe perché per ogni uso viene richiesta una nuova istanza dalla factory.</span><span class="sxs-lookup"><span data-stu-id="85815-111">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>

<span data-ttu-id="85815-112">Questo articolo non contiene informazioni dettagliate su come implementare IDbDependencyResolver, ma funge da riferimento per i tipi di servizio (ovvero i tipi di interfaccia e di classe di base) per i quali EF chiama GetService e la semantica dell'oggetto chiave per ognuno di questi chiamate.</span><span class="sxs-lookup"><span data-stu-id="85815-112">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span>

## <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="85815-113">System. Data. Entity. IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="85815-113">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="85815-114">**Versione introdotta**: Ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="85815-114">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="85815-115">**Oggetto restituito**: inizializzatore di database per il tipo di contesto specificato</span><span class="sxs-lookup"><span data-stu-id="85815-115">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="85815-116">**Chiave**: non usata; sarà null</span><span class="sxs-lookup"><span data-stu-id="85815-116">**Key**: Not used; will be null</span></span>  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="85815-117">Func < System. Data. Entity. Migrations. SQL. MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="85815-117">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="85815-118">**Versione introdotta**: Ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="85815-118">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="85815-119">**Oggetto restituito**: una factory per creare un generatore SQL che può essere usato per le migrazioni e altre azioni che comportano la creazione di un database, ad esempio la creazione di un database con inizializzatori di database.</span><span class="sxs-lookup"><span data-stu-id="85815-119">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="85815-120">**Key**: stringa che contiene il nome invariante del provider ADO.NET che specifica il tipo di database per cui verrà generato SQL.</span><span class="sxs-lookup"><span data-stu-id="85815-120">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="85815-121">Ad esempio, viene restituito il generatore di SQL Server SQL per la chiave "System. Data. SqlClient".</span><span class="sxs-lookup"><span data-stu-id="85815-121">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="85815-122">Per altri dettagli sui servizi correlati al provider in EF6, vedere la sezione [modello di provider EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="85815-122">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="85815-123">System. Data. Entity. Core. Common. DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="85815-123">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="85815-124">**Versione introdotta**: Ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="85815-124">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="85815-125">**Oggetto restituito**: il provider EF da usare per un nome invariante del provider specificato</span><span class="sxs-lookup"><span data-stu-id="85815-125">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="85815-126">**Key**: stringa che contiene il nome invariante del provider ADO.NET che specifica il tipo di database per cui è necessario un provider.</span><span class="sxs-lookup"><span data-stu-id="85815-126">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="85815-127">Il provider SQL Server, ad esempio, viene restituito per la chiave "System. Data. SqlClient".</span><span class="sxs-lookup"><span data-stu-id="85815-127">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="85815-128">Per altri dettagli sui servizi correlati al provider in EF6, vedere la sezione [modello di provider EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="85815-128">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="85815-129">System. Data. Entity. Infrastructure. IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="85815-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="85815-130">**Versione introdotta**: Ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="85815-130">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="85815-131">**Oggetto restituito**: la factory di connessione che verrà usata quando EF crea una connessione di database per convenzione.</span><span class="sxs-lookup"><span data-stu-id="85815-131">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="85815-132">Ovvero, quando nessuna connessione o stringa di connessione viene assegnata a EF e non è possibile trovare una stringa di connessione nel file app. config o Web. config, questo servizio viene utilizzato per creare una connessione per convenzione.</span><span class="sxs-lookup"><span data-stu-id="85815-132">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="85815-133">La modifica della factory di connessione può consentire a EF di usare un tipo diverso di database, ad esempio SQL Server Compact Edition, per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="85815-133">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="85815-134">**Chiave**: non usata; sarà null</span><span class="sxs-lookup"><span data-stu-id="85815-134">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="85815-135">Per altri dettagli sui servizi correlati al provider in EF6, vedere la sezione [modello di provider EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="85815-135">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="85815-136">System. Data. Entity. Infrastructure. IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="85815-136">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="85815-137">**Versione introdotta**: Ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="85815-137">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="85815-138">**Oggetto restituito**: un servizio che può generare un token del manifesto del provider da una connessione.</span><span class="sxs-lookup"><span data-stu-id="85815-138">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="85815-139">Questo servizio viene in genere usato in due modi.</span><span class="sxs-lookup"><span data-stu-id="85815-139">This service is typically used in two ways.</span></span> <span data-ttu-id="85815-140">In primo luogo, può essere utilizzato per evitare Code First la connessione al database durante la compilazione di un modello.</span><span class="sxs-lookup"><span data-stu-id="85815-140">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="85815-141">In secondo luogo, può essere utilizzato per forzare Code First a compilare un modello per una versione specifica del database, ad esempio per forzare un modello per SQL Server 2005 anche se talvolta viene utilizzato SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="85815-141">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="85815-142">**Durata degli oggetti**: Singleton: lo stesso oggetto può essere usato più volte e simultaneamente da thread diversi</span><span class="sxs-lookup"><span data-stu-id="85815-142">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="85815-143">**Chiave**: non usata; sarà null</span><span class="sxs-lookup"><span data-stu-id="85815-143">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="85815-144">System. Data. Entity. Infrastructure. IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="85815-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="85815-145">**Versione introdotta**: Ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="85815-145">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="85815-146">**Oggetto restituito**: un servizio che può ottenere una factory del provider da una connessione specificata.</span><span class="sxs-lookup"><span data-stu-id="85815-146">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="85815-147">In .NET 4,5 il provider è accessibile pubblicamente dalla connessione.</span><span class="sxs-lookup"><span data-stu-id="85815-147">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="85815-148">In .NET 4, l'implementazione predefinita di questo servizio usa alcune euristiche per trovare il provider corrispondente.</span><span class="sxs-lookup"><span data-stu-id="85815-148">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="85815-149">Se questi errori hanno esito negativo, è possibile registrare una nuova implementazione di questo servizio per fornire una soluzione appropriata.</span><span class="sxs-lookup"><span data-stu-id="85815-149">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="85815-150">**Chiave**: non usata; sarà null</span><span class="sxs-lookup"><span data-stu-id="85815-150">**Key**: Not used; will be null</span></span>  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="85815-151">Func < DbContext, System. Data. Entity. Infrastructure. IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="85815-151">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="85815-152">**Versione introdotta**: Ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="85815-152">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="85815-153">**Oggetto restituito**: Factory che genererà una chiave di cache del modello per un determinato contesto.</span><span class="sxs-lookup"><span data-stu-id="85815-153">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="85815-154">Per impostazione predefinita, EF memorizza nella cache un modello per ogni tipo di DbContext per provider.</span><span class="sxs-lookup"><span data-stu-id="85815-154">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="85815-155">Per aggiungere altre informazioni, ad esempio nome schema, alla chiave della cache, è possibile usare un'implementazione diversa di questo servizio.</span><span class="sxs-lookup"><span data-stu-id="85815-155">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="85815-156">**Chiave**: non usata; sarà null</span><span class="sxs-lookup"><span data-stu-id="85815-156">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="85815-157">System. Data. Entity. Spatial. DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="85815-157">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="85815-158">**Versione introdotta**: Ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="85815-158">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="85815-159">**Oggetto restituito**: provider spaziale EF che aggiunge il supporto al provider EF di base per i tipi spaziali geography e Geometry.</span><span class="sxs-lookup"><span data-stu-id="85815-159">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="85815-160">**Chiave**: DbSptialServices viene richiesta in due modi.</span><span class="sxs-lookup"><span data-stu-id="85815-160">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="85815-161">Per prima cosa, i servizi spaziali specifici del provider vengono richiesti usando un oggetto DbProviderInfo, che contiene il nome invariante e il token del manifesto, come chiave.</span><span class="sxs-lookup"><span data-stu-id="85815-161">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="85815-162">In secondo luogo, è possibile chiedere a DbSpatialServices senza chiave.</span><span class="sxs-lookup"><span data-stu-id="85815-162">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="85815-163">Viene usato per risolvere il "provider spaziale globale" usato per la creazione di tipi DbGeography o DbGeometry autonomi.</span><span class="sxs-lookup"><span data-stu-id="85815-163">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="85815-164">Per altri dettagli sui servizi correlati al provider in EF6, vedere la sezione [modello di provider EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="85815-164">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="85815-165">Func < System. Data. Entity. Infrastructure. IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="85815-165">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="85815-166">**Versione introdotta**: Ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="85815-166">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="85815-167">**Oggetto restituito**: una factory per creare un servizio che consente a un provider di implementare nuovi tentativi o un altro comportamento quando le query e i comandi vengono eseguiti sul database.</span><span class="sxs-lookup"><span data-stu-id="85815-167">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="85815-168">Se non viene fornita alcuna implementazione, EF eseguirà semplicemente i comandi e propagherà le eccezioni generate.</span><span class="sxs-lookup"><span data-stu-id="85815-168">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="85815-169">Per SQL Server questo servizio viene usato per fornire un criterio di ripetizione dei tentativi particolarmente utile quando viene eseguito su server di database basati su cloud, ad esempio SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="85815-169">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="85815-170">**Key**: oggetto ExecutionStrategyKey contenente il nome invariante del provider e, facoltativamente, un nome del server per il quale verrà utilizzata la strategia di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="85815-170">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="85815-171">Per altri dettagli sui servizi correlati al provider in EF6, vedere la sezione [modello di provider EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="85815-171">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="85815-172">Func < DbConnection, String, System. Data. Entity. Migrations. History. HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="85815-172">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="85815-173">**Versione introdotta**: Ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="85815-173">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="85815-174">**Oggetto restituito**: Factory che consente a un provider di configurare il mapping di HistoryContext alla tabella `__MigrationHistory` utilizzata dalle migrazioni di EF.</span><span class="sxs-lookup"><span data-stu-id="85815-174">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="85815-175">HistoryContext è un Code First DbContext e può essere configurato usando l'API Fluent normale per modificare elementi quali il nome della tabella e le specifiche di mapping delle colonne.</span><span class="sxs-lookup"><span data-stu-id="85815-175">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="85815-176">**Chiave**: non usata; sarà null</span><span class="sxs-lookup"><span data-stu-id="85815-176">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="85815-177">Per altri dettagli sui servizi correlati al provider in EF6, vedere la sezione [modello di provider EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="85815-177">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="85815-178">System. Data. Common. DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="85815-178">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="85815-179">**Versione introdotta**: Ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="85815-179">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="85815-180">**Oggetto restituito**: provider ADO.NET da usare per un nome invariante del provider specificato.</span><span class="sxs-lookup"><span data-stu-id="85815-180">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="85815-181">**Key**: stringa che contiene il nome invariante del provider ADO.NET</span><span class="sxs-lookup"><span data-stu-id="85815-181">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="85815-182">Questo servizio non viene in genere modificato direttamente perché l'implementazione predefinita Usa la registrazione del provider ADO.NET normale.</span><span class="sxs-lookup"><span data-stu-id="85815-182">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="85815-183">Per altri dettagli sui servizi correlati al provider in EF6, vedere la sezione [modello di provider EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="85815-183">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="85815-184">System. Data. Entity. Infrastructure. IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="85815-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="85815-185">**Versione introdotta**: Ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="85815-185">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="85815-186">**Oggetto restituito**: un servizio utilizzato per determinare un nome invariante del provider per un tipo di DbProviderFactory specificato.</span><span class="sxs-lookup"><span data-stu-id="85815-186">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="85815-187">L'implementazione predefinita di questo servizio usa la registrazione del provider ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="85815-187">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="85815-188">Ciò significa che se il provider ADO.NET non è registrato in modo normale perché DbProviderFactory viene risolto da EF, sarà necessario anche risolvere questo servizio.</span><span class="sxs-lookup"><span data-stu-id="85815-188">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="85815-189">**Key**: istanza di DbProviderFactory per la quale è richiesto un nome invariante.</span><span class="sxs-lookup"><span data-stu-id="85815-189">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="85815-190">Per altri dettagli sui servizi correlati al provider in EF6, vedere la sezione [modello di provider EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="85815-190">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="85815-191">System. Data. Entity. Core. mapping. ViewGeneration. IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="85815-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="85815-192">**Versione introdotta**: Ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="85815-192">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="85815-193">**Oggetto restituito**: una cache degli assembly che contengono visualizzazioni pregenerate.</span><span class="sxs-lookup"><span data-stu-id="85815-193">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="85815-194">Una sostituzione viene in genere utilizzata per indicare a EF quali assembly contengono viste pregenerate senza eseguire alcuna individuazione.</span><span class="sxs-lookup"><span data-stu-id="85815-194">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="85815-195">**Chiave**: non usata; sarà null</span><span class="sxs-lookup"><span data-stu-id="85815-195">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="85815-196">System. Data. Entity. Infrastructure. Pluraling. IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="85815-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="85815-197">**Versione introdotta**: Ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="85815-197">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="85815-198">**Oggetto restituito**: un servizio usato da EF ai nomi plurali e singolari.</span><span class="sxs-lookup"><span data-stu-id="85815-198">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="85815-199">Per impostazione predefinita, viene usato un servizio di pluralismo in inglese.</span><span class="sxs-lookup"><span data-stu-id="85815-199">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="85815-200">**Chiave**: non usata; sarà null</span><span class="sxs-lookup"><span data-stu-id="85815-200">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="85815-201">System. Data. Entity. Infrastructure. Intercettation. IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="85815-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="85815-202">**Versione introdotta**: Ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="85815-202">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="85815-203">**Oggetti restituiti**: qualsiasi intercettore che deve essere registrato all'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="85815-203">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="85815-204">Si noti che questi oggetti vengono richiesti usando la chiamata GetServices e tutti gli intercettori restituiti da qualsiasi resolver di dipendenza verranno registrati.</span><span class="sxs-lookup"><span data-stu-id="85815-204">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="85815-205">**Chiave**: non usata; sarà null.</span><span class="sxs-lookup"><span data-stu-id="85815-205">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="85815-206">Func < System. Data. Entity. DbContext, Action < String\>, System. Data. Entity. Infrastructure. Interceptor. DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="85815-206">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="85815-207">**Versione introdotta**: Ef 6.0.0</span><span class="sxs-lookup"><span data-stu-id="85815-207">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="85815-208">**Oggetto restituito**: Factory che verrà usata per creare il formattatore di log del database che verrà usato quando il contesto. La proprietà database. log è impostata nel contesto specificato.</span><span class="sxs-lookup"><span data-stu-id="85815-208">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="85815-209">**Chiave**: non usata; sarà null.</span><span class="sxs-lookup"><span data-stu-id="85815-209">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="85815-210">Func < System. Data. Entity. DbContext\></span><span class="sxs-lookup"><span data-stu-id="85815-210">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="85815-211">**Versione introdotta**: Ef 6.1.0</span><span class="sxs-lookup"><span data-stu-id="85815-211">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="85815-212">**Oggetto restituito**: Factory che verrà usata per creare istanze di contesto per le migrazioni quando il contesto non dispone di un costruttore senza parametri accessibile.</span><span class="sxs-lookup"><span data-stu-id="85815-212">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="85815-213">**Key**: oggetto Type per il tipo di DbContext derivato per il quale è necessaria una factory.</span><span class="sxs-lookup"><span data-stu-id="85815-213">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="85815-214">Func < System. Data. Entity. Core. Metadata. Edm. IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="85815-214">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="85815-215">**Versione introdotta**: Ef 6.1.0</span><span class="sxs-lookup"><span data-stu-id="85815-215">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="85815-216">**Oggetto restituito**: Factory che verrà utilizzata per creare serializzatori per la serializzazione di annotazioni personalizzate fortemente tipizzate in modo che possano essere serializzate e desterilizzate in XML per l'utilizzo in Migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="85815-216">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="85815-217">**Key**: il nome dell'annotazione da serializzare o deserializzare.</span><span class="sxs-lookup"><span data-stu-id="85815-217">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="85815-218">Func < System. Data. Entity. Infrastructure. TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="85815-218">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="85815-219">**Versione introdotta**: Ef 6.1.0</span><span class="sxs-lookup"><span data-stu-id="85815-219">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="85815-220">**Oggetto restituito**: Factory che verrà usata per creare gestori per le transazioni, in modo che sia possibile applicare una gestione speciale per situazioni come la gestione degli errori di commit.</span><span class="sxs-lookup"><span data-stu-id="85815-220">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="85815-221">**Key**: oggetto ExecutionStrategyKey che contiene il nome invariante del provider e, facoltativamente, un nome di server per il quale verrà utilizzato il gestore delle transazioni.</span><span class="sxs-lookup"><span data-stu-id="85815-221">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
