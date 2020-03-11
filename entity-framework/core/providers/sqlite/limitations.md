---
title: Provider di database SQLite-limitazioni-EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 2f80dc195265787318ac4925dd937da45ffad011
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78417774"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Limitazioni del provider di database EF Core SQLite

Il provider SQLite presenta alcune limitazioni relative alle migrazioni. La maggior parte di queste limitazioni è il risultato di limitazioni nel motore di database SQLite sottostante e non sono specifiche di EF.

## <a name="modeling-limitations"></a>Limitazioni di modellazione

La libreria relazionale comune (condivisa da Entity Framework provider di database relazionali) definisce le API per i concetti di modellazione comuni alla maggior parte dei motori di database relazionali. Alcuni di questi concetti non sono supportati dal provider SQLite.

* Schemi
* Sequenze
* Colonne calcolate

## <a name="query-limitations"></a>Limitazioni delle query

SQLite non supporta in modo nativo i tipi di dati seguenti. EF Core possibile leggere e scrivere i valori di questi tipi ed è supportata anche l'esecuzione di query per verificarne l'uguaglianza (`where e.Property == value`). Altre operazioni, tuttavia, come il confronto e l'ordinamento richiederanno una valutazione sul client.

* DateTimeOffset
* Decimal
* TimeSpan
* UInt64

Invece di `DateTimeOffset`, è consigliabile usare i valori DateTime. Quando si gestiscono più fusi orari, è consigliabile convertire i valori in formato UTC prima di salvare e quindi tornare al fuso orario appropriato.

Il tipo di `Decimal` fornisce un livello elevato di precisione. Se questo livello di precisione non è necessario, tuttavia, è consigliabile usare invece Double. È possibile usare un [convertitore di valori](../../modeling/value-conversions.md) per continuare a usare Decimal nelle classi.

``` csharp
modelBuilder.Entity<MyEntity>()
    .Property(e => e.DecimalProperty)
    .HasConversion<double>();
```

## <a name="migrations-limitations"></a>Limitazioni delle migrazioni

Il motore di database SQLite non supporta una serie di operazioni dello schema supportate dalla maggior parte degli altri database relazionali. Se si tenta di applicare una delle operazioni non supportate a un database SQLite, verrà generata un'`NotSupportedException`.

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
| RenameColumn         | ✔          | 2.2.2            |
| RenameIndex          | ✔          | 2.1              |
| RenameTable          | ✔          | 1.0              |
| EnsureSchema         | ✔ (no-op)  | 2.0              |
| DropSchema           | ✔ (no-op)  | 2.0              |
| Inserimento               | ✔          | 2.0              |
| Aggiornamento               | ✔          | 2.0              |
| Delete               | ✔          | 2.0              |

## <a name="migrations-limitations-workaround"></a>Soluzione alternativa alle migrazioni

È possibile aggirare alcune di queste limitazioni scrivendo manualmente il codice nelle migrazioni per eseguire una ricompilazione della tabella. Una ricompilazione della tabella comporta la ridenominazione della tabella esistente, la creazione di una nuova tabella, la copia di dati nella nuova tabella e l'eliminazione della tabella precedente. Per eseguire alcuni di questi passaggi, sarà necessario usare il metodo `Sql(string)`.

Per altri dettagli, vedere la documentazione di SQLite per [altri tipi di modifiche dello schema della tabella](https://sqlite.org/lang_altertable.html#otheralter) .

In futuro, EF può supportare alcune di queste operazioni usando l'approccio di ricompilazione della tabella dietro le quinte. È possibile [tenere traccia di questa funzionalità nel progetto GitHub](https://github.com/aspnet/EntityFrameworkCore/issues/329).
