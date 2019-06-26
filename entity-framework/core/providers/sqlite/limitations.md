---
title: Database SQLite - limitazioni - Provider EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: eaa7d5b1496172e4f3821433a1cd098ee7e8b737
ms.sourcegitcommit: 9bd64a1a71b7f7aeb044aeecc7c4785b57db1ec9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394798"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Limitazioni del Provider SQLite EF Core Database

Il provider SQLite ha alcune limitazioni delle migrazioni. La maggior parte di queste limitazioni sono il risultato delle limitazioni nel motore di database SQLite sottostante e non sono specifica di Entity Framework.

## <a name="modeling-limitations"></a>Limitazioni di modellazione

La libreria relazionale comune (condivisa dai provider di database relazionale di Entity Framework) definisce le API per la modellazione di concetti che sono comuni alla maggior parte dei motori di database relazionale. Un paio di questi concetti non sono supportati dal provider di SQLite.

* Schemi
* Sequenze
* Colonne calcolate

## <a name="query-limitations"></a>Limitazioni delle query

SQLite in modo nativo non supporta i seguenti tipi di dati. EF Core può leggere e scrivere i valori di questi tipi e l'esecuzione di query per verificare l'uguaglianza (`where e.Property == value`) è inoltre il supporto. Altre operazioni, tuttavia, come il confronto e ordinamento richiederanno la valutazione sul client.

* DateTimeOffset
* Decimale
* TimeSpan
* UInt64

Invece di `DateTimeOffset`, è consigliabile usare i valori DateTime. Quando si gestiscono più fusi orari, è consigliabile convertire i valori in formato UTC prima di salvare e quindi riconvertendo nuovamente al fuso orario appropriato.

Il `Decimal` tipo fornisce un livello elevato di precisione. Se non è necessario tale livello di precisione, tuttavia, è consigliabile usare doppie invece. È possibile usare una [convertitore](../../modeling/value-conversions.md) per continuare a usare decimale nelle classi.

``` csharp
modelBuilder.Entity<MyEntity>()
    .Property(e => e.DecimalProperty)
    .HasConversion<double>();
```

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
