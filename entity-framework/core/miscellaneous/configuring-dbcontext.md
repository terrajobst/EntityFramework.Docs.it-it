---
title: Configurazione di un oggetto DbContext - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 316d363d4a1b8a909efc1c32b492280c0d16cb4e
ms.sourcegitcommit: 960e42a01b3a2f76da82e074f64f52252a8afecc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/08/2019
ms.locfileid: "65405215"
---
# <a name="configuring-a-dbcontext"></a>Configurazione di un DbContext

Questo articolo illustra i modelli di base per la configurazione di un `DbContext` tramite un `DbContextOptions` per connettersi a un database con uno specifico provider di EF Core e comportamenti facoltativi.

## <a name="design-time-dbcontext-configuration"></a>Configurazione Design-time DbContext

Fase di progettazione di Entity Framework Core strumenti quali [migrazioni](xref:core/managing-schemas/migrations/index) devono essere in grado di individuare e creare un'istanza di lavoro di un `DbContext` tipo per raccogliere informazioni dettagliate sui tipi di entità dell'applicazione e sul relativo mapping a uno schema di database. Questo processo può essere automatica, purché lo strumento è possibile creare con facilità il `DbContext` in modo tale che verrà configurato in modo analogo al modo in cui dovrebbe essere configurato in fase di esecuzione.

Mentre qualsiasi criterio di ricerca che fornisce le informazioni necessarie per la configurazione per il `DbContext` può funzionare in fase di esecuzione, gli strumenti che richiede l'uso di un `DbContext` in fase di progettazione può funzionare solo con un numero limitato di modelli. Questi prerequisiti sono analizzati più dettagliatamente la [creare il contesto in fase di progettazione](xref:core/miscellaneous/cli/dbcontext-creation) sezione.

## <a name="configuring-dbcontextoptions"></a>Configurazione di DbContextOptions

`DbContext` deve avere un'istanza di `DbContextOptions` per poter eseguire qualsiasi operazione. Il `DbContextOptions` istanza contiene le informazioni di configurazione, ad esempio:

- Il provider di database da utilizzare, in genere selezionata richiamando un metodo, ad esempio `UseSqlServer` o `UseSqlite`. Questi metodi di estensione richiedono che il pacchetto di provider corrispondente, ad esempio `Microsoft.EntityFrameworkCore.SqlServer` o `Microsoft.EntityFrameworkCore.Sqlite`. I metodi sono definiti nel `Microsoft.EntityFrameworkCore` dello spazio dei nomi.
- Qualsiasi stringa di connessione necessaria o identificatore dell'istanza del database, in genere passata come argomento al metodo di selezione del provider indicato in precedenza
- Tutti i selettori comportamento opzionale a livello del provider, in genere anche concatenati all'interno della chiamata al metodo di selezione del provider
- Tutti i selettori generali comportamento di EF Core, in genere concatenati dopo o prima del metodo di selezione del provider

L'esempio seguente configura il `DbContextOptions` per usare il provider SQL Server, contenuto in una connessione di `connectionString` variabile, un timeout del comando a livello del provider e un selettore di comportamento di EF Core che esegue tutte le query eseguite nel `DbContext` [senza registrazione](xref:core/querying/tracking#no-tracking-queries) per impostazione predefinita:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Metodi di selezione del provider e altri metodi di comportamento selettore indicato in precedenza sono i metodi di estensione su `DbContextOptions` o classi di opzioni specifiche del provider. Per accedere a questi metodi di estensione potrebbe essere necessario disporre di uno spazio dei nomi (in genere `Microsoft.EntityFrameworkCore`) nell'ambito e includere le dipendenze aggiuntive di un pacchetto nel progetto.

Il `DbContextOptions` può essere fornito per il `DbContext` eseguendo l'override di `OnConfiguring` (metodo) o esternamente tramite un argomento del costruttore.

Se vengono utilizzati entrambi, `OnConfiguring` viene applicata per ultima ed possibile sovrascrivere le opzioni fornite per l'argomento del costruttore.

### <a name="constructor-argument"></a>Argomento del costruttore

Codice del contesto con il costruttore:

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
> Il costruttore di base di DbContext accetta anche la versione non generica di `DbContextOptions`, ma usa la versione non generica non è consigliata per le applicazioni con più tipi di contesto.

Codice dell'applicazione per l'inizializzazione dall'argomento di costruttore:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

Codice del contesto con `OnConfiguring`:

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

Il codice dell'applicazione per inizializzare un `DbContext` che usa `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Questo approccio non adatto all'esecuzione di test, a meno che il test completo del database di destinazione.

### <a name="using-dbcontext-with-dependency-injection"></a>Utilizzo di DbContext con inserimento delle dipendenze

EF Core supporta l'utilizzo di `DbContext` con un contenitore di inserimento delle dipendenze. Il tipo DbContext può essere aggiunti al contenitore del servizio tramite il `AddDbContext<TContext>` (metodo).

`AddDbContext<TContext>` renderà sia del tipo DbContext `TContext`e le corrispondenti `DbContextOptions<TContext>` disponibile per l'inserimento dal contenitore dei servizi.

Visualizzare [ulteriori informazioni](#more-reading) sotto per altre informazioni sull'inserimento delle dipendenze.

Aggiunta di `Dbcontext` all'inserimento delle dipendenze:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Ciò richiede l'aggiunta di un [argomento del costruttore](#constructor-argument) per il tipo DbContext che accetta `DbContextOptions<TContext>`.

Codice contesto:

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
public class MyController
{
    private readonly BloggingContext _context;

    public MyController(BloggingContext context)
    {
      _context = context;
    }

    ...
}
```

Codice dell'applicazione (tramite ServiceProvider direttamente, meno comune):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a>Come evitare problemi relativi al threading DbContext

Entity Framework Core non supporta più operazioni in parallelo in esecuzione sullo stesso `DbContext` istanza. Ciò include l'esecuzione parallela di query asincrona e qualsiasi uso esplicito simultanea da più thread. Di conseguenza, always `await` asincrono chiama immediatamente oppure usare separato `DbContext` istanze per le operazioni eseguite in parallelo.

EF Core quando rileva che un tentativo di usare una `DbContext` istanza contemporaneamente, si noterà un `InvalidOperationException` con un messaggio simile al seguente: 

> Una seconda operazione avviata in questo contesto prima dell'operazione precedente completata. Ciò è causato generalmente da thread diversi utilizzando la stessa istanza di DbContext, ma i membri di istanza sono necessariamente essere thread-safe.

Quando viene rilevato accesso simultaneo, può comportare un comportamento indefinito, arresti anomali delle applicazioni e il danneggiamento dei dati.

Sono disponibili gli errori comuni che possono inadvernetly causa accesso simultaneo nella stessa `DbContext` istanza:

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>Se si dimentica di attendere il completamento di un'operazione asincrona prima di avviare qualsiasi altra operazione in stesso DbContext

I metodi asincroni abilitato EF Core avviare le operazioni che accedono al database in modo non bloccante. Tuttavia, se un chiamante non attende il completamento di uno di questi metodi e continua a eseguire altre operazioni sul `DbContext`, dello stato del `DbContext` può essere (e molto probabilmente verrà) danneggiato. 

Attendere sempre i metodi asincroni di EF Core immediatamente.  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>In modo implicito la condivisione di istanze DbContext attraverso più thread tramite inserimento delle dipendenze

Il [ `AddDbContext` ](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) registra il metodo di estensione `DbContext` tipi con una [durata con ambito](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) per impostazione predefinita. 

Ciò è protetta da problemi di accesso simultaneo in applicazioni ASP.NET Core, poiché è presente un solo thread per l'esecuzione di ogni richiesta client in un determinato momento e poiché ogni richiesta di ottenere un ambito di inserimento delle dipendenze separato (e pertanto un oggetto separato `DbContext` istanza).

Tuttavia necessario assicurarsi che qualsiasi codice eseguito in modo esplicito più thread in parallelo `DbContext` istanze non sono mai accesed contemporaneamente.

Mediante l'inserimento di dipendenza, questo scopo, il contesto viene registrato come ambiti hanno come ambiti e la creazione (tramite `IServiceScopeFactory`) per ogni thread, o registrando il `DbContext` temporanei (usando l'overload di `AddDbContext` che accetta un `ServiceLifetime` parametro).

## <a name="more-reading"></a>Ulteriori informazioni

* Lettura [Guida introduttiva ad ASP.NET Core](../get-started/aspnetcore/index.md) per altre informazioni sull'uso di EF con ASP.NET Core.
* Lettura [inserimento delle dipendenze](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) per altre informazioni sull'uso di inserimento delle dipendenze.
* Lettura [Testing](testing/index.md) per altre informazioni.
