---
title: Database SQLite - limitazioni - Provider EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 53262bc926d79f42c4418a62717a462564dc80bf
ms.sourcegitcommit: 6c4e06bc62d98442530e93a44725e38e59483d42
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/18/2019
ms.locfileid: "58131411"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Limitazioni del Provider SQLite EF Core Database

Il provider SQLite ha alcune limitazioni delle migrazioni. La maggior parte di queste limitazioni sono il risultato delle limitazioni nel motore di database SQLite sottostante e non sono specifica di Entity Framework.

## <a name="modeling-limitations"></a>Limitazioni di modellazione

La libreria relazionale comune (condivisa dai provider di database relazionale di Entity Framework) definisce le API per la modellazione di concetti che sono comuni alla maggior parte dei motori di database relazionale. Un paio di questi concetti non sono supportati dal provider di SQLite.

* Schemi
* Sequenze

## <a name="migrations-limitations"></a>Limitazioni delle migrazioni

Il motore di database SQLite non supporta un numero di operazioni dello schema supportati per la maggior parte degli altri database relazionali. Se si tenta di applicare una delle operazioni non supportate per un database SQLite un `NotSupportedException` verrà generata.

| Operazione            | È supportata? | Richiede la versione |
|:---------------------|:-----------|:-----------------|
| AddColumn            | ✔          | 1.0              |
| AddForeignKey        | ✗          |                  |
| AddPrimaryKey        | ✗          |                  |
| AddUniqueConstraint  | ✗          |                  |
| AlterColumn          | ✗          |                  |
| CreateIndex          | ✔          | 1.0              |
| CreateTable          | ✔          | 1.0              |
| DropColumn           | ✗          |                  |
| DropForeignKey       | ✗          |                  |
| DropIndex            | ✔          | 1.0              |
| DropPrimaryKey       | ✗          |                  |
| DropTable            | ✔          | 1.0              |
| DropUniqueConstraint | ✗          |                  |
| RenameColumn         | ✔          | 2.2.2            |
| RenameIndex          | ✔          | 2.1              |
| RenameTable          | ✔          | 1.0              |
| EnsureSchema         | ✔ (no-op)  | 2.0              |
| DropSchema           | ✔ (no-op)  | 2.0              |
| INS               | ✔          | 2.0              |
| Aggiorna               | ✔          | 2.0              |
| Eliminare               | ✔          | 2.0              |

## <a name="migrations-limitations-workaround"></a>Soluzione alternativa di limitazioni di migrazioni

È possibile risolvere alcune di queste limitazioni da scrivere manualmente il codice per le migrazioni per eseguire una tabella ricompilazione. Una ricompilazione della tabella comporta la ridenominazione della tabella esistente, la creazione di una nuova tabella, la copia di dati nella nuova tabella e l'eliminazione della tabella precedente. È necessario usare il `Sql(string)` metodo per eseguire alcuni di questi passaggi.

Visualizzare [rendendo altri tipi di tabella di modifiche dello Schema](http://sqlite.org/lang_altertable.html#otheralter) nella documentazione di SQLite per altri dettagli.

In futuro, EF potrebbe supportare alcune di queste operazioni usando l'approccio di ricompilazione tabella dietro le quinte. È possibile [tenere traccia di questa funzionalità nel nostro progetto GitHub](https://github.com/aspnet/EntityFrameworkCore/issues/329).
