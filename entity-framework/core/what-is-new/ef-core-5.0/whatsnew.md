---
title: Novità di EF Core 5.0
description: Panoramica delle nuove funzionalità di EF Core 5.0Overview of new features in EF Core 5.0
author: ajcvickers
ms.date: 03/30/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: c047a308cadf44eea577dcab29b68b36942a50df
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634272"
---
# <a name="whats-new-in-ef-core-50"></a>Novità di EF Core 5.0

EF Core 5.0 è attualmente in fase di sviluppo.
Questa pagina conterrà una panoramica delle modifiche interessanti introdotte in ogni anteprima.

Questa pagina non duplica il [piano per EF Core 5.0.](plan.md)
Il piano descrive i temi generali per EF Core 5.0, incluso tutto ciò che si prevede di includere prima della spedizione della versione finale.

Aggiungeremo link da qui alla documentazione ufficiale così come viene pubblicata.

## <a name="preview-2"></a>Preview 2

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a>Usare un attributo di C' per specificare un campo di sostegno delle proprietà

È ora possibile usare un attributo di C, per specificare il campo di backup per una proprietà.
Questo attributo consente a EF Core di scrivere e leggere dal campo di backup come si verificherebbe normalmente, anche quando non è possibile trovare automaticamente il campo di backup.
Ad esempio:

```CSharp
public class Blog
{
    private string _mainTitle;

    public int Id { get; set; }

    [BackingField(nameof(_mainTitle))]
    public string Title
    {
        get => _mainTitle;
        set => _mainTitle = value;
    }
}
```

La documentazione viene tracciata per problema [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).

### <a name="complete-discriminator-mapping"></a>Mappatura completa dei discriminatori

EF Core utilizza una colonna discriminatore per il [mapping TPH di una gerarchia di ereditarietà](/ef/core/modeling/inheritance).
Alcuni miglioramenti delle prestazioni sono possibili finché EF Core conosce tutti i valori possibili per il discriminatore.
EF Core 5.0 implementa ora questi miglioramenti.

Ad esempio, le versioni precedenti di Entity Framework Core genererebbe sempre questo codice SQL per una query che restituisce tutti i tipi in una gerarchia:For example, previous versions of EF Core would always generate this SQL for a query returning all types in a hierarchy:

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

EF Core 5.0 genererà ora quanto segue quando viene configurato un mapping completo del discriminatore:EF Core 5.0 will now generate the following when a complete discriminator mapping is configured:

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

Sarà il comportamento predefinito a partire dall'anteprima 3.

### <a name="performance-improvements-in-microsoftdatasqlite"></a>Miglioramenti delle prestazioni in Microsoft.Data.SqlitePerformance improvements in Microsoft.Data.Sqlite

Sono stati apportati due miglioramenti delle prestazioni per SQLIte:We have made two performance improvements for SQLIte:

* Il recupero di dati binari e di stringa con GetBytes, GetChars e GetTextReader è ora più efficiente utilizzando SqliteBlob e flussi.
* L'inizializzazione di SqliteConnection è ora lazy.

Questi miglioramenti sono nel ADO.NET Microsoft.Data.Sqlite provider e quindi migliorare anche le prestazioni al di fuori di Entity Framework Core.These improvements are in the ADO.NET Microsoft.Data.Sqlite provider and so it also improve performance outside of EF Core.

## <a name="preview-1"></a>Preview 1

### <a name="simple-logging"></a>Registrazione semplice

Questa funzionalità aggiunge `Database.Log` funzionalità simili a In EF6.
Vale a dire, fornisce un modo semplice per ottenere i log da EF Core senza la necessità di configurare qualsiasi tipo di framework di registrazione esterna.

La documentazione preliminare è inclusa nello [stato settimanale EF per il 5 dicembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

La documentazione aggiuntiva viene monitorata per problema [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).

### <a name="simple-way-to-get-generated-sql"></a>Modo semplice per ottenere codice SQL generato

EF Core 5.0 `ToQueryString` introduce il metodo di estensione, che restituirà il codice SQL generato da Entity Framework Core durante l'esecuzione di una query LINQ.

La documentazione preliminare è inclusa nello [stato settimanale EF per il 9 gennaio 2020.](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246)

La documentazione aggiuntiva viene monitorata per [#1331.](https://github.com/dotnet/EntityFramework.Docs/issues/1331)

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a>Usare un attributo di C' per indicare che un'entità non ha una chiaveUse a C' attribute to indicate that an entity has no key

Un tipo di entità può ora essere `KeylessAttribute`configurato come senza chiave utilizzando il nuovo .
Ad esempio:

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

La documentazione viene tracciata per [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>Connection or connection string can be changed on initialized DbContext

Ora è più semplice creare un'istanza DbContext senza alcuna stringa di connessione o di connessione.
Inoltre, la connessione o la stringa di connessione può ora essere attivata nell'istanza del contesto.
Questa funzionalità consente alla stessa istanza di contesto di connettersi dinamicamente a database diversi.

La documentazione viene tracciata per [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).

### <a name="change-tracking-proxies"></a>Proxy di rilevamento delle modifiche

EF Core può ora generare proxy di runtime che implementano automaticamente [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) e [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).
Questi quindi segnalare le modifiche al valore delle proprietà dell'entità direttamente in Entity Framework Core, evitando la necessità di eseguire la scansione per le modifiche.
Tuttavia, i proxy sono dotati di una propria serie di limitazioni, quindi non sono per tutti.

La documentazione viene tracciata per problema [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).

### <a name="enhanced-debug-views"></a>Visualizzazioni di debug migliorate

Le visualizzazioni di debug sono un modo semplice per esaminare le funzionalità interne di EF Core durante il debug dei problemi.
Una visualizzazione di debug per il modello è stata implementata qualche tempo fa.
Per EF Core 5.0, abbiamo reso la visualizzazione del modello più facile da leggere e aggiunto una nuova visualizzazione di debug per le entità rilevate nel gestore dello stato.

La documentazione preliminare è inclusa nello [stato settimanale EF per il 12 dicembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).

La documentazione aggiuntiva viene tenuta traccia in base al problema [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).

### <a name="improved-handling-of-database-null-semantics"></a>Gestione dei valori null del database migliorata

I database relazionali in genere considerano NULL come valore sconosciuto e pertanto non sono uguali a qualsiasi altro valore NULL.
Mentre il linguaggio C' considera null come un valore definito, che confronta uguale a qualsiasi altro valore null.
EF Core per impostazione predefinita converte le query in modo che utilizzino la semantica null di C.
EF Core 5.0 migliora notevolmente l'efficienza di queste traduzioni.

La documentazione viene tracciata dal problema [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).

### <a name="indexer-properties"></a>Proprietà dell'indicizzatore

EF Core 5.0 supporta il mapping delle proprietà dell'indicizzatore di C.
Queste proprietà consentono alle entità di fungere da contenitori di proprietà in cui le colonne sono mappate alle proprietà denominate nel contenitore.

La documentazione viene tracciata per problema [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).

### <a name="generation-of-check-constraints-for-enum-mappings"></a>Generazione di vincoli CHECK per i mapping di enumerazione

Le migrazioni di EF Core 5.0 possono ora generare vincoli CHECK per i mapping delle proprietà enum.
Ad esempio:

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

La documentazione viene tracciata per problema [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).

### <a name="isrelational"></a>IsRelational

È `IsRelational` stato aggiunto un nuovo metodo `IsSqlServer` `IsSqlite`oltre `IsInMemory`all'oggetto esistente , , e .
Questo metodo può essere utilizzato per verificare se il DbContext utilizza qualsiasi provider di database relazionale.
Ad esempio:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

La documentazione viene tracciata per problema [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).

### <a name="cosmos-optimistic-concurrency-with-etags"></a>Cosmos concorrenza ottimistica con ETags

Il provider di database cosmos di Azure supporta ora la concorrenza ottimistica tramite ETag.
Utilizzare il generatore di modelli in OnModelCreating per configurare un ETag:Use the model builder in OnModelCreating to configure an ETag:

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

SaveChanges genererà `DbUpdateConcurrencyException` quindi un conflitto di concorrenza, che [può essere gestito](https://docs.microsoft.com/ef/core/saving/concurrency) per implementare i tentativi e così via.

La documentazione viene tracciata per problema [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).

### <a name="query-translations-for-more-datetime-constructs"></a>Traduzioni di query per altri costrutti DateTimeQuery translations for more DateTime constructs

Le query contenenti nuova costruzione DateTime vengono ora convertite.

Inoltre, le seguenti funzioni di SQL Server sono ora mappate:

* DateDiffWeek
* DateFromParts

Ad esempio:

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

La documentazione viene tracciata per problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translations-for-more-byte-array-constructs"></a>Conversioni di query per altri costrutti di matrici di byteQuery translations for more byte array constructs

Le query che utilizzano le proprietà Contains, Length, SequenceEqual e così via su byte[] vengono ora convertite in SQL.

La documentazione preliminare è inclusa nello [stato settimanale EF per il 5 dicembre 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

La documentazione aggiuntiva viene monitorata per [problema #2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-reverse"></a>Query translation for Reverse

Le `Reverse` query che utilizzano vengono ora tradotte.
Ad esempio:

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

La documentazione viene tracciata per problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-bitwise-operators"></a>Conversione di query per operatori bit per bitQuery translation for bitwise operators

Le query che utilizzano operatori bit per bit vengono ora convertite in più casi Ad esempio:Query using bitwise operators are now translated in more cases For example:

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

La documentazione viene tracciata per problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-strings-on-cosmos"></a>Traduzione di query per stringhe su Cosmos

Le query che usano i metodi stringa Contains, StartsWith e EndsWith vengono ora convertite quando si usa il provider database Cosmos di Azure.Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.

La documentazione viene tracciata per problema [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).
