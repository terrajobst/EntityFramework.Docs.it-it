---
title: Creazione in fase di progettazione DbContext - Core EF
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 5fcd9e362d76127e7acadd9e552ef3ac90967a37
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
<a name="design-time-dbcontext-creation"></a>Creazione in fase di progettazione DbContext
==============================
Alcuni degli strumenti di Entity Framework comandi richiedono un'istanza di DbContext deve essere creato in fase di progettazione ora (ad esempio, quando esegue i comandi di migrazioni). Sono disponibili vari modi che gli strumenti di tentare di crearlo.

<a name="from-application-services"></a>Dai servizi dell'applicazione
-------------------------
Se il progetto di avvio è un'applicazione ASP.NET di base, gli strumenti di provare a ottenere l'oggetto DbContext dal provider di servizi dell'applicazione. Ottengono richiamando `Program.BuildWebHost()` e l'accesso di `IWebHost.Services` proprietà. Qualsiasi DbContext registrati mediante `IServiceCollection.AddDbContext<TContext>()` può essere trovato e create in questo modo. Questo modello è stato [introdotto in ASP.NET 2.0 Core][1]

<a name="using-the-default-constructor"></a>Utilizzando il costruttore predefinito
-----------------------------
Se l'elemento DbContext non è possibile ottenere dal provider del servizio di applicazione, gli strumenti di ricerca per il tipo DbContext all'interno del progetto. Provare a creare utilizzando il costruttore predefinito.

<a name="from-a-design-time-factory"></a>Da una factory in fase di progettazione
--------------------------
È anche possibile stabilire gli strumenti come creare l'elemento DbContext implementando `IDesignTimeDbContextFactory`. Se una classe che implementa questa interfaccia viene trovata all'interno del progetto, gli strumenti di ignorare le altre modalità di creazione DbContext.
Essi utilizzano sempre la factory in fase di progettazione. Una factory è particolarmente utile se è necessario configurare l'elemento DbContext in modo diverso per la fase di progettazione di in fase di esecuzione.

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

> [!NOTE]
> Il `args` parametro non viene attualmente utilizzato. È presente [un problema] [ 2] la possibilità di specificare gli argomenti in fase di progettazione da strumenti di rilevamento.

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
