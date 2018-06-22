---
title: Stringhe di connessione - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: b4ed01f0452d74ac49d3fde780caa5f1b25a6e97
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052531"
---
# <a name="connection-strings"></a><span data-ttu-id="d0cf9-102">Stringhe di connessione</span><span class="sxs-lookup"><span data-stu-id="d0cf9-102">Connection Strings</span></span>

<span data-ttu-id="d0cf9-103">La maggior parte dei provider di database richiede una qualche forma di stringa di connessione per connettersi al database.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="d0cf9-104">In alcuni casi la stringa di connessione contiene informazioni riservate che devono essere protetti.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="d0cf9-105">È necessario anche modificare la stringa di connessione quando si sposta l'applicazione tra ambienti, ad esempio sviluppo, test e produzione.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="net-framework-applications"></a><span data-ttu-id="d0cf9-106">Applicazioni .NET framework</span><span class="sxs-lookup"><span data-stu-id="d0cf9-106">.NET Framework Applications</span></span>

<span data-ttu-id="d0cf9-107">Applicazioni .NET framework, ad esempio Windows Form, WPF, Console e ASP.NET 4, dispongono di un modello di stringa di connessione già testate.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-107">.NET Framework applications, such as WinForms, WPF, Console, and ASP.NET 4, have a tried and tested connection string pattern.</span></span> <span data-ttu-id="d0cf9-108">La stringa di connessione deve essere aggiunti al file app. config delle applicazioni (Web. config se si utilizza ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="d0cf9-108">The connection string should be added to your applications App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="d0cf9-109">Se la stringa di connessione contiene informazioni riservate, ad esempio nome utente e password, è possibile proteggere il contenuto del file di configurazione mediante [configurazione protetta](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span><span class="sxs-lookup"><span data-stu-id="d0cf9-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using [Protected Configuration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span></span>

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>

  <connectionStrings>
    <add name="BloggingDatabase"
         connectionString="Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" />
  </connectionStrings>
</configuration>
```

> [!TIP]  
> <span data-ttu-id="d0cf9-110">Il `providerName` impostazione non è obbligatorio nei componenti di base di EF le stringhe di connessione in App. config perché il provider di database viene configurato tramite codice.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="d0cf9-111">Sarà quindi possibile leggere la stringa di connessione utilizzando il `ConfigurationManager` API del contesto `OnConfiguring` metodo.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="d0cf9-112">Potrebbe essere necessario aggiungere un riferimento di `System.Configuration` assembly framework per poter usare questa API.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="d0cf9-113">Piattaforma UWP (Universal Windows Platform)</span><span class="sxs-lookup"><span data-stu-id="d0cf9-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="d0cf9-114">Le stringhe di connessione in un'applicazione UWP sono in genere una connessione specifica solo un nome di file locale di SQLite.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="d0cf9-115">Sono in genere non contengono informazioni riservate e non è necessario essere modificati in quanto un'applicazione viene distribuita.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="d0cf9-116">Di conseguenza, queste stringhe di connessione sono in genere supportate da rendere disponibile nel codice, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="d0cf9-117">Se si desidera spostarli fuori dal codice UWP supporta il concetto di impostazioni, vedere il [sezione Impostazioni App della documentazione UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) per informazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
            optionsBuilder.UseSqlite("Data Source=blogging.db");
    }
}
```

## <a name="aspnet-core"></a><span data-ttu-id="d0cf9-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d0cf9-118">ASP.NET Core</span></span>

<span data-ttu-id="d0cf9-119">In ASP.NET Core, il sistema di configurazione è molto flessibile e può essere archiviata la stringa di connessione `appsettings.json`, una variabile di ambiente, l'archivio segreto utente o un'altra origine di configurazione.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="d0cf9-120">Vedere il [sezione di configurazione della documentazione di ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="d0cf9-121">L'esempio seguente mostra la stringa di connessione archiviata nella `appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="d0cf9-122">Il contesto viene in genere configurato `Startup.cs` con la stringa di connessione da leggere dalla configurazione.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="d0cf9-123">Si noti il `GetConnectionString()` metodo cerca un valore di configurazione, la cui chiave è `ConnectionStrings:<connection string name>`.</span><span class="sxs-lookup"><span data-stu-id="d0cf9-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
