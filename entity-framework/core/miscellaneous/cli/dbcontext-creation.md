---
title: Creazione in fase di progettazione DbContext - Core EF
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 8b38d300d31038bdf5cd877aa3c42b7f5f97eabc
ms.sourcegitcommit: 7113e8675f26cbb546200824512078bf360225df
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
ms.locfileid: "30202484"
---
<a name="design-time-dbcontext-creation"></a>Creazione in fase di progettazione DbContext
==============================
Alcuni dei comandi strumenti di base di Entity Framework (ad esempio, il [migrazioni] [ 1] comandi) richiedono un oggetto derivato `DbContext` istanza deve essere creato in fase di progettazione per raccogliere i dettagli relativi all'applicazione tipi di entità e il relativo mapping a uno schema di database. Nella maggior parte dei casi, è consigliabile che il `DbContext` in tal modo creato è configurato in modo analogo a come sarebbe [configurato in fase di esecuzione][2].

Sono disponibili vari modi gli strumenti di provare a creare il `DbContext`:

<a name="from-application-services"></a>Dai servizi dell'applicazione
-------------------------
Se il progetto di avvio è un'applicazione ASP.NET di base, gli strumenti di provare a ottenere l'oggetto DbContext dal provider di servizi dell'applicazione.

Lo strumento innanzitutto provare a ottenere il provider di servizi richiamando `Program.BuildWebHost()` e l'accesso di `IWebHost.Services` proprietà.

> [!NOTE]
> Quando si crea una nuova applicazione ASP.NET Core 2.0, questo hook è incluso per impostazione predefinita. Nelle versioni precedenti di EF Core e ASP.NET Core, gli strumenti di provano a richiamare `Startup.ConfigureServices` direttamente per ottenere i provider di servizi dell'applicazione, ma questo modello non funziona correttamente nelle applicazioni ASP.NET Core 2.0. Se si esegue l'aggiornamento di un'applicazione ASP.NET di base 1. x 2.0, è possibile [modificare il `Program` classe seguire il nuovo modello][3].

Il `DbContext` devono essere registrati come servizi in provider del servizio dell'applicazione stessa e tutte le dipendenze nel relativo costruttore. Ciò può essere facilmente ottenuto con [un costruttore nel `DbContext` che accetta un'istanza di `DbContextOptions<TContext>` come argomento] [ 4] e l'utilizzo di [ `AddDbContext<TContext>` (metodo)] [5].

<a name="using-a-constructor-with-no-parameters"></a>Utilizzando un costruttore senza parametri
--------------------------------------
Se l'elemento DbContext non è possibile ottenere dal provider del servizio di applicazione, gli strumenti di cercare derivata `DbContext` tipo all'interno del progetto. Quindi provare a creare un'istanza con un costruttore senza parametri. Può essere il costruttore predefinito se la `DbContext` viene configurato usando il [ `OnConfiguring` ] [ 6] metodo.

<a name="from-a-design-time-factory"></a>Da una factory in fase di progettazione
--------------------------
È anche possibile stabilire gli strumenti come creare l'elemento DbContext implementando il `IDesignTimeDbContextFactory<TContext>` interfaccia: se una classe che implementa questa interfaccia viene trovata in uno stesso progetto derivata `DbContext` o nel progetto di avvio dell'applicazione, ignorare gli strumenti altri modi di creare invece di DbContext e si usa la factory in fase di progettazione.

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
> Il `args` parametro non viene attualmente utilizzato. È presente [un problema] [ 7] la possibilità di specificare gli argomenti in fase di progettazione da strumenti di rilevamento.

Una factory in fase di progettazione può essere particolarmente utile se è necessario configurare l'elemento DbContext in modo diverso per la fase di progettazione a fase di esecuzione, se il `DbContext` accetta costruttore parametri aggiuntivi non sono registrati in DI, se non si utilizza DI affatto o se per alcuni motivo si preferisce non hanno un `BuildWebHost` nell'applicazione ASP.NET Core (metodo)  
Classe `Main`.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
