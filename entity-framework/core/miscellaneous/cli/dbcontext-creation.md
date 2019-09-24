---
title: Creazione DbContext della fase di progettazione-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f83d4b16227d114a1cac1514667484a908fea4ac
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197571"
---
<a name="design-time-dbcontext-creation"></a>Creazione DbContext della fase di progettazione
==============================
Alcuni dei comandi degli strumenti di EF Core (ad esempio, i comandi delle [migrazioni][1] ) richiedono la `DbContext` creazione di un'istanza derivata in fase di progettazione per raccogliere informazioni dettagliate sui tipi di entità dell'applicazione e su come eseguono il mapping a uno schema del database. Nella maggior parte dei casi, è preferibile che il `DbContext` creato sia configurato in modo analogo a come verrebbe [configurato][2]in fase di esecuzione.

Esistono diversi modi in cui gli strumenti tentano di `DbContext`creare:

<a name="from-application-services"></a>Da servizi applicazioni
-------------------------
Se il progetto di avvio usa il [ASP.NET Core host Web][3] o l' [host generico .NET Core][4], gli strumenti tentano di ottenere l'oggetto DbContext dal provider di servizi dell'applicazione.

Per prima cosa gli strumenti tentano di ottenere il provider di `Program.CreateHostBuilder()`Servizi richiamando `Build()`, quindi accedendo alla `Services` proprietà.

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

La `DbContext` stessa e tutte le dipendenze nel costruttore devono essere registrate come servizi nel provider di servizi dell'applicazione. Questa operazione può essere eseguita facilmente con [un `DbContext` Costruttore su che accetta un'istanza di `DbContextOptions<TContext>` come argomento][5] e usando il [ `AddDbContext<TContext>` metodo][6].

<a name="using-a-constructor-with-no-parameters"></a>Uso di un costruttore senza parametri
--------------------------------------
Se il DbContext non può essere ottenuto dal provider di servizi dell'applicazione, gli strumenti cercano il `DbContext` tipo derivato all'interno del progetto. Tentano quindi di creare un'istanza usando un costruttore senza parametri. Può essere il costruttore predefinito se `DbContext` è configurato utilizzando il [`OnConfiguring`][7] metodo.

<a name="from-a-design-time-factory"></a>Da una factory della fase di progettazione
--------------------------
È anche possibile indicare agli strumenti come creare il DbContext implementando l' `IDesignTimeDbContextFactory<TContext>` interfaccia: Se una classe che implementa questa interfaccia si trova nello stesso progetto dell'oggetto derivato `DbContext` o nel progetto di avvio dell'applicazione, gli strumenti ignorano le altre modalità di creazione di DbContext e usano invece la factory della fase di progettazione.

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
> Il `args` parametro non è attualmente utilizzato. Si è verificato [un problema][8] durante il rilevamento della possibilità di specificare gli argomenti della fase di progettazione dagli strumenti.

Una factory della fase di progettazione può essere particolarmente utile se è necessario configurare DbContext in modo diverso per la fase di progettazione rispetto al runtime, `DbContext` se il costruttore accetta parametri aggiuntivi non viene registrato in di, se non si utilizza o se per alcuni motivo per cui si preferisce non avere `BuildWebHost` un metodo nella `Main` classe dell'applicazione ASP.NET Core.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
