---
title: "Confronto delle singole funzionalità disponibili in EF Core ed EF6"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f22f29ef-efc0-475d-b0b2-12a054f80f95
uid: efcore-and-ef6/features
ms.openlocfilehash: 696ff2c8ec788c08880ecb3b07e10dc081b0323b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/27/2017
---
# <a name="ef-core-and-ef6-feature-by-feature-comparison"></a>Confronto delle singole funzionalità disponibili in EF Core ed EF6

La tabella seguente confronta le funzionalità disponibili in EF Core e in EF6. Si tratta di un confronto generale e non tutte le funzionalità sono elencate. Non vengono inoltre forniti dettagli sulle possibili differenze di funzionamento della stessa funzionalità.

La colonna EF Core contiene il numero della versione del prodotto in cui la funzionalità è apparsa per la prima volta.

| **Creazione di un modello** |**EF 6** |**EF Core** |
|-|-|-|
| Mapping delle classi di base                         | Sì | 1.0 |
| Convenzioni                                 | Sì | 1.0 |
| Convenzioni personalizzate                          | Sì | 1.0 (parziale) |
| Annotazioni dei dati                            | Sì | 1.0 |
| API Fluent                                  | Sì | 1.0 |
| Ereditarietà: tabella per gerarchia      | Sì | 1.0 |
| Ereditarietà: tabella per tipo           | Sì |     |
| Ereditarietà: tabella per classe concreta | Sì |     |
| Proprietà con stato shadow                     |     | 1.0 |
| Chiavi alternative                              |     | 1.0 |
| Molti-a-molti senza entità join            | Sì |     |
| Generazione di chiavi: database                    | Sì | 1.0 |
| Generazione di chiavi: client                      |     | 1.0 |
| Tipi complessi/di proprietà                         | Sì | 2.0 |
| Dati spaziali                                | Sì |     |
| Visualizzazione grafica del modello            | Sì |     |
| Editor di modello grafico                      | Sì |     |
| Formato del modello: codice                          | Sì | 1.0 |
| Formato del modello: EDMX (XML)                    | Sì |     |
| Creazione del modello dal database: riga di comando    | Sì | 1.0 |
| Creazione del modello dal database: procedura guidata di VS       | Sì |     |
| Aggiornamento del modello dal database                  | Partial | |
| Filtri di query globali                        |     | 2.0 |
| Suddivisione di tabelle                             | Sì | 2.0 |
| Suddivisione di entità                            | Sì |     |
| Mapping di funzione scalare del database            | Scarse | 2.0 |
| Mapping campi                               |     | 1.1 |
| | | |
| **Esecuzione di query su dati** |**EF6** |**EF Core** |
| Query LINQ                                | Sì | 1.0 (in corso per le query complesse) |
| Query SQL generate come leggibili                      | Scarse | 1.0 |
| Valutazione client/server mista              |     | 1.0 |
| Caricamento dei dati correlati: eager                 | Sì | 1.0 |
| Caricamento dei dati correlati: lazy                  | Sì |     |
| Caricamento dei dati correlati: esplicito              | Sì | 1.1 |
| Query SQL non elaborate: tipi modello                | Sì | 1.0 |
| Query SQL non elaborate: tipi non modello            | Sì |     |
| Query SQL non elaborate: composizione con LINQ        |     | 1.0 |
| Query compilate esplicite                 | Scarse | 2.0 |
| | | |
| **Salvataggio di dati** |**EF6** |**EF Core** |
| Rilevamento modifiche: snapshot                   | Sì | 1.0 |
| Rilevamento modifiche: notifica               | Sì | 1.0 |
| Accesso allo stato registrato                     | Sì | 1.0 |
| Concorrenza ottimistica                      | Sì | 1.0 |
| Transazioni                                | Sì | 1.0 |
| Invio in batch di istruzioni                      |     | 1.0 |
| Stored procedure                            | Sì |     |
| API di basso livello grafo disconnesso           | Scarse | 1.0 |
| End-to-end grafo disconnesso               |     | 1.0 (parziale) |
| | | |
| **Altre funzionalità** |**EF6** |**EF Core** |
| Migrazioni                                  | Sì | 1.0 |
| API di creazione/eliminazione del database             | Sì | 1.0 |
| Dati del valore di inizializzazione                                   | Sì |     |
| Resilienza della connessione                       | Sì | 1.1 |
| Hook del ciclo di vita (eventi, intercettazione)      | Sì |     |
| Pooling DbContext                           |     | 2.0 |
| | | |
| **Provider di database** |**EF6**|**EF Core** |
| SQL Server                                  | Sì | 1.0 |
| MySQL                                       | Sì | 1.0 |
| PostgreSQL                                  | Sì | 1.0 |
| Oracle                                      | Sì | 1.0 (solo a pagamento<sup>(1)</sup>) |
| SQLite                                      | Sì | 1.0 |
| SQL Compact                                 | Sì | 1.0 <sup>(2)</sup> |
| DB2                                         | Sì |     |
| In memoria (per il test)                      |     | 1.0 |
| | | |
| **Piattaforme** |**EF6** |**EF Core** |
| .NET Framework (console, Windows Form, WPF, ASP.NET) | Sì | 1.0 |
| .NET Core (console, ASP.NET Core)           |     | 1.0 |
| Mono e Xamarin                              |     | 1.0 (in corso) |
| UWP                                         |     | 1.0 (in corso) |

<sup>1</sup> È in fase di elaborazione un provider ufficiale per Oracle.
<sup>2</sup> Il provider SQL Server Compact funziona solo in .NET Framework (non in .NET Core).
