---
title: Configurazione basata su codice - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
caps.latest.revision: 3
ms.openlocfilehash: d6a33434e582fcd7ce756b447d7f2cbab4ca43ec
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122741"
---
# <a name="code-based-configuration"></a><span data-ttu-id="f553a-102">Configurazione basata su codice</span><span class="sxs-lookup"><span data-stu-id="f553a-102">Code-based configuration</span></span>
> [!NOTE]
> <span data-ttu-id="f553a-103">**Solo EF6 e versioni successive**: funzionalità, API e altri argomenti discussi in questa pagina sono stati introdotti in Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="f553a-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="f553a-104">Se si usa una versione precedente, le informazioni qui riportate, o parte di esse, non sono applicabili.</span><span class="sxs-lookup"><span data-stu-id="f553a-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="f553a-105">Configurazione per un'applicazione Entity Framework può essere specificata in un file di configurazione (app.config/web.config) o tramite codice.</span><span class="sxs-lookup"><span data-stu-id="f553a-105">Configuration for an Entity Framework application can be specified in a config file (app.config/web.config) or through code.</span></span> <span data-ttu-id="f553a-106">Quest'ultimo è nota come configurazione basata su codice.</span><span class="sxs-lookup"><span data-stu-id="f553a-106">The latter is known as code-based configuration.</span></span>  

<span data-ttu-id="f553a-107">Configurazione in un file di configurazione è descritta una [articolo separato](config-file.md).</span><span class="sxs-lookup"><span data-stu-id="f553a-107">Configuration in a config file is described in a [separate article](config-file.md).</span></span> <span data-ttu-id="f553a-108">Il file di configurazione ha la precedenza sulla configurazione basata su codice.</span><span class="sxs-lookup"><span data-stu-id="f553a-108">The config file takes precedence over code-based configuration.</span></span> <span data-ttu-id="f553a-109">In altre parole, se è impostata un'opzione di configurazione in entrambe le code e nel file di configurazione, viene utilizzata l'impostazione nel file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="f553a-109">In other words, if a configuration option is set in both code and in the config file, then the setting in the config file is used.</span></span>  

## <a name="using-dbconfiguration"></a><span data-ttu-id="f553a-110">Usando DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="f553a-110">Using DbConfiguration</span></span>  

<span data-ttu-id="f553a-111">Configurazione basata su codice in EF6 e versioni successive è possibile creare una sottoclasse di System.Data.Entity.Config.DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="f553a-111">Code-based configuration in EF6 and above is achieved by creating a subclass of System.Data.Entity.Config.DbConfiguration.</span></span> <span data-ttu-id="f553a-112">Le seguenti linee guida da seguire quando si crea una sottoclasse DbConfiguration:</span><span class="sxs-lookup"><span data-stu-id="f553a-112">The following guidelines should be followed when subclassing DbConfiguration:</span></span>  

- <span data-ttu-id="f553a-113">Creare una sola classe DbConfiguration per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f553a-113">Create only one DbConfiguration class for your application.</span></span> <span data-ttu-id="f553a-114">Questa classe specifica le impostazioni a livello di dominio applicazione.</span><span class="sxs-lookup"><span data-stu-id="f553a-114">This class specifies app-domain wide settings.</span></span>  
- <span data-ttu-id="f553a-115">Inserire la classe DbConfiguration nello stesso assembly come la classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="f553a-115">Place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="f553a-116">(Vedere la *lo spostamento DbConfiguration* sezione se si desidera modificare questa impostazione.)</span><span class="sxs-lookup"><span data-stu-id="f553a-116">(See the *Moving DbConfiguration* section if you want to change this.)</span></span>  
- <span data-ttu-id="f553a-117">Assegnato alla classe DbConfiguration un costruttore pubblico senza parametri.</span><span class="sxs-lookup"><span data-stu-id="f553a-117">Give your DbConfiguration class a public parameterless constructor.</span></span>  
- <span data-ttu-id="f553a-118">Impostare le opzioni di configurazione tramite la chiamata di metodi DbConfiguration protetti all'interno di questo costruttore.</span><span class="sxs-lookup"><span data-stu-id="f553a-118">Set configuration options by calling protected DbConfiguration methods from within this constructor.</span></span>  

<span data-ttu-id="f553a-119">Attenendosi alle linee guida consente a EF di individuare e usare la configurazione automaticamente da entrambi gli strumenti che necessita di accedere al modello e quando viene eseguita l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="f553a-119">Following these guidelines allows EF to discover and use your configuration automatically by both tooling that needs to access your model and when your application is run.</span></span>  

## <a name="example"></a><span data-ttu-id="f553a-120">Esempio</span><span class="sxs-lookup"><span data-stu-id="f553a-120">Example</span></span>  

<span data-ttu-id="f553a-121">Una classe derivata da DbConfiguration potrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="f553a-121">A class derived from DbConfiguration might look like this:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

namespace MyNamespace
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            SetDefaultConnectionFactory(new LocalDBConnectionFactory("mssqllocaldb"));
        }
    }
}
```  

<span data-ttu-id="f553a-122">Questa classe imposta per usare la strategia di esecuzione di SQL Azure - per ripetere automaticamente le operazioni di database non riuscita - e Usa Local DB per i database creati per convenzione da Code First di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f553a-122">This class sets up EF to use the SQL Azure execution strategy - to automatically retry failed database operations - and to use Local DB for databases that are created by convention from Code First.</span></span>  

## <a name="moving-dbconfiguration"></a><span data-ttu-id="f553a-123">Lo spostamento DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="f553a-123">Moving DbConfiguration</span></span>  

<span data-ttu-id="f553a-124">Esistono casi in cui non è possibile inserire la classe DbConfiguration nello stesso assembly come la classe DbContext.</span><span class="sxs-lookup"><span data-stu-id="f553a-124">There are cases where it is not possible to place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="f553a-125">Ad esempio, potrebbe essere due DbContext ciascuna delle classi in assembly diversi.</span><span class="sxs-lookup"><span data-stu-id="f553a-125">For example, you may have two DbContext classes each in different assemblies.</span></span> <span data-ttu-id="f553a-126">Sono disponibili due opzioni per gestirla.</span><span class="sxs-lookup"><span data-stu-id="f553a-126">There are two options for handling this.</span></span>  

<span data-ttu-id="f553a-127">La prima opzione è usare il file di configurazione per specificare l'istanza DbConfiguration da utilizzare.</span><span class="sxs-lookup"><span data-stu-id="f553a-127">The first option is to use the config file to specify the DbConfiguration instance to use.</span></span> <span data-ttu-id="f553a-128">A tale scopo, impostare l'attributo codeConfigurationType della sezione entityFramework.</span><span class="sxs-lookup"><span data-stu-id="f553a-128">To do this, set the codeConfigurationType attribute of the entityFramework section.</span></span> <span data-ttu-id="f553a-129">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f553a-129">For example:</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

<span data-ttu-id="f553a-130">Il valore di codeConfigurationType deve essere l'assembly e il nome completo dello spazio dei nomi della classe DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="f553a-130">The value of codeConfigurationType must be the assembly and namespace qualified name of your DbConfiguration class.</span></span>  

<span data-ttu-id="f553a-131">La seconda opzione consiste nell'inserire DbConfigurationTypeAttribute sulla classe del contesto.</span><span class="sxs-lookup"><span data-stu-id="f553a-131">The second option is to place DbConfigurationTypeAttribute on your context class.</span></span> <span data-ttu-id="f553a-132">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f553a-132">For example:</span></span>  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

<span data-ttu-id="f553a-133">Il valore passato all'attributo può essere di tipo DbConfiguration: come illustrato in precedenza - o stringa del nome tipo completo di assembly e dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="f553a-133">The value passed to the attribute can either be your DbConfiguration type - as shown above - or the assembly and namespace qualified type name string.</span></span> <span data-ttu-id="f553a-134">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f553a-134">For example:</span></span>  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a><span data-ttu-id="f553a-135">Impostare in modo esplicito DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="f553a-135">Setting DbConfiguration explicitly</span></span>  

<span data-ttu-id="f553a-136">Esistono alcune situazioni in cui configurazione potrebbe essere necessari prima di qualsiasi tipo DbContext è stato utilizzato.</span><span class="sxs-lookup"><span data-stu-id="f553a-136">There are some situations where configuration may be needed before any DbContext type has been used.</span></span> <span data-ttu-id="f553a-137">Alcuni esempi sono:</span><span class="sxs-lookup"><span data-stu-id="f553a-137">Examples of this include:</span></span>  

- <span data-ttu-id="f553a-138">Uso di DbModelBuilder per compilare un modello senza un contesto</span><span class="sxs-lookup"><span data-stu-id="f553a-138">Using DbModelBuilder to build a model without a context</span></span>  
- <span data-ttu-id="f553a-139">Uso di altri framework/codice di alcune utilità che usa un oggetto DbContext in cui tale contesto viene usato prima che venga utilizzato il contesto dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="f553a-139">Using some other framework/utility code that utilizes a DbContext where that context is used before your application context is used</span></span>  

<span data-ttu-id="f553a-140">In tali situazioni Entity Framework è in grado di individuare automaticamente la configurazione ed è invece necessario eseguire una delle operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="f553a-140">In such situations EF is unable to discover the configuration automatically and you must instead do one of the following:</span></span>  

- <span data-ttu-id="f553a-141">Impostare il tipo DbConfiguration nel file di configurazione, come descritto nel *lo spostamento DbConfiguration* sezione precedente</span><span class="sxs-lookup"><span data-stu-id="f553a-141">Set the DbConfiguration type in the config file, as described in the *Moving DbConfiguration* section above</span></span>
- <span data-ttu-id="f553a-142">Chiamare il metodo DbConfiguration.SetConfiguration statico durante l'avvio dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="f553a-142">Call the static DbConfiguration.SetConfiguration method during application startup</span></span>  

## <a name="overriding-dbconfiguration"></a><span data-ttu-id="f553a-143">Si esegue l'override DbConfiguration</span><span class="sxs-lookup"><span data-stu-id="f553a-143">Overriding DbConfiguration</span></span>  

<span data-ttu-id="f553a-144">Esistono alcune situazioni in cui è necessario eseguire l'override di impostazioni di configurazione nella DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="f553a-144">There are some situations where you need to override the configuration set in the DbConfiguration.</span></span> <span data-ttu-id="f553a-145">Ciò non avviene in genere dagli sviluppatori di applicazioni, ma piuttosto da provider di terze parti e plug-in non è possibile usare una classe derivata DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="f553a-145">This is not typically done by application developers but rather by third party providers and plug-ins that cannot use a derived DbConfiguration class.</span></span>  

<span data-ttu-id="f553a-146">A tale scopo, Entity Framework consente a un gestore eventi da registrare che consentono di modificare la configurazione esistente subito prima che venga bloccato.</span><span class="sxs-lookup"><span data-stu-id="f553a-146">For this, EntityFramework allows an event handler to be registered that can modify existing configuration just before it is locked down.</span></span>  <span data-ttu-id="f553a-147">Fornisce inoltre un metodo sugar in modo specifico per la sostituzione di qualsiasi servizio restituito dal localizzatore di servizi di Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f553a-147">It also provides a sugar method specifically for replacing any service returned by the EF service locator.</span></span> <span data-ttu-id="f553a-148">Si tratta di come si dovrà essere utilizzato:</span><span class="sxs-lookup"><span data-stu-id="f553a-148">This is how it is intended to be used:</span></span>  

- <span data-ttu-id="f553a-149">All'avvio dell'app (prima che venga utilizzato Entity Framework) il plug-in o provider deve registrare il metodo del gestore eventi per questo evento.</span><span class="sxs-lookup"><span data-stu-id="f553a-149">At app startup (before EF is used) the plug-in or provider should register the event handler method for this event.</span></span> <span data-ttu-id="f553a-150">(Si noti che ciò deve verificarsi prima che l'applicazione usa Entity Framework).</span><span class="sxs-lookup"><span data-stu-id="f553a-150">(Note that this must happen before the application uses EF.)</span></span>  
- <span data-ttu-id="f553a-151">Il gestore dell'evento effettua una chiamata a ReplaceService per ogni servizio che deve essere sostituito.</span><span class="sxs-lookup"><span data-stu-id="f553a-151">The event handler makes a call to ReplaceService for every service that needs to be replaced.</span></span>  

<span data-ttu-id="f553a-152">Repalce IDbConnectionFactory e DbProviderService, ad esempio, si potrebbe registrare un gestore simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="f553a-152">For example, to repalce IDbConnectionFactory and DbProviderService you would register a handler something like this:</span></span>  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

<span data-ttu-id="f553a-153">Nel codice sopra MyProviderServices e MyConnectionFactory rappresentano le implementazioni del servizio.</span><span class="sxs-lookup"><span data-stu-id="f553a-153">In the code above MyProviderServices and MyConnectionFactory represent your implementations of the service.</span></span>  

<span data-ttu-id="f553a-154">È anche possibile aggiungere i gestori delle dipendenze aggiuntive per ottenere lo stesso effetto.</span><span class="sxs-lookup"><span data-stu-id="f553a-154">You can also add additional dependency handlers to get the same effect.</span></span>  

<span data-ttu-id="f553a-155">Si noti che è anche possibile eseguire il wrapping DbProviderFactory in questo modo, ma tale operazione così influiranno solo Entity Framework e non utilizzi di DbProviderFactory all'esterno di EF.</span><span class="sxs-lookup"><span data-stu-id="f553a-155">Note that you could also wrap DbProviderFactory in this way, but doing so will only affect EF and not uses of the DbProviderFactory outside of EF.</span></span> <span data-ttu-id="f553a-156">Per questo motivo è opportuno continuare a eseguire il wrapping DbProviderFactory come prima.</span><span class="sxs-lookup"><span data-stu-id="f553a-156">For this reason you’ll probably want to continue to wrap DbProviderFactory as you have before.</span></span>  

<span data-ttu-id="f553a-157">È inoltre necessario tenere presenti i servizi che si esegue all'esterno dell'applicazione, ad esempio, durante l'esecuzione di migrazioni dalla Console di gestione pacchetti.</span><span class="sxs-lookup"><span data-stu-id="f553a-157">You should also keep in mind the services that you run externally to your application - for example, when running migrations from the Package Manager Console.</span></span> <span data-ttu-id="f553a-158">Quando si esegue la migrazione dalla console di che tenterà di trovare i DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="f553a-158">When you run migrate from the console it will attempt to find your DbConfiguration.</span></span> <span data-ttu-id="f553a-159">Tuttavia, se otterrà il servizio sottoposto a wrapping dipende da dove il gestore dell'evento è stato registrato.</span><span class="sxs-lookup"><span data-stu-id="f553a-159">However, whether or not it will get the wrapped service depends on where the event handler it registered.</span></span> <span data-ttu-id="f553a-160">Se è registrato come parte della costruzione del DbConfiguration deve eseguire il codice e il servizio è necessario ottenere il wrapping.</span><span class="sxs-lookup"><span data-stu-id="f553a-160">If it is registered as part of the construction of your DbConfiguration then the code should execute and the service should get wrapped.</span></span> <span data-ttu-id="f553a-161">In genere non si tratterà il case e ciò significa che gli strumenti non otterranno il servizio sottoposto a wrapping.</span><span class="sxs-lookup"><span data-stu-id="f553a-161">Usually this won’t be the case and this means that tooling won’t get the wrapped service.</span></span>  
