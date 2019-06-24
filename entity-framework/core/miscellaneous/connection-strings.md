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
# <a name="connection-strings"></a>Stringhe di connessione

La maggior parte dei provider di database richiede una qualche forma di stringa di connessione per connettersi al database. In alcuni casi questa stringa di connessione contiene informazioni riservate che devono essere protetti. È necessario anche modificare la stringa di connessione come si trasferiscono le applicazioni tra ambienti, ad esempio sviluppo, test e produzione.

## <a name="net-framework-applications"></a>Applicazioni .NET framework

Le applicazioni .NET framework, ad esempio WinForms, WPF, Console e ASP.NET 4, hanno un modello di stringa di connessione collaudate e testate. La stringa di connessione deve essere aggiunti al file app. config delle applicazioni (Web. config se si usa ASP.NET). Se la stringa di connessione contiene informazioni riservate, ad esempio nome utente e password, è possibile proteggere il contenuto del file di configurazione utilizzando [la configurazione protetta](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).

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
> Il `providerName` impostazione non è necessaria in stringhe di connessione EF Core archiviate nel file app. config perché il provider di database viene configurato tramite codice.

È quindi possibile leggere la stringa di connessione usando il `ConfigurationManager` API nel contesto `OnConfiguring` (metodo). Potrebbe essere necessario aggiungere un riferimento al `System.Configuration` assembly del framework per poter usare questa API.

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

## <a name="universal-windows-platform-uwp"></a>Piattaforma UWP (Universal Windows Platform)

Stringhe di connessione in un'applicazione UWP sono in genere una connessione di SQLite che specifica solo un nome di file locale. Sono in genere non contengono informazioni riservate e non è necessario essere modificata come un'applicazione viene distribuita. Di conseguenza, queste stringhe di connessione sono in genere accettabile deve rimanere nel codice, come illustrato di seguito. Se si desidera spostarli di fuori di codice di piattaforma UWP supporta il concetto di impostazioni, vedere la [sezione di impostazioni dell'App della documentazione di UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) per informazioni dettagliate.

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

## <a name="aspnet-core"></a>ASP.NET Core

In ASP.NET Core è molto flessibile, il sistema di configurazione e la stringa di connessione può essere archiviata in `appsettings.json`, una variabile di ambiente, l'archivio user secret o un'altra origine di configurazione. Vedere le [sezione di configurazione della documentazione di ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) per altri dettagli. L'esempio seguente mostra la stringa di connessione archiviata nella `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Il contesto è generalmente configurato nel `Startup.cs` con la stringa di connessione da leggere dalla configurazione. Si noti il `GetConnectionString()` metodo cerca un valore di configurazione la cui chiave è `ConnectionStrings:<connection string name>`. È necessario importare il [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) dello spazio dei nomi per usare questo metodo di estensione.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
