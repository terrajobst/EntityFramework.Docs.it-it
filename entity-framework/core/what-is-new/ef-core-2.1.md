---
title: "Novità di EF Core 2.1 - EF Core"
author: divega
ms.author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: 1e5e9839bae1e5da4082d90c02d098bb3b2b43bd
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2018
---
# <a name="new-features-in-ef-core-21"></a>Nuove funzionalità di EF Core 2.1
> [!NOTE]  
> Questa versione è ancora in anteprima.

Oltre a numerosi piccoli miglioramenti e più di cento correzioni di bug del prodotto, EF Core 2.1 include diverse nuove funzionalità:

## <a name="lazy-loading"></a>Caricamento lazy
EF Core contiene ora i blocchi predefiniti necessari per consentire a tutti di creare classi di entità che possono caricare le proprietà di navigazione su richiesta. È stato anche creato un nuovo pacchetto, Microsoft.EntityFrameworkCore.Proxies, che sfrutta questi blocchi predefiniti per produrre classi di proxy con caricamento lazy in base a classi di entità con modifiche minime, ad esempio classi con le proprietà di navigazione virtuale.

Leggere la [sezione sul caricamento lazy](xref:core/querying/related-data#lazy-loading) per altre informazioni su questo argomento.

## <a name="parameters-in-entity-constructors"></a>Parametri nei costruttori di entità
In quanto uno dei blocchi predefiniti richiesti per il caricamento lazy, è stata abilitata la creazione di entità che accettano parametri nei propri costruttori. È possibile usare i parametri per inserire i valori delle proprietà, i delegati del caricamento lazy e i servizi.

Leggere la [sezione sul costruttore di entità con parametri](xref:core/modeling/constructors) per altre informazioni su questo argomento.

## <a name="value-conversions"></a>Conversioni di valori
Fino ad ora, EF Core poteva solo eseguire il mapping di proprietà con tipi supportati in modo nativo dal provider di database sottostante. I valori venivano copiati tra le colonne e le proprietà senza alcuna trasformazione. A partire da EF Core 2.1, si possono applicare conversioni dei valori per trasformare i valori ottenuti dalle colonne prima che vengano applicate alle proprietà e viceversa. Sono disponibili un certo numero di conversioni che possono essere applicate per convenzione in base alle esigenze, nonché un'API di configurazione esplicita che consente la registrazione di conversioni personalizzate tra colonne e le proprietà. Alcune applicazioni di questa funzionalità sono le seguenti:

- Archiviazione delle enumerazioni come stringhe
- Mapping di integer senza segno con SQL Server
- Crittografia e decrittografia automatiche dei valori delle proprietà

Leggere la [sezione sulle conversioni dei valori](xref:core/modeling/value-conversions) per altre informazioni su questo argomento.  

## <a name="linq-groupby-translation"></a>Conversione di GroupBy LINQ
Prima della versione 2.1, in EF Core l'operatore LINQ GroupBy veniva sempre valutato in memoria. È ora supportata la conversione nella clausola SQL GROUP BY nella maggior parte dei casi.

Questo esempio mostra una query con GroupBy usata per calcolare varie funzioni di aggregazione:

``` csharp
var query = context.Orders
    .GroupBy(o => new { o.CustomerId, o.EmployeeId })
    .Select(g => new
        {
          g.Key.CustomerId,
          g.Key.EmployeeId,
          Sum = g.Sum(o => o.Amount),
          Min = g.Min(o => o.Amount),
          Max = g.Max(o => o.Amount),
          Avg = g.Average(o => Amount)
        });
```

La conversione SQL corrispondente è simile alla seguente:

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a>Seeding dei dati
Con la nuova versione sarà possibile fornire i dati iniziali per popolare un database. Diversamente da EF6, il seeding dei dati è associato a un tipo di entità come parte della configurazione del modello. Le migrazioni di EF Core possono calcolare automaticamente quali operazioni di inserimento, aggiornamento o eliminazione devono essere applicate durante l'aggiornamento del database a una nuova versione del modello.

Ad esempio, è possibile usare questo codice per configurare i dati iniziali per un'operazione Post in `OnModelCreating`:

``` csharp
modelBuilder.Entity<Post>().SeedData(new Post{ Id = 1, Text = "Hello World!" });
```

Leggere la [sezione sul seeding dei dati](xref:core/modeling/data-seeding) per altre informazioni su questo argomento.  

## <a name="query-types"></a>Tipi di query
Un modello EF Core può ora includere tipi di query. Diversamente dai tipi di entità, per i tipi di query non vengono definite chiavi e questi tipi non possono essere inseriti, eliminati o aggiornati (ovvero sono di sola lettura), ma possono essere restituiti direttamente dalle query. Alcuni degli scenari di utilizzo per i tipi di query sono:

- Mapping a viste senza chiavi primarie
- Mapping a tabelle senza chiavi primarie
- Mapping a query definite nel modello
- Utilizzo come tipo restituito per le query `FromSql()`

Leggere la [sezione sui tipi di query](xref:core/modeling/query-types) per altre informazioni su questo argomento.

## <a name="include-for-derived-types"></a>Include per i tipi derivati
Sarà ora possibile specificare le proprietà di navigazione definite solo su tipi derivati durante la scrittura di espressioni per il metodo `Include`. Per la versione fortemente tipizzata di `Include`, è supportato l'uso di un cast esplicito o dell'operatore `as`. Sono ora supportati anche i riferimenti ai nomi di proprietà di navigazione definite su tipi derivati nella versione stringa di `Include`:

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

Leggere la [sezione su Include con tipi derivati](xref:core/querying/related-data#include-on-derived-types) per altre informazioni su questo argomento.

## <a name="systemtransactions-support"></a>Supporto di System.Transactions
È stata aggiunta la possibilità di utilizzare le funzionalità di System.Transactions, ad esempio TransactionScope. Questa funzionalità sarà disponibile sia per .NET Framework che per .NET Core quando si usano provider di database che la supportano.

Leggere la [sezione su System.Transactions](xref:core/saving/transactions#using-systemtransactions) per altre informazioni su questo argomento.

## <a name="better-column-ordering-in-initial-migration"></a>Migliore ordinamento delle colonne nella migrazione iniziale
In base ai suggerimenti dei clienti, sono state aggiornate le migrazioni per generare inizialmente le colonne per le tabelle nello stesso ordine con cui sono dichiarate le proprietà nelle classi. Si noti che EF Core non può modificare l'ordine quando vengono aggiunti nuovi membri dopo la creazione iniziale delle tabelle.

## <a name="optimization-of-correlated-subqueries"></a>Ottimizzazione di sottoquery correlate
È stata migliorata la conversione di query per evitare l'esecuzione di "N + 1" query SQL in molti scenari comuni in cui l'utilizzo di una proprietà di navigazione nella proiezione comporta il join dei dati dalla query radice con i dati da una sottoquery correlata. L'ottimizzazione richiede la memorizzazione nel buffer dei risultati dalla sottoquery ed è necessario modificare la query per acconsentire esplicitamente al nuovo comportamento.

Ad esempio, la query seguente viene in genere convertita in un'unica query per Customers, oltre a N (dove "N" è il numero di clienti restituito) query separate per Orders:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

Includendo `ToList()` nella posizione corretta, si indica che la memorizzazione nel buffer è appropriata per Orders e viene quindi abilitata l'ottimizzazione:

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

Si noti che questa query verrà convertita in solo due query SQL: una per Customers e la successiva per Orders.

### <a name="ownedattribute"></a>OwnedAttribute

È ora possibile configurare [tipi di entità di proprietà](xref:core/modeling/owned-entities) aggiungendo semplicemente l'annotazione `[Owned]` al tipo e assicurandosi quindi che l'entità proprietario venga aggiunta al modello:

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="database-provider-compatibility"></a>Compatibilità dei provider di database

EF Core 2.1 è stato progettato per essere compatibile con i provider di database creati per EF Core 2.0. Anche se alcune delle funzionalità descritte in precedenza (ad esempio, le conversioni di valori) richiedono un provider aggiornato, altre, come il caricamento lazy, saranno disponibili anche con i provider esistenti.

> [!TIP]
> Se si rilevano incompatibilità impreviste o qualsiasi problema per le nuove funzionalità o se si desidera inviare commenti o suggerimenti, usare lo [strumento per la registrazione dei problemi](https://github.com/aspnet/EntityFrameworkCore/issues/new).
