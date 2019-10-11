---
title: Confrontare Entity Framework 6 ed Entity Framework Core - EF
description: Offre istruzioni per poter scegliere tra Entity Framework 6 ed Entity Framework Core.
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 9fe4905de5bd81fce083d620724b7fad4c6dd11b
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182033"
---
# <a name="compare-ef-core--ef6"></a>Confronto tra EF Core e EF6

Entity Framework è un Object-Relational Mapper (ORM) per .NET. In questo articolo vengono confrontate le due versioni: Entity Framework 6 ed Entity Framework Core.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6) è una tecnologia di accesso ai dati testata. Il primo rilascio risale al 2008, come parte di .NET Framework 3.5 SP1 e Visual Studio 2008 SP1. A partire dalla versione 4.1 viene inclusa come pacchetto NuGet [EntityFramework](https://www.nuget.org/packages/EntityFramework/). EF6 viene eseguito in .NET Framework 4.x e .NET Core dalla versione 3.0 in poi.

EF6 continuerà a essere un prodotto supportato, per il quale saranno generati miglioramenti secondari e correzioni di bug.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core) è una riscrittura completa di EF6, rilasciato per la prima volta nel 2016. Viene offerto in pacchetti Nuget, il principale è [Microsoft.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/). EF Core è un prodotto multipiattaforma eseguito in .NET Core.

EF Core è stato progettato per offrire un'esperienza di sviluppo simile a EF6. Molte delle principali API sono invariate, in modo che EF Core risulti familiare agli sviluppatori che hanno usato EF6.

## <a name="feature-comparison"></a>Confronto tra funzionalità

EF Core offre nuove funzionalità che non verranno implementate in EF6 (ad esempio [chiavi alternative](xref:core/modeling/alternate-keys), [aggiornamenti batch](xref:core/what-is-new/ef-core-1.0#relational-batching-of-statements) e [valutazione mista client/database nelle query LINQ](xref:core/querying/client-eval). Trattandosi di una nuova base di codice, mancano anche alcune funzionalità presenti in EF6.

Le tabelle seguenti confrontano le funzionalità disponibili in EF Core e in EF6. Si tratta di un confronto generale in cui non vengono elencate tutte le funzionalità o illustrate le differenze tra la stessa funzionalità nelle diverse versioni di Entity Framework.

La colonna EF Core contiene la versione del prodotto in cui la funzionalità è apparsa per la prima volta.

### <a name="creating-a-model"></a>Creazione di un modello

| **Funzionalità**                                           | **EF 6** | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Mapping delle classi di base                                   | Sì      | 1.0                                   |
| Costruttori con parametri                          |          | 2.1                                   |
| Conversioni di valori di proprietà                            |          | 2.1                                   |
| Tipi con mapping senza chiavi                             |          | 2.1                                   |
| Convenzioni                                           | Sì      | 1.0                                   |
| Convenzioni personalizzate                                    | Sì      | 1.0 (parziale)                         |
| Annotazioni dei dati                                      | Sì      | 1.0                                   |
| API Fluent                                            | Sì      | 1.0                                   |
| Ereditarietà: tabella per gerarchia                | Sì      | 1.0                                   |
| Ereditarietà: tabella per tipo                     | Sì      |                                       |
| Ereditarietà: tabella per classe concreta           | Sì      |                                       |
| Proprietà con stato shadow                               |          | 1.0                                   |
| Chiavi alternative                                        |          | 1.0                                   |
| Molti-a-molti senza entità join                      | Sì      |                                       |
| Generazione delle chiavi: Database                              | Sì      | 1.0                                   |
| Generazione delle chiavi: Client                                |          | 1.0                                   |
| Tipi complessi/di proprietà                                   | Sì      | 2.0                                   |
| Dati spaziali                                          | Sì      | 2.2                                   |
| Visualizzazione grafica del modello                      | Sì      |                                       |
| Editor di modello grafico                                | Sì      |                                       |
| Formato del modello: Codice                                    | Sì      | 1.0                                   |
| Formato del modello: EDMX (XML)                              | Sì      |                                       |
| Creazione del modello dal database: Riga di comando              | Sì      | 1.0                                   |
| Creazione del modello dal database: procedura guidata di Visual Studio                 | Sì      |                                       |
| Aggiornamento del modello dal database                            | Partial  |                                       |
| Filtri di query globali                                  |          | 2.0                                   |
| Suddivisione di tabelle                                       | Sì      | 2.0                                   |
| Suddivisione di entità                                      | Sì      |                                       |
| Mapping di funzione scalare del database                      | Poor     | 2.0                                   |
| Mapping campi                                         |          | 1.1                                   |
| Tipi riferimento nullable (C# 8.0)                     |          | 3.0                                   |

### <a name="querying-data"></a>Esecuzione di query su dati

| **Funzionalità**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Query LINQ                                          | Sì      | 1.0 (in corso per le query complesse) |
| Query SQL generate come leggibili                                | Poor     | 1.0                                   |
| Conversione di GroupBy                                   | Sì      | 2.1                                   |
| Caricamento dei dati correlati: eager                           | Sì      | 1.0                                   |
| Caricamento dei dati correlati: caricamento eager per i tipi derivati |          | 2.1                                   |
| Caricamento dei dati correlati: Differita                            | Sì      | 2.1                                   |
| Caricamento dei dati correlati: Explicit                        | Sì      | 1.1                                   |
| Query SQL non elaborate: Tipi di entità                         | Sì      | 1.0                                   |
| Query SQL non elaborate: tipi di entità senza chiave                 | Sì      | 2.1                                   |
| Query SQL non elaborate: Composizione con LINQ                  |          | 1.0                                   |
| Query compilate in modo esplicito                           | Poor     | 2.0                                   |
| Linguaggio di query basato su testo (Entity SQL)                | Sì      |                                       |
| await foreach (C# 8.0)                                |          | 3.0                                   |

### <a name="saving-data"></a>Salvataggio di dati

| **Funzionalità**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Rilevamento modifiche: Snapshot                             | Sì      | 1.0                                   |
| Rilevamento modifiche: Notifica                         | Sì      | 1.0                                   |
| Rilevamento modifiche: Proxy                              | Sì      |                                       |
| Accesso allo stato registrato                               | Sì      | 1.0                                   |
| Concorrenza ottimistica                                | Sì      | 1.0                                   |
| Transazioni                                          | Sì      | 1.0                                   |
| Invio in batch di istruzioni                                |          | 1.0                                   |
| Mapping di stored procedure                              | Sì      |                                       |
| API di basso livello grafo disconnesso                     | Poor     | 1.0                                   |
| End-to-end grafo disconnesso                         |          | 1.0 (parziale)                         |

### <a name="other-features"></a>Altre funzionalità

| **Funzionalità**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Migrazioni                                            | Sì      | 1.0                                   |
| API di creazione/eliminazione del database                       | Sì      | 1.0                                   |
| Dati del valore di inizializzazione                                             | Sì      | 2.1                                   |
| Resilienza della connessione                                 | Sì      | 1.1                                   |
| Hook del ciclo di vita (eventi, intercettazione)                | Sì      |                                       |
| Registrazione semplice (Database.Log)                         | Sì      |                                       |
| Pooling DbContext                                     |          | 2.0                                   |

### <a name="database-providers"></a>Provider di database

| **Funzionalità**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| SQL Server                                            | Sì      | 1.0                                   |
| MySQL                                                 | Sì      | 1.0                                   |
| PostgreSQL                                            | Sì      | 1.0                                   |
| Oracle                                                | Sì      | 1.0                                   |
| SQLite                                                | Sì      | 1.0                                   |
| SQL Server Compact                                    | Sì      | 1.0 <sup>(1)</sup>                    |
| DB2                                                   | Sì      | 1.0                                   |
| Firebird                                              | Sì      | 2.0                                   |
| Jet (Microsoft Access)                                |          | 2.0 <sup>(1)</sup>                    |
| Cosmos DB                                             |          | 3.0                                   |
| In memoria (per il test)                               |          | 1.0                                   |

<sup>1</sup> I provider SQL Server Compact e Jet funzionano solo in .NET Framework (non in .NET Core).

### <a name="net-implementations"></a>Implementazioni di .NET

| **Funzionalità**                                           | **EF6**            | **EF Core**                           |
|:------------------------------------------------------|:-------------------|:--------------------------------------|
| .NET Framework                                        | Sì                | 1.0 (rimosso nella versione 3.0)                  |
| .NET Core                                             | Sì (aggiunto nella versione 6.3) | 1.0                                   |
| Mono e Xamarin                                        |                    | 1.0 (in corso)                     |
| UWP                                                   |                    | 1.0 (in corso)                     |

## <a name="guidance-for-new-applications"></a>Linee guida per le nuove applicazioni

È consigliabile usare EF Core per una nuova applicazione se vengono soddisfatte entrambe le condizioni seguenti:
* Per l'app sono necessarie le funzionalità di .NET Core. Per altre informazioni, vedere [Scelta di .NET Core o .NET Framework per le app server](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).
* EF Core supporta tutte le funzionalità necessarie per l'app. Se manca una funzionalità necessaria, controllare la [roadmap per EF Core](xref:core/what-is-new/index) per scoprire se è previsto il supporto per tale funzionalità in futuro. 

È consigliabile usare EF6 se vengono soddisfatte entrambe le condizioni seguenti:
* L'app verrà eseguita in Windows e in.NET Framework 4.0 o versione successiva.
* EF6 supporta tutte le funzionalità necessarie per l'app.

## <a name="guidance-for-existing-ef6-applications"></a>Linee guida per le applicazioni EF6 esistenti

A seguito delle modifiche sostanziali in EF Core, non è consigliabile spostare un'applicazione da EF6 a EF Core se non in presenza di un valido motivo per apportare la modifica. Se si vuole passare a EF Core per usare le nuove funzionalità, assicurarsi di essere a conoscenza delle relative limitazioni. Per altre informazioni, vedere [Conversione da EF6 a EF Core](porting/index.md). **Il passaggio da EF6 a EF Core è per lo più una conversione anziché un aggiornamento.**

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni, vedere la documentazione:
* <xref:core/index>
* <xref:ef6/index>
