---
title: Le stringhe di connessione e modelli - Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
caps.latest.revision: 3
ms.openlocfilehash: ca597e68a5b3e2085612669ee81da10ba6969eeb
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122760"
---
# <a name="connection-strings-and-models"></a><span data-ttu-id="c551b-102">Modelli e le stringhe di connessione</span><span class="sxs-lookup"><span data-stu-id="c551b-102">Connection strings and models</span></span>
<span data-ttu-id="c551b-103">In questo argomento illustra come Entity Framework consente di individuare la connessione di database da usare e il modo in cui è possibile modificarlo.</span><span class="sxs-lookup"><span data-stu-id="c551b-103">This topic covers how Entity Framework discovers which database connection to use, and how you can change it.</span></span> <span data-ttu-id="c551b-104">I modelli creati con Code First e la finestra di progettazione di Entity Framework sono descritte in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="c551b-104">Models created with Code First and the EF Designer are both covered in this topic.</span></span>  

<span data-ttu-id="c551b-105">In genere un'applicazione Entity Framework Usa una classe derivata da DbContext.</span><span class="sxs-lookup"><span data-stu-id="c551b-105">Typically an Entity Framework application uses a class derived from DbContext.</span></span> <span data-ttu-id="c551b-106">Questa classe derivata verrà chiamare uno dei costruttori nella classe base DbContext al controllo:</span><span class="sxs-lookup"><span data-stu-id="c551b-106">This derived class will call one of the constructors on the base DbContext class to control:</span></span>  

- <span data-ttu-id="c551b-107">Modalità di connessione il contesto a un database, ovvero come una stringa di connessione è trovare/usata</span><span class="sxs-lookup"><span data-stu-id="c551b-107">How the context will connect to a database — that is, how a connection string is found/used</span></span>  
- <span data-ttu-id="c551b-108">Se verrà utilizzato il contesto di calcolare un modello utilizzando Code First o caricare un modello creato con la finestra di progettazione di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="c551b-108">Whether the context will use calculate a model using Code First or load a model created with the EF Designer</span></span>  
- <span data-ttu-id="c551b-109">Opzioni avanzate aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c551b-109">Additional advanced options</span></span>  

<span data-ttu-id="c551b-110">I frammenti seguenti illustrano che alcuni modi i costruttori DbContext è utilizzabile.</span><span class="sxs-lookup"><span data-stu-id="c551b-110">The following fragments show some of the ways the DbContext constructors can be used.</span></span>  

## <a name="use-code-first-with-connection-by-convention"></a><span data-ttu-id="c551b-111">Utilizzare Code First con connessione per convenzione</span><span class="sxs-lookup"><span data-stu-id="c551b-111">Use Code First with connection by convention</span></span>  

<span data-ttu-id="c551b-112">Se non è ancora eventuali altre opzioni di configurazione nell'applicazione, quindi chiamare il costruttore senza parametri in DbContext causerà DbContext per l'esecuzione in modalità Code First con una connessione al database creata per convenzione.</span><span class="sxs-lookup"><span data-stu-id="c551b-112">If you have not done any other configuration in your application, then calling the parameterless constructor on DbContext will cause DbContext to run in Code First mode with a database connection created by convention.</span></span> <span data-ttu-id="c551b-113">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c551b-113">For example:</span></span>  

``` csharp  
namespace Demo.EF
{
    public class BloggingContext : DbContext
    {
        public BloggingContext()
        // C# will call base class parameterless constructor by default
        {
        }
    }
}
```  

<span data-ttu-id="c551b-114">In questo esempio DbContext Usa il nome completo dello spazio dei nomi di class—Demo.EF.BloggingContext—as del contesto derivato il nome del database e crea una stringa di connessione per il database usando SQL Express o LocalDB.</span><span class="sxs-lookup"><span data-stu-id="c551b-114">In this example DbContext uses the namespace qualified name of your derived context class—Demo.EF.BloggingContext—as the database name and creates a connection string for this database using either SQL Express or LocalDB.</span></span> <span data-ttu-id="c551b-115">Se entrambi sono installati, SQL Express verrà utilizzato.</span><span class="sxs-lookup"><span data-stu-id="c551b-115">If both are installed, SQL Express will be used.</span></span>  

<span data-ttu-id="c551b-116">Visual Studio 2010 include SQL Express per impostazione predefinita e Visual Studio 2012 e versioni successive include LocalDB.</span><span class="sxs-lookup"><span data-stu-id="c551b-116">Visual Studio 2010 includes SQL Express by default and Visual Studio 2012 and later includes LocalDB.</span></span> <span data-ttu-id="c551b-117">Durante l'installazione, il pacchetto EntityFramework NuGet controlla quali server di database è disponibile.</span><span class="sxs-lookup"><span data-stu-id="c551b-117">During installation, the EntityFramework NuGet package checks which database server is available.</span></span> <span data-ttu-id="c551b-118">Il pacchetto NuGet aggiornerà quindi il file di configurazione, impostare il server di database predefinito che usa Code First quando si crea una connessione per convenzione.</span><span class="sxs-lookup"><span data-stu-id="c551b-118">The NuGet package will then update the configuration file by setting the default database server that Code First uses when creating a connection by convention.</span></span> <span data-ttu-id="c551b-119">Se SQL Express è in esecuzione, verrà utilizzato.</span><span class="sxs-lookup"><span data-stu-id="c551b-119">If SQL Express is running, it will be used.</span></span> <span data-ttu-id="c551b-120">Se SQL Express non è disponibile quindi LocalDB verrà registrato come impostazione predefinita invece.</span><span class="sxs-lookup"><span data-stu-id="c551b-120">If SQL Express is not available then LocalDB will be registered as the default instead.</span></span> <span data-ttu-id="c551b-121">Non vengono apportate modifiche al file di configurazione se contiene già un'impostazione per la factory di connessione predefinita.</span><span class="sxs-lookup"><span data-stu-id="c551b-121">No changes are made to the configuration file if it already contains a setting for the default connection factory.</span></span>  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a><span data-ttu-id="c551b-122">Uso di Code First con una connessione con convenzione e nome del database specificato</span><span class="sxs-lookup"><span data-stu-id="c551b-122">Use Code First with connection by convention and specified database name</span></span>  

<span data-ttu-id="c551b-123">Se non è ancora eventuali altre opzioni di configurazione nell'applicazione, quindi chiamare il costruttore string in DbContext con il nome del database da usare causerà DbContext per l'esecuzione in modalità Code First con una connessione al database creata per convenzione per il database di tale nome.</span><span class="sxs-lookup"><span data-stu-id="c551b-123">If you have not done any other configuration in your application, then calling the string constructor on DbContext with the database name you want to use will cause DbContext to run in Code First mode with a database connection created by convention to the database of that name.</span></span> <span data-ttu-id="c551b-124">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c551b-124">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

<span data-ttu-id="c551b-125">In questo esempio DbContext Usa "BloggingDatabase" come nome del database e crea una stringa di connessione per il database usando SQL Express (installato con Visual Studio 2010) o database locale (installato con Visual Studio 2012).</span><span class="sxs-lookup"><span data-stu-id="c551b-125">In this example DbContext uses “BloggingDatabase” as the database name and creates a connection string for this database using either SQL Express (installed with Visual Studio 2010) or LocalDB (installed with Visual Studio 2012).</span></span> <span data-ttu-id="c551b-126">Se entrambi sono installati, SQL Express verrà utilizzato.</span><span class="sxs-lookup"><span data-stu-id="c551b-126">If both are installed, SQL Express will be used.</span></span>  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="c551b-127">Utilizzare Code First con stringa di connessione nel file app.config/web.config</span><span class="sxs-lookup"><span data-stu-id="c551b-127">Use Code First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="c551b-128">È possibile scegliere di inserire una stringa di connessione nel file app. config o Web. config.</span><span class="sxs-lookup"><span data-stu-id="c551b-128">You may choose to put a connection string in your app.config or web.config file.</span></span> <span data-ttu-id="c551b-129">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c551b-129">For example:</span></span>  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

<span data-ttu-id="c551b-130">Si tratta di un metodo semplice per usare un server di database diverso da SQL Express o LocalDB DbContext, ovvero l'esempio precedente viene specificato un database di SQL Server Compact Edition.</span><span class="sxs-lookup"><span data-stu-id="c551b-130">This is an easy way to tell DbContext to use a database server other than SQL Express or LocalDB — the example above specifies a SQL Server Compact Edition database.</span></span>  

<span data-ttu-id="c551b-131">Se il nome della stringa di connessione corrisponde al nome del contesto (con o senza qualifica dello spazio dei nomi) quindi si troverà da DbContext quando viene usato il costruttore senza parametri.</span><span class="sxs-lookup"><span data-stu-id="c551b-131">If the name of the connection string matches the name of your context (either with or without namespace qualification) then it will be found by DbContext when the parameterless constructor is used.</span></span> <span data-ttu-id="c551b-132">Se il nome di stringa di connessione è diverso dal nome del contesto è possibile indicare a DbContext per usare la connessione in modalità Code First passando il nome di stringa di connessione per il costruttore DbContext.</span><span class="sxs-lookup"><span data-stu-id="c551b-132">If the connection string name is different from the name of your context then you can tell DbContext to use this connection in Code First mode by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="c551b-133">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c551b-133">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="c551b-134">In alternativa, è possibile usare il formato "nome =\<nome stringa di connessione\>" per la stringa passata al costruttore DbContext.</span><span class="sxs-lookup"><span data-stu-id="c551b-134">Alternatively, you can use the form “name=\<connection string name\>” for the string passed to the DbContext constructor.</span></span> <span data-ttu-id="c551b-135">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c551b-135">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="c551b-136">Questo formato rende esplicita che si prevede che la stringa di connessione possano essere trovati in file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c551b-136">This form makes it explicit that you expect the connection string to be found in your config file.</span></span> <span data-ttu-id="c551b-137">Se una stringa di connessione con il nome specificato non viene trovata, verrà generata un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="c551b-137">An exception will be thrown if a connection string with the given name is not found.</span></span>  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="c551b-138">Database/Model First con stringa di connessione nel file app.config/web.config</span><span class="sxs-lookup"><span data-stu-id="c551b-138">Database/Model First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="c551b-139">I modelli creati con la finestra di progettazione di Entity Framework sono diversi da Code First in quanto il modello esiste già e non viene generato dal codice quando viene eseguita l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c551b-139">Models created with the EF Designer are different from Code First in that your model already exists and is not generated from code when the application runs.</span></span> <span data-ttu-id="c551b-140">Il modello è in genere presente come un file EDMX nel progetto.</span><span class="sxs-lookup"><span data-stu-id="c551b-140">The model typically exists as an EDMX file in your project.</span></span>  

<span data-ttu-id="c551b-141">La finestra di progettazione aggiungerà una stringa di connessione Entity Framework nel file app. config o Web. config.</span><span class="sxs-lookup"><span data-stu-id="c551b-141">The designer will add an EF connection string to your app.config or web.config file.</span></span> <span data-ttu-id="c551b-142">Questa stringa di connessione è speciale perché contiene informazioni su come trovare le informazioni nel file EDMX.</span><span class="sxs-lookup"><span data-stu-id="c551b-142">This connection string is special in that it contains information about how to find the information in your EDMX file.</span></span> <span data-ttu-id="c551b-143">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c551b-143">For example:</span></span>  

``` xml  
<configuration>  
  <connectionStrings>  
    <add name="Northwind_Entities"  
         connectionString="metadata=res://*/Northwind.csdl|  
                                    res://*/Northwind.ssdl|  
                                    res://*/Northwind.msl;  
                           provider=System.Data.SqlClient;  
                           provider connection string=  
                               &quot;Data Source=.\sqlexpress;  
                                     Initial Catalog=Northwind;  
                                     Integrated Security=True;  
                                     MultipleActiveResultSets=True&quot;"  
         providerName="System.Data.EntityClient"/>  
  </connectionStrings>  
</configuration>
```  

<span data-ttu-id="c551b-144">La finestra di progettazione di Entity Framework genera anche codice che chiede di DbContext per usare la connessione passando il nome di stringa di connessione per il costruttore DbContext.</span><span class="sxs-lookup"><span data-stu-id="c551b-144">The EF Designer will also generate code that tells DbContext to use this connection by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="c551b-145">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="c551b-145">For example:</span></span>  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

<span data-ttu-id="c551b-146">DbContext è in grado di caricare il modello esistente (anziché di utilizzare Code First per calcolarla dal codice) perché la stringa di connessione è una stringa di connessione EF contenente i dettagli del modello da usare.</span><span class="sxs-lookup"><span data-stu-id="c551b-146">DbContext knows to load the existing model (rather than using Code First to calculate it from code) because the connection string is an EF connection string containing details of the model to use.</span></span>  

## <a name="other-dbcontext-constructor-options"></a><span data-ttu-id="c551b-147">Altre opzioni di costruttore DbContext</span><span class="sxs-lookup"><span data-stu-id="c551b-147">Other DbContext constructor options</span></span>  

<span data-ttu-id="c551b-148">La classe DbContext contiene altri costruttori e i modelli di utilizzo che consentono alcuni scenari più avanzati.</span><span class="sxs-lookup"><span data-stu-id="c551b-148">The DbContext class contains other constructors and usage patterns that enable some more advanced scenarios.</span></span> <span data-ttu-id="c551b-149">Alcune di queste sono:</span><span class="sxs-lookup"><span data-stu-id="c551b-149">Some of these are:</span></span>  

- <span data-ttu-id="c551b-150">È possibile usare la classe DbModelBuilder per compilare un modello Code First senza creare un'istanza di un'istanza di DbContext.</span><span class="sxs-lookup"><span data-stu-id="c551b-150">You can use the DbModelBuilder class to build a Code First model without instantiating a DbContext instance.</span></span> <span data-ttu-id="c551b-151">Il risultato di questo è un oggetto DbModel.</span><span class="sxs-lookup"><span data-stu-id="c551b-151">The result of this is a DbModel object.</span></span> <span data-ttu-id="c551b-152">È quindi possibile passare questo oggetto DbModel a uno dei costruttori DbContext quando si è pronti per creare l'istanza di DbContext.</span><span class="sxs-lookup"><span data-stu-id="c551b-152">You can then pass this DbModel object to one of the DbContext constructors when you are ready to create your DbContext instance.</span></span>  
- <span data-ttu-id="c551b-153">È possibile passare una stringa di connessione completa per DbContext anziché solo il nome di stringa del database o una connessione.</span><span class="sxs-lookup"><span data-stu-id="c551b-153">You can pass a full connection string to DbContext instead of just the database or connection string name.</span></span> <span data-ttu-id="c551b-154">Per impostazione predefinita questa stringa di connessione viene usata con il provider SqlClient; Ciò può essere modificato impostando un'implementazione diversa della IConnectionFactory nel contesto. Database.DefaultConnectionFactory.</span><span class="sxs-lookup"><span data-stu-id="c551b-154">By default this connection string is used with the System.Data.SqlClient provider; this can be changed by setting a different implementation of IConnectionFactory onto context.Database.DefaultConnectionFactory.</span></span>  
- <span data-ttu-id="c551b-155">È possibile usare un oggetto DbConnection esistente passandolo al costruttore oggetto DbContext.</span><span class="sxs-lookup"><span data-stu-id="c551b-155">You can use an existing DbConnection object by passing it to a DbContext constructor.</span></span> <span data-ttu-id="c551b-156">Se l'oggetto di connessione è un'istanza di EntityConnection, quindi il modello specificato nella connessione sarà utilizzata, anziché calcolando un modello utilizzando Code First.</span><span class="sxs-lookup"><span data-stu-id="c551b-156">If the connection object is an instance of EntityConnection, then the model specified in the connection will be used rather than calculating a model using Code First.</span></span> <span data-ttu-id="c551b-157">Se l'oggetto è un'istanza di un altro tipo, ad esempio, SqlConnection, quindi il contesto viene utilizzato per la modalità Code First.</span><span class="sxs-lookup"><span data-stu-id="c551b-157">If the object is an instance of some other type—for example, SqlConnection—then the context will use it for Code First mode.</span></span>  
- <span data-ttu-id="c551b-158">È possibile passare un oggetto ObjectContext esistente a un costruttore DbContext per creare un oggetto DbContext ritorno a capo il contesto esistente.</span><span class="sxs-lookup"><span data-stu-id="c551b-158">You can pass an existing ObjectContext to a DbContext constructor to create a DbContext wrapping the existing context.</span></span> <span data-ttu-id="c551b-159">Può essere utilizzato per le applicazioni esistenti che utilizza ObjectContext ma che vuole sfruttare i vantaggi di DbContext in alcune parti dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c551b-159">This can be used for existing applications that use ObjectContext but which want to take advantage of DbContext in some parts of the application.</span></span>  
