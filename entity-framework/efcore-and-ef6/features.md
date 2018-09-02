---
title: Confronto delle singole funzionalità disponibili in EF Core ed EF6
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f22f29ef-efc0-475d-b0b2-12a054f80f95
uid: efcore-and-ef6/features
ms.openlocfilehash: ed6ce51e7560e19e0d572f3d81cef7cbb310beec
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995335"
---
# <a name="ef-core-and-ef6-feature-by-feature-comparison"></a>Confronto delle singole funzionalità disponibili in EF Core ed EF6

La tabella seguente confronta le funzionalità disponibili in EF Core e in EF6. Si tratta di un confronto generale e non tutte le funzionalità sono elencate. Non vengono inoltre forniti dettagli sulle possibili differenze di funzionamento della stessa funzionalità.

La colonna EF Core contiene il numero della versione del prodotto in cui la funzionalità è apparsa per la prima volta.

| **Creazione di un modello**                                  | **EF 6** | **EF Core**                           |
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
|                                                       |          |                                       |
| **Esecuzione di query su dati**                                     | **EF6**  | **EF Core**                           |
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
|                                                       |          |                                       |
| **Salvataggio di dati**                                       | **EF6**  | **EF Core**                           |
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
|                                                       |          |                                       |
| **Altre funzionalità**                                    | **EF6**  | **EF Core**                           |
| Migrazioni                                            | Yes      | 1.0                                   |
| API di creazione/eliminazione del database                       | Yes      | 1.0                                   |
| Dati del valore di inizializzazione                                             | Yes      | 2.1                                   |
| Resilienza della connessione                                 | Yes      | 1.1                                   |
| Hook del ciclo di vita (eventi, intercettazione)                | Yes      |                                       |
| Registrazione semplice (Database.Log)                         | Yes      |                                       |
| Pooling DbContext                                     |          | 2.0                                   |
|                                                       |          |                                       |
| **Provider di database**                                | **EF6**  | **EF Core**                           |
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
|                                                       |          |                                       |
| **Piattaforme**                                         | **EF6**  | **EF Core**                           |
| .NET Framework (console, Windows Form, WPF, ASP.NET)      | Yes      | 1.0                                   |
| .NET Core (console, ASP.NET Core)                     |          | 1.0                                   |
| Mono e Xamarin                                        |          | 1.0 (in corso)                     |
| UWP                                                   |          | 1.0 (in corso)                     |

<sup>1</sup> È attualmente disponibile un provider a pagamento. È in fase di sviluppo un provider ufficiale gratuito per Oracle.
<sup>2</sup> Questo provider funziona solo in .NET Framework (non in .NET Core).
