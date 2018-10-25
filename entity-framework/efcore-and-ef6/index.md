---
title: Confrontare Entity Framework 6 ed Entity Framework Core
description: Offre istruzioni per poter scegliere tra Entity Framework 6 ed Entity Framework Core.
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 0f9f0d4708fa283855eddf2cfc231b37356e413e
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022350"
---
# <a name="compare-ef-core--ef6"></a>Confronto tra EF Core e EF6

Entity Framework è un Object-Relational Mapper (ORM) per .NET. Questo articolo mette a confronto le due versioni: Entity Framework 6 ed Entity Framework Core.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6) è una tecnologia di accesso ai dati testata. Il primo rilascio risale al 2008, come parte di .NET Framework 3.5 SP1 e Visual Studio 2008 SP1. A partire dalla versione 4.1 viene inclusa come pacchetto NuGet [EntityFramework](https://www.nuget.org/packages/EntityFramework/). La tecnologia EF6 viene eseguita in .NET Framework 4.x, vale a dire solo in Windows. 

EF6 continuerà a essere un prodotto supportato, per il quale saranno generati miglioramenti secondari e correzioni di bug.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core) è una riscrittura completa di EF6, rilasciato per la prima volta nel 2016. Viene offerto in pacchetti Nuget, il principale è [Microsoft.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/). EF Core è un prodotto multipiattaforma che può essere eseguito in .NET Core o .NET Framework.

EF Core è stato progettato per offrire un'esperienza di sviluppo simile a EF6. Molte delle principali API sono invariate, in modo che EF Core risulti familiare agli sviluppatori che hanno usato EF6.

## <a name="feature-comparison"></a>Confronto tra funzionalità

EF Core offre nuove funzionalità che non verranno implementate in EF6 (ad esempio [chiavi alternative](xref:core/modeling/alternate-keys), [aggiornamenti batch](xref:core/what-is-new/ef-core-1.0#relational-batching-of-statements) e [valutazione mista client/database nelle query LINQ](xref:core/querying/client-eval). Trattandosi di una nuova base di codice, mancano anche alcune funzionalità presenti in EF6.

Le tabelle seguenti confrontano le funzionalità disponibili in EF Core e in EF6. Si tratta di un confronto generale in cui non vengono elencate tutte le funzionalità o illustrate le differenze tra la stessa funzionalità nelle diverse versioni di Entity Framework.

La colonna EF Core contiene la versione del prodotto in cui la funzionalità è apparsa per la prima volta.

### <a name="creating-a-model"></a>Creazione di un modello

| **Funzionalità**                                           | **EF 6** | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Mapping delle classi di base                                   | Yes      | 1.0                                   |
| Costruttori con parametri                          |          | 2.1                                   |
| Conversioni di valori di proprietà                            |          | 2.1                                   |
| Tipi mappati senza chiavi (tipi di query)               |          | 2.1                                   |
| Convenzioni                                           | Yes      | 1.0                                   |
| Convenzioni personalizzate                                    | Yes      | 1.0 (parziale)                         |
| Annotazioni dei dati                                      | Yes      | 1.0                                   |
| API Fluent                                            | Yes      | 1.0                                   |
| Ereditarietà: tabella per gerarchia                | Yes      | 1.0                                   |
| Ereditarietà: tabella per tipo                     | Yes      |                                       |
| Ereditarietà: tabella per classe concreta           | Yes      |                                       |
| Proprietà con stato shadow                               |          | 1.0                                   |
| Chiavi alternative                                        |          | 1.0                                   |
| Molti-a-molti senza entità join                      | Yes      |                                       |
| Generazione di chiavi: database                              | Yes      | 1.0                                   |
| Generazione di chiavi: client                                |          | 1.0                                   |
| Tipi complessi/di proprietà                                   | Yes      | 2.0                                   |
| Dati spaziali                                          | Yes      |                                       |
| Visualizzazione grafica del modello                      | Yes      |                                       |
| Editor di modello grafico                                | Yes      |                                       |
| Formato del modello: codice                                    | Yes      | 1.0                                   |
| Formato del modello: EDMX (XML)                              | Yes      |                                       |
| Creazione del modello dal database: riga di comando              | Yes      | 1.0                                   |
| Creazione del modello dal database: procedura guidata di VS                 | Yes      |                                       |
| Aggiornamento del modello dal database                            | Partial  |                                       |
| Filtri di query globali                                  |          | 2.0                                   |
| Suddivisione di tabelle                                       | Yes      | 2.0                                   |
| Suddivisione di entità                                      | Yes      |                                       |
| Mapping di funzione scalare del database                      | Poor     | 2.0                                   |
| Mapping campi                                         |          | 1.1                                   |

### <a name="querying-data"></a>Esecuzione di query su dati

| **Funzionalità**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Query LINQ                                          | Yes      | 1.0 (in corso per le query complesse) |
| Query SQL generate come leggibili                                | Poor     | 1.0                                   |
| Valutazione client/server mista                        |          | 1.0                                   |
| Conversione di GroupBy                                   | Yes      | 2.1                                   |
| Caricamento dei dati correlati: eager                           | Yes      | 1.0                                   |
| Caricamento dei dati correlati: caricamento eager per i tipi derivati |          | 2.1                                   |
| Caricamento dei dati correlati: lazy                            | Yes      | 2.1                                   |
| Caricamento dei dati correlati: esplicito                        | Yes      | 1.1                                   |
| Query SQL non elaborate: tipi di entità                         | Yes      | 1.0                                   |
| Query SQL non elaborate: tipi non di entità (ad esempio, tipi di query)       | Yes      | 2.1                                   |
| Query SQL non elaborate: composizione con LINQ                  |          | 1.0                                   |
| Query compilate in modo esplicito                           | Poor     | 2.0                                   |
| Linguaggio di query basato su testo (Entity SQL)                | Yes      |                                       |

### <a name="saving-data"></a>Salvataggio di dati

| **Funzionalità**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Rilevamento modifiche: snapshot                             | Yes      | 1.0                                   |
| Rilevamento modifiche: notifica                         | Yes      | 1.0                                   |
| Rilevamento delle modifiche: proxy                              | Yes      |                                       |
| Accesso allo stato registrato                               | Yes      | 1.0                                   |
| Concorrenza ottimistica                                | Yes      | 1.0                                   |
| Transazioni                                          | Yes      | 1.0                                   |
| Invio in batch di istruzioni                                |          | 1.0                                   |
| Mapping di stored procedure                              | Yes      |                                       |
| API di basso livello grafo disconnesso                     | Poor     | 1.0                                   |
| End-to-end grafo disconnesso                         |          | 1.0 (parziale)                         |

### <a name="other-features"></a>Altre funzionalità

| **Funzionalità**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Migrazioni                                            | Yes      | 1.0                                   |
| API di creazione/eliminazione del database                       | Yes      | 1.0                                   |
| Dati del valore di inizializzazione                                             | Yes      | 2.1                                   |
| Resilienza della connessione                                 | Yes      | 1.1                                   |
| Hook del ciclo di vita (eventi, intercettazione)                | Yes      |                                       |
| Registrazione semplice (Database.Log)                         | Yes      |                                       |
| Pooling DbContext                                     |          | 2.0                                   |

### <a name="database-providers"></a>Provider di database

| **Funzionalità**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| SQL Server                                            | Yes      | 1.0                                   |
| MySQL                                                 | Yes      | 1.0                                   |
| PostgreSQL                                            | Yes      | 1.0                                   |
| Oracle                                                | Yes      | 1.0 <sup>(1)</sup>                    |
| SQLite                                                | Yes      | 1.0                                   |
| SQL Server Compact                                    | Yes      | 1.0 <sup>(2)</sup>                    |
| DB2                                                   | Yes      | 1.0                                   |
| Firebird                                              | Yes      | 2.0                                   |
| Jet (Microsoft Access)                                |          | 2.0 <sup>(2)</sup>                    |
| In memoria (per il test)                               |          | 1.0                                   |

<sup>1</sup> È attualmente disponibile un provider a pagamento per Oracle. È in fase di sviluppo un provider ufficiale gratuito per Oracle.

<sup>2</sup> I provider SQL Server Compact e Jet funzionano solo in .NET Framework (non in .NET Core).

### <a name="net-implementations"></a>Implementazioni di .NET

| **Funzionalità**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| .NET Framework (console, Windows Form, WPF, ASP.NET)      | Yes      | 1.0                                   |
| .NET Core (console, ASP.NET Core)                     |          | 1.0                                   |
| Mono e Xamarin                                        |          | 1.0 (in corso)                     |
| UWP                                                   |          | 1.0 (in corso)                     |

## <a name="guidance-for-new-applications"></a>Linee guida per le nuove applicazioni

È consigliabile usare EF Core per una nuova applicazione se vengono soddisfatte entrambe le condizioni seguenti:
* Per l'app sono necessarie le funzionalità di .NET Core. Per altre informazioni, vedere [Scelta di .NET Core o .NET Framework per le app server](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).
* EF Core supporta tutte le funzionalità necessarie per l'app. Se manca una funzionalità necessaria, controllare la [roadmap per EF Core](xref:core/what-is-new/roadmap) per scoprire se è previsto il supporto per tale funzionalità in futuro. 

È consigliabile usare EF6 se vengono soddisfatte entrambe le condizioni seguenti:
* L'app verrà eseguita in Windows e in.NET Framework 4.0 o versione successiva.
* EF6 supporta tutte le funzionalità necessarie per l'app.

## <a name="guidance-for-existing-ef6-applications"></a>Linee guida per le applicazioni EF6 esistenti

A seguito delle modifiche sostanziali in EF Core, non è consigliabile spostare un'applicazione da EF6 a EF Core se non in presenza di un valido motivo per apportare la modifica. Se si vuole passare a EF Core per usare le nuove funzionalità, assicurarsi di essere a conoscenza delle relative limitazioni. Per altre informazioni, vedere [Conversione da EF6 a EF Core](porting/index.md). **Il passaggio da EF6 a EF Core è per lo più una conversione anziché un aggiornamento.** 

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere la documentazione:
* <xref:core/index>
* <xref:ef6/index>
