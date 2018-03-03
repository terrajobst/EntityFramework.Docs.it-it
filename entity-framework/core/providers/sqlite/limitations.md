---
title: Database SQLite - limitazioni - Provider EF Core
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 8a60ccfc61a5757df8ebedf257379d4d3dbffbf6
ms.sourcegitcommit: 60b831318c4f5ec99061e8af6a7c9e7c03b3469c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/02/2018
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Limitazioni del Provider di SQLite EF Core Database

Il provider di SQLite è un numero di limitazioni di migrazioni. La maggior parte di queste limitazioni sono il risultato delle limitazioni nel motore di database SQLite sottostante e non sono specifica di Entity Framework.

## <a name="modeling-limitations"></a>Limitazioni di modellazione

Libreria comune relazionale (condivisa dai provider di database relazionale di Entity Framework) definisce le API per la modellizzazione concetti che sono comuni alla maggior parte dei motori di database relazionale. Due di questi concetti non sono supportati dal provider di SQLite.

* Schemi
* Sequenze

## <a name="migrations-limitations"></a>Limitazioni delle migrazioni

Il motore di database SQLite non supporta un numero di operazioni sullo schema supportate dalla maggioranza degli altri database relazionali. Se si tenta di applicare una delle operazioni non supportate in un database SQLite un `NotSupportedException` verrà generata.

| Operazione            | Supportato? | Richiede la versione |
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
| RenameColumn         | ✗          |                  |
| RenameIndex          | ✔          | 2.1              |
| RenameTable          | ✔          | 1.0              |
| EnsureSchema         | ✔ (nessuna operazione)  | 2.0              |
| DropSchema           | ✔ (nessuna operazione)  | 2.0              |
| INS               | ✔          | 2.0              |
| Aggiorna               | ✔          | 2.0              |
| Eliminare               | ✔          | 2.0              |

## <a name="migrations-limitations-workaround"></a>Soluzione alternativa limitazioni migrazioni

È possibile risolvere alcune di queste limitazioni da scrivere manualmente il codice per le migrazioni per eseguire una tabella ricompilare. Una ricompilazione della tabella comporta la ridenominazione della tabella esistente, creando una nuova tabella, la copia dei dati nella nuova tabella ed eliminazione della tabella precedente. È necessario utilizzare il `Sql(string)` metodo per eseguire alcune di queste operazioni.

Vedere [apportare altri tipi di tabella le modifiche dello Schema](http://sqlite.org/lang_altertable.html#otheralter) nella documentazione di SQLite per altri dettagli.

In futuro, EF possono supportare alcune di queste operazioni tramite l'approccio di ricompilazione tabella dietro le quinte. È possibile [tenere traccia di questa funzionalità nel progetto GitHub](https://github.com/aspnet/EntityFrameworkCore/issues/329).
