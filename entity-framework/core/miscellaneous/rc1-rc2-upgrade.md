---
title: L'aggiornamento da Entity Framework Core 1.0 RC1 a RC2 - Core a Entity Framework
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
ms.technology: entity-framework-core
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: ae5077c30642e3f40f51adee429821978f194460
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/15/2017
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>L'aggiornamento da Entity Framework Core 1.0 RC1 a 1.0 RC2

In questo articolo vengono fornite indicazioni per lo spostamento di un'applicazione compilata con i pacchetti RC1 a RC2.

## <a name="package-names-and-versions"></a>Versioni e i nomi di pacchetto

Tra RC1 e RC2, è stato modificato da "Entity Framework 7" a "Entity Framework Core". È possibile leggere altre informazioni sui motivi per la modifica in [questo post di Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). A causa di questa modifica, i nomi dei pacchetti impostate da `EntityFramework.*` a `Microsoft.EntityFrameworkCore.*` e la versioni da `7.0.0-rc1-final` a `1.0.0-rc2-final` (o `1.0.0-preview1-final` per gli strumenti).

**È necessario rimuovere completamente i pacchetti RC1 e quindi installare il RC2 quelle.** Di seguito è riportato il mapping per alcuni pacchetti comuni.

| Pacchetto di RC1                                               | Equivalente di RC2                                                       |
| --------------------------------------------------------- | -------------------------------------------------------------------- |
| EntityFramework.MicrosoftSqlServer 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer 1.0.0-rc2-final      |
| EntityFramework.SQLite 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Sqlite 1.0.0-rc2-final      |
| EntityFramework7.Npgsql 3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres<to be advised>      |
| EntityFramework.SqlServerCompact35 7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35 1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40 7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40 1.0.0-rc2-final      |
| EntityFramework.InMemory 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final      |
| EntityFramework.IBMDataServer 7.0.0-beta1     | Non è ancora disponibile per RC2                                            |
| EntityFramework.Commands 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design 1.0.0-rc2-final      |

## <a name="namespaces"></a>Namespaces

Insieme a nomi di pacchetto, gli spazi dei nomi è stato modificato da `Microsoft.Data.Entity.*` a `Microsoft.EntityFrameworkCore.*`. È possibile gestire questa modifica con una ricerca/sostituzione di `using Microsoft.Data.Entity` con `using Microsoft.EntityFrameworkCore`.

## <a name="table-naming-convention-changes"></a>Tabella delle modifiche di convenzione di denominazione

Una modifica significativa di funzionalità preso in RC2 consiste nell'utilizzare il nome della `DbSet<TEntity>` proprietà per un'entità specificata come nome della tabella viene eseguito il mapping, anziché solo il nome della classe. Altre informazioni su questa modifica in [il problema correlato annuncio](https://github.com/aspnet/Announcements/issues/167).

Per le applicazioni esistenti RC1, è consigliabile aggiungere il codice seguente all'inizio del `OnModelCreating` metodo per mantenere la strategia di denominazione RC1:

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Se si desidera adottare la strategia di denominazione di nuovo, sarebbe consigliabile correttamente completato il resto dei passaggi di aggiornamento quindi il codice di rimozione e la creazione di una migrazione per applicare la tabella rinominata.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>AddDbContext / Startup.cs modifica (solo per progetti ASP.NET Core)

Nella versione RC1, è necessario aggiungere i servizi di Entity Framework per il provider di servizi di applicazione, `Startup.ConfigureServices(...)`:

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

In RC2, è possibile rimuovere le chiamate a `AddEntityFramework()`, `AddSqlServer()`e così via.:

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

È inoltre necessario aggiungere un costruttore, del contesto derivato, che accetta le opzioni di contesto e li passa al costruttore di base. Ciò è necessario perché sono state rimosse alcune il trucco sconvolgere che snuck in background:

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>Passando IServiceProvider

Se si dispone di codice RC1 che passa un `IServiceProvider` al contesto, questo ha spostato in `DbContextOptions`, non è un parametro di costruttore separato. Utilizzare `DbContextOptionsBuilder.UseInternalServiceProvider(...)` per impostare il provider del servizio.

### <a name="testing"></a>Test

Lo scenario più comune per effettuare questa operazione consiste nel controllare l'ambito di un database InMemory durante il test. Vedere aggiornato [Testing](testing/index.md) articolo per un esempio di eseguire questa operazione con RC2.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Risolvere i servizi interni dal Provider di servizi di applicazione (solo progetti ASP.NET Core)

Se si dispone di un'applicazione ASP.NET Core e si desidera EF per risolvere i servizi interni dal provider del servizio dell'applicazione, si verifica un overload di `AddDbContext` che consente di configurare questa impostazione:

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> È consigliabile consentire EF internamente gestire i propri servizi, a meno che non esista un motivo per combinare i servizi EF interni in provider di servizi dell'applicazione. Il motivo principale, che si consiglia di eseguire questa operazione consiste nell'utilizzare il provider di servizi di applicazione per sostituire i servizi EF utilizzato internamente

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>I comandi DNX = > CLI .NET (solo progetti ASP.NET Core)

Se è stata utilizzata in precedenza il `dnx ef` comandi per i progetti ASP.NET 5, queste sono ora spostati `dotnet ef` comandi. Si applica comunque la stessa sintassi del comando. È possibile utilizzare `dotnet ef --help` per informazioni sulla sintassi.

Il modo in cui sono registrati i comandi è stato modificato in RC2, a causa di DNX verrà sostituito da .NET CLI. I comandi sono ora registrati un `tools` sezione `project.json`:

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> Se si utilizza Visual Studio, è ora possibile utilizzare Console di gestione pacchetti per eseguire i comandi di Entity Framework per i progetti ASP.NET Core (ciò non è supportato nella versione RC1). È necessario registrare i comandi di `tools` sezione di `project.json` per eseguire questa operazione.

## <a name="package-manager-commands-require-powershell-5"></a>I comandi di gestione pacchetti richiedono PowerShell 5

Se si utilizzano i comandi di Entity Framework nella Console di gestione pacchetti in Visual Studio, è necessario verificare di che aver installato PowerShell 5. Questo è un requisito temporaneo che verrà rimossa nella versione successiva (vedere [emettere &#5327;](https://github.com/aspnet/EntityFramework/issues/5327) per altri dettagli).

## <a name="using-imports-in-projectjson"></a>Utilizzo di "Importa" in Project

Alcune delle dipendenze del Core EF non supporta .NET Standard ancora. Core EF nei progetti .NET Standard e .NET Core potrebbe essere necessario aggiungere "Importa" per JSON come soluzione alternativa temporanea.

Quando si aggiungono EF, ripristino NuGet visualizzerà questo messaggio di errore:

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

La soluzione alternativa consiste nell'importare manualmente il profilo portabile "portabile net451 + win8". Questa forza NuGet da trattare questo file binari che corrispondono a questo fornita come un framework compatibile con .NET Standard, anche se non lo siano. Sebbene "portabile net451 + win8" non è compatibile con .NET Standard al 100%, è sufficiente compatibile per la transizione da libreria di classi Portabile per .NET Standard. Le importazioni possono essere rimossi quando le dipendenze di EF infine l'aggiornamento a .NET Standard.

È possibile aggiungere più Framework di "Importa" nella sintassi di matrice. Altre importazioni potrebbero essere necessari se si aggiungono librerie aggiuntive al progetto.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

Vedere [emettere #5176](https://github.com/aspnet/EntityFramework/issues/5176).
