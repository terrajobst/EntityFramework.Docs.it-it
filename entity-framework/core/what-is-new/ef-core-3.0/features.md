---
title: Nuove funzionalità di EF Core 3.0 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 7501a806271c9734e85e31845f260f2d512da077
ms.sourcegitcommit: a8b04050033c5dc46c076b7e21b017749e0967a8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/02/2019
ms.locfileid: "58867957"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a>Nuove funzionalità incluse in EF Core 3.0 (attualmente in anteprima)

> [!IMPORTANT]
> Si tenga presente che i set di funzionalità e le pianificazioni delle versioni future sono sempre soggette a modifiche e che questa pagina, nonostante l'impegno profuso per mantenerla aggiornata, potrebbe non riflettere sempre i piani più recenti.

Nell'elenco seguente sono riportate le principali nuove funzionalità pianificate per EF Core 3.0.
La maggior parte delle funzionalità non è inclusa nell'anteprima corrente, ma sarà disponibile non appena si faranno passi avanti rispetto a RTM.

Il motivo è che all'inizio del rilascio ci si concentra sull'implementazione di [modifiche pianificate che causano un'interruzione](xref:core/what-is-new/ef-core-3.0/breaking-changes).
Molte di queste modifiche sono miglioramenti apportati a EF Core in modo indipendente.
Per sbloccare ulteriori miglioramenti sono necessarie molte altre modifiche. 

Per un elenco completo dei miglioramenti e delle correzioni di bug in corso, vedere [questa query nello strumento di gestione dei problemi](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).

## <a name="linq-improvements"></a>Miglioramenti di LINQ 

[Problema n. 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

Il lavoro su questa funzionalità è iniziato ma non è incluso nell'anteprima corrente.

LINQ consente di scrivere query di database senza uscire dal linguaggio preferito, sfruttando i vantaggi delle informazioni complete sui tipi per ottenere IntelliSense e il controllo dei tipi in fase di compilazione.
LINQ consente però di scrivere anche un numero illimitato di query complesse e ciò ha sempre rappresentato una grande sfida per i provider LINQ.
Nelle prime versioni di EF Core, questa complicazione è stata risolta in parte, cercando di individuare quali parti di una query possono essere convertite in SQL e consentendo quindi l'esecuzione del resto della query in memoria nel client.
L'esecuzione sul lato client può essere appropriata in alcuni casi, ma in molti altri casi può causare query inefficienti che potrebbero non essere identificate fino a quando un'applicazione viene distribuita nell'ambiente di produzione.
In EF Core 3.0 sono previste modifiche sostanziali del funzionamento dell'implementazione di LINQ e delle procedure per testarla.
Gli obiettivi sono: ottenere una maggiore solidità, ad esempio per evitare di compromettere il funzionamento delle query con il rilascio di patch, riuscire a convertire più espressioni correttamente in SQL, generare query efficienti in un maggior numero di casi ed evitare che query inefficienti non vengano rilevate.

## <a name="cosmos-db-support"></a>Supporto di Cosmos DB 

[Problema n. 8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

Questa funzionalità è inclusa nell'anteprima corrente, ma non è ancora completa. 

è in corso lo sviluppo di un provider Cosmos DB per EF Core, per consentire agli sviluppatori che hanno familiarità con il modello di programmazione di EF di selezionare facilmente Azure Cosmos DB come destinazione per il database dell'applicazione.
L'obiettivo è quello di rendere alcuni dei vantaggi di Cosmos DB, ad esempio la distribuzione globale, la disponibilità "AlwaysOn", la scalabilità elastica e la bassa latenza, ancora più accessibili per gli sviluppatori .NET.
Il provider abiliterà la maggior parte delle funzionalità di EF Core, come il rilevamento delle modifiche automatico, LINQ e le conversioni dei valori, con l'API SQL in Cosmos DB.
Questo progetto è stato avviato prima di EF Core 2.2 e [sono state pubblicate alcune versioni di anteprima del provider](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
Il nuovo piano prevede di continuare a sviluppare il provider insieme a EF Core 3.0. 

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Le entità dipendenti che condividono la tabella con l'entità di sicurezza sono ora facoltative

[Problema n. 9005](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

Questa funzionalità verrà introdotta in EF Core 3.0 anteprima 4.

Si consideri il modello seguente:
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

A partire da EF Core 3.0, se `OrderDetails` è di proprietà di `Order` o mappato in modo esplicito alla stessa tabella, sarà possibile aggiungere un `Order` senza `OrderDetails` e tutte le proprietà di `OrderDetails`, ad eccezione della chiave primaria, verranno mappate a colonne che ammettono i valori Null.
In fase di query, EF Core imposterà `OrderDetails` su `null` se una delle relative proprietà obbligatorie non ha un valore o se non sono presenti proprietà obbligatorie oltre alla chiave primaria e tutte le proprietà sono `null`.

## <a name="c-80-support"></a>Supporto di C# 8.0

[Problema n. 12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Problema n. 10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)

Il lavoro su questa funzionalità è iniziato ma non è incluso nell'anteprima corrente.

Lo scopo è consentire ai clienti Microsoft di sfruttare alcune delle [nuove funzionalità previste per C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/), ad esempio i flussi asincroni (tra cui `await foreach`) e i tipi riferimento nullable durante l'uso di EF Core.

## <a name="reverse-engineering-of-database-views"></a>Decompilazione delle viste di database

[Problema n. 1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

Questa funzionalità è inclusa nell'anteprima corrente.

I [tipi di query](xref:core/modeling/query-types), introdotti in EF Core 2.1 e considerati tipi di entità senza chiavi in EF Core 3.0, rappresentano i dati che possono essere letti dal database, ma non aggiornati.
Questa caratteristica li rende la scelta ideale per le viste di database nella maggior parte degli scenari, quindi si prevede di automatizzare la creazione dei tipi di entità senza chiavi durante la decompilazione delle viste di database.

## <a name="property-bag-entities"></a>Entità elenco proprietà

[Problemi n. 13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) e [9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)

Il lavoro su questa funzionalità è iniziato ma non è incluso nell'anteprima corrente. 

questa funzionalità riguarda l'abilitazione di entità che archiviano dati in proprietà indicizzate anziché in proprietà regolari, nonché la possibilità di usare istanze della stessa classe .NET (in modo potenzialmente semplice come `Dictionary<string, object>`) per rappresentare tipi di entità diversi nello stesso modello di EF Core.
Questa funzionalità è un trampolino di lancio per il supporto delle relazioni molti-a-molti senza un'entità di join ([problema n. 1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)), ovvero uno dei miglioramenti più richiesti per EF Core.

## <a name="ef-63-on-net-core"></a>EF 6.3 in .NET Core

[Problema EF6 n. 271](https://github.com/aspnet/EntityFramework6/issues/271)

Il lavoro su questa funzionalità è iniziato ma non è incluso nell'anteprima corrente. 

siamo consapevoli che molte applicazioni esistenti usano versioni precedenti di EF e che la loro conversione per EF Core al solo scopo di sfruttare i vantaggi di .NET Core può talvolta richiedere notevoli sforzi.
Per questo motivo, la prossima versione di EF 6 verrà adattata per supportare l'esecuzione in .NET Core 3.0.
Lo scopo è quello di facilitare la conversione delle applicazioni esistenti con modifiche minime.
Sono previste alcune limitazioni. Ad esempio:
- Saranno necessari nuovi provider per lavorare con altri database oltre al supporto per SQL Server incluso in .NET Core
- Il supporto spaziale con SQL Server non sarà attivato

Si noti anche che in questa fase non sono previste nuove funzionalità per EF 6.
