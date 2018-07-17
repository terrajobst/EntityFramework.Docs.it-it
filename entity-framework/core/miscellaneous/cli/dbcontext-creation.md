---
title: Creazione DbContext in fase di progettazione - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 7c16017d3b97d115841050fe6ac0fdbeb5e71d94
ms.sourcegitcommit: 00cb52625b57c1ea339ded1454179fe89b6bcfea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/16/2018
ms.locfileid: "39067531"
---
<a name="design-time-dbcontext-creation"></a>Creazione DbContext in fase di progettazione
==============================
Alcuni dei comandi degli strumenti di Entity Framework Core (ad esempio, il [migrazioni] [ 1] comandi) richiedono un oggetto derivato `DbContext` istanza deve essere creato in fase di progettazione per raccogliere i dettagli relativi all'applicazione tipi di entità e sul relativo mapping a uno schema di database. Nella maggior parte dei casi, è consigliabile che il `DbContext` creato in questo modo è configurata in modo analogo a come sarebbe [configurato in fase di esecuzione][2].

Esistono diversi modi, provare gli strumenti per creare il `DbContext`:

<a name="from-application-services"></a>Dai servizi dell'applicazione
-------------------------
Se il progetto di avvio è un'app ASP.NET Core, strumenti di tentare di ottenere l'oggetto DbContext da provider di servizi dell'applicazione.

Lo strumento tenta innanzitutto di ottenere il provider di servizi richiamando `Program.BuildWebHost()` e l'accesso di `IWebHost.Services` proprietà.

> [!NOTE]
> Quando si crea una nuova applicazione ASP.NET Core 2.0, questo hook è incluso per impostazione predefinita. Nelle versioni precedenti di EF Core e ASP.NET Core, strumenti di provano a richiamare `Startup.ConfigureServices` direttamente per ottenere il provider di servizi dell'applicazione, ma questo modello non sono più funzioni correttamente nelle applicazioni ASP.NET Core 2.0. Se si esegue l'aggiornamento di un'applicazione di ASP.NET Core 1.x a 2.0, è possibile [modificare il `Program` classe seguire il nuovo modello][3].

Il `DbContext` se stesso e tutte le dipendenze nel costruttore devono essere registrati come servizi in provider di servizi dell'applicazione. Ciò può essere ottenuta facilmente facendo in modo che [costruttore sul `DbContext` che accetta un'istanza di `DbContextOptions<TContext>` come argomento] [ 4] e l'uso di [ `AddDbContext<TContext>` (metodo)] [5].

<a name="using-a-constructor-with-no-parameters"></a>Usando un costruttore senza parametri
--------------------------------------
Se DbContext non può essere ottenuto dal provider del servizio dell'applicazione, gli strumenti di cercare derivato `DbContext` tipo all'interno del progetto. Quindi provano a creare un'istanza con un costruttore senza parametri. Questo può essere il costruttore predefinito se la `DbContext` viene configurato usando il [ `OnConfiguring` ] [ 6] (metodo).

<a name="from-a-design-time-factory"></a>Da una factory in fase di progettazione
--------------------------
È anche possibile indicare gli strumenti come creare l'oggetto DbContext implementando il `IDesignTimeDbContextFactory<TContext>` interfaccia: se una classe che implementa questa interfaccia si trova in uno stesso progetto derivato `DbContext` o nel progetto di avvio dell'applicazione, gli strumenti da ignorare le altre modalità di creazione invece DbContext e utilizzare la factory in fase di progettazione.

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

Una factory in fase di progettazione può essere particolarmente utile se è necessario configurare DbContext in modo diverso per la fase di progettazione più in fase di esecuzione se il `DbContext` accetta costruttore i parametri aggiuntivi non sono registrati nell'inserimento di dipendenze, se non si usa l'inserimento delle dipendenze del tutto o se per alcuni motivo si preferisce non hanno una `BuildWebHost` metodo nell'applicazione ASP.NET Core `Main` classe.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
