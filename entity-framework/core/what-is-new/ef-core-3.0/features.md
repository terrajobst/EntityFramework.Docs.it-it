---
title: Nuove funzionalità di EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: d938f17daecd5031147951d0018602c5635de41d
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149099"
---
# <a name="new-features-included-in-ef-core-30"></a>Nuove funzionalità incluse in EF Core 3,0

Nell'elenco seguente sono riportate le principali nuove funzionalità pianificate per EF Core 3.0.

EF Core 3,0 è una versione principale e contiene anche numerose [modifiche di rilievo](xref:core/what-is-new/ef-core-3.0/breaking-changes), ovvero miglioramenti dell'API che possono avere un impatto negativo sulle applicazioni esistenti.  

## <a name="linq-improvements"></a>Miglioramenti di LINQ 

LINQ consente di scrivere query di database senza uscire dal linguaggio scelto, sfruttando i vantaggi delle informazioni dettagliate sui tipi per offrire IntelliSense e il controllo dei tipi in fase di compilazione.
LINQ consente inoltre di scrivere un numero illimitato di query complesse contenenti espressioni arbitrarie (chiamate al metodo o operazioni).
La gestione di tutte queste combinazioni è sempre stata una sfida significativa per i provider LINQ.
In EF Core 3,0, l'implementazione di LINQ è stata riscritta per consentire la traduzione di più espressioni in SQL, per generare query efficienti in più casi, per evitare che le query inefficienti non vengano rilevate e per semplificare l'introduzione graduale di una nuova query funzionalità e prestazioni improvementswithout le applicazioni e i provider di dati esistenti.

### <a name="client-evaluation"></a>Valutazione client

La modifica della progettazione principale in EF Core 3,0 ha a che fare con il modo in cui gestisce le espressioni LINQ che non è in grado di convertire in SQL o nei parametri:

Nelle prime versioni EF Core semplicemente capito quali parti di una query possono essere convertite in SQL ed eseguito il resto della query sul client.
Questo tipo di esecuzione lato client può essere auspicabile in alcune situazioni, ma in molti altri casi può causare query inefficienti.
Se ad esempio EF Core 2,2 non è stato in grado di convertire `Where()` un predicato in una chiamata, è stata eseguita un'istruzione SQL senza un filtro, vengono lette tutte le righe dal database e quindi filtrate in memoria.
Questo può essere accettabile se il database contiene un numero ridotto di righe, ma può causare problemi di prestazioni significativi o persino un errore dell'applicazione se il database contiene un numero elevato di righe.
In EF Core 3,0 è stata limitata la valutazione client solo nella proiezione di primo livello (l'ultima chiamata a `Select()`).
Quando EF Core 3,0 rileva espressioni che non possono essere convertite altrove nella query, viene generata un'eccezione in fase di esecuzione.

## <a name="cosmos-db-support"></a>Supporto di Cosmos DB 

Il provider di Cosmos DB per EF Core consente agli sviluppatori che hanno familiarità con il modello di programmazione EF di utilizzare facilmente Azure Cosmos DB come database dell'applicazione.
L'obiettivo è quello di rendere alcuni dei vantaggi di Cosmos DB, ad esempio la distribuzione globale, la disponibilità "AlwaysOn", la scalabilità elastica e la bassa latenza, ancora più accessibili per gli sviluppatori .NET.
Il provider abiliterà la maggior parte delle funzionalità di EF Core, come il rilevamento delle modifiche automatico, LINQ e le conversioni dei valori, con l'API SQL in Cosmos DB.

## <a name="c-80-support"></a>Supporto di C# 8.0

EF Core 3,0 sfrutta alcune delle nuove funzionalità di C# 8,0:

### <a name="asynchronous-streams"></a>Flussi asincroni

I risultati della query asincrona vengono ora esposti usando la `IAsyncEnumerable<T>` nuova interfaccia standard e possono essere `await foreach`usati usando.

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Process(o);
} 
```

### <a name="nullable-reference-types"></a>Tipi riferimento nullable 

Quando questa nuova funzionalità è abilitata nel codice, EF Core possibile determinare il supporto di valori Null delle proprietà dei tipi refrence (tipi primitivi come le proprietà di stringa o di navigazione) per stabilire il supporto di valori null per le colonne e le relazioni nel database.

## <a name="interception"></a>Intercettazione

La nuova API di intercettazione in EF Core 3,0 consente a a livello di osservare e modificare il risultato delle operazioni di database di basso livello che si verificano nell'ambito della normale operazione di EF Core, ad esempio l'apertura di connessioni, le transazioni initating e l'esecuzione di comandi. 

## <a name="reverse-engineering-of-database-views"></a>Decompilazione delle viste di database

I tipi di entità senza chiavi (noti in precedenza come [tipi di query](xref:core/modeling/keyless-entity-types)) rappresentano i dati che possono essere letti dal database, ma non possono essere aggiornati.
Questa caratteristica li rende un ottimo adattamento per il mapping delle viste di database nella maggior parte degli scenari, quindi è stata automatizzata la creazione di tipi di entità senza chiavi quando reverse engineering viste di database.

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Le entità dipendenti che condividono la tabella con l'entità di sicurezza sono ora facoltative

A partire da EF Core 3.0, se `OrderDetails` è di proprietà di `Order` o mappato in modo esplicito alla stessa tabella, sarà possibile aggiungere un `Order` senza `OrderDetails` e tutte le proprietà di `OrderDetails`, a eccezione della chiave primaria, verranno mappate a colonne che ammettono i valori Null.

In fase di query, EF Core imposterà `OrderDetails` su `null` se una delle relative proprietà obbligatorie non ha un valore o se non sono presenti proprietà obbligatorie oltre alla chiave primaria e tutte le proprietà sono `null`.

``` csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

## <a name="ef-63-on-net-core"></a>EF 6.3 in .NET Core

siamo consapevoli che molte applicazioni esistenti usano versioni precedenti di EF e che la loro conversione per EF Core al solo scopo di sfruttare i vantaggi di .NET Core può talvolta richiedere notevoli sforzi.
Per questo motivo, è stata abilitata la versione newewst di EF 6 per l'esecuzione in .NET Core 3,0.
Esistono alcune limitazioni, ad esempio:
- Per lavorare in .NET Core sono necessari nuovi provider
- Il supporto spaziale con SQL Server non sarà attivato

## <a name="postponed-features"></a>Funzionalità posticipate

Alcune funzionalità pianificate originariamente per EF Core 3,0 sono state rimandate alle versioni future: 

- Possibilità di ingore delle parti di un modello nelle migrazioni, tracciate come [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).
- Entità contenitore delle proprietà, registrate come due problemi distinti: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) sulle entità di tipo condiviso e [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) sul supporto del mapping delle proprietà indicizzate.
