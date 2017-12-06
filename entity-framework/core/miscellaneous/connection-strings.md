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
---
# <a name="connection-strings"></a>Stringhe di connessione

La maggior parte dei provider di database richiede una qualche forma di stringa di connessione per connettersi al database. In alcuni casi la stringa di connessione contiene informazioni riservate che devono essere protetti. È necessario anche modificare la stringa di connessione quando si sposta l'applicazione tra ambienti, ad esempio sviluppo, test e produzione.

## <a name="net-framework-applications"></a>Applicazioni .NET framework

Applicazioni .NET framework, ad esempio Windows Form, WPF, Console e ASP.NET 4, dispongono di un modello di stringa di connessione già testate. La stringa di connessione deve essere aggiunti al file app. config delle applicazioni (Web. config se si utilizza ASP.NET). Se la stringa di connessione contiene informazioni riservate, ad esempio nome utente e password, è possibile proteggere il contenuto del file di configurazione mediante [configurazione protetta](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).

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
> Il `providerName` impostazione non è obbligatorio nei componenti di base di EF le stringhe di connessione in App. config perché il provider di database viene configurato tramite codice.

Sarà quindi possibile leggere la stringa di connessione utilizzando il `ConfigurationManager` API del contesto `OnConfiguring` metodo. Potrebbe essere necessario aggiungere un riferimento di `System.Configuration` assembly framework per poter usare questa API.

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

Le stringhe di connessione in un'applicazione UWP sono in genere una connessione specifica solo un nome di file locale di SQLite. Sono in genere non contengono informazioni riservate e non è necessario essere modificati in quanto un'applicazione viene distribuita. Di conseguenza, queste stringhe di connessione sono in genere supportate da rendere disponibile nel codice, come illustrato di seguito. Se si desidera spostarli fuori dal codice UWP supporta il concetto di impostazioni, vedere il [sezione Impostazioni App della documentazione UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) per informazioni dettagliate.

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

In ASP.NET Core, il sistema di configurazione è molto flessibile e può essere archiviata la stringa di connessione `appsettings.json`, una variabile di ambiente, l'archivio segreto utente o un'altra origine di configurazione. Vedere il [sezione di configurazione della documentazione di ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) per altri dettagli. L'esempio seguente mostra la stringa di connessione archiviata nella `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Il contesto viene in genere configurato `Startup.cs` con la stringa di connessione da leggere dalla configurazione. Si noti il `GetConnectionString()` metodo cerca un valore di configurazione, la cui chiave è `ConnectionStrings:<connection string name>`.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
