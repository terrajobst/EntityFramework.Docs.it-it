---
title: Connection Strings - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: 52a8527170845d3e73ebcec518713ade3f3844f0
ms.sourcegitcommit: 06073f8efde97dd5f540dbfb69f574d8380566fe
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/24/2019
ms.locfileid: "67333845"
---
# <a name="connection-strings"></a><span data-ttu-id="a9689-102">Stringhe di connessione</span><span class="sxs-lookup"><span data-stu-id="a9689-102">Connection Strings</span></span>

<span data-ttu-id="a9689-103">La maggior parte dei provider di database richiede una qualche forma di stringa di connessione per connettersi al database.</span><span class="sxs-lookup"><span data-stu-id="a9689-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="a9689-104">In alcuni casi questa stringa di connessione contiene informazioni riservate che devono essere protetti.</span><span class="sxs-lookup"><span data-stu-id="a9689-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="a9689-105">È necessario anche modificare la stringa di connessione come si trasferiscono le applicazioni tra ambienti, ad esempio sviluppo, test e produzione.</span><span class="sxs-lookup"><span data-stu-id="a9689-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="net-framework-applications"></a><span data-ttu-id="a9689-106">Applicazioni .NET framework</span><span class="sxs-lookup"><span data-stu-id="a9689-106">.NET Framework Applications</span></span>

<span data-ttu-id="a9689-107">Le applicazioni .NET framework, ad esempio WinForms, WPF, Console e ASP.NET 4, hanno un modello di stringa di connessione collaudate e testate.</span><span class="sxs-lookup"><span data-stu-id="a9689-107">.NET Framework applications, such as WinForms, WPF, Console, and ASP.NET 4, have a tried and tested connection string pattern.</span></span> <span data-ttu-id="a9689-108">La stringa di connessione deve essere aggiunti al file app. config delle applicazioni (Web. config se si usa ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="a9689-108">The connection string should be added to your applications App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="a9689-109">Se la stringa di connessione contiene informazioni riservate, ad esempio nome utente e password, è possibile proteggere il contenuto del file di configurazione utilizzando [la configurazione protetta](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span><span class="sxs-lookup"><span data-stu-id="a9689-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using [Protected Configuration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span></span>

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
> <span data-ttu-id="a9689-110">Il `providerName` impostazione non è necessaria in stringhe di connessione EF Core archiviate nel file app. config perché il provider di database viene configurato tramite codice.</span><span class="sxs-lookup"><span data-stu-id="a9689-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="a9689-111">È quindi possibile leggere la stringa di connessione usando il `ConfigurationManager` API nel contesto `OnConfiguring` (metodo).</span><span class="sxs-lookup"><span data-stu-id="a9689-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="a9689-112">Potrebbe essere necessario aggiungere un riferimento al `System.Configuration` assembly del framework per poter usare questa API.</span><span class="sxs-lookup"><span data-stu-id="a9689-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="a9689-113">Piattaforma UWP (Universal Windows Platform)</span><span class="sxs-lookup"><span data-stu-id="a9689-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="a9689-114">Stringhe di connessione in un'applicazione UWP sono in genere una connessione di SQLite che specifica solo un nome di file locale.</span><span class="sxs-lookup"><span data-stu-id="a9689-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="a9689-115">Sono in genere non contengono informazioni riservate e non è necessario essere modificata come un'applicazione viene distribuita.</span><span class="sxs-lookup"><span data-stu-id="a9689-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="a9689-116">Di conseguenza, queste stringhe di connessione sono in genere accettabile deve rimanere nel codice, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="a9689-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="a9689-117">Se si desidera spostarli di fuori di codice di piattaforma UWP supporta il concetto di impostazioni, vedere la [sezione di impostazioni dell'App della documentazione di UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) per informazioni dettagliate.</span><span class="sxs-lookup"><span data-stu-id="a9689-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

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

## <a name="aspnet-core"></a><span data-ttu-id="a9689-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a9689-118">ASP.NET Core</span></span>

<span data-ttu-id="a9689-119">In ASP.NET Core è molto flessibile, il sistema di configurazione e la stringa di connessione può essere archiviata in `appsettings.json`, una variabile di ambiente, l'archivio user secret o un'altra origine di configurazione.</span><span class="sxs-lookup"><span data-stu-id="a9689-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="a9689-120">Vedere le [sezione di configurazione della documentazione di ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) per altri dettagli.</span><span class="sxs-lookup"><span data-stu-id="a9689-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="a9689-121">L'esempio seguente mostra la stringa di connessione archiviata nella `appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="a9689-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="a9689-122">Il contesto è generalmente configurato nel `Startup.cs` con la stringa di connessione da leggere dalla configurazione.</span><span class="sxs-lookup"><span data-stu-id="a9689-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="a9689-123">Si noti il `GetConnectionString()` metodo cerca un valore di configurazione la cui chiave è `ConnectionStrings:<connection string name>`.</span><span class="sxs-lookup"><span data-stu-id="a9689-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span> <span data-ttu-id="a9689-124">È necessario importare il [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) dello spazio dei nomi per usare questo metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="a9689-124">You need to import the [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) namespace to use this extension method.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
