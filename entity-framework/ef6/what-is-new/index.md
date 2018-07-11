---
title: Novità - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
caps.latest.revision: 3
ms.openlocfilehash: 0da2ce778a765037ecacd0726cbb7cda08b5683f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911705"
---
# <a name="whats-new-in-ef6"></a>Novità di EF6

Per ottenere le funzionalità più recenti e il livello di stabilità più elevato è consigliabile usare l'ultima versione rilasciata di Entity Framework.
È possibile, tuttavia, usare una versione precedente o provare i miglioramenti resi disponibili nella versione provvisoria più recente.
Per installare versioni specifiche di Entity Framework, vedere [Get Entity Framework](~/ef6/fundamentals/install.md) (Ottenere Entity Framework).

Questa pagina documenta le funzionalità incluse in ogni nuova versione.

## <a name="recent-releases"></a>Versioni recenti

### <a name="ef-tools-update-in-visual-studio-2017-157"></a>Aggiornamento di Entity Framework Tools in Visual Studio 2017 15.7

Nel mese di maggio 2018 è stata rilasciata una versione aggiornata di EF6 Tools in Visual Studio 2017 15.7.
Include miglioramenti in alcune aree problematiche comuni:

- Una revisione dell'accessibilità dell'interfaccia utente
- Soluzione alternativa per la regressione delle prestazioni di SQL Server nella decompilazione [n. 4](https://github.com/aspnet/entityframework6/issues/4)
- Supporto dell'aggiornamento del modello dal database per modelli di dimensioni superiori in SQL Server [n. 185](https://github.com/aspnet/EntityFramework6/issues/185)

Questa nuova versione di EF Tools installa il runtime di EF 6.2 durante la creazione di un modello in un nuovo progetto. Con le versioni precedenti di Visual Studio è possibile usare il runtime di EF 6.2 (o altre versioni precedenti di EF) installando la versione corrispondente del pacchetto NuGet.

### <a name="ef-62-runtime"></a>Runtime di EF 6.2

Il runtime di EF 6.2 è stato rilasciato in NuGet nel mese di ottobre 2017.
Grazie principalmente al lavoro svolto dalla community di contributori open source, EF 6.2 include numerose [correzioni di bug](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) e [miglioramenti del prodotto](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).

Di seguito viene riportato un breve elenco delle modifiche più importanti che interessano il runtime di EF 6.2:

- Riduzione del tempo di avvio grazie al caricamento di modelli Code First finiti da una cache persistente [n. 275](https://github.com/aspnet/EntityFramework6/issues/275)
- API Fluent per definire gli indici [n. 274](https://github.com/aspnet/EntityFramework6/issues/274)
- DbFunctions.Like() per abilitare la scrittura di query LINQ che vengono convertite in LIKE in SQL [n. 241](https://github.com/aspnet/EntityFramework6/issues/241)
- Migrate.exe ora supporta l'opzione -script [n. 240](https://github.com/aspnet/EntityFramework6/issues/240)
- EF6 consente ora di usare i valori chiave generati da una sequenza in SQL Server [n. 165](https://github.com/aspnet/EntityFramework6/issues/165)
- Aggiornamento dell'elenco di errori temporanei per la strategia di esecuzione SQL Azure [n. 83](https://github.com/aspnet/EntityFramework6/issues/83)
- Bug: i nuovi tentativi di query o comandi SQL restituiscono l'errore "SqlParameter già contenuto in un altro elemento SqlParameterCollection" [n. 81](https://github.com/aspnet/EntityFramework6/issues/81)
- Bug: frequente timeout della valutazione di DbQuery.ToString() nel debugger [n. 73](https://github.com/aspnet/EntityFramework6/issues/73)

## <a name="future-releases"></a>Versioni future

Per informazioni sulla versione futura di EF6, vedere la [Roadmap](roadmap.md).

## <a name="past-releases"></a>Versioni precedenti

La pagina [Versioni precedenti](past-releases.md) contiene un archivio di tutte le versioni precedenti di Entity Framework e delle funzionalità principali introdotte in ogni versione. 
