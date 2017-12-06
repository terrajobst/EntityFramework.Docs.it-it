---
title: Configurazione di un elemento DbContext - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 96abf3b94be3e1d19f833644f1c2f6f13fe0e730
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2017
---
# <a name="configuring-a-dbcontext"></a>Configurazione di un elemento DbContext

Questo articolo illustra i modelli per la configurazione di un `DbContext` con `DbContextOptions`. Le opzioni vengono utilizzate principalmente per selezionare e configurare l'archivio dati.

## <a name="configuring-dbcontextoptions"></a>Configurazione DbContextOptions

`DbContext`deve avere un'istanza di `DbContextOptions` per l'esecuzione. È possibile configurare eseguendo l'override `OnConfiguring`, o fornita esternamente tramite un argomento del costruttore.

Se si utilizzano entrambi, `OnConfiguring` viene eseguita con le opzioni fornite, pertanto è additivo e può sovrascrivere le opzioni fornite per l'argomento del costruttore.

### <a name="constructor-argument"></a>Argomento del costruttore

Codice di contesto con un costruttore

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]  
> Il costruttore di base di DbContext accetta inoltre la versione non generica di `DbContextOptions`. Per le applicazioni con più tipi di contesto non è consigliabile usare la versione non generica.

Codice dell'applicazione per inizializzare dall'argomento del costruttore

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

> [!WARNING]  
> `OnConfiguring`ultima e può essere sovrascritto ottenute dal costruttore o DI opzioni. Questo approccio non si presta a test (a meno che non tutto il database di destinazione).

Codice di contesto con `OnConfiguring`:

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

Il codice dell'applicazione per inizializzare con `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a>Con l'inserimento di dipendenze DbContext

Supporta l'utilizzo di EF `DbContext` con un contenitore dell'inserimento di dipendenza. Il tipo DbContext può essere aggiunti al contenitore del servizio con `AddDbContext<TContext>`.

`AddDbContext`renderà sia il tipo DbContext, `TContext`, e `DbContextOptions<TContext>` disponibili per l'aggiunta dal contenitore del servizio.

Vedere [lettura più](#more-reading) sotto per informazioni sull'inserimento di dipendenze.

Aggiunta di dbcontext per l'inserimento di dipendenze

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Questo richiede l'aggiunta di un [argomento del costruttore](#constructor-argument) al tipo DbContext che accetta `DbContextOptions`.

Codice di contesto:

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

Codice dell'applicazione (in ASP.NET Core):

``` csharp
public MyController(BloggingContext context)
```

Codice dell'applicazione (tramite ServiceProvider direttamente, meno comune):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a>Utilizzo`IDesignTimeDbContextFactory<TContext>`

In alternativa, per le opzioni sopra riportate, è inoltre possibile fornire un'implementazione di `IDesignTimeDbContextFactory<TContext>`. Strumenti di Entity Framework è possono utilizzare questa factory per creare un'istanza del DbContext. Questo potrebbe essere necessario per abilitare esperienze in fase di progettazione specifiche, ad esempio le migrazioni.

Implementare questa interfaccia per abilitare i servizi in fase di progettazione per i tipi di contesto che non dispongono di un costruttore predefinito pubblico. Servizi in fase di progettazione rileverà automaticamente le implementazioni di questa interfaccia nello stesso assembly come contesto derivato.

Esempio:

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

## <a name="more-reading"></a>Lettura di più

* Lettura [introduzione su ASP.NET Core](../get-started/aspnetcore/index.md) per ulteriori informazioni sull'utilizzo di Entity Framework con ASP.NET Core.
* Lettura [Dependency Injection](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) per ulteriori informazioni sull'utilizzo DI.
* Lettura [Testing](testing/index.md) per ulteriori informazioni.
