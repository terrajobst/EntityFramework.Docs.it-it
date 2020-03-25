---
title: Novità di EF Core 5,0
author: ajcvickers
ms.date: 03/15/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 08a93555fd76d8a9f6d3011f59d9a34f76d0b22f
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136248"
---
# <a name="whats-new-in-ef-core-50"></a>Novità di EF Core 5,0

EF Core 5,0 è attualmente in fase di sviluppo.
Questa pagina conterrà una panoramica delle modifiche interessanti introdotte in ogni anteprima.

Questa pagina non duplica il [piano per EF Core 5,0](plan.md).
Il piano descrive i temi generali per EF Core 5,0, inclusi tutti gli elementi che si prevede di includere prima di distribuire la versione finale.

I collegamenti da qui vengono aggiunti alla documentazione ufficiale appena pubblicata.

## <a name="preview-1"></a>Preview 1

### <a name="simple-logging"></a>Registrazione semplice

Questa funzionalità aggiunge funzionalità simili a `Database.Log` in EF6.
Ovvero, fornisce un modo semplice per ottenere i log da EF Core senza la necessità di configurare alcun tipo di Framework di registrazione esterno.

La documentazione preliminare è inclusa nello [stato settimanale EF del 5 dicembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

La documentazione aggiuntiva viene rilevata in base al problema [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).

### <a name="simple-way-to-get-generated-sql"></a>Modo semplice per ottenere SQL generato

EF Core 5,0 introduce il metodo di estensione `ToQueryString` che restituirà il SQL che EF Core genererà durante l'esecuzione di una query LINQ.

La documentazione preliminare è inclusa nello [stato settimanale EF per il 9 gennaio 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).

La documentazione aggiuntiva viene rilevata in base al problema [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a>Usare un C# attributo per indicare che un'entità non ha una chiave

È ora possibile configurare un tipo di entità senza chiavi usando il nuovo `KeylessAttribute`.
Ad esempio,

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

La documentazione viene rilevata in base al problema [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>La stringa di connessione o di connessione può essere modificata in DbContext inizializzato

È ora più semplice creare un'istanza di DbContext senza connessione o stringa di connessione.
Inoltre, la connessione o la stringa di connessione possono ora essere mutate sull'istanza del contesto.
Ciò consente alla stessa istanza del contesto di connettersi dinamicamente a database diversi.

La documentazione viene rilevata in base al problema [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).

### <a name="change-tracking-proxies"></a>Proxy di rilevamento delle modifiche

EF Core ora possono generare proxy di runtime che implementano automaticamente [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) e [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).
Queste segnalano quindi le modifiche ai valori delle proprietà dell'entità direttamente a EF Core, evitando la necessità di analizzare le modifiche.
Tuttavia, i proxy presentano un set di limitazioni, quindi non sono destinati a tutti.

La documentazione viene rilevata in base al problema [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).

### <a name="enhanced-debug-views"></a>Viste di debug migliorate

Le visualizzazioni di debug rappresentano un modo semplice per esaminare gli elementi interni di EF Core durante il debug dei problemi.
Una visualizzazione di debug per il modello è stata implementata qualche tempo fa.
Per EF Core 5,0, la visualizzazione modello è stata semplificata per la lettura e l'aggiunta di una nuova visualizzazione debug per le entità rilevate nel gestore di stato.

La documentazione preliminare è inclusa nello [stato settimanale EF per il 12 dicembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).

La documentazione aggiuntiva viene rilevata in base al problema [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).

### <a name="improved-handling-of-database-null-semantics"></a>Gestione migliorata della semantica null del database

I database relazionali considerano in genere NULL come valore sconosciuto e pertanto non uguale a qualsiasi altro valore NULL.
C#d'altra parte, considera null come un valore definito che confronta uguale a qualsiasi altro valore null.
EF Core per impostazione predefinita converte le query in modo che utilizzino C# la semantica null.
EF Core 5,0 migliora notevolmente l'efficienza di queste traduzioni.

La documentazione viene rilevata in base al problema [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).

### <a name="indexer-properties"></a>Proprietà indicizzatore

EF Core 5,0 supporta il mapping C# delle proprietà dell'indicizzatore.
In questo modo le entità possono fungere da contenitori di proprietà in cui viene eseguito il mapping delle colonne alle proprietà denominate nel contenitore.

La documentazione viene rilevata in base al problema [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).

### <a name="generation-of-check-constraints-for-enum-mappings"></a>Generazione di vincoli check per i mapping enum

EF Core 5,0 le migrazioni possono ora generare vincoli CHECK per i mapping delle proprietà di enumerazione.
Ad esempio,

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

La documentazione viene rilevata in base al problema [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).

### <a name="isrelational"></a>Relazionale

È stato aggiunto un nuovo metodo di `IsRelational`, oltre ai `IsSqlServer`, `IsSqlite`e `IsInMemory`esistenti.
Questa operazione può essere utilizzata per verificare se DbContext utilizza un provider di database relazionale.
Ad esempio,

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

La documentazione viene rilevata in base al problema [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).

### <a name="cosmos-optimistic-concurrency-with-etags"></a>Concorrenza ottimistica di Cosmos con ETag

Il provider di database Azure Cosmos DB supporta ora la concorrenza ottimistica tramite ETag.
Usare il generatore di modelli in OnModelCreating per configurare un ETag:

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

SaveChanges genera quindi un `DbUpdateConcurrencyException` in un conflitto di concorrenza, che [può essere gestito](https://docs.microsoft.com/ef/core/saving/concurrency) per implementare i tentativi e così via.


La documentazione viene rilevata in base al problema [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).

### <a name="query-translations-for-more-datetime-constructs"></a>Eseguire query sulle traduzioni per più costrutti DateTime

Vengono ora convertite le query contenenti nuove costruzioni DateTime.

Inoltre, vengono ora mappate le funzioni di SQL Server seguenti:
* DateDiffWeek
* DateFromParts

Ad esempio,

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

La documentazione viene rilevata in base al problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translations-for-more-byte-array-constructs"></a>Eseguire query sulle traduzioni per altri costrutti di matrici di byte

Le query che usano le proprietà Contains, length, SequenceEqual e così via su byte [] vengono ora convertite in SQL.

La documentazione preliminare è inclusa nello [stato settimanale EF del 5 dicembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

La documentazione aggiuntiva viene rilevata in base al problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-reverse"></a>Conversione di query per invertire

Le query che usano `Reverse` ora vengono convertite.
Ad esempio,

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

La documentazione viene rilevata in base al problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-bitwise-operators"></a>Conversione di query per operatori bit per bit

Le query che utilizzano operatori bit per bit vengono ora convertite in più casi, ad esempio:

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

La documentazione viene rilevata in base al problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-strings-on-cosmos"></a>Conversione di query per le stringhe in Cosmos

Le query che usano i metodi String contengono, StartsWith e EndsWith vengono convertite quando si usa il provider di Azure Cosmos DB.

La documentazione viene rilevata in base al problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).
