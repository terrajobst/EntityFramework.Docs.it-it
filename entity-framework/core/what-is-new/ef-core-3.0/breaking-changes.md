---
title: Modifiche che causano un'interruzione in EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 748db8a71a04a2d696ef21a03319906b9fc776be
ms.sourcegitcommit: a709054b2bc7a8365201d71f59325891aacd315f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829226"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a>Modifiche che causano un'interruzione incluse in EF Core 3.0 (attualmente in anteprima)

> [!IMPORTANT]
> Si tenga presente che i set di funzionalità e le pianificazioni delle versioni future sono sempre soggette a modifiche e che questa pagina, nonostante l'impegno profuso per mantenerla aggiornata, potrebbe non riflettere sempre i piani più recenti.

Le modifiche all'API e al comportamento seguenti possono potenzialmente interrompere le applicazioni sviluppate per EF Core 2.2.x durante l'aggiornamento alla versione 3.0.0.
Le modifiche che si prevede abbiano impatto solo sui provider di database sono documentate nelle [modifiche che influiscono sul provider](../../providers/provider-log.md).
Le interruzioni nelle nuove funzionalità introdotte da un'anteprima 3.0 a un'altra anteprima 3.0 non sono documentate.

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a>Le query LINQ non vengono più valutate nel client

[Problema n. 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Vedere anche il problema n. 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.

**Comportamento precedente**

Nelle versioni precedenti alla versione 3.0, quando EF Core non era in grado di convertire un'espressione inclusa in una query in SQL o in un parametro, l'espressione veniva automaticamente valutata nel client.
Per impostazione predefinita, la valutazione client di espressioni potenzialmente dispendiose si limitava ad attivare solo un avviso.

**Nuovo comportamento**

A partire dalla versione 3.0, EF Core consente solo la valutazione delle espressioni nella proiezione di primo livello (l'ultima chiamata `Select()` nella query) nel client.
Quando le espressioni in altre posizioni all'interno della query non possono essere convertite in SQL o in un parametro, viene generata un'eccezione.

**Perché?**

La valutazione client automatica delle query consente di eseguire numerose query anche nel caso in cui parti importanti delle query non possono essere convertite.
Questo comportamento può causare un comportamento imprevisto e potenzialmente dannoso che può diventare evidente solo in produzione.
Ad esempio, una condizione in una chiamata `Where()` che non può essere convertita può causare il trasferimento di tutte le righe della tabella del server di database e l'applicazione del filtro nel client.
È probabile che questa situazione non venga rilevata se la tabella contiene solo alcune righe in fase di sviluppo, ma che abbia un grande impatto quando l'applicazione passa in produzione dove la tabella può contenere milioni di righe.
Gli avvisi di valutazione client inoltre si sono rivelati molto facili da ignorare durante lo sviluppo.

Inoltre, la valutazione client automatica può causare problemi in cui il miglioramento della conversione di query per espressioni specifiche causa modifiche impreviste che causano un'interruzione da una versione all'altra.

**Mitigazioni**

Se una query non può essere convertita completamente, riscrivere la query in un formato che possa essere convertito o usare `AsEnumerable()`, `ToList()` o un elemento simile per riportare in modo esplicito i dati nel client dove possono essere quindi ulteriormente elaborati usando LINQ to Objects.

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a>Entity Framework Core non è più incluso nel framework condiviso di ASP.NET Core

[Annunci problema n. 325](https://github.com/aspnet/Announcements/issues/325)

Questa modifica è stata introdotta in ASP.NET Core 3.0 anteprima 1. 

**Comportamento precedente**

Nelle versioni precedenti ad ASP.NET Core 3.0, quando si aggiungeva un riferimento a un pacchetto in `Microsoft.AspNetCore.App` o `Microsoft.AspNetCore.All`, veniva inserito EF Core e alcuni dei provider di dati di EF Core come il provider di SQL Server.

**Nuovo comportamento**

A partire dalla versione 3.0, il framework condiviso di ASP.NET Core non include EF Core o provider di dati di EF Core.

**Perché?**

Prima di questa modifica, per ottenere EF Core erano necessarie procedure diverse, a seconda che l'applicazione avesse o meno come destinazione ASP.NET Core e SQL Server. Inoltre, l'aggiornamento di ASP.NET Core forzava l'aggiornamento di EF Core e del provider di SQL Server, non sempre auspicabile.

Con questa modifica, la procedura per ottenere EF Core è la stessa in tutti i provider, le implementazioni .NET supportate e i tipi di applicazioni.
Gli sviluppatori possono ora controllare anche in modo preciso quando vengono aggiornati EF Core e i provider di dati di EF Core.

**Mitigazioni**

Per usare EF Core in un'applicazione ASP.NET Core 3.0 o in un'altra applicazione supportata, aggiungere in modo esplicito un riferimento al pacchetto al provider di database di EF Core che verrà usato dall'applicazione.

## <a name="query-execution-is-logged-at-debug-level"></a>L'esecuzione di query viene registrata a livello di debug

[Problema n. 14523](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.

**Comportamento precedente**

Nelle versioni precedenti a EF Core 3.0 l'esecuzione di query e altri comandi veniva registrata a livello `Info`.

**Nuovo comportamento**

A partire da EF Core 3.0, la registrazione dell'esecuzione di comandi/SQL è a livello `Debug`.

**Perché?**

Questa modifica è stata apportata per ridurre i disturbi a livello di registrazione `Info`.

**Mitigazioni**

Questo evento di registrazione è definito da `RelationalEventId.CommandExecuting` con l'ID evento 20100.
Per registrare SQL di nuovo al livello `Info`, configurare in modo esplicito il livello in `OnConfiguring` o `AddDbContext`.
Ad esempio:
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a>I valori di chiave temporanei non sono più impostati nelle istanze di entità

[Problema n. 12378](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.

**Comportamento precedente**

Nelle versioni precedenti a EF Core 3.0 i valori temporanei venivano assegnati a tutte le proprietà di chiave per cui veniva in seguito generato un valore reale dal database.
In genere questi valori temporanei erano numeri negativi elevati.

**Nuovo comportamento**

A partire dalla versione 3.0, EF Core archivia il valore di chiave temporaneo come parte delle informazioni di rilevamento dell'entità e non modifica la proprietà di chiave.

**Perché?**

Questa modifica è stata apportata per impedire che i valori di chiave temporanei diventino erroneamente permanenti quando un'entità rilevata in precedenza da un'istanza `DbContext` viene spostata in un'altra istanza `DbContext`. 

**Mitigazioni**

Le applicazioni che assegnano valori di chiave primaria in chiavi esterne per creare associazioni tra le entità possono dipendere dal comportamento precedente se le chiavi primarie vengono generate dall'archivio e appartengono a entità con stato `Added`.
Questo può essere evitato:
* Non usando chiavi generate dall'archivio.
* Impostando le proprietà di navigazione in modo da creare relazioni anziché impostando valori di chiave esterna.
* Ottenendo i valori di chiave temporanei effettivi dalle informazioni di rilevamento dell'entità.
Ad esempio, `context.Entry(blog).Property(e => e.Id).CurrentValue` restituisce il valore temporaneo anche quando `blog.Id` non è stato impostato.

## <a name="detectchanges-honors-store-generated-key-values"></a>DetectChanges rispetta i valori di chiave generati dall'archivio

[Problema n. 14616](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.

**Comportamento precedente**

Nelle versioni precedenti a EF Core 3.0 un'entità non rilevata individuata da `DetectChanges` veniva rilevata nello stato `Added` e inserita come nuova riga quando veniva eseguita una chiamata a `SaveChanges`.

**Nuovo comportamento**

A partire da EF Core 3.0, se un'entità usa valori di chiave generati e viene impostato un valore di chiave, l'entità viene rilevata nello stato `Modified`.
Ciò significa che si presuppone l'esistenza di una riga per l'entità che viene aggiornata quando viene eseguita una chiamata a `SaveChanges`.
Se il valore di chiave non viene impostato o se il tipo di entità non usa chiavi generate, la nuova entità viene rilevata come `Added` come nelle versioni precedenti.

**Perché?**

Questa modifica è stata apportata per rendere più semplice e coerente l'uso di grafici di entità disconnesse con chiavi generate dall'archivio.

**Mitigazioni**

Questa modifica può interrompere un'applicazione se un tipo di entità è configurato per l'uso di chiavi generate ma i valori di chiave sono impostati in modo esplicito per le nuove istanze.
La correzione consiste nel configurare in modo esplicito le proprietà di chiave per non usare valori generati.
Ad esempio, con l'API Fluent:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

Oppure con annotazioni dei dati:

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a>Le eliminazioni a catena vengono ora eseguite immediatamente per impostazione predefinita

[Problema n. 10114](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.

**Comportamento precedente**

Nelle versioni precedenti alla versione 3.0, EF Core applicava azioni a catena (eliminazione delle entità dipendenti quando veniva eliminata un'entità di sicurezza obbligatoria o veniva recisa la relazione con un'entità di sicurezza obbligatoria) solo dopo la chiamata a SaveChanges.

**Nuovo comportamento**

A partire dalla versione 3.0, EF Core applica le azioni a catena non appena viene rilevata la condizione di attivazione.
Ad esempio, la chiamata a `context.Remove()` per eliminare un'entità di sicurezza causa anche l'impostazione immediata di tutti i dipendenti obbligatori correlati rilevati su `Deleted`.

**Perché?**

Questa modifica è stata apportata per migliorare l'esperienza di associazione di dati e degli scenari di controllo in cui è importante individuare le entità che verranno eliminate _prima_ della chiamata a `SaveChanges`.

**Mitigazioni**

Il comportamento precedente può essere ripristinato tramite le impostazioni in `context.ChangedTracker`.
Ad esempio:

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a>I tipi di query vengono consolidati con tipi di entità

[Problema n. 14194](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.

**Comportamento precedente**

Nelle versioni precedenti a EF Core 3.0 i [tipi di query](xref:core/modeling/query-types) erano uno strumento per eseguire query su dati che non definiscono una chiave primaria in modo strutturato.
Veniva infatti usato un tipo di query per eseguire il mapping di tipi di entità senza chiavi (più probabilmente da una vista, ma anche da una tabella), mentre veniva usato un tipo di entità normale quando era disponibile una chiave (più probabilmente da una tabella, ma anche da una vista).

**Nuovo comportamento**

Un tipo di query diventa ora semplicemente un tipo di entità senza chiave primaria.
I tipi di entità senza chiave hanno la stessa funzionalità dei tipi di query nelle versioni precedenti.

**Perché?**

Questa modifica è stata apportata per ridurre la confusione riguardo lo scopo dei tipi di query.
In particolare, si tratta di tipi di entità senza chiave che sono intrinsecamente di sola lettura per questo motivo ma non dovrebbero essere usati solo perché un tipo di entità deve essere di sola lettura.
Analogamente, spesso vengono mappati alle viste solo perché le viste spesso non definiscono le chiavi.

**Mitigazioni**

Le parti dell'API seguenti sono ora obsolete:
* **`ModelBuilder.Query<>()`** - È necessario chiamare `ModelBuilder.Entity<>().HasNoKey()` per contrassegnare un tipo di entità come tipo senza chiavi.
Non ne viene eseguita la configurazione per convenzione per evitare una configurazione errata quando è prevista una chiave primaria che tuttavia non corrisponde alla convenzione.
* **`DbQuery<>`** - Usare `DbSet<>`.
* **`DbContext.Query<>()`** - Usare `DbContext.Set<>()`.

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a>L'API di configurazione per le relazioni di tipo di proprietà è stata modificata

[Problema n. 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Problema n. 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Problema n. 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)

Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.

**Comportamento precedente**

Nelle versioni precedenti a EF Core 3.0 la configurazione della relazione di proprietà veniva eseguita direttamente dopo la chiamata a `OwnsOne` o `OwnsMany`. 

**Nuovo comportamento**

A partire da EF Core 3.0, è disponibile l'API Fluent per configurare una proprietà di navigazione per il proprietario usando `WithOwner()`.
Ad esempio:

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

La configurazione correlata alla relazione tra proprietario e elemento di proprietà deve essere ora concatenata dopo `WithOwner()` in modo analogo a come vengono configurate altre relazioni.
La configurazione per il tipo di proprietà viene comunque concatenata dopo `OwnsOne()/OwnsMany()`.
Ad esempio:

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

Inoltre, la chiamata a `Entity()`, `HasOne()` o `Set()` con una destinazione di tipo di proprietà genera ora un'eccezione.

**Perché?**

Questa modifica è stata apportata per creare una separazione più netta tra la configurazione del tipo di proprietà e la _relazione_ con il tipo di proprietà.
Ciò consente di eliminare ambiguità e confusione su metodi come `HasForeignKey`.

**Mitigazioni**

Modificare la configurazione delle relazioni dei tipi di proprietà per usare la superficie della nuova API come illustrato nell'esempio precedente.

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a>La convenzione di proprietà di chiave esterna non ha più lo stesso nome della proprietà dell'entità di sicurezza

[Problema n. 13274](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.

**Comportamento precedente**

Si consideri il modello seguente:
```C#
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}

```
Nelle versioni precedenti a EF Core 3.0 veniva usata la proprietà `CustomerId` per la chiave esterna per convenzione.
Tuttavia, se `Order` è un tipo di proprietà, `CustomerId` sarebbe la chiave primaria e ciò non è in genere auspicabile.

**Nuovo comportamento**

A partire da 3.0, EF Core non tenta di usare le proprietà per le chiavi esterne per convenzione se hanno lo stesso nome della proprietà dell'entità di sicurezza.
Viene ancora eseguita la corrispondenza tra i criteri del nome del tipo dell'entità di sicurezza concatenato al nome della proprietà dell'entità di sicurezza e il nome di navigazione concatenato al nome della proprietà dell'entità di sicurezza.
Ad esempio:

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

**Perché?**

Questa modifica è stata apportata per evitare una definizione errata della proprietà di chiave primaria nel tipo di proprietà.

**Mitigazioni**

Se la proprietà è stata progettata per essere la chiave esterna e di conseguenza parte della chiave primaria, configurarla in modo esplicito come chiave esterna.

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a>Ogni proprietà usa la generazione di chiavi di tipo intero in memoria indipendenti

[Problema n. 6872](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.

**Comportamento precedente**

Nelle versioni precedenti a EF Core 3.0 veniva usato un unico generatore di valori condiviso per tutte le proprietà di chiavi di tipo intero in memoria.

**Nuovo comportamento**

A partire da EF Core 3.0, ogni proprietà di chiave di tipo intero riceve un generatore di valori quando viene usato il database in memoria.
Inoltre, se il database viene eliminato, la generazione di chiavi viene reimpostata per tutte le tabelle.

**Perché?**

Questa modifica è stata apportata per allineare maggiormente la generazione di chiavi in memoria alla generazione di chiavi del database reale e per migliorare la possibilità di isolare i test l'uno dall'altro quando viene usato il database in memoria.

**Mitigazioni**

Ciò può interrompere un'applicazione che si basa sull'impostazione di valori di chiave in memoria specifici.
È consigliabile non basare l'applicazione su valori di chiave specifici o eseguire l'aggiornamento per passare al nuovo comportamento.

## <a name="backing-fields-are-used-by-default"></a>I campi sottostanti vengono usati per impostazione predefinita

[Problema n. 12430](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.

**Comportamento precedente**

Nelle versioni precedenti alla versione 3.0, anche se il campo sottostante di una proprietà era noto, per impostazione predefinita EF Core eseguiva la lettura e la scrittura del valore della proprietà usando i metodi getter e setter della proprietà.
L'eccezione era costituita dall'esecuzione di query in cui il campo sottostante, se noto, veniva impostato direttamente.

**Nuovo comportamento**

A partire da EF Core 3.0, se il campo sottostante di una proprietà è noto, la lettura e la scrittura della proprietà vengono sempre eseguite usando il campo sottostante.
Ciò potrebbe causare un'interruzione dell'applicazione se l'applicazione si basa su un comportamento aggiuntivo codificato nei metodi getter o setter.

**Perché?**

Questa modifica è stata apportata per impedire a EF Core di attivare per errore la logica di business per impostazione predefinita quando si eseguono operazioni di database che interessano le entità.

**Mitigazioni**

È possibile ripristinare il comportamento delle versioni precedenti alla versione 3.0 tramite la configurazione della modalità di accesso delle proprietà nell'API Fluent modelBuilder.
Ad esempio:

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a>Viene generata un'eccezione se vengono trovati più campi sottostanti compatibili

[Problema n. 12523](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.

**Comportamento precedente**

Nelle versioni precedenti a EF Core 3.0, se più campi soddisfacevano le regole di ricerca del campo sottostante di una proprietà, veniva selezionato un solo campo in base a un ordine di precedenza.
Ciò poteva causare l'uso di un campo non corretto nei casi ambigui.

**Nuovo comportamento**

A partire da EF Core 3.0, se più campi corrispondono alla stessa proprietà, viene generata un'eccezione.

**Perché?**

Questa modifica è stata apportata per evitare di usare automaticamente un campo rispetto a un altro quando un solo campo può essere quello corretto.

**Mitigazioni**

Per le proprietà con campi sottostanti ambigui, il campo da usare deve essere specificato in modo esplicito.
Ad esempio, con l'API Fluent:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a>AddDbContext/AddDbContextPool non chiamano più AddLogging e AddMemoryCache

[Problema n. 14756](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.

**Comportamento precedente**

Prima di EF Core 3.0, la chiamata di `AddDbContext` oppure `AddDbContextPool` comporta anche la registrazione dei servizi di registrazione e memorizzazione nella cache con inserimento delle dipendenze tramite chiamate a [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) e [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

**Nuovo comportamento**

A partire da EF Core 3.0, `AddDbContext` e `AddDbContextPool` non registreranno più questi servizi con inserimento delle dipendenze.

**Perché?**

EF Core 3.0 non richiede che questi servizi siano inclusi nel contenitore di inserimento delle dipendenze dell'applicazione. Tuttavia, se `ILoggerFactory` è registrato nel contenitore di inserimento delle dipendenze dell'applicazione, verrà ancora usato da EF Core.

**Mitigazioni**

Se l'applicazione necessita di questi servizi, registrarli in modo esplicito con il contenitore di inserimento delle dipendenze usando [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) o [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a>DbContext.Entry esegue ora un DetectChanges locale

[Problema n. 13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.

**Comportamento precedente**

Nelle versioni precedenti a EF Core 3.0 la chiamata a `DbContext.Entry` causava il rilevamento delle modifiche per tutte le entità rilevate.
Ciò garantiva l'aggiornamento dello stato esposto in `EntityEntry`.

**Nuovo comportamento**

A partire da EF Core 3.0, la chiamata a `DbContext.Entry` causa ora solo il tentativo di rilevare le modifiche nell'entità specificata e in tutte le relative entità di sicurezza rilevate.
Ciò significa che le modifiche apportate altrove potrebbero non essere state rilevate tramite la chiamata al metodo e ciò potrebbe avere implicazioni sullo stato dell'applicazione.

Si noti che se `ChangeTracker.AutoDetectChangesEnabled` è impostato su `false`, verrà disabilitato anche questo tipo di rilevamento delle modifiche locali.

Gli altri metodi che causano il rilevamento delle modifiche, ad esempio `ChangeTracker.Entries` e `SaveChanges`, causano ancora un `DetectChanges` completo di tutte le entità rilevate.

**Perché?**

Questa modifica è stata apportata per migliorare le prestazioni predefinite dell'uso di `context.Entry`.

**Mitigazioni**

Chiamare `ChgangeTracker.DetectChanges()` in modo esplicito prima di chiamare `Entry` per garantire il comportamento precedente alla versione 3.0.

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a>Le chiavi matrice di byte e di stringhe non vengono generate dal client per impostazione predefinita

[Problema n. 14617](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.

**Comportamento precedente**

Nelle versioni precedenti a EF Core 3.0 le proprietà di chiave `string` e `byte[]` potevano essere usate senza impostare in modo esplicito un valore non Null.
In questi casi, il valore di chiave veniva generato nel client come GUID, serializzato in byte per `byte[]`.

**Nuovo comportamento**

A partire da EF Core 3.0 viene generata un'eccezione che indica che non è stato impostato alcun valore di chiave.

**Perché?**

Questa modifica è stata apportata poiché i valori `string`/`byte[]` generati dal client non risultano in genere utili e il comportamento predefinito rendeva complesso ragionare sui valori di chiave generati in un modo comune.

**Mitigazioni**

È possibile ripristinare il comportamento precedente alla versione 3.0 specificando in modo esplicito che le proprietà di chiave devono usare i valori generati se non viene impostato alcun altro valore non Null.
Ad esempio, con l'API Fluent:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

Oppure con annotazioni dei dati:

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a>ILoggerFactory è ora un servizio con ambito

[Problema n. 14698](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.

**Comportamento precedente**

Nelle versioni precedenti a EF Core 3.0 `ILoggerFactory` veniva registrato come servizio singleton.

**Nuovo comportamento**

A partire da EF Core 3.0, `ILoggerFactory` viene registrato come servizio con ambito.

**Perché?**

Questa modifica è stata apportata per consentire l'associazione di un logger a un'istanza `DbContext` che abilita altre funzionalità e rimuove alcuni casi di comportamento anomalo, ad esempio un'esplosione dei provider di servizi interni.

**Mitigazioni**

Questa modifica non dovrebbe influire sul codice dell'applicazione a meno che non vengano registrati e usati servizi personalizzati nel provider di servizi interno di EF Core.
Questa condizione, tuttavia, non è comune.
In questi casi la maggior parte delle operazioni continuano a essere eseguite correttamente, ma qualsiasi servizio singleton dipendente da `ILoggerFactory` dovrà essere modificato per ottenere `ILoggerFactory` in modo diverso.

Se si verificano situazioni simili, inviare una segnalazione nello [strumento di gestione dei problemi in GitHub relativo a EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) per comunicare in che modo viene usato `ILoggerFactory` per consentirci di comprendere meglio come evitare ulteriori interruzioni in futuro.

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a>IDbContextOptionsExtensionWithDebugInfo unito in IDbContextOptionsExtension

[Problema n. 13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.

**Comportamento precedente**

`IDbContextOptionsExtensionWithDebugInfo` era un'interfaccia facoltativa aggiuntiva estesa da `IDbContextOptionsExtension` che consentiva di evitare le modifiche che causavano interruzioni nell'interfaccia durante il ciclo di versioni 2.x.

**Nuovo comportamento**

Le interfacce sono ora unite in `IDbContextOptionsExtension`.

**Perché?**

Questa modifica è stata apportata perché le interfacce sono concettualmente un'unica interfaccia.

**Mitigazioni**

Tutte le implementazioni di `IDbContextOptionsExtension` dovranno essere aggiornate per supportare il nuovo membro.

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a>I proxy di caricamento lazy non presuppongono più che le proprietà di navigazione vengano caricate completamente

[Problema n. 12780](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.

**Comportamento precedente**

Nelle versioni precedenti a EF Core 3.0, quando `DbContext` veniva eliminato non esisteva alcun metodo per scoprire se una determinata proprietà di navigazione in un'entità ottenuta da un contesto specifico veniva caricata completamente o meno.
I proxy presupponevano invece che venisse caricata una navigazione di riferimento se era presente un valore non Null e che venisse caricata una navigazione di raccolta se era presente un valore.
In questi casi il tentativo di eseguire un caricamento lazy non avrebbe avuto alcun esito.

**Nuovo comportamento**

A partire da Entity Framework Core 3.0, i proxy tengono traccia del caricamento o mancato caricamento di una proprietà di navigazione.
Ciò significa che il tentativo di accedere a una proprietà di navigazione caricata dopo l'eliminazione del contesto non avrà mai alcun esito, anche quando la navigazione caricata è vuota o ha valore Null.
Al contrario, il tentativo di accedere a una proprietà di navigazione non caricata genera un'eccezione se il contesto viene eliminato anche se la proprietà di navigazione è una raccolta non vuota.
Se si verifica questa situazione significa che il codice dell'applicazione sta tentando di usare il caricamento lazy in un momento non valido e l'applicazione deve essere modificata in modo da non eseguire questa operazione.

**Perché?**

Questa modifica è stata apportata per rendere coerente e corretto il comportamento durante un tentativo di caricamento lazy in un'istanza `DbContext` eliminata.

**Mitigazioni**

Aggiornare il codice dell'applicazione per fare in modo che non venga tentato il caricamento lazy con un contesto eliminato oppure specificare una configurazione in modo che non venga eseguita alcuna operazione come descritto nel messaggio di eccezione.

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a>La creazione di un numero eccessivo di provider di servizi interni è ora un errore per impostazione predefinita

[Problema n. 10236](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.

**Comportamento precedente**

Nelle versioni precedenti a EF Core 3.0 veniva registrato un avviso per le applicazioni che creavano un numero eccessivo di provider di servizi interni.

**Nuovo comportamento**

A partire da EF Core 3.0, l'avviso viene considerato un errore e viene generata un'eccezione. 

**Perché?**

Questa modifica è stata apportata per gestire meglio il codice dell'applicazione tramite un'esposizione più esplicita di questa situazione di errore.

**Mitigazioni**

L'azione più appropriata quando si verifica questo errore consiste nell'individuare la causa radice e nell'interrompere la creazione di numerosi provider di servizi interni.
È possibile tuttavia convertire nuovamente l'errore in avviso o ignorarlo tramite una configurazione in `DbContextOptionsBuilder`.
Ad esempio:

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a>Nuovo comportamento per la chiamata di HasOne/HasMany con una singola stringa

[Problema n. 9171](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

Questa modifica verrà introdotta in EF Core 3.0 anteprima 4.

**Comportamento precedente**

Prima di EF Core 3.0, il codice che chiama `HasOne` o `HasMany` con una singola stringa era interpretato in modo poco chiaro.
Ad esempio:
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

Apparentemente, il codice mette in relazione `Samuri` con un altro tipo di entità tramite la proprietà di navigazione `Entrance`, che può essere privata.

In realtà, il codice tenta di creare una relazione con un tipo di entità denominato `Entrance` senza proprietà di navigazione.

**Nuovo comportamento**

A partire da EF Core 3.0, il codice sopra riportato ora esegue quello che avrebbe dovuto fare in precedenza.

**Perché?**

Il comportamento precedente era molto poco chiaro, soprattutto durante la lettura del codice di configurazione e la ricerca di errori.

**Mitigazioni**

Questa modifica causerà problemi solo nelle applicazioni che configurano relazioni in modo esplicito usando stringhe per i nomi dei tipi e senza specificare in modo esplicito la proprietà di navigazione.
Non è uno scenario comune.
Il comportamento precedente può essere ottenuto passando esplicitamente `null` per il nome della proprietà di navigazione.
Ad esempio:

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a>L'annotazione Relational:TypeMapping è ora TypeMapping

[Problema n. 9913](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

Questa modifica è stata introdotta in EF Core 3.0 anteprima 2.

**Comportamento precedente**

Il nome di annotazione delle annotazioni di mapping del tipo era "Relational:TypeMapping".

**Nuovo comportamento**

Il nome di annotazione delle annotazioni di mapping del tipo è ora "TypeMapping".

**Perché?**

Il mapping dei tipi non viene più usato solo per i provider di database relazionali.

**Mitigazioni**

Ciò causa un'interruzione solo nelle applicazioni che accedono al mapping dei tipi direttamente come annotazione. Questa situazione non è comune.
L'azione più appropriata per risolvere il problema consiste nell'usare la superficie dell'API per accedere al mapping dei tipi anziché l'annotazione.

## <a name="totable-on-a-derived-type-throws-an-exception"></a>ToTable in un tipo derivato genera un'eccezione 

[Problema n. 11811](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.

**Comportamento precedente**

Nelle versioni precedenti a EF Core 3.0, la chiamata a `ToTable()` in un tipo derivato veniva ignorata poiché soltanto la strategia di mapping dell'ereditarietà era una tabella per gerarchia dove la chiamata non era valida. 

**Nuovo comportamento**

A partire da EF Core 3.0 e in preparazione all'aggiunta del supporto per la tabella per tipo e per TPC in una versione successiva, la chiamata a `ToTable()` in un tipo derivato genera un'eccezione per evitare una modifica del mapping imprevista in futuro.

**Perché?**

Attualmente il mapping di un tipo derivato in una tabella diversa non è un'operazione valida.
Questa modifica consente di evitare interruzioni future quando l'operazione diventerà un'operazione valida.

**Mitigazioni**

Rimuovere qualsiasi tentativo di mapping di tipi derivati in altre tabelle.

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a>ForSqlServerHasIndex sostituito con HasIndex 

[Problema n. 12366](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.

**Comportamento precedente**

Nelle versioni precedenti a EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` offriva un metodo per configurare le colonne usate con `INCLUDE`.

**Nuovo comportamento**

A partire da EF Core 3.0, l'uso di `Include` in un indice è supportato a livello relazionale.
Usare `HasIndex().ForSqlServerInclude()`.

**Perché?**

Questa modifica è stata apportata per consolidare l'API per gli indici con `Includes` in un'unica posizione per tutti i provider di database.

**Mitigazioni**

Usare la nuova API, come illustrato in precedenza.

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a>EF Core non invia più pragma per l'imposizione della chiave esterna di SQLite

[Problema n. 12151](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

Questa modifica è stata introdotta in EF Core 3.0 anteprima 3.

**Comportamento precedente**

Nelle versioni precedenti a EF Core 3.0, EF Core inviava `PRAGMA foreign_keys = 1` quando veniva aperta una connessione a SQLite.

**Nuovo comportamento**

A partire da EF Core 3.0, EF Core non invia più `PRAGMA foreign_keys = 1` quando viene aperta una connessione a SQLite.

**Perché?**

Questa modifica è stata apportata poiché EF Core usa `SQLitePCLRaw.bundle_e_sqlite3` per impostazione predefinita. Ciò significa che l'imposizione della chiave esterna è abilitata per impostazione predefinita e non deve essere abilitata in modo esplicito ogni volta che viene aperta una connessione.

**Mitigazioni**

Le chiavi esterne sono abilitate per impostazione predefinita in SQLitePCLRaw.bundle_e_sqlite3, usato per impostazione predefinita per EF Core.
Per gli altri casi, è possibile abilitare le chiavi esterne specificando `Foreign Keys=True` nella stringa di connessione.

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a>Microsoft.EntityFrameworkCore.Sqlite dipende ora da SQLitePCLRaw.bundle_e_sqlite3

**Comportamento precedente**

Nelle versioni precedenti a EF Core 3.0, EF Core usava `SQLitePCLRaw.bundle_green`.

**Nuovo comportamento**

A partire da EF Core 3.0, EF Core usa `SQLitePCLRaw.bundle_e_sqlite3`.

**Perché?**

Questa modifica è stata apportata per rendere coerente la versione di SQLite usata in iOS con le altre piattaforme.

**Mitigazioni**

Per usare la versione di SQLite nativa in iOS, configurare `Microsoft.Data.Sqlite` per l'uso di un'aggregazione `SQLitePCLRaw` diversa.
