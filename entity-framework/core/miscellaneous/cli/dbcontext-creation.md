---
title: Creazione DbContext della fase di progettazione-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: c36dae150085b1ab509288f6fabfdd8ed7201ca8
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812026"
---
# <a name="design-time-dbcontext-creation"></a>Creazione di DbContext in fase di progettazione

Alcuni dei comandi degli strumenti EF Core (ad esempio, i comandi delle [migrazioni][1] ) richiedono la creazione di un'istanza di `DbContext` derivata in fase di progettazione per raccogliere informazioni dettagliate sui tipi di entità dell'applicazione e su come eseguono il mapping a uno schema del database. Nella maggior parte dei casi, è preferibile che il `DbContext` creato venga configurato in modo analogo a come verrebbe [configurato][2]in fase di esecuzione.

Esistono diversi modi in cui gli strumenti tentano di creare la `DbContext`:

## <a name="from-application-services"></a>Da servizi applicazioni

Se il progetto di avvio usa il [ASP.NET Core host Web][3] o l' [host generico .NET Core][4], gli strumenti tentano di ottenere l'oggetto DbContext dal provider di servizi dell'applicazione.

Gli strumenti tentano innanzitutto di ottenere il provider di servizi richiamando `Program.CreateHostBuilder()`, chiamando `Build()`e quindi accedendo alla proprietà `Services`.

``` csharp
public class Program
{
    public static void Main(string[] args)
        => CreateHostBuilder(args).Build().Run();

    // EF Core uses this method at design time to access the DbContext
    public static IHostBuilder CreateHostBuilder(string[] args)
        => Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(
                webBuilder => webBuilder.UseStartup<Startup>());
}

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
        => services.AddDbContext<ApplicationDbContext>();
}

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

> [!NOTE]
> Quando si crea una nuova applicazione ASP.NET Core, questo hook è incluso per impostazione predefinita.

Il `DbContext` stesso e tutte le dipendenze nel costruttore devono essere registrate come servizi nel provider di servizi dell'applicazione. Questa operazione può essere eseguita facilmente con [un costruttore sul `DbContext` che accetta un'istanza di `DbContextOptions<TContext>` come argomento][5] e usando il [Metodo`AddDbContext<TContext>`][6].

## <a name="using-a-constructor-with-no-parameters"></a>Uso di un costruttore senza parametri

Se il DbContext non può essere ottenuto dal provider di servizi dell'applicazione, gli strumenti cercano il tipo di `DbContext` derivato all'interno del progetto. Tentano quindi di creare un'istanza usando un costruttore senza parametri. Può essere il costruttore predefinito se il `DbContext` viene configurato utilizzando il metodo [`OnConfiguring`][7] .

## <a name="from-a-design-time-factory"></a>Da una factory della fase di progettazione

È anche possibile indicare agli strumenti come creare il DbContext implementando l'interfaccia `IDesignTimeDbContextFactory<TContext>`: se una classe che implementa questa interfaccia si trova nello stesso progetto della `DbContext` derivata o nel progetto di avvio dell'applicazione, gli strumenti ignorano l'altro modi per creare il DbContext e usare invece la factory in fase di progettazione.

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
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

> [!NOTE]
> Il parametro `args` non è attualmente utilizzato. Si è verificato [un problema][8] durante il rilevamento della possibilità di specificare gli argomenti della fase di progettazione dagli strumenti.

Una factory della fase di progettazione può essere particolarmente utile se è necessario configurare il DbContext in modo diverso per la fase di progettazione rispetto al runtime, se il costruttore `DbContext` accetta parametri aggiuntivi non è registrato in DI, se non si usa o se per qualche motivo preferisce non avere un metodo di `BuildWebHost` nella classe `Main` dell'applicazione ASP.NET Core.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
